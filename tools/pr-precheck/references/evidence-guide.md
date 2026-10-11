# Evidence guide: where evidence lives in a PR package

Four families, named for the failure categories they catch. In an eval bundle everything is in the one bundle file (or its `.json` twin); in live mode it is in the student's working copy and on GitHub.

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- *Eval bundle:* the plan-context block. It holds the plan's files, steps, "Not in scope" list, test plan, and any deviation note. The diff is under the candidate PR's `### Diff`; the commit list is `### Commits`; the description's claims are under `### Description`.
- *Live:* `plan.md` (its scope, steps, "not in scope", and `## Deviations` section); the diff from `git diff main...HEAD` and `git diff main...HEAD --stat` for the file list; `git log main..HEAD --oneline` for commits; the claims in `pr_draft.md`.

**What good looks like.** Every changed file and hunk is named by the plan or explained by a deviation note, nothing on the plan's "not in scope" list is touched, and every planned step shows up in the diff or is disclosed as deferred. Silent drift runs both ways: *more* than the plan (a new flag or config option, an unrelated function rewritten, a rename pass, reworded help text) and *less* (a promised docs update or second fix that is absent) with no note. The description is part of this family: a description that says "exactly as planned" or "docs now document X" over a diff that does otherwise is drift, however clean the checklist looks. Read the diff, not the description.

## Test evidence (harness category: not-tested)

**Where it lives.**
- *Eval bundle:* the candidate PR's `### Test evidence` section, read against the plan-context block's test plan and the repro evidence it cites. The repo's own checks are named in the plan's test plan or the repo-facts block.
- *Live:* `test_evidence.md` (the before and after repro, and the output of `make test-unit`, `make test-integration`, `make lint`, `make typecheck`), read against `plan.md`'s test plan and your unit 2 repro steps. The same evidence must appear in `pr_draft.md` under Testing.

**What good looks like.** The evidence runs the plan's repro on the path the fix changes and shows the failing output before and the expected output after, with the command visible. Every in-scope failure mode the plan names is exercised. Added tests call the fixed behavior. The repo's checks are shown with their outcome; a failing or "no tests ran" result reported with its reason is evidence, hidden or missing is not. "Tested locally", "tests pass", or "works on my machine" with no named behavior is not evidence, and neither is a repro of a path the fix never touches (a GET when the bug is in the POST).

## Diff quality (harness category: unreviewable)

**Where it lives.**
- *Eval bundle:* the candidate PR's `### Diff` (every `+` and `-` line) and `### Commits`.
- *Live:* `git diff main...HEAD` and `git diff main...HEAD --stat`, and `git log main..HEAD --oneline`. Untracked files (plan.md, comment.md, pr_draft.md, test_evidence.md) must not appear in the diff.

**What good looks like.** One fix, with its test and any required doc/changelog line, and nothing else. The debris tells: added print/eprintln/console.log/DEBUG lines; commented-out code or an abandoned first attempt; added functions or imports nothing uses (`allow(dead_code)`, `_unused_...`); TODO/FIXME on the change; hunks that only re-indent, re-order imports, or re-print identical lines; unrelated drive-by edits; commits named "wip", "fix", "misc cleanups while debugging". A right fix still fails here if its change is buried. `--stat` should list only the files the fix changes.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- *Eval bundle:* the repo-facts block: its "pull requests" line (template sections and checklist asks) and "contribution policy" line (including any AI-use policy). The thread highlights hold any explicit maintainer direction. The description is under `### Description`; changelog or whatsnew files, if required, must appear in `### Diff`.
- *Live:* `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue, Changes, Testing, Screenshots/Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md` in the Path Review repo; the issue thread; and your `pr_draft.md`. The course requires an AI-use disclosure under Notes for Reviewers, in your own words.

**What good looks like.** Every section or ask the repo states is met with real content: `Closes #<number>` present, checklist items true and not contradicted by the diff, a required changelog/whatsnew entry in the diff, tests added when the repo asks. A required AI-use disclosure is present and in the author's own words (the tool used and how). An explicit maintainer instruction or question is engaged. Boilerplate, empty sections, or a visibly skipped ask fails, however good the fix. Whether the description's claims match the diff is plan fidelity, above.
