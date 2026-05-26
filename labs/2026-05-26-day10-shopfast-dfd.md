# Day 010 Lab — ShopFast Data Flow Diagram (L0 + L1)

**Date:** 2026-05-26
**Time budget:** 45-60 min
**Tool:** Excalidraw (https://excalidraw.com — free, no signup) **or** draw.io (https://app.diagrams.net) **or** OWASP Threat Dragon
**Output:** This file + one PNG/SVG in `labs/assets/`

> Goal: Pehla complete DFD bana — paper pe nahi, *actually* draw kar. Tasveer banane ki act se hi 30%+ threats nikalte hain. Phir us tasveer pe STRIDE-per-element chala ke 5 *naye* threats nikaal jo Day 8 (4 Questions) aur Day 9 (STRIDE per code) labs mein nahi mile the.

---

## Part A — L0 Context Diagram (10 min)

ShopFast ka L0 banao. Rules:

- **Ek bada process circle** labeled `ShopFast (System)` (sab kuch yahan andar abhi)
- **Sab external entities** rectangles mein
- **Har flow ek arrow** with label (kya data flow ho raha hai, direction bhi)
- **Trust boundary** Internet ↔ ShopFast around the big circle (dashed line)

Suggested external entities to include (kam-se-kam 5):
- [ ] Customer (Browser)
- [ ] Payment Gateway (e.g., Stripe)
- [ ] Shipping Partner API
- [ ] Email/SMS Provider (e.g., SendGrid, Twilio)
- [ ] Third-party Analytics (e.g., Google Analytics, Segment) ← yeh BA Magecart waala
- [ ] Admin / Customer-Support staff
- [ ] (Bonus) Inventory Supplier / Warehouse System
- [ ] (Bonus) Fraud-detection SaaS

### Embed your L0 image here

```
[ paste exported PNG/SVG path: ./assets/2026-05-26-day10-shopfast-l0.png ]
```

![ShopFast L0](./assets/2026-05-26-day10-shopfast-l0.png)

### Quick reflection (3-5 lines)
- Kitne external entities miled? ___
- Kaunsa entity tune **pehle background mein assume** kar liya tha aur draw karte waqt yaad aaya? ___
- Sabse zyada arrows kis entity ki taraf hain? (= probably your highest blast radius)

---

## Part B — L1 Decomposition (15 min)

Ab L0 ka bada circle todo. ShopFast ke andar 5-7 sub-processes draw karo (kam se kam):

- [ ] API Gateway (rate limit + TLS termination)
- [ ] Auth Service
- [ ] Catalog Service
- [ ] Cart / Checkout Service
- [ ] Payment Orchestration Service
- [ ] Orders Service
- [ ] Notification Service
- [ ] Admin Portal Backend
- [ ] (Bonus) Search Service / Recommendation Service

Data stores (kam se kam 4):
- [ ] Users DB
- [ ] Catalog DB
- [ ] Orders DB
- [ ] Cart Cache (Redis)
- [ ] Audit Log Store (append-only)
- [ ] (Bonus) Event Bus (Kafka topic)

Trust boundaries to draw (dashed lines):
- [ ] Internet ↔ DMZ (before API Gateway)
- [ ] DMZ ↔ Internal LAN (after API Gateway, before microservices)
- [ ] Internal ↔ External Payment (Checkout/Payment Service ↔ Stripe)
- [ ] Public Web ↔ Admin Plane (Admin Portal isolation)
- [ ] (Bonus) PII data zone vs analytics/derived data zone

### Embed your L1 image here

![ShopFast L1](./assets/2026-05-26-day10-shopfast-l1.png)

### Per-arrow data label check
List 5 most important arrows with their data labels (e.g., `Customer → API GW : { JWT, cartId, items[] }`):

1. _____________________
2. _____________________
3. _____________________
4. _____________________
5. _____________________

---

## Part C — Trust-Boundary Crossings → STRIDE (15 min)

Identify **every arrow that crosses a dashed line.** List them in this table. Yahi 80% threats hain.

| # | From → To | Crosses which boundary | Likely STRIDE letters | Worst case if exploited |
|---|-----------|------------------------|------------------------|--------------------------|
| 1 | Customer → API Gateway | Internet ↔ DMZ | S, T, I, D | _____ |
| 2 | API Gateway → Checkout Service | DMZ ↔ Internal | S, T | _____ |
| 3 | Checkout → Payment Gateway | Internal ↔ External | S, T, I | _____ |
| 4 | Browser → 3rd-party Analytics | Code-origin ↔ external (BA lesson!) | T, I | _____ |
| 5 | Admin → Admin Portal | Public Web ↔ Admin Plane | S, E | _____ |
| 6 | _____ → _____ | _____ | _____ | _____ |
| 7 | _____ → _____ | _____ | _____ | _____ |

---

## Part D — 5 NEW Threats (Day 8/9 labs mein NAHI mile the) (10 min)

Diagram dekh ke (sirf brainstorm nahi — *visual cue se* nikal), 5 threats jo pehle nahi mile. Format:

**Threat #1**
- **Location on DFD:** _____ (e.g., "Browser ↔ Analytics arrow")
- **STRIDE letter:** _____
- **Description (1-2 lines):** _____
- **Why missed earlier:** _____ (assumption mein chhoot gaya? naya component? new boundary?)
- **Architectural mitigation:** _____
- **Control type (Day 6):** _____ (preventive/detective/corrective/compensating)

**Threat #2** _(same format)_

**Threat #3** _(same format)_

**Threat #4** _(same format)_

**Threat #5** _(same format)_

> Tip: BA Magecart-style threat dhundh — kya tera koi flow browser ke andar third-party JS ke through jaata hai? Agar haan — woh threat list mein likh.

---

## Part E — Apne Project (System-X2) L0 (10 min) — Bonus

Ek 5-min sketch banao apne *real* current project ka L0. Sirf paper pe theek hai — photo nahi chahiye. Phir likh:

- Kitne external entities? ___
- Kitne assumed-but-undrawn flows pehli baar mein yaad aaye? ___
- Kaunsa ek "undrawn flow" sabse risky lag raha? (yeh aaj ka **journal answer** ban gaya)
  - _____

---

## ✅ Done Criteria

- [ ] L0 diagram drawn + exported + embedded
- [ ] L1 diagram drawn + exported + embedded
- [ ] At least 5 boundary-crossing arrows tabled with STRIDE
- [ ] 5 NEW threats listed in Day 8/9 ke comparison mein
- [ ] System-X2 L0 reflection done
- [ ] (Bonus) DFD ko OWASP Threat Dragon mein bhi import kiya — auto-threats compare karo

---

## Notes / Gotchas captured during the lab

- _____
- _____
- _____
