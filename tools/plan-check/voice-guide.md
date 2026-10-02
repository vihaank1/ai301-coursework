# Voice guide: how I talk upstream

## Who I am in threads

I'm a CS student at UGA making my first contributions in Path Review.
I'm here to reproduce and fix one issue at a time, and readers should
expect exact commands and real output from me, not opinions about the
codebase. I'm new, so I say what I ran and what I saw, and nothing
past that.

## Rules I write by

### Rule: promise the next step, not the result

Before I've reproduced, I say what I'll do next. I never promise a fix,
a timeline, or that the bug is what the issue says it is.

- Wrong: "I'll have a fix up for this in a day or two."
- Right: "Next I'll run the issue's snippet and the four xfail tests and post what I see."

### Rule: name the issue's specifics

Every comment names something only this issue has: the input, the
function, the file, or the test. If the comment could be pasted onto
another issue, it's not ready.

- Wrong: "Hi, I'd like to work on this one, please assign me!"
- Right: "I'd like to work on the `(555) 123-4567` case that `PIIScrubber.scrub()` leaves unredacted."

### Rule: show output, don't describe it

When I say something happened, the output is right there in a code
block, copied from my terminal, not retyped from memory.

- Wrong: "Ran it and yeah, the phone number doesn't get redacted."
- Right: "Output from `python -c ...` on my machine:" followed by the pasted output.

### Rule: causes are guesses until I've shown them

I label a cause as a hypothesis unless I have output that proves it.

- Wrong: "The bug is the `\b` before `\(` in the regex."
- Right: "My guess is the `\b` before the optional `\(`; I haven't confirmed that yet."

### Rule: disclose AI help in one plain line

I use an AI assistant for drafting and checking. Every comment I post
says so in one line, and I only post words I've read and agree with.

- Wrong: (no mention, comment drafted with an assistant)
- Right: "AI disclosure: I used Claude to help draft and check this comment; I ran every command myself."

## Things I never post

- A fix date, a "guaranteed", or "this should be easy"
- "Please assign me" / "reserve this for me" (a classmate's claim doesn't block anyone here)
- "Same as above, can confirm" or any repro that isn't from my own environment
- "Confirmed" or "reproduced" before I have output to paste
- Output I edited, trimmed without saying so, or retyped by hand
- Praise filler ("amazing project!") in place of issue details
