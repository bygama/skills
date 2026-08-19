# Debugging techniques

<!-- Derived from superpowers' systematic-debugging/root-cause-tracing.md,
     defense-in-depth.md, and condition-based-waiting.md (v6.3.0), MIT
     License, Copyright (c) 2025 Jesse Vincent. Adapted 2026-08-19.
     Classified substantial (MAT-94); evidence:
     work/mat-94-attribution-skills/DECISIONS.md. Full upstream permission
     notice: NOTICE (repo root). -->

Four techniques the phased process reaches for by name, distilled — the
patterns survive, the language-bound example file and the `npm`-bound
bisection script do not.

- [Backward tracing](#backward-tracing) — the symptom is not the source
- [Defense in depth](#defense-in-depth) — make the bug impossible, not absent
- [Condition-based waiting](#condition-based-waiting) — the cure for flaky timing
- [Test-pollution bisection](#test-pollution-bisection) — find which test dirties the tree

## Backward tracing

Bugs surface deep in the stack — a file written to the wrong place, a
database opened on the wrong path, a command run in the wrong directory.
The instinct is to fix where the error appeared. That is the symptom.

**Trace backward until the value stops being wrong**, then fix there.

1. **Observe the symptom** exactly — the operation, the bad value, the
   place it landed.
2. **Find the immediate cause** — the line that directly performed it.
3. **Ask what called it**, and with what argument. Then what called
   *that*. Keep going.
4. **Stop at the origin** — the first frame where the value was correct,
   or where it was conjured wrong. That is the root cause.

Two traps worth naming, because both make a wrong value look like a
wrong *place*:

- **Empty is not absent.** An empty string as a working directory
  resolves to the process's own cwd; an undefined path becomes the
  current one. Check for empty and undefined explicitly — they produce
  the same symptom as a wrong-but-present value and none of the same
  cause.
- **Initialization order.** A value read at module load, before the
  setup hook that fills it, is empty for reasons no amount of staring at
  the call site reveals.

### When manual tracing runs out

Instrument the dangerous operation rather than guessing:

```
before the risky call:
  capture a stack trace
  log it with the value, the process cwd, and the relevant env vars
```

Log **before** the operation, not in its failure handler — an operation
that succeeds at doing the wrong thing never reaches the handler. In
tests, write to a channel the runner actually shows: many runners
swallow the application logger and pass `stderr` through.

Read the captured traces for the repeating frame — the same test file,
the same parameter, the same caller — and that is the trigger.

## Defense in depth

One validation feels sufficient once the bug is found. It isn't: a
single check gets bypassed by a different code path, removed in a
refactor, or mocked away in a test.

Fix at the source, then **validate at every layer the bad value passed
through**, so the failure becomes structurally impossible rather than
currently absent.

| Layer | Purpose | Catches |
|---|---|---|
| Entry point | reject invalid input at the API boundary | most bad calls |
| Business logic | the value must make sense for THIS operation | edge cases past the boundary |
| Environment guard | refuse dangerous operations in dangerous contexts | e.g. a destructive filesystem call outside a temp dir under test |
| Instrumentation | capture context when the others fail | the structural misuse nobody predicted |

Applying it: trace the data flow, list every checkpoint the value
crosses, add the layer appropriate to each, then try to bypass layer 1
and confirm layer 2 catches it. Layers you never tested are decoration.

The environment guard is the one most often skipped and most often
decisive — a check that refuses to `git init`, drop a table, or delete a
tree outside a temp directory while tests run converts a catastrophic
class of bug into a loud error.

## Condition-based waiting

Flaky tests usually encode a guess about timing. The guess passes
locally and fails in CI under load, which is why "add 500ms" is not a
fix and "add 5000ms" is a slower race.

**Wait for the condition you actually care about, not for a duration.**

```
❌  sleep(50); expect(result).toBeDefined()
✅  waitFor(() => result !== undefined); expect(result).toBeDefined()
```

A polling helper is ~10 lines in any language: call the predicate, return
when truthy, sleep a short interval, throw a descriptive error past a
deadline. Three details matter — poll at ~10ms rather than as fast as
possible, always carry a timeout with a message naming what was awaited,
and re-read the value inside the loop rather than closing over a stale
copy.

| Waiting for | Predicate |
|---|---|
| an event | `events.some(e => e.type === 'DONE')` |
| a state | `machine.state === 'ready'` |
| a count | `items.length >= 5` |
| a file | `exists(path)` |
| a compound | `obj.ready && obj.value > 10` |

**When a fixed delay is genuinely correct:** the behavior under test IS
timing (a debounce, a throttle, a tick interval). Then wait on the
triggering condition first, and only then wait the fixed span — derived
from the known interval, never guessed, with a comment saying where the
number came from.

## Test-pollution bisection

When something appears during a test run — a stray directory, a mutated
fixture, a leaked record — and no one knows which test produced it,
bisect instead of reading the suite.

1. Confirm the tree is clean and the artifact is absent.
2. Run **one** test file.
3. Check whether the artifact appeared. If yes, that file is the
   polluter — stop.
4. Otherwise clean up and repeat with the next file.

Two conditions make the result trustworthy: reset the state between
runs, or a single early polluter masks every later one; and run files
individually rather than as a filtered suite, since shared setup can
produce the artifact on its own.

This is a dozen lines of shell in any runner and deliberately not
shipped as a script here — the loop is the technique, and hard-coding
one project's test command is what made the original version
unportable.
