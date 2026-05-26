# Day 006 Quiz — Security Control Types (Preventive · Detective · Corrective · Compensating)

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** 4 control types by function — ek-ek 1-line meaning.
> _Hint:_ Preventive (stop before), Detective (alert during/after), Corrective (fix after), Compensating (alternative when primary infeasible).

**Q2. (E · D)** Control types by **nature** (orthogonal axis) — 3 categories?
> _Hint:_ Administrative (policy/process/training), Technical/Logical (tools/code/config), Physical (locks/cameras/locations).

**Q3. (M · D)** "Deterrent" control — yeh function ya nature?
> _Hint:_ Function — discourages attempt (sign "CCTV monitored", warning banner). 5th function category in some frameworks.

**Q4. (M · D)** "Directive" control — example?
> _Hint:_ Policy that mandates behavior — Acceptable Use Policy, password policy, code review requirement. Administrative + Preventive blend.

**Q5. (E · D)** Detective control 3 examples list.
> _Hint:_ IDS/IPS in monitor mode, SIEM alerts, file integrity monitoring (FIM), CCTV review, audit log analysis.

**Q6. (E · D)** Preventive control 3 examples.
> _Hint:_ Firewall block rules, AuthN/Z, encryption, input validation, locks, MFA.

**Q7. (M · D)** Corrective control 3 examples.
> _Hint:_ Patches, backups/restore, incident response, quarantine, password reset, IR runbooks.

**Q8. (M · D)** Compensating control real-world test — kya 3 conditions chahiye?
> _Hint:_ (1) Equivalent risk reduction. (2) Documented justification. (3) Beyond what minimum requires. (PCI DSS Appendix B style.)

**Q9. (H · D)** Function × Nature matrix banaya tha (4×3 = 12 cells). Ek-ek mein control example.
> _Hint:_ E.g., Preventive+Admin = security training; Detective+Physical = CCTV; Corrective+Technical = automated rollback; Compensating+Admin = manual log review when SIEM down.

**Q10. (H · D)** "Recovery" vs "Corrective" — same hain?
> _Hint:_ Often used interchangeably. Stricter view: Corrective = fix the immediate problem (patch). Recovery = restore service (backup restore). Both follow detection.

---

## Section 2 — Concept Relationships (Q11-Q20)

**Q11. (M · R)** Detection bina prevention ka kya value?
> _Hint:_ Some — but prevention without detection = blind to bypass. Together = mature security.

**Q12. (M · R)** Detection fired but corrective response fail = Target 2013 pattern. Kya seekhe?
> _Hint:_ Detection ≠ Response. Tools must trigger workflows + on-call must act. SOC maturity matters.

**Q13. (H · R)** "Mean Time to Detect" (MTTD) vs "Mean Time to Respond" (MTTR) — kis pe focus pehle?
> _Hint:_ Both. MTTD reduces dwell time. MTTR reduces impact. Industry MTTD ~200+ days historically → goal: hours.

**Q14. (M · R)** Compensating control "as good as" primary — kaun decide?
> _Hint:_ Risk assessor + auditor. Documented in risk register with sign-off. Not engineer's solo call.

**Q15. (H · R)** "Preventive controls fail closed vs fail open" — fail kis side ho?
> _Hint:_ "Fail closed" (deny) for security. "Fail open" (allow) for safety/availability — depends on system. E.g., fire door fails open (life safety).

**Q16. (M · R)** Detection signal-to-noise problem — solution?
> _Hint:_ Tuning, threat intelligence enrichment, correlation rules, automation (SOAR), ML triage.

**Q17. (H · R)** Same risk ke liye 4 type controls — diversify kyun karte?
> _Hint:_ Functional redundancy. Prevention may fail, detection catches, correction recovers, compensating fills gaps.

**Q18. (M · R)** "Manual approval workflow" — kaunsa function?
> _Hint:_ Preventive primarily (gates the action). Also Directive (defines who can approve).

**Q19. (H · R)** "Honey pots" — function kya?
> _Hint:_ Detective primary (reveals attacker presence). Also Deterrent (if attacker realizes). Some say deception/proactive.

**Q20. (H · R)** "Threat hunting" — preventive ya detective?
> _Hint:_ Detective (proactive subtype). Search for indicators without alerts being raised.

---

## Section 3 — ShopFast Application (Q21-Q28)

**Q21. (M · S)** ShopFast payment service ke har function 1-1 control example.
> _Hint:_ Preventive: WAF + tokenization. Detective: anomalous-purchase ML. Corrective: chargeback handler + refund + IR. Compensating: manual review queue for blocked transactions.

**Q22. (M · S)** ShopFast me admin account compromise detect — 3 detective controls?
> _Hint:_ Impossible-travel login, off-hours admin activity, mass record export, privilege change alerts.

**Q23. (H · S)** ShopFast me legacy admin tool MFA support nahi karta — compensating control design.
> _Hint:_ IP allow-list + jump host with MFA + session recording + tight access window + heightened logging. Document rationale + sunset date.

**Q24. (M · S)** ShopFast pe DDoS ke against — 4 controls (1 per type)?
> _Hint:_ Preventive: rate-limit + CDN scrubbing. Detective: traffic baseline alert. Corrective: failover to backup region. Compensating: manual whitelist if scrubber breaks.

**Q25. (H · S)** ShopFast me leaked customer data SOC ko 6 ghante baad pata — kaunsa control fail?
> _Hint:_ Detective (slow MTTD). Likely missing DLP, weak SIEM rules, no exfil alerts.

**Q26. (M · S)** ShopFast me code review mandatory before merge — kaunsa control + nature?
> _Hint:_ Preventive + Administrative. Plus Directive (policy).

**Q27. (H · S)** ShopFast warehouse pe physical control DiD?
> _Hint:_ Preventive (badge access), Detective (CCTV + tailgate sensors), Corrective (incident playbook), Deterrent (signage).

**Q28. (H · S)** ShopFast me SIEM rule fires false positive 95% — fix kya?
> _Hint:_ Tune thresholds, add correlation context, enrich with threat intel, route low-confidence to triage queue. Don't disable.

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** WAF — kaunsa function?
> _Hint:_ Preventive primary (block). Detective secondary (logs).

**Q30. (M · B)** "Audit log" — kaunsa function?
> _Hint:_ Detective. Forensic enabler. (Also evidence for Accounting/non-repudiation.)

**Q31. (M · B)** "Automatic restart on crash" — kaunsa?
> _Hint:_ Corrective + Availability. (Risk: masks underlying issue → couple with detection.)

**Q32. (H · B)** "Canary deployments" — kaunsa function?
> _Hint:_ Detective primary (catches bad release on small slice). Corrective via auto-rollback. Preventive-ish in spirit.

**Q33. (H · B)** "Network ACL deny by default" — kaunsa + nature?
> _Hint:_ Preventive + Technical. Maps to least-privilege principle.

**Q34. (H · B)** Spring Boot mein `@PreAuthorize` + audit interceptor — function pair.
> _Hint:_ `@PreAuthorize` = Preventive (gate). Interceptor logging = Detective.

**Q35. (H · B)** "Chaos engineering" — kaunsa function?
> _Hint:_ Detective indirectly (reveals weak spots). Plus tests Corrective controls (auto-recovery).

---

## Section 5 — Incident: Target 2013 (Q36-Q43)

**Q36. (M · I)** Target breach year + records exposed?
> _Hint:_ Nov-Dec 2013. ~40M card numbers + ~70M PII.

**Q37. (M · I)** Initial vector?
> _Hint:_ HVAC vendor (Fazio Mechanical) phished → vendor portal creds → Target network (flat) → POS malware (BlackPOS).

**Q38. (M · I)** Detective control jo fire hua but missed?
> _Hint:_ FireEye alerts on POS malware activity. Multiple alerts. Ignored/triaged-low by SOC team. Symantec scanner also detected.

**Q39. (H · I)** Day 6 lens — Detection succeeded, Corrective failed. Kya seekhe?
> _Hint:_ Detection wasted if not acted on. People + process must convert detection → containment. SOC playbook + escalation matter.

**Q40. (M · I)** Compare Target vs Equifax DiD?
> _Hint:_ Equifax = no detection (cert expired). Target = detection fired but no corrective response. Both DiD failures, different shapes.

**Q41. (H · I)** Architecture controls to prevent: list 4.
> _Hint:_ (1) Vendor network segmentation. (2) Vendor portal MFA. (3) POS isolation from corporate. (4) Tokenization → reduce data value. (5) Egress filtering.

**Q42. (M · I)** Cost to Target?
> _Hint:_ ~$292M (settlements, legal, IT). CEO + CIO out. Reputational drop.

**Q43. (H · I)** Post-Target industry change?
> _Hint:_ EMV chip rollout accelerated in US (Oct 2015 liability shift). PCI DSS updated. Vendor risk management programs matured.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** SOAR (Security Orchestration, Automation, Response) Detective→Corrective gap kaise pat karta?
> _Hint:_ Auto-runbooks: alert → enrich → quarantine → ticket → notify. Reduces MTTR drastically.

**Q45. (H · A)** XDR (Extended Detection & Response) vs SIEM?
> _Hint:_ XDR = pre-integrated cross-source detection + automated response. SIEM = log lake + rules (you build). XDR replaces some SIEM use-cases.

**Q46. (H · A)** "Autonomic security" — AI agents detect + respond without human?
> _Hint:_ Future direction. Risk: false positives auto-isolating critical services. Need guardrails + human escalation thresholds.

**Q47. (H · A)** "Deception technology" (Illusive, TrapX) — kaunsa function future?
> _Hint:_ Detective (high-fidelity — only attackers touch honeypots). Plus Delay/Deterrent. Active defense category.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** DiD (Day 5) ke har layer mein 4 function types — example layer mein.
> _Hint:_ Network layer: Preventive (firewall block), Detective (IDS), Corrective (auto-quarantine), Compensating (manual ACL when IDS down).

**Q49. (H · X)** STRIDE mitigations (Day 9) ko function types mein map karo.
> _Hint:_ Spoofing → Preventive (AuthN). Tampering → Preventive (HMAC) + Detective (integrity check). Repudiation → Detective (audit). DoS → Preventive (rate limit) + Corrective (autoscale). EoP → Preventive (least priv) + Detective (audit alarms).

**Q50. (H · X)** Trust boundary (Day 11) crossings pe kaunse 2 function controls minimum?
> _Hint:_ Preventive (AuthN/AuthZ/encryption) + Detective (log the crossing). 2 minimum, ideal all 4.

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#security-controls #preventive #detective #corrective #compensating #target-2013 #soar #xdr #function-nature-matrix #quiz-day-006
