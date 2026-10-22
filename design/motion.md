# Motion

One curve does almost everything: **`--ease-out: cubic-bezier(.16,1,.3,1)`**. It starts fast and lands soft, like a jelly settling. Anything that "arrives" uses it. Symmetric loops use `--ease-io: cubic-bezier(.65,0,.35,1)`.

| Element | Property | Duration | Curve | Trigger |
|---|---|---|---|---|
| Section reveal `.r` | opacity .001→1, translateY 18px→0 | 560ms | ease-out | IO, 15% visible, −12% bottom margin |
| Reveal stagger | transition-delay | `--d` × 60ms | — | per child index |
| Reveal failsafe | adds `.in` to all | 4000ms timeout | — | boot |
| Hero Format Reveal | canvas timeline | 3000ms | ease-out (JS solver) | load |
| · pour | grain position start→swirl | 0–45% (+0–16% delay per grain) | ease-out | — |
| · hesitate | swirl clock ×0.2 | 45–55% | linear | — |
| · collapse | grain → lozenge target | 55–80% | ease-out | — |
| · crossfade | cloud α 1→0, jelly α 0→1 | 80–100% | ease-out | — |
| · wobble | two sines, 3θ @1.15rad/s + 5θ @1.9rad/s | infinite | — | after load |
| · caustic drift | highlight centre | 18s / 23s periods | sine | infinite |
| Hero scroll coupling | squash, rim, lift | 0–600px scroll | linear | scroll |
| Marquee | translateX | 42px/s base + velocity × 0.55 (cap 1100px/s), lerp 0.12 | — | visible |
| Velocity decay | peak-held speed | ×0.02 per second | exponential | each frame |
| Progress hairline | scaleX | per scroll frame | — | scroll |
| Button hover | border, glow, arrow +3px | 220ms | ease-out | hover |
| Button press | translateY 1px, scale .99 | 120ms | ease-out | active |
| Flavour panel hover | baseline hue line scaleX .18→1 | 700ms | ease-out | hover |
| Flavour stick hover | lift 2%, rotate −4°→−1° | 900ms | ease-out | hover |
| Stick sheen | background-position sweep | 5.5s loop, 1.2s offsets | ease-io | infinite |
| Spec row type-in | clip-path inset 100%→0 | 700ms, +70ms per row | ease-out | IO |
| Spec pending pill | opacity, scale .8→1 | 300 / 420ms, +380ms after row | ease-out | IO |
| Spec scanline | top 0→100% | 1.6s, once | ease-io | IO |

## Rules

- Motion explains arrival. Nothing loops for decoration except the jelly wobble and the stick sheen, which are both "material" behaviour.
- No springs, overshoot or bounce. The curve already lands soft.
- `prefers-reduced-motion: reduce` makes every transition and animation instant. The hero draws its final frame once, the marquee stays still, and there is no parallax.
- Never animate layout properties. Use transform, opacity and clip-path only.
