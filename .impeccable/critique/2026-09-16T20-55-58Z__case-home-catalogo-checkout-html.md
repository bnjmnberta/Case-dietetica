---
target: case-home-catalogo-checkout.html
total_score: 24
max_score: 40
na_heuristics: 
p0_count: 0
p1_count: 4
target_identity: "file:C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
target_fingerprint: "sha256:b22eae5c0e85546bcfb9a33c417bbed6d145045496a189597bf1b8f94dbe9de5"
target_path: "C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
timestamp: 2026-09-16T20-55-58Z
slug: case-home-catalogo-checkout-html
closed: true
---
Method: dual-agent (A: a810216b5986d9087 · B: aeaca308d208289ee)

# Critique — case-home-catalogo-checkout.html

## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 2 | `#cartCount` targeted by JS but doesn't exist in markup — no cart badge anywhere; checkout has no processing state |
| 2 | Match System / Real World | 3 | Correct es-AR pricing/taxonomy, but no Mercado Pago and shipping charged even though pickup is implied |
| 3 | User Control and Freedom | 3 | Good qty/remove/filter controls; no way back from checkout, no post-order edit |
| 4 | Consistency and Standards | 3 | `.cat-tab.active` and `.pay-tab.active` are the same "selected chip" pattern with inverted color logic |
| 5 | Error Prevention | 2 | Cart/payment field toggling is solid; zero input masking on DNI, card number, vencimiento, CVV |
| 6 | Recognition Rather Than Recall | 2 | No cart badge forces users to remember what they added across a 21-card scroll |
| 7 | Flexibility and Efficiency | 2 | No search, no combinable dietary filters, no saved address/payment despite a repeat-purchase loyalty model |
| 8 | Aesthetic and Minimalist Design | 3 | Disciplined type system; unused `--gold` token; near-empty full-viewport hero |
| 9 | Error Recovery | 1 | No `.field-error`/`:invalid` styling anywhere — falls back to raw native browser validation |
| 10 | Help and Documentation | 3 | Excellent DNI/Ley 25.326 microcopy; footer legal links are dead `href="#"` |
| **Total** | | **24/40** | **Acceptable** |

## Design Specificity Verdict

**LLM assessment:** Partially specific. What's genuinely CASE-built and hard to fake: the Socio/General pricing math with live "Ahorrás $X," Wednesday-aware coupon logic, es-AR peso formatting, and the DNI/Ley 25.326 copy. What still reads generic: payment options are Efectivo/Crédito/Débito with no Mercado Pago (the dominant local rail), `--gold` is declared and never used, dark mode is documented in PRODUCT.md but not implemented, and Club CASE — called the product's core business engine — never actually happens on this page, only links out. Strip the pricing logic and copy and the checkout shell could belong to any small e-commerce site.

**Deterministic scan:** `impeccable detect --json` returned zero findings (exit 0). Tool sanity-checked against `plan-case.html` in the same run, which returned dozens of real findings — confirms the scanner is live, not silently broken. The Fraunces `overused-font` ignore rule was verified working: suppressed by default, reproduced only with `--no-config`, consistent with the documented PRODUCT.md rationale. Read together with Assessment A: the detector's visual/slop layer is clean, but the real issues here live one layer down — orphaned JS references, missing states, absent payment rails — which a pattern scanner can't see. Detector-clean does not mean issue-free on this page.

**Visual overlays:** Not available this run. Browser injection/live-server overlay mode has no CLI entry point in `impeccable detect --help`; only an interactive `/live` slash command exists, which isn't scriptable headlessly. Assessment B substituted a mobile screenshot (clean, no overflow) plus DOM-based layout verification at 1440px (zero elements outside viewport bounds) in place of a broken desktop screenshot capture — a tooling limitation in this session's browser pane, not a defect in the page.

## Overall Impression

Real, measurable progress since the 14/40 run — the placeholder-icon problem is gone, replaced with genuine food photography, and the Socio-pricing mechanic is executed with actual craft. But the page still behaves like checkout was assembled from a generic template with local copy pasted on top: the cart has no visible count, the payment methods skip the rail most Argentine shoppers actually use, and a completed order leaves a still-clickable cart sitting above it. The biggest opportunity is closing the gap between "Club CASE is the business" (PRODUCT.md) and "Club CASE is two outbound links" (this page).

## What's Working

1. Real food photography replaced the SVG-on-gradient placeholders — directly resolves the #1 weakness flagged in PRODUCT.md, e.g. the Bowl de Açaí and Milanesa de soja cards with proper alt text.
2. Socio-pricing math is bespoke, not decorative — every card computes General vs. Socio with live "Ahorrás $X," and the cart recomputes Wednesday-aware savings (`isWednesday()` gating the extra 5%).
3. Ley 25.326 legal-note copy is well-judged: specific statute, plain language, explicit opt-out, right at the moment a first-time shopper hands over a DNI.

## Priority Issues

**[P1] No persistent cart indicator — dead code, not a deliberate omission**
Why it matters: `renderCart()` writes to `document.getElementById('cartCount')` (lines 732–733) but no `#cartCount` element exists in the markup. Fails Visibility of System Status right at add-to-cart, the moment that should build confidence.
Fix: add a cart icon + `<span id="cartCount">` to the sticky `.cat-bar`, wired to the total already computed one line above.
Suggested command: /impeccable harden

**[P1] Hero 3D wordmark has no fallback**
Why it matters: the hero logo depends entirely on three.js + MTLLoader/OBJLoader pulling a 1.9MB `.obj` over CDN, no `<noscript>`, no static image, no text fallback. Confirmed failing in Assessment A's test environment. On slow connections, ad blockers, or older phones — plausible for a neighborhood dietética's customer base — the hero shows a blank gradient where "CASE" should be, silently.
Fix: set a static poster image (an outline logo asset already exists in the project) as default canvas content, swap to the 3D render only after a confirmed load.
Suggested command: /impeccable harden

**[P1] Checkout defaults to card, ignores pickup, skips Mercado Pago**
Why it matters: shipping ($1.000) and address are always required even though the cash-note implies in-store pickup exists, with no control to select it. Payment tabs are Efectivo/Crédito/Débito — no Mercado Pago, the payment rail actually dominant for small AR businesses. Reads like an international template with local payment culture edited out.
Fix: add a pickup/delivery toggle that gates shipping and address requirement; add Mercado Pago as a payment tab.
Suggested command: /impeccable shape

**[P1] No styled error states; stale cart persists after order success**
Why it matters: zero `.field-error`/`:invalid` CSS exists despite a fully bespoke input design — validation falls back to native browser bubbles that will look and behave inconsistently. After "Confirmar pedido," the cart section and its "Ir al pago" CTA stay populated and clickable, risking a confusing accidental second order and weakening the peak-end resolution.
Fix: add inline error messaging on required fields; clear/hide the cart once `successPanel` shows, replace with a "Seguir comprando" link.
Suggested command: /impeccable harden

**[P2] Catalog collapses to a flat 21-card wall under the default filter**
Why it matters: fails the cognitive-load chunking check (>4 items, no grouping) and directly undercuts the brand's own "Elegí por objetivo" promise — categories exist only as tab labels, forcing an all-or-one choice between seeing everything unsorted or exactly one category.
Fix: render category sub-headers when the filter is "Todos," or add combinable dietary-need facets (vegano + sin-tacc together), matching how the brand voice already tells people to shop.
Suggested command: /impeccable layout

## Persona Red Flags

**Casey (mobile):** at 375px only ~3 of 8 category tabs are visible with no scroll-hint; the 21-item "Todos" wall means heavy thumb-scrolling; the 1.9MB 3D hero asset is a real mobile-data cost for this audience.

**Riley (stress-tester):** the WebGL/CDN hero chain has no degradation path and was observed failing in this session; DNI/card/vencimiento/CVV accept free text with placeholder hints only, no masking or format validation before submit.

**Jordan (first-timer):** with no cart badge, a first-time visitor gets zero feedback that "Agregar" registered until they scroll far past the catalog. Club CASE — the stated core loyalty engine — is never shown or joinable on this page, only linked away.

## Minor Observations

- `--gold: #C8933A`, one of PRODUCT.md's three named brand colors, is declared and never used anywhere in the file.
- No dark-mode implementation (`prefers-color-scheme`/`[data-theme]`) despite PRODUCT.md documenting a mirrored dark palette; only a code comment references it.
- Cart qty steppers (`−`/`+`) carry no `aria-label` naming which product they affect.
- Category and payment tabs are plain buttons, not `role="tablist"`/`aria-selected` — selection state is color-only, not announced to assistive tech.
- Footer "Términos de Servicio," "Política de Privacidad," and both social icons link to `href="#"`.
- Favicon missing (404 on /favicon.ico) — cosmetic.

## Questions to Consider

1. If Club CASE is "el motor de negocio... central al producto, no un extra," why is it never actually joinable on this page — only linked away? What would this page look like if signup were as reachable as "Agregar"?
2. The hero spends a full viewport and 1.9MB animating a wordmark visitors already know they're looking at. What would that engineering budget do redirected at the food itself, given the brand's whole differentiator is fixing "cero fotografía real"?
3. Every card already shows the Socio price to non-members. If the discount is visible before signing up, what's actually left to persuade someone to join Club CASE?
