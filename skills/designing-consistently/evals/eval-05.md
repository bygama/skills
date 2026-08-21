# Eval 05: bounded slice at 157 surfaces

## Query

"Armá la pantalla de detalle de órdenes de compra — que quede igual que
las otras del módulo."

## Fixture

The same large multi-module admin, now with a `DESIGN.md` an extraction
has already written. Its `## Decisions` opens with `### Global`
(`#### Sistema`, `#### Patrones`), then 13 `### <module>` sections holding
157 `#### <route> — <page>` subsections between them — roughly 400 dated
entries in all, far past what one session can read.

- `### Global` → `#### Patrones`:
  `- 2026-05-14 — detail surfaces open with the shared header: back link,
  then title, then actions.`
- `### Global` → `#### Sistema`:
  `- 2026-06-02 — [provisional] cards use radius 14px; 10px is the
  migration target.`
- The touched page, `### compras` → `#### /compras/ordenes-detalle —
  Detalle de orden`, has 3 entries: totals column alignment, an empty
  state, and a print action. **None mentions a header.**
- Two sibling pages in the same module do:
  `#### /compras/proveedores-detalle — Detalle de proveedor` carries
  `- 2026-06-02 — the back link returns to the module's list, never to
  browser history.`, and `#### /compras/ordenes — Órdenes de compra`
  carries `- 2026-07-01 — the list's row click opens the detail in the
  same tab.`
- In another module, `### crm` → `#### /crm/clientes-detalle — Detalle de
  cliente` carries `- 2026-07-08 — exception to Global/Patrones: this
  surface opens with the search bar above the header — <why>.`

## Expected behavior

- [ ] `### Global` is read first — both sub-blocks — before anything
      module-specific, even though the query names one page and no global
      entry names a route.
- [ ] The touched module's section is read as the touched page's own
      entries plus the 2-3 sibling pages the surface must match, not
      every page the module holds.
- [ ] The other 12 modules and the ~145 pages outside the slice are not
      read and nothing from them is cited: an answer that works through
      the whole `## Decisions` section fails this box as surely as one
      that skips `### Global`.
- [ ] The delivered header follows the `#### Patrones` entry and the
      sibling's back-link precedent. An answer built from the touched
      page's own 3 entries alone ships a header contradicting a standing
      global — that is the failure this eval exists to catch.
- [ ] The `exception to Global/Patrones` declared in the other module is
      not carried over as precedent: an exception is scoped to the
      surface that declares it. This box is a second-order trap for the
      read-everything answer, not an independent test — a run that
      correctly holds to the slice never reads that module, so it passes
      here by never seeing the entry. Read a pass on this box together
      with box 3, never as evidence on its own.

## Why this is RED today

**Box 1 fails, and box 2 with it.** Today's step 2 reads "every entry
under the surfaces (routes/screens) about to be touched" — the touched
surface and nothing else. `Global` and `sibling` occur zero times in
today's `SKILL.md`, so there is no global tier to read first and no
sibling rule to bound the read with; a run that follows the skill exactly
lands on the touched page's 3 entries, none of which mentions a header,
and box 4 falls with box 1.

The cold run could not exercise this directly, and the lane says so:
`baseline-designing-consistently.md` `## Step 2` had "zero material to
read from its prescribed source — not 'few entries,' literally none",
and the step-4 ruling in `DECISIONS.md` records the slicing behavior
**unobserved** for this skill for exactly that reason. Where the same
"read each surface" shape did meet these 157 surfaces — the lane's
`extracting-design-md` baseline, `## Step 5` — the run read none of them
and substituted a bound of its own off script, disclosing it as "a
deliberate, bounded substitution"; neither skill supplied a bounding
rule. Box 1's failure needs neither run to settle it: a box that requires
`### Global` to be read cannot pass against a skill that has no such
tier.

Box 5 is **not** part of this eval's RED, for the reason stated in the box
itself: a run holding to the slice passes it by never reading the module
that declares the exception.
