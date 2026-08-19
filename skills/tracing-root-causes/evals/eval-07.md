# Eval 07: trace to the source, then close the loop properly

## Query

"Running the test suite leaves a stray `.git` directory inside
`packages/core/` — our source tree, not a temp dir. Nothing in the tests
mentions git explicitly. Find it and fix it."

## Expected behavior

- [ ] Traces BACKWARD from where the symptom appears to where the bad
      value originates, instead of fixing at the point of the error —
      names the chain (what ran `git init` → what passed it that
      directory → where that directory value came from) rather than
      guarding the last frame.
- [ ] Recognizes the specific mechanism worth knowing: an empty-string
      `cwd` silently resolves to `process.cwd()`, so "wrong directory"
      and "no directory" produce the same symptom — and checks for the
      empty/undefined case rather than assuming a wrong-but-present
      path.
- [ ] When manual tracing runs out, proposes instrumentation at the
      dangerous operation — capture a stack trace plus the directory,
      `cwd`, and environment BEFORE the call, not after it fails — and
      notes that in tests this must go through a channel the runner
      actually shows.
- [ ] When the triggering test is unknown, proposes bisection over test
      files (run them one at a time, check for the pollution after each,
      stop at the first that produces it) rather than reading the whole
      suite.
- [ ] Fixes at the SOURCE, then adds validation at the layers the bad
      value passed through — entry point, business logic, and an
      environment guard that refuses the dangerous operation outside a
      temp directory under test — so the bug becomes structurally
      impossible rather than merely absent.
- [ ] Writes the failing reproduction BEFORE the fix, and changes one
      thing at a time.
- [ ] Records the evidence in the lane's `PROGRESS.md` — not in an
      ad-hoc debugging log, a scratch file, or the chat alone.
- [ ] Ends by handing off to `work-verify` for the completion gate and
      does NOT declare the work done, verified, or shippable on its own
      authority. "The suite is clean now" is reported as a result to be
      verified, not as a verdict.
