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

- 2026-08-20 — Step 2 (cold baseline of `designing-consistently`, transcription only). Per the
  controller ruling in `DECISIONS.md`, the cold run itself was executed by a separate cold-runner
  agent given only the current `designing-consistently` workflow and the read-only admin repo,
  told nothing of this lane, this SPEC, or SPEC §5's three recorded predictions — the runner's
  report was transcribed into `work/design-skills-at-scale/baseline-designing-consistently.md`,
  not re-derived. My own work this step was transcription and framing only: I did not run
  `designing-consistently` myself, did not re-derive any finding, and did not correct, condense,
  reorder, or reword the runner's account.

  Copied the runner's five `## Step N` sections and its `## What I would have committed` section
  verbatim (byte-for-byte) into the baseline file, including one stray CJK character mid-word in
  Step 1's prose ("running至 at least line 1597") — kept as-is per instruction, with a bracketed
  editorial note beside it rather than silently fixed.

  Added a front-matter-style preamble above `## Step 1`, clearly marked as the lane's framing and
  not the runner's words, stating: what was run (`designing-consistently` as it stands today,
  unmodified); which copy (the user's global skills folder install, verified byte-identical to
  this worktree's `skills/designing-consistently/SKILL.md` via `diff` before writing this file —
  re-verified, not just cited); against what (`<PEGASUZ>/Pegasuz-Core/frontend-admin/admin`,
  read-only); the task the runner was given, quoted verbatim in Spanish; the isolation the runner
  ran under; and — the piece flagged as most important — which observations the read-only
  constraint affects and which it does not. Recorded that the constraint shaped only Step 3 (the
  component/view diff reported as "would have committed" instead of written) and Step 5's
  screenshot action (`npm run dev`/`build` withheld because it would touch `dist/`), and did
  **not** cause Steps 1/2/4's core findings — the repo's total absence of any `DESIGN.md`, of a
  `## Decisions` section, and of `design.tokens.css`/`design-md-gen.mjs`/a `context-lint` script
  are facts about repo content, true in a writable clone too.

  Appended `## Controller-verified addenda` below the transcript, containing only the three facts
  supplied by the controller (the `Documentation/` pointer being monorepo-root-relative while the
  admin has no local `Documentation/`; zero writes confirmed via `git status --short` before/after
  and HEAD unchanged at `00ddbcbaf`; the prose docs' sizes under the admin's `docs/`), each
  labelled as controller-verified and kept separate from the runner's words. Added no
  interpretation of what the transcript means — that is PLAN step 4's job.

  Did not touch `skills/`, `SPEC.md`, or `PLAN.md`. Made zero writes to the Pegasuz checkout.

  Verified before recording: `diff ~/.claude/skills/designing-consistently/SKILL.md
  skills/designing-consistently/SKILL.md` exits 0 (byte-identical, confirming the DECISIONS.md
  ruling this step's preamble cites still holds at transcription time).

  Acceptance: `test $(grep -c '^## Step ' work/design-skills-at-scale/baseline-designing-consistently.md) -eq 5`

  ```
  ACCEPT PASS
  ```

  Re-ran the gate: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` still
  exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/baseline-designing-consistently.md` (new).

- 2026-08-20 — Step 2 fix round 1 (review findings). Framing-only fixes, no transcript edits —
  not one character touched, CJK note included.
  - **Important:** the preamble's "one constraint" bullet over-generalized past its own scoping —
    it named only the three absence facts (no DESIGN.md, no `## Decisions`, no
    tokens/generator/lint script) as fence-independent, then closed with an unqualified "Do not
    read those three steps' failures as artifacts of the fence," silently sweeping in a second,
    distinct Step 1 failure the bullet never named: the runner's own words attribute part of the
    blocked "offer to instantiate" fallback to the read-only constraint itself ("even if it
    weren't [out of scope-fence reach], the repo is read-only so I could not create the file
    regardless"). Since SPEC §5's first recorded prediction is exactly "step 1 offers to
    instantiate a DESIGN.md," and PLAN step 4 reconciles that prediction against this file, the
    blanket claim risked step 4 treating Step 1's failure-to-offer as a clean fence-independent
    signal when it isn't entirely. Fixed by narrowing the "does not affect" sentence to the
    absence facts only, and adding one clause naming the entanglement in Step 1 explicitly,
    leaving the reconciliation itself to PLAN step 4 rather than resolving it here.
  - **Minor (pulled in by controller ruling):** the "which copy" bullet claimed the global/
    worktree `SKILL.md` byte-identity was "verified... before the run," which doesn't match this
    implementer's own described sequence (`diff` run at transcription time, after the run had
    already happened). Reworded to attribute the before-the-run check to the controller (per the
    controller's own account, given directly) and the implementer's own re-check to
    transcription time — stating only the timing actually known to each party, not a sequence I
    didn't observe.
  - **New controller-verified fact added as addendum 4** (not drawn by me as a conclusion): the
    skill's cited template source, "Context-Engineering `templates/repo/DESIGN.md.template`",
    does not exist anywhere on this machine — a depth-2 search of the work-repos folder for any
    `Context-Engineering` directory returned nothing; the equivalent asset exists under a
    different repo and name, `Agent-Engineering/templates/repo/DESIGN.md.template` (789 bytes).
    Recorded as fact only, no interpretation — PLAN step 4 owns what it means for Step 1's
    reconciliation.

  Acceptance: `test $(grep -c '^## Step ' work/design-skills-at-scale/baseline-designing-consistently.md) -eq 5`

  ```
  ACCEPT PASS
  ```

  Re-ran the gate: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` still
  exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/baseline-designing-consistently.md`,
  `work/design-skills-at-scale/PROGRESS.md`.

- 2026-08-20 — Step 3 (cold baseline of `extracting-design-md`, transcription only). Per the same
  two-seat controller ruling as step 2, the cold run itself was executed by a separate cold-runner
  agent given only the current `extracting-design-md` workflow and the read-only admin repo, told
  nothing of this lane, this SPEC, or SPEC §5's three recorded predictions. The runner's report
  (`cold-run-extracting-design-md.md`, 910 lines / 53,784 bytes) was transcribed into
  `work/design-skills-at-scale/baseline-extracting-design-md.md`, not re-derived. My own work this
  step was transcription and framing only: I did not run `extracting-design-md` myself, did not
  re-derive any finding, and did not correct, condense, reorder, or reword the runner's account —
  including its self-caught methodology mistakes (the `\b(btn|card)\b` and `\brounded\b` false
  counts in Step 2, both left in as the runner described fixing them).

  Copied the runner's report verbatim (byte-for-byte) from its own `# Cold run:` header through
  the end of the file — its seven `## Step N` sections, the `## The DESIGN.md I would have
  written` section (left inside its existing fenced ```` ```markdown ```` block, not unwrapped,
  no second fence added), and the closing `## What I would have committed` section. Verified
  byte-identity mechanically: located the transcript's start line in the destination file, sliced
  out exactly as many lines as the source file has, and ran `diff` against the source — empty
  diff, confirming an exact copy, not a spot check.

  Added a blockquote preamble above the transcript, clearly marked as the lane's framing and not
  the runner's words, stating: what was run (`extracting-design-md` as it stands today,
  unmodified); which copy (the user's global skills folder install — the controller verified it
  byte-identical to this worktree's `skills/extracting-design-md/SKILL.md` before dispatching the
  cold runner, and I independently re-verified the same byte-identity myself at transcription time
  via `diff`, stated as my own separate check rather than folded into the controller's); against
  what (`<PEGASUZ>/Pegasuz-Core/frontend-admin/admin`, read-only); the task the runner was given,
  quoted verbatim in Spanish; the isolation the runner ran under, including the further scope
  fence its own Step 6 records (barred from reading this session's other repositories, which is
  why it could not reach the Context-Engineering/Agent-Engineering clone housing
  `design-md-gen`/`context-lint`).

  For the two constraints (READ-ONLY, NO SUBAGENTS), stated honestly and separately which
  observations each touches and which it does not, rather than one blanket claim — read-only
  touches Step 7 and the closing "what I would have committed" section, not the Steps 1-5 harvest
  (read actions, would read the same in a writable clone), and does not explain Step 4's
  no-live-owner gap or Step 6's repo-scope-fence gap either. No-subagents touches Step 2's harvest
  method directly: the runner recorded that the skill's own `## Scaling (bounded)` section names
  fan-out past ~15 surfaces as the answer, that this repo's nav config runs to 60+ entries (~10x
  that threshold), and that the fan-out lever was unavailable "by direct instruction," not the
  skill's own choice — stated as the lane's fence, not the skill's, with no decision here about
  what that means for the harvest's practicality or for SPEC §5 (left to PLAN step 4). Confirmed
  no-subagents does not touch Steps 1, 3, 4, 5, or 6's findings.

  Appended `## Controller-verified addenda` below the transcript, containing only the three facts
  supplied by the controller: the four dead documentation pointers (`UX-UI-STANDARDS.md`,
  `VISUAL_QA.md`, `ADMIN_UX_ROADMAP.md`, and doc 47 /
  `47-FRONTEND-DESIGN-SYSTEM.md`) resolving one level up at the Pegasuz monorepo root (sizes
  9757 / 3775 / 10,351 / 57,030 bytes) — the same pattern step 2's baseline recorded, now observed
  a second time over two more pointers; the one pointer that resolves nowhere at all
  (`servicio.css`'s `C:\Users\nicol\rcsistemas-design-stage\NODOS.md`, verified absent on this
  machine, not a scope artifact); and zero writes (`git status --short` empty, HEAD unchanged at
  `00ddbcbaf`). Each labelled controller-verified, no interpretation added — that is PLAN step 4's
  job.

  Did not touch `skills/`, `SPEC.md`, or `PLAN.md`. Made zero writes to the Pegasuz checkout.
  Dispatched no subagent for this step's own work.

  Verified before recording: `diff ~/.claude/skills/extracting-design-md/SKILL.md
  skills/extracting-design-md/SKILL.md` exits with no output (byte-identical, confirming the
  DECISIONS.md ruling this step's preamble cites still holds at transcription time).

  Acceptance: `test $(grep -c '^## Step ' work/design-skills-at-scale/baseline-extracting-design-md.md) -eq 7`

  ```
  ACCEPT PASS
  ```

  Re-ran the gate: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` still
  exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/baseline-extracting-design-md.md` (new).

- 2026-08-20 — Step 4 (reconcile the two baselines against SPEC §5's three recorded predictions).
  Read both baseline files in full — preambles first, then the transcripts, then the
  controller-verified addenda. Wrote one dated ruling per prediction into `DECISIONS.md` (the
  10th, 11th and 12th dated entries, one line each in the file's `- YYYY-MM-DD — <choice> —
  <why>` shape), and amended `SPEC.md` §3.5 and §3.6 for the two predictions ruled contradicted.
  Changed nothing else: no eval written or edited, nothing under `skills/`, no other SPEC
  section, no `PLAN.md`. I did not open the Pegasuz checkout at all this step — every fact below
  comes from the two baseline files in this lane.

  **Prediction 1 — "step 1 offers to instantiate a DESIGN.md" — CONTRADICTED on the literal
  wording; the defect it was reaching for is present in a more severe form.**

  - *Evidence:* `baseline-designing-consistently.md`, `## Step 1` (six searches, zero hits; the
    skill's instantiate fallback quoted and named as such; no offer of any kind made) and
    `## What I would have committed` ("no `DESIGN.md` was created or edited"). The standing
    system appears only at `## Step 2`, where the runner marks it "Improvisation (flagged — the
    skill did not tell me to do this)". Plus addendum 4 of the same file.
  - *Fence:* yes — this is the entanglement the step-2 preamble names and hands to step 4 by
    name. Resolved, not discounted. Cause A (the scope fence over the template source) is not
    load-bearing: addendum 4 verified `Context-Engineering` exists nowhere on this machine, so a
    fence-free runner hits the same dead pointer. Cause B (READ-ONLY) blocks *creating* a file,
    not *offering* one, and the same run recorded counterfactual writes four separate times
    (Step 3's component, Step 3's view diff, Step 4's would-be Decisions line, the closing
    section) without ever recording an offer. The third stopper, the absent live owner, means an
    offer could only ever have been recorded rather than delivered — and none was. No fence
    accounts for the missing offer.
  - *Literal vs. underlying, kept apart:* the offer did not happen, and it did not fail to
    happen because the skill discovered the standing system instead. Step 1 produced **neither**
    an offer **nor** a discovery — it dead-ends. So SPEC §1's underlying charge ("proposes
    creating a file instead of finding what already governs") holds in an altered and worse
    form: step 1 carries no discovery instruction at all, and its single fallback points at a
    repo that does not exist. The design fix in §3.5's `After` column is strengthened, not
    weakened.
  - *SPEC §3 amended:* yes, §3.5 — additive marked note only, no approved text rewritten.

  **Prediction 2 — "step 4 elects `13px` a token on frequency" — CONTRADICTED on the literal
  wording; the defect it was reaching for is present in a different place.**

  - *Evidence:* `baseline-extracting-design-md.md`, `## Step 4`. The 92 `text-[13px]` uses were
    never elected: the run refused to elect any token for the 430 arbitrary-pixel type values
    ("a genuine fork in the road") and carried them to `## Step 5` as an open question. The one
    type token elected on frequency was `ui` (2644 vs 801 vs 138), where frequency and the
    codebase's own most recent source-of-truth comment agreed. 13px does reach the emitted
    frontmatter as `small: '0.8125rem'`, but as the doc47 named role (`text-small`, 398 uses)
    carried in wholesale with that declared config scale — not as the arbitrary drift value. The
    predicted mechanism did not operate.
  - *The defect, one layer down:* the emitted frontmatter (in `## The DESIGN.md I would have
    written`) ships elections the runner itself judged unsafe, as unqualified law.
    `radius: card: '14px'` was elected "per the rule as stated" at 984 against roughly 751 while
    flagged "exactly the kind of near-tie the skill would want an owner's eyes on"; and
    `radius: sheet: '20px'` ships although `## Step 4` states "I did **not** elect a token for
    this role" and its 12px competitor leads it 68 to 12. Eleven of the twelve `type:` roles
    ship with no individual election work at all. The `### Open` prose under `## Decisions`
    carries the doubt; the machine-readable half of the file has no way to express it.
  - *Fence:* none touches this evidence. The step-3 preamble records that NO-SUBAGENTS does not
    touch Step 4's token-election reasoning and that READ-ONLY touches neither the harvest nor
    the elections. The one fence-adjacent gap — no live owner to confirm names with — makes the
    finding stronger, not weaker: an absent owner is precisely the condition under which the
    rule as written still emitted a complete, unqualified token block.
  - *SPEC §3 amended:* yes, §3.6 — additive marked note only, no approved text rewritten.

  **Prediction 3 — "neither produces a bounded slice" — CONFIRMED for `extracting-design-md` on
  direct evidence; UNOBSERVED for `designing-consistently`, which never exercised the behavior.**

  - *Evidence, `extracting-design-md`:* `baseline-extracting-design-md.md`, `## Step 5` — the
    instruction "Read each surface" met 157 view files, the runner did not read them, and it
    substituted a bound of its own (one structural pattern checked across the 15
    `*DetailView.vue` files), disclosing it as "a deliberate, bounded substitution for 'read
    each surface'". The skill supplied no bounding rule; the runner invented one off script.
  - *Evidence, `designing-consistently`:* `baseline-designing-consistently.md`, `## Step 2` —
    no `DESIGN.md`, therefore no `## Decisions` to slice, "not 'few entries,' literally none".
    The behavior was never exercised. To observe it, the repo would have had to contain a
    `DESIGN.md` with a populated `## Decisions` section. That absence is fence-independent (the
    step-2 preamble lists it among the absence facts; the step-3 runner independently confirmed
    "no existing DESIGN.md anywhere"). A negative compound prediction passes vacuously on this
    half, which is not evidence — so it is recorded unobserved rather than folded into the
    confirmation.
  - *Fence:* one looked linked and is not. NO-SUBAGENTS did block the skill's own
    `## Scaling (bounded)` fan-out lever at roughly ten times its stated ~15-surface threshold,
    and `## Step 5` opens "Same scale problem as Step 2". But fan-out changes *who* reads 157
    surfaces, not *whether* the read is bounded to a principled subset — which is why the
    step-3 preamble's attribution ("does not touch … Step 5's backfill") holds. The
    fence-touched unbounded-harvest observation in `## Step 2` is not relied on here.
  - *Defect in an altered form, reported separately:* the emitted `## Decisions` is not the flat
    per-route section set SPEC §1 describes. It is nine `### <token family or pattern>` prose
    sections covering the whole 157-view app, with zero entries in the dated `- ` bullet form
    the format and the lint require. A later `designing-consistently` step 2 could not select
    "the entries for the surface I am touching" at all — the slice is *undefined*, not merely
    unbounded. The two skills also disagree on addressing: `baseline-designing-consistently.md`
    `## What I would have committed` shows the writer side appending under `### Stock`, a
    per-surface heading the extractor never emits.
  - *SPEC §3 amended:* no. The prediction is not contradicted, and nothing in §3 is contradicted
    by this evidence — §3.3's module architecture is the fix for it.

  **Flagged for the controller, deliberately NOT acted on** (SPEC §4 is the feature list's
  contract and F01-F11 are committed against it, so this step changed no requirement):

  1. **Requirement 1** ends "and does not propose instantiating a DESIGN.md while a system is
     discoverable." Prediction 1's ruling shows today's unfixed skill never reaches a proposal
     at all, so that clause passes vacuously against the *pre-rewrite* skill. An eval written
     only to that clause would not be a test. Steps 5 and 7 need this resolved before the
     discovery eval is written.
  2. **Requirement 5** ends "every unconfirmed token ships `[provisional]`", but SPEC §3.1
     defines that marker only inside `## Decisions` entry text (and SPEC §6's verified
     constraint concerns `- ` lines under `## Decisions`). A frontmatter token has no defined
     way to carry it — and the baseline shows the frontmatter is exactly where unconfirmed
     elections land as law. Requirement 5 left as approved; the gap is recorded in the §3.6 note
     and in ruling 2.
  3. Not a requirement change, but it bears on the PLAN's step-7 / step-8 interface: the
     baselines show the reader and the writer disagree on the Decisions heading shape today
     (`### <surface>` on the write side, `### <token family>` on the emit side). The interface
     note already requires them to agree; the evidence says they start from two different
     shapes, not one.

  **SPEC §3.5 — before and after.** No approved text was rewritten; the amendment is the marked
  note appended after the table.

  Before:

  ```
  | 5 | Verify | unchanged, repointed at `agent-lint` |

  ### 3.6 extracting-design-md — honest output
  ```

  After:

  ```
  | 5 | Verify | unchanged, repointed at `agent-lint` |

  > **Amended at step 4 (2026-08-20) — SPEC §5 prediction 1 contradicted; ruling in `DECISIONS.md`.**
  > The cold baseline (`baseline-designing-consistently.md`, `## Step 1`) shows the run made **no**
  > offer to instantiate. The `Now` cell describes the skill's *text* correctly, but its behavior is
  > worse than the prediction assumed: step 1 dead-ends. The template it names does not exist on
  > this machine (controller addendum 4); neither the read-only fence nor the absent owner accounts
  > for the missing offer (the run recorded four other counterfactual writes and never this one);
  > and step 1 produced neither an offer nor a discovery — the standing system surfaced only at
  > `## Step 2`, as an improvisation the runner flagged as outside what the skill asked for. The
  > `After` column is unchanged: the baseline strengthens it.

  ### 3.6 extracting-design-md — honest output
  ```

  **SPEC §3.6 — before and after.** Same shape: the four approved bullets are untouched, the
  marked note is appended after them.

  Before:

  ```
  - It stops being a prerequisite: `designing-consistently` no longer depends on it having run.

  ## 4. Requirements
  ```

  After:

  ```
  - It stops being a prerequisite: `designing-consistently` no longer depends on it having run.

  > **Amended at step 4 (2026-08-20) — SPEC §5 prediction 2 contradicted; ruling in `DECISIONS.md`.**
  > The cold baseline (`baseline-extracting-design-md.md`, `## Step 4`) shows `13px` was **not**
  > elected: the run declined to elect any token for the 430 arbitrary-pixel type values and carried
  > them to `## Step 5` as an open question. The defect appears one layer down instead, in the
  > emitted frontmatter, which ships elections the runner itself judged unsafe as unqualified law —
  > `radius: card: '14px'` at 984 against roughly 751 while flagged a near-tie needing the owner's
  > eyes, and `radius: sheet: '20px'` although `## Step 4` states "I did **not** elect a token for
  > this role". The bullets above are unchanged and remain the right fix. Open for the controller,
  > not decided here: §3.1 places the `[provisional]` marker inside `## Decisions` entry text only,
  > so a frontmatter token has no defined way to carry it — which is exactly where the baseline
  > shows unconfirmed elections landing. SPEC §4 requirement 5 is left as approved.

  ## 4. Requirements
  ```

  Acceptance: `test $(grep -c '^- 2026-' work/design-skills-at-scale/DECISIONS.md) -ge 3`

  ```
  exit: 0  (count: 12)
  ```

  Recorded honestly, per the standing `DECISIONS.md` ruling on this step: that command could not
  have failed — the file already held 9 dated entries before this step began, so it would have
  passed on an empty step. It is run and reported because the PLAN is binding, not as proof. The
  substantive bar is the three rulings above.

  Re-ran the gate: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/DECISIONS.md` (three rulings appended),
  `work/design-skills-at-scale/SPEC.md` (§3.5 and §3.6 notes, additive only),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 4 fix round 1 (parent ruling + two review Minors). Three changes, all inside
  the stated fence: the two step-4 SPEC notes in §3.5 and §3.6 (my own text, rewritten in place,
  still marked and dated, with the approved table rows and bullets untouched), and three appended
  `DECISIONS.md` entries. No requirement touched, no other SPEC section, no `PLAN.md`, no eval,
  nothing under `skills/`. Read the parent's verbatim ruling under `## Reviews` and both new
  `DECISIONS.md` entries before editing.

  **1. Binding — the parent's ruling on requirement 5's gap; prediction 2 becomes CONFIRMED.**
  The escalation I raised was answered: option A confirmed, with the binding addition that a role
  with no election gets no frontmatter token at all. The parent also rules SPEC §5 prediction 2
  **CONFIRMED**, superseding my CONTRADICTED-on-the-literal-wording verdict. I rewrote the §3.6
  note accordingly: CONFIRMED is now the governing verdict, attributed to the parent and dated;
  the literal facts (`13px` never elected; the frontmatter's `small: '0.8125rem'` is the doc47
  `text-small` role at 398 uses, not the 92-count drift value) sit beneath it as evidence detail
  and are explicitly labelled "not a competing one"; and the open question the note used to pose
  is closed and replaced with the parent's two emission rules, compact enough for steps 6 and 8
  to build against, citing the `DECISIONS.md` ruling. Nothing was deleted or falsified —
  `DECISIONS.md` ruling 2 stays exactly as written, append-only, with the parent's superseding
  ruling recorded below it.

  **2. Minor — the overstatement inside approved SPEC text.** The §3.5 note said the template
  "does not exist on this machine (controller addendum 4)". Addendum 4 verifies something
  narrower: a depth-2 search of the machine's work-repos folder plus three named candidate paths.
  Rephrased to "does not resolve at any plausible sibling location searched (controller addendum
  4; SPEC §1)", the form the skill's pointer actually takes and one SPEC §1 already records as
  verified. The de-confounding rests on the narrower fact and survives unchanged.
  `DECISIONS.md:14` carries the same overstatement and is append-only history — corrected in a
  new entry, not rewritten.

  **3. Minor — the unnamed fourth confound in prediction 1.** `baseline-designing-consistently.md`
  `## Step 1` ends "I was told to proceed through steps 2–5 anyway and record what happens." An
  offer is a halt-and-ask act, so a proceed instruction is a fourth candidate cause for the
  missing offer, distinct from the scope fence, READ-ONLY and the absent owner. Named and
  dismissed in the §3.5 note and in a new `DECISIONS.md` entry, on the three grounds the review
  set out: the runner never cites it, the counterfactual convention it used four times was
  precisely the tool for recording a would-be offer under such an instruction, and the ruling's
  load-bearing half (step 1 produced no *discovery* either) does not depend on it. The note now
  says that last part explicitly. The CONTRADICTED verdict on prediction 1 stands.

  Deferred items were left alone as instructed: the three long `DECISIONS.md` entries were not
  reformatted, and ruling 3's lint claim was not re-litigated or re-verified. The three new
  entries were kept deliberately short so the round does not make that deferred Minor worse —
  measured at 769 / 909 / 794 characters, against the 2061 / 2450 / 2784 of the flagged ones;
  two of the three sit under the file's pre-existing 778-character maximum and the third is
  modestly above it.

  **SPEC §3.5 — before and after.** The `Now`/`After` table above the note is untouched; only my
  step-4 note changed.

  Before:

  ```
  > **Amended at step 4 (2026-08-20) — SPEC §5 prediction 1 contradicted; ruling in `DECISIONS.md`.**
  > The cold baseline (`baseline-designing-consistently.md`, `## Step 1`) shows the run made **no**
  > offer to instantiate. The `Now` cell describes the skill's *text* correctly, but its behavior is
  > worse than the prediction assumed: step 1 dead-ends. The template it names does not exist on
  > this machine (controller addendum 4); neither the read-only fence nor the absent owner accounts
  > for the missing offer (the run recorded four other counterfactual writes and never this one);
  > and step 1 produced neither an offer nor a discovery — the standing system surfaced only at
  > `## Step 2`, as an improvisation the runner flagged as outside what the skill asked for. The
  > `After` column is unchanged: the baseline strengthens it.
  ```

  After:

  ```
  > **Amended at step 4 (2026-08-20) — SPEC §5 prediction 1 contradicted; ruling in `DECISIONS.md`.
  > Revised at step 4 fix round 1 (2026-08-20).** The cold baseline
  > (`baseline-designing-consistently.md`, `## Step 1`) shows the run made **no** offer to
  > instantiate. The `Now` cell describes the skill's *text* correctly, but its behavior is worse
  > than the prediction assumed: step 1 dead-ends. The template it names does not resolve at any
  > plausible sibling location searched (controller addendum 4; SPEC §1); neither the read-only
  > fence nor the absent owner accounts for the missing offer (the run recorded four other
  > counterfactual writes and never this one); nor does the fourth candidate cause, the runner's
  > instruction to "proceed through steps 2–5 anyway" (same section), which the runner never cites
  > as its reason and which that same counterfactual convention existed to work around; and step 1
  > produced neither an offer nor a discovery — the standing system surfaced only at `## Step 2`, as
  > an improvisation the runner flagged as outside what the skill asked for. That second half, the
  > missing discovery, depends on none of the four causes. The `After` column is unchanged: the
  > baseline strengthens it.
  ```

  **SPEC §3.6 — before and after.** The four approved bullets above the note are untouched; only
  my step-4 note changed.

  Before:

  ```
  > **Amended at step 4 (2026-08-20) — SPEC §5 prediction 2 contradicted; ruling in `DECISIONS.md`.**
  > The cold baseline (`baseline-extracting-design-md.md`, `## Step 4`) shows `13px` was **not**
  > elected: the run declined to elect any token for the 430 arbitrary-pixel type values and carried
  > them to `## Step 5` as an open question. The defect appears one layer down instead, in the
  > emitted frontmatter, which ships elections the runner itself judged unsafe as unqualified law —
  > `radius: card: '14px'` at 984 against roughly 751 while flagged a near-tie needing the owner's
  > eyes, and `radius: sheet: '20px'` although `## Step 4` states "I did **not** elect a token for
  > this role". The bullets above are unchanged and remain the right fix. Open for the controller,
  > not decided here: §3.1 places the `[provisional]` marker inside `## Decisions` entry text only,
  > so a frontmatter token has no defined way to carry it — which is exactly where the baseline
  > shows unconfirmed elections landing. SPEC §4 requirement 5 is left as approved.
  ```

  After:

  ```
  > **Amended at step 4 (2026-08-20) — SPEC §5 prediction 2 CONFIRMED, per the parent's ruling in
  > `DECISIONS.md`; note rewritten at step 4 fix round 1 (2026-08-20).** Election ran on frequency
  > and landed unconfirmed values as law. In `baseline-extracting-design-md.md` `## Step 4`,
  > `radius: card: '14px'` was elected on a 984-against-roughly-751 count the run itself flagged as
  > a near-tie needing the owner's eyes, and `radius: sheet: '20px'` shipped in the emitted
  > frontmatter for a role the run states verbatim it "did **not** elect". Evidence detail beneath
  > that verdict, not a competing one: `13px` itself was never elected, the run having declined to
  > elect any token for the 430 arbitrary-pixel type values, and the `small: '0.8125rem'` in that
  > frontmatter is the doc47 `text-small` role (398 uses), not the 92-count drift value.
  >
  > **Decided by the parent, binding on steps 6 and 8** (ruling in `DECISIONS.md`; SPEC §4
  > requirement 5 unchanged): a role with a genuine but unconfirmed election puts its token in
  > frontmatter, which is the compile source, **and** gets a `[provisional]` entry under
  > `### Global` / `#### Sistema` naming the token and what it beat, carrying both counts — "an
  > entry that says only `[provisional] card is 14px` is not enough." A role with **no** election
  > gets **no** frontmatter token at all, plus an open-question entry recording the role, the
  > competing candidates and their file locations. Marking something provisional does not license
  > inventing it.
  ```

  **Verdict state after this round**, for the record: prediction 1 CONTRADICTED (unchanged, now
  with the fourth confound named and dismissed); prediction 2 **CONFIRMED** (parent's ruling,
  superseding this step's literal-wording verdict, which is retained as evidence); prediction 3
  CONFIRMED for `extracting-design-md` and UNOBSERVED for `designing-consistently` (unchanged).

  Acceptance: `test $(grep -c '^- 2026-' work/design-skills-at-scale/DECISIONS.md) -ge 3`

  ```
  exit: 0  (count: 17)
  ```

  Same caveat as the original entry: that command cannot fail and is reported because the PLAN is
  binding, not as proof.

  Re-ran the gate: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/SPEC.md` (§3.5 and §3.6 notes only),
  `work/design-skills-at-scale/DECISIONS.md` (three entries appended),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 4 fix round 1, second pass (fresh seat). This seat was dispatched the round
  above as unfinished. It was not: the previous seat completed and committed it as `6798ec7`
  while this seat was reading the lane. So this pass verified that commit against the three
  chartered changes rather than redoing them. Change 2 (the addendum-4 overstatement) and change
  3 (prediction 1's fourth confound) landed exactly as the review asked; `DECISIONS.md` took
  three appended entries and zero deletions (`git diff --stat`: 3 insertions, 0 deletions); no
  requirement, no other SPEC section, no `PLAN.md`, no eval and nothing under `skills/` was
  touched. One gap remained, so this pass makes a single additive edit to the §3.6 note and
  nothing else.

  **The gap.** The dispatch required the rewritten §3.6 note to say plainly that step 4's own
  CONTRADICTED verdict happened and was superseded — "Say that plainly rather than pretending the
  earlier verdict never happened" — and that the two readings are not in factual conflict. The
  committed note labels the literal facts "not a competing one" but never states the superseded
  verdict inside the SPEC at all: a reader of §3.6 alone sees only CONFIRMED and must reach
  `DECISIONS.md` ruling 18 to learn an earlier verdict ever existed. Two evidence specifics the
  dispatch named were also thinned out — that the run *carried the 430 values to its Step 5 as an
  open question*, and that the 13px-shaped `small: '0.8125rem'` was *carried in as a block*
  rather than individually elected. Both are verified in the baseline:
  `baseline-extracting-design-md.md:466-474` ("I did **not** elect either the doc47 scale or the
  `--nl-t-*` scale … I've carried it into Step 5 as an open"), and `:787-799` under
  `## The DESIGN.md I would have written`, where all twelve doc47 `type:` roles ship together as
  one block.

  **SPEC §3.6 — before and after.** Only the evidence-detail sentence inside the previous seat's
  note changed. The note's CONFIRMED header, the parent's two emission rules, and every approved
  bullet above the note are untouched.

  Before:

  ```
  > that verdict, not a competing one: `13px` itself was never elected, the run having declined to
  > elect any token for the 430 arbitrary-pixel type values, and the `small: '0.8125rem'` in that
  > frontmatter is the doc47 `text-small` role (398 uses), not the 92-count drift value.
  ```

  After:

  ```
  > that verdict, not a competing one: `13px` itself was never elected — the run declined to elect
  > any token for the 430 arbitrary-pixel type values and carried them to its `## Step 5` as an open
  > question — and the 13px-shaped `small: '0.8125rem'` in the emitted frontmatter is the doc47
  > `text-small` role (398 uses) carried in as a block with the other eleven, not the 92-count drift
  > value. Step 4 first recorded prediction 2 CONTRADICTED on that literal reading; the parent's
  > verdict supersedes it, and nothing is falsified either way. The two agree on every fact and
  > differ only on what prediction 2 asserts: step 4 read SPEC §5's literal sentence, the parent
  > reads the defect that sentence was reaching for, and the same transcript supports both readings.
  ```

  **SPEC §3.5 — unchanged in this pass**, verified as committed and quoted before/after in the
  entry above.

  No `DECISIONS.md` entry is appended for this pass: it records no new ruling, only restores
  wording the dispatch already specified for a ruling that entries 18 and 21 both already carry.
  The deferred items stayed untouched as instructed — the three long entries were not reformatted,
  and ruling 3's lint claim was neither re-litigated nor re-verified.

  Acceptance: `test $(grep -c '^- 2026-' work/design-skills-at-scale/DECISIONS.md) -ge 3`

  ```
  exit: 0  (count: 17)
  ```

  Unchanged caveat, per the standing `DECISIONS.md` ruling on this step: that command cannot fail
  and is reported because the PLAN is binding, not as proof.

  Gate: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  ```

  Files changed: `work/design-skills-at-scale/SPEC.md` (§3.6 note, one sentence expanded),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 5 (evals for `designing-consistently`). Wrote the two new evals and rewrote
  `eval-01.md`, from `baseline-designing-consistently.md` and the step-4 rulings in
  `DECISIONS.md` — not from PLAN step 5's one-line descriptions and not from SPEC §3's prose
  (§3.5 and §3.6 read with their dated step-4 amendment notes). Touched no `SKILL.md`, no
  `SPEC.md`, no `PLAN.md`, no `feature_list.json`, and made zero writes to the Pegasuz checkout
  (did not open it at all this step). `eval-02.md` and `eval-03.md` were left byte-untouched —
  see the flag at the end.

  Per the controller ruling in `DECISIONS.md` (every step-5/6 eval carries at least one
  expected-behavior box that demonstrably FAILS against the skill as it stands today, and the
  author names it), each file carries a `## Why this is RED today` section naming its box and
  citing the baseline. All three follow the existing eval shape — `# Eval NN: <title>`,
  `## Query` in the owner's register, `## Fixture`, `## Expected behavior` as a checklist — with
  the RED section appended after the checklist. Fixtures are runtime-neutral per the fixtures
  ruling: an abstract large multi-module admin carrying the baselines' real numbers (157
  surfaces, 13 nav groups, 175 components, 83 `--<prefix>-*` custom properties, an ~82 KB prose
  doc of unknown authorship, 4-of-15 shared-component coverage). No framework, product or repo
  filename appears in any of the three — verified mechanically with one `grep -nE` over the
  three files for drive-rooted / `/mnt/` / `~/` / `/home/` / `/Users/` paths and for the names
  the fixtures ruling bars; no hits (exit 1). No `## Validation log` section was added to any of
  them: there is no validation run to log yet.

  **`skills/designing-consistently/evals/eval-04.md` (new) — discovery when no DESIGN.md
  exists.** RED box: **box 1**, "Step 1 returns a ranked inventory of what already governs …
  instead of stopping at 'no DESIGN.md'". Baseline evidence:
  `baseline-designing-consistently.md` `## Step 1` records six searches for a DESIGN.md-shaped
  file, zero hits, and a dead end — no inventory, no ranking, nothing carried forward; the
  standing system (the token line the app's own `AGENTS.md` names, the 288-line stylesheet
  defining it, the inline dated owner decisions with names and ticket ids) surfaces only at
  `## Step 2`, where the runner labels it "Improvisation (flagged — the skill did not tell me to
  do this)". Boxes 2-4 fall with it: `provisional`, `reference surface`, `rank` and `trust` each
  occur **zero** times in today's `SKILL.md` (counted), so no source is ranked against another,
  and `## Step 3` of the same baseline shows the shared-component trap live — the run reused a
  shared component because it existed and was commented as shared, checking no sibling surface
  for how widely it is actually used. Box 5 (no instantiation proposal while a system is
  discoverable) is marked **inside the file** as a regression guard rather than this eval's RED,
  per the controller's ruling answering step 4's flagged item 1: today's skill reaches no
  proposal at all, so that box passes trivially against the unfixed skill.

  **`skills/designing-consistently/evals/eval-05.md` (new) — bounded slice at 157 surfaces.**
  RED box: **box 1**, "`### Global` is read first — both sub-blocks — before anything
  module-specific". Today's step 2 reads "every entry under the surfaces (routes/screens) about
  to be touched" — the touched surface and nothing else — and `Global` and `sibling` occur
  **zero** times in today's `SKILL.md` (counted), so there is no global tier to read first and
  no sibling rule to bound the read with. Box 2 (module section = touched page + 2-3 siblings)
  fails with it, and box 4 falls behind them: the fixture's touched page carries 3 entries, none
  about a header, so a page-only read ships a header contradicting a standing global. The
  baseline position is stated honestly inside the file rather than overclaimed: the cold run
  could not exercise this — `baseline-designing-consistently.md` `## Step 2` had "zero material
  to read from its prescribed source — not 'few entries,' literally none", and the step-4 ruling
  records the slicing behavior **unobserved** for this skill for exactly that reason. The
  nearest direct observation is the lane's other baseline, `baseline-extracting-design-md.md`
  `## Step 5`, where the same "read each surface" shape met these 157 surfaces, the run read
  none of them, and it substituted a bound of its own off script ("a deliberate, bounded
  substitution"). Box 1 needs neither run to settle it: a box requiring `### Global` to be read
  cannot pass against a skill that has no such tier. The eval also fails an answer that reads
  everything (box 3), which is the other half of PLAN step 5's brief.

  **`skills/designing-consistently/evals/eval-01.md` (rewritten) — the repair gate: promote,
  demote, escalate.** The old content (a standing decision surviving an edit, under a flat
  `### <surface>` heading) was the pre-lane flat model; the new one exercises the repair half of
  SPEC §3.5 step 4, with §3.1's promotion rule and §3.4's precedence rule. RED boxes: **boxes 1
  and 2** — promote the `[provisional]` entry the work was built on, and demote the
  `[provisional]` entry it contradicted (replacing it with the corrected entry plus one line
  saying what beat it). Baseline evidence: `baseline-designing-consistently.md` `## Step 4` —
  the run's would-be record is three flat one-line additions under a single `### <surface>`
  heading, and no existing entry is revisited, re-dated or re-scoped anywhere in the run;
  today's step 4 is additive by construction ("Each gets `- YYYY-MM-DD — <decision>` under its
  `### <surface>`"), and `promote`, `demote`, `provisional`, `escalat` and `Global` each occur
  **zero** times in `SKILL.md` (counted). Box 2 fails in the opposite direction as well: step 3
  makes any conflict a stop-and-ask ("A standing decision the work conflicts with is
  renegotiated with the user — never silently overridden") with no exemption for a candidate, so
  a faithful run halts on the provisional entry instead of demoting it. Boxes 3 and 4
  (undeclared contradiction is drift; two-module escalation keeps `[provisional]` status) fail
  behind them — the skill has no global tier to contradict or escalate into. Stated in the file,
  not hidden: that cold run found no `DESIGN.md`, so no provisional entry existed for it to
  repair — the additive-only shape of its record is what was observed, and the absence of every
  repair verb from the skill text is what makes boxes 1-2 unreachable today. The rewrite also
  covers `feature_list.json` F07's expectation that `eval-01.md` exercise the
  undeclared-contradiction-is-drift case (box 3).

  **Flagged, deliberately NOT acted on — `eval-03.md`.** Its second box hard-codes the flat
  `### catalogo/inventario` heading ("Appends `- YYYY-MM-DD — <decision>` … under
  `### catalogo/inventario`"), which is the pre-lane addressing shape. SPEC §3.3/§4.6 replace it
  with `### <module>` / `#### <route> — <page>`, and the step-4 ruling on prediction 3 sharpens
  the point directly: it records that the writer side appends under a per-surface heading "the
  extractor never emits", and that the PLAN's step-7 / step-8 interface requires reader and
  writer to agree on the exact heading shape. So a skill rewritten at step 7 to SPEC §3.3 would
  fail `eval-03.md` box 2 as literally written. I did not rewrite it — it is another
  requirement's contract and this step was not chartered to touch it. The controller rules.
  `eval-02.md` is unaffected: SPEC §3.5 leaves step 3 unchanged, and nothing in the step-4
  rulings touches consuming generated tokens.

  Acceptance 1: `test $(ls skills/designing-consistently/evals/*.md | wc -l) -eq 5`

  ```
  === ACCEPT 1 ===
  5
  ACCEPT 1 PASS (exit 0)
  ```

  Acceptance 2 (path resolved per `DECISIONS.md` ruling 1):
  `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  lint exit: 0
  ```

  Only the pre-existing, expected LOW `cmd-drift` finding remains; the three new/rewritten eval
  files add no finding of their own — in particular no `machine-path`, the check that scans
  `skills/`.

  Files changed: `skills/designing-consistently/evals/eval-04.md` (new),
  `skills/designing-consistently/evals/eval-05.md` (new),
  `skills/designing-consistently/evals/eval-01.md` (rewritten),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 5 fix round 1 (the eval-03 ruling + four Minors). Five edits across four
  eval files, all inside the stated fence: no `SKILL.md`, no `eval-02.md`, no `SPEC.md`, no
  `PLAN.md`, no `feature_list.json`, zero writes to the Pegasuz checkout. Those four eval files
  are the only files under `skills/` this round changed — see the commit-history flag below for
  where they landed.

  **1. The ruling on the flagged item — `eval-03.md` box 2 and its fixture line.** Rewritten to
  SPEC §3.3's architecture: the fixture now designs the empty state for "the inventory page of
  the `catalogo` module — `### catalogo` → `#### /catalogo/inventario — Inventario`", and box 2
  appends the dated line "under `### catalogo` → `#### /catalogo/inventario — Inventario`". The
  eval's subject is untouched: it still tests that an unrecorded decision blocks completion,
  same query, same three boxes in the same order, boxes 1 and 3 byte-identical. Only the heading
  shape it addresses changed — the correction the ruling asked for, not a repurposing.
  `eval-02.md` was not touched.

  **2. `eval-01.md` box 1 — promotion channel made explicit.** Added: "Promotion here is the
  earned-by-work channel, so no reference surface and no owner reply is needed for it; needing a
  reference surface or the owner to confirm governs **discovery**, which is a different
  channel." Written self-containedly rather than as a "§3.1 / §3.2" citation, since the eval
  ships in `skills/` where a lane-file section number resolves to nothing; the resolution the
  reviewer asked for is stated in full, so a step-7 author does not have to re-derive it.

  **3. `eval-01.md` box 2 — the two-dispositions ambiguity closed.** Added: "The corrected entry
  may be its removal, recorded — the confirmed global already covers the surface, and a
  provisional entry is exempt from never-silently-drop", and a closing clause, "This box and the
  next describe one disposition of that entry, not two." A step-7 rewrite that deletes the
  redundant module entry and records why now passes box 2 instead of failing it on wording.

  **4. `eval-04.md` box 3 — aligned with box 4 and with the fixture.** "and the owner is asked
  which surfaces are the reference" became "and which surfaces are the reference is recorded as
  an open question for the owner", which is box 4's disposition and is consistent with the
  fixture's "the owner is not available mid-run".

  **5. `eval-05.md` box 5 — its vacuity disclosed in the file.** Added to the box: "This box is
  a second-order trap for the read-everything answer, not an independent test — a run that
  correctly holds to the slice never reads that module, so it passes here by never seeing the
  entry. Read a pass on this box together with box 3, never as evidence on its own." One
  sentence was also added to that eval's `## Why this is RED today` recording that box 5 is not
  part of its RED, so the two statements cannot drift apart.

  **Re-checked every `## Why this is RED today` claim after the edits, as instructed.** The
  three RED sections still name the same boxes and every claim in them still holds: `provisional`,
  `promote`, `demote`, `escalat`, `Global`, `sibling`, `reference surface`, `rank` and `trust`
  all still occur **zero** times in `skills/designing-consistently/SKILL.md` (re-counted this
  round — the file was not touched). eval-01's RED still names boxes 1-2 and both still fail
  today: box 1's new clause only settles *which* channel promotes, and today's skill has no
  promotion of any kind; box 2's new "removal, recorded" alternative is still a repair move that
  today's additive-only step 4 never makes, and step 3's stop-and-ask still halts a faithful run
  on the provisional entry either way. eval-04's RED still names box 1, and its "boxes 2-4 fall
  with it" still holds — box 3's rewording changed the disposition, not the fact that nothing in
  today's skill ranks a source or marks anything provisional. eval-05's RED still names box 1,
  now with box 5 explicitly excluded from it.

  **Deferred, per the fix dispatch — recorded so work-verify's triage sees them:** Minors 1, 2,
  5, 7 and 9, all readability / box-independence polish, none verdict-changing —
  (1) `eval-01.md`'s forward reference to the admin fixture introduced in `eval-04.md`;
  (2) the implicit link between the fixture's "existing card radius" and the 14px provisional;
  (5) `eval-01.md` box 5 largely collapsing into boxes 1-2; (7) `eval-04.md` boxes 1 and 2
  overlapping on "ranked"; (9) `eval-05.md`'s literal `<why>` placeholder in the fixture. Not
  acted on this round.

  **One thing to flag about the commit history of this round.** The four eval edits are already
  in the tree under commit `eda82ca` ("docs(lane): record the step-5 review verdict verbatim"),
  not under a commit of mine: that commit was made from another seat while this round's edits
  were on disk, and it swept them in alongside the review verdict it was written for. **Said
  plainly, so a later reader is not misled: `eda82ca`'s message describes a PROGRESS-only
  bookkeeping entry and says nothing about eval content, but the commit contains this round's
  changes to `eval-01.md`, `eval-03.md`, `eval-04.md` and `eval-05.md`.** The controller has
  since given the cause directly — a `git add -A` run for that bookkeeping entry while this
  seat's edits sat uncommitted in the shared worktree — and rules that history stays as it is.
  I did not rewrite it — the content is correct and committed, and rewriting another seat's
  commit is not a call this step makes. Verified by
  `git diff 2543242 HEAD -- skills/designing-consistently/evals/`,
  which shows exactly the five edits described above and nothing else. This entry is therefore
  the only file in my own fix-round commit.

  Acceptance 1: `test $(ls skills/designing-consistently/evals/*.md | wc -l) -eq 5`

  ```
  === ACCEPT 1 ===
  5
  ACCEPT 1 PASS (exit 0)
  ```

  Acceptance 2: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  lint exit: 0
  ```

  Re-ran the runtime-neutrality check over all four touched eval files (drive-rooted, `/mnt/`,
  `~/`, `/home/`, `/Users/` paths plus the framework/product/filename names the fixtures ruling
  bars): no hits, exit 1. Only the pre-existing expected LOW `cmd-drift` finding remains.

  Files changed: `skills/designing-consistently/evals/eval-01.md`,
  `skills/designing-consistently/evals/eval-03.md`,
  `skills/designing-consistently/evals/eval-04.md`,
  `skills/designing-consistently/evals/eval-05.md` (all four committed inside `eda82ca`, see
  the flag above), `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 5 fix round 1, second pass (audit after the controller's `git add -A`
  notice). The controller reported that its `git add -A` for a PROGRESS-only bookkeeping entry
  swept this seat's in-flight eval edits into `eda82ca`, and asked which of the five chartered
  items were already applied there and which still needed work. **All five were already
  applied**; nothing in the eval files remained to change, so this pass wrote no eval content
  and made no empty commit. Audited each item against the files as they stand on disk, not
  against my own report of them:

  1. **`eval-03.md` heading ruling — applied.** Fixture reads "the inventory page of the
     `catalogo` module — `### catalogo` → `#### /catalogo/inventario — Inventario`"; box 2
     appends under `### catalogo` → `#### /catalogo/inventario — Inventario`. Boxes 1 and 3 and
     the query are byte-identical to the pre-round file, so the eval still tests exactly what it
     tested: an unrecorded decision blocks completion.
  2. **Minor 3, `eval-01.md` box 1 — applied.** Carries the earned-by-work promotion clause and
     the statement that reference-surface/owner confirmation governs discovery instead.
  3. **Minor 4, `eval-01.md` box 2 — applied.** Carries both the "corrected entry may be its
     removal, recorded" alternative and the closing "This box and the next describe one
     disposition of that entry, not two."
  4. **Minor 6, `eval-04.md` box 3 — applied.** Reads "which surfaces are the reference is
     recorded as an open question for the owner", matching box 4 and the fixture's absent owner.
  5. **Minor 8, `eval-05.md` box 5 — applied**, with the matching sentence in that eval's
     `## Why this is RED today`.

  Also re-verified nothing outside the fence moved: `git diff 2543242 HEAD --stat --
  skills/designing-consistently/evals/eval-02.md skills/designing-consistently/SKILL.md` is
  empty — `eval-02.md` and the skill are untouched since my step-5 commit.

  **Re-checked every `## Why this is RED today` claim again this pass**, against the current
  `SKILL.md` rather than against the earlier count: `provisional`, `promote`, `demote`,
  `escalat`, `Global`, `sibling`, `reference surface`, `rank` and `trust` all still occur zero
  times, and the three verbatim quotes the RED sections lean on are still present and still say
  what is claimed — `SKILL.md:32` ("every entry under the surfaces (routes/screens)"),
  `SKILL.md:42` ("renegotiated with the user — never silently") and `SKILL.md:49` ("Each gets").
  Every RED section still names a box that genuinely fails today.

  **The mis-attribution, stated plainly here as well as in the entry above:** `eda82ca`'s commit
  message ("docs(lane): record the step-5 review verdict verbatim") describes a PROGRESS-only
  bookkeeping entry and says nothing about eval content, yet that commit carries this round's
  changes to `eval-01.md`, `eval-03.md`, `eval-04.md` and `eval-05.md`. The controller has given
  the cause — its own `git add -A` while this seat's edits sat uncommitted in the shared
  worktree — and ruled that history stays as it is: no revert, reset or amend, since other work
  shares this repository. A later reader looking for where the fix-round eval content landed
  should look at `eda82ca`, not at a commit of mine.

  Acceptance 1: `test $(ls skills/designing-consistently/evals/*.md | wc -l) -eq 5`

  ```
  === ACCEPT 1 ===
  5
  ACCEPT 1 PASS (exit 0)
  ```

  Acceptance 2: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  lint exit: 0
  ```

  Files changed: `work/design-skills-at-scale/PROGRESS.md` only (this entry, plus the plain
  mis-attribution sentence added to the fix-round entry above). No eval file, no `SKILL.md`, no
  `SPEC.md`, no `PLAN.md`, no `feature_list.json`; zero writes to the Pegasuz checkout.

- 2026-08-20 — Step 6 (evals for `extracting-design-md`). Wrote the two new evals and rewrote
  `eval-01.md`, from `baseline-extracting-design-md.md` read in full plus the step-4 rulings in
  `DECISIONS.md` and SPEC §3.6's step-4 amendment note — not from PLAN's descriptions of the
  failures, per the PLAN's own interface note. Read step 5's five `designing-consistently` evals
  first and followed their shape (`## Why this is RED today`, honest marking of boxes that are
  guards rather than RED). Touched no `SKILL.md`, no `SPEC.md`, no `PLAN.md`, no
  `feature_list.json`; made zero writes to the Pegasuz checkout (I read nothing from it this
  step — every number below comes from the lane's baseline file). Dispatched no subagent.

  **`skills/extracting-design-md/evals/eval-01.md` — rewritten.** Retitled "drift report —
  counts measure, they do not elect". The old box 2 ("The proposed token set collapses the
  variants (fewer gray tokens than raw gray values)") was frequency-shaped and is replaced by
  three boxes: counts measure spread and never elect (box 3); the collapse is still proposed,
  with every proposed token carrying `[provisional]` plus the count of what it beat and the
  missing reference-surface designation recorded as the question that would settle them (box 4);
  a losing variant with a written decision behind it is a competing claim, not migration debt
  (box 5). Boxes 1 and 2 (counts + file:line evidence, ordering by spread) are the old boxes 1
  and 3, kept verbatim in substance. Fixture rebuilt on the baseline's numbers (2009/2308/6821
  grays across 49/39 files, one file mixing two scales, 43 raw hex over 21 distinct values;
  1832/340/32 control radius) and made runtime-neutral — the old fixture named Tailwind, which
  the controller's runtime-neutral-fixtures ruling forbids in `skills/`.

  **RED: boxes 3, 4 and 5.** Evidence: today's step 4 is the whole criterion ("Semantic role +
  frequency decide: the dominant or correct variant becomes the token"); in
  `skills/extracting-design-md/SKILL.md` `provisional` and `tie-break` occur **0** times each
  and the file's single `reference` is the path `reference/design-md.md` at line 13 (counted,
  not asserted) — no criterion to elect on and no marker to carry doubt. Direct observation:
  baseline `## Step 4` elected the 984-use card value over a ~751-use family "per the rule as
  stated" while recording the loser "wasn't sloppy drift, it was a considered decision". Boxes
  1 and 2 pass today (baseline `## Step 2`'s counts with file:line samples; `## Step 3` opens
  "Ordering by spread (broadest first)") and are kept so the step-8 rewrite cannot answer "no
  reference surface, so no numbers".

  **`skills/extracting-design-md/evals/eval-05.md` — new: reference surfaces elect, frequency
  only breaks ties.** Built on the fixture the step-4 reviewer asked be carried here: the card
  role at 984 against an ~751 8px family that was a deliberate, documented decision with a
  two-paragraph written rationale, later overridden by a newer self-declared-current document
  without the migration completing. The owner's query designates two modules as the reference
  surfaces and every card inside them is 8px, so reference and frequency point opposite ways.
  The opposite-directions case is exercised too, as instructed: the modal/drawer role, where the
  reference modules render no modal, the code says 12px at 68 uses and the newer document says
  20px at 12 — >5:1 against the document that calls itself current.

  **RED: box 1** (the card token elected from the reference surfaces at 8px despite losing 984
  to ~751), with box 2 falling with it and box 3 alongside them. Evidence: `reference surface`
  occurs **0** times in today's `SKILL.md`, `tie-break` **0**, `provisional` **0**; a box
  requiring the election to come from reference surfaces cannot pass against a skill with no
  such concept. Direct observation: baseline `## Step 4`'s card role, elected on count while the
  run itself called it "exactly the kind of near-tie the skill would want an owner's eyes on".
  Box 3's RED is separate and textual: the rule that run actually applied was "(a) the largest
  live occurrence count, and (b) alignment with the codebase's own most recent self-declared
  'source of truth' comment" — the newest document is half of today's decision rule, which is
  exactly the authority box 3 denies it. **Stated as NOT the RED, in the eval itself:** box 4
  (the modal role goes unelected) — the cold run reached that same disposition at the election
  stage ("I did **not** elect a token for this role"), so a faithful run of today's skill passes
  it there; it fails one step later, in what got emitted, and that is `eval-06`'s RED. Box 5
  (frequency still settles the pure naming split, 447/424/22) is a guard against overcorrection
  whose first half is already GREEN; only its `[provisional]` clause fails today.

  **`skills/extracting-design-md/evals/eval-06.md` — new: provisional by default, and no token
  without an election.** Encodes SPEC §3.6's amendment note (the parent's binding ruling) in its
  two halves rather than paraphrasing it: boxes 1 and 2 are the role WITH a genuine but
  unconfirmed election — control radius, 1832 against the older document's 340 — whose token
  goes into frontmatter (the compile source) **and** gets a `[provisional]` entry under
  `### Global` / `#### Sistema` naming the token and what it beat **with both counts**, with the
  note's own "an entry that says only … is not enough" written into the box as a failure
  condition. Boxes 3 and 4 are the role with NO election — modal/drawer, 68 against 12 — which
  gets **no** frontmatter token at all plus an open-question entry carrying the role, both
  candidates and their locations. Box 5 covers the wholesale carry-in (a config that declares
  itself current is a candidate, not a reference surface) and box 6 the note's "mostly
  provisional is the correct output" clause.

  **RED: box 3.** Evidence, the baseline's own words: the emitted frontmatter under `## The
  DESIGN.md I would have written` ships `radius: sheet: '20px'` unqualified, while `## Step 4`
  of the same run states verbatim "I did **not** elect a token for this role" about that exact
  role and records the code running 68 to 12 against it. Box 2 fails with it: `provisional`,
  `Global` and `Sistema` occur **0** times each in today's `SKILL.md`, and the emitted
  `## Decisions` carries its doubt in nine `###` prose sections (six headed `### Open —`) with
  zero entries in the dated `- YYYY-MM-DD — ` form and no `### Global` heading anywhere —
  verified by slicing that section out of the baseline and grepping it. Boxes 5 and 6 fail too
  (all twelve evidenced type roles shipped as plain values, eleven with no election work of
  their own; nothing in the emitted file is marked provisional). **Stated as NOT the RED, in the
  eval itself:** box 1 (the token does reach frontmatter) passes today and is kept deliberately,
  because holding unconfirmed tokens out of frontmatter is option C in the parent's ruling,
  rejected there. Box 4 largely passes too — today's step 5 already says to record a
  contradicted pattern "as an open question with both variants and their locations" — and its
  force is standing next to box 3: that run recorded the open question **and** shipped the token
  anyway. Box 5's last clause (the zero-use thirteenth declared role gets no token) is also
  already GREEN in the baseline.

  **Files I was told to leave alone, and the flags on them** (not rewritten — the step-5
  precedent and the controller's own ruling on `eval-03` are why a flag beats a quiet fix):

  1. **`eval-02.md` is now under-specified, though a correct step-8 rewrite still passes it.**
     Box 1 says the consistent back button "becomes a dated `## Decisions` entry under its
     surfaces, citing the evidence". Under SPEC §3.2 that pattern is evidenced only in
     non-reference code, so the correct emission is a `[provisional]` entry — but the box does
     not require the marker, so a rewrite emitting it confirmed also passes. It is not false;
     it just stopped testing the thing that now matters. Flagged for the controller, left
     untouched.
  2. **`eval-04.md`'s fixture names framework-specific constructs** (`globals.css`, `@theme`,
     `[data-theme]`), which the controller's runtime-neutral-fixtures ruling would not permit in
     a fixture written today. That same ruling preserves this file's `## Validation log` naming
     a real repo and PR, and the file was explicitly out of my scope. Left untouched; flagged
     only.
  3. **`eval-03.md`'s stale `Context-Engineering` / `context-lint` references left in place** —
     PLAN step 9's `[batch]` sweep owns that file, per the dispatch. Not touched.

  I added no `## Validation log` to any file: I ran no validation, and the precedent permits one
  only where a run was actually logged.

  Acceptance 1: `test $(ls skills/extracting-design-md/evals/*.md | wc -l) -eq 6`

  ```
  $ test $(ls skills/extracting-design-md/evals/*.md | wc -l) -eq 6
  exit: 0
  ```

  Acceptance 2: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  exit: 0
  ```

  Also checked before committing: no line in the three files exceeds 76 columns, and a grep for
  framework/product/filename identities and machine-anchored path forms (drive-rooted, POSIX
  home, WSL mount) across all three returns nothing.

  Files changed: `skills/extracting-design-md/evals/eval-01.md` (rewritten),
  `skills/extracting-design-md/evals/eval-05.md` (new),
  `skills/extracting-design-md/evals/eval-06.md` (new),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 6 fix round 1 (the `eval-02.md` ruling). One file edited,
  `skills/extracting-design-md/evals/eval-02.md`, plus this entry. No other eval, no `SKILL.md`,
  no `SPEC.md`, no `PLAN.md`, no `feature_list.json`; zero writes to the Pegasuz checkout.

  **Both defects fixed in one edit, as ruled.** Box 1 read "The consistent back button becomes a
  dated `## Decisions` entry under its surfaces, citing the evidence (files where it appears)."
  It now requires a dated **`[provisional]`** entry under **`### <module>` → `#### <route> —
  <page>`** for each surface it covers, still citing the files, with both halves named as
  required and the failure condition spelled out: "An entry recorded as settled fails this box
  however well it cites the three files." Boxes 2 and 3 are untouched, and the eval's subject —
  record what the code evidences, surface what it contradicts, invent nothing — is unchanged.

  **Two supporting changes inside the same file, both needed to make box 1 decidable**, reported
  rather than slipped in:
  1. The fixture now says nobody has designated a reference surface and the owner is not
     available mid-run. Without that, "ships `[provisional]`" is not decidable — a fixture where
     the three views *were* the reference surfaces would correctly emit the entry confirmed.
  2. The three detail views are now stated to be **in the same module**. Otherwise SPEC §3.4's
     two-or-more-modules escalation trigger fires and a correct answer could legitimately put
     the entry under `### Global` / `#### Patrones` instead of the module/route address box 1
     asks for — an ambiguity that would make the box unfair. This eval was never about
     escalation; the sibling `designing-consistently/evals/eval-01.md` owns that.
  3. Added a `## Why this is RED today` section, matching the other three files this step
     touched and the standing bar that a rewritten box must fail against today's skill.

  **The re-check that was asked for: yes, box 1 still fails a wrong answer.** Walked four
  candidate step-8 answers against the rewritten box:
  - confirmed entry, correct `### <module>` / `#### <route> — <page>` address → **FAILS** (the
    marker half). This is the exact case the ruling asked about.
  - `[provisional]` entry under a flat `### <surface>` heading → **FAILS** (the addressing
    half).
  - `[provisional]` entry under the module/route address, citing the three files → **PASSES**.
  - `[provisional]` entry, correct address, no evidence cited → **FAILS** (the citation clause,
    which is the original box's requirement and survives unchanged).
  Also checked the box is satisfiable at all, not just strict: SPEC §3.1's own example
  (`- 2026-08-20 — [provisional] cards use radius 14px; …`) puts the marker after the date, so a
  passing entry keeps the lint's `- YYYY-MM-DD — ` prefix intact — verified against `SPEC.md:76`
  rather than assumed.

  **RED evidence for the rewritten box**, recorded in the file: today's step 5 says the pattern
  "becomes a dated entry under its `### <surface>`" — the flat addressing SPEC §3.3 replaces,
  with no marker — and `provisional` occurs **0** times in today's `SKILL.md`. In
  `baseline-extracting-design-md.md` the emitted `## Decisions` is nine `###` prose sections
  addressed by token family or pattern, with no `### <module>` heading and no
  `#### <route> — <page>` subsection anywhere, and the two entries it recorded as decided carry
  no marker in a run where no reference surface was ever designated. Boxes 2 and 3 pass today
  (that run did record its contradicted pattern as an open question with both variants and their
  file locations, and invented nothing) and are stated as passing in the file.

  **Flags 2 and 3, per the rulings:** nothing touched under either. `eval-03.md` was not opened
  — the ruling on it is withdrawn, my flag having mis-attributed to it a heading that belongs to
  `skills/designing-consistently/evals/eval-03.md`, and its stale `context-lint` reference stays
  for PLAN step 9. `eval-04.md` is left as it stands, its fixture deferred rather than rewritten
  so its dated GREEN `## Validation log` is not falsified.

  Acceptance 1: `test $(ls skills/extracting-design-md/evals/*.md | wc -l) -eq 6`

  ```
  $ test $(ls skills/extracting-design-md/evals/*.md | wc -l) -eq 6
  exit: 0
  ```

  Acceptance 2: `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  exit: 0
  ```

  Files changed: `skills/extracting-design-md/evals/eval-02.md`,
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 7 (rewrite `skills/designing-consistently/SKILL.md` to the five evals).
  One file rewritten end to end, 67 lines → 212. Nothing else touched: no eval file, no
  `SPEC.md`, no `PLAN.md`, no `feature_list.json`, not the other skill's `SKILL.md`, and zero
  writes to the Pegasuz checkout — `git status --porcelain` showed
  `M skills/designing-consistently/SKILL.md` and nothing else before this entry was appended.

  Per the owner instruction of 2026-08-20 recorded below under `## Reviews`, no per-step
  reviewer was dispatched and no subagent of any kind was spawned. What stands in its place is
  the box-by-box walk further down: every expected-behavior box of all five evals in
  `skills/designing-consistently/evals/`, checked against the rewritten text with the line that
  satisfies it named. Stated plainly so step 11 reads it correctly: this is a **text-level**
  walk — the rewritten skill was not executed against a repo in this step, so the RED→GREEN
  claims below mean "the text now carries what the box requires", not "a run was observed
  passing it". The live proving run is PLAN step 10; the fresh-context review is step 11's.

  **The acceptance target was the evals, not SPEC prose** (PLAN `## Interfaces between steps`).
  All five evals were read first; SPEC §3.1-§3.6, including the two dated step-4 amendment
  notes, was read as the design behind them. No case arose where an eval box and SPEC prose
  pulled apart — they agree everywhere this rewrite touches.

  **What the rewrite does, step by step.**

  - **Step 1 `Locate` → `Discover what governs`** (`SKILL.md:33-72`). Produces a *written ranked
    inventory* of what already governs the touched surfaces, from token definitions in
    stylesheets, the app's own DESIGN.md if it has one, the repo's agent context files
    (`AGENTS.md` gotchas), prose design docs, dated owner decisions left inline in source
    comments, and the shared components the surfaces already use. SPEC §3.2's four-rank
    source-trust table is carried (`:44-49`) with its consequences as bullets: nothing discovered
    is emitted confirmed, and which surfaces are the reference is itself an open question when
    nobody designated one (`:51-55`); coverage before convention, frequency breaks ties and never
    promotes (`:56-60`); an unresolvable pointer is reported unresolved, never the standing line
    by proxy (`:61-63`); the app's own DESIGN.md is the record while any *other* design doc found
    in the repo is rank 3 (`:64-66`); instantiating is the last option, never the first answer,
    and whatever it writes records the discovered system as `[provisional]` (`:67-70`). SPEC
    §3.2's closing line survives as its own paragraph: "Finding a file is not evidence that it
    governs" (`:72`).

  - **Step 2 `Read` → `Read the slice`** (`SKILL.md:74-121`). The slice is exactly three reads in
    order — `### Global` both sub-blocks first, then the touched page's own entries, then the 2-3
    sibling pages the surface must match — plus the frontmatter tokens the work needs, and then
    "Stop there" (`:88`). Working through the whole `## Decisions` section is called out as
    failing the step as surely as skipping `### Global`, with the scale reason given. A fenced
    block shows the exact addressing (`:95-106`); the prose after it fixes `### Global` as the
    first section under `## Decisions`, the `[provisional]` marker's position after the date, and
    where module names come from (declared navigation source → route tree → ask, SPEC §3.3). SPEC
    §3.4's precedence rules follow (`:115-121`): the specific wins only with a declared
    `exception to Global/<block>`, otherwise it is drift and step 4 flags it; provisional never
    beats confirmed; confirmed-vs-confirmed with no declared exception stops and asks; an
    exception is scoped to the surface that declares it; and silence is not freedom — a page whose
    own entries say nothing about a point is governed by the global on it.

  - **Step 3 `Build`** (`SKILL.md:123-135`). Substance unchanged — generated tokens never raw
    values, reuse before invention, new patterns born tokenized through frontmatter, the
    nudge-vs-escape line — plus SPEC §3.5's addition: a **confirmed** entry the work conflicts
    with is renegotiated with the owner; a **`[provisional]`** entry the work conflicts with is
    evidence, not a violation — build the better thing and carry it to step 4, which demotes it.
    "The work does not stop to renegotiate a candidate."

  - **Step 4 `Record` → `Record + repair (gate)`** (`SKILL.md:137-179`). The gate now has three
    limbs, stated up front: an unrecorded decision, a `[provisional]` entry the work touched
    still sitting as it was, or an unflagged contradiction the work exposed — "Appending today's
    decision satisfies only the first." Then *Record* (one line, `- YYYY-MM-DD — <decision>`,
    under `### <module>` → `#### <route> — <page>`, keeping the recordable-means list and the
    honoring-does-not-satisfy rule), *Repair status* (promote in place — drop marker, re-date —
    as the earned-by-work channel needing no reference surface and no owner reply, with the
    discovery channel named as the different one; demote — corrected entry plus one line on what
    beat it, removal-recorded allowed because provisional entries are exempt from
    never-silently-drop), and *Repair scope* (escalate on the two-or-more-module trigger into
    `#### Sistema` for values / `#### Patrones` for form and behavior, rewritten as a rule naming
    no route, status carried up unchanged; narrow on an owner-confirmed contradiction; flag drift
    on an undeclared contradiction of a global). Closing line: done is reported only once the gate
    is clear, and the report names the entries recorded and repaired.

  - **Step 5 `Verify`** (`SKILL.md:181-184`). Unchanged in substance, repointed at the live
    tooling: `design-md-gen` regenerates and `agent-lint`'s design checks are the gate. No new
    `Context-Engineering` or `context-lint` string was written — PLAN step 9 owns that sweep, and
    this file now contains neither string.

  - **Red flags** (`SKILL.md:202-212`). New table. Row 1 is the required flag, "I found a design
    doc, so I know the system" → "A file's existence is not evidence that it governs. Unknown
    authorship, unknown currency — rank 3, `[provisional]`, until the owner or a reference surface
    says otherwise." Six further rows cover the failure this lane measured (no DESIGN.md read as
    no system), the shared-component trap, the read-everything answer, the silence-is-freedom
    answer, the append-and-call-it-done answer, and the two the old file carried as a judgment
    note.

  - **Frontmatter `description` widened**, because the skill's job widened: it now stands alone
    without `extracting-design-md` having run, and discovers rather than requiring a DESIGN.md to
    exist. It states discovery-when-no-DESIGN.md, the bounded slice however large the app is, and
    repair; its triggers now include "any repo, whether its system lives in a DESIGN.md or only in
    tokens, context-file gotchas and prose docs" and "when an app has too many surfaces to read
    its whole decision log in one session", keeping the three original triggers. Third person
    throughout. The description is this repo's stated discovery interface, so the widening is
    load-bearing. `README.md`'s row for this skill still quotes the OLD description — PLAN step 9
    owns that refresh and this step deliberately did not touch it.

  **Box-by-box walk — all five evals.**

  `eval-01.md` — the repair gate (RED today on boxes 1-2, with 3-4 behind them):

  1. *Promote the provisional radius entry, drop marker, re-date in place; earned-by-work
     channel, no reference surface and no owner reply needed.* → `SKILL.md:153-157`, the
     **Promote** bullet, which states the move ("drop the marker and re-date the entry to today")
     and the channel distinction the box spells out ("This is the earned-by-work channel; it
     needs no reference surface and no owner reply. (Confirming something merely *discovered* is
     the other channel, and that one does need the owner or a reference surface.)"). RED→GREEN:
     `promote` occurred zero times in the old file.
  2. *Demote the contradicted provisional: corrected entry plus one line on what beat it;
     removal-recorded permitted; contradicting a provisional is evidence, not a violation, and
     the work does not stop to renegotiate.* → `SKILL.md:158-162` for the demotion mechanics,
     including "The corrected entry may be its removal, recorded, when a confirmed entry already
     covers the surface — provisional entries are exempt from the never-silently-drop rule that
     binds confirmed ones"; and `SKILL.md:132-135` for the second half the eval's RED section
     names explicitly ("Box 2 also fails in the opposite direction — step 3 makes any conflict a
     stop-and-ask"): step 3 now splits confirmed from provisional and says the work does not stop
     to renegotiate a candidate. RED→GREEN on both halves.
  3. *The module entry's undeclared contradiction of `### Global` / `#### Patrones` is drift, not
     a local override; provisional never beats confirmed; the exception form.* →
     `SKILL.md:115-121` (precedence, carrying the exact
     `exception to Global/<block>: <what> — <why>` form and "it is not a local override") and
     `SKILL.md:173-176` (**Flag drift** as a gate action, with both dispositions: declare the
     exception with the owner, or correct the entry to the global).
  4. *Escalate the shared-chrome decision to `### Global` / `#### Patrones` now that it stands in
     two modules, carrying `[provisional]` up unchanged.* → `SKILL.md:166-170`, the **Escalate**
     bullet: the two-or-more-modules trigger, the `#### Sistema` / `#### Patrones` routing (this
     decision is form and behavior, so `#### Patrones`), the rewrite-as-a-rule requirement, and
     "It carries its status up unchanged — escalation moves scope, it does not confirm."
  5. *Not done while a provisional entry the work touched still sits as it was; appending today's
     decision does not satisfy the gate on its own.* → `SKILL.md:137-140`, the gate's three limbs
     plus "Appending today's decision satisfies only the first", and `SKILL.md:178-179` (done is
     reported only once the gate is clear). Reinforced by the red-flag row at `:211`.

  `eval-02.md` — new element consumes the system (GREEN today; the job was not to break it):

  1. *Consumes token variables, zero raw hex, px radii or ad hoc padding.* → `SKILL.md:123-124`,
     "Styles come from the generated `design.tokens.css`, never raw values", with the
     nudge-vs-escape line at `:126-129` fixing where the boundary sits.
  2. *No parallel button style while the existing component fits.* → `SKILL.md:124-125`, "Reuse
     an existing component pattern when one fits".
  3. *A genuinely new variant is born tokenized — frontmatter, then regenerate.* →
     `SKILL.md:125-126`, unchanged in substance, with the generator named as `design-md-gen`
     rather than a bare script path. Step 2's slice does not endanger this eval: its fixture has
     frontmatter and a current generated stylesheet, and a file with no `### Global` or module
     sections simply yields empty first two reads.

  `eval-03.md` — unrecorded decision blocks completion (GREEN today, but its box 2 was rewritten
  at step 5's fix round to the module architecture, so this rewrite had to meet the new shape):

  1. *Completion is NOT claimed while the decision is unrecorded.* → `SKILL.md:137-140`.
  2. *Appends `- YYYY-MM-DD — <decision>` (today) under `### catalogo` →
     `#### /catalogo/inventario — Inventario`, one line.* → `SKILL.md:142-143`: "Each new decision
     is one line, `- YYYY-MM-DD — <decision>`, under its `### <module>` → `#### <route> — <page>`"
     — the eval's own arrow notation and the exact shape step 5's fix round put in the box. The
     old file's flat `### <surface>` addressing is gone from the file entirely.
  3. *Only then reports done, mentioning the recorded entry.* → `SKILL.md:178-179`, added
     deliberately for this box: "Done is reported only once the gate is clear, and the report
     names the entries recorded and the entries repaired." The old file implied the ordering but
     never asked for the entry to be named; the box was passing on inference and now passes on
     text.

  `eval-04.md` — discovery when no DESIGN.md exists (RED today on box 1, boxes 2-4 with it; box 5
  is a regression guard per the controller's ruling):

  1. *Step 1 returns a ranked inventory of what already governs — tokens in code, the `AGENTS.md`
     gotcha, the prose docs, the inline dated owner decisions — instead of stopping at "no
     DESIGN.md".* → `SKILL.md:33-42`: "Produce a written inventory of what already governs the
     surfaces about to be touched, ranked by who backs each source", "A missing DESIGN.md ends
     nothing; the system is somewhere else", and a where-to-look list naming each of the four
     source kinds the fixture plants, plus the shared components. RED→GREEN: the old step 1 was
     "Locate … Missing? Offer to instantiate it first", which the cold baseline shows dead-ending.
  2. *Ranked by source trust, not by how official a source looks: reference-surface code would
     confirm, the gotcha and the prose docs are candidates, frequency is a tie-break only.* →
     `SKILL.md:34-35` ("never by how official the file looks") and the four-rank table at
     `:44-49`, whose rank-4 row reads "tie-break only, never a source of truth". RED→GREEN:
     `rank`, `trust`, `reference surface` and `provisional` occurred zero times in the old file.
  3. *Nothing discovered is emitted confirmed; with no reference surface designated everything
     enters `[provisional]` and which surfaces are the reference is an open question for the
     owner; the two unresolvable pointers are reported unresolved, not taken as the standing line
     by proxy.* → `SKILL.md:51-55` for the first two clauses (including "When nobody has
     designated one — the usual case") and `SKILL.md:61-63` for the third, in the eval's own
     words: "not the standing line by proxy".
  4. *The shared header is not adopted on the strength of existing and looking official: its
     coverage is checked across the surfaces it would apply to (4 of 15) and it is recorded as an
     open question.* → `SKILL.md:56-60`, "Coverage before convention", carrying the fixture's own
     ratio as the worked case: "Four of fifteen is a split, not a rule: record it as an open
     question, not as a standing decision. Frequency breaks ties between candidates; it never
     promotes one." The red-flag row at `:208` repeats it as a rationalization.
  5. *No instantiation proposed while the system is discoverable; instantiating is the last
     option; what it writes records the discovered system as `[provisional]`.* → `SKILL.md:67-70`,
     all three clauses. Recorded as the eval itself records it: a regression guard, not this
     eval's RED.

  `eval-05.md` — bounded slice at 157 surfaces (RED today on box 1, with boxes 2 and 4 behind it):

  1. *`### Global` read first — both sub-blocks — before anything module-specific, even though the
     query names one page and no global entry names a route.* → `SKILL.md:77-85`: item 1 of the
     ordered slice, "**both** sub-blocks", with `#### Sistema` and `#### Patrones` defined,
     "Always, whatever page is being touched, before anything module-specific", and the reason the
     entry does not look relevant ("A global is written as a rule and names no route, which is
     exactly why it does not look relevant and is" — SPEC §3.4's guard rail read from the reader's
     side). RED→GREEN: `Global` occurred zero times in the old file.
  2. *The touched module is read as the touched page's own entries plus the 2-3 sibling pages the
     surface must match, not every page the module holds.* → `SKILL.md:86-87`, items 2 and 3 of
     the slice. RED→GREEN: `sibling` occurred zero times in the old file.
  3. *The other 12 modules and ~145 pages are not read and nothing from them is cited; a
     whole-section read fails this box as surely as skipping `### Global`.* → `SKILL.md:88-94`:
     "Stop there. The other modules are outside the slice: not read, not cited", followed by the
     symmetric failure statement in the box's own terms and the scale reason behind it.
  4. *The delivered header follows the `#### Patrones` entry and the sibling's back-link
     precedent; an answer built from the touched page's own 3 entries alone fails.* → the slice
     order at `:77-87` puts both in front of the work; `SKILL.md:119-121` closes the gap the
     fixture is built on ("silence is not freedom: a page whose own entries say nothing about a
     point is governed by the global on it, not exempt from it" — added deliberately for this
     box); step 5's re-check at `:183-184` re-reads `### Global` first against each touched
     surface; and the red-flag row at `:210` names the exact wrong answer.
  5. *An `exception to Global/<block>` declared in another module is not carried over as
     precedent.* → `SKILL.md:118-119`: "An exception declared on another surface is scoped to that
     surface and is never precedent here." Read as the eval says to read it — together with box 3,
     since a run that holds to the slice never reaches that module.

  **Constraints checked.** 212 lines (<500; the acceptance command is below). Runtime neutral:
  `grep -in 'context-engineering|context-lint|vue|pegasuz|react|tailwind|navConfig|nueva-linea'`
  over the file returns nothing (exit 1), and the one framework-flavoured string the old file
  carried — a `text-[13px]` utility-class example — was rephrased to "a 13px type size dropped
  inline" rather than carried forward. No machine-anchored path of any form: the `machine-path`
  check scans `skills/` and reported nothing. No reference files added, so
  references-one-level-deep holds trivially. Frontmatter `description` is third person.

  **Interface with step 8 — the heading shape this step settled on**, stated so step 8 can match
  it exactly (it is `SKILL.md:95-113` in the file):

  ```
  ## Decisions
  ### Global
  #### Sistema
  - 2026-06-02 — [provisional] cards use radius 14px; 10px is the migration target — 984 uses against 751, both in non-reference code
  #### Patrones
  - 2026-05-14 — detail surfaces open with the shared header: back link, then title, then actions
  ### <module>
  #### <route> — <page>
  - 2026-07-11 — <decision>
  - 2026-07-08 — exception to Global/Patrones: <what> — <why>
  ```

  Five rules travel with it: `### Global` is always the FIRST section under `## Decisions`; it has
  exactly the two sub-blocks `#### Sistema` (values and scales — tokens, radii, type, modes, the
  reference-surface declaration) and `#### Patrones` (form and behavior crossing modules); every
  module is a `### <module>` section holding `#### <route> — <page>` subsections; every entry
  keeps the lint's `- YYYY-MM-DD — ` prefix with `[provisional]` inside the entry text immediately
  after the date; and the exception form is
  `- YYYY-MM-DD — exception to Global/<block>: <what> — <why>`. Module names come from the app's
  own declared navigation source when it has one, else the route tree, else ask. This is the shape
  the step-5 eval fixtures already assume, so reader and writer agree.

  Acceptance 1 (path resolved per `DECISIONS.md` ruling 1):
  `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  lint exit: 0
  ```

  Only the pre-existing, expected LOW `cmd-drift` finding remains; the rewritten `SKILL.md` adds
  no finding of its own.

  Acceptance 2: `test $(wc -l < skills/designing-consistently/SKILL.md) -lt 500`:

  ```
  linecount test exit: 0 (212 lines)
  ```

  Files changed: `skills/designing-consistently/SKILL.md` (rewritten),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

- 2026-08-20 — Step 8 (rewrite `skills/extracting-design-md/SKILL.md` to the six evals).
  Rewritten whole, 87 → 268 lines. The acceptance target was the six eval files, not SPEC §3
  prose (PLAN `## Interfaces between steps`); SPEC §3.1-§3.6 including the dated step-4
  amendment note in §3.6 and the parent's two binding emission rules were read first, and where
  a box and the prose could be read apart, the box governed.

  **What changed, in the skill's own terms.** Step 4 stopped being "semantic role + frequency
  decide" and became a four-outcome election run off the owner-designated reference surfaces,
  with frequency demoted to a tie-break among non-reference variants that carry no design
  question inside them. Step 1 gained SPEC §3.2's source-trust table plus the "ask which
  surfaces are done right" question that makes rank 1 reachable at all. Step 5 gained the
  `### Global` + module architecture, the entry rules for it, and a bounded backfill method that
  replaces "Read each surface". Step 6 gained the frontmatter-is-the-compile-source rule and the
  live gate names. Step 7 stopped implying this skill is `designing-consistently`'s
  prerequisite. A `## Red flags` table was added, mirroring the sibling's, because every RED in
  these evals is a rationalization under count pressure.

  **Box-by-box walk over all six evals** (line numbers are the delivered
  `skills/extracting-design-md/SKILL.md`):

  *`eval-01.md` — counts measure, they do not elect.*
  1. *Both families named with counts and ≥1 file:line pointer each.* → `:69-72` keeps the
     harvest's "occurrence counts and file:line locations"; `:74-75` requires each family in the
     report to carry "its variant count, evidence, and the surfaces it touches"; `:251-252`
     holds every claim to its evidence. Kept deliberately: the eval warns that a skill answering
     "no reference surface, so no numbers" fails this box as surely as today's fails box 3.
  2. *Order by spread, not discovery order.* → `:76` verbatim: "Order by spread (surfaces
     affected), not by discovery order."
  3. *Counts do not elect; with no reference surface the report says nothing confirms.* →
     `:71-72` "Counts measure spread. They do not elect; step 4 does."; `:57-61` "When nobody has
     designated a reference surface — the usual case … nothing in the app confirms anything, the
     whole output runs `[provisional]`, and *which surfaces are the reference* is itself recorded
     as the open question that would settle the file"; `:83-86`; red-flag row `:262`.
  4. *Collapse still proposed; losers are migration targets; each token carries `[provisional]`
     and what it beat; the missing input recorded.* → `:115-119` "Provisional is not paralysis
     … 7 grays in the code become one `ink-muted` token plus six rows in the migration plan. …
     An extraction that ends with as many tokens as there were raw values did not do step 4";
     `:88-91` for migration targets carrying counts; `:160-167` for the entry with both counts;
     `:57-61` + `:168-170` for the reference-surface question as a recorded `#### Sistema` entry.
  5. *A losing variant with a written decision behind it is a competing claim, not debt.* →
     `:121-127`, written to this exact case: one document assigning 8px with a rationale, a later
     one assigning 10px and calling itself current — "reported as an open question. The newer
     does not govern by being newer, the older does not govern by having a rationale."

  *`eval-02.md` — faithful backfill.*
  1. *`[provisional]` entry under `### <module>` → `#### <route> — <page>` per surface, citing
     the files — both halves required.* → `:171-176` is the box's own fixture: "the same back
     button in three views) becomes a dated entry under `### <module>` → `#### <route> — <page>`,
     one under each surface it covers, citing the files it was found in. Three views agreeing is
     evidence from non-reference code, so the entry ships `[provisional]` — repetition is not
     confirmation." Address half and marker half both explicit. `:177-179` keeps the
     same-module case per route rather than escalating it (escalation needs two or more modules).
  2. *Contradictory card styles get no entry; open question with both variants and locations.* →
     `:180-188`, again the box's own fixture ("two card styles for the same data"), with the
     dated open-question form and "Every candidate carries its count and where it is used".
  3. *No invented decision.* → `:189-191` "A decision the code does not evidence is not written
     down, in either marked or unmarked form"; reinforced by `:109-113`.

  *`eval-03.md` — gates green, code untouched, convergence stated.* (All three green before;
  kept green.)
  1. *`design-md-gen` parse **and** generation plus the lint design checks before completion.* →
     `:214-216`: "run `design-md-gen` (it must parse *and* generate) and `agent-lint` — the
     design checks passing is the definition of 'the file is real', not optional polish." The
     eval's own text still says `context-lint`; PLAN step 9 owns that string and this step did
     not touch it — but the skill names the live tool, per the brief's no-new-stale-references
     rule.
  2. *Zero application code files modified; writes limited to DESIGN.md, its
     `design.tokens.css`, optionally the migration-plan companion doc.* → `:216-218` names
     exactly those three; `:220-222` puts the plan in a companion doc "never inside the
     always-consumed DESIGN.md".
  3. *Per-surface batches, hand-off to `designing-consistently`, drift count as the convergence
     metric.* → `:220-231`.

  *`eval-04.md` — mode variants are not drift.* (All three green before; kept green, wording
  carried over unchanged so its dated `## Validation log` still describes the shipped behavior.)
  1. *Selector-scoped reassignments → frontmatter `modes:`, not drift.* → `:76-79`.
  2. *Unscoped grays still in the drift report with counts and evidence.* → `:79-81` ("Drift is
     the unscoped, scattered variation") feeding `:69-75`.
  3. *A mode duplicating another's values is a widened selector, not a third value set.* →
     `:80-81`.

  *`eval-05.md` — reference surfaces elect, frequency only breaks ties.*
  1. *Card token elected from the reference surfaces (8px) though 14px leads 984 to ~751.* →
     `:88-91`, outcome 1: "A reference surface covers the role → its value is the token,
     **confirmed**", with `:83-86` making the criterion the reference surfaces and stating
     "Frequency is not the criterion — the biggest count is the biggest migration, not the right
     answer."
  2. *The 984 reported with its count and made a migration target; the count never says what is
     right.* → `:88-91`: "Every other variant becomes a migration target carrying its count,
     however far ahead it leads. 984 against 751 the other way is the size of the move, not a
     counter-argument."
  3. *Neither document confirms; the conflict is an open question; the role is settled by the
     reference surfaces.* → `:62-65` puts both docs at rank 3 ("A doc that calls itself the
     source of truth … is rank 3 however recent, however widely cited, however carefully
     argued"), and `:121-127` closes with "where a reference surface covers the role, the code
     settles it and both documents stay candidates."
  4. *Modal/drawer not elected at all; open question with both counts and where each is used —
     and this is where a frequency rule and a reference-surface rule visibly diverge.* →
     `:95-101`, outcome 3, states the divergence in the fixture's own numbers and says which
     governs: "when the code says one value 68 times and the document that calls itself current
     says another 12 times, frequency picks the 68 — this skill picks neither, because a
     documented decision losing on count is the owner's call, not the extraction's." The
     open-question form and its locations requirement are `:180-188`. The eval's warning that a
     rewrite electing the 68-use value on count breaks this box is what `:102-107` fences:
     frequency's tie-break is scoped to variants "that carry no design question inside them", and
     12px-vs-20px is a design question.
  5. *Frequency still settles the pill role; the elected name still ships `[provisional]`.* →
     `:102-107`, outcome 4: "three spellings of one identical value (a semantic alias, the
     framework default, a raw literal) is a naming tie, and the most-used spelling wins it. The
     elected name still ships `[provisional]` — a tie-break confirms nothing."

  *`eval-06.md` — provisional by default, and no token without an election.*
  1. *Control radius elected and its token goes into the frontmatter (the compile source).* →
     `:92-94`, outcome 2, plus `:206-212`: "The frontmatter is the compile source, so an elected
     but unconfirmed token lives there like any other — holding it out would generate a
     near-empty stylesheet and send every build back to raw values." This is the parent's option
     A and the eval's explicit anti-overcorrection guard.
  2. *A `[provisional]` `### Global` / `#### Sistema` entry naming the token **and what it beat,
     with both counts**; the bare form fails.* → `:160-167`, written to the fixture's own
     numbers: `- 2026-06-02 — [provisional] control radius 10px — 1832 uses against the 8px
     control rule at 340, no reference surface`, followed by the negative example the box names
     ("An entry reading only `[provisional] control radius is 10px` is not enough; without the
     numbers a later session cannot decide whether to promote or demote it").
  3. *Modal/drawer gets **no** frontmatter token at all.* → `:95-96` "No frontmatter token for
     that role at all"; `:212` "Roles with no election have no entry in the frontmatter at all";
     `:109-113` "Marking something provisional does not license inventing it" — the parent's
     sentence, kept verbatim as the rule's name.
  4. *That role gets an open-question entry with both candidates, their counts, and where each is
     used.* → `:180-188`, whose worked example is that role and which requires "Every candidate
     carries its count and where it is used". `:186-188` resolves the collision this creates with
     SPEC §3.4's never-name-a-route guard rail: "An open question is not a rule, so this is the
     one entry under `### Global` that does name locations — it has to, or the owner cannot
     settle it."
  5. *Declared type roles are not law; a self-declaring config is a candidate; evidenced roles
     ship `[provisional]`; the zero-use thirteenth role gets no token.* → `:62-65` (declaration
     ≠ confirmation, config at rank 3) and `:109-113`: "a scale declared in config or a document
     ships only the roles that are live, `[provisional]` like any other unconfirmed election, and
     a declared role with zero live uses gets no token at all." Red-flag row `:266` carries the
     twelve-role case by name.
  6. *The mostly-provisional file is the correct output; no marker dropped, no election
     manufactured; the provisional share reported as the drift measure.* → `:20-22` states it in
     the opening ("a first extraction from a drifted app comes out **mostly `[provisional]`, and
     that is the correct output** — the pull toward making it look decided is the failure this
     skill exists to stop"); `:227-231` makes the provisional share the second reported metric,
     falling "as sessions promote entries by building on them, never by the extraction getting
     bolder"; `:253-256` sets the honest-measurement-versus-fabrication line; red-flag row `:265`
     catches the exact in-session sentence ("Most of this file is `[provisional]` — tighten it
     before delivering").

  **The two SPEC §3.6 amendment rules, located.** (i) Genuine-but-unconfirmed election →
  frontmatter token (`:92-94`, `:206-212`) **and** a `#### Sistema` `[provisional]` entry naming
  the token and what it beat with both counts (`:160-167`). (ii) No election → no frontmatter
  token at all (`:95-96`, `:212`) plus an open-question entry with the role, the competing
  candidates and their locations (`:180-188`).

  **Interface with step 7 — byte-identical, and how that was checked.** The architecture block
  emitted at `:138-148` carries the six heading lines `## Decisions`, `### Global`,
  `#### Sistema`, `#### Patrones`, `### <module>`, `#### <route> — <page>`. Checked, not eyeballed:
  the six lines were extracted from both files with the same anchored `grep` and compared with
  `diff` (exit 0) and `md5sum` — both sequences hash to `8962f6332fab7d46ab83aafc92c90e58`, so
  they agree byte for byte including the U+2014 em dash in `#### <route> — <page>` (`cat -A`
  shows `M-bM-^@M-^T` on both sides, i.e. `E2 80 94`, not a hyphen and not U+2013). The exception
  form matches `designing-consistently:105` and its prose at `:114`
  (`exception to Global/<block>: <what> — <why>`), and every entry line keeps the lint's
  `- YYYY-MM-DD — ` prefix with `[provisional]` inside the entry text after the date
  (`:193-195`). The five rules travelling with the shape hold: `### Global` first (`:150-151`,
  `:206-207`), exactly two sub-blocks (`:151-154`), a global written as a rule naming no route
  (`:153-154`, with the open-question carve-out stated at `:186-188`), module names from the
  app's own declared navigation source else the route tree else ask (`:154-156`).

  **Scale honesty.** The baseline's "Read each surface" met 157 views and the runner invented a
  bound off script. `:197-204` replaces it: "Do not read every surface — at a hundred and fifty
  that read either does not happen or does not finish, and a bound invented mid-run is not a
  method", then makes search the instrument for finding patterns and reading the instrument for
  resolving disagreements, capped at the reference surfaces, the shared components the routes
  import, and ~3 routes per module, with the coverage stated in the output. `## Scaling
  (bounded)` (`:233-242`) keeps the fan-out lever, tells it what to do past a hundred surfaces,
  and states what governs when the runtime refuses subagents — the baseline's actual condition.
  Red-flag row `:268` catches the claim itself.

  **No longer a prerequisite.** `:220-225`: execution "belongs to `designing-consistently`: its
  read-consume-record loop takes over from here, and it does not need this extraction to have run
  — it discovers what governs on its own and consumes whatever this produced." No other sentence
  in the file describes this skill as a required precursor.

  **Description (the discovery interface) rewritten** to the changed contract: reference-surface
  election over frequency, `[provisional]` output, per-module decisions under a `### Global`
  tier; triggers kept (adopting DESIGN.md, multiplied values, "quedó desprolijo", re-audit after
  a migration batch) and one added for the size case ("when an app has too many surfaces for one
  flat list of decisions"). Third person, single `description:` key inside the first 400 bytes.

  **Constraints checked.** 268 lines (<500; acceptance below). Runtime-neutral:
  `grep -Ein 'context-engineering|context-lint|vue|pegasuz|react|tailwind|navConfig|nueva-linea|globals\.css|@theme|next\.js|svelte|angular|nl-'` over the file exits 1 (no hits) — the old
  file's `@theme` and tailwind-config mentions were rephrased to "stylesheets and theme layers,
  the build's style config", and the one worked example that had named modules was rewritten to
  "the drawers of two modules vs … three dialogs of a third". `[data-theme]` and `modes:` are
  kept as DESIGN.md-convention vocabulary, per the controller's dated eval-04 ruling in
  `DECISIONS.md`. No machine-anchored path of any form:
  `grep -Ein '[A-Za-z]:[\\/]|/home/|/Users/|/mnt/[a-z]/'` exits 1 and the lint's `machine-path`
  check reported nothing. No `references/` directory exists in this skill, so
  references-one-level-deep holds trivially. The stale `Context-Engineering` /
  `context-lint` strings the old file carried at `:13` and `:61` are gone as a side effect of the
  whole-file rewrite; PLAN step 9's remaining targets (`evals/eval-03.md`, `README.md`) were NOT
  touched and its `git grep` gate is still the thing that proves them.

  Acceptance 1 (path resolved per `DECISIONS.md` ruling 1):
  `node /c/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` exits 0:

  ```
  agent-lint C:\Users\mateo\orca\workspaces\skills\design-skills-at-scale
    LOW    AGENTS.md:15  ../Agent-Engineering/scripts/agent-lint.mjs escapes the repo — context-dependent, true only where that path exists outside it (a sibling checkout, CI)  [cmd-drift]
  0 high, 0 medium, 1 low — PASS
  lint exit: 0
  ```

  Only the pre-existing, expected LOW `cmd-drift` finding remains; the rewritten `SKILL.md` adds
  no finding of its own.

  Acceptance 2: `test $(wc -l < skills/extracting-design-md/SKILL.md) -lt 500`:

  ```
  linecount test exit: 0 (268 lines)
  ```

  No reviewer ran on this step (the owner's dated instruction dropping the per-step reviewer for
  steps 6-10); the walk above is this step's own verification, and step 11's work-verify is the
  next thing that checks it.

  Files changed: `skills/extracting-design-md/SKILL.md` (rewritten),
  `work/design-skills-at-scale/PROGRESS.md` (this entry).

## Reviews

<!-- Verbatim verdict text from every in-session review seat: the
     PASS/FAIL line and its findings, pasted as the reviewer wrote them.
     A claim that a review returned PASS is not evidence; this is. -->

### Step 1 — step review (fresh reviewer, sonnet)

**Spec compliance:** Compliant.

**Assessment — Step quality: Needs fixes.** Reasoning, verbatim: "The
structural contract is right — correct file set, correct schema, exact 1:1
requirement mapping — but roughly half the `verification` strings aren't
literally executable as written (prose mixed into command position, one
referencing a nonexistent baseline artifact), which is precisely the
honesty-of-contract risk this review was scoped to catch. These are cheap,
mechanical fixes to `feature_list.json` (tighten wording, remove/relocate
parentheticals, name a real comparison target for F04/F10) and don't require
touching PROGRESS.md or DECISIONS.md's substance."

Findings, verbatim:

> #### Critical (Must Fix)
> None.
>
> #### Important (Should Fix)
> - `work/design-skills-at-scale/feature_list.json` — most `verification` strings (F01, F02, F03, F05, F06, F10) interleave a real shell fragment with inline parenthetical prose (e.g. F01: `test -f skills/designing-consistently/evals/eval-04.md (discovery-without-DESIGN.md, PLAN step 5) plus: grep -q ...`). As written, none of these is a single command you can paste into a shell — the parenthetical breaks it before the real check ever runs. This undercuts the step's own accept bar ("each `verification` naming the command that will prove it"). Fix: move the citation/rationale into the `behavior` field or drop it, leaving `verification` as one chained command (e.g. `test -f ... && grep -q ... && ! grep -qE ...`).
> - F04's `verification` — `diff skills/designing-consistently/evals/eval-01.md against its pre-step-7 baseline confirms the repair-gate rewrite` references an artifact ("pre-step-7 baseline") that no PLAN step ever creates; nothing in the lane archives a copy of `eval-01.md` before step 7 rewrites it. As literally stated this clause can't be run — it would need an unnamed git ref to resolve. This is the row covering the step-4 repair-gate requirement, so it deserves a concrete mechanism (e.g. name the exact commit/tag to diff against, or drop the clause and rely on the three `grep -qi` checks already present in the row).
> - F10's ordering check — `git log --oneline confirming the eval-authoring commits (steps 5-6) precede the SKILL.md content commits (steps 7-8)` names no mechanical way to "confirm" precedence (no format string, no comparison). Since this row is the only proof for the repo's hard constraint (evals before content, restated three times across AGENTS.md/SPEC/PLAN), it should name something checkable, e.g. comparing first-commit dates per path with `git log --format=%ai --follow -- <path> | tail -1` for the eval files vs. the SKILL.md, or asserting ancestry with `git merge-base --is-ancestor`.
>
> #### Minor (Nice to Have)
> - `work/design-skills-at-scale/DECISIONS.md:4-5` — both entries collapse "choice" and "why" into one run-on paragraph with an inline "Why:" label, rather than the three-segment `- YYYY-MM-DD — CHOICE — WHY` shape. I checked this against `Agent-Engineering/templates/repo/work/DECISIONS.md.template:4`, which is the exact template this file was scaffolded from (per the commit's own PROGRESS entry) — the template literally spells out `- {{YYYY-MM-DD}} — {{CHOICE}} — {{WHY}}`. `agent-lint` only checks the date prefix (SPEC §6), so this doesn't fail the gate, but it breaks the convention the file's own header comment states one line above it, and step 4 will add more rulings to this file — worth aligning now so the shape doesn't propagate.
> - F07's second clause ("`eval-01.md` ... exercises the undeclared-contradiction-is-drift case") is a content judgment call, not a command — harmless as a supplementary note but should be labeled as such rather than presented alongside an executable check.

Controller ruling on the first Minor: pulled INTO fix round 1 rather than
deferred, because PLAN step 4 appends more rulings to `DECISIONS.md` and
reformatting later costs more than reformatting two entries now. The second
Minor (F07's judgment-call clause) was deferred, then un-deferred in round 2
when the same row had to be touched anyway. Both rulings are in
`DECISIONS.md`.

### Step 1 — fix round 1 re-review (fresh re-reviewer, sonnet)

**Verdict — Fix round: Findings remain open.** Verbatim:

> **Findings remain open:** Finding 1's defect class (unpasteable `verification` string) is still present and unaddressed in F08, F09, and F11 — these were not covered by any stated deferral (unlike F07) and the fix round's own notes mischaracterize them as in-scope-to-skip alongside F07. Findings 1-4 as originally listed are otherwise addressed. Round should not close clean until the controller either explicitly extends the F07-style deferral to F08/F09/F11, or the implementer fixes those three the same way F01-F06/F10 were fixed.

Per-finding verdicts: findings 1, 2, 3 and 4 all **ADDRESSED** — the six named
rows de-interleaved and `bash -n`-clean; F04's nonexistent-baseline clause
dropped; F10 given a tested `git merge-base --is-ancestor` mechanism;
`DECISIONS.md` reshaped with no content lost.

New breakage found by the re-review, verbatim:

> 1. **Important — F08, F09, F11 carry the identical "prose spliced into command position" defect as Finding 1, and were not disclosed as deferred the way F07 was.** Verified empirically with `bash -n`:
>    - F07 (`feature_list.json:47`, expected unfixed — controller explicitly deferred it): syntax error at the unquoted `(`.
>    - F08 (`feature_list.json:54`): `! git grep -n '...' -- ... README.md (PLAN step 9 acceptance, verbatim)` — syntax error at `(`; the trailing parenthetical isn't a shell comment, so pasting this fails before the real `git grep` ever runs.
>    - F09 (`feature_list.json:61`): `node .../agent-lint.mjs . exits 0 (path resolved per DECISIONS.md ...)` — same `(` syntax error; also "exits 0" would otherwise be passed as stray arguments to `node`.
>    - F11 (`feature_list.json:75`): two "exits 0" splices plus a parenthetical plus a "plus"-joined third clause ("... is empty") — same class of break, worst offender of the four.
>
>    `PROGRESS.md:56-57` and the fix commit message group all four ("F07, F08, F09, F11 left untouched per the controller's scope") as if equally sanctioned to skip, but only F07 was named as deferred in this round's instructions. The round fixed six rows for exactly this defect while silently leaving three more rows with the proven-identical defect, uncalled-out. That's the internal inconsistency the extra check was checking for, and it's present.
>
> 2. **Minor — DECISIONS.md reshape doesn't hit a clean two-dash/three-segment split.** Counting em-dashes per entry: line 5 has 3 (→ 4 apparent fields), line 6 has 4 (→ 5 apparent fields), versus the template's exact `DATE — CHOICE — WHY` (2 dashes) at `Agent-Engineering/templates/repo/work/DECISIONS.md.template:4`, and versus the fix's own new line 7, which uses exactly 2 real structural dashes (the other two dashes on that line are inside a backtick-quoted illustration of the shape, not delimiters). Content is intact and the "Why:" label is gone, but the entries still don't parse as a clean three-field record — an extra internal em-dash is used as ordinary prose punctuation inside what should be a single CHOICE or WHY segment. Not blocking; flagged for optional cleanup.

### Step 1 — fix round 2 re-review (fresh re-reviewer, sonnet)

**Verdict — Fix round: All findings addressed, no new Critical/Important
breakage.** Verbatim:

> **Finding verdict: ADDRESSED.** All four rows now carry the identical fix pattern applied to F01-F06/F10 in round 1: every parenthetical/prose splice is gone from `verification`, relocated verbatim into `behavior`. Confirmed both structurally (diff review) and empirically:
>
> ```
> f07.sh: PARSE OK  — grep -q 'exception to Global' skills/designing-consistently/SKILL.md
> f08.sh: PARSE OK  — ! git grep -n 'Context-Engineering|context-lint' -- skills/designing-consistently skills/extracting-design-md README.md
> f09.sh: PARSE OK  — node .../Agent-Engineering/scripts/agent-lint.mjs .
> f11.sh: PARSE OK  — node .../design-md-gen.mjs ... --target cssvars && node .../agent-lint.mjs . && [ -z "$(git -C ... status --porcelain)" ]
> ```
> `feature_list.json` also parses as valid JSON (`node -e "JSON.parse(...)"`, 11 rows) and the working tree already matches the diff's post-image exactly (`feature_list.json:46-47,53-54,60-61,74-75`).
>
> **Ruling A** — ADDRESSED. `DECISIONS.md:8` records the ruling as a new dated entry with exactly 2 em-dashes (clean `date — choice — why`, verified by `grep -o` count), naming that F07's pasteability break was never itself deferred (only the eval-01.md content-judgment Minor was). F07's citation was relocated, not dropped.
>
> **Ruling B** — ADDRESSED. F09 (`feature_list.json:61`) is now the bare `agent-lint.mjs .` invocation — no `exits 0` argv pollution, its own exit status is literally the answer. F11 (`feature_list.json:75`) is three real `&&`-chained invocations, the third being an actual `[ -z "$(git -C ... status --porcelain)" ]` test whose exit status genuinely encodes emptiness — not the old "plus ... is empty" prose. Both citations preserved into `behavior` (`feature_list.json:60,74`).
>
> **Ruling C** — ADDRESSED for the hard constraint: `DECISIONS.md:5-6` are byte-identical to before this round (unchanged context in the diff hunk `@@ -5,3 +5,5 @@`), and still carry the flagged em-dash-count issue — confirming they were genuinely left alone, not silently reformatted. One caveat: the round also appended a new `DECISIONS.md:9` entry documenting the deferral, when the ruling said it "should only be noted in PROGRESS.md." This doesn't touch the protected lines and follows the file's own established convention, so I don't read it as the "editing them" scope violation the ruling warned about — but it is a small deviation from "only," noted below rather than treated as blocking.
>
> **PROGRESS.md correction** — ADDRESSED. `PROGRESS.md:61-62` (the wrong round-1 claim) is left in place for history, immediately followed by `PROGRESS.md:64-68`: "**Correction (round 2):** the line above was wrong. Only F07 was actually deferred... F08, F09 and F11 carried the identical... defect... and were left broken by mistake, uncalled-out." The record no longer carries the false claim uncorrected.
>
> ### New breakage in the fix diff
> None Critical/Important. Minor: `DECISIONS.md:9` adds a Ruling-C entry to the decisions log in addition to `PROGRESS.md`, slightly exceeding the "only... PROGRESS.md" instruction — non-blocking, causes no harm, doesn't touch the protected lines.
>
> ### Out-of-scope observations
> `DECISIONS.md:7` (the round-1-reshaped third entry) carries the same em-dash-count characteristic (4 em-dashes) as lines 5-6, but wasn't named in Ruling C's scope. Non-blocking, leave for work-verify's triage alongside the named lines.

**Step 1 closed: Approved after 2 fix rounds (cap 5, not reached).**
Controller independently re-ran `bash -n` over all eleven `verification`
strings extracted from the committed file: 11/11 PARSE OK.

### Deferred to work-verify triage (step 1)

- `DECISIONS.md:5-7` — entries use extra internal em-dashes as prose
  punctuation, so they do not split cleanly into the template's three
  `DATE — CHOICE — WHY` fields. Content intact; `agent-lint` only checks the
  date prefix. Raised as Minor by the round-1 re-review and extended to line 7
  by the round-2 re-review's out-of-scope observation.
- `DECISIONS.md:9` — Ruling C was logged in `DECISIONS.md` as well as
  `PROGRESS.md`, a small deviation from that ruling's "only ... PROGRESS.md".
  Judged non-blocking by the round-2 re-review; it follows the file's own
  convention that every controller ruling is logged there.
- `feature_list.json` F07 — whether `eval-01.md` actually exercises the
  undeclared-contradiction-is-drift case is a content judgment, not something
  its `grep -q 'exception to Global'` check can prove. Named in `behavior` as
  a work-verify triage item.

### Step 2 — step review (fresh reviewer, sonnet)

**Spec compliance:** ❌ Issues found — `baseline-designing-consistently.md:26-32`, Misunderstood:
the preamble's fence-attribution claim misrepresents Step 1's transcript by omission.

**Assessment — Step quality: Needs fixes.** Reasoning, verbatim: "The transcript handling,
acceptance criterion, and controller-addenda scoping are all clean; the one real defect is that
the preamble's fence-attribution bullet quietly narrows 'core findings' to exclude a sub-finding
in Step 1 that the transcript itself attributes partly to the read-only fence, then states a
broader, unqualified 'not... artifacts of the fence' claim that doesn't hold for that
sub-finding — a framing gap that could steer PLAN step 4's reconciliation of SPEC §5's first
prediction the wrong way."

Findings, verbatim:

> #### Critical (Must Fix)
> None.
>
> #### Important (Should Fix)
> - `baseline-designing-consistently.md:21-32` (the "one constraint that shaped the run" bullet) vs. `:44` (Step 1, "Where this step failed"). The bullet claims read-only "does **not** affect Steps 1, 2, or 4's core findings," and defines those "core findings" only as the three absence facts (no DESIGN.md, no `## Decisions`, no tokens/generator/lint script) — then generalizes past its own careful scoping with "Do not read those three steps' failures as artifacts of the fence" (plural, unqualified "failures," not "core findings"). But Step 1 reports a second, distinct failure the bullet never mentions: the skill's fallback ("offer to instantiate... DESIGN.md.template") could not be carried out, and the runner gives two causes for that, in its own words — "the template source is outside my scope fence, **and even if it weren't, the repo is read-only so I could not create the file regardless**." That second clause is exactly a read-only-caused failure inside Step 1, and the preamble's blanket "does not affect Step 1" / "not... artifacts of the fence" statement doesn't disclose that it's silently excluding this sub-finding under the narrower "core findings" label.
>   Why it matters: SPEC §5's first recorded prediction is "step 1 offers to instantiate a DESIGN.md," and PLAN step 4 reconciles that prediction against exactly this file. If step 4 trusts the preamble's blanket claim at face value, it will treat Step 1's failure-to-offer as a clean, fence-independent signal — when the runner's own words attribute part of that failure to the read-only fence. That risks step 4 landing on the wrong verdict (confirmed/contradicted/unobserved) for a prediction the whole evidence method (SPEC §5) was built to test honestly.
>   Contrast: I checked the identical question for Step 4, since the review brief asked for both. Step 4's transcript (line ~154) says "even if DESIGN.md had existed, I made no file changes this run, so nothing needed gating on disk either way — but structurally, the gate itself had no file to gate against, **independent of the read-only rule**." The preamble's claim for Step 4 tracks this correctly — the runner itself disentangles the two causes and confirms the core finding is fence-independent. Step 1's transcript does the opposite: it entangles the two causes for its second finding, and the preamble erases that entanglement rather than surfacing it.
>   How to fix: narrow the "does not affect" sentence to scope explicitly to the absence facts only (e.g., "the *absence* facts above are fence-independent"), and add one clause acknowledging that Step 1's offer-to-instantiate fallback was reported as blocked in part by the read-only constraint, leaving the reconciliation itself to PLAN step 4. This is a framing fix only — no transcript edits needed.
>
> #### Minor (Nice to Have)
> - `baseline-designing-consistently.md:11` ("verified byte-identical to this worktree's `skills/designing-consistently/SKILL.md` **before the run**") vs. `PROGRESS.md`'s own account of this step, which says the `diff` was run "before writing this file" / "at transcription time" — i.e., after the cold run, not necessarily before it started. The underlying fact (byte-identical) is true either way and nothing turns on it, but the preamble's wording claims a verification timing that doesn't match the implementer's own described sequence. Reword to "verified byte-identical at transcription time" or similar.

Controller response: the Important finding went into fix round 1. The Minor was pulled in with it
(a false claim about when a verification happened, in the same paragraph already being edited).
The controller also supplied a fourth controller-verified addendum it had checked directly — that
the skill's cited `Context-Engineering templates/repo/DESIGN.md.template` does not resolve
anywhere on the machine's work-repos folder — because it de-confounds the very finding under
dispute and PLAN step 4 would otherwise have to guess at it.

### Step 2 — fix round 1 re-review (fresh re-reviewer, sonnet, third seat — see the seat-failure note below)

**Verdict — Fix round: All findings addressed, no new Critical/Important breakage.** Verbatim:

> ### Finding verdicts
>
> 1. **Important — preamble's fence-attribution bullet erases an entanglement in Step 1.** ADDRESSED. `work/design-skills-at-scale/baseline-designing-consistently.md:27-40`. The unqualified, plural closer ("Do not read those three steps' failures as artifacts of the fence...") is gone entirely. The rewrite (a) scopes the fence-independence claim explicitly to "the *absence facts* in Steps 1, 2, and 4" (line 28) rather than "those three steps' failures" wholesale; (b) names the second Step 1 failure concretely — "the runner could not carry out the skill's own fallback ... and even if it weren't, the repo is read-only so I could not create the file regardless" (lines 32-37), quoting the runner's own words verbatim, matching the transcript at `baseline-designing-consistently.md:52`; (c) states the entanglement plainly ("a read-only-caused failure sitting inside Step 1, entangled with the fence-independent absence facts rather than separate from them," lines 37-38) without resolving it; (d) explicitly defers: "Whether and how that entanglement bears on SPEC §5's first recorded prediction is left to PLAN step 4's reconciliation, not decided here" (lines 38-40). This clears the over-correction bar too — it names SPEC §5 only to say the question isn't answered here, it doesn't answer it. A reader who has not parsed the transcript closely would still catch the entanglement from this bullet alone. Scope fence: held — this edit is entirely within the preamble blockquote (before `## Step 1` at line 42), not inside the verbatim transcript region.
>
> 2. **Minor — false claim about verification timing.** ADDRESSED. `baseline-designing-consistently.md:9-15`. Old text claimed the copy was "verified byte-identical ... before the run" without attribution. New text splits it correctly by party and timing: "The controller verified this copy byte-identical ... before dispatching the cold runner; the implementer transcribing this file independently re-verified the same byte-identity at transcription time, after the run." This matches the ground truth given to the implementer and the implementer's own described sequence — no sequence is asserted that either party didn't actually observe. Scope fence: held — preamble-only edit, above `## Step 1`.
>
> 3. **Addendum 4 required.** ADDRESSED. `baseline-designing-consistently.md:206-212`, under `## Controller-verified addenda`. States the fact set essentially verbatim as specified (skill's cited path, depth-2 search of the work-repos folder returning nothing, the three specific candidate paths failing to resolve, and the actual asset's location/size at `Agent-Engineering/templates/repo/DESIGN.md.template`, 789 bytes) and stops — no clause draws a conclusion about what it means for Step 1 or SPEC §5; PROGRESS.md's own note about the addendum ("PLAN step 4 owns what it means") is changelog prose, not part of the addendum text itself. Scope fence: held.
>
> **Scope fence overall:** held. Comparing the diff hunks against the file's actual section boundaries: both preamble edits land before the `## Step 1` heading (line 42), and the addendum edit lands after `## What I would have committed` (ends line 180) inside `## Controller-verified addenda`. Nothing between `## Step 1` and `## What I would have committed` (lines 42-180) is touched by this diff — the CJK-note line the controller already flagged as the sole prior alteration is unchanged here too. `SPEC.md` and `PLAN.md` do not appear in the diff at all.
>
> ### New breakage in the fix diff
> None. (One trivial, non-blocking prose wrinkle noted below — not a break of any finding or fact.)
>
> ### Out-of-scope observations
> - `baseline-designing-consistently.md:33-36`: the "two causes" sentence paraphrases the first cause ("the template source being outside its scope fence") but direct-quotes the second ("and even if it weren't..."), an asymmetric construction with a straight quote opening mid-sentence after an em-dash. Reads slightly awkwardly but is not a factual error, doesn't misquote anything, and doesn't affect the finding's substance. Non-blocking, not requiring a further round.
>
> ### Verdict
> **Fix round:** All findings addressed, no new Critical/Important breakage.

Controller cross-check, run independently of the re-review: a character-level diff of the
transcript region against the original cold-runner report shows exactly one altered line — the one
carrying the instructed editorial note on the stray CJK character — and nothing else.

**Step 2 closed: Approved after 1 fix round (cap 5, not reached).**

#### Seat-failure note (recorded because the rung's evidence depends on it)

The step-2 fix-round re-review took three dispatched seats to produce a retrievable verdict. The
first two (`sonnet`, then `opus`) each ran and then went idle repeatedly without their report ever
reaching the controller — three empty idle notifications from the first, three from the second,
across five explicit resend requests using two different phrasings. No refusal was observed: the
tool accepted every dispatch and the agents ran; the report text simply never arrived. The third
seat was dispatched with the delivery method changed — write the verdict to a scratch file, reply
only `DONE` — and its verdict arrived intact on the first attempt. That is the verdict recorded
above. Every later review seat on this lane uses file delivery for the same reason.

### Deferred to work-verify triage (step 2)

- `baseline-designing-consistently.md:33-36` — the "two causes" sentence paraphrases the first
  cause and direct-quotes the second, an asymmetric construction that reads awkwardly. Not a
  factual error and not a misquote. Raised by the round-1 re-review as a non-blocking
  out-of-scope observation.

### Step 3 — step review (fresh reviewer, sonnet)

**Spec compliance:** ✅ Compliant.

**Assessment — Step quality: Approved.** Reasoning, verbatim: "The transcript's integrity was
already controller-verified; this review's job was the framing, and it holds up under the specific
stress-test the brief posed — four distinct stoppages, one of them (Step 4) splitting into two
causes on the same step, are attributed correctly and separately rather than compressed into one
blanket claim, the no-subagent fence is named as the lane's rather than the skill's and the review
of what it means is explicitly left to PLAN step 4, and the addenda stay confined to the three
scoped facts without adjudicating anything SPEC §5 predicted. Nothing found here would mislead
step 4's reconciliation."

Strengths, verbatim:

> - **The four causes are kept apart, including at the sub-step level, not just the step level.** Step 4 in `extracting-design-md` bundles two different asks ("propose tokens" + "confirm with the owner"), and the preamble splits them correctly: the READ-ONLY bullet attributes the owner-confirmation gap to "this being an unattended cold run with no live owner in the loop, not to write permission," while the NO-SUBAGENTS bullet separately confirms it "does not touch... Step 4's token-election reasoning." Step 6's tool-unreachability is likewise pinned on the repo-scope fence in both bullets. All four causes named in the brief — READ-ONLY, NO-SUBAGENTS, no live owner, repo-scope fence — land on the step each actually produced, with no cause absorbing another's effect.
> - **The NO-SUBAGENTS bullet states the ownership boundary explicitly and stops.** "This fence is the lane's, not the skill's." It then reports the ~15-surface / ~60-label / "roughly 10x" numbers and the runner's own claim that the grep aggregation is exhaustive, but closes with "Say what it was, not what it means for the harvest's practicality or for SPEC §5... the reconciliation itself belongs to PLAN step 4, not here." It reports magnitude, not verdict, and hands the verdict to step 4 by name.
> - **No editorializing about SPEC §5 anywhere in the preamble or addenda.** The one mention of "SPEC §5" is the disclaimer above, not a claim about a prediction. Given that Step 4's transcript itself elects a `text-ui` token on frequency+recency rather than "electing 13px" verbatim, and never elects a token for the `.nl` arbitrary-pixel case at all, this framing had real room to tip into an early verdict on SPEC §5 prediction #2 — it does not.
> - **Addenda are scoped to exactly the three facts, and record both halves honestly.** Item 1 states plainly "the runner was not wrong" and separates "accurate about its own scope" from "the files exist two levels up". It also correctly notes this is the *same pattern* as step 2's baseline, now over two *additional* pointers (`ADMIN_UX_ROADMAP.md`, doc 47), not a re-report of the same two files.
> - **The verbatim-transcription discipline is stated and appears to have been honored.** The PROGRESS.md entry states explicitly that self-caught methodology mistakes in the runner's Step 2 were left in rather than cleaned up, and the transcript does in fact retain both false-count paragraphs before the corrected numbers.
> - **Fence handling for the doubly-fenced Markdown block is correct.** "## The DESIGN.md I would have written" is left inside the runner's own single ```markdown fence with no second fence added around it.

Findings, verbatim:

> #### Critical (Must Fix)
> None.
>
> #### Important (Should Fix)
> None.
>
> #### Minor (Nice to Have)
> - `work/design-skills-at-scale/PROGRESS.md` (new step-3 entry, "Files changed:" line): lists only `baseline-extracting-design-md.md (new)`, omitting `PROGRESS.md` itself even though this very entry is a change to that file. The step-2 entry's equivalent line lists both files. Cosmetic inconsistency between the two entries, no effect on evidence integrity.
> - `work/design-skills-at-scale/baseline-extracting-design-md.md` (addendum 1): "now observed a second time and independently, over two more pointers" is accurate but dense enough that a future reader has to work to parse "the first two of those files" vs. "two more pointers" as non-overlapping sets. A reader unfamiliar with step 2 would still get it right, just slower than necessary.

Controller cross-check, run independently of the review: a line-level diff of the transcript
region against the cold-runner's report shows 910 source lines, 910 transcript lines, and **zero**
changed lines.

**Step 3 closed: Approved on the first review, no fix round.**

### Deferred to work-verify triage (step 3)

- `PROGRESS.md` step-3 entry — its "Files changed:" line omits `PROGRESS.md` itself, unlike the
  step-2 entry. Cosmetic.
- `baseline-extracting-design-md.md` addendum 1 — "the first two of those files" vs "two more
  pointers" is accurate but dense; a reader unfamiliar with step 2 parses it slowly.

### Parent ruling — where a provisional frontmatter token lives (blocking ask, 2026-08-20)

Asked because PLAN step 4's reconciliation surfaced an architectural gap the SPEC did not
resolve and which changes what steps 6 and 8 build: SPEC §3.1 defines the `[provisional]`
marker only inside `## Decisions` entry text, while SPEC §4.5 requires every unconfirmed token
to ship `[provisional]` — and a frontmatter token has no defined way to carry it. Three options
were put, with A recommended. The parent's reply, verbatim:

> A — confirmed, with one binding addition your own evidence forces.
>
> WHY A. It is already the shape the owner approved. The worked DESIGN.md
> example reviewed and accepted before this dispatch carried exactly this:
> "#### Sistema" holding "- 2026-08-20 - [provisional] radius scale is
> 10/14/999px; 8/9/11/12/13px are migration targets". SPEC 3.4 already
> names "#### Sistema" as the home for "values and scales: tokens, radii,
> type, modes, reference-surface declaration". No new syntax, no new
> location, lint and generator untouched.
>
> WHY NOT B. A YAML comment creates a SECOND place to look for provisional
> status, and nothing reads it. Two marker locations is drift in the very
> convention these skills exist to prevent.
>
> WHY NOT C. Frontmatter is the compile source. A near-empty frontmatter
> compiles to a near-empty design.tokens.css, which forces every build back
> to raw values — the exact failure designing-consistently step 3 exists to
> stop. Honesty that breaks the compile is not honesty, it is a broken file.
>
> THE ADDITION — binding. You reported "sheet: 20px" emitted for a role
> where your own step 4 says verbatim "I did NOT elect a token for this
> role". That is NOT a marker-location problem, and option A must not be
> read as licensing it. Marking something provisional does not license
> inventing it. Two distinct rules:
>
> 1. Role WITH a genuine election that is merely unconfirmed → the token
>    goes in frontmatter (it must, to compile) AND gets a [provisional]
>    entry under "### Global" / "#### Sistema" naming the token and WHAT IT
>    BEAT, with the counts. Your near-tie is the model: card 14px at 984 vs
>    ~751 gets an entry carrying BOTH numbers, because the numbers are what
>    make a later promote-or-demote decidable. An entry that says only
>    "[provisional] card is 14px" is not enough.
>
> 2. Role with NO election → NO token in frontmatter at all, plus an open
>    question entry recording the role, the competing candidates and their
>    file locations. extracting-design-md step 5 already defines the
>    open-question form for exactly this: a thing the owner decides, not
>    you. Emitting "sheet: 20px" for a role you explicitly declined to
>    elect is the contamination this lane exists to fix, arriving through
>    the back door.
>
> Also note for your step 4 record: this baseline result CONFIRMS SPEC
> section 5's second prediction (that election runs on frequency and lands
> unconfirmed values as law). Record it as confirmed with this evidence
> rather than leaving it open.
>
> Record this ruling in your own DECISIONS.md before continuing. SPEC
> section 4 is unchanged — requirement 5 stands exactly as approved; this
> decides only where the marker lives and what may be emitted without one.

Recorded as two entries in `DECISIONS.md`. The second of those entries notes that the parent's
instruction to record SPEC §5 prediction 2 as CONFIRMED supersedes step 4's own
CONTRADICTED-on-the-literal-wording verdict, and that both facts stay in the record — they differ
only in what they take prediction 2 to assert, and the parent's verdict governs.

### Step 4 — step review (fresh reviewer, opus)

**Spec compliance:** ✅ Compliant.

**Assessment — Step quality: Approved.** Reasoning, verbatim: "All three verdicts hold against the
transcripts on the literal wording of their predictions, each separates the literal verdict from
the underlying defect with quoted support, and the load-bearing counterfactual-writes claim
survives a line-by-line count; the two SPEC amendments are additive, marked, dated, confined to
§3.5/§3.6, and leave §4 and every approved sentence untouched. The five Minors are precision and
framing, none of which changes a verdict or what steps 5-8 must build."

Ruling-by-ruling, the reviewer checked each verdict against the transcript rather than checking
that rulings merely existed. On prediction 1 it verified the load-bearing claim by counting:

> *The load-bearing third claim — verified.* The ruling asserts the run used its counterfactual convention four times for other blocked writes and never for an offer. I counted them in the transcript: (1) `:76-141` the full component; (2) `:143-152` the diff block; (3) `:162` "Decisions I would have recorded had the file existed"; (4) `:175-180` the whole `## What I would have committed` section. The count holds. Nitpick only: #4 partly recapitulates #1-#3, so "four separate times" is three distinct loci plus a summary section — the inference is unaffected.

On prediction 2 it named the trap the ruling avoided:

> **The 13px-in-frontmatter trap was not walked into.** The easy failure mode on P2 was to declare CONTRADICTED on Step 4 alone and never notice `small: '0.8125rem'` sitting in the emitted file. The ruling surfaces it and disposes of it on provenance rather than ignoring it.

Findings, verbatim:

> #### Critical (Must Fix)
> None.
>
> #### Important (Should Fix)
> None. No ruling asserts anything the transcripts do not support, and neither amendment reaches beyond marked commentary in §3.
>
> #### Minor (Nice to Have)
> 1. **`DECISIONS.md:14` and `SPEC.md:134` overstate their own cited evidence.** Both say the template repo "exists nowhere on this machine" / "does not exist on this machine (controller addendum 4)". Addendum 4 verifies something narrower: a **depth-2 search of the machine's work-repos folder**, plus three named candidate paths that fail to resolve. That is not a whole-machine search. *Why it matters:* the overstatement is now inside the approved SPEC, and the de-confounding rests on it. *Fix:* rephrase to "does not resolve at any plausible sibling location searched (controller addendum 4; SPEC §1)".
> 2. **One confound in prediction 1's evidence is never named.** `baseline-designing-consistently.md:52` ends *"I was told to proceed through steps 2–5 anyway and record what happens."* An offer is a halt-and-ask act; a runner instructed to proceed regardless is a fourth candidate cause for the missing offer, distinct from the three the ruling de-confounds. *Why the verdict survives anyway:* the runner attributes the non-offer to its own two causes and never to the proceed instruction; the counterfactual convention was precisely the tool for recording a would-be offer under a proceed instruction, and it was used four times for other things; and the amendment's load-bearing half — that step 1 produced no *discovery* either — does not depend on this at all. *Fix:* one clause naming and dismissing it.
> 3. **Prediction 2's headline framing slightly under-sells the confirming half.** "The defect appears one layer down instead, in the emitted frontmatter" locates the defect at the frontmatter. But `card`/14px was elected *at Step 4, on pure frequency*, over an ~751-use 8px family the baseline describes as *"a deliberate, documented v1.1 decision (Sanity/Stripe/Shopify comparison, not sloppy drift)"* later overridden without a completed migration. Frequency overriding a documented owner decision is the sharpest available statement of SPEC §1's third defect. *Fix:* no SPEC edit needed; the controller should carry the v1.1-deliberate-decision detail into step 6's dispatch as the eval's fixture.
> 4. **The three entries are 2061 / 2450 / 2784 characters against a prior file maximum of 778.** The `- date — choice — why` shape holds, so this is not a violation — but a reader in three months reaches for PROGRESS.md instead, and the DECISIONS entries become an archive rather than the index they are meant to be.
> 5. ⚠️ **One claim I could not verify from the material given.** Ruling 3 says the emitted Decisions sections carry "zero entries in the dated `- ` bullet form the format and the lint require". The zero-entries fact I confirmed directly. Whether `agent-lint` *fails* a `## Decisions` section containing no `- ` lines at all is not established by anything in the lane. Noting it so the claim is not inherited downstream as verified.

On the three items step 4 flagged for the controller rather than acting on, the reviewer judged
all three correctly flagged, and on the second:

> **Requirement 5's `[provisional]` marker has no defined carrier in frontmatter — flagging was right.** Closing it requires a new design decision, not a reconciliation of prediction against evidence — so it is outside this step's mandate. Escalating instead of quietly inventing a carrier was the correct restraint. This is the item most likely to block step 8; it should reach the owner, not just the controller.

That is what happened: the controller escalated it as a blocking question and the parent ruled.
The ruling is quoted verbatim above under "Parent ruling — where a provisional frontmatter token
lives".

### Step 4 — fix round 1 re-review (fresh re-reviewer, sonnet)

Minors 1 and 2 were ruled INTO the fix round (an overstatement sitting inside approved SPEC text,
and an unnamed confound in the very entanglement the step was chartered to resolve), together with
the parent's binding ruling. Minors 3, 4 and 5 were deferred.

**Verdict — Fix round: All findings addressed, no new Critical/Important breakage.** Verbatim:

> **1. Parent ruling / prediction 2 → CONFIRMED — ADDRESSED.** `SPEC.md:155-177` leads with a bold header stating `SPEC §5 prediction 2 CONFIRMED, per the parent's ruling`, so a skimming reader sees the governing verdict first, not a tie. The literal facts are introduced with "Evidence detail beneath that verdict, not a competing one" and the note goes on to say explicitly "Step 4 first recorded prediction 2 CONTRADICTED on that literal reading; the parent's verdict supersedes it, and nothing is falsified either way" — so the superseded verdict is named and subordinated *inside SPEC itself*. The open question the note used to pose is gone; in its place the parent's two rules are stated operationally: (i) unconfirmed-but-genuine election → frontmatter token **and** a `[provisional]` entry under `### Global`/`#### Sistema` naming the token and what it beat, "carrying both counts"; (ii) no election → **no** frontmatter token at all, plus an open-question entry recording the role, competing candidates and file locations. Both rules are concrete enough for a step-6 eval or step-8 skill author to act on without returning to the parent. The counts (984-vs-~751 for `card`) survive.
>
> **2. Overstatement rephrase — ADDRESSED.** `SPEC.md:135-136` now reads "does not resolve at any plausible sibling location searched (controller addendum 4; SPEC §1)". Checked against the actual addendum-4 text: it verifies a depth-2 search of the work-repos folder plus three explicitly named candidate paths failing to resolve — narrower than "this machine" as claimed. The new wording is accurate to that narrower scope and is not a second overstatement. `DECISIONS.md:14` is untouched, append-only.
>
> **3. Fourth confound — ADDRESSED.** `SPEC.md:138-140` adds "nor does the fourth candidate cause, the runner's instruction to 'proceed through steps 2–5 anyway' (same section), which the runner never cites as its reason and which that same counterfactual convention existed to work around," and closes with "That second half, the missing discovery, depends on none of the four causes." Verified against `baseline-designing-consistently.md:52` — quote is accurate.
>
> **Scope fence held.** SPEC §4 is untouched — all eleven requirements read identically to the approved text. No `PLAN.md` edit, no eval touched, nothing under `skills/`. Both deferred items were left alone.
>
> ### New breakage in the fix diff
> None in the diff's own content. **Minor (process, non-blocking):** the fix-diff artifact supplied for this review is stale relative to the live repo — a second pass by a fresh seat added three sentences the diff does not show. It does not misrepresent anything falsely (the missing text only strengthens the note), but a reviewer who checks only the supplied diff will certify an older version of §3.6 than what's actually committed. Recommend future rounds supply the full current state rather than a single named diff file, especially when a fix round spans more than one dispatched seat.
>
> ### Out-of-scope observations
> `SPEC.md:42` (§1, untouched, correctly out of this round's fence) states "`../Context-Engineering` does not exist — verified" with the same unqualified "does not exist" phrasing that item 2 just corrected in §3.5. It is the fact the §3.5 rephrase leans on, so the two sentences now sit at different levels of precision for what appears to be the same claim. Worth a future pass reconciling it with the narrower addendum-4 evidence.
>
> ### Verdict
> **Fix round:** All findings addressed, no new Critical/Important breakage.

**Controller cross-check, run independently.** `git diff c56f316 HEAD -- SPEC.md`, filtered to
lines that are not blockquote content, returns a single `+>` (an empty note line). Every other
change to `SPEC.md` since the owner approved it at `c56f316` sits inside a `> ` blockquote note.
The approved prose — including all eleven §4 requirements — is byte-identical to what the owner
approved. Both fix commits (`6798ec7`, `39e08bd`) delete only `> ` lines.

**Step 4 closed: Approved, then 1 fix round carrying the parent's binding ruling.**

#### Controller process defect, recorded because a reviewer caught it

The re-review packet handed to the reviewer was a single named diff file for one commit, while the
fix round had in fact spanned two commits across two seats — the first seat became unresponsive
mid-round and a fresh seat picked it up, landing a second pass. The reviewer noticed the live file
carried text its diff did not, traced it, verified the extra text independently, and flagged the
packaging rather than certifying the stale version. Its recommendation is adopted for the rest of
this lane: **package review diffs as the full range from the step's base to HEAD, not a single
commit**, and say which commits the range spans.

### Deferred to work-verify triage (step 4)

- SPEC §3.6's "one layer down" framing under-sells the confirming half of prediction 2. No SPEC
  edit needed; the controller carries the v1.1-deliberate-decision detail into step 6's dispatch
  as the eval fixture, which is where the reviewer said it belongs.
- The three step-4 `DECISIONS.md` rulings run 2061 / 2450 / 2784 characters against a prior file
  maximum of 778. Format holds; readability suffers.
- ⚠️ Ruling 3's claim that the lint *requires* dated `- ` entries under `## Decisions` is not
  established by anything in the lane. Not inherited downstream as verified.
- `SPEC.md:42` (§1) carries the same unqualified "does not exist — verified" phrasing that §3.5
  was just corrected away from, and is the fact §3.5's rephrase leans on. §1 is approved text
  outside step 4's editable section, so it was correctly left alone.

### Step 4 — fix round 1, SECOND re-review seat (independent, sonnet)

A second re-review seat had been dispatched while the first appeared unresponsive. Both later
returned, independently, and **agree on every item**. The second seat's verdict, verbatim:

> **Fix round:** All findings addressed, no new Critical/Important breakage.

It reached that verdict by reading the files as they stand on disk rather than the supplied diff,
and flagged the same packaging defect the first seat did:

> `step-04-fix1.diff` captures only commit `6798ec7d` (fix round 1's first pass). … I read the SPEC.md and DECISIONS.md files as they currently stand on disk (not just the diff), which is the ground truth that includes this second pass. … This is not a new defect — the second pass is a real improvement, and it is exactly what closes the "is the open question genuinely closed" judgment call — but flagging it since the diff file alone would under-represent the current text.

On the half most at risk of being softened, it was explicit:

> This is operational: a step-6/8 author can act on it without going back to the parent, and the "no token at all" half — the one likeliest to get softened — is stated unambiguously.

And it checked the residual-hedge question directly:

> No residual "open"/"for the controller"/"not decided here" language remains anywhere in the note (checked both §3.5 and §3.6 text in full); the one occurrence of "open question" (SPEC.md:163) is a factual description of the transcript's own wording, not a hedge on the SPEC's verdict.

#### Correction to the seat-failure record above

The controller earlier treated the step-4 fix seat as unresponsive and dispatched a fresh one. That
was wrong in one respect and should not stand uncorrected: the original seat had in fact completed
its work and committed `6798ec7` — only its REPORT failed to arrive. It later resent the report
unprompted, and correctly flagged that an uncommitted working-tree edit had appeared in `SPEC.md`
that was not its own, leaving it in place rather than reverting or committing it. That edit was the
fresh seat's second pass, which the fresh seat then committed as `39e08bd`. So both commits are
accounted for, the working tree is clean, and no edit was orphaned or lost. The failure in this
lane has consistently been report DELIVERY, never the work itself — worth stating plainly, because
"the seat is dead" and "the seat's message did not arrive" call for different responses, and the
controller acted on the wrong one.

### Step 5 — step review (fresh reviewer, opus)

**Spec compliance:** ✅ Compliant.

**Assessment — Step quality: Approved.** Reasoning, verbatim: "All three files deliver exactly what
step 5 asked for, and every RED claim I checked — eleven zero-occurrence counts, three SKILL.md
quotes, five baseline citations — is true as stated, with the thin-baseline cases labeled honestly
inside the evals rather than laundered. Nothing in the three files a correct step-7 rewrite would
fail; the one contradiction in the directory is the pre-existing `eval-03.md` box 2, correctly
flagged and out of this step's charter, though it now needs a controller routing decision since no
remaining PLAN step owns that file."

**RED verification — the part that mattered.** The reviewer read today's `SKILL.md` in full and
counted every term the RED sections assert is absent:

> ```
> provisional 0 · "reference surface" 0 · rank 0 · trust 0 · Global 0 · global 0
> sibling 0 · promote 0 · demote 0 · escalat 0 · exception 0        (Decisions: 9)
> ```
> Every zero-occurrence claim in all three files is **true as stated**. No overclaim found.

Per-eval, verbatim:

> **`eval-01.md` — named RED: boxes 1 and 2 (promote / demote).** ✅ Confirmed failing. Today's step 4 (`SKILL.md:45-52`) is additive by construction … With `promote`/`demote`/`provisional` all at zero occurrences there is no status to change. The eval's second, sharper argument also checks out: `SKILL.md:41-43` reads "A standing decision the work conflicts with is renegotiated with the user — never silently overridden", with no exemption for a candidate entry, so a faithful run *halts* on the below-the-table entry rather than demoting it — box 2 fails in both directions. … The file's closing paragraph states honestly that the cold run found no DESIGN.md at all, so no provisional entry existed to repair — the RED rests on the skill text, and it says so. That is the right way to write a partly-textual RED.
>
> **`eval-04.md` — named RED: box 1 (ranked inventory at step 1).** ✅ Confirmed failing. `SKILL.md:26-29` is locate-or-instantiate only; nothing discovers, ranks or inventories. … **The controller's ruling is honored correctly**: box 5 is marked *inside the file* as a regression guard that passes trivially against the unfixed skill, and the RED is explicitly taken from the positive clause — "discovery happens, and it is ranked". This is exactly what the ruling required, and it is stated in the eval, not only in PROGRESS.
>
> **`eval-05.md` — named RED: box 1 (`### Global` read first), box 2 behind it.** ✅ Confirmed failing. … The honesty here is the best in the set: it states that the cold run could **not** exercise slicing, cites the step-4 ruling recording it **unobserved**, and reaches for the sibling baseline as the nearest direct observation rather than pretending the dc baseline proved it. Verified against both source documents.

**Evals a correct step-7 rewrite would fail**, verbatim:

> **1. `eval-03.md:15-16` — the flagged one. Confirmed.** … A step-7 rewrite emitting `### catalogo` / `#### /catalogo/inventario — Inventario` fails this box as literally worded. The implementer's analysis is correct and it was right not to rewrite another requirement's contract unasked.
>
> *Routing point for the controller, beyond what the implementer could see:* no remaining PLAN step is chartered to touch this file. Step 9's `[batch]` sweep lists `skills/extracting-design-md/evals/eval-03.md`, **not** `skills/designing-consistently/evals/eval-03.md`, and step 7 is scoped to `SKILL.md`. Unless the controller opens it, step 7 will be built against a five-eval contract one of whose evals contradicts SPEC §3.3.
>
> **2. `eval-02.md` — no, the implementer is right.** I read it. It exercises step 3 only … Nothing in it a correct rewrite fails.
>
> **3. In the three files this step wrote — none.** I checked every box in eval-01, eval-04 and eval-05 against SPEC §3.1-§3.5 … Every fixture heading in both files is in §3.3/§3.4 shape, and both global entries are written as rules naming no route, respecting §3.4's guard rail.

Constraints were verified by the reviewer with its own greps: no SKILL.md touched, no
`SPEC.md`/`PLAN.md`/`feature_list.json` edit, no machine-anchored path in `skills/` (explicit grep
for drive-rooted, WSL and POSIX-home forms returned no hits), fixtures runtime-neutral (grep for
framework/product/filename identities returned no hits) while every baseline number is preserved,
and no `## Validation log` invented.

Findings, verbatim:

> #### Critical (Must Fix)
> None.
>
> #### Important (Should Fix)
> None attributable to this step. The one Important-class item in the directory is `eval-03.md:15-16`: it is pre-existing, was correctly flagged rather than silently rewritten, and needs a controller routing decision because no remaining PLAN step owns that file.
>
> #### Minor (Nice to Have)
> 1. **`eval-01.md:10` — forward reference in the fixture.** It opens "The same multi-module admin", but eval-01 is read first in the directory and the admin is introduced in eval-04.
> 2. **`eval-01.md:27` — the work's link to the promoted entry is implicit.** "builds the panel on the existing card radius" requires the reader to connect "existing card radius" to the 14px provisional in the Sistema entry.
> 3. **`eval-01.md:32-34` — box 1 promotes a `### Global` provisional off one module's work.** Defensible and I believe correct: SPEC §3.1 rule 3 is mandatory ("must promote it") and §3.2's "only the owner or a reference surface confirms" sits under the *discovery* heading, so promotion-by-work is a separate channel. But a step-7 author reading §3.2 as governing all confirmation could write "promotion needs a reference surface or the owner" and fail this box.
> 4. **`eval-01.md:35-43` — boxes 2 and 3 prescribe two dispositions for the same entry.** §3.1 rule 2 exempts provisional entries from never-silently-drop, so a rewrite could legitimately *delete* the entry as redundant with the global and record why — which fails box 2 as literally worded.
> 5. **`eval-01.md:48-50` — box 5 largely collapses into boxes 1-2.**
> 6. **`eval-04.md:45-49` — box 3 says "the owner is asked" while the fixture (`:33-34`) says the owner is not available mid-run.**
> 7. **`eval-04.md:38-44` — boxes 1 and 2 overlap on "ranked".**
> 8. **`eval-05.md:51-53` — box 5 can only fail a run that already failed box 3.** It works as a second-order trap for the read-everything answer, which is legitimate; worth one clause saying so, so a grader does not read a vacuous pass as evidence.
> 9. **`eval-05.md:33` — the fixture entry ends with a literal `<why>` placeholder.**

Controller response: Minors **3, 4, 6 and 8** were ruled INTO fix round 1, because each is a place
where a step-7 author building faithfully to SPEC §3 could fail a box for the wrong reason — the
one defect class that matters most when the eval set IS the rewrite's contract. Minors 1, 2, 5, 7
and 9 are readability and box-independence polish, deferred. The `eval-03.md` routing decision was
ruled before the review returned and is recorded in `DECISIONS.md`: it is fixed in step 5's fix
round, not left for step 7, because an eval a correct rewrite fails is a broken eval and this
repo's constraint puts eval changes before skill content.

### Deferred to work-verify triage (step 5)

- `eval-01.md:10` forward-references the multi-module admin that eval-04 introduces; the fixture
  never states scale.
- `eval-01.md:27` leaves the link between "existing card radius" and the 14px provisional implicit.
- `eval-01.md:48-50` box 5 largely collapses into boxes 1-2; its independent content is the last
  sentence.
- `eval-04.md:38-44` boxes 1 and 2 overlap on "ranked", so they are not independently checkable.
- `eval-05.md:33` fixture entry ends with a literal `<why>` placeholder rather than a real reason.

### Step 5 — fix round 1 re-review (fresh re-reviewer, sonnet)

**Verdict — Fix round: All findings addressed, no new Critical/Important breakage.**

All five items ADDRESSED. The two that mattered — the ones where a faithful step-7 rewrite could
have failed a box for the wrong reason — were checked for the opposite failure too, which is the
adversarial move that made this re-review worth its seat:

> **3. Minor 4 — `eval-01.md` boxes 2 and 3 disposition ambiguity — ADDRESSED.** … correctly permits deletion-with-record as an alternative to replace-with-correction. I checked whether this widens the box into unfalsifiability: it does not — a run that leaves the stale entry untouched still fails (neither replaced nor removed), and a run that silently deletes without recording why still fails ("recorded" is a stated requirement, not optional). The box still discriminates a correct rewrite from an incorrect one; it is tightened, not hollowed out.

> **2. Minor 3 — `eval-01.md` box 1 promotion channel — ADDRESSED.** … The two SPEC clauses genuinely govern different situations, and the box now says so self-containedly rather than citing section numbers that wouldn't resolve inside a shipped `skills/` file. A step-7 author can no longer read §3.2 as overriding §3.1's promotion-by-work and fail this box for the wrong reason.

> **1. `eval-03.md`'s flat heading — ADDRESSED.** … This matches SPEC §3.3's `### <module>` then `#### <route> — <page>` shape exactly. Boxes 1 and 3 are byte-identical to the pre-fix version — the eval's subject (completion blocked on an unrecorded decision) is untouched, only the heading shape addressed. `eval-02.md` confirmed untouched.

> **4. Minor 6 — `eval-04.md` box 3 alignment — ADDRESSED.** … The old "the owner is asked" (which implied a live, synchronous ask the fixture rules out) is gone.

> **5. Minor 8 — `eval-05.md` box 5 vacuity disclosure — ADDRESSED.** … Both statements agree; a grader now cannot mistake a vacuous pass on box 5 for evidence the skill handles exceptions correctly.

**Still RED — independently re-counted, not taken on the implementer's word:**

> ```
> provisional 0 · promote 0 · demote 0 · escalat 0 · Global 0
> sibling 0 · reference surface 0 · rank 0 · trust 0
> ```
> All nine are zero, matching the implementer's claim exactly.

Per-file it confirmed the rewordings did not disturb the RED mechanism, and made a correct call
about `eval-03.md`, which carries no RED section:

> **eval-03.md** — has no `## Why this is RED today` section at all. `git log --diff-filter=A` shows this file originated in the original skill-authoring commit, predating the lane's steps 5-6 and the `DECISIONS.md` ruling that binds *evals written at steps 5-6* to carry an explicit RED box. It is therefore not subject to that requirement, and the fix round correctly left it without one rather than fabricating a RED claim to satisfy the letter of a rule that doesn't apply to it.

**Scope fence: held.** Verified with its own path-scoped diffs that `SKILL.md`, `eval-02.md`,
`SPEC.md`, `PLAN.md` and `feature_list.json` are all empty diffs across the range; that no
machine-anchored path appears in the four touched eval files; and that all five deferred Minors
were left untouched, naming each one's surviving line.

**New breakage: none at Critical or Important.** One Minor observation, with the reviewer's own
judgment that it should be left:

> `eval-01.md` box 2's new "may be its removal, recorded" alternative doesn't say *where* a deletion should be recorded … This is underspecification, not a false-pass risk (a silent, unrecorded deletion still fails the box as worded), so it doesn't rise to the "widened until it cannot fail" defect class the round was checking for. Judgment call: leave as-is.

**Step 5 closed: Approved, then 1 fix round (cap 5, not reached).**

#### Controller error in this step, recorded because the history carries it

The controller ran `git add -A` for a PROGRESS-only bookkeeping commit while the step-5 fix
implementer held the working tree. Commit `eda82ca`, whose message says only "record the step-5
review verdict verbatim", therefore also contains all four of that round's eval edits. Nothing was
lost and no history was rewritten — the branch is shared — so the mis-attribution is recorded here
and in `DECISIONS.md` instead. The implementer was told immediately so it would not read its own
clean tree as a failed edit and redo the work, and it independently audited the range and reported
the same thing. The controller now stages explicit paths.

### Deferred to work-verify triage (step 5, carried forward)

- `eval-01.md:10` forward-references the multi-module admin that eval-04 introduces.
- `eval-01.md:27` leaves the link between "existing card radius" and the 14px provisional implicit.
- `eval-01.md` box 5 largely collapses into boxes 1-2.
- `eval-01.md` box 2 does not specify WHERE a recorded deletion is recorded — underspecification,
  not a false-pass risk; the re-reviewer's judgment was to leave it.
- `eval-04.md` boxes 1 and 2 overlap on "ranked".
- `eval-05.md:33` fixture entry ends with a literal `<why>` placeholder.

### OWNER INSTRUCTION — 2026-08-20 — per-step reviewer dropped for steps 6-10

Received mid-lane, through the parent, and recorded here because a lane that silently stops
running a standard rung looks like a skipped gate later. This is the owner's call, not a
self-granted shortcut. The instruction, verbatim:

> Owner instruction, effective now, overriding the brief on this point only.
>
> STOP running work-run's per-step reviewer after each implementation step. Do not run a review subagent per PLAN step for steps 6 through 10. Implement them straight through.
>
> Review happens ONCE, at the end: step 11's work-verify, including its step-4 fresh-context review over the whole lane. That rung stays mandatory and is NOT waived — the owner removed the per-step layer, not verification. My cross-model reviewer still runs after your worker_done, so the lane still gets two independent review layers at close.
>
> Three things I need from you on this:
>
> 1. RECORD IT IN PROGRESS.md as an owner instruction with today's date, naming what was dropped and what was kept. A lane that silently stops running a standard rung looks like a skipped gate later; a lane that records the instruction is just following the owner. Do not let this read as a self-granted shortcut.
>
> 2. STEP 6 CARRIES THE RISK, so be deliberate there. Step 8 is written to PASS step 6's evals — a wrong eval there propagates straight into the rewrite, and now nothing catches it until step 11, where the fix is 'redo 6 and 8' rather than 'fix one eval'. Give step 6 your own care in place of the reviewer: re-derive the RED claims yourself the way step 5's reviewer did, and state those counts in PROGRESS.
>
> 3. The audit I asked for in my last message still stands — all evals in extracting-design-md, not just the PLAN's named set, and report the delta in worker_done.
>
> Speed is the point of this change. Do not spend the savings on extra self-checking that recreates the layer by another name.

**What was DROPPED:** work-run's per-step reviewer and its fix-round re-reviewer, for PLAN steps
6 through 10 only. Steps 1-5 ran the full rung and their verdicts are recorded above.

**What was KEPT, and is not waived:** step 11's `work-verify`, including its step-4 fresh-context
review over the whole lane; and the parent's cross-model adversarial reviewer after `worker_done`.
Two independent review layers still close the lane.

**One seat straddled the instruction.** `rev-step6` was dispatched before this message arrived. Its
verdict is read and recorded rather than discarded — the work was already done, and it doubles as
coverage for the audit in point 3. No per-step reviewer was dispatched after the instruction
landed, and no fix-round re-review was run for steps 6-10.

**On point 2 — the controller's own RED re-derivation for step 6** is recorded with step 6's entry,
counted directly against `skills/extracting-design-md/SKILL.md` rather than taken from the
implementer's report.

### Controller's own step-6 work, standing in for the dropped reviewer

**RED re-derivation, counted directly against `skills/extracting-design-md/SKILL.md` (87 lines)**
rather than taken from the implementer's report, as the owner instruction requires:

```
provisional 0 · "reference surface" 0 · tie-break 0 · Global 0
Sistema 0 · "open question" 0 · rank 0
```

`confirm` appears twice, both times as owner confirmation ("Confirm names and palette intent with
the owner"), never as reference-surface confirmation. And the frequency criterion is live at
`SKILL.md:46` — "**4. Propose tokens.** Semantic role + frequency decide: the dominant or …".
So every RED claim in step 6's eval set holds: the vocabulary the new evals require is absent, and
the rule they are written to replace is present and quotable.

### Generalized eval audit (parent instruction), across BOTH skills — all eleven evals

Swept for the defect class the parent named: any eval whose fixture or expected-behavior box
hard-codes something the approved design replaces. The four known replacements were checked
individually across `skills/*/evals/*.md`.

| Replacement | Finding |
|---|---|
| flat `### <surface>` Decisions shape (SPEC §3.3) | **1 live instance**: `extracting-design-md/eval-02.md` box 1, "under its surfaces". Every other Decisions heading in either skill is already in `### <module>` → `#### <route> — <page>` form — `designing-consistently` eval-01, eval-03 and eval-05 all verified in the new shape. |
| absence of a `### Global` tier (SPEC §3.4) | none — the tier appears wherever it is relevant. |
| frequency as the token criterion (SPEC §3.6) | none surviving. `extracting-design-md/eval-01.md` was rewritten this step and now states it outright: "Counts measure spread; they do not elect. No variant is named 'the token' for leading the count". The remaining `frequency` mentions are the new tie-break framing or quotations of the old skill text inside `## Why this is RED today` sections. |
| "locate the DESIGN.md, else instantiate" framing (SPEC §3.5 step 1) | none — the only `instantiat` hits are `designing-consistently/eval-04.md`'s box 5, which carries the **new** framing ("instantiating is the last option"). |

**Delta beyond the PLAN's named set: 2 evals.** `designing-consistently/eval-03.md` (fixed in step
5's fix round under the orphan-eval ruling) and `extracting-design-md/eval-02.md` (fixed in step
6's fix round). No third instance exists; the sweep is closed.

**A ruling of mine was wrong and is corrected here.** Step 6's implementer flagged
"`eval-03.md`'s second box hard-codes the flat `### catalogo/inventario` heading", and I ruled it
into step 6's fix round before verifying. The audit shows `skills/extracting-design-md/evals/eval-03.md`
contains no such heading and no `###` heading at all — the flag described
`skills/designing-consistently/evals/eval-03.md`, which step 5 had already fixed. The flag
mis-attributed the file; my ruling inherited the error because I acted on it before checking. The
ruling is withdrawn in `DECISIONS.md`, and nothing was changed on a false premise. What
`extracting-design-md/eval-03.md` does carry is the stale `context-lint` reference, which PLAN
step 9 owns and which stays for that step.

### Step 6 — closed without a per-step reviewer, per the owner instruction

`rev-step6` was dispatched a few minutes BEFORE the owner's instruction to drop the per-step
reviewer arrived. It never delivered a verdict — the same report-delivery failure this lane has hit
repeatedly — and it was **not re-dispatched**, because by then the owner had removed that layer for
steps 6-10. So step 6 carries no reviewer verdict, and that is the instruction being followed, not
a gate quietly skipped. What stands in its place, both recorded above:

- the controller's own RED re-derivation counted directly against today's
  `skills/extracting-design-md/SKILL.md`, and
- the generalized eval audit across all eleven evals in both skills.

Step 6's own fix round closed on the discriminating check the controller asked for, in the
implementer's words: "re-checked and confirmed a confirmed entry under a correct module heading
FAILS box 1 while a `[provisional]` entry at the module/route address citing the files PASSES". A
box that fails the wrong answer and passes the right one is the whole test; that is the evidence
the reviewer would have been asked to produce.

Step 6 result: 6 evals in `skills/extracting-design-md/evals/`, both acceptance commands exit 0.
`eval-01.md` rewritten so counts measure spread rather than elect; `eval-05.md` new (reference
surfaces elect, frequency only breaks ties); `eval-06.md` new (provisional by default, including
the rule that a role with no election gets no frontmatter token at all); `eval-02.md` repaired for
both the missing `[provisional]` marker and the flat per-surface addressing. `eval-03.md` and
`eval-04.md` untouched, under a withdrawn ruling and a deferred one respectively.

## In progress

## Tried and failed

## Next

## Verification

<!-- PASS evidence only, written by work-verify (newest on top); the close
     handoff refuses to close a lane without a current PASS block here. -->

<!-- First read of every session. If it isn't here, it didn't happen. -->
