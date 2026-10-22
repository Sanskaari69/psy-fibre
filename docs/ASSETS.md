# Asset map

Verified 22 Oct 2026 in headless Chrome at 1440, 768, 390 and 360: every reference returned 200 with no console errors.

| File | Purpose | Used by (`index.html`) | Fit / box | Status |
|---|---|---|---|---|
| `assets/brand/psy-wordmark.png` (985×417, transparent) | Supplied `psy` logo, never retyped | header link, hero, both pack boxes, final scene, footer, `.stick` stand-in (CSS), `<link rel=preload>` | natural ratio, width set per slot | ✅ loads everywhere |
| `assets/scenes/lineup.webp` + `lineup.jpg` fallback (1672×941) | Scene 03b: three sticks with fruit | `<picture>` in `.line` | desktop `cover` 72% 50%; phone 5:4 crop at 80% 60% | ✅ "6 G FIBRE" / "15 g" retouched out |
| `assets/flavours/mango-lime.webp` (900×1200, transparent) | Mango + Lime still | `.f-scene[data-flavour=mango-lime] .shot` | `contain`, no distortion | ✅ correct flavour |
| `assets/flavours/raspberry-blood-orange.webp` | Raspberry + Blood Orange still | `[data-flavour=raspberry-blood-orange]` | `contain` | ✅ correct flavour |
| `assets/flavours/watermelon-mint.webp` | Watermelon + Mint still | `[data-flavour=watermelon-mint]` | `contain` | ✅ correct flavour |
| `assets/og/psy-og.jpg` (1200×630, 116 KB) | Share card | `og:image`, `twitter:image` (absolute URLs) | — | ✅ |
| `assets/favicon/favicon.svg` | Tab icon | `<link rel=icon>` | — | ✅ |
| `assets/og/scroll-demo.gif` | README demo | README only | — | ✅ not used on the page, by design |
| `design/tokens.css` | Tokens | `<link rel=stylesheet>` | — | ✅ |

## Missing

| Asset | Needed by | Why it is missing |
|---|---|---|
| 30-day still life (was `assets/og/still-life.webp`, referenced only inside an HTML comment, now removed) | Final scene `#waitlist` | The only supplied render (tumbler desk shot) has a straw, which reads as a drink; it also has garbled labels and a "6 G FIBRE" print. |
| Final pack artwork | Scene 03b and flavour scenes | Current images are retouched renders; caption on the page says so. |
