# Evidence guide: where evidence lives in a plan package

In an eval bundle the sections are: Repo facts, Issue, Thread highlights, Repro evidence, Candidate plan, Candidate plan comment. In live mode, the same parts live on GitHub and in the student's drafts: the repo's README and CONTRIBUTING.md (repo facts), the issue body, the issue thread, the student's posted repro comment (repro evidence), plan.md (the plan), and comment.md (the plan comment). The package is what the drafts contain and quote, not other files in the working directory.

## Diagnosis and grounding

Where it lives: the plan's "Cause" or "Diagnosis" line in the Candidate plan. The behavior it must explain lives in the Repro evidence block (live: the posted repro comment): the numbered steps with their outputs, and above all the control runs ("same steps without X", "same input on Linux", "with the flag removed") and any line saying something was ruled out. The issue body and thread give the reported symptom but are not proof of cause.

What good looks like: the stated cause accounts for the symptom in the steps AND for every control. A cause that blames component X is contradicted if a control keeps X in the loop and the symptom disappears, or if a step shows the symptom already present before X runs. A thread member's confident diagnosis is not evidence; only the shown output is. A cause labelled as a guess is fine if the plan's first step confirms it.

## Scope

Where it lives: the Candidate plan's "Scope" / "In scope" / "Not in scope" lines and the numbered approach steps. The single behavior to compare against is the one reported in the Issue and isolated by the Repro evidence.

What good looks like: one bounded change at the place the repro isolates, plus its regression test, with a not-in-scope line that leaves the neighbors alone. Honest deferral ("I will not do X because Y") is good scope, even when it narrows the fix. A drive-by rewrite looks like extra approach steps nobody asked for: a refactor, migration, new setting, dependency upgrade, UI rework, retry framework, CI matrix, or a second symptom bundled in "while in the area".

## Executability

Where it lives: the Candidate plan's "Approach" steps, its named files, functions, or areas, and the order of work.

What good looks like: a stranger could start step 1 today. Files or functions are named (or the single place the repro points at), the approach is one chosen change, and the steps are in a workable order. Not executable: "investigate", "somewhere in the input stack", "upstream or vendored, whichever is easier", "maybe also check other linters", or any step whose real decision is left for build time.

## Test plan

Where it lives: the Candidate plan's "Test" / "Test plan" section, read against the numbered steps and artifacts in the Repro evidence.

What good looks like: it re-runs the repro (same steps, command, or input) against the change and names the observable that must flip: the exact output, exit code, value, or rendered state that the repro showed wrong. A decisive test plan names a before and an after. Not decisive: "run the full test suite", "should feel fast", "nothing else should feel broken", or any test with no outcome tied to the reported behavior.

## Honesty

Where it lives: the Candidate plan's "Risks", "Unknowns", or "Open questions" lines (live: and its "## Deviations" heading after the build), and every sentence in the plan or comment stated as fact.

What good looks like: each stated fact has a repro step behind it, and each open question that a step depends on is called an open question ("I have not measured X; if it shows up I will do Y"). False confidence looks like "this will fix it", "the root cause is X", or "only affects Y" with nothing in the Repro evidence behind it, or an approach step that silently relies on an unknown the plan never names. A short plan with no risks section is fine when nothing unknown is at stake. An honest mid-build deviation is recorded under "## Deviations" and counts as honest work.

## Comms

Where it lives: the Candidate plan comment, read against (a) the Thread highlights (live: the whole issue thread), looking for owner or maintainer direction, named culprits, requested tests, and linked prior-art PRs, and (b) the Repo facts block's contribution policy, bug-report template asks, and review notes (live: CONTRIBUTING.md and any AI policy file).

What good looks like: thread-aware means the comment shows it read the thread: it follows the maintainer's direction, engages a competing PR or branch, or says in its own words why it departs. Boilerplate that would fit any issue is not thread-aware. For AI policy: if the policy requires disclosure in comments, the comment names the tool and what it was used for in one line; if the policy requires human own-words comments, the comment reads as specific, personal words about this issue; if disclosure is only required in pull requests, or the policy is permissive or absent, nothing is required. Every package is treated as AI-assisted work.
