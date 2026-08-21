# Eval 01: the repair gate — promote, demote, escalate

## Query

"Agregale a la vista de existencias el panel de movimientos recientes
arriba de la tabla, igual que el que hicimos en compras."

## Fixture

The same multi-module admin, with a `DESIGN.md` whose `## Decisions`
opens with `### Global` and continues into `### <module>` /
`#### <route> — <page>` sections:

- `### Global` → `#### Sistema`:
  `- 2026-06-02 — [provisional] cards use radius 14px; 10px is the
  migration target — 984 uses against 751, both in non-reference code.`
- `### Global` → `#### Patrones`:
  `- 2026-05-14 — a summary panel sits above the filter + table block,
  never inside the table's own control stack.` (confirmed)
- `### inventario` → `#### /inventario/existencias — Existencias`:
  `- 2026-07-11 — [provisional] summary panels on this page sit below the
  table.` — no exception declared.
- `### compras` → `#### /compras/ordenes — Órdenes de compra`:
  `- 2026-08-13 — [provisional] the recent-movements panel reuses the
  shared summary-card chrome in its list variant.`

The session builds the panel on the existing card radius and places it
above the table, following `#### Patrones` and the owner's ask.

## Expected behavior

- [ ] Promotes the `[provisional]` radius entry the work was built on:
      drops the marker and re-dates it to today, in place — it was
      touched, it held. Promotion here is the earned-by-work channel, so
      no reference surface and no owner reply is needed for it; needing a
      reference surface or the owner to confirm governs **discovery**,
      which is a different channel.
- [ ] Demotes the module's `[provisional]` below-the-table entry the work
      contradicted: replaces it with the corrected entry plus one line
      saying what beat it. The corrected entry may be its removal,
      recorded — the confirmed global already covers the surface, and a
      provisional entry is exempt from never-silently-drop. Contradicting
      a provisional entry is evidence, not a violation — the work does
      not stop to renegotiate it. This box and the next describe one
      disposition of that entry, not two.
- [ ] Treats that module entry's contradiction of `### Global` /
      `#### Patrones` as drift rather than a local override: it never
      declared `exception to Global/Patrones`, and provisional never
      beats confirmed. A legitimate exception has to be written as
      `- YYYY-MM-DD — exception to Global/<block>: <what> — <why>`.
- [ ] Escalates the shared-chrome panel decision to `### Global` /
      `#### Patrones` now that it stands in two modules, carrying its
      `[provisional]` status up unchanged — escalation moves scope, it
      does not confirm.
- [ ] Does not report done while a provisional entry the work touched
      still sits as it was. Appending today's new decision under the
      touched page does not satisfy the gate on its own.

## Why this is RED today

**Boxes 1 and 2 fail** — with boxes 3 and 4 behind them. Today's step 4
is additive by construction: "Each gets `- YYYY-MM-DD — <decision>` under
its `### <surface>`", with no move that revisits an entry already in the
file. `promote`, `demote`, `provisional`, `escalat` and `Global` occur
zero times in `SKILL.md`, so there is no status to promote or demote, no
scope to escalate, and no global tier to escalate into.

`baseline-designing-consistently.md` `## Step 4` is the direct
observation: the run's would-be record is three flat one-line additions
under a single `### <surface>` heading, and no existing entry is
revisited, re-dated or re-scoped anywhere in the run. Box 2 also fails in
the opposite direction — step 3 makes any conflict a stop-and-ask ("A
standing decision the work conflicts with is renegotiated with the user
— never silently overridden") with no exemption for a candidate, so a
faithful run halts on the below-the-table entry instead of demoting it.

Stated honestly: that cold run found no `DESIGN.md` at all, so no
provisional entry existed for it to repair. The additive-only shape of
its record is what was observed; the absence of every repair verb from
the skill text is what makes the first two boxes unreachable today.
