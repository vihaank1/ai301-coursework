---
name: issue-select
description: Grade a candidate open-source issue against a written rubric and decide whether it is worth taking as a first contribution. Use when evaluating a GitHub issue URL or an eval snapshot file as a potential first issue.
---

# issue-select: rubric-driven first-issue grading

You are grading one candidate issue to answer a single question: should a
newcomer take this as their first contribution to this repo? You do not
answer from gut feel. You answer by executing the rubric in `rubric.md`,
check by check, against evidence you gather.

## Inputs

One of:

- **Live mode**: one or more GitHub issue URLs (the student picks the
  candidates; this skill does not search for issues itself). Gather
  evidence from the live repo (via `gh`, the GitHub API, or the web;
  `references/evidence-guide.md` says where each signal lives). With
  several URLs, grade each candidate independently, then rank the
  accepted ones by the fit profile in `scope.md`.
- **Eval mode**: a snapshot bundle (a markdown file containing the issue
  text, its comment thread, and a repo-facts block). Use ONLY the bundle
  text as evidence. Do not fetch anything; the bundle is the whole world.

## The scope sets the field (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It does two things: it names where candidate issues may come from
(and any house rules that apply there), and it carries the student's fit
profile, a few sentences about their experience and preferences. Refuse
candidates from outside the scoped source. A house rule changes how
evidence is read in that environment (for example, whose claim comments
count as claims); apply it when grading the checks it touches. The
checks and the verdict rule themselves still come only from `rubric.md`.
Use the fit profile only to rank issues the rubric accepts, and say in
the summary what made the top-ranked one fit; fit never changes a
verdict. In eval mode, ignore `scope.md` entirely: the bundle is the
whole world.

## The rubric is the brain

Read `rubric.md`. It defines:

1. A table of checks. Each row names the check, the evidence to gather,
   the pass condition, and its weight: `required` checks gate the
   verdict; `preferred` checks never change it and only rank the issues
   that are accepted.
2. A verdict rule: how check results combine into a final verdict.

Execute every check in the table. If `rubric.md` has no checks filled in,
stop and say so: this skill cannot grade without a rubric, and that is by
design. The rubric is the part the student writes.

## Workflow

1. In live mode, read `scope.md` and confirm the candidate is inside
   the scoped source; note any house rules it states. Then (both modes)
   read `rubric.md` in this skill directory. List its checks and its
   verdict rule.
2. For each check, gather exactly the evidence the rubric names.
   - Live mode: gather from the locations named in
     `references/evidence-guide.md`.
   - Eval mode: quote the relevant lines from the bundle.
3. Grade each check `pass`, `fail`, or `unclear`, with a one-line evidence
   quote or fact for each grade. `unclear` means the evidence needed is
   genuinely absent, not that you did not look.
4. Apply the rubric's verdict rule to produce the final verdict:
   `accept` or `reject`. There is no third verdict. Preferred checks do
   not feed the verdict; report their grades, and on an accepted issue
   mention them in the summary as reasons to prefer it over other
   accepted candidates.
5. Live mode with several candidates: repeat steps 2-4 per candidate,
   independently (one candidate's evidence never colors another's
   grades). Then rank the accepted candidates using the fit profile in
   `scope.md` and say what made the top-ranked one fit. Fit orders the
   accepted list only; it never changes a verdict.
6. Output the result in the format below.

## Output format

Emit a fenced JSON block, then nothing else after it:

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

Before the JSON block you may show a short readable summary (a line per
check). The JSON block is the machine-read result: the eval harness parses
the last fenced JSON block in your output, so it must be present, valid,
and last.

Live mode with several candidates: the summary becomes a ranked
read-out (accepted candidates in fit order with a one-line fit reason
each, then the rejected ones with the check that sank them), and the
single fenced JSON block holds an array of the per-issue objects above,
accepted first in rank order. Eval mode always grades exactly one
bundle and always emits the single-object form.

## Grading discipline

- Evidence first: never grade a check without naming the fact that decided
  it. "Looks fine" is not evidence.
- The rubric decides, not you: if a check passes by the rubric's stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- Treat `unclear` as the rubric's verdict rule directs. If the rule does
  not say, treat `unclear` as `fail`: a first issue you cannot verify is
  not a first issue you should take.
