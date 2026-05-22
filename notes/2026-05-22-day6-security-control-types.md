# Day 006 — Security Control Types: Preventive, Detective, Corrective, Compensating

**Date:** 2026-05-22
**Phase:** Q1 → Month 1 → Week 1 (The Security Mindset)
**Theme:** Har layer ko classify karo — woh attack ko *rokti* hai (preventive), *pakadti* hai (detective), ya *theek* karti hai (corrective); aur jab ideal control mumkin na ho to *compensating* control banao.

---

## Core Concept (in my own words)

> 4-6 sentences mein samjha: char function types (preventive/detective/corrective/compensating) kya hain, aur "har control ke DO naam hote hain" (function × nature) ka kya matlab hai? Pretend you're explaining to a junior backend dev jo sochta hai "security = bas prevention (`[Authorize]` + WAF)."

_Your summary here..._

---

## The Key Terms — One-Liner Each

- **Preventive control:** _(your one-liner)_
- **Detective control:** _(your one-liner)_
- **Corrective control:** _(your one-liner)_
- **Compensating control:** _(your one-liner)_
- **Deterrent control:** _(your one-liner)_
- **Directive control:** _(your one-liner)_
- **Function vs Nature (the two axes):** _(your one-liner)_
- **Left of boom / Right of boom:** _(your one-liner)_

---

## The Two Axes — Say It Cold

- **Axis 1 (function):** _(list the types)_
- **Axis 2 (nature):** _(list: technical / administrative / physical)_
- **Har control ke kitne labels hote hain?** → _(answer)_
- **3 examples, each with BOTH labels:**
  1. `[Authorize]` → _____ + _____
  2. CCTV camera → _____ + _____
  3. Restore from backup → _____ + _____

---

## The Attack Timeline (recall karke likh)

```
── BEFORE ───────┃ BOOM ┃─────── AFTER ───────►
?                ┃  ⚡  ┃   ?        ?       ?
```
- **Left of boom controls:** _____ , _____
- **Right of boom controls:** _____ , _____ , _____
- **Equifax failure timeline side:** _____ (kyun)
- **Target failure timeline side:** _____ (kyun)

---

## The Detect → Correct Handoff (aaj ka dil)

- **"Ek detective control bina ___ ke ek logfile hai."** → _(blank)_
- **Smoke-alarm analogy ke 2 failure modes:** (a) _____ (Equifax-style) (b) _____ (Target-style)
- **Jab bhi detective control lagao, foran 3 sawaal:** (1) _____ (2) _____ (3) _____
- **Day 4 AAA connection:** kaunsa A = detective? → _____ ; kaunse 2 A = preventive? → _____

---

## Compensating Control — Say It Cold

- **Compensating control kab use hota hai?** → _(answer)_
- **PCI-DSS ka test (3 cheezein jo compensating control ko meet karni chahiye):** → _____ , _____ , _____
- **ShopFast legacy example (no MFA) ka 3-control bundle:** → _____ , _____ , _____
- **Sabse khatarnaak trap:** → _(answer: permanent crutch / no sunset)_

---

## Mental Models (apne shabdon)

1. **Attack timeline (left/right of boom)** → _(your version)_
2. **Control matrix (function × nature)** → _("khaali cells = ___")_
3. **Detective = smoke alarm** → _(your version + the response-handoff question)_

---

## Self-Check Quiz Answers

Answer **without peeking**. Mark difficulty: (E)/(M)/(H). Galat ya peek kiya toh ❌.

**Q1. (E) Char function types + ek-line definition each.**
_Answer..._

**Q2. (E) Do axes + ek control dono pe label.**
_Answer..._

**Q3. (M) Left/right of boom apne shabdon; Equifax & Target ka failure side.**
_Answer..._

**Q4. (M) "Detective control bina ___ ke logfile hai" + Target justification.**
_Answer..._

**Q5. (M) Compensating control + legacy scenario bundle + sunset date kyun.**
_Answer..._

**Q6. (M) AAA mein kaunsa A detective, kaunse 2 preventive + kyun.**
_Answer..._

**Q7. (H — Target) 2 corrective failures + kis EK ka fix sabse bada difference.**
_Answer..._

**Q8. (H — ShopFast) `POST /api/refunds` ke liye chaaron types ka ek-ek control (compensating ke liye constraint maan le).**
_Answer..._

**Q9. (H — Mera project) System-X2 crown-jewel: kitne preventive vs detective? Detective khaali ho to pehla kaunsa add + uske fire hone par kaun-kya-kitni-der.**
_Answer..._

**Q10. (H — Futurist) SOAR kis problem ko solve karta (Target tie); LLM app ke liye chaaron types ka ek-ek control (prompt injection).**
_Answer..._

---

## Journal Question

> "Apne kisi production system (System-X2 ya koi aur) ke controls ko honestly char buckets mein daal — preventive, detective, corrective, compensating. Kaunsa bucket sabse bhara hai aur kaunsa sabse khaali? Agar tera 'detective' aur 'corrective' bucket khaali hai — tu Equifax (no detect) aur Target (no correct) dono ke combination ki taraf ja raha hai. Ek concrete detective control jo aaj add kar sakta hai, aur uske fire hone par teri response (kaun-kya-kitni-der) kya hogi?"

_Answer here..._

---

## Connection to System-X2 / My Backend Work

> "Tu microservices mein already corrective controls lagata hai — circuit breaker (auto-isolate), retry, K8s liveness probe (auto-restart), auto-rollback. Yeh defense-in-depth-against-failure ke corrective controls hain. Inhe security ke char function types pe map kar."

_Answer here..._

**Resilience controls I already use → mapped to function type:**
1. (e.g. circuit breaker) → corrective
2.
3.

**Detective + corrective (against attacker) I should add:**
1. Detective:
2. Corrective:

---

## Target (2013) Takeaways — Control-Types Lens

- The detective control that **worked** (and when it fired): _____
- The TWO corrective failures: (1) _____ (2) _____
- The ONE fix that would have made the biggest difference (+ why): _____
- Equifax vs Target — one-line difference (which half of the handoff failed): _____
- "RAM scraping / BlackPOS" in one line: _____
- One architectural lesson I will remember: _____

---

## Pattern Recognition (secondary incidents)

- **Maersk / NotPetya (2017):** the corrective/recovery control = _____ ; saved by = _____ (luck or design?) ; lesson = _____
- **Equifax (recall):** detective control existed but was _____ (control existence ≠ control working)
- **Common pattern (right-of-boom controls):** _____

---

## Things That Stuck

1.
2.
3.

## Things I Need to Re-read

1.
2.

## Terms That Need More Practice

- [ ] Four function types (say all four cold + define)
- [ ] Function × nature (two labels per control)
- [ ] Left of boom / right of boom
- [ ] Detect → correct handoff ("detective bina response = logfile")
- [ ] Compensating control (PCI test: intent + rigor + risk)
- [ ] Compensating control trap (permanent crutch / no sunset)
- [ ] Deterrent ≠ preventive
- [ ] AAA → control-type mapping (accounting = detective)
- [ ] Fail-safe (deny) vs fail-open (allow)
- [ ] SOAR = automated corrective

---

## Lab Status

- [ ] Lab `labs/2026-05-22-day6-control-classification.md` started
- [ ] Part A: 8 controls classified (function + nature) + matrix + empty-cell risk
- [ ] Part B: detect→correct runbook (who/what/how-fast/escalation/evidence) + automated-vs-manual + FP trade-off
- [ ] Part C: 3+ compensating controls + PCI intent/rigor + sunset/owner/decay risk
- [ ] Part D: P+D+C handler labeled + fail-safe justification + DB-trigger independence
- [ ] Part E: CTO recommendation with Target tie-in
- [ ] (Bonus) Ran control-matrix audit on a real project endpoint + fail2ban demo

---

## Futurist Angle (3-5 years)

> SOAR (automated corrective — Target ka fix); XDR + eBPF (detection har layer pe); AI dono taraf (UEBA detective + auto-triage corrective, lekin alert-fatigue/false-positive); LLM app control types (input filter→preventive, output scan/log→detective, tool revoke/kill-switch→corrective, human approval→compensating); self-healing/immutable infra (corrective by design); crypto-agility as a compensating control for post-quantum. In mein se ek pe 2 lines apne shabdon mein:

_Your answer..._

---

## Tomorrow's Preview

**Day 7 → 🔄 Week 1 Recap + 5-question quiz + teach-back (reverse Claude).** Naya concept nahi. Week 1 ke 6 din (CIA → Threat/Vuln/Risk → Attack Surface/Blast Radius → AAA → Defense-in-Depth → Control Types) ka recap, quiz, notes ke weak spots, aur tu ek concept mujhe padhayega. Aaj raat Day 1-6 notes ek baar revise kar lena.

---

#control-types #preventive #detective #corrective #compensating #left-of-boom #target-2013 #blackpos #ram-scraping #equifax #maersk-notpetya #soar #fail2ban #pci-dss #aaa #shopfast
