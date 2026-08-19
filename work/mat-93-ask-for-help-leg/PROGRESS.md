# PROGRESS — MAT-93

Lane: `work/mat-93-ask-for-help-leg/` · Tier M · Judgment lane

## Baseline (2026-08-19, clean tree at `e969b67`)

- Upstream source present and read: superpowers v6.3.0
  `skills/systematic-debugging/SKILL.md` (plugin cache,
  `~/.claude/plugins/cache/claude-plugins-official/superpowers/6.3.0/`)
  — the same version MAT-46 absorbed, so the judgment compares against
  the absorbed source with no version drift.
- Phase 3.4 "When You Don't Know" upstream carries four beats: say "I
  don't understand X" · don't pretend to know · **ask for help** ·
  research more. House `skills/tracing-root-causes/SKILL.md` (214
  lines) carries the first, second and fourth (phase 3, "Name what you
  don't understand"); "ask for help" is absent — the named omission of
  MAT-46 D8 (record in git history at `e969b67^`).
- Sibling precedent read: MAT-47 D-003 dropped "'ask your human
  partner' as the universal escape hatch" while keeping the AE route
  "where the escape hatch is genuinely needed".
- Lint baseline (via absolute path to the AE checkout):
  `0 high, 0 medium, 1 low — PASS`, exit 0; sole finding the
  pre-existing `AGENTS.md:15` cmd-drift LOW (DECISIONS.md D2).

## Status

- [x] Lane opened: SPEC.md + DECISIONS.md (D1 verdict, D2 lint gate) +
      this baseline
- [ ] Parent approval of SPEC/verdict (design-first gate, blocking ask)
- [ ] PLAN.md shaped after approval
- [ ] Execution (work-run ceremony)
- [ ] work-verify (M: static + behavioral + fresh-context review)
- [ ] work-handoff: PR open (`Closes MAT-93`), worker_done
