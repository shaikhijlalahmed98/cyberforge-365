# Lesson Depth Standard — DO NOT DELETE

> This file is the **quality contract** for every `lessons/day-XXX.md` in this repo. The Claude routine reads this at the start of each session to maintain a consistent depth standard. If a lesson feels vague, this file is the reference for what "deep enough" means.

---

## Minimum required sections (every lesson)

1. **Table of Contents** — for navigability
2. **Today's Briefing** — theme, why it matters today, connection to yesterday
3. **Core concept — proper unpacking** (60% of word count)
   - Multiple framings of the definition (NIST framing, practical framing, architect framing)
   - Historical context where relevant (where did this idea come from, why)
   - Multiple sub-concepts each treated with their own depth
   - Concrete code examples in .NET AND Spring Boot (Ijlal's stacks)
   - Real configs (NGINX, Postgres, K8s, etc.) where applicable
   - Anti-patterns table — "mat kar yeh"
   - Common failure modes — numbered list with explanations
   - Backend-dev intuition — one liner takeaway per sub-section
4. **Mental models** — at least 2-3 reusable models the student can carry forward
5. **Trade-offs analysis** — visualized as table or diagram
6. **Architect's decision framework** — step-by-step procedure to apply today's concept
7. **Beyond the basics** — futurist lens, related frameworks, what's evolving
8. **Real-world incident — deep study** (one primary)
   - Timeline (compressed table)
   - Numbers (records, dollars, people affected)
   - Technical root cause (CVE if applicable, with explanation of the actual vulnerability)
   - CIA breakdown (or relevant framework breakdown) — which pillars failed and how
   - Multiple root cause layers (not just "they didn't patch")
   - Architectural controls that would have prevented each layer (table format)
   - Architect's takeaway as a quotable insight
   - Authoritative sources (GAO reports, Senate reports, post-mortems, CVE numbers)
9. **Pattern recognition** — at least 1-2 brief secondary incidents tied to today's concept
10. **Common misconceptions cleared** — table format
11. **Glossary** — every new term defined, ordered alphabetically or by appearance
12. **Self-check quiz** — 6-10 questions of varied difficulty, not multiple choice
13. **Lab assignment** — link to `labs/day-XXX.md`
14. **Reflection & Tomorrow** — journal question, connection prompt, next-day preview
15. **Progress entry** — exact markdown line for `progress.md`

---

## Word count and density targets

| Section | Target |
|---------|--------|
| Total lesson | 3,500-5,000 words |
| Core concept | 2,000-3,000 words |
| Incident study | 600-1,000 words |
| Code snippets | At least 5 concrete examples per lesson |
| Tables | At least 4 (for structured comparison) |
| Diagrams (ASCII or referenced) | At least 1 |

A lesson under 2,500 words is **insufficient depth**. Rewrite.

---

## Voice rules

- **Tone:** Roman Urdu (Latin script only) mixed with English. Senior dost guiding a junior — not corporate trainer.
- **Headers, code blocks, commands, file paths, CVE numbers, incident names:** English.
- **Explanations, analogies, encouragement, casual framing:** Roman Urdu.
- **Analogies:** Always tied to .NET, Spring Boot, microservices, enterprise backend experience.
- **No filler:** Every paragraph must teach something or transition meaningfully.
- **No vague generalities:** "Security is important" = ❌. "Equifax's IDS was blind for 19 months because of an expired cert" = ✅.

---

## CRITICAL: Storytelling rule (yeh sabse important hai)

Ijlal ne explicitly bola: technical terms ke beech mein agar koi shabd drop kar diya bina backstory ke, woh confuse hota hai. Solution: **storytelling style — har technical term ko kahani ki tarah introduce karo.**

### The rule

**Never drop a technical term in passing.** Every term must be introduced with:

1. **The problem that existed first** (kya issue tha jise yeh term solve karta hai)
2. **The story of how someone solved it** (kab, kisne, kis context mein)
3. **The term as the natural conclusion** ("...isi solution ka naam pada X")
4. **Concrete example or analogy** to cement understanding

### Bad example (what NOT to write)

> "Spring Boot mein SecureRandom (CSPRNG — Cryptographically Secure Pseudo-Random Number Generator) JVM ke java.security.SecureRandom ko wrap karta hai. Linux pe yeh /dev/urandom se entropy uthata hai."

**Problems:** "wrap," "CSPRNG," "/dev/urandom," "entropy" — sab dropped bina backstory ke. Reader has to Google 4 terms.

### Good example (what TO write)

> "Chal pehle samjhte hain ke random number kaisey bante hain. Tu jab `new Random()` likhta hai Java mein, woh ek mathematical formula chalata hai jo number generate karta hai — bilkul random nahi, **pseudo-random** (pseudo = jhutha, kyunki actually predictable hai). Yeh formula ek 'seed' se start hota hai (usually current time). Agar attacker tera seed guess kar le, woh tere agle saare 'random' numbers predict kar sakta hai. 2000s mein cracker tools games hack karne ke liye yahi karte the.
>
> Toh phir asli random kahan se laaye? Computer naturally random nahi hai — predictable machine hai. Solution: real world se randomness uthao. Linux ne ek special file banayi `/dev/urandom` — yeh kernel ke andar ek pool maintain karti hai jo collect karta hai unpredictable cheezein: keyboard timing, mouse movement, disk seek delays, network packet arrival. Yeh randomness ki collection ko **entropy** kehte hain (entropy = predict karne ki mushkil).
>
> Java ne `/dev/urandom` ko expose karne ke liye class banayi: **SecureRandom**. Yeh class basically `/dev/urandom` aur Java ke beech bridge code hai — programmer language mein bridge code ko **wrapper** kehte hain. Aaj se yeh yaad rakh: **Random** class = math se generated = predictable = NEVER use for security. **SecureRandom** = OS entropy se = unpredictable = use for tokens, sessions, keys."

**Why it works:** Reader ko 4 terms (pseudo-random, entropy, /dev/urandom, wrapper, SecureRandom) **discover karte hue mile** — Google nahi karna pada. Story dimagh mein stick ho gayi.

### Checklist for every concept introduced

- [ ] Pehle **problem** establish ki?
- [ ] Phir **history** ya **context** diya kiska tha?
- [ ] Phir **solution attempt** ya naya concept introduce kiya?
- [ ] Phir **term** as the natural label diya?
- [ ] Phir **concrete example** ya analogy se cement kiya?
- [ ] Phir **takeaway** or **rule** for the reader to remember?

If any unchecked, rewrite as story.

### Where storytelling matters most

- Defining any cryptographic concept (hash, MAC, signature, KMS, HSM)
- Introducing any protocol (TLS, OAuth, OIDC, SAML)
- Explaining any acronym (CVE, CVSS, IDS, DLP, WAF)
- Discussing any historical incident (always set the scene first)
- Introducing any framework (CIA, STRIDE, NIST, MITRE ATT&CK)

### Tables and bullet points

Tables and bullets are for **comparison and reference**, not primary teaching. Teach in **prose** (paragraphs), summarize in **tables**.

Bad: jump straight into a table of "5 properties of TLS." Good: tell the story of how TLS evolved from SSL, then summarize properties in a table at the end of the section.

---

## Code example rules

- At least one .NET (C#) example per lesson where applicable
- At least one Spring Boot (Java) example per lesson where applicable
- Real syntax, not pseudocode
- Comments in code explain WHY, not WHAT
- Show both the right way AND a labeled wrong way when discussing anti-patterns

---

## Incident study rules

- Must be a **documented public incident** with verifiable sources
- Must include **CVE numbers** where technical vulnerabilities are involved
- Must include **multiple linked sources** (no "according to a blog post")
- Preferred sources: NIST NVD, US GAO reports, CISA advisories, Senate/House oversight reports, vendor post-mortems, MITRE ATT&CK
- Avoid lazy storytelling — give specific dates, specific systems, specific failures

---

## Self-check quiz rules

- 6-10 questions
- No multiple choice — short answer or analysis
- Mix difficulties: definition recall (easy) → application (medium) → architecture analysis (hard)
- At least one question that asks the student to apply today's concept to **their own past projects**
- At least one ShopFast scenario question

---

## Lab assignment rules

- Completable in 30-60 minutes
- Concrete tools, exact commands, success criteria
- Always tied to ShopFast where possible (continuity matters)
- Output is a file in `labs/` that becomes portfolio evidence

---

## When to deviate

- **Day 7, 14, 21 (recap days):** No new concept. Skip Core Concept depth. Add a 5-question quiz of week's content + teach-back prompt.
- **Day 30, 60, 90 (capstones):** Multi-day deliverable, not a single lesson. Output is a portfolio artifact.
- **Day 90, 180, 270, 360 (quarterly):** Scenario-based architecture assessment. Add a public-publication suggestion.
- **Red Team Roulette days (1-2/month):** Open with "🚨 SCENARIO: It's 3:47 AM..." Do not advance curriculum. Walk through detection → containment → eradication → recovery.

---

## Checklist before commit

Before pushing any `lessons/day-XXX.md`, verify:

- [ ] Word count ≥ 2,500
- [ ] All 15 required sections present
- [ ] At least one .NET code example
- [ ] At least one Spring Boot code example (where applicable)
- [ ] Incident study has CVE/sources
- [ ] At least 4 tables
- [ ] At least 1 diagram (ASCII OK)
- [ ] Anti-patterns table present
- [ ] Glossary at least 8 terms
- [ ] Self-check has 6+ questions
- [ ] Lab file referenced and exists
- [ ] Reflection prompts personalized to Ijlal's backend background
- [ ] Progress entry line included at end
- [ ] Lesson reads in Roman Urdu mentor tone (not corporate)

If any unchecked, rewrite before pushing.
