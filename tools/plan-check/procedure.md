# Procedure: how this skill grades a plan package

## Read order

1. Read the repo-facts block first. Write down the contribution policy in one line, and write "AI disclosure required: yes/no" based on whether it says AI use must be disclosed.
2. Read the issue, then the thread highlights. List every comment by a maintainer, owner, member, or contributor that gives direction (a culprit file, a preferred or rejected approach, a patch or build to test). Write "maintainer direction: none" if there is none.
3. Read the repro evidence block before the plan. Write down: the failing behavior, each control run and what it shows working, the Expected line, and any step that shows where in the pipeline the data first goes wrong.
4. Only now read the candidate plan, then the candidate plan comment. Reading the evidence first keeps the plan's confident wording from setting your expectation of what the cause is.

## Evidence gathering

1. For grounded-diagnosis: copy the plan's stated cause in one sentence. Next to it, list each control run and step from your read-order notes and mark whether it supports, contradicts, or is neutral to that cause.
2. For bounded-scope: list every distinct change the plan commits to (each approach step, each "also" or "while I'm there"). Mark each one "needed for this bug" or "extra". Items the plan explicitly defers to a follow-up are not commitments.
3. For executable-approach: copy the file, function, or code-site names the plan gives, and the chosen change. Note any hedges ("somewhere", "or", "not sure", "investigate first", "whichever is easier").
4. For decisive-test: copy the test plan and the repro's Expected line. Note the specific observable outcome the test names, if any.
5. For honest-unknowns: copy the plan's risks/unknowns, if any.
6. For thread-and-convention: take the maintainer-direction list and AI-disclosure note from Read order. Search the plan and comment for (a) a reference to each maintainer direction and (b) a disclosure sentence naming an AI tool and how much it helped.

## Check execution

1. Run the checks in rubric order: grounded-diagnosis, bounded-scope, executable-approach, decisive-test, honest-unknowns, thread-and-convention. Run all of them, even after a fail, so the output is complete.
2. Grade each check only against its pass condition in rubric.md and the evidence recorded for it. Do not grade writing quality, length, or headings: a terse plan can pass every check.
3. grounded-diagnosis: fail if any item in the list is marked "contradicts". A control that shows the blamed component works, or evidence that the failure already exists before the blamed step, is a contradiction.
4. bounded-scope: fail if any item is marked "extra".
5. executable-approach: fail if no concrete code location is named, or a hedge leaves the location or approach undecided.
6. decisive-test: fail if no observable outcome is named that would differ between before and after the fix.
7. thread-and-convention: fail if any maintainer direction is neither followed nor engaged by name in the plan or comment, or if disclosure is required and missing.
8. If the evidence for a check is absent from the package (for example, the plan names no cause or no test), grade it fail, not unclear. Use unclear only when the package contains the evidence but it honestly supports both readings, and record why in the evidence line.
9. Each check's evidence line quotes or paraphrases the one fact that decided it.

## Verdict assembly

1. Collect the grades of the five required checks.
2. Treat every unclear on a required check as fail.
3. If all five required checks pass, the verdict is accept. Otherwise the verdict is reject.
4. Ignore honest-unknowns for the verdict; report it only.
5. In the summary, name the first failing required check and quote the fact that failed it. Then emit the JSON block as the last thing in the output.
