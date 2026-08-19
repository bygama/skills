# PLAN — MAT-94

Lane: `work/mat-94-attribution-skills/` · Tier M · SPEC approved (D1)

## Constraints (every step)

- Comments, README, and an optional root NOTICE only — no graded
  behavior changes anywhere, therefore no eval changes (the evals-first
  hard constraint is honored because nothing beyond notices is in
  scope). If a step finds itself wanting a behavior edit, it stops and
  escalates instead.
- Fenced: `CODE_OF_CONDUCT.md`, `LICENSE`, `AGENTS.md`, all skills
  other than testing-first and tracing-root-causes, everything in the
  AE repo (the sibling lane owns it).
- Upstream ground truth:
  `C:/Users/mateo/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/skills/`.
  A missing diff target means stop and escalate — never classify from
  memory.
- Classification verdicts are evidence-first: quoted verbatim runs /
  structural parallels in DECISIONS.md, never a bare "looks similar".
- Notice format: SPEC's normalized block; per D1 the per-file comment
  stays short and points at NOTICE for the full MIT permission text
  when NOTICE exists.
- `skills/*/SKILL.md` stays <500 lines; frontmatter untouched.
- Lint acceptance: `node
  C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` PASS
  (exit 0), no findings attributable to this lane.
- All artifacts in English.

## Steps

1. **[judgment] Classify testing-first against upstream v6.3.0.** Diff
   `skills/testing-first/SKILL.md` against upstream
   `test-driven-development/SKILL.md`, and
   `skills/testing-first/references/writing-good-tests.md` against
   upstream `test-driven-development/writing-good-tests.md`. Record in
   DECISIONS.md under the exact heading `## D2 — classification:
   testing-first`: what was diffed, quoted matches (verbatim runs,
   structural parallels — Iron Law and rationalization table are the
   expected hot spots), one verdict per file
   (substantial / idea-only), the action each verdict implies. No
   skill file is edited in this step.
   *Acceptance:* `grep -q "## D2 — classification: testing-first"
   work/mat-94-attribution-skills/DECISIONS.md` → exit 0, AND `git
   diff --name-only HEAD` touches no `skills/**` file in this commit.
   *Commit:* `docs(lane): D2 — classify testing-first vs upstream
   v6.3.0`.

2. **[judgment] Classify tracing-root-causes, section by section.**
   Scope: ONLY the MAT-46 additions (commits 9d1b574, c147b56, 0e11081
   — the phased spine, the iron law, the rationalization tables, and
   `references/techniques.md`'s four techniques) plus the ask-for-help
   section the branch base carries (PR #12). The 57-line owner-original
   base (`git show 9d1b574~1:skills/tracing-root-causes/SKILL.md`) is
   NOT a candidate. Diff each section group against upstream
   `systematic-debugging/SKILL.md` and its technique references
   (`root-cause-tracing.md`, `condition-based-waiting.md`,
   `defense-in-depth.md`, `find-polluter.sh`). Record in DECISIONS.md
   under the exact heading `## D3 — classification:
   tracing-root-causes`: per-section verdicts with quoted evidence; a
   partial-substantial outcome lists the exact section names a notice
   must carry. No skill file is edited in this step.
   *Acceptance:* `grep -q "## D3 — classification: tracing-root-causes"
   work/mat-94-attribution-skills/DECISIONS.md` → exit 0, AND `git
   diff --name-only HEAD` touches no `skills/**` file in this commit.
   *Commit:* `docs(lane): D3 — classify tracing-root-causes sections vs
   upstream v6.3.0`.

3. **[judgment] NOTICE judgment — decide, then create iff yes.** Read
   D2 + D3 verdicts. If any file/section is substantial, judge whether
   MIT's "notice shall be included in all copies or substantial
   portions" wants a root NOTICE carrying the upstream copyright line
   + full MIT permission text + the enumerated derived files; record
   the judgment (yes or no, with reasoning) in DECISIONS.md under the
   exact heading `## D4 — NOTICE judgment`, and create `NOTICE` at the
   repo root iff yes. Per D1, NOTICE is where the full permission text
   lives.
   *Acceptance:* `grep -q "## D4 — NOTICE judgment"
   work/mat-94-attribution-skills/DECISIONS.md` → exit 0; AND if the
   judgment is yes, `grep -q "Permission is hereby granted" NOTICE` →
   exit 0.
   *Commit:* `docs: D4 — NOTICE judgment for substantial ports` (file
   included iff created).

4. **[mechanical] Apply/normalize the per-file notices.** Exactly per
   D2 + D3 verdicts and the SPEC's format: normalize the two existing
   testing-first notices (`SKILL.md:8-9`,
   `references/writing-good-tests.md:6-7`) in place — one block each,
   never stacked; extend tracing-root-causes' existing provenance
   comment (SKILL.md lines 10-14) with the parts-named notice iff D3
   says so; add/extend a notice in
   `skills/tracing-root-causes/references/techniques.md` iff D3
   classifies its techniques substantial; idea-only verdicts get NO
   notice. Where NOTICE exists (D4), each comment points there for the
   full text.
   *Acceptance:* lint PASS per constraints, AND `wc -l` of both
   SKILL.md files < 500, AND `grep -c "Copyright (c) 2025 Jesse
   Vincent"` per touched file equals exactly the count D2/D3 verdicts
   demand (no stacked duplicates).
   *Commit:* `docs(attribution): normalize per-file upstream notices
   per MAT-94 classification`.

5. **[mechanical] README Provenance lines.** Rewrite the affected
   lines of README's existing `## Provenance` section so each
   classified skill carries exactly one line/entry: verdict
   (derived-with-notice / idea-only-rewritten, per D2/D3) + evidence
   pointer (`work/mat-94-attribution-skills/DECISIONS.md`; plus NOTICE
   if D4 created it). Honest and small — no marketing, no growth
   beyond the classified skills.
   *Acceptance:* `grep -q "testing-first" README.md && grep -q
   "tracing-root-causes" README.md` (both within `## Provenance`,
   verified by reading), AND lint PASS per constraints.
   *Commit:* `docs(readme): provenance lines per MAT-94
   classification`.

## Interfaces between steps

- Steps 3, 4, 5 consume DECISIONS.md verdicts by the exact headings
  `## D2 — classification: testing-first` and `## D3 — classification:
  tracing-root-causes`.
- Step 4 consumes D4's outcome (`## D4 — NOTICE judgment`, and the
  root `NOTICE` file iff created) for the pointer inside each per-file
  comment.
- Step 5 consumes D2/D3 verdicts and D4's outcome for the README
  lines' wording and pointers.

## After the steps

work-verify (M): static + behavioral layers, step-4 fresh-context
review with verdict text recorded in the lane; then work-handoff — PR
open with base `bygama/mat-93-ask-for-help-leg`, body `Closes MAT-94`,
Linear attach, worker_done. Never merged by this lane.
