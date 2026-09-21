# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

**Verdict output**

Live-mode run of `issue-select` on three candidates (#53, #60, #73), output pasted
verbatim. The chosen issue, #53, is first in the ranked read-out and first in the JSON
array, with verdict `accept`.

```
Scope check — scope.md names codepath/pathreview-ai301-fa26-s3; all three URLs are
inside it. Path Review house rule noted: classmates' claim comments do not block an
issue. (Moot here — all three threads are empty.)

Repo facts gathered 2026-09-21: archived: no; last default-branch commits 2026-09-16,
2026-09-16, 2026-09-16, 2026-08-24, 2026-08-24 (all by Andrew Burke); no releases
published (no tags); docs/CONTRIBUTING.md states no policy on AI or contribution
tooling.

### Ranked read-out

Accepted, in fit order

1. #53 — PII scrubber fails to redact parenthesized US phone numbers — the best fit: a
   pure-Python regex fix in safety/pii_scrubber.py with four named failing tests, so it
   is fully verifiable with pytest alone and needs none of the Docker/make setup
   toolchain the fit profile wants to avoid.
2. #60 — Faithfulness checker crashes when context has text: None — same shape and even
   smaller (a .get() default that None defeats), with a runnable three-line reproduction
   and one named failing test; ranked second only because a one-token fix teaches less
   codebase reading.
3. #73 — README and .env.example disagree about LLM API key — documentation-only, which
   the fit profile welcomes, but it touches no Python and exercises the least code
   comprehension of the three.

Rejected: none.

| Check                  | #53           | #60           | #73           |
|------------------------|---------------|---------------|---------------|
| repo-not-archived      | pass          | pass          | pass          |
| commits-recent         | pass          | pass          | pass          |
| unclaimed-formally     | pass          | pass          | pass          |
| no-active-claim        | pass          | pass          | pass          |
| single-bounded-change  | pass          | pass          | pass          |
| settled-spec           | pass (bug)    | pass (bug)    | pass (bug)    |
| ai-policy-permits      | pass          | pass          | pass          |
| released-recently (P)  | unclear       | unclear       | unclear       |
| maintainer-answers (P) | unclear       | unclear       | unclear       |
| newcomer-signposted(P) | pass          | pass          | pass          |
```

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "Repo page shows no archive banner; tracker is accepting new issues (21 open, newest 2026-09-16)."},
      {"name": "commits-recent", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16, 5 days before today - well inside the 90-day window."},
      {"name": "unclaimed-formally", "grade": "pass", "evidence": "Issue #53 lists assignees: none and no linked PRs in any state."},
      {"name": "no-active-claim", "grade": "pass", "evidence": "Comment thread is empty (0 comments), so no claim comment exists inside the 90-day window."},
      {"name": "single-bounded-change", "grade": "pass", "evidence": "One change: widen the phone-number pattern in pii_scrubber.py to match the parenthesized format; no sub-issue list, no call for a stream of PRs."},
      {"name": "settled-spec", "grade": "pass", "evidence": "Bug report, exempt by the rubric; the correct behavior is stated - scrub() must redact '(555) 123-4567' as it already redacts '555-123-4567'."},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no policy on AI or contribution tooling; silence passes."},
      {"name": "released-recently", "grade": "unclear", "evidence": "No releases published (repo has no tags); preferred row, so liveness rests on commits-recent."},
      {"name": "maintainer-answers", "grade": "unclear", "evidence": "No issue in the tracker has drawn a maintainer reply yet - the tracker was seeded 2026-09-10, so there is no latency sample to read."},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue' and 'tier-1'; the body names pii_scrubber.py and four failing test cases."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "No archive banner; repo is writable and the tracker is live."},
      {"name": "commits-recent", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16, 5 days ago."},
      {"name": "unclaimed-formally", "grade": "pass", "evidence": "assignees: none; linked PRs: none."},
      {"name": "no-active-claim", "grade": "pass", "evidence": "0 comments on the thread."},
      {"name": "single-bounded-change", "grade": "pass", "evidence": "One change in FaithfulnessChecker.check(): make the None-valued 'text' key fall back to an empty string."},
      {"name": "settled-spec", "grade": "pass", "evidence": "Bug report, exempt; body gives a runnable reproduction and the exact TypeError raised."},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "docs/CONTRIBUTING.md is silent on AI use."},
      {"name": "released-recently", "grade": "unclear", "evidence": "No releases published."},
      {"name": "maintainer-answers", "grade": "unclear", "evidence": "No maintainer first-response sample available on this tracker yet."},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels 'good first issue', 'rag', 'tier-1'; body names the failing test test_none_context_chunk_text."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "No archive banner; issue #73 was opened 2026-09-16."},
      {"name": "commits-recent", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16, the same day this issue was filed."},
      {"name": "unclaimed-formally", "grade": "pass", "evidence": "assignees: none; linked PRs: none."},
      {"name": "no-active-claim", "grade": "pass", "evidence": "0 comments on the thread."},
      {"name": "single-bounded-change", "grade": "pass", "evidence": "One purpose across two files - make README.md and .env.example agree on OPENROUTER_API_KEY and the LLM_PROVIDER options; two files for one purpose is not an umbrella."},
      {"name": "settled-spec", "grade": "pass", "evidence": "Documentation task, exempt by the rubric; core/config.py already defines both keys, so the correct content is settled, not a product decision."},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no policy on AI or contribution tooling."},
      {"name": "released-recently", "grade": "unclear", "evidence": "No releases published."},
      {"name": "maintainer-answers", "grade": "unclear", "evidence": "No maintainer replies anywhere in the tracker yet to sample."},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels 'good first issue', 'docs', 'tier-1'; body names both files to change and estimates 1-2 hours."}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

Three runs, in order:

1. `--only issue-01,issue-04,issue-05,issue-09,issue-15,issue-20` — **6/6**. These six
   were the bundles I expected my draft rubric to get wrong, so I paid about $1.20 to
   test them before paying $4 for the whole set. They are the two long-but-accepted
   tasks (issue-01, issue-04), the stale-claim trap (issue-09), and the three scope
   rejects that a naive size check would have mishandled (issue-05, issue-15,
   issue-20).
2. `--only issue-02,issue-03,issue-06,issue-07,issue-08,issue-10,issue-11,issue-12,issue-13,issue-14,issue-16,issue-17,issue-18,issue-19`
   — **14/14**. The remaining fourteen, run as a second partial so that a full run would
   only be needed once.
3. Full run, `--save-run eval-run.txt` — **20/20 scored items (bar: 18/20: PASS)**,
   with `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1
   scope 4/4`. This is the run committed as `eval-run.txt`.

The rubric was not revised between runs; the partials were confidence checks on a rubric
I had already reasoned through bundle by bundle, not a repair loop.

**Issue analysis**

`issue-09` (conda/conda#7617, "conda config clear option").

My rubric decided **accept**. The gold label is **accept**. They agree.

The reasoning that produced my result is entirely in the `no-active-claim` and
`unclaimed-formally` rows. This bundle is built to sink a rubric that treats any claim
as a claim. Its thread opens with `MesaJonathan (NONE) on 2022-01-20` writing "I'd like
to take a swing at this as my first open-source contribution. Does it need to be
assigned to me?" — an unmistakable claim comment. It also carries one linked PR,
`conda/conda#11627 (closed)`. A rubric that fails on "a claim comment exists" or on "a
linked PR exists" rejects this issue and loses the point.

My two claim rows are written so that neither fires. `unclaimed-formally` counts only
linked PRs in the `open` state, and says so explicitly — `conda/conda#11627` is closed,
which the rubric reads as an abandoned attempt rather than a live claim.
`no-active-claim` counts a claim only if it is dated within 90 days of the capture date;
the claim here is dated 2022-01-20 against a capture date of 2026-08-05, roughly 1,658
days stale. The thread then confirms the reading rather than contradicting it: the
maintainer `jakirkham (MEMBER)` replied "Think you can just give it a try if you are
interested", and later commented "(bump)" — a maintainer inviting takers, not guarding a
claim.

The issue is also eighteen months older than anything else in the accept column, which
is why I did not let issue age become a check of its own. Age was telling me the issue
was unloved, not that it was unavailable, and the gold label agrees.

**Check rationale**

From `tools/issue-select/rubric.md`, the `no-active-claim` row as it is currently
written:

> | `no-active-claim` | Every comment in the Comments section, with its author and its date: comments saying "I'll take this", "working on this", "can I work on this", "@bot claim", or naming a pull request the commenter has opened. | Passes when no such claim comment is dated **within 90 days** of the capture date. A claim older than 90 days is stale and does not block the issue, especially where a maintainer has since invited new takers. Automated stale-bot or area-label bot comments are never claims. Fails when a human claim comment falls inside the 90-day window. | required |

Three decisions are baked into that wording.

The first is that the check is dated at all. My first draft read "no comment claims the
issue", with no window, and I could not apply it to issue-09 without rejecting an issue
whose maintainer had explicitly invited takers. A claim is a statement about who is
working on something *now*, so it has to decay. 90 days is the same window I used in
`commits-recent`, which keeps the rubric to one recency constant instead of several I
would have to justify separately.

The second is the exclusion of bots. issue-08's thread ends with `zulipbot` writing
"We noticed that you have not made any updates to this issue or linked PRs for 10 days.
Please comment here if you are still actively working on it." That is a bot *questioning*
a claim, and issue-08's own gold note says "the bot nudge does not clear the claim". Left
unstated, a bot comment could be read either as a claim or as a release; naming bots as
never-claims removes the ambiguity in both directions, and issue-08 is then rejected by
`unclaimed-formally` on its assignee and open PR, which is the honest reason.

The third is that this row grades only the comment thread. Assignees and linked PRs live
in `unclaimed-formally`. Splitting them means the harness's `note` column tells me which
kind of claim fired, so a disagreement points at one row to fix rather than two.

**Trade-offs**

The 90-day window is a bet that a claim goes stale on a fixed clock, and it will be
wrong in both directions eventually. It waves through a genuine slow contributor — a
volunteer who claimed an issue four months ago and is still, quietly, working on it —
and it blocks on a drive-by "can I work on this?" posted last week by someone who never
returned. It also only reads comments, so a claim made anywhere else (a project board, a
Discord thread, a maintainer's mailing list) is invisible to it.

On this eval set, nothing changed, and here is how I know. issue-09 was in my first
`--only` run precisely as the canary for this row: it is the only scored bundle whose
accept verdict depends on a stale claim being discounted, and it came back `accept` in
that partial run and again in the committed full run. In the other direction, the window
never had to do the rejecting: the four bundles in the `claimed` category (issue-03,
issue-08, issue-13, issue-18) all fail `unclaimed-formally` first, on an assignee or on
at least one open linked PR, and issue-13 has no comments at all. So `no-active-claim`
carried zero rejects across the twenty scored items. It is currently insurance rather
than a load-bearing check, and if I were tuning for this eval set alone I could delete
it without losing a point. I kept it because live mode is where it earns its place:
Path Review issues are claimed in comments, and the house rule in `scope.md` tells the
skill to discount classmates' claims there, which is a rule that needs a claim check to
attach to.

---

## Selection rationale

**Selection rationale**

*1. The issue's fit to my interests and to the time available.*

I picked #53, the PII scrubber missing parenthesized phone numbers, because it is a
Python change I can prove I got right. The fix lives in one regex in
`safety/pii_scrubber.py`, and the issue names four failing tests, so "done" is not a
judgment call — it is `pytest` going green. That matters to me more than the subject
matter does right now. I am comfortable in Python and I have used regex plenty, so
almost all of the work is the part I actually want practice at: finding my way around a
codebase I did not write and opening a pull request a maintainer can review without
having to ask me questions. On time, this is the realistic piece. PathReview's full
setup wants Docker and `make setup`, and the safety module is pure functions, so I can
reproduce the bug and verify the fix from the test suite without standing up the whole
application. I have a week for Unit 2, not a month.

*2. What the verdict identified correctly, and what I weighed that the rubric could not.*

The rubric got the mechanical facts right and I trust them: nothing is assigned, no PR
is open against it, the thread is empty, the repo was pushed to five days ago, and
`docs/CONTRIBUTING.md` says nothing that would rule out how I work. The `settled-spec`
row also correctly waved it through as a bug report — the correct behavior is not up for
debate, `(555) 123-4567` should be redacted the same way `555-123-4567` already is.

What the rubric could not weigh is that all three candidates passed every check, so the
verdict alone did not choose for me. It accepted #60 and #73 just as cleanly. Three
things decided it that no check in my table measures. First, #60 is *too* small — the
fix is giving `.get()` a real default, which is a one-token change I would learn almost
nothing from. Second, #73 is documentation only; the rubric cannot tell that I already
know how to edit a README, and Unit 2 is about reproducing a bug, which a docs issue
gives me no practice at. Third, and the one I keep coming back to: #53 has four failing
tests attached to it, and a bug with tests already written is a bug someone has already
proven is real. My rubric has no row for "how cheaply can I confirm this is broken
before I start", and that is the single thing I cared about most.

*3. The anticipated difficulty in claiming it.*

Low, and the Path Review house rules are why. `scope.md` spells out that this is a
classroom repo: classmates' claim comments do not block an issue, several people can be
on the same one, and credit attaches to the pull request I open rather than to whether
it merges. Right now #53 has no comments and no assignee anyway, so I am not even
stepping on anyone. The real risks are smaller and more boring. One is that #53 is
labelled `good first issue` and `tier-1` in a repo full of students who were all handed
the same tracker this week, so it may well be claimed by the time I comment — which
costs me nothing under the house rule, but I would rather know than assume. The other is
that I have not written the claim comment yet; Unit 2 covers that with the voice guide,
and per the assignment I have deliberately not commented on the issue. The thing I am
least sure of is not the claim, it is CI: `docs/CONTRIBUTING.md` requires all five jobs
green — `lint`, `typecheck`, `test-unit`, `test-integration`, `frontend` — and
`test-integration` is the one that might need services I have not stood up. That is the
part of Unit 2 I expect to lose time to, not the claiming.
