# PLAN — tracing-root-causes absorbs systematic-debugging

Spec: [SPEC.md](SPEC.md) · Rulings: [DECISIONS.md](DECISIONS.md) ·
Evidence: [PROGRESS.md](PROGRESS.md)

## Constraints — apply to every step

1. **Evals-first, provable in history.** Step 1's commit contains ONLY
   eval files and lands strictly before any commit touching SKILL.md or
   `references/`. `git log --stat` is the proof; a claim in PROGRESS is
   not.
2. **Fenced files — never staged by any step:** `README.md`,
   `AGENTS.md`, any directory under `skills/` other than
   `skills/tracing-root-causes/`, and anything in the Agent-Engineering
   repo. Needed index wording is reported in PROGRESS, not applied
   (D4).
3. **AE authoring rules** (`reference/skills.md`): SKILL.md <500 lines;
   references exactly one level deep from SKILL.md (a reference file
   links no further reference file); a reference >100 lines opens with
   a table of contents; `description` in third person, ≤1024 chars;
   forward slashes in every path.
4. **Absorption, not replacement.** `name: tracing-root-causes` and the
   OMC provenance comment survive verbatim; the three existing evals
   are not weakened.
5. **Lint invocation** is by absolute path — `node
   C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .` — and
   the gate is scoped per D2: zero findings attributable to this lane,
   the pre-existing `AGENTS.md:15` cmd-drift excepted and proved.

## Steps

### 1. Add four evals grading the absorbed machinery `[judgment]`

Write `skills/tracing-root-causes/evals/eval-04.md` … `eval-07.md` in
the repo's existing eval format (`## Query` with the verbatim request,
`## Expected behavior` as an objective checkbox list), covering: the
iron law under emergency time pressure (04), fix-thrashing and the
3-strikes architecture stop (05), authority/social pressure to skip
investigation (06), and a deep-stack symptom traced to its source with
defense-in-depth plus the AE handoff to `work-verify` (07). They grade
content that does not exist yet and are expected to fail against the
current SKILL.md — that is the point.

*Interface produced for step 3:* the checklist lines of evals 04-07 are
the acceptance surface SKILL.md must satisfy; step 3 reads these four
files as its requirements, not this PLAN.

**Acceptance**
- `ls skills/tracing-root-causes/evals/*.md | wc -l` → `7`
- `git show --stat --name-only HEAD` lists only paths under
  `skills/tracing-root-causes/evals/` → exit 0
- `git diff HEAD~1 HEAD -- skills/tracing-root-causes/SKILL.md` is
  empty → exit 0

### 2. Write the techniques reference `[judgment]`

Create `skills/tracing-root-causes/references/techniques.md`: a table of
contents followed by four distilled techniques — backward tracing to the
source, defense-in-depth validation (the four layers), condition-based
waiting (poll the condition, not the clock), and test-pollution
bisection (the `find-polluter` technique as runner-agnostic
instructions, no shipped script, per D3). No Graphviz. No TypeScript
example file. The file links no further reference (constraint 3).

*Interface produced for step 3:* the relative link target
`references/techniques.md` and its four section anchors, which SKILL.md
cites by name at the phases that use them (`isolate`, `fix`).

**Acceptance**
- `test -f skills/tracing-root-causes/references/techniques.md` → exit 0
- `grep -c "^## " skills/tracing-root-causes/references/techniques.md`
  → `4`
- TOC present when >100 lines: `head -20 …/techniques.md | grep -q
  "^- \[" ` → exit 0
- one level deep: `grep -E "\]\((\.\./)?references/" …/techniques.md`
  → exit 1 (no match)

### 3. Rewrite SKILL.md as the one house debugging skill `[judgment]`

Grow `skills/tracing-root-causes/SKILL.md` into the absorbed skill:
frontmatter `description` carrying BOTH trigger families; the iron law;
the phased spine `reproduce → isolate → hypothesize → disconfirm → fix`
with the existing six-step discipline surviving inside `hypothesize`
and `disconfirm`; the 3-strikes architecture rule; ONE merged
rationalization table; the output shape; and the AE handoff — evidence
to the lane's `PROGRESS.md`, `work-verify` owns the completion gate, the
skill never claims done. The closing note that delegated the fix loop to
`superpowers:systematic-debugging` is replaced by the supersession
statement. Cites `references/techniques.md` (step 2) at `isolate` and
`fix`.

**Acceptance**
- `grep -q "^name: tracing-root-causes$"
  skills/tracing-root-causes/SKILL.md` → exit 0
- `grep -c "" skills/tracing-root-causes/SKILL.md` → `< 500`
- `grep -q "work-verify" skills/tracing-root-causes/SKILL.md` → exit 0
- `grep -q "references/techniques.md"
  skills/tracing-root-causes/SKILL.md` → exit 0
- `grep -q "complements superpowers:systematic-debugging"
  skills/tracing-root-causes/SKILL.md` → exit 1 (delegation gone)
- `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  reports no `skill-size` / `skill-frontmatter` finding

### 4. Self-grade the seven evals against the landed skill `[judgment]`

Read each of the seven eval files against the SKILL.md that now exists
and record, per eval, which checklist lines the skill's text supports
and where it does not. A gap found here is fixed in SKILL.md or
`techniques.md` — never by weakening the eval (constraint 4, and the
repo's evals-first rule). The grading table lands in PROGRESS.md.

**Acceptance**
- PROGRESS.md contains a row per eval 01-07 with a verdict → `grep -c
  "^| eval-0" work/mat-46-debugging-absorbs/PROGRESS.md` → `7`
- `git diff --stat HEAD~2 -- skills/tracing-root-causes/evals/` shows no
  eval weakened after step 1 (additions only, or empty)

### 5. Record evidence and the fenced-file report `[mechanical]`

Fill `work/mat-46-debugging-absorbs/PROGRESS.md`: the verbatim lint
output for this worktree AND for clean `main` (proving the cmd-drift
pre-existing, D2), the eval-grading table from step 4, the commit-order
proof, and the README index wording this lane could not apply (D4).

**Acceptance**
- `git archive main | tar -x -C <scratch>/clean-main && node
  C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs
  <scratch>/clean-main` output quoted verbatim in PROGRESS.md
- `git log --oneline --stat` excerpt in PROGRESS.md shows the eval
  commit before the SKILL.md commit
- `node C:/Briar/repos/mine/Agent-Engineering/scripts/agent-lint.mjs .`
  → the single pre-existing `AGENTS.md:15 [cmd-drift]` MEDIUM and
  nothing else

### 6. Push the branch and open the PR `[mechanical]`

`git push -u origin bygama/mat-46-debugging-absorbs`, then `gh pr
create` against `main` with `Closes MAT-46` in the body. **Never
merged by this lane** — merging is the parent's action.

**Acceptance**
- `gh pr view --json url,state` → state `OPEN`
- `gh pr view --json body | grep -q "Closes MAT-46"` → exit 0
- `gh pr checks` → the `standard` check `pass` (D2: the authoritative
  gate)

## Out of scope

Everything in SPEC.md's *Out* and *Fenced* sections. No new skill
directory, no change to the other three skills, no upstream AE edit.
