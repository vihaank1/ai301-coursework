---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?** Never answer a different question, never grade more than one package per run, and never answer from gut feel. Execute the components in this directory.

A PR package is a candidate pull request read against the plan it claims to implement and the issue that plan belongs to. It contains:

- the **plan** (scope, boundary, steps, test plan, and any deviation notes),
- the **diff** and commit list,
- the **PR title and description**,
- the **test evidence**,
- the **issue** and the repo's stated asks (PR template, contributing docs, AI-use policy).

The question is whether the diff, the description, and the evidence agree with the plan and meet the repo's standards. It is not whether the code is good in the abstract, and it is not whether the writing is polished.

## Inputs and modes

Decide the mode first. If the input is a package bundle (an id like `pkg-07` or a bundle file), run eval mode. If the input is a PR URL or a request to grade "my draft PR", run live mode.

**Live mode** grades the student's own submission before it goes out. Read these inputs, from the top folder of the student's fork clone, on their branch:

- `plan.md`, including everything under `## Deviations`. This is the plan the PR must implement. A house-chain student reads the house plan and the house repro pack instead; the same checks grade the same things.
- The branch's diff: run `git diff main...HEAD` (three dots). It shows every committed change the branch makes relative to `main`. Uncommitted edits are not part of the package. Also run `git log main..HEAD --oneline` for the commit list.
- The draft PR title (first line) and description (the rest) in `pr_draft.md`.
- The test evidence in `test_evidence.md`: the before and after repro run and the output of the repo's four checks (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`).
- The issue and its thread, read from the issue URL.
- The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, which supply the repo's stated asks and AI-use policy.

Never read `plan.md`, `comment.md`, `pr_draft.md`, or `test_evidence.md` as part of the diff; they are untracked inputs that must not be committed.

**Eval mode** grades a package bundle. The bundle is the whole world. Every fact comes from the bundle text: its repo-facts block, issue and thread, plan-context block, and candidate PR (title, description, commits, diff, test evidence). Fetch nothing. Read nothing else. Do not look at the live issue; when the live page and the bundle disagree, the bundle wins. Always run every check and the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, before opening any other input.

- Find the `Repo:` line. If it still carries a placeholder (`<ORG>`, `<PATH-REVIEW-REPO>`, or any angle-bracketed text), stop without grading. Tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo, and emit no verdict.
- If the PR, branch, or issue is for a repository other than the one on the `Repo:` line, refuse to grade it and say which repo is in scope. Never guess a scope.
- Apply the house rules in `scope.md` when reading the package: the PR comes from the student's fork branch, it is one PR per issue, and the template is always used.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the rubric is graded, read `voice-guide.md` and hold the outgoing PR text, the title and the description, against its rules. Report each rule the draft breaks in the summary, quoting the offending words and the rule. Do not put voice findings in the JSON checks and do not let them change the verdict: the voice guide is the student's own taste, and only a rubric check may hold a package. If the draft breaks no rule, say so in one line.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order and use each for its own job:

1. `rubric.md` defines the checks and the verdict rule. These are the only checks you run. Never invent a check or soften a pass condition.
2. `references/evidence-guide.md` is the map: for each check family it says where the evidence lives (in a bundle, and in a live working copy) and what good looks like.
3. `procedure.md` says how to execute: read order, evidence gathering, check execution, verdict assembly. Follow it as written.

When the procedure is silent on a step, say so in the summary ("procedure gap: ...") and apply the plainest reading of the rubric. Do not improvise a new method. If `rubric.md` has no checks, or `procedure.md` has no steps, or the evidence guide has no content, refuse to grade: say which component is empty and emit no verdict. An empty tool that invents checks produces noise that looks like judgment.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations", and no score. Reservations belong in a check's evidence line.

Write a short per-check summary a person can read, then end the reply with one fenced JSON block, valid and last, with nothing after it. List every check from `rubric.md` once, in rubric order, using the check's exact name. The schema is fixed:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Grade no check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be ready; a beautiful, confident one can hide drift. Read the diff and the evidence themselves against the plan, the issue, and the stated standards. Never grade formatting, length, or enthusiasm. Never take a description's claim ("exactly as planned", "all tests pass") as evidence; check it against the diff.
- **The rubric decides, not the run.** If a check passes by its stated condition but feels wrong, it still passes. If it fails by its stated condition but looks fine, it still fails. The fix belongs in the rubric, not in this run.
- **The procedure decides how, not the run.** Follow `procedure.md` and report its gaps.
- **Honest outcomes count.** A shortfall the plan's deviation notes or the description discloses plainly is not a failure. A shortfall that is silent is.
- **Unclear is not pass.** Where `rubric.md`'s verdict rule says nothing about `unclear`, treat it as a fail: a claim you cannot verify from the package is a PR that is not ready to submit.
