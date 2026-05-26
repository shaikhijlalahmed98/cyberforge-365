# Day 011 Quiz — Trust Boundaries

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** Trust boundary ki 1-line definition.
> _Hint:_ Logical line where trust level changes. Assumptions valid on one side may not hold on the other. Verification required at the crossing.

**Q2. (E · D)** Trust = ? (real meaning).
> _Hint:_ Implicit assumption that an actor/code/process/input honors a specific property. NOT "good intentions."

**Q3. (M · D)** Trust boundary "node" hai ya "line"?
> _Hint:_ Line — yeh DFD pe element nahi. Edge/crossing/relationship.

**Q4. (M · D)** 5 trust boundary types?
> _Hint:_ Network, Process, Privilege/Role, Code-origin, Organizational.

**Q5. (M · D)** "Wall" vs "Checkpoint" mental model?
> _Hint:_ Wall = nothing passes. Checkpoint = passes but verified. Trust boundaries are checkpoints, not walls. Real walls = air-gaps only.

**Q6. (M · D)** "Implicit trust" vs "Explicit trust"?
> _Hint:_ Implicit = assumed without verification (default-allow). Explicit = verified per crossing (AuthN/AuthZ/mTLS). Zero-trust = always explicit.

**Q7. (M · D)** Network boundary 3 examples.
> _Hint:_ DMZ ↔ internal VLAN, VPC peering, public ↔ private subnet, on-prem ↔ cloud, region ↔ region.

**Q8. (M · D)** Process boundary 3 examples.
> _Hint:_ Kernel ↔ userland, container ↔ host, parent ↔ child process, sandbox boundary, VM ↔ hypervisor.

**Q9. (M · D)** Privilege boundary 3 examples.
> _Hint:_ Anonymous → Authenticated → Admin. sudo crossing. Service account elevation. Just-in-time elevation requests.

**Q10. (H · D)** Code-origin boundary 3 examples.
> _Hint:_ 1st-party ↔ 3rd-party JS, vendor library in your binary, npm package, container image, build artifact provenance.

---

## Section 2 — Per-Crossing Question Framework (Q11-Q20)

**Q11. (M · R)** Crossing pe 3 mandatory questions?
> _Hint:_ (1) What assumption am I making? (2) What if assumption fails? (3) What enforces the assumption?

**Q12. (M · R)** Crossing-pe-control 4 types example (DiD).
> _Hint:_ Preventive (AuthN/AuthZ/encrypt), Detective (log+monitor), Corrective (revoke + auto-rollback), Compensating (manual approval if primary fails).

**Q13. (H · R)** Crossing has data flow + caller — kis ke liye AuthN?
> _Hint:_ Caller (workload/user). Plus optional message-level AuthN (signed payload).

**Q14. (M · R)** Crossing without any control = ?
> _Hint:_ "Flat" zone — implicit trust. Most breaches exploit this (Target, Equifax internal pivot).

**Q15. (H · R)** Crossing pe "fail-safe default" matlab?
> _Hint:_ When in doubt, deny. Default-deny network rules, deny-by-default RBAC.

**Q16. (M · R)** Microservices = "every call is a crossing" — kyun?
> _Hint:_ Monolith trust internal (cheap). Microservices: network call = different process/host/possibly tenant. Each call = boundary.

**Q17. (H · R)** mTLS crossing pe kya verify karta?
> _Hint:_ Both sides' identities via certs. Both: AuthN. Encryption: C. Tamper-prevention: I. 3-in-1.

**Q18. (H · R)** SPIFFE workload identity crossing pe kya solve karta?
> _Hint:_ Workload identity decoupled from network (IP). Cryptographic, attested, automatable. Replaces "IP allowlist" with "service-X allowlist."

**Q19. (H · R)** "Lateral movement" trust boundaries ke against kya represent karta?
> _Hint:_ Attacker hops across boundaries by abusing implicit trust between segments. Zero-trust kills lateral movement by removing implicit trust.

**Q20. (H · R)** "East-west" vs "north-south" traffic boundary?
> _Hint:_ N-S = external ↔ internal (perimeter, classical defended). E-W = service ↔ service (often un-defended, blast radius high).

---

## Section 3 — ShopFast Application (Q21-Q28)

**Q21. (M · S)** ShopFast L1 mein 6 boundary crossings list.
> _Hint:_ Browser ↔ API GW; API GW ↔ Order Svc; Order Svc ↔ DB; Order Svc ↔ Payment Svc; Payment Svc ↔ PSP (external org); Audit Svc ↔ Log store; Browser ↔ 3rd-party JS (code-origin).

**Q22. (M · S)** ShopFast browser ↔ checkout boundary assumption + threat?
> _Hint:_ Assumes browser executes only our JS. Magecart breaks this. Mitigation: CSP + SRI.

**Q23. (H · S)** ShopFast Order Svc ↔ Payment Svc boundary — kya implicit trust?
> _Hint:_ "Caller is legit internal service." Without mTLS: any compromised internal pod can call. Add: mTLS + scoped tokens.

**Q24. (H · S)** ShopFast ↔ PSP (external) boundary — 3 controls.
> _Hint:_ mTLS, API keys with rotation, allowlist IPs, signed webhooks (HMAC), idempotency keys.

**Q25. (M · S)** ShopFast HVAC vendor portal boundary (Target-style) — kya control?
> _Hint:_ Network segmentation from POS/inventory. MFA. Separate IDP. Egress filtering. Vendor risk assessment.

**Q26. (H · S)** ShopFast browser ↔ analytics JS (Google/3rd party) boundary?
> _Hint:_ Code-origin boundary. Controls: SRI hash, CSP restrict, Trusted Types, Permissions-Policy header.

**Q27. (H · S)** ShopFast checkout iframe (Stripe/payment) — boundary type?
> _Hint:_ Multiple: code-origin (Stripe-served JS), org (Stripe), browser (iframe sandboxing isolates DOM). PCI scope reduced.

**Q28. (M · S)** ShopFast user upload to S3 — boundary?
> _Hint:_ Trust: browser ↔ presigned URL ↔ S3. Control: time-limited URL, content-type whitelist, virus scan post-upload, separate bucket.

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** Spring Boot microservice cluster, no mTLS — kis boundary missing?
> _Hint:_ Process + Network between pods. All east-west traffic implicit trust.

**Q30. (M · B)** ASP.NET Core gateway → backend boundary control?
> _Hint:_ Internal cert mTLS + scoped JWT + IP allowlist + per-route policies.

**Q31. (H · B)** Kubernetes namespace boundary — 3 controls?
> _Hint:_ NetworkPolicy (deny cross-namespace), RBAC (role per namespace), ResourceQuota, ServiceAccount per namespace.

**Q32. (H · B)** "Database boundary" — beyond network controls, what?
> _Hint:_ Row-level security (RLS) for multi-tenant, separate read/write users, query timeouts, query firewall (e.g., Imperva), DAM.

**Q33. (H · B)** Build pipeline boundary controls 4?
> _Hint:_ Signed commits, hermetic builds, isolated builders, SBOM generation, artifact signing (Sigstore/cosign), provenance attestation (SLSA, in-toto).

**Q34. (H · B)** Sidecar (Envoy) ↔ app process boundary?
> _Hint:_ Same pod, localhost. Trust: still verify. Controls: app binds 127.0.0.1 only, sidecar handles all external + AuthZ.

**Q35. (H · B)** Service mesh boundary value-add vs plain L3 network policy?
> _Hint:_ Mesh = L7 identity-based (workload identity). NetPolicy = L3/L4 IP-based. Mesh stronger but heavier. NetPolicy as baseline.

---

## Section 5 — Incident: SolarWinds SUNBURST 2020 (Q36-Q43)

**Q36. (M · I)** SolarWinds attack actor + product?
> _Hint:_ APT29 (Cozy Bear / Russian SVR) attributed by US Gov. SolarWinds Orion network monitoring platform.

**Q37. (M · I)** Discovery date + by whom?
> _Hint:_ Dec 8, 2020 by FireEye (now Mandiant) — they noticed their own breach via stolen Red Team tools. Traced back to Orion update.

**Q38. (H · I)** Attack chain stages?
> _Hint:_ (1) Initial access to SolarWinds dev env (unknown vector). (2) Implant in build server (SUNSPOT malware injecting SUNBURST into Orion DLL at build). (3) Tainted binary signed legitimately. (4) Distributed via update server. (5) ~18,000 customers received it. (6) ~100 customers actively exploited (targeted).

**Q39. (H · I)** "Missing boundary" — kahan?
> _Hint:_ Source code ↔ shipped binary. Build process was treated as trusted black box. No attestation that source produced the binary.

**Q40. (H · I)** "Signature = possession, not provenance" matlab?
> _Hint:_ Signing key was held legitimately. Signature attested possession of key. It did NOT attest binary matches source. Provenance gap.

**Q41. (H · I)** SLSA framework levels (high-level)?
> _Hint:_ L0-L4. L1 = build script. L2 = hosted, source-versioned. L3 = isolated build. L4 = two-party review + hermetic + reproducible.

**Q42. (H · I)** Architectural controls to prevent (3)?
> _Hint:_ Hermetic + reproducible builds, build-process attestation (SLSA L3+), egress filter on build server, two-party review of build config changes, isolated CI runners.

**Q43. (H · I)** Aftermath — US gov + industry?
> _Hint:_ Biden EO 14028 (May 2021) — SBOM mandate, federal supply chain rules, Sigstore adoption, OpenSSF growth, CISA secure-by-design push.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** Zero-trust 3 core tenets (NIST 800-207)?
> _Hint:_ Never trust always verify, least-privilege, assume breach. (Plus: continuous validation, micro-segmentation, identity-centric.)

**Q45. (H · A)** SPIFFE/SPIRE workload identity 2027+ vision?
> _Hint:_ Default identity layer in cloud-native. Replaces IP/static-cert auth. Federated across clouds. Cryptographic + automatable.

**Q46. (H · A)** eBPF-enforced boundaries (Cilium, Tetragon) — advantage?
> _Hint:_ Kernel-level enforcement. Workload identity. Lower overhead than sidecar. Programmable policy.

**Q47. (H · A)** Confidential computing + trust boundary?
> _Hint:_ TEE (Intel SGX, AMD SEV, AWS Nitro) creates hardware-enforced process boundary. Even host OS can't see enclave data. New strongest boundary.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** DFD (Day 10) dashed lines = trust boundaries — Day 11 ka kya deepens?
> _Hint:_ Day 10 = identify + draw. Day 11 = per-crossing questions + control assignment.

**Q49. (H · X)** STRIDE (Day 9) trust boundary crossings pe kyun concentrate hota?
> _Hint:_ Trust-level mismatch = each STRIDE letter higher likelihood. Boundary = STRIDE multiplier.

**Q50. (H · X)** Defense-in-depth (Day 5) trust boundary mein kaise apply?
> _Hint:_ Each boundary = a layer. DiD = multiple boundaries. Real-world: don't trust internal — make internal boundaries explicit too. Zero-trust = max DiD at every hop.

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#trust-boundary #zero-trust #spiffe #mtls #service-mesh #solarwinds-sunburst #slsa #sigstore #supply-chain-security #confidential-computing #ebpf #cilium #quiz-day-011
