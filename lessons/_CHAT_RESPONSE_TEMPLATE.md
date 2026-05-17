# Chat Response Template — DO NOT DELETE

> This is the **locked format** for every daily chat response. Confirmed by Ijlal on 2026-05-16.
> The Claude routine reads this at session start. Every response in chat MUST follow this exact structure.
> All long content (lessons, labs, reflections) goes to git files. Chat = log + hints only.

---

## The locked format

```markdown
### ✅ Day XXX — Routine Log (executed in this order)

1. `git checkout main && git pull origin main` → branch confirmed
2. Read `progress.md` → last entry: [DAY] → today = Day [XXX]
3. Read `curriculum.md` → topic: [TOPIC] ([PHASE] / Month [N] / Week [N])
4. Read `lessons/_DEPTH_STANDARD.md` → loaded quality rules
5. Wrote `lessons/day-XXX.md` ([WORDCOUNT] words, storytelling style)
6. Wrote `labs/day-XXX-[SLUG].md`
7. Wrote `notes/day-XXX.md` (reflection template)
8. Updated `progress.md` (Day XXX entry + stats N/365)
9. `git add` → `git commit` → `git push origin main` ✅ (`[SHA]`)

---

### 📋 How to approach today's lesson (~60 min total)

- **5 min** → Section 1 ([OPENING STORY HOOK]) — sets the *why* of [TOPIC]
- **30 min** → Sections [X-Y] (core concepts) — read **opening story first**, then mechanisms make sense
- **10 min** → Section [N] ([INCIDENT NAME] deep study) — cement on a real disaster
- **10 min** → Section [N] (quiz) — answer in `notes/day-XXX.md` **without peeking**
- **30-45 min (separate sitting)** → Section [N] (lab) — [ONE-LINE LAB SUMMARY]

---

### 🎯 One sentence to internalize aaj

> *"[KEY INSIGHT FROM TODAY'S LESSON IN ONE LINE]"*

---

### 🔗 GitHub

- 📖 Lesson: https://github.com/shaikhijlalahmed98/cyberforge-365/blob/main/lessons/day-XXX.md
- 🔬 Lab: https://github.com/shaikhijlalahmed98/cyberforge-365/blob/main/labs/day-XXX-[SLUG].md
- 📝 Notes: https://github.com/shaikhijlalahmed98/cyberforge-365/blob/main/notes/day-XXX.md

---

[Optional 1-line CTA only if relevant — e.g., on recap days, capstones, red-team scenarios]
```

---

## Rules

### What CHAT must contain
1. **Routine log** — sequential, numbered, what was done in this session in execution order
2. **Approach hints** — short bullets, time estimates per section, no full explanations
3. **One internalization sentence** — quotable insight
4. **3 GitHub links** — lesson, lab, notes

### What CHAT must NOT contain
- ❌ Lesson content (no paragraphs explaining concepts)
- ❌ Code snippets (those live in lesson files)
- ❌ Glossary or definitions (in lesson)
- ❌ Quiz questions (in lesson)
- ❌ Long incident descriptions (in lesson)
- ❌ Anything that duplicates what's already in git files
- ❌ Asking for confirmation to proceed (trigger fires = do the work)
- ❌ "Should I…" questions (just do it; user can correct after)

### Length target
- **Chat response: 25-40 lines maximum, including code blocks and links**
- Anything longer = trim and move to git

### Special day overrides
- **Day 7, 14, 21, 28 (weekly recap):** Replace "Approach hints" with **"5-question quiz prompt (answer in notes)"** — quiz body lives in lesson file
- **Day 30, 60, ... (monthly capstone):** Approach hints become **multi-day breakdown** ("Today: outline; Tomorrow: draft; Day 30: publish to portfolio/")
- **Day 90, 180, ... (quarterly assessment):** Add a line **"📰 Publish suggestion: [topic for LinkedIn/dev.to]"**
- **Red Team Roulette days:** Replace entire format with a **"🚨 SCENARIO: It's 3:47 AM..."** opener; lesson file becomes an incident playbook, lab becomes the response walkthrough

### Tone
- Roman Urdu + English mix (same as lessons)
- Direct, no preamble, no "let me know if..."
- Mentor, not corporate trainer

---

## Example (from Day 001 — the gold standard)

See commit `ff08316` and the chat exchange where Ijlal confirmed this format on 2026-05-16.
