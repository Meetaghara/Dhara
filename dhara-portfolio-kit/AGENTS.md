# AGENTS.md — Dhara Patadia portfolio

These rules apply to every task in this workspace.

## Sources of truth (in this order)
1. `reference/layout-mockup.png` — the finalised design. Match its layout, colours and copy.
2. `reference/Dhara_Patadia_CV.pdf` — the only source of facts, numbers, dates and titles.
3. `docs/PORTFOLIO_BRIEF.md` — full spec: tokens, components, sections, motion, acceptance checks. Read the relevant section before changing anything.

## Content
- Never invent facts, metrics, testimonials, clients, logos, links or screenshots. Missing info → `TODO(Dhara): …` and mention it in the walkthrough.
- All copy lives in `src/content/site.ts` and `src/content/case-studies.ts`; components never hard-code copy.
- Never publish Dhara's phone number. Amazon content stays at CV-level detail; diagrams are conceptual only.

## Design
- Use only the CSS variables from brief §3. No ad-hoc hex values, no new colours, no new fonts.
- Fonts: Bricolage Grotesque (display) and DM Sans (body) via `next/font`.
- Bordered cards = 2px `--ink` border + hard offset shadow; soft cards = no hard border.

## Motion
- Stack: GSAP + ScrollTrigger + `@gsap/react` (`useGSAP`) + Lenis. No other animation libraries.
- Animate `transform`/`opacity` only (accordion may use `grid-template-rows`).
- Everything inside `gsap.matchMedia()` with a reduced-motion branch; pointer effects only for `(hover: hover) and (pointer: fine)`.
- Content must be visible without JavaScript. Clean up all tweens, triggers and listeners on unmount.

## Quality bar
- Accessibility: semantic landmarks, visible focus (2px `--terracotta-strong`, offset 3px), keyboard-operable accordion and drawer, contrast ≥ 4.5:1.
- Lighthouse mobile: Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95, SEO 100.
- No new dependencies beyond brief §2 without asking first.

## Verification
- Before calling any phase done: run the site, screenshot at 1440px and 390px wide, compare with `reference/layout-mockup.png`, and include the screenshots in the walkthrough.
- `npm run build` must pass with no console errors.
