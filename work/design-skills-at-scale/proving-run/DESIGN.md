---
name: Pegasuz Admin
colors:
  canvas: '#F4F6F8'
  surface: '#FFFFFF'
  surface-2: '#FAFBFC'
  surface-sunken: '#EBEEF2'
  surface-elevated: '{colors.surface}'
  line-subtle: '#E2E7ED'
  line-strong: '#8E9BAE'
  ink-heading: '#131A28'
  ink: '#20293A'
  ink-secondary: '#4A5568'
  ink-muted: '#64748B'
  ink-disabled: '#9AA6B8'
  chrome: '#181D28'
  chrome-hover: '#222937'
  chrome-border: '#2D3646'
  chrome-text: '#E8ECF3'
  chrome-dim: '#98A4B8'
  tenant-accent: '#F85810'
  tenant-accent-hover: '#DE470B'
  tenant-accent-soft: '#FEEEE5'
  tenant-accent-border: '#FBC9AC'
  tenant-accent-contrast: '#FFFFFF'
  tenant-accent-strong: '#C2410C'
  focus-ring: '{colors.tenant-accent}'
  state-success: '#16A34A'
  state-success-soft: '#EAF7EF'
  state-success-text: '#166534'
  state-success-border: '#BFE5CD'
  state-error: '#DC2626'
  state-error-soft: '#FDECEC'
  state-error-text: '#991B1B'
  state-error-border: '#F4C7C7'
  state-warning: '#D97706'
  state-warning-soft: '#FCF3E3'
  state-warning-text: '#92400E'
  state-warning-border: '#F1DCB4'
  state-info: '#0284C7'
  state-info-soft: '#E9F5FC'
  state-info-text: '#075985'
  state-info-border: '#BCE0F3'
  state-pending: '#EA8A00'
  state-pending-soft: '#FDF2E0'
  state-pending-text: '#8F5400'
  state-pending-border: '#F5DCAE'
  domain-cash: '#059669'
  domain-cash-soft: '#E6F6F0'
  domain-cash-text: '#065F46'
  domain-cash-border: '#B4E5D3'
  domain-stock: '#0D9488'
  domain-stock-soft: '#E6F5F4'
  domain-stock-text: '#115E59'
  domain-stock-border: '#B0E0DC'
  domain-fiscal: '#7C3AED'
  domain-fiscal-soft: '#F3EFFD'
  domain-fiscal-text: '#5B21B6'
  domain-fiscal-border: '#DDD2F8'
  domain-ml: '#FFE600'
  domain-ml-soft: '#FFFBE0'
  domain-ml-text: '#6B5900'
  domain-ml-border: '#EBDB6B'
  domain-mp: '#009EE3'
  domain-mp-soft: '#E5F5FC'
  domain-mp-text: '#00618C'
  domain-mp-border: '#AEDFF4'
  domain-orders: '#2563EB'
  domain-orders-soft: '#EBF1FD'
  domain-orders-text: '#1E40AF'
  domain-orders-border: '#C5D6F8'
  domain-customers: '#0E7490'
  domain-customers-soft: '#E7F4F8'
  domain-customers-text: '#155E75'
  domain-customers-border: '#B6DEE9'
  domain-purchases: '#A16207'
  domain-purchases-soft: '#F8F1E2'
  domain-purchases-text: '#713F12'
  domain-purchases-border: '#E5D3AC'
  domain-branches: '#A21CAF'
  domain-branches-soft: '#F9EDFB'
  domain-branches-text: '#701A75'
  domain-branches-border: '#EACBF0'
  domain-employees: '#BE185D'
  domain-employees-soft: '#FCEBF2'
  domain-employees-text: '#831843'
  domain-employees-border: '#F2C4D8'
typography:
  ui:
    fontFamily: "'Inter Variable', Inter, system-ui, sans-serif"
    fontSize: '0.9375rem'
    lineHeight: '1.5rem'
    letterSpacing: '0'
    fontWeight: '400'
  table:
    fontSize: '0.875rem'
    lineHeight: '1.25rem'
    letterSpacing: '0'
    fontWeight: '400'
  micro:
    fontSize: '0.6875rem'
    lineHeight: '0.875rem'
    letterSpacing: '0.015em'
    fontWeight: '500'
  caption:
    fontSize: '0.75rem'
    lineHeight: '1rem'
    letterSpacing: '0.01em'
    fontWeight: '500'
  small:
    fontSize: '0.8125rem'
    lineHeight: '1.125rem'
    letterSpacing: '0.005em'
    fontWeight: '400'
  body:
    fontSize: '0.875rem'
    lineHeight: '1.375rem'
    letterSpacing: '0'
    fontWeight: '400'
  body-lg:
    fontSize: '1rem'
    lineHeight: '1.625rem'
    letterSpacing: '0'
    fontWeight: '400'
  lead:
    fontSize: '1.125rem'
    lineHeight: '1.75rem'
    letterSpacing: '-0.005em'
    fontWeight: '400'
  subtitle:
    fontSize: '1.125rem'
    lineHeight: '1.625rem'
    letterSpacing: '-0.01em'
    fontWeight: '500'
  title:
    fontSize: '1.375rem'
    lineHeight: '1.875rem'
    letterSpacing: '-0.015em'
    fontWeight: '500'
  heading:
    fontSize: '1.75rem'
    lineHeight: '2.25rem'
    letterSpacing: '-0.02em'
    fontWeight: '500'
  hero:
    fontSize: '2.25rem'
    lineHeight: '2.75rem'
    letterSpacing: '-0.025em'
    fontWeight: '400'
rounded:
  ctl: '10px'
  pill: '9999px'
modes:
  emilopez:
    selector: ':root[data-tenant="emilopez"]'
    colors:
      tenant-accent: '#9E1B32'
      tenant-accent-hover: '#7F1527'
      tenant-accent-soft: '#FBEAEC'
      tenant-accent-border: '#EFB9C0'
      tenant-accent-contrast: '#FFFFFF'
      tenant-accent-strong: '#8A1729'
  ecommerce-test:
    selector: ':root[data-tenant="ecommerce-test"]'
    colors:
      tenant-accent: '#F06030'
      tenant-accent-hover: '#D94E22'
      tenant-accent-soft: '#FDECE4'
      tenant-accent-border: '#F8C4AC'
      tenant-accent-contrast: '#FFFFFF'
      tenant-accent-strong: '#C2410C'
---

# Pegasuz Admin — Design System

## Overview

One feature-gated admin build serves every tenant. 157 view files across 16 declared
navigation modules, 175 components, no prior DESIGN.md.

**This whole file is provisional.** No owner has designated a reference surface, so
nothing in the app can confirm anything: every token below is an election made against
non-reference code and every `## Decisions` entry carries `[provisional]`. Settling
which surfaces are the reference is the single change that would let most of this file
be promoted — it is recorded as the first open question under `### Global` /
`#### Sistema`.

Roles that nothing settles carry **no token at all**: card radius, modal/drawer radius,
the type scale for the redesign-line surfaces, the "fast" motion duration, the spacing
scale and the control height are open questions, not entries in the frontmatter above.
A first extraction from a drifted app that comes out mostly provisional with holes in
it is the honest measurement, not a failed run.

**Coverage bound.** Every count in this file comes from tree-wide search over
`src/**/*.{vue,js,css}`, which is exhaustive over the pattern searched. Files read in
full: the four stylesheets in `src/assets/css/`, `tailwind.config.js`,
`src/data/navConfig.js`, `src/router/index.js`, `docs/admin-design-system-v1.0.md`,
`src/components/ui/DetailHeader.vue`, and the 15 `*DetailView.vue` files checked for
one structural pattern. That is roughly 25 of 518 source files and 15 of 157 views —
6 of the 16 navigation modules have Decisions entries below. The spacing/gap scale was
not harvested at all; that gap is recorded as an open question rather than filled by
guessing.

## Colors

The semantic set above is elected over the two raw gray scales still live in the tree.
It is the set the app's own token file declares and the utility layer maps one-to-one,
and it dominates usage — but the token file is a declaration, not a confirmation, so
the election is provisional.

Migration targets, not tokens: `neutral-*` (2009 occurrences across 49 files, the
framework default scale) and `warm-*` (2308 across 39 files, a custom scale that was
meant to replace `neutral-*` and never finished). Only one file uses both, so the drift
is whole surfaces committed to one scale or the other, not per-line noise. A third
legacy family (`midnight`/`charcoal`/`silver`/`accent`) has zero live uses and gets no
token. 43 inline hex literals across 21 distinct values remain; most are third-party
brand colors that should never be tokenized, five are untracked grays duplicating the
`ink-*` roles.

Note on compilation: the generated variable names are `--color-<name>`, so today's
`--tenant-accent` becomes `--color-tenant-accent` and `--color-text-muted` becomes
`--color-ink-muted`. Adopting this file is therefore a rename pass over the app's token
layer, not only a value migration.

## Typography

Twelve live roles of the app's declared v2 scale, elected over the framework default
scale (7144 occurrences against 1941). A thirteenth declared role (`2xs`) has zero live
uses and gets **no token** — a declaration is not a role.

Not covered by this scale: 430 arbitrary pixel type sizes across 17 distinct values,
concentrated in the redesign-line components. Those surfaces have a declared scale of
their own with zero consumers. Which scale governs them is an open question below, and
no token was elected for it.

## Elevation & Depth

Four elevation levels (`e1`–`e4`) wired from a single source into the utility layer,
811 occurrences, one consumer chain, no competing scale — the one family in this app
that is not drifted. The DESIGN.md frontmatter has no shadow group (the token groups
are colors, typography, spacing, rounded, components), so the scale stays in the app's
stylesheet and is recorded as a Decisions entry instead of a token.

## Shapes

Two radius tokens elected: `ctl` (10px, controls) and `pill` (9999px, full-round).

Two roles have **no token**, deliberately. The app's current radius config annotates two
different values as "cards" (14px and 8px) and two as "modales, drawers" (20px and
12px); the code splits roughly 984 to 751 on the first and 12 to 68 on the second, and
the losing 8px family is a written, argued decision rather than sloppiness. Frequency
would settle both; nothing that can actually confirm does, so both are open questions
and neither value is in the frontmatter. Two further radius scales are declared in CSS
and have zero consumers outside their own definition files.

## Components

There is no shared button primitive: buttons are built per view, and control height is
spread across six values (`h-7` 128, `h-8` 245, `h-9` 455, `h-10` 662, `h-11` 189,
`h-12` 170) with no majority — the declared 40px standard covers 36% of them. A shared
detail-page header component exists and is used by 4 of the 15 detail views; the other
11 hand-roll their own. Both are open questions below, not settled component rules.

## Do's and Don'ts

- Do consume the generated `design.tokens.css`. Do not add a raw hex, a `text-[Npx]`
  or a `rounded-[Npx]` to a surface this file governs.
- Do treat every entry below as a candidate until a session builds on it and it holds.
  Do not read `[provisional]` as "approximately true" — it means nobody has confirmed
  it and the counts are recorded so the next session can decide.
- Do leave a role with no token empty. Do not fill it from the biggest count.

## Decisions

### Global

#### Sistema

- 2026-08-21 — [provisional] no reference surface is designated for this app: the owner
  has named none, so nothing in the codebase can confirm anything and every entry in
  this file is a candidate. The only surfaces anyone has pointed at are
  `/admin/shop-products` and `/admin/shop-stock`, and second-hand at that ("my friend
  told me the best front and tokens was from products and stock"). Hearsay names a
  candidate, not a reference. This is the open question that would settle the file.
- 2026-08-21 — [provisional] the semantic color set (`canvas`/`surface-*`/`line-*`/
  `ink-*`/`chrome-*`/`tenant-accent-*`/`state-*`/`domain-*`) is the color source —
  `ink-*` alone at 6821 uses against `neutral-*` at 2009 and `warm-*` at 2308, the two
  raw scales it replaces. Declared current by the app's own token file
  (`src/assets/css/tokens.css:1`, "ÚNICA fuente de verdad de color"), which is a
  declaration and confirms nothing.
- 2026-08-21 — [provisional] control radius is 10px (`ctl`) — 1832 uses against 340 for
  the 8px rule the v1.0 document states as hard ("Inputs son siempre `rounded-lg`",
  `docs/admin-design-system-v1.0.md:314`) plus 32 raw `rounded-[10px]`. Elected because
  the live config names exactly one token for this role (`tailwind.config.js:244`,
  "inputs, selects, botones") and annotates the v1.0 radius table as overridden
  (`:248-249`); the older rule is superseded rather than competing. Promote only against
  a designated reference surface.
- 2026-08-21 — [provisional] full-round radius is 9999px (`pill`) — 447 uses against 424
  for the identical framework value and 22 raw `rounded-[999px]`. Same number, three
  spellings: a naming tie broken on frequency, which confirms nothing.
- 2026-08-21 — [provisional] the type scale is the twelve live roles of the declared v2
  scale — 7144 uses against 1941 for the framework default scale it replaces. The
  thirteenth declared role (`2xs`) has zero live uses and gets no token: a declared role
  with no uses is not a role.
- 2026-08-21 — [provisional] one type family for the whole admin, Inter, with the serif
  retired by a dated owner note carried in the source ("ADDENDUM 2026-07-06: el serif
  muere en el admin", `tailwind.config.js:201`) — 402 uses of the family utilities. The
  two declarations disagree on the fallback stack only (`tokens.css:66` lists Inter
  Variable first, `tailwind.config.js:204` does not); the token above carries the token
  file's stack and the discrepancy is real but sub-role.
- 2026-08-21 — [provisional] elevation is a four-level scale (`--e1`..`--e4`, 811 uses:
  646/92/15/58) with one definition and one consumer chain, and no competing scale — the
  only family in this app that is not drifted. It gets no frontmatter token because the
  format has no shadow group, not because it is unsettled.
- 2026-08-21 — open question: card radius — 14px at 984 uses (`rounded-card`) against an
  8px family at roughly 751 (`rounded-boutique` 373, `rounded-lg` 340, `rounded-[8px]`
  38). The live config annotates BOTH as cards (`tailwind.config.js:245` "cards,
  paneles" and `:248` "override v1.0 — cards"), and the 8px assignment carries a written
  two-paragraph rationale in `docs/admin-design-system-v1.0.md` rather than being drift.
  No token elected: a documented decision losing on count is the owner's call.
- 2026-08-21 — open question: modal and drawer radius — 20px at 12 uses
  (`rounded-sheet`) against 12px at 68 (`rounded-boutique-lg`). The config annotates both
  for the same role (`tailwind.config.js:246,249`) and here the newer declaration is the
  one the code contradicts, better than 5 to 1. No token elected.
- 2026-08-21 — open question: the type scale for the redesign-line surfaces — 430
  arbitrary pixel sizes across 17 distinct values (13px×92, 11px×76, 14px×74, 10px×68,
  12px×61, then a long tail) in `src/components/productos-nl/` and
  `src/components/stock-nl/`, against the scale `src/assets/css/nueva-linea.css` declares
  for exactly those components (`--nl-t-11`..`--nl-t-33`, mechanically extracted from a
  design export) which has **zero** consumers anywhere in `src`, against the global v2
  scale above. Three candidates, no token elected.
- 2026-08-21 — open question: the "fast" motion duration — 120ms declared at
  `tokens.css:85` and reached in practice through a raw utility 1226 times, against 150ms
  declared for the same semantic name at `tailwind.config.js` and reached through the
  named utility 261 times. The named class gives a different number than the token of the
  same name; the CSS variables themselves have zero consumers. No token elected.
- 2026-08-21 — open question: the spacing scale — `--space-1`..`--space-16` declared at
  `tokens.css:73-74` with **zero** consumers outside their own definition; spacing in
  views is raw framework utilities, which this pass did not harvest. No token elected and
  the gap is disclosed rather than filled: a scale nobody consumes is not evidence, and a
  family nobody counted is not either.
- 2026-08-21 — open question: control height — 40px declared at `tokens.css:91` with zero
  consumers of the variable, against six live values (`h-7` 128, `h-8` 245, `h-9` 455,
  `h-10` 662, `h-11` 189, `h-12` 170 — 1849 total) where the declared value is a 36%
  plurality. The same line declares scoped variants ("sm: 32 · lg: 48 · POS: 56") that
  nothing consumes either. No token elected.
- 2026-08-21 — open question: two radius scales are declared in CSS and dead —
  `--radius-sm`..`--radius-pill` in `tokens.css:75` has exactly one match in the whole
  tree, its own definition, and `--nl-r-*` in `nueva-linea.css` is consumed only through
  a raw-value escape in 16 files. Whether the dead one is deleted or wired up is the
  owner's call; neither is a token here.

#### Patrones

- 2026-08-21 — [provisional] the accent group is the only per-tenant surface: one build
  serves every tenant and only `tenant-accent-*` is reassigned, under a
  `[data-tenant]` selector (`tokens.css:110-119`, two tenants declared). Every other
  token is shared, and a component never branches on tenant — it consumes the semantic
  token and the selector does the rest.
- 2026-08-21 — open question: the detail-page header — 4 of the 15 `*DetailView.vue`
  files use the shared `src/components/ui/DetailHeader.vue`
  (`ProveedorDetailView.vue`, `ShopCajaDetailView.vue`, `ShopEmpleadoDetailView.vue`,
  `ShopSucursalDetailView.vue`), and 11 hand-roll a back link and title
  (`CrmClienteDetailView.vue`, `CrmEventoDetailView.vue`, `OrdenCompraDetailView.vue`,
  `properties/PropertyDetailView.vue`, `rentals/ContractDetailView.vue`,
  `rentals/RentalClientDetailView.vue`, `ShopClienteDetailView.vue`,
  `ShopOrderDetailView.vue`, `ShopProductDetailView.vue`, `ShopReclamoDetailView.vue`,
  `ShopTransferenciaDetailView.vue`). The hand-rolled ones are structurally different,
  not merely restyled. A shared component existing is not evidence that it is the
  convention; the owner says whether the 11 migrate or the component retires.
- 2026-08-21 — open question: the deprecated global control classes — `.btn` 41 uses and
  `.btn-primary` 15, in 7 files across three modules, although the stylesheet that
  defines them marks them "PROHIBIDO usar en código nuevo". Several combine them with
  raw framework colors instead of the state tokens
  (`src/views/admin/properties/PropertiesListView.vue:304`,
  `src/components/admin/AdminMediaLibrary.vue:127`). Could be drift, could be modules
  parked on the old system pending their own redesign — the code cannot say which.

### Tienda

#### /admin/shop-stock — Stock

- 2026-08-21 — [provisional] exception to Global/Sistema: this route's tokens come from
  the redesign token layer (`src/assets/css/nueva-linea.css`), not from the global set —
  the layer declares the precedence itself, "si `tokens.css` (v2, doc 47 §32) contradice
  a este archivo, manda ESTE" (`:13-14`), and isolates it under a single root class so
  nothing leaks either way. The view is `src/views/admin/ShopStockView.vue`, which mounts
  that root class and composes the 28 components under `src/components/stock-nl/` and
  `src/components/productos-nl/`.
- 2026-08-21 — [provisional] this route is one of the two surfaces hearsay names as the
  best-built in the app, which is why the reference-surface question above matters here
  first: if the owner designates it, its values confirm and most of `#### Sistema`
  promotes or gets replaced in one pass.

#### /admin/shop-products — Productos

- 2026-08-21 — [provisional] exception to Global/Sistema: same redesign token layer and
  same declared precedence as `/admin/shop-stock`; the view is
  `src/views/admin/ShopProductsListView.vue`.

#### /admin/store-settings — Configuración de la tienda

- 2026-08-21 — [provisional] exception to Global/Sistema: the third and last view in the
  tree that mounts the redesign line's root class
  (`src/views/admin/StoreSettingsView.vue`), under the same declared precedence. Recorded
  because the exception is a property of these three surfaces, not of the module.

#### /admin/shop-order/:id — Detalle de Pedido

- 2026-08-21 — [provisional] this route hand-rolls its detail header: a breadcrumb-style
  back link plus a separate "Volver" link, rather than the shared component's
  `back-to`/`back-label` props (`src/views/admin/ShopOrderDetailView.vue`). Recorded as
  what the surface does, under the app-wide open question in `Global/Patrones` — not as
  a rule.

### Compras

#### /admin/proveedores/:id — Proveedor

- 2026-08-21 — [provisional] this route opens with the shared detail header
  (`src/components/ui/DetailHeader.vue`): back link, then title, then the meta and
  actions slots.

#### /admin/ordenes-compra/:id — Orden de compra

- 2026-08-21 — [provisional] this route hand-rolls its header while its sibling
  `/admin/proveedores/:id` uses the shared component. The module contradicts itself 1 to
  1, which is why nothing about the header is written as a module rule here.

### Sucursales

#### /admin/sucursales/:id — Sucursal

- 2026-08-21 — [provisional] this route opens with the shared detail header.

#### /admin/empleados/:id — Empleado

- 2026-08-21 — [provisional] this route opens with the shared detail header.

#### /admin/cajas/:id — Caja

- 2026-08-21 — [provisional] this route opens with the shared detail header. All three
  detail routes in this module agree, which makes it the only module in the app that is
  internally consistent on the header — evidence toward the `Global/Patrones` open
  question, and still non-reference code, so it does not settle it.

### Inmobiliaria

#### /admin/properties — Propiedades

- 2026-08-21 — [provisional] this route still uses the deprecated global control classes
  and raw framework colors (`src/views/admin/properties/PropertiesListView.vue:304`,
  `class="btn bg-red-600 text-white hover:bg-red-700"`) instead of the state tokens. Not
  an exception: nothing declares one. Recorded here so the `Global/Patrones` open
  question can be answered per module.

#### /admin/inmobiliaria — Resumen

- 2026-08-21 — [provisional] same deprecated control classes as `/admin/properties`
  (`src/views/admin/InmobiliariaDashboardView.vue`). Two routes in this module on the old
  system is what makes "parked module or drift" a real question rather than a stray file.

### Servicio técnico

#### /servicio — Hoy

- 2026-08-21 — [provisional] exception to Global/Sistema: this module renders under its
  own visual line, a scoped token set in `src/assets/css/servicio.css` with its own
  surfaces, ink ramp, accent and a fixed 1.212 line-height, isolated under one root class
  "mismo criterio de aislamiento que `.nl`" and sourced from a design export the file
  names as its authority. It covers the module's routes, which share that shell. Its
  authority pointer is broken: the manifest it cites is an absolute path on another
  person's machine, so the export cannot be re-checked from this repo.
