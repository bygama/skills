---
name: testing-first
description: Enforces test-first implementation — the Iron Law (no production code without a failing test first), RED-GREEN-REFACTOR with both verification beats mandatory, and the rationalization table that catches "just this once". Cycle evidence lands in the lane's PROGRESS.md, and completion hands off to work-verify instead of being self-claimed. Use before writing implementation code for any feature, bugfix, or behavior change; when writing or reviewing a test; when tests are being added after the fact; and whenever time pressure, sunk cost, or "I already manually tested it" is arguing for skipping TDD. Covers test-driven development (TDD).
---

# Testing first

<!-- Adapted from superpowers' test-driven-development (v6.3.0), 2026-08-19.
     Kept: the Iron Law, the cycle, the rationalizations, the red flags.
     Adapted to AE surfaces; dropped what only worked inside that suite's
     chain. Provenance: work/mat-47-house-tdd/DECISIONS.md. -->

Write the test first. Watch it fail. Write the least code that passes.

**If you did not watch the test fail, you do not know it tests the right
thing.** That single sentence is the whole discipline; everything below
defends it against the arguments you will make against it.

Violating the letter of the rules is violating the spirit of the rules.

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Wrote the code before the test? Delete it and start from the test.

Delete means delete. Not kept as reference, not "adapted" while the
tests get written, not read one more time. Code you keep is code you
reproduce — and reproducing it is testing after, with extra steps.

**Applies to:** new features, bugfixes, refactors, behavior changes.
**Applies here too:** a skill, an eval, a prompt, a script. In this
library the failing test is the eval, which is why evals change before
content does — same law, different artifact.

**Exceptions:** throwaway prototypes, generated code, configuration.
These are the owner's call, not yours to grant — route the ask through
the lane, or the orchestration mailbox when dispatched, and record the
answer in DECISIONS.md. Thinking "skip it just this once"? That is the
rationalization, not the exception.

## The cycle

### RED — write the failing test

One minimal test showing what should happen. One behavior, a name that
describes that behavior, real code rather than mocks.

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

The version to avoid asserts on a mock — `expect(mock).toHaveBeenCalledTimes(3)`
under a name like `'retry works'` — which tests the mock's bookkeeping,
not the retry.

### Verify RED — watch it fail

**Mandatory. This is the beat everyone skips.** Run the test.

Three things must hold: it *fails* rather than errors, the failure
message is the one you expected, and it fails because the behavior is
missing — not because of a typo or an unresolved import.

- Passes already? You are testing behavior that exists. Fix the test.
- Errors? Fix the error and rerun until it fails properly.

### GREEN — the least code that passes

Simplest thing that turns the test green. No options object nobody
asked for, no second strategy, no refactor of neighboring code. YAGNI is
not a style preference here — extra behavior is behavior no test proved.

### Verify GREEN — watch it pass

**Mandatory.** The test passes, the rest of the suite still passes, and
the output is pristine — no new warnings, no stray logs.

Test still failing? Fix the code, never the test. Something else broke?
Fix it now, not later.

### REFACTOR — clean up on green

Only once green: remove duplication, improve names, extract helpers.
Tests stay green, behavior stays identical. Then the next failing test.

## Evidence goes in the lane

The two verification beats produce the only proof that the cycle
happened, and proof that lives in your reply dies with the session.

**Inside a lane** (M and above), append to `work/<slug>/PROGRESS.md` as
each beat happens: the command, its exit, and one line of result — the
RED entry naming the failure you saw, the GREEN entry naming the pass.
That is the record `work-verify` reads, and it is what survives
compaction, a handoff, or a fresh reviewer arriving with no context.

**At S tier**, with no lane, quote the command and its exit in the
reply — the tier's own definition of done.

Write it when you see it. Evidence reconstructed at the end is a memory
of the run, not the run. And write it nowhere else: this skill produces
no artifacts of its own — no scratch test plan, no side ledger, no notes
file. The lane is the record.

## Good tests

| Quality | Good | Bad |
|---|---|---|
| Minimal | One behavior. An "and" in the name means split it. | `test('validates email and domain and whitespace')` |
| Clear | The name states the behavior | `test('test1')` |
| Honest | Fails when the behavior breaks | Passes because a mock is mounted |

Writing or changing any test, adding a mock, or adding a test-only
helper? Read
[references/writing-good-tests.md](references/writing-good-tests.md) —
name the break before the body, derive expectations by hand, assert real
behavior rather than mock bookkeeping, and run the mutation check before
you finish.

## Handing off — this skill does not mark work done

Run this self-check at the end of the cycle. It is a *self-check*, not
verification:

- [ ] Every new behavior has a test
- [ ] Each test was watched failing before its implementation existed
- [ ] Each failed for the expected reason — missing behavior, not a typo
- [ ] Minimal code written to pass each one
- [ ] Whole suite green, output pristine
- [ ] Real code exercised; mocks only where unavoidable
- [ ] Edge cases and error paths covered
- [ ] RED and GREEN evidence is in PROGRESS.md, written as it happened

A box you cannot tick means you skipped TDD — go back to the cycle, do
not proceed.

Every box ticked means the cycle is done, **not that the work is done.**
`work-verify` owns the completion gate. Hand off there: do not declare
the work complete, do not write a done verdict into PROGRESS.md, do not
flip a `feature_list.json` row to `passing`. A green suite is an input
to that gate, never a substitute for it.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "Too simple to break" | Simple code breaks. The test costs thirty seconds. |
| "I'll test after" | Tests written after pass immediately, which proves nothing. They may test the wrong thing, test the implementation instead of the behavior, or miss the case you already forgot. You never watched it fail, so you never proved it can catch anything. |
| "Tests after achieve the same thing — spirit, not ritual" | Tests-after answer "what does this do?". Tests-first answer "what should this do?". Written after, they are biased by the code in front of you: you cover the cases you remembered, not the ones you would have discovered. |
| "Already manually tested it" | Manual testing leaves no record of what was covered, cannot be re-run when the code changes, and quietly shrinks under pressure. "Worked when I tried it" is not coverage. |
| "Deleting X hours of work is wasteful" | Sunk cost — those hours are spent either way. The real choice is rewrite with TDD (high confidence) versus bolt tests on after (low confidence, probable bugs). Keeping code you cannot trust is the waste. |
| "Keep it as reference and write tests first" | You will adapt it, and that is testing after. Delete means delete. |
| "I need to explore first" | Fine — explore, then throw the exploration away and start with TDD. |
| "Hard to test means the test is wrong" | Listen to the test. Hard to test is hard to use; the design is talking to you. |
| "TDD will slow me down" | TDD is the fast path: bugs caught before commit, regressions prevented, refactors made without fear. The shortcut ends in production debugging. |
| "Existing code here has no tests" | You are improving it. Add the test for the behavior you are touching. |
| "work-verify will catch it" | work-verify proves a command exited 0. It cannot prove a test was ever watched failing — untested-first code passes that gate carrying its bug. |
| "The PLAN step's acceptance command is my test" | Acceptance proves the step ran. It is one command for a whole step, written by the planner before any of this — not a per-behavior test, and never a RED you watched. |
| "It's a skill/doc, not code — TDD doesn't apply" | The eval is the failing test. Evals change before content, and content that shipped first has an eval written to fit it. |
| "I'll add the PROGRESS evidence at the end" | Then it is recollection, not evidence. Write each beat when you see it. |

## Red flags — stop and start over

Code written before the test · a test added after the implementation · a
test that passed on its first run · not being able to say why the test
failed · "tests come later" · "just this once" · "I already tested it
manually" · "it's about spirit, not ritual" · "keep it as reference" ·
"deleting those hours is wasteful" · "TDD is dogmatic, I'm being
pragmatic" · "this case is different because…" · "the acceptance command
covers it" · "I'll write the evidence up afterwards".

Every one of these means the same thing: delete the code, start from the
test.

## When stuck

| Problem | Move |
|---|---|
| Don't know how to test it | Write the API you wish existed, then the assertion. Still stuck: ask the owner through the lane. |
| The test is too complicated | The design is too complicated. Simplify the interface. |
| Everything has to be mocked | The code is too coupled. Inject the dependency. |
| The setup is enormous | Extract helpers. Still enormous? The design is the problem. |

## Bugfixes

Never fix a bug without a test. Write the failing test that reproduces
it, then run the cycle: the test proves the fix and stands guard against
the regression forever.

Reproducing is not diagnosing. When the cause is not yet known, the
diagnosis is `tracing-root-causes`' job — competing hypotheses, evidence,
active disconfirmation — and this skill takes over once you can write a
test that fails for the right reason.

## The final rule

```
Production code -> a test exists, and it failed first
Otherwise       -> not TDD
```

No exception without the owner's permission, recorded in DECISIONS.md.
