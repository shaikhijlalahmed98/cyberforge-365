# Day 004 — AAA: Authentication, Authorization & Accounting/Auditing

**Date:** 2026-05-20
**Phase:** Q1 → Month 1 → Week 1 (The Security Mindset)
**Theme:** Teen sawaal har darwaze pe — Tu kaun hai? Tujhe ijaazat hai? Jo tune kiya uska record hai?

---

## Core Concept (in my own words)

> 4-6 sentences mein samjha: AAA ke teen A ka aapas mein kya rishta hai, aur yeh CIA Triad ko kaise enforce karte hain? Pretend you're explaining to a junior backend dev jo sirf "login + roles" tak sochta hai aur teesra A (accounting) ko ignore karta hai.

_Your summary here..._

---

## The Three A's — One-Liner Each

- **Authentication (AuthN):** _(your one-liner)_
- **Authorization (AuthZ):** _(your one-liner)_
- **Accounting/Auditing:** _(your one-liner)_
- **Which order do they run in?** _(answer)_
- **Which two are preventive? Which is detective?** _(answer)_

---

## AuthN vs AuthZ — Say It Cold

- **Authentication =** _("tu kaun hai" / identity)_
- **Authorization =** _("tujhe kya allowed" / permission)_
- **401 =** _(AuthN or AuthZ?)_
- **403 =** _(AuthN or AuthZ?)_
- **IDOR/BOLA is a failure of:** _(AuthN / AuthZ-coarse / AuthZ-fine)_

---

## The Three Factors (recall karke likh)

1. Something you **know** → _(example)_
2. Something you **have** → _(example)_
3. Something you **are** → _(example)_
4. **MFA ki sahi definition** (do passwords kyun MFA nahi?) → _(answer)_
5. (Emerging) Somewhere you are / something you do → _(answer)_

---

## The 5 W's of an Audit Log

1. **Who** →
2. **What** →
3. **When** →
4. **Where** →
5. **Outcome** →

**3 cheezein jo audit log mein KABHI nahi:**
1.
2.
3.

**3 rules jo log ko tamper-evident banayein:**
1.
2.
3.

---

## Non-Repudiation

- **Definition (apne shabdon):**
- **3 cheezein jo isay enable karti hain:**
  1.
  2.
  3.
- **CIA Triad mein kyun nahi thi? Kaunsa extended model isay add karta hai?**

---

## Self-Check Quiz Answers

Answer **without peeking**. Mark difficulty: (E)asy / (M)edium / (H)ard. Galat ya peek kiya toh ❌.

**Q1. (E) AuthN, AuthZ, Accounting define + order.**
_Answer..._

**Q2. (E) 401 vs 403 in AAA terms.**
_Answer..._

**Q3. (M) "Two passwords is MFA" kyun galat? Real MFA definition.**
_Answer..._

**Q4. (M) `[Authorize(Roles="Customer")]` laga di — kaunsa AAA failure abhi bhi possible? Naam + fix.**
_Answer..._

**Q5. (M) Audit log ke 5 W's + 3 cheezein jo kabhi log na ho.**
_Answer..._

**Q6. (H) Non-repudiation kya hai? 3 enablers. CIA mein kyun nahi thi?**
_Answer..._

**Q7. (H) Morgan Stanley: authN theek tha phir bhi breach — kis A ka failure + ek architectural control.**
_Answer..._

**Q8. (H — ShopFast) `POST /api/refunds` ke liye teeno A ke ek-ek control (AuthN strength, AuthZ coarse+fine, audit 5 W's).**
_Answer..._

**Q9. (H — Mera project) System-X2 ka ek sensitive endpoint: AuthN/AuthZ(coarse+fine)/Accounting — kya hai, kya missing.**
_Answer..._

**Q10. (H — Futurist) API keys ka future replacement (naam + why) + AI agents ke liye accounting kyun naya frontier.**
_Answer..._

---

## Journal Question

> "Apne kisi production project ka ek sensitive action soch (refund, delete, export). Kya tujhe *aaj* bata sakte ho ke wo action **kisne, kab, kahan se** kiya — ek tamper-proof record se? Ya tere paas sirf debug logs hain jo developer edit kar sakta hai? Agar audit nahi hai, pehla audit event kaunsa add karega?"

_Answer here..._

---

## Connection to System-X2 / My Backend Work

> "Microservices mein service-to-service calls — abhi wo ek-doosre ko *kaise* authenticate karte hain? Shared API key? Network trust ('andar wala safe hai')? Yeh AAA ke first-A (machine authentication) ka kaunsa weak pattern hai, aur SPIFFE/mTLS isay kaise theek karega?"

_Answer here..._

**Current service-to-service auth (honest):**

**Weak pattern it represents:**

**SPIFFE/mTLS fix (one line):**

---

## Morgan Stanley (2015) Takeaways

- The chain (which A worked, which failed):
- Why authentication being fine made it WORSE (insider trap):
- Why authorization (need-to-know/least-privilege) was the core failure:
- Why accounting was "partial" (post-hoc, not real-time):
- One architectural control I will remember (UEBA? least privilege? DLP?):

---

## Things That Stuck

1.
2.
3.

## Things I Need to Re-read

1.
2.

## Terms That Need More Practice

- [ ] AuthN vs AuthZ (say it cold)
- [ ] 401 vs 403 reflex
- [ ] Three factors + real MFA definition
- [ ] IDOR/BOLA as fine-grained authZ failure
- [ ] 5 W's of audit log
- [ ] Non-repudiation + Parkerian Hexad
- [ ] "AuthN once, AuthZ every time, Accounting always"
- [ ] RADIUS/TACACS+ origin of AAA
- [ ] SPIFFE/workload identity (machine authN)

---

## Lab Status

- [ ] Lab `labs/2026-05-20-day4-aaa-shopfast-audit.md` started
- [ ] Part A: AAA mapping table (8 endpoints, 4 columns)
- [ ] Part B: IDOR found + fixed handler + audit event + correct status
- [ ] Part C: audit schema (5 W's + 2 extra) + tamper rules + never-log + anomaly rule
- [ ] Part D: JWT claims classified + alg:none explained + 4 validations
- [ ] Part E: CTO recommendation with Morgan Stanley tie-in
- [ ] (Bonus) Ran AAA audit on a real project endpoint

---

## Futurist Angle (3-5 years)

> AuthN → passkeys/FIDO2 + SPIFFE workload identity. AuthZ → policy-as-code (OPA/Cedar) + continuous authorization (zero-trust). Accounting → tamper-evident logs + AI agent non-repudiation. In mein se ek pe 2 lines apne shabdon mein:

_Your answer..._

---

## Tomorrow's Preview

**Day 5 → Defense-in-Depth.** AAA ek layer hai — agar wo fail ho to peeche kya? Multiple overlapping controls, har ek doosre ka backup. "Ek galti = game over" ko rokna.

---

#aaa #authentication #authorization #accounting #non-repudiation #idor #mfa #passkeys #spiffe #morgan-stanley #equifax
