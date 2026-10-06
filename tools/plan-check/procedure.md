# Procedure: how this skill grades a plan package

Follow these steps in order. They are for grading a plan, not for making one. Do not fix the plan, rewrite it, or suggest a better one; only grade what the package contains.

## Read order

1. Read the repo-facts block first (live mode: the repo's README, CONTRIBUTING.md, and any AI policy file). Write down three things: the AI-policy sentence (or "none stated"), any review-bandwidth or template note, and the bug-report template asks. This comes first because the ai-disclosure and repo-conventions checks need it and nothing else in the package changes it.
2. Read the issue (title, body, labels). Write down one sentence: the exact behavior the issue reports (the wrong output, error, or state), and what the reporter expected instead.
3. Read the thread highlights (live mode: the whole issue thread). For every comment by an owner or maintainer, write down any named culprit (file, function, layer), any requested approach, any requested test, and any linked pull request or branch. If there are none, write "no maintainer direction".
4. Read the repro evidence block (live mode: the student's posted repro comment). Write down every numbered step with what it showed, then separately list every control run or "ruled out" observation, each as "X behaves correctly / still fails when Y". These controls are what the cause is judged against, so read them before reading the plan's cause.
5. Read the candidate plan top to bottom. Write down: the stated cause (one sentence), the in-scope list, the not-in-scope list, the files or areas named, each approach step, the test plan, and any stated risks or unknowns. If a part is missing, write "missing" for it.
6. Read the candidate plan comment last. Write down what it commits to, whether it mentions the maintainer direction from step 3, and whether it contains any AI-assistance statement (tool and what it was used for).

## Evidence gathering

1. For cause-grounded: take the cause from step 5 and the step list and control list from step 4. Put them side by side: for each control run and each step, ask "does this observation still hold if the plan's cause is true?" Record the first observation that does not, quoted, or "none contradict".
2. For scope-bounded: take the issue's reported behavior from step 2 and number each approach step from step 5. Next to each step write "needed for the fix or its regression test" or "not needed". Record every "not needed" step quoted, and whether the plan lists it as out of scope.
3. For executable: from step 5, record the file or function names the plan gives, and whether the approach is one chosen change or a list of options. Quote any phrase that postpones a decision ("somewhere", "whichever is easier", "investigate", "maybe", "not sure").
4. For test-decisive: from the test plan, record (a) whether it re-runs the repro steps or input from step 4, and (b) the exact observable outcome it names, quoted. If the outcome is a feeling or "tests pass" with no link to the reported behavior, record "no observable outcome".
5. For claims-honest: from the plan and comment, list every sentence stated as fact (cause, scope, "will fix", "affects only X"). For each, find the repro step that shows it. Record any that nothing shows. Also list the plan's own unknowns and whether any approach step silently depends on one.
6. For thread-aware: from step 3 and the plan comment, record each maintainer direction and whether the plan follows it, engages it, or ignores it. Quote the sentence in the plan or comment that shows it.
7. For ai-disclosure: from step 1 and step 6, record the policy kind (disclosure required in comments / human own-words required / disclosure only in pull requests / permissive / none) and quote the AI-assistance statement in the comment, or write "no statement".
8. For repo-conventions: from step 1, record any template, test, CLA, or review-bandwidth note, and whether the plan contradicts it.

## Check execution

1. Execute the checks in the rubric's table order. Grade each check only from the notes recorded above, and quote the deciding fact in the check's evidence field.
2. A check is pass only if its pass condition holds in full from the package text. It is fail if the fail condition is met. Use unclear only when the package leaves the deciding fact genuinely unreadable (for example the repro block is cut off); a plan that is merely terse is not unclear.
3. If the evidence for a check is absent from the package, do not search elsewhere in eval mode. Grade what is there: an absent test plan or absent cause is a fail, not unclear.
4. When a plan passes a condition on its words but you suspect the words are not backed by the repro (for example a confident cause), re-read the control list from step 4 before grading; that is the only time a check may send you back to an earlier read. Otherwise do not re-read the whole package.
5. Do not let one check's result change another's. A plan that fails scope-bounded can still pass cause-grounded; grade each on its own evidence.
6. Grade the plan, not the polish. Length, headings, and confident tone count for nothing. A short plan that satisfies every pass condition passes.

## Verdict assembly

1. List the grade of every check as pass, fail, or unclear.
2. Apply the rubric's verdict rule: accept only if every required check is pass. Any required check that is fail, or unclear, makes the verdict reject. Preferred checks are reported but never change the verdict.
3. In the readable summary, give one line per check with its grade, and for the deciding check(s) (every required check that is not pass) quote the fact that decided it. For an accept, quote the strongest passing evidence for cause-grounded and test-decisive.
4. Emit the JSON block with one entry per check (preferred checks included, with their own grades), the verdict, and nothing after the block. In live mode, add any voice-guide rule the draft comment breaks to the summary above the block, quoting the rule. List any step in this procedure that was silent or unclear as a procedure gap in the summary.
