# 📖 CyberForge 365 — Master Curriculum

**Version:** 1.0
**Mode:** Beginner-friendly, depth-first, incident-driven
**Time commitment:** 60–90 minutes/day, 7 days/week

---

## 🧭 How This Curriculum Works

This is a **scaffolding**, not a script. The daily Claude routine uses this file to know *where I am* and *what to teach*. Each week has:

- **Theme** — the big idea
- **Daily topics** (Days 1-6) — what gets taught
- **Day 7** — recap & quiz (always)
- **Featured incident** — one real-world breach tied to the week
- **Week lab** — hands-on project completed by Day 7
- **Futurist note** — how this concept evolves in the next 5 years

The routine will expand each day into a full lesson with examples, analogies (drawn from backend dev / microservices), and exact tool instructions.

---

# 🌱 QUARTER 1: FOUNDATIONS (Days 1-90)

> *"You can't defend what you don't understand."*

Goal by Day 90: Speak the language of security. Threat-model any system. Understand encryption, authentication, and authorization at depth.

---

## Month 1: Security First Principles (Days 1-30)

### Week 1 (Days 1-7): The Security Mindset
- **Day 1:** What is cybersecurity? CIA triad (Confidentiality, Integrity, Availability)
- **Day 2:** Threat vs. vulnerability vs. risk vs. exploit — the vocabulary
- **Day 3:** Attack surface, attack vectors, blast radius
- **Day 4:** AAA (Authentication, Authorization, Accounting/Auditing)
- **Day 5:** Defense-in-depth — why one layer is never enough
- **Day 6:** Security control types: preventive, detective, corrective, compensating
- **Day 7:** 🔄 Week recap + 5-question quiz

**🎯 Featured incident:** **Equifax (2017)** — 147M records lost to an unpatched Apache Struts vulnerability. Single point of failure, no segmentation.

**🔬 Week lab:** Create your first **risk register** for a fictional enterprise system. Identify 10 assets, score them with CVSS basics.

**🚀 Futurist note:** The CIA triad is being challenged by AI agents that need *non-repudiable* actions. New models like Parkerian Hexad gain relevance.

---

### Week 2 (Days 8-14): Threat Modeling Foundations
- **Day 8:** Why threat modeling matters — shifting security left
- **Day 9:** STRIDE methodology (Spoofing, Tampering, Repudiation, Info Disclosure, DoS, EoP)
- **Day 10:** Data Flow Diagrams (DFDs) — your first one
- **Day 11:** Trust boundaries — where threats live
- **Day 12:** DREAD scoring & threat prioritization
- **Day 13:** Attack trees — thinking like an adversary
- **Day 14:** 🔄 Week recap + threat-modeling drill

**🎯 Featured incident:** **Target (2013)** — Attackers entered via an HVAC vendor, pivoted to POS systems. No trust boundaries.

**🔬 Week lab:** Threat-model a sample microservice architecture (provided by routine). Output: DFD + STRIDE table + top 5 prioritized threats.

**🚀 Futurist note:** Threat modeling for AI systems — prompt injection, training data poisoning, model extraction — is the new frontier.

---

### Week 3 (Days 15-21): Risk Management & Governance
- **Day 15:** Quantitative vs. qualitative risk analysis
- **Day 16:** Risk treatment: accept, avoid, transfer, mitigate
- **Day 17:** Risk appetite & risk tolerance — the business side
- **Day 18:** Security policies, standards, procedures, guidelines
- **Day 19:** Intro to compliance frameworks: NIST CSF, ISO 27001, SOC 2
- **Day 20:** GRC (Governance, Risk, Compliance) — what it means in practice
- **Day 21:** 🔄 Week recap

**🎯 Featured incident:** **Uber breach cover-up (2016)** — Lack of governance led to a $148M settlement. Policy failure, not technical.

**🔬 Week lab:** Map a fictional company's controls to NIST CSF's 5 functions (Identify, Protect, Detect, Respond, Recover).

---

### Week 4 (Days 22-30): The Monthly Capstone
- **Day 22-27:** Apply Week 1-3 concepts to a fictional "ShopFast" e-commerce platform
- **Day 28:** Build the full threat model
- **Day 29:** Create a risk register with treatments
- **Day 30:** 🏆 **MONTHLY CAPSTONE** — Publish to `portfolio/month-01-threat-model.md`

**🎯 Featured incident:** **Yahoo (2013-14)** — 3 billion accounts compromised. Detection failure, governance failure, response failure.

**🔬 Capstone deliverable:** Portfolio-grade threat model document with DFD, STRIDE analysis, risk register, and top 10 prioritized findings. Publishable on LinkedIn.

---

## Month 2: Cryptography Demystified (Days 31-60)

### Week 5 (Days 31-37): Crypto Foundations
- **Day 31:** Why crypto matters — the math you actually need
- **Day 32:** Symmetric encryption (AES, ChaCha20) — fast, shared secret
- **Day 33:** Asymmetric encryption (RSA, ECC) — slow, key pairs
- **Day 34:** Hash functions (SHA-2, SHA-3, BLAKE3) — one-way magic
- **Day 35:** Message Authentication Codes (HMAC) — integrity proof
- **Day 36:** Digital signatures — non-repudiation in practice
- **Day 37:** 🔄 Week recap + crypto vocabulary drill

**🎯 Featured incident:** **LinkedIn (2012)** — 6.5M password hashes leaked, cracked in days. Unsalted SHA-1 = catastrophe.

**🔬 Week lab:** In .NET Core, demonstrate AES encryption, RSA key pair generation, SHA-256 hashing, and HMAC signing. Compare timings.

---

### Week 6 (Days 38-44): TLS, PKI & Certificates
- **Day 38:** What HTTPS actually does — the TLS 1.3 handshake
- **Day 39:** Public Key Infrastructure (PKI) — the trust chain
- **Day 40:** X.509 certificates — anatomy of a cert
- **Day 41:** Certificate Authorities (CAs), root stores, certificate transparency
- **Day 42:** Certificate pinning, HSTS, OCSP
- **Day 43:** mTLS (mutual TLS) — service-to-service trust
- **Day 44:** 🔄 Week recap

**🎯 Featured incident:** **DigiNotar (2011)** — Compromised CA issued fake Google certs to surveil Iranian users. The whole trust chain crumbled.

**🔬 Week lab:** Set up a local CA with `openssl`, issue certs for two microservices, enable mTLS between them. Use Wireshark to confirm encryption.

---

### Week 7 (Days 45-51): Encryption in Practice
- **Day 45:** Data in transit vs. data at rest vs. data in use
- **Day 46:** Database encryption: TDE, column-level, application-level
- **Day 47:** Disk encryption (LUKS, BitLocker) — what it protects
- **Day 48:** Confidential computing & TEEs (Intel SGX, AMD SEV) — data in use
- **Day 49:** Homomorphic encryption — compute on encrypted data
- **Day 50:** End-to-end encryption (Signal protocol, MLS)
- **Day 51:** 🔄 Week recap

**🎯 Featured incident:** **Capital One (2019)** — 100M records exfiltrated. Data was encrypted at rest, but credentials gave plain-text access.

**🔬 Week lab:** Configure MySQL TDE for a sample database. Then implement column-level encryption for PII in an ASP.NET Core app.

**🚀 Futurist note:** Confidential computing is becoming the default for sensitive cloud workloads. Homomorphic encryption will unlock medical/financial AI.

---

### Week 8 (Days 52-60): Key Management & Post-Quantum
- **Day 52:** The hardest problem in crypto: key management
- **Day 53:** Key lifecycle: generation, storage, rotation, revocation, destruction
- **Day 54:** HSMs (Hardware Security Modules) — what & why
- **Day 55:** Cloud KMS: AWS KMS, Azure Key Vault, GCP KMS
- **Day 56:** Secrets management: HashiCorp Vault, Doppler, AWS Secrets Manager
- **Day 57:** Post-quantum cryptography — the "harvest now, decrypt later" threat
- **Day 58:** NIST PQC standards: Kyber, Dilithium, SPHINCS+
- **Day 59-60:** 🏆 **MONTHLY CAPSTONE** — Crypto architecture for ShopFast

**🎯 Featured incident:** **Solar Winds (2020)** — SAML token forgery via stolen signing key. Key compromise = total trust compromise.

**🔬 Capstone:** Design and document a complete cryptographic architecture for the ShopFast platform, including PQ-readiness plan. Publish to portfolio.

**🚀 Futurist note:** By 2030, NIST mandates PQ migration for federal systems. Banks and healthcare follow. Start now.

---

## Month 3: Identity, Authentication & Authorization (Days 61-90)

### Week 9 (Days 61-67): Authentication Deep Dive
- **Day 61:** Passwords — why they're the worst (and still everywhere)
- **Day 62:** Password storage done right: bcrypt, scrypt, Argon2id
- **Day 63:** MFA: TOTP, push, SMS (and why SMS is broken)
- **Day 64:** FIDO2 / WebAuthn / Passkeys — the passwordless future
- **Day 65:** Hardware security keys (YubiKey, Titan)
- **Day 66:** Biometrics — convenience vs. security trade-offs
- **Day 67:** 🔄 Week recap

**🎯 Featured incident:** **Twitter Bitcoin Scam (2020)** — Social engineering + weak MFA on admin tools = celebrity account takeover.

**🔬 Week lab:** Implement passkeys (WebAuthn) in a sample web app. Compare with TOTP and SMS-based MFA.

---

### Week 10 (Days 68-74): OAuth, OIDC & SSO
- **Day 68:** OAuth 2.0 — delegated authorization (not authentication!)
- **Day 69:** OAuth flows: Authorization Code + PKCE, Client Credentials, Device Code
- **Day 70:** OpenID Connect (OIDC) — identity on top of OAuth
- **Day 71:** JWT (JSON Web Tokens) — anatomy & pitfalls
- **Day 72:** SAML 2.0 — the enterprise SSO standard
- **Day 73:** SSO architectures: IdP, SP, federated identity
- **Day 74:** 🔄 Week recap

**🎯 Featured incident:** **Okta breach (2022)** — Sub-processor compromise gave attackers access to customer support tools, exposing client data.

**🔬 Week lab:** Build an OAuth 2.0 + OIDC flow with Auth0 / Keycloak. Inspect JWTs. Implement refresh token rotation.

---

### Week 11 (Days 75-81): Authorization Models
- **Day 75:** Principle of Least Privilege (PoLP) — the foundation
- **Day 76:** RBAC (Role-Based Access Control) — when & how
- **Day 77:** ABAC (Attribute-Based Access Control) — fine-grained control
- **Day 78:** ReBAC (Relationship-Based) — Google Zanzibar style
- **Day 79:** Policy-as-code: OPA (Open Policy Agent), Cedar
- **Day 80:** Just-in-Time (JIT) access & privileged access management (PAM)
- **Day 81:** 🔄 Week recap

**🎯 Featured incident:** **Microsoft Storm-0558 (2023)** — Stolen consumer signing key forged enterprise tokens. Authorization model failure.

**🔬 Week lab:** Implement RBAC + ABAC in an ASP.NET Core API using OPA for policy decisions.

---

### Week 12 (Days 82-90): Identity at Enterprise Scale
- **Day 82:** Directory services: Active Directory, LDAP, Azure AD / Entra ID
- **Day 83:** Identity federation & cross-domain trust
- **Day 84:** Service accounts, workload identity, SPIFFE/SPIRE
- **Day 85:** Identity governance & administration (IGA)
- **Day 86:** Zero-Trust identity model — identity as the new perimeter
- **Day 87-89:** 🏆 **QUARTERLY CAPSTONE PREP** — IAM redesign for ShopFast
- **Day 90:** 🎯 **Q1 ASSESSMENT** — Full architecture review + publication

**🎯 Featured incident:** **SolarWinds → Microsoft (2020-21)** — Forged SAML tokens via golden SAML attack accessed Microsoft cloud tenants.

**🏆 Q1 Quarterly Capstone:** Publish a long-form article: *"Identity-First Security: Redesigning ShopFast's Auth Architecture"* — to blog/LinkedIn/dev.to.

**🚀 Futurist note:** Workload identity (SPIFFE) is replacing API keys. Decentralized identity (DIDs, verifiable credentials) is rising in finance/gov.

---

# 🌿 QUARTER 2: DOMAIN DEEP DIVES (Days 91-180)

> *"Security is not a product, but a process."* — Bruce Schneier

Goal by Day 180: Master the four core security domains — Network, Application, Cloud, Compliance.

---

## Month 4: Network Security (Days 91-120)

### Week 13 (Days 91-97): Network Fundamentals for Security
- **Day 91:** OSI & TCP/IP refresher through a security lens
- **Day 92:** Common ports, protocols, and their vulnerabilities
- **Day 93:** DNS — how it works & how it gets attacked
- **Day 94:** BGP, ASN, internet routing security
- **Day 95:** Network segmentation strategies
- **Day 96:** DMZ pattern, jump hosts, bastion architecture
- **Day 97:** 🔄 Week recap

**🎯 Featured incident:** **Dyn DNS DDoS (2016)** — Mirai botnet took down Twitter, Netflix, Reddit. DNS = single point of failure.

**🔬 Week lab:** Set up a basic network in AWS: VPC, public/private subnets, NAT gateway. Verify segmentation with `nmap` and traceroute.

---

### Week 14 (Days 98-104): Firewalls, IDS/IPS & WAF
- **Day 98:** Stateful firewalls vs. NGFW (Next-Gen Firewalls)
- **Day 99:** IDS vs. IPS — detection vs. prevention
- **Day 100:** Signature-based vs. anomaly-based detection
- **Day 101:** WAF (Web Application Firewall) — OWASP CRS rules
- **Day 102:** Network DLP (Data Loss Prevention)
- **Day 103:** Honeypots & deception technology
- **Day 104:** 🔄 Week recap

**🎯 Featured incident:** **Capital One (2019, revisited)** — Misconfigured WAF allowed SSRF to AWS metadata service. Defense bypassed.

**🔬 Week lab:** Deploy AWS WAF in front of a vulnerable app. Block OWASP Top 10 attack patterns. Test with OWASP ZAP.

---

### Week 15 (Days 105-111): VPN, Zero-Trust Networks
- **Day 105:** VPN technologies: IPSec, OpenVPN, WireGuard
- **Day 106:** Why VPNs are dying — the perimeter problem
- **Day 107:** Zero-Trust Network Access (ZTNA) — never trust, always verify
- **Day 108:** Microsegmentation — isolating workloads
- **Day 109:** Service mesh (Istio, Linkerd) for zero-trust
- **Day 110:** SASE & SSE — converged network security
- **Day 111:** 🔄 Week recap

**🎯 Featured incident:** **Colonial Pipeline (2021)** — Single compromised VPN credential, no MFA, no segmentation = $4.4M ransom + national crisis.

**🔬 Week lab:** Deploy Istio service mesh on a K8s cluster. Enable mTLS between services. Define authorization policies.

**🚀 Futurist note:** SASE/ZTNA replacing VPN in enterprises by 2027. eBPF-based runtime security (Cilium, Falco) is rising fast.

---

### Week 16 (Days 112-120): DDoS, Egress, Monitoring + Capstone
- **Day 112:** DDoS attacks: volumetric, protocol, application-layer
- **Day 113:** DDoS mitigation: scrubbing centers, Cloudflare, AWS Shield
- **Day 114:** Egress filtering — controlling outbound traffic
- **Day 115:** Network monitoring: NetFlow, sFlow, packet capture
- **Day 116:** Network forensics fundamentals
- **Day 117-119:** 🏆 **MONTHLY CAPSTONE** — Zero-Trust network design for ShopFast
- **Day 120:** Publish to portfolio

**🎯 Featured incident:** **GitHub DDoS (2018)** — 1.35 Tbps memcached amplification, largest DDoS ever. Mitigated in 10 minutes.

**🏆 Monthly Capstone:** Design a zero-trust network architecture for a multi-region SaaS platform. Diagrams + decision document.

---

## Month 5: Application Security (Days 121-150)

### Week 17 (Days 121-127): OWASP Top 10 (Part 1)
- **Day 121:** A01: Broken Access Control — IDOR, privilege escalation
- **Day 122:** A02: Cryptographic Failures
- **Day 123:** A03: Injection — SQL, NoSQL, command, LDAP
- **Day 124:** A04: Insecure Design — security in design phase
- **Day 125:** A05: Security Misconfiguration
- **Day 126:** Practical exploitation in WebGoat / DVWA
- **Day 127:** 🔄 Week recap

**🎯 Featured incident:** **OPM (2015)** — SQL injection in a contractor portal exposed 21.5M federal employee records.

**🔬 Week lab:** Exploit & remediate OWASP Top 10 (A01-A05) vulnerabilities in OWASP WebGoat.

---

### Week 18 (Days 128-134): OWASP Top 10 (Part 2)
- **Day 128:** A06: Vulnerable and Outdated Components
- **Day 129:** A07: Identification & Authentication Failures
- **Day 130:** A08: Software & Data Integrity Failures
- **Day 131:** A09: Security Logging & Monitoring Failures
- **Day 132:** A10: Server-Side Request Forgery (SSRF)
- **Day 133:** OWASP API Top 10 — APIs are different
- **Day 134:** 🔄 Week recap

**🎯 Featured incident:** **Log4Shell (CVE-2021-44228)** — JNDI lookup vulnerability in ubiquitous Java logging library. Wormable, internet-scale.

**🔬 Week lab:** Reproduce Log4Shell in a sandboxed app. Then patch it. Document detection rules.

---

### Week 19 (Days 135-141): Secure SDLC & DevSecOps
- **Day 135:** Shift-left security — finding bugs early
- **Day 136:** SAST (Static Analysis) — SonarQube, Semgrep, CodeQL
- **Day 137:** DAST (Dynamic Analysis) — Burp Suite, OWASP ZAP
- **Day 138:** SCA (Software Composition Analysis) — Snyk, Dependabot, Trivy
- **Day 139:** IAST & RASP — runtime protection
- **Day 140:** Secrets scanning: GitLeaks, TruffleHog
- **Day 141:** 🔄 Week recap

**🎯 Featured incident:** **Uber GitHub leak (2016)** — AWS keys committed to GitHub. Led to 57M-record breach.

**🔬 Week lab:** Add SAST (Semgrep), SCA (Trivy), and secrets scanning (GitLeaks) to a GitHub Actions CI pipeline.

---

### Week 20 (Days 142-150): API Security + Capstone
- **Day 142:** REST API security: auth, rate limiting, input validation
- **Day 143:** GraphQL security pitfalls (introspection, deep queries)
- **Day 144:** gRPC & service-to-service security
- **Day 145:** Webhook security — verification & replay protection
- **Day 146:** API Gateway patterns (Kong, AWS API Gateway, Apigee)
- **Day 147-149:** 🏆 **MONTHLY CAPSTONE** — Secure API design for ShopFast
- **Day 150:** Publish

**🎯 Featured incident:** **Optus API breach (2022)** — Unauthenticated API endpoint exposed 9.8M customers' data in Australia.

**🏆 Monthly Capstone:** Design & document a hardened API security architecture: auth, rate limiting, WAF rules, monitoring, incident playbook.

---

## Month 6: Cloud Security & Compliance (Days 151-180)

### Week 21 (Days 151-157): Cloud Security Model
- **Day 151:** Shared responsibility model — who owns what
- **Day 152:** Cloud IAM deep dive (AWS IAM, Azure RBAC, GCP IAM)
- **Day 153:** IAM policies, roles, federation patterns
- **Day 154:** Cloud secrets & KMS
- **Day 155:** Cloud networking security (VPC, Security Groups, NACLs)
- **Day 156:** Cloud logging: CloudTrail, Azure Activity Log, GCP Audit Logs
- **Day 157:** 🔄 Week recap

**🎯 Featured incident:** **Capital One (2019, revisited)** — Cloud IAM + SSRF + over-privileged role = 100M records gone.

**🔬 Week lab:** Build a least-privilege IAM policy in AWS. Use IAM Access Analyzer. Test denial paths.

---

### Week 22 (Days 158-164): Container & Kubernetes Security
- **Day 158:** Container fundamentals through security eyes
- **Day 159:** Image security: minimal base images, vulnerability scanning
- **Day 160:** Runtime security: Falco, eBPF-based detection
- **Day 161:** Kubernetes security: RBAC, NetworkPolicies, PodSecurityStandards
- **Day 162:** Admission controllers: OPA Gatekeeper, Kyverno
- **Day 163:** Secrets in K8s: External Secrets Operator, SOPS
- **Day 164:** 🔄 Week recap

**🎯 Featured incident:** **Tesla Kubernetes mining (2018)** — Unsecured K8s dashboard exposed AWS creds, used for crypto mining.

**🔬 Week lab:** Harden a K8s cluster: enable PodSecurityStandards, NetworkPolicies, Falco runtime detection.

---

### Week 23 (Days 165-171): Compliance Frameworks
- **Day 165:** ISO 27001 — the ISMS
- **Day 166:** SOC 2 (Type I & II) — Trust Service Criteria
- **Day 167:** PCI-DSS for payment systems
- **Day 168:** HIPAA for healthcare
- **Day 169:** GDPR & data privacy laws (CCPA, PDPA, DPDP)
- **Day 170:** Compliance-as-code: AWS Config, Cloud Custodian
- **Day 171:** 🔄 Week recap

**🎯 Featured incident:** **British Airways GDPR fine (2020)** — €20M fine for failing to protect customer data. Magecart attack.

**🔬 Week lab:** Map ShopFast's controls to SOC 2 Common Criteria. Identify gaps.

---

### Week 24 (Days 172-180): Cloud Misconfigurations + Q2 Capstone
- **Day 172:** Top cloud misconfigurations (public S3, exposed RDS, overprivileged roles)
- **Day 173:** CSPM tools: Prowler, ScoutSuite, Wiz, AWS Security Hub
- **Day 174:** Cloud detection & response (CDR): GuardDuty, Defender for Cloud
- **Day 175:** Multi-cloud security strategy
- **Day 176-178:** 🏆 **Q2 QUARTERLY CAPSTONE** — Cloud security architecture for ShopFast
- **Day 179:** Publish article + GitHub repo of IaC examples
- **Day 180:** 🎯 **Q2 ASSESSMENT**

**🎯 Featured incident:** **Accenture S3 leak (2017)** — 4 public S3 buckets exposed credentials, decryption keys, client data. Classic misconfiguration.

**🏆 Q2 Quarterly Capstone:** A complete cloud security reference architecture (Terraform/CDK) for a multi-tier SaaS app, with compliance mapping.

---

# 🌳 QUARTER 3: ARCHITECTURE & DEFENSE (Days 181-270)

> *"The best defense is a well-architected offense awareness."*

Goal by Day 270: Design defense-in-depth systems. Lead incident response. Understand the attacker's mindset.

---

## Month 7: Secure Architecture Patterns (Days 181-210)

### Week 25 (Days 181-187): Architectural Security Patterns
- **Day 181:** Defense-in-depth in modern architectures
- **Day 182:** Zero-trust architecture (NIST SP 800-207) deep dive
- **Day 183:** BeyondCorp model — Google's zero-trust journey
- **Day 184:** Secure-by-design vs. secure-by-default
- **Day 185:** Privacy-by-design (Cavoukian's 7 principles)
- **Day 186:** Resilience patterns: bulkheads, circuit breakers, graceful degradation
- **Day 187:** 🔄 Week recap

**🎯 Featured incident:** **Marriott (2018)** — 500M records, undetected for 4 years. M&A integration without security review.

**🔬 Week lab:** Redesign ShopFast as zero-trust. Document every trust assumption removed.

---

### Week 26 (Days 188-194): Microservices Security
- **Day 188:** The expanded attack surface of microservices
- **Day 189:** Service-to-service auth: mTLS, JWT, SPIFFE
- **Day 190:** Service mesh security deep dive (Istio policies)
- **Day 191:** API gateway security patterns
- **Day 192:** Event-driven architecture security (Kafka, queues)
- **Day 193:** Sidecar pattern for security concerns
- **Day 194:** 🔄 Week recap

**🎯 Featured incident:** **Drizly (2020)** — Microservice with exposed admin panel led to 2.5M user breach.

**🔬 Week lab:** Build a 3-service microservice app with mTLS, OPA-based authorization, and centralized auth via Keycloak.

---

### Week 27 (Days 195-201): Data Security Architecture
- **Day 195:** Data classification & labeling
- **Day 196:** Data lineage & provenance
- **Day 197:** Tokenization vs. encryption vs. masking
- **Day 198:** Database security architecture (RLS, dynamic data masking)
- **Day 199:** Data lake / warehouse security
- **Day 200:** Privacy engineering: differential privacy, k-anonymity
- **Day 201:** 🔄 Week recap

**🎯 Featured incident:** **Pegasus Airlines (2022)** — 6.5TB exposed via misconfigured AWS S3. Sensitive flight & crew data.

**🔬 Week lab:** Implement Row-Level Security (RLS) in PostgreSQL. Add column-level encryption for PII. Test with multiple user roles.

---

### Week 28 (Days 202-210): Threat Modeling at Scale + Capstone
- **Day 202:** Threat modeling at enterprise scale (PASTA, LINDDUN for privacy)
- **Day 203:** Attack trees & kill chains (Cyber Kill Chain, MITRE ATT&CK)
- **Day 204:** Adversary emulation — thinking like APTs
- **Day 205:** Architectural Decision Records (ADRs) for security
- **Day 206:** Security review process — gates & guardrails
- **Day 207-209:** 🏆 **MONTHLY CAPSTONE** — Enterprise threat model
- **Day 210:** Publish

**🎯 Featured incident:** **SolarWinds Sunburst (2020)** — Supply-chain attack against build pipeline. 18K orgs compromised.

**🏆 Monthly Capstone:** A 30+ page enterprise threat model document for ShopFast, including LINDDUN privacy analysis and MITRE ATT&CK mapping.

---

## Month 8: Detection, Response & Forensics (Days 211-240)

### Week 29 (Days 211-217): SIEM & Detection Engineering
- **Day 211:** SIEM fundamentals — Splunk, Elastic, Sentinel, Chronicle
- **Day 212:** Log sources: what to log, what not to log
- **Day 213:** Detection engineering as code — Sigma rules
- **Day 214:** MITRE ATT&CK as a detection framework
- **Day 215:** SOAR — Security Orchestration, Automation, and Response
- **Day 216:** XDR (Extended Detection & Response) emergence
- **Day 217:** 🔄 Week recap

**🎯 Featured incident:** **Sony Pictures (2014)** — Detection failed for months. 100TB exfiltrated.

**🔬 Week lab:** Set up Elastic SIEM. Ingest logs from a sample app. Write 5 Sigma detection rules. Test by triggering them.

---

### Week 30 (Days 218-224): Incident Response Mastery
- **Day 218:** IR frameworks: NIST SP 800-61, SANS PICERL
- **Day 219:** Preparation — playbooks, tabletops, runbooks
- **Day 220:** Detection & analysis — triage at speed
- **Day 221:** Containment strategies — short-term & long-term
- **Day 222:** Eradication & recovery
- **Day 223:** Lessons learned & post-mortems
- **Day 224:** 🔄 Week recap

**🎯 Featured incident:** **Maersk NotPetya (2017)** — $300M loss. Heroic recovery saved by one offline domain controller in Ghana.

**🔬 Week lab:** Write an IR playbook for "credential stuffing attack on ShopFast." Run a tabletop with imaginary stakeholders.

---

### Week 31 (Days 225-231): Digital Forensics
- **Day 225:** Forensic process: identification, preservation, analysis, presentation
- **Day 226:** Chain of custody — legal & technical
- **Day 227:** Disk forensics: imaging, file carving (Autopsy, FTK)
- **Day 228:** Memory forensics (Volatility)
- **Day 229:** Network forensics (Wireshark, Zeek)
- **Day 230:** Cloud forensics — challenges & approaches
- **Day 231:** 🔄 Week recap

**🎯 Featured incident:** **TheShadowBrokers / Equation Group leak** — How forensic analysis attributed the NSA tools.

**🔬 Week lab:** Acquire a memory image. Use Volatility to find malicious processes. Document findings forensically.

---

### Week 32 (Days 232-240): Threat Hunting + Capstone
- **Day 232:** Threat hunting — proactive vs. reactive
- **Day 233:** Hypothesis-driven hunting with MITRE ATT&CK
- **Day 234:** Behavioral analytics & UEBA
- **Day 235:** Threat intelligence: strategic, operational, tactical
- **Day 236:** Indicators of Compromise (IoCs) vs. TTPs
- **Day 237-239:** 🏆 **MONTHLY CAPSTONE** — Detection & response playbook
- **Day 240:** Publish

**🎯 Featured incident:** **APT41 (Chinese state actor)** — Years of intrusion only found via proactive hunting.

**🏆 Monthly Capstone:** Build a complete detection & response playbook for ShopFast, including 20+ Sigma rules and 5 IR runbooks.

---

## Month 9: Offensive Security & Red Teaming (Days 241-270)

### Week 33 (Days 241-247): Attacker Mindset
- **Day 241:** Why defenders need to think like attackers
- **Day 242:** Reconnaissance: OSINT, recon-ng, theHarvester
- **Day 243:** Web app pentesting basics (Burp Suite)
- **Day 244:** Network pentesting basics (nmap, Metasploit)
- **Day 245:** Common exploits to understand (EternalBlue, BlueKeep, PrintNightmare)
- **Day 246:** Social engineering & phishing — the human attack surface
- **Day 247:** 🔄 Week recap

**🎯 Featured incident:** **DEF CON Voting Village** — Researchers find systemic flaws yearly. Why offensive research matters.

**🔬 Week lab:** Pentest a deliberately vulnerable VM (HackTheBox / TryHackMe machine). Document findings as a defender would receive them.

---

### Week 34 (Days 248-254): Red Team vs. Blue Team
- **Day 248:** Red team operations — beyond pentesting
- **Day 249:** Blue team — the defenders' toolkit
- **Day 250:** Purple team — collaboration model
- **Day 251:** Adversary emulation (Caldera, Atomic Red Team)
- **Day 252:** Tabletop exercises & breach simulations
- **Day 253:** Bug bounty programs — crowd-sourced security
- **Day 254:** 🔄 Week recap

**🎯 Featured incident:** **Coinbase bug bounty saves $250K** — Ethical hacker found critical flaw, responsibly disclosed.

**🔬 Week lab:** Run an Atomic Red Team test. Detect it with your Elastic SIEM rules. Improve detection where it failed.

---

### Week 35 (Days 255-261): AI/ML Security (FUTURIST CORE)
- **Day 255:** AI security threat landscape (MITRE ATLAS)
- **Day 256:** Prompt injection — direct & indirect
- **Day 257:** Training data poisoning
- **Day 258:** Model extraction & membership inference
- **Day 259:** Adversarial examples in computer vision & NLP
- **Day 260:** Securing AI agents — the agentic AI threat model
- **Day 261:** 🔄 Week recap

**🎯 Featured incident:** **Microsoft Tay (2016) → Bing Chat jailbreaks (2023)** — From data poisoning to prompt injection.

**🔬 Week lab:** Build a vulnerable chatbot. Exploit it with prompt injection. Implement defenses (Lakera, NeMo Guardrails).

**🚀 Futurist note:** AI red teaming is becoming a distinct discipline. OWASP LLM Top 10 is now reference. Agentic AI risks dominate 2026+.

---

### Week 36 (Days 262-270): Q3 Quarterly Capstone
- **Day 262-263:** Plan your "Breach Diary" project
- **Day 264-268:** Build it — detect, respond, learn
- **Day 269:** 🏆 **Q3 QUARTERLY CAPSTONE** — "Breach Diary" portfolio piece
- **Day 270:** 🎯 **Q3 ASSESSMENT** + publish to LinkedIn, dev.to, personal blog

**🎯 Featured incident:** Your choice — analyze any major 2023-2025 breach in depth.

**🏆 Q3 Quarterly Capstone:** "Breach Diary" — A long-form deep-dive analyzing a recent major breach, with: timeline, attack chain (MITRE ATT&CK mapped), root cause analysis, what should have prevented it (architectural), detection that would have caught it, and IR response evaluation. Aim for 3000+ words.

---

# 🌲 QUARTER 4: SPECIALIZATION & MASTERY (Days 271-365)

> *"Specialists earn the title 'architect.'"*

Goal by Day 365: Deep expertise in chosen specialization. Job-ready portfolio. Public reputation. Year-2 plan.

---

## Month 10: Choose Your Specialization (Days 271-300)

### Week 37 (Days 271-277): The Big Decision
- **Day 271:** Mapping the security career landscape
- **Day 272-275:** Deep dives into each specialization path:
  - **Path A:** Microservices/Container Security Architect
  - **Path B:** Cloud Security (AWS/Azure/GCP) Architect
  - **Path C:** Incident Response & Threat Hunting Lead
  - **Path D:** Compliance/GRC & Security Strategy
  - **Path E:** AI/ML Security Specialist (emerging, high demand)
- **Day 276:** Skills audit — where am I strongest?
- **Day 277:** 🎯 **DECISION DAY** — Pick your path

**🔬 Week lab:** Take a self-assessment for each path. Score yourself on the skills required. Choose based on data + passion.

---

### Week 38-40 (Days 278-300): Specialization Deep Dive

Curriculum diverges based on your chosen path. The routine will generate the specific 23-day plan when you make the decision on Day 277.

**General structure for any path:**
- Days 278-284: Foundations of the specialization (tools, frameworks)
- Days 285-291: Hands-on projects
- Days 292-298: Advanced topics & certifications path
- Days 299-300: 🏆 **MONTHLY CAPSTONE** — Specialization-specific deliverable

**🎯 Featured incidents:** Curated specifically for your specialization.

---

## Month 11: Portfolio Building (Days 301-330)

### Week 41-44 (Days 301-330): Public Reputation Sprint

**The goal:** Build a portfolio that gets you hired. Three artifacts:

#### Artifact 1: Technical Blog Post Series (Days 301-310)
- 3-part series on your specialization
- Each post 1500-2500 words
- Real code, real diagrams, real incidents
- Published to dev.to, Medium, or personal blog
- Cross-posted to LinkedIn

#### Artifact 2: Open Source Contribution (Days 311-320)
- Build a security tool / library / detection ruleset
- Examples by path:
  - **A:** RBAC middleware for .NET, IAM policy validator
  - **B:** Terraform modules for hardened cloud baseline
  - **C:** Sigma rule library for common attack patterns
  - **D:** Compliance-as-code library (e.g., AWS Config rules)
  - **E:** Prompt injection detector / AI red team toolkit
- Documented, tested, MIT-licensed
- Promoted on Twitter/X, LinkedIn, security forums

#### Artifact 3: Conference Talk Proposal (Days 321-330)
- 🏆 **MONTHLY CAPSTONE** — Submit a CFP to a security conference
- Suggested venues: BSides (anywhere), OWASP local chapter, DevSecCon, SecTor
- Even if not accepted, you have a polished talk + slides

**🎯 Featured incident:** None this month — you're building, not just learning.

---

## Month 12: Launch & Year 2 (Days 331-365)

### Week 45 (Days 331-337): Certification Push
- Pick ONE cert aligned with your path:
  - **Foundational:** CompTIA Security+, ISC2 CC (Certified in Cybersecurity — free!)
  - **Architecture:** CCSP (Cloud), CISSP (broad, requires exp)
  - **Cloud:** AWS Security Specialty, Azure Security Engineer, GCP Pro Cloud Sec
  - **K8s/Cloud-Native:** CKS (Certified Kubernetes Security Specialist)
  - **IR:** GIAC GCIH, GCIA
  - **Offensive:** OSCP (if going purple/red team)
  - **AI Security:** No major certs yet — get ahead by publishing
- 🎯 Schedule the exam by Day 337

**🔬 Lab:** Take 2-3 practice tests. Identify weak areas. Drill them.

---

### Week 46 (Days 338-344): Networking & Community
- **Day 338:** Update LinkedIn — full transformation
- **Day 339:** Update GitHub profile README — showcase CyberForge journey
- **Day 340:** Join 3 security communities (OWASP, local DefCon group, Discord)
- **Day 341:** Reach out to 5 security professionals for informational interviews
- **Day 342:** Speak at one local meetup (even a 5-min lightning talk)
- **Day 343:** Document your journey publicly — recap thread on X/LinkedIn
- **Day 344:** 🔄 Week recap

---

### Week 47 (Days 345-351): Job Search Preparation
- **Day 345:** Resume rewrite — security-architect framing
- **Day 346:** Target company list (50 companies)
- **Day 347:** Interview prep — common security architect questions
- **Day 348:** System design for security interviews
- **Day 349:** Behavioral prep — STAR stories from CyberForge year
- **Day 350:** Take-home assignment practice (threat modeling, design)
- **Day 351:** 🔄 Week recap

---

### Week 48-52 (Days 352-365): The Final Sprint

#### Days 352-360: 🏆 YEAR-END CAPSTONE
A flagship portfolio piece that demonstrates everything you've learned:
- **Option A:** A complete security architecture for a fictional fintech startup (60+ pages, multiple diagrams, threat model, IR plan, compliance mapping)
- **Option B:** A deep-dive analysis of a 2026 major breach with full architectural alternative designs
- **Option C:** An open-source security framework/tool with documentation, tests, and adoption strategy

Publish everywhere. This is your headline.

#### Days 361-364: 🔮 YEAR 2 PLANNING
- Day 361: Reflection — what surprised me? What energized me?
- Day 362: Market analysis — where is security going? Where will you go?
- Day 363: Year 2 specialization roadmap (Claude generates this with you)
- Day 364: Set Year 2 goals — specific, measurable, ambitious

#### Day 365: 🎉 GRADUATION
- Full retrospective post (LinkedIn, blog, dev.to)
- Update README of this repo with results
- Plant the flag — you are now a cybersecurity architect
- Take 2 days off before Year 2 starts

---

# 📚 Real-World Incidents Reference Library

Throughout the year, we'll study 50+ real incidents. Major ones include:

| Year | Incident | Lesson |
|------|----------|--------|
| 2011 | DigiNotar | PKI/CA trust failure |
| 2012 | LinkedIn | Bad password hashing |
| 2013 | Target | Vendor pivoting, no segmentation |
| 2013-14 | Yahoo | 3B accounts, governance failure |
| 2015 | OPM | SQL injection at gov scale |
| 2016 | Mirai/Dyn | IoT DDoS |
| 2016 | Uber GitHub leak | Secrets in code |
| 2017 | Equifax | Unpatched component |
| 2017 | NotPetya | Supply chain weaponization |
| 2018 | Marriott | Undetected for 4 years |
| 2018 | GitHub DDoS (1.35Tbps) | Memcached amplification |
| 2019 | Capital One | Cloud IAM + SSRF |
| 2020 | Twitter Bitcoin scam | MFA + social eng |
| 2020 | SolarWinds | Build pipeline supply chain |
| 2021 | Colonial Pipeline | VPN + no MFA |
| 2021 | Log4Shell | Ubiquitous library RCE |
| 2022 | Okta | Sub-processor compromise |
| 2022 | Optus | Unauthenticated API |
| 2023 | Microsoft Storm-0558 | Stolen signing key |
| 2024+ | (Curated as they happen) | Latest threats |

---

# 🛠️ Tools You'll Master This Year

**Threat Modeling:** OWASP Threat Dragon, Microsoft TMT, IriusRisk
**Crypto:** OpenSSL, age, Vault, AWS KMS
**AppSec:** Burp Suite, OWASP ZAP, Semgrep, SonarQube, Trivy, Snyk
**Cloud:** Prowler, ScoutSuite, Wiz/CSPM, AWS Security Hub, Cloud Custodian
**K8s:** Falco, OPA Gatekeeper, kube-bench, Trivy, Polaris
**SIEM:** Elastic Security, Splunk (community), Wazuh
**IR/Forensics:** Volatility, Autopsy, Wireshark, Zeek, Velociraptor
**Red Team:** nmap, Metasploit, Burp, Caldera, Atomic Red Team
**AI Security:** Lakera Guard, NeMo Guardrails, Garak, PyRIT

---

# 📖 Recommended Books (in order)

1. **Threat Modeling: Designing for Security** — Adam Shostack (Months 1-3)
2. **Security Engineering** — Ross Anderson (reference, all year)
3. **The Web Application Hacker's Handbook** — Stuttard & Pinto (Month 5)
4. **Designing Data-Intensive Applications** — Martin Kleppmann (Month 7, security lens)
5. **Sandworm** — Andy Greenberg (Month 8, narrative IR lessons)
6. **The Cuckoo's Egg** — Cliff Stoll (anytime, classic)
7. **Tribe of Hackers: Red Team / Blue Team** — Marcus Carey (Month 9)
8. **Click Here to Kill Everybody** — Bruce Schneier (Month 11, futurist lens)

---

# 🎓 Online Resources

- **TryHackMe** — beginner-friendly hands-on (Months 1-6)
- **HackTheBox** — intermediate+ (Months 7+)
- **PortSwigger Web Security Academy** — free, world-class (Month 5)
- **SANS Cyber Aces Online** — free foundational courses
- **MITRE ATT&CK Navigator** — reference all year
- **OWASP Cheat Sheet Series** — bookmark all of them
- **Krebs on Security** (blog) — daily reading
- **Dark Reading** (news) — weekly scan
- **Risky Business** (podcast) — weekly episodes

---

**This is a living document.** Update it as the curriculum evolves with my interests and the market.

*Last updated: Day 1 of CyberForge 365*
