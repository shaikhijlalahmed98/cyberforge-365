# Day 004 Quiz — AAA: Authentication, Authorization, Accounting/Auditing

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** AAA ke 3 A's full form aur 1-line meaning.
> _Hint:_ Authentication (kaun ho?), Authorization (kya allowed ho?), Accounting/Auditing (kya kiya, kab, kaise — non-repudiable record).

**Q2. (E · D)** AuthN vs AuthZ — 1 line farak.
> _Hint:_ AuthN = identity proof. AuthZ = permission to act on resource.

**Q3. (M · D)** "Authentication factor" 3 categories likh + ek-ek example.
> _Hint:_ Knowledge (password, PIN). Possession (token, phone, smartcard). Inherence (fingerprint, face, retina).

**Q4. (M · D)** MFA vs 2FA — same hain?
> _Hint:_ 2FA = exactly 2 factors. MFA = 2 or more. Often used interchangeably.

**Q5. (E · D)** RBAC, ABAC, ReBAC — ek-ek line.
> _Hint:_ RBAC = role-based. ABAC = attribute-based (context-aware). ReBAC = relationship-based (Google Zanzibar).

**Q6. (M · D)** "Authorization" claims-based system mein kahan stored?
> _Hint:_ JWT claims, OAuth scopes, OIDC ID-token, server-side policy engine (OPA).

**Q7. (M · D)** "Accounting" vs "Logging" — same?
> _Hint:_ Logging = all events. Accounting = security-relevant subset (who-did-what-when), tamper-evident, retained.

**Q8. (H · D)** Non-repudiation kya hai? Kaise enforce hota?
> _Hint:_ Actor cannot deny action. Enforced via signed audit logs, separation of duties, cryptographic signatures on actions.

**Q9. (M · D)** Federation vs SSO — farak.
> _Hint:_ SSO = one auth, multiple apps. Federation = trust between identity domains (SAML/OIDC across orgs).

**Q10. (H · D)** "Step-up authentication" matlab + use case.
> _Hint:_ Sensitive action triggers additional auth (e.g., re-MFA before large transfer). Risk-adaptive.

---

## Section 2 — Concept Relationships (Q11-Q20)

**Q11. (M · R)** AuthN strong + AuthZ kamzor → kya hota?
> _Hint:_ Authenticated user can access resources they shouldn't (IDOR, BOLA, horizontal/vertical privesc).

**Q12. (M · R)** AuthZ strong + AuthN kamzor → kya hota?
> _Hint:_ Attacker impersonates legit user (credential stuffing, phishing) — AuthZ ke saare checks useless.

**Q13. (H · R)** AuthN + AuthZ dono solid, Accounting missing → kya risk?
> _Hint:_ Insider threat undetectable (Morgan Stanley case). No forensic trail. Repudiation possible.

**Q14. (M · R)** Why "the 3rd A is the under-loved one"?
> _Hint:_ Developers focus on gatekeeping (N+Z). Accounting feels like "after-the-fact" busywork — until breach forensics fails.

**Q15. (M · R)** RBAC ki limitation in microservices/multi-tenant — example.
> _Hint:_ "Role explosion" — too many roles per tenant; can't express "edit this specific doc." ABAC/ReBAC solves.

**Q16. (H · R)** OAuth2 vs OIDC — AAA mein kahan baith?
> _Hint:_ OAuth2 = AuthZ framework (token issuance). OIDC = identity layer (AuthN) on top. Common mistake: OAuth2 used as auth.

**Q17. (H · R)** JWT signature verify but expiration not checked → kaunsa A failure?
> _Hint:_ AuthZ failure indirectly (stale tokens accepted). AuthN trust = unbounded.

**Q18. (M · R)** "Privilege creep" definition + AAA pe impact.
> _Hint:_ User accumulates permissions over time (role joins). AuthZ becomes silently over-broad. Regular access reviews fix.

**Q19. (H · R)** Service account vs user account — Accounting challenges?
> _Hint:_ Service accounts shared across systems → "who did this?" ambiguous. Need per-workload identity (SPIFFE).

**Q20. (H · R)** "Break-glass" account — kya hai aur Accounting requirement?
> _Hint:_ Emergency admin access bypassing normal flows. Must be: pre-approved, time-boxed, alarmed, audited heavily.

---

## Section 3 — ShopFast Application (Q21-Q28)

**Q21. (M · S)** ShopFast login flow mein AuthN failure scenarios — 3.
> _Hint:_ Weak password policy, no MFA, credential stuffing without rate-limit, JWT 'none' algo, OAuth state CSRF.

**Q22. (M · S)** ShopFast admin dashboard AuthZ failure — 1 example.
> _Hint:_ IDOR — `/admin/order/{id}` returns any order regardless of role. Vertical privesc.

**Q23. (H · S)** ShopFast warehouse picker login — kis A pe sabse zyada attention?
> _Hint:_ AuthN (shared device, theft risk) + Accounting (track who shipped what, returns/fraud).

**Q24. (M · S)** ShopFast customer cart pe user `userId=123` ko `userId=124` se modify kare — kaunsa attack?
> _Hint:_ BOLA / IDOR (Broken Object Level Authorization). OWASP API #1.

**Q25. (H · S)** ShopFast me refund button only Finance role — implement kaise?
> _Hint:_ RBAC. ASP.NET `[Authorize(Roles="Finance")]` or OPA policy. Plus client-side hide is UX only — server is authoritative.

**Q26. (M · S)** ShopFast me delivery driver app — AAA design?
> _Hint:_ AuthN: phone OTP + device binding. AuthZ: scope = "today's deliveries only." Accounting: GPS + delivery proof signed.

**Q27. (H · S)** ShopFast me audit log mein "admin deleted order" but admin denies — repudiation kaise prevent?
> _Hint:_ Signed action receipts (HMAC with admin's session key). Append-only WORM log. Multiple-party signoff (4-eyes).

**Q28. (M · S)** ShopFast me 3rd-party PSP integration — federated auth flow?
> _Hint:_ OAuth2 client credentials between ShopFast and PSP. PSP returns transaction ID. Both sides log.

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** ASP.NET Core mein AuthZ policies kaise define?
> _Hint:_ `services.AddAuthorization(opts => opts.AddPolicy("FinanceOnly", p => p.RequireRole("Finance")))` + `[Authorize(Policy="FinanceOnly")]`.

**Q30. (M · B)** Spring Security ke filter chain mein AuthN aur AuthZ kahan?
> _Hint:_ `AuthenticationFilter` (UsernamePasswordAuth, BearerToken) → `AuthorizationFilter` (FilterSecurityInterceptor) → controller.

**Q31. (H · B)** Microservices mein "AuthN at gateway, AuthZ at service" — pros aur cons?
> _Hint:_ Pro: centralized AuthN. Con: services must trust gateway (mTLS) or verify token independently. Best: do both ("zero-trust internal").

**Q32. (H · B)** OPA (Open Policy Agent) System-X2 mein integrate karne ki value?
> _Hint:_ Policy-as-code, separate from app logic, versioned, testable. Decouples AuthZ from business code.

**Q33. (M · B)** JWT in microservices — kahan validate karein? Per-service ya gateway only?
> _Hint:_ Both ideal. Gateway validates signature. Services re-validate (defense-in-depth + scope checking).

**Q34. (H · B)** "Token sidecar" pattern (e.g., Envoy ext_authz) — AAA mein kya benefit?
> _Hint:_ AuthN/AuthZ offloaded from app code. Languages-agnostic. Centralized policy.

**Q35. (H · B)** Audit log table design — append-only kaise enforce DB level?
> _Hint:_ Grant only INSERT to app role; no UPDATE/DELETE. Trigger to reject UPDATE. WORM storage. Optional: HMAC chain (each row signs prev row hash).

---

## Section 5 — Incident: Morgan Stanley Insider (Q36-Q43)

**Q36. (M · I)** Morgan Stanley financial advisor insider case year + brief?
> _Hint:_ Galen Marsh, 2014-2015. Financial advisor accessed ~730K client records, took data home, eventually leaked online.

**Q37. (M · I)** Yeh AuthN failure tha ya AuthZ?
> _Hint:_ AuthZ failure — his role allowed access to more records than business need. Plus weak Accounting/DLP.

**Q38. (H · I)** Yeh case 3rd A (Accounting) ka role kaise expose karta?
> _Hint:_ MS detected after data appeared online (external). No internal anomaly detection on advisor's access volume. Accounting was reactive, not proactive.

**Q39. (H · I)** Architectural control jo prevent karta — 3 options.
> _Hint:_ (1) Least-privilege query scope (client-of-advisor only). (2) UEBA — detect anomalous data exports. (3) DLP — prevent bulk download. (4) MFA + watermarked downloads.

**Q40. (M · I)** Why insiders bypass perimeter security?
> _Hint:_ Already inside trust zone. AuthN done legitimately. Only AuthZ + Accounting can catch.

**Q41. (H · I)** Penalty + post-incident architecture changes?
> _Hint:_ FINRA $1M, SEC settlement. Marsh got probation. MS adopted UBA + tighter role scoping + monitored client export volumes.

**Q42. (M · I)** "Trust but verify" yeh kahawat AAA ke kis context mein fit?
> _Hint:_ Insider/employee — authenticated trust + ongoing verification via Accounting/UEBA. Aage zero-trust isi ka extreme.

**Q43. (H · I)** Compare with: Edward Snowden — kya AAA failure?
> _Hint:_ Similar pattern — privileged sysadmin AuthZ too broad + Accounting limited. Plus shared SSO credentials reportedly used.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** Passwordless future — FIDO2/WebAuthn ka core idea?
> _Hint:_ Public-key crypto. Browser/device holds private key. Site verifies signature. Phishing-resistant.

**Q45. (H · A)** Passkeys vs hardware key — UX vs security tradeoff?
> _Hint:_ Passkeys = synced across devices (iCloud/Google) — usable. Hardware key = strongest but device-bound, lossable.

**Q46. (H · A)** "Continuous authentication" — kya hai?
> _Hint:_ Behavioral biometrics + risk scoring throughout session, not just login. Re-prompt on anomaly.

**Q47. (H · A)** Decentralized identity (DIDs, verifiable credentials) AAA ko kaise badle?
> _Hint:_ User-owned identity, presented as VCs. AuthN without central IdP. Selective disclosure (e.g., "age > 18" without DOB).

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** STRIDE (Day 9) AAA ko negate karta — kaunsa S, R, E?
> _Hint:_ Spoofing → AuthN. Repudiation → Accounting. Elevation of Privilege → AuthZ.

**Q49. (H · X)** CIA (Day 1) + AAA mil ke "complete security model" kaise banate?
> _Hint:_ CIA = what to protect. AAA = how to control access + verify. Together: "data with controlled, auditable access."

**Q50. (H · X)** Trust boundary (Day 11) crossing pe AAA ke kaunse element check?
> _Hint:_ AuthN (caller identity verified?), AuthZ (caller allowed for action?), Accounting (record the crossing).

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#aaa #authentication #authorization #accounting #audit #non-repudiation #rbac #abac #morgan-stanley #fido2 #passkeys #quiz-day-004
