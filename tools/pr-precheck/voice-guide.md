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

### Rule: state an approach as a proposal, and name what I haven't checked

A plan comment commits me to an approach in front of the maintainers, so I say which change I'm proposing and flag the part I'm unsure of as a question.

- Wrong: "The fix is to rewrite the section parser; this will solve it."
- Right: "I'd add a fallback in `_extract_sections()` for text with no headings. I haven't checked whether this changes chunk counts for documents that do have headings; I'll test that first."

### Rule: answer the maintainer's direction before proposing my own

If a maintainer already suggested an approach, my comment says whether I'm following it, and why if I'm not.

- Wrong: (ignores the owner's note and proposes something else)
- Right: "Following your note about the split happening in `_extract_sections()`, I'll keep the change there."

### Rule: quote my own repro, not a summary of it

The plan quotes the output from my repro comment that the diagnosis and test plan rely on.

- Wrong: "As shown in my repro, this is the cause."
- Right: "My repro showed `len(chunk(text))` is 0 without a heading and 1 with one leading `# Title`."

### Rule: a PR title earns its thirty seconds

The title says what the change does to which component, in the repo's commit style, so a maintainer can triage it without opening the diff.

- Wrong: "Fix issue 56" / "Updates"
- Right: "fix(ingestion): keep documents with no headings in StructuralChunker"

### Rule: the description promises exactly what the diff contains

Every sentence about content or coverage must be true of `git diff main...HEAD`. I list what changed, and I do not claim "no other changes" unless the stat shows it.

- Wrong: "Implements the plan exactly, with no other changes." (when the diff touches a file the plan never named)
- Right: "Changes `_extract_sections()` in `structural_chunker.py` and removes the xfail marker on `test_document_with_no_headings`; nothing else."

### Rule: a shortfall is stated plainly, once, with its reason

When the PR leaves something out on purpose, I say what and why in one sentence, so it reads as a scoping decision and not an apology.

- Wrong: "Sorry, I couldn't get to the text-before-first-heading case, hopefully that's ok."
- Right: "Text before the first heading in a document that has headings is still dropped; it is a different symptom and stays out of this PR."

### Rule: in the PR, AI use goes in Notes for Reviewers in my own words

I say which tool I used, for which steps, and what I did myself (ran the commands, read the diff).

- Wrong: "Used AI tools." / (nothing, when the template asks)
- Right: "I used Claude to help trace the cause in `_extract_sections()` and draft the two-line change; I ran the tests and read the full diff myself."

## Things I never post

- "Same as above, can confirm" or any piggyback on a classmate's repro.
- A fix promise or a delivery date.
- "Guaranteed", "definitely", "100%" about anything I haven't shown.
- Output I didn't run myself, or output edited to look cleaner.
- "Please assign me" with nothing about the issue.
- A delivery date or "I'll have a PR up by Friday" for the build.
- A plan that adds work the issue didn't ask for ("while I'm in here...").
- "Same approach as above" or any plan copied from a classmate's comment.
- "Tests pass" with no command or output behind it.
- A ticked checklist box that my pasted output does not support.
- "No other changes" or "exactly as planned" without checking the diff stat first.
