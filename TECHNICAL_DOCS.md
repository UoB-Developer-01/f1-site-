# F1 Greatest Drivers — Technical Documentation

## Overview

A single-file static web application displaying the top 10 Formula 1 drivers of all time. Built with vanilla HTML, CSS, and JavaScript — no build tools or dependencies beyond Google Fonts.

**File:** `index.html`  
**Stack:** HTML5 · CSS3 · Vanilla JS · Google Fonts  
**Entry point:** Open `index.html` directly in a browser (no server required).

---

## File Structure

```
f1-site-/
├── index.html          # Entire application (markup, styles, scripts)
└── f1_images/          # Driver portrait images
    ├── hamilton.jpg
    ├── schumacher.jpg
    ├── fangio.jpg
    ├── senna.jpg
    ├── prost.jpg
    ├── vettel.jpg
    ├── stewart.jpg
    ├── mansell.jpg
    ├── lauda.jpg
    └── verstappen.jpg
```

---

## Architecture

The application is structured in three layers inside a single HTML file:

| Layer | Location | Responsibility |
|---|---|---|
| Styles | `<style>` in `<head>` | Layout, theming, animations |
| Markup | `<body>` | Static shell (nav, hero, footer) |
| Data + Logic | `<script>` at end of `<body>` | Driver data, card rendering, animations, scroll reveal |

### Rendering Pattern

Cards are not hard-coded in HTML. Instead, a `drivers` array holds all driver data, and `buildCard(d)` generates HTML strings that are injected via `innerHTML` into `<main id="drivers">`.

```js
document.getElementById('drivers').innerHTML = drivers.map(buildCard).join('');
```

---

## CSS Design System

### Custom Properties (`--` variables)

Defined on `:root` and used throughout:

| Variable | Value | Usage |
|---|---|---|
| `--red` | `#E10600` | Primary accent (F1 brand red) |
| `--red-dark` | `#a00400` | Darker red variant |
| `--red-glow` | `rgba(225,6,0,0.35)` | Box-shadow glow effects |
| `--white` | `#ffffff` | Primary text |
| `--off-white` | `#f0f0f0` | Nav title text |
| `--card-bg` | `#111111` | Card background |
| `--card-border` | `#222222` | Card border / dividers |
| `--dark` | `#0a0a0a` | Page background |
| `--mid` | `#1a1a1a` | Mid-tone surface |
| `--text-muted` | `#888` | Secondary / label text |
| `--carbon1/2` | `#1a1a1a / #141414` | Carbon texture tones |

### Typography

Two typefaces loaded from Google Fonts:

- **Barlow Condensed** — headings, badges, rank numbers, driver names. Weights: 400–900.
- **Titillium Web** — body text, bios, labels. Weights: 200–900.

### Layout Components

#### Navigation (`nav`)
- Fixed, full-width, `z-index: 100`.
- `height: 56px`, `backdrop-filter: blur(12px)`.
- Red 2px bottom border as the F1 brand separator.
- Logo uses a clipped badge (`clip-path: polygon`) for the angled "F1" pill.

#### Hero Section (`.hero`)
- Full-viewport-height (`min-height: 100vh`), flexbox centered.
- Layered decorative elements (all `pointer-events: none`):
  - `body::before` — subtle carbon-weave texture via `repeating-linear-gradient`.
  - `.hero-lines` — animated speed lines (JS-generated `<div>` elements).
  - `.checkered-band` — checkered flag strip at top and bottom via `repeating-conic-gradient`.
  - `.hero-slash` — diagonal red tint overlay.
  - `.hero-bg-number` — giant "10" watermark at `opacity: 0.04`.

#### Driver Cards (`.driver-card`)

Three-column CSS Grid layout:

```
grid-template-columns: 100px 280px 1fr
grid-template-rows:    auto auto
```

| Column | Class | Content |
|---|---|---|
| 1 | `.card-rank` | Rank number, spans 2 rows |
| 2 | `.card-photo` | Driver image, spans 2 rows |
| 3 | `.card-info` | Name, nationality, years, teams, championships badge |
| 3 (row 2) | `.card-stats` | Stats grid + bio paragraph |

**Hover effects:**
- Card translates `(-4px, +4px)` and applies red box-shadow.
- Photo scales to `1.05` and removes grayscale filter.
- Corner triangle accent (`.corner-deco`) saturates to full red.
- Inner red gradient overlay (`::before`) fades in.

---

## JavaScript

### Data Model

Each driver object in the `drivers` array:

```js
{
  rank: Number,           // 1–10
  name: String,
  nationality: String,
  years: String,          // e.g. "2007 – Present"
  teams: String,          // " · " separated list
  championships: Number,
  stats: {
    wins: Number,
    poles: Number,
    podiums: Number,
    fastestLaps: Number
  },
  bio: String,            // paragraph text
  img: String,            // relative path to image
  maxWins: Number         // reference value for bar scaling (always 103)
}
```

### `buildCard(d)` — Card Template Function

Generates the full card HTML string for one driver. Stat bar widths are calculated as percentages relative to fixed reference maximums:

| Stat | Reference Max | Rationale |
|---|---|---|
| Wins | 103 | Hamilton's record |
| Pole Positions | 102 | Hamilton's record |
| Podiums | 191 | Hamilton's record |
| Fastest Laps | 54 | Hamilton's record |

The bar fill width is applied as an inline `style` at render time:

```js
style="width:${(d.stats.wins / maxStat * 100).toFixed(1)}%"
```

### Animated Speed Lines

12 `<div class="speed-line">` elements are created programmatically on load:

```js
for (let i = 0; i < 12; i++) {
  // randomised: top position, width, animation-delay, animation-duration
}
```

Each line runs the `speedLine` keyframe animation, which translates the element from `-100%` to `100vw` with an opacity arc.

### Scroll Reveal

Uses the `IntersectionObserver` API. Every `.reveal` element (each `.driver-card`) starts at `opacity: 0; transform: translateY(30px)`. When 8% of the card enters the viewport, the observer adds `.visible` (which transitions to `opacity: 1; transform: none`) and then unobserves the element.

```js
const observer = new IntersectionObserver(callback, { threshold: 0.08 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

---

## Responsive Breakpoints

| Breakpoint | Grid Change |
|---|---|
| `≤ 1000px` | Photo column shrinks: `80px 220px 1fr` |
| `≤ 768px` | Stacked layout: `60px 1fr` · photo spans full width as top row · stats use 4-column sub-grid |
| `≤ 520px` | Stats sub-grid collapses to 2 columns · rank column narrows to `50px` |

---

## Image Handling

Images use `loading="lazy"` for deferred loading. The `onerror` handler provides a minimal fallback if an image file is missing:

```html
onerror="this.style.background='#1a1a1a'; this.style.display='block'"
```

Images are styled with `object-fit: cover` and `object-position: top center` to keep faces visible in the fixed card height.

---

## Animations Summary

| Name | Trigger | Description |
|---|---|---|
| `speedLine` | On load (looping) | Speed lines sweep left-to-right across hero |
| `bob` | On load (looping) | Scroll cue arrow bobs up and down |
| Card hover | Mouse hover | Translate, shadow, image scale, gradient |
| `.reveal` | Scroll (IntersectionObserver) | Fade + slide-up on first viewport entry |

---

## Extending the Application

### Adding a Driver

Append an object to the `drivers` array in the `<script>` block following the existing schema. Place a corresponding image in `f1_images/`.

### Changing the Reference Bar Maximum

Update the `maxStat` constant inside `buildCard` and the hard-coded denominators for poles, podiums, and fastest laps if a new all-time record is set.

### Adding a New Stat Column

1. Add the stat key to the driver objects.
2. Add a new `<div class="stat-item">` block inside the `stats-grid` template in `buildCard`.
3. Adjust `grid-template-columns` on `.stats-grid` if needed.
