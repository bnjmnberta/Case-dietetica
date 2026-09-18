---
target: case-home-catalogo-checkout.html hero
total_score: 16
max_score: 32
na_heuristics: 7,10
p0_count: 2
p1_count: 2
target_identity: "file:C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
target_fingerprint: "sha256:c6c97ca9916b1bde44ddb4a931288114bb3ce2f2a552d2eaf1f7cb6a478ecc18"
target_path: "C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
timestamp: 2026-09-15T19-47-58Z
slug: case-home-catalogo-checkout-html
---
Method: dual-agent (A: Design review: CASE hero section · B: Detector + browser evidence: CASE hero)

## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 3 | Cart count, active tab/pay-tab states clear; disabled "Confirmar pedido" on empty cart is a nice touch |
| 2 | Match System / Real World | 1 | "Nutrición de Alto Rendimiento" performance-gym language doesn't match a neighborhood dietética's real-world mental model |
| 3 | User Control and Freedom | 3 | Qty +/-, trash, category filter all reversible |
| 4 | Consistency and Standards | 1 | Hero's visual language directly contradicts the rest of the same page |
| 5 | Error Prevention | 2 | Required fields exist; no card-number/CVV format validation |
| 6 | Recognition Rather Than Recall | 3 | Prices/qty/thumbnails all visible, nothing to recall |
| 7 | Flexibility and Efficiency | n/a | Not applicable on this landing/checkout surface |
| 8 | Aesthetic and Minimalist Design | 2 | Catalog/cart/checkout are clean; hero is maximalist, drags the average down |
| 9 | Error Recovery | 1 | No inline error messaging beyond native `required` |
| 10 | Help and Documentation | n/a | Not applicable on this surface |
| **Total** | | **16/32** | **Acceptable (50%)** |

## Design Specificity Verdict

**Verdict: violates the documented brand truth.** This hero could belong to a sports-drink, an F1 team, a gym-tech SaaS, or a sneaker launch — nothing in it is legible as "Santa Fe neighborhood dietética."

PRODUCT.md is explicit and says the incumbent identity should be *preserved*:
> "Tipografía: Fraunces (display/serif) + Public Sans (texto) + JetBrains Mono (precios/códigos)."
> "Paleta: verde lima (#8FCB3C) + verde bosque profundo (#0F3D33) + dorado (#C8933A) sobre papel crudo (#F7F8F2)."
> "Tono: orgánico, 'comida real', cálido, con acabado editorial (**no clínico, no genérico-fitness neón**)."

Checked against the current hero:
- **Type**: Outfit (heavy uppercase) carries the headline; Fraunces is demoted to a small italic accent on 3 words. Public Sans is absent (replaced by Geist). JetBrains Mono is absent — even the order-code pill uses `font-family:"Geist",monospace`, which mislabels a proportional sans as monospace.
- **Palette**: `--lime:#9CE71C` is far hotter/more saturated than the documented `#8FCB3C`. Forest green `#0F3D33` is gone, replaced by generic `--ink:#111615`. Gold `#C8933A` is dropped entirely, replaced by an undocumented teal `#1B8D5C`.
- **Tone**: PRODUCT.md names the exact thing to avoid — "genérico-fitness neón" — and the hero (chrome/lime 3D wordmark, glossy gradient, "Nutrición de Alto Rendimiento," "alta performance") is close to a textbook example of it.
- The build's own code comment says it plainly: `// ported from landonorris.com's data-anim-high technique`. That F1-driver-site lineage is legible in the final result.
- **Beyond visuals**: Club CASE — "el motor de negocio... central al producto, no un extra" per PRODUCT.md — is entirely absent: no precio Socio, no tachado price, no CASE-MIE coupon, no DNI/Ley 25.326 field. The category set (5: Proteínas/Snacks/Bebidas/Meal Prep/Suplementos) doesn't match the documented 7 (missing Panificados Dietéticos, Dietas Especiales, Packs, Merchandising). No nutritional tags on any card.

**Deterministic scan**: `impeccable detect --json` found 2 findings, both `low-contrast` (warning/quality), both from the same rule: `.btn-teal{ color:#fff }` on `background:var(--teal)` (#1B8D5C) resolves to **4.2:1**, short of the 4.5:1 AA body-text threshold (16px/700 doesn't qualify for the 3:1 large-text exception). Applies to both primary CTAs: "Ir al checkout" and "Confirmar pedido." Independently confirmed live via `getComputedStyle` — not a false positive. No other automated findings; 0 missing `alt` attributes across 8 images, no viewport overflow at 1280px or 390px, all network requests 200 OK.

## Overall Impression

The "no me convence" instinct is correct and well-founded. The hero is a well-crafted piece of motion work — GSAP `SplitText` line-reveal, a genuinely nice drag-to-rotate 3D interaction — wearing the wrong brand. It reads as "cool tech demo," not "neighborhood dietética." The lower 80% of the page (catalog, cart, checkout) is a calmer, more coherent system that actually resembles the documented identity far more closely; the hero is the part that doesn't belong to its own page. The single biggest opportunity: keep the 3D-rotation mechanic (it's good craft) but re-skin it — Fraunces as the dominant display face, forest-green/gold as the dominant palette, copy pulled back from "alto rendimiento" toward "sin vueltas" — and separately, treat Club CASE's absence as a product-completeness gap, not a style note.

## What's Working

- **The reveal engineering is clean**: proper `SplitText` line-splitting, staggered per-line clip-path + color-wipe timeline, correct use of `document.fonts.ready` to avoid a pre-font-load layout shift. Good craft, independent of brand fit.
- **The catalog/cart/checkout flow is a solid small e-commerce system**: qty stepper, conditional card fields, disabled-until-non-empty submit, empty-cart state, animated success checkmark — sensible micro-states throughout.
- **Real food photography on product cards** directly fixes the exact weakness PRODUCT.md calls out ("todas las fotos de producto son placeholders... esto es lo que el usuario pidió corregir explícitamente") — this part of the brief landed well.

## Priority Issues

**[P0] Hero identity contradicts the documented brand truth**
- **Why it matters**: It's the first thing every visitor sees, and it misrepresents what kind of business CASE is — precisely the source of "no me convence."
- **Fix**: Make Fraunces the dominant display face (not just an italic accent), bring back forest-green `#0F3D33` and gold `#C8933A` as the dominant palette instead of teal/near-black, rewrite the copy away from "alto rendimiento/performance" toward the documented "sin vueltas" voice. Keep the 3D-rotation mechanic — it doesn't need to go, it needs a different material/color/type/copy around it.
- **Suggested command**: `/impeccable quieter` (tone) + `/impeccable typeset` (type hierarchy) + `/impeccable colorize` (palette)

**[P0] Club CASE / precio Socio is entirely missing**
- **Why it matters**: PRODUCT.md calls this "el motor de negocio... central al producto, no un extra." A repeat member who expects to see their savings on every product and a DNI field at checkout finds none of it — this reads as a regression, not an upgrade.
- **Fix**: Add precio Socio + tachado general price + ahorro to each product card; surface the CASE-MIE Wednesday coupon in cart/checkout; add a DNI field (framed under Ley 25.326) to the delivery form.
- **Suggested command**: `/impeccable shape` (re-scope the catalog/checkout data model), then `/impeccable harden`

**[P1] Primary CTA buttons fail WCAG AA contrast**
- **Why it matters**: White text on `#1B8D5C` measures 4.2:1, short of the 4.5:1 body-text minimum — on both "Ir al checkout" and "Confirmar pedido," the two buttons that convert. Confirmed by the detector and independently by live `getComputedStyle` sampling, not a false positive.
- **Fix**: Darken `--teal` (e.g. toward `#146B47` or similar, verify ≥4.5:1) or drop the gold/forest palette back in per the P0 fix above and re-check contrast on whatever color replaces it.
- **Suggested command**: `/impeccable audit`

**[P1] Mobile hero: rotating 3D logo visually interferes with the headline text**
- **Why it matters**: At 390px width, Assessment B confirmed via two screenshots 2 seconds apart that the logo mesh is actively rotating (not a frozen testing artifact) — during part of its rotation, light-green strokes pass directly behind/through the "HÁBITOS" / "COMIDA REAL" letterforms, hurting legibility for what's likely the majority of this audience's traffic.
- **Fix**: On mobile, shrink or reposition the 3D canvas away from the text band, increase `.hero-overlay`'s top padding at the 900px breakpoint, or reduce the logo's rendered scale when the headline is in view.
- **Suggested command**: `/impeccable adapt`

**[P2] Category taxonomy and nutritional tags don't match the documented catalog**
- **Why it matters**: The current 5 categories (Proteínas/Snacks/Bebidas/Meal Prep/Suplementos) don't match PRODUCT.md's 7 (missing Panificados Dietéticos, Dietas Especiales, Packs, Merchandising); no product shows nutritional tags (vegano/sin TACC/keto/apto diabéticos) — exactly the information PRODUCT.md says this audience shops by.
- **Fix**: Align categories to the documented 7; add a tag row to `.pcard-body`.
- **Suggested command**: `/impeccable shape`

## Persona Red Flags

**Jordan (first-time visitor)**: Lands on a spinning chrome logo and "alto rendimiento" copy, reasonably concludes this is a sports-nutrition/energy brand — then scrolls into açaí bowls and wraps and has to mentally re-file what the business even is. That recalibration cost is exactly the "no me convence" feeling, just not consciously named.

**Casey (mobile shopper)**: Very likely hits the logo/headline text collision confirmed above, on top of a heavier hero (WebGL + GSAP SplitText + Lenis) that costs more battery/CPU than a grocery-style browse task justifies on a mid-range Android device.

**Club CASE member (project-specific, derived from PRODUCT.md's "quién compra")**: A repeat customer who joined for precio cuidado and the Wednesday coupon opens this new checkout and finds no membership pricing, no savings shown, no DNI field — for this specific persona the redesign is a step backward from the incumbent site, not an upgrade.

## Minor Observations

- `font-family:"Geist",monospace` on `.code-pill` mislabels a proportional sans as monospace; use an actual monospace stack (or the documented JetBrains Mono).
- No `@media (prefers-reduced-motion: reduce)` anywhere in the page — confirmed by both assessments. The 3D rotation and the GSAP reveal run unconditionally for vestibular-sensitive users.
- Checkout form has no inline error/validation messaging beyond native `required` — a moment of real transaction anxiety gets no supportive feedback.
- Footer copy ("Tu combustible inteligente diario... potenciar tu vida") continues the performance-brand voice site-wide rather than containing the mismatch to the hero.
- Cart-item thumbnails' alt text is just the product name, without quantity/variant context for screen-reader users scanning the list.
- `.reveal-wipe` divs are appended into each `.line` on every load with no cleanup — low risk today since the reveal never re-runs, but a latent leak if it's ever retriggered.

## Questions to Consider

1. If Club CASE is the business's actual engine per PRODUCT.md, was leaving it out of this redesign a deliberate scope cut, or did the hero's exploration quietly crowd out the product requirements over the session?
2. The hero's own code credits landonorris.com as its technique source — was a Santa Fe dietética ever really the intended reference, or did "cool 3D reveal" end up steering the brand more than the brand steered the technique?
3. Is the fix a materials/color/copy re-skin of the existing chrome-logo rig (keep the mechanic, change everything around it), or does the F1-site lineage make it the wrong mechanic for a "sin vueltas, comida real" brand regardless of reskinning?
