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

## In progress

## Tried and failed

## Next

## Verification

<!-- PASS evidence only, written by work-verify (newest on top); the close
     handoff refuses to close a lane without a current PASS block here. -->

<!-- First read of every session. If it isn't here, it didn't happen. -->
