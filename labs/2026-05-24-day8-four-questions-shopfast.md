# Lab — Day 008: Your First Threat Model (Shostack's 4 Questions on ShopFast Checkout)

**Date:** 2026-05-24
**Linked lesson:** `lessons/2026-05-24-day8-why-threat-modeling-shift-left.md`
**Time:** 30–60 min
**Tools:** Sirf tera dimaag + yeh markdown file. (No STRIDE/DFD yet — woh Day 9–10.)

> **Goal:** Pehli baar ek system ko *attacker ki nazar* se dekhna, structured tareeqe se. Aaj hum "what can go wrong?" wali muscle build kar rahe hain, aur **design flaw ko implementation bug se distinguish** karna seekh rahe hain. Output ek 1-page artifact jo portfolio evidence ban jaye.

---

## 🎯 Success Criteria (yeh hone par lab pass)

- [ ] **Part A:** ShopFast checkout flow describe kiya (Q1 — "what are we working on?")
- [ ] **Part B:** Kam se kam **5** "what can go wrong?" threats (Q2) — adversarial, happy-path nahi
- [ ] **Part C:** Har threat ke saamne ek **control** (Q3) — Week 1 ke control types se (preventive/detective/corrective)
- [ ] **Part D:** Kam se kam **1 design flaw** vs **1 implementation bug** clearly labeled (yeh aaj ka core skill)
- [ ] **Part E:** "Did we do a good job?" (Q4) — top-2 threats prioritize + 1 line: kab is model ko revisit karoge
- [ ] **Bonus:** Apne real project (System-X2) ke ek feature pe yahi 4-Q repeat

---

## The ShopFast Checkout Flow (context)

ShopFast hamara fictional e-commerce platform hai (continuity — har lab ka system). Aaj ka scope: **checkout**. Simplified flow:

```
[Browser/Mobile App]
      │  1. POST /api/cart/checkout  { cartId, paymentToken, shippingAddr }
      ▼
[Checkout Service] ──2. price/total calculate──► [Catalog Service] (item prices)
      │
      ├─3. apply coupon──► [Promotions Service]
      ├─4. charge────────► [Payment Gateway]  (external, 3rd party)
      ├─5. create order──► [Orders DB]
      └─6. send confirmation──► [Email/Notification Service]
```

(Yeh tera mental "Q1 — what are we working on?" hai. Part A mein isse apne shabdon mein refine kar.)

---

## Part A — Q1: "What are we working on?" (5 min)

Apne shabdon mein checkout flow likh. **Khaas dhyan:** kahan **data store** hoti hai, aur kahan **trust badalta hai** (e.g. browser → server, server → external payment gateway). Yeh poora threat model ka foundation hai — jo nahi samjha, uske threats nahi soch sakte.

**Components & data flow (your words):**

_..._

**Sabse sensitive data points (CIA crown jewels — Day 1 recall):**
1. _(e.g. paymentToken)_
2. _..._
3. _..._

**Trust boundaries I can spot (jahan "kam-trusted" se "zyada-trusted" mein data jaata hai):**
- _(e.g. browser → checkout service)_
- _..._

---

## Part B — Q2: "What can go wrong?" (15 min — yeh dil hai)

Ab attacker ban. **Kam se kam 5** threats. Happy-path mat soch — soch "agar main is system ko abuse karna chahun?" Har ek ke liye: kya hoga, kaun karega, kis component pe.

> Trigger questions agar atak jaye (Day 1-7 recall): kya koi **doosre ka** order chhed sakta hai? Kya koi **price/total tamper** kar sakta hai? Kya koi action karke baad mein **deny** kar sakta hai? Kya koi **flood** kar sakta hai? Kya koi **data leak** kar sakta hai? Kya koi **privilege** badha sakta hai?

| # | Threat ("what can go wrong") | Kis component pe | Impact (CIA pillar) |
|---|------------------------------|------------------|---------------------|
| 1 | _(e.g. client cart total tamper karke ₹1 mein checkout)_ | _Checkout Service_ | _Integrity_ |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 (stretch) | | | |

---

## Part C — Q3: "What are we going to do about it?" (10 min)

Har threat ke saamne ek **control**. Week 1 ka control-type label bhi daal (preventive / detective / corrective). Yaad rakh — yeh wahi reasoning hai jo Day 7 ke risk register ko feed karta hai.

| Threat # | Control | Type (P/D/C) | Where it lives (which layer/service) |
|----------|---------|--------------|--------------------------------------|
| 1 | _(e.g. server-side price recompute from Catalog; ignore client total)_ | _Preventive_ | _Checkout Service_ |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

---

## Part D — Design Flaw vs Implementation Bug (10 min — aaj ka core skill)

Apni Part B list mein se classify kar. Yeh distinction architect banata hai.

**Design flaw(s)** — soch hi galat (patch se nahi, redesign se theek). Kam se kam 1:
1. _(e.g. "client total bhejta hai aur server trust karta hai" — yeh design assumption galat hai; redesign = server authority)_

**Why yeh design flaw hai (not impl bug):** _(1 line — "code waisa hi chalta jaisa likha; galti soch mein hai")_

**Implementation bug(s)** — design theek, code mein galti (patchable). Kam se kam 1:
1. _(e.g. "coupon validation regex galat hai" — design theek, fix = ek line)_

**The big realization (1 line):** _Kaunsa threat modeling se pakda jaata aur scanner/pentest se kyun miss hota?_

---

## Part E — Q4: "Did we do a good job?" (5 min)

**Top-2 threats ko prioritize kar** (likelihood × impact — gut feel abhi, formal DREAD Day 12):
1. _(highest priority threat + 1 line why)_
2. _..._

**Yeh threat model kab REVISIT karunga?** (trigger ya date)
> _(e.g. "jab naya payment provider add ho" / "jab guest-checkout feature aaye")_

**Cost reflection (shift-left ka point):** Agar yeh top threat design mein pakda (aaj, jaise abhi) vs production breach mein — 1x vs approx ___x? _(1 line)_

---

## Bonus — Apne Project Pe Repeat (System-X2)

Apne kisi real feature pe yahi 4 sawaal chala (chhota — 10 min):
- **Q1 (1 line):** Feature kya hai?
- **Q2 (2 threats):** _..._
- **Q3 (controls):** _..._
- **Q4 — design flaw mila?** _(haan/nahi + kya)_ — **agar haan, yeh aaj ka sabse valuable output hai.**

---

## ✅ Definition of Done

- [ ] Yeh file bhari hui (saare Parts A–E)
- [ ] ≥5 threats, har ek ko ek control
- [ ] ≥1 design flaw aur ≥1 impl bug, clearly labeled + reasoning
- [ ] Top-2 prioritized + revisit trigger
- [ ] (Bonus) System-X2 feature pe 4-Q done
- [ ] Commit + push to **main**

> **Yeh file portfolio evidence hai.** Month-1 capstone (Day 30 — ShopFast Threat Model) mein yeh checkout analysis directly reuse hogi. Aaj ka 1-pager future ka building block hai.

---

#lab #threat-modeling #shostack-4-questions #shopfast #checkout #design-flaw #implementation-bug #shift-left #day8
