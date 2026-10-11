# Contract: the fixed interface every pr-precheck tool satisfies

<!--
Staff wrote this file. It ships complete and you do not edit it: the
contract is the last rail. Weeks 1 through 3 handed you a working
SKILL.md and asked you to fill judgment files behind it. This week
there is no working SKILL.md: every file's content is yours to write,
and this contract is what makes your tool, your classmates' tools, and
the instructor's master tool comparable. The harness reads the layout
below; the bar compares verdicts produced through the interface below.
Build anything you want inside it. Do not build around it.
-->

## The question

A pr-precheck tool answers exactly one question about exactly one PR
package: **is this ready to submit?** A PR package is a candidate pull
request (title, description, commits, diff, test evidence), read
against the plan it claims to implement and the issue that plan
belongs to. The tool never answers a different question, never grades
more than one package per run, and never answers from gut feel: it
executes the components in this directory.

## The layout

One fixed layout for the whole cohort, so grading stays uniform and
harness feedback stays signal. Every file sits at exactly this path
inside the tool directory:

    SKILL.md                     the frame: what the tool does, and how
    rubric.md                    the checks and the verdict rule
    procedure.md                 the operating steps
    voice-guide.md               your week-2 voice guide, pasted forward
    scope.md                     where the tool may operate (you fill the Repo line)
    references/
      evidence-guide.md          where evidence lives in a PR package

Nothing about this layout is yours to invent: no extra components, no
renamed files, no relocated references. The harness inlines
`SKILL.md`, `rubric.md`, `procedure.md`, and
`references/evidence-guide.md` from these exact paths; a file
somewhere else does not exist as far as the interface is concerned.
This is the same shape weeks 2 and 3 shipped; what changed is who
writes the contents.

## Modes

The tool runs in exactly two modes, and SKILL.md must define both:

- **Live mode**: the student's own submission, checked before it goes
  out. Inputs: their `plan.md` (deviation notes included), the diff on
  their branch, their draft PR title and description, and their test
  evidence, read against their issue. A house-chain student reads the
  house plan and the house repro pack instead; the same checks grade
  the same things there. Live mode gathers issue-side evidence from
  the real repo (the thread, the PR template, the stated policy).
- **Eval mode**: a package bundle is the whole world. Every fact comes
  from the bundle text; nothing is fetched, nothing else is read. Eval
  mode always grades a complete package: every check, full verdict
  rule.

## Verdict space

Binary: `accept` (ready to submit) or `reject` (hold). There is no
third verdict, no "accept with reservations", no score. Reservations
belong in check evidence lines, not in the verdict.

## Output

The tool's reply ends with a fenced JSON block, and nothing follows
it. The schema is fixed and may not be altered, extended, or
reordered:

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

The harness parses the last fenced JSON block in the output. A
readable per-check summary may come before the block; the block is the
machine-read result and must be present, valid, and last.

## The scope seam

In live mode, the tool reads `scope.md` before anything else. The
scope names where the student's PR must live and the house rules of
that environment; the tool refuses to grade work outside the scoped
repo, and if the scope's repo line still carries an unfilled
placeholder it stops without grading and tells the student to fill
the `Repo:` line in `scope.md` with their section's Path Review repo.
It never guesses a scope. In eval mode, `scope.md` is ignored
entirely. This seam is why the same tool graduates: staff swap one
file and the field of view changes.

## The voice seam

In live mode, the tool also reads `voice-guide.md` (the student's own
rules for how they write upstream, carried forward from week 2) and
holds the outgoing PR text, the title and the description, against
those rules, reporting any rule the draft breaks in the summary. The
voice guide never changes the verdict on its own unless the rubric has
a check that reads it. In eval mode, `voice-guide.md` is ignored
entirely: voice is personal and carries no gold labels; universal
communication-quality checks live in the rubric.

## The refusal rule

A tool with no rubric content or no procedure content refuses to grade
and says so, by design. The shipped templates are empty on purpose; an
empty tool that invents checks at runtime is worse than no tool,
because its verdicts look like judgment and are noise. The instruction
comments inside the templates are guides, not content: the harness
ignores comments when it checks whether a file is still a template, so
keep them or delete them as you fill each file; only your own written
text counts.

## Grading discipline (contract terms)

These are not style advice; they are interface guarantees the master
tool honors and the bar assumes:

- **Evidence first.** No check is graded without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be
  ready and a beautiful confident one can be hiding drift. Every check
  reads the artifact itself against the plan, the issue, and the
  stated standards, never the formatting.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it still passes; the fix belongs in the
  rubric, not in the run.
- **The procedure decides how, not the run.** The tool follows
  `procedure.md` as written and reports its gaps instead of silently
  inventing steps.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs; where the rule is silent, an unverifiable
  claim is a failing one. A PR you cannot verify from the package is a
  PR that is not ready to submit.

## Raw material

You are not starting from nothing. Your week-1 and week-3 rubrics are
the pattern for `rubric.md`; your week-2 and week-3 evidence guides
already map three of this week's evidence families; your week-3
procedure is the pattern for `procedure.md`; your voice guide pastes
straight into its slot. The weeks 1-3 frames you ran are the pattern
for what a SKILL.md must decide. In this materials package the
instructor masters sit in `../instructor/master-skill/` for the
harness bar to compare against (in production they stay
instructor-side); the parts bin that matters is the one you built.
