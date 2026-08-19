# Architecture.md — App Flow & Technical Architecture

> Read `PRD.md` first for feature scope. This file defines how the system is built.

---

## 1. Tech Stack

| Layer | Choice | Notes |
|---|---|---|
| Frontend | Next.js (App Router) + React + TypeScript | Server components by default, client components only where interactivity is needed |
| Styling | Tailwind CSS | See `Design.md` for theme tokens |
| Backend | Next.js server actions / route handlers | Keep API surface inside the same repo for MVP |
| Validation | Zod | Every input (form, API route, server action) validated at the boundary |
| ORM | Prisma | Type-safe DB access |
| Database | PostgreSQL | Managed instance (Neon/Supabase/RDS — decide in PRD refinement) |
| Auth | Managed provider (e.g. Clerk) or NextAuth with a DB adapter | Secure, session-based; role field for `user` vs `admin` |
| AI | LLM API (provider TBD) called **only** for explanation/ranking-language/roadmap text — never as source of eligibility truth | See §4 |
| Search | Postgres full-text/filter queries for MVP | Vector/semantic search deferred to a later phase |
| Hosting | Vercel (frontend + edge) + managed Postgres | Simple, low-ops MVP deployment |

---

## 2. High-Level App Flow

```
New User:
Landing → Sign Up → Select User Type (Student/Citizen) → Profile Setup
        → Profile Completion → Personalized Dashboard

Student:
Dashboard → Career Goal → Skill Gap Analysis → Roadmap
          → Recommended Opportunities → Opportunity Detail → Save/Apply

Citizen:
Dashboard → Profile → Scheme Matching → Recommended Schemes
          → Eligibility Explanation → Scheme Detail → Official Application

Admin:
Admin Login → Admin Dashboard → Opportunity Management
            → Verification → Publish/Update
```

**Recommendation flow (critical architecture rule):**

```
Official/curated scheme data
   → structured eligibility rules (deterministic, code-owned)
   → deterministic matching engine (rule-based scoring)
   → AI explanation layer (plain-language "why," never invents rules)
   → UI (labeled: Official criteria / Preliminary match / AI explanation)
```

The LLM is never the source of truth for eligibility — it only explains a result the rules engine already produced.

---

## 3. Folder & File Structure

```
/
├── app/
│   ├── (public)/
│   │   ├── page.tsx                  # Landing
│   │   ├── about/page.tsx
│   │   ├── sign-in/page.tsx
│   │   └── sign-up/page.tsx
│   ├── (auth)/
│   │   ├── onboarding/page.tsx
│   │   ├── dashboard/page.tsx
│   │   ├── profile/page.tsx
│   │   ├── career/
│   │   │   ├── roadmap/page.tsx
│   │   │   └── skill-gap/page.tsx
│   │   ├── opportunities/
│   │   │   ├── page.tsx              # discovery/search/filters
│   │   │   └── [id]/page.tsx         # detail page
│   │   ├── schemes/page.tsx
│   │   └── saved/page.tsx
│   ├── admin/
│   │   ├── page.tsx                  # admin dashboard
│   │   ├── opportunities/page.tsx
│   │   ├── opportunities/[id]/edit/page.tsx
│   │   └── reports/page.tsx
│   ├── api/
│   │   ├── recommendations/route.ts
│   │   ├── roadmap/route.ts
│   │   ├── schemes/match/route.ts
│   │   └── report/route.ts
│   └── layout.tsx
│
├── components/
│   ├── ui/                           # buttons, cards, inputs (shared primitives)
│   ├── dashboard/
│   ├── opportunity/
│   ├── scheme/
│   ├── roadmap/
│   └── skeletons/                    # loading-state skeleton components (see Security.md)
│
├── lib/
│   ├── db/                           # Prisma client, queries
│   ├── auth/                         # session helpers, role guards
│   ├── matching/                     # deterministic rule-based scoring engine
│   ├── ai/                           # LLM client + prompt templates, output validation
│   ├── validation/                   # Zod schemas
│   ├── rate-limit/                   # per-route/per-page rate limiting (see Security.md)
│   └── utils/
│
├── prisma/
│   ├── schema.prisma
│   └── migrations/
│
├── types/
│
├── public/
│
├── PRD.md
├── Architecture.md
├── Rules.md
├── Phases.md
├── Design.md
├── Memory.md
└── Security.md
```

---

## 4. AI Layer Responsibilities (only these four)

1. **Profile analysis** — structured profile → recommendation context.
2. **Skill gap analysis (explanation only)** — the comparison itself is deterministic; AI phrases the summary.
3. **Recommendation explanation** — "why this was shown," grounded in the rule engine's output.
4. **Eligibility explanation** — translate structured eligibility rules into plain language, without adding or altering rules.

AI output is always rendered in a visually distinct "AI-generated" block, separate from official criteria and preliminary-match badges.

---

## 5. Data Model (initial entities)

`User`, `UserProfile`, `StudentProfile`, `Opportunity`, `SavedOpportunity`, `Recommendation`, `CareerRoadmap`, `Skill` — see `PRD.md` §3 onboarding fields for the profile schema. Full field list and Prisma schema to be finalized during Phase 1 (see `Phases.md`).