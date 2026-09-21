# Rubric: is this a good first issue?

Seven required checks, three preferred. The required checks are the four
families from the lecture (maintainer alive, repo in use, scope fits a
newcomer, nobody else is on it) plus the contribution-policy surface.
The preferred checks never change a verdict; they rank the issues that
survive.

Two rules run through the whole table. **All recency thresholds are
measured against the bundle's capture date in eval mode, and against
today in live mode.** And **each check fails only on the evidence it
names** — a check never borrows a reason from another row.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-not-archived` | The `archived:` flag on the repo line of the repo-facts block (live: the archive banner on the repo front page). | Passes when `archived: no`. Fails when `archived: yes` — an archived repo is read-only and cannot take a pull request at all. | required |
| `commits-recent` | The dated list under "last 5 default-branch commits", plus "last push to any branch", in the repo-facts block. Take the most recent date in that list. | Passes when the most recent default-branch commit is dated **within 90 days** of the capture date. Fails when the newest commit is older than 90 days. Judge this on commit dates only: a repo with no published release can still pass here, and a stale release cannot fail it. | required |
| `unclaimed-formally` | The "this issue:" line of the repo-facts block: `assignees:` and `linked PRs:` with a state per PR (live: the Assignees and Development boxes on the issue). | Passes when the issue has **no assignee** AND **zero linked PRs in the `open` state**. Linked PRs marked `closed` or `merged` do not count — an abandoned attempt is not a live claim. Fails on any assignee, or on one or more open linked PRs, including an open PR from a contributor's fork. | required |
| `no-active-claim` | Every comment in the Comments section, with its author and its date: comments saying "I'll take this", "working on this", "can I work on this", "@bot claim", or naming a pull request the commenter has opened. | Passes when no such claim comment is dated **within 90 days** of the capture date. A claim older than 90 days is stale and does not block the issue, especially where a maintainer has since invited new takers. Automated stale-bot or area-label bot comments are never claims. Fails when a human claim comment falls inside the 90-day window. | required |
| `single-bounded-change` | The issue title and body, and the `linked PRs:` line. | Passes when the body asks for **one change**, even a large or multi-file one. Fails only when the issue is explicitly an umbrella: it calls itself a tracking, mega, or meta issue; or its body is a **list of other issue numbers** to be worked separately; or it invites **an ongoing stream of separate PRs from different contributors** ("PRs welcome big and small", "incrementally adding more X"), which the linked-PR line usually confirms with many PRs from different people. A long, detailed task touching several files for one purpose is NOT an umbrella and passes. | required |
| `settled-spec` | The issue body, its labels, the author's `author_association`, and the full comment thread with author associations. | Applies to **feature and enhancement requests that add or change user-facing behavior**. Bug reports and documentation tasks are exempt and pass automatically. For a feature request, passes when a maintainer (OWNER / MEMBER / COLLABORATOR) has either filed it or stated a concrete approach in the thread. Fails when (a) a non-maintainer proposes the behavior and no maintainer has endorsed a concrete approach, or (b) the thread shows the correct behavior or format still being questioned with no maintainer having closed the question. A product decision nobody has made is not a newcomer's decision to make. | required |
| `ai-policy-permits` | The "contribution policy" line of the repo-facts block (live: `CONTRIBUTING.md`, the docs it links to, `AI_POLICY.md`, PR templates). | Fails **only** on an outright ban — wording that refuses AI-assisted or AI-generated contributions as such ("we do not accept AI-generated code or documentation"). Passes on conditions: disclosure, human review, personal understanding, testing requirements are terms to follow, not bans. Passes when no policy is stated at all; silence is not a restriction. | required |
| `released-recently` | "latest release" on the repo-facts block. | Passes when the latest release is dated within 365 days of the capture date. Graded `unclear` when the repo has published no releases — commits already carried liveness in `commits-recent`, and this row only ranks. | preferred |
| `maintainer-answers` | The "maintainer first-response sample" list in the repo-facts block. | Passes when at least one sampled issue drew a first owner/member/collaborator comment within 30 days. Never gates a verdict: a thin sample is normal in a repo whose maintainers work through pull requests rather than issue threads. | preferred |
| `newcomer-signposted` | The issue's labels, and the body's closing sections. | Passes when the issue carries a `good first issue` / `help wanted` / `easy` label, or the body names the files, directory, or acceptance criteria to start from. Ranks accepted issues; a friendly label is a hint, never a claim that the issue is free. | preferred |

## Verdict rule

**accept** when every `required` check grades `pass`. **reject** when any
`required` check grades `fail` or `unclear` — an unverifiable first issue
is not a first issue worth taking, so `unclear` is treated as `fail` on
required rows.

`preferred` checks never change the verdict. Grade and report them
anyway: on an accepted issue they are the tie-breakers that rank it
against the other accepted candidates, and an `unclear` on a preferred
row (for example a repo that publishes no releases) is not a mark
against the issue.
