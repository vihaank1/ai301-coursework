# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/136

**Branch**

`fix/53-parenthesized-phone-redaction`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run, and no partial re-runs: 20/20 scored items (bar: 18/20: PASS), with every category matched: `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. The `eval-run.txt` I committed ends with `agreement: 20/20 scored items  (bar: 18/20: PASS)`, which matches.

**Package analysis**

**pkg-09** (sharkdp/fd#2053). My rubric decided `reject`, and the gold label is `reject`. The gold note reads: "inner-error surfacing implemented, but the diff also rewords unrelated flag help text and adds a new --time-format flag the plan explicitly scopes out, under a description claiming 'exactly as planned'; arguable because the extra work is adjacent, but the new flag is unannounced scope."

My rubric rejects it because two required checks fail on it. "Diff matches plan" says: "Fail on any unplanned, undisclosed hunk beyond the fix, even a small adjacent one", and the new `--time-format` flag and the reworded help text are hunks the plan never names. "Description matches diff" fails because the description's "exactly as planned" claim is contradicted by those hunks. The package is arguable because the extra work is adjacent and the fix itself is right. I wrote the pass conditions to read the diff against the plan's "not in scope" list rather than to ask whether the extra work is reasonable, so a good fix does not excuse an unannounced flag.

**Check rationale**

The check, as it reads in `tools/pr-precheck/rubric.md`:

| No debris in the diff | Added and removed lines of the unified diff, and the commit messages. | The diff contains no debug output (print, eprintln, console.log, DEBUG logging), no commented-out code or abandoned first attempts, no dead or unused functions or imports added, no TODO or FIXME left on the change, and no formatting, re-indent, or re-ordering hunks unrelated to the fix. One real, in-scope fix with nothing riding along passes. Fail if any of these is present. | required |

It reads this way because pkg-12, pkg-15 and pkg-18 are correct, evidenced fixes that gold rejects for debris alone, so the pass condition has to name the specific tells (debug output, commented-out code, dead code or imports, a TODO left on the change, formatting or re-ordering hunks) and not ask whether the fix is good. I chose a list of observable tells that two graders can apply and get the same answer. I rejected a vaguer wording such as "the diff is clean", which grades an impression. It ends with "One real, in-scope fix with nothing riding along passes" so that a terse clear-accept such as pkg-19 is not held for being short.

**Trade-offs**

A case I accept the rubric will miss: "Maintainer direction engaged" is `preferred`, so it never changes a verdict. A PR that ignores an explicit maintainer instruction in the thread would still be accepted if every required check passes. I made it preferred because the packages in the set that fail on standards fail on the repo's own template and AI-disclosure asks (pkg-01, pkg-20), which are required checks, and I did not want the thread read to reject a PR on a judgment call about what counts as an instruction. I did not re-run any package with `--only` or add canaries, because I changed no file after the one full run; its fingerprints in `eval-run.txt` are the files I uploaded.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
