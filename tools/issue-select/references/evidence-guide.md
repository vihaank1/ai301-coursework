# Evidence guide: where first-issue signals live on GitHub

Every rubric check needs an evidence source. This guide maps the four
criterion families to concrete places you can look: on github.com when
you are sizing up a live issue by hand, and in the snapshot bundle when
you are in eval mode. It closes with one repo-level surface the four
families do not cover: the contribution policy. If a signal is not
listed here, name your own source in the rubric; just make it somewhere
a grader can actually look.

## Family 1: is the maintainer alive?

A repo with code but no humans will leave your PR unreviewed forever.

| Signal | On github.com | In the eval bundle |
|---|---|---|
| Recent default-branch commits | the repo's front page: the line above the file list shows the newest commit and its date; click the commit count next to it to see the recent history | "last 5 default-branch commits" under Repo facts |
| Who is committing | the same commit list shows each commit's author. A commit authored by a bot (username ending in `[bot]`) is not maintainer life by itself, but a bot merging a human's pull request is: open the commit and see whose PR it merged | commit author names in the same list |
| Issue response latency | open a few recently updated issues (Issues tab, sort by recently updated) and see how long the first reply from someone with an Owner, Member, or Collaborator badge took | "maintainer first-response sample" under Repo facts |
| Maintainer activity in this thread | the badges next to commenters' names in the issue thread (Owner, Member, Collaborator) | the Comments section: each comment shows its author_association |

## Family 2: is the repo in use?

Contributing to abandoned software teaches the workflow but earns you a
merged PR nobody runs.

| Signal | On github.com | In the eval bundle |
|---|---|---|
| Release recency | the Releases box in the right sidebar of the repo front page: the latest release and its date | "latest release" under Repo facts |
| Last push | the newest commit's date on the front page; the Branches page shows when other branches last moved (a lively wire, not proof of health) | "last push to any branch" under Repo facts |
| Archived flag | an archived repo shows a "This repository has been archived" banner across the top and is read-only: hard dead | "archived:" on the repo line |
| Adoption scale | the star count at the top of the repo page; for libraries, the "Used by" counter in the right sidebar | stars on the repo line |

## Family 3: does the scope fit a newcomer?

This family lives in the issue text itself, not in any sidebar. Read the
body and the thread and ask:

- Is this one bounded piece of work? Scope fails when the issue is
  explicitly an umbrella or tracking issue (a list of sub-items meant to
  be split into separate work), when the thread shows the design is
  still being debated and no maintainer has settled it, or when a
  maintainer says outright that the fix touches core internals ("this
  needs changes to the parser").
- Short is not the same as unscoped. A terse body, an
  acceptance-criteria checklist, or a bug report without reproduction
  steps can still be a perfectly bounded first issue, especially when
  the opener is a maintainer or the issue carries a good-first-issue
  label. Grade the size of the work being asked for, not the polish of
  the writeup.
- Pure usage questions ("how do I get this to work?") are support
  requests, not contributions: fail.
- Age and history: an issue open for years with several abandoned
  attempts (closed, unmerged PRs in its history) is telling you
  something about its real difficulty.

## Family 4: is anyone already on it?

The label archaeology family. A "good first issue" label is a claim by the
maintainer that the issue is friendly; it is not a claim that the issue is
free. Verify both.

| Signal | On github.com | In the eval bundle |
|---|---|---|
| Assignee | the Assignees box in the issue's right sidebar | "this issue: assignees:" under Repo facts |
| Linked PRs | the Development box in the issue's right sidebar lists formally linked PRs: an open one is an active claim, a closed unmerged one is an abandoned attempt. Not every PR gets formally linked; people often just mention their PR in the comments, so read the thread too, and when the sidebar and the thread disagree, believe the thread | "linked PRs:" with state per PR, plus any PRs mentioned in the Comments section |
| Claim comments | read the thread for "I'll take this", "can I work on this", "working on this"; note the date and whether a maintainer answered | the Comments section |
| Label freshness | the small grey event lines between comments show when each label was added; compare with how long the issue has been open | issue open date vs capture date |

## The fifth surface: are you allowed to contribute the way you work?

An issue can pass all four families and still be a dead end, because
the repo's rules reject your workflow before a maintainer reads a line
of your code. Some projects ban AI-generated contributions outright;
many more set conditions (disclose AI use, personally understand and
test every change, human-review AI output). In this course your
contribution workflow is AI-assisted, so this surface applies to you.

| Signal | On github.com | In the eval bundle |
|---|---|---|
| Contribution policy | `CONTRIBUTING.md` in the repo root or `.github/`, and any contributor docs it links out to; the policy often hides one click away from the repo | the "contribution policy" line under Repo facts |
| Dedicated AI policy files | files like `AI_POLICY.md` or `AI_USAGE_POLICY.md`; an `AGENTS.md` file is the opposite signal, instructions written for AI coding agents | quoted or summarized on the same line |
| Templates | PR and issue templates sometimes require an AI-use disclosure checkbox | the same line |

How to grade what you find:

- **An outright ban** ("we do not accept AI-generated code") is a fail:
  submitting AI-assisted work against a stated ban wastes the
  maintainer's time and yours, however good the issue looks.
- **Conditions are not bans.** Disclosure, personal understanding,
  testing, and human-review requirements are terms to follow, not
  reasons to walk away. Most policies you will meet are this kind.
- **Silence passes.** Most repos state nothing; that is not a
  restriction.

## Reading the repo-facts block honestly

The bundle's repo-facts block is captured on a stated date. Every recency
threshold in your rubric ("within 90 days") is measured against that
capture date, not against today. Live mode measures against today.
