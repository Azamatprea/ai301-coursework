# Plan: issue #56, StructuralChunker drops documents with no headings

## Repro evidence I rely on (from my repro comment on #56, fork at f89c06f, Python 3.14.6)

- Issue snippet: `StructuralChunker().chunk('This is a plain document with no headings at all. ' * 20, {})` printed `0`.
- Control, same text with one leading `# Title` line: printed `1`.
- `test_document_with_no_headings` with `--runxfail`: `E assert 0 >= 1  +  where 0 = len([])`, `1 failed`; as marked it shows `1 xfailed`.

## Diagnosis

The control shows the missing heading is the trigger: the same text produces 1 chunk with a heading and 0 without. Reading `_extract_sections()` in `ingestion/chunking/structural_chunker.py`, two conditions require a heading before any content is kept:

1. In the `else` branch, a content line is only appended `if heading_stack or current_section_lines`. With no heading, both are empty on the first line, so no line is ever appended and the list stays empty.
2. The final save is `if current_section_lines and heading_stack`, which also needs a heading.

So a headingless document ends with `sections == []` and `chunk()` returns `[]`. This comes from reading the code against the repro; I have not instrumented it. Build step 1 prints `sections` for the issue's text to confirm it before I change anything.

## Scope

In scope: `_extract_sections()` in `ingestion/chunking/structural_chunker.py`, and removing the strict `xfail` marker on `test_document_with_no_headings` in `tests/unit/test_structural_chunker.py` (a strict xfail would turn a passing test into a failure).

Not in scope:
- Text that appears before the first heading in a document that does have headings. It is dropped today and stays that way; it is a different symptom from this issue.
- `strategy_selector.py` and `semantic_chunker.py`. Only `readme` source types reach `StructuralChunker` through the selector, and I am not changing which chunker is chosen.
- Any change to heading parsing, the 800-token limit, or metadata for documents that have headings.

## Files I will touch

- `ingestion/chunking/structural_chunker.py`
- `tests/unit/test_structural_chunker.py`

## Approach

1. Confirm the diagnosis: run `StructuralChunker()._extract_sections(text)` on the issue's text and check it returns `[]`.
2. In `_extract_sections()`, append every content line to `current_section_lines` (remove the `heading_stack or current_section_lines` condition).
3. Change the final save to `if current_section_lines:`, so a document with no headings yields one section with path `[]` and level `0` (`heading_path` is then `""`, `heading_level` is `0`). Sections in documents that do have headings are built exactly as before.
4. Remove the `xfail` decorator from `test_document_with_no_headings`.
5. Commit on branch `fix/56-structural-chunker-no-headings`.

## Test plan

Re-run my unit 2 repro on the branch. Expected after the fix:

1. The issue's snippet prints `1` (was `0`).
2. The control with a leading `# Title` still prints `1` (unchanged).
3. `pytest tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings` reports `1 passed` with no `--runxfail` (was `1 xfailed`, and `assert 0 >= 1` under `--runxfail`).
4. The whole file `tests/unit/test_structural_chunker.py` passes, so documents with headings are unchanged.

## Risks and unknowns

- I have not checked whether any caller relies on `chunk()` returning `[]` for headingless text. I only found `StrategySelector.chunk()` and `ingestion/pipeline.py` calling it; I will grep the call sites before I open the PR in unit 4.
- A headingless document over 800 tokens goes to `SemanticChunker` with `heading_path == ""`. I have not run that case, so I will add it to the checks in step 3 of the test plan if it behaves oddly.

## Deviations

Nothing about the planned change moved: same two files, same two conditions in `_extract_sections()`, same `xfail` removal, same out-of-scope list. Two things were different in how I built and checked it:

- I committed the change on `fix/56-structural-chunker-no-headings` through GitHub's web upload instead of a local clone, and the fork's CI (run manually with workflow_dispatch) ran the unit tests. I did not re-run the issue's snippet locally after the change, and I did not do build step 1 (printing `_extract_sections()` output); the test going from XFAIL to PASSED is my confirmation of the diagnosis.
- I grepped the callers I listed as an unknown: only `StrategySelector.chunk()` calls `StructuralChunker.chunk()` (used by `ingestion/pipeline.py`), and only `source_type == "readme"` reaches it. Nothing there relies on an empty result, so the posted plan is still true.
