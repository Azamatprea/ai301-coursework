---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?**

A PR package is a candidate pull request read against the plan it claims to implement and the issue that plan belongs to. It contains the candidate's title, description, commit list, diff, and test evidence. Those are read against three things: the plan (its scope, files, test plan, and any deviation notes), the issue (title, body, thread), and the repo's own stated asks (PR template, contributing guide, AI policy).

Never grade more than one package per run. Never answer a different question (is the plan good, is the bug real, is the code elegant). Never answer from gut feel: execute `procedure.md`, which applies `rubric.md` to evidence located with `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes. If the user's request names a bundle file or a bundle id (such as `pkg-07`), or the prompt itself contains the whole bundle text, run in eval mode. Otherwise run in live mode.

**Live mode**: the student's own submission, checked before it goes out. Read these inputs:

1. `plan.md` in the top folder of the working copy, including its `## Deviations` section. This is the plan the PR claims to implement.
2. The branch's diff: run `git diff main...HEAD` (three dots) from the working copy. It is every committed change the branch makes compared with `main`. Also run `git log main..HEAD --oneline` for the commit list and `git diff main...HEAD --stat` for the changed-file list. Uncommitted changes are not part of the package; if `git status` shows uncommitted edits to tracked files, say so in the summary.
3. `pr_draft.md`: the PR title is its first line, the description is everything under it.
4. `test_evidence.md`: the pasted output of the repo's checks and the before/after repro.
5. The issue: its URL comes from the request. Read the issue body and its whole thread (via `gh issue view <number> --repo <repo> --comments`, the GitHub API, or the web).
6. The repo's stated asks: the PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, read from the working copy or the repo.

A house-chain student (one working the house issue, not their own) reads the house plan and the house repro pack instead of their own `plan.md` and repro; the same checks grade the same things there.

Read the package the way a maintainer will read the pull request: the package is what these inputs contain, not other files in the working directory. Never edit the student's files and never fix the PR; only grade it.

**Eval mode**: a package bundle is the whole world. Every fact comes from the bundle text (repo facts, issue, thread highlights, plan context, candidate title, description, commits, diff, test evidence). Fetch nothing, run nothing, and read nothing else. Eval mode always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, before opening any input above. It names the one repo where the pull request must live and the house rules of that environment (PR from your own fork's branch, one PR per issue per student, the template is always used, a classmate's PR on the same issue does not block yours, credit attaches to opening the PR).

- Apply those house rules when you read the evidence. They change what counts as compliant, for example: the template must be filled in with real content, and a branch must be on the student's fork.
- Refuse to grade a package whose issue or PR targets any repo other than the one on the `Repo:` line. Say which repo it targeted and stop, with no JSON verdict.
- If the `Repo:` line still carries a placeholder (angle brackets such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading. Tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope.
- In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after reading the scope, read `voice-guide.md`: the student's own rules for how they write upstream. Hold the outgoing PR text (the title and the description from `pr_draft.md`, and nothing else) against those rules. For every rule the draft breaks, report it in the readable summary, quoting the rule's heading and the offending words from the draft.

The voice guide never changes the verdict on its own. Only a check in `rubric.md` can change the verdict, and none of the checks reads the voice guide. In eval mode, ignore `voice-guide.md` entirely: voice is personal and carries no gold labels, and the universal communication-quality checks live in the rubric.

## Component reads

The skill has three parts that connect in a fixed way:

- `rubric.md` defines the checks (name, evidence, pass condition, weight) and the verdict rule. It decides what to grade and what counts as a pass.
- `references/evidence-guide.md` is the map. For each of the four evidence families (plan fidelity, test evidence, diff quality, standards and comms) it says where that evidence lives in an eval bundle and, live, in the working copy and on GitHub, and what good looks like.
- `procedure.md` is how to work. Execute it as written, in order: read order, evidence gathering, check execution, verdict assembly.

Read all three before grading. If the procedure is silent on a step you need, do not invent one. Report the gap in the readable summary (a line starting "Procedure gap:") and grade with the rubric's stated condition. If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in (only the template comments), stop and say so: this tool cannot grade without a rubric and a procedure, by design. Do not make up checks at runtime.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations", and no score. Put reservations in a check's evidence line, not in the verdict.

Before the JSON block, show a short readable summary: one line per check with its grade and the deciding fact, then (live mode) any voice-guide rules the draft breaks and any procedure gaps. Then end the reply with the fenced JSON block below, valid JSON, as the last thing in the reply, with nothing after it. Include one entry per check in the rubric's table, preferred checks too. The `item` is the PR URL or, if there is none yet, the issue URL (live), or the bundle id (eval). Do not alter, extend, or reorder the schema.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be ready and a beautiful, confident one can be hiding drift. Read the diff, the evidence, and the description themselves against the plan, the issue, and the repo's stated asks, never their formatting, length, or tone. A description that says "implements the plan exactly" is a claim to check against the diff, not a reason to pass.
- **The rubric decides, not you.** If a check passes by its stated condition but feels wrong, it still passes; note the tension in the summary if you want. The fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as written and report its gaps instead of papering over them.
- **An honest shortfall can be ready.** A PR that states what it deliberately does not do, in the plan or the description, and whose diff matches that statement, is not drifting. Less than everything is not a failure; unannounced less, or unannounced more, is.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict rule directs. If the rule is silent, an unverifiable claim is a failing one: a PR you cannot verify from the package is not ready to submit. Use `unclear` only when the deciding fact is genuinely unreadable (for example a cut-off diff), never for a package that is merely terse.
