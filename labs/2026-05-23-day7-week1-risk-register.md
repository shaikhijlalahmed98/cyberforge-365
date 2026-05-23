# Lab — Day 007: Your First Risk Register (ShopFast, Week 1 Consolidation)

**Date:** 2026-05-23
**Linked lesson:** `lessons/2026-05-23-day7-week1-recap.md`
**Time:** 45–60 min
**Goal:** Week 1 ke saare concepts ko ek hi portfolio-grade artifact mein convert karna — ek **Risk Register**. Yaani (A) ShopFast ke 10 assets identify + CIA-tag karna, (B) un par threats/vulns map karke top risks nikaalna, (C) har top risk ko **CVSS v3.1 basics** se score karna, (D) har risk ke saamne ek **named control + control type** assign karna, aur (E) top-3 ko **treatment decision** (accept/mitigate/transfer/avoid) dena.

> **Why this matters:** Risk register woh *ek* document hai jo har security architect har engagement mein banata, maintain, aur defend karta hai. Yeh teri Week 1 vocabulary (CIA → risk → surface → AAA → depth → controls) ka **practical proof** hai. Yeh file Day 28-30 ke ShopFast threat-model capstone mein seedha reuse hogi — isliye serious banao.

---

## Background — ShopFast (continuity)

ShopFast = ek fictional e-commerce platform (hamara running case study). Architecture jo ab tak establish hua:

```
                         ┌─────────────────────────────┐
Internet ─► [CDN/WAF] ─► [API Gateway] ─► microservices │
                         │   ├─ auth-service            │
                         │   ├─ catalog-service         │
                         │   ├─ order-service           │
                         │   ├─ payment-service ◄── crown jewel
                         │   └─ refund-service  ◄── crown jewel
                         └──────────┬──────────────────┘
                                    │
                      [Postgres: orders, users(PII), payments]
                                    │
                      [audit/SIEM] ─► [on-call]
                      [object storage: invoices, ID scans]
                      [secrets: API keys, DB creds]
```

---

## Part A — Asset Inventory + CIA Tagging (~12 min)

Pehla kaam (Day 1 lens): **kya bachana hai?** 10 assets list kar. Har asset ke liye batao kaunsa CIA pillar sabse zyada matter karta hai (H/M/L har pillar ke liye), kaun owner hai, aur kya yeh "crown jewel" hai.

Fill this table (kuch seed kiye hain, baaki tu bhar):

| # | Asset | Confidentiality | Integrity | Availability | Owner | Crown jewel? |
|---|-------|:---:|:---:|:---:|-------|:---:|
| 1 | User PII (name, email, address) in `users` table | H | H | M | Data team | ✅ |
| 2 | Payment card data flow (`payment-service`) | H | H | H | Payments | ✅ |
| 3 | Refund capability (`POST /api/refunds`) | | | | | |
| 4 | Auth/session tokens (`auth-service`) | | | | | |
| 5 | Order history (`orders` table) | | | | | |
| 6 | Stored secrets (DB creds, API keys) | | | | | |
| 7 | Audit/SIEM logs | | | | | |
| 8 | Object storage (invoices, ID scans) | | | | | |
| 9 | Product catalog + pricing | | | | | |
| 10 | _(your choice — e.g. admin console)_ | | | | | |

**Success criteria:**
- 10 assets, har ek pe C/I/A rating (H/M/L).
- Ek line: kaunsa pillar overall ShopFast ke liye sabse critical hai aur kyun?
- 3+ assets "crown jewel" marked, justification ke saath.

---

## Part B — Threat → Vulnerability → Risk Mapping (~12 min)

Day 2-3 lens: **kaun todega, kis weakness se, kahan se ghus ke?** Apne 10 assets mein se top exposures choose kar aur risk statements likh.

Risk statement format (yeh phrasing seekh — interviews mein kaam aati hai):

> **"[Threat actor]** could exploit **[vulnerability]** via **[attack vector]** to compromise **[CIA pillar]** of **[asset]**, resulting in **[business impact]**."**

Likh **5+ risk statements.** Example (seed):

> R1: "An external attacker could exploit a missing object-level authorization check (IDOR) via the `GET /api/orders/{id}` endpoint to compromise the **Confidentiality** of **other users' order PII**, resulting in mass data exposure and GDPR liability."

| ID | Risk statement (full sentence) | Asset | CIA pillar(s) | Attack vector |
|----|--------------------------------|-------|---------------|---------------|
| R1 | (seed above) | Order PII | C | IDOR on REST endpoint |
| R2 | | | | |
| R3 | | | | |
| R4 | | | | |
| R5 | | | | |

**Success criteria:** 5 risk statements full-sentence format mein; har ek pe asset + CIA pillar + vector clearly tagged.

---

## Part C — Score with CVSS v3.1 Basics (~15 min)

Day 2 mein humne kaha tha risk ko *measure* karna hai. Industry standard tareeqa = **CVSS** (Common Vulnerability Scoring System). Aaj sirf **Base score** ke 8 metrics use karenge — yeh quantitative hone ki pehli practice hai.

**CVSS v3.1 Base metrics (har risk ke liye choose):**

| Metric | Options | Quick meaning |
|--------|---------|---------------|
| **AV** Attack Vector | Network / Adjacent / Local / Physical | Kahan se reach? (N = internet = worst) |
| **AC** Attack Complexity | Low / High | Kitna mushkil exploit? |
| **PR** Privileges Required | None / Low / High | Attacker ko pehle se kya access chahiye? |
| **UI** User Interaction | None / Required | Victim ko click karna padega? |
| **S** Scope | Unchanged / Changed | Kya blast radius doosre component tak jaata hai? (Day 3!) |
| **C** Confidentiality impact | None / Low / High | |
| **I** Integrity impact | None / Low / High | |
| **A** Availability impact | None / Low / High | |

**Tool:** Official calculator use kar — **https://www.first.org/cvss/calculator/3.1** . Metrics select kar, woh Base score (0.0–10.0) + ek **vector string** dega (jaise `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`).

**Severity bands:** 0.0 None · 0.1–3.9 Low · 4.0–6.9 Medium · 7.0–8.9 High · 9.0–10.0 Critical.

Score apne 5 risks:

| ID | CVSS vector string | Base score | Severity | Note (kaunsa metric sabse zyada bumped score?) |
|----|--------------------|:----------:|:--------:|-----------------------------------------------|
| R1 | `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N` | _(calc)_ | | PR:L kyunki logged-in customer hona zaroori |
| R2 | | | | |
| R3 | | | | |
| R4 | | | | |
| R5 | | | | |

**Success criteria:**
- 5 risks scored, **actual vector strings** (calculator se), guesses nahi.
- Ek observation: **Scope:Changed** kaunse risk pe lagta hai? (Hint: Capital One — SSRF ek service mein, lekin IAM creds churaa ke S3 tak — woh Scope:Changed hai. Yeh Day 3 ka "blast radius" CVSS mein dikhta hai.)

---

## Part D — Assign Controls (type-labeled) + Treatment (~15 min)

Day 4-6 lens: har risk ke saamne defense. Har risk ke liye ek **named control** + uska **control type** (Preventive/Detective/Corrective/Compensating) likh. Phir top-3 ko **treatment decision** de.

**Treatment options (Day 16 ka preview, par aaj basics):**
- **Mitigate** — control laga ke risk kam karo (most common)
- **Accept** — risk chhota/cost zyada, formally accept (with sign-off)
- **Transfer** — insurance / third-party pe daal do
- **Avoid** — feature hi hata do / kaam hi mat karo

| ID | CVSS | Control (named) | Control type | Treatment | Residual risk note |
|----|:----:|-----------------|--------------|-----------|--------------------|
| R1 | | Object-level authZ check (owner == caller) + 403 | Preventive | Mitigate | Add audit log (detective) too |
| R2 | | | | | |
| R3 | | | | | |
| R4 | | | | | |
| R5 | | | | | |

**Then — Top-3 prioritized findings.** CVSS score se rank kar (highest first) aur 2 line har ek pe: "kyun yeh #1, aur ek action item":

1. **#1 (highest CVSS):** _____
2. **#2:** _____
3. **#3:** _____

**Success criteria:**
- Har risk pe ek named control + sahi control type.
- Top-3 ranked by CVSS with action items.
- Kam se kam ek risk pe note: "yahan sirf preventive hai, detective add karna chahiye" (Equifax/Target sabaq).

---

## Part E — Trace One Risk Through the Full Week-1 Loop (~6 min, the synthesis)

Apne top risk (R-highest) ko utha aur use **poore Day 1→6 decision-loop** se guzaar — yeh prove karta hai ke Week 1 connected hai, bikhra nahi:

> **R-top:** _____
> 1. **CIA (Day 1):** kaunsa pillar threatened? → _____
> 2. **Risk (Day 2):** threat × vuln × impact, ek line → _____
> 3. **Surface (Day 3):** vector kya, blast radius kahan tak → _____
> 4. **AAA (Day 4):** kaunsa A fail ho raha / chahiye → _____
> 5. **Depth (Day 5):** is risk ke against kitni independent layers? → _____
> 6. **Controls (Day 6):** kaunse types maujood, kaunsa missing → _____

**Success criteria:** Ek risk, chheh steps, har step ek line. Yeh tera Week 1 ka "I can do this cold" proof hai.

---

## Deliverable

Yeh file fill karke `labs/` mein commit ho jayegi (aaj ke push mein scaffold already hai). Output ki shakl: 10 assets + 5 scored risks + controls + top-3 + ek full-loop trace. **Yeh Day 28-30 capstone ka pehla building block hai** — isay theek se bharo.

> Done ho jaye to chat mein "lab done" likh, ya apni risk-register table paste kar — main scores aur control-type choices review karunga.
