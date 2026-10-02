# Plan for #53: parenthesized US phone numbers not redacted

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

## Repro evidence this plan relies on

From my unit 2 reproduction (`main` @ `2f4e82f`, Python 3.11.15):

```
$ python -c "from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(repr(s.scrub('Call me at (555) 123-4567 or 555-123-4567')))
print(s.detect('Call me at (555) 123-4567'))
print(s.detect('Call me at 555-123-4567'))"
'Call me at (555) 123-4567 or [REDACTED]'
[]
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

```
$ pytest tests/unit/test_pii_scrubber.py -k "us_phone_number_redaction or us_phone_formats or detect_phone_pii or phone_at_start_of_text" --runxfail -q
4 failed, 21 deselected in 0.70s
```

The dashed control in the same string is caught, so `scrub()`/`detect()` and the `phone_us` digit groups work; only the parenthesized form fails.

## Diagnosis

The `phone_us` pattern on line 16 of `safety/pii_scrubber.py` has two problems, and both block `(555) 123-4567`:

1. It starts with `\b` before the optional `\(`. In `"at (555)"` the characters on both sides of that position (a space and `(`) are non-word characters, so `\b` can never match there, and the `(` can never be part of a match.
2. Its separators are `[-.]?`, so the space after `)` in `(555) 123` is not allowed.

I checked this against the pattern directly: the current pattern finds `(555)123-4567` inside a longer string (no space, so it falls back to matching from the digits), but not `(555) 123-4567`. That matches the repro: the dashed number is caught and the parenthesized one with a space is not.

## Scope

In scope: the `phone_us` regex only, and removing the `@pytest.mark.xfail` marker from the four tests the issue names (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`). CONTRIBUTING.md says that removing the strict xfail marker is part of fixing a seeded bug.

Not in scope:
- `test_mixed_pii_and_text`. It also carries an `issue #53` xfail marker, but Shimmy0530 showed on the thread that it fails because the `street_address` pattern swallows `5 years developing Python appl`. That is not a phone bug, so its marker stays and it keeps xfailing.
- The `street_address`, `phone_intl`, `email` and `ssn` patterns.
- The `scrub()`/`detect()` logic, including the `# noqa: B007` line.

## Files I'll touch

- `safety/pii_scrubber.py`: the `phone_us` entry in `PII_PATTERNS` (line 16)
- `tests/unit/test_pii_scrubber.py`: delete the xfail decorator on the four tests above

## Approach

1. Replace the leading `\b` with `(?<!\w)`, which means "not preceded by a letter or digit". It still blocks matches in the middle of a longer number and also allows a match that starts at `(`.
2. Change the three separators from `[-.]?` to `[-.\s]?` so a single space is allowed. The new pattern is:
   `(?<!\w)(?:\+?1[-.\s]?)?\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})\b`
3. Remove the four xfail markers.
4. Run `make check && make test-unit`, per CONTRIBUTING.md.

## Test plan

1. Re-run the unit 2 snippet. Expected after the fix:
   ```
   'Call me at [REDACTED] or [REDACTED]'
   [{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
   [{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
   ```
   The first two lines flip from the repro output. The third (dashed control) is unchanged.
2. Re-run the unit 2 pytest command without `--runxfail`, now that the markers are gone. Expected: `4 passed`. Before the fix it was `4 failed`.
3. Run the whole file, `pytest tests/unit/test_pii_scrubber.py -q`. Expected: everything passes, except that `test_mixed_pii_and_text` still reports as xfailed. In particular, `test_detect_no_false_positives` and the SSN tests must still pass.

## Risks and unknowns

- Allowing a space as a separator could make digit runs like `555 123 4567` in non-phone text count as phone numbers. That's intended for a phone redactor, but I haven't checked a large text sample for false positives. The file's existing false-positive test is my only check.
- Ordering: `phone_us` runs before `ssn` in `PII_PATTERNS`. I checked that `123-45-6789` doesn't match the new pattern (its groups are 3-2-4), but I'm relying on the existing SSN tests to confirm that.
- If a maintainer wants `test_mixed_pii_and_text` handled under #53, that becomes a separate change to `street_address`, and I'd ask before doing it.

## Deviations

None. The build on `fix/53-parenthesized-phone-redaction` (commit `b58e0ab`) is the plan as written: the one `phone_us` regex change in `safety/pii_scrubber.py`, plus the four xfail markers removed in `tests/unit/test_pii_scrubber.py`. Every test-plan outcome matched what I predicted. The snippet now prints `'Call me at [REDACTED] or [REDACTED]'`, the four named tests went from `4 failed` to `4 passed`, and the full file went from `20 passed, 5 xfailed` to `24 passed, 1 xfailed`, with only `test_mixed_pii_and_text` still xfailing as planned. `ruff check` and `ruff format --check` pass on both files. The posted plan is still accurate, so no follow-up comment is needed.
