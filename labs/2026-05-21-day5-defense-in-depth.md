# Lab — Day 005: Defense-in-Depth on ShopFast

**Date:** 2026-05-21
**Linked lesson:** `lessons/2026-05-21-day5-defense-in-depth.md`
**Time:** 30–60 min
**Goal:** Ek ShopFast crown-jewel flow pe defense-in-depth ko **draw**, **break**, aur **build** karna — yaani (A) layers ki Swiss-cheese map banao, (B) ek "Equifax-style aligned holes" failure scenario likho, (C) .NET/Spring Boot mein multi-layer validation+authZ code karo, (D) ek detective audit layer add karo, aur (E) CTO ko ek architect recommendation do.

> **Why this matters:** Aaj tu woh reflex bana raha hai jo har security architect ke paas hota hai — kisi bhi feature pe dekhte hi "is crown jewel ke saamne kitni *independent* layers hain, aur agar pehli toot jaaye to peeche kya?" Lab ka output Day 30 ShopFast threat-model capstone mein reuse hoga.

---

## Setup — ShopFast crown-jewel flow

Is lab mein hum ek flow pe focus karenge: **`POST /api/refunds`** (paisa wapas karna). Yeh crown jewel hai — money movement + PII touch. Attacker ki manzil: bina haq ke refund issue karna (ya doosre ka refund data churaana).

Reference architecture (simplified):

```
Internet → [CDN/WAF] → [API Gateway] → [refund-service] → [payments DB]
                                              │
                                         [audit/SIEM]
```

---

## Part A — Swiss-Cheese Layer Map (~15 min)

`POST /api/refunds` ke saamne **kam se kam 6 layers** identify kar. Har layer ke liye yeh table bhar. Soch lesson ke 7-layer stack (perimeter/network/host/identity/app/data/monitoring) ke hisaab se.

| # | Layer | Concrete control (ShopFast-specific) | Type (Prevent / Detect / Correct) | Is layer mein "hole" (weakness) kya ho sakta hai? |
|---|---|---|---|---|
| 1 | Perimeter | _e.g. WAF blocks injection + rate-limits refund calls_ | Prevent | _e.g. WAF rule gap, bypass via direct origin IP_ |
| 2 | Network | | | |
| 3 | Identity | _e.g. step-up MFA for refunds > X amount_ | Prevent | |
| 4 | Application | _e.g. fine-grained authZ: support agent assigned to this order?_ | Prevent | |
| 5 | Data | | | |
| 6 | Monitoring | _e.g. anomaly alert: agent issuing N× baseline refunds_ | **Detect** | |

**Success criteria:**
- 6 rows bhare hue.
- Kam se kam **2 alag types** (sirf Prevent nahi — kam se kam ek Detect/Correct).
- Har layer ke saamne uska "hole" likha (yeh Swiss-cheese ka point hai).
- Layers **heterogeneous** hain (do consecutive layers same vendor/assumption pe depend NA karein — likh ke verify kar).

---

## Part B — Break It: "Aligned Holes" Scenario (~10 min)

Recall Swiss-cheese model: disaster tab jab saare holes line up ho jaayein. Ab tu Equifax ban — apni Part A ki layers le aur ek **chain** likho jisme har layer ka hole exploit hota hua attacker ko refund issue karne tak le jaaye.

Yeh format use kar (apni layers se):

```
1. WAF (Perimeter):     hole = ____________ → attacker passes because ____________
2. Network:             hole = ____________ → reaches refund-service because ____________
3. Identity (MFA):      hole = ____________ → because ____________
4. App authZ:           hole = ____________ → issues refund because ____________
5. Monitoring:          hole = ____________ → undetected because ____________
RESULT: unauthorized refund of $____ , detected after ____ days (ya never)
```

Phir ek line likh: **"Agar in 5 mein se SIRF EK layer ka hole band hota, attack ruk jaata — kaunsa, aur kyun?"** (Yeh Equifax ka core sabaq hai.)

**Success criteria:** 5-step aligned-hole chain + ek "single layer that would have stopped it" sentence.

---

## Part C — Build It: Multi-Layer Validation + AuthZ Code (~15 min)

Ek refund handler likho jisme **teen independent layers** ho — input validation, coarse authZ, aur fine-grained (ownership/assignment) authZ. Apni primary stack pick kar.

### Option 1 — .NET (C#)

```csharp
// ShopFast — RefundsController (DEFENSE-IN-DEPTH version)
[HttpPost("api/refunds")]
[Authorize(Roles = "Support")]                 // LAYER 1: coarse authZ (role gate)
public async Task<IActionResult> IssueRefund([FromBody] RefundRequest req)
{
    // LAYER 2: input validation — never trust the client (DiD against bad data)
    if (req.OrderId <= 0 || req.AmountCents <= 0 || req.AmountCents > 10_000_00)
        return BadRequest("Invalid refund request");

    var order = await _orders.FindAsync(req.OrderId);
    if (order is null) return NotFound();

    // LAYER 3: fine-grained authZ — is THIS agent allowed THIS order?
    var agentId = User.FindFirstValue(ClaimTypes.NameIdentifier);
    if (order.AssignedSupportAgentId != agentId)
        return Forbid();                       // 403: authenticated, but not authorized for this object

    // LAYER 4: business invariant — can't refund more than paid (data integrity layer)
    if (req.AmountCents > order.PaidCents)
        return BadRequest("Refund exceeds amount paid");

    await _payments.RefundAsync(order, req.AmountCents);
    // (Part D will add the detective layer here)
    return Ok();
}
```

### Option 2 — Spring Boot (Java)

```java
// ShopFast — RefundController (DEFENSE-IN-DEPTH version)
@PostMapping("/api/refunds")
@PreAuthorize("hasRole('SUPPORT')")            // LAYER 1: coarse authZ
public ResponseEntity<Void> issueRefund(
        @Valid @RequestBody RefundRequest req, // LAYER 2: bean validation (@Min, @Max on the DTO)
        @AuthenticationPrincipal UserDetails agent) {

    Order order = orders.findById(req.getOrderId())
            .orElseThrow(() -> new ResponseStatusException(NOT_FOUND));

    // LAYER 3: fine-grained authZ — agent assigned to THIS order?
    if (!order.getAssignedSupportAgentId().equals(agent.getUsername())) {
        throw new ResponseStatusException(FORBIDDEN);   // 403
    }
    // LAYER 4: business invariant (data integrity)
    if (req.getAmountCents() > order.getPaidCents()) {
        throw new ResponseStatusException(BAD_REQUEST, "Refund exceeds amount paid");
    }

    payments.refund(order, req.getAmountCents());
    // (Part D: detective layer)
    return ResponseEntity.ok().build();
}
```

**Tasks:**
1. Apni stack ka version likh/run kar (ya existing project mein ek endpoint pe yeh 4-layer pattern apply kar).
2. Identify kar: in 4 layers mein se kaunsi **independent** hain? (Hint: agar role-check bypass ho jaye — `[Authorize]` bug — to bhi Layer 3 ownership check rok lega. Yeh independence hai.)
3. Ek line likh: **Layer 4 (DB-side) constraint** bhi add karo (`CHECK (refund_cents <= paid_cents)` Postgres mein) — yeh kis common-mode failure se bachata hai jab app-layer Layer 4 bug ho?

**Success criteria:** Handler mein 3+ independent layers, 403 (not 401) for ownership failure, aur ek DB-level constraint mention.

---

## Part D — Add the Detective Layer (~10 min)

Lesson ka sabse bada sabaq: **prevention chup-chaap fail hoti hai; detection tujhe react karne deti hai** (Equifax = 76 din blind). Ab refund handler mein ek detective layer add kar.

Ek structured audit event likh (JSON), aur ek anomaly rule (pseudocode):

```json
{
  "event":   "REFUND_ISSUED",
  "who":     "agent:Z12345 | role:Support",
  "what":    "POST /api/refunds order=ORD-998 amount_cents=499900",
  "when":    "2026-05-21T14:07:33Z",
  "where":   "ip=203.0.113.7 trace=abc-123",
  "outcome": "SUCCESS",
  "...":     "tu 2 aur fields add kar (e.g. order.assignedAgent match? amount vs baseline?)"
}
```

**Tasks:**
1. Upar wale event ko complete kar — 2 fields add jo anomaly detection ko power dein.
2. Ek anomaly rule English/pseudocode mein likh jo "aligned holes" wali abuse pakde:
   `ALERT if one agent issues > 3× their 7-day average refund amount in 1 hour`
3. **Critical question:** Yeh detective layer kya tab bhi kaam karti hai jab Layer 1-4 (saari preventive) bypass ho jaayein? (Answer: haan — isiliye yeh **independent** layer hai. Yahi Equifax mein missing tha.)
4. Ek line: yeh audit log kahan jaani chahiye (hint: **off-host**, append-only — kyun? Equifax ke plaintext-on-fileshare sabaq se connect kar).

**Success criteria:** Complete audit event (5 W's + 2 extra), 1 anomaly rule, aur "off-host append-only" reasoning.

---

## Part E — One-Paragraph Architect Recommendation (~5 min)

ShopFast ke CTO ko ek paragraph (4–6 lines): *"Hamare refund flow pe abhi sabse pehle kaunsi defense-in-depth layer add karni chahiye aur kyun?"* — Equifax ko tie-in kar (har layer thi lekin holes line up ho gaye; ek bhi zinda layer 147M record bacha leti). Ek concrete pehla step recommend kar (hint: aksar **detective layer** sabse zyada ROI deti hai kyunki woh sabse zyada ignore hoti hai).

---

## Deliverables checklist

- [ ] Part A: Swiss-cheese map — 6 layers, 2+ types, har layer ka "hole", heterogeneity verified
- [ ] Part B: 5-step aligned-holes chain + "single layer that stops it" sentence
- [ ] Part C: multi-layer handler (3+ independent layers, 403 for ownership) + DB constraint mention
- [ ] Part D: complete audit event + anomaly rule + off-host reasoning
- [ ] Part E: CTO recommendation paragraph with Equifax tie-in
- [ ] (Bonus) Apne real project ke ek crown-jewel endpoint pe yeh poora defense-in-depth audit chalaya

---

## Stretch goal (optional)

Apne local .NET ya Spring Boot project mein:
- Ek endpoint pe **4-layer** pattern actually implement kar (validation → coarse authZ → fine authZ → audit).
- Phir **deliberately ek layer toड** (role check comment out kar) aur dekho ke fine-grained authZ (Layer 3) ab bhi rokta hai — **yeh defense-in-depth ko apni aankhon se prove karna hai.**
- Audit log output ka screenshot/snippet evidence ke taur pe paste kar.
- Bonus: Postgres mein `CHECK` constraint laga ke app-layer bug ke bawajood invalid refund block hote dekho — DB layer = last line of defense.
