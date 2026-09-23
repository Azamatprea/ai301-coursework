# Rubric: is this a good first issue?

All recency windows are measured against the bundle's capture date in eval
mode, and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: "last 5 default-branch commits" (dates and authors) and the "maintainer first-response sample" | At least one of: (a) a default-branch commit authored by a human (author name not ending in `[bot]` and not an obvious automation account like `*-bot`) dated within 90 days; (b) a bot-authored merge whose message names a PR from a human contributor (e.g. "Merge pull request #N from <human>/...") dated within 90 days; (c) a sampled issue opened within the last 90 days that got its first owner/member/collaborator comment within 14 days. Commits that are only dependency bumps or generated content, merged by bots, do not count. Fail if none of (a), (b), (c) holds. | required |
| repo-in-use | Repo facts: the repo line's "archived:" flag, "latest release", and "last 5 default-branch commits" | Fail if archived is "yes". Otherwise pass if the latest release is dated within 12 months OR any human-authored default-branch commit (same definition as maintainer-alive) is dated within 12 months. "latest release: none published" is fine if the commit condition holds. | required |
| unclaimed | Repo facts: "this issue: assignees" and "linked PRs"; plus the full Comments section (claim comments and PRs mentioned in the thread) | Fail if ANY of: (1) an assignee is listed; (2) a linked PR is "open"; (3) a comment in the thread says a PR has been opened for this issue and no later comment says it was closed or abandoned; (4) a claim comment ("I'll take this", "working on this", "can I work on this", "/assign") dated within 180 days that no maintainer (OWNER/MEMBER/COLLABORATOR) later released or reassigned. Closed or merged PRs alone are not claims. Claims older than 180 days with no follow-up PR are stale and do not fail this check. When the sidebar and the thread disagree, the thread wins. | required |
| scoped | Issue body and Comments section, including each commenter's author_association | Pass only if the issue asks for ONE bounded change that is already specified: either a bug with the wrong behavior and the expected behavior described (repro steps optional), or a feature/docs task whose required behavior is stated in the body or confirmed by a maintainer in the thread. A checklist of steps toward one deliverable (e.g. one docs addition that touches a few named files) is still one bounded change, and a short or unpolished body is not a fail. Fail if any of: it is an umbrella, tracking, or "megaissue" list of sub-items meant to be split into separate PRs, or a codebase-wide sweep ("add X across the codebase"); it is a pure usage/support question; the design is still debated and no maintainer has settled what to build (a maintainer saying "sounds reasonable" without stating the behavior is not a settled spec); a maintainer says the fix touches core internals; or the history shows 2 or more closed, unmerged PRs for it. | required |
| ai-policy | Repo facts: the "contribution policy" line (CONTRIBUTING.md, AI policy files, templates) | Fail only on an explicit ban of AI-generated or AI-assisted contributions (e.g. "we do not accept AI-generated code"). Conditions (disclose AI use, understand and test every change, human review) pass. No statement passes. An AGENTS.md file is a positive signal and passes. | required |
| newcomer-signal | Issue labels and the opener's author_association | Pass if the issue carries a "good first issue" / "help wanted" style label OR was opened by an OWNER/MEMBER/COLLABORATOR. | preferred |

## Verdict rule

Accept if and only if every required check (maintainer-alive, repo-in-use,
unclaimed, scoped, ai-policy) passes. Any required fail rejects. `unclear`
on a required check counts as fail: a first issue I cannot verify is not one
I should take. The one preferred check, newcomer-signal, never changes the
verdict; it only ranks issues that are already accepted (label or
maintainer-filed issues rank higher).
