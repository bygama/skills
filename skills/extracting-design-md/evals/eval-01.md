# Eval 01: drift report — counts measure, they do not elect

## Query

"che, el diseño de este proyecto quedó re inconsistente — cada botón es
distinto y hay como mil grises. Armá el DESIGN.md desde lo que ya hay y
decime qué tan desprolijo está."

## Fixture

A large multi-module admin with no `DESIGN.md`: one build, 157 surfaces
across 13 nav groups. Nobody has designated a reference surface, and the
owner is not available mid-run.

Grays — three scales alive at once:

- the framework's own default gray scale, 2009 occurrences across 49
  files;
- a custom warm scale written to replace it, 2308 occurrences across 39
  files;
- a semantic set (`--<prefix>-muted`, `-secondary`, `-heading`,
  `-disabled`), 6821 occurrences, largest single role 2442.

Exactly **one** file uses two of the three together: the drift is whole
surfaces committed to one scale, not per-line sprinkling. On top of them,
43 raw hex literals covering 21 distinct values, five of which are just
more untracked grays.

Button radius — three treatments for the same control role: 10px at 1832
uses, 8px at 340, and a raw arbitrary `[10px]` at 32. The 8px one is not
an accident. Two prose design documents of unknown authorship sit in
`docs/`: the older (1358 lines) states 8px as a hard rule for controls;
the newer declares 10px current, calls itself the source of truth, and is
cited by number in stylesheet comments across the app.

## Expected behavior

- [ ] The drift report names both families with occurrence counts and at
      least one file:line evidence pointer per family.
- [ ] Severity/order in the report follows spread (how many surfaces are
      affected), not discovery order.
- [ ] Counts measure spread; they do not elect. No variant is named "the
      token" for leading the count — no reference surface is designated
      here, so nothing in this app confirms anything, and the report says
      that instead of reading 1832-against-340 as a verdict.
- [ ] The collapse is still proposed, not abandoned: the report lands on
      fewer tokens than there are raw values, and the variants that lose
      are named migration targets rather than tokens. Each proposed token
      carries `[provisional]` and the count of what it beat, and the
      missing input — which surfaces the owner treats as the reference —
      is recorded as the question that would settle them.
- [ ] A losing variant with a written decision behind it is reported as a
      competing claim, not swept into migration debt: the 340-use 8px
      control rule is one document contradicting another, and which
      document governs is the owner's call.

## Why this is RED today

**Boxes 3, 4 and 5 fail.** Today's step 4 is the entire criterion:
"Semantic role + frequency decide: the dominant or correct variant
becomes the token; the other variants become migration targets, NOT
tokens." In `skills/extracting-design-md/SKILL.md`, `provisional` and
`tie-break` occur zero times each, and the single occurrence of
`reference` is the path `reference/design-md.md` on line 13 — there is no
other criterion to elect on and no marker to carry doubt with.

`baseline-extracting-design-md.md` `## Step 4` is the direct
observation. At the card role the run elected the 984-use value over a
roughly 751-use family "per the rule as stated", in the same paragraph
where it recorded that the losing family "wasn't sloppy drift, it was a
considered decision" carrying a two-paragraph written rationale, and
called its own election "exactly the kind of near-tie the skill would
want an owner's eyes on". The count decided it, and the emitted
frontmatter carries the result unqualified.

Boxes 1 and 2 pass today and are kept deliberately. That same baseline's
`## Step 2` produced real counts with file:line samples for every family,
and its `## Step 3` opens "Ordering by spread (broadest first)". The
rewrite has to keep both: a skill that answered "no reference surface, so
no numbers" would fail box 1 as surely as today's fails box 3.
