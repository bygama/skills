---
name: testing-first
description: Test-first by default, wherever a runnable test and a clear expected outcome exist — RED before GREEN with both beats watched, one RED per rule the asker stated, and one proof per touched existing command that its old behaviour holds against HEAD. Skipping is allowed only with a named reason written in the lane's Log before the code; "just this once", time pressure, sunk cost and "I tested it manually" are never reasons, and the rationalization table catches them. Evidence lands in the Log of work/<slug>/LANE.md (a v1 lane - PROGRESS.md) as it happens, a task is ticked only with its proof, and completion hands off to work-verify instead of being self-claimed. Use before writing implementation code for any feature, bugfix, or behavior change; when writing or reviewing a test; when tests are being added after the fact; when authoring a skill or an eval, where the eval is the failing test; and whenever pressure is arguing for skipping TDD. Covers test-driven development (TDD).
---

# Testing first

<!-- Derived from superpowers' test-driven-development/SKILL.md (v6.3.0),
     MIT License, Copyright (c) 2025 Jesse Vincent. Adapted 2026-08-19.
     Classified substantial (MAT-94); evidence:
     https://github.com/bygama/skills/blob/002ef05fd8b9aceab6f7ed9f14cb8b8fa076441b/work/mat-94-attribution-skills/DECISIONS.md. Full upstream permission
     notice: NOTICE (repo root). -->

Write the test first. Watch it fail. Write the least code that passes.

**If you did not watch the test fail, you do not know it tests the right
thing.** That single sentence is the whole discipline; everything below
defends it against the arguments you will make against it.

## The default

Test-first is the default wherever a runnable test and a clear expected
outcome exist: new features, bugfixes, refactors, behavior changes. Three
parts:

1. **RED before GREEN, watched.** The failing test runs and fails for the
   right reason before the code that fixes it exists.
2. **One RED per requested rule.** Each rule the asker stated gets its
   own failing test, traced to its spec. "Lowercase it and strip accents"
   is two REDs, not one combined test that turns red once.
3. **One proof per touched existing command** that its old behaviour
   holds: stdout, stderr and exit compared against `HEAD`. See
   [Proving the old behaviour](#proving-the-old-behaviour-against-head).

**Applies here too:** a skill, an eval, a prompt, a script. In this
library the failing test is the eval, which is why evals change before
content does.

Wrote the code before the test? Do not defend it: write the test, watch
it fail against the code with the behavior removed or broken (revert it,
or `git worktree` the parent commit), then restore. A test that never
failed proves nothing, and that is what "keeping it as reference"
produces.

## Skipping needs a named reason

Skipping test-first is allowed only with a **named reason written in the
lane's Log before the code**. You may name it yourself; no permission
round trip. The reasons:

- no runnable harness (and building one is out of scope)
- an exploratory spike that will be thrown away
- generated code
- pure configuration
- a UI change verified by an e2e or visual check instead

```markdown
- L7 2026-10-27 — test-first skipped: pure configuration, no runnable
  harness (nginx -t only checks syntax). Verify: `nginx -t`.
```

Name the substitute proof in the same line and still run it. The Log
line comes first; a reason written after the edit is a rationalization
with a timestamp. No reason on the list fits? Then test first.

Never reasons: "just this once", time pressure, sunk cost, "I tested it
manually", "too simple to break", "it's only a refactor". They sit in
the [rationalization table](#common-rationalizations) for a reason.

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

**Inside a lane**, append to the `## Log` of `work/<slug>/LANE.md` as
each beat happens (a v1 lane: its `PROGRESS.md`): the command, its exit,
and one line of result — the RED entry naming the failure you saw, the
GREEN entry naming the pass, the regression proof naming what was
compared. A skipped test-first and its reason go there too. That is the
record `work-verify` reads, and it survives compaction, a handoff, or a
fresh reviewer arriving with no context.

A task is ticked in `## Tasks` only with its proof already in the Log or
on the task line. No proof, no tick.

**At S tier**, with no lane, quote the command and its exit in the
report — the tier's own definition of done.

Write it when you see it. Evidence reconstructed at the end is a memory
of the run, not the run. And write it nowhere else: this skill produces
no artifacts of its own — no scratch test plan, no side ledger, no notes
file. The lane is the record.

### What that looks like — a bugfix, end to end

Bug: an empty email is accepted. RED first, and the failure is watched:

```
$ npm test -- submitForm
FAIL  expected 'Email required', received undefined
```

Then the least code that passes — an early return on a blank email — and
the pass is watched:

```
$ npm test -- submitForm
PASS  14 passing
```

Both beats go into the `## Log` of `work/<slug>/LANE.md` as they happen,
one line each:

```markdown
- L4 RED  `npm test -- submitForm` → exit 1 — "expected 'Email required',
  received undefined"; fails because the guard does not exist yet
- L5 GREEN `npm test -- submitForm` → exit 0 — 14 passing, no warnings
```

The RED line is the one that carries the proof. It names the failure
that was actually seen and why it was the right one — which is the only
thing separating a test that guards the behavior from a test that was
written afterwards and never could have failed.

## Proving the old behaviour against HEAD

Every existing command the change touches gets one proof that what it
did before, it still does: stdout, stderr and exit code, same inputs,
`HEAD` against the working copy. Keep it practical:

```bash
git worktree add --detach /tmp/head-copy HEAD   # or: git show HEAD:bin/tool > /tmp/tool-head
for input in 'Hello' '' 'ñandú'; do
  printf '%s' "$input" | /tmp/head-copy/bin/tool >/tmp/h.out 2>/tmp/h.err; echo $? >/tmp/h.rc
  printf '%s' "$input" | bin/tool                >/tmp/w.out 2>/tmp/w.err; echo $? >/tmp/w.rc
  cmp /tmp/h.out /tmp/w.out && cmp /tmp/h.err /tmp/w.err && cmp /tmp/h.rc /tmp/w.rc
done
git worktree remove /tmp/head-copy
```

No `git stash`: it hides your work and loses it on a failed pop. Cover
the inputs the change is *not* about — empty input, the error path, the
flags nobody mentioned — since the requested rules already have their
REDs. Log the comparison command and its observed result. A difference
outside the requested rules is a finding, not a tick. No old behaviour
(a new command), no proof owed.

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

- [ ] Every new behavior, and every rule the asker stated, has its own test
- [ ] Each test was watched failing before its implementation existed
- [ ] Each failed for the expected reason — missing behavior, not a typo
- [ ] Minimal code written to pass each one
- [ ] Whole suite green, output pristine
- [ ] Real code exercised; mocks only where unavoidable
- [ ] Edge cases and error paths covered
- [ ] Each touched existing command proven unchanged against `HEAD`
- [ ] RED, GREEN and regression evidence is in the Log, written as it happened; any skip has its named reason there, dated before the code

A box you cannot tick means you skipped TDD — go back to the cycle, do
not proceed.

Every box ticked means the cycle is done, **not that the work is done.**
`work-verify` owns the completion gate. Hand off there: do not declare
the work complete, do not write a done verdict into the Log, do not
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
| "Keep it as reference and write tests first" | You will adapt it, and that is testing after. Write the test, watch it fail with the behavior removed, then restore. |
| "I need to explore first" | Fine, and it is a named reason: log "exploratory spike, thrown away" before you start, then throw it away and start with TDD. |
| "Hard to test means the test is wrong" | Listen to the test. Hard to test is hard to use; the design is talking to you. |
| "TDD will slow me down" | TDD is the fast path: bugs caught before commit, regressions prevented, refactors made without fear. The shortcut ends in production debugging. |
| "Existing code here has no tests" | You are improving it. Add the test for the behavior you are touching. |
| "work-verify will catch it" | work-verify proves a command exited 0. It cannot prove a test was ever watched failing — untested-first code passes that gate carrying its bug. |
| "The PLAN step's acceptance command is my test" | Acceptance proves the step ran. It is one command for a whole step, written by the planner before any of this — not a per-behavior test, and never a RED you watched. |
| "It's pure config / no harness, so I'll just skip" | Maybe, and then say so: a Log line with the reason, before the edit. Silence is not a skip, it is an omission. |
| "The suite passing proves the old behaviour holds" | The suite pins what someone thought to assert. stdout, stderr and exit against `HEAD` pin what the command did. |
| "It's a skill/doc, not code — TDD doesn't apply" | The eval is the failing test. Evals change before content, and content that shipped first has an eval written to fit it. |
| "I'll add the Log evidence at the end" | Then it is recollection, not evidence. Write each beat when you see it. |

## Red flags — stop and start over

Code written before the test · a test added after the implementation · a
test that passed on its first run · not being able to say why the test
failed · "tests come later" · "just this once" · "I already tested it
manually" · "it's about spirit, not ritual" · "keep it as reference" ·
"deleting those hours is wasteful" · "TDD is dogmatic, I'm being
pragmatic" · "this case is different because…" · "the acceptance command
covers it" · "I'll write the evidence up afterwards" · a skip with no Log line ·
a skip whose reason is not on the list.

Every one of these means the same thing: stop, write the test (or the
named reason, before the code), and start from there.

## When stuck

| Problem | Move |
|---|---|
| Don't know how to test it | Write the API you wish existed, then the assertion. Still stuck: ask the owner through the lane, or name "no runnable harness" in the Log if that is true. |
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
Runnable test + clear outcome -> a test exists, and it failed first
Otherwise                     -> a named reason in the Log, before the code
```
