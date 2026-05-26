# Day 010 — Data Flow Diagrams (DFDs) (Notes)

**Date:** 2026-05-26
**Phase:** Q1 → Month 1 → Week 2 (Threat Modeling Foundations)
**Theme:** STRIDE-per-element ka asli canvas. Shostack Q1 ("what are we working on?") ka tool. Trust boundary = threats ka birthplace.

> Yeh file aaj ka **asli kaam** hai. Lesson padhna passive hai; yahan bina peek kiye likhna active recall hai.
> ⚠️ **Honest flag (Day 7/8/9 ne bhi bola):** pichle notes mostly blank reh gaye. Aaj target chhota rakh — **Core Concept (4-6 line) + 5 building blocks shapes ke saath cold + Q1/Q2/Q5/Q6 answers**. Itna fill kar diya toh aaj jeet gaya. *Bhare* hue notes ki zaroorat hai, perfect ki nahi.

---

## Core Concept (in my own words)

> 4-6 sentences: DFD kya hai, kyun zaroori hai (assumed flow ≠ actual flow), aur trust boundary ka magic. Ek junior backend dev ko samjha jo sequence diagrams se DFD ka farak nahi jaanta.

_Your summary here..._

---

## DFD ke 5 Building Blocks — Say It Cold (table bina peek kiye)

| # | Block | Shape | STRIDE letters applicable | Ek-line role |
|---|-------|-------|---------------------------|---------------|
| 1 | External Entity | | | |
| 2 | Process | | | |
| 3 | Data Store | | | |
| 4 | Data Flow | | | |
| 5 | Trust Boundary | | | (amplifier, not threat target itself) |

- **Trust boundary kis liye dashed line?** _____
- **Process pe full STRIDE kyun?** _____
- **External entity pe sirf S/R kyun, T/E nahi?** _____

---

## DFD Levels (recall karke)

- **L0 (Context):** _____ (kya dikhta, audience kaun)
- **L1:** _____ (kya appear hota jo L0 mein nahi tha)
- **L2/L3:** _____ (kab banate)
- **Rule of thumb (Shostack):** _____ elements per diagram

---

## Trust Boundary Types — 5 yaad?

1. Network boundary — _____
2. Process boundary — _____
3. Privilege/role boundary — _____
4. Code-origin boundary — _____ ← (BA Magecart wala)
5. _____ — _____ (compliance/org/data-classification — koi ek)

---

## The Key Terms — One-Liner Each

- **DFD:** _(your one-liner)_
- **External Entity:** _(your one-liner)_
- **Process:** _(your one-liner)_
- **Data Store:** _(your one-liner)_
- **Data Flow:** _(your one-liner)_
- **Trust Boundary:** _(your one-liner)_
- **L0 Context Diagram:** _(your one-liner)_
- **Decomposition:** _(your one-liner — kab aur kyun)_
- **Subresource Integrity (SRI):** _(your one-liner)_
- **Content Security Policy (CSP):** _(your one-liner)_
- **Homoglyph attack:** _(your one-liner)_
- **Threat-modeling-as-code (TMaC):** _(your one-liner + 2 tool names)_

---

## Mental Models (apne shabdon)

1. **"Picture before checklist"** → _(your version)_
2. **"Threats live on the dashed lines"** → _(your version)_
3. **"Assumed flow ≠ actual flow"** → _(your version)_
4. **"Decompose until each process fits in one head"** → _(your version)_

---

## DFD vs Other Diagrams — 1-line farak

- DFD vs Sequence diagram: _____
- DFD vs Architecture diagram: _____
- DFD vs C4 Container/Component: _____
- DFD vs ER diagram: _____

---

## Self-Check Quiz Answers

Answer **without peeking**. Mark difficulty (E)/(M)/(H). Galat ya peek kiya toh ❌.

**Q1. (E) DFD ke 5 building blocks + har ki shape.**
_Answer..._

**Q2. (E) Trust boundary 1 line + DFD shape.**
_Answer..._

**Q3. (M) L0 vs L1 farak; kab L0 kaafi, kab L1 chahiye.**
_Answer..._

**Q4. (M) Sequence diagram vs DFD: 2 structural farak; security mein DFD kyun preferred.**
_Answer..._

**Q5. (M — BA Magecart) Assumed vs actual DFD ka farak; 2 architectural controls.**
_Answer..._

**Q6. (M — STRIDE × DFD) Data Flow pe kaunse 3 letters + baaki 3 kyun nahi.**
_Answer..._

**Q7. (H — ShopFast) L1 se 3 boundary-crossings; har pe 1 threat + 1 mitigation.**
_Answer..._

**Q8. (H — Mera project) System-X2 L0; external entities count; ek "background mein assumed" entity.**
_Answer..._

**Q9. (H — Futurist) TMaC vs whiteboard DFD: 2 advantages + 1 trade-off.**
_Answer..._

---

### Quiz Scorecard
- 🟢 solid: ___ / 9
- 🟡 shaky: ___ / 9
- 🔴 guessed: ___ / 9
- **Re-revise topics:** ___

---

## Journal Question

> "Apne current project (System-X2 ya koi naya feature) ka L0 + L1 DFD imagine kar. Imaandari se: kaunse 2 components ya data flows hain jo 'background mein' assume hote hain par kabhi formally draw nahi hue? Yeh saare 'undrawn flows' tera blind spot hain — woh kya hain, aur agle design review mein kis ek ko tu pehle draw karke STRIDE chalayega?"

_Answer here..._

---

## Connection to System-X2 / My Backend Work

> "Microservices mein har service ka apna README hota hai. Aaj se: har service README ke top pe ek chhota ASCII DFD ho — kaun mujhe call karta hai, main kaunko call karta hoon, mera data kahan rukta hai, trust boundaries kahan hain. 5 min ka kaam, lifetime ki clarity."

**Mera ek service (jise pehle pick karunga):** _____
**Uska mini-DFD (ASCII, 3-5 line):**

```
[ caller ] ──── { data } ────► ( my-service ) ──── { ... } ────► ═══════
                                       │                          ═══DB══
                                       ▼                          ═══════
                                  ┄┄┄┄┄┄┄┄┄┄ trust boundary ┄┄┄┄┄┄┄┄
                                       │
                                  ( downstream )
```

**Sabse uncertain trust crossing:** _____

---

## British Airways Magecart (2018) Takeaways — DFD Lens

- Assumed data flow (1 line): _____
- Actual data flow that nobody drew (1 line): _____
- Number of cards stolen: _____
- Exfil domain (homoglyph): _____
- 2 architectural controls jo prevent karte (browser-side):
  1. _____
  2. _____
- Server-side ICO fine: £_____
- "First-party origin ≠ first-party code" — ek line mein meaning: _____
- One architectural lesson I'll carry: _____

---

## Pattern Recognition (secondary)

- **T-Mobile 2023 API breach:** kaunsa flow draw NAHI tha? _____ ; records exposed: _____
- **Common pattern (BA + T-Mobile):** _____ (assumed vs actual flow)

---

## Things That Stuck

1.
2.
3.

## Things I Need to Re-read

1.
2.

## Terms That Need More Practice

- [ ] DFD 5 building blocks (shape + STRIDE applicability)
- [ ] Trust boundary 5 types
- [ ] L0 vs L1 decomposition rules (7±2)
- [ ] Sequence vs DFD farak
- [ ] BA Magecart 2018 — code-origin boundary lesson
- [ ] SRI + CSP (browser-side controls)
- [ ] TMaC tools: pytm, Threagile, STRIDE-GPT
- [ ] Homoglyph attack concept
- [ ] T-Mobile 2023 API (undrawn flow)

---

## Lab Status

- [ ] Lab `labs/2026-05-26-day10-shopfast-dfd.md` started
- [ ] Part A: L0 drawn + exported + embedded
- [ ] Part B: L1 drawn + 5+ sub-processes + 4+ stores + 4+ trust boundaries
- [ ] Part C: ≥5 boundary-crossings tabled with STRIDE
- [ ] Part D: 5 NEW threats (not in Day 8/9 labs)
- [ ] Part E: System-X2 L0 mental sketch + reflection

---

## Futurist Angle (3-5 years)

> Threat-modeling-as-code (pytm, Threagile) → DFD versioned in Git, CI-checked, PR-blocking. eBPF-based runtime observability (Cilium Hubble) → observed DFD vs designed DFD diff = drift detection. AI-aware DFD notation (MITRE ATLAS, OWASP LLM Top 10) → prompt context, RAG retrieval, tool-use flows. In mein se ek pe 2 lines apne shabdon mein:

_Your answer..._

---

## Tomorrow's Preview

**Day 11 → Trust Boundaries — jahan threats paida hote hain.** Aaj DFD mein dashed lines draw karna seekha. Kal sirf un dashed lines pe deep-dive: kaun-kaun se trust boundaries hoti hain (network, process, privilege, org, code-origin), aur har crossing pe kya questions poochne hain. Zero-trust architecture ka conceptual foundation.

---

#dfd #data-flow-diagram #threat-modeling #trust-boundary #shostack-q1 #stride-per-element #l0-context #l1-decomposition #british-airways-magecart-2018 #subresource-integrity #content-security-policy #t-mobile-api-breach-2023 #shopfast #threat-modeling-as-code #pytm #threagile #homoglyph-attack
