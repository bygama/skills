# Eval 06: one RED per requested rule, and the old behaviour proven against HEAD

## Query

"Make `bin/slugify` lowercase its input and strip accents. Keep
everything else it does the same."

## Fixture

A lane is open at `work/slugify-rules/` with `LANE.md`; S1 quotes the
request verbatim. `bin/slugify` is an existing command: it reads stdin,
prints a slug on stdout, writes a warning to stderr and exits 2 on empty
input. The repo has a runnable test suite (`npm test`). The change
touches `bin/slugify`.

Provenance — expected failure modes, not a controlled baseline run: an
agent writes a single combined test ("slugify works") that goes red once
and green once, covering one of the two rules by accident; and it
declares "everything else the same" on the strength of the suite passing,
though no test pins the stderr text or the exit code of the old command.

## Expected behavior

- [ ] Writes two failing tests, one per requested rule — lowercasing and
      accent stripping — each watched failing for its own expected
      reason before its implementation. A single test covering both
      rules fails this box.
- [ ] Appends both RED beats and both GREEN beats to the `## Log` of
      `work/slugify-rules/LANE.md` as they happen, each with command,
      exit and one-line result.
- [ ] Treats `bin/slugify` as an existing command the change touches and
      records one proof that its old behaviour holds: stdout, stderr and
      exit code compared against `HEAD` — running the committed version
      from a `git worktree` (or a `git show HEAD:bin/slugify` copy) and
      the working copy on the same inputs, including the empty-input
      case (stderr warning, exit 2).
- [ ] Does not use `git stash` for the comparison, and does not claim
      the old behaviour holds on the strength of the suite alone.
- [ ] Records the comparison command and its observed result in the Log;
      the task is ticked in `## Tasks` only after it is there.
- [ ] If the comparison shows a difference outside the two requested
      rules, reports it as a finding instead of ticking.
