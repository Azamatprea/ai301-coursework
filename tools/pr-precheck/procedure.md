# Procedure: how this skill grades a PR package

Follow these steps in order. They are for grading a pull request, not for making one. Do not fix the PR, rewrite its text, or suggest a better one; only grade what the package contains. In eval mode, the bundle is the only source; in live mode, use the sources named in `references/evidence-guide.md`.

## Read order

The plan is read before the diff and the diff before the description, because the side-by-side checks compare each later artifact to an earlier one. If you read the description first, its confident wording colors how you read the diff.

1. Live mode only: read `scope.md` first (stop and refuse per SKILL.md if the `Repo:` line is a placeholder or the PR targets another repo), then `voice-guide.md`. Eval mode: skip both.
2. Read the repo's stated asks: the repo-facts block (eval), or the PR template and `docs/CONTRIBUTING.md` (live). Write down: (a) every required PR-template section and checklist item, (b) every artifact the repo asks for (issue-closing line, changelog or whatsnew entry, translation string, test), (c) the commands the repo names for checking a change (test, lint, type check), (d) the AI-policy sentence, or "none stated". This comes first because `template-honored`, `repo-checks-shown`, and `ai-disclosed` need it and nothing later changes it.
3. Read the issue (title, body) and write one sentence: the exact behavior it reports and what was expected instead. Then read the thread (eval: thread highlights; live: the whole thread) and write down any maintainer direction (named culprit, requested approach, requested test, competing PR).
4. Read the plan (eval: the plan-context block; live: `plan.md`). Write down, each in its own line: the files it names; its in-scope list; its not-in-scope list and any "known limit" or deferral; its approach steps; its test plan with the expected-after outcome; any controls the test plan promises; and every entry under Deviations (live) or deviation note (eval). Write "missing" for any part that is absent.
5. Read the diff. List every changed file with a one-phrase summary of what changed in each. Then read each hunk once, specifically for leftovers (see step 3 of Evidence gathering). Read the commit list and note any message that is vague ("wip", "fix", "misc", "cleanups").
6. Read the candidate PR title and description last. Write down every sentence that claims what the PR contains, covers, or leaves out, and any statement about AI use.
7. Read the test evidence. Write down each command or flow shown, its output, and whether it was run before the change, after it, or both.

## Evidence gathering

Gather each pairing from the notes above. Do not go back to the raw package except where a step says so.

1. For `diff-in-plan`: put the file list from step 5 next to the plan's files, scope, and deviation notes from step 4. Next to each changed file and each hunk write "plan", "test for the plan", "repo-required (named ask)", "deviation note", or "not covered". Quote every "not covered" item. Separately record each planned deliverable (a fix, a doc, a test) and whether the diff contains it; a planned deliverable absent from the diff must be matched by an advance deferral in the plan or description, or it is "missing and undisclosed".
2. For `claims-match-diff`: take the claim sentences from step 6. For each, find the hunk or file in the diff that makes it true and write it next to the claim. Write "no matching hunk" for any claim the diff does not back (for example "docs now document the limitation" with no docs file in the diff). Write "contradicted" for any "nothing else changed" or "exactly as planned" claim when step 1 found "not covered" items. Record each stated limitation and whether the diff leaves out exactly that.
3. For `diff-reviewable`: from the hunks, list each added line that is a debug print or log, each commented-out line or block, each function or import nothing uses, each TODO or FIXME the diff adds, each hunk that only re-indents, reorders imports, or reprints identical lines, and each hunk in a file unrelated to the fix. Quote each. If there are none, write "none found". A line that is a real comment explaining the fix is not debris.
4. For `repro-rerun-decisive`: from the plan's test plan, take the repro (command, input, or flow) and the expected-after. From the evidence (step 7), find the run that matches that repro. Record: (a) whether the command, input, or flow shown is the plan's, or a neighbor of it (a control, a different input, a different path); (b) the exact observable output before and after, quoted; (c) whether it matches the expected-after; (d) whether every case or failure mode the plan named has its own run. If the evidence is a sentence with no output, write "bare claim".
5. For `repo-checks-shown`: from step 2(c), take the repo's check commands. From the evidence, find each command's result, or a named test that exercises the fix and passes. Record any command that failed, was skipped, or is absent, and whether the description or evidence gives a reason.
6. For `template-honored`: from step 2(a) and 2(b), take each required section and each requested artifact. For each, record "present with real content" (quote a few words), "empty or boilerplate", or "absent". Look in the description for sections and the closing line; look in the diff for changelog, whatsnew, translation, or test files. Record each ticked checklist box and whether the evidence supports it.
7. For `ai-disclosed`: from step 2(d), classify the policy: disclosure required, own-words required, permissive, disclosure elsewhere, or none stated. In live mode on Path Review, also treat the template's Notes for Reviewers ask for how AI was used as a requirement. Quote the AI-use statement from the description, or write "no statement". Note whether it names a tool and what the tool did.
8. For `commits-and-title`: take the title and commit messages from steps 5 and 6 and the repo's stated conventions from step 2. Record any that break them.

## Check execution

1. Run the checks in the rubric's table order. Grade each only from the notes above and quote the deciding fact in its evidence line (a file name, a quoted claim, a quoted output line, a quoted section).
2. A check is `pass` only if its pass condition holds in full from the package text. It is `fail` if any fail condition in the rubric is met. Use `unclear` only when the deciding fact is genuinely unreadable (for example a diff that is cut off mid-hunk). A PR that is merely terse is not unclear.
3. If the evidence for a check is absent from the package (no test evidence block, no description), do not look elsewhere in eval mode and do not guess in live mode. An absent test run is a `fail`, not `unclear`.
4. Grade each check on its own evidence. A package that fails `diff-reviewable` still gets graded on `repro-rerun-decisive` from its own notes, and a package that passes `claims-match-diff` can still fail `diff-in-plan`.
5. When a check passes on the description's words but you suspect the diff or the evidence does not back them (a confident "implements the plan exactly", a confident "tests pass"), the only permitted return to an earlier read is to re-read the diff's file list (step 5) or the evidence output (step 7) once for that check. Otherwise do not re-read the whole package.
6. A passing condition that feels too lenient still passes, and a failing one that feels too strict still fails. Note the tension in the summary if you want; do not change the grade.
7. Grade the PR, not the polish. Length, headings, tone, and a tidy checklist count for nothing on their own; a complete checklist over a diff that drifts from the plan still fails `diff-in-plan`.

## Verdict assembly

1. List every check's grade: pass, fail, or unclear.
2. Apply the rubric's verdict rule: `accept` only if every required check is `pass`. Any required check that is `fail`, or `unclear`, makes the verdict `reject`. The preferred check is reported and never changes the verdict.
3. In the readable summary, give one line per check with its grade and the deciding fact. For a `reject`, name the deciding check: the first failing (or unclear) required check in the rubric's table order. Quote its deciding fact in the summary and put the same fact in its JSON `evidence` field. List the other failing required checks too, each with its own quoted fact.
4. For an `accept`, quote the strongest passing evidence for `diff-in-plan` and `repro-rerun-decisive`.
5. Emit the JSON block with one entry per check in table order, preferred check included, then the verdict, and nothing after the block. In live mode, put any voice-guide rule the title or description breaks above the block, quoting the rule, and list any step in this procedure that was silent as "Procedure gap: ...".
