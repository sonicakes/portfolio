# Khokhloma Portfolio — Design Specification

**Project:** Personal portfolio for Sonia — front-end & full-stack developer
**Design direction:** Russian folk art (Khokhloma), derived from the owner's own hand-painted spoon (Kudrina style: black field, crimson rosette, gold leaves, rowan berries)
**Reference implementation:** `khokhloma-portfolio.html` (single-file, production-representative). Treat it as the source of truth for all values; this document explains the *why* behind every value.
**Status:** Design approved through iterative review. Ready for implementation.

---

## 1. Design thesis

The page treats the interface the way Khokhloma masters treat wood: a dark, quiet lacquer ground with ornament applied only where it earns its place. The signature element is a hand-drawn SVG *kustik* (bush composition) in the hero, recreated from the owner's physical artwork. Everything else is disciplined: hairline borders, mono-caps labels, restrained motion. One inversion — the "golden Khokhloma" contact panel — closes the page like turning the spoon over.

**Authenticity constraint:** the palette never leaves the four traditional Khokhloma colors (black, red, gold, green) plus derived tints for text legibility. No blues, no purples, no neutral grays. This is a hard rule, equivalent in spirit to the Kino Royale "red only on the hero silhouette" token rule.

---

## 2. Design tokens

### 2.1 Color

| Token | Hex | Role | Usage rules |
|---|---|---|---|
| `--lacquer` | `#0e0c08` | Page ground | Body background. Warm near-black (never pure `#000` — lacquer has warmth). |
| `--lacquer-soft` | `#171309` | Raised surfaces | Card and panel backgrounds only. One step of elevation; no deeper elevation system exists or should be added. |
| `--crimson` | `#e0272d` | Primary accent | Berries, rosette petals, hover states, the Cyrillic hero line, card category labels, craft-list bullets (odd). Never used for body text or backgrounds. |
| `--crimson-deep` | `#a31418` | Crimson shadow | Berry center dots, hover text on gold ground. Derived tone; never appears alone. |
| `--gold` | `#f0b31c` | Secondary accent / brand | Headings, links, nav mark, CTA borders, teardrop leaves, craft-list bullets (even). The "voice" color of the brand. |
| `--gold-deep` | `#c98f0d` | Gold shadow | Rosette center ring, CTA border, gradient stops. |
| `--green` | `#1e4a1f` | Structural botanical | Main vine stems, green leaves. Never for text. |
| `--green-vine` | `#3f6d2a` | Fine botanical | Tendrils, leaf veins, tech tag borders/text. The only green permitted for (small, non-body) text. |
| `--ink-cream` | `#eadfbe` | Body text | Warm cream, not white — reads as aged lacquer highlight. All paragraph text. |
| `--ink-dim` | `#b8a878` | Secondary text | Eyebrows, captions, muted labels, hero blurb. |
| `--hairline` | `rgba(240,179,28,.28)` | Borders | All 1px borders (nav bottom, cards, fact panel, footer top). Gold at 28% — visible but quiet. |

**Gold contact panel (inverted section) locals:** gradient `165deg, #f4bc27 → #e8a70f (55%) → #cf920c`; body text on gold `#4d3c08`; inner keyline `rgba(14,12,8,.55)` at 1.5px, inset 8px.

**Justification:** Every hex is sampled/derived from the physical artwork. Two-tier accents (crimson/crimson-deep, gold/gold-deep, green/green-vine) mirror how the painting itself layers each pigment. Cream instead of white keeps contrast comfortable on OLED and avoids the sterile look that pure white causes on warm blacks. Contrast: `--ink-cream` on `--lacquer` ≈ 12.9:1 (AAA); `--ink-dim` on `--lacquer` ≈ 7.5:1 (AA+ for its small-caption sizes); `--lacquer` on mid-gold ≈ 9:1.

### 2.2 Typography

| Role | Face | Weights | Usage |
|---|---|---|---|
| Display | **Ruslan Display** | 400 only (single-weight face) | h1, h2, card titles, nav wordmark, menu overlay links, Cyrillic accents. Never below ~1.15rem (letterforms clog small). |
| Body | **PT Sans** | 400, 700 | All paragraphs, facts, list items. |
| Utility | **PT Mono** | 400 | Eyebrows, nav links, tags, CTA labels, facts labels, footer. Always uppercase + letterspacing for label use. |

Source: Google Fonts. `display=swap`.

**Justification:** Ruslan Display is drawn after Russian lubok/folk lettering and has native Cyrillic — it carries the theme in the letterforms themselves without resorting to fake-Cyrillic kitsch (a hard anti-goal). PT Sans and PT Mono are from ParaType, the Moscow type foundry — a quiet thematic in-joke and a genuinely excellent workhorse pairing with full Cyrillic support. Three-role system: display speaks, body disappears, mono organizes.

**Type scale (reference values):**
- h1: `clamp(2.6rem, 7vw, 4.4rem)`, letterspacing .05em, gold, glow `0 0 26px rgba(240,179,28,.22)`
- Cyrillic hero line («Соня»): `clamp(1.15rem, 3vw, 1.6rem)`, crimson, letterspacing .42em + matching text-indent to optically center
- h2: `clamp(1.5rem, 3.6vw, 2.1rem)`, gold, centered
- Eyebrow/sub: .78rem PT Mono, letterspacing .22em, uppercase, `--ink-dim`
- Body: 17px base, line-height 1.65
- Menu overlay links: `clamp(1.7rem, 5vw, 2.5rem)` Ruslan + .75rem mono Russian sublabel

**Bilingual rule:** Russian appears exactly where it does work, never as decoration soup: (1) «Соня» under the hero name; (2) Russian sublabels in the menu overlay (О себе / Работы / Ремесло / Связь); (3) «Расписано вручную» opening the footer. All Russian text carries `lang="ru"`.

### 2.3 Layout & spacing

- Content column: `max-width: 1060px`, 24px side padding.
- Section rhythm: 72px vertical padding per section; dividers between sections.
- Side-vine gutter: at ≥820px, body gets 52px left/right padding (`--vine-gutter`) so fixed side vines own their lane and never overlap content.
- Border radius: 2px (buttons, keylines, tags) and 4px (cards, panels) only. Khokhloma is crisp; big radii would feel off-brand.
- Breakpoints: **640px** (minor), **760px** (about grid stacks), **820px** (side vines appear, gutter applied), **880px** (card grids stack), **1180px** (desktop inline nav replaces burger).

---

## 3. Components

### 3.1 Navigation bar
Sticky, `rgba(14,12,8,.92)` + `backdrop-filter: blur(6px)`, hairline bottom border. Left: circular flower mark (gold ring, six crimson berries around gold center) + "Sonia" in Ruslan gold. Right: **≥1180px** — inline links (PT Mono caps, gold, 28px gap); **<1180px** — burger button.

*Justification:* Desktop visitors (recruiters) get the conventional pattern; the theatrical burger overlay is reserved for touch contexts where full-screen menus are native. The 1180px threshold clears the 1060px content column plus gutters.

### 3.2 Burger button (signature micro-interaction)
48px circle, hairline gold border (gold on hover). Inside: three gold bars (22×2.4px, 6.5px vertical rhythm) + one crimson rowan berry (6.5px, with deep-crimson center dot) perched at the right end of the middle bar.

**Open state (`aria-expanded="true"`):** outer bars fold to a crimson ✕ (±45°, translate ±6.5px); middle bar collapses (`scaleX(.2)`, opacity 0); berry translates to center and scales 1.5×. Easing: `cubic-bezier(.6,.05,.2,1.2)` at .38s — slight overshoot gives the fold a hand-made snap.

*Justification:* Reads instantly as a menu (recognition first), but the berry makes it unmistakably of this brand. The berry-to-center move gives the ✕ a focal point.

### 3.3 Menu overlay
Full-screen fixed, `rgba(14,12,8,.97)`, z-index 110 (above nav at 90). Left edge: vertical vine SVG that draws itself top-to-bottom on each open (1.4s ease-out via `stroke-dashoffset` + `pathLength="1"`), sprouting tendril curls; berry clusters fade at 1.2s. Links: 4 items, centered, Ruslan gold with Russian mono sublabels, staggered rise-in (.25s/.4s/.55s/.7s delays, .55s duration). Behavior: closes on link click and Escape; body scroll locked while open; button carries `aria-controls` + live `aria-label`.

*Justification:* The overlay is a moment of theatre proportionate to a portfolio's job (be remembered). The vine re-draws on every open because the animation resets when the `.open` class drops — cheap delight, zero JS animation code.

### 3.4 Hero (signature element)
Centered, in order: kustik SVG → h1 "Sonia" → «Соня» → role eyebrow → blurb (max 560px) → CTA ("See the work", mono caps, gold border, crimson fill on hover).

**Kustik SVG** (760×400 viewBox, `min(560px, 92vw)` wide): symmetric composition — central 8-petal crimson rosette with black petal veins and gold center; two main vine stems; four fine tendrils with spiral curls; two gold teardrop leaves; two green leaves; two 12-berry rowan clusters (`<use>` of a shared berry symbol: crimson circle + deep-crimson center dot); five scattered gold rim dots. All geometry mirrors the physical spoon's composition.

**Load animation (paint-order sequence, plays once, ~2.5s total):**
1. Main vines draw from center outward — 1.8s ease-out, .2s delay (`stroke-dasharray/offset` with `pathLength="1"`)
2. Fine tendrils — 1.3s, .9s delay
3. Leaves fade in — .9s, 1.5s delay
4. Rosette blooms — springy scale .45→1, `cubic-bezier(.3,1.4,.5,1)`, .8s, 1.8s delay
5. Berry clusters + rim dots fade — .9s, 2s delay

*Justification:* The sequence mirrors how a kustik is actually painted (stems → leaves → flower → berries), so the motion *teaches* the craft. It plays once and settles — no perpetual motion in the hero (tested and rejected: floating/sheen effects read as noise).

### 3.5 Section dividers
Small SVG flourishes (340×44): a sinusoidal green vine with one central crimson berry and two gold dots. Alternating wave phase between consecutive dividers so the rhythm doesn't stutter.

### 3.6 About section
Two-column grid (1.4fr / 1fr, stacks at 760px): prose left; "fact panel" right — `--lacquer-soft` card, hairline border, plus an inner crimson keyline (`rgba(224,39,45,.25)`, inset 6px) echoing painted double borders on lacquerware. Facts as label/value rows with dotted separators (PT Mono caps labels, right-aligned values).

### 3.7 Project cards
3-up grid (stacks at 880px). `--lacquer-soft`, hairline border, 4px radius. Top-right: three CSS-only berry dots. Content: Ruslan title (gold) → crimson mono category → description → tech tags (mono, `--green-vine` text + 55%-alpha green border — the *only* green text on the site). Hover: border to full gold + `translateY(-3px)`.

*Justification:* Cards stay quiet so the hero keeps its monopoly on ornament; the three berries are a 0-asset brand echo. Green tags subtly separate "technology" as botanical/structural information from the red/gold editorial layer.

### 3.8 Craft (skills) section
Three columns titled "Front of house / Back of house / Workshop" (mono caps, gold), each opened by a 2px crimson top rule. List bullets are 8px circles alternating crimson/gold — berry strings.

### 3.9 Contact panel — "golden Khokhloma" inversion
The single inverted surface: gold gradient ground, black inner keyline (1.5px, inset 8px), all text `--lacquer`/`#4d3c08`, links underlined black turning `--crimson-deep` on hover. Crowned by a crossed-spoons SVG: two black lacquer spoons (±22°) each carrying the six-berry flower and a gold handle stripe. Heading: "Commission a piece".

*Justification:* Golden Khokhloma is the traditional counterpart to black-ground work; placing it last makes the page read black → black → black → **gold**, like turning the spoon over. It also makes the CTA the highest-contrast moment on the page, which is exactly where a portfolio wants it. The inversion must remain singular — a second gold section would destroy the effect.

### 3.10 Side vines (scroll companion)
Two mirrored fixed SVG columns (44px wide, 5px from viewport edges, z-index 5, `pointer-events:none`, visible ≥820px only, gutter reserved via body padding).

**Layers per vine:**
1. **Dashed guide line** — the full serpentine path, `stroke-dasharray: 4 9`, 1.4px, `--green-vine` at 32% opacity. Always visible; signals "the path continues — scroll."
2. **Solid stem** — same path, 4px `--green`, drawn via JS: `strokeDashoffset = 1 − p` where `p = 0.06 + 0.94 × (scrollY / maxScroll)` (6% seed sprout visible at page top; reaches 100% exactly at the footer). rAF-throttled scroll listener, passive, recalculated on resize.
3. **Tendrils** — three curl paths, sprout via CSS `stroke-dashoffset` transition (.9s ease-out) when `p` passes `data-at` ∈ {0.2, 0.45, 0.78}.
4. **Berries** — four groups (two crimson triple-clusters, two gold dots) popping with springy scale (.35→1, `cubic-bezier(.3,1.5,.5,1)`) at `data-at` ∈ {0.3, 0.55, 0.75, 0.95}.

All growth is reversible — scrolling up retracts the vine to the dashed guide.

*Justification:* Ties the menu overlay's vine language to the main page; the dashed underdrawing mirrors how real Khokhloma masters sketch ornament before painting — the visitor's scroll is the brush. Scroll-linked (not time-linked) so the user owns the animation.

---

## 4. Motion system summary

| Motion | Trigger | Duration/Easing | Repeats? |
|---|---|---|---|
| Kustik paint sequence | Page load | ~2.5s staged (see 3.4) | Once |
| Section rise-ins (`.rise`) | IntersectionObserver, threshold .12 | .7s ease, 16px rise | Once per section |
| Burger morph | Click | .38s `cubic-bezier(.6,.05,.2,1.2)` | Toggles |
| Overlay vine + links | Menu open | 1.4s draw + staggered .55s rises | Every open |
| Side vine growth | Scroll position | Direct-mapped, tendrils .9s, berries .6s | Continuous, reversible |
| Card hover | Hover | .3s border + lift | — |
| CTA hover | Hover | .25s fill swap | — |

**Global rule:** exactly one springy easing (`.3, 1.4–1.5, .5, 1`) shared by rosette bloom and berry pops — one consistent "pop" voice. Everything else eases conventionally. No infinite/ambient animations anywhere.

**`prefers-reduced-motion: reduce`:** every animation above is neutralized to its end state (kustik fully painted, vines fully grown with all berries, overlay appears without choreography, no transitions). Implemented in both CSS and the JS scroll handler (JS checks `matchMedia` and short-circuits).

---

## 5. Accessibility & quality floor

- Focus: `:focus-visible` — 2px gold outline, 3px offset, on all links/buttons.
- Burger: `aria-expanded`, `aria-controls="menu"`, live `aria-label` (Open/Close menu). Overlay: `role="dialog"`, `aria-label`. Escape closes; scroll lock while open.
- Decorative SVGs: `aria-hidden="true"`. Meaningful SVGs (kustik): `role="img"` + descriptive `aria-label`.
- All Russian text: `lang="ru"`.
- `::selection` styled (crimson/lacquer).
- Contrast: see §2.1. Verify `--ink-dim` never drops below 14px equivalent.
- Fully responsive to 360px; side vines and inline nav degrade gracefully per breakpoints in §2.3.

---

## 6. Implementation gotchas (learned during design — do not relearn)

1. **CSS `transform` overrides SVG `transform` attributes.** Animating `transform: scale()` on an element that positions itself via `transform="translate(x,y)"` silently deletes the translate. **Pattern:** wrap in an outer `<g transform="translate(...)">` (position) and animate an inner `<g>` with `transform-box: fill-box; transform-origin: center`. This applies identically in React/JSX.
2. **`pathLength="1"` normalization.** Every draw-on path sets `pathLength="1"` so `stroke-dasharray: 1` / `stroke-dashoffset: 0→1` works without measuring `getTotalLength()`. Keep this when porting; do not switch to JS length measurement.
3. **The dashed guide line must NOT have `pathLength="1"`** — its `stroke-dasharray: 4 9` is in user units and normalizing would collapse the dashes.
4. **Cascade order for the 1180px nav swap:** keep the `@media(min-width:1180px)` block *after* all `.burger` base rules (currently also guarded with `!important` on the hide).
5. **Inline `style.strokeDashoffset` (JS) beats the stylesheet** — the initial CSS `stroke-dashoffset` on `.stem` is only a pre-JS fallback.
6. **Scroll math:** `maxScroll = documentElement.scrollHeight − innerHeight`; guard the division when content is shorter than the viewport.
7. Ruslan Display is single-weight — never synthesize bold on it.
8. If porting to Tailwind v4, define all §2.1 tokens in `@theme` and forbid raw hex in components (same discipline as the Kino Royale token rules).

---

## 7. Tech stack recommendation

No stack has been chosen. Two strong options, one recommendation:

**Option A — Next.js 15 + TypeScript + Tailwind v4 (recommended).**
Matches the owner's active stack (Kino Royale site), so conventions, tooling, and muscle memory transfer directly, and Claude Code can mirror patterns from that codebase. Use App Router with `output: 'export'` (fully static — this site needs no server), `next/font` for the three Google faces (eliminates layout shift and third-party font requests), SVGs as typed React components, tokens in Tailwind `@theme`. Room to grow: MDX case studies, a Decap-backed blog, OG image generation.

**Option B — Astro + Tailwind v4.**
Objectively the best-fit tool for a static, content-first page: zero JS by default, the two small scripts (scroll vines, burger) ship as tiny inline islands, best-in-class Lighthouse. Choose this if the portfolio will stay a single page and stack consistency matters less than raw performance.

**Not recommended:** SPA scaffolds (plain Vite+React) — no routing/SEO benefits for a content page; plain HTML (Option C) is already done — the reference file can simply be deployed to Netlify as-is if speed-to-live matters more than maintainability.

**Hosting:** Netlify (consistent with royal-simulator and cinefileblog infrastructure; DNS patterns already established).

**Suggested repo structure (Option A):**
```
app/
  layout.tsx        # fonts, tokens, nav, side vines, footer
  page.tsx          # hero, about, work, craft, contact
components/
  Nav.tsx  Burger.tsx  MenuOverlay.tsx
  Kustik.tsx  SideVines.tsx  Divider.tsx
  ProjectCard.tsx  FactPanel.tsx  ContactPanel.tsx
lib/
  useScrollProgress.ts   # rAF-throttled, reduced-motion aware
styles/
  theme.css              # @theme tokens (§2.1, §2.2)
```

---

## 8. Content inventory & open items

**Real content already in place:** name, role, bio copy, three projects (Kino Royale, Royal Simulator, Cinefile Blog) with accurate tech tags, skills columns, location/certification facts.

**To supply before launch:**
- [ ] Real links: email, GitHub, LinkedIn, Letterboxd (currently placeholders in contact row)
- [ ] Project links (live site URLs on cards)
- [ ] Favicon — recommend the single-spoon or circle-flower mark from the reference file
- [ ] OG/social meta + share image (kustik on lacquer, 1200×630)
- [ ] Decide whether hero blurb mentions availability/open-to-work
- [ ] Optional: photo/scan of the physical spoon in the About section ("the artwork behind this site")
- [ ] Analytics choice, if any
