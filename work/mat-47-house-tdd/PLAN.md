# PLAN — MAT-47 house TDD skill

Spec: [SPEC.md](SPEC.md) · Decisions: [DECISIONS.md](DECISIONS.md)

## Constraints (every step)

- **Evals before content.** No commit adds `skills/testing-first/SKILL.md`
  or its reference before the commit that adds the evals. The order must
  be provable in `git log`, so the eval commit is its own commit.
- **Fences.** Do not touch `skills/tracing-root-causes/**` (MAT-46 in
  flight), anything in the Agent-Engineering repo, or `AGENTS.md` — the
  standard lints skill enumeration in AGENTS.md as an anti-pattern.
- **Lint baseline.** `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  from the worktree root exits 1 on clean main with exactly one
  pre-existing finding. Owner ruling (recorded in DECISIONS.md, D-002):
  acceptance is zero findings *attributable to this lane* — the summary
  line must stay `0 high, 1 medium, 0 low — FAIL` and the single finding
  must stay `AGENTS.md:15 ... [cmd-drift]`. Every step below uses that as
  its lint acceptance; CI `standard` on the PR is the authoritative gate.
- **Never merge.** Push and open the PR; the parent merges.

## Steps

1. **[mechanical]** Open the lane: `SPEC.md`, `PLAN.md`, `DECISIONS.md`
   (D-001 naming, D-002 lint ruling), `PROGRESS.md` carrying the clean-main
   lint baseline evidence. Commit alone, before any `skills/` file exists.
   *Acceptance:* `ls work/mat-47-house-tdd/` lists all four files; `git show --stat HEAD`
   touches nothing under `skills/`; lint summary line unchanged.

2. **[judgment]** Write ≥3 evals to `skills/testing-first/evals/eval-01.md`
   … `eval-0N.md` — repo eval shape (`## Query`, `## Fixture`,
   `## Expected behavior` checklist), each targeting one adapted behavior
   the upstream skill does NOT produce: (a) evidence written into the
   lane's PROGRESS.md per cycle, (b) refusal to self-claim done — hand-off
   to `work-verify` instead, (c) the Iron Law surviving a plausible
   rationalization under time pressure. Commit alone; no SKILL.md, no
   `references/` in this commit.
   *Acceptance:* `git show --stat HEAD` lists only `evals/` files; `ls skills/testing-first/`
   shows `evals` and nothing else; lint summary line unchanged.

3. **[judgment]** Write `skills/testing-first/references/writing-good-tests.md`
   — adapted from the upstream reference: keep name-the-break, exercise-the-real-thing,
   both gate functions, the mutation check, the warning signs; drop the
   suite-internal pointers; land it before SKILL.md so no commit ever
   carries a dangling link. Reference >100 lines opens with a table of
   contents (AE authoring rule).
   *Acceptance:* `wc -l` on the file; lint summary line unchanged (a
   `broken-link` or `skill-*` finding here would move it).

4. **[judgment]** Write `skills/testing-first/SKILL.md` — Iron Law,
   RED→GREEN→REFACTOR with both verify beats mandatory, rationalization
   table (plus the AE-specific excuses), red flags, and the AE hand-off
   section replacing the upstream "before marking work complete"
   checklist. Links the step-3 reference at
   `references/writing-good-tests.md` — exactly one level deep.
   *Acceptance:* `node -e` line count <500; frontmatter has third-person
   `description` with what + when; lint summary line unchanged.

5. **[mechanical]** Index surfaces in `README.md`: add the `testing-first/`
   row to the Skills table, and drop TDD from the "Used alongside"
   superpowers line (that line is also MAT-46's target for "debugging" —
   expect a rebase conflict and keep the edit surgical). `AGENTS.md` stays
   untouched.
   *Acceptance:* `grep -c 'testing-first' README.md` ≥ 1;
   `grep -c 'testing-first' AGENTS.md` = 0; lint summary line unchanged.

Cycle tail (not steps): `work-verify` records the evidence into
PROGRESS.md, then `work-handoff` commits, pushes, opens the PR with
`Closes MAT-47`, and posts the tracker update.
