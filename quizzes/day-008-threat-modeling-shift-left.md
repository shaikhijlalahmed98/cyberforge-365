# Day 008 Quiz — Why Threat Modeling Matters & Shift-Left

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** Threat modeling 1-line definition.
> _Hint:_ Structured process to identify, enumerate, and prioritize potential threats against a system — **before** they become exploits.

**Q2. (E · D)** Shostack's 4 questions list — exact wording.
> _Hint:_ (1) What are we working on? (2) What can go wrong? (3) What are we going to do about it? (4) Did we do a good job?

**Q3. (E · D)** "Shift-left" matlab.
> _Hint:_ Move security activities earlier in SDLC — design → CI → review → production. "Left" of timeline = earlier.

**Q4. (M · D)** "1x → 100x cost curve" matlab.
> _Hint:_ Design-stage fix costs 1x. Same fix in production costs 100x (rebuild, regression, incident response, brand). Defect cost grows ~10x per phase.

**Q5. (M · D)** "Implementation bug" vs "Design flaw" — farak + ek-ek example.
> _Hint:_ Impl bug = code-level (buffer overflow, missing input validation, off-by-one). Design flaw = architectural (wrong protocol choice, missing AuthZ layer, plaintext storage).

**Q6. (M · D)** Pentest vs Threat Model — farak.
> _Hint:_ Pentest = find vulns in built system. Threat model = anticipate threats during design. Pre vs post. Complementary, not substitutes.

**Q7. (M · D)** "Insecure by design" matlab.
> _Hint:_ System works as designed, but the design itself enables abuse. CWE-1357. Can't patch — must redesign.

**Q8. (H · D)** Threat modeling output kya hota?
> _Hint:_ DFD + threat list (STRIDE) + risk-scored prioritization + mitigation plan + tracking IDs. Living artifact.

**Q9. (H · D)** "Threat modeling maturity model" — 4 stages?
> _Hint:_ Ad-hoc (1 person) → Defined (template) → Repeatable (process) → Continuous (integrated in CI / TMaC). Per OWASP.

**Q10. (H · D)** "Threat modeling as code" (TMaC) tools 3 list.
> _Hint:_ pytm (Python), Threagile (YAML), Microsoft Threat Modeling Tool, IriusRisk, STRIDE-GPT.

---

## Section 2 — Why Shift-Left (Q11-Q20)

**Q11. (M · R)** Shift-left ke 3 benefits beyond cost.
> _Hint:_ Faster feedback to developers, fewer prod incidents, less context-switching, security culture, easier compliance evidence.

**Q12. (M · R)** Shift-left "shift-right" se exclusive nahi — kyun?
> _Hint:_ Right-side (runtime detection, IR, observability) still needed for unknown-unknowns. Modern view: shift-everywhere.

**Q13. (H · R)** Why pentest at end fails — 3 reasons?
> _Hint:_ Design flaws can't be patched cheaply, time pressure to ship overrides fix, scope-of-pentest limited, residual risks accepted under deadline.

**Q14. (M · R)** "Security champions" program shift-left ka enabler kaise?
> _Hint:_ Dev team mein embedded security advocate — reviews design, runs threat model sessions, peer learning. Distributes load.

**Q15. (M · R)** Threat modeling kab karna chahiye? 3 trigger points.
> _Hint:_ New feature/service, major refactor, integration with vendor, post-incident review, architecture changes.

**Q16. (H · R)** "Threat model debt" matlab.
> _Hint:_ Skipped threat models accumulating as undocumented design risk — like tech debt for security. Pay later, painfully.

**Q17. (M · R)** Threat modeling session — kaun-kaun hone chahiye?
> _Hint:_ Architect, dev lead, QA, security engineer, optional: ops/SRE + product manager (for asset value).

**Q18. (H · R)** Time-box per session — ideal?
> _Hint:_ 60-90 min initial, 30 min iterations. Avoid > 2hr fatigue. Multiple short > one long.

**Q19. (H · R)** "Whiteboard threat modeling" vs "TMaC" — kab kaunsa?
> _Hint:_ Whiteboard = early design, exploration. TMaC = mature, automated CI checks, version control, reproducible.

**Q20. (H · R)** "Threat modeling is the cheapest security activity per $ saved" — argument banao.
> _Hint:_ Catches design flaws at 1x cost. Pentests catch impl bugs at 10-50x. Incidents at 100-1000x. Ratio: hours per session vs $ saved per breach.

---

## Section 3 — Shostack's 4 Questions Deep Dive (Q21-Q28)

**Q21. (E · D)** Q1 "What are we working on?" — kaunsa tool answer karta?
> _Hint:_ DFD (Day 10). Plus asset inventory + data classification.

**Q22. (E · D)** Q2 "What can go wrong?" — kaunsa framework?
> _Hint:_ STRIDE (Day 9), attack trees (Day 13), kill chain, MITRE ATT&CK.

**Q23. (M · D)** Q3 "What are we going to do?" — mitigation strategies kya?
> _Hint:_ Eliminate, Mitigate, Transfer, Accept. (Plus deterrent.) Document each threat's strategy.

**Q24. (M · D)** Q4 "Did we do a good job?" — kaise verify?
> _Hint:_ Pentest, red team, bug bounty, runtime monitoring, post-mortem learnings → loop back to Q1.

**Q25. (H · D)** Q4 most-skipped step — kyun?
> _Hint:_ Validation effort > documentation, no clear "done" criteria, ship pressure. Yet most important for maturity.

**Q26. (M · D)** "Tampering with hash key" yeh kaunsa Shostack Q?
> _Hint:_ Q2 (what can go wrong). STRIDE-T threat.

**Q27. (H · D)** Adam Shostack ki "Threat Modeling: Designing for Security" book ki core thesis?
> _Hint:_ "Threat modeling is for everyone, not just security experts. 4 questions democratize it."

**Q28. (H · D)** "Threat modeling manifesto" 2020 — co-authors kya argument karte?
> _Hint:_ Values: people > process, journeys > one-time, varying methods, exposing risk > checking boxes. https://www.threatmodelingmanifesto.org

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** Microservice mein threat model kis level pe karna chahiye?
> _Hint:_ Both: per-service (focused) + system-wide (interaction). System view catches cross-service flaws.

**Q30. (M · B)** ASP.NET Core feature release pe threat model integrate karne ke 3 steps.
> _Hint:_ Design doc requires threat model section → PR template requires affected services → CI runs pytm/Threagile against architecture YAML.

**Q31. (H · B)** Spring Boot REST API mein design-flaw vs impl-bug ek-ek example.
> _Hint:_ Design flaw: no AuthZ check on `GET /users/{id}` (IDOR by design). Impl bug: SQLi in 1 endpoint.

**Q32. (H · B)** Kubernetes deployment ke threat model 3 high-level threats?
> _Hint:_ Container escape, RBAC misconfig (cluster-admin), secrets in env vars, etcd unencrypted, supply chain (image), supply chain (helm chart).

**Q33. (M · B)** CI/CD pipeline ke threat model — kaunse 4 assets?
> _Hint:_ Source repo, build artifacts, signing keys, secrets/credentials.

**Q34. (H · B)** "Continuous threat modeling" CI mein kaise?
> _Hint:_ Architecture-as-code (YAML) → CI parses → STRIDE rules engine → PR comment with new threats → blocks if unmitigated.

**Q35. (H · B)** "Trust assumption documentation" SDLC mein kab?
> _Hint:_ Design doc → "We assume X is trusted because Y." Reviewed quarterly + on integration changes.

---

## Section 5 — Incident: Zoom CVE-2019-13450 (Q36-Q43)

**Q36. (M · I)** Zoom 2019 vuln researcher + scope?
> _Hint:_ Jonathan Leitschuh. Mac Zoom client. CVE-2019-13450.

**Q37. (H · I)** Vulnerability technical root cause?
> _Hint:_ Zoom installed local web server (port 19421) on user's Mac. Persisted even after uninstall. Any website could send HTTP requests → force-join meeting + auto-enable camera.

**Q38. (H · I)** "Insecure by design" kyun classify hua impl bug nahi?
> _Hint:_ The local web server was a deliberate design choice for "smooth UX." Not a bug — a feature. Couldn't be "patched"; had to be redesigned.

**Q39. (H · I)** Zoom ki initial response (90-day disclosure) kya thi?
> _Hint:_ Defended design, called it "feature for UX." Refused to remove web server. Only after public + Apple intervention agreed to fix.

**Q40. (M · I)** Apple ne kaisa unusual step liya?
> _Hint:_ Silent MRT (Malware Removal Tool) update — removed Zoom's local web server on millions of Macs without user action. Rare for Apple.

**Q41. (H · I)** Architectural lesson?
> _Hint:_ UX > Security tradeoffs are valid but must be threat-modeled. "Convenience" features that bypass browser sandbox = serious design red flag.

**Q42. (H · I)** Shift-left lesson?
> _Hint:_ Threat model would've caught "local web server" as Q2 threat (TES — Tampering, Elevation via local HTTP). Pre-ship review failed. Post-ship costed brand + dev time.

**Q43. (H · I)** Post-2019 Zoom security investments?
> _Hint:_ E2EE meetings, CISO hired (Jason Lee), bug bounty expanded, security council (Stamos consult). Reactive but substantial.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** "Privacy by Design" — Ann Cavoukian ke 7 principles 1-line.
> _Hint:_ Proactive not reactive, default privacy settings, embedded into design, full functionality positive-sum, end-to-end security, visibility + transparency, respect user privacy.

**Q45. (H · A)** "Secure by Design" CISA initiative — 3 principles?
> _Hint:_ Take ownership, radical transparency, customer outcomes > revenue. Move from "blame the user" to "build it safer."

**Q46. (H · A)** AI-assisted threat modeling — STRIDE-GPT etc. — caveats?
> _Hint:_ LLM hallucinations, biased toward common threats (misses novel), needs human review, integration with codebase context still maturing.

**Q47. (H · A)** "Threat modeling for AI systems" — 3 new threat categories?
> _Hint:_ Prompt injection, training data poisoning, model extraction. Plus: hallucination weaponization, RAG context manipulation, agentic tool misuse.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** Day 9 (STRIDE) Shostack ke kaunse Q ka answer?
> _Hint:_ Q2 — "what can go wrong?" STRIDE = enumeration framework.

**Q49. (H · X)** Day 10 (DFD) Shostack ke kaunse Q?
> _Hint:_ Q1 — "what are we working on?" DFD = the picture.

**Q50. (H · X)** Days 11 (Trust Boundaries) + 12 (DREAD) Shostack Q's mapping?
> _Hint:_ Trust Boundaries → Q1 (where threats live in DFD). DREAD → Q3 (prioritization for "what to do").

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#threat-modeling #shift-left #shostack-4-questions #insecure-by-design #zoom-cve-2019-13450 #tmac #secure-by-design #privacy-by-design #quiz-day-008
