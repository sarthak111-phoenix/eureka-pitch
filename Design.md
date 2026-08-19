# Design.md — Visual & Interaction Design Direction

> This is a high-trust, government-adjacent product (see `PRD.md` §4 Trust & Safety) aimed at students and citizens across India, many on mobile. Design must read as **credible and calm**, not flashy for its own sake — even though the theme is dark.

---

## 1. Theme Direction

**Baseline decision: dark theme. Not white/light as the default.**

Two directions are still open — pick one and lock it in before Phase 0 finishes, rather than mixing both:

### Option A — Dark Metallic (shine/gloss)
- Near-black base (`#0B0D10`–`#121417`) with metallic-gradient accents (subtle silver/steel gradients on cards, borders, buttons).
- Highlights use soft specular gradients (light-to-dark diagonal sheens) rather than flat color — think brushed steel / gunmetal.
- Best if the product wants to feel premium/"fintech-grade trustworthy."
- Risk: gradients + shine can read as gimmicky or hurt readability if overused — reserve shine for accents (buttons, badges, active states), never for body backgrounds or long text blocks.

### Option B — Dark Flat Palette
- Near-black/charcoal base with a restrained accent palette (2–3 accent colors max) — e.g. deep teal/emerald or indigo/amber as primary accents, keeping most surfaces flat charcoal/slate.
- Best if the product wants to feel calm, government-adjacent, and highly readable at scale.
- Lower visual risk, easier accessibility/contrast management.

**Recommendation for a trust-first product with lots of dense information (schemes, eligibility text, tables):** lean toward Option B as the base, with Option A's metallic shine reserved as a *micro-accent* (primary CTA button, active nav item, save/bookmark icon) rather than the whole UI. This keeps eligibility/scheme text highly legible while still giving the product a distinct, premium feel.

### Core Palette (starting point — adjust once a direction is chosen)
| Token | Purpose | Example |
|---|---|---|
| `--bg-base` | Page background | `#0B0D10` |
| `--bg-surface` | Card/panel background | `#15171B` |
| `--bg-surface-raised` | Modal/dropdown | `#1C1F24` |
| `--border-subtle` | Card borders, dividers | `#2A2D33` |
| `--text-primary` | Body/headings | `#F2F3F5` |
| `--text-secondary` | Muted text, meta info | `#9CA3AF` |
| `--accent-primary` | Primary CTA, links, active states | project-specific (teal/indigo/amber — decide) |
| `--accent-metallic` | Reserved shine accent (buttons/badges only) | steel/silver gradient |
| `--success` | Verified/matched state | `#34D399` (muted) |
| `--warning` | Preliminary-match / disclaimer banners | `#FBBF24` (muted) |
| `--danger` | Errors, flagged activity | `#F87171` (muted) |

Do not use pure white (`#FFFFFF`) surfaces anywhere. Text-on-dark should also avoid pure white for body copy — off-white (`#F2F3F5` or similar) is easier on the eyes at length.

---

## 2. Typography

- **Headings:** a clean geometric or grotesque sans-serif with good weight range (e.g. Inter, Sora, or Space Grotesk) — pick one that supports Devanagari/regional scripts reasonably well if multi-language UI is planned later (see `PRD.md` "preferred language" field).
- **Body:** Inter or similar highly-legible UI sans-serif — this product has dense eligibility/scheme text, so legibility beats personality here.
- **Numeric/data (deadlines, scores, verification dates):** tabular-nums variant if available, for alignment in lists/tables.
- Type scale: keep it restrained — 5–6 sizes total (e.g. 12/14/16/20/24/32px), consistent line-height (1.5 for body, 1.2 for headings).
- High contrast is mandatory against the dark background — verify WCAG AA at minimum for body text, especially for eligibility/legal text (see `PRD.md` Trust & Safety and Accessibility requirements).

---

## 3. Fonts, Animation & Scrolling — Direction (Figma/Stitch exploration)

You mentioned still exploring animation/scroll styles via Figma or Stitch — that's fine to leave open, but scope it deliberately rather than open-ended:

- **Prototype 2–3 candidate directions** in Figma/Stitch before committing in code: e.g. (1) subtle scroll-reveal + metallic-accent hero, (2) flat dark with parallax-lite scroll sections, (3) minimal/no-animation, fast-loading utilitarian dark UI.
- **Constraint to keep in mind while exploring:** this is an information-dense, mobile-first, trust-critical app — animation should support scanning and hierarchy (fade/slide-in for cards as they enter viewport, smooth section transitions on landing/about pages), not distract from eligibility text or slow down perceived performance.
- Once a direction is chosen, document the final choice here (specific fonts, animation library if any, easing/duration tokens) so it becomes a fixed reference for `Rules.md` ("what to use").
- Landing/marketing pages (public, low information density) are where more expressive scroll/animation belongs. Authenticated app pages (dashboard, opportunity lists, scheme detail) should stay closer to Option 3 — minimal, fast, skeleton-loading-first (see `Security.md` §4) — because users are scanning dense, important information, not being marketed to.

**Open item to close out before Phase 0 ends:** final font pair, final palette (A/B/hybrid decision above), and animation library choice (if any) — record the decision directly in this file once made, and reference it from `Memory.md` as a completed decision.

---

## 4. Component Notes

- **Cards** (dashboard "For You," "Your Career," "Government Benefits," "Saved" sections per `PRD.md`): use `--bg-surface` with `--border-subtle`, subtle elevation via shadow rather than heavy borders.
- **Disclaimer/eligibility banners:** always visually distinct — a bordered callout using `--warning`, never blended into normal card styling, since these carry legal/trust weight.
- **AI-generated text blocks:** a consistent visual marker (icon + label, e.g. "AI explanation") distinct from official/structured data blocks — this is a trust requirement, not just a style choice.
- **Skeleton loaders** (see `Security.md`): should match the shape of the real content (card skeletons, list-row skeletons) using a subtle shimmer in `--bg-surface-raised` tones — never a bright/white shimmer on the dark theme.