# PROGRESS — MAT-94

Lane: `work/mat-94-attribution-skills/` · Tier M
Branch: `bygama/mat-94-attribution-skills` (stacked on
`bygama/mat-93-ask-for-help-leg`, PR base = that branch)

## State

- [x] SPEC.md written and committed (c7565bb)
- [x] SPEC approved by parent at the design-first gate (D1)
- [x] PLAN.md shaped (5 steps)
- [x] Step 1 — D2: classify testing-first
- [x] Step 2 — D3: classify tracing-root-causes
- [x] Step 3 — D4: NOTICE judgment (yes; `NOTICE` created)
- [x] Step 4 — notices applied/normalized
- [ ] Step 5 — README Provenance lines
- [ ] work-verify (M) incl. fresh-context review
- [ ] work-handoff: PR open (base mat-93 branch), Linear attach,
      worker_done

## Done

### Step 1 — D2: classify testing-first vs upstream v6.3.0

**Implemented.** Diffed both testing-first files against their upstream
counterparts in
`C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/skills/test-driven-development/`
and recorded the classification in DECISIONS.md under
`## D2 — classification: testing-first`. Version and license confirmed
from the upstream tree itself (`.claude-plugin/plugin.json` →
`"version": "6.3.0"`, `"license": "MIT"`; `LICENSE` → `Copyright (c)
2025 Jesse Vincent`) rather than from the port record.

Method: whole-file read of all four files; a mechanical pass for
whitespace-normalized identical lines (≥20 chars) and longest common
word runs; then targeted `diff -u` over every section those passes
flagged. Every claim in D2 carries local + upstream line numbers.

**Verdicts.**

- `skills/testing-first/SKILL.md` → **SUBSTANTIAL**. Nine identical
  normalized lines, a 40-word verbatim code block (the RED example), a
  byte-identical Iron Law, a 19-word verbatim prose run in the
  rationalization table; and, heavier, dense structural parallels —
  the RED/verify/GREEN/verify/REFACTOR spine, 10 of 11 rationalization
  rows in order, 12 of 13 red flags in order, 7 of 8 checklist boxes,
  the same four When-Stuck rows, the same worked bugfix example, the
  same final-rule block.
- `skills/testing-first/references/writing-good-tests.md` →
  **SUBSTANTIAL** (much stronger). 33 identical normalized lines, a
  111-word contiguous verbatim run, both gate blocks near
  byte-identical, all three code examples verbatim, the ten-row quick
  reference in upstream order with eight rows byte-identical, and
  11-for-11 warning signs several of which differ only by a terminal
  period.

Actions implied (consumed by steps 3-5): whole-file notice form for
both files, normalized in place over the MAT-47 blocks
(`SKILL.md:8-12`, `references/writing-good-tests.md:6-10`) — never
stacked; D4's NOTICE judgment is live, not moot; README Provenance
records testing-first as derived-with-notice.

**Acceptance.**

```
$ grep -q "## D2 — classification: testing-first" \
    work/mat-94-attribution-skills/DECISIONS.md; echo $?
0

$ git diff --name-only HEAD
work/mat-94-attribution-skills/DECISIONS.md
# → no skills/** file touched
```

Lane constraint re-checked (not a step-1 gate):

```
$ node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .
0 high, 0 medium, 1 low — PASS
```

The single LOW is the pre-existing `AGENTS.md:15` cmd-drift note; not
attributable to this lane.

**Files changed.** `work/mat-94-attribution-skills/DECISIONS.md`
(D2 entry appended), `work/mat-94-attribution-skills/PROGRESS.md`
(this report). No skill file edited, per the step.

**Concerns.** None on the verdicts — the writing-good-tests evidence is
overwhelming and SKILL.md's structural case is dense and quotable. One
handoff note for step 4: the SPEC cites the existing testing-first
notices as `SKILL.md:8-9` and `writing-good-tests.md:6-7`; the actual
blocks are five lines each (8-12 and 6-10) and carry a stale
`work/mat-47-house-tdd/DECISIONS.md` pointer that this lane's
normalization supersedes.

### Step 1 — review verdict (fresh reviewer, opus)

Verdict text, verbatim:

> ### Spec compliance
> ✅ Compliant
>
> ### Assessment
> **Step quality:** Approved
> **Reasoning:** The step does exactly what was asked and nothing else —
> two evidence-backed verdicts, zero skill edits, correct heading and
> commit — and every quantitative claim I independently recomputed
> matched to the digit, with line-number spot-checks accurate
> throughout. The five minors are precision slips in "differs only in"
> style sub-claims, all in the direction of slightly overstating
> similarity on a file whose SUBSTANTIAL verdict is already established
> by a 111-token verbatim run and 33 identical lines; worth tightening
> for quotability, but they change no verdict and block no downstream
> step.

The reviewer independently recomputed every mechanical number in D2
(line counts 249/320 and 220/198; 9 and 33 normalized identical lines;
40- and 111-token longest runs) and all reproduced exactly; every
line-number spot-check held. Critical: none. Important: none.

**Deferred minors (for work-verify's triage)** — all five are
precision slips in subordinate evidence claims inside D2/PROGRESS, none
verdict-affecting:

1. DECISIONS.md Gate-1 paragraph: "differs only in …" list misses a
   fifth edit (`and`→`,` at local writing-good-tests.md:91 vs upstream
   :72) — enumerate completely or soften.
2. DECISIONS.md "Every code example is verbatim" caveat understates two
   comment-line rewrites (local 111 vs upstream 92; local 128 vs
   upstream 110); executable lines verbatim claim itself confirmed.
3. DECISIONS.md quick-reference paragraph: local 194 / upstream 177
   carries two edits in one row (`or document`→`or a document` AND
   `Run it /`→`Run it, or`), not one per row.
4. PROGRESS.md step-1 acceptance capture is pre-commit (post-commit
   `git diff --name-only HEAD` is empty); note it or use
   `git show --name-only HEAD`.
5. DECISIONS.md "What is genuinely local": upstream's Bad RED example
   survives in prose at local SKILL.md:68 (uncounted match in the
   verdict's favour); red-flags bullet names 12-of-13 without naming
   the dropped item.

### Step 2 — D3: classify tracing-root-causes vs upstream v6.3.0

**Implemented.** Section-by-section classification of the MAT-46
absorption plus the MAT-93 ask-for-help leg against upstream
`systematic-debugging/` v6.3.0 and its four technique references,
recorded in DECISIONS.md under `## D3 — classification:
tracing-root-causes`. Version and license re-confirmed from the upstream
tree itself (`.claude-plugin/plugin.json` → `"version": "6.3.0"`,
`"license": "MIT"`; `LICENSE` → `Copyright (c) 2025 Jesse Vincent`).

Method: whole-file read of all six files; two mechanical passes
(whitespace-normalized identical lines ≥20 chars, and longest common
word runs) run **twice** — strict, then loose (lowercased, markdown
emphasis and punctuation stripped) so re-voicing that only changes case
or `**bold**` markers still surfaces; then `grep -c -F` on both sides
for every fragment quoted as identical. Candidate/base separation came
from the diffs themselves (`git show` on 9d1b574, c147b56, 0e11081,
f330637, and `9d1b574~1` for the 57-line base), not from commit prose.

**Headline mechanical result** — this pair is verbatim-poor, unlike
testing-first: **zero** normalized identical lines between local
SKILL.md (241) and upstream SKILL.md (283), longest common run 8 words;
**zero** identical lines between `references/techniques.md` (139) and
any of the four upstream technique files, longest run 7 words. So the
case, where it exists, is structural — which is why the file splits by
section rather than taking one verdict.

**Verdicts.**

- `skills/tracing-root-causes/SKILL.md` → **PARTIAL (substantial in
  parts).** Five sections substantial, and the notice must name exactly
  these: `### 2. Isolate`, `### 5. Fix`, `## Three strikes → question
  the architecture`, `## When the cause really is environmental`,
  `## Rationalizations`. Idea-only (no notice): `## The iron law`,
  `### 1. Reproduce`, `### 3. Hypothesize`, `### 4. Disconfirm`,
  `## When the investigation stalls`, the frontmatter description.
  Strongest evidence: three byte-identical cells in the
  Rationalizations table (`Multiple fixes at once saves time`,
  `Partial understanding guarantees bugs. Read it completely.`,
  `It's probably X, let me fix that`), 6 of upstream's 8 table rows in
  1:1 correspondence, upstream's three redirection signals plus
  `Return to Phase 1` carried case-only, Three-strikes' three-item
  pattern list item-for-item with a near-verbatim closing sentence, and
  defense-in-depth's four layers by upstream's names in upstream's
  order inside phase 5.
- `skills/tracing-root-causes/references/techniques.md` →
  **SUBSTANTIAL, whole-file form** (no owner-original base; created
  whole by 5c7d82f). Three of four techniques are close structural
  derivations of three specific upstream documents — step headings,
  the four-layer taxonomy with near-verbatim purposes, the four-step
  application procedure, the five-row predicate table with three
  byte-identical predicates, three-for-three "common mistakes" and
  three-for-three justified-delay requirements. The fourth
  (test-pollution bisection) is idea-only and recorded as such.

**Notable negative findings**, recorded because they cut against a
blanket verdict: the iron law is *not* copied (`NO FIX WITHOUT A ROOT
CAUSE FIRST` vs `NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST`; the
only ≥3-word hit in that whole section is the heading plus one token);
the MAT-93 stall section has **zero** common runs at a 3-word floor
against upstream's entire SKILL.md; and phases 3-4 instruct the
*opposite* of upstream's "Form Single Hypothesis" because that text is
the owner's base.

Actions implied (consumed by steps 3-5): parts-only notice **extending**
the existing provenance comment at SKILL.md:8-14 (never a second block),
naming the five sections and keeping base + unnamed sections marked
original; whole-file notice at the top of techniques.md citing all three
upstream sources and superseding, not stacking on, its existing
"Absorbed from…" prose line; D4 gains a second skill to enumerate;
README records tracing-root-causes as derived-with-notice **in part**.

**Acceptance.**

```
$ grep -q "## D3 — classification: tracing-root-causes" \
    work/mat-94-attribution-skills/DECISIONS.md; echo $?
0

$ git diff --name-only HEAD
work/mat-94-attribution-skills/DECISIONS.md
work/mat-94-attribution-skills/PROGRESS.md

$ git diff --name-only HEAD | grep '^skills/'
# (no output) → no skills/** file touched
```

Post-commit equivalent, since `git diff --name-only HEAD` is empty once
committed (step-1 review minor 4):

```
$ git show --name-only --format= HEAD
work/mat-94-attribution-skills/DECISIONS.md
work/mat-94-attribution-skills/PROGRESS.md
```

Lane constraint re-checked (not a step-2 gate):

```
$ node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .
0 high, 0 medium, 1 low — PASS
```

The single LOW is the same pre-existing `AGENTS.md:15` cmd-drift note;
not attributable to this lane.

**Files changed.** `work/mat-94-attribution-skills/DECISIONS.md` (D3
entry appended), `work/mat-94-attribution-skills/PROGRESS.md` (state box
+ this report). No skill file edited, per the step.

**Concerns.**

1. `### 1. Reproduce` is the least comfortable call in the entry — its
   three candidate bullets do map 1:1 onto upstream's Phase 1 items in
   order, and one carries a 5-word verbatim (`What are the exact
   steps?`). It is called idea-only because the sequence is textbook
   debugging rather than distinctive selection, and decisively because
   the section's second bullet is owner-base text carried verbatim from
   the 57-line original — naming the section would attribute the
   owner's own prose upstream. The reasoning is in D3 so a reviewer can
   overturn it cheaply; adding the section to the notice is a one-word
   change to step 4 if they disagree.
2. Scope note for the record: `references/techniques.md` was added by
   **5c7d82f**, not by the three commits the PLAN's step-2 text names
   (9d1b574, c147b56, 0e11081). It is in scope regardless — the step
   scopes it by name — and `git log --diff-filter=A` confirms 5c7d82f
   is its only creating commit with no later edits, so nothing about
   the candidate set changes. Flagged so the discrepancy is not
   rediscovered as a defect downstream.
3. The SPEC's normalized notice format has a single `<upstream-path>`
   slot, but techniques.md derives from three upstream files. D3 asks
   step 4 to name all three; that is a faithful extension of the
   format, not a redesign, but it is a judgment step 4 will have to
   execute rather than copy.

### Step 2 — review verdict (fresh reviewer, opus)

Verdict text, verbatim:

> ### Spec compliance
> ✅ Compliant
>
> ### Assessment
> **Step quality:** Approved
> **Reasoning:** The classification is right where it matters — every
> byte-identical claim I sampled is true, the candidate/base split
> matches the actual git history exactly, and the five named sections
> are the correct notice scope; I uphold the `### 1. Reproduce`
> idea-only call on its merits, though its "decisive" second reason
> should be demoted. The remaining findings are precision defects in an
> evidence record (one overstated structural parallel, two off-by-one
> cites, four uncovered-candidate bookkeeping leaks) that change no
> verdict and no downstream action.

The reviewer verified the candidate/base split against git history
itself (no candidate commit missed; `5c7d82f` is techniques.md's only
commit), spot-checked eight byte-identical claims (all true, right
lines), and confirmed the techniques.md structural claims — "SUBSTANTIAL
is the right call and is not close". Implementer concern 1 (Reproduce
idea-only): **UPHELD on the merits**; concern 2 confirmed as a real
catch; concern 3 ruled no-fix-needed (naming all three upstream sources
is the faithful reading of the SPEC template). Critical: none.
Important: none.

**Deferred minors (for work-verify's triage)** — all six are precision
defects in the D3 evidence record, none verdict- or action-changing:

1. Reproduce idea-only reasoning inverted: the BASE-bullet argument
   (presented as "decisive") should be demoted to a supporting note —
   the merits argument (no distinctive selection) carries the call; a
   section with one base bullet is still nameable under the SPEC's
   parts-only format.
2. One techniques.md structural claim wrong on inspection (DECISIONS.md
   ≈628-632): the fourth instrumentation clause maps to
   root-cause-tracing.md 92-95, not the 156-161 tips block, and the
   order doesn't match — restate as "tips (156-161) plus
   analyze-traces guidance (92-95), reordered".
3. Two off-by-one upstream cites (upstream 196 cited as 197; 241 as
   240) and "Common Mistakes (85-93)" where heading is 84, first item
   86.
4. Four bookkeeping leaks under `#### Not candidates`: three MAT-46/93
   additions parked under an "excluded" heading, and `## The phases`
   (local 42-44) gets no verdict anywhere — all four plainly idea-only
   (checked by reviewer); relabel the block and add `## The phases`.
5. The phased-spine arrangement question deserves one explicit
   paragraph (generic scientific-method loop + base items 3-6 already
   supplied hypothesize/disconfirm) instead of a parenthetical.
6. `evals/` never mentioned: upstream ships pressure-test scenario
   files; local eval-04..07 are different genre (house format, no
   ported expression — reviewer checked) — one sentence in D3 excluding
   `evals/` would close the question permanently.

### Step 3 — D4: NOTICE judgment

**Implemented.** Read the D2 + D3 verdicts, judged the MIT question, and
recorded the judgment in DECISIONS.md under `## D4 — NOTICE judgment`.
**Judgment: YES** — so `NOTICE` was created at the repo root.

**The reasoning, in short.** MIT conditions two artifacts on "copies or
substantial portions", not one: the copyright notice *and* the
permission notice. D2/D3 establish four files carrying substantial
portions (both testing-first files whole-file, `techniques.md`
whole-file, `tracing-root-causes/SKILL.md` in five named sections), so
the condition is triggered. The second fact decides the rest: the
permission notice is currently **nowhere** in this repo — `LICENSE` is
the owner's own grant (© 2026 Mateo García) and cannot stand in for
Jesse Vincent's, the MAT-47 per-file comments carry no license text, and
`techniques.md`'s prose "Absorbed from…" line carries neither copyright
nor permission notice. Something had to carry it.

Three carriers were weighed in D4 and the alternatives rejected on the
record: inlining the full text into each per-file comment (~17
boilerplate lines × 4 into line-budgeted, agent-loaded skill files, four
copies free to drift, no gain over one authoritative copy) and amending
`LICENSE` to a joint notice (SPEC-fenced, and it would misstate a
third-party component as co-authorship of the whole). One root `NOTICE`
with the per-file comments pointing at it wins, which is also the shape
D1 anticipated.

The counterfactual is recorded too — had every verdict come back
idea-only, the judgment would have been **no**. The answer is contingent
on D2/D3, not automatic, and D4 says so explicitly so the reasoning is
reusable by the sibling AE lane.

**What `NOTICE` says.** Repo's own license line → `LICENSE`; an explicit
"this file is additive, it changes nothing about that license"; upstream
identification (superpowers, homepage, v6.3.0 as vendored in the
official marketplace, MIT); the four derived files enumerated against
their upstream sources — `techniques.md` naming its three sources
individually and *not* `find-polluter.sh` (idea-only per D3), and
`tracing-root-causes/SKILL.md` marked in-part with its five section
headings quoted exactly as they appear in the file; a closing "no other
file in this repository is derived from superpowers"; then the upstream
`LICENSE` in full, indented four spaces.

The reproduced license body was verified rather than eyeballed:

```
$ sed -n '/^    MIT License/,$p' NOTICE | sed 's/^    //' > /tmp/notice-mit.txt
$ diff -u --strip-trailing-cr \
    "C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/LICENSE" \
    /tmp/notice-mit.txt
# (no output) → identical to upstream modulo line endings
```

The five section headings in NOTICE were likewise taken from the file,
not from D3's prose (`grep -n "^#\{2,3\} " skills/tracing-root-causes/SKILL.md`
→ lines 59, 118, 133, 170, 185 match character for character, `→`
included).

**Acceptance.**

```
$ grep -q "## D4 — NOTICE judgment" \
    work/mat-94-attribution-skills/DECISIONS.md; echo $?
0

$ grep -q "Permission is hereby granted" NOTICE; echo $?
0
```

Lane constraint re-checked (not a step-3 gate):

```
$ node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .
0 high, 0 medium, 1 low — PASS
```

Same pre-existing `AGENTS.md:15` cmd-drift LOW as steps 1-2; not
attributable to this lane. `LICENSE` untouched (`git status` shows it
unmodified).

**Files changed.** `NOTICE` (new, repo root),
`work/mat-94-attribution-skills/DECISIONS.md` (D4 entry appended),
`work/mat-94-attribution-skills/PROGRESS.md` (state box + this report).
No `skills/**` file edited — step 4 owns those.

Post-commit file list:

```
$ git show --name-only --format= HEAD
NOTICE
work/mat-94-attribution-skills/DECISIONS.md
work/mat-94-attribution-skills/PROGRESS.md
```

**Concerns.**

1. NOTICE's file enumeration is a maintenance surface: a future port
   that lands without updating it leaves NOTICE quietly incomplete.
   Flagged in D4 as the accepted cost of the checkable form (the
   alternative — "some files are derived" — carries the license text but
   tells a reader nothing). If the owner wants it enforced, the natural
   home is an agent-lint rule in the AE repo, which this lane is fenced
   from; worth a follow-up ticket rather than a silent risk.
2. `NOTICE` is written with LF while checked-out files in the worktree
   show CRLF. This is cosmetic only: `core.autocrlf=true` and the index
   stores LF for every file (`git ls-files --eol` → `i/lf` on LICENSE,
   README.md, SKILL.md), so the committed blob is normalized like every
   other file and a fresh checkout renders it CRLF. Prior lane steps
   left DECISIONS.md the same way.
3. Step 4 note: D4 asks four per-file comments to point at `NOTICE`, one
   more file than the SPEC's scope paragraph enumerates by name
   (`techniques.md` is conditional there, "notice only if classified
   substantial" — D3 classified it substantial, so it takes one).

### Step 3 — review verdict (fresh reviewer, opus)

Verdict text, verbatim:

> ### Spec compliance
> ✅ Compliant
>
> ### Assessment
> **Step quality:** Approved
> **Reasoning:** The judgment is correctly grounded in the D2/D3
> record, the enumeration matches those verdicts exactly (including the
> `find-polluter.sh` exclusion and the in-part marking), the reproduced
> MIT text is verbatim against upstream, and `LICENSE` is untouched —
> both acceptance conditions hold. The only defects are a sentence in
> NOTICE that step 4 must make true and one unevidenced negative claim,
> neither of which undermines the step's output.

Reviewer independently re-verified: MIT text in NOTICE line-by-line
identical to upstream LICENSE (indent-only transform, documented); the
four enumerated files match D2/D3 verdicts exactly; the five quoted
section headings match the live SKILL.md character-for-character;
repo LICENSE absent from the diff. Critical: none. Important: none.

**Binding note for step 4** (from minor 1): NOTICE:13-15 claims every
derived file carries a comment pointing at NOTICE — step 4 must land
that pointer in all FOUR files (testing-first SKILL.md +
writing-good-tests.md, tracing-root-causes SKILL.md + techniques.md),
or the sentence needs softening.

**Deferred minors (for work-verify's triage):**

1. NOTICE:13-15 forward-looking sentence false until step 4 lands (see
   binding note above).
2. NOTICE:54 repo-wide negative claim ("no other file is derived")
   rests on port-record scope, not diff evidence — reviewer grepped
   `superpowers` across skills/ and found only the four enumerated
   files; add "per the MAT-94 classification record" or a D4 line
   stating the basis.
3. PROGRESS step-3 report re-narrates D4's reasoning at length —
   two copies of one argument can drift; future steps could keep
   PROGRESS to outcome + evidence.
4. ⚠️ NOTICE:21-22 marketplace repo slug (anthropics/
   claude-plugins-official) inferred from the cache directory name,
   not from a local source — descriptive prose, low stakes.

### Step 4 — apply/normalize the per-file notices

**Implemented.** Exactly per D2 + D3 verdicts and the SPEC's normalized
format, with the step-3 binding note honored (all four files now point
at `NOTICE` for the full permission text, making its NOTICE:13-15
forward-looking sentence true):

1. `skills/testing-first/SKILL.md:8-12` — the MAT-47 five-line block
   replaced in place (not stacked) with the whole-file substantial form:
   upstream path now includes the file (`test-driven-development/SKILL.md`,
   was bare `test-driven-development`), MIT + Copyright (c) 2025 Jesse
   Vincent, adaptation date, `Classified substantial (MAT-94)` pointing at
   this lane's DECISIONS.md (superseding the stale
   `work/mat-47-house-tdd/DECISIONS.md` pointer per D2 action item 1), plus
   a `NOTICE` pointer for the full permission text.
2. `skills/testing-first/references/writing-good-tests.md:6-10` — same
   treatment; the MAT-47 editorial content ("chat-partner framing is
   gone...") dropped in favor of the normalized four-fact block plus the
   `NOTICE` pointer.
3. `skills/tracing-root-causes/SKILL.md:8-14` — the existing OMC/MAT-46/
   MAT-93 provenance comment kept verbatim and **extended** (not a second
   block) with the parts-only form: the five section headings quoted
   exactly as they appear live in the file (verified with `grep -n
   "^#\{2,3\} "` — all five match character-for-character, `→` included),
   "base and remaining sections are original" stated explicitly per D3's
   instruction not to let the notice read as if the whole file were
   derived, `Classified substantial-in-part (MAT-94)`, DECISIONS.md
   pointer, `NOTICE` pointer.
4. `skills/tracing-root-causes/references/techniques.md:1-13` — no
   frontmatter, so the HTML comment block was placed right after the H1,
   before the body prose. The old "Absorbed from superpowers'
   `systematic-debugging` (v6.3.0) and distilled" clause (a bare prose
   provenance claim with no license or copyright) was removed from the
   body sentence rather than left standing beside the new comment, per D3
   action item 2 ("should not be left standing as a second, weaker
   provenance claim"); the sentence now reads "Four techniques the phased
   process reaches for by name, distilled — ...". The new comment names
   all three upstream sources individually (`root-cause-tracing.md`,
   `defense-in-depth.md`, `condition-based-waiting.md`) and omits
   `find-polluter.sh`, whose section D3 classified idea-only.

No `skills/**` file other than these four was touched. No skill content,
frontmatter, or graded behavior changed — comment blocks and one body
sentence only.

**Acceptance.**

```
$ node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .
0 high, 0 medium, 1 low — PASS
```

The single LOW is the same pre-existing `AGENTS.md:15` cmd-drift note as
steps 1-3; not attributable to this lane.

```
$ wc -l skills/testing-first/SKILL.md skills/tracing-root-causes/SKILL.md
  249 skills/testing-first/SKILL.md
  249 skills/tracing-root-causes/SKILL.md
  498 total
```

Both < 500.

```
$ for f in skills/testing-first/SKILL.md \
           skills/testing-first/references/writing-good-tests.md \
           skills/tracing-root-causes/SKILL.md \
           skills/tracing-root-causes/references/techniques.md; do
    echo "$f: $(grep -c 'Copyright (c) 2025 Jesse Vincent' "$f")"
  done
skills/testing-first/SKILL.md: 1
skills/testing-first/references/writing-good-tests.md: 1
skills/tracing-root-causes/SKILL.md: 1
skills/tracing-root-causes/references/techniques.md: 1
```

Each touched file carries exactly one occurrence — one verdict, one
block, per file, matching D2 (one verdict each for the two testing-first
files) and D3 (one verdict for SKILL.md as a whole, naming five sections
within that single block; one verdict for techniques.md as a whole). No
stacked duplicates.

```
$ grep -n "^#\{2,3\} " skills/tracing-root-causes/SKILL.md
## The iron law / ## The phases / ### 1. Reproduce / ### 2. Isolate /
### 3. Hypothesize / ### 4. Disconfirm / ### 5. Fix /
## Three strikes → question the architecture /
## When the investigation stalls /
## When the cause really is environmental / ## Rationalizations /
## Output shape / ## Where this ends / ## Judgment notes
```

Confirms the five headings quoted in the extended notice match the live
file exactly.

**Files changed.** `skills/testing-first/SKILL.md`,
`skills/testing-first/references/writing-good-tests.md`,
`skills/tracing-root-causes/SKILL.md`,
`skills/tracing-root-causes/references/techniques.md` — comment blocks
(and, in techniques.md, one body sentence) only.

**Concerns.** None on the mechanics — every acceptance number was
recomputed after the edit, not assumed. One judgment call worth a
reviewer's eye: for techniques.md I read D3 action item 2 as license to
edit the body prose (not only add a comment), since the SPEC's own
concern is "never a second block stacked" and the old sentence was an
unlicensed provenance claim in prose, not a second HTML-comment block.
If a reviewer prefers the prose left untouched with only the comment
added, that is a one-sentence revert.

Commit: `docs(attribution): normalize per-file upstream notices per
MAT-94 classification` (5e1484b).
