# CrownCalm — Sanctuary Suite
### Next.js 14 · Tailwind CSS · Brutalist-Editorial DTC

---

## Visual Identity — Read This First

The design is **light mode, cream base, brutalist-corporate editorial**.

| Element | Value |
|---|---|
| Background | `#F2ECE4` — warm cream |
| Text primary | `#1A1A1A` — deep charcoal |
| Text secondary | `#666666` — mid grey |
| Display font | Playfair Display (editorial serif, weight 400) |
| Mono font | Space Mono (ALL CAPS labels, body, UI, specs) |
| Primary accent | `#2563EB` — indigo (CTAs, links) |
| Warm accent | `#E8624A` — terracotta (secondary CTA, emotional peak) |
| Spacing base | 8px · section/card padding: 46px |

**Brand signature — the Dissolve Block:** Mechanism/phase headings use a solid
color rectangle that dissolves rightward into a dithered pixel-art checkerboard pattern.
See the Master Strategy design reference. This appears on section labels, timeline phases,
and mechanism tags. It is the single most distinctive visual element of the system.

**Structural grid rules:** Ultra-thin vertical lines (`rgba(26,26,26,0.07)`) divide
sections into columns as a background element — like the quarterly columns in the
design reference. Applied via the `.has-grid-rules` utility class.

**Reference aesthetic:** The uploaded Master Strategy design (cream background,
Playfair + Space Mono, dithered color blocks, strict columnar grid).

---

## Stack

| Layer | Choice |
|---|---|
| Framework | Next.js 14 (App Router) |
| Styling | Tailwind CSS + CSS custom properties (tokens.css) |
| Animations | Framer Motion |
| Canvas BG | Vanilla Canvas 2D (dot-matrix, pointer drift) |
| Fonts | Playfair Display + Space Mono (Google Fonts) |
| Deploy | Vercel |

---

## Project Structure

```
crowncalm/
├── app/
│   ├── layout.tsx              # Root layout — nav, fonts, canvas bg
│   ├── page.tsx                # Home page
│   ├── products/
│   │   └── page.tsx            # Product listing — Specimen Expander
│   ├── products/[slug]/
│   │   └── page.tsx            # PDP
│   ├── science/
│   │   └── page.tsx            # Science / mechanism deep-dive
│   └── blog/
│       └── page.tsx            # Blog index
├── components/
│   ├── Nav.tsx                 # Transparent → floating island pill
│   ├── CanvasBackground.tsx    # Dot-matrix canvas (cream-mode, faint indigo)
│   ├── DissolveBlock.tsx       # Brand signature dithered dissolve rect
│   ├── HeroSection.tsx         # Home S1
│   ├── TrustBar.tsx            # Overlapping avatars + stars + drawer
│   ├── ReviewsDrawer.tsx       # Slide-up review cards
│   ├── BucketTheory.tsx        # Home S2 — animated overflow diagram
│   ├── PainAcknowledgment.tsx  # Home S3 — 3-column typographic
│   ├── FourPillars.tsx         # Home S4 — expandable panels
│   ├── CompetitorMatrix.tsx    # Home S5 — comparison table
│   ├── SocialProof.tsx         # Home S6 — 3 zones
│   ├── ThirtyDayChallenge.tsx  # Home S7 — 30-day timeline
│   ├── PriceJustification.tsx  # Home S8 — ROI stack
│   ├── IdentityShiftCTA.tsx    # Home S9 — Emotional Delta CTA
│   ├── SpecimenExpander.tsx    # /products — 5-product accordion
│   ├── ProductBento.tsx        # PDP — asymmetric image grid
│   └── SpecTable.tsx           # PDP — spec table with mechanism reasons
├── data/
│   └── products.ts             # All product data, specs, copy
├── styles/
│   └── tokens.css              # All design tokens — source of truth
├── utils/
│   └── dissolve.ts             # Dithered dissolve block canvas generator
└── public/
    └── images/
        ├── hero-suite.webp
        └── products/
```

---

## Google Fonts — Add to app/layout.tsx

```tsx
import { Playfair_Display, Space_Mono } from 'next/font/google'

const playfair = Playfair_Display({
  subsets: ['latin'],
  weight: ['400', '700'],
  style: ['normal', 'italic'],
  variable: '--font-display',
  display: 'swap',
})

const spaceMono = Space_Mono({
  subsets: ['latin'],
  weight: ['400', '700'],
  style: ['normal', 'italic'],
  variable: '--font-mono',
  display: 'swap',
})
```

---

## Pages

### `/` — Home
**Above fold:** Hero (converted traffic) — enter the internal monologue, island nav, canvas bg, trust bar with reviews drawer.

**Below fold (cold traffic conversion arc):**
- S2: Bucket Theory — animated overflow diagram with dissolve-block labels
- S3: Pain Acknowledgment — three typographic columns (Physical / Functional / Identity)
- S4: Four Pillars — expandable mechanism panels with dissolve-block headers
- S5: Competitor Matrix — checkmark table
- S6: Social Proof — 3 strategic zones
- S7: 30-Day Challenge — three-phase timeline
- S8: Price Justification — ROI cost comparison
- S9: Emotional Delta CTA — the final descent

### `/products` — Sanctuary Suite Overview
- Brief intro section
- **Specimen Expander** — 5-product interactive accordion
- Trust bar repeat + CTA

### `/products/[slug]` — Individual PDP
Slugs: `neuro-massager` | `optic-shield` | `cryo-wrap` | `sleep-buds` | `sanctuary-bundle`

### `/science` — Science page (white-paper style, PubMed linked)

### `/blog` — Blog / education index

---

## Nav Behavior

- **Default (scroll = 0):** Full-width, 64px tall, `background: transparent`. Sits over hero.
- **Scrolled (> 100px):** Transforms into a **floating island pill** — centered, max-width 560px, `backdrop-filter: blur(20px)`, `background: rgba(242,236,228,0.88)` (cream frosted glass), `border: 1px solid rgba(26,26,26,0.12)`, `border-radius: 9999px`. Floats 16px from top.
- **Links:** Science · Products · Blog (no "Protocol" link)
- **Right side:** Cart icon (hidden by default, `.cart-visible` class shows it) + "Begin the Protocol" primary button
- **Mobile:** Island always visible. Hamburger opens full-screen overlay.

---

## Specimen Expander (Key Interactive Component)

Location: `/products` page.

**5 rows. One always active. Click to expand.**

**Inactive row (80px tall):**
- Left: product number (01–05, Space Mono), mechanism eyebrow, product name (Playfair Display large), 1-line description
- Right: small square product image (112×112px), flush right
- The inactive row has the dissolve-block color for that product's phase color on the number

**Active row (~100vh):**
- Left panel (55%): large product name + mechanism explanation (2–3 paragraphs) + key specs in `<code>` tags
- Right panel (45%): asymmetric bento grid of 3–4 product images (varied `grid-row/column: span` values)
- Transition: Framer Motion `AnimatePresence` + `layout` prop, 400ms ease-in-out

**Products:**
1. 360° Neuro-Massager — Gate Control Activation — `var(--phase-1)` lavender
2. FL-41 Optic Shield — Spectral Filtration — `var(--phase-2)` magenta
3. Cryo-Compression Core — Thermodynamic Vasoconstriction — `var(--phase-3)` ochre
4. Acoustic Silence Sleep Buds — Auditory Masking — `var(--phase-4)` yellow
5. The Sanctuary Bundle — The Compound Protocol — `var(--phase-5)` cornflower

---

## CTA Language Rules — NEVER violate

| ❌ Never | ✅ Always |
|---|---|
| Buy Now | Begin the Protocol |
| Add to Cart | Start the Protocol |
| Kit / Bundle | Sanctuary Suite |
| Revolutionary / Breakthrough | (omit — use mechanism specificity) |
| Clinically proven / Medical device | Scientifically inspired / Mechanism-based |
| Treats pain / Cures / Prevents | Manages conditions / Down-regulates |
| ! (exclamation points in body) | . (period — calm, precise) |
| AMAZING / BREAKTHROUGH | (delete — never use) |

---

## Design Rules — NEVER override

1. Background is always `var(--color-bg)` — `#F2ECE4`. No white, no dark.
2. Text primary is `#1A1A1A`. Never pure black `#000000`.
3. Buttons use `border-radius: 6px`. The island nav uses `border-radius: 9999px`. These are the only two radius values for interactive elements.
4. No pill-shaped buttons. `var(--radius-btn)` = 6px always.
5. No multi-color gradients on backgrounds. Phase colors are solid blocks with a dithered dissolve edge — not gradient fades.
6. Fonts: Playfair Display for all headings H1–H4. Space Mono for all body, labels, specs, CTAs, eyebrows.
7. Space Mono body text is always uppercase in labels and eyebrows. Sentence case in body paragraphs.
8. All spec numbers (`480–520nm`, `20–40 kPa`, `<45dB`) must be in `<code>` elements using `var(--color-spec)` and `var(--color-spec-bg)`.
9. Canvas background is always z-index 0, pointer-events none, inside a `position: relative` parent.
10. All animations: only `transform` and `opacity`. Never animate `width`, `height`, `top`, `left`.
11. `prefers-reduced-motion` disables all animations (instant opacity).
12. Never use Inter, Roboto, or Open Sans.
13. No decorative blobs, no neon glows, no AI-aesthetic gradients.
14. The dissolve block is the only textural decoration permitted.
15. Spacing must use `var(--space-*)` or `var(--padding-*)` tokens. No hard-coded px values in components.

---

## Product Data Reference

### 1. 360° Neuro-Massager
- **Mechanism:** Gate Control Activation — rhythmic intermittent pneumatic compression
- **Phase color:** `var(--phase-1)` lavender `#D2B6F7`
- **Pressure range:** `20–40 kPa` (therapeutic window — above 40 kPa triggers guarding response)
- **Motor noise:** `<45 dB` (below phonophobic trigger threshold)
- **Heat range:** `38°C–42°C` dual-zone carbon fiber heating elements
- **Auto-shutoff:** 15 min (habituation prevention — not a safety feature)
- **Modes:** Relax / Sleep / Vitality / Beauty / Soothing
- **Key diff:** Pneumatic (organized, rhythmic) vs. vibration (random noise). Gate Control only responds to organized input.

### 2. FL-41 Optic Shield
- **Mechanism:** Spectral filtration of `480–520nm` blue-green light (ipRGC pathway)
- **Phase color:** `var(--phase-2)` magenta `#E596FF`
- **Filtration:** Blocks up to `80%` of the `480–520nm` band
- **Lens:** CR-39 optical-grade polycarbonate — `50%` lighter than glass, zero distortion
- **Indoor transmission:** `50%` — functional in normal lighting
- **Certification:** ANSI Z80.1-2025
- **Adaptation:** `10–15 min` visual cortex white-balance recalibration (pink tint recedes — this is confirmation it's working, not a flaw)
- **Key diff:** Not a blue-light filter. Targets the exact wavelength that activates the ipRGC pain pathway.

### 3. Cryo-Compression Core
- **Mechanism:** Active vasoconstriction at occipital + temporal + carotid arterial sites
- **Phase color:** `var(--phase-3)` ochre `#F5B061`
- **Temperature:** `2°C–8°C` therapeutic window
- **Freeze cycle:** 20 min standard freezer
- **Effective duration:** 30 min per freeze cycle
- **Application rule:** Max 20 min continuous (paradoxical vasodilation after 20 min)
- **Material:** Medical-grade neoprene exterior, S-Class Hydrogel interior
- **Sequencing rule:** Heat (massager 8–10 min) → Cold (cryo-wrap 20 min). NOT cold-only.

### 4. Acoustic Silence Sleep Buds
- **Mechanism:** Passive attenuation + active auditory masking (Brown Noise)
- **Phase color:** `var(--phase-4)` yellow `#FCD667`
- **Passive attenuation:** `30 dB` (seal-fit silicone tips)
- **Driver:** `6mm` micro-dynamic
- **Noise type:** Brown Noise — low-frequency weighted, minimal high-frequency irritant vs. White Noise
- **Shell:** `<1.3cm` profile (side-sleeper threshold — below outer ear profile)
- **Battery:** 10-hour continuous / 48-hour case
- **Connectivity:** Bluetooth 6.0

### 5. The Sanctuary Bundle
- **Phase color:** `var(--phase-5)` cornflower `#6FA4FF`
- **Price:** $345 (one-time, no subscription)
- **Includes:** All 4 devices + The Sanctuary Protocol ebook (instant digital delivery)
- **Ebook:** 130 pages — Bucket Theory, 4-Phase Migraine Cycle, 15-Min Calibration, 30-Day Challenge
- **Guarantee:** 30-Day Protocol Guarantee (follow the protocol; full refund if no meaningful reduction)
- **Key diff:** Only compound 4-mechanism protocol at DTC accessible price point.

---

## Science Copy Bank — Use Verbatim

```
BUCKET THEORY:
"Your migraines aren't random. They're physics."

GATE CONTROL (massager):
"Every other head massager vibrates. Vibration is disorganized.
Disorganized input doesn't close the pain gate. This one breathes.
Rhythmic. Organized. Gate-closing."

SUB-VISIBLE FLICKER (glasses):
"The lights you're reading this under right now flicker
100–120 times per second. You can't see it.
Your nervous system registers every cycle."

COLD SEQUENCING (cryo-wrap):
"Ice during the attack. Warmth after the storm.
Most people do this backwards."

BROWN NOISE (buds):
"White Noise is full-spectrum noise. For a sensitized
auditory system, it's a new stressor disguised as a solution.
Brown Noise masks. It doesn't add."

IDENTITY CTA:
"You are not a migraine victim.
You are someone who has been given the wrong tools."

COMPETITOR REFRAME:
"You haven't failed migraine management.
Single-mechanism solutions have failed you."
```

---

## Trust Signals — TrustBar Component

```
[5 overlapping avatars] ★★★★★ 4,247 verified outcomes
· Medically Reviewed · ANSI Z80.1-2025 Certified · 30-Day Protocol Guarantee · Instant Ebook Delivery
```

Clicking the avatar+star cluster opens the ReviewsDrawer component.

---

## Getting Started

```bash
npx create-next-app@latest crowncalm --typescript --tailwind --app
cd crowncalm
npm install framer-motion
mkdir -p styles utils public/images/products
# copy tokens.css into styles/tokens.css
# add @import '../styles/tokens.css'; at top of app/globals.css
npm run dev
```

---

## References
- Brand & conversion strategy: Master Brand & Conversion Brief v1.0
- Science content: The Sanctuary Protocol ebook (130 pages)
- Design system: Master Strategy design reference (cream, Playfair + Space Mono, dithered dissolve)
- Layout patterns: CrownCalm Section Layout Bible (270 variations)
- Store layout reference: apolloneuro.com (conversion architecture)
