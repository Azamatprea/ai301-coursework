# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56

**Verdict output**

Live-mode run with the final rubric (the same `rubric.md` uploaded to `tools/issue-select/` and fingerprinted in `eval-run.txt`), command: `claude -p "issue-select: grade this candidate first issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56"`

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
All evidence is in. Summary per check:

- **maintainer-alive: pass.** Human-authored default-branch commits by Aburke225 on 2026-09-16, seven days before today. Aburke225 (COLLABORATOR) also replied on issues #52 and #43 within six days of their opening.
- **repo-in-use: pass.** Not archived. No releases published, but the human commit condition holds (latest 2026-09-16).
- **unclaimed: pass.** No assignees, zero comments, no linked PRs. The only activity is two "referenced" events from commits in a student's fork (vchlinh/ai301-coursework), which are not PRs on this repo, and the Path Review house rule ignores classmate claims anyway.
- **scoped: pass.** Type A bug filed by a COLLABORATOR: `StructuralChunker.chunk()` returns an empty list for heading-less documents. Body gives a repro snippet and names the failing test `test_document_with_no_headings`. One bounded fix, no design debate, no abandoned PRs.
- **ai-policy: pass.** No AI statement in docs/CONTRIBUTING.md, the PR template, or the README. Silence passes.
- **newcomer-signal (preferred): pass.** Labeled `good first issue`, `bug`, `ingestion`, `tier-1`, and opened by a COLLABORATOR.

**Verdict: accept.** Fit is strong too: it is a Python bug in the ingestion pipeline (chunking), it already has a strict-xfail failing test to drive the fix, and it is tier-1 rather than a large feature. CONTRIBUTING.md even uses issue #56 as its worked example, so remember to delete the `@pytest.mark.xfail` marker on that test as part of the PR.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/56",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass",
     "evidence": "Human commits by Aburke225 on 2026-09-16 (3 that day), within 90 days of 2026-09-23; Aburke225 [COLLABORATOR] commented on #52/#43 on 2026-09-16, 6 days after they opened."},
    {"name": "repo-in-use", "grade": "pass",
     "evidence": "archived: false; latest release: none published; human-authored default-branch commit dated 2026-09-16, within 12 months."},
    {"name": "unclaimed", "grade": "pass",
     "evidence": "assignees: []; comments: 0; no linked or mentioned PRs in this repo (only 2 'referenced' events from commits in fork vchlinh/ai301-coursework on 2026-09-22)."},
    {"name": "scoped", "grade": "pass",
     "evidence": "BUG by COLLABORATOR Aburke225: 'StructuralChunker.chunk() returns an empty list for any document without markdown headings'; repro snippet and failing test test_document_with_no_headings named; single fix."},
    {"name": "ai-policy", "grade": "pass",
     "evidence": "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md and README.md contain no statement on AI-generated or AI-assisted contributions."},
    {"name": "newcomer-signal", "grade": "pass",
     "evidence": "Labels: bug, good first issue, ingestion, tier-1; opener author_association: COLLABORATOR."}
  ],
  "verdict": "accept"
}
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run, first rubric: every item errored before grading (`20 item(s) errored; fix and re-run.` / `agreement: 0/0 scored items`). Cause: the Claude Code CLI was not logged in yet; no rubric was graded.
2. Smoke run, `--only issue-01`, first rubric: `agreement: 1/1 scored items`.
3. Full run, first rubric: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. Misses: issue-04 and issue-19 (`failed: scoped`), issue-20 (`graded accept`).
4. Partial run after rewriting `scoped`, `--only issue-04,issue-19,issue-20,issue-01,issue-09,issue-14,issue-16`: `agreement: 7/7 scored items`.
5. Full run, final rubric (the committed `eval-run.txt`): `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

**issue-20** (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar"). Gold label: `reject`. My rubric's decision: `accept` in run 3, `reject` in run 5.

Why run 3 accepted it: my first `scoped` check only asked whether the required behavior was stated, and this body states it very cleanly. The grader quoted it back as its pass evidence: "Body states required behavior: 'toolbar shape... place, resize, and move it like other elements... export correctly' with explicit v1 scope boundary". Every other check passed too (excalidraw is very alive, no assignee, no PRs, no AI ban), so nothing stopped it.

Why that was wrong: a polished spec is not the same as an agreed one. The issue was "opened by cursor[bot] (NONE)", has "Comments (0 total)", "labels: none", and says "Logo asset TBD." Nobody on the excalidraw team has said they want a company-logo tool at all, so the real decision (should this exist?) is still unmade, and a newcomer's PR would be answering a product question nobody asked them. After the rewrite, run 5's grader failed it on exactly that: "New feature request opened by cursor[bot] (NONE association) with 0 comments — no maintainer acceptance of the request or of what to build, and body states 'Logo asset TBD'."

**Check rationale**

Check: `scoped` (weight: `required`). Pass condition, quoted as currently written in `tools/issue-select/rubric.md`:

> Classify the issue, then apply that type's rule. (A) BUG (something that worked or should work is broken, including a regression from an earlier version): pass if the broken behavior is described. If the opener is an OWNER/MEMBER/COLLABORATOR, a terse body, a list of example instances of the same defect ("including X, Y, Z, etc."), or a maintainer's list of likely causes still describes ONE fix; "additional suggestions" or nice-to-have ideas are optional extras, not required scope. (B) DOCS task: pass if the body names what to change and where (a checklist of steps toward one docs deliverable is still one change). (C) NEW FEATURE (a capability the project never had): pass only if the opener is an OWNER/MEMBER/COLLABORATOR, or a maintainer in the thread has accepted the request and stated what to build. A feature request from a NONE/CONTRIBUTOR/bot opener with no maintainer reply fails, however detailed its body, because the product decision is unmade; open placeholders like "TBD" also fail. Any type fails if: it is an umbrella, tracking, or "megaissue" list meant to be split into separate PRs, or a codebase-wide sweep ("add X across the codebase"); it is a pure usage/support question; the thread shows an unresolved design debate with no maintainer decision (a maintainer saying "sounds reasonable" without stating the behavior is not a decision); a maintainer says the fix touches core internals; or the history shows 2 or more closed, unmerged PRs for it.

Why it has this form: in run 3 one sentence ("pass only if the issue asks for ONE bounded change that is already specified") was failing issues for the wrong reasons in both directions. It read the wording of the body instead of the kind of work being asked for. On issue-04 it treated "Including remove identity, fuse spiders, remove self loops, etc." as an open-ended list, and on issue-19 it treated a maintainer's "two potential causes" plus "Additional suggestions" as an unsettled multi-item list. Both are really one bug diagnosed by a COLLABORATOR. Meanwhile it passed issue-20 because the feature was described well. Splitting by type fixes both: for a bug the question is "is the breakage described?" (and a maintainer's shorthand counts), for a feature the question is "has a maintainer agreed to build this?". The shared fail list at the end keeps the scope traps (issue-05's codebase-wide sweep, issue-10's megaissue, issue-15's two closed PRs) failing no matter which type they fall into.

**Trade-offs**

The feature rule (C) gives up good feature ideas that come from outside contributors and simply haven't been answered yet: if a user files a clear, useful request and no maintainer has replied, my rubric rejects it even if the team would happily merge it. I accept that miss, because for a first contribution an unanswered feature request is exactly where PRs sit unreviewed. It also leans on the bug/feature label the grader picks, so a borderline "regression or new feature?" issue (like calib-01's p5.js `min()` request) depends on the grader calling it a regression.

Canary check: because loosening the bug rule and tightening the feature rule could flip accepts, I re-ran `--only` on the three misses plus the four accepts most exposed to the change (issue-01 docs by a CONTRIBUTOR, issue-09 feature by a MEMBER, issue-14 docs by a CONTRIBUTOR, issue-16 bug by a CONTRIBUTOR) before spending on a full run. All seven matched gold (`agreement: 7/7 scored items`), and the confirming full run changed nothing else (`categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`).

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit and time.** I build and deploy LLM/RAG systems at work, so a bug in the ingestion chunker is right in my lane: it's Python, it's the part of a RAG pipeline I actually care about (a document that silently never gets indexed is a real failure I've seen), and there's already a failing test (`test_document_with_no_headings`) to drive the fix. It's tier-1, so I expect a few hours, which fits around my job and classes.
2. **What the verdict got right, and what I weighed on top.** The skill correctly saw that the repo is active (human commits on 2026-09-16), that nobody has an open PR on it, and that it's one bounded bug filed by a collaborator. It also correctly rejected #60 because a classmate already has PR #74 open. What the rubric can't weigh: whether the fix is "chunk the whole doc as one block" or "fall back to another chunker", which is a small design choice I'll need to make and justify, and that I'd rather learn this codebase through the ingestion path I'll keep touching in later units than through a one-off endpoint.
3. **Difficulty claiming it.** Low on paper: no assignee, no comments, no PRs. But it's a well-known issue (the contributing guide uses #56 as its worked example, and the skill spotted commits referencing it in a classmate's fork), so I expect other students to pick it too. Under the Path Review house rule that doesn't block me, but I'll claim it early in Unit 2 and make sure my fix and tests stand on their own.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
