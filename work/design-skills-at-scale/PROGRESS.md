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

## In progress

## Tried and failed

## Next

## Verification

<!-- PASS evidence only, written by work-verify (newest on top); the close
     handoff refuses to close a lane without a current PASS block here. -->

<!-- First read of every session. If it isn't here, it didn't happen. -->
