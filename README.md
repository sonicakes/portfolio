# Khokhloma Portfolio

Personal portfolio for Sonia — front-end & full-stack developer.  
Design direction: Russian folk art (Khokhloma), derived from the owner's own hand-painted spoon (Kudrina style: black field, crimson rosette, gold leaves, rowan berries).

**Stack:** Astro 7 · Tailwind v4 · TypeScript · Static export → Netlify

---

## Commands

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start local dev server at `localhost:4321`  |
| `npm run build`   | Build to `./dist/`                          |
| `npm run preview` | Preview production build locally            |

---

## Project structure

```
src/
  layouts/
    Base.astro          # HTML shell — fonts, global styles, Nav, SideVines, MenuOverlay, footer
  pages/
    index.astro         # Full page assembly; all content data lives here as typed arrays
  components/
    FlowerMark.astro    # Nav logo SVG: gold ring, six crimson berries, gold center
    Kustik.astro        # Hero kustik SVG with full paint-order animation
    Spoons.astro        # Crossed-spoons SVG for the contact panel crown
    Divider.astro       # Section flourish SVG; phase="a"|"b" alternates the wave
    Nav.astro           # Sticky nav; inline links ≥1180px, burger <1180px
    Burger.astro        # Three-bar + rowan berry button; morphs to crimson ✕ on open
    MenuOverlay.astro   # Full-screen overlay: vine draw, staggered links, script
    SideVines.astro     # Scroll-driven growing vine columns; rAF-throttled script
    ProjectCard.astro   # Props: title, kind, description, tags[]
    FactPanel.astro     # Props: facts[]; inner crimson keyline
    ContactPanel.astro  # Golden inversion section; Spoons crown; contact row
  styles/
    theme.css           # @theme tokens (§2.1/§2.2); global CSS; keyframes; reduced-motion
public/
  favicon.svg / .ico
```

The full design rationale lives in [`khokhloma-design-spec.md`](./khokhloma-design-spec.md). What follows is the same reasoning condensed for the README — the *why* behind every decision.

---

## 1. Design thesis

The page treats the interface the way Khokhloma masters treat wood: a dark, quiet lacquer ground with ornament applied only where it earns its place. The signature element is a hand-drawn SVG *kustik* (bush composition) in the hero, recreated from the owner's physical artwork. Everything else is disciplined: hairline borders, mono-caps labels, restrained motion. One inversion — the "golden Khokhloma" contact panel — closes the page like turning the spoon over.

**Authenticity constraint:** the palette never leaves the four traditional Khokhloma colors (black, red, gold, green) plus derived tints for text legibility. No blues, no purples, no neutral grays. This is a hard rule.

---

## 2. Design tokens

### 2.1 Color

All tokens are defined once in `src/styles/theme.css` under `@theme`. No raw hex values appear in component files.

| Token | Hex | Role | Usage rules |
|---|---|---|---|
| `--color-lacquer` | `#0e0c08` | Page ground | Body background. Warm near-black — never pure `#000`, lacquer has warmth. |
| `--color-lacquer-soft` | `#171309` | Raised surfaces | Card and panel backgrounds only. One step of elevation; no deeper levels. |
| `--color-crimson` | `#e0272d` | Primary accent | Berries, rosette petals, hover states, the Cyrillic hero line, card category labels, craft bullets (odd). Never body text or backgrounds. |
| `--color-crimson-deep` | `#a31418` | Crimson shadow | Berry center dots, hover text on gold ground. Never appears alone. |
| `--color-gold` | `#f0b31c` | Secondary accent / brand | Headings, links, nav mark, CTA borders, teardrop leaves, craft bullets (even). The "voice" of the brand. |
| `--color-gold-deep` | `#c98f0d` | Gold shadow | Rosette center ring, CTA border, gradient stops. |
| `--color-green` | `#1e4a1f` | Structural botanical | Main vine stems, green leaves. Never for text. |
| `--color-green-vine` | `#3f6d2a` | Fine botanical | Tendrils, leaf veins, tech tag borders/text. The only green permitted for text (small, non-body only). |
| `--color-ink-cream` | `#eadfbe` | Body text | Warm cream, not white — reads as aged lacquer highlight. All paragraph text. |
| `--color-ink-dim` | `#b8a878` | Secondary text | Eyebrows, captions, muted labels, hero blurb. |
| `--color-hairline` | `rgba(240,179,28,.28)` | Borders | All 1px borders. Gold at 28% — visible but quiet. |

**Justification:** Every hex is sampled or derived from the physical artwork. The two-tier accent pairs (crimson/crimson-deep, gold/gold-deep, green/green-vine) mirror how Khokhloma painting layers each pigment. Cream instead of white keeps contrast comfortable on OLED and avoids the sterile look that pure white causes on warm blacks. Contrast ratios: `--color-ink-cream` on `--color-lacquer` ≈ 12.9:1 (AAA); `--color-ink-dim` on `--color-lacquer` ≈ 7.5:1 (AA+ at its caption sizes).

**Golden contact panel locals (not in @theme — these are unique to that one surface):** gradient `165deg, #f4bc27 → #e8a70f (55%) → #cf920c`; body text on gold `#4d3c08`; inner keyline `rgba(14,12,8,.55)` at 1.5px, inset 8px.

### 2.2 Typography

Three roles, three faces, each with a distinct job and a thematic reason:

| Role | Face | Weights | Usage |
|---|---|---|---|
| Display | **Ruslan Display** | 400 only (single-weight) | h1, h2, card titles, nav wordmark, menu overlay links, Cyrillic accents. Never below ~1.15rem. |
| Body | **PT Sans** | 400, 700 | All paragraphs, facts, list items. |
| Utility | **PT Mono** | 400 | Eyebrows, nav links, tags, CTA labels, fact labels, footer. Always uppercase + letterspacing. |

**Justification:** Ruslan Display is drawn after Russian *lubok* (folk woodcut) lettering and has native Cyrillic — it carries the theme in the letterforms themselves without fake-Cyrillic kitsch, which is a hard anti-goal. PT Sans and PT Mono are from ParaType, the Moscow type foundry — a quiet thematic in-joke and a genuinely excellent workhorse pairing with full Cyrillic support. Three-role system: display speaks, body disappears, mono organizes.

**Type scale:**
- h1: `clamp(2.6rem, 7vw, 4.4rem)`, letterspacing `.05em`, gold, glow `0 0 26px rgba(240,179,28,.22)`
- Cyrillic hero line «Соня»: `clamp(1.15rem, 3vw, 1.6rem)`, crimson, letterspacing `.42em` + matching `text-indent` to optically center
- h2: `clamp(1.5rem, 3.6vw, 2.1rem)`, gold, centered
- Eyebrow/sub: `.78rem` PT Mono, letterspacing `.22em`, uppercase, `--color-ink-dim`
- Body: 17px base, line-height 1.65
- Menu overlay links: `clamp(1.7rem, 5vw, 2.5rem)` Ruslan + `.75rem` mono Russian sublabel

**Bilingual rule:** Russian appears exactly where it does work — never as decoration soup. Three places only: (1) «Соня» under the hero name; (2) Russian sublabels in the menu overlay (О себе / Работы / Ремесло / Связь); (3) «Расписано вручную» opening the footer. All Russian text carries `lang="ru"`.

### 2.3 Layout & spacing

- Content column: `max-width: 1060px`, 24px side padding (`.wrap`)
- Section rhythm: 72px vertical padding per section; SVG dividers between sections
- Side-vine gutter: at ≥820px, body gets 52px left/right padding (`--vine-gutter`) so fixed side vines own their lane and never overlap content
- Border radius: 2px (buttons, keylines, tags) and 4px (cards, panels) only — Khokhloma is crisp; large radii would feel off-brand
- Breakpoints: **640px** (minor), **760px** (about grid stacks), **820px** (side vines appear, gutter applied), **880px** (card grids stack), **1180px** (desktop inline nav replaces burger)

---

## 3. Components

### 3.1 Navigation — `Nav.astro`

Sticky, `rgba(14,12,8,.92)` + `backdrop-filter: blur(6px)`, hairline bottom border. Left: `FlowerMark.astro` (circular flower mark: gold ring, six crimson berries, gold center dot) + "Sonia" in Ruslan gold. Right: at ≥1180px inline links (PT Mono caps); at <1180px, `Burger.astro`.

*Justification:* Desktop visitors (recruiters) get the conventional navigation pattern; the theatrical burger overlay is reserved for touch contexts where full-screen menus are native. The 1180px threshold clears the 1060px content column plus gutters.

### 3.2 Burger button — `Burger.astro`

48px circle, hairline gold border (full gold on hover). Inside: three gold bars (22×2.4px, 6.5px vertical rhythm) + one crimson rowan berry (6.5px, with deep-crimson center dot) perched at the right end of the middle bar.

**Open state:** outer bars fold to a crimson ✕ (±45°, translate ±6.5px); middle bar collapses (`scaleX(.2)`, opacity 0); berry translates to center and scales 1.5×. Easing: `cubic-bezier(.6,.05,.2,1.2)` at `.38s` — slight overshoot gives the fold a hand-made snap.

*Justification:* Reads instantly as a menu (recognition first), but the berry makes it unmistakably of this brand. The berry-to-center move gives the ✕ a focal point rather than just an animation.

### 3.3 Menu overlay — `MenuOverlay.astro`

Full-screen fixed, `rgba(14,12,8,.97)`, z-index 110 (above nav at 90). Left edge: a vertical vine SVG that draws itself top-to-bottom on each open (1.4s ease-out via `stroke-dashoffset` + `pathLength="1"`), sprouting tendril curls; berry clusters fade in at 1.2s. Links: 4 items, centered, Ruslan gold with Russian PT Mono sublabels, staggered rise-in (.25s / .4s / .55s / .7s delays, .55s duration). Closes on link click and Escape; scroll locked while open; `aria-expanded`, `aria-controls`, live `aria-label` on the burger.

*Justification:* The overlay is a moment of theatre proportionate to a portfolio's job — being remembered. The vine re-draws on every open because the animation resets when the `.open` class drops, which is automatic CSS behaviour. Cheap delight, zero JS animation code.

### 3.4 Hero — `Kustik.astro` + page header

Centered, in order: kustik SVG → h1 "Sonia" → «Соня» → role eyebrow → blurb (max 560px) → CTA ("See the work").

**Kustik SVG** (`760×400` viewBox, `min(560px, 92vw)` wide): symmetric composition — central 8-petal crimson rosette with black petal veins and gold center ring; two main vine stems; four fine tendrils with spiral curls; two gold teardrop leaves; two green leaves; two 12-berry rowan clusters (`<use>` of a shared `#cluster` symbol); five scattered gold rim dots. All geometry mirrors the physical spoon's composition.

**Paint-order animation (plays once, ~2.5s total):**
1. Main vines draw from center outward — 1.8s ease-out, .2s delay
2. Fine tendrils — 1.3s, .9s delay
3. Leaves fade in — .9s, 1.5s delay
4. Rosette blooms — springy scale `.45→1`, `cubic-bezier(.3,1.4,.5,1)`, .8s, 1.8s delay
5. Berry clusters + rim dots fade — .9s, 2s delay

*Justification:* The sequence mirrors how a kustik is actually painted (stems → leaves → flower → berries), so the motion teaches the craft. It plays once and settles — no perpetual motion in the hero. Floating and sheen effects were tested and rejected; they read as noise against an ornament this detailed.

### 3.5 Section dividers — `Divider.astro`

Small SVG flourishes (`340×44`): a sinusoidal green vine with one central crimson berry and two gold dots. Accepts a `phase` prop (`"a"` | `"b"`) that inverts the wave, so consecutive dividers alternate rather than repeat.

### 3.6 About section

Two-column grid (1.4fr / 1fr, stacks at 760px): prose left; `FactPanel.astro` right. The fact panel is `--color-lacquer-soft` with a hairline outer border and an inner crimson keyline (`rgba(224,39,45,.25)`, inset 6px) echoing the painted double borders on real lacquerware. Facts render as label/value rows with dotted separators — PT Mono caps labels, right-aligned values.

### 3.7 Project cards — `ProjectCard.astro`

3-up grid (stacks at 880px). `--color-lacquer-soft`, hairline border, 4px radius. Top-right corner: three CSS-only berry dots (8px crimson circles with `crimson-deep` centers — zero asset cost). Content: Ruslan title (gold) → crimson mono category → description → tech tags (PT Mono, `--color-green-vine` text + 55%-alpha green border). Hover: border lifts to full gold + `translateY(-3px)`.

*Justification:* Cards stay quiet so the hero keeps its monopoly on ornament; the three berries are a brand echo at negligible cost. Green tags visually separate "technology" as botanical/structural information from the editorial red/gold layer — they read differently at a glance.

### 3.8 Craft section

Three columns titled "Front of house / Back of house / Workshop" (PT Mono caps, gold), each opened by a 2px crimson top rule. List bullets are 8px circles alternating crimson/gold — berry strings in miniature.

### 3.9 Contact panel — `ContactPanel.astro`

The single inverted surface on the page: gold gradient ground (`165deg, #f4bc27 → #e8a70f → #cf920c`), black inner keyline (1.5px, inset 8px), all text `--color-lacquer`/`#4d3c08`, links underlined black turning `--color-crimson-deep` on hover. Crowned by `Spoons.astro`: two black lacquer spoons (±22°) each carrying the six-berry flower mark and a gold handle stripe.

*Justification:* Golden Khokhloma is the traditional counterpart to black-ground work; placing it last makes the page read black → black → black → **gold**, like turning the spoon over to find the painted bowl. It is also the highest-contrast moment on the page, which is exactly where a portfolio should put its call to action. The inversion must remain singular — a second gold section would dissolve the effect.

### 3.10 Side vines — `SideVines.astro`

Two mirrored fixed SVG columns (44px wide, 5px from viewport edges, z-index 5, `pointer-events: none`, visible ≥820px only, body padding reserves the gutter).

**Three layers per vine:**
1. **Dashed guide** — full serpentine path, `stroke-dasharray: 4 9`, 1.4px, `--color-green-vine` at 32% opacity. Always visible; signals "scroll for more."
2. **Solid stem** — same path, 4px `--color-green`, JS-driven: `strokeDashoffset = 1 − p` where `p = 0.06 + 0.94 × (scrollY / maxScroll)`. 6% seed sprout visible before scrolling; 100% at the footer. rAF-throttled, passive, recalculated on resize.
3. **Tendrils + berries** — three curl paths sprout (CSS `stroke-dashoffset` transition, .9s) and four berry groups pop (springy scale `.35→1`, `cubic-bezier(.3,1.5,.5,1)`, .6s) when scroll `p` passes their `data-at` thresholds.

All growth is reversible — scrolling up retracts the vine to the dashed underdrawing.

*Justification:* Ties the menu overlay's vine language to the main page experience. The dashed underdrawing mirrors how Khokhloma masters sketch ornament before painting — the visitor's scroll is the brush. Scroll-linked rather than time-linked so the user owns the animation completely.

---

## 4. Motion system

| Motion | Trigger | Duration / Easing | Repeats? |
|---|---|---|---|
| Kustik paint sequence | Page load | ~2.5s staged | Once |
| Section rise-ins | IntersectionObserver, threshold .12 | .7s ease, 16px rise | Once per section |
| Burger morph | Click | .38s `cubic-bezier(.6,.05,.2,1.2)` | Toggles |
| Overlay vine + links | Menu open | 1.4s draw + staggered .55s rises | Every open |
| Side vine growth | Scroll position | Direct-mapped; tendrils .9s; berries .6s | Continuous, reversible |
| Card hover | Hover | .3s border + lift | — |
| CTA hover | Hover | .25s fill swap | — |

**Global rule:** exactly one springy easing — `cubic-bezier(.3, 1.4–1.5, .5, 1)` — shared by the rosette bloom and all berry pops. One consistent "pop" voice. Everything else eases conventionally. No infinite or ambient animations anywhere on the page.

**`prefers-reduced-motion: reduce`:** every animation is neutralized to its end state in `theme.css`. The JS scroll handler checks `matchMedia` and short-circuits to the fully-grown state immediately.

---

## 5. Accessibility

- `:focus-visible` — 2px gold outline, 3px offset, on all interactive elements
- Burger: `aria-expanded`, `aria-controls="menu"`, live `aria-label` (Open menu / Close menu)
- Menu overlay: `role="dialog"`, `aria-label="Site menu"`; Escape closes; scroll locked while open
- Decorative SVGs: `aria-hidden="true"`. Kustik: `role="img"` + descriptive `aria-label`
- All Russian text: `lang="ru"`
- `::selection` styled (crimson background, lacquer text)
- Fully responsive to 360px viewport width

---

## 6. Implementation notes

A few non-obvious patterns enforced in this codebase:

1. **SVG `transform` vs CSS `transform`:** animating CSS `transform: scale()` on an element that positions itself via the SVG `transform="translate(x,y)"` attribute silently deletes the translate. Pattern: outer `<g transform="translate(...)">` holds position; inner `<g class="rosette">` holds the CSS animation. `transform-box: fill-box; transform-origin: center` on the inner group.

2. **`pathLength="1"` normalisation:** every draw-on path sets `pathLength="1"` so `stroke-dasharray: 1` / `stroke-dashoffset: 0→1` works without calling `getTotalLength()`. Do not switch to JS measurement.

3. **Dashed guide must NOT have `pathLength="1"`** — its `stroke-dasharray: 4 9` is in SVG user units; normalising would collapse the dashes to nothing.

4. **Inline JS `style.strokeDashoffset` overrides the stylesheet** — the initial CSS value on `.stem` is only a pre-JS fallback for the instant before the scroll script initialises.

5. **Scroll math:** `maxScroll = documentElement.scrollHeight − innerHeight`. When content is shorter than the viewport this is 0; the JS guards the division.

6. **Ruslan Display is single-weight** — never apply `font-weight: 700` or `font-weight: bold` to it; the browser will synthesise a fake bold that destroys the letterforms.

7. **Token discipline:** all §2.1 hex values are defined once in `@theme` in `theme.css`. Raw hex in component `<style>` blocks is only permitted for the golden contact panel's local values (gradient stops, dark body text) that are unique to that one surface.

---

## 7. Tech stack rationale

**Astro 7 + Tailwind v4** was chosen over Next.js 15 (the owner's active stack for Kino Royale) for this specific project.

Astro's "zero JS by default" model is the ideal fit for a content-first single page: it renders to pure HTML + CSS at build time and ships no framework runtime to the browser. The only JavaScript on this site is the ~40-line scroll vine handler and the burger menu toggle — both are tiny vanilla scripts that need no React, no hydration, and no virtual DOM. Shipping them as `<script>` blocks inside Astro components keeps them scoped and co-located without adding bundle weight.

Tailwind v4 is wired in via `@tailwindcss/vite` (the Vite plugin, not the Astro integration) — no `tailwind.config.js`. All design tokens live in a single `@theme {}` block in `theme.css`, which is the v4-idiomatic equivalent of the token discipline used in Kino Royale.

**Hosting:** Netlify static deploy, consistent with `royal-random-simulator` and `cinefile-blog` infrastructure.

---

## 8. Before launch

- [ ] Real links: email, GitHub, LinkedIn, Letterboxd (currently placeholder `href="#"` in `ContactPanel.astro`)
- [ ] Project card URLs (live site links)
- [ ] Favicon — the circle flower mark (`FlowerMark.astro` geometry) or single-spoon silhouette
- [ ] OG / social share image (kustik on lacquer ground, 1200×630)
- [ ] Hero blurb: decide whether to mention availability / open-to-work
- [ ] Optional: photo or scan of the physical spoon in the About section
- [ ] Analytics choice, if any
