# Procedure: how this tool grades a PR package

The package is graded side by side: the diff against the plan, the evidence against the test plan, the description against both. Do the reading first, grade second.

## Read order

1. **Live mode only:** read `scope.md` (see SKILL.md's scope seam), then `rubric.md`. Eval mode: read `rubric.md` first.
2. Read the **repo facts and the repo's stated asks** (eval: the repo-facts block; live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`). Note down every template section, checklist item, changelog/whatsnew ask, test ask, and any AI-use policy. These are the standards-wall asks.
3. Read the **issue and thread**. Note the bug in one line and any explicit maintainer instruction or open question.
4. Read the **plan** before the diff or the description. Note down: the files the plan names, the steps it promises, its "not in scope" list, its test plan (the repro and the expected after), and any deviation notes. Reading the plan first fixes what the diff should look like; without it, every hunk looks reasonable.
5. Read the **diff and commit list** hunk by hunk. For each hunk note the file, what it does, and whether it is a fix hunk, a test, a doc/changelog line, or something else.
6. Read the **test evidence**. Note each command run, the path it exercised, and what it printed.
7. Read the **PR title and description last**, once you know what the diff really does. Note every claim it makes about the change, the evidence, and the repo's asks. Do not let the description's framing shape your reading of the diff.

## Evidence gathering

Use `references/evidence-guide.md` to find each item. Gather and record:

- **Plan fidelity:** list the changed files and set them against the plan's named files. For each hunk, mark "in plan", "in deviation note", or "unplanned". List each plan step and mark "in diff", "deferred and disclosed", or "missing". Check the "not in scope" list against the hunks.
- **Description claims:** list each claim in the description ("exactly as planned", "docs updated", "tests added", "no changes outside X") and find the hunk or file that supports it. Mark each "supported", "contradicted", or "no support".
- **Test evidence:** write down the plan's repro and test plan items, then match each to a command and output in the evidence. Record whether the output shows a before and an after, whether the path exercised is the one the fix changes, and whether any in-scope failure mode is never run. Check any added test: does it call the fixed behavior? Record the repo's checks named in the plan or docs and whether their outcome is shown.
- **Debris:** scan every `+` and `-` line for print/log/debug statements, commented-out code, added functions or imports nothing uses, TODO/FIXME, and hunks that only change whitespace, indentation, import order, or re-print identical lines. Read the commit messages for "wip", "fix", "cleanup".
- **Standards:** match each ask from step 2 to the description or diff. Record `Closes #`, each template section's content, the changelog entry, and the disclosure text (who wrote it, whether it says AI was used and how).

If a needed item is not in the package (no plan, no evidence section, no description), record "absent" and do not fill the gap from your own knowledge of the project.

## Check execution

1. Run the checks in the order they appear in `rubric.md`.
2. For each check, apply only its pass condition to the facts gathered above. Name the one fact or quote that decided it; that goes in the `evidence` field.
3. If the evidence for a check is genuinely absent from the package, grade it `unclear`, not `pass`.
4. A check may be graded from the notes without re-reading the whole package, but re-open the diff or evidence when the notes are not enough to quote the deciding fact.
5. Do not let one check's result move another's: a package can fail "Diff matches plan" and still pass "Decisive test evidence". Grade each on its own pass condition.
6. Do not grade polish. A terse description with complete evidence passes; a confident, beautifully formatted one with a hunk the plan never names fails.
7. Check the honest-outcome rule on every plan-fidelity and evidence check: if a shortfall is disclosed in a deviation note or the description, it is not a failure on its own.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every `required` check is `pass`; `reject` if any `required` check is `fail`; treat `unclear` as `fail`. `preferred` checks never change the verdict.
2. When more than one required check failed, the deciding check is the first failing required check in rubric order. Quote its evidence line in the summary first and name any other failing checks after it.
3. Live mode: after the verdict, report voice-guide breaks as a short list (rule, offending words), separate from the checks.
4. Write the per-check summary, then end with the fenced JSON block from SKILL.md: every rubric check once, in rubric order, with grade and a one-line evidence quote, then the verdict. Nothing follows the block.
