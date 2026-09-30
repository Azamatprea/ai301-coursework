# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

Azamatprea

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56#issuecomment-5917298674

Hi! I'd like to work on this one: `StructuralChunker.chunk()` returning `[]` for a document with no markdown headings, so it never reaches the RAG index.

My plan is to run the issue's snippet and `test_document_with_no_headings` on a clean checkout of current `main`, plus a control run with one leading `#` heading on the same text to check that missing headings are really the trigger. I'll post a repro report with my environment, commands, and output in a follow-up comment before I open a PR.

(I used Claude to help draft this comment; the plan and the run will be my own.)

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56#issuecomment-5917302751

Reproduced on current `main`. Everything below is from my own run, so anyone can re-run it.

**Environment**
- OS: macOS 26.5 (build 25F71), arm64
- Python: 3.14.6 (fresh venv)
- Repo: my fork `Azamatprea/pathreview-ai301-fa26-s1` at commit `f89c06f` (2026-09-16), unmodified, same as upstream `main`
- Deps: tiktoken 0.14.0, numpy 2.5.3, pytest 9.1.1

**Steps (from a clean clone)**
```
git clone https://github.com/Azamatprea/pathreview-ai301-fa26-s1.git && cd pathreview-ai301-fa26-s1
python3 -m venv .venv && . .venv/bin/activate
pip install tiktoken numpy pytest pytest-asyncio
```

1. The issue's snippet:
```
python -c "from ingestion.chunking.structural_chunker import StructuralChunker
c = StructuralChunker()
print(len(c.chunk('This is a plain document with no headings at all. ' * 20, {})))"
0
```

2. Control, same text with one leading `# Title` heading:
```
python -c "from ingestion.chunking.structural_chunker import StructuralChunker
c = StructuralChunker()
print(len(c.chunk('# Title\n' + ('This is a plain document with no headings at all. ' * 20), {})))"
1
```

3. The related test, first as marked (xfail), then with `--runxfail` to show the real assertion:
```
python -m pytest tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings -rX
tests/unit/test_structural_chunker.py x    [100%]
1 xfailed in 0.06s

python -m pytest tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings --runxfail
E       assert 0 >= 1
E        +  where 0 = len([])
1 failed in 0.06s
```

**Expected:** a document with no headings still produces at least one chunk (the test asserts `len(result) >= 1`).
**Actual:** `chunk()` returns `[]`, while the same text with one heading gives 1 chunk. So the missing heading is the trigger, and the whole document never reaches the index.

My guess (not verified yet): in `_extract_sections()`, content lines are only collected once `heading_stack` is non-empty, so a document with no headings ends with no sections. Next I'll confirm that in the code before proposing a change.

(I used Claude to help format this report; the commands and output are from my own run.)

## Eval iterations

**Run history**

1. Smoke run on pkg-20 and pkg-02 (`--only`, first attempt): 0/0 scored, every item errored with "claude exited 1" because my Claude Code login had expired. Not a rubric result.
2. Full run (first attempt): same auth failure, 0/0 scored, harness refused to write eval-run.txt.
3. After `/login`, smoke run `--only pkg-20,pkg-02`: 2/2 (disclosure 1/1, wrong-target 1/1).
4. Full confirming run with `--save-run eval-run.txt`: 19/20 scored items, bar 18/20: PASS, every category matched (clear-accept 7/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4).

**Package analysis**

pkg-05 (conda/conda#16543). My rubric decided reject, failed on steps-rerunnable; the gold label is accept. The report describes its `env.yml` ("a valid `dependencies:` list plus a `category:` section") instead of pasting it, and my steps-rerunnable pass condition says "every input they need is either inline or public". The grader read the missing file contents as an unshown input and failed it. The gold label reads it the other way: the description is precise enough that a stranger can rebuild the file in one line, and the artifact (the EnvironmentSectionNotValid warning on stdout plus the json.tool parse failure) shows exactly the issue's behavior. I think the gold is right: my check punished a minimal input that was described rather than one that was actually missing, which is the difference between pkg-05 and pkg-18 (a private monorepo nobody can rebuild).

**Check rationale**

"Pass if a stranger with only public resources could run the steps and reach the trigger: the exact command(s) or code are shown and every input they need is either inline or public. Fail if the repro depends on private code, unshared config, or files not shown, or if the steps skip the action the issue says triggers the bug."

This is steps-rerunnable. I wrote it outcome-first ("could a stranger reach the trigger") instead of counting steps, because in the Rubric Swap my step-count wording was read as a formatting check, and a terse report like calib-01 should pass. The "private code, unshared config" clause is there to catch pkg-18 (golangci-lint repro inside a private monorepo), and "skip the action that triggers the bug" catches wrong-target packages that never run the trigger.

**Trade-offs**

steps-rerunnable is strict about inputs, and that strictness costs me pkg-05: a precisely described but unpasted `env.yml` gets failed like an unshared config. Loosening it to "inline, public, or described precisely enough to recreate" would likely flip pkg-05 to accept, but it risks letting pkg-18 or pkg-06 through, and it touches the unfollowable-comms category. I kept it as is because 19/20 already clears the bar with every category matched, and a loosening would need a canary run (`--only pkg-05,pkg-18,pkg-06,pkg-20`) before another full run. I accept missing pkg-05-style cases in exchange for never passing a repro a stranger cannot rebuild.
