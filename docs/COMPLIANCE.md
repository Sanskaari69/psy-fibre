# Claims rulebook

PSY. is a **food product**. Every string on every PSY. surface (site, label, deck, ad, social) passes this page before it ships.

## The product, stated correctly

- PSY. is a **ready-to-consume psyllium fibre jelly in a single stick**.
- Never write, draw, render or imply powder, sachets to empty into water, shakers, stirring, or a glass of fibre drink. That format is dead.

## May say: format benefits

| ✅ Allowed | Why |
|---|---|
| One stick, one serving. Nothing to measure. | Describes the format |
| No water source required to take it. | Format fact. Always paired with the hydration advisory |
| Five seconds, once a day. | Describes the ritual, not an outcome |
| Contains psyllium husk. | Ingredient fact (quantity stays `[pending]` until the lab confirms it) |
| Fibre, finally in good taste. | Sensory/brand line, not a health outcome |

## May not say: health, medical, efficacy

| ❌ Banned | Category |
|---|---|
| relieves / treats / cures constipation, IBS, piles | Disease claim |
| detox, cleanse, flush | Unsubstantiated function claim |
| weight loss, appetite control, "keeps you full" | Efficacy claim |
| gut health, healthy gut, microbiome | Function claim pending validation |
| improves absorption, lowers cholesterol, blood sugar | Health claim (needs FSSAI-approved substantiation) |
| clinically proven, doctor recommended | Invented validation |
| "high in fibre", "source of fibre" | Nutrient content claim. Allowed **only** once a lab-verified fibre value meets the FSSAI threshold (see `psy-internal/research`) |

## Numbers

- **Invent nothing.** Fibre grams, calories, sugar, shelf life, FSSAI licence number, price, manufacturer, MOQ, certification and clinical results all render as `—` with a `[pending]` pill until confirmed in writing.
- If a plausible-sounding number appears in a draft, it is replaced with `[pending]`.

## Mandatory advisory (all surfaces that describe use)

> Psyllium is a swelling fibre. Always take with adequate water. Final serving directions, warnings and contraindications will follow formulation lock and professional review.

## Conduct

- No fake urgency, countdowns, "only N left", or numeric scarcity.
- No invented reviews, testimonials, ratings or press logos.
- The waitlist form does not transmit data until a backend is chosen. The confirmation copy says so.

## Banned words (site text scan, must return 0)

`powder` · `sachet` · `mix` · `stir` · `shaker` · `dissolve`

The scan is substring-based, so it also catches CSS such as `mix-blend-mode`. Don't use those properties.

```bash
for w in powder sachet mix stir shaker dissolve; do printf "%-9s %s\n" "$w" "$(grep -oi "$w" index.html | wc -l)"; done
```
