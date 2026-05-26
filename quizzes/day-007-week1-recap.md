# Day 007 Quiz — Week 1 Recap (Security Mindset)

**Total:** 50 deep questions — synthesis across Days 1-6
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — The Decision-Loop Model (Q1-Q10)

**Q1. (E · D)** Week-1 ka "decision-loop" 6-step model likh.
> _Hint:_ (1) CIA tag → (2) Identify threat/vuln/risk → (3) Map attack surface + vectors + blast radius → (4) Apply AAA → (5) Layer DiD → (6) Mix control types (P/D/C/Compensating).

**Q2. (M · D)** Yeh loop linear hai ya iterative?
> _Hint:_ Iterative. Threat landscape changes → reassess CIA priorities → controls shift. Architect's continuous loop.

**Q3. (M · R)** Loop me sabse pehle CIA kyun?
> _Hint:_ Without knowing what matters (C/I/A priority), all downstream controls misallocated. CIA = "value of asset" anchor.

**Q4. (M · R)** Risk = T × V × Impact — Day 1 (CIA) kis term mein fit?
> _Hint:_ Impact — directly proportional to which CIA pillar is violated.

**Q5. (H · R)** Attack surface (Day 3) DiD layers (Day 5) ke saath kaise relate?
> _Hint:_ Surface defines what to defend. DiD defines how many concentric defenses. Surface reduction + DiD = lower exposure × higher resilience.

**Q6. (H · R)** AAA (Day 4) DiD ke kis layer pe primary?
> _Hint:_ App + identity layer. Network layer DiD bhi AAA (mTLS) leverages. Multi-layer presence.

**Q7. (M · R)** Control types (Day 6) loop ke last step kyun?
> _Hint:_ After threats + controls identified, classify by function → ensure all 4 functions covered. Quality check, not first decision.

**Q8. (H · R)** Ek scenario likh jahan loop mein step 4 (AAA) miss kiya jaata to kya hota?
> _Hint:_ Encrypted/segmented but unauthenticated access — Capital One-ish via SSRF + IAM. Or insider with valid login but over-scope (Morgan Stanley).

**Q9. (H · R)** Loop ko ek diagram banao text mein.
> _Hint:_ `[CIA] → [T×V=R] → [Surface→Vectors→Blast] → [AAA] → [DiD layers] → [P/D/C/Comp]` → loop back to [CIA] when context changes.

**Q10. (H · R)** "Security mindset" 1-line summary post-Week-1?
> _Hint:_ "Assume something will fail; layer defenses so failure of one doesn't end the story; verify everyone, log everything, recover gracefully."

---

## Section 2 — Cross-Day Vocabulary Test (Q11-Q20)

**Q11. (E · X)** Day 1: 3 letters of CIA + 1-line each (without peeking).
> _Hint:_ Confidentiality (authorized access only), Integrity (no unauthorized change), Availability (accessible when needed).

**Q12. (E · X)** Day 2: Risk = ? equation form.
> _Hint:_ Threat × Vulnerability × Impact (or Likelihood × Impact).

**Q13. (M · X)** Day 3: Attack vector vs surface vs blast radius — 3 lines.
> _Hint:_ Surface = entry points. Vector = path used. Blast radius = compromise spread.

**Q14. (M · X)** Day 4: 3 A's — ek-ek 1-line.
> _Hint:_ AuthN, AuthZ, Accounting (record + non-repudiation).

**Q15. (M · X)** Day 5: Swiss-cheese + assume-breach — ek-ek line.
> _Hint:_ Multi-layer holes don't align if layers diverse. Don't trust perimeter — assume attacker inside.

**Q16. (M · X)** Day 6: 4 function types — alphabetical.
> _Hint:_ Compensating, Corrective, Detective, Preventive. (Plus optional: Deterrent, Directive.)

**Q17. (H · X)** Yeh 6 days mein "single concept" jo tujhe sabse zyada strike kiya — kyun?
> _Hint:_ (Your answer.) Try: assume-breach, or 3rd A, or DiD swiss-cheese.

**Q18. (H · X)** Yeh 6 days ka "least intuitive" concept tere liye?
> _Hint:_ (Your answer.) Common picks: non-repudiation, compensating control, "fail closed vs open".

**Q19. (H · X)** "Crown jewel" har day kahan mention hua?
> _Hint:_ Day 1 (CIA tag), Day 3 (blast radius minimize), Day 4 (AAA tight), Day 5 (DiD concentric), Day 6 (all 4 controls).

**Q20. (H · X)** "Defense vs Offense" Week 1 ne kya orientation diya?
> _Hint:_ Defender mindset, with assume-breach (offensive perspective injected). Week 2 mein STRIDE explicitly adversarial.

---

## Section 3 — Incident Recall (Q21-Q28)

**Q21. (M · I)** Day 3 incident — name + year + records?
> _Hint:_ Capital One 2019, ~106M.

**Q22. (M · I)** Capital One ka attack chain 4 steps?
> _Hint:_ WAF SSRF → IMDS → broad IAM → S3 exfil.

**Q23. (M · I)** Day 4 incident — name + role?
> _Hint:_ Morgan Stanley, Galen Marsh, financial advisor, 730K records.

**Q24. (M · I)** Morgan Stanley primary failure?
> _Hint:_ AuthZ over-scope + weak Accounting (UEBA missing).

**Q25. (M · I)** Day 5 incident — name + year + records?
> _Hint:_ Equifax 2017, 147M.

**Q26. (H · I)** Equifax mein 8 aligned holes (any 5)?
> _Hint:_ Patch miss, asset inventory, vuln scan blind, expired cert/IDS blind, no segmentation, plaintext creds, no egress filter, no DLP.

**Q27. (M · I)** Day 6 incident — name + year?
> _Hint:_ Target 2013.

**Q28. (H · I)** Target — detection fired by which tool? Response failed why?
> _Hint:_ FireEye (also Symantec). SOC team triaged as low / ignored. Process + people failure, not tool failure.

---

## Section 4 — ShopFast Cumulative Application (Q29-Q35)

**Q29. (M · S)** ShopFast pe Week 1 ka 6-step loop apply karke ek complete pass likh (1 line per step).
> _Hint:_ (1) Crown jewels: payment + auth + order DB. (2) Threats: cred-stuffing, Magecart, SSRF, insider. (3) Surface: login, payment, admin, image-proxy. (4) AAA: MFA + role-tight + audit. (5) DiD: WAF→AuthN→AuthZ→encryption→logging. (6) Controls: rate-limit (P) + SIEM (D) + autoscale (C) + manual review (Comp).

**Q30. (M · S)** ShopFast me Capital One repeat na ho — 3 architectural decisions.
> _Hint:_ IMDSv2 enforced, scoped IAM roles, egress block from web tier to IMDS, VPC endpoints for S3.

**Q31. (M · S)** ShopFast me Morgan Stanley repeat na ho — 3 controls.
> _Hint:_ Least-priv per-advisor data scope, UEBA for bulk exports, DLP on outbound, MFA + watermarked downloads.

**Q32. (H · S)** ShopFast me Equifax repeat na ho — 4 controls.
> _Hint:_ Patch SLA + automation, asset inventory + ASM, cert lifecycle automation, network segmentation, egress filtering.

**Q33. (H · S)** ShopFast me Target repeat na ho — 4 controls.
> _Hint:_ Vendor network segmentation, MFA for portals, SOC playbook + IR drills, POS tokenization.

**Q34. (H · S)** ShopFast me CIO ko 2-minute pitch dey: "humare top 3 risks aur kya kar rahe hain".
> _Hint:_ E.g., (1) Magecart on checkout — CSP+SRI mitigation. (2) Insider/AuthZ — UEBA + least-priv. (3) Patch hygiene — CI auto-update + SLA.

**Q35. (H · S)** ShopFast risk register se top-1 risk ka full row kya hoga (Day 2 lab pattern)?
> _Hint:_ Risk · Threat actor · Vector · Likelihood · Impact · Inherent · Controls · Residual · Owner · Review.

---

## Section 5 — Teach-Back (Q36-Q43)

> _Yeh "reverse Claude" wale — har question pe tu junior dev ko samjhata jaisa likh._

**Q36. (M · X)** "CIA Triad" ko junior backend dev ko 30 second mein samjhao.
> _Your teach-back..._

**Q37. (M · X)** "AuthN vs AuthZ" ko 30 second mein.
> _Your teach-back..._

**Q38. (M · X)** "Defense in depth" car analogy ya building analogy mein.
> _Your teach-back..._ Try: locked car door (P) + alarm (D) + immobilizer (C) + steering wheel club (Comp).

**Q39. (M · X)** "Why threat modeling > pentest alone" 60 sec answer (Day 8 preview).
> _Your teach-back..._

**Q40. (H · X)** "Risk equation" sales/PM ko CFO style mein samjhao.
> _Your teach-back..._

**Q41. (H · X)** "Capital One breach" 90 sec mein junior dev ko.
> _Your teach-back..._

**Q42. (H · X)** "Compensating control" PCI auditor ke saamne defend kaise karega?
> _Your teach-back..._

**Q43. (H · X)** "Why detection alone is not enough" Target case se prove kar.
> _Your teach-back..._

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** Week 1 ke concepts ka "zero-trust" reframe — 4 bullets.
> _Hint:_ Never trust (Day 5 assume-breach), Always verify (Day 4 AAA per-hop), Least-privilege (Day 3 blast radius), Continuous monitoring (Day 6 detective).

**Q45. (H · A)** AI/LLM apps mein Week 1 concepts kaise apply hote? Ek-ek line.
> _Hint:_ CIA: model weights + training data. Threats: prompt injection. Surface: tool-calls/RAG. AAA: agent identity. DiD: input + output filters + monitoring. Controls: all 4 needed.

**Q46. (H · A)** Quantum + Week 1 — kis pillar pe sabse zyada threat?
> _Hint:_ Confidentiality (RSA/ECC broken). "Harvest now, decrypt later." Migrate to PQC.

**Q47. (H · A)** "Security architect" vs "Security engineer" — Week 1 ke baad farak?
> _Hint:_ Architect designs the decision-loop. Engineer implements specific controls. Architect sees system holistically.

---

## Section 7 — Self-Assessment & Pace (Q48-Q50)

**Q48. (H · X)** Week 1 mein tera **strongest** topic kaunsa? Aur **shakiest**?
> _Your answer..._

**Q49. (H · X)** Agar koi 1 day re-do karna ho, kaunsa? Kyun?
> _Your answer..._

**Q50. (H · X)** Pace check: 7 days mein 7 concepts — Week 2 ke liye same speed sustainable? Honest answer.
> _Your answer..._

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___
- **Pace check:** sustainable / need slow / need speed-up

---

#week1-recap #cia #risk-equation #attack-surface #aaa #defense-in-depth #control-types #capital-one #morgan-stanley #equifax #target #decision-loop #quiz-day-007
