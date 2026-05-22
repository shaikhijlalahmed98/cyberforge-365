# Lab — Day 006: Classify, Wire & Compensate (Control Types on ShopFast)

**Date:** 2026-05-22
**Linked lesson:** `lessons/2026-05-22-day6-security-control-types.md`
**Time:** 30–60 min
**Goal:** Control types ko **classify**, **wire**, aur **compensate** karna — yaani (A) ShopFast ke controls ko function × nature matrix mein daal ke gaps dhoondna, (B) ek detective control ko poore detect→correct response runbook ke saath wire karna, (C) ek legacy-system compensating-control bundle design karna, (D) .NET/Spring Boot mein preventive+detective+corrective handler code karna, aur (E) CTO ko ek architect recommendation dena.

> **Why this matters:** Aaj tu woh reflex bana raha hai jo har auditor/architect ke paas hota hai — kisi system ko dekhte hi "is risk pe kaunse control types maujood hain aur kaunsa missing hai?" Khaas kar woh sawaal jo Equifax (no detect) aur Target (no correct) dono ne miss kiya. Output Day 30 ShopFast threat-model capstone mein reuse hoga.

---

## Setup — ShopFast crown-jewel flow (continuity from Day 5)

Wahi flow jo kal use kiya: **`POST /api/refunds`** (money movement + PII). Attacker ki manzil: bina haq ke refund issue karna ya refund data churaana.

```
Internet → [CDN/WAF] → [API Gateway] → [refund-service] → [payments DB]
                                              │
                                         [audit/SIEM] ──► [on-call / SOAR]
```

---

## Part A — Control Matrix (classify + find gaps) (~12 min)

Neeche ShopFast ke (kuch real, kuch missing) controls hain. Har ek ko **function × nature** classify kar, aur phir matrix mein **gaps** identify kar.

**A1. Classify these 8 controls** (function + nature dono likh):

| # | Control | Function (prevent/detect/correct/compensate/deter) | Nature (technical/admin/physical) |
|---|---|---|---|
| 1 | WAF blocks SQL injection on `/api/refunds` | | |
| 2 | `[Authorize(Roles="Support")]` on the endpoint | | |
| 3 | Audit log: "agent X refunded order Y, $Z, at T" | | |
| 4 | Anomaly alert: agent refunds > 3× their 7-day avg | | |
| 5 | Auto-revoke agent's session token on anomaly | | |
| 6 | Quarterly secure-coding training for devs | | |
| 7 | "All refund activity is monitored & logged" banner in agent console | | |
| 8 | Nightly encrypted DB backup (restore tested monthly) | | |

**A2. Now plot into the matrix.** Har control ka number sahi cell mein likh:

| Function ↓ / Nature → | Technical | Administrative | Physical |
|---|---|---|---|
| Preventive | | | |
| Detective | | | |
| Corrective | | | |
| Compensating | | | |
| Deterrent | | | |

**Success criteria:**
- 8 controls dono axes pe label.
- Matrix bhara hua.
- Likh: **kaunse cells khaali hain?** Inme se kaunsa khaali cell ShopFast ke liye sabse khatarnaak hai aur kyun? (Hint: agar "physical" column poora khaali hai — kya tera DB ek cloud provider pe hai? to physical unka responsibility — yeh Day 6 ka "shared responsibility" foreshadow hai. Lekin agar detective/corrective khaali ho — yeh tera Equifax/Target risk hai.)

---

## Part B — Wire the Detect → Correct Handoff (~12 min)

Aaj ka core sabaq: **detective control bina corrective response ke ek logfile hai (Target).** Ab tu woh handoff wire karega.

Control #4 (anomaly alert) le. Iska poora **detect → correct runbook** likho:

```
DETECTION RULE:
  ALERT "REFUND_ANOMALY" if agent issues > 3× their 7-day avg refund $ in 1 hour

WHEN IT FIRES:
  WHO responds:      ____________  (e.g. SOC on-call / automated SOAR)
  WHAT happens:      ____________  (corrective action — be specific)
  HOW FAST (SLA):    ____________  (e.g. auto in <30s, or human ack <15 min)
  ESCALATION if no ack: ____________
  EVIDENCE captured: ____________  (what to log for forensics)
```

**Tasks:**
1. Upar wala runbook bhar — har blank concrete.
2. Decide: yeh corrective **automated** ho ya **manual**? Apna choice justify kar (hint: Target mein human SOC ne miss kiya — high-severity money movement ke liye automation kyun behtar?).
3. Ek line: agar yeh corrective control galti se zyada aggressive ho (har bade refund pe legit agent ko block kare), kya **trade-off** hai? Isay tune kaise karega (false positive vs missed attack)?

**Success criteria:** Complete runbook (who/what/how-fast/escalation/evidence), automated-vs-manual decision with justification, aur ek false-positive trade-off line.

---

## Part C — Design a Compensating Control Bundle (~10 min)

**Scenario:** ShopFast ka purana `legacy-refund-engine` (2016 ka, vendor band ho gaya) **MFA support nahi karta** aur **modern audit hooks nahi** rakhta. Primary preventive control (MFA) aur primary detective (app audit) — dono **infeasible** is service pe. Replacement Q4 mein planned hai.

Tu CISO hai. Ek **compensating control bundle** design kar jo "no MFA + no app audit" wale risk ko equivalent had tak reduce kare.

| # | Compensating control | Kis missing control ko compensate karta hai | Type (function + nature) |
|---|---|---|---|
| 1 | _e.g. legacy-refund-engine ko alag segmented subnet, sirf jump host se reachable_ | MFA (access control) | technical preventive |
| 2 | | | |
| 3 | | | |

**Tasks:**
1. Kam se kam **3** compensating controls likh (upar 1 example diya).
2. PCI-DSS test apply kar — likho ek line: yeh bundle kaise (a) original control ki **intent** poori karta hai aur (b) **equivalent rigor** rakhta hai?
3. **Sunset date + owner** likh. Ek line: agar yeh sunset date miss ho jaye to kya risk hai (Day 5 "decayed control" + Day 6 "permanent crutch" se connect kar)?

**Success criteria:** 3+ compensating controls mapped to missing controls, PCI intent+rigor justification, sunset date + owner + decay risk.

---

## Part D — Build It: Preventive + Detective + Corrective Handler (~15 min)

Ek refund handler likho/extend kar jisme **teeno function types** ho. Apni primary stack pick kar.

### Option 1 — .NET (C#)

```csharp
[HttpPost("api/refunds")]
[Authorize(Roles = "Support")]                       // PREVENTIVE: coarse authZ
public async Task<IActionResult> IssueRefund([FromBody] RefundRequest req)
{
    // PREVENTIVE: input validation
    if (req.OrderId <= 0 || req.AmountCents is <= 0 or > 10_000_00)
        return BadRequest("Invalid refund request");

    var order = await _orders.FindAsync(req.OrderId);
    if (order is null) return NotFound();

    var agentId = User.FindFirstValue(ClaimTypes.NameIdentifier);
    if (order.AssignedSupportAgentId != agentId)
        return Forbid();                             // PREVENTIVE: fine-grained authZ

    // CORRECTIVE (proactive containment): anomaly check BEFORE committing money
    if (await _anomaly.IsAbnormalRefund(agentId, req.AmountCents))
    {
        await _tokens.RevokeSession(agentId);        // CORRECTIVE: auto-revoke
        _audit.Emit(AuditEvent.Blocked("REFUND_ANOMALY", agentId, req)); // DETECTIVE
        return StatusCode(423, "Refund locked pending review"); // fail-safe (deny)
    }

    await _payments.RefundAsync(order, req.AmountCents);

    // DETECTIVE: success audit (the 3rd A — accounting)
    _audit.Emit(AuditEvent.Success("REFUND_ISSUED", agentId, order, req));
    return Ok();
}
```

### Option 2 — Spring Boot (Java)

```java
@PostMapping("/api/refunds")
@PreAuthorize("hasRole('SUPPORT')")                          // PREVENTIVE
public ResponseEntity<Void> issueRefund(@Valid @RequestBody RefundRequest req, // PREVENTIVE (bean validation)
                                        @AuthenticationPrincipal UserDetails agent) {
    Order order = orders.findById(req.getOrderId())
            .orElseThrow(() -> new ResponseStatusException(NOT_FOUND));
    if (!order.getAssignedSupportAgentId().equals(agent.getUsername()))
        throw new ResponseStatusException(FORBIDDEN);          // PREVENTIVE

    // CORRECTIVE: anomaly → contain before committing
    if (anomaly.isAbnormalRefund(agent.getUsername(), req.getAmountCents())) {
        tokens.revokeSession(agent.getUsername());            // CORRECTIVE: auto-revoke
        audit.blocked("REFUND_ANOMALY", agent.getUsername(), req); // DETECTIVE
        throw new ResponseStatusException(LOCKED, "Refund locked pending review");
    }

    payments.refund(order, req.getAmountCents());
    audit.success("REFUND_ISSUED", agent.getUsername(), order, req); // DETECTIVE
    return ResponseEntity.ok().build();
}
```

**Tasks:**
1. Apni stack ka version likh/run kar (ya existing project ke ek endpoint pe yeh pattern apply kar).
2. Har line ke saamne comment likh: yeh kaunsa function type hai (P/D/C)?
3. **Bonus — Postgres trigger (independent detective):** lesson Section 2.8 ka `trg_audit_refund` trigger add kar. Ek line likho: yeh app-layer audit se **independent** kyun hai aur kis bypass se bachata hai? (Day 5 independence concept.)
4. **Critical question:** is handler mein "fail-safe (deny)" vs "fail-open (allow)" — anomaly pe humne deny kiya (`423/LOCKED`). Kyun deny, allow nahi? (Money movement context.)

**Success criteria:** Handler mein P + D + C teeno labeled, fail-safe choice justify, aur DB trigger ka independence reasoning.

---

## Part E — One-Paragraph Architect Recommendation (~5 min)

ShopFast ke CTO ko ek paragraph (4–6 lines): *"Hamare refund flow ke controls mein sabse bada gap kaunsa control TYPE hai (Part A matrix se), aur woh kyun sabse khatarnaak hai?"* — Target ko tie-in kar (detective fire hui par corrective response fail = 40M cards). Ek concrete pehla step recommend kar (hint: aksar gap "wired corrective response" hota hai — detection hai, lekin uske fire hone par kaun-kya-kitni-der define nahi).

---

## Deliverables checklist

- [ ] Part A: 8 controls classified (function + nature) + matrix plotted + empty-cell risk identified
- [ ] Part B: complete detect→correct runbook (who/what/how-fast/escalation/evidence) + automated-vs-manual + false-positive trade-off
- [ ] Part C: 3+ compensating controls mapped + PCI intent/rigor justification + sunset date/owner/decay risk
- [ ] Part D: P+D+C handler labeled + fail-safe justification + DB-trigger independence reasoning
- [ ] Part E: CTO recommendation paragraph with Target tie-in
- [ ] (Bonus) Apne real project ke ek crown-jewel endpoint ka control matrix banaya aur missing type identify kiya

---

## Stretch goal (optional)

Apne local .NET ya Spring Boot project mein:
- Ek endpoint pe **P + D + C** pattern actually implement kar (validation/authZ → audit event → anomaly-triggered revoke/deny).
- `fail2ban` install kar (ya container mein) aur lesson ka jail config laga ke **deliberately 5 failed logins** kar — dekho IP ban hota hai (detective + corrective ek saath, apni aankhon se).
- Audit event ka output + fail2ban ban log ka screenshot/snippet evidence ke taur pe paste kar.
- Bonus: ek backup restore drill chala (chhoti DB) aur **time** measure kar — yeh tera RTO (recovery time) hai. Untested backup = no recovery control, yaad hai?
