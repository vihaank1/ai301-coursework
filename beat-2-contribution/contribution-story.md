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
**Pull Request:** https://github.com/ESCOMP/CTSM/pull/4309

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

**Unit 6: Installed the voice guard, patched my scout's scope check, and posted a claim the guard let through**

I installed the voice guard (a hook in `~/.claude/settings.json` and my guide at `~/.claude/voice-guard/`), filled in the Upstream section of my voice guide, and confirmed it fires: `claude "run this command: gh issue comment --help"` was blocked with "no body the guard can read". Setup broke in two places: `repro-check` and `pr-precheck` weren't installed in `~/.claude/skills`, and my scout's `rubric.md` was still the blank template, so I installed the skills from my course repo and replaced the rubric with my Unit 1 version plus the scope patch. My own rule against promising "a timeline" conflicted with a claim that names a report-back date, so I reworded it to ban fix dates only, and my claim draft then passed both the guard and repro-check. I wrote a 22-line `CLAUDE.local.md` for CTSM (conventions, the commands I use, and that the repo states no AI policy), copied it into my clone and kept it out of git with `.git/info/exclude`. The claim is posted on #2313. I accidentally posted it twice, which the ship-gate's precheck noticed in the thread, and I deleted the second copy.

**Unit 7: Built the fix and ran the ship-gate; it stopped me once, then passed**

I wrote `ship-manifest.md` with three must-pass checks (the whitespace diff check on my branch, my new workflow file parses as YAML, and the repo's own `validate_xml.py`) and ran `check_gate.py` on my real branch. The first run stopped: the pr-precheck rejected it because my plan's last step (run the workflow on a PR) had no evidence and my PR description still had placeholders. I fixed that by running the workflow on two throwaway PRs in my fork (one passing in 14s, one failing in 10s) and writing the results into the description. The next runs passed. Two more things broke along the way: git's default whitespace rules flagged other things than trailing spaces, so I had to turn off `space-before-tab` and `blank-at-eof`; and the gate's voice-guide report said my Testing section described output instead of pasting it, so I rewrote it as separate command-and-output blocks and reran the gate. When I opened the PR through Claude Code, the gate blocked the first attempt because the command used a shell variable for the body file path, then passed once the path was literal.

**Unit 8: Installed the PR monitor, wrote my playbook, and ran it on the open PR: it says wait**

I installed the monitor (`~/.claude/pr-monitor/`), checked `monitor.py --state` (free, no model call), and wrote `playbook.md`: reply rules, provisional thresholds (14 days to a first review, 10 days after a reply), one nudge, and walk-away criteria including eight weeks of silence after the nudge. After opening PR #4309 I ran the full monitor (one model call). Its report: opened and last activity at 2026-11-08 23:31Z, 1 check passing, mergeable but blocked on a required review, 0 comments or reviews, state QUIET (under a day since the last activity by anyone but me). Its drafted move was "No nudge yet. Wait note.", which matches my playbook, so I posted nothing. GitHub is also holding CTSM's workflows, including mine, for a maintainer's approval, which the monitor does not show as a failure. I have not yet looked at recent merged outside-contributor PRs to adjust my thresholds, so they stay provisional until I do.

---

## Why I Chose This Issue

I found my candidates on the course's Issue Selection site, where I held three Python-tagged issues: Canner/WrenAI #1421, CodeGraphContext #614 and ESCOMP/CTSM #2313. My Unit 5 scan never ran (a login problem, described in the Unit 5 entry), so I never got a ranked search. This week I graded the three by URL with my scout after patching its scope check. It accepted CTSM #2313 as "straightforward" (one new GitHub check, bounded), accepted WrenAI #1421 as "ambitious" while noting that a maintainer had moved that work to #1555, which is assigned, and rejected CodeGraphContext #614 because a maintainer said a design question must be settled first. I chose CTSM #2313, which the scout accepted, so I am not choosing against it.

It fits because it is small, well-scoped developer tooling: a CI check that fails when a line added in a PR ends in whitespace, skipping Python files. I can finish that in two weeks, even though CTSM is a Fortran climate model and not the AI work I would prefer. I hope to learn how GitHub Actions workflows are written and tested, and how to work in a project whose maintainers reply slowly (the scout's maintainer-answers check failed on it). On the tool: the scout held up on scope and claim checks, but its claim check did not follow WrenAI's redirect to another issue, and it could only read the first 40 of #1768's 55 config lines, so it could not tell whether that PR already covers trailing whitespace (the PR's description says it does). I would add a check that follows a maintainer's redirect to the issue the work moved to.

Claim comment: https://github.com/ESCOMP/CTSM/issues/2313#issuecomment-6728250513 (posted 2026-11-08 through Claude Code, so the voice guard checked it first).

**Toolkit**

- Tool: issue-scout (my rubric with the scope-feasible patch), run as `claude "issue-scout: grade <three issue URLs>"`
- What it produced: CTSM #2313 accept (straightforward); WrenAI #1421 accept (ambitious, redirected to #1555); CodeGraphContext #614 reject (settled-spec)
- Difficulties: the Unit 5 scan never ran; the installed `rubric.md` was still the blank template until this week; the claim check missed a maintainer redirect
- What changed: replaced `single-bounded-change` with `scope-feasible` (dated line in `scan-notes.md`); I would add a redirect-following claim check

---

## Understanding the Issue

**Problem Description**

CTSM has no automated check that rejects a pull request adding lines that end in whitespace. Trailing spaces slip in, show up as noise in later diffs, and nobody finds out until review (or never). The issue asks for a GitHub check that does this. Python files are out of scope because `black` already handles those.

**Expected Behavior**

A PR that adds a line ending in a space or tab fails a GitHub check and says which file and line. A PR with clean added lines passes it.

**Current Behavior**

Nothing checks it. I added one line ending in a space to `README.md` in my fork and opened a PR; all six checks that CTSM runs passed (see Reproduction Process).

**Affected Components**

CI only: the `.github/workflows/` directory (16 workflow files on `master` today, none about whitespace). The maintainer comment on the issue says this can come out of #1768, a pre-commit PR that is shown as stalled, so that PR is related but not touched.

**Toolkit**

- Tool: repro-check, run on my claim draft before I posted it
- What it produced: a verdict that the claim matched what the thread showed (the #1768 comment, no competing PR)
- Difficulties: it could only read the issue thread; the workflow directory I had to look at myself
- What changed: I added the question about #1768 to my claim after reading that comment

---

## Reproduction Process

**Environment Setup**

macOS 15.6.1 on a MacBook Air, git 2.39.5 (Apple Git-154), Python 3.12.7. I forked ESCOMP/CTSM to `vihaank1/CTSM`, cloned my fork to `~/Documents/CTSM` and used `master` as the base. This is a CI-only change, so nothing needs building or running locally. Two challenges: my first clones into `~/Documents` left partial `.git` folders, which I deleted and re-cloned; and my repro script's first run failed at its branch step ("fatal: a branch named 'whitespace-repro' already exists"), so that run happened on my fix branch and its "workflow found" result was wrong. I noticed because the output listed my own `whitespace-check.yml`, cleaned up the stray line it added, and reran on the right branch (below).

**Steps to Reproduce**

1. On a branch from `master` named `whitespace-repro`, add one line ending in a space to `README.md` and commit it (commit 2077329).
2. Run the check the issue asks for, using git itself: `git -c core.whitespace=-space-before-tab,-blank-at-eof diff --check origin/master...HEAD -- . ':(exclude)*.py'`
3. List the workflows and search for one that mentions whitespace: `ls .github/workflows/*.yml | wc -l` and `grep -rli whitespace .github/workflows/`
4. Push the branch and open a PR in my fork (not upstream), then read its checks.
5. Observed: git sees the problem, but no CTSM check does.

**Reproduction Evidence**

- **Commit showing reproduction:** commit 2077329 on branch `whitespace-repro` in https://github.com/vihaank1/CTSM, shown in fork PR https://github.com/vihaank1/CTSM/pull/3
- **Screenshots/logs:**

```
== whitespace-repro: does git see the problem?
README.md:67: trailing whitespace.
+- [Linnea Hawkins] (https://github.com/linniahawkins)repro line with trailing space
exit code: 2
== workflows on this branch:       16 files
grep exit code: 1 (1 = no workflow mentions it)
```

`gh pr checks 3 --repo vihaank1/CTSM`:

```
All checks were successful
0 cancelled, 0 failing, 6 successful, 0 skipped, and 0 pending checks
Check that XML files are well-formed/check-xml (push)
Check that XML files are well-formed/check-xml (pull_request)
git-fleximod test/fleximod-test (3.7) (push)
git-fleximod test/fleximod-test (3.7) (pull_request)
git-fleximod test/fleximod-test (3.x) (push)
git-fleximod test/fleximod-test (3.x) (pull_request)
```

- **My findings:** reproduced. A PR with a trailing space passes every check CTSM runs, because none of the 16 workflows looks for whitespace, and git can already find it (exit 2). My first script run was on the wrong branch, which is why this evidence is from the second run.

**Toolkit**

- Tool: `repro.sh`, a script I wrote to print my environment, add the line, run the git check and search the workflows
- What it produced: the output above
- Difficulties: its branch step did not stop on failure, so its first run silently used the wrong branch and gave a misleading "found it" result; git's default whitespace rules also flagged things beyond trailing spaces
- What changed: I reran it on the right branch, and I would make the script stop if the branch step fails; I added the two `core.whitespace` flags to the command

---

## Solution Approach

**Analysis**

The cause is simply that the check does not exist. CTSM's workflows live in `.github/workflows/`, and `xml-check.yml` is a good model: its own file, runs on pushes and pull requests, checkout pinned to a commit SHA. Git already has what's needed, since `git diff --check` reports trailing whitespace on added lines only, so existing whitespace elsewhere in the repo never makes a PR fail.

**Proposed Solution**

One new workflow, `.github/workflows/whitespace-check.yml`, that runs on pull requests, checks out with full history, and runs `git diff --check` between the PR's base branch and its head with Python files excluded and git's other whitespace rules switched off. Deliberately NOT changing: the pre-commit setup in #1768 (that is the maintainer's separate plan), any existing whitespace in the repo (cleanup would be a huge unrelated diff), and Python rules (`black` covers them).

**Implementation Plan**

1. Add `.github/workflows/whitespace-check.yml` (21 lines), modelled on `xml-check.yml`.
2. Run the same command locally for three cases: clean branch, one added trailing space, same line in a `.py` file.
3. Run the workflow on two throwaway PRs in my fork (one clean, one with a trailing space).

Deviations: my claim said I would first post a small script and its output by Nov 15; I went straight to the workflow instead. The PR description does not mention that change from my claim, so I am adding a short note on the issue with the PR link (drafted in `issue-update.md`, and it goes through the voice guard). I also opened the PR before a maintainer answered my question about whether to build this standalone or wait for #1768; the PR description says that question is open.

**Toolkit**

- Tool: `plan.md`, the plan file my ship-gate's pr-precheck compares the diff against
- What it produced: a check that the diff was the single file the plan names, and a rejection on the first run because the plan's last step had no evidence
- Difficulties: none met beyond that rejection, which was correct
- What changed: I ran the fork tests and put their results in the description

---

## Testing Strategy

Before: fork PR #3 has a trailing space and every check passes, so nothing catches it. After, locally, with the command the workflow uses (three cases, exact output in `test_evidence.md` and in the PR description):

```
clean branch:              exit code 0
one added trailing space:  exit code 2, README.md:67: trailing whitespace.
same line in a .py file:   exit code 0
```

In my fork, the workflow ran on two throwaway PRs: the clean one passed (14s, https://github.com/vihaank1/CTSM/actions/runs/42534038809) and the one with a trailing space failed (10s, https://github.com/vihaank1/CTSM/actions/runs/42534044067); the failing PR's other six checks still passed. The repo's own `python3 .github/workflows/validate_xml.py` exits 0 and the workflow file parses as YAML. On the real PR #4309, GitHub is holding CTSM's workflows (including mine) for a maintainer to approve them, so they have not run upstream yet.

**Toolkit**

- Tool: ship-gate (`check_gate.py` with my manifest) and `gh pr checks`
- What it produced: PASS on my branch after three checks and the pr-precheck, plus the pass/fail rows from my fork runs
- Difficulties: the first run stopped for missing evidence; later reports flagged output being described instead of pasted
- What changed: I ran the fork tests, then rewrote the Testing section of the PR description as separate command-and-output blocks and reran the gate

---

## Implementation Notes

**2026-11-08**

Built `whitespace-check.yml` from the `xml-check.yml` pattern. First local test used `core.whitespace=blank-at-eol` alone and still flagged other rules (git starts from its defaults), so the command now turns off `space-before-tab` and `blank-at-eof` instead. Pushed two throwaway branches to my fork and opened PRs #1 and #2 there; the clean one passed and the one with a trailing space failed, as planned.

**2026-11-08**

Ship-gate stopped me on the first run (missing evidence for the plan's last step, placeholders in the description). I fixed both, reran, and it passed. After the gate's voice-guide report I rewrote the Testing section and reran it again, and it passed.

**2026-11-08**

Reproduction: my script's first run used the wrong branch, so I reran it on `whitespace-repro` and used that output. Opened fork PR #3 to show CTSM's checks all pass on a trailing space. Opened the real PR #4309 to ESCOMP/CTSM through Claude Code. I had posted my claim on the issue twice, so I deleted the second copy.

**Toolkit**

- Tool: git (`diff --check`), GitHub Actions in my fork, and Claude Code for running and checking commands
- What it produced: the workflow, the pass and fail runs, and the PR
- Difficulties: git's default whitespace rules; my own script's silent branch failure; the duplicate claim comment
- What changed: explicit `core.whitespace` flags; I would make my scripts stop on the first error

---

## Pull Request

**PR Link:** https://github.com/ESCOMP/CTSM/pull/4309

**PR Description:** Adds `.github/workflows/whitespace-check.yml`, which runs on pull requests and fails if any added line ends in whitespace, skipping Python files. It uses CTSM's PR template, says `Resolves #2313` and `Related to #1768`, shows the local and fork test output as pasted blocks, states that the question on the issue (standalone workflow or wait for #1768) is still open, and ends with an AI-use disclosure line.

**Maintainer Feedback:**

- 2026-11-08: none yet. GitHub shows CTSM's workflows (including mine) as awaiting a maintainer's approval to run, and the PR as blocked on a required review. My PR monitor run at 18:50 local reported QUIET with 0 comments and drafted "No nudge yet. Wait note.", so I did nothing.

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate through Claude Code, with the voice guard on every comment
- What it produced: the open PR #4309
- Difficulties: the gate blocked my first attempt because I used a shell variable for the body file path
- What changed: I reran with a literal path and it went through

---

## Learnings and Reflections

**Technical Skills Gained**

How a GitHub Actions workflow is written and tested on a fork before it goes upstream; how `git diff --check` and `core.whitespace` decide what counts as a whitespace error; how pinning a checkout to a commit SHA works; and how first-time contributors from a fork have their CI held for approval.

**Challenges Overcome**

The Unit 5 scan never ran, so I graded my picks by URL after patching my scout's scope check. My repro script silently used the wrong branch until I read its output closely. The ship-gate stopped me once for missing evidence, which was fair.

**What I'd Do Differently Next Time**

Wait for the maintainer's answer before building when I have asked a design question, or at least say plainly in the PR what is still open (I did the second, not the first). I'd also make my scripts stop on their first error and check each tool's output instead of trusting the summary line.

---

## Resources Used

- CTSM's `.github/workflows/xml-check.yml`, which I modelled my workflow on
- CTSM's CONTRIBUTING file and PR template
- Maintainer comment on https://github.com/ESCOMP/CTSM/issues/2313 saying this can come out of #1768, and that PR's description
- `git diff --check` and the `core.whitespace` setting in the git documentation
