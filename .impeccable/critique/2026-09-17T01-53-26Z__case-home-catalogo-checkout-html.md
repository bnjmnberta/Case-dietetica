---
target: case-home-catalogo-checkout.html
total_score: 29
max_score: 40
na_heuristics: 
p0_count: 0
p1_count: 2
target_identity: "file:C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
target_fingerprint: "sha256:4a9f0064a92139c578c1943350922777e60d8ed3816189fa9731db8619394deb"
target_path: "C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
timestamp: 2026-09-17T01-53-26Z
slug: case-home-catalogo-checkout-html
---
Method: dual-agent (A: aa17d2fb9b74b7a45 · B: a81802b08691de420)

# Critique (re-run) — case-home-catalogo-checkout.html

## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 3 | Cart badge, "Procesando...", live totals all work; sticky navbar clips section headings on anchor jump |
| 2 | Match System / Real World | 4 | Precio General/Socio, DNI+Ley 25.326, Wednesday banner — genuinely domain-accurate |
| 3 | User Control and Freedom | 3 | Good per-item controls; no undo on remove, no way to edit/cancel a placed order |
| 4 | Consistency and Standards | 3 | Visually consistent; role="tablist"/role="tab" declared on both tab groups without the roving-tabindex/arrow-key behavior that role promises |
| 5 | Error Prevention | 2 | Confirm button disables on empty cart; validation is submit-only, no live/blur-time checks |
| 6 | Recognition Rather Than Recall | 4 | Prices, active state, DNI purpose all always visible — no memory burden anywhere tested |
| 7 | Flexibility and Efficiency | 2 | Zero autocomplete attributes on any checkout field — blocks autofill entirely |
| 8 | Aesthetic and Minimalist Design | 4 | Strongest dimension — type system, whitespace, real photography |
| 9 | Error Recovery | 3 | Inline Spanish messages are specific and plain-language; don't clear live as user retypes |
| 10 | Help and Documentation | 1 | No FAQ, no delivery-time estimate, no "¿Dudas?" near checkout |
| **Total** | | **29/40** | **Good** |

## Design Specificity Verdict

**LLM assessment:** Mostly earned. Real food photography reads as one coherent shoot, copy is convincingly rioplatense, Club CASE/DNI/Ley 25.326 framing is Argentina-specific, not boilerplate. The tell: --gold:#C8933A — one of PRODUCT.md's three named brand colors — is declared in :root and never used anywhere in the file. Small visually, but it's the fingerprint of a template that absorbed the palette into variables without ever deciding where gold belongs.

**Deterministic scan:** impeccable detect --json returns zero findings (exit 0) — still clean after this session's five rounds of edits. Sanity-checked: the Fraunces overused-font ignore rule is still active and still suppressing a real match (7 occurrences), not masking anything else since there's nothing else to mask.

**Live verification (not just code-reading):** Both agents interacted with the actual page. All 5 of this session's fixes were independently re-confirmed working live: cart badge increments on Agregar, hero fallback hides only after a confirmed 3D load, pickup toggle correctly zeroes shipping and unrequires address, Mercado Pago tab shows its note and hides card fields, empty-submit shows inline Spanish errors (no native bubbles), and a full submission empties the cart. The category-grouped catalog was confirmed live at both viewport widths with no overlap or clipping.

## Overall Impression

Real jump: 24 → 29/40. The category-grouping fix is the standout — both assessments independently confirmed it actually solves the flat-wall problem, not just cosmetically. But this pass also surfaced a self-inflicted issue: adding role="tablist"/role="tab" to the payment and fulfillment tabs (done in the last polish pass, for consistency) without the arrow-key roving-tabindex behavior that role contractually promises to screen-reader/keyboard users — that's worse than not using the pattern at all. The other new-since-last-time finding, a sticky navbar that clips the "Finalizar Pedido" heading on anchor jump, lands at the worst possible moment: right as a visitor commits to paying.

## What's Working

1. The category-grouped catalog fix is fully verified, not just claimed — confirmed live at 1440px and 390px: 7 legible sub-sections, real photography, no overlap. The most impactful fix of the five.
2. The hero 3D→fallback handoff works exactly as designed — confirmed live: canvas renders, heroLogoFallback hides only after the model's load callback fires (network log shows both .obj/.mtl returning 200 before the hide).
3. Checkout validation and success flow are all real — inline Spanish errors, pickup toggle, Mercado Pago, "Procesando...", and post-order cart-clear all verified via live interaction across two independent agents.

## Priority Issues

**[P1] Sticky navbar clips section headings on in-page anchor jump**
Why it matters: confirmed live on mobile — clicking "Ir al pago" scrolls #checkout to y=0, then the persistent sticky bar renders on top, visibly cutting the "Finalizar Pedido" heading in half. This happens at the exact moment a visitor commits to paying — the worst place in the flow for a "did this even work" glitch.
Fix: give the Lenis scrollTo calls an offset equal to catBar.offsetHeight, or add scroll-margin-top to anchor-target sections matching the stuck-bar height.
Suggested command: /impeccable harden

**[P1] ARIA tablist pattern declared but not implemented**
Why it matters: role="tablist"/role="tab"/aria-selected are on both tab groups (added this session), but there's no roving tabindex and no arrow-key handler — verified via JS inspection. A screen-reader user is told "use arrow keys" by the semantics, and arrow keys do nothing. This is a regression from the last polish pass, not a pre-existing gap.
Fix: either implement roving tabindex + arrow-key handling per the ARIA APG tabs pattern, or drop to plain button-group semantics that match the actual click-only interaction.
Suggested command: /impeccable harden

**[P2] Success screen doesn't recap the order**
Why it matters: after a Ley 25.326-covered data submission and a payment commitment, #successPanel shows only a checkmark, generic sentence, and order code — no items, total, or restated delivery/pickup detail. The peak-end moment under-delivers relative to the effort just spent.
Fix: snapshot cart/total/fulfillment choice before clearCart() runs, render a one-line recap in the success panel.
Suggested command: /impeccable clarify

**[P2] Category filter row still shows 8 simultaneous options, now duplicating the grouped-catalog taxonomy**
Why it matters: Todos + 7 pills sit permanently in the sticky navbar, above the catalog's own 7 category headings — the same taxonomy is now presented twice on screen at once. Pre-existing, not introduced this session, but worth naming since the catalog fix changed its context.
Fix: collapse to a dropdown/sheet, or keep the mobile horizontal-scroll pattern at all breakpoints with a scroll-affordance cue instead of wrapping to two rows on mid-desktop widths.
Suggested command: /impeccable layout

**[P2] Checkout fields have no autocomplete attributes**
Why it matters: verified in source — ckNombre, ckDireccion, ckCardNum, ckVenc, ckCvv carry no autocomplete hints, blocking browser/password-manager autofill for a program whose whole premise (Club CASE) is repeat purchasing.
Fix: add standard tokens — autocomplete="name", "shipping street-address", "cc-number", "cc-exp", "cc-csc".
Suggested command: /impeccable harden

## Persona Red Flags

**Jordan (first-timer):** hits the clipped "Finalizar Pedido" heading right as they commit to paying — exactly the audience most likely to abandon at a glitch they have no prior context to explain away.

**Sam (accessibility-dependent):** the broken tablist pattern actively contradicts what their screen reader/keyboard workflow expects at the point of selecting a payment method. Worth noting positively: aria-live="alert" on inline field errors and aria-invalid/aria-describedby wiring are correctly implemented.

**Alex (power-user/repeat buyer):** no autocomplete, no saved address, no quick-reorder — every visit is a cold-start form fill, despite Club CASE explicitly wanting this person back weekly.

## Minor Observations

- Mobile category-scroll strip has no fade/arrow cue at its trailing edge — a user may not realize Dietas Especiales/Bebidas/Packs/Merchandising exist past "Panificados..." unless they happen to swipe.
- DNI placeholder ("Sin puntos") is an instruction while Nombre/Dirección placeholders are example values — inconsistent placeholder pattern within the same field group.
- No "pago seguro" reassurance near the card fields specifically; the only trust copy (Ley 25.326) is scoped to the DNI/delivery panel.
- Add-to-cart gives no feedback at the click itself — only the off-to-the-side navbar badge updates.
- --gold remains a dead CSS variable.
- The in-memory cart seeds with demo data (acai:1, wrap:2) on every fresh load rather than starting empty — confirmed at runtime, pre-existing from before this session, intentional-looking demo scaffolding rather than a bug, but worth being aware of if this ever moves toward a real backend.

## Questions to Consider

1. Now that the catalog groups by category, does the 8-pill filter bar still need to exist above the fold — or did grouping make it redundant duplication of the same taxonomy?
2. The success screen says less than the cart's own summary panel said thirty seconds earlier — what would it look like if the checkout's own state simply carried forward into the confirmation instead of disappearing?
3. Club CASE is "el motor de negocio, no un extra" per the brief, yet joining it requires leaving this page entirely, mid-checkout, DNI half-typed — what would inline enrollment at the highest-intent moment look like?
