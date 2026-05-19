# Lab — Day 002 — Vulnerability & Risk Triage on ShopFast

**Time:** 45-60 minutes
**Deliverable:** This file, filled in with your risk triage table and a 1-paragraph patching recommendation.
**Skills practiced:** Reading CVE entries on NVD, applying CVSS scores, computing business-context risk.

---

## Background — ShopFast Inventory

ShopFast (our fictional e-commerce platform we'll use all year) has the following stack. Pretend you're the new security architect and this is your day-1 inventory.

| Service | Tech | Exposed? | Data It Touches |
|---------|------|----------|-----------------|
| `WebFrontend` | Next.js 13 | Internet-facing | Session cookies, user-visible data |
| `AuthService` | Spring Boot 2.7, Spring Security 5.7 | Internet-facing via API gateway | Credentials, MFA tokens, password hashes |
| `OrderService` | .NET 6, PostgreSQL 13 | Internal only (API gateway proxies) | Order records, addresses |
| `PaymentService` | .NET 6, integrates with Stripe; uses Log4j-wrapper for logging | Internal only | Last-4 of cards, Stripe tokens |
| `RecommendationService` | Python 3.9, uses Apache Kafka 2.8 client | Internal only | Browsing history (no PII) |
| `AdminPortal` | Spring Boot 2.6, behind VPN+MFA, 6 users | Internal-VPN-only | Full DB read access for ops |

---

## Step 1 — Look Up Three Real CVEs on NVD (15 min)

Open https://nvd.nist.gov/vuln/search.

Look up each of these CVEs and capture the data into the table below.

### CVE-2021-44228 (Log4Shell)
- CVSS v3.1 base score: ___
- Attack Vector (AV): ___
- Attack Complexity (AC): ___
- Privileges Required (PR): ___
- Confidentiality / Integrity / Availability impact (C/I/A): ___ / ___ / ___
- Public exploit available? (yes/no): ___
- On CISA KEV list? (check https://www.cisa.gov/known-exploited-vulnerabilities-catalog): ___

### CVE-2022-22965 (Spring4Shell)
- CVSS v3.1 base score: ___
- AV / AC / PR: ___ / ___ / ___
- C / I / A impact: ___ / ___ / ___
- Public exploit available? ___
- On CISA KEV? ___

### CVE-2022-42889 (Apache Commons Text "Text4Shell")
- CVSS v3.1 base score: ___
- AV / AC / PR: ___ / ___ / ___
- C / I / A impact: ___ / ___ / ___
- Public exploit available? ___
- On CISA KEV? ___

---

## Step 2 — Map CVEs to ShopFast Services (10 min)

For each CVE, ask: **which ShopFast services are plausibly affected?** (Based on the tech stack table above. You can make reasonable assumptions about dependencies.)

| CVE | Plausibly Affects | Why |
|-----|--------------------|-----|
| CVE-2021-44228 (Log4Shell) | _e.g., PaymentService (Log4j-wrapper), AuthService (Spring Boot uses Log4j transitively)_ | Logging library is in transitive dependencies |
| CVE-2022-22965 (Spring4Shell) | ___ | ___ |
| CVE-2022-42889 (Text4Shell) | ___ | ___ |

---

## Step 3 — Build the Risk Triage Table (15 min)

For **each (CVE × Service) combination from Step 2**, compute the risk. Use this scoring rubric:

**Likelihood** (1-5):
- 1 = behind VPN+MFA, no public exploit
- 3 = internet-exposed but no public weaponized exploit
- 5 = internet-exposed AND public exploit on CISA KEV

**Impact** (1-5):
- 1 = no sensitive data, no business disruption
- 3 = some sensitive data OR mild business disruption
- 5 = regulated/PII/payment data OR critical business function

**Risk = Likelihood × Impact** (1-25)
- 1-6 = LOW
- 7-12 = MEDIUM
- 13-19 = HIGH
- 20-25 = CRITICAL

| # | CVE | Service | Likelihood (1-5) | Impact (1-5) | Risk Score | Risk Level | Notes |
|---|-----|---------|------------------|--------------|------------|------------|-------|
| 1 | CVE-2021-44228 | PaymentService | | | | | |
| 2 | CVE-2021-44228 | AuthService | | | | | |
| 3 | CVE-2022-22965 | AuthService | | | | | |
| 4 | CVE-2022-22965 | AdminPortal | | | | | |
| 5 | CVE-2022-42889 | _your pick_ | | | | | |
| 6 | _your own_ | _your pick_ | | | | | |

Add more rows if you find more combinations.

---

## Step 4 — Patching Priority Recommendation (10 min)

Sort the table from Step 3 by Risk Score (highest first). Write a 1-paragraph (5-7 sentences) recommendation to your fictional CTO:

> **Recommendation:**
>
> _Your paragraph here. Address: (a) which CVE+service combo gets patched first, (b) why, (c) is there a compensating control we can deploy in the next 2 hours while patching proceeds (e.g., WAF rule, egress block), (d) which combos can wait for the next maintenance window, and why._

---

## Step 5 — Reflection (5 min)

Answer in 2-3 sentences each:

**Q1: Did any (CVE × Service) combination surprise you with a LOWER risk than expected based on CVSS alone? Which, and why?**

_Your answer..._

**Q2: Of the controls you mentioned (WAF, egress block, etc.), which one would you put as standard from Day 1 of any new service launch, even before any specific CVE existed?**

_Your answer..._

**Q3: If you applied this same exercise to your real production projects (System-X2 or otherwise), name the ONE thing about your current dependency hygiene that would worry you most.**

_Your answer..._

---

## Success Criteria

You've completed this lab when:

- [ ] All 3 CVEs looked up on NVD with real CVSS data filled in
- [ ] Mapping table (Step 2) has at least 4 plausible (CVE × Service) entries
- [ ] Risk triage table (Step 3) has at least 5 rows scored end-to-end
- [ ] Sorted recommendation paragraph written
- [ ] Reflection answered honestly (no need to be impressive — honest is the goal)

Commit this file to `main` with the message:
```
Day 002 lab: ShopFast risk triage with Log4Shell/Spring4Shell/Text4Shell
```

---

## Bonus (Optional, 15 min) — Try a Real Scanner

Install **OWASP Dependency-Check** locally (or use the GitHub Action) and run it against any open-source Java/.NET project on your machine. Capture:
- How many CVEs did it find?
- Were any HIGH or CRITICAL?
- Were they direct dependencies or transitive?

Add findings as an appendix below.

```
Tool used: ___
Project scanned: ___
Total CVEs found: ___
Critical/High: ___
Direct vs transitive: ___ / ___
Most surprising finding: ___
```
