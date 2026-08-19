# Rules.md — Engineering & AI Rules

> These rules are binding for anyone (human or AI) contributing code to this repo.
> When in doubt, prefer the boring, well-supported option over the clever one.

---

## 1. What to Use

- **Next.js App Router** with server components as the default; add `"use client"` only where interactivity truly requires it.
- **TypeScript everywhere** — `strict: true`, no `any` unless explicitly justified in a comment.
- **Zod** for all input validation at every boundary (forms, server actions, API routes).
- **Prisma** for all database access. No raw SQL unless there's a documented performance reason, and even then it must be parameterized.
- **Tailwind CSS** for styling, using the design tokens defined in `Design.md`. No inline styles except for truly dynamic, computed values.
- **Server actions / route handlers** for mutations, always behind an auth + role check.
- **Environment variables** for all secrets/keys — never hardcoded, never committed.
- **Skeleton loading components** (see `Security.md` §4) for any view that fetches data, especially opportunity lists, dashboard, and roadmap generation.

---

## 2. What to Avoid

### Libraries
- Avoid adding a new dependency for something 20 lines of code can solve.
- Avoid state-management libraries (Redux, Zustand, etc.) unless a real cross-cutting state need appears — server components + React state should cover MVP.
- Avoid heavy animation libraries; prefer CSS transitions/Tailwind + a light library (e.g. Framer Motion) only if `Design.md` animation needs justify it.
- Avoid ORMs/query builders other than Prisma — don't mix data-access patterns.
- Avoid client-side-only auth checks — every protected route/action must also check on the server.
- Avoid picking an AI SDK/library without checking current docs first (see `product-self-knowledge` practice: verify current model names/params, don't rely on memory).

### Error Handling
- Never swallow errors silently (`catch {}` with nothing in it is forbidden).
- Every API route / server action returns a consistent error shape: `{ error: { code, message } }`.
- User-facing errors are friendly and non-technical; internal errors are logged with enough context to debug (route, user id if available, timestamp) but never leak stack traces or DB errors to the client.
- Distinguish validation errors (400), auth errors (401/403), not-found (404), and server errors (500) — don't collapse everything into a generic failure.
- Any AI call must have a fallback path (e.g., show the deterministic data without the AI explanation) if the AI call fails or times out — the AI layer is additive, never a hard dependency for core functionality to render.

### Boundaries for AI (coding assistants working on this repo)
- **Never invent government scheme data, eligibility rules, deadlines, or benefits.** If real data isn't available, use clearly marked placeholder/sample data and flag it.
- **Never let the LLM be the source of truth for eligibility.** Eligibility matching is deterministic code in `lib/matching/`; the LLM only explains an already-computed result.
- **Never remove or weaken disclaimers** ("preliminary match, not an official decision") when touching recommendation or scheme UI.
- **Never widen data collection** beyond what a feature explicitly needs — no adding new "nice to have" profile fields without a stated reason in `PRD.md`.
- **Never commit secrets, API keys, or `.env` files.**
- **Never bypass the rate limiting / security layer** described in `Security.md` when adding new routes.
- **Always update `Memory.md`** after completing meaningful work — see that file's rules.
- **Always check `Architecture.md`'s folder structure** before creating new files/folders — don't invent a parallel structure.
- When uncertain about scope, prefer asking / leaving a `// TODO` with context over guessing and building the wrong thing.