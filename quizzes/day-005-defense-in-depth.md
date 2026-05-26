# Day 005 Quiz — Defense in Depth

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** Defense-in-depth (DiD) ki 1-line definition.
> _Hint:_ Multiple independent layers of security, designed so failure of one is caught/contained by next.

**Q2. (E · D)** "Swiss cheese model" ka analogy DiD ke liye.
> _Hint:_ Har layer = ek cheese slice with holes. Attack stops jab koi slice's solid section line of holes ko block kar de.

**Q3. (E · D)** "Assume breach" mindset kya hai?
> _Hint:_ Don't ask "if breach"; ask "when". Design assuming attacker is already inside. Limits trust + maximizes detection/containment.

**Q4. (M · D)** DiD ke 3 dimensions/layers kya hain (classic categorization)?
> _Hint:_ Administrative (policies, training), Technical (firewalls, IAM, crypto), Physical (locks, biometrics, datacenter).

**Q5. (M · D)** "Layered controls" vs "redundant controls" — farak.
> _Hint:_ Layered = different control types at different stages. Redundant = same control duplicated (e.g., 2 firewalls). Both useful but distinct.

**Q6. (M · D)** "Diversity of defense" ka principle?
> _Hint:_ Different vendors/implementations per layer — same vuln won't kill all layers. e.g., not all Cisco, mix Palo Alto + Fortinet.

**Q7. (E · D)** "Perimeter" defense kyun fail hota modern times mein?
> _Hint:_ Cloud, mobile, SaaS, remote work — no clear perimeter. "Castle and moat" obsolete. Zero-trust replaces.

**Q8. (M · D)** Network DiD ke 4 layers example.
> _Hint:_ Edge firewall → IDS/IPS → Internal segmentation → Host firewall → App-layer WAF.

**Q9. (H · D)** Application DiD ke 4 layers.
> _Hint:_ Input validation → AuthN → AuthZ → Output encoding → Logging/monitoring. (More: rate-limit, WAF, CSP, SRI.)

**Q10. (H · D)** "Compensating control" DiD mein kya role play karta?
> _Hint:_ Jab primary control feasible nahi, alternate control jo equivalent risk reduction de (Day 6 detail).

---

## Section 2 — Concept Relationships (Q11-Q20)

**Q11. (M · R)** DiD aur Zero-Trust — competing ya complementary?
> _Hint:_ Complementary. DiD = "multiple layers". ZT = "every layer verifies independently". ZT enforces DiD at granular level.

**Q12. (M · R)** Single layer mein heavy investment vs thin layers across — kis approach DiD prefers?
> _Hint:_ Thin layers across (depth > height). One impenetrable wall fails catastrophically; multiple checkpoints fail gracefully.

**Q13. (H · R)** DiD aur blast radius (Day 3) ka relation?
> _Hint:_ Each layer = a containment boundary. More layers → smaller blast per breach.

**Q14. (M · R)** "Holes in cheese aligned" — kya iska real example?
> _Hint:_ All layers share same misconfig (e.g., default creds across firewall + IDS + app). Diversity defeats this.

**Q15. (H · R)** Cost of layers vs marginal risk reduction — kab "diminishing returns" aata?
> _Hint:_ Marginal layer cost > marginal risk × asset value. Quantify via FAIR.

**Q16. (M · R)** "Outside-in" vs "Inside-out" DiD design?
> _Hint:_ Outside-in: classic perimeter-first. Inside-out: crown-jewel-first (most rings around the most valuable asset).

**Q17. (H · R)** DiD ka relationship with detection vs prevention?
> _Hint:_ Layers include both. Prevention slows attacker, detection catches once a layer is breached. Both needed.

**Q18. (M · R)** Microservices DiD — every service its own perimeter — pros/cons?
> _Hint:_ Pro: tiny blast radius. Con: operational complexity, network overhead, more attack surface (more endpoints).

**Q19. (H · R)** "Bypass" attack DiD ko kaise defeat?
> _Hint:_ Goes around layers (e.g., insider, supply-chain, social eng) instead of through. Need DiD on alternate paths too.

**Q20. (H · R)** "Single sign-on" DiD ke against argument kya?
> _Hint:_ SSO = one credential = one breach = many systems. Compensate with MFA, session monitoring, step-up auth.

---

## Section 3 — ShopFast Application (Q21-Q28)

**Q21. (M · S)** ShopFast crown jewels par DiD layers list — payment service ke around 5+ layers.
> _Hint:_ Edge: WAF + DDoS. Network: VPC + segmentation. App: input validation + AuthN + AuthZ. Data: tokenization + encryption + DB audit. Egress: outbound filtering + DLP.

**Q22. (M · S)** ShopFast login DiD layers minimum 4?
> _Hint:_ Rate limiting + MFA + bot/captcha + impossible-travel detection + password breach check (HIBP).

**Q23. (H · S)** ShopFast checkout XSS prevent karne ke 4 layers?
> _Hint:_ Input sanitization → output encoding → CSP header → SRI on 3rd-party scripts → browser SameSite cookies.

**Q24. (H · S)** ShopFast admin panel breach hone par damage limit kaise?
> _Hint:_ Network: VPN-only + jump host. AuthN: hardware-key MFA. AuthZ: just-in-time elevation. Data: encrypted secrets in vault. Audit: every action logged + alarmed.

**Q25. (M · S)** ShopFast cloud account-level DiD — 4 controls?
> _Hint:_ Org SCPs + IAM least-priv + GuardDuty + CloudTrail + Config Rules + Security Hub.

**Q26. (H · S)** ShopFast warehouse network DiD pe AC failed (Target-style) — kya hota agar layers thik hote?
> _Hint:_ HVAC partner network → segmented from POS (no flat network). POS → tokenized. Egress filtered (no C2). Detection from CloudTrail/SIEM.

**Q27. (M · S)** ShopFast me CDN+WAF+RASP teeno kyun?
> _Hint:_ CDN absorbs DDoS. WAF filters known patterns. RASP catches runtime context (e.g., SQLi after eval). Three-layer app defense.

**Q28. (H · S)** ShopFast me PCI DSS compliance ke 5 DiD layers ka mapping?
> _Hint:_ Network seg (Req 1), Encryption (Req 3-4), Vuln mgmt (Req 11), AuthN/Z (Req 7-8), Logging (Req 10).

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** Microservice deployment ke DiD: code → prod kitne layers?
> _Hint:_ SAST in IDE → pre-commit hooks → CI SAST/SCA → image scan → admission controller → runtime (Falco/Tetragon) → WAF → app-layer.

**Q30. (M · B)** Spring Boot app DiD application layer pe — 4 layers.
> _Hint:_ Spring Security AuthN/Z → method-level `@PreAuthorize` → input validation (`@Valid`) → output sanitization → logging interceptor.

**Q31. (H · B)** Container DiD layers (host → container → process)?
> _Hint:_ Host hardening (CIS) → kernel hardening (seccomp/AppArmor) → image scanning → runtime detection → network policies → pod security standards.

**Q32. (M · B)** Database DiD layers.
> _Hint:_ Network ACL + TLS + AuthN + RBAC (row-level) + column encryption + DAM (DB activity monitoring) + backup encryption.

**Q33. (H · B)** "Defense-in-depth in CI/CD" — 5 controls.
> _Hint:_ Signed commits (Sigstore), secret scanning, SAST, SCA, container scan, IaC scan (Checkov), policy-as-code (OPA Gatekeeper), artifact signing.

**Q34. (H · B)** "Just-in-time access" DiD mein kahan fit?
> _Hint:_ AuthZ layer — privileges granted only for task duration, then revoked. Lateral movement window shrinks.

**Q35. (H · B)** Service mesh DiD value-add over plain network?
> _Hint:_ mTLS + per-service identity + L7 authZ + observability. Replaces "trust within cluster" with verifiable hops.

---

## Section 5 — Incident: Equifax 2017 (Q36-Q43)

**Q36. (M · I)** Equifax breach year + records exposed?
> _Hint:_ 2017. 147M consumers (US + UK + Canada).

**Q37. (M · I)** Initial vulnerability + CVE?
> _Hint:_ Apache Struts CVE-2017-5638 (RCE via Content-Type header). Public disclosure March 7, 2017.

**Q38. (M · I)** Patch availability vs exploitation timeline?
> _Hint:_ Patch March 7. Exploitation began mid-May. Equifax discovered late-July. ~60 day gap.

**Q39. (H · I)** Day 5 lens — Equifax mein kitne layers ne fail kiya? List "8 aligned holes."
> _Hint:_ (1) Patch mgmt missed. (2) Asset inventory incomplete. (3) Vuln scanner couldn't auth-scan. (4) Cert renewal lapsed → network monitor blind for 10 months. (5) No network segmentation → lateral. (6) Plain-text creds in repo. (7) No outbound egress filtering → 76 days of exfil. (8) No data classification/DLP.

**Q40. (H · I)** "Expired SSL cert" detection-blindness ka root cause failure?
> _Hint:_ Cert expired → IDS decrypt SSL nahi kar sakta → 10 months blind. Cert mgmt = often overlooked DiD layer.

**Q41. (M · I)** Settlement + executive impact?
> _Hint:_ $700M+ settlement (FTC + states). CIO and CSO resigned. CEO retired.

**Q42. (H · I)** Architectural lessons mapped to DiD layers.
> _Hint:_ Patch hygiene (admin DiD), asset inventory (foundational), TLS cert lifecycle (technical), network seg (technical), egress monitoring (technical), data tokenization (technical).

**Q43. (H · I)** Compare Equifax (DiD failure) with Capital One (single-point IAM): kis-ka DiD primary issue?
> _Hint:_ Equifax = 8-layer cascade. Capital One = mainly 1-2 (broad IAM + IMDSv1). Both DiD failures, different shapes.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** "Zero-trust" DiD ka evolution ya replacement?
> _Hint:_ Evolution — DiD principle stays, perimeter assumption removed. ZT = DiD at every hop, not just edge.

**Q45. (H · A)** AI in DiD — kahan add hota?
> _Hint:_ Detection (UEBA), triage (SOAR), threat hunting (ML over SIEM), runtime anomaly (eBPF + ML). Caveat: adversarial ML attacks.

**Q46. (H · A)** SASE (Secure Access Service Edge) DiD model ko kaise re-shape karta?
> _Hint:_ Cloud-delivered DiD — SD-WAN + ZTNA + SWG + CASB + FWaaS. Network + security converged.

**Q47. (H · A)** Confidential computing DiD ka kaunsa naya layer add karta?
> _Hint:_ Data-in-use protection (TEE/enclave). Beyond at-rest + in-transit. Cloud provider can't see data.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** Day 6 (control types — preventive/detective/corrective/compensating) DiD ke har layer mein 4 type honi chahiye?
> _Hint:_ Ideally yes. Each layer has prevention + detection + recovery + (sometimes) compensating fallback. Mature DiD = all 4.

**Q49. (H · X)** STRIDE-per-element (Day 9) DiD ke design phase mein kaise help karta?
> _Hint:_ Per element, per threat → identify which layer mitigates. DFD + STRIDE → DiD blueprint.

**Q50. (H · X)** Blast radius (Day 3) DiD ke output metric ke roop mein kaise use?
> _Hint:_ After-DiD blast = small footprint. Measure: "agar layer N break ho, layer N+1 ka radius kya?" — kaise containment graph.

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#defense-in-depth #swiss-cheese-model #assume-breach #equifax-2017 #zero-trust #layered-security #quiz-day-005
