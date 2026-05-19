# 📝 Daily Notes

This folder holds my day-by-day learning notes. **One file per day**, named `<YYYY-MM-DD>-day<N>-<slug>.md`.

## File naming convention

- Format: `<YYYY-MM-DD>-day<N>-<topic-slug>.md`
- `<YYYY-MM-DD>` = calendar date the note was written (chronological sort)
- `<N>` = day number (no zero-padding) — `day1`, `day2`, ..., `day365`
- `<topic-slug>` = kebab-case theme matching the lesson file
- Examples: `2026-05-16-day1-cia-triad.md`, `2026-05-19-day2-threat-vulnerability-risk-exploit.md`

## Note template

Every daily note should follow this structure:

```markdown
# Day XXX — [Theme]

**Date:** YYYY-MM-DD
**Phase:** Q1 / Q2 / Q3 / Q4
**Week:** N
**Streak:** X days

## 🧠 Core concept learned
[The one big idea today. In my own words.]

## 🔗 Connection to backend dev
[How does this relate to what I already know? Microservices? .NET? Spring Boot?]

## 🎯 Incident takeaway
[The breach we studied. Root cause in one line. Architectural lesson in one line.]

## 🔬 Lab outcome
[What I built. What broke. What I fixed. Link to /labs/<YYYY-MM-DD>-day<N>-<slug>/]

## ❓ Open questions
[What I didn't understand. Will revisit on recap day.]

## 🚀 Futurist angle
[How does this concept evolve in 3-5 years? Quantum? AI? Zero-trust?]

## 📌 Tomorrow's preview
[One line from Claude]
```

## Why notes matter

This is the **proof of understanding**. The Claude routine reads recent notes to:
- Adjust difficulty if I'm struggling
- Skip concepts I've already nailed
- Connect today's lesson to yesterday's insight
- Generate personalized week recaps

**Write in my own words.** Copy-pasting Claude's output defeats the purpose. The note is the moment knowledge becomes mine.

## Searchability tip

Tag notes with `#topics` at the bottom for easy grep later:

```
#stride #threat-modeling #microservices #dotnet
```

By Day 365 I'll have a personal knowledge base I can search and cite.
