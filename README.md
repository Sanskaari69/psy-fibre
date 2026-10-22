<div align="center">

<img src="assets/brand/psy-wordmark.png" alt="psy" width="220">

**Daily Fibre. A New Form.**

A ready-to-eat psyllium fibre jelly in a single stick: Mango + Lime, Raspberry + Blood Orange, Watermelon + Mint. This is the pre-launch site for India, told in seven scenes. Grains pour into a living jelly, the page floods with gel, the word FIBRE splits open around the stick, the lights come up on the three packs, and thirty sticks fly into their packs.

[**Live site →**](https://sanskaari69.github.io/psy-fibre/) &nbsp;·&nbsp; [Design system](design/DESIGN_SYSTEM.md) &nbsp;·&nbsp; [Motion](design/motion.md) &nbsp;·&nbsp; [Architecture](docs/ARCHITECTURE.md) &nbsp;·&nbsp; [Claims rulebook](docs/COMPLIANCE.md)

<img src="assets/og/scroll-demo.gif" alt="Scroll-through of the psy site: format reveal, jelly flood, FIBRE split, flavour laboratory, packs assembling" width="720">

</div>

---

## Architecture

### Page structure: seven scenes

```mermaid
flowchart LR
  classDef plum fill:#100B1D,stroke:#C9A3FF,color:#F6F3FA
  classDef rise fill:#19102B,stroke:#C9A3FF,color:#F6F3FA
  classDef purple fill:#281238,stroke:#9B4DFF,color:#F6F3FA
  H["01 Hero<br/>wordmark + canvas reveal"]:::plum --> M["Marquee<br/>scroll velocity"]:::purple --> F["02 Jelly flood<br/>WebGL · pinned 260vh"]:::plum --> P["Problem<br/>.r reveal"]:::plum --> A["03 FIBRE splits<br/>pinned 230vh"]:::rise --> C["Landscape chart"]:::plum --> LU["03b Lights up on the line<br/>pinned 220vh ≥900px"]:::plum --> L["04 Flavour lab<br/>horizontal pin ≥900px"]:::plum --> K["05 Packs assemble<br/>pinned 240vh · the twist"]:::purple --> S["06 How to + spec sheet"]:::rise --> W["07 Final composition<br/>waitlist"]:::plum
```

### Animation pipeline: Format Reveal

```mermaid
flowchart LR
  B([load]) --> R{reduced motion?}
  R -- yes --> Z["final jelly, drawn once"]
  R -- no --> A["pour · 1400 grains"] --> HS["hesitate"] --> CL["collapse onto<br/>superellipse targets"] --> X["crossfade to<br/>gradient jelly body"] --> WB["wobble forever<br/>+ 40 ambient grains"]
  SC[/"scroll 0–600px"/] -- "squash · rim · lift 0.3×" --> WB
  IO[/"hero off screen"/] -- "pause RAF" --> A
```

### Asset drop flow

```mermaid
flowchart LR
  RN["Founder render"] --> RT["retouch claim lines<br/>cut out"] --> WP["&lt;slug&gt;.webp<br/>900×1200"] --> D["assets/flavours/"] --> SL[".shot[data-slot]<br/>fixed 3/4 box"] --> NS["zero layout shift<br/>CSS stand-in hides"]
  WP --> OG["assets/og/psy-og.png"] --> MT["og:image"]
```

Deeper diagrams and the failure-mode table: [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md).

---

## Design system

| | |
|---|---|
| **Ground** | `--plum #100B1D` · `--plum-rise #19102B` · `--purple #281238` |
| **Light** | `--violet #9B4DFF`, rim light and accents only · `--rim #C9A3FF` hairlines |
| **Type** | `--paper #F6F3FA` · `--mute #9A8FAE` |
| **Faces** | Bodoni Moda (display) · Hanken Grotesk (body) · IBM Plex Mono (utility) |
| **Scale** | 13 / 15 / 17 / 22 / 30 / 44 / 68 / 104, fluid |
| **Surfaces** | Matte. 1px hairlines, ground shifts, 2px panel radius, 999px pills |
| **Motion** | `cubic-bezier(.16,1,.3,1)` for everything that arrives |

The rule that holds it together: violet is light hitting an edge, never paint.

---

## Run it

No build step.

```bash
git clone https://github.com/Sanskaari69/psy-fibre.git
cd psy-fibre
python3 -m http.server 5190
# open http://localhost:5190
```

Every push to `main` deploys to GitHub Pages through [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml). The workflow first runs a **claims gate** that fails the deploy if the page contains any banned format word.

## Structure

```
psy-fibre/
├── index.html            single-file site · inline CSS + JS · imports design/tokens.css
├── design/
│   ├── tokens.css        colour, type, space, motion: the only place values live
│   ├── DESIGN_SYSTEM.md
│   └── motion.md         every animation with its duration and curve
├── docs/
│   ├── ARCHITECTURE.md   mermaid: page, animation pipeline, asset flow
│   ├── CONTENT.md        every string on the site, for copy review
│   ├── ASSETS.md         verified asset map
│   └── COMPLIANCE.md     what may and may not be said
├── assets/
│   ├── flavours/         per-flavour stick renders (transparent)
│   ├── scenes/           full-bleed scene images
│   ├── og/               share cards, scroll demo
│   └── favicon/
└── .github/workflows/deploy.yml
```

## Engineering notes

- **Readable before script.** Headline, copy and CTAs are plain HTML at full opacity. Reveal pre-states apply only under `html.anim`, which JS adds at boot, and a 4s failsafe reveals everything.
- **Canvas 2D + one WebGL shader, vanilla JS.** No framework, no GSAP, no Three.js. Holds 60fps at 4× CPU throttle at phone size (hero and jelly flood measured). DPR capped at 2, 1.5 on phones; the flood renders at 0.5× on phones.
- **Pinned scenes** are tall and sticky only when motion is on. With JS off or reduced motion, every scene is a normal static section.
- **Canvas 2D hero.** The RAF loop stops when the hero leaves the viewport. State is stored in normalised units, so a resize never restarts the animation.
- **Zero layout shift** on asset swap: every drop zone is a fixed `aspect-ratio: 3/4` box.
- **`prefers-reduced-motion`**: final frame only, no wobble, no parallax, static marquee.

## Placeholders

Every value below renders as `—` with a `pending` pill until it is confirmed in writing.

| Field | Where it shows | Unlocked by |
|---|---|---|
| Psyllium husk per serving | Spec sheet | Formulation lock (Gate 1) |
| Total dietary fibre | Spec sheet | Lab analysis (Gate 4) |
| Added sugar | Spec sheet | Lab analysis (Gate 4) |
| Energy | Spec sheet | Lab analysis (Gate 4) |
| Net quantity per stick | Spec sheet | Pilot batch (Gate 2) |
| Gelling system | Spec sheet | Formulation lock (Gate 1) |
| Storage | Spec sheet | Shelf-life study (Gate 4) |
| Shelf life | Spec sheet | Shelf-life study (Gate 4) |
| FSSAI category | Spec sheet | Regulatory review (Gate 5) |
| Manufactured by | Spec sheet | Manufacturer selection (Gate 2) |
| Flavour tasting notes (×3) | Flavour laboratory | Sensory panel (Gate 3) |
| Flavour pack renders (×3) | `assets/flavours/*.webp`, `assets/scenes/lineup.webp` | Live as retouched renders; swap for final artwork after formulation lock |
| 30-day still life | Final scene | A render without a straw, the "6 G FIBRE" print or garbled labels |
| psy 10 and psy 30 price | Packs | Costing and pilot batch |
| FSSAI licence number | Footer | Licence issued (Gate 5) |
| Registered address | Footer | Company registration |
| Waitlist backend | Final scene | The form stores nothing yet; choose a provider before launch |

---

<sub>Code: MIT. The PSY. name, wordmark and brand assets are not licensed for reuse. PSY. is a food product, not a medicine.</sub>
