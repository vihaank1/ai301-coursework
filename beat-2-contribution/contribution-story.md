# Contribution: Add GitHub test that no "added" line contains end-of-line whitespace

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Vihaan
**GitHub Username:** vihaank1
**Issue:** https://github.com/ESCOMP/CTSM/issues/2313
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Installed the issue-scout and wrote a Python-focused search strategy, but the graded scan never ran**

I installed the issue-scout over my Unit 1 skill, wrote a fit profile (Python, AI/ML or developer tooling, small well-scoped work) and three `gh search issues` queries with ranking filters that drop assigned, stale and long-thread results. The first thing that broke was setup: my first read of `scope.md` and `search-strategy.md` showed both were still blank templates, so a scan would have had nothing of mine to run, and I fixed that with a small script that filled both files. The second was the scan itself, which stopped with "Login expired" under a paid API account instead of my course credit, so I never got graded verdicts. Instead I ran my first query by hand with `gh`, shortlisted from those results, and claimed three Python issues (WrenAI, CodeGraphContext, CTSM) on the Issue Selection site; I'm asking in the tech-help channel how to log in with my course credit so I can run the scan and grade my picks.

**Unit 6: Installed the voice guard and patched my scout's scope check; the repo-memory file is still to do**

I installed the voice guard (a hook in `~/.claude/settings.json` and my guide at `~/.claude/voice-guard/`), filled in the Upstream section of my voice guide, and confirmed it fires: `claude "run this command: gh issue comment --help"` was blocked with "no body the guard can read". Setup broke in two places: `repro-check` and `pr-precheck` weren't installed in `~/.claude/skills`, and my scout's `rubric.md` was still the blank template, so I installed the skills from my course repo and replaced the rubric with my Unit 1 version plus the scope patch. My own rule against promising "a timeline" conflicted with a claim that names a report-back date, so I reworded it to ban fix dates only, and my claim draft then passed both the guard and repro-check. I have not written `CLAUDE.local.md` for CTSM yet; that waits until I fork and clone the repo.

**Unit 7: Not started yet (written 2026-11-08)**

The ship-gate comes after I build the fix, which I have not started. I will fill this in with what the gate produced on my real branch.

**Unit 8: Not started yet (written 2026-11-08)**

The PR monitor needs an open pull request, and I do not have one yet. I will fill this in once I have a review thread for it to read.

---

## Why I Chose This Issue

I found my candidates on the course's Issue Selection site, where I held three Python-tagged issues: Canner/WrenAI #1421, CodeGraphContext #614 and ESCOMP/CTSM #2313. My Unit 5 scan never ran (a login problem, described in the Unit 5 entry), so I never got a ranked search. This week I graded the three by URL with my scout after patching its scope check. It accepted CTSM #2313 as "straightforward" (one new GitHub check, bounded), accepted WrenAI #1421 as "ambitious" while noting that a maintainer had moved that work to #1555, which is assigned, and rejected CodeGraphContext #614 because a maintainer said a design question must be settled first. I chose CTSM #2313, which the scout accepted, so I am not choosing against it.

It fits because it is small, well-scoped developer tooling: a CI check that fails when a line added in a PR ends in whitespace, skipping Python files. I can finish that in two weeks, even though CTSM is a Fortran climate model and not the AI work I would prefer. I hope to learn how GitHub Actions workflows are written and tested, and how to work in a project whose maintainers reply slowly (the scout's maintainer-answers check failed on it). On the tool: the scout held up on scope and claim checks, but its claim check did not follow WrenAI's redirect to another issue, and it could only read the first 40 of #1768's 55 config lines, so it could not tell whether that PR already covers trailing whitespace (the PR's description says it does). I would add a check that follows a maintainer's redirect to the issue the work moved to.

Claim comment: claim pending 2026-11-08 (I will add the link once it is posted).

**Toolkit**

- Tool: issue-scout (my rubric with the scope-feasible patch), run as `claude "issue-scout: grade <three issue URLs>"`
- What it produced: CTSM #2313 accept (straightforward); WrenAI #1421 accept (ambitious, redirected to #1555); CodeGraphContext #614 reject (settled-spec)
- Difficulties: the Unit 5 scan never ran; the installed `rubric.md` was still the blank template until this week; the claim check missed a maintainer redirect
- What changed: replaced `single-bounded-change` with `scope-feasible` (dated line in `scan-notes.md`); I would add a redirect-following claim check

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
