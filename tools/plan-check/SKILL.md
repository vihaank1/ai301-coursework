---
name: plan-check
description: Grade a plan package (a candidate plan plus a candidate plan comment, read against the issue and its reproduction evidence) and decide whether it is ready to post and build from. Use when checking a draft plan or plan comment before posting, or when grading an eval package bundle.
---

# plan-check: rubric-driven plan grading

You are grading one plan package to answer a single question: is this
ready to post and build from? A package is a candidate plan and a
candidate plan comment, read against the issue they belong to and the
reproduction evidence that plan builds on. You do not answer from gut
feel, and this file no longer tells you how to work: you answer by
executing the student-authored grading procedure in `procedure.md`,
which applies the rubric in `rubric.md` to evidence gathered per
`references/evidence-guide.md`.

## Inputs

One of:

- **Live mode**: the student's own `plan.md` and draft plan comment,
  plus their issue's URL. Gather the issue-side evidence live (via
  `gh`, the GitHub API, or the web; `references/evidence-guide.md`
  says where each signal lives), and take the reproduction evidence
  from the student's posted repro comment on that issue (their week-2
  proof; the plan must follow from it). A student on the house issue
  has no posted repro comment of their own: there, the reproduction
  evidence is the house repro pack as quoted in the drafts, and if the
  drafts quote no repro evidence at all, that absence is what the
  relevant checks grade. Read the drafts the way a
  maintainer on the thread will read the posted comment: the package
  is what the drafts contain and quote, not other files in the
  student's working directory.
- **Eval mode**: a package bundle (a markdown file containing the
  issue context, a repo-facts block, a repro-evidence block, the
  candidate plan, and the candidate plan comment). Use ONLY the bundle
  text as evidence. Do not fetch anything; the bundle is the whole
  world. Eval mode always grades a complete package: every check,
  full verdict rule.

## The scope sets the field (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names where the student's issue must live and the house rules
that apply there; a house rule changes how evidence is read in that
environment. Refuse to grade a package for an issue outside the scoped
source. If the scope's repo line still carries an unfilled placeholder,
stop without grading and tell the student to get their cohort's scope
file from the instructor; never guess a scope. In eval mode, ignore
`scope.md` entirely.

## The voice guide gates outgoing words (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, with wrong/right examples. Check the draft
plan comment against those rules and report any rule the draft breaks
in the summary, quoting the rule. The voice guide never changes the
rubric's verdict on its own unless the rubric has a check that reads
it. In eval mode, ignore `voice-guide.md` entirely: voice is personal
and carries no gold labels; the universal communication-quality checks
live in the rubric.

## The rubric is the brain

Read `rubric.md`. It defines:

1. A table of checks. Each row names the check, the evidence to
   gather, the pass condition, and its weight: `required` checks gate
   the verdict; `preferred` checks never change it.
2. A verdict rule: how check results combine into a final verdict.

`references/evidence-guide.md` is the rubric's map: where each kind of
evidence lives in a plan package (and, live, on GitHub), and what good
looks like there.

## The procedure is the hands

Weeks 1 and 2, this file told you the workflow. This week it does not:
**execute `procedure.md`**. That file is the skill's operating
procedure, written by the student, and it must decide the read order,
how each evidence family gets gathered, how a check executes against
gathered evidence, and how check grades become the verdict. Follow it
as written, the same way an executor follows a rubric: exactly,
without improvising around gaps. Where the procedure is silent, note
the gap in your summary rather than silently inventing a step; a
procedure gap is feedback the student needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so: this skill cannot grade without a rubric
AND a procedure, and that is by design. The rubric, the evidence
guide, and the procedure are the parts the student writes; the voice
guide carries over from week 2.

## Verdict and output

The verdict space is binary: `accept` (ready to post and build from)
or `reject` (hold). There is no third verdict. Emit a fenced JSON
block, then nothing else after it:

```json
{
  "item": "<issue URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Before the JSON block you may show a short readable summary (a line
per check, plus any voice-guide notes in live mode). The JSON block is
the machine-read result: the eval harness parses the last fenced JSON
block in your output, so it must be present, valid, and last.

## After an honest deviation (live mode)

Plans meet reality. When a build deviates from the posted plan and the
student updates `plan.md` to record what changed and why, re-run this
skill on the updated package before any follow-up comment is posted:
the deviation note is part of the plan now, and it gets graded like
everything else. A deviation recorded in the plan is honest work; a
deviation that only exists in the diff is not.

## Grading discipline

- Evidence first: never grade a check without naming the fact that
  decided it. "Looks fine" is not evidence.
- Grade the plan, not the polish: a terse complete plan can be ready
  and a long confident one can be unbuildable or wrong. Every check
  reads the thing itself against the issue and its repro evidence,
  never the formatting.
- The rubric decides, not you: if a check passes by the rubric's
  stated condition but feels wrong, it still passes. Note the tension
  in the summary if you want; the fix belongs in the rubric, not in
  the run.
- The procedure decides how, not you: follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule
  does not say, treat `unclear` as `fail`: a plan you cannot verify
  from the package is a plan that is not ready to build from.
