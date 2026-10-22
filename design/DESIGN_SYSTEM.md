# PSY. Design System

**One idea:** violet is light, not paint. The brand lives on matte plum ground, and violet only appears where light would hit an edge: a rim, a hairline, a focus ring, a glowing numeral.

Source of truth: [`tokens.css`](tokens.css). Nothing in `index.html` hardcodes a colour or a font size.

## Palette

| Token | Hex | Role |
|---|---|---|
| `--plum` | `#100B1D` | Page ground |
| `--plum-rise` | `#19102B` | Raised bands |
| `--purple` | `#281238` | Deep accent ground (marquee, packs) |
| `--violet` | `#9B4DFF` | Rim light and accents **only**. Never a large fill, never a gradient hero |
| `--rim` | `#C9A3FF` | Hairline highlights, italic display accents |
| `--paper` | `#F6F3FA` | Type |
| `--mute` | `#9A8FAE` | Secondary type, a violet-biased grey (never neutral grey) |
| `--line` | `rgba(201,163,255,.10)` | 1px separators |
| `--flavour-1/2/3` | `#7A5CFF` `#C77DFF` `#E25CFF` | Flavour panel accents only, all from the violet family |

Single dark theme. `color-scheme: dark` on `:root`, explicit background on `body`, no light mode.

## Type

| Role | Family | Weights | Rules |
|---|---|---|---|
| Display | Bodoni Moda | 400, 500 | Leading 0.95–1.05, never under 28px, italic for the accent word |
| Body | Hanken Grotesk | 300–600 | Measure capped at 65ch |
| Utility | IBM Plex Mono | 400, 500 | Uppercase, 0.14em tracking: eyebrows, spec tables, pack labels, legal lines |

Scale: **13 / 15 / 17 / 22 / 30 / 44 / 68 / 104**, fluid via `clamp()` (`--t-13` … `--t-104`). `text-wrap: balance` on every heading.

## Surfaces

- Matte. No glassmorphism, no card shadows.
- Separation comes from 1px `--line` hairlines and ground shifts between `--plum` and `--plum-rise`.
- Radius: **2px** hairline panels, **999px** pills, **0** everywhere else.

## Layout

- Full-bleed bands, inner wrapper max 1120px.
- Gutters: `max(16px, env(safe-area-inset-*))` on each band's inner wrapper; vertical spacing via `padding-block` only.
- Sibling spacing via `gap`, never per-element margins. `min-width: 0` on text-holding grid/flex children.
- Left-weighted, asymmetric. Nothing centred by default.
- Works down to 360px with no horizontal page scroll.

## Banned

Cream grounds · terracotta · acid-green pops · purple-to-blue gradients · Inter · Space Grotesk · emoji section markers · centred everything · uniform rounded cards with accent rails.

## Components

- **Eyebrow:** mono 13px, `--rim`, preceded by an 18px violet hairline with glow.
- **Button:** pill, mono uppercase, 48px min height. Solid = paper on plum. Ghost = hairline border. Hover = rim border + violet halo.
- **Pending pill:** violet 1px outline, mono 10.5px uppercase, `--rim` text. It marks every unconfirmed value.
- **Spec table:** mono, tabular numerals, hairline rows, label left, value right.
- **Drop zone `.shot`:** fixed 3/4 box. A CSS stick stand-in shows until an `<img>` is present.

Motion: see [`motion.md`](motion.md).
