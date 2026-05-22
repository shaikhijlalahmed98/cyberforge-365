# Day 006 — Security Control Types: Preventive, Detective, Corrective, Compensating

**Date:** 2026-05-22
**Phase:** Q1 → Month 1 → Week 1 (The Security Mindset)
**Theme:** Kal humne layers lagayin (defense-in-depth). Aaj har layer ko classify karenge — woh attack ko *rokti* hai, *pakadti* hai, ya *theek* karti hai — aur jab ideal control mumkin na ho to *compensating* control kaise design karte hain.

---

## Table of Contents

1. [Today's Briefing](#1-todays-briefing)
2. [Core Concept — Control Types Unpacked](#2-core-concept--control-types-unpacked)
   - 2.1 [Pehle ek kahani: bank kaise sochta hai](#21-pehle-ek-kahani-bank-kaise-sochta-hai)
   - 2.2 [Do axes — har control ke DO naam hote hain](#22-do-axes--har-control-ke-do-naam-hote-hain)
   - 2.3 [Preventive — rok do, hone hi mat do](#23-preventive--rok-do-hone-hi-mat-do)
   - 2.4 [Detective — pakdo, jab prevention chup-chaap fail ho](#24-detective--pakdo-jab-prevention-chup-chaap-fail-ho)
   - 2.5 [Corrective — detection ke baad theek karo / restore karo](#25-corrective--detection-ke-baad-theek-karo--restore-karo)
   - 2.6 [Compensating — jab ideal control mumkin na ho](#26-compensating--jab-ideal-control-mumkin-na-ho)
   - 2.7 [Deterrent & Directive — bonus do, taake matrix poora ho](#27-deterrent--directive--bonus-do-taake-matrix-poora-ho)
   - 2.8 [Tu yeh pehle se karta hai — backend mein control types](#28-tu-yeh-pehle-se-karta-hai--backend-mein-control-types)
   - 2.9 [Anti-patterns — "mat kar yeh"](#29-anti-patterns--mat-kar-yeh)
   - 2.10 [Common failure modes](#210-common-failure-modes)
3. [Mental Models](#3-mental-models)
4. [Trade-offs Analysis](#4-trade-offs-analysis)
5. [Architect's Decision Framework](#5-architects-decision-framework)
6. [Beyond the Basics — Futurist Lens](#6-beyond-the-basics--futurist-lens)
7. [Real-World Incident — Target (2013) Deep Study](#7-real-world-incident--target-2013-deep-study)
8. [Pattern Recognition — Secondary Incidents](#8-pattern-recognition--secondary-incidents)
9. [Common Misconceptions Cleared](#9-common-misconceptions-cleared)
10. [Glossary](#10-glossary)
11. [Self-Check Quiz](#11-self-check-quiz)
12. [Lab Assignment](#12-lab-assignment)
13. [Reflection & Tomorrow](#13-reflection--tomorrow)
14. [Progress Entry](#14-progress-entry)

---

## 1. Today's Briefing

**Phase:** Q1 Foundations · **Month 1:** Security First Principles · **Week 1:** The Security Mindset · **Day 6 of 365**

**Why this matters TODAY:** Ijlal, kal (Day 5) tune defense-in-depth seekha — "heterogeneous layers lagao." Lekin ek sawaal adhoora reh gaya: **heterogeneous by what?** Layers alag *kis* basis pe honi chahiye? Aaj iska jawaab hai: **function ke basis pe.** Ek mazboot stack mein kuch layers attack ko *rokti* hain (preventive), kuch *pakadti* hain (detective), kuch *theek* karti hain (corrective). Jab tu yeh vocabulary internalize kar leta hai, to kisi bhi system ko dekhte hi gap dikhne lagta hai: "yahan 5 preventive controls hain lekin ek bhi detective nahi — yeh Equifax wala blind spot hai." Yeh woh lens hai jo audit, threat model, aur architecture review — teeno mein roz kaam aayega.

**Kal se connection:** Day 5 mein Equifax ka sabaq tha — "saari preventive layers fail huyin aur **detective layer (SSL inspection) decayed thi**, isliye 76 din koi nahi jaaga." Aaj hum exactly woh distinction formalize kar rahe hain. Aur Day 4 (AAA) se bhi connection: yaad hai "teesra A" — **Accounting/Auditing**? Woh literally ek **detective control** hai. Authentication aur Authorization **preventive** hain. Aaj woh dots connect honge.

---

## 2. Core Concept — Control Types Unpacked

### 2.1 Pehle ek kahani: bank kaise sochta hai

Chal pehle ek aisi jagah se shuru karte hain jise har koi samajhta hai — ek bank branch. Computers chhod, sirf socho ek bank apne cash ko kaise protect karta hai.

Bank ke bahar ek board lagta hai: *"24/7 CCTV surveillance. Armed guards on premises."* Yeh board chor ke andar aane se **pehle** usay sochne pe majboor karta hai — "rehne do, kahin aur try karte hain." Is board ne abhi tak kisi ko **roka** nahi, lekin **discourage** kiya. Ab vault ka steel door — yeh chor ko physically **rok** deta hai, chahe woh kitna bhi try kare. Phir CCTV cameras — yeh chor ko rokte nahi, lekin agar woh andar aa gaya, to camera **record** kar leta hai aur alarm bajata hai — yaani "kuch ghalat ho raha hai" yeh **bata** deta hai. Phir alarm sun ke police aati hai, chor ko pakadti hai, churaaya hua paisa wapas laati hai, aur insurance baaki nuksaan **cover** karti hai — yaani jo nuksaan ho gaya usay **theek** karti hai.

Ab gaur kar — bank ne chaar **alag kism** ke kaam kiye:
1. **Discourage kiya** (warning board) — *deterrent*
2. **Roka** (vault door) — *preventive*
3. **Detect kiya** (CCTV + alarm) — *detective*
4. **Theek kiya** (police + insurance) — *corrective*

Aur ek paanchwa scenario socho: maan le vault ka steel door ban hi nahi sakta (purani building hai, structure support nahi karta). Bank chhod thodi dega? Nahi — woh **alternative** lagayega: cash ko ek time-locked safe mein rakhega + extra guard + cameras double kar dega. Yeh "ideal control nahi laga sakte to equivalent protection ka jugaad" — isay **compensating control** kehte hain.

Yeh exactly woh classification hai jo security architecture mein use hoti hai. Bas "chor aur vault" ki jagah "attacker aur data" rakh de. Aaj hum yeh poori vocabulary seekhenge — kyunki ek architect ka aadha kaam yeh hai ke woh kisi bhi control ko dekhte hi keh sake "yeh kis kism ka hai, aur is system mein kaunsi kism missing hai."

**Takeaway:** Har security control kuch na kuch *karta* hai — rokta, pakadta, theek karta, ya discourage karta. Us "kya karta hai" ko classify karna seekhna = architect ki aankh.

### 2.2 Do axes — har control ke DO naam hote hain

Yeh sabse pehle pakad le, kyunki log yahan confuse hote hain. Har security control ko hum **do alag axes** pe label karte hain, ek saath:

**Axis 1 — Function (yeh control *karta kya* hai?):**
- **Preventive** — incident ko hone se rokta hai
- **Detective** — incident ho raha hai ya ho chuka, yeh batata hai
- **Corrective** — incident ke baad nuksaan theek/restore karta hai
- **Compensating** — ideal control jab mumkin na ho, uska alternative
- **Deterrent** — attacker ko discourage karta hai (psychological)
- **Directive** — behavior mandate karta hai (policy, SOP)
- (**Recovery** — corrective ka woh hissa jo specifically restore pe focus karta hai — backups, DR)

**Axis 2 — Nature / implementation (yeh control *banaya kaise* gaya?):**
- **Technical** (a.k.a. *logical*) — code/tech mein implement (firewall, encryption, IAM, `[Authorize]`)
- **Administrative** (a.k.a. *managerial*) — policies, procedures, training, background checks
- **Physical** — duniya mein physically (locks, guards, badge readers, fences)

Ab kamaal ki baat: **har control dono axes pe ek label rakhta hai.** Misaal:
- Ek `[Authorize(Roles="Admin")]` attribute = **technical** (nature) + **preventive** (function).
- Ek security awareness training = **administrative** + **preventive** (deterrent bhi).
- Ek CCTV camera = **physical** + **detective**.
- Ek audit log = **technical** + **detective**.
- Backup se restore = **technical** + **corrective/recovery**.

Toh jab koi poochhe "yeh kis kism ka control hai?" — sahi jawaab hamesha do shabd hai. "Encryption ek technical preventive control hai." Yeh do-axis soch tujhe matrix banane deti hai (Section 3, Mental Model 2), aur matrix mein **khaali cells = tere blind spots.**

> **Backend-dev intuition:** Tu code ko bhi do tarah classify karta hai — "yeh kya karta hai" (business logic vs validation vs logging) aur "yeh kahan rehta hai" (controller vs service vs repository). Security controls bhi waise hi: *function* (kya karta hai) + *nature* (kahan/kaise implement). Ek hi muscle.

### 2.3 Preventive — rok do, hone hi mat do

Yeh sabse intuitive hai, aur yahin pe junior devs ruk jaate hain (sochte hain security = sirf prevention). Preventive control ka maqsad: **incident ko hone hi mat do.**

Pehle yeh samajh ke yeh idea kahan se aaya. Classical security 1970s-80s mein bani — military aur mainframe duniya — jahan soch thi "agar tum perimeter strong rakho aur access control sahi karo, to bura aadmi andar aayega hi nahi." Yaani poora bharosa **prevention** pe. Yeh kaam karta hai jab tak... koi ek control fail na ho (jo hamesha hota hai, jaise Day 5 mein dekha). Lekin prevention abhi bhi pehli line hai, kyunki **roki gayi cheez ka koi cleanup nahi karna padta** — sabse sasta long-term.

Examples (ShopFast/backend context):
- **WAF** jo SQL injection patterns block kare — *technical preventive*
- **`[Authorize]` / `@PreAuthorize`** — authorization se unauthorized access roko — *technical preventive*
- **Input validation + parameterized queries** — injection roko — *technical preventive*
- **MFA** — stolen password se login roko — *technical preventive*
- **Encryption at rest** — disk chori ho to data padha na ja sake — *technical preventive*
- **Least privilege IAM** — compromised account ka blast radius pehle se chhota — *technical preventive*
- **Background checks before hiring** — insider threat pehle se filter — *administrative preventive*
- **Door lock / badge reader** — physical entry roko — *physical preventive*

**Failure mode jo yaad rakhna:** Prevention **chup-chaap** fail hoti hai. WAF rule mein gap reh gaya, ya `[Authorize]` kisi naye endpoint pe lagana bhool gaye — attacker andar aa gaya aur **tujhe pata bhi nahi chala.** Isi liye akeli prevention kaafi nahi. Yeh seedha next section pe le jaata hai.

> **Backend-dev intuition:** Preventive control = woh `if (!valid) return BadRequest()` jo galat input ko process hone se pehle hi rok deta hai. Zaroori, lekin agar yahi sirf hai aur logging nahi, to jab bypass ho to tu blind hai.

### 2.4 Detective — pakdo, jab prevention chup-chaap fail ho

Yeh woh control hai jo junior dev sabse zyada underestimate karta hai, aur architect sabse zyada value karta hai. Detective control attack ko **rokta nahi** — woh batata hai ke "kuch ghalat ho raha hai (ya ho chuka)."

Kyun chahiye? Section 2.3 ka punchline yaad kar: **prevention chup-chaap fail hoti hai.** Agar tere paas sirf preventive controls hain aur woh fail ho jaayein, to breach **mahino** chal sakti hai aur tujhe khabar nahi (Equifax: 76 din; Marriott: 4 saal). Detective control woh smoke alarm hai — aag ko bujhata nahi, lekin tujhe jaga deta hai jab tak bujhana mumkin hai.

Yaad hai Day 4 ka **teesra A — Accounting/Auditing**? Woh literally yahi hai. Authentication (kaun ho?) aur Authorization (kya kar sakte ho?) **preventive** hain. Accounting (kisne kya kab kiya — audit log) **detective** hai. Day 4 mein humne kaha tha "non-repudiation ke liye accounting chahiye" — aaj us baat ko control-type ka naam mil gaya.

Examples:
- **IDS/IPS** (Intrusion Detection System) — network pe malicious patterns detect kare — *technical detective* (IPS prevention bhi karta hai)
- **SIEM** (Splunk/Elastic/Sentinel) — logs ko correlate kar ke alert kare — *technical detective*
- **Audit logs** (kisne refund issue kiya, kab) — *technical detective*
- **FIM** (File Integrity Monitoring) — koi system file badle to alert — *technical detective*
- **Anomaly/behavioral detection** (UEBA) — "yeh agent normal se 10× refunds kar raha hai" — *technical detective*
- **CCTV / motion sensor** — *physical detective*
- **Log review / internal audit** — *administrative detective*

**Sabse important rule jo aaj internalize karna hai:** *Ek detective control bekaar hai agar uske peeche ek **response process** na ho.* Smoke alarm baj raha hai lekin koi ghar pe nahi hai — to alarm ne kya bachaya? **Kuch nahi.** Yeh exactly Target (2013) ka sabaq hai (Section 7) — unka detective control (FireEye) ne **kaam kiya, alert diya** — lekin response process fail ho gaya, kisi ne act nahi kiya. **Detection bina response ke ek logfile hai, control nahi.** Yeh detective → corrective ka handoff hi aaj ka dil hai.

> **Backend-dev intuition:** Detective control = teri observability stack. Prometheus metrics, Grafana alerts, distributed tracing, structured logs. Tu jaanta hai — "metric collect karna useless hai agar alert wired na ho aur on-call koi na ho." Security mein bilkul wahi: log likhna useless hai agar koi padhta/alert hota na ho.

### 2.5 Corrective — detection ke baad theek karo / restore karo

Detective ne bataya "kuch ghalat hai." Ab kya? Yahan **corrective control** aata hai — woh action jo nuksaan ko **rokta, contain karta, ya reverse** karta hai detection ke baad.

Soch ka flow yeh hai: **Detect → Respond → Correct.** Corrective control woh hai jo "respond" ko action mein badalta hai:
- Compromised host ko network se **isolate/quarantine** karna
- Leaked token/credential ko **revoke** karna
- Vulnerable system ko **patch** karna (patch khud preventive hai, lekin breach ke baad apply karna corrective response hai)
- Malware **remove** karna
- Backup se data **restore** karna (yeh **recovery**, corrective ka ek subset)
- Buggy deploy ko **rollback** karna
- Failover to standby

Corrective controls do flavour mein aate hain — **manual** (ek human runbook follow kare) ya **automated** (system khud react kare). Architect ka goal hai high-severity cheezon ke liye corrective ko **automate** karna, kyunki human response slow hai (Target mein human SOC ne 9000 alerts ke beech FireEye ka alert chhod diya — automation hota to malware delete ho jaata).

Examples:
- **Auto-isolate** (EDR jo compromised endpoint ko network se kaat de) — *technical corrective*
- **Token revocation endpoint** — *technical corrective*
- **fail2ban** — failed login attempts detect kare (detective) **aur** IP ko iptables se ban kare (corrective) — yeh dono ek saath hai (Section 2.8 mein config)
- **Restore from backup** (recovery) — *technical corrective*
- **Incident response plan execute karna** — *administrative corrective*
- **Fire suppression system** (aag bujhana) — *physical corrective*

**Failure mode jo yaad rakhna:** Corrective control jise kabhi **test** na kiya gaya ho, woh control hai hi nahi. Sabse classic: "humare paas backups hain" — lekin restore kabhi try nahi kiya, aur emergency mein pata chalta hai backup corrupt hai ya restore 3 din leta hai. **Untested backup = no backup.** (Maersk NotPetya ka sabaq — Section 8.)

> **Backend-dev intuition:** Tu yeh pehle se karta hai — circuit breaker (fail hone par auto-recover), retry with backoff, health-check-driven auto-restart (K8s liveness probe), auto-rollback on failed deploy. Yeh sab **corrective controls against failure** hain. Same mental muscle, ab attacker ke against: auto-revoke, auto-isolate, restore.

### 2.6 Compensating — jab ideal control mumkin na ho

Yeh sabse "architect-level" concept hai aur sabse galat samjha jaata hai. Toh dhyaan se.

Pehle problem samajh. Real duniya mein, kabhi-kabhi woh control jo tu **lagana chahta hai** — lagaya hi nahi ja sakta. Kyun?
- **Legacy system** — woh 2009 ka payments service jo MFA support hi nahi karta, aur vendor band ho gaya.
- **Cost/feasibility** — full re-architecture ka paisa/time nahi abhi.
- **Operational constraint** — woh database 24/7 chalni chahiye, downtime le ke patch nahi kar sakte abhi.
- **Compatibility** — naya control ek critical integration tod dega.

Toh kya security chhod de? Nahi. Architect ka pragmatism yeh hai: **jab primary (ideal) control feasible na ho, ek aisa alternative control (ya controls ka bundle) lagao jo *equivalent risk reduction* de.** Isay **compensating control** kehte hain.

Yeh term formally **PCI-DSS** (payment card industry standard) se mashhoor hua. PCI kehta hai: agar tum koi requirement literally meet nahi kar sakte, to ek "compensating control" document karo jo (a) requirement ki **intent** poori kare, (b) original jitni **rigor** rakhe, (c) original se "above and beyond" ho, aur (d) similar risk address kare. Yaani jugaad allowed hai, lekin **documented, justified, aur equally strong** jugaad — chalta-hai wala nahi.

**Concrete example (ShopFast):** Maan le ShopFast ka purana `legacy-invoicing` service MFA support nahi karta (primary preventive control = MFA, *infeasible*). Compensating control bundle:
1. Us service ko ek **alag segmented network** mein daal do (sirf VPN + jump host se reachable).
2. Saari access ko **session recording + audit** ke through guzaaro (detective).
3. **IP allow-list** + **time-of-day restriction** lagao.
4. **Sunset date** set karo — "Q3 tak yeh service replace hoga."

Yeh chaar milke "MFA na hone" wale risk ko kaafi had tak compensate karte hain. Note: compensating control aksar **doosre types ka mixture** hota hai (yahan network preventive + detective audit + administrative policy).

**Sabse khatarnaak trap:** Compensating control ko **permanent** bana dena. Woh "Q3 tak replace hoga" — Q3 aata hai, jaata hai, service abhi bhi wahin hai 3 saal baad. Compensating control **tech debt** hai jise actively manage karna padta hai, warna woh decay ho jaata hai (Day 5: decayed control). Aur doosra trap: kisi requirement se bachne ke liye "compensating control" ka label maar dena bina equivalent rigor ke — PCI auditors isay turant pakad lete hain, aur asal mein tu khud ko dhoka de raha hai.

> **Backend-dev intuition:** Compensating control = woh **feature flag** ya **rate limit** jo tune ek vulnerable endpoint pe lagaya kyunki proper fix abhi ship nahi ho sakta. "Pura fix nahi kar sakta abhi, to risk ko temporarily contain kar deta hoon" — bilkul wahi soch. Bas yaad rakh: temporary ka matlab temporary, permanent crutch nahi.

### 2.7 Deterrent & Directive — bonus do, taake matrix poora ho

Char main types ke alawa do aur hain jo CISSP-style taxonomy mein aate hain. Inhe halka samajh, lekin pehchaan zaroor:

- **Deterrent control:** Attacker ko **psychologically discourage** karta hai, physically rokta nahi. Misaal: login page pe *"All activity is monitored and logged"* banner; visible CCTV; "violators will be prosecuted" sign. Yeh determined attacker ko nahi rokta, lekin opportunistic / insider ko sochne pe majboor karta hai. **Galti mat kar:** deterrent ko prevention samajhna. Banner se koi hacker ruk nahi jaata.

- **Directive control:** Behavior ko **mandate** karta hai. Misaal: security policy ("passwords must be 14+ chars"), acceptable use policy, SOP/runbook. Yeh administrative hote hain. Yeh "kya karna chahiye" define karte hain; unhe enforce karne ke liye preventive/detective controls chahiye.

Inko isliye jaanna zaroori hai ke audit/compliance (Month 6) mein yeh formally aate hain, aur jab tu controls ko classify karega to in dono ko miss karega to matrix adhoora rahega.

### 2.8 Tu yeh pehle se karta hai — backend mein control types

Theory bahut hui — ab code. Achhi khabar: tere backend mein yeh chaaron types **pehle se** maujood hain, bas tune unhe security vocabulary nahi di. Dekh.

**Example 1 — .NET (C#): teeno function types ek hi flow mein**

```csharp
// ShopFast — RefundsController, har layer ko control-type ke saath label kiya
[HttpPost("api/refunds")]
[Authorize(Roles = "Support")]              // PREVENTIVE (technical): role gate
public async Task<IActionResult> IssueRefund([FromBody] RefundRequest req)
{
    // PREVENTIVE: input validation — bad data ko process hone se roko
    if (req.OrderId <= 0 || req.AmountCents is <= 0 or > 10_000_00)
        return BadRequest("Invalid refund request");

    var order = await _orders.FindAsync(req.OrderId);
    if (order is null) return NotFound();

    var agentId = User.FindFirstValue(ClaimTypes.NameIdentifier);
    if (order.AssignedSupportAgentId != agentId)
        return Forbid();                    // PREVENTIVE: fine-grained authZ

    await _payments.RefundAsync(order, req.AmountCents);

    // DETECTIVE (technical): structured audit event — "kisne kya kab" (Day 4 ka 3rd A)
    _audit.Emit(new AuditEvent(
        Event: "REFUND_ISSUED",
        Who:   agentId,
        What:  $"order={order.Id} amount_cents={req.AmountCents}",
        When:  DateTimeOffset.UtcNow,
        Where: HttpContext.Connection.RemoteIpAddress?.ToString(),
        Outcome: "SUCCESS"));

    return Ok();
}
```

Note: yahan **preventive** (authZ + validation) aur **detective** (audit event) ek hi handler mein. Corrective abhi missing hai — woh response side pe aata hai (anomaly rule fire → token revoke / agent suspend).

**Example 2 — Spring Boot (Java): corrective control via Resilience4j (against failure muscle, ab security context)**

```java
// CORRECTIVE pattern: jab downstream payment gateway compromise/abuse detect ho,
// circuit breaker usay isolate kar deta hai — blast radius contain.
@PostMapping("/api/refunds")
@PreAuthorize("hasRole('SUPPORT')")                 // PREVENTIVE
@CircuitBreaker(name = "paymentGateway", fallbackMethod = "refundFallback") // CORRECTIVE (isolate)
public ResponseEntity<Void> issueRefund(@Valid @RequestBody RefundRequest req, // PREVENTIVE (bean validation)
                                        @AuthenticationPrincipal UserDetails agent) {
    Order order = orders.findById(req.getOrderId())
            .orElseThrow(() -> new ResponseStatusException(NOT_FOUND));
    if (!order.getAssignedSupportAgentId().equals(agent.getUsername()))
        throw new ResponseStatusException(FORBIDDEN);            // PREVENTIVE

    payments.refund(order, req.getAmountCents());
    auditLogger.refundIssued(agent.getUsername(), order, req);   // DETECTIVE
    return ResponseEntity.ok().build();
}

// CORRECTIVE: gateway misbehaving → fail safe (deny), not fail open (allow)
private ResponseEntity<Void> refundFallback(RefundRequest req, UserDetails agent, Throwable t) {
    auditLogger.refundBlocked(agent.getUsername(), req, t);      // DETECTIVE on the corrective path too
    return ResponseEntity.status(SERVICE_UNAVAILABLE).build();   // fail-safe default
}
```

**Example 3 — Real config: `fail2ban` (detective + corrective ek saath)**

`fail2ban` ek perfect example hai ke ek tool DO function types kaise nibhata hai — woh logs scan kar ke failed logins **detect** karta hai (detective), phir offending IP ko firewall se **ban** karta hai (corrective).

```ini
# /etc/fail2ban/jail.d/shopfast-api.conf
[shopfast-auth]
enabled  = true
filter   = shopfast-auth          # DETECTIVE: log pattern jo failed-auth match kare
logpath  = /var/log/shopfast/auth.log
maxretry = 5                      # threshold: 5 fails
findtime = 600                    # 10 min window
bantime  = 3600                   # CORRECTIVE: offending IP ko 1 ghante ban
action   = iptables-multiport[name=shopfast, port="https"]
```

**Example 4 — Real config: Postgres trigger as a detective control (tamper-evident audit)**

```sql
-- DETECTIVE (technical): har refund DB row change ko append-only audit table mein record karo.
-- App-layer audit bypass ho jaye (Layer independence!) to bhi DB ye pakad leta hai.
CREATE TABLE refund_audit (
    id          bigserial PRIMARY KEY,
    refund_id   bigint,
    actor       text,
    amount_cents bigint,
    occurred_at timestamptz NOT NULL DEFAULT now()
);
CREATE OR REPLACE FUNCTION audit_refund() RETURNS trigger AS $$
BEGIN
    INSERT INTO refund_audit(refund_id, actor, amount_cents)
    VALUES (NEW.id, current_user, NEW.amount_cents);
    RETURN NEW;
END; $$ LANGUAGE plpgsql;
CREATE TRIGGER trg_audit_refund AFTER INSERT ON refunds
FOR EACH ROW EXECUTE FUNCTION audit_refund();
```

Dekh — yeh DB-level audit ek **independent detective layer** hai (Day 5 ka independence concept). Agar app ka audit code bypass ho (ya developer bhool jaaye), DB trigger phir bhi record karta hai. Defense-in-depth + control types = saath chalte hain.

### 2.9 Anti-patterns — "mat kar yeh"

| Anti-pattern | Kya galat hai | Sahi soch |
|---|---|---|
| **All-prevention, no-detection** | Sirf WAF/authZ/encryption; koi audit/IDS/SIEM nahi | Har critical path pe ek detective control — assume prevention chup-chaap fail hogi (Equifax) |
| **Detection bina response** | Alert fire hote hain lekin koi runbook/on-call/automation nahi | Har detective control ke peeche ek corrective process (manual ya automated) — warna logfile hai, control nahi (Target) |
| **Auto-correction disabled "to be safe"** | EDR/auto-delete off kar diya false-positive ke darr se | Tune karo, off mat karo — Target ne FireEye auto-delete off kiya tha |
| **Compensating control = permanent** | "Temporary" jugaad jo 3 saal chala | Sunset date + owner + periodic review; primary fix ka ticket |
| **"Compensating" label bina rigor** | Requirement se bachne ke liye weak alternative ko compensating keh dena | Equivalent intent + rigor + risk coverage document karo (PCI test) |
| **Write-only logs** | Logs collect hote hain, koi padhta/alert nahi | Alerting + retention + off-host (Day 5) — log = detective sirf tab jab dekha jaaye |
| **Untested recovery** | "Backups hain" lekin restore kabhi try nahi kiya | Restore drills; RTO/RPO measure karo — untested backup = no backup |
| **Deterrent ko prevention samajhna** | "Monitored" banner laga ke secure samajhna | Banner discourage karta hai, rokta nahi — pair with real preventive |

### 2.10 Common failure modes

1. **Detection without response (Target).** Detective control fire hua, lekin response process — human ya automated — fail ho gaya. Sabse mehnga failure, kyunki paisa detection pe laga aur woh waste gaya.
2. **Logs written but never monitored.** "Compliance ke liye log kar rahe hain" lekin koi dashboard/alert nahi. Breach ke baad forensics mein kaam aate hain, lekin **roka kuch nahi** — late detection.
3. **Compensating control becomes a permanent crutch.** Temporary jugaad ko kabhi proper fix nahi mila; control decay ho gaya; ab woh false sense of security deta hai.
4. **Recovery never tested.** Backups exist par restore broken/slow. Asli emergency mein pata chalta hai. (Maersk almost — bachey ek lucky offline DC se.)
5. **Deterrent/directive mistaken for technical control.** Policy likh di ("MFA mandatory") lekin technically enforce nahi kiya — directive control bina preventive enforcement ke sirf kaagaz hai.
6. **One control overloaded for many needs.** Ek hi log ko prevention + detection + recovery sab maan lena. Ek control aam taur pe ek-do function deta hai; gaps ko alag controls se bharo.
7. **Alert fatigue.** Itne false-positive alerts ke asli alert dab jaata hai (Target: roz hazaaron alerts, FireEye ka asli alert noise mein gum). Detection ko tune karna utna hi zaroori jitna usay lagana.

---

## 3. Mental Models

**Model 1 — The Attack Timeline ("left of boom / right of boom").** Har control attack ki ek timeline pe baithta hai. Socho ek horizontal line jiske beech mein "BOOM" (the compromise moment) hai:

```
   ── BEFORE the attack ─────────┃ BOOM ┃───────── AFTER the attack ──────────►
   Deterrent   Preventive        ┃ ⚡  ┃   Detective    Corrective   Recovery
   (discourage)(stop it)         ┃     ┃   (spot it)   (contain it) (restore it)
   "mat aao"   "andar mat aane do"┃     ┃   "pakdo!"    "rok/theek"  "wapas khada karo"
```

"Left of boom" = deterrent + preventive (attack hone se pehle). "Right of boom" = detective + corrective + recovery (attack ke during/after). Architect ka kaam: **dono taraf controls ho.** Sirf left = Equifax (sab prevention, andhi detection). Detect tha lekin right side ka corrective handoff toota = Target. **Achha system poori timeline cover karta hai.**

**Model 2 — The Control Matrix (function × nature).** Ek grid banao: rows = function (preventive/detective/corrective/compensating/deterrent/directive), columns = nature (technical/administrative/physical). Apne system ke har control ko us grid mein daal. **Khaali cells = blind spots.** Agar "detective" row poori khaali hai — tu Equifax ki taraf ja raha hai. Agar "physical" column khaali hai aur tera data ek office server room mein hai — physical control missing. Yeh matrix audit ka sabse fast tool hai.

| Function ↓ / Nature → | Technical | Administrative | Physical |
|---|---|---|---|
| Preventive | WAF, authZ, encryption | hiring background check | door lock, badge |
| Detective | SIEM, audit log, IDS | log review, internal audit | CCTV, motion sensor |
| Corrective | auto-isolate, restore | IR plan execution | fire suppression |
| Compensating | network isolation + extra monitoring (for un-patchable system) | extra approval workflow | escort policy for restricted area |

**Model 3 — "A detective control is a smoke alarm." (the response-handoff model).** Yeh aaj ka dil hai. Smoke alarm (detective) aag bujhata nahi — woh tujhe **jagata** hai taake tu (ya sprinkler = corrective) bujha sake. Do tarah se yeh useless ho jaata hai: (a) battery dead (Equifax: SSL cert expired → alarm hi nahi baja), ya (b) baj raha hai par koi ghar pe nahi / sun ke ignore kar diya (Target: alarm baja, koi aaya nahi). **Detection aur correction ek hi cheez ke do halve hain — ek bina doosre ke adhoora.** Jab bhi tu detective control lagaye, foran poochho: "iske fire hone par **kya hota hai, kaun karta hai, kitni der mein?**"

---

## 4. Trade-offs Analysis

Har control type ki apni keemat aur limitation hai. Architect "sab kuch lagao" nahi karta — woh sahi mix chunta hai.

| Control type | Strength | Weakness / cost | Kab lean karo |
|---|---|---|---|
| **Preventive** | Incident hota hi nahi → cleanup nahi; sasta long-term | Chup-chaap fail hota hai; over-tight = legit users block (false positives), friction | Pehli choice hamesha; high-frequency known attacks |
| **Detective** | Sasta add karna; assume-breach ko enable karta; forensics | Akela kuch rokta nahi; **response chahiye**; alert fatigue / false positives | Har critical path pe (non-negotiable); jahan prevention bypass ho sakti hai |
| **Corrective** | Damage contain/reverse; resilience | Reactive — nuksaan thoda ho hi chuka; automate na ho to slow | High-severity jahan fast containment chahiye; automate where possible |
| **Compensating** | Pragmatic — jab ideal control infeasible | Aksar weaker; complexity; permanent ban-ne ka risk | Legacy/cost/feasibility constraint; **sunset date ke saath** |
| **Deterrent** | Bohot sasta; opportunistic/insider ko discourage | Determined attacker ko nahi rokta; measurable nahi | Pair-with, kabhi akele bharosa mat karo |

**Key insight:** Yeh types **competitors nahi, teammates** hain. Sahi sawaal "preventive ya detective?" nahi — sahi sawaal "**is risk ke liye mere paas dono taraf (left & right of boom) coverage hai?**" Aur cost-wise: prevention sabse sasta long-term (cleanup nahi), detection sasta to add lekin response process ka hidden cost, correction mehnga (incident already happening), compensating mein hidden maintenance cost. Risk-proportionate (Day 5) raho.

---

## 5. Architect's Decision Framework

Jab kisi risk/asset ke liye controls design karne ho, yeh sequence chala:

**Step 1 — Risk pakdo.** (Day 2: risk = likelihood × impact.) Kaunsa threat, kis asset pe, kitna impact. Crown jewel (Day 1) pe zyada mehnat.

**Step 2 — Pehle PREVENT karne ki koshish.** "Kya is risk ko hone se rok sakta hoon?" Prevention sabse sasta long-term hai. Yeh teri default pehli layer.

**Step 3 — Maan le prevention fail hogi → kya DETECT karega?** Har preventive control ke saath poochho: "agar yeh bypass ho jaaye, mujhe pata kaise chalega?" Agar jawaab "pata nahi chalega" — wahan ek detective control add karo. **(Yeh woh sawaal hai jo Equifax ne nahi poochha.)**

**Step 4 — Detection fire ho to kya CORRECT karega — aur kaun, kitni der mein?** Har detective control ke peeche ek response: automated (best for high-severity) ya manual runbook + on-call. **(Yeh woh handoff hai jo Target ne miss kiya.)** Likho: detection → action → owner → SLA.

**Step 5 — Ideal control infeasible? COMPENSATING design karo.** Agar primary control nahi laga sakte (legacy/cost), ek equivalent-risk-reduction bundle banao. Document: kaunsi requirement, kyun infeasible, compensating control kya, equivalent rigor kaise, **sunset date** kya.

**Step 6 — Matrix pe plot karo, gaps dhoondo.** Saare controls function × nature grid mein daal. Khaali rows/columns = blind spots. Detective row khaali? Physical column khaali? Fix.

**Step 7 — Test karo (especially corrective/recovery).** Backup restore drill chalao. Detective control ko deliberately trigger kar ke dekho alert + response actually fire hota hai. **Untested control = no control.**

**Step 8 — Maintain & review.** Har control ka owner + health check (Day 5 decayed-control). Compensating controls ka periodic review (permanent ban-ne se roko).

---

## 6. Beyond the Basics — Futurist Lens

**SOAR — corrective controls ka automation (aaj→3 saal).** Target ka core problem: human response slow/missing. **SOAR** (Security Orchestration, Automation, and Response) exactly yeh fix karta hai — detection fire hote hi automated playbook chalta hai: token revoke, host isolate, ticket open, on-call page — sab seconds mein. Future mein corrective controls zyada se zyada **automated** honge (human-in-the-loop sirf high-impact decisions pe). Yeh Month 8 (Detection & Response) mein detail aayega.

**XDR + eBPF — detection har layer pe.** Detective controls deeper ja rahe hain — **XDR** (Extended Detection & Response) endpoint + network + cloud + identity ke signals correlate karta hai; **eBPF**-based tools (Falco, Cilium Tetragon) kernel ke andar se runtime behavior detect karte hain, application ko chhue bina. Detection ki cost gir rahi hai, coverage barh rahi hai.

**AI: dono taraf.** AI detective controls ko powerful banata hai (UEBA — "yeh behavior anomalous hai" jo rule-based miss karta), lekin **alert fatigue + false positives** ka naya rasta bhi. Aur ulta: AI-driven auto-triage (corrective) alerts ko prioritize/resolve karega. Sabse important — **AI/LLM apps ke liye control types**: input filter (preventive) → output scanning + prompt-injection detection/logging (detective) → tool-access revocation / kill-switch (corrective) → human approval for high-impact actions (compensating, jab tak model fully trusted nahi). OWASP LLM Top 10 literally yeh layering prescribe karta hai.

**Self-healing / immutable infrastructure — corrective by design.** Cloud-native duniya mein "repair" ki jagah "replace" — compromised container ko theek karne ki jagah usay maar ke fresh image se reprovision. Corrective control architecture mein **bake** ho gaya, bolt-on nahi.

**Compensating controls for post-quantum.** Abhi har jagah Kyber/Dilithium swap nahi kar sakte ("harvest now, decrypt later" threat — Day 5). Interim compensating control: **crypto-agility wrapper** (algorithm swap-able banao) + sensitive data flows pe extra monitoring + sabse sensitive data ko already PQ-hybrid karna. Yeh ek classic compensating play hai — ideal (full PQ migration) abhi infeasible, to equivalent-risk-reduction bundle. (Month 2.)

---

## 7. Real-World Incident — Target (2013) Deep Study

> Yeh incident Day 5 mein humne **segmentation lens** se chhua (vendor pivot, flat network). Aaj isay **control-types lens** se dekhte hain — kyunki Target security ki sabse painful kahani hai: **detective control ne kaam kiya, alert diya, aur phir bhi 40 million cards chori ho gaye — kyunki corrective response fail ho gaya.** Equifax (Day 5) = detection thi hi nahi. Target = detection thi, response nahi tha. Do alag failure modes, ek hi handoff (detect→correct) ke.

**Scene set karo:** Target US ka doosra sabse bada retailer hai. December 2013 — Christmas shopping ka peak. Crores log roz Target ke stores mein card swipe kar rahe hain. Har swipe ka data ek **Point-of-Sale (POS)** terminal se guzarta hai. Yeh data hi attacker ki manzil thi.

### Timeline (compressed)

| Date (2013) | Event |
|---|---|
| **~Sep–Nov** | Attackers ne **Fazio Mechanical** (Target ka HVAC/refrigeration vendor) ko phishing email se compromise kiya — vendor portal credentials churaaye |
| **~Nov 15** | Stolen vendor creds se Target ke external vendor portal mein entry; flat network ki wajah se POS environment tak pivot (PREVENTIVE missing: segmentation) |
| **Nov 27** | **BlackPOS / "Kaptoxa"** malware POS terminals pe deploy — RAM se card data scrape karna shuru (RAM scraping) |
| **Nov 30** | **FireEye** malware-detection system ne alert fire kiya (`malware.binary`). Bangalore (India) security team ne flag kiya, Minneapolis SOC ko bheja — **DETECTIVE control ne kaam kiya** |
| **Dec 2** | Aur alerts fire huye; **Symantec Endpoint Protection** ne bhi suspicious activity flag kiya |
| **Dec 2–12** | Alerts pe **koi action nahi**. FireEye ka **auto-delete** (automated CORRECTIVE) feature Target ki team ne pehle se **disable** kiya hua tha. Data exfiltration jaari |
| **Dec 12** | US **DOJ** ne Target ko notify kiya (banks ne fraudulent cards ko Target tak trace kiya) — yaani breach **bahar se** pata chali, internal response se nahi |
| **Dec 15** | Target ne malware confirm kiya aur remove kiya |
| **Dec 19** | Public disclosure |

### Numbers
- **~40 million** payment card numbers (track data) chori.
- **~70 million** customers ka PII (naam, address, phone, email) — overlap ke saath, total ~**110 million** records aksar cite hote hain.
- Cost: Target ko ~**$202 million** total (legal, remediation, reissuance); **$18.5M** multistate settlement (2017, us waqt ka largest data-breach settlement); banks ko card reissuance ka alag bojh.
- CEO **Gregg Steinhafel** ne May 2014 mein resign kiya (retail history mein pehla CEO jo data breach pe gaya); CIO bhi gaye.

### Technical root cause — BlackPOS / RAM scraping
POS terminal jab card swipe hota hai, card ka magnetic stripe data (track data) ek lamhe ke liye terminal ki **RAM mein plaintext** hota hai (encryption se pehle/process ke dauraan). **BlackPOS** (a.k.a. Kaptoxa) malware exactly is window ko exploit karta hai — woh POS process ki memory ko continuously scan karta hai (**RAM scraping**) aur card data nikaal ke ek internal staging server pe likhta hai, phir wahan se bahar exfiltrate karta hai. Yeh "data in use" attack hai (Day 5 ki layering mein Layer 3/5). Important: yeh **detect ho sakta tha aur hua bhi** — FireEye ne malware ka behavior pakda. Problem code mein nahi, **response** mein thi.

### Control-types breakdown — yeh aaj ka asli sabaq

| Control type | Status at Target | Kya hua |
|---|---|---|
| **Preventive — network segmentation** | ❌ Missing | Vendor portal se POS tak flat path — Day 5 ka sabaq |
| **Preventive — POS hardening / app-whitelisting** | ❌ Weak | BlackPOS bina rok-tok install/run ho gaya |
| **Preventive — vendor portal 2FA** | ❌ Missing | Stolen single credential = entry |
| **Detective — FireEye malware detection** | ✅ **Worked!** | Nov 30 + Dec 2 ko alert fire kiye |
| **Detective — Symantec EPP** | ✅ Worked | Dec 2 ko bhi flag kiya |
| **Corrective — automated (FireEye auto-delete)** | ⚠️ **Disabled** | Team ne off rakha tha — automated correction off |
| **Corrective — human response (SOC)** | ❌ **Failed** | Alerts ignore/under-prioritize ho gaye |

Gaur kar: **detective controls ne theek kaam kiya.** Paisa detection pe kharch hua aur woh fire bhi hua. Lekin **detect→correct ka handoff toot gaya** — automated correction disabled tha, aur human response process alerts ko act nahi kar paaya (alert fatigue + unclear ownership across Bangalore↔Minneapolis). **Yahi Day 6 ka core sabaq hai: detective control bina corrective response ke ek mehnga logfile hai.**

### Equifax vs Target — ek line mein farq (yaad rakhne layak)
- **Equifax (2017):** Detective control **decayed/missing** tha (SSL inspection cert 19 mahine expired) → alarm baja hi nahi → 76 din blind.
- **Target (2013):** Detective control **kaam kar raha tha** → alarm baja → lekin **koi aaya nahi** (corrective fail).

Dono ka net result same (massive breach), lekin failure ka point alag — ek mein detect missing, doosre mein correct missing. **Architect dono halves audit karta hai.**

### Architect's takeaway (quotable)
> *"Target ke paas detection thi aur woh fire bhi hui — phir bhi 40 million cards gaye. Kyunki ek detective control bina corrective response ke sirf ek logfile hai. Jab bhi koi alert lagao, pehle yeh decide karo: yeh fire ho to **kya hoga, kaun karega, kitni der mein** — warna tum sirf apni breach ka high-resolution recording bana rahe ho."*

### Authoritative sources
- **US Senate Committee on Commerce, Science & Transportation**, *"A 'Kill Chain' Analysis of the 2013 Target Data Breach"* (March 26, 2014).
- **Bloomberg Businessweek** — Riley, Elgin, Lawrence, Matlack, *"Missed Alarms and 40 Million Stolen Credit Card Numbers: How Target Blew It"* (March 13, 2014).
- **KrebsOnSecurity** (Brian Krebs) — original breach reporting (Dec 2013) aur Fazio Mechanical vendor link.
- **Target multistate settlement** (2017), ~$18.5M (announced by state Attorneys General).

---

## 8. Pattern Recognition — Secondary Incidents

**Maersk / NotPetya (2017) — corrective/recovery control, by luck not design.** June 2017 mein NotPetya wiper malware ne Maersk (world's largest shipping company) ke ~49,000 laptops aur ~4,000 servers ko minute-on-minute tabaah kar diya — **Active Directory ke saare domain controllers wipe ho gaye**, jo identity ka backbone hai. Recovery namumkin lag raha tha kyunki AD restore karne ke liye ek surviving copy chahiye thi — aur saari online copies gone thi. Bach gaye **sirf isliye** ke Ghana (Accra) ka ek domain controller us waqt **power outage** ki wajah se offline tha — woh accidentally ek clean backup ban gaya. Us ek machine se poora Maersk dobara khada hua. Cost: ~**$300 million**. **Control-types sabaq:** recovery (corrective) control ko **deliberately design** karna padta hai — offline/immutable backups, tested restore — Maersk ko yeh **luck** se mila, design se nahi. "Untested/online-only backup = no recovery control." (Yeh Month 8 IR mein detail aayega.)

**Equifax (2017) — recall, control-type lens.** Day 5 mein deeply dekha. Aaj ek line: Equifax ke paas detective control (SSL/egress inspection) thi lekin woh **decayed** (expired cert 19 mahine) — yaani control matrix mein "detective" cell bhara hua *dikhta* tha lekin actually dead tha. **Sabaq:** control ka existence ≠ control ka kaam karna. Detective controls ko continuously health-check karo.

Dono cases ka pattern: **right-of-boom controls (detect/correct/recover) ko log "set and forget" samajhte hain — woh exactly wahin fail hote hain jab sabse zyada zaroorat hoti hai.**

---

## 9. Common Misconceptions Cleared

| Misconception | Reality |
|---|---|
| "Security = prevention" | Prevention sirf left-of-boom. Detective + corrective (right-of-boom) ke bina prevention fail hone par tu blind aur helpless (Equifax, Target). |
| "Detective control laga di to safe" | Detection bina response process ke ek logfile hai. Detect→correct handoff define karo (Target). |
| "Compensating control = jugaad jo requirement skip kar de" | Compensating control ko equivalent **intent + rigor + risk coverage** dena padta hai (PCI test); warna self-deception. |
| "Har control ka ek hi naam hota hai" | Har control ke DO labels: function (preventive/detective/...) + nature (technical/admin/physical). |
| "Auto-correction risky hai, off kar do" | Tune karo, off mat karo — Target ne FireEye auto-delete off kiya tha aur pachhtaaya. |
| "Backups hain to recovery sorted" | Untested/online-only backup = no recovery. Restore drill + offline copy chahiye (Maersk). |
| "Warning banner attacker ko rok dega" | Banner = deterrent (psychological), preventive nahi. Determined attacker pe asar nahi. |
| "Audit log compliance ke liye hai" | Audit log ek **detective control** hai (Day 4 ka 3rd A) — agar monitor/alert ho. Compliance side-effect hai. |

---

## 10. Glossary

- **Administrative (managerial) control:** Policies, procedures, training, hiring checks — log/process ke through implement, code ke nahi.
- **Alert fatigue:** Itne (aksar false-positive) alerts ke asli alert noise mein dab jaata hai — Target ka contributing factor.
- **Compensating control:** Jab primary/ideal control feasible na ho, ek alternative (aksar bundle) jo *equivalent risk reduction* de, equivalent intent + rigor ke saath (PCI-DSS concept).
- **Corrective control:** Detection ke baad nuksaan ko contain/reverse/restore karne wala control (isolate, revoke, patch, restore, rollback).
- **Detective control:** Incident ho raha/chuka, yeh identify/alert karne wala control (IDS, SIEM, audit log, FIM, anomaly detection). Rokta nahi.
- **Deterrent control:** Attacker ko psychologically discourage karne wala (warning banner, visible CCTV) — rokta nahi.
- **Directive control:** Behavior mandate karne wala (policy, SOP, acceptable-use) — administrative.
- **Function (of a control):** Control *karta kya* hai — preventive/detective/corrective/compensating/deterrent/directive.
- **Left of boom / Right of boom:** Attack timeline ka mental model — "boom" = compromise; left = deter/prevent, right = detect/correct/recover.
- **Nature (of a control):** Control *implement kaise* hua — technical/administrative/physical.
- **Physical control:** Real-world physical protection (lock, guard, badge reader, fence, fire suppression).
- **Preventive control:** Incident ko hone hi se rokne wala (WAF, authZ, encryption, MFA, validation, least privilege).
- **RAM scraping (BlackPOS/Kaptoxa):** Malware technique jo process ki memory se plaintext data (jaise POS card track data) churaata hai.
- **Recovery control:** Corrective ka subset jo specifically restore pe focus karta hai (backups, DR, failover).
- **SOAR (Security Orchestration, Automation, and Response):** Detection fire hone par automated corrective playbooks chalane wala system.
- **Technical (logical) control:** Tech/code mein implement control (firewall, encryption, IAM, audit code).

---

## 11. Self-Check Quiz

> Answer **without peeking**, `notes/2026-05-22-day6-security-control-types.md` mein. Difficulty mark kar: (E)/(M)/(H).

**Q1. (E)** Char main control functions naam kar (preventive/detective/corrective/compensating) aur har ek ko ek line mein define kar.

**Q2. (E)** "Har control ke DO naam hote hain" — kaunse do axes? Ek example de jisme tu ek hi control ko dono axes pe label kare.

**Q3. (M)** "Left of boom / right of boom" model apne shabdon mein. Equifax aur Target — har ek ka failure timeline ke kis taraf tha?

**Q4. (M)** Ek detective control bina ____ ke ek logfile hai. Blank bhar, aur Target se ek line mein justify kar.

**Q5. (M)** Compensating control kya hai? Ek scenario de (legacy system) jisme primary control infeasible ho, aur ek 2-3 control ka compensating bundle design kar. Sunset date kyun zaroori hai?

**Q6. (M)** Day 4 ke AAA mein se kaunsa "A" detective control hai aur kaunse do preventive? Ek line mein kyun.

**Q7. (H — Target)** Target ke paas detection thi aur woh fire bhi hui — phir bhi 40M cards gaye. Do alag corrective failures naam kar jo is breach mein hue. Inme se kis EK ko theek karna sabse bada difference banata?

**Q8. (H — ShopFast)** `POST /api/refunds` ke liye chaaron function types ka **ek-ek** concrete control design kar (preventive, detective, corrective, compensating). Compensating ke liye ek realistic constraint maan le (e.g. legacy refund engine MFA support nahi karta).

**Q9. (H — Mera project)** System-X2 ka koi ek crown-jewel flow soch. Uske saamne kitne preventive controls hain aur kitne detective? Agar detective row khaali hai — aaj ke baad pehla kaunsa detective control add karega, aur uske fire hone par **kaun, kya, kitni der mein** karega?

**Q10. (H — Futurist)** SOAR control-types ke kis problem ko solve karta hai (Target se tie kar)? Phir ek LLM/AI app ke liye chaaron function types ka ek-ek control naam kar (prompt injection ke against).

---

## 12. Lab Assignment

➡️ **Lab file:** [`labs/2026-05-22-day6-control-classification.md`](../labs/2026-05-22-day6-control-classification.md)

**One-line:** ShopFast ke controls ko function × nature matrix mein classify karo (gaps dhoondo), ek detective control ko complete detect→correct response runbook ke saath wire karo, ek legacy-system compensating-control bundle (sunset date ke saath) design karo, aur .NET/Spring Boot mein ek preventive+detective+corrective handler code karo. 30-60 min.

---

## 13. Reflection & Tomorrow

**Journal question (notes mein answer):** *"Apne kisi production system (System-X2 ya koi aur) ke controls ko honestly char buckets mein daal — preventive, detective, corrective, compensating. Kaunsa bucket sabse bhara hai aur kaunsa sabse khaali? Agar (jaise zyada developers ke saath hota hai) tera 'detective' aur 'corrective' bucket khaali hai — tu Equifax (no detect) aur Target (no correct) dono ke combination ki taraf ja raha hai. Ek concrete detective control jo aaj add kar sakta hai, aur uske fire hone par teri response (kaun-kya-kitni-der) kya hogi?"*

**Connection prompt:** *"Tu microservices mein already corrective controls lagata hai — circuit breaker (auto-isolate), retry, K8s liveness probe (auto-restart), auto-rollback. Yeh defense-in-depth-against-failure ke corrective controls hain. In char ko security ke char function types pe map kar: tere observability/alerting stack mein se kaunsa detective hai, kaunsa corrective? Ab security ke against wahi pattern: ek detective (audit/anomaly) + ek corrective (auto-revoke/auto-isolate) jo tu add karega."*

**Tomorrow (Day 7) preview:** **🔄 Week 1 Recap + 5-question quiz.** Kal naya concept nahi — Week 1 ke 6 din (CIA → Threat/Vuln/Risk → Attack Surface/Blast Radius → AAA → Defense-in-Depth → Control Types) ka recap, ek 5-sawaal quiz, tere notes ke weak spots ki review, aur ek **teach-back** (tu mujhe ek concept padhaayega — reverse Claude). Pace adjustment ka discussion bhi. Aaj raat apne Day 1-6 notes ek baar dekh lena taake kal tayyar ho.

---

## 14. Progress Entry

```
Day 006 | 2026-05-22 | Q1 | Security control types: preventive, detective, corrective, compensating | ✅ Done | Function×nature matrix internalized; Target re-studied — detection fired but corrective response failed (vs Equifax's missing detection); mapped 4 control types onto ShopFast refunds + System-X2
```

---

#control-types #preventive #detective #corrective #compensating #defense-in-depth #left-of-boom #target-2013 #blackpos #ram-scraping #equifax #maersk-notpetya #soar #fail2ban #pci-dss #aaa #shopfast
