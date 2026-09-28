# Evidence guide: where proof lives in a reproduction package

The map for every check in `rubric.md`. In eval mode the bundle is the
whole world: "Issue" and "Thread highlights" are the issue side, "Repo
facts" is the repo's conventions, "Candidate claim comment" and
"Candidate repro report" are the package. In live mode the issue side
comes from the GitHub issue page (title, body, labels, every comment)
and the repo's own docs (README, CONTRIBUTING, SETUP, issue templates,
any AI policy file); the package is the student's draft file(s).

## Environment

- Where it lives: the "Environment" line or opening lines of the
  candidate repro report (sometimes folded into the first step, e.g. a
  `--version` output or a `pip show` line). The issue's target lives in
  the issue body (the version it was filed against, the OS, any
  driver/build/shell/locale it names) and in thread highlights (a
  maintainer saying "confirmed on main" or "only on Windows" / "only
  in release builds"). Live: the issue body and comments; the repo's
  SETUP doc for what a standard environment is.
- What good looks like: the report names (1) the version it ran, (2)
  the OS/platform, and (3) every setting the issue or thread says
  matters. The version and platform match the issue's target, or the
  report says in its own words that they differ ("filed against 13.0.0;
  unchanged on 15.2.0"). A record that silently tests an older release
  than the one the issue was confirmed on is a deviation, not an
  environment. Platform differences only count when the issue ties
  the bug to a platform ("Windows-only", "macOS + fish"); Arch vs.
  Ubuntu on a cross-platform CLI bug is not a deviation. No record at
  all fails, even when the log looks right,
  because nobody can place the attempt.

## Steps

- Where it lives: the "Steps" section of the candidate repro report,
  plus any code/config/input file it shows inline. The trigger lives in
  the issue body (the exact input, syntax, flag, config, or sequence
  the reporter says causes the bug) and in maintainer notes in the
  thread ("only with `--replace`"). Live: the draft repro comment and
  the issue's own reproduction steps.
- What good looks like: from a clean start, a stranger could type
  exactly what the report shows and get to the same point. Exact
  commands or code are given; inputs are shown or are unambiguously the
  issue's own; nothing depends on a private repo, an unshared config,
  or "our internal setup". Compare the report's input to the issue's
  character by character where it matters: a changed operator, range
  form, variable, or dropped flag means the trigger is gone, however
  good the rest looks. Terse is fine; short steps that carry the
  trigger pass, and an input described precisely enough to rebuild
  ("an env.yml with a valid dependencies list plus a `category:`
  section") counts as given. What fails is an input only the author
  has: a private repo, a config they did not share.

## Behavior shown

- Where it lives: fenced output in the candidate repro report
  (terminal output, tracebacks, exit codes, logs, CSS/rendered output,
  JSON), and the report's "Expected"/"Actual" lines. The target
  behavior lives in the issue body: the exact error type or message,
  exit code, wrong value, or crash described there.
- What good looks like: the artifact displays the issue's behavior
  itself: same error class and message family, same wrong value, same
  crash vs. non-crash, produced by the issue's trigger. Watch for
  adjacent behaviors that look similar: a graceful argument/syntax/
  compile error (exit 1) instead of a panic or crash (exit 101); an
  old version's ValueError instead of the current bug; garbled output
  in a terminal that is still running, narrated as a crash; banners,
  version strings, or session lists that only prove the program runs.
  A control run (the same command without the trigger, behaving
  correctly) strengthens a pass but is not required. For an honest
  cannot-reproduce, good is an artifact showing what the faithful
  attempt produced.

## Honesty

- Where it lives: the verbs and adjectives in both the claim comment
  and the repro report ("reproduced", "confirmed", "verified",
  "guaranteed", "always", "root cause is"), the "Actual" line, and any
  statement of scope (other versions, other platforms, other
  channels). Hold each against the artifacts found under Behavior
  shown.
- What good looks like: each claim points at something shown, and
  says no more than it shows. "Actual" restates what the artifact
  displays. A cause is labeled as a hypothesis ("likely", "my guess")
  unless evidence for it is shown. A cannot-reproduce says what was
  tried, shows the result, and names what differed from the
  reporter's setup; that is honest and passes. It fails when a
  diagnosis is asserted with no transcript, when "confirmed" sits on
  top of an artifact that shows something else, or when the report
  generalizes the bug to a version or platform it never tested (or
  that maintainers said does not reproduce). The artifact bar is on
  the central claim; a control run or side observation described in
  prose, consistent with what is shown, is not an overclaim.

## Comms

- Where it lives: the candidate claim comment, read against the issue
  title/body/thread; the "Repo facts" block's contribution policy line
  (CONTRIBUTING, AI_POLICY, AI usage sections) and bug-report template
  asks. Live: the draft claim comment, the issue page, the repo's
  CONTRIBUTING and any AI policy file, and `scope.md`'s house rules.
- What good looks like, claim: it names something only this issue has
  (the symptom, input, function, file, or component) and a concrete
  next step ("I'll read how X computes Y"), and it promises
  investigation or a report, never a fix by a date, a guarantee, or a
  reservation. Interchangeable "please assign me, I'll fix it fast"
  text, praise-and-request with no issue detail, and bare "+1 same
  here" all fail.
- What good looks like, disclosure: read the policy text for what it
  requires and where. If it requires disclosing AI use in comments or
  in any form, the claim or report must contain an explicit
  disclosure (tool and extent), because every package is treated as
  AI-assisted work. If the policy is permissive, requires disclosure
  only in pull requests, only asks for human-written words, or states
  no AI policy, no disclosure is needed and this passes. A disclosure
  that is present when not required is fine.
