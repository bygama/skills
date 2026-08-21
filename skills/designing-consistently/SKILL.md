---
name: designing-consistently
description: Keeps UI work consistent with the design system an app actually has — discovering what already governs when no DESIGN.md exists, reading a bounded slice of the decision log however large the app is, and repairing that record as the work proves it. Use when building or modifying UI in any repo, whether its system lives in a DESIGN.md or only in tokens, context-file gotchas and prose docs, when new screens must match existing ones, when an app has too many surfaces to read its whole decision log in one session, when buttons or patterns come out different every session, or when design decisions get lost between sessions.
---

# Designing consistently

UI drift has two sources: styles invented in-session instead of consumed
from the system, and decisions that live only in conversation memory. The
record of those decisions is a **living file** — honoring it is half the
loop; writing back is the other half, and it is the half that gets
skipped.

Two things make that hard in a real app. The system often is not a file
yet: it is tokens in a stylesheet, dated gotchas in the repo's own agent
context file, owner decisions left as comments in source. And once the
file exists, it outgrows a session — a hundred and fifty surfaces is a
normal size, and reading it whole is neither possible nor a virtue.

## Workflow

Copy this checklist and tick items off:

```
Consistency progress:
- [ ] 1. Discover what governs
- [ ] 2. Read the slice
- [ ] 3. Build consuming the system
- [ ] 4. Record + repair (gate)
- [ ] 5. Verify
```

**1. Discover.** Produce a written inventory of what already governs the
surfaces about to be touched, ranked by who backs each source — never by
how official the file looks. A missing DESIGN.md ends nothing; the system
is somewhere else.

Where to look: token definitions in stylesheets (custom properties, theme
config), the app's own DESIGN.md if it has one, the repo's agent context
files (`AGENTS.md` gotchas), prose design docs, dated owner decisions left
inline in source comments (an owner name, a date, a ticket id), and the
shared components the surfaces already use.

| Rank | Source | Status on entry |
|---|---|---|
| 1 | Code at surfaces the **owner designated** as reference | **confirms** |
| 2 | Owner context files — `AGENTS.md` gotchas, dated inline owner decisions | `[provisional]` — the strongest candidates |
| 3 | Prose docs found in the repo (authorship and currency unknown) | `[provisional]` |
| 4 | Frequency in non-reference code | tie-break only, never a source of truth |

- **Nothing discovered is emitted confirmed.** Only the owner, or code at
  a designated reference surface, confirms. When nobody has designated
  one — the usual case — every discovered item enters `[provisional]`,
  and *which surfaces are the reference* is itself recorded as an open
  question for the owner.
- **Coverage before convention.** A shared component that exists and
  looks like the convention is a candidate until its coverage is counted
  across the surfaces it would apply to. Four of fifteen is a split, not
  a rule: record it as an open question, not as a standing decision.
  Frequency breaks ties between candidates; it never promotes one.
- **A pointer that does not resolve is reported unresolved** — a gotcha
  citing a document that is not where it says is evidence of rot, not the
  standing line by proxy.
- **The app's own DESIGN.md is the record, not a rank-3 doc**: its
  confirmed entries are binding, its `[provisional]` entries are
  candidates. Any *other* design doc found in the repo is rank 3.
- **Instantiating a DESIGN.md is the last option, never the first
  answer.** Discover first and work from what was found; propose the file
  only when nothing governs, or when the owner asks. Whatever it writes
  records the discovered system as `[provisional]`.

Finding a file is not evidence that it governs.

**2. Read the slice.** The decision log may hold every surface in the app.
Read exactly this much, in this order:

1. `### Global`, **both** sub-blocks — `#### Sistema` (values and scales:
   tokens, radii, type, modes, which surfaces are reference) and
   `#### Patrones` (form and behavior crossing modules). Always, whatever
   page is being touched, before anything module-specific. A global is
   written as a rule and names no route, which is exactly why it does not
   look relevant and is.
2. The touched page's own `#### <route> — <page>` entries.
3. The 2-3 sibling pages in the same module the surface must match.

Plus, from the frontmatter, the tokens and components the work will need.

Stop there. The other modules are outside the slice: not read, not cited.
Working through the whole `## Decisions` section fails this step as surely
as skipping `### Global` does — at this size an unbounded read means
reading nothing carefully.

The addressing an extraction writes and this step reads:

```
## Decisions
### Global
#### Sistema
- 2026-06-02 — [provisional] cards use radius 14px; 10px is the migration target — 984 uses against 751, both in non-reference code
#### Patrones
- 2026-05-14 — detail surfaces open with the shared header: back link, then title, then actions
### <module>
#### <route> — <page>
- 2026-07-11 — <decision>
- 2026-07-08 — exception to Global/Patrones: <what> — <why>
```

`### Global` is always the first section under `## Decisions`, and
`[provisional]` sits inside the entry text, right after the date. Module
names come from the app's own declared navigation source when it has one,
else the route tree, else ask.

**Precedence.** The more specific wins **only if it declares the
exception**, in that `exception to Global/<block>: <what> — <why>` form.
Without the declaration, a module entry contradicting a global is drift,
and step 4 flags it — it is not a local override. `[provisional]` never
beats confirmed at any level. Confirmed against confirmed with no declared
exception: stop and ask. An exception declared on another surface is
scoped to that surface and is never precedent here. And silence is not
freedom: a page whose own entries say nothing about a point is governed
by the global on it, not exempt from it.

**3. Build.** Styles come from the generated `design.tokens.css`, never
raw values. Reuse an existing component pattern when one fits; a genuinely
new pattern is born tokenized: values added to DESIGN.md frontmatter, then
`design-md-gen` regenerates. The line: one-off micro-layout offsets (a
22px nudge, a hairline width) may stay inline, but new **colors, type
sizes, radii, and shadows** always go through the frontmatter — a 13px
type size dropped inline is a type-scale escape, not a nudge.

A **confirmed** entry the work conflicts with is renegotiated with the
owner — never silently overridden. A **`[provisional]`** entry the work
conflicts with is evidence, not a violation: build the better thing and
carry the conflict to step 4, which demotes the entry. The work does not
stop to renegotiate a candidate.

**4. Record + repair (gate).** Work is not complete while any of these is
true: a decision is unrecorded, a `[provisional]` entry the work touched
still sits as it was, or a contradiction the work exposed is unflagged.
Appending today's decision satisfies only the first.

*Record.* Each new decision is one line, `- YYYY-MM-DD — <decision>`,
under its `### <module>` → `#### <route> — <page>`. Recordable means: a
new pattern (a timeline rail, an empty-state shape), a changed
presentation of an existing decision (the back button moving into a
sticky header), or a layout choice a future session could contradict.
Honoring the existing entries does NOT satisfy this gate — new decisions
were made the moment the page looks different than before.

*Repair status* — every `[provisional]` entry the work touched moves, in
place:

- **Promote** — the work was built on it and it held: drop the marker and
  re-date the entry to today. This is the earned-by-work channel; it needs
  no reference surface and no owner reply. (Confirming something merely
  *discovered* is the other channel, and that one does need the owner or a
  reference surface.)
- **Demote** — the work contradicted it: replace it with the corrected
  entry plus one line saying what beat it. The corrected entry may be its
  removal, recorded, when a confirmed entry already covers the surface —
  provisional entries are exempt from the never-silently-drop rule that
  binds confirmed ones.

*Repair scope* — in the same pass:

- **Escalate** when the same decision now stands in two or more modules:
  move it up to `### Global`, `#### Sistema` for a value or scale,
  `#### Patrones` for form or behavior, rewritten as a rule that names no
  route. It carries its status up unchanged — escalation moves scope, it
  does not confirm.
- **Narrow** when a legitimate, owner-confirmed contradiction proves a
  global is really one module's: move it down into that module.
- **Flag drift** when a module entry contradicts a global without
  declaring `exception to Global/<block>`. Either the owner confirms the
  exception and the entry is rewritten to declare it, or the entry is
  corrected to the global.

Done is reported only once the gate is clear, and the report names the
entries recorded and the entries repaired.

**5. Verify.** Regenerate the tokens with `design-md-gen` and run
`agent-lint` — the design checks passing is the gate, not optional polish.
Re-check each touched surface against its slice: `### Global` first, then
its own entries. Screenshot the result when the environment allows it.

## Judgment notes

- Consistency beats novelty by default; when the brief explicitly asks for
  a new direction, update the record first, then build.
- Entries are one line each — longer rationale belongs in the prose
  sections, referenced from the entry.
- A provisional entry carries what it beat and by how much where counts
  exist. `[provisional] cards are 14px` alone leaves the next session no
  way to decide it.
- When the record and the code disagree, status decides who asks: a
  confirmed entry against contradicting code is a stop-and-ask; a
  provisional entry against code at a reference surface is a demotion.
  Never pick silently.
- A global that names a route is almost always a misplaced module entry.
  Put it back down.

## Red flags

| Said in-session | What it actually means |
|---|---|
| "I found a design doc, so I know the system" | A file's existence is not evidence that it governs. Unknown authorship, unknown currency — rank 3, `[provisional]`, until the owner or a reference surface says otherwise. |
| "There is no DESIGN.md, so there is no system" | Step 1 was not run. The system is in the tokens, the gotchas and the owner's inline decisions. |
| "This component is shared and looks official, so it is the convention" | Its coverage was never counted. Frequency is a tie-break, not truth. |
| "I read the whole Decisions section to be safe" | The slice was skipped. At a hundred surfaces that read is unbounded, and unbounded means nothing was read carefully. |
| "The touched page has no entry about it, so nothing governs it" | `### Global` was not read first. Globals name no route on purpose. |
| "I appended my decision, so the gate is satisfied" | Recording is half of step 4. The provisional entries the work touched still have to move. |
| "The task didn't mention DESIGN.md" · "I'm in a hurry" | The two ways drift happens. Neither skips step 2 or step 4. |
