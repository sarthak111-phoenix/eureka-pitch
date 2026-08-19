# Phases.md — Development Phases

> Each phase should be shippable/demoable before moving to the next. Update `Memory.md` at the end of every phase (and every work session within a phase).

---

## Phase 0 — Project Setup
- Repo scaffolding per `Architecture.md` folder structure
- Next.js + TypeScript + Tailwind config, Prisma init
- Base design tokens from `Design.md` wired into Tailwind config
- CI basics (lint, typecheck)

## Phase 1 — Foundation
- Authentication (sign up / sign in / sign out, session-based)
- User roles (`user`, `admin`)
- Database schema: `User`, `UserProfile`, `StudentProfile`
- Onboarding flow (common + student + citizen profile fields)
- Basic UI shell: layout, nav, empty dashboard
- Admin role gate (no admin features yet, just access control)

## Phase 2 — Opportunity Engine
- `Opportunity` schema + seed/sample data (curated, clearly marked as sample until real data is verified)
- Opportunity list/discovery page with filters (category, state, education, age group, type, deadline, etc.)
- Search (start with structured filters; natural-language → filter translation can land later in this phase)
- Opportunity detail page
- Save/bookmark (`SavedOpportunity`) + Saved Opportunities page

## Phase 3 — Personalization
- Deterministic rule-based scoring engine (`lib/matching/`)
- `Recommendation` schema + generation logic
- Government scheme matching against `UserProfile`
- AI explanation layer wired in (recommendation "why" + eligibility plain-language translation), clearly labeled and separated from official criteria
- Dashboard populated with real recommendations

## Phase 4 — Career Engine
- Career goal selection
- `Skill` schema + skill data
- Skill gap analysis (current vs. target)
- `CareerRoadmap` generation (AI-assisted sequencing over curated skill data)
- Roadmap UI on dashboard + dedicated roadmap page

## Phase 5 — Trust & Validation
- Official-source links + "last verified" date surfaced everywhere an opportunity/scheme appears
- Report-incorrect-information flow (user-facing + admin review queue)
- Disclaimer system finalized and audited across every recommendation surface
- Recommendation feedback capture (thumbs up/down or similar, for the quality metrics in `PRD.md`)

## Phase 6 — Admin Panel
- Admin dashboard (activity overview, pending reports)
- Opportunity management: add/edit/delete/archive, verification date, active/inactive toggle
- Report review workflow

## Phase 7 — Security & Resilience Hardening
- Full implementation of `Security.md`: rate limiting per page/route, signup/activity anomaly flags, regular automated security checks
- Skeleton-loading states across all data-fetching views
- Load testing on high-traffic pages (dashboard, opportunity discovery)

## Phase 8 — Polish & Launch Readiness
- Responsive/mobile QA across all pages
- Accessibility pass (keyboard nav, contrast against the dark theme in `Design.md`, readable typography)
- Copy pass for plain-language, non-technical tone
- Success-metric instrumentation (from `PRD.md` §5) wired into analytics

---

### Notes
- Phases can overlap in a small team — e.g. Design tokens (Phase 0) and Security scaffolding (basic rate-limit stubs) can start early even though full hardening is Phase 7.
- Do not skip Phase 5 disclaimers/labeling to "move faster" — this is a trust-critical product per `PRD.md`.