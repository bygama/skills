# Eval 04: discovery when no DESIGN.md exists

## Query

"Agregale a la vista de existencias un panel de movimientos recientes —
las últimas entradas y salidas — arriba de la tabla."

## Fixture

A large multi-module admin: one build, 157 surfaces across 13 nav groups,
175 shared components. There is **no** `DESIGN.md` anywhere in it, no
generated token stylesheet, and no design-check command. A standing
system exists anyway:

- 83 `--<prefix>-*` custom properties defined in one stylesheet; the
  target surface and most of its module are built on them.
- The app's own `AGENTS.md` carries a dated gotcha naming that
  custom-property line "the current visual line" and citing two standards
  documents — neither resolves from the app directory (both sit two
  levels up, at the monorepo root).
- `docs/` holds five prose documents of unknown authorship and unknown
  currency, the largest ~82 KB, none of them in DESIGN.md shape.
- Dated owner decisions are scattered through source as inline comments
  carrying an owner, a date and a ticket id — e.g. "KPIs en 4+4+4 con
  jerarquía por importancia (dueño, 2026-06-02, ADM-114)".
- The target surface's sibling page in the same module is built on an
  older, entirely different token set: two systems side by side in one
  feature area.
- A shared header component exists, uses the current token set, and looks
  like the convention — 4 of the 15 detail surfaces use it; the other 11
  hand-roll their own header.

Nobody has designated a reference surface, and the owner is not
available mid-run.

## Expected behavior

- [ ] Step 1 returns a ranked inventory of what already governs — the
      tokens in code, the `AGENTS.md` gotcha, the prose docs, the inline
      dated owner decisions — instead of stopping at "no DESIGN.md".
- [ ] The inventory is ranked by source trust, not by how official a
      source looks: code at owner-designated reference surfaces would
      confirm, the `AGENTS.md` gotcha and the prose docs are candidates,
      and frequency in non-reference code is a tie-break only.
- [ ] Nothing discovered is emitted confirmed. No reference surface is
      designated here, so every discovered item enters `[provisional]`
      and which surfaces are the reference is recorded as an open
      question for the owner; the two unresolvable pointers are reported
      unresolved, not taken as the standing line by proxy.
- [ ] The shared header is not adopted as the convention on the strength
      of existing and looking official: its coverage is checked across
      the surfaces it would apply to (4 of 15), so it is recorded as an
      open question for the owner rather than as a standing decision.
- [ ] No DESIGN.md instantiation is proposed while that system is
      discoverable; instantiating is the last option, and whatever it
      later writes records the discovered system as `[provisional]`.

## Why this is RED today

**Box 1 fails.** `baseline-designing-consistently.md` `## Step 1` records
six searches for a DESIGN.md-shaped file, zero hits, and a dead end — no
inventory, no ranking, nothing carried forward, and the skill's own
fallback unreachable. The standing system in that run (the token line the
app's `AGENTS.md` names, the 288-line stylesheet defining it, the inline
dated owner decisions with names and ticket ids) surfaced only at
`## Step 2`, where the runner labels it "Improvisation (flagged — the
skill did not tell me to do this)".

Boxes 2-4 fail with it: `provisional`, `reference surface`, `rank` and
`trust` occur zero times in today's `SKILL.md`, so no source is ranked
against another, and `## Step 3` of that baseline shows the shared-
component trap live — the run reused a shared component because it
existed and was commented as shared, checking no sibling surface for how
widely it is actually used.

**Box 5 is a regression guard on the rewrite, not this eval's RED**
(controller ruling in the lane's `DECISIONS.md`, answering step 4's
flagged item 1): today's skill reaches no proposal at all, so that box
passes trivially against the unfixed skill. The RED is the positive
clause — discovery happens, and it is ranked.
