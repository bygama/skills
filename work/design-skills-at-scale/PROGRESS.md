# design-skills-at-scale — progress

## Done

- 2026-08-20 — Step 1 (lane init). Created `PROGRESS.md` (from
  `Agent-Engineering/templates/repo/work/PROGRESS.md.template`), `DECISIONS.md`
  (from the same template family, with the two standing rulings from the
  controller recorded verbatim and dated), and `feature_list.json` with 11
  rows `F01`-`F11` mapped one-to-one onto SPEC §4 requirements 1-11 in order.
  Each row's `verification` names the concrete command(s) that will prove it
  at step 11: the PLAN's own acceptance commands where the requirement maps
  directly to one (F08 → PLAN step 9's `git grep` sweep verbatim; F09 →
  the resolved `agent-lint` invocation; F10 → PLAN steps 5-6's eval-count
  checks plus a `git log` ordering check that evals precede skill-content
  commits; F11 → PLAN step 10's `design-md-gen` + `agent-lint` pair plus an
  AE-repo-untouched check), and for behavioral requirements (F01-F07) the
  eval file expected to encode the behavior (named per the existing
  sequential convention — `eval-04.md`/`eval-05.md` new in
  `designing-consistently`, `eval-05.md`/`eval-06.md` new in
  `extracting-design-md`, `eval-01.md` rewritten in both) plus the
  grep/lint check that will prove the rewritten `SKILL.md` carries that
  behavior in prose. All rows start `state: not_started`, `evidence: null`.

  Verified before recording: ruling 2's path,
  `/c/Briar/repos/mine/../../repos/work/Pegasuz/Pegasuz-Core/frontend-admin/admin`
  (= `/c/Briar/repos/work/Pegasuz/Pegasuz-Core/frontend-admin/admin`), exists
  on this machine and matches the spelling given — no correction needed.

  Acceptance: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  (the sibling checkout's real location per the ruling above) exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Only the pre-existing, expected LOW `cmd-drift` finding remains (per
  DECISIONS.md ruling 1); the `lane-incomplete` MEDIUM that was failing the
  lint before this step is gone. Files changed: `work/design-skills-at-scale/PROGRESS.md`,
  `work/design-skills-at-scale/DECISIONS.md`, `work/design-skills-at-scale/feature_list.json`
  (all new).

- 2026-08-20 — Step 1 fix round 1 (review findings). Fixed the three
  Important findings, all in `feature_list.json`:
  - **Interleaved prose broke the shell command** (F01, F02, F03, F05, F06,
    F10): each `verification` had a parenthetical citation spliced into the
    middle of the command, so none was actually pasteable. Moved every
    citation (eval filenames, PLAN step numbers, SPEC cross-refs) into
    `behavior`; `verification` is now a single `&&`-chained line per row.
  - **F04 referenced a nonexistent artifact**: dropped the
    `diff … against its pre-step-7 baseline` clause (no PLAN step archives
    a pre-rewrite copy of `eval-01.md`), and moved the eval-01.md-rewrite
    citation into `behavior`. `verification` now stands on the three
    `grep -qi` checks alone (promote/demote/escalat).
  - **F10 named no mechanical ordering check**: replaced the vague
    `git log --oneline confirming … precede …` with
    `git merge-base --is-ancestor "$(git log --format=%H -1 -- <evals-dir>)" "$(git log --format=%H -1 -- <skill-md>)"`,
    run for both skills. Verified the mechanism before recording it (see
    below) — the eval-count checks (5, 6) still gate it via `&&`.
  - `F07`, `F08`, `F09`, `F11` left untouched per the controller's scope
    (F07's content-judgment clause explicitly deferred to work-verify).

  **Correction (round 2):** the line above was wrong. Only F07 was actually
  deferred, and only its content-judgment clause — not its pasteability.
  F08, F09 and F11 carried the identical prose-spliced-into-command defect
  as F01/F02/F03/F05/F06 and were left broken by mistake, uncalled-out.
  See the round-2 entry below for the fix.

  Reshaped `DECISIONS.md`'s first two entries into the
  `- YYYY-MM-DD — <choice> — <why>` shape the file's own header comment and
  `Agent-Engineering/templates/repo/work/DECISIONS.md.template:4` specify,
  losing none of their content (both the resolved AE path and its
  `agent-lint.mjs:25-31` citation; both the resolved Pegasuz path, the
  verified-present note, and the `agent-lint.mjs:354`
  scoped-to-shipped-surfaces / never-in-`skills/` citation are still
  present, just re-segmented). Appended a third dated entry recording this
  round's controller ruling to pull that Minor into the fix round now
  rather than defer it, since PLAN step 4 appends more rulings to this same
  file.

  **Re-ran the gate:** `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  still exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  **Sanity-ran every rewritten `verification` string** (F01-F06, F10)
  against the repo as it stands today (before PLAN steps 5-8 exist). All
  ran without shell errors and correctly returned exit 1 — expected, since
  each references an eval file, `SKILL.md` phrase, or eval count that a
  downstream step (5-8) has not produced yet; none is claimed as passing
  now. F08 (unchanged) also correctly still exits 1 today — the stale
  `Context-Engineering`/`context-lint` references it checks for are still
  present, pending PLAN step 9. F09 (unchanged) exits 0 today, as it always
  has. Before recording F10's `merge-base` mechanism, tested it in
  isolation against the two currently-existing paths
  (`skills/designing-consistently/evals`,
  `skills/designing-consistently/SKILL.md`): both resolve to the same
  pre-existing commit, and `git merge-base --is-ancestor <sha> <same-sha>`
  correctly exits 0 (a commit is its own ancestor) — confirming the command
  is syntactically valid and evaluates in the expected direction, not
  merely copy-pasted from the reviewer's suggestion unverified. It was not
  run against the lane's own future step-5/7 commits, since those do not
  exist yet.

  Files changed: `work/design-skills-at-scale/feature_list.json`,
  `work/design-skills-at-scale/DECISIONS.md`.

- 2026-08-20 — Step 1 fix round 2 (review findings). Round 1 fixed the
  interleaved-prose defect in six rows (F01-F06, F10) but left F07, F08,
  F09 and F11 carrying the identical defect, and mis-reported them as
  intentionally untouched — see the correction inserted above. Per the
  controller's rulings, fixed all four this round:
  - **F07**: dropped the trailing `, and … exercises the
    undeclared-contradiction-is-drift case.` clause from `verification`
    (it was prose after a comma, not part of a runnable command) and moved
    it into `behavior`, flagged there as a content judgment for
    work-verify's triage — this resolves the original Minor (F07's
    judgment-call note) as a side effect, per Ruling A, rather than
    conflicting with it. `verification` is now the bare
    `grep -q 'exception to Global' skills/designing-consistently/SKILL.md`.
  - **F08**: moved `(PLAN step 9 acceptance, verbatim)` into `behavior`;
    `verification` is now the bare `! git grep -n
    'Context-Engineering\|context-lint' -- skills/designing-consistently
    skills/extracting-design-md README.md`, unchanged in substance from
    PLAN step 9's own acceptance command.
  - **F09**: moved the `exits 0 (path resolved …)` prose into `behavior`;
    `verification` is now the bare
    `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`.
  - **F11** (worst offender, per Ruling B): moved both `exits 0` splices
    and the PLAN-step/path-resolution parenthetical into `behavior`;
    rewrote the "AE repo untouched" clause as a real test —
    `[ -z "$(git -C /c/Briar/repos/mine/Agent-Engineering status
    --porcelain)" ]` — instead of the prose `plus … is empty`.
    `verification` is now three `&&`-chained commands whose combined exit
    status is the row's answer:
    `node design-md-gen.mjs … && node agent-lint.mjs . && [ -z "$(git -C
    … status --porcelain)" ]` (paths as recorded in the file).

  Recorded Ruling A (why F07 comes off deferral this round) and Ruling C
  (the em-dash-count Minor on `DECISIONS.md:5-6`, deferred to work-verify,
  not acted on) as dated entries in `DECISIONS.md`.

  **Verified all eleven `verification` strings parse as single commands.**
  Extracted every row's `verification` string verbatim to its own file and
  ran `bash -n` over each of the eleven — all eleven report no syntax
  error (F01 through F11, all OK). This is the full set, not a sample.

  **Ran all eleven** against the repo as it stands today: F09 exits 0
  (already true); F01-F08, F10, F11 all exit 1, and for a downstream
  reason in every case, not a broken command — F01/F03 (`eval-04.md`/
  `eval-05.md` don't exist yet), F02/F04/F06 (`SKILL.md` phrases not
  written yet), F05/F10 (eval counts still 4/3, not 6/5), F07 (`exception
  to Global` not written yet), F08 (stale references still present,
  pending step 9), F11 (`design-md-gen` fails with `ENOENT` on
  `work/design-skills-at-scale/proving-run/DESIGN.md`, which step 10
  creates). None of these is claimed as passing now; all rows stay
  `not_started`.

  **Re-ran the gate:** `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  still exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/feature_list.json`,
  `work/design-skills-at-scale/DECISIONS.md`, `work/design-skills-at-scale/PROGRESS.md`
  (the correction to the round-1 entry).

## In progress

## Tried and failed

## Next

## Verification

<!-- PASS evidence only, written by work-verify (newest on top); the close
     handoff refuses to close a lane without a current PASS block here. -->

<!-- First read of every session. If it isn't here, it didn't happen. -->
