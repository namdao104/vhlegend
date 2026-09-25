# VH Legend — Brand Website Design System

Applies the Legend design system (SKILL.md / DESIGN.md) to a public brand and
capabilities website built from the *AI Infrastructure Overview, 2026* deck.

---

## 1. Context and goals

**Design intent in one sentence:** present Legend as an AI infrastructure
manufacturing and deployment partner — not a rack vendor — by letting the
factory, the specification and the configuration grid carry the argument.

- Product surface: public marketing website (single page, anchor navigation).
- Audience: data-centre operators, EPC and M&E contractors, system integrators,
  distributors and OEM partners. Technical buyers, not consumers.
- Primary job: produce a qualified next meeting — a site assessment, a sample
  rack, or an OEM conversation.
- Deliverable: `vhlegend-website.html`, one self-contained file. All imagery is
  inlined as base64, so it opens from disk, an email attachment or any static
  host with no asset paths to wire up.

---

## 2. Design tokens and foundations

### Inherited unchanged
`font.family.*`, `font.size.xs → 4xl`, `font.weight.base`, `space.1 → space.8`,
`radius.*`, `shadow.1`, `motion.duration.*`, `color.text.primary`,
`color.text.secondary`, `color.text.inverse`, `color.border.muted`.

### Brand extension — `color.brand.*`
Sampled from the source deck so web and deck stay one identity.

| Token | Value | Use |
|---|---|---|
| `color.brand.900` | `#0c1730` | Page-level dark surface: hero, factory, engineering, retrofit, contact |
| `color.brand.800` | `#162549` | Raised dark panels, pull quotes, "Legend brand" card |
| `color.brand.700` | `#1f3864` | Configuration chips, statistic figures, rules |
| `color.brand.600` | `#2c4079` | Chip hover |
| `color.brand.200` | `#8fa3c9` | Labels on dark |
| `color.brand.100` | `#cadcfc` | Supporting text on dark |
| `color.accent` | `#b08d3e` | Gold. Dimension lines, section rules, focus ring |
| `color.accent.bright` | `#d3aa4f` | Gold, lifted. Primary button only — hover `#dab254`, active `#c49a45` |
| `color.accent.ink` | `#7a5e1f` | Gold text on light surfaces (AA-safe) |
| `color.neutral.500` | `#6b7280` | Sources, captions, indicative figures |

Light surfaces are re-pointed for a marketing page: `surface.base = #ffffff`,
`surface.raised = #f4f6fb`, `surface.strong = #e3e8f2`.

### Display type extension
The inherited scale stops at `4xl = 32px`, which is a dashboard scale. Marketing
display sizes are added rather than invented inline:
`font.size.5xl = 40px`, `6xl = 56px`, `7xl = 76px`. All headline sizes are
`clamp()`-bound so the same token holds on a 390 px viewport.

### Spacing extension
`space.9 = 24px`, `space.10 = 32px`, `space.11 = 48px`, `space.12 = 72px`,
`space.13 = 112px` — section rhythm needs steps above 18 px. No component uses a
raw pixel value outside the scale.

### Layout
Content column `min(1240px, 100% − 48px)`. Body measure capped at `64ch`,
headlines at `20ch`. Sections alternate white → tint → dark so the page reads as
distinct chapters rather than a single scroll.

---

## 3. Component-level rules

### 3.1 Button (`.btn`)
- **Anatomy:** label, optional leading icon, `radius.xs`, `space.5 / space.9` padding.
- **Variants:** `primary` (`accent.bright` with near-black ink), `ghost` (hairline on dark).
  The primary action sits one step brighter than the structural gold so it reads
  as the thing to press, not as another rule on the page. Ink contrast is 8.4:1,
  and every state stays above 7:1 — brightening further is allowed, darkening
  below `#c49a45` is not.
- **States:** default · hover (lighter gold / border to white) · focus-visible
  (2 px gold ring, 3 px offset) · active (darker gold) · disabled
  (`aria-disabled="true"`, 45% opacity, pointer events off) · loading and error
  are not used on this surface, as no button performs an async action.
- **Behaviour:** labels never wrap (`white-space: nowrap`); at ≤640 px padding
  and font size step down instead of the label truncating.
- **Long content:** a label longer than ~24 characters must be rewritten, not wrapped.

### 3.2 Configuration matrix (`.matrix`, `.chip`, `.readout`)
The signature component: eight configurations plotted as cooling method × rack density.
- **Anatomy:** row header (method) · five density cells · axis row · readout panel.
- **Variants:** `chip` (production) and `chip.dev` (gold outline, in development —
  AI-R8 only). The visual difference must not be colour alone; the `dev` variant
  also carries the words "in dev." in its label.
- **States:** default · hover · focus-visible · selected (`aria-pressed="true"`,
  brand-900 fill, 1 px lift) · empty cell (no control, not a disabled control).
- **Interaction:** chips are real `<button>` elements — pointer, `Enter`,
  `Space` and tab order all work with no extra handling. The readout is
  `aria-live="polite"`, so a screen reader hears the new configuration.
- **Responsive:** both axes must survive every breakpoint — they *are* the
  information. Above 820 px the grid renders as plotted, scrolling horizontally
  inside `.matrix` between 821 and 1080 px. At or below 820 px the grid is
  replaced by `.mlist`: cooling method becomes a group heading and rack density
  moves into each chip as a right-aligned label, so all eight configurations are
  visible with no horizontal scroll. The list is generated at load from the grid
  markup — one source of truth, and only one presentation is ever in the
  accessibility tree.
- **Selection is shared.** `select()` matches on `data-code`, not object
  identity, so grid and list stay in sync when the viewport is resized.
- **Readout follow.** At or below 820 px the readout sits beneath the list, so
  selection scrolls it into view (`block: 'center'`, `behavior: 'auto'` under
  reduced motion). The panel is the answer to the tap and must not update
  off-screen.
- **Empty state:** before any selection the readout shows the confirmed
  production SKUs — never a blank panel.

### 3.3 Product card (`.product`)
- **Anatomy:** white product shot · gold dimension line with `2000 mm` callout ·
  name · SKU · footprint · feature list.
- **Rule:** the dimension line is the one decorative device allowed to repeat. It
  encodes a real fact (one 2.0 m frame across all three deployments) and must not
  be used on any block where no dimension is being stated.
- **States:** static content; hover is not used, because the card is not a control.
- **Long content:** feature lists cap at four items; the SKU never truncates.

### 3.4 Statistic (`.stat`, `.bigstat`, `.threeup`)
- **Anatomy:** figure in brand-700 · 3 px top rule · caption at `font.size.lg`.
- **Rule:** every statistic must carry its source within the same section
  (McKinsey Global AI Survey 2025; Singapore ICT energy share). A figure without
  a visible source must not ship.

### 3.5 Pull quote (`.pullquote`)
- **Anatomy:** plain `brand.800` panel, one short claim. No rule, no marker —
  the dark panel against the white section is the emphasis.
- **Width rule:** the panel must span the full content column so its edges align
  with the grid above it. A panel that stops short of the grid edge reads as a
  mistake, not as emphasis.
- **Measure rule:** the text inside is capped at `72ch`. Trailing space on the
  right is intentional — the panel aligns, the line length stays readable.
- **States:** static content; not a control, so no hover.

### 3.6 Section header (`.kicker` + `h2` + `.sub`)
- Kicker is sentence case with a 28 × 2 px gold rule, never all caps.
- One `h2` per section, `20ch` maximum, stated as a claim rather than a label.

### 3.7 View switcher (`.viewtoggle`)
The site ships with a 1280 px viewport, so phones and small tablets render the
desktop layout at true proportions by default. Desktop browsers ignore the
viewport meta entirely and are unaffected. The switcher lets a visitor drop to
the responsive layout.
- **Anatomy:** fixed bottom-right pill, gold marker, label naming the view it
  switches *to* ("Desktop view" → "Mobile view").
- **Mechanism:** replaces the `<meta name="viewport">` node — `width=1280`
  against `width=device-width, initial-scale=1`. The node is replaced rather
  than edited in place, which is the reliable path on iOS Safari. The desktop
  width is set in the served HTML rather than applied by script, so there is no
  flash of the responsive layout before JS runs.
- **States:** default · hover · focus-visible · pressed (`aria-pressed`
  reflects the active view).
- **Device detection:** `screen.width` / `screen.height` report the hardware and
  are unaffected by the viewport meta; `window.innerWidth` already reads 1280 at
  load and must not be used. A device whose smaller dimension is ≤ 820 px is
  treated as small.
- **Deep link:** `…/vhlegend-website.html#mobile` opens straight in the
  responsive layout.
- **Escape hatch rule:** the control must stay visible and tappable in *both*
  views. In forced-desktop the whole page is scaled to roughly a quarter size,
  so `html.forced-desktop .viewtoggle` restates its size in raw pixels
  (44 px label, 26 px padding). Without this the user is trapped in a view they
  cannot exit.
- **Prerequisite:** `text-size-adjust: 100%` on `html`. Mobile browsers boost
  font sizes when the layout viewport exceeds the device width, which inflates
  body copy while leaving small flex items alone — the desktop view looks
  broken without it.
- **Default:** desktop proportions on small devices, by business decision. The
  switch does not persist across page loads, so every visit starts in the
  desktop view.
- **Known cost of this default:** body copy renders at roughly 30% of its
  authored size on a 390 px phone, which is below comfortable reading size and
  is treated as a mobile-usability failure by search engines. The responsive
  layout is fully built and one line away — set the document's viewport meta
  back to `width=device-width, initial-scale=1` and invert the `small` branch in
  the switcher. Revisit if mobile bounce rate or search ranking becomes a
  concern.

### 3.8 Navigation (`header`, `nav`)
- Sticky, `brand.900` at 94% with backdrop blur, hairline bottom border.
- Links show a gold 2 px underline on hover and a visible focus ring on tab.
- Below 1000 px the link list is withdrawn and the primary CTA persists — the
  page is anchor-navigable by scroll, so no hamburger menu is introduced.

---

## 4. Accessibility requirements and acceptance criteria

Target: **WCAG 2.2 AA**. Each criterion below is pass/fail in implementation.

| # | Criterion | Test |
|---|---|---|
| A1 | Body and caption text meets 4.5:1 against its surface | Sample `#524b63` on `#ffffff` (7.4:1), `#c6cfe2` on `#0c1730` (11.2:1), `#7a5e1f` on `#f4f6fb` (5.1:1) |
| A2 | Gold is never used as small text on white-gold or on brand-900 | Grep for `color.accent` on `font.size.md` over light surfaces; must resolve to `accent.ink` |
| A3 | Every interactive element has a visible focus indicator | Tab the whole page; 2 px gold ring at 3 px offset must appear on each stop, including the skip link |
| A4 | The configuration matrix is fully operable by keyboard | Tab to a chip, press `Enter` and `Space`; the readout must update for both |
| A5 | Selection state is exposed to assistive technology | `aria-pressed` flips to `true` on exactly one chip; readout region is `aria-live="polite"` |
| A6 | Meaning never depends on colour alone | The in-development configuration is identifiable from its text label with colour removed |
| A7 | Decorative images are hidden from the accessibility tree | Hero background carries `alt=""` and `aria-hidden="true"`; every content image has descriptive `alt` |
| A8 | Motion is user-triggered and respects preference | `prefers-reduced-motion: reduce` collapses all transitions and disables smooth scroll |
| A9 | Content reflows to 320 px with no horizontal page scroll | Only `.matrix` scrolls, and it scrolls within its own container |
| A10 | Heading order is sequential | `h1` once in the hero; each section opens with `h2`; no level is skipped |
| A11 | A skip link precedes the navigation | First `Tab` from page load reveals "Skip to content" |
| A12 | Contact details are reachable as actions | Every phone number is a `tel:` link, every address a `mailto:` link |

---

## 5. Content and tone standards

Concise, confident, implementation-focused. Sentence case throughout.

- **Claim, then qualify.** "8,000 m² built to data-centre tolerances" followed by
  the establishment year and the processes under one roof.
- **Name the constraint, not the adjective.** Write "above roughly 15 kW per
  rack, air alone stops being viable" — not "industry-leading thermal performance".
- **Indicative figures are labelled as such.** Density bands, floor loading and
  cooling-energy savings all carry "roughly", "estimated" or "indicative", and
  the matrix note states that AI-R8 is in development.
- **CTAs name what happens.** "Book a site assessment" produces a site
  assessment; "Start an OEM conversation" opens an OEM enquiry. The label does
  not change between the header, the retrofit band and the contact section.
- **Avoid:** "solutions", "cutting-edge", "world-class", exclamation marks, and
  any claim the factory cannot evidence.

Examples:
> **Do:** "Any layer can be supplied on its own and integrated with the racks, power and cooling you already run."
> **Don't:** "Flexible, modular solutions tailored to your unique needs."

---

## 6. Anti-patterns and prohibited implementations

- Do not introduce a second accent colour. Gold carries precision and every
  primary action; a second accent destroys that association.
- Do not add decorative gradients, glows or floating particles to the dark
  sections. The dark surface is the factory, not a product launch.
- Do not animate sections on scroll. Motion is reserved for two cases: what
  answers a click (the configuration readout), and motion that carries
  information a static image cannot. The world map is the only instance of the
  second: the dashes flow toward Singapore, so the line states direction as well
  as connection. It runs only while the map is in the viewport, and is disabled
  entirely under `prefers-reduced-motion: reduce`. A new animation must clear the
  same bar — decorative reveals do not.
- Do not collapse the configuration matrix into a flat list of cards. The
  stacked mobile presentation is permitted only because it keeps both axes —
  cooling method as the group heading, density inside the chip. A list that
  drops either axis loses the argument.
- Do not leave the mobile matrix as a horizontally scrolling grid. At 390 px the
  row labels consume a third of the viewport and the first three rows open
  empty, so the grid reads as broken.
- Do not state a statistic without its source, or a density band without its
  "indicative" qualifier.
- Do not set gold (`#b08d3e`) as body text on light surfaces; use `accent.ink`.
- Do not hard-code hex values or pixel spacing in new components. Extend the
  token block instead.
- Do not add a hamburger menu that duplicates anchors already reachable by scroll.

---

## 7. Migration notes

1. **Surfaces re-pointed.** The inherited `surface.base = #000000` and
   `surface.raised = #ecebf0` describe an authenticated dashboard. A marketing
   page inverts this: white base, brand-900 for emphasis chapters. The dark
   surface is now a section-level choice rather than the page default.
2. **Display scale added.** `5xl / 6xl / 7xl` exist only on this surface. Do not
   back-port them into dashboard components.
3. **`radius.md = 50px` and `radius.lg` are unused here.** Pill geometry reads as
   consumer; industrial hardware reads as `radius.xs`. Both tokens remain valid
   for other surfaces.
4. **Typeface delivery.** Mona Sans loads from Google Fonts with the inherited
   fallback stack. If the site is deployed behind a network that blocks Google
   Fonts, self-host the family and replace the `<link>` — the token values do not
   change.

---

## 8. QA checklist

- [ ] Every colour, space, radius and duration in new CSS resolves to a token.
- [ ] Each interactive component defines default, hover, focus-visible, active
      and disabled; states not used are justified in this document.
- [ ] Tab traversal reaches every link, button and chip, in visual order, with a
      visible ring at each stop.
- [ ] `Enter` and `Space` both actuate every chip; the readout announces.
- [ ] Contrast sampled on: body on white, caption on tint, `accent.ink` on tint,
      brand-100 on brand-900, gold button ink on gold.
- [ ] Layout checked at 320, 390, 768, 1024, 1440 and 1920 px;
      `document.documentElement.scrollWidth` equals `window.innerWidth` at each.
- [ ] Exactly one matrix presentation is rendered per breakpoint — count visible
      `.chip` elements; the answer must be 8, never 16.
- [ ] Tapping a configuration on mobile brings the readout into view.
- [ ] Label-plus-qualifier pairs (`.group li`, `.flow div`) stack rather than
      competing for one line below 640 px.
- [ ] Anchor navigation lands below the sticky header (`scroll-margin-top`).
- [ ] Full-width panels (pull quotes, bands) share left and right edges with the
      grid in the same section — measure, don't eyeball.
- [ ] On a phone the document opens at `width=1280`; the switcher returns to
      `width=device-width` and back, and stays tappable in both views.
- [ ] On a desktop browser the switcher is hidden and the layout is untouched.
- [ ] In forced-desktop the page matches the desktop render: matrix as a grid,
      four factory photos in one row, three products in one row.
- [ ] `text-size-adjust: 100%` is present; body copy does not inflate in
      forced-desktop.
- [ ] `prefers-reduced-motion: reduce` disables transitions, smooth scroll and
      the map animation (computed `animation-name` must read `none`).
- [ ] The map animation stops when the map leaves the viewport.
- [ ] Every content image has meaningful `alt`; the hero background is hidden.
- [ ] Every statistic shows its source; every indicative figure is labelled.
- [ ] Every phone and email is an actionable link and matches the deck exactly.
- [ ] No placeholder text, no lorem, no "TBC" remains.
- [ ] Page opens correctly from `file://` with no network — images inline, text
      readable in the fallback typeface.
