---
name: extracting-design-md
description: Reverse-engineers a DESIGN.md (Google Labs format) from an already-built project — electing tokens from the surfaces the owner designates as reference rather than from whichever value is most frequent, shipping everything unconfirmed as `[provisional]`, and writing decisions per module under a `### Global` tier. Use whenever an existing codebase should adopt DESIGN.md, when UI values have multiplied (several grays, mixed radii, inconsistent buttons), when the owner says the UI looks inconsistent or "quedó desprolijo", when an app has too many surfaces for one flat list of decisions, or to re-audit drift after a migration batch — even if nobody says the word DESIGN.md.
---

# Extracting DESIGN.md

Turns the design system a codebase *actually uses* into a DESIGN.md the
tooling can enforce. Output format matters as much as analysis quality: a
freeform writeup can be excellent and still unenforceable — only the spec
format (YAML frontmatter tokens + prose sections + `## Decisions`) plugs
into the generator, the lint gates, and `designing-consistently`.

The hard part is not finding the values. It is writing them down without
canonizing drift. A drifted app disagrees with itself, and only two
things can settle a disagreement: the owner, and the code at the surfaces
the owner points to. Counts cannot. A value with 984 uses and nothing
behind it is 984 surfaces of spread, not a decision.

So a first extraction from a drifted app comes out **mostly
`[provisional]`, and that is the correct output** — the pull toward
making it look decided is the failure this skill exists to stop.

## Workflow

Copy this checklist and tick items off:

```
Extraction progress:
- [ ] 1. Inventory sources and rank them
- [ ] 2. Harvest and cluster values
- [ ] 3. Drift report
- [ ] 4. Elect tokens from the reference surfaces
- [ ] 5. Backfill Decisions (Global + modules)
- [ ] 6. Write DESIGN.md → generate → lint
- [ ] 7. Migration plan + hand-off
```

**1. Inventory and rank.** List every source that could speak for the
design: token definitions in stylesheets and theme layers, the build's
style config, the classes and inline styles components actually use, the
repo's agent context files, prose design docs, dated owner decisions left
inline in source comments. One extraction per app in a monorepo.

Then ask the question that decides the whole run: **which surfaces does
the owner consider done right?** Those are the reference surfaces. Ask
early — it is the cheapest question in the extraction and the only thing
in the app that can confirm anything.

| Rank | Source | Status on output |
|---|---|---|
| 1 | Code at the surfaces the **owner designated** as reference | **confirms** |
| 2 | Owner context files — agent context gotchas, dated inline owner decisions | `[provisional]` — the strongest candidates |
| 3 | Prose design docs and declared style config found in the repo (authorship, currency unknown) | `[provisional]` |
| 4 | Frequency in non-reference code | tie-break only, never a source of truth |

- **Nothing extracted is emitted confirmed.** When nobody has designated
  a reference surface — the usual case, and the case when the owner is
  unavailable mid-run — nothing in the app confirms anything, the whole
  output runs `[provisional]`, and *which surfaces are the reference* is
  itself recorded as the open question that would settle the file.
- **A declaration is not a confirmation.** A doc that calls itself the
  source of truth, or a config that declares a scale, is rank 3 however
  recent, however widely cited, however carefully argued. The migration
  it implies may never have run — the code says whether it did.
- **Finding a file is not evidence that it governs.** Authorship and
  currency unknown means candidate.

**2. Harvest.** Grep the values by family (colors, radii, spacing, type,
shadows) with occurrence counts and file:line locations — counts are what
make the report objective instead of vibes. Counts measure spread. They
do not elect; step 4 does.

**3. Drift report.** Near-duplicates, outliers, competing patterns; each
family with its variant count, evidence, and the surfaces it touches.
Order by spread (surfaces affected), not by discovery order. Theme modes
are not drift: a selector-scoped reassignment of the same variable
(`[data-theme="dark"] { --paper: … }`) is an intentional mode — it goes
to frontmatter `modes:` (values + selector, per the convention). Drift is
the unscoped, scattered variation. A mode whose values duplicate
another's is the same mode with a widened selector, not a new value set.

**4. Elect tokens.** One question per semantic role: which value does
this role get? The criterion is what the **reference surfaces** use.
Frequency is not the criterion — the biggest count is the biggest
migration, not the right answer. Per role, in order:

1. **A reference surface covers the role** → its value is the token,
   **confirmed**. Every other variant becomes a migration target carrying
   its count, however far ahead it leads. 984 against 751 the other way
   is the size of the move, not a counter-argument.
2. **No reference surface covers it, and the other inputs agree** →
   elect, `[provisional]`. The token goes in the frontmatter (step 6) and
   the entry that carries its doubt goes in `#### Sistema` (step 5).
3. **No reference surface covers it, and the other inputs contradict each
   other** → **no election**. No frontmatter token for that role at all,
   and an open question instead. This is exactly where a frequency rule
   and this one diverge: when the code says one value 68 times and the
   document that calls itself current says another 12 times, frequency
   picks the 68 — this skill picks neither, because a documented decision
   losing on count is the owner's call, not the extraction's.
4. **Frequency's one job** is breaking a tie among non-reference variants
   that carry no design question inside them: three spellings of one
   identical value (a semantic alias, the framework default, a raw
   literal) is a naming tie, and the most-used spelling wins it. The
   elected name still ships `[provisional]` — a tie-break confirms
   nothing.

**No token without an election.** A role the code does not evidence is
not elected: a scale declared in config or a document ships only the
roles that are live, `[provisional]` like any other unconfirmed election,
and a declared role with zero live uses gets no token at all. Marking
something provisional does not license inventing it.

**Provisional is not paralysis.** Outcome 2 is the common case and it
still collapses: 7 grays in the code become one `ink-muted` token plus
six rows in the migration plan. What the marker changes is the standing
of the result, not the number of tokens. An extraction that ends with as
many tokens as there were raw values did not do step 4.

**A losing variant with a written decision behind it is not migration
debt.** Two documents contradicting each other — one assigning 8px to a
role with a written rationale, a later one assigning 10px and calling
itself current — is a competing claim, reported as an open question. The
newer does not govern by being newer, the older does not govern by having
a rationale, and where a reference surface covers the role, the code
settles it and both documents stay candidates.

Confirm token names and palette intent with the owner when the owner is
available; when not, name them by semantic role and say the names are
unconfirmed too.

**5. Backfill Decisions.** The section is addressed by module, with a
global tier first — this is the shape `designing-consistently` reads a
bounded slice out of:

```
## Decisions
### Global
#### Sistema
- YYYY-MM-DD — [provisional] <values and scales: tokens, radii, type, modes, reference-surface declaration>
#### Patrones
- YYYY-MM-DD — <form and behavior crossing modules>
### <module>
#### <route> — <page>
- YYYY-MM-DD — <decision>
- YYYY-MM-DD — exception to Global/<block>: <what> — <why>
```

`### Global` is always the first section under `## Decisions`, with
exactly those two sub-blocks: `#### Sistema` for values and scales
(tokens, radii, type, modes, and the reference-surface declaration),
`#### Patrones` for form and behavior crossing modules. A global is
written as a rule and never names a route. Module names come from the
app's own declared navigation source when it has one, else the route
tree, else ask.

What goes where:

- **Every step-4 election that a reference surface did not confirm** gets
  a `#### Sistema` entry naming the token **and what it beat, carrying
  both counts**: `- 2026-06-02 — [provisional] control radius 10px —
  1832 uses against the 8px control rule at 340, no reference surface`.
  An entry reading only `[provisional] control radius is 10px` is not
  enough; without the numbers a later session cannot decide whether to
  promote or demote it. A role that had no competitor says that, and
  carries its own count instead.
- **The reference-surface declaration** is itself a `#### Sistema` entry:
  which surfaces the owner designated, or that none was designated and
  the whole file is therefore provisional.
- **A pattern the code evidences consistently** (the same back button in
  three views) becomes a dated entry under `### <module>` →
  `#### <route> — <page>`, one under each surface it covers, citing the
  files it was found in. Three views agreeing is evidence from
  non-reference code, so the entry ships `[provisional]` — repetition is
  not confirmation.
- **A pattern standing in two or more modules** is written once under
  `#### Patrones`, as a rule naming no route, carrying its status
  unchanged. Within a single module it stays per route.
- **A pattern the code CONTRADICTS** (two card styles for the same data)
  gets no decision entry. It becomes an open question, placed where the
  decision would have gone — value and scale questions under
  `#### Sistema`, surface questions under their module:
  `- 2026-06-02 — open question: modal radius — 12px at 68 uses in the
  drawers of two modules vs 20px at 12 in three dialogs of a third`.
  Every candidate carries its count and where it is used. An open
  question is not a rule, so this is the one entry under `### Global`
  that does name locations — it has to, or the owner cannot settle it.
- **Nothing else.** A decision the code does not evidence is not written
  down, in either marked or unmarked form. Deciding it yourself hides
  exactly the inconsistency the owner asked you to surface.

Every entry keeps the `- YYYY-MM-DD — ` prefix the lint and the generator
require; `[provisional]` sits inside the entry text, immediately after
the date.

*Bounded backfill.* Do not read every surface — at a hundred and fifty
that read either does not happen or does not finish, and a bound invented
mid-run is not a method. Patterns are found by **searching** for their
markers across the whole tree; the counts, the file lists and the
contradictions all come from the search. Read in full only the reference
surfaces, the shared components the routes import, and up to ~3 routes
per module where the search disagrees with itself. Then state the bound
in the output: how many surfaces were read out of how many, and which.

**6. Write + gates.** DESIGN.md in the spec format: frontmatter tokens
(quoted hex), prose sections in spec order, `## Decisions` with
`### Global` first. The frontmatter is the compile source, so an elected
but unconfirmed token lives there like any other — holding it out would
generate a near-empty stylesheet and send every build back to raw values.
Its doubt is carried by its `#### Sistema` entry, not by its absence.
Roles with no election have no entry in the frontmatter at all.

Then run `design-md-gen` (it must parse *and* generate) and `agent-lint`
— the design checks passing is the definition of "the file is real", not
optional polish. Zero application code files are edited in this skill;
the only writes are DESIGN.md, its generated `design.tokens.css`, and the
migration plan's companion doc.

**7. Migration plan + hand-off.** Per-surface batches, effort-sized, with
exact find→replace rows per fix (mechanical to execute), in a companion
doc — never inside the always-consumed DESIGN.md. Execution belongs to
`designing-consistently`: its read-consume-record loop takes over from
here, and it does not need this extraction to have run — it discovers
what governs on its own and consumes whatever this produced.

State the metric: re-run the harvest after each batch; the drift finding
count must go down; done at zero findings + lint PASS. "Pixel perfect" is
a measurement, not a feeling. Report the provisional share the same way —
it is the second drift number, and it falls as sessions promote entries
by building on them, never by the extraction getting bolder.

## Scaling (bounded)

Harvesting is read-only and may fan out — one subagent per app or major
module — consolidated in a single context. Past roughly 15 surfaces a
fan-out is worth proposing, with the owner's opt-in; past a hundred it is
the only way to reach full coverage in one session. When the runtime
refuses subagents, the bound in step 5 still governs: search wide, read
narrow, and report the coverage instead of implying the whole app was
read. Writes never parallelize across surfaces that share components; a
batch finishes its gates before the next starts.

## Judgment notes

- Re-runs are the point, not a smell: extraction on a clean repo yields
  zero findings — that idempotence is what makes the drift count a
  metric.
- If the repo has no build to verify against, say so in the plan — never
  silently skip a verification step.
- The report speaks to the owner: severity comes from how much of the UI
  a family touches, and every claim carries its evidence.
- Two numbers are the deliverable alongside the file: how many findings,
  and how much of the output is provisional. A file that is 80%
  provisional on a drifted app is an honest measurement; the same file
  shipped fully confirmed is a fabrication.

## Red flags

| Said in-session | What it actually means |
|---|---|
| "10px leads 1832 to 340, so 10px is the token" | The count measured spread, not correctness. With no reference surface nothing here confirms anything — elect `[provisional]` and record both numbers. |
| "The newest doc declares itself the source of truth" | Rank 3. Unknown authorship, unknown currency, and a migration that may never have run. It never confirms. |
| "Nothing settles this role, but the file needs a value" | Marking something provisional does not license inventing it. No election means no frontmatter token, plus an open question. |
| "Most of this file is `[provisional]` — tighten it before delivering" | The provisional share *is* the measurement. Tightening it is manufacturing decisiveness. |
| "The config declares twelve type roles, so that is the scale" | A declaration is not a confirmation. Live roles ship provisional; a declared role with zero uses gets no token. |
| "Three views do it the same way, so it is decided" | Evidence from non-reference code. `[provisional]` entry, not a settled one. |
| "I read every surface to backfill Decisions" | At this size that read did not finish. Search wide, read narrow, and say what the coverage was. |
