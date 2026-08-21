# Eval 02: faithful backfill — record what is, ask what conflicts

## Query

"Armá también la sección de Decisions con lo que el proyecto ya decidió."

## Fixture

Three detail views in the same module share an identical back button
(top-left, same classes); two listing pages render the same data type with
two different card styles (bordered vs shadowed). Nobody has designated a
reference surface, and the owner is not available mid-run.

## Expected behavior

- [ ] The consistent back button becomes a dated `[provisional]` entry
      under `### <module>` → `#### <route> — <page>` for each surface it
      covers, citing the evidence (the files where it appears). Both
      halves are required: the address is the module/route architecture,
      not a flat per-surface heading; and the marker is not optional —
      no reference surface is designated here, so three views agreeing is
      evidence from non-reference code, a candidate and never a
      confirmation. An entry recorded as settled fails this box however
      well it cites the three files.
- [ ] The contradictory card styles do NOT get a decision entry — they are
      surfaced as an open question for the owner with both variants and
      their locations.
- [ ] No decision is invented that the code does not evidence.

## Why this is RED today

**Box 1 fails, on both halves.** Today's step 5 says the consistently
evidenced pattern "becomes a dated entry under its `### <surface>`" —
the flat per-surface addressing SPEC §3.3 replaces, and no marker of any
kind. `provisional` occurs zero times in
`skills/extracting-design-md/SKILL.md`.

`baseline-extracting-design-md.md` shows both halves failing in the
emitted file. Its `## Decisions` is nine `###` prose sections addressed
by token family or pattern — no `### <module>` heading and no
`#### <route> — <page>` subsection anywhere — and the two it recorded as
decided (`### 2026-08-20 — Radius, control role` and the pill/full one)
carry no marker, in a run where no reference surface was ever designated.

Boxes 2 and 3 pass today and are unchanged. That same run recorded the
contradicted pattern it checked as an open question carrying both
variants and their file locations, and invented no decision the code did
not evidence.
