# Day 008 — Why Threat Modeling Matters: Shifting Security Left (Notes)

**Date:** 2026-05-24
**Phase:** Q1 → Month 1 → Week 2 (Threat Modeling Foundations)
**Theme:** Defenses *kahan/kab* lagein — design ke time pe, breach ke baad nahi. Shostack ke 4 sawaal + shift-left.

> Yeh file aaj ka **asli kaam** hai. Lesson padhna passive hai; yahan bina peek kiye likhna active recall hai. (Day 7 recap ne flag kiya tha — notes blank reh rahe hain. Aaj kam se kam Core Concept + 3 quiz answers fill kar.)

---

## Core Concept (in my own words)

> 4-6 sentences: threat modeling kya hai, "shift left" ka matlab, aur yeh kyun important hai. Ek junior backend dev ko samjha jo sochta hai "pentest deploy se pehle kara lenge, kaafi hai."

_Your summary here..._

---

## The Key Terms — One-Liner Each

- **Threat modeling:** _(your one-liner)_
- **Shift left:** _(your one-liner)_
- **Shostack's 4 Questions:** _(naam se chaaron)_
- **Design flaw:** _(your one-liner)_
- **Implementation bug:** _(your one-liner)_
- **Insecure Design (OWASP A04):** _(your one-liner)_
- **Reactive vs Proactive security:** _(your one-liner)_

---

## Shostack's 4 Questions — Say It Cold

1. _____________________ (Q1)
2. _____________________ (Q2)
3. _____________________ (Q3)
4. _____________________ (Q4)

- **Day 7 ka risk register kaunse sawaal ka output tha?** → _____
- **Q2 mein STRIDE kahan fit hota hai (Day 9 preview)?** → _____

---

## The Cost Curve (recall karke likh)

```
Requirements → Design → Code → Test → Deploy → Production → BREACH
   1x            1x      ~5x    ~15x    ?         ?            ?
```
- **Design vs production breach cost multiplier:** _____
- **Yeh curve kya justify karti hai?** → _____
- **Shift-left ek line mein:** _____

---

## Design Flaw vs Implementation Bug (aaj ka dil)

- **Implementation bug:** _(definition + 1 example)_
- **Design flaw:** _(definition + 1 example)_
- **Kaunsa patch se theek hota hai, kaunsa redesign maangta hai?** → _____
- **Threat modeling kaunsa pakadta hai, aur SAST/pentest kyun doosra miss karta hai?** → _____
- **Refund endpoint (§2.6) ke 2 design flaws:** (1) _____ (2) _____
- **Threat-modeled version mein "authority" kiske paas?** → _____

---

## Mental Models (apne shabdon)

1. **The 4 Questions** → _(your version)_
2. **"Left of build"** (vs Week 1 ka "left of boom") → _(your version)_
3. **Implementation bug vs design flaw** ("patch ya redesign?") → _(your version)_
4. **1x → 100x cost curve** → _(your version)_

---

## Self-Check Quiz Answers

Answer **without peeking**. Mark difficulty (E)/(M)/(H). Galat ya peek kiya toh ❌.

**Q1. (E) Threat modeling 1 line + Shostack ke 4 sawaal naam se.**
_Answer..._

**Q2. (E) "Shift left" + 1x→100x curve kya justify karti hai.**
_Answer..._

**Q3. (M) Implementation bug vs design flaw — fark + ek-ek example + kaunsa TM se pakda kyun.**
_Answer..._

**Q4. (M) Risk register kaunse Shostack sawaal ka output; baaki 3 kahan fit.**
_Answer..._

**Q5. (M — Zoom) Root design decision + Q2 kaise pakadta + Apple ne kya unusual kiya aur signal.**
_Answer..._

**Q6. (M — Code) Refund endpoint ke 2 design flaws + "authority" kiske paas + kyun design change (code trick nahi).**
_Answer..._

**Q7. (H — ShopFast) Checkout pe 4 sawaal — Q1 (1-2 line), Q2 (3 threats), Q3 (har ek control).**
_Answer..._

**Q8. (H — Mera project) Ek bina-security-socha shipped feature → ek design flaw + design-phase vs ab cost (1x vs ?).**
_Answer..._

**Q9. (H — Futurist) LLM chatbot ke 3 AI threats (prompt injection/poisoning/extraction) + STRIDE ki jagah kaunsa framework.**
_Answer..._

---

### Quiz Scorecard
- 🟢 solid: ___ / 9
- 🟡 shaky: ___ / 9
- 🔴 guessed: ___ / 9
- **Re-revise topics:** ___

---

## Journal Question

> "Apne career mein ek feature soch jise tune ship kiya aur baad mein security/abuse issue nikla (ya nikal sakta tha). Imaandari se: woh **implementation bug** tha ya **design flaw**? Agar design flaw tha — kya koi 30-minute 'what can go wrong?' session usse design phase mein pakad leta? Aur tab fix karna ab fix karne se kitna sasta hota?"

_Answer here..._

---

## Connection to System-X2 / My Backend Work

> "Tu microservices design karte waqt diagrams banata hai (sequence, API contracts). Agle naye feature pe us *normal design ritual* mein ek extra sawaal add kar: **'what can go wrong?'** Dekh kya surface hota hai jo pehle miss ho jaata. Yeh shift-left ki pehli aadat hai."

**Mera design ritual abhi (steps):**
1.
2.

**"What can go wrong?" add karne par jo naye threats surface honge (guess 2):**
1.
2.

---

## Zoom (2019) Takeaways — Threat-Modeling Lens

- The root **design** decision (UX convenience): _____
- Q2 "what can go wrong?" use kaise pakad leta: _____
- The 3 things an attacker website could do: (1) _____ (2) _____ (3) _____
- CVE number(s): _____
- What Apple did (unusual) + kya signal: _____
- "Implementation bug nahi, design flaw" — kyun: _____
- One architectural control I'll remember (secure-by-default? trust boundary? design TM?): _____

---

## Pattern Recognition (secondary)

- **Peloton API (2021):** the design assumption that failed = _____ ; common pattern with Zoom = _____
- **The shared lesson (both):** koi code bug nahi tha — flaw _____ mein tha; scanner kyun miss karta = _____

---

## Things That Stuck

1.
2.
3.

## Things I Need to Re-read

1.
2.

## Terms That Need More Practice

- [ ] Shostack's 4 Questions (cold recall, in order)
- [ ] Shift left + cost curve (1x→100x)
- [ ] Design flaw vs implementation bug
- [ ] Insecure Design (OWASP A04)
- [ ] Reactive vs proactive security
- [ ] Zoom CVE-2019-13450 (the design decision)
- [ ] "Threat model that ships > perfect one that doesn't"

---

## Lab Status

- [ ] Lab `labs/2026-05-24-day8-four-questions-shopfast.md` started
- [ ] Part A: checkout flow + data stores + trust boundaries
- [ ] Part B: ≥5 "what can go wrong" threats (adversarial)
- [ ] Part C: each threat → control + type
- [ ] Part D: ≥1 design flaw + ≥1 impl bug, labeled + reasoning
- [ ] Part E: top-2 prioritized + revisit trigger + cost reflection
- [ ] (Bonus) System-X2 feature pe 4-Q repeat

---

## Futurist Angle (3-5 years)

> Threat-modeling-as-code (OWASP pytm, Threagile, IriusRisk); AI/LLM threat modeling (OWASP LLM Top 10, MITRE ATLAS — prompt injection/poisoning/extraction); AI-assisted "what can go wrong?" brainstorm; "secure by design/default" (CISA 2023). In mein se ek pe 2 lines apne shabdon mein:

_Your answer..._

---

## Tomorrow's Preview

**Day 9 → STRIDE methodology.** Aaj humne poocha "what can go wrong?" (Shostack Q2); kal seekhenge ek structured checklist — **S**poofing, **T**ampering, **R**epudiation, **I**nformation disclosure, **D**enial of service, **E**levation of privilege — taake brainstorm mein koi category miss na ho. Microsoft ka classic framework.

---

#threat-modeling #shift-left #shostack-4-questions #insecure-design #owasp-a04 #design-flaw #implementation-bug #zoom #cve-2019-13450 #peloton-api #proactive-security #shopfast #stride-preview
