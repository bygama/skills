# Eval 06: provisional by default, and no token without an election

## Query

"Escribime el DESIGN.md con lo que haya. No tengo tiempo de confirmarte
nada ahora, mandale igual y después lo miro."

## Fixture

The same large multi-module admin: one build, 157 surfaces across 13 nav
groups, no `DESIGN.md`. **No reference surface is designated and the
owner is unavailable for the whole run** — nothing in this fixture can
confirm anything.

- **Control radius** — 10px at 1832 uses, against 340 for the 8px
  utility the older of the two `docs/` design documents states as a hard
  rule for controls, and 32 raw `[10px]`. The count and the newer
  document, the one that declares itself the source of truth, point the
  same way: an election is available here, just not a confirmation.
- **Modal/drawer radius** — 20px at 12 uses, the newer document's
  declared value, against 12px at 68 uses, the older document's hard
  rule. The two inputs contradict each other and the code runs >5:1
  against the newer document. No election is available at all. The 68
  sit in the drawer components of the `ventas` and `stock` modules; the
  12 sit in three dialogs under `configuración`.
- **Type scale** — the build config declares a twelve-role scale and the
  newer document names it current. Live use across those roles is very
  uneven — 2984, 2644, 662, 398, 156, 138, 74, 25, 23, 22, 17, 1 — and
  only the second-largest role has any election work behind it (2644
  against 801 for the framework-default equivalent and 138 for a sibling
  role). A thirteenth role is declared alongside them and has zero live
  uses anywhere. A second, framework-default scale is still live at 1941
  uses, and 430 raw pixel type values across 17 distinct numbers sit
  outside both scales.

## Expected behavior

- [ ] The control radius is elected, and its token goes into the
      frontmatter. The frontmatter is the compile source, so a token
      nothing has confirmed still has to live there for the file to
      generate anything.
- [ ] That election also gets a `[provisional]` entry under `### Global`
      / `#### Sistema` naming the token **and what it beat, with both
      counts** — 10px at 1832 against the 8px control rule at 340. An
      entry reading only "`[provisional]` control radius is 10px" fails
      this box: without the numbers, a later session cannot decide
      whether to promote or demote it.
- [ ] The modal/drawer role gets **no** frontmatter token at all — not
      the newer document's 20px, not the 68-use 12px, nothing. Marking a
      value provisional does not license inventing it, and this is the
      box an answer fails by shipping the value it just said it could not
      elect.
- [ ] That role gets an open-question entry instead, recording the role,
      both candidates with their counts, and where each one is used.
- [ ] The declared type roles are not carried into the frontmatter as law
      because a config file and a document declare them current: a config
      that declares itself current is a candidate, not a reference
      surface, so the eleven evidenced roles with no election work of
      their own ship `[provisional]` exactly like the one that has it —
      and the thirteenth, which no code evidences at all, is not an
      election either and gets no frontmatter token.
- [ ] The mostly-provisional file is delivered as the correct output. No
      marker is dropped, and no election is manufactured on the nearest
      count, to make the file look decided; the share of the output that
      is provisional is reported as the drift measure it is, not as a
      failure of the extraction.

## Why this is RED today

**Box 3 fails, on the baseline's own words.** In
`baseline-extracting-design-md.md`, the emitted frontmatter under `## The
DESIGN.md I would have written` ships `radius: sheet: '20px'` as an
unqualified value, while `## Step 4` of the same run states verbatim "I
did **not** elect a token for this role" about that exact role and
records the code running 68 to 12 against it. The value that shipped is
the one the code contradicts >5:1; it shipped because the newest document
declares it. That is this eval's RED.

**Box 2 fails with it.** `provisional`, `Global` and `Sistema` occur zero
times each in `skills/extracting-design-md/SKILL.md`, so there is no
marker, no global tier and no `#### Sistema` block for such an entry to
live in. The baseline's emitted `## Decisions` carries its doubt in nine
`###` prose sections — six of them headed `### Open —` — with zero
entries in the dated `- YYYY-MM-DD — ` form the format and the lint
require, and no `### Global` heading anywhere in it.

**Boxes 5 and 6 fail.** All twelve evidenced type roles reached that same
frontmatter as plain values, eleven of them with no individual election
work behind them at all, and nothing anywhere in the emitted file is
marked provisional — there is no marker in the skill to mark it with.
Box 5's last clause is the one part already GREEN: the zero-use
thirteenth role was left out of that frontmatter, and the rewrite has to
keep leaving it out.

**Box 1 passes today and is kept on purpose.** The baseline did ship the
control token in frontmatter, and the rewrite must not overcorrect into
holding unconfirmed tokens out of it: that was option C in the parent's
ruling in the lane's `DECISIONS.md`, rejected because it compiles a
near-empty token stylesheet and sends every build back to raw values.

**Box 4 is not the RED either.** Today's step 5 already says to record a
contradicted pattern "as an open question with both variants and their
locations", and the baseline's `### Open — Radius, modal/drawer role`
does carry both variants and both counts (though not their file
locations, which other open questions in that same run do carry). Its
force here is standing next to box 3: that run recorded the open question
**and** shipped the token anyway.
