# Day 003 — Attack Surface, Attack Vectors & Blast Radius

**Date:** 2026-05-19
**Phase:** Q1 → Month 1 → Week 1 (The Security Mindset)
**Theme:** Kitne darwaze (surface), kis raaste (vector), kitni door tak (blast radius)

---

## Core Concept (in my own words)

> 4-6 sentences mein samjha: attack surface, attack vector, aur blast radius ka aapas mein kya rishta hai? Pretend you're explaining to a junior backend dev jo abhi naya endpoint add karke khush ho raha hai.

_Your summary here..._

---

## The Three Words — One-Liner Each

- **Attack surface:** _(your one-liner)_
- **Attack vector:** _(your one-liner)_
- **Blast radius:** _(your one-liner)_
- **Which maps to `Likelihood`?** _(surface / vector / blast radius)_
- **Which maps to `Impact`?** _(surface / vector / blast radius)_

---

## The Four Faces of Attack Surface (recall karke likh)

1. Network surface →
2. Software/application surface →
3. Human/social surface →
4. Physical surface →
5. (Cloud era) Cloud-config surface →

---

## Self-Check Quiz Answers

Answer **without peeking**. Mark difficulty: (E)asy / (M)edium / (H)ard. Galat ya peek kiya toh ❌.

**Q1. Surface / vector / blast radius — ek line each + which maps to Likelihood vs Impact.**
_Answer..._

**Q2. Attack surface ke 4-5 faces; cloud era mein decisive kaunsa aur kyun.**
_Answer..._

**Q3. "Least functionality" kya hai? Log4Shell se explain.**
_Answer..._

**Q4. Spring Boot Actuator ki galat config + secure version. Kyun secure?**
_Answer..._

**Q5. SSRF apne shabdon mein (kyun "forgery"?). Capital One mein kaunsa internal address, kya nikla?**
_Answer..._

**Q6. Capital One mein sirf IMDSv2 YA scoped IAM hota — breach itna bada hota? Defense-in-depth reasoning.**
_Answer..._

**Q7. North-south vs east-west; blast radius kis mein? Microservices mein east-west contain kaise?**
_Answer..._

**Q8. (ShopFast) RecommendationService pop hua. Flat network vs default-deny NetworkPolicy blast radius. 2 controls taake PaymentService reachable na ho.**
_Answer..._

**Q9. (Mera project) System-X2 ka ek internet-facing endpoint: surface, ek vector, blast radius. Pehla control kaunsa?**
_Answer..._

**Q10. (H) Surface 0 possible nahi (zero residual risk wala sabaq). Architect ka asli goal surface aur blast radius dono ke liye — ek line each.**
_Answer..._

---

## Journal Question

> "Apne kisi production project (System-X2 ya koi aur) ka network soch — kya wo 'crunchy outside, soft inside' tha (perimeter strong, internal services aapas mein freely baat karte the)? Agar ek service compromise hoti, blast radius kitna bada hota? Aaj ke baad tu sabse pehla east-west control kaunsa lagayega?"

_Answer here..._

---

## Connection to System-X2 / My Backend Work

> "Tu microservices mein resilience ke liye blast radius already control karta hai (circuit breakers, bulkheads). Wahi mental muscle security blast radius pe kaise apply hota hai? Resilience aur security blast-radius controls mein kya overlap hai, kya farq?"

_Answer here..._

**Resilience blast-radius controls I already use:**
1.
2.

**Security blast-radius controls (new) I should add:**
1.
2.

---

## Capital One (2019) Takeaways

- The chain (surface → vector → privilege → data → detection):
- Why SSRF + IMDSv1 was the enabling combo:
- Why over-broad IAM role was the real catastrophe:
- One architectural control I will remember (IMDSv2? least privilege? egress detection?):

---

## Things That Stuck

1.
2.
3.

## Things I Need to Re-read

1.
2.

## Terms That Need More Practice

- [ ] Surface vs vector distinction (say it cold)
- [ ] North-south vs east-west traffic
- [ ] SSRF + IMDS attack path
- [ ] IMDSv1 vs IMDSv2 (why v2 blocks SSRF)
- [ ] Least privilege as blast-radius control
- [ ] NetworkPolicy default-deny pattern
- [ ] Zero trust = blast radius shrunk to one service

---

## Lab Status

- [ ] Lab `labs/2026-05-19-day3-attack-surface-blast-radius.md` started
- [ ] Part A: surface inventory ≥8 entries, all 4 types
- [ ] Part B: vector mapping for 3 high-value surfaces
- [ ] Part C: flat + segmented blast-radius maps
- [ ] Part D: real tool output pasted (nmap / actuator / IAM)
- [ ] Part E: 3 control snippets for ShopFast
- [ ] Part F: CTO recommendation with Capital One tie-in
- [ ] (Bonus) Traced IMDSv1 vs IMDSv2 path on a cloud VM

---

## Futurist Angle (3-5 years)

> Zero trust default kaise blast radius ko 1 service tak shrink karta hai? eBPF/Cilium, SPIFFE workload identity, aur AI agent tool-calling ka blast radius — in mein se ek pe 2 lines.

_Your answer..._

---

## Tomorrow's Preview

**Day 4 → AAA: Authentication, Authorization, Accounting/Auditing.** Wo teen mechanisms jo har darwaze pe poochte hain: "tu kaun hai? tujhe ijaazat hai? jo tune kiya uska record hai?" — blast radius control ka identity-side foundation.

---

#attack-surface #attack-vector #blast-radius #ssrf #imdsv2 #least-privilege #zero-trust #capital-one #microservices
