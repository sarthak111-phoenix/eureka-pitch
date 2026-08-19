# PRD.md — Product Requirement Document

> Project: **Opportunity Engine India** (working name)
> Source: derived from `MVP_1.md`

---

## 1. What to Build

A web platform where Indian students and citizens create a structured profile and receive **personalized, explainable** recommendations across two connected engines:

1. **Career Growth Engine** — turns a student's profile into a personalized skill/career roadmap and identifies skill gaps against a target career.
2. **Government Opportunity Engine** — compares a citizen's profile against a curated database of government schemes/scholarships/benefits and surfaces potentially relevant ones in plain language.

**Core Loop:** `Profile → Match → Explain → Discover → Save → Act`

**Core Hypothesis to prove in MVP:** If a user provides a structured profile, can the platform reliably turn that into a small, understandable, personalized list of career opportunities and/or government schemes?

**Product Principle:** This is NOT "an AI that knows everything about every scheme." It IS "a trusted, personalized discovery layer that points users toward authoritative official sources." AI explains and ranks — it is never the source of truth for eligibility.

---

## 2. Targeted Users

### Primary

**A. Students / Early-career users** — want career direction, skill-gap identification, learning roadmaps, internships, scholarships, competitions, certifications, skill-development programs.

**B. General citizens** — want government schemes, subsidies, scholarships, employment programs, healthcare/welfare benefits, financial assistance, skill-development programs.

### Secondary

**C. Admin / Content manager** — manages opportunity data, adds/updates government schemes, manages career resources, reviews reported incorrect info, monitors platform activity.

---

## 3. Features (MVP Scope)

| # | Feature | Summary |
|---|---------|---------|
| 1 | Authentication | Sign up / sign in / sign out, secure session-based auth, personal profile |
| 2 | Onboarding | Collect only the minimum profile data needed for matching (common + student + citizen fields) |
| 3 | Personalized Dashboard | Welcome/profile summary, recommended opportunities, recommended schemes, roadmap summary, saved items, profile completeness, disclaimers |
| 4 | AI Career Roadmap | Personalized, sequenced skill roadmap for a chosen career goal (skills, why it matters, resources, suggested project, sequence) |
| 5 | Skill Gap Analysis | Current skills vs. target career requirements → covered / missing / priority / sequence |
| 6 | Government Scheme Matching | Structured scheme database matched deterministically against user profile |
| 7 | AI Eligibility Explanation | Plain-language translation of official eligibility rules — clearly labeled as AI-generated, separate from official criteria and from preliminary match |
| 8 | Opportunity Recommendations | Unified opportunity model spanning schemes, scholarships, internships, competitions, certifications, courses, jobs, projects |
| 9 | Intelligent Ranking | Rule-based scoring (profile relevance, eligibility match, interests, goal, location, education, skill gap, deadline) + AI-generated explanation text |
| 10 | Search & Filters | Manual browsing with filters (category, state, education, age group, type, domain, deadline, eligibility, government level) + natural-language search converted to structured filters |
| 11 | Save / Bookmark | Save opportunities, view from Dashboard → Saved Opportunities |
| 12 | Opportunity Detail Page | Full detail: description, who it's for, eligibility, benefits, documents, how to apply, dates, official source, last verified date, save button |
| 13 | Admin Panel | Add/edit/delete/archive opportunities, update eligibility, set verification date, categorize, activate/deactivate, review reports |

### Explicitly Out of Scope for MVP
Full government application submission, direct government API integration (unless trivially available), automatic document verification, Aadhaar-based auth, financial transactions, complete national scheme coverage, autonomous scraping, AI chatbot as primary interface, advanced ML ranking models, automatic job placement, guaranteed outcomes, complex social features.

---

## 4. Trust & Safety Requirements

- Always show official sources and last-verified date.
- Never claim guaranteed eligibility.
- Clearly label AI-generated explanations vs. official criteria vs. preliminary match.
- Never fabricate schemes, benefits, deadlines, or eligibility criteria.
- Provide a "report incorrect information" path.
- Minimize collection of sensitive personal information; protect profile data; role-based admin access.
- Core disclaimer must appear wherever recommendations are shown:
  > "Recommendations shown by this platform are preliminary matches based on the information provided by the user. Final eligibility is determined by the respective government department or official program authority. Always verify current requirements through the official source before applying."

---

## 5. Success Metrics

**Primary:** profile completion rate, recommendation click-through rate, save rate, detail-page engagement, roadmap generation rate, scheme-match engagement, search-to-opportunity interaction rate.

**Quality:** % recommendations judged relevant, % scheme records recently verified, user-reported incorrect info, AI explanation error rate, recommendation feedback rate.

**North Star:** % of users who discover at least one opportunity they consider relevant and useful.

---

## 6. MVP Acceptance Criteria

- User can create an account and complete a profile.
- Student can enter a career goal and receive a generated roadmap.
- System can identify basic skill gaps.
- Citizen receives scheme recommendations based on profile data.
- User can see *why* a recommendation was shown.
- User can open the official source and save opportunities.
- Admin can create/update opportunity records with verification/source info.
- Preliminary matching is always visually distinct from official eligibility.
- Fully responsive on desktop and mobile.

---

## 7. Open Questions (resolve before full PRD sign-off)

- Students, citizens, or both as first-priority audience?
- Which states launch first — central-only or state schemes too?
- Who verifies scheme data, and how often is it re-verified?
- Which AI provider/model, and what data is allowed to reach it?
- Which profile fields are essential vs. sensitive, and what's the retention policy?
- Long-term sustainability / partner model (institutions, NGOs, government bodies)?