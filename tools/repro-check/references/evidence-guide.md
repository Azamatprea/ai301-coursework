# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: eval bundle: the repro report's "Environment" line/section, compared with the issue body's version/OS lines, the thread highlights (maintainer notes like "only on Windows", "only release builds"), and the repo-facts "latest release" and bug-report template asks. Live mode: the draft repro comment's environment block; the issue body and maintainer comments on GitHub; for Path Review, the repo README/setup docs (Python version, install command, commit SHA).
- What good looks like: the version tested and OS are named; any factor the issue or maintainers say changes the failure (driver, build profile, shell, backend, config) is named; if the tested version is not the one the issue targets, the report says so ("filed against 4.53.2, I tested 4.53.3"). A silent jump to an old release is a fail even if everything else looks fine.

## Steps

- Where it lives: eval bundle: the repro report's numbered steps, commands and code blocks (including any `cat` of input files). Live mode: the draft's steps plus the commands quoted in it; only what the draft itself contains counts, not other files on the student's machine.
- What good looks like: from a clean starting state (fresh clone / install of a named version) to the trigger, every command is shown and every input is inline or public. "In our monorepo with our config" or "run the usual setup" is not followable. The step that the issue says triggers the bug must actually appear.

## Behavior shown

- Where it lives: the output excerpts, logs, tracebacks, test output, exit codes and described screenshots in the repro report, read side by side with the issue's own output/error text and input.
- What good looks like: the artifact shows the issue's specific symptom (same panic text, same exception, same wrong value, same failing test name), produced from the issue's input or a faithful minimal version. Check the input character by character against the issue: a changed operator, an unbound variable, a different flag or syntax can turn the bug into a graceful error, which is an adjacent symptom, not the bug. A control run (same steps, the working variant) is strong extra evidence. Output that only proves the tool runs (version banner, session list) shows nothing.

## Honesty

- Where it lives: every sentence in the claim comment and the report's analysis/expected/actual that asserts something ("confirmed", "deterministic", "ten times", "root cause", "guaranteed", "also on release X"), matched to an artifact shown.
- What good looks like: claims stop where the artifacts stop. "Reproduced" only when the shown output is the issue's behavior; "cannot reproduce" with the attempt shown and what differed named is honest and passes; hypotheses are marked as hypotheses. Red flags: certainty language with no artifact, a root-cause diagnosis with no transcript, a different error narrated as "exactly the class of failure described", generalizing to versions nobody tested.

## Comms

- Where it lives: the claim comment read against the issue; both comments read against the repo-facts contribution policy (AI section) and bug-report template. Live mode: CONTRIBUTING.md / AI_POLICY.md in the repo, the issue template, and the scope.md house rules (classmate claims do not block; never piggyback).
- What good looks like: the claim names this issue's specifics (component, trigger, behavior) and a next step that is investigation, not a guaranteed fix or a date. If the repo's policy requires AI disclosure in comments or "all AI usage", the comment discloses tool and extent; if it requires human-written comments, the text is specific and in the contributor's own voice; a PR-only disclosure rule does not apply to issue comments. Boilerplate ("Hello maintainers, please assign me, I will fix this in 2 days") fails anywhere.
