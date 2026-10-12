# Dhara Patadia — Portfolio Build Brief

> **For the Antigravity agent.** Read this whole file and `AGENTS.md` before planning.
> Open `reference/layout-mockup.png` and keep it beside you for every section.

---

## 0. Inputs and source of truth

| Priority | Source | Use it for |
|---|---|---|
| 1 | `reference/layout-mockup.png` | The finalised design: section order, layout, colours, card colours, icons, copy. Match it. |
| 2 | `reference/Dhara_Patadia_CV.pdf` | Every fact, number, date and title. If the mockup and CV disagree on a fact, the CV wins (see §6.12). |
| 3 | This brief | Tokens, components, behaviour, motion, structure, acceptance checks. |
| Motion reference | https://www.aahil-khan.xyz | Recreate the *behaviour* in §8 (values were measured from the live site). Do **not** copy its code, content, yellow palette, Space Grotesk font, `/canvas` playground, games, custom cursor or easter eggs. |
| Layout references (already baked into the mockup) | https://www.baghel.dev, https://gazimdanas.netlify.app | Context only: clean single-page storytelling, warm cream background, short punchy section headings. Do not re-derive the layout from them. |

**Never invent** facts, metrics, clients, testimonials, logos, links or screenshots. If something is missing, put `TODO(Dhara): …` in the content file and list it in your walkthrough.

---

## 1. Goal

A recruiter-facing, single-page portfolio plus three case-study pages that position Dhara for **AI Product Owner, AI Product Manager and AI Adoption roles** (hybrid, London). Tone: warm, editorial, confident. Motion: polished and calm — a product person's site, not a developer playground.

---

## 2. Tech stack

- **Next.js** (latest stable), App Router, TypeScript, static export (`output: 'export'`).
- **Tailwind CSS v4**; all design tokens as CSS variables (§3) mapped in `@theme`.
- **gsap** + **ScrollTrigger** + **@gsap/react** (`useGSAP`) and **lenis** for smooth scroll — the same stack the motion reference uses.
- **next/font/google**: Bricolage Grotesque (display) and DM Sans (body).
- **lucide-react** for UI icons; small inline SVGs for doodles and the LinkedIn "in" mark.
- No other UI or animation libraries (no framer-motion, AOS, shadcn, three.js) unless Dhara asks.
- npm. Deploy target: Vercel (static).

---

## 3. Design tokens (sampled from the mockup)

```css
:root {
  /* Surfaces */
  --cream:            #FCF9F2;  /* page background */
  --cream-2:          #FAF7EF;  /* tools bar, chips, outline buttons */
  --blush:            #F8DFD6;  /* blush cards */
  --blush-light:      #FCEFE6;  /* education cards */
  --portrait:         #FBE5D9;  /* portrait card */
  --sage:             #EBEEE0;  /* sage cards, open accordion row */
  --sand:             #F1E7DA;  /* sand cards */
  --white:            #FFFFFF;

  /* Ink (olive-charcoal) */
  --ink:              #343C2D;  /* headings, borders, hard shadows */
  --ink-deep:         #2B3125;  /* hero subtitle, strongest text */
  --ink-soft:         #4F5349;  /* eyebrows, small labels */
  --muted:            #65665F;  /* body copy — mockup grey #868782 darkened to pass WCAG AA on every surface */
  --line:             #5F5F58;  /* hairlines, footer rule */

  /* Terracotta family */
  --terracotta:       #B8766A;  /* brand accent: tints, borders, highlights */
  --terracotta-strong:#A9604F;  /* primary button fill — white text 4.7:1 (mockup #B8766A is 3.6:1) */
  --terracotta-light: #C98478;  /* DP logo badge */
  --coral:            #EEB0A2;  /* blob behind portrait */
  --rust:             #94503F;  /* icons, DP monogram, doodles */

  /* Shape */
  --bw: 2px;
  --radius-card: 14px;
  --radius-btn: 10px;
  --radius-pill: 999px;
  --shadow-hard:    3px 4px 0 0 var(--ink);
  --shadow-hard-lg: 5px 7px 0 0 var(--ink);

  /* Motion */
  --ease-out: cubic-bezier(.16, 1, .3, 1);

  /* Layout */
  --container: 1200px;
  --gutter: clamp(1.25rem, 5vw, 3rem);
  --section-space: clamp(5rem, 10vw, 8rem);
  --header-h: 72px;
}
::selection { background: color-mix(in srgb, var(--terracotta) 28%, transparent); }
```

**Typography**

| Role | Font | Weight | Size | Notes |
|---|---|---|---|---|
| Hero name | Bricolage Grotesque | 800 | `clamp(3.5rem, 9vw, 7.5rem)` | line-height .92, letter-spacing −0.035em, colour `--ink` |
| Section heading (h2) | Bricolage Grotesque | 800 | `clamp(2.25rem, 4.6vw, 3.75rem)` | line-height 1.0, letter-spacing −0.03em |
| Card title / stat number | Bricolage Grotesque | 700 | 1.25rem / `clamp(1.75rem, 2.6vw, 2.25rem)` | |
| Hero subtitle | DM Sans | 700 | `clamp(1.25rem, 2vw, 1.6rem)` | `--ink-deep` |
| Body | DM Sans | 400 | 1.0625rem, line-height 1.65 | `--muted` |
| Eyebrow | DM Sans | 600 | .75rem, uppercase, letter-spacing .18em | `--ink-soft` |
| Small meta / dates | DM Sans | 500 | .875rem | `--muted` |

Measure proportions against the mockup (it is a scaled-down desktop render) and tune within these ranges.

**Card rules**
- *Bordered card*: `--bw` solid `--ink`, `--radius-card`, `--shadow-hard`, padding `clamp(1.25rem, 2vw, 1.75rem)`. Used for stats, expertise, work cards, tools bar, experience list.
- *Soft card*: no hard border/shadow, same radius. Used for the About "Writing, community and curiosity" card (blush) and education cards (blush-light with a 1.5px `--ink`/15% border).

---

## 4. Layout

- Centered container `max-width: var(--container)`, side padding `var(--gutter)`.
- Vertical rhythm between sections: `var(--section-space)`.
- Each section: eyebrow → h2 → content, left-aligned, as in the mockup.
- Section ids for nav: `#about`, `#expertise`, `#work`, `#experience`, `#contact`.

---

## 5. Shared components

`Button` (primary / outline, optional leading icon, optional trailing arrow), `Card` (bordered / soft, `tone: blush | sage | sand | cream`), `Chip` (pill tag), `Eyebrow`, `SectionHeading` (splits lines/words into masked spans for §8 reveals), `IconTile`, `MonogramBadge` ("DP"), `Doodle` (inline SVG: `sparkle` = three short radiating strokes; `squiggle` = loose spring/coil), `Accordion`, `BackToTop`.

Icons (lucide-react, stroke 2–2.25): MapPin, ArrowRight, FileText, Rocket, Layers, Database, GraduationCap, Target, Settings, BarChart3, Users, Wrench, Mail, ChevronDown, Crown, MessageSquareText, User, Image, Video, ArrowUp.

---

## 6. Sections — layout and exact copy

### 6.1 Header (sticky)
- Left: `MonogramBadge` "DP" (terracotta-light rounded square, letters in `--ink`/rust) + "Dhara Patadia" (DM Sans 700).
- Center/right: links **About · Expertise · Work · Experience · Contact**.
- Right: primary small button **"View CV →"** → `/Dhara_Patadia_CV.pdf` (new tab).
- Mobile (<900px): links collapse into a burger → right-side drawer with scrim; Esc closes; focus moves into the drawer and returns to the burger.

### 6.2 Hero (two columns ≈ 58 / 42)
Left:
- Location line: MapPin (rust) + **"Cambridge, UK"**
- Name (two lines): **"Dhara"** / **"Patadia"**
- Subtitle: **"AI Product Ownership & Operations"**
- Tagline: **"Turning complex AI ideas into products people can use."**
- Buttons: primary **"Explore my work →"** (scrolls to `#work`) · outline **"View CV"** with FileText icon (opens the PDF).

Right:
- Portrait card ≈ 4:4.5, rotated −2deg, 3px `--ink` border, `--shadow-hard-lg`, background `--portrait` with two softer organic shapes inside, large **"DP"** monogram in `--rust` (Bricolage 800).
- Behind it: a `--coral` organic blob, offset left-bottom and right-top as in the mockup.
- Doodles: `sparkle` top-left of the card (rust), `squiggle` bottom-right (terracotta).
- If `public/portrait.jpg` exists, show the photo inside the card instead of the monogram. **Do not render the words "Portrait placeholder"** — that was a design note.

### 6.3 Stats row (4 equal bordered cards)
| Card tone | Icon | Value | Label |
|---|---|---|---|
| sand | Rocket (rust) | **12 weeks** | MLense production launch |
| sage | Layers (ink) | **7** | ML task types |
| blush | Database (ink) | **~5,000** | Data points reviewed monthly |
| sand | GraduationCap (ink) | **Distinction** | MSc Big Data Science |

### 6.4 About (`#about`) — two columns
Left: eyebrow **ABOUT**, h2 **"From product vision to reliable AI."**, paragraph:
> I work across AI product delivery, data quality and adoption. My experience spans launching MLense at Data Pixel and supporting multimodal machine-learning data at Amazon.

Right: soft blush card with `sparkle` doodle top-right:
- Title **"Writing, community and curiosity."**
- Text: *Outside of work, I enjoy exploring ideas through writing, being part of community initiatives and staying curious. It helps me bring a broader perspective to the work I do.*
- Pills: **Author · Tennis · Baking**

### 6.5 Expertise & tools (`#expertise`)
Eyebrow **EXPERTISE & TOOLS**, h2 **"What I bring to the table."**

Four bordered cards (icon in rust, title, three short lines):

| Tone | Icon | Title | Lines |
|---|---|---|---|
| blush | Target | Product Ownership | Define product strategy · Prioritise roadmaps · Drive delivery and adoption |
| sage | Settings | AI Operations | Support data quality · Manage annotation workflows · Enable scalable operations |
| sand | BarChart3 | Process Improvement | Identify bottlenecks · Streamline workflows · Build practical solutions |
| sage | Users | Stakeholder Collaboration | Work across technical and non-technical teams · Align on goals and outcomes · Support clear communication |

Tools bar (full-width bordered card, cream-2): Wrench icon + **"Tools I work with:"** + chips **Notion · Asana · Excel · PowerPoint · Slack · Teams · ChatGPT · Claude** (PowerPoint added from CV, §6.12).

### 6.6 Selected work (`#work`)
Eyebrow **SELECTED WORK**, h2 **"Decisions. Delivery. Impact."**, right-aligned link **"View all work →"** → `/work`.

Bento grid: left large card (01) spanning two rows; right column stacks 02 and 03. Each card: small number + short dash rule, title, subtitle, meta, button, illustration.

| # | Tone | Title | Subtitle | Meta | Button | Illustration |
|---|---|---|---|---|---|---|
| 01 | blush | **MLense** | No-code AutoML product launch | Roadmap to production in 12 weeks. | primary "View case study →" → `/work/mlense` | Browser-window mock built in HTML/CSS: traffic-light dots, "MLense" title bar, left nav, headline **"Build. Train. Deploy."**, sub-line "Turn your data into real-world AI models, without the complexity.", small bar chart, row of four icon tiles. Label **"Concept illustration"** (keep — it is honest). |
| 02 | sage | **Amazon** | Improving how teams share resolved issues | Proposal under review. 4 teams mapped. | outline "View case study →" → `/work/amazon-guidance` | Conceptual cluster: message tiles (MessageSquareText) in blush/terracotta with two person avatars. No Amazon UI or internal tools. |
| 03 | sand | **MSc Research** | Multimodal content prediction | 500 videos, 79% lower average error vs metadata-only baseline. | outline "View case study →" → `/work/msc-research` | Flow: three input tiles (Image, text/FileText, Video) → arrows → node → bar chart. Tag **"Academic study"**. |

### 6.7 Experience & education (`#experience`)
Eyebrow **EXPERIENCE & EDUCATION**, h2 **"The journey so far."** Two columns ≈ 64 / 36 with a thin vertical divider.

**Work experience** — bordered accordion list; one row open at a time; Amazon open by default; open row background `--sage`.

| Logo tile | Company | Role | Dates | Expanded text |
|---|---|---|---|---|
| "A" | **Amazon** | Senior ML Data Associate II | Nov 2025 – Present | Monthly multimodal annotation and quality review, supporting machine-learning models with high-quality data across audio, image, video and text. Work with cross-functional teams to resolve edge cases and improve data quality at scale. |
| "DP" | **Data Pixel** | AI Product Lead | Nov 2024 – Nov 2025 | Owned MLense, a no-code AutoML platform covering 7 machine-learning task types. Led 2 developers and a 5-person marketing agency through 6 two-week sprints, taking the product from proof of concept to production launch on Google Cloud in 12 weeks. |
| "S" | **Satv-Script** | Business Analyst Intern | Mar 2023 – Apr 2023 | Mapped use cases for a client's existing product and turned the findings into written summaries and presentations for the client team. |

Logo tiles: neutral monogram tiles in the palette by default. Add a `logo?: string` field so official logos can be dropped in later if Dhara chooses.

**Education** — two soft blush-light cards:
- Crown icon · **Queen Mary University of London** · MSc Big Data Science · Distinction
- GraduationCap icon · **Atmiya University** · B.Tech Computer Engineering · First Class with Distinction

### 6.8 Contact (`#contact`)
Left: eyebrow **CONTACT**, h2 **"Let's talk about better AI products."** with a `sparkle` doodle to its right.
Right: buttons — primary (Mail icon) **patadiadhara79@gmail.com** → `mailto:` · outline **LinkedIn** → `https://www.linkedin.com/in/dhara-patadia-617947188` (new tab) · outline **View CV** (FileText). Below: MapPin + **"Cambridge, UK"**.
**Do not publish the phone number anywhere on the site.**

### 6.9 Footer
Hairline rule (`--line`). Left **"Dhara Patadia"**; right **"AI Product Ownership & Operations · Cambridge, UK"**.

### 6.10 Back-to-top button
Round 48px button, bordered, `--shadow-hard`, fixed bottom-right; appears after 900px of scroll (§8.7).

### 6.11 Mobile order
Hero: location → name → subtitle → tagline → buttons → portrait (centred, ~75% width) → stats as 2×2. About and contact stack. Expertise 1 col (<640) / 2 col (640–1024) / 4 col. Work bento becomes one column (01, 02, 03). Experience above education.

### 6.12 Copy corrections applied from the CV
These keep the finalised layout but make the facts match the CV:
1. Tools: add **PowerPoint** (it is on the CV).
2. Amazon description: include **audio** (CV: audio, video, image and text).
3. Atmiya: add **First Class with Distinction**.

---

## 7. Case-study pages

Routes: `/work` (index of the three cards), `/work/mlense`, `/work/amazon-guidance`, `/work/msc-research`.

Shared template: back link "← Back to work" (returns to `/#work`), number + title + one-line summary, meta row (Role · Timeline · Team · Status), then sections. Same tokens and card styles as the home page. Use only the facts below.

**MLense — No-code AutoML product launch** (Data Pixel Ltd, AI Product Lead, Nov 2024 – Nov 2025; team: 2 developers + 5-person marketing agency; status: launched v1.0 on Google Cloud)
- *Context:* UK start-up building no-code AI tools for business users. MLense lets business analysts and domain experts train and explain models in the browser without coding; 7 ML task types.
- *Problem:* slow model building, seen first-hand.
- *Decisions and delivery:* roadmap built around that problem; working MVP in month one; 6 two-week sprints over 12 weeks, each with agreed scope and acceptance criteria; proof of concept (v0.1) → production launch (v1.0).
- *What made it different:* Gemini-powered recommendations that pre-build a model pipeline from an uploaded dataset; an assistant that explains results in plain English.
- *Self-serve:* 4 pipeline templates (beginner → expert), in-platform documentation portal, in-app feedback centre for bugs and feature requests, admin dashboard to review them.
- *Quality:* launched with 70 automated tests passing and 6 security issues fixed; client demos; every change request logged in writing.
- *Go-to-market:* 3 target segments led by business analysts and domain experts without coding skills; 5-phase plan from LinkedIn content to performance partners; Pro and Team pricing planned at US$29 and US$99 a month after a free launch.
- Visual: 12-week sprint timeline (6 bars, v0.1 → v1.0 markers) — animated on scroll.

**Amazon — Improving how teams share resolved issues** (Senior ML Data Associate II, Amazon, Cambridge, UK, Nov 2025 – present; status: proposal under review)
- *Problem spotted:* resolved SIM (issue-resolution) tickets took too long to reach the convention pages teams work from.
- *Proposal:* link each SIM to its task on the convention page so every team works from one current source of guidance.
- *Change plan:* impact and benefits mapped for 4 teams; 6-step implementation plan; pilot on 1 live workflow before full rollout; a low-build alternative.
- *Feedback loop for managers:* with SIMs visible during L1 audits, tasks that cause repeated overturns show up early and can be fixed for the whole team in a huddle.
- *Expected impact:* estimated 80–85% gain in how well guidance is shared — always label it **estimated** and the proposal **under review**.
- Visuals must be conceptual diagrams only — no internal screenshots, tool names beyond the CV, or data.

**MSc Research — Multimodal content prediction** (Queen Mary University of London, MSc Big Data Science, Distinction, Sep 2023 – Sep 2024)
- *Dissertation:* Multimodal AI for Predicting the Success of Content Creation.
- *Approach:* predicted YouTube view counts from metadata and LLM-scored transcripts across 500 hand-collected videos; designed an 11-question LLM scoring framework (clarity, engagement, credibility and more) that turned transcripts into model features.
- *Comparison:* 3 approaches — metadata-only baseline, visual-only deep learning model, LLM-scored model. The LLM-scored model beat both.
- *Result:* 79% lower average error than the metadata-only baseline.
- Chart rule: you may show baseline = 100 vs LLM-scored = 21 (relative error). Do not plot the visual-only model with a made-up number.

---

## 8. Motion system (recreated from aahil-khan.xyz)

### 8.1 Principles
- Animate `transform` and `opacity` only (exception: accordion uses `grid-template-rows`).
- One easing language: GSAP `power3.out` for timelines; CSS `var(--ease-out)` = `cubic-bezier(.16,1,.3,1)` for hovers; hover durations .14–.38s.
- Everything lives inside `gsap.matchMedia()` with a reduced-motion branch; pointer effects only on `(hover: hover) and (pointer: fine)`.
- Content is fully visible without JavaScript.

### 8.2 Smooth scroll (Lenis)
- `new Lenis({ duration: 1.05, smoothWheel: true })`.
- `lenis.on('scroll', ScrollTrigger.update)`; drive Lenis from the GSAP ticker: `gsap.ticker.add(t => lenis.raf(t * 1000))`; `gsap.ticker.lagSmoothing(0)`.
- Not created when reduced motion is preferred (native scroll instead). Destroy on unmount.
- After `document.fonts.ready`, call `ScrollTrigger.refresh()`.

### 8.3 Navigation behaviour
- **In-page anchors:** intercept `a[href^="#"]` and call `lenis.scrollTo(target, { offset: -(headerHeight + 12) })`.
- **Arrival flash:** on arrival, set `data-flash` on the target section's eyebrow → a terracotta highlight sweeps behind the eyebrow and fades (≈1s, plays twice); remove the attribute after 2.1s.
- **Sticky header:** set `data-stuck="1"` when `scrollY > 24` → 2px `--ink` bottom border + `color-mix(in srgb, var(--cream) 96%, transparent)` background with a light backdrop blur.
- **Active link:** ScrollTrigger per section (`start: 'top 55%'`, `end: 'bottom 55%'`, `onEnter`/`onEnterBack`) → the matching nav link gets `data-on="1"` (ink at 7% pill background, ink text).

### 8.4 Hero intro timeline
`gsap.timeline({ defaults: { ease: 'power3.out' } })`, positions in seconds:

| At | Target | From | Duration | Stagger |
|---|---|---|---|---|
| 0 | Hero background layers (grid + glow) | `autoAlpha: 0` | 1.1 | — |
| 0.10 | Location line | `y: 14, autoAlpha: 0` | 0.6 | — |
| 0.15 | Name lines — each line in an `overflow:hidden` wrapper | `yPercent: 108` | 0.95 | 0.08 |
| 0.50 | Subtitle + tagline words — each word in an `overflow:hidden` wrapper | `yPercent: 105` | 0.7 | 0.035 |
| 0.55 | Portrait card | `y: 24, rotate: -6, autoAlpha: 0` → rest at −2deg | 0.9 | — |
| 0.60 | Coral blob | `scale: 0.85, autoAlpha: 0` | 1.0 | — |
| 0.80 | Doodles | stroke draw-in (`strokeDashoffset`) | 0.8 | 0.1 |
| 0.95 | Hero buttons | `y: 18, autoAlpha: 0` | 0.6 | 0.06 |
| 1.05 | Stats cards | `y: 18, autoAlpha: 0` | 0.6 | 0.06 |
| 1.10 | Stat numbers count up (12, 7, 5,000 with `en-GB` separators; keep "~" and " weeks" static) | from 0 | 1.1, `power2.out` | — |

"Distinction" does not count — it uses the masked word reveal.

**No flash before the intro:** an inline `<head>` script adds `html.js-motion` only when reduced motion is not preferred; CSS hides `[data-intro]` under that class; the timeline's `autoAlpha` reveals them. Safety timeout: remove the class after 2.5s if GSAP hasn't started.

### 8.5 Hero ambient layer (fine pointer only)
- **Cursor glow:** absolutely positioned layer with `radial-gradient(circle 240px at var(--sx) var(--sy), color-mix(in srgb, var(--terracotta) 22%, transparent) 0%, color-mix(in srgb, var(--terracotta) 7%, transparent) 50%, transparent 76%)`. Fades in (opacity .5s) while the pointer is inside the hero. Position is eased every frame: `s += (target - s) * 0.11`.
- **Faint grid:** 88px line grid in `--ink` at ~4% opacity, masked to fade out by 55% of the hero height; drifts slowly and parallaxes opposite the pointer (max ~46px x, 30px y). On touch devices it loops `x: 88, y: 88` over 9s linearly. If the grid visibly changes the look of the mockup at rest, remove it and keep only the glow.
- **Blob breathing:** `scale: 1.06`, 13s, `sine.inOut`, `yoyo`, `repeat: -1`.
- **Portrait tilt:** card follows the pointer with max 6deg rotateX/rotateY (perspective 900px), eased with the same 0.11 lerp, returning to −2deg.
- **Hero scroll-out:** hero content `yPercent: -12, autoAlpha: 0.25`, `ease: 'none'`, ScrollTrigger `start: 'top top'`, `end: 'bottom top'`, `scrub: true`.

### 8.6 Scroll reveals
- Elements with `.reveal`: `gsap.set('.reveal', { y: 28, autoAlpha: 0 })`, then `ScrollTrigger.batch('.reveal', { start: 'top 88%', once: true, onEnter: els => gsap.to(els, { y: 0, autoAlpha: 1, duration: 0.7, ease: 'power3.out', stagger: 0.09, overwrite: true }) })`.
- Apply to: eyebrows, paragraphs, every card, the tools bar, the accordion, education cards, contact buttons.
- **Section headings** reuse the hero's masked line reveal (`yPercent: 105`, 0.8s, stagger .08) when they reach `top 85%`, once.
- **Tool chips:** `y: 12, autoAlpha: 0`, 0.45s, stagger .025, when the tools bar reaches `top 70%`, once.

### 8.7 Section specifics
- **Work illustrations** (once, when each card enters): MLense mock bars grow `scaleY` from 0 (origin bottom, stagger .06) and icon tiles fade up; Amazon message tiles pop `scale: .8 → 1` with `autoAlpha` (stagger .08); MSc arrows draw in, then bars grow.
- **Experience accordion:** CSS `grid-template-rows: 0fr → 1fr` (.45s `var(--ease-out)`), chevron rotates 180deg (.3s), row background fades to `--sage` (.26s). Buttons with `aria-expanded` / `aria-controls`.
- **Contact background:** soft radial field (blush + coral, low opacity) moves `yPercent: 14` with `scrub` from `top bottom` to `bottom top`.
- **Back-to-top:** set `data-far="1"` on the page wrapper when `scrollY > 900`; button goes `opacity 0 → 1`, `translateY(10px) → 0`, .26s; clicking uses `lenis.scrollTo(0)`.
- **Case-study pages:** heading uses the masked line reveal on load; body uses `.reveal`; the MLense sprint timeline bars grow left-to-right (stagger .08) when in view.

### 8.8 Hover and focus (wrap in `@media (hover: hover) and (pointer: fine)`; mirror on `:focus-visible`)

| Element | Hover |
|---|---|
| Bordered cards | `translate(-2px, -3px)`, shadow `--shadow-hard` → `--shadow-hard-lg`, .26s |
| Work cards | as above + illustration nudges `translateY(-4px)` |
| Primary button | lift `translate(-1px, -2px)` + larger hard shadow; an `--ink` fill wipes in from the left (`scaleX 0 → 1`, origin left, .38s) under the label; label stays light; trailing arrow `translateX(3px)` |
| Outline button | `translate(-1px, -1px)` + `3px 3px 0 0 var(--ink)` shadow appears; border stays ink |
| Header links | ink 7% pill background, ink text |
| Chips / pills | `translateY(-2px)`, border `--terracotta`, background terracotta 12% over cream, .2s |
| "View all work →" | 2px underline grows into a full blush highlight (`background-size: 100% 2px → 100% 100%`), arrow nudges right |
| Accordion row | ink 5% background, chevron nudges down 2px |
| Education cards | `translate(-1px, -2px)` with a soft shadow |
| DP badge | `rotate(-6deg) scale(1.05)` |

Focus ring everywhere: 2px solid `--terracotta-strong`, offset 3px.

### 8.9 Reduced motion and fallbacks
- `gsap.matchMedia().add({ motion: '(prefers-reduced-motion: no-preference)', reduce: '(prefers-reduced-motion: reduce)' }, …)` — all tweens, ScrollTriggers and Lenis only in the `motion` branch.
- CSS: under `prefers-reduced-motion: reduce`, transitions and animations take .01ms.
- Clean up every tween, ScrollTrigger and listener on unmount (`useGSAP` with a scope ref).

### 8.10 Do not
Custom cursor replacement, canvas/WebGL, scroll-jacking or long pinned sections, horizontal scroll sections, parallax on body text, continuous loops other than the slow ambient ones, preloader screens.

---

## 9. Responsive
Breakpoints 640 / 900 / 1024 / 1280. Check at 1440, 1280, 1024, 768, 390 and 360 wide: no horizontal scroll, tap targets ≥ 44px, name never overflows.

## 10. Accessibility
Semantic landmarks (`header`, `nav`, `main`, `section` with headings, `footer`), skip link, one `h1` (the name), logical heading order, visible focus, accordion and drawer fully keyboard-operable, decorative SVGs `aria-hidden`, meaningful `alt` text, text contrast ≥ 4.5:1 (tokens above already pass), `lang="en-GB"`.

## 11. SEO, metadata, performance
- Title: **Dhara Patadia — AI Product Ownership & Operations**. Description: *AI product lead in Cambridge, UK, turning complex AI ideas into products people can use: MLense launch, AI operations at Amazon, MSc Big Data Science (Distinction).*
- Open Graph image generated with `opengraph-image.tsx` in the palette (name, subtitle, DP monogram). Favicon/icon: DP badge.
- JSON-LD `Person` (name, jobTitle, address Cambridge UK, sameAs LinkedIn, alumniOf both universities).
- `next/font` with `display: swap`; images through `next/image` (unoptimized for static export) with explicit sizes; GSAP only in client components.
- Targets (Lighthouse mobile): Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95, SEO 100. No layout shift from the intro.

## 12. Project structure
```
AGENTS.md
docs/PORTFOLIO_BRIEF.md
reference/layout-mockup.png
reference/Dhara_Patadia_CV.pdf
public/Dhara_Patadia_CV.pdf
public/portrait.jpg              (optional, added later)
src/app/layout.tsx  page.tsx  work/page.tsx  work/[slug]/page.tsx  opengraph-image.tsx  icon.tsx
src/components/ui/…              (Button, Card, Chip, Eyebrow, SectionHeading, IconTile, Doodle, Accordion)
src/components/sections/…        (Header, Hero, Stats, About, Expertise, Work, Journey, Contact, Footer, BackToTop)
src/components/motion/…          (SmoothScroll provider, useHeroIntro, useReveals, usePointerGlow)
src/content/site.ts              (ALL home-page copy and facts)
src/content/case-studies.ts      (case-study content from §7)
src/app/globals.css              (tokens from §3 + base styles)
```
All copy lives in `src/content/*` so text edits never touch components.

## 13. Build phases and acceptance checks
Work phase by phase. At the end of each phase: run the site, take screenshots at **1440px** and **390px** wide, compare against the mockup in your walkthrough, then continue.

1. **Scaffold** — Next.js + Tailwind v4 + fonts + tokens + content files + shared UI components.
2. **Static desktop build** — every section in §6 with exact copy, no motion. Side-by-side compare with the mockup: same order, copy, card colours, icons, proportions.
3. **Responsive** — §6.11 and §9, including the mobile drawer.
4. **Motion** — §8 in full, then test with reduced motion on.
5. **Case studies** — `/work` and the three pages from §7.
6. **Polish** — SEO/OG/icon/JSON-LD, accessibility and Lighthouse pass, `npm run build` static export, README with run and deploy steps.

Done means: matches the mockup at 1440px; no horizontal scroll at 360px; keyboard-only navigation works; reduced motion shows everything instantly; no console errors; build passes; every fact traces to the CV.

## 14. Open items for Dhara (leave TODOs, don't block)
- Portrait photo (4:5, ≥ 1200px tall) → `public/portrait.jpg`. Until then the DP monogram card is used.
- Final CV PDF for `public/` — consider a web version without the phone number.
- Whether to show official company logos (default: neutral monogram tiles).
- Confirm the Amazon case study's level of detail is fine to share publicly.
- Domain name.
