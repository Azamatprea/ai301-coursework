# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

Azamatprea

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56#issuecomment-6025972182

Plan for the StructuralChunker no-headings drop, built from my repro above (`chunk()` printed 0 for the issue's text, 1 with one leading `# Title`, and the test fails with `assert 0 >= 1` under `--runxfail`).

My reading of `_extract_sections()`: content lines are only kept `if heading_stack or current_section_lines`, and the final save needs `heading_stack` too, so a document with no headings ends with no sections. That is from reading the code, not instrumented yet; my first step is to print `sections` for the issue's text to confirm it.

The change I'd propose: collect every content line, and save the final section even with no headings (path `[]`, level 0), plus removing the strict `xfail` on `test_document_with_no_headings`. Only `structural_chunker.py` and that test file. I would leave out text before the first heading in documents that do have headings (dropped today, separate symptom) and anything in `strategy_selector.py`.

Test plan: re-run my repro on the branch and expect the snippet to print 1 (was 0), the heading control to still print 1, and the test to pass without `--runxfail`.

One open question: I haven't checked whether any caller relies on the empty result for headingless text, so I'll grep the call sites before opening a PR.

(I used Claude to help draft this comment and check my plan against my rubric; the repro and the code reading are mine.)

---

## Your branch

**Branch**

fix/56-structural-chunker-no-headings

**Evidence**

Before (my unit 2 repro, fork at f89c06f, local run, from my repro comment):
```
python -c "from ingestion.chunking.structural_chunker import StructuralChunker
c = StructuralChunker()
print(len(c.chunk('This is a plain document with no headings at all. ' * 20, {})))"
0
python -m pytest tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings --runxfail
E       assert 0 >= 1
E        +  where 0 = len([])
1 failed in 0.06s
```

Before (CI on the fork, `main` at f89c06f, workflow_dispatch run https://github.com/Azamatprea/pathreview-ai301-fa26-s1/actions/runs/37535307892, `pytest tests/unit -v --tb=short`, Python 3.11.16, ubuntu):
```
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings XFAIL [ 90%]
================= 375 passed, 53 xfailed, 3 warnings in 7.81s ==================
```

After (CI on the fork, branch `fix/56-structural-chunker-no-headings` at 0572545, run https://github.com/Azamatprea/pathreview-ai301-fa26-s1/actions/runs/37535266091, same command, Python 3.11.17, ubuntu):
```
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings PASSED [ 90%]
================= 376 passed, 52 xfailed, 3 warnings in 11.14s =================
```
The same run's lint (ruff and black --check) and typecheck (mypy) jobs also completed successfully. The test that was XFAIL on main is PASSED on the branch and nothing else changed status (53 xfailed to 52 xfailed, 375 passed to 376 passed).

## Eval iterations

**Run history**

I have not run the course harness (`run_eval.py`) yet, so there is no real harness score to report and `eval-run.txt` in this folder is still the template. The only runs so far were dry runs where I had graders follow my skill files on the 20 scored packages and compared the verdicts to `gold-labels.json` myself: 19/20 on the first pass (only pkg-14 disagreed), then 6/6 on a canary re-grade (pkg-14, pkg-10, pkg-17, pkg-18, pkg-12, pkg-13) after I loosened one check. These dry runs are not harness output.

**Package analysis**

pkg-14 (zellij-org/zellij#5174). On the first dry run my rubric decided reject, failing only `executable`; the gold label is accept. The plan names the `zellij-server` and `zellij-client` areas and says "exact functions to be pinned in the PR after tracing the query issuance with debug logs". My first `executable` wording required naming the file(s) or function(s), so a plan that named crates and deferred exact functions failed it. The gold label reads it as ready: the approach is already chosen (one reattach-handshake fix), the diagnosis fits the repro, the Windows variant is honestly deferred, and the repro loop is a decisive test. I changed the check so naming the specific module or component is enough when the approach is already chosen; on the re-grade it moved to accept.

**Check rationale**

"Pass if a stranger could start the first step today without asking the author anything: the plan names the file(s), function(s), or the specific module or component the repro evidence points to, and the chosen approach is one concrete change, not a list of options. Naming a component and saying the exact function will be pinned while making that change is fine when the approach itself is already chosen. Fail if the plan defers a real decision to build time ("somewhere", "upstream or vendored, whichever is easier", "investigate X", "maybe also check Y", an unchosen layer or an unchosen approach) or names no files, components, or areas at all."

This is the `executable` check. It reads this way because the first version asked for exact file or function names and wrongly failed pkg-14. I rejected "any plan that names a function passes", because that would let pkg-18 through (its fix is "upstream or vendored, whichever is easier"). The line I kept is between a pinned approach with a location still to be found (fine) and an unchosen approach or layer (fail).

**Trade-offs**

The loosened `executable` check gives up strictness on location: a plan that names only a component now passes this check, so a plan that is vague about where in the component it will edit could slip through here. What stops that is that the approach has to be one chosen change. I checked it with canaries on the dry run: pkg-10, pkg-17, pkg-18 and pkg-12 (all gold reject, all still reject) and pkg-13 (gold accept, still accept). The cases it will still miss are plans that choose an approach but pick the wrong component, which is `cause-grounded`'s job rather than this check's. I still need to confirm this on a real full harness run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in `tools/plan-check/`.
