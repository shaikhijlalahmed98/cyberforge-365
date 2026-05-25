# Lab — Day 009: STRIDE-per-Element on ShopFast Checkout

**Date:** 2026-05-25
**Linked lesson:** `lessons/2026-05-25-day9-stride.md`
**Time:** 30–60 min
**Tools:** Tera dimaag + yeh markdown + (optional) Microsoft Threat Modeling Tool ya draw.io for the diagram.

> **Goal:** Kal (Day 8) tune ShopFast checkout pe *instinct* se "what can go wrong?" socha. Aaj wahi flow **STRIDE checklist** se systematically guzaarega — har element pe applicable letters chala ke. Maqsad: dekhna ke checklist kitne **naye** threats surface karta hai jo instinct miss kar gaya (khaas Repudiation aur DoS). Output ek STRIDE threat table jo Day 30 capstone (ShopFast Threat Model) ka building block banega.

---

## 🎯 Success Criteria (yeh hone par lab pass)

- [ ] **Part A:** Checkout flow ke elements 4 types mein tag kiye (external entity / process / data store / data flow)
- [ ] **Part B:** STRIDE-per-element table bhari — har element pe sirf **applicable** letters
- [ ] **Part C:** Kam se kam **8 threats** likhe, aur har 6 STRIDE letters mein se **kam se kam 1** represented ho
- [ ] **Part D:** Har threat ko canonical mitigation + Day 6 control-type (P/D/C) label
- [ ] **Part E:** Day 8 list se compare — kitne **naye** threats STRIDE ne diye? (khaas R aur D)
- [ ] **Bonus:** System-X2 ke ek service-to-service call pe STRIDE chala

---

## The ShopFast Checkout Flow (recall from Day 8)

```
[Browser/Mobile App] ──(external entity)
      │  DF1: POST /api/cart/checkout { cartId, paymentToken, shippingAddr }
      ▼
[Checkout Service] ──(process)
      │   ├─ DF2 ─► [Catalog Service] (process, external-ish)  — item prices
      │   ├─ DF3 ─► [Promotions Service] (process)             — coupon
      │   ├─ DF4 ─► [Payment Gateway] (EXTERNAL entity, 3rd party) — charge
      │   ├─ DF5 ─► [Orders DB] (data store)                   — create order
      │   └─ DF6 ─► [Email/Notification Service] (process)     — confirmation
```

---

## Part A — Elements ko 4 types mein tag kar (5 min)

STRIDE-per-element chalane ke liye pehle har cheez ko ek type do (Day 10 DFD preview).

| Element | Type (External entity / Process / Data store / Data flow) |
|---------|-----------------------------------------------------------|
| Browser/Mobile App | _(e.g. External entity)_ |
| Checkout Service | _..._ |
| Catalog Service | _..._ |
| Payment Gateway (3rd party) | _..._ |
| Orders DB | _..._ |
| DF1: browser → Checkout | _(Data flow)_ |
| DF4: Checkout → Payment Gateway | _..._ |
| DF5: Checkout → Orders DB | _..._ |

---

## Part B — STRIDE-per-element applicability (5 min)

Lesson §2.4 table use kar. Har element pe ✅ lagao jahan woh letter applicable hai (external entity = S,R · process = sab · data store = T,R*,I,D · data flow = T,I,D).

| Element | S | T | R | I | D | E |
|---------|---|---|---|---|---|---|
| Browser (ext entity) | | | | | | |
| Checkout Service (process) | | | | | | |
| Payment Gateway (ext entity) | | | | | | |
| Orders DB (data store) | | | | | | |
| DF1 browser→Checkout (flow) | | | | | | |
| DF4 Checkout→Gateway (flow) | | | | | | |

---

## Part C — Threats likho (Part B ke ✅ cells ko threats mein convert kar) — 15 min (dil)

Har applicable letter pe ek **specific** threat likh ("Tampering possible" ❌; "client cartTotal tamper karke ₹1 order" ✅). Kam se kam **8** rows, aur **har 6 letters** se kam se kam 1.

| # | Element | STRIDE letter | Specific threat ("what can go wrong") | Impact (CIA/AAA) |
|---|---------|---------------|----------------------------------------|------------------|
| 1 | Checkout Service | **T** | _(e.g. client cartTotal tamper → ₹1 checkout)_ | Integrity |
| 2 | DF4 Checkout→Gateway | **S** | _(e.g. fake "payment success" webhook bina signature)_ | AuthN |
| 3 | Checkout Service | **R** | _(e.g. refund/order ka koi audit log nahi — user mukar jaye)_ | Non-repudiation |
| 4 | Orders DB / response | **I** | _(e.g. order response dusre user ka PII leak — IDOR)_ | Confidentiality |
| 5 | Checkout Service | **D** | _(e.g. million-item cart / ReDoS coupon → service down)_ | Availability |
| 6 | Admin path | **E** | _(e.g. normal user admin refund-approve call kare)_ | AuthZ |
| 7 | _..._ | | | |
| 8 | _..._ | | | |

---

## Part D — Mitigations + control type (10 min)

Lesson §2.5 ke canonical mitigation table se uthao, phir Day 6 ka control-type label (Preventive/Detective/Corrective) lagao.

| Threat # | Canonical mitigation | Concrete control (kahan/kaise) | Type (P/D/C) |
|----------|----------------------|--------------------------------|--------------|
| 1 | Integrity (server recompute) | _(server-side total from Catalog; ignore client)_ | Preventive |
| 2 | Authenticate | _(HMAC-signed webhook / mTLS)_ | Preventive |
| 3 | Log + sign | _(append-only audit: actor/action/at/ip)_ | Detective |
| 4 | Encrypt + authorize | _(DTO + ownership check)_ | Preventive |
| 5 | Limit + isolate | _(rate limit + max cart size + timeout)_ | Preventive |
| 6 | Authorize | _([Authorize(Roles)] + deny-by-default)_ | Preventive |
| 7 | | | |
| 8 | | | |

---

## Part E — Compare with Day 8 (5 min — yeh aaj ka "aha")

Kal ke `labs/2026-05-24-day8-four-questions-shopfast.md` ki Part B list khol.

- **Day 8 mein kitne threats aaye (instinct):** ___
- **Aaj STRIDE se kitne aaye:** ___
- **Kitne bilkul NAYE the (Day 8 mein nahi the):** ___
- **Kaunse letters Day 8 mein TOTALLY miss the?** _(mera guess: R aur D)_ → ___
- **Ek line — checklist ne kya diya jo instinct nahi de paaya:**
  > _..._

---

## Bonus — System-X2 service-to-service call pe STRIDE (10 min)

Apne real system ka ek microservice → microservice call le (ek data flow).

- **Call kya hai (1 line):** _(e.g. OrderSvc → InventorySvc, reserve stock)_
- **S — Caller authenticate hota hai?** _(mTLS? token? ya "andar ka network safe hai" assume?)_ → _____
- **T — Payload tamper-proof / TLS?** → _____
- **I — Encrypted in transit? Over-fetching?** → _____
- **D — Rate limit / timeout / circuit breaker?** _(yeh tere paas resilience se already ho sakta — Day 6 connection)_ → _____
- **E — Callee blindly trust karta hai caller ke role/claims?** → _____
- **Sabse weak letter (undefended):** _____ — **yeh aaj ka sabse valuable finding hai.**

---

## ✅ Definition of Done

- [ ] Yeh file bhari hui (Parts A–E)
- [ ] ≥8 threats, har 6 STRIDE letters represented
- [ ] Har threat ko mitigation + control-type label
- [ ] Day 8 se comparison done (naye threats counted)
- [ ] (Bonus) System-X2 service call pe STRIDE
- [ ] Commit + push to **main**

> **Yeh file portfolio evidence hai.** Day 10 (DFD) mein yeh hi elements proper diagram banenge, aur Day 30 capstone (ShopFast Threat Model) mein yeh STRIDE table directly reuse hogi. Aaj ka systematic pass future ka foundation hai.

---

#lab #stride #threat-modeling #stride-per-element #shopfast #checkout #spoofing #tampering #repudiation #information-disclosure #denial-of-service #elevation-of-privilege #day9
