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

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
