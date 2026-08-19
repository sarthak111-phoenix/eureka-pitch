# Security.md — Security, Rate Limiting & Resilience

> This product handles personal profile data (age, location, education, income category, etc.) and is government-adjacent, so security is not optional polish — see `PRD.md` §4 Trust & Safety. This file is implemented fully in `Phases.md` Phase 7, but basic stubs (rate-limit middleware, logging hooks) should exist from Phase 0 onward so they're not bolted on late.

---

## 1. Regular Security Checkups

Set up recurring, not one-time, checks:

- **Dependency audit:** run `npm audit` (or equivalent) on a schedule (e.g. weekly CI job) and on every PR that touches `package.json`. Block merges on high/critical vulnerabilities without an explicit, documented waiver.
- **Auth/session review:** periodically verify session expiry, token rotation, and that role checks (`user` vs `admin`) are enforced server-side on every protected route/action — not just hidden in the UI.
- **Access control spot-check:** confirm a logged-in non-admin user cannot reach `/admin/*` routes or admin server actions directly (URL guessing, direct API calls).
- **Secrets check:** verify no API keys/secrets are present in the repo, client bundle, or logs (grep for common key patterns as part of CI).
- **Data-minimization review:** periodically re-check that collected profile fields still match what `PRD.md` actually needs — remove/flag any field creep.
- **Dependency/AI-provider docs check:** before changing anything AI-related, verify current API/model behavior against live docs rather than assuming — outdated assumptions about provider APIs are a real failure mode here.

Log the outcome of each checkup in `Memory.md` (Key Decisions / Known Issues) so there's a trail.

---

## 2. Rate Limiting — Divided Per Page/Route

Do not apply one global rate limit to the whole app. Different pages/routes have very different cost and abuse profiles — divide limits accordingly:

| Surface | Why it needs its own limit | Suggested approach |
|---|---|---|
| **Auth (sign up / sign in)** | Prime target for credential stuffing / bot signups | Strict limit per IP + per email (e.g. a handful of attempts per minute, exponential backoff on repeated failures) |
| **Onboarding / profile submit** | Prevent scripted mass fake-profile creation | Per-account limit + basic bot signal check (see §3) |
| **AI-backed routes** (roadmap generation, eligibility explanation, recommendation explanation) | Highest cost per request (LLM calls) | Tightest limit of all — per-user cooldown between generations, and a daily/session cap |
| **Search & Filters / Opportunity discovery** | High traffic, low cost per request, but scrapeable | Moderate limit, generous enough for normal browsing, tuned against scraping patterns |
| **Save/bookmark actions** | Low cost, but can be spammed | Light limit, mostly to prevent abuse loops |
| **Admin routes** | Low traffic, high sensitivity | Strict limit + always behind role check + optionally IP allowlist for extra safety |
| **Report-incorrect-info submissions** | Can be spammed to bury real reports | Per-user/per-IP limit + basic duplicate detection |

Implementation notes:
- Centralize the limiter logic in `lib/rate-limit/` (see `Architecture.md`) so every route uses the same primitive, just configured with different keys/windows per surface above.
- Rate-limit by a combination of identity signals where possible (user id when logged in, IP + fingerprint when not) rather than IP alone.
- Return a clear, non-technical rate-limit message to the user, and a proper `429` status with `Retry-After` where applicable.

---

## 3. Flagging Miscellaneous Signups & Suspicious Activity

- **Signup anomaly signals** to track and flag (not necessarily auto-block) for admin review:
  - Many signups from the same IP/device in a short window.
  - Disposable/throwaway email domains.
  - Profile data that's implausible or clearly patterned (e.g. sequential fake names, identical profiles at scale).
  - Accounts that generate AI content (roadmaps/explanations) at a rate far beyond normal human use.
- **Activity flags** to track:
  - Repeated failed logins.
  - Rapid save/unsave or search churn suggesting scraping rather than browsing.
  - Excessive "report incorrect information" submissions from one account.
- **Handling flagged activity:**
  - Do not silently hard-block on a single signal — queue flagged accounts/activity for admin review (extend the Admin panel from `Phases.md` Phase 6 with a lightweight "Flagged Activity" view).
  - Log flags with enough context (what triggered it, when, which account/IP) without over-collecting unrelated personal data.
  - Escalate to auto-throttle (not necessarily ban) when a flag pattern is severe and repeated.

---

## 4. Skeleton Loaders for Slow/High-Latency States

Whenever a page depends on a fetch that could be slow — especially AI-backed generation (roadmap, eligibility explanation) or larger data pulls (opportunity discovery, dashboard aggregation) — the UI must show a **skeleton loading state** matching the real content's shape, not a blank screen or spinner-only state, so the page still feels responsive and the layout doesn't jump when data arrives.

Guidelines:
- Build skeleton components in `components/skeletons/` (see `Architecture.md`) that mirror each real component's layout: card skeletons for opportunity cards, line-skeletons for text blocks, block skeletons for the roadmap timeline, etc.
- Use skeletons on: Dashboard load, Opportunity discovery/search results, Opportunity detail, Roadmap generation, Skill-gap analysis, Scheme matching/eligibility explanation.
- Style per `Design.md` — subtle shimmer using the dark-theme surface tokens (`--bg-surface-raised`), never a bright/white shimmer.
- Treat any AI-backed generation (roadmap, explanations) as **always** needing a skeleton/progressive state, since LLM latency is inherently variable — never let these routes render a blank page while waiting.
- If a request is taking unusually long (approaching a timeout threshold) or a backend dependency looks like it's struggling, prefer degrading gracefully: keep showing the skeleton (or a "this is taking longer than usual" note) and retry/fall back rather than crashing the page or showing a raw error.
- For AI routes specifically: since the AI layer is explicitly non-critical-path (see `Rules.md` — AI calls must have a fallback), if generation fails or times out, fall back to showing the deterministic data (e.g. the rule-based recommendation list without the AI explanation text) rather than blocking the whole view.

---

## 5. Summary Checklist (use during Phase 7 hardening)

- [ ] Rate limiting implemented per-surface as in §2, not globally
- [ ] Signup/activity flags implemented and visible in an admin "Flagged Activity" view
- [ ] Recurring dependency/audit checks wired into CI
- [ ] Server-side role checks confirmed on every admin route/action
- [ ] Skeleton loaders present on every data-fetching page, especially AI-backed ones
- [ ] AI-route fallback behavior confirmed (deterministic data still renders if AI call fails/times out)
- [ ] No secrets in repo/client bundle (CI check passes)
- [ ] Findings and decisions logged in `Memory.md`