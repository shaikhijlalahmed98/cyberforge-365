# Day 010 Quiz — Data Flow Diagrams (DFDs)

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** DFD ka full form aur 1-line definition.
> _Hint:_ Data Flow Diagram. Visual representation of how data moves through a system — entities, processes, stores, flows, boundaries.

**Q2. (E · D)** DFD ke 5 building blocks aur shape.
> _Hint:_ External Entity (rectangle), Process (circle/rounded), Data Store (parallel lines / open rectangle), Data Flow (arrow), Trust Boundary (dashed line).

**Q3. (E · D)** DFD kis Shostack Q ka tool?
> _Hint:_ Q1 — "What are we working on?"

**Q4. (M · D)** L0 (Context) DFD vs L1 farak.
> _Hint:_ L0 = system as single circle + external entities. L1 = decomposes system into sub-processes + internal stores. L2+ deeper.

**Q5. (M · D)** Shostack rule of thumb — elements per diagram?
> _Hint:_ 7±2 elements. More = decompose to next level. Less = combine.

**Q6. (M · D)** Trust boundary kya hai — DFD context mein?
> _Hint:_ Line where trust level changes. Threats live here. Dashed line on diagram.

**Q7. (M · D)** "Process" element STRIDE-applicability?
> _Hint:_ All 6 letters (S, T, R, I, D, E).

**Q8. (M · D)** "External Entity" pe applicable STRIDE?
> _Hint:_ S + R only.

**Q9. (H · D)** DFD vs C4 Container/Component — farak.
> _Hint:_ DFD = data-centric (what flows). C4 = structural (what components). Complementary. DFD better for security analysis.

**Q10. (H · D)** DFD vs sequence diagram?
> _Hint:_ Sequence = time-ordered messages between specific actors. DFD = aggregate data flow, no timing. Security analysis prefers DFD for boundary visibility.

---

## Section 2 — DFD Levels & Decomposition (Q11-Q20)

**Q11. (M · R)** Kab L2 banao?
> _Hint:_ When L1 element has internal complexity worth modeling (e.g., "auth service" → "token issuer" + "session mgr" + "MFA verifier").

**Q12. (M · R)** Decomposition stop kab — "process fits in one head"?
> _Hint:_ When single process is small enough that one engineer fully understands it → STRIDE per element becomes meaningful, not abstract.

**Q13. (H · R)** L0 mein kaunsi 3 details NAHI hoti?
> _Hint:_ Internal processes, stores, internal trust boundaries (sometimes shown). L0 = "system + outside world" only.

**Q14. (M · R)** L1 mein "internal" trust boundaries?
> _Hint:_ Yes — e.g., between web tier and DB tier, between tenants, between privilege zones. L1 shows them.

**Q15. (H · R)** Inconsistent levels in one diagram — kyun problem?
> _Hint:_ Visual hierarchy breaks; threat coverage uneven. Stick to one level per diagram; reference parent.

**Q16. (M · R)** "Data Store" implicit ya explicit hona chahiye?
> _Hint:_ Explicit, always. Caches, queues, temp files — all are stores. Implicit ones = blind spots.

**Q17. (H · R)** Cache (Redis, Memcached) DFD pe show karna chahiye?
> _Hint:_ Haan. Has data → integrity (T), disclosure (I), DoS (D) applicable.

**Q18. (M · R)** Message queue (Kafka, RabbitMQ) — store ya flow?
> _Hint:_ Both views valid. Pragmatic: model as store (persistent) + 2 flows (in + out).

**Q19. (H · R)** "External" definition cloud context mein?
> _Hint:_ Anything outside your trust boundary — even other internal teams or vendors with VPC peering. Trust level matters, not network proximity.

**Q20. (H · R)** Browser-side JS (1st party from your origin) — process ya external entity?
> _Hint:_ Process running in untrusted environment (user's browser). Special treatment — half external, half delegated. BA Magecart wala bug yahi.

---

## Section 3 — Trust Boundaries Preview (Q21-Q28)

**Q21. (M · D)** Trust boundary 5 types?
> _Hint:_ Network, Process, Privilege/Role, Code-origin, Organizational.

**Q22. (M · D)** Per-crossing ka core question?
> _Hint:_ "What assumption is being made at this boundary?" Then: "What if it's wrong?" + "What verifies it?"

**Q23. (H · S)** ShopFast me code-origin boundary example.
> _Hint:_ ShopFast page loads 3rd-party JS (analytics, payment iframe). Crossing = 1st party origin ↔ 3rd party code (still same origin in browser).

**Q24. (H · S)** ShopFast me org boundary example.
> _Hint:_ ShopFast ↔ HVAC vendor portal, ↔ PSP, ↔ shipping vendor API.

**Q25. (M · R)** Trust boundary "amplifier, not target" — meaning?
> _Hint:_ Boundary itself isn't attacked; threats are amplified across it (i.e., severity ↑ when assumption breaks). Not a node, but a multiplier.

**Q26. (H · R)** Implicit vs explicit trust boundary?
> _Hint:_ Implicit = exists in code/policy but not on diagram. Explicit = drawn. Goal of DFD = make all implicit boundaries explicit.

**Q27. (H · R)** Zero-trust DFD pe kaisa dikhega?
> _Hint:_ Every hop = dashed line. Every flow = encrypted + authenticated. No "trust zone" simplification.

**Q28. (H · R)** Trust boundary pe controls — Preventive + Detective minimum?
> _Hint:_ Yes — AuthN/AuthZ/encrypt (P) + log/monitor (D) every crossing.

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** Microservice DFD pe kaunse 5 elements hoga typical?
> _Hint:_ External: API consumer, Internal service caller. Processes: service, sidecar. Stores: DB, cache. Flows: ingress, DB calls, external API. Boundary: pod ↔ pod, namespace, cluster.

**Q30. (H · B)** Spring Boot app DFD me hidden flow — log shipping?
> _Hint:_ Logs → logstash/ELK → may contain PII → trust boundary to log infrastructure. Often missed.

**Q31. (M · B)** ASP.NET app me background service DFD pe — process ya external?
> _Hint:_ Process (same app boundary). Different process if separate service.

**Q32. (H · B)** Kubernetes admission webhook — DFD me where?
> _Hint:_ Process (separate from API server). Trust boundary: kube-apiserver ↔ webhook. mTLS required.

**Q33. (H · B)** CI/CD pipeline ka DFD elements?
> _Hint:_ EE: developer, registry. Process: build, test, scan, sign, deploy. Store: artifact registry, secrets vault, source repo. Flow: commit → build → image → deploy. Boundaries: dev ↔ build ↔ deploy zones.

**Q34. (H · B)** Service mesh sidecar DFD me alag se show?
> _Hint:_ Yes — separate process. Sidecar = trust amplifier (mTLS, AuthZ). Boundary: app process ↔ sidecar.

**Q35. (H · B)** "Threat-modeling-as-code" workflow ka mini-DFD?
> _Hint:_ EE: dev. Process: TM parser (pytm/Threagile). Store: arch.yaml + threat-rules.yaml. Flow: PR → parse → STRIDE rules → comment. Boundary: PR review.

---

## Section 5 — Incident: BA Magecart 2018 (Q36-Q43)

**Q36. (M · I)** British Airways breach date + records?
> _Hint:_ Aug-Sep 2018. ~380k+ cards (some sources 429k). 22 days exposure.

**Q37. (H · I)** Magecart attack technique?
> _Hint:_ Attacker compromised baways.com via 3rd-party script (Modernizr 2.6.2 modified). Skimmer JS read form fields on checkout, exfiltrated to attacker domain.

**Q38. (H · I)** Exfil domain detail (homoglyph)?
> _Hint:_ Used domain like `baways.com` (b-a-w-a-y-s) and `baways.com`-similar IDN. Visual lookalike of ba.com.

**Q39. (H · I)** "Assumed flow vs actual flow" DFD lens?
> _Hint:_ Assumed: browser → checkout → BA server. Actual: browser executed attacker JS sending form data to attacker before/during BA submit. Browser-side process was never on BA's DFD.

**Q40. (H · I)** Architectural control — SRI?
> _Hint:_ Subresource Integrity. `<script integrity="sha384-...">`. Browser refuses to execute if hash mismatch. Magecart-modified script = hash mismatch.

**Q41. (H · I)** Architectural control — CSP?
> _Hint:_ Content Security Policy. `connect-src` directive restricts which domains scripts can `fetch()`/`XHR` to. Restricts exfil destinations.

**Q42. (M · I)** GDPR fine (ICO)?
> _Hint:_ Initially proposed £183.4M (record at time). Reduced to £20M post-COVID hardship.

**Q43. (H · I)** "First-party origin ≠ first-party code" lesson?
> _Hint:_ Script served from your domain doesn't mean you wrote it. Modern web pulls 3rd-party. Trust boundary = code origin (vendor) even when served from your domain.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** TMaC tools 4 list.
> _Hint:_ pytm (Python), Threagile (YAML), IriusRisk (commercial), STRIDE-GPT (LLM), OWASP Threat Dragon (visual).

**Q45. (H · A)** "Versioned DFDs in Git" advantages?
> _Hint:_ Diff over time, PR-blockable on threat changes, audit trail, reproducible, integrates with CI.

**Q46. (H · A)** eBPF + Cilium Hubble runtime DFD discovery — kya capability?
> _Hint:_ Observed actual data flows live (not designed). Compare designed DFD vs observed → drift detection. Real-time threat surface visibility.

**Q47. (H · A)** AI-aware DFD notation — kya new elements?
> _Hint:_ LLM process (special — prompts, context, tools). RAG retrieval store. Tool-call flows. Prompt-injection-aware boundaries. MITRE ATLAS framework.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** DFD STRIDE ke saath kaise integrate hota practically?
> _Hint:_ Draw DFD → per-element STRIDE checklist → per crossing threats → DREAD/CVSS score → mitigations on diagram.

**Q49. (H · X)** Trust boundaries (Day 11) DFD se kaise extract?
> _Hint:_ Every dashed line in DFD = boundary. Day 11 deep-dives the "what to verify" per boundary.

**Q50. (H · X)** Attack trees (Day 13 preview) DFD se kya alag?
> _Hint:_ DFD = defender's view of system. Attack tree = attacker's goal decomposition. Different perspectives, complementary in Q2.

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#dfd #data-flow-diagram #threat-modeling #l0-context #l1-decomposition #trust-boundary #ba-magecart-2018 #subresource-integrity #content-security-policy #tmac #pytm #threagile #quiz-day-010
