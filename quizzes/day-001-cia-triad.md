# Day 001 Quiz — CIA Triad

**Total:** 50 deep questions
**Difficulty:** E = Easy · M = Medium · H = Hard
**Categories:** D = Definition · R = Relationship · S = ShopFast · B = Backend/System-X2 · I = Incident · A = Architect/Futurist · X = Cross-concept
**Self-grade:** 🟢 solid · 🟡 shaky · 🔴 guessed
**Rule:** Pehle answer khud likh, phir hint dekh. Peek = ❌.

---

## Section 1 — Core Definitions (Q1-Q10)

**Q1. (E · D)** CIA Triad ka full form aur har letter ka 1-line meaning likh.
> _Hint:_ Confidentiality (authorized access only), Integrity (no unauthorized change/tampering), Availability (accessible to authorized users when needed).

**Q2. (E · D)** Confidentiality aur "privacy" mein ek farak — kya security control privacy guarantee karta?
> _Hint:_ Confidentiality = technical (access control). Privacy = legal/policy (consent, purpose). C ⊂ Privacy.

**Q3. (E · D)** Integrity ka 1 example **data at rest** aur 1 example **data in transit** mein likh.
> _Hint:_ At rest = checksums/file hashes. In transit = HMAC/TLS message integrity.

**Q4. (E · D)** Availability metric kaunsa banta enterprise mein? 99.99% ka annual downtime kitna?
> _Hint:_ Uptime SLA. 99.99% ≈ 52 min/year downtime.

**Q5. (M · D)** Non-repudiation CIA ka hissa hai ya alag? Kahan fit hota?
> _Hint:_ Strictly nahi — Parkerian Hexad mein hai. CIA mein "Integrity" ke under stretch karte but real home AAA (Accounting/Audit).

**Q6. (M · D)** "Authenticity" vs "Integrity" — 1 line farak.
> _Hint:_ Integrity = data unchanged. Authenticity = data origin verified (who sent it).

**Q7. (E · D)** Confidentiality ke 3 control families likh.
> _Hint:_ Encryption (at rest, in transit), Access control (RBAC/ABAC), Data classification + DLP.

**Q8. (M · D)** Availability ke 3 attack types?
> _Hint:_ DoS/DDoS, Ransomware (data unavailable), Resource exhaustion (memory/file-handles).

**Q9. (M · D)** "Data Integrity" vs "System Integrity" — farak.
> _Hint:_ Data Integrity = file/record unchanged. System Integrity = OS/config/binaries unchanged (rootkits attack this).

**Q10. (H · D)** "Eventual consistency" (e.g., DynamoDB) Integrity violate karti hai? Argument banao.
> _Hint:_ Nahi — divergence transient hai, designed bounded staleness. Integrity violation hota jab unauthorized actor change kare. EC = a design tradeoff with Availability.

---

## Section 2 — Concept Relationships (Q11-Q20)

**Q11. (E · R)** C vs A tradeoff ka classic example?
> _Hint:_ Strong encryption + lost key = data unrecoverable. C raised, A dropped.

**Q12. (M · R)** I vs A tradeoff ka example?
> _Hint:_ Write-quorum (Raft/Paxos) increases I, lowers write availability under network partition (CAP).

**Q13. (M · R)** "Crown jewels" concept — CIA ke kaunsa letter usually prioritize hota financial system mein?
> _Hint:_ Integrity (transaction correctness > data secrecy or 5-min downtime).

**Q14. (M · R)** Healthcare records ke liye CIA priority order likh — kyun?
> _Hint:_ Usually A > I > C (life-saving access in ER vs HIPAA confidentiality). Debatable, business-context.

**Q15. (M · R)** Military/Intel: CIA priority kya?
> _Hint:_ C > I > A (better data destroyed than disclosed).

**Q16. (H · R)** "Data deletion on attack" Integrity violation hai ya Availability?
> _Hint:_ Both — deletion = no data (A failure) + unauthorized state change (I failure). Often counted as A.

**Q17. (H · R)** Ek scenario likh jahan C ko strengthen karne se I drop ho jaye.
> _Hint:_ Heavy encryption without integrity tag (e.g., AES-ECB without HMAC) → ciphertext malleable, I drops.

**Q18. (M · R)** Backup strategy CIA ke kaunse 2 letters serve karta?
> _Hint:_ I (point-in-time recovery if data tampered) + A (recovery from loss).

**Q19. (H · R)** Public-key crypto: C kaunsa key se? Authenticity kaunsa key se?
> _Hint:_ C = encrypt with recipient's public key. Authenticity = sign with sender's private key.

**Q20. (H · R)** Defense-in-depth (Day 5 preview) CIA ke kis pillar pe sabse zyada lagta?
> _Hint:_ Usually C — layered access controls. But truly applies to all three.

---

## Section 3 — ShopFast Application (Q21-Q28)

**Q21. (M · S)** ShopFast ke 3 crown jewels likh aur har ka primary CIA concern.
> _Hint:_ Payment Service (C+I — card data + transaction integrity); Auth Service (C+I — credentials + session integrity); Order DB (I+A — order correctness + uptime).

**Q22. (M · S)** ShopFast Product Catalog (read-only public listings) CIA priority?
> _Hint:_ A > I > C (public data, but defacement = brand damage, downtime = revenue loss).

**Q23. (M · S)** ShopFast user review system mein I violation kaisi dikhe?
> _Hint:_ Fake reviews, edited ratings, deleted negative reviews by attacker.

**Q24. (H · S)** Cart abandoned hone par cart data lost ho jaye — kaunsa CIA violation hai?
> _Hint:_ A (cart unavailable). I usually nahi unless cart silently changed.

**Q25. (H · S)** ShopFast checkout page mein "cart price 100x kar diya by user via tampering" — kaunsa CIA?
> _Hint:_ Integrity (server-side validation missing). NEVER trust client-sent price.

**Q26. (M · S)** Customer email leak through ShopFast — kaunsa pillar, aur kaunsa regulatory implication?
> _Hint:_ Confidentiality. GDPR (EU), CCPA (CA), PDPL (Pakistan/KSA).

**Q27. (M · S)** ShopFast ke promotional banner pe attacker malicious script inject kare (XSS) — kaunsa CIA primary?
> _Hint:_ Integrity (page content tampered). Downstream → C compromise of users.

**Q28. (H · S)** ShopFast inventory count gadbad — "1 stock dikhata, 50 orders accept karta" — yeh kaunsa CIA failure?
> _Hint:_ Integrity of inventory state (race condition / no atomic decrement). Business consequence: overselling.

---

## Section 4 — Backend / System-X2 Application (Q29-Q35)

**Q29. (M · B)** Microservice-to-microservice call mein C kaise enforce karta?
> _Hint:_ mTLS (Service Mesh / SPIFFE) + service-level authorization tokens.

**Q30. (M · B)** PostgreSQL row-level security CIA ke kis pillar ko strengthen?
> _Hint:_ Confidentiality (multi-tenant isolation at DB layer).

**Q31. (M · B)** "Audit log table" CIA ke kis pillar ko enforce?
> _Hint:_ Integrity (proof of what happened) + Accountability (3rd A). Append-only ideal.

**Q32. (H · B)** [Authorize(Roles="Finance")] attribute .NET Core mein — C ya I?
> _Hint:_ Confidentiality (access control gate) — but indirectly Integrity (only Finance can modify finance data).

**Q33. (H · B)** Redis cache poisoning karne wale attack mein CIA ka kya failure?
> _Hint:_ Integrity (stale/wrong data served as authoritative). Indirect C compromise possible.

**Q34. (M · B)** Health endpoints (`/health`) public expose karna — kaunsa CIA risk?
> _Hint:_ C (info disclosure of versions, dependencies); aids reconnaissance for A attacks (DoS).

**Q35. (H · B)** Database backup tape unencrypted truck mein loaded — kaunsa CIA risk + control?
> _Hint:_ Confidentiality. Control: backup encryption + key management (KMS/HSM).

---

## Section 5 — Incident Lens (Q36-Q43)

**Q36. (M · I)** Equifax 2017 (preview) — primary CIA pillar violated?
> _Hint:_ Confidentiality (147M PII leaked).

**Q37. (M · I)** Ransomware attack (e.g., Colonial Pipeline 2021) — kaunsa CIA pillar primarily?
> _Hint:_ Availability (data encrypted/unavailable). Modern double-extortion bhi C breach.

**Q38. (M · I)** Stuxnet (2010, Iran nuclear) — kaunsa pillar?
> _Hint:_ Integrity — centrifuge control values tampered while showing normal readings.

**Q39. (H · I)** SolarWinds 2020 — primary pillar agar binary tampered tha?
> _Hint:_ Integrity (signed but tainted) + downstream C breach of victim orgs.

**Q40. (M · I)** Website defacement (e.g., political hacktivism) — pillar?
> _Hint:_ Integrity. Often A also (downtime).

**Q41. (H · I)** "MitM TLS strip" attack — pillar?
> _Hint:_ Confidentiality primary, Integrity secondary (data can be modified mid-flight).

**Q42. (H · I)** "Slow DDoS" (Slowloris) — pillar + why "slow"?
> _Hint:_ Availability. Slow = exhausts connection slots with low bandwidth — bypasses naive rate-limit.

**Q43. (H · I)** "Logic bomb" by departing employee deleting prod data — pillar(s)?
> _Hint:_ Integrity (unauthorized deletion) + Availability (data lost). Insider threat → Day 4 AAA.

---

## Section 6 — Architect & Futurist (Q44-Q47)

**Q44. (H · A)** Parkerian Hexad ke 3 extra pillars CIA mein add kiye — list.
> _Hint:_ Possession/Control, Authenticity, Utility.

**Q45. (H · A)** AI agents ke aane se CIA challenge kya? Kaunsa naya pillar baat ho rahi?
> _Hint:_ Non-repudiation of agent actions (kaun bola "approve $1M transfer"). Provenance + agent identity.

**Q46. (H · A)** Quantum computing CIA ke kis pillar ko threaten karta hai sabse pehle?
> _Hint:_ Confidentiality — "harvest now, decrypt later" against RSA/ECC. Post-quantum crypto (Kyber, Dilithium) ka motivation.

**Q47. (H · A)** Confidential computing (Intel SGX, AMD SEV) CIA ke kis pillar ko strengthen?
> _Hint:_ Confidentiality of data **in use** (encrypted memory, attested enclaves).

---

## Section 7 — Cross-Concept Connections (Q48-Q50)

**Q48. (H · X)** STRIDE (Day 9) ke 6 letters CIA ke teen ko negate karte hain — har CIA letter pe map kar.
> _Hint:_ C → Information Disclosure; I → Tampering; A → DoS. (Plus AAA: S→AuthN, R→Accounting, E→AuthZ.)

**Q49. (H · X)** Defense-in-depth (Day 5) CIA ke teeno pillars pe lagta — ek example layer dene wala har pillar ke liye.
> _Hint:_ C: TLS + DB encryption + RBAC. I: HMAC + audit log + WAF. A: CDN + autoscale + DR.

**Q50. (H · X)** Threat modeling (Day 8) shuru karne se pehle har asset par CIA tag kyun lagaate hain?
> _Hint:_ Priority — ek asset ki C critical, doosre ki A. Aage mitigations isi ke hisaab se budget hote.

---

## Scorecard

- 🟢 solid: ___ / 50
- 🟡 shaky: ___ / 50
- 🔴 guessed: ___ / 50
- **Re-revise topics:** ___

---

#cia-triad #confidentiality #integrity #availability #parkerian-hexad #non-repudiation #shopfast #quiz-day-001
