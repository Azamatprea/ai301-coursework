# Voice guide: how I talk upstream

## Who I am in threads

I'm Azamat, a student in CodePath AI301 and a forward deployed engineer who works on RAG/ingestion pipelines day to day. In Path Review I'm a first-time contributor: I reproduce, I report what I actually saw, and I say what I'll check next. Readers can expect short comments with commands and output, not promises.

## Rules I write by

### Rule: promise investigation, not a fix

I only commit to the next thing I'm going to look at. No fix guarantees, no dates.

- Wrong: "I'll have a fix up for the chunker by Friday."
- Right: "Next I'm going to run the failing test on main and post what I see."

### Rule: name the specifics

Every comment names this issue's component, trigger, or test, so it couldn't be pasted on another issue.

- Wrong: "Hi, I'd love to work on this, please assign me!"
- Right: "I'd like to work on the StructuralChunker dropping docs with no headings (test_document_with_no_headings)."

### Rule: say only what my output shows

"Reproduced" means I pasted output that shows the reported behavior. Guesses about cause are labelled as guesses.

- Wrong: "Confirmed, the root cause is definitely the header regex."
- Right: "The test fails with the output below; my guess (not verified yet) is that the header split returns no sections."

### Rule: disclose AI help when the repo asks

If the repo's policy asks for AI disclosure, I say what tool I used and for what, in one line.

- Wrong: (no mention, when CONTRIBUTING.md requires it)
- Right: "I used Claude to help draft this comment and check my repro against my rubric; the commands and output are from my own run."

### Rule: record the environment every time

Every repro comment names Python version, OS, and the commit I tested.

- Wrong: "Ran the tests and it fails."
- Right: "Python 3.11.9, macOS 15, fork at commit abc1234: `pytest tests/test_chunker.py -k no_headings` fails:"

## Things I never post

- "Same as above, can confirm" or any piggyback on a classmate's repro.
- A fix promise or a delivery date.
- "Guaranteed", "definitely", "100%" about anything I haven't shown.
- Output I didn't run myself, or output edited to look cleaner.
- "Please assign me" with nothing about the issue.
