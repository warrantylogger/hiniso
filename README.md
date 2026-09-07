# Handoff: Hiniso Singapore marketing site

## Overview
A three-page marketing site for Hiniso premium dry cabinets (Singapore/Malaysia distribution):
a **Home** page (hero, product features, distributor verification, distributor-enquiry contact),
an **About** page, and a **Retailers** page with country-tabbed authorized-retailer listings.
Dark, editorial aesthetic with an amber accent, cinematic hero, and scroll-reveal motion.

## About the design files
The files in `design/` are **design references created in HTML** — a working prototype showing
the intended look, copy, and behavior. They are **not production code to copy directly**.

The build target is a **statically-rendered, SEO-indexable site**. Recreate these designs in the
target codebase's existing environment (React/Next.js, Astro, Nuxt, plain templates, etc.) using
its established patterns. If no environment exists, choose a static-site framework that ships
server-rendered HTML — the SEO requirements below are the reason this handoff exists.

**Do not ship the prototype.** It renders client-side from a single file with in-memory page state,
so crawlers see an effectively empty document and there is only one URL. That is the specific
problem the implementation must solve.

## Fidelity
**High fidelity.** Colors, typography, spacing, motion timings, and copy below are final.
Recreate pixel-accurately using the codebase's own primitives.

---

## SEO requirements (the point of this rebuild)

1. **Server-rendered HTML.** Full text content present in the initial HTML response — no
   client-only rendering of copy.
2. **Real routes**, one indexable URL each:
   - `/` — Home
   - `/about` — About
   - `/retailers` — Authorized retailers
   - `/retailers/singapore`, `/retailers/malaysia`, `/retailers/vietnam`, `/retailers/india` —
     country views (the prototype's tabs are client state; make each a crawlable URL, with the
     tab UI navigating between them and `/retailers` defaulting to Singapore)
3. **Per-page metadata**: unique `<title>`, `<meta name="description">`, `<link rel="canonical">`,
   Open Graph + Twitter card tags.
   - Home title: `Hiniso Premium Dry Cabinet`
   - Home description: `Hiniso premium dry cabinets — Japan technology since 2017. On-site service, original parts, and safety-standard compliance through authorized distributors.`
4. **Structured data (JSON-LD)**: `Organization` on all pages; `LocalBusiness` per retailer entry
   on the retailer pages; `BreadcrumbList` on About/Retailers.
5. **Real image files**, not data URIs — responsive `srcset`, `width`/`height` attributes to
   reserve space (CLS), `loading="lazy"` below the fold, `loading="eager"` +
   `fetchpriority="high"` on the hero, modern formats (AVIF/WebP with fallback).
6. **Semantic headings**: exactly one `<h1>` per page; section headings as `<h2>`, card titles as
   `<h3>` (the prototype uses `<h2>` for feature cards — fix this in implementation).
7. `sitemap.xml` and `robots.txt`.
8. Self-host the two web fonts (`font-display: swap`) rather than loading from Google Fonts.
9. Replace the CDN icon library with inline SVG icons (see Assets) — no runtime icon script.

---

## Design tokens

### Color
| Token | Value | Use |
|---|---|---|
| `ink` | `oklch(0.14 0.01 260)` | Dark page background |
| `ink-deep` | `#000000` | Header, footer |
| `accent` | `oklch(0.83 0.165 92)` | Amber — CTAs, kickers, icons, active nav |
| `accent-hover` | `oklch(0.68 0.14 85)` | CTA hover background |
| `on-accent` | `oklch(0.16 0.015 260)` | Text on amber buttons |
| `paper` | `oklch(0.985 0.004 106)` | Light page background (About, Retailers) |
| `paper-alt` | `oklch(0.968 0.005 250)` | Alternating light section background |
| `text-dark` | `oklch(0.195 0.012 258)` | Body text on light |
| `text-muted` | `oklch(0.505 0.014 256)` | Secondary text on light |
| `hairline-light` | `oklch(0.9 0.008 252)` | Borders on light |
| `badge-bg` | `oklch(0.93 0.032 233)` | "Authorized" badge background |
| `badge-fg` | `oklch(0.32 0.075 240)` | "Authorized" badge text |
| White alphas on dark | `rgba(255,255,255, .03 / .05 / .06 / .08 / .1 / .12 / .15 / .2 / .4 / .5 / .6 / .7 / .8)` | Surfaces, borders, secondary text |

### Typography
- **Display / headings**: Space Grotesk — 500, 700
- **Body**: DM Sans — 300, 400, 500, 700
- `-webkit-font-smoothing: antialiased` on body

| Role | Size | Weight | Tracking | Notes |
|---|---|---|---|---|
| Hero `h1` | `clamp(40px, 6vw, 72px)` | 700 | `-0.02em` | `line-height: 0.95`; second clause in `accent` |
| Hero subhead | 19px | 400 | — | `line-height: 1.65`, white, `text-shadow: 0 1px 12px oklch(0.14 0.01 260 / 0.9)` |
| Eyebrow pill | 10px | 700 | `0.3em` | uppercase, `accent` |
| Section kicker | 10px | 700 | `0.3em` | uppercase, `accent` |
| Card kicker | 11px | 700 | `0.2em` | uppercase, `rgba(255,255,255,0.5)` |
| Card title | 20px | 700 | `-0.02em` | Space Grotesk, white |
| Card body | 14px | 400 | — | `line-height: 1.65`, `rgba(255,255,255,0.7)` |
| Light section `h2` | 30px | 700 | `-0.02em` | Space Grotesk |
| Retailers `h1` | `clamp(30px, 4vw, 36px)` | 700 | `-0.02em` | — |
| Retailer name `h3` | 16px | 700 | — | Space Grotesk |
| Button label | 14px | 700 | `0.1em` | uppercase |
| Small link label | 11px | 700 | `0.2em` | uppercase |
| Footer heading | 11px | 500 | `0.22em` | uppercase, `rgba(255,255,255,0.5)` |
| Footer body / links | 14px | 400 | — | `rgba(255,255,255,0.6–0.7)` |
| Copyright | 12px | 400 | — | `rgba(255,255,255,0.5)` |

### Layout
- Content max width **1152px**, centered, `box-sizing: border-box`, horizontal padding **24px**
  (**20px** below 768px). Every container shares this — header, hero, all sections, footer — so the
  logo, hero headline, and all section content align on one left edge.
- Vertical section padding: 96px (dark sections), 80px (distributor), 64px (light sections),
  56px (retailers header), 48px (footer), 20px (copyright bar).
- Breakpoints: **768px** (mobile/desktop split), **1024px** (4-up feature grid), **640px** (2-up).

### Radius / elevation
- 2px — buttons, pills, badges, dropdown, contact card
- 4px — feature cards
- 6px — light-section images, retailer tabs and buttons
- 8px — retailer list cards, "why buy" panel
- Card hover shadow: `0 24px 48px -32px rgba(0,0,0,0.9)`
- Dropdown shadow: `0 24px 60px -20px rgba(0,0,0,0.8)`
- Stuck header shadow: `0 12px 32px -18px rgba(0,0,0,0.9)`

### Spacing scale in use
4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 48, 56, 64, 80, 96 px

---

## Screens

### 1. Site header (all pages)
- **Layout**: sticky, `top: 0`, `z-index: 40`, height **64px**, background `#000`,
  `border-bottom: 1px solid rgba(255,255,255,0.1)`. Inner container is the 1152px flex row,
  `justify-content: space-between`, `gap: 16px`.
- **Logo**: `assets/hiniso-logo.png`, height **22px**, width auto. Links to `/`.
- **Desktop nav** (≥768px): flex row, `gap: 28px`, 14px. Items: **About**, **Downloads** (dropdown),
  **Retailers**, **Contact**. Default `#fff`; hover and current-page state `accent`.
- **Downloads dropdown**: opens on `mouseenter`, closes 150ms after `mouseleave`. Absolutely
  positioned, `margin-top: 8px`, `min-width: 160px`, `#000`, 1px `rgba(255,255,255,0.1)` border,
  radius 2px, 4px padding. Items: **Catalogue** →
  `https://www.hiniso.com.sg/s/HINISO-Models-Product-Catalogue-New.pdf`, **User Guide** →
  `https://www.hiniso.com.sg/s/EN-HINISO.pdf` — both `target="_blank" rel="noopener noreferrer"`,
  8px/12px padding, 14px, hover `rgba(255,255,255,0.1)` + `accent` text.
  Chevron rotates 180° over 180ms `cubic-bezier(0.23, 1, 0.32, 1)` when open.
- **Mobile** (<768px): desktop nav hidden; 44×44 hamburger button (three 2px rounded lines,
  `currentColor`) that swaps to an X when open.
- **Mobile menu panel**: full-width below the header, `#000`, `border-top: 1px solid
  rgba(255,255,255,0.1)`, padding `8px 24px 24px`. Stacked links each `min-height: 52px`, 16px/500,
  `border-bottom: 1px solid rgba(255,255,255,0.08)`: **Home**, **About**, **Retailers**. Then a
  `Downloads` label (10px/700/`0.3em`, `rgba(255,255,255,0.4)`) and the two PDF links
  (`min-height: 48px`, 15px, `rgba(255,255,255,0.75)`). Closes with a full-width amber
  **Contact us** button, height 52px. Panel animates in: `opacity 0→1`,
  `translateY(-8px)→0`, 220ms `cubic-bezier(0.23, 1, 0.32, 1)`.
- **Scroll state**: add the shadow above once `scrollY > 8`, transitioning over 220ms.

### 2. Home — Hero
- **Layout**: `position: relative`, `min-height: 640px`, `overflow: hidden`, flex, vertically
  centered. Text column `max-width: 672px`, section padding 96px vertical.
- **Background**: `<picture>` — `assets/hero-mobile.png` below 768px, `assets/hero-desktop.png`
  above; absolutely positioned, `object-fit: cover`, `object-position: 100% 50%` desktop /
  `50% 100%` mobile.
- **Ken Burns**: `scale(1) → scale(1.1)`, **13s** desktop / **15s** mobile,
  `cubic-bezier(0.23, 1, 0.32, 1)`, `forwards`, `transform-origin: 65% 50%` desktop /
  `50% 100%` mobile. Disabled under `prefers-reduced-motion` and on low-end devices.
- **Scrim** (single absolute layer over the image):
  - Desktop: `linear-gradient(90deg, oklch(0.14 0.01 260 / 0.75) 0%, oklch(0.14 0.01 260 / 0.6) 45%, oklch(0.14 0.01 260 / 0.28) 72%, oklch(0.14 0.01 260 / 0.05) 92%), rgba(0, 0, 0, 0.18)`
  - Mobile: `linear-gradient(180deg, oklch(0.14 0.01 260 / 0.7) 0%, oklch(0.14 0.01 260 / 0.45) 48%, oklch(0.14 0.01 260 / 0.15) 100%), rgba(0, 0, 0, 0.18)`
- **Eyebrow pill**: inline-flex, `gap: 10px`, padding `6px 12px`, radius 2px,
  `border: 1px solid oklch(0.83 0.165 92 / 0.4)`, `background: oklch(0.83 0.165 92 / 0.1)`,
  `backdrop-filter: blur(4px)`. Leading 6px amber dot pulsing `opacity 1 → .35 → 1` over 2s
  `ease-in-out` infinite. Copy: **Japan technology since 2017**
- **H1**: `Premium dry cabinets. ` + amber `Premium quality, trusted performance`
- **Subhead**: `DryBox Official distributors provide on-site service, original parts, and safety-standard compliance.`
- **CTAs** (flex, wrap, `gap: 16px`, `margin-top: 40px`): **Shop Singapore** → `/retailers/singapore`,
  **Shop Malaysia** → `/retailers/malaysia`. Both height 52px, padding `0 36px`, radius 2px,
  `accent` background, `on-accent` text; hover `accent-hover` background + `#fff` text (160ms);
  `:active` `scale(0.98)` (120ms).
- **Entrance stagger** on load: eyebrow 0ms, h1 70ms, subhead 140ms, CTA row 210ms —
  `opacity 0→1`, `translateY(12px)→0`, 460ms `cubic-bezier(0.23, 1, 0.32, 1)`.

### 3. Home — Feature cards
Four-card grid inside the 1152px container, 96px vertical padding.
`gap: 20px`; columns `repeat(4, minmax(0,1fr))` ≥1024px, `repeat(2, minmax(0,1fr))` ≥640px,
single column below.

Each card: `display: flex; flex-direction: column`, padding `36px 32px`, radius 4px,
`border: 1px solid rgba(255,255,255,0.12)`, `background: rgba(255,255,255,0.03)`.
Hover — `background: rgba(255,255,255,0.06)`, `border-color: oklch(0.83 0.165 92 / 0.45)`,
`translateY(-4px)`, card shadow; 250ms (`transform` on `cubic-bezier(0.23, 1, 0.32, 1)`).

Card contents, in order: 36px amber icon (`stroke-width: 1.5`) → kicker (`margin-top: 32px`) →
title (`margin-top: 12px`) → body (`margin-top: 12px`).

| Icon | Kicker | Title | Body |
|---|---|---|---|
| snowflake | SYSTEM | Thermo-electric tech | HINISO's Thermo-electric Desiccant Technology prevents microscopic fungi and corrosive damage stemming from excess environmental humidity, without the use of noisy compressors. It's quick, heatless and provides full protection for your moisture sensitive gear. |
| battery-low | POWER | Low energy use | Dehumidification is achieved through the safe and silent operation with very low DC power consumption of less than 8, 15, or 20 watts. Suitable for worldwide usage. |
| gauge | CONTROL | Fully automatic | Once programmed, the cabinet operates automatically, activating only when internal humidity exceeds the set level and resuming when the internal humidity goes over 2% RH than set value. |
| lightbulb | INTERFACE | Easy control | A LED convenience light illuminates the cabinet and turns on or off easily. All components are also modular for easy disassembly. |

### 4. Home — Distributor verification
Single-column stack in the 1152px container, `gap: 32px`, 80px vertical padding,
`align-items: start` — heading block and card share the same left edge.

- **Heading block**: kicker `Verification`; `h2` (Space Grotesk 30px/700/`-0.02em`, uppercase, white)
  reading `Authorized` / `distributors` on two lines.
- **Card**: full width, radius 2px, `border: 1px solid rgba(255,255,255,0.1)`,
  `background: rgba(255,255,255,0.05)`, padding 32px.
  - **Guarantee chips**: flex, wrap, `gap: 16px`. Each — inline-flex, `gap: 8px`, padding `8px 14px`,
    `border: 1px solid rgba(255,255,255,0.15)`, 11px/700/`0.1em` uppercase,
    `rgba(255,255,255,0.8)`, with a 14px amber `shield-check` icon.
    Chips: **On-site service**, **Original parts**, **Safety-standard adapters**
  - **Footer row**: `margin-top: 32px`, `padding-top: 32px`,
    `border-top: 1px solid rgba(255,255,255,0.1)`, flex, wrap, space-between, `gap: 24px`.
    - Copy (`max-width: 448px`, 14px/300, `rgba(255,255,255,0.6)`): `Hiniso products are only
      covered under warranty when purchased through our verified network of specialist retailers.`
    - Link → `/retailers`: inline-flex, `gap: 8px`, 11px/700/`0.2em` uppercase white,
      `border-bottom: 2px solid accent`, `padding-bottom: 4px`, 16px `arrow-right` icon.
      Label: **Find a retailer**. Hover → `accent`.

### 5. Home — Contact / distributor enquiry
`id="contact"`, 96px vertical padding, centered text.
Card: full width, `border: 1px solid rgba(255,255,255,0.15)`, `background: rgba(0,0,0,0.4)`,
padding `64px 32px`.
- `h2` (Space Grotesk, `clamp(28px, 3.5vw, 36px)`/700/`-0.02em`): `Want to become a distributor?`
- `p` (18px, `rgba(255,255,255,0.7)`): `Email us and we will get in touch with you`
- **Form** (`margin-top: 40px`, flex, wrap, centered, `gap: 16px`): email input — height 52px,
  `max-width: 448px`, radius 2px, `border: 1px solid rgba(255,255,255,0.2)`,
  `background: rgba(255,255,255,0.05)`, padding `0 20px`, 15px, white text,
  focus `border-color: accent`; placeholder `Enter your email`. Submit button — height 52px,
  padding `0 32px`, amber, label **Get in touch**.
- **Prototype behavior**: composes a `mailto:tessa@drybox.com.sg` link with subject
  `Distributor Enquiry`. **In production**, replace with a server-side form POST + validation,
  success/error states, and spam protection. Keep `sales@hiniso.com.sg` as the published address.

### 6. Header/footer Contact behavior
Scrolls smoothly to `#contact` with a **64px** offset for the sticky header; instant jump under
`prefers-reduced-motion`. From another page: navigate to Home first, then scroll. In the rebuild,
this is `/#contact` with `scroll-margin-top: 64px` on the section.

### 7. About page (`/about`)
Light background (`paper`), `text-dark`. Three sections, each `border-top: 1px solid hairline-light`.

1. **Premium dry cabinets** — two columns (`1.4fr 1fr` ≥768px, stacked below), `gap: 40px`,
   `align-items: center`, 64px vertical padding. Text left, product photo right
   (`hiniso-product-mtpynq2x-btep.jpg`, radius 6px, 1px light border, white background,
   `object-fit: cover`, lazy). Alt: *Compact Hiniso dry box with digital humidity display*.
   Body: `30 million annually and founded since 2011 to provide premium value dry cabinets and dry
   boxes. All models come with digital hygrometers, built modular for easy parts replacement.
   5-year warranty on electronic parts through authorised distributors.`
2. **Function meets style** — `paper-alt` background. Single column stack, `gap: 40px`,
   64px vertical padding. Centered heading + body on top (`max-width: 621px`, auto margins),
   full-width image below (`hiniso-product_02-mtqm0sww-g1cs.jpg`, radius 6px, 1px light border,
   lazy). Alt: *Two Hiniso dry cabinets, one storing camera bodies and lenses, one storing violins*.
   Body: `Functional and versatile, every Hiniso dry cabinet is sized to fit a range of collections
   and spaces.`
3. **Company** — centered, 64px vertical padding, body `max-width: 768px`:
   `Manufactured in Zhuhai, China, with dedicated R&D, design, production, and sales teams and
   corporate offices in Singapore. Over 50 staff, worldwide revenue above USD 1,000,000 annually.`

### 8. Retailers page (`/retailers`)
Light background, 1152px container, `padding: 56px 24px`.
- `h1` **Authorized retailers**; subhead (16px, `text-muted`)
  `Find a distributor with local warranty and safety mark approved stock`
- **Country tabs** (`margin-top: 28px`, flex, wrap, `gap: 8px`): Singapore, Malaysia, Vietnam, India.
  Each height 40px, radius 6px, padding `0 20px`, 14px/500, 180ms color transitions.
  Inactive — transparent background, `hairline-light` border, `text-muted`.
  Active — `ink` background and border, `oklch(0.97 0.004 250)` text.
  **Implement as links to the country URLs**, not client-side tab state.
- **Listings**: grouped by `state` where present (Malaysia only), each group preceded by an `h2`
  (14px/600/`0.06em` uppercase, `text-muted`) and rendered as a card `<ul>` — radius 8px,
  1px `hairline-light` border, white background, `overflow: hidden`. Groups spaced 32px apart.
- **Row**: flex, wrap, `gap: 16px`, padding 24px, `align-items: flex-start`, space-between,
  `border-top: 1px solid hairline-light`. Left — name `h3`, address (14px, `text-muted`,
  `line-height: 1.6`), optional hours/contact line. Right — an **AUTHORIZED** badge
  (radius 2px, `badge-bg`/`badge-fg`, padding `4px 8px`, 11px/500/`0.06em` uppercase) and, when a
  URL exists, a **Website** button (height 36px, radius 6px, 1px `hairline-light` border,
  padding `0 16px`, 14px, hover `oklch(0.955 0.006 250)`).
- **Why-buy panel** (`margin-top: 40px`, radius 8px, `paper-alt`, padding 24px):
  `h2` **Why buy from an authorized retailer**; body (`max-width: 672px`, 14px, `text-muted`):
  `Local warranty, safety mark approved adapters, and genuine parts. Unauthorized sellers may carry
  grey market stock with no warranty support.`

**Retailer data** — treat as content, not hardcoded markup (CMS collection or JSON). Full dataset,
including addresses, hours, and URLs, is in the `Component.retailers` object in
`design/Hiniso Home.dc.html`; copy it verbatim. Counts: Singapore 2, Malaysia 15 (across
Kuala Lumpur, Selangor, Penang, Perak, Pahang, Malacca, Johor), Vietnam 1, India 1.

### 9. Footer (all pages)
`background: #000`, `border-top: 1px solid rgba(255,255,255,0.1)`.
Grid `repeat(auto-fit, minmax(220px, 1fr))`, `gap: 32px`, padding `48px 24px`.
1. **HINISO** (Space Grotesk 16px/700 white) + `Premium dry cabinets at value prices. Japan
   technology since 2017.` (14px, `rgba(255,255,255,0.6)`, `max-width: 320px`)
2. **EXPLORE** — About (`/about`), Products (`/`), Retailers (`/retailers`)
3. **SUPPORT** — Catalogue (PDF, new tab), User Guide (PDF, new tab), Warranty policy
   (`/retailers`), Contact (`/#contact`)

Link lists: `gap: 8px`, 14px, `rgba(255,255,255,0.7)`, hover `accent`.
Copyright bar: `border-top: 1px solid rgba(255,255,255,0.1)`, padding `20px 24px`, 12px,
`rgba(255,255,255,0.5)`: `© 2026 Hiniso. Manufactured in Zhuhai, China. Corporate offices in
Singapore.`

---

## Interactions & motion

### Scroll reveal (sections and cards)
- Initial: `opacity: 0`, `translateY(48px)`
- Revealed: `opacity: 1`, `translateY(0)`
- Transition: **1900ms** on both properties, `cubic-bezier(0.215, 0.610, 0.355, 1)`
- Trigger: `IntersectionObserver`, `rootMargin: "0px 0px -10% 0px"`, `threshold: 0`; each element
  reveals once
- **Stagger**: elements entering together are grouped by shared parent and row (rounded top
  position), sorted **left to right**, and given `transition-delay` of `index × 240ms`, capped at
  the 6th item
- `will-change: opacity, transform` while pending, cleared once revealed
- Revealed elements: feature cards (staggered), distributor heading block, distributor card,
  contact card, About text blocks and images, retailer group cards, why-buy panel

### Reduced motion / low-end fallback
Everything visible immediately, no transform, no transitions — triggered by
`prefers-reduced-motion: reduce`, `navigator.deviceMemory <= 2`,
`navigator.hardwareConcurrency <= 2`, or missing `IntersectionObserver`.
Ken Burns and staggered entrances are likewise disabled.

### Other transitions
- Buttons/CTAs: `background-color`/`color` 160ms ease; `:active` `scale(0.98)` 120ms
  `cubic-bezier(0.23, 1, 0.32, 1)`
- Dropdown: `opacity 0→1`, `translateY(-4px) scale(0.97)→none`, 160ms
  `cubic-bezier(0.23, 1, 0.32, 1)`, `transform-origin: top left`
- Retailer tabs: 180ms ease on background, color, border
- Header shadow: 220ms `cubic-bezier(0.23, 1, 0.32, 1)`

---

## State (post-rebuild)
Routing replaces most of the prototype's state. What remains client-side:
- `menuOpen` — mobile nav panel
- `downloadsOpen` — desktop Downloads dropdown (hover, 150ms close delay)
- `isStuck` — header shadow, from a scroll listener at `scrollY > 8`
- Reveal observer bookkeeping

Page and country selection become URLs. The retailer dataset should be fetched or built at
build time, not held in a component.

---

## Assets

In `design/`:
| File | Use |
|---|---|
| `assets/hiniso-logo.png` | Header logo (render at 22px height) |
| `assets/favicon.png` | Favicon / apple-touch-icon |
| `assets/hero-desktop.png` | Hero background ≥768px (source 1500×760) |
| `assets/hero-mobile.png` | Hero background <768px (source 800×1200) |
| `hiniso-product-mtpynq2x-btep.jpg` | About — "Premium dry cabinets" |
| `hiniso-product_02-mtqm0sww-g1cs.jpg` | About — "Function meets style" |

**Client-supplied originals** — request higher-resolution masters before launch and generate
responsive AVIF/WebP variants. Give every image explicit `width`/`height`.

**Icons** — the prototype loads Lucide from a CDN. Replace with inline SVG (or the codebase's icon
component) for: `snowflake`, `battery-low`, `gauge`, `lightbulb`, `shield-check`, `arrow-right`.
Menu, close, and chevron icons are already hand-written inline SVG in the prototype — copy those.

**Fonts** — Space Grotesk (500, 700) and DM Sans (300, 400, 500, 700), loaded from Google Fonts in
the prototype. Self-host with `font-display: swap` and preload the display face.

---

## Files
- `design/Hiniso Home.dc.html` — the full prototype: markup, inline styles, motion CSS, and the
  logic class (retailer dataset, feature copy, observers, navigation)
- `design/support.js` — prototype runtime only; **do not port**
- `design/assets/`, `design/*.jpg` — image assets listed above

## Open items for the client
- Confirm the enquiry form's destination address and handling (prototype uses
  `tessa@drybox.com.sg`; footer publishes `sales@hiniso.com.sg`)
- Confirm the About page's "founded since 2011" against the hero's "since 2017"
- Provide high-resolution image masters and any additional retailer entries
