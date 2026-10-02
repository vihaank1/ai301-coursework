# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

vihaank1

**Plan comment**

TODO-PASTE-PLAN-COMMENT-PERMALINK

Plan for #53, built from my own reproduction.

My repro on `main` @ `2f4e82f`: `scrub('Call me at (555) 123-4567 or 555-123-4567')` returns `'Call me at (555) 123-4567 or [REDACTED]'`, `detect()` returns `[]` for the parenthesized number but finds `555-123-4567`, and the four tests named in the issue fail with `--runxfail` (`4 failed`).

**Diagnosis:** the `phone_us` pattern (`safety/pii_scrubber.py` line 16) has two gaps. Its separators are `[-.]?`, so the space after `)` isn't allowed. It also starts with `\b` before the optional `\(`, and a word boundary can't fall between a space and `(`, so the `(` can never start a match. I get the same separator reading others posted here, plus the `\b` part.

**Change (one regex, plus markers):**
- Replace the leading `\b` with `(?<!\w)` and widen the separators to `[-.\s]?`.
- Remove the `xfail` markers from `test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii` and `test_phone_at_start_of_text`, as CONTRIBUTING.md asks for seeded bugs.

**Not in scope:** `test_mixed_pii_and_text`. As Shimmy0530 showed above, it fails because `street_address` matches `5 years developing Python appl`, not because of the phone pattern. Its marker stays, and I'd treat it as a separate fix unless a maintainer wants it folded in here. No other patterns change.

**Test:** re-run the repro snippet and expect `'Call me at [REDACTED] or [REDACTED]'`, with `detect()` returning a `phone_us` match for `(555) 123-4567` (the dashed control unchanged). The four tests go from `4 failed` to `4 passed` with their markers removed. Then the full `test_pii_scrubber.py` should pass, with only `test_mixed_pii_and_text` still xfailed, and `make check && make test-unit` should pass.

**Risk:** allowing a space separator could flag spaced digit runs in non-phone text. I'm relying on `test_detect_no_false_positives` and the SSN tests to catch regressions, and I haven't tested a larger text sample.

AI disclosure: I used Claude (an AI assistant) to help read the regex, draft this plan, and check it against the tests. I reviewed and edited the plan myself before posting.

---

## Your branch

**Branch**

fix/53-parenthesized-phone-redaction

**Evidence**

```
=== BEFORE (main @ 2f4e82f) ===
$ python -c "<repro snippet>"
'Call me at (555) 123-4567 or [REDACTED]'
2026-10-02 18:17:36 [info     ] pii_detected                   count=0 types=0
[]
2026-10-02 18:17:36 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
$ pytest tests/unit/test_pii_scrubber.py -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text" --runxfail -q
E       AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
E           AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
E       assert 0 > 0
E        +  where 0 = len([])
E       AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text
4 failed, 21 deselected in 0.08s
$ pytest tests/unit/test_pii_scrubber.py -q
20 passed, 5 xfailed in 0.05s
```

```
=== AFTER (branch fix/53-parenthesized-phone-redaction) ===
$ python -c "<repro snippet>"
'Call me at [REDACTED] or [REDACTED]'
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
$ pytest tests/unit/test_pii_scrubber.py -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text" -q
4 passed, 21 deselected in 0.04s
$ pytest tests/unit/test_pii_scrubber.py -q
24 passed, 1 xfailed in 0.05s
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, saved with `--save-run eval-run.txt`: 19/20 scored items (bar: 18/20: PASS), categories clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. The only disagreement was pkg-14 (gold accept, graded reject on executable-approach). Since this run already cleared the bar and the category floor, I kept it as my final run instead of loosening a check to chase pkg-14.

**Package analysis**

pkg-14 (zellij-org/zellij#5174). Gold: accept. My rubric: reject, failed on executable-approach. The plan's diagnosis is grounded (fresh attach is clean, the leak starts at reattach, 0.44.1 is clean), the scope is bounded to the Unix reattach handshake with the Windows variant deferred and explained, and the test plan is decisive (5 SSH reattach cycles with no rgb strings in any pane). The problem for my rubric is the Files section: it names the attach/reattach path in `zellij-server` and the query issuance in `zellij-client`, but says "exact functions to be pinned in the PR after tracing the query issuance with debug logs." My pass condition fails a plan when "key decisions are deferred to build time," and the grader read "exact functions to be pinned in the PR" as exactly that. The gold label reads it as ready: the change itself is chosen (drain OSC query responses before pane input is wired, bounded to OSC patterns), and only the precise function name is left to trace. I think gold is right, because a stranger could start from the named crate and code path, and the plan isn't deferring a decision about what to do, only which line it lands on. My check doesn't separate "deferring the decision" from "deferring the exact line number" well enough.

**Check rationale**

| executable-approach | The plan's approach: named files, functions, or code sites, and the chosen change. | Pass if a stranger could start the edit without asking the author anything: at least one concrete file, function, or code site is named AND one specific change is chosen. Fail if the location is "somewhere", the layer is undecided ("X or Y, whichever is easier", "not sure"), the work is "investigate/profile first", or key decisions are deferred to build time. | required |

It reads this way because the unbuildable packages all fail the same way: they never commit to where or what. pkg-18 says recover() "somewhere" and "upstream or vendored, whichever is easier," pkg-17 says "gocui? tcell? not sure," and pkg-10 is profile-and-optimize with no files. So the pass condition needs two things together: a concrete location AND one chosen change, and the fail list names those exact dodges ("somewhere", "X or Y, whichever is easier", "investigate/profile first"). I rejected a looser version that only asked for "a named file or area," because pkg-17 and pkg-18 both name an area (an input stack, a linter runner) and would have passed. I also rejected counting sections or requiring a "Files:" heading, because calib-01 is terse with one named file and is ready, so the check has to judge whether a stranger could start, not how the plan is laid out.

**Trade-offs**

executable-approach gives up pkg-14. Its clause "key decisions are deferred to build time" is what catches pkg-18, where every real decision (where to recover, upstream or vendored, which other linters) is pushed to the build. But the same clause also catches a good plan that has chosen its change and only leaves the exact function to be pinned by tracing, which is pkg-14 (gold accept, my verdict reject). I could loosen it to "deferring the exact function is fine if the file/module and the change are chosen," but that risks letting pkg-17 or pkg-18 through, since they also gesture at a module, and with this run already at 19/20 with every category matched I chose not to spend another full run on it. The case I accept it will miss: an honest, grounded plan whose author says "I'll pin the exact function after tracing" will be held even when a maintainer would happily let them start.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
