# Memory.md — Project Memory / Progress Log

> **Purpose:** This file is the single source of truth for "where the project actually is." Any AI (or human) resuming work after a break, a long queue, or a new session must read this file **first**, before touching code, to avoid hallucinating progress, redoing finished work, or contradicting past decisions.

---

## 0. Rules for Updating This File (read before editing)

1. **Update this file after every meaningful work session** — not just at the end of a phase. If you built/fixed/decided something and then stopped, write it down before you stop.
2. **Never mark something "Completed" unless it actually works** (builds, runs, passes whatever basic check applies). If it's partial, say so explicitly under "In Progress."
3. **Always update "Currently Working On"** to reflect the actual next file/task — this is the resume point for the next session.
4. **Log decisions, not just tasks** — e.g. "chose Option B dark palette in Design.md" belongs here so future sessions don't re-litigate it.
5. **Keep entries dated** so there's a timeline. Newest entries at the top of the Log.
6. **Don't delete history** — if something completed earlier gets reverted or changed, add a new dated entry noting the change; don't silently rewrite the past.
7. **Be specific.** "Worked on dashboard" is not useful. "Implemented dashboard layout + For You / Your Career / Saved cards per Design.md, wired to mock data — recommendation data not yet real (Phase 3)" is useful.

---

## 1. Current Status Snapshot

**Current Phase:** _(fill in — see `Phases.md`, e.g. "Phase 0 — Project Setup")_

**Currently Working On:** _(the exact file/feature being actively worked on right now — this is the resume point)_

**Blocked On:** _(anything waiting on a decision, external data, or a missing credential — or "Nothing" )_

---

## 2. Completed So Far

_(nothing yet — this is a fresh project. Update this list as work lands. One line per completed, working item, e.g.:)_

- [ ] _(example format)_ ~~Phase 0: repo scaffolding, Next.js + TS + Tailwind + Prisma init~~ — *not yet done*

---

## 3. Key Decisions Log (append-only, newest first)

_(record locked-in decisions here so they aren't re-decided or contradicted later — e.g. palette choice from `Design.md`, auth provider choice, AI provider choice)_

- _(none yet)_

---

## 4. Known Issues / TODOs Carried Forward

_(anything left half-done, any TODO comments left in code, anything intentionally deferred to a later phase)_

- _(none yet)_

---

## 5. Session Log (append-only, newest entry on top)

Format per entry:
```
### YYYY-MM-DD — <short title>
- What was done:
- Files touched:
- Decisions made:
- What's next:
```

_(no sessions logged yet — the first contributor/AI to touch this repo should add the first entry here)_