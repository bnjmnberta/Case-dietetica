---
target: case-home-catalogo-checkout.html
total_score: 14
max_score: 40
na_heuristics: 
p0_count: 3
p1_count: 2
target_identity: "file:C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
target_fingerprint: "sha256:b8c687a2688d250b9c1b1ad08ebd22cb5b02cc86f08579a48007232d3654823d"
target_path: "C:\\Users\\Benja\\Desktop\\Claude Cowork\\TARS\\CASE ( Ejemplo )\\case-home-catalogo-checkout.html"
timestamp: 2026-09-16T00-34-27Z
slug: case-home-catalogo-checkout-html
---
Method: dual-agent (A: Design review re-run · B: Detector + browser evidence re-run)

## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 1 | "Agregar" produces no visible feedback; `#cartCount` targets an element that no longer exists; 0 `aria-live` |
| 2 | Match System / Real World | 3 | Voseo consistent, Ley 25.326 correctly framed, es-AR money. Docked: no cuotas, no Mercado Pago |
| 3 | User Control and Freedom | 1 | After submit there is no undo, no "seguir comprando"; the cart stays live and recalculates past a frozen order |
| 4 | Consistency and Standards | 2 | `--lime-deep` means both "saving" and "shipping charge"; `--gold` and `.wrap` declared, used zero times |
| 5 | Error Prevention | 1 | `#ckVenc`/`#ckCvv` never `required`; no email/phone; no `:invalid` styling |
| 6 | Recognition Rather Than Recall | 1 | Checkout shows no order summary (totals ~1500px above); `CASE-MIE` advertised with no field to enter it |
| 7 | Flexibility and Efficiency | 1 | No search, no sort, no dietary-attribute filter — the axis PRODUCT.md says the audience shops by |
| 8 | Aesthetic and Minimalist Design | 2 | Palette/type/paper ground are handsome; 847px content-free hero and 21 identical lime button slabs are not |
| 9 | Error Recovery | 0 | There is no error state anywhere in the file |
| 10 | Help and Documentation | 2 | The Ley 25.326 note is excellent; no product detail, macros, shipping or returns info |
| **Total** | | **14/40** | **Poor (35%)** |

## Design Specificity Verdict

**Specific below the fold, generic above it — which is the wrong way round.**

What is now genuinely authored for CASE, verified against PRODUCT.md:
- **Type stack exact and correctly assigned**: Fraunces on headings/product titles, Public Sans on body, JetBrains Mono on every price and code, with `tabular-nums` so the dual-price column aligns.
- **Palette verbatim**: `--lime:#8FCB3C`, `--brand:#0F3D33`, `--bg:#F7F8F2`.
- **Voice**: consistently voseo'd and non-grandiose — "Mirá que esté todo bien antes de pasar al pago", "Elegí por objetivo", "Sin letra chica". The strongest specificity signal on the page.
- **Ley 25.326 note**: shield icon, plain reason, scope, opt-out, statute named — exactly what PRODUCT.md demanded ("visible y creíble… no decoración").
- **Santa Fe is materially present** in footer and placeholders.

What would still transplant unchanged to any wellness brand:
- **The hero.** 847px desktop / 682px mobile of pure CSS gradient. Zero photography, zero product, zero logo, zero navigation, zero CTAs. PRODUCT.md names the incumbent's primary defect as "todas las fotos de producto son placeholders… **Cero fotografía real**" — the catalog obeys, the hero reverts to exactly the abstract-gradient move the brief was written against. `assets/img/hero-market.jpg` and `case-v2/hero-bg.png` sit unused in the repo.
- **The signature motion is an acknowledged port** — the source comment names landonorris.com and the markup still carries the donor's `data-anim-high="right, lime"` attribute verbatim.
- **`--gold:#C8933A` is declared and used zero times** (verified). PRODUCT.md names a three-colour identity; the execution is two-colour.
- **Dark mode is documented ("Dark mode espejado") and entirely absent.**
- **The photography does not read as one session** — the brief's explicit requirement. Six lighting setups across six images; `soy-milanesa.jpg` reads as hard-flash fried fast food, actively fighting the "comida real vs. ultraprocesados" positioning.
- **The lime italic emphasis lands on "etiqueta"** — the thing the sentence argues *against*.

## Deterministic scan — and an important correction

`impeccable detect --json` returns `[]`, exit 0. **That is a false negative, not a clean bill, and it invalidates the "detector limpio" claim made in the two previous turns of this session.**

Assessment B proved it with a controlled A/B: the same CSS with statically-authored elements produces **7 findings** (including `2.5:1 — text #6ea82c on #eff2e7` and three `11px body text`); the identical CSS with elements injected via a JS template literal produces `[]`. **The detector only evaluates statically-authored markup.** CASE builds its entire catalog and cart through `innerHTML` template literals, so every product card, tag, price tier, savings note and cart row is structurally invisible to it — essentially the whole commercial surface.

Config note: the `overused-font=fraunces` ignore is a legitimate suppression of a true-but-intended positive. The `overused-font=geist` ignore is now **inert** — Geist is no longer referenced anywhere — and should be pruned.

## Overall Impression

The brand-truth work landed. Type, palette, voice and legal framing are now authored for this product and would not transplant — that was the P0 of the first critique and it is genuinely closed.

What the first critique never tested, this one did: the interaction layer. Clicking, submitting, typing a DNI. It is hollow. "Agregar" — the most-repeated action on the page, the one the whole catalog exists to produce — changes nothing visible. Club CASE is advertised 22 times and obtainable zero times. And the palette pass I performed introduced six new WCAG failures, all of them on the loyalty mechanic and the dietary tags this audience shops by.

The single biggest opportunity: the page currently looks like CASE and behaves like a mockup. Closing that gap is worth more than any further visual refinement.

## What's Working

- **The Wednesday mechanic is wired to the system clock, not mocked.** `isWednesday()` drives a copy swap, a gradient swap, *and* a real 5% compounding onto the Socio total. A documented business mechanic that actually changes state on the correct day is more than most portfolio pieces attempt.
- **The Ley 25.326 note is the best-authored element on the page** — under the field it justifies, not in a policy link.
- **Alt text and reduced-motion discipline are both correct.** All 21 product images carry descriptive Spanish alts. `prefers-reduced-motion` is honoured in three places, including an early `return` that skips the SplitText timeline so the headline can never be stranded mid-clip.
- **Arithmetic is fully consistent.** B recomputed the cart independently: adding a $6.800 item moved subtotal $10.900→$17.700, Socio $11.250→$17.640, saving $650→$1.060 — every figure cross-checks.

## Priority Issues

**[P0] "Agregar" is a black hole** — Verified: `#cartCount` does not exist in the DOM (removed when the header was stripped), `[aria-live]` count is 0, and the button label is unchanged after click. The cart does update, 2514px below the fold.
*Why it matters*: the most-repeated action on the page gives no acknowledgement. Users click twice, three times, then leave. Keyboard and screen-reader users get nothing.
*Fix*: build the sticky header (wordmark + cart count + Club CASE link) that `#cartCount` already expects; flash the button to "Agregado ✓" for ~1.2s; wrap the count in `aria-live="polite"`.
*Suggested command*: `/impeccable harden`

**[P0] Club CASE is decorative at the point of sale** — Precio Socio is advertised 21× in the grid plus once in the summary. Typing a valid DNI into `#ckDni` leaves the total unchanged while the Socio price sits stranded beside it. There is no coupon field for `CASE-MIE`. The only way to join is an off-page exit *from inside the cart summary*.
*Why it matters*: PRODUCT.md calls Club CASE "central al producto, no un extra". As shipped it is a price-anchoring device — the page shows a better price 22 times and never lets you have it. The DNI field's own label promises "solo para el precio Socio" and then does nothing.
*Fix*: on a valid DNI, apply Socio pricing to the live total and relabel the summary; add a coupon input that accepts `CASE-MIE`; put the documented 4-field join inline in the cart summary.
*Suggested command*: `/impeccable shape`

**[P0] Six Club CASE and nutrition signals fail WCAG AA** — all introduced by this session's palette pass, all the same root cause: `--lime-deep:#6EA82C` as small text on light ground. Independently verified at **2.88:1** (need 4.5:1): `.save-note` "Ahorrás $410", `.price-socio .lbl` "SOCIO", `.club-nudge .head`, `.club-nudge a` "Sumate gratis", `.sum-row .val.teal`. `.tag` measures **2.54:1** on its own `#EFF2E7` ground. The `:focus-visible` ring also fails 1.4.11 at 2.54–2.88:1 on light surfaces, and checkout inputs are the one `outline:none` on the page, keyed to `:focus` rather than `:focus-visible`.
*Why it matters*: every failing item is either the loyalty mechanic or the dietary attribute the audience shops by — and the *struck-through, deprecated* general price renders at 7.66:1, more legible than the member price. An exact inversion of business priority, plus a keyboard-accessibility failure.
*Fix*: add a `--lime-ink:#3F6B14` (≈5.5:1 on white) for green **text** on light surfaces; reserve `--lime` for fills. Darken the focus ring to `--brand` and restore an outline on inputs via `:focus-visible`. Stop colouring "Envío express" green — it is a charge, not a saving.
*Suggested command*: `/impeccable audit`

**[P1] No email is collected, and the terminal state is not terminal** — `input[type=email]` count is 0, yet the success panel reads "Te mandamos el detalle por mail." Post-submit the cart stays fully live: B added a product and the total moved $5.500→$9.000 while the confirmed order stayed frozen.
*Why it matters*: the page promises a communication it structurally cannot send, PRODUCT.md's "drop semanal por email" has no capture point anywhere, and the confirmed order diverges from what the page displays.
*Fix*: add a required email field (it doubles as the drop-semanal hook); on submit clear and freeze the cart, echo items + total + address in the success panel, add "Seguir comprando" and a Club CASE join CTA.
*Suggested command*: `/impeccable harden`

**[P1] No max-width anywhere, and the hero has no job** — `.wrap { max-width:1280px }` is defined and applied to **zero elements** (verified). At 2700px viewport, `.pcard` renders 818px wide against a fixed `height:240px` thumb — a 3.4:1 letterbox strip — and `.cart-item` stretches to 2086px. Separately the hero is 847px of gradient with 0 CTAs and 0 photography while `hero-market.jpg` sits unused.
*Fix*: apply `.wrap` to `section.block`, the `.cat-bar` inner and `.footer-content`; change `.pcard .thumb` to `aspect-ratio:4/3`; put real photography and a primary CTA in the hero.
*Suggested command*: `/impeccable adapt`

## Persona Red Flags

**Malena, 34 — coeliac regular** (PRODUCT.md: "buscan opciones dietéticas específicas… sin TACC"): the only filter axis is category, so she opens all 7 tabs and reads 21 cards — where "Sin TACC" is the lowest-contrast text on the page at 2.54:1/11px. No ingredient list, no per-100g panel, no product detail page.

**Norma, 61 — price-sensitive regular** (PRODUCT.md: "valoran ahorro recurrente"): counts "Ahorrás $410" across 21 cards, types her DNI into the field that says "solo para el precio Socio" — and the total does not move. The single strongest reason to buy from CASE instead of the supermarket is advertised 22 times and delivered zero times.

**Joaquín, 27 — first-timer on a phone**: 1.75 screens of gradient before any food. The cart already holds $11.900 he didn't add. He taps "Agregar", sees nothing, taps twice more. Reaching for "+" he hits the 22×22px trash icon beside it — no confirm, item gone.

## Minor Observations

- **23 images, 0 with `width`/`height`, `case-v2/*.png` run 1.2–1.7MB each** — including two cart thumbnails (1.5MB, 1.4MB) displayed at 64×64px. Everything loads on first paint, with layout shift.
- **Orphaned comma at 390px**: the headline breaks as "lo que necesitás" / ", no por lo que" — SplitText wraps at the `</strong>` boundary. Confirmed settled layout, not a capture artifact. It also leaves one empty `<strong></strong>`.
- No `role="tab"` / `aria-selected` / `aria-pressed` on either tab group — screen readers get 8 and 3 unrelated buttons with no selection state.
- No `<header>`, `<nav>`, or skip link anywhere.
- **Crédito and Débito render an identical form**; no cuotas, no Mercado Pago.
- **Pickup contradiction**: `#cashNote` offers "o en el local retirando con tu código de orden", but no pickup option exists and $1.000 envío is charged unconditionally.
- `.pcard` has no `:hover` rule; cards are inert and lead nowhere.
- Filtering collapses the document 7233px→3658px with no scroll anchoring.
- "Ir al pago" stays enabled and primary-styled on an empty cart.
- `panel.scrollIntoView({behavior:'smooth'})` is the one motion path not gated by `prefers-reduced-motion`, and it fights Lenis for scroll control.
- **Dietas Especiales — the keto/apto-diabéticos category that is the store's actual differentiator — is the thinnest at 2 items**, leaving a half-empty row.
- Instagram handle `@case.dietetica` is documented and never shown; the footer glyph points at `href="#"`.

## Questions to Consider

1. The brief's diagnosis was "cero fotografía real". The catalog answers it; the hero — the entire first viewport — answers it with a gradient while `hero-market.jpg` sits unused. If a visitor only ever saw the first screen, what would tell them this is a food shop in Santa Fe rather than a wellness SaaS?
2. `data-anim-high="right, lime"` is still in the markup and the comment names the donor site. What is the line-wipe *saying* about CASE? Would you rather have a reveal that only makes sense for a shop whose argument is that labels lie — or one more product photograph above the fold?
3. Precio Socio appears 22 times and is obtainable zero times without leaving the page. What would have to be true on *this page* for a visitor to become a socio **before** they pay, and what would that do to the cart summary?
