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
