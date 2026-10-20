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

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/208

**Branch**

fix/56-structural-chunker-no-headings

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One complete scored run: agreement 18/20, bar 18/20: PASS (the run committed as eval-run.txt, Oct 14, Sonnet, 20 packages). The only run before it was an --only smoke on the four calibration packages: 4/4 matched gold. Calibration packages are never scored, so that smoke wrote no eval-run.txt. No rubric or skill edits happened between the smoke and the full run, so the 18/20 in eval-run.txt is the last and only scored full run.

**Package analysis**

pkg-05 (nushell/nushell#18848, keybinding merge-by-name). My rubric: reject. Gold label: accept (clear-accept). The deciding check was repro-rerun-decisive. The plan's test plan names two repro cases: re-run the issue's three-line script and expect both atuin rows, and re-run with two same-name same-key bindings and expect one row plus a warning. The package's evidence shows the first case as before/after tables and names cargo test, but the second case appears only as a prose assertion ("same-key redefine prints the one-time warning"), with its real proof living in the diff's new unit test rather than in a shown re-run. My check fails a plan with two repro cases when only one is re-run, so it graded fail and the verdict rejected. The gold label reads the package as decisively evidenced because the changed behavior is observable in the shown tables and the second case is covered by a committed test. My check reads the letter of the plan's test plan; the gold reads the outcome. Same shape on pkg-13, which is why clear-accept finished 5/7 while every other category went 100%.

**Check rationale**

Quoted from tools/pr-precheck/rubric.md exactly as uploaded:

"repro-rerun-decisive | The test evidence read against the plan's test plan and the reproduction it builds on (the issue's repro command, input, or flow). See evidence-guide "Test evidence". | Pass if the evidence re-runs the plan's repro (its command, input, or flow, not a neighbor of it) on the change, names the observable that changed (an exact output, value, exit code, or screen state), and the observable matches the plan's expected-after. Controls the plan names must be re-run too if the plan promised them. Fail if the evidence is only a bare claim ("tested locally", "works on my machine", "tests pass") with no output or named test, if it exercises the path the fix did not change (a different input, a control only), if the plan named two failure modes or two repro cases and only one is re-run, or if the shown output does not match the expected-after. | required"

Why it reads this way: the not-tested family it exists to catch is defined by gold labels as "no observable before/after, 'tests pass' with nothing named, evidence exercising the unchanged path, or repo checks never run", so early drafts that said "evidence shows testing" were too vague to grade consistently. The revision names the three things evidence must actually carry (the plan's own repro re-run, a named observable, a match against the plan's expected-after) and adds the two-cases rule after the unit-2 lesson that a plan naming two failure modes is only half-proven by re-running one. The cost of that strictness is exactly pkg-05 and pkg-13, accepted here as the trade-off.

**Trade-offs**

repro-rerun-decisive trades recall for safety. It will never pass a bare "tests pass" claim, and the price is that it holds clear-accept packages whose evidence proves the fix through a committed test instead of a shown second re-run: pkg-05 and pkg-13 in the committed run. I kept it required and literal on purpose; loosening it toward "some evidence exists" is what would let the not-tested family through, which the gold labels punish hardest. Nothing changed elsewhere between the smoke and the committed run: the smoke (4/4 on calib packages) and the full run used the same rubric and skill files with no edits in between, so the two clear-accept misses are the check's stated strictness, not drift. If I revise it later, the canaries I would add to --only are pkg-02 (the clear-accept that currently agrees) and pkg-04 (the not-tested reject), so a loosened check shows up as a pkg-04 flip before it costs a confirming full run.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
