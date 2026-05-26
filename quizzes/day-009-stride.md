# Day 009 Quiz — STRIDE Methodology

**Total:** 50 deep questions
**Difficulty:** E / M / H · **Categories:** D / R / S / B / I / A / X
**Self-grade:** 🟢 🟡 🔴

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** STRIDE acronym expand karo + 1-line meaning per letter.
> _Hint:_ Spoofing (identity), Tampering (data/code change), Repudiation (deny action), Information Disclosure (leak), Denial of Service (availability), Elevation of Privilege (gain more rights).

**Q2. (E · D)** STRIDE kis MS engineer ne propose kiya + kab?
> _Hint:_ Loren Kohnfelder + Praerit Garg, Microsoft, 1999. "The threats to our products."

**Q3. (M · D)** STRIDE CIA+AAA ke negation hai — pair kar.
> _Hint:_ S↔AuthN, T↔Integrity, R↔Non-repudiation/Accounting, I↔Confidentiality, D↔Availability, E↔AuthZ.

**Q4. (M · D)** STRIDE-per-element matlab?
> _Hint:_ Each DFD element gets its applicable STRIDE letters — not all letters apply to all elements.

**Q5. (M · D)** STRIDE-per-interaction matlab + per-element se kab better?
> _Hint:_ Focus on data flows between elements. Better when interactions complex (mTLS, API contracts). Per-element simpler for first pass.

**Q6. (M · D)** Spoofing ke 3 examples.
> _Hint:_ Credential phishing, session hijack, fake DNS/CA, IP spoofing, GPS spoofing.

**Q7. (M · D)** Tampering ke 3 examples.
> _Hint:_ HTTP param manipulation, DB record tamper, binary/file modification, log file deletion, MitM mid-flight.

**Q8. (M · D)** Repudiation ke 2 examples.
> _Hint:_ Admin deletes log to deny action; user denies "I never placed that order"; insider claims "system bug" for fraud.

**Q9. (M · D)** Info Disclosure 3 examples.
> _Hint:_ DB dump, error message leaks stack/SQL, S3 public, verbose logging, side-channel (timing, cache).

**Q10. (M · D)** DoS 3 categories.
> _Hint:_ Volumetric (bandwidth), Protocol (SYN flood, Slowloris), Application-layer (HTTP flood, expensive queries).

---

## Section 2 — STRIDE-per-Element Matrix (Q11-Q20)

**Q11. (M · R)** External Entity pe applicable STRIDE letters?
> _Hint:_ S + R only. (Can be spoofed, can repudiate; can't really tamper/leak from itself; DoS targets system not entity; EoP needs context).

**Q12. (M · R)** Process pe applicable STRIDE letters?
> _Hint:_ All 6 (S, T, R, I, D, E) — full spectrum.

**Q13. (M · R)** Data Store pe applicable?
> _Hint:_ T, R, I, D — no S (data doesn't impersonate). E debatable (privilege boundary at access).

**Q14. (M · R)** Data Flow pe applicable?
> _Hint:_ T, I, D — flow can be tampered, sniffed, disrupted. Not S/R/E (it's a channel, not actor).

**Q15. (H · R)** Per-element table memorize karne ka mnemonic?
> _Hint:_ "EE: S R" · "Proc: All" · "Store: T R I D" · "Flow: T I D". Or remember "actors do S/R/E; channels do T/I/D".

**Q16. (M · R)** External Entity pe Tampering kyun nahi?
> _Hint:_ EE = outside system; we can't model its internal integrity. We model only its interaction.

**Q17. (H · R)** Data Store pe Repudiation kya matlab?
> _Hint:_ Store doesn't keep tamper-evident record of who-wrote-what. Admin can deny. Mitigation: signed/append-only audit log.

**Q18. (H · R)** Process pe Spoofing ka practical scenario?
> _Hint:_ One microservice impersonates another (no mTLS). Or attacker spawns rogue process with legit name.

**Q19. (H · R)** Process pe Elevation real-world?
> _Hint:_ Container escape, sudo misconfig, vulnerable kernel module, RCE → service account → root.

**Q20. (H · R)** Data Flow pe Info Disclosure default mitigation?
> _Hint:_ Encryption in transit (TLS, mTLS, IPsec, WireGuard).

---

## Section 3 — Canonical Mitigations Per Letter (Q21-Q28)

**Q21. (M · D)** S → mitigation 3?
> _Hint:_ AuthN (MFA, certs, FIDO2), workload identity (SPIFFE), DNS-SEC, anti-phishing.

**Q22. (M · D)** T → mitigation 3?
> _Hint:_ Integrity protection (HMAC, signatures, hashes), code signing, immutable infra, RBAC/least-priv, WAF.

**Q23. (M · D)** R → mitigation 3?
> _Hint:_ Signed audit logs, append-only WORM, separate logging account, digital signatures on actions, NTP-synced timestamps.

**Q24. (M · D)** I → mitigation 3?
> _Hint:_ Encryption at rest + in transit, least-priv access, error sanitization, data classification + DLP.

**Q25. (M · D)** D → mitigation 3?
> _Hint:_ Rate limiting, CDN/anti-DDoS, autoscaling, circuit breakers, queue back-pressure.

**Q26. (M · D)** E → mitigation 3?
> _Hint:_ Least-priv, separation of duties, input validation, sandboxing, container isolation (seccomp/AppArmor), MFA for sensitive actions.

**Q27. (H · D)** "Defense across STRIDE" — har letter pe layered controls list (DiD + STRIDE).
> _Hint:_ S: AuthN + step-up + behavioral. T: HMAC + WAF + DAM. R: log + sign + offsite. I: encrypt + DLP + redaction. D: WAF + CDN + autoscale + circuit. E: least-priv + sandbox + audit.

**Q28. (H · D)** STRIDE threat ko **eliminate** vs **mitigate** — kab kya?
> _Hint:_ Eliminate when feasible (don't store the data → no I). Mitigate when feature requires existence (encrypt, AuthZ, log).

---

## Section 4 — ShopFast & Backend Application (Q29-Q35)

**Q29. (M · S)** ShopFast login process pe STRIDE walkthrough — 2 threats per letter.
> _Hint:_ S: phishing, credential stuffing. T: tampered JWT, modified password reset email. R: deleted login logs. I: password leak, session token leak. D: brute-force account lockout abuse, login endpoint DDoS. E: privilege escalation via role param.

**Q30. (M · B)** Spring Boot API pe T threat aur mitigation.
> _Hint:_ Tampered request body (e.g., `userId` swap). Mitigation: server-side authoritative ID from JWT, not from body. `@Valid` + signed claims.

**Q31. (M · B)** Microservice-to-microservice call pe S threat?
> _Hint:_ Rogue service in cluster impersonating peer. Mitigation: mTLS + SPIFFE workload identity.

**Q32. (H · S)** ShopFast me Info Disclosure jo developers commonly miss?
> _Hint:_ GraphQL introspection in prod, error stack traces, debug headers (X-Powered-By), JWT in URL query, S3 bucket listing.

**Q33. (H · B)** Kubernetes pe Elevation threat aur defense.
> _Hint:_ Pod with `hostPID:true` or `privileged:true` → escape. Defense: Pod Security Standards (restricted), seccomp profile, no privileged containers.

**Q34. (H · S)** ShopFast checkout pe DoS variants 3?
> _Hint:_ HTTP flood, expensive query (regex DoS), inventory race-condition lock-up, cart-add slow API endpoint exhaustion.

**Q35. (H · B)** Async messaging (RabbitMQ/Kafka) pe STRIDE threats — 1 per letter.
> _Hint:_ S: producer spoofing topic. T: message body tamper in transit. R: consumer denies processing. I: message body leaked in logs. D: queue flooding. E: poison message → consumer crash → priv esc via DLQ admin.

---

## Section 5 — Incident: Zerologon CVE-2020-1472 (Q36-Q43)

**Q36. (M · I)** Zerologon affects + CVSS?
> _Hint:_ Microsoft Netlogon (AD DC). CVSS 10.0 critical. CVE-2020-1472. Disclosed Aug 2020 (researcher Tom Tervoort, Secura).

**Q37. (H · I)** Root cause cryptographic flaw?
> _Hint:_ AES-CFB8 IV set to all-zeros in Netlogon authentication. Cipher property: 1-in-256 attempts results in all-zero ciphertext → bypass auth.

**Q38. (H · I)** Exploitation impact?
> _Hint:_ Unauthenticated attacker on network → reset domain controller's machine password to empty → full domain admin → game over.

**Q39. (H · I)** STRIDE letters primarily violated?
> _Hint:_ S (auth bypass = spoofing the DC's own account) + E (elevation to domain admin). Crypto design flaw enabling both.

**Q40. (H · I)** "Insecure design" classification?
> _Hint:_ Yes — IV choice was deliberate (for some compatibility reason). Crypto best-practice = unique IV per encryption. Design-stage threat model would have flagged.

**Q41. (M · I)** Patch timeline / Microsoft response?
> _Hint:_ Patch Aug 11, 2020. Enforcement mode Feb 9, 2021. Many orgs delayed → exploited. CISA emergency directive issued.

**Q42. (H · I)** Architectural lesson?
> _Hint:_ Crypto primitives must be reviewed for non-standard parameter choices. "Don't roll your own crypto" + audit even standard-looking code.

**Q43. (H · I)** Detection signal for Zerologon exploit?
> _Hint:_ Event ID 4742 with anomalous password change for DC machine account. SIEM rule + immediate alert.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** STRIDE 2.0 / extensions kya hain?
> _Hint:_ ASTRIDE (privacy + adversarial AI angle), LINDDUN (privacy-focused), STRIDE-LM (LLM-specific). STRIDE remains base.

**Q45. (H · A)** STRIDE-LM AI/LLM apps mein kya add karta?
> _Hint:_ Prompt injection (S/T blend), training data poisoning (T), model extraction (I), agentic abuse (E), hallucination-driven misinfo (T+R blend).

**Q46. (H · A)** "STRIDE for hardware/IoT" — kaunse 2 special letters need attention?
> _Hint:_ T (firmware tamper), I (side-channel — power, EM, timing). Plus physical access factor.

**Q47. (H · A)** Auto-generated threat models via LLM (STRIDE-GPT) — 3 risks?
> _Hint:_ Hallucinated threats, missed novel ones, biased to common patterns, false sense of "thoroughness", over-reliance reduces dev thinking.

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** STRIDE Shostack Q2 ka answer kyun?
> _Hint:_ STRIDE = enumeration framework for "what can go wrong" — directly answers Q2 across DFD elements.

**Q49. (H · X)** DFD (Day 10) STRIDE ke liye kyun pre-requisite?
> _Hint:_ STRIDE applies to elements; elements come from DFD. No diagram → no per-element analysis → STRIDE incomplete.

**Q50. (H · X)** DREAD (Day 12) STRIDE ke baad kyun?
> _Hint:_ STRIDE identifies threats; DREAD scores them. Without DREAD/CVSS, STRIDE list = unprioritized backlog.

---

## Scorecard
- 🟢 ___/50 · 🟡 ___/50 · 🔴 ___/50
- **Re-revise:** ___

---

#stride #spoofing #tampering #repudiation #information-disclosure #denial-of-service #elevation-of-privilege #zerologon #cve-2020-1472 #stride-per-element #stride-lm #quiz-day-009
