# DECISIONS — MAT-94

## D1 — SPEC approved as written; NOTICE carries the full license text (parent, 2026-08-19)

Asked at the design-first gate whether the SPEC (including the
lane-proposed normalized notice format) stands. Parent ruling, verbatim:

> APPROVED as written, including the normalized notice format — an HTML
> comment after frontmatter travels with the file (the point of
> per-file), and the README Provenance section provides the
> reader-visible layer, so the pair covers both needs. One nuance to
> honor while executing: if any file is classified SUBSTANTIAL and a
> NOTICE file turns out to be needed (your item 4), the NOTICE is where
> the full upstream MIT permission text lives — the per-file comment
> stays short and points there. Shape PLAN.md and proceed. Record this
> ruling in your DECISIONS.md.

Consequences for the plan: the NOTICE judgment runs BEFORE notices are
applied, so per-file comments can point at NOTICE when it exists; the
per-file comment never inlines the full MIT permission text.

## D2 — classification: testing-first

### What was diffed

Upstream ground truth, verified present this session:
`C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/`.
Version confirmed from `.claude-plugin/plugin.json` (`"version":
"6.3.0"`, `"license": "MIT"`, author Jesse Vincent) and the license from
that tree's `LICENSE` (`MIT License` / `Copyright (c) 2025 Jesse
Vincent`).

| Local (this repo) | Upstream (v6.3.0) |
|---|---|
| `skills/testing-first/SKILL.md` (249 lines) | `skills/test-driven-development/SKILL.md` (320 lines) |
| `skills/testing-first/references/writing-good-tests.md` (220 lines) | `skills/test-driven-development/writing-good-tests.md` (198 lines) |

Method: whole-file read of all four; a mechanical pass for
whitespace-normalized identical lines (≥20 chars) and for the longest
common word runs; then targeted `diff -u` over the sections the two
passes flagged. Quotes below are verbatim from the files, with local
and upstream line numbers.

### File 1 — `skills/testing-first/SKILL.md`

**Verbatim runs.** Nine whitespace-normalized identical lines. The
longest single run is 40 words — the RED example, copied whole
(local 52-66 / upstream 76-90):

```typescript
test('retries failed operations 3 times', async () => {
  let attempts = 0;
  const operation = () => {
    attempts++;
    if (attempts < 3) throw new Error('fail');
    return 'success';
  };

  const result = await retryOperation(operation);

  expect(result).toBe('success');
  expect(attempts).toBe(3);
});
```

The Iron Law is byte-identical (local 25 / upstream 34):

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

So are both bad-test literals in the Good Tests table —
`test('validates email and domain and whitespace')` (local 156 /
upstream 202) and `test('test1')` (local 157 / upstream 203) — and the
table headers `| Quality | Good | Bad |` and `| Excuse | Reality |`.

One prose run survives the rewrite inside the rationalization table
(local 195 / upstream 217), 19 words verbatim:

> which proves nothing. They may test the wrong thing, test the
> implementation instead of the behavior, or miss the

followed shortly by a second, 12 words verbatim:

> You never watched it fail, so you never proved it can catch

**Structural parallels.** This is the heavier evidence: the selection,
sequence, and worked examples are upstream's, re-voiced.

- *The cycle* (local 45-101 / upstream 47-196) is upstream's spine in
  order — RED, Verify RED, GREEN, Verify GREEN, REFACTOR — with both
  verify beats marked mandatory in both files ("**MANDATORY. Never
  skip.**" upstream 115 → "**Mandatory. This is the beat everyone
  skips.**" local 74) and the same three confirmations under each
  ("Test fails (not errors) / Failure message is expected / Fails
  because feature missing (not typos)" upstream 121-124 → "it *fails*
  rather than errors, the failure message is the one you expected, and
  it fails because the behavior is missing" local 76-78), and the same
  two follow-ups ("Test passes? … Fix test. / Test errors? Fix error,
  re-run" upstream 126-128 → local 81-82).
- *Common rationalizations* (local 190-207 / upstream 212-226): 10 of
  upstream's 11 rows have a 1:1 counterpart in the same order — "Too
  simple", "I'll test after", "spirit not ritual", "already manually
  tested", "deleting X hours", "keep as reference", "need to explore",
  "hard to test", "TDD will slow me down", "existing code has no
  tests". Only "Manual test faster" is dropped; four house rows
  (work-verify, PLAN acceptance, skill/doc, PROGRESS evidence) are
  added. Row order and the excuse/reality shape are upstream's.
- *Red flags* (local 209-220 / upstream 228-244): 12 of upstream's 13
  items, in upstream's order, plus two house additions — down to the
  closing move ("**All of these mean: Delete code. Start over with
  TDD.**" → "Every one of these means the same thing: delete the code,
  start from the test.").
- *Verification checklist* (local 172-182 / upstream 283-296): 7 of
  upstream's 8 boxes in order (two merged into one), plus one house
  box, plus the same failure clause ("Can't check all boxes? You
  skipped TDD. Start over." → "A box you cannot tick means you skipped
  TDD").
- *When stuck* (local 222-229 / upstream 298-305): the same four
  problems in the same order with the same four answers (wished-for
  API; simplify the interface; dependency injection; extract helpers).
- *Bugfix example* (local 121-150 / upstream 246-281): the same
  worked example — empty email accepted, `'Email required'`, RED then
  GREEN — with near-identical console output ("FAIL: expected 'Email
  required', got undefined" → "FAIL  expected 'Email required',
  received undefined").
- *Final rule* (local 244-247 / upstream 315-318): the same two-line
  code block, `→` rewritten as `->`.
- *Iron Law delete clause* (local 28-32 / upstream 37-45): upstream's
  four "no exceptions" bullets compressed to prose, keeping the
  distinctive "Delete means delete" verbatim.

**What is genuinely local.** The `## Evidence goes in the lane` and
`## Handing off` sections (PROGRESS.md beats, the work-verify handoff),
the eval-is-the-failing-test framing, and the four house rationalization
rows are owner-original; upstream's dot-graph, its `<Good>/<Bad>` tag
convention, its GREEN code samples, and its "When to Use" and "Repeat"
sections were dropped. That delta is real adaptation, but it does not
displace what was kept.

**Verdict: SUBSTANTIAL.** Copyright protects expression, and the
expression carried over here is not only the handful of verbatim runs
but the selection and arrangement — which rationalizations to answer,
in what order, with which worked example — which is exactly what a
compilation's expression consists of. A reader who knows upstream reads
this file as upstream, rewritten.

### File 2 — `skills/testing-first/references/writing-good-tests.md`

Much stronger than File 1: 33 whitespace-normalized identical lines and
a 111-word contiguous verbatim run.

**The two principles** are byte-identical (local 26-29 / upstream
11-14):

```
1. Every test names the break it catches
2. Every test exercises the real thing
```

**Both gate blocks** are near byte-identical. Gate 1 (local 86-98 /
upstream 67-79) differs only in `→`→`->`, "is derived"→"was derived",
"the code's logic"→"that code's logic", and one added article:

```
BEFORE writing the test body:
  Name the production change that would make this test fail.

  Cannot name one            -> redesign around an observable behavior
  "The source text changed"  -> run the artifact, assert its effects
  Only intentional decisions -> change detector; test the behavior
                                that depends on the decision
```

Gate 2 (local 155-166 / upstream 137-148) is identical across all ten
content lines except the header ("BEFORE adding a mock or test helper:"
→ "BEFORE adding a mock or a test-only helper:") and one comma.

**Every code example is verbatim** — the mirror-assertion pair
(`buildSearchQuery({ tag: 'urgent' })`, local 50-54 / upstream 34-38),
the navigation/sidebar-mock pair (local 109-112 / upstream 90-93), and
the `vi.mock('ToolCatalog', …)` / `vi.mock('MCPServerManager')` pair
(local 124-129 / upstream 106-111). Only the `❌`/`✅` markers were
converted to prose comments.

**Quick reference table** (local 190-201 / upstream 173-184): all ten
rows present in upstream's order; eight are byte-identical, the other
two differ by one punctuation mark and one article. The 111-word
longest common run is this table's tail.

**Warning signs** (local 203-221 / upstream 186-198): 11 items, 11
counterparts, same order. Several differ only by a terminal period —
e.g. "Expected values are hidden behind loops, builders, or helpers"
and "The test would still matter if only the framework remained" are
otherwise byte-identical.

**Bold lead-ins carried verbatim**: "**Your code, not the framework.**
Test the contract your code makes at", "**Mock at the right level.**
Learn every side effect of the real method", "**Make doubles specific.**
When arguments, call counts, or ordering are", "**Production classes
carry production methods only.** Cleanup that only tests need lives in
test utilities, never as a `destroy()` on the" — each an identical
opening line of an identically-placed paragraph.

**What is genuinely local.** The chat-partner framing is gone
("**your human partner's correction:** …" upstream 96 → "The question
to ask yourself: …" local 115); document testing points at this repo's
`evals/` convention instead of `superpowers:writing-skills`; mutation
findings are routed to the lane's PROGRESS.md; a Contents list was
added and upstream's "Tests Ship With the Implementation" section was
folded into the mutation check. Editorial, not transformative.

**Verdict: SUBSTANTIAL** — the strongest of the two by a wide margin.

### Actions these verdicts imply

1. Both files take a per-file upstream notice in the SPEC's normalized
   **whole-file** form (not the parts-only form): upstream file +
   v6.3.0, MIT + Copyright (c) 2025 Jesse Vincent, adaptation date,
   "Classified substantial (MAT-94)" with the pointer to this file.
   Step 4 normalizes the two MAT-47 notices already present
   (`SKILL.md:8-12`, `references/writing-good-tests.md:6-10`) **in
   place** — one block per file, never a second block stacked on the
   old one, and the MAT-47 `work/mat-47-house-tdd/DECISIONS.md`
   pointer is superseded by this lane's.
2. At least one file is substantial, so D4's NOTICE judgment (step 3)
   is live rather than moot; if NOTICE is created, both comments point
   there for the full permission text per D1.
3. README `## Provenance` (step 5) records testing-first as
   **derived-with-notice**, covering both files, pointing at
   `work/mat-94-attribution-skills/DECISIONS.md`.
4. No `skills/**` file is edited in this step — classification only.

## D3 — classification: tracing-root-causes

> Line-number note (added at work-verify, per the fresh-context
> review's Important finding): the LOCAL line numbers below are as of
> the pre-step-4 files. Step 4 added 8 comment lines to
> `skills/tracing-root-causes/SKILL.md` and 6 to
> `references/techniques.md` — add those offsets to resolve a local
> cite against the live tree. Upstream cites are unaffected.

### What was diffed

Same upstream tree as D2, re-verified this step:
`C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/`
— `.claude-plugin/plugin.json` gives `"version": "6.3.0"`, `"license":
"MIT"`, author Jesse Vincent; that tree's `LICENSE` gives `MIT License`
/ `Copyright (c) 2025 Jesse Vincent`.

| Local (this repo) | Upstream (v6.3.0) |
|---|---|
| `skills/tracing-root-causes/SKILL.md` (241 lines) | `skills/systematic-debugging/SKILL.md` (283 lines) |
| `skills/tracing-root-causes/references/techniques.md` (139 lines) | `root-cause-tracing.md` (169), `defense-in-depth.md` (122), `condition-based-waiting.md` (115), `find-polluter.sh` (72) |

**Candidate set (what this entry judges).** Only the MAT-46 absorption
and the MAT-93 leg. The 57-line owner-original base — `git show
9d1b574~1:skills/tracing-root-causes/SKILL.md`, the
Context-Engineering/OMC salvage — is **not** a candidate, and text that
survives from it is marked BASE below and excluded from every verdict.
Candidates were established from the diffs themselves, not from the
commit messages: `git show 9d1b574`, `c147b56`, `0e11081`, `f330637`
(the PR #12 ask-for-help leg). One scope note: `references/techniques.md`
was added by **5c7d82f** ("docs: techniques reference for the absorbed
debugging skill"), one commit before 9d1b574, not by the three commits
the PLAN names; `git log --diff-filter=A` confirms 5c7d82f is its only
creating commit and no commit has touched it since. It is in scope
either way — the PLAN scopes it by name ("`references/techniques.md`'s
four techniques") — and it has no owner-original ancestor.

**Method.** Whole-file read of all six files; then two mechanical
passes — whitespace-normalized identical lines (≥20 chars) and longest
common word runs — run twice, once strict and once *loose*
(lowercased, markdown emphasis and punctuation stripped) so that
re-voicing that changes only case or `**bold**` markers still surfaces;
then `grep -c -F` on every fragment quoted below, against both sides, so
no "byte-identical" claim here rests on eyeballing. Line numbers are
local / upstream.

### The headline mechanical result

Unlike testing-first, this pair is **verbatim-poor**:

- `skills/tracing-root-causes/SKILL.md` vs upstream `SKILL.md`:
  **zero** whitespace-normalized identical lines (≥20 chars). The
  longest common run in the whole file is **8 words**, loose-normalized
  (local 53 / upstream 59-60).
- `references/techniques.md` vs each of the four upstream technique
  files: **zero** identical lines, and the longest common run against
  any of them is **7 words** (local 17 / `root-cause-tracing.md` 5).

For contrast, D2 found 9 and 33 identical lines and 40- and 111-word
runs on the two testing-first files. So the case here, where it exists,
is a **structure** case, not a copying case — which is exactly the
distinction the ruling asks for, and it is why this file splits by
section instead of taking one verdict.

### SKILL.md, section by section

#### `## The iron law` (local 21-40) — **IDEA-ONLY**

The law itself is *not* copied. Local 23-25 against upstream 16-18:

```
NO FIX WITHOUT A ROOT CAUSE FIRST          (local)
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST   (upstream)
```

Loose-matching the whole 20-line section against upstream's entire
SKILL.md at a 3-word floor returns exactly one hit — `The iron law NO`
(local 21-24 / upstream 14-17), i.e. the heading plus the first token of
the fenced block. Everything under it is original: "A fix proposed
before the cause is known is a guess wearing a diff", and the two
carve-outs (mitigation under a live incident; doing what the owner
asked, disagreement stated once) have no upstream counterpart at all —
upstream's entire body is one line, "If you haven't completed Phase 1,
you cannot propose fixes."

What *is* carried is a presentational convention — an all-caps
imperative in a fenced block under an "Iron Law" heading — which
superpowers uses across its skills and which is a title-and-format
device, not protectable expression. Idea-only.

#### `### 1. Reproduce` (local 46-57) — **IDEA-ONLY** (closest such call)

Upstream's Phase 1 items 1-3 map onto three of the four bullets, in
order: read errors (upstream 52-56) → local 48-49; reproduce
consistently (58-62) → local 53-55; check recent changes (64-68) →
local 56-57. Sub-items track too — "Note line numbers, file paths,
error codes" → "full message, full stack, line numbers, codes"; "They
often contain the exact solution" → "It frequently names the answer".
One fragment is verbatim, `What are the exact steps?` (local 53 /
upstream 60, `grep -c -F` → 1 and 1), and the loose pass reads 8
consecutive words across it because "Can you trigger it reliably?"
became "**Trigger it reliably.**".

Called idea-only for two reasons, and it is the least comfortable call
in this entry. First, "read the error, reproduce it, check what
changed" is the textbook opening of every debugging guide; unlike a
rationalization table there is no distinctive *selection* being
appropriated, and a five-word question is not protectable on its own.
Second and decisively: the section's second bullet is BASE text,
carried verbatim from the 57-line original (base 17-19 → local 49-52,
"Restate exactly what was observed — which artifact, what behavior,
when. If you catch yourself rewriting the observation to fit a
theory, stop."). Naming this section in a notice would attribute the
owner's own prose to Jesse Vincent, which the ruling forbids more
firmly than it demands breadth.

(Note for a reviewer re-running the mechanical pass: it flags `If you
catch yourself` at local 51 / upstream 216. That is a false positive —
local 51 is BASE text that predates the absorption by two commits, and
upstream 216 is the lead-in to Red Flags. It is not evidence of
anything.)

#### `### 2. Isolate` (local 59-76) — **SUBSTANTIAL**

No BASE text; all four bullets are MAT-46/0e11081, and all four have an
upstream parent.

Upstream's *chosen illustration* for a multi-component system is
carried near-whole (local 63-64 / upstream 72) — `API → service →
database` is byte-identical (`grep -c -F` → 1 and 1), and the first
pair differs by one suffix:

> **WHEN system has multiple components (CI → build → signing, API →
> service → database):**            *(upstream)*

> In a multi-component system (CI → build → sign, API → service →
> database), log what enters and what leaves each boundary, run once,
> and read which hop breaks.        *(local)*

The procedure under it is upstream's too: "Log what data enters
component / Log what data exits component" → "log what enters and what
leaves each boundary"; "Run once to gather evidence showing WHERE it
breaks" → "run once, and read which hop breaks" (upstream 76-83).

Upstream's Phase 2 "Pattern Analysis" (120-142) is then compressed
four-items-into-two, keeping the distinctive bits verbatim: `list every
difference, however small` (local 68 / upstream 135 — identical but for
upstream's sentence-initial capital) and upstream's own quoted idiom
`"that can't matter"` (upstream 136 `Don't assume "that can't matter"`
→ local 68-69 `"That can't matter" is a hypothesis, not an
observation`). The dependency bullet (local 70-72) is a three-for-three
compression of upstream 138-141: "What other components does this need?
/ What settings, config, environment? / What assumptions does it make?"
→ "the services, config, environment, and assumptions the broken path
needs to work at all". The fourth bullet reproduces upstream's own
reference-pointer move (upstream 112, "See `root-cause-tracing.md` in
this directory" → local 73-76's link to `references/techniques.md`).

Verdict: substantial. The section is upstream's evidence-gathering
sequence with upstream's illustration attached.

#### `### 3. Hypothesize` (local 78-95) — **IDEA-ONLY**

Mostly not a candidate: bullets 1-2 ("Compete the explanations", "Rank
evidence by strength") are BASE, carried near-verbatim from base 20-29.
The only candidate is bullet 3, added by 0e11081, against upstream's
Phase 3.4 (162-166): "Say 'I don't understand X' / Don't pretend to
know / … / Research more" → "'I don't know how X works yet' is a
finding, not a confession — say it, then go read X completely". Idea
carried, no expression: zero common runs at a 4-word floor across the
bullet.

Worth recording as evidence *against* reading the phased spine as
upstream's: upstream's Phase 3.1 is **"Form Single Hypothesis"**
(147-150, "State clearly: 'I think X is the root cause because Y'"),
and this section instructs the opposite — "Hold ≥2 from deliberately
different frames" — because that instruction is the owner's base. The
two documents disagree at the core of hypothesis formation.

#### `### 4. Disconfirm` (local 97-116) — **IDEA-ONLY**

Bullets 1, 2 and 4 are BASE (base 30-41), and bullet 4's cost clause is
c147b56, original. The one candidate is bullet 3, "**Test minimally.**
One variable, the smallest change that discriminates" (local 107-109)
against upstream's Phase 3.2 (152-155): "Test Minimally / Make the
SMALLEST possible change to test hypothesis / One variable at a time".
The two-word heading is upstream's; the rest is re-voiced, and the
closing sentence ("A bigger timeout after a failed timeout is not a new
hypothesis — it is the same one, retried louder") is original. A
two-word section title carries no notice.

#### `### 5. Fix` (local 118-131) — **SUBSTANTIAL**

No BASE text. Bullet for bullet against upstream's Phase 4 (168-190),
in upstream's order:

| local | upstream |
|---|---|
| "Simplest case that fails; a test if there's a framework, a script if there isn't" (120-122) | "Simplest possible reproduction / Automated test if possible / One-off test script if no framework" (173-175) |
| "**Fix at the source**, not where the error surfaced. One change, one concern — no bundled refactor, no \"while I'm here\"." (123-124) | "Address the root cause identified / ONE change at a time / No \"while I'm here\" improvements / No bundled refactoring" (181-184) |
| "Confirm the reproduction now passes and nothing else broke" (130-131) | "Test passes now? / No other tests broken?" (186-187) |

`while I'm here` is byte-identical (`grep -c -F` → 1 and 1) — upstream's
own scare-quoted idiom, kept inside quotes.

The third bullet (125-129) is heavier still: "entry, business logic,
environment guard, instrumentation" is `defense-in-depth.md`'s **four
layers, by upstream's names, in upstream's order** (Layer 1 Entry Point
Validation → Layer 2 Business Logic Validation → Layer 3 Environment
Guards → Layer 4 Debug Instrumentation, upstream 22-72). A named,
ordered four-part taxonomy is the kind of selection-and-arrangement the
ruling calls expression.

Only the hand-off clause ("then stop; see *Where this ends*") is house.
Verdict: substantial.

#### `## Three strikes → question the architecture` (local 133-144) — **SUBSTANTIAL**

The closest carry in the file. Upstream 198-212 against local 133-144:

Its three-item pattern list is reproduced item-for-item, in order:

> - Each fix reveals new shared state/coupling/problem in different place
> - Fixes require "massive refactoring" to implement
> - Each fix creates new symptoms elsewhere      *(upstream 201-203)*

> - Each fix reveals a new problem somewhere else.
> - Each fix needs "just a bit of refactoring" to land.
> - The same symptom keeps returning in a new costume.   *(local 138-140)*

And the closing sentence is upstream's, re-punctuated with a clause
appended:

> This is NOT a failed hypothesis - this is a wrong architecture.
> *(upstream 212)*

> This is not a failed hypothesis — it is a wrong architecture, and
> more fixes make it worse.       *(local 143-144)*

Loose-matched, that is `This is not a failed hypothesis` (6 words) plus
`is a wrong architecture` (4) — ten of upstream's twelve words, in
order, differing in case and one em-dash. The threshold (three), the
"before attempt four" rule (upstream 197, "DON'T attempt Fix #4 without
architectural discussion"), and the escalation ("Discuss with your human
partner before attempting more fixes" → "get a decision before attempt
four") are all upstream's. What was dropped is upstream's
question-fundamentals trio (206-208). Substantial.

#### `## When the investigation stalls` (local 146-168) — **IDEA-ONLY**

The MAT-93 leg (f330637), and the cleanest verdict in the entry.
Loose-matching this section against upstream's *entire* SKILL.md at a
**3-word** floor returns **zero** hits — not one three-word sequence in
23 lines.

Its upstream prompt is two words: `- Ask for help`, the third bullet of
Phase 3.4 (upstream 165), which MAT-46 D8 held back and MAT-93 judged
in. Everything the section says — earn the ask (an affordable
discriminating probe means run it), name the recipient, use a channel
that waits, carry the evidence — answers questions upstream never
raises, in a world (dispatched agent seats, blocking asks, lanes) that
upstream does not describe. A two-word bullet is a prompt, not
expression. Idea-only; no notice.

#### `## When the cause really is environmental` (local 170-183) — **SUBSTANTIAL**

Added by 0e11081; no BASE text. Upstream's "When Process Reveals 'No
Root Cause'" (266-275) is a ten-line section, and its numbered list
survives three-for-three in order:

| local | upstream |
|---|---|
| "**Record what was ruled out**, and how." (176-177) | "2. Document what you investigated" (271) |
| "**Handle it deliberately** — a bounded retry, a timeout, a clear error message." (178-181) | "3. Implement appropriate handling (retry, timeout, error message)" (272) |
| "**Add the monitoring** that would make the next occurrence legible" (182-183) | "4. Add monitoring/logging for future investigation" (273) |

The illustrative triple in item 2 is upstream's exact three examples in
upstream's exact order. The opening qualifier tracks as well:
"environmental, timing-dependent, or external" (upstream 268) →
"timing, an external dependency, or a platform difference" (local
171-172). Upstream's closing disbelief was not dropped, only relocated
— it is row 14 of the Rationalizations table (see below). Short, but
substantial for its length: a three-item ordered procedure plus its
illustrations is the whole of what upstream's section is.

#### `## Rationalizations` (local 185-205) — **SUBSTANTIAL**

The merged table, and the strongest verbatim evidence in SKILL.md.
Three fragments are byte-identical, each confirmed `grep -c -F` → 1 and
1 on both sides:

| fragment | local | upstream |
|---|---|---|
| `Multiple fixes at once saves time` (whole Excuse cell) | 200 | 252 |
| `Partial understanding guarantees bugs. Read it completely.` (whole Reality cell) | 201 | 253 |
| `It's probably X, let me fix that` (whole Excuse cell) | 193 | 221 (Red Flags) |

Six of upstream's eight `## Common Rationalizations` rows (244-255) have
a 1:1 local counterpart, several near-verbatim in *both* cells:

> \| "Emergency, no time for process" \| Systematic debugging is FASTER
> than guess-and-check thrashing. \|      *(upstream 249)*

> \| "Emergency — no time for process" \| Systematic is FASTER than
> guess-and-check thrashing. The process is the short path out, not a
> tax on the outage. \|                   *(local 189)*

— the Excuse differs by one punctuation mark, the Reality's first
sentence by one dropped word. Likewise `First fix sets the pattern`
(upstream 250) → `The first fix sets the pattern` (local 191);
`"One more fix attempt" (after 2+ failures)` (upstream 255) → `"One more
attempt" (after 2+)` (local 197); `Seeing symptoms ≠ understanding root
cause.` (upstream 254) → `Seeing a symptom is not understanding a
cause.` (local 193); `95% of "no root cause" cases are incomplete
investigation` (upstream 275) → `95% of "no root cause" is incomplete
investigation` (local 202). Upstream's Red Flags supplied two more
Excuse cells (`"Just try changing X and see if it works"` 218 → local
192; `"It's probably X, let me fix that"` 221 → local 193).

The section's closing line (local 204-205) compresses upstream's whole
`## your human partner's Signals You're Doing It Wrong` section
(233-242): three of its five quoted signals are carried, each differing
only in the sentence-initial capital that inlining removed — `Is that
not happening?` / `Stop guessing` / `We're stuck?` (upstream 237, 239,
240) → `"is that not happening?"`, `"stop guessing"`, `"we're stuck?"`
— followed by upstream's own instruction, `Return to Phase 1.` (242) →
`return to phase 1.`

Five of the fourteen local rows are genuinely not upstream's: "This same
fix worked last week elsewhere", "The senior engineer is sure", "Four
hours in — I can't waste them", "Bigger timeout, then ship it", and "It
broke right after the deploy" — the last being BASE (base 52, "Temporal
proximity ('it broke right after X') is bottom-tier evidence"). Upstream
row 4 ("I'll write test after confirming fix works") was dropped. That
delta is real, but a compilation whose expression is *which excuses to
answer* keeps six of its eight sources, in a recognizable arrangement,
with three cells copied character for character. Substantial.

#### Frontmatter `description` (local 3) — **IDEA-ONLY**

Rewritten by 9d1b574. Loose-matches upstream's description (line 3) for
six words — `failure, or unexpected behavior, BEFORE proposing` — and
the trigger list overlaps upstream's "When to Use" block (24-30:
performance problems, build failures, unexpected behavior). It is
functional discovery metadata, the overlapping span is a generic phrase,
and the local description carries content upstream has none of (crash,
regression, flaky/intermittent, the explain-without-fixing clause, the
supersession clause). No notice.

#### Not candidates (excluded from every verdict above)

Opening paragraph (16-19, BASE, extended), `## Output shape` (207-217,
BASE + one MAT-93 coherence line), `## Where this ends` (219-229, house
AE integration — no upstream counterpart of any kind), `## Judgment
notes` (231-241, BASE reshaped; its third bullet is the supersession
statement, which is about upstream rather than from it).

#### SKILL.md verdict: **PARTIAL — substantial in parts**

The SPEC's parts-only form applies: the file has an owner-original base
and a majority of its candidate sections are idea-only. **The notice
must name exactly these five sections**, and no others:

1. `### 2. Isolate`
2. `### 5. Fix`
3. `## Three strikes → question the architecture`
4. `## When the cause really is environmental`
5. `## Rationalizations`

Named as they appear in the file, so a reader can check the claim
against the text. The upstream file to cite is
`skills/systematic-debugging/SKILL.md`; sections 2 and 5 also draw on
`defense-in-depth.md` (the four layers) and, via section 5's pointer,
`condition-based-waiting.md` — but those two are cited in full by
`references/techniques.md`'s own notice, so SKILL.md's block need not
enumerate them.

### `references/techniques.md` — **SUBSTANTIAL** (whole file)

No owner-original base: created whole by 5c7d82f, untouched since. Its
own opening already says what it is — "Absorbed from superpowers'
`systematic-debugging` (v6.3.0) and distilled" (local 3-4) — a
provenance claim in prose, with no license or copyright, which is
precisely the gap this lane closes.

Verbatim is thin (longest run 7 loose words: "The instinct is to fix
where the error appeared", local 17, against "Your instinct is to fix
where the error appears, but that's treating a symptom",
`root-cause-tracing.md` 5). The case is structural, and it is dense.

**Backward tracing (local 13-57) ← `root-cause-tracing.md`.** The four
numbered steps are upstream's five, in order, with 4 and 5 merged, under
near-identical headings: "1. Observe the Symptom / 2. Find Immediate
Cause / 3. Ask: What Called This? / 4. Keep Tracing Up / 5. Find
Original Trigger" (upstream 34-59) → "1. Observe the symptom / 2. Find
the immediate cause / 3. Ask what called it / 4. Stop at the origin"
(local 21-27). The opening keeps upstream's *three chosen
illustrations*, reordered: "git init in wrong directory, file created in
wrong location, database opened with wrong path" (upstream 5) → "a file
written to the wrong place, a database opened on the wrong path, a
command run in the wrong directory" (local 15-17). The instrumentation
sub-section reproduces upstream's four stack-trace tips (156-161) —
log **before** the operation not after it fails; use a channel the
runner shows rather than the logger; include value, cwd and env; read
the traces for the repeating frame — as four clauses in the same order
(local 41-57).

**Defense in depth (local 59-83) ← `defense-in-depth.md`.** Upstream's
four layers are carried as a four-row table with upstream's names,
upstream's order, and upstream's purposes near-verbatim: "Reject
obviously invalid input at API boundary" (upstream 23) → "reject invalid
input at the API boundary"; "Ensure data makes sense for this operation"
(41) → "the value must make sense for THIS operation" (identical but for
the caps, `grep -c -F` → 1 local / 1 upstream on `sense for this
operation` case-folded); "Prevent dangerous operations in specific
contexts" (53) → "refuse dangerous operations in dangerous contexts";
"Capture context for forensics" (73) → "capture context when the others
fail". Upstream's four-step application procedure (89-94) survives
four-for-four in one sentence (local 76-78), the last step
near-verbatim: "Try to bypass layer 1, verify layer 2 catches it" → "try
to bypass layer 1 and confirm layer 2 catches it" (`try to bypass layer
1` and `layer 2 catches it` both `grep -c -F` → 1 and 1, case aside).
The opening triple carries too: "bypassed by different code paths,
refactoring, or mocks" (upstream 5) → "bypassed by a different code
path, removed in a refactor, or mocked away in a test" (local 62-63).

**Condition-based waiting (local 85-118) ← `condition-based-waiting.md`.**
The predicate table is upstream's five rows in upstream's order (event,
state, count, file, compound — upstream 52-56 / local 107-111), and
three of the five predicates are byte-identical, each `grep -c -F` → 1
and 1: `machine.state === 'ready'`, `items.length >= 5`, `obj.ready &&
obj.value > 10` (a fourth, `e.type === 'DONE'`, is identical inside a
changed call). The before/after block keeps upstream's ❌/✅ convention,
its 50 ms, its `waitFor`, and its `expect(result).toBeDefined()`
assertion (upstream 36-46 / local 93-96). Upstream's three "Common
Mistakes" (85-93) become local's "Three details matter", three-for-three
in order: poll every 10 ms, always carry a timeout with a message,
re-read inside the loop rather than caching. Upstream's three
requirements for a justified fixed delay (104-107) survive
three-for-three in order as well (local 113-118).

**Test-pollution bisection (local 120-139) ← `find-polluter.sh`** —
**idea-only within a substantial file.** What is carried is the
algorithm (confirm clean, run one file, check, repeat); none of the
script's expression is — not its bash, its usage text, its emoji output,
or its `npm test` binding — and local 136-139 says so explicitly
("deliberately not shipped as a script here"). Recorded so the notice's
scope is legible; it does not change the file's verdict.

Verdict: **substantial, whole-file form** (the SPEC reserves the
parts-only form for files with an owner-original base; this file has
none). Three of its four sections are close structural derivations of
three specific upstream documents, and the file exists to carry them.

### Actions these verdicts imply

1. **`skills/tracing-root-causes/SKILL.md`** — parts-only notice, named
   sections exactly as listed above (Isolate, Fix, Three strikes, When
   the cause really is environmental, Rationalizations), citing
   `skills/systematic-debugging/SKILL.md` v6.3.0, MIT, Copyright (c)
   2025 Jesse Vincent. Per the SPEC, step 4 **extends the existing
   provenance HTML comment** (local 8-14) rather than adding a second
   block; the OMC/Context-Engineering base attribution and the MAT-46 /
   MAT-93 lines stay. The wording must keep the base and the unnamed
   sections marked original — this file is majority owner-original and
   the notice may not read as if it were not.
2. **`skills/tracing-root-causes/references/techniques.md`** — notice in
   the whole-file form. It has no frontmatter, so the block goes at the
   top of the file, before or merged with the existing "Absorbed
   from…" line (which should not be left standing as a second,
   weaker provenance claim). It should name all three upstream sources
   it derives from — `root-cause-tracing.md`, `defense-in-depth.md`,
   `condition-based-waiting.md` — rather than only the parent skill;
   `find-polluter.sh` is not cited, its section being idea-only.
3. **D4 (step 3)** was already live from D2; D3 adds a second skill to
   the enumeration a NOTICE file would carry, if the judgment lands yes.
4. **README `## Provenance` (step 5)** records tracing-root-causes as
   **derived-with-notice, in part** — the honest phrasing here differs
   from testing-first's, because the base is the owner's and the file is
   majority original. One line, pointing at this entry.
5. No `skills/**` file is edited in this step — classification only.

## D4 — NOTICE judgment

**Judgment: YES.** A root `NOTICE` is needed, and it was created in this
step (`NOTICE`, repo root). `LICENSE` is byte-untouched.

### The question

MIT grants the rights "subject to the following conditions":

> The above copyright notice and this permission notice shall be
> included in all copies or substantial portions of the Software.

Two artifacts are conditioned, not one — the **copyright notice**
(`Copyright (c) 2025 Jesse Vincent`) and the **permission notice** (the
`Permission is hereby granted…` grant plus the all-caps warranty
disclaimer that MIT's own text treats as part of it). The grant this
repository exercises is `modify, merge, … distribute`, so the condition
travels into the derivative, not only into a verbatim copy. Two facts
therefore decide the judgment: does this repo carry a *substantial
portion*, and is the *permission notice* currently present anywhere in
it.

### Fact 1 — substantial portions are present (D2 + D3)

Four files, established by diff evidence rather than assumption:

| File | Verdict | Entry |
|---|---|---|
| `skills/testing-first/SKILL.md` | substantial, whole file | D2 |
| `skills/testing-first/references/writing-good-tests.md` | substantial, whole file (33 identical lines, 111-word run) | D2 |
| `skills/tracing-root-causes/references/techniques.md` | substantial, whole file | D3 |
| `skills/tracing-root-causes/SKILL.md` | substantial in five named sections | D3 |

The condition is triggered. This is contingent, not automatic: had every
verdict come back idea-only, the answer here would be **no** — MIT
conditions the copying of the Software, and an idea-only rewrite carries
none of it. The counterfactual is recorded so the reasoning is reusable,
not only the outcome.

### Fact 2 — the permission notice is currently absent

`LICENSE` (repo root) carries this repository's own MIT grant,
`Copyright (c) 2026 Mateo García (bygama)` — it is not Jesse Vincent's
notice and cannot stand in for it. The MAT-47 per-file comments name
upstream but carry no license text at all, and
`references/techniques.md` opens with a bare prose provenance line
("Absorbed from superpowers' `systematic-debugging` (v6.3.0) and
distilled") with neither copyright nor permission notice. So the
upstream copyright notice is partially present and the permission notice
is nowhere in the repository. Something must carry it.

### Where it goes — three options weighed

1. **Inline the full permission text in each per-file comment.**
   Rejected. It satisfies the condition, but costs ~17 boilerplate lines
   × 4 files inside skill files that are line-budgeted (<500) and read in
   full by an agent at load time, and it creates four copies free to
   drift apart. No gain over one authoritative copy every file points
   at.
2. **Amend `LICENSE` to a dual/joint notice.** Rejected, and fenced by
   the SPEC besides. It would misstate the facts: this repository is
   MIT © 2026 Mateo García, and upstream material is a *third-party
   component* inside it, not a co-author of the whole.
3. **One root `NOTICE` with the full upstream license text, enumerating
   the derived files; per-file comments stay short and point there.**
   Chosen. It is the conventional carrier for third-party notices, keeps
   exactly one authoritative copy of the text, and puts the enumeration
   where a distributor or a reader looks for it. D1 already ruled this
   is where the full permission text lives *if* a NOTICE turned out to
   be needed; this step's job was to decide whether it is, and it is.

### What NOTICE contains

This repository's own license line pointing at `LICENSE`; a statement
that the file is additive and changes nothing about that license;
upstream identification (project, homepage, v6.3.0 as vendored in the
official plugin marketplace, MIT); the enumeration of the four derived
files against their upstream sources — with `techniques.md`'s three
sources named individually (`root-cause-tracing.md`,
`defense-in-depth.md`, `condition-based-waiting.md`;
`find-polluter.sh` is **not** cited, its section being idea-only per D3)
and `tracing-root-causes/SKILL.md` marked in-part, its five section
headings quoted exactly as they appear in the file so the claim is
checkable; a closing "no other file in this repository is derived from
superpowers"; and finally the upstream `LICENSE` reproduced in full,
indented four spaces and otherwise byte-identical.

Verified rather than asserted:

```
$ sed -n '/^    MIT License/,$p' NOTICE | sed 's/^    //' > /tmp/notice-mit.txt
$ diff -u --strip-trailing-cr \
    ".../superpowers/6.3.0/LICENSE" /tmp/notice-mit.txt
# (no output) → identical modulo line endings
```

The enumeration is the part that will rot if a later port lands without
updating it. That is the accepted cost of the honest form — a NOTICE
that only said "some files are derived" would carry the license text but
tell a reader nothing checkable.

### Consequences for the remaining steps

- **Step 4**: each per-file comment stays short and points at `NOTICE`
  for the full permission text (D1). Four files carry that pointer — the
  two testing-first files, `tracing-root-causes/SKILL.md`, and
  `references/techniques.md`.
- **Step 5**: README `## Provenance` may point at `NOTICE` alongside
  this DECISIONS.md, since the file now exists.
- `LICENSE` remains untouched — no step in this lane edits it.
