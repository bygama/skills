# Eval 05: reference surfaces elect, frequency only breaks ties

## Query

"Los tokens buenos son los de productos y stock, eso está bien hecho.
Armá el DESIGN.md tomando eso como referencia y decime qué hacemos con
el resto."

## Fixture

The same large multi-module admin: one build, 157 surfaces across 13 nav
groups, no `DESIGN.md`. The query designates two modules — `productos`
and `stock` — as the reference surfaces. Nothing else in the app is
designated, and the owner is unavailable for follow-up once the run
starts.

Two prose design documents sit in `docs/`, both of unknown authorship:

- the older one (1358 lines) assigns **8px** to cards, with a
  two-paragraph written rationale comparing three commercial admin
  products — a deliberate, documented decision, not drift;
- the newer one (57 KB) assigns **14px**, declares itself the source of
  truth, and is cited by number in stylesheet comments across the app.
  The migration it implies was never completed.

Counts across the whole app:

- **card role** — 14px at 984 uses (the newer document's value) against
  an 8px family of roughly 751 (a named 8px utility at 373, a shared 8px
  utility used in card context at 340, raw `[8px]` at 38).
- **modal/drawer role** — 20px at 12 uses (the newer document's value)
  against 12px at 68 (the older document's hard rule for modals).
- **pill role** — three spellings of one identical full-round value: a
  semantic alias at 447, the framework default at 424, a raw `[999px]`
  at 22.

**Every card inside `productos` and `stock` is built at 8px.** Neither
module renders a modal or a drawer anywhere.

## Expected behavior

- [ ] The card token is elected from what the reference surfaces
      evidence — 8px — even though 14px leads the app 984 to ~751.
      Reference-surface code is the only thing here that confirms; a
      count from everywhere else is not a competing authority.
- [ ] The 984 uses of 14px are reported with their count and become
      migration targets. The count says how much has to move, never what
      is right: an answer that elects 14px because it is the bigger
      number fails this box however carefully it flags the doubt.
- [ ] Neither document confirms anything. The newer one does not govern
      by being newer; the older one does not govern by carrying a written
      rationale. Both are prose docs of unknown authorship and currency,
      i.e. candidates — the conflict between them is recorded as an open
      question, and the role is settled by the reference surfaces
      instead.
- [ ] The modal/drawer role is not elected at all. The reference modules
      render no modal, so nothing confirms it, and the two remaining
      inputs point opposite ways: the code says 12px at 68 uses, the
      newer document says 20px at 12 — a >5:1 lead against the document
      that calls itself current. It goes to the owner as an open question
      carrying both counts and where each is used.
- [ ] Frequency still settles the pill role: 447 / 424 / 22 are three
      spellings of one identical value with no design question inside
      them, and breaking that tie among non-reference surfaces is exactly
      what frequency is for. The elected name still ships
      `[provisional]` — no reference surface confirmed it either.

## Why this is RED today

**Box 1 fails, and box 2 falls with it.** Today's step 4 says "Semantic
role + frequency decide: the dominant or correct variant becomes the
token". In `skills/extracting-design-md/SKILL.md` the phrase `reference
surface` occurs zero times — the file's one `reference` is the path
`reference/design-md.md` on line 13 — and `tie-break` and `provisional`
occur zero times each. A box that requires the election to come from the
reference surfaces cannot pass against a skill that has no such concept,
and a box that requires the losing 984 to be reported as spread rather
than authority has nothing in the skill text to rest on.

`baseline-extracting-design-md.md` `## Step 4` is the same case, run for
real: 984 against roughly 751 at the card role, elected "per the rule as
stated", while the run recorded in that same paragraph that the losing
family "wasn't sloppy drift, it was a considered decision" with a
two-paragraph rationale in the older document, which a later document
"apparently overrode without the migration having caught up", and called
its own election "exactly the kind of near-tie the skill would want an
owner's eyes on". It elected it anyway, because the rule it was following
left it no other move.

**Box 3 fails with them.** The rule that run actually applied was "(a)
the largest live occurrence count, and (b) alignment with the codebase's
own most recent self-declared 'source of truth' comment" — the newest
document is half the decision rule today, which is precisely the
authority box 3 denies it.

**Box 4 is not this eval's RED**, stated plainly. The same baseline
reached the same disposition at that role: `## Step 4` records "I did
**not** elect a token for this role" for the modal/drawer case and
carried it to `## Step 5` as an open question, so a faithful run of
today's skill passes box 4 at the election stage. What that run then
emitted is a different failure — `sheet: '20px'` shipped in its
frontmatter anyway — and that one is `eval-06`'s RED, not this eval's.
Box 4 is here because it is where a frequency rule and a
reference-surface rule visibly diverge, and because a rewrite that
elected the 68-use value on count would break it.

**Box 5 is a guard against overcorrection**, half of it already GREEN:
the baseline elected the semantic pill name over its two spellings and
would pass the first sentence. Its last sentence — the `[provisional]`
marker on a token no reference surface confirmed — fails today for the
same reason box 1 does.
