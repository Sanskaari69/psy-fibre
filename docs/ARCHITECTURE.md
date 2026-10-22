# Architecture

PSY. is one static HTML file plus one stylesheet of tokens. No build step and no framework. Every moving part is described below as a mechanism, so the next person can change it without reading all of `index.html`.

## 1 · Page structure (v1: superseded by the seven-scene v2 in the README)

Each band is a full-bleed ground colour with a 1120px inner wrapper. Reveals fire from one shared `IntersectionObserver` (`rootMargin: 0 0 -12% 0`, threshold `0.15`). Nothing in the first viewport is a `.r`, so the first frame is complete before any script runs.

```mermaid
flowchart TB
  classDef plum fill:#100B1D,stroke:#C9A3FF,color:#F6F3FA
  classDef rise fill:#19102B,stroke:#C9A3FF,color:#F6F3FA
  classDef purple fill:#281238,stroke:#9B4DFF,color:#F6F3FA

  bar["Progress hairline · fixed · scaleX = scrollY / max"]:::purple
  hero["HERO · --plum<br/>static HTML copy + canvas<br/>no reveal: visible at first paint"]:::plum
  mq["1 MARQUEE · --purple<br/>speed = 42px/s + scroll velocity"]:::purple
  prob["2 PROBLEM · --plum<br/>.r stagger d=0..4"]:::plum
  fmt["3 FORMAT · --plum-rise<br/>.r stagger d=0..5"]:::rise
  chart["3b FORMAT LANDSCAPE · --plum<br/>inline SVG · .r"]:::plum
  flav["4 FLAVOURS · --plum<br/>3 panels · .r d=1..3<br/>mobile: x-snap scroller"]:::plum
  use["5 HOW TO · --plum-rise<br/>.r d=1..4"]:::rise
  spec["6 TRANSPARENCY · --plum<br/>rows type in via clip-path<br/>pills pop at +380ms · scanline"]:::plum
  packs["7 PACKS · --purple<br/>.r d=1..3"]:::purple
  wait["8 WAITLIST · --plum<br/>submit → preventDefault → data-state=done"]:::plum
  foot["FOOTER · --plum-rise · static"]:::rise

  hero --> mq --> prob --> fmt --> chart --> flav --> use --> spec --> packs --> wait --> foot
  bar -. tracks .-> hero
```

## 2 · Animation pipeline: Format Reveal

One `requestAnimationFrame` loop drives one canvas. `clock` only advances while the loop runs, so pausing off-screen never skips the reveal. Particles, targets and silhouette vertices are stored in normalised units, so a resize rescales the scene and keeps its place in the timeline.

```mermaid
flowchart LR
  load([boot]) --> rm{prefers-reduced-motion?}
  rm -- yes --> still["clock = 3000ms<br/>draw once: final jelly<br/>no wobble · no parallax"]
  rm -- no --> loop

  subgraph loop["RAF loop · DPR ≤ 2 · runs only while hero is on screen and tab visible"]
    direction LR
    pour["0–45% POUR<br/>1400 grains fall from above frame<br/>per-grain delay 0–16%"] --> hes["45–55% HESITATE<br/>swirl clock slows to 0.2×"]
    hes --> col["55–80% COLLAPSE<br/>lerp → superellipse targets<br/>(rejection-sampled, n=4)<br/>size 1→2px · spacing 1.18→1"]
    col --> xf["80–100% CROSSFADE<br/>cloud α 1→0 · jelly α 0→1"]
    xf --> wob["∞ WOBBLE<br/>120 verts · 2 summed sines<br/>amp 1.5% W · caustic drift<br/>+ 40 ambient grains"]
  end

  scroll[/"scroll 0–600px"/] -- "squash y −14% · x +6%<br/>rim ×1.6 · lift 0.3× (cap 12% H)" --> wob
  io[/"IntersectionObserver on canvas"/] -- "off screen → stop RAF" --> loop
  ro[/"ResizeObserver"/] -- "resize backing store, redraw" --> loop
```

Easing throughout is `cubic-bezier(.16,1,.3,1)`, solved in JS by Newton iteration so canvas and CSS share one curve.

## 3 · Asset flow: pack renders

Renders are live (founder renders, claim lines retouched out). Each flavour panel holds a fixed `aspect-ratio: 3/4` drop zone, so the swap cannot shift layout.

```mermaid
flowchart LR
  render["Approved pack render<br/>(still → reviewed at full size)"] --> export["Retouch claim lines out<br/>cut out, 900×1200 WebP<br/>name: &lt;slug&gt;.webp"]
  export --> dir["assets/flavours/"]
  dir --> slot["index.html<br/>.shot[data-slot=flavour-&lt;slug&gt;]<br/>img, object-fit: contain"]
  slot --> css{".shot:has(img)"}
  css -- true --> hide["CSS stick stand-in hidden<br/>img fills box · object-fit: cover"]
  css -- false --> stand["CSS stick stand-in shown<br/>hue = --flavour-N"]
  export --> og["assets/og/psy-og.jpg<br/>1200×630, lineup crop"]
  og --> meta["og:image / twitter:card<br/>in &lt;head&gt;"]
```

## Failure modes designed against

| Failure | Guard |
|---|---|
| JS fails, or a thumbnail capture runs before scripts | `.r` is visible by default; the hidden pre-state applies only under `html.anim`. A no-JS CSS lozenge sits behind the canvas. |
| An IntersectionObserver never fires | A 4s timeout adds `.in` to every `.r`. |
| Canvas blurs or goes blank after DPR/zoom change | `ResizeObserver` rebuilds the backing store at `min(2, DPR)` and redraws. |
| Hero burns battery off screen | RAF stops when the canvas leaves the viewport or the tab hides. |
| `tokens.css` 404 on GitHub Pages subpath | Relative path `design/tokens.css`, no leading slash. |
