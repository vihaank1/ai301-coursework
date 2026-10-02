# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: in an eval bundle, the cause is the opening lines of "## Candidate plan" (often labeled "Cause:" or "Diagnosis:"). The behavior it must explain is in "## Repro evidence": the numbered Steps, the Artifact (stack trace, output), the Control line, and Expected/Actual. Live: the diagnosis section of plan.md, read against the student's posted repro comment on the issue (or the house repro pack quoted in the draft).
- What good looks like: the cause names the code path the repro points at, and every control run is consistent with it. A control that shows the blamed component working (the same input parses fine without a flag, the same feature works in another context), or a step showing the data is already wrong before the blamed step, means the diagnosis is contradicted, however confident it sounds.

## Scope

- Where it lives: the plan's "Change" or "Scope" paragraph, its "In:" and "Not in scope / Out:" lines, and the numbered approach steps. Live: the scope and files sections of plan.md.
- What good looks like: one bounded change at the site the repro isolates, plus a regression test, with bigger ideas explicitly deferred. A drive-by rewrite adds migrations, dependency upgrades, new options or UI, CI matrices, or "while I'm in there" refactors on top of the fix.

## Executability

- Where it lives: the approach steps and any named files (`path/to/file.ext`), functions, or branches in the candidate plan. Live: the files and approach sections of plan.md.
- What good looks like: a named file or function and one chosen change, so a stranger could open the file and start. Hedges like "somewhere", "gocui? tcell? not sure", "upstream or vendored, whichever is easier", or "profile first" mean nobody can start.

## Test plan

- Where it lives: the plan's "Test" or "Test plan" paragraph, mapped onto the repro evidence's Steps and Expected line. Live: the test plan section of plan.md against the unit 2 repro steps.
- What good looks like: re-run the repro steps (or add the repro as a regression test) and name the observable that flips: "at step 3 the color must change", "exits 0", "both tests pass, control unchanged". Vague plans say "run the full suite", "should feel fast", "nothing else should feel broken".

## Honesty

- Where it lives: the plan's "Risk" or "Unknowns" lines, any hedged statements, and, in live mode, the "## Deviations" section of plan.md.
- What good looks like: untested assumptions are named as open, with what will happen if they go wrong. False confidence states an unverified claim as fact. A mid-build change is recorded under Deviations with what changed and why.

## Comms

- Where it lives: "## Candidate plan comment", read against "## Thread highlights" (each maintainer, owner, or contributor comment) and the "contribution policy" bullet of "## Repo facts" (look for AI_POLICY or "AI usage must be disclosed"). Live: the draft comment.md against the issue thread on GitHub and the repo's CONTRIBUTING.md.
- What good looks like: the comment engages any explicit maintainer direction by name (follows it, or says why not) and, when the repo requires it, includes an AI disclosure naming the tool and the extent of help. Boilerplate that ignores a maintainer's posted culprit or test build, or skips a required disclosure, is not thread-aware.
