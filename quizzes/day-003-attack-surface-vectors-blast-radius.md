# Day 003 Quiz — Attack Surface, Vectors & Blast Radius

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** Attack surface ki ek-line definition.
> _Hint:_ Sab entry-points jahan se untrusted input/code system mein aa sakta — yeh sum hai.

**Q2. (E · D)** Attack vector vs attack surface — farak.
> _Hint:_ Surface = static set of entry points. Vector = specific path/method actor uses (e.g., phishing email, SQLi).

**Q3. (E · D)** Blast radius ka 1-line meaning.
> _Hint:_ Ek compromise hone par damage ka spread/scope — kitna fail ek bar?

**Q4. (M · D)** "External attack surface" vs "Internal attack surface" — example.
> _Hint:_ External = internet-facing endpoints (DNS, web, API). Internal = post-breach reachable (intranet, DBs, queues).

**Q5. (M · D)** Physical attack surface ka 1 example.
> _Hint:_ Datacenter doors, USB ports, console access, lost laptop.

**Q6. (M · D)** Human attack surface ka 1 example.
> _Hint:_ Phishing, vishing, pretexting, helpdesk social engg, insider.

**Q7. (M · D)** "Cloud attack surface" ka unique 3 items?
> _Hint:_ Public S3 buckets, exposed metadata service (IMDS), over-permissive IAM roles, console misconfig.

**Q8. (H · D)** "Shadow IT" attack surface ka kya impact?
> _Hint:_ Unknown unknowns — assets exist that security team is unaware of. Adds untracked surface.

**Q9. (M · D)** "Attack surface reduction" ka 3 techniques.
> _Hint:_ Disable unused features, close unused ports, remove unused libs/services, network segmentation.

**Q10. (H · D)** ASM (Attack Surface Management) tools kya monitor karte hain?
> _Hint:_ External DNS, SSL certs, leaked credentials, open ports, exposed buckets. Continuous discovery + drift detection.

---

## Section 2 — Concept Relationships (Q11-Q20)

**Q11. (M · R)** Surface grow karta hai jab — 3 cases?
> _Hint:_ New feature shipped, new dependency added, port opened, employee onboarded, integration with vendor.

**Q12. (M · R)** Blast radius minimize karne ka 3 architectural techniques.
> _Hint:_ Segmentation (VLAN/VPC/namespace), Least privilege (IAM), Micro-segmentation (service mesh), Bulkheads (circuit breakers).

**Q13. (M · R)** Monolith vs microservices — attack surface vs blast radius tradeoff?
> _Hint:_ Microservices: surface ↑ (more network calls), blast radius ↓ (per-service isolation). Monolith: opposite.

**Q14. (H · R)** "Container escape" blast radius kis tak extend?
> _Hint:_ Container → host kernel → other containers on host → potentially other nodes if cluster RBAC weak.

**Q15. (H · R)** IAM role with `*` permissions blast radius?
> _Hint:_ Whole AWS account / region — Capital One-style lateral pivot.

**Q16. (M · R)** Defense in depth (Day 5) blast radius pe kaisa impact?
> _Hint:_ Reduces — each layer slows propagation, increases detection chances.

**Q17. (H · R)** SSRF (Server-Side Request Forgery) attack surface mein kahan?
> _Hint:_ Any feature jo server-side URL fetch karta (image proxy, webhook, PDF render). Often deeply internal — bypasses perimeter.

**Q18. (M · R)** Compromised CI/CD pipeline blast radius?
> _Hint:_ Production deploys, secrets, all artifacts signed by that pipeline. SolarWinds-class.

**Q19. (H · R)** "Crown jewel analysis" blast radius modeling ke liye kyun useful?
> _Hint:_ Backwards-design — identify what attacker wants → enumerate paths to reach it → close paths.

**Q20. (H · R)** "Attack graphs" (BloodHound, AWStealth) blast radius visualize karte — kaise?
> _Hint:_ Graph nodes = principals/resources, edges = permissions. Shortest path from compromised entity → crown jewel = blast radius preview.

---

## Section 3 — ShopFast Application (Q21-Q28)

**Q21. (M · S)** ShopFast ka top 5 external attack surface item.
> _Hint:_ Web app login, Public API endpoints, Customer-facing search, Payment redirect URL, DNS/MX records.

**Q22. (M · S)** ShopFast ke 3 likely attack vectors top to bottom.
> _Hint:_ Credential stuffing on login, Magecart on checkout, SSRF on image-proxy, SQLi on legacy admin, Phishing on warehouse staff.

**Q23. (H · S)** ShopFast warehouse staff WiFi share karta corporate network se — blast radius?
> _Hint:_ Compromised personal device → corporate → potentially POS, inventory DB, even payment processor link.

**Q24. (H · S)** ShopFast checkout pe attacker XSS chala — blast radius?
> _Hint:_ Cookie/session theft of every browsing user → account takeover → stored cards (if not tokenized).

**Q25. (M · S)** ShopFast payment service kis trust boundary pe?
> _Hint:_ Network boundary (PCI scope), org boundary (vendor PSP), data classification (PAN tokenized vs raw).

**Q26. (H · S)** ShopFast SSRF mile customer-image upload feature pe — blast radius kya?
> _Hint:_ Cloud metadata service (IMDS) → IAM credentials → S3 + DBs (Capital One pattern).

**Q27. (M · S)** ShopFast admin panel attack surface reduce karne ka 3 step.
> _Hint:_ IP allowlist, MFA, VPN-only, separate URL/host, rate-limit.

**Q28. (H · S)** "Burst-sale" Black Friday — attack surface temporarily badhta hai?
> _Hint:_ Haan — auto-scaled instances may inherit weaker images, new ad-hoc endpoints. Plus rate-limiting often relaxed under load.

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** Spring Actuator `/env` endpoint exposed blast radius?
> _Hint:_ Env vars leak (DB creds, API keys) → cascading compromise of downstream systems.

**Q30. (M · B)** Microservice without ingress restrictions — blast radius if one is compromised?
> _Hint:_ East-west reach to all peers. Flat network problem.

**Q31. (H · B)** "Egress filtering" attack surface ya blast radius reduce karta?
> _Hint:_ Blast radius — limits what compromised service can call out to (no C2, no exfil).

**Q32. (H · B)** Kubernetes pod with `hostNetwork: true` blast radius impact?
> _Hint:_ Bypasses pod network isolation, sees host's interfaces → much larger surface from inside.

**Q33. (M · B)** Database read replica access from a compromised service — blast radius?
> _Hint:_ Data exfil (C), but not state-modifying. Lower than primary access.

**Q34. (H · B)** "Service Mesh" attack surface analysis mein add karta — pros aur cons?
> _Hint:_ Pro: mTLS reduces east-west blast radius. Con: sidecar itself is new code = new vulns (e.g., Istio CVEs).

**Q35. (H · B)** "Confidential computing" blast radius reduce karta enclave compromise se?
> _Hint:_ Enclave breach affects only enclave data; OS/host can't trivially access. Stronger isolation than process boundary.

---

## Section 5 — Incident: Capital One 2019 (Q36-Q43)

**Q36. (M · I)** Capital One breach year + attacker name?
> _Hint:_ 2019. Paige Thompson (Erratic). Ex-AWS employee.

**Q37. (M · I)** Records exposed?
> _Hint:_ ~106M (US + Canada) — names, SSNs, bank accounts, credit scores.

**Q38. (H · I)** Attack chain (4 stages) likh — surface → ?
> _Hint:_ (1) Misconfigured WAF (ModSecurity) → SSRF. (2) SSRF → IMDS (169.254.169.254). (3) IMDS → temporary IAM credentials of role `*WAF-Role*`. (4) Over-broad IAM → S3 `ListBuckets` + `GetObject` → exfil.

**Q39. (M · I)** Root cause architectural decision?
> _Hint:_ IAM role too broad — listed/read all buckets. SSRF was the trigger, but IAM scope was the explosion.

**Q40. (H · I)** Cloud-specific lesson — IMDS v1 vs v2 ka difference?
> _Hint:_ IMDSv1 = simple GET, exploitable via SSRF. IMDSv2 = session-token (PUT then GET), defeats most SSRF. Default in new AWS.

**Q41. (H · I)** Blast radius reduce karne ke 3 architectural controls jo Capital One ko bachate.
> _Hint:_ (1) Scoped IAM (no `*`). (2) Egress restrictions from WAF to IMDS. (3) VPC endpoints for S3 with IAM policies that reject open `aws:SourceIp`.

**Q42. (M · I)** Settlement amount?
> _Hint:_ $190M class-action + $80M OCC fine.

**Q43. (H · I)** Detection — Capital One ko kab pata chala aur kaise?
> _Hint:_ ~3 months after exfil. Via responsible disclosure tip (GitHub gist leaked code). Detection internal nahi tha — yeh aur badi failure.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** EASM (External ASM) tools — 3 ka naam.
> _Hint:_ Censys, Shodan, RiskIQ/Microsoft Defender EASM, Randori, Bishop Fox CAST.

**Q45. (H · A)** "Continuous attack surface monitoring" CI mein kaise integrate?
> _Hint:_ Daily DNS sweeps, cert transparency log monitoring (CT logs), open-port scans, leaked-credential checks → ticket creation.

**Q46. (H · A)** Zero-trust architecture blast radius ke saath kaise relate?
> _Hint:_ ZT removes implicit trust → each hop checked → lateral movement (blast) much slower.

**Q47. (H · A)** AI/LLM-powered attacks attack surface mein kya add karte?
> _Hint:_ Prompt injection endpoints, RAG knowledge poisoning, model API extraction, agentic tool-call abuse.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** Trust boundary (Day 11) attack surface ka subset hai?
> _Hint:_ Nahi — boundary ek line hai, surface multiple points. Trust boundary crossings *are* surface points if external entity is at one side.

**Q49. (H · X)** STRIDE-per-element (Day 9) attack surface ko system mein decompose karta — kaise?
> _Hint:_ Har DFD element → STRIDE checklist → effectively attack-surface-by-element catalog.

**Q50. (H · X)** Defense-in-depth (Day 5) blast radius pe kaam karta ya attack surface pe?
> _Hint:_ Mainly blast radius (containment). Surface reduction is a separate discipline (hardening).

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#attack-surface #attack-vector #blast-radius #ssrf #imds #capital-one-2019 #cloud-iam #easm #quiz-day-003
