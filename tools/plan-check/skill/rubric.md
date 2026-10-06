# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis | The plan's stated cause, read against every step, control run, and Expected/Actual line in the repro evidence | The stated cause explains every behavior the repro evidence records, and no repro step rules it out. Fail if a repro observation contradicts the cause (e.g. a control run where the blamed part is bypassed and the bug still appears), or if the cause is taken from a thread claim the repro evidence does not support. | required |
| scope | Where the plan says what it will change, and its not-in-scope line, read against the issue's reported bug | The plan names each file it will change and says what it won't touch, and every change listed is needed to fix the reported bug (the fix, the same fix at sites with the identical defect, and tests for it). Fail if it also includes a refactor, rename, migration, dependency bump, new feature, or redesign the fix does not require. | required |
| stranger-can-start | The plan's change list | Someone new to the repo could start the edit without asking a question: the plan names where the change goes (file, function, or a uniquely identifiable code area) and what the change is. Fail if the location is unnamed or the change is only a goal ("improve handling", "investigate and adjust"). | required |
| test | The plan's test plan, read against the repro evidence's steps and Expected/Actual lines | The test plan names at least one observable outcome that differs between the broken and fixed code, such as the repro steps re-run with the expected result stated. Fail if it only says "verify it works", "make sure nothing breaks", or "run the existing tests", or names an outcome that would be the same before and after the fix. | required |
| thread-direction | The plan comment and plan, read against the thread highlights (comments from OWNER, MEMBER, or COLLABORATOR, and any linked or open PR) | If a maintainer gave explicit direction in the thread (where the cause is, which approach to take or avoid, what to test, or a PR already in progress), the comment follows it or names it and gives a reason for diverging. Pass if the thread holds no such direction. Fail if the comment proceeds as if that direction or PR were not there. | required |
| ai-policy | The plan comment, read against the repo-facts contribution policy | If the repo's stated AI policy requires something of issue comments (disclosure of AI use, the tool and extent of use, or that a human wrote or reviewed it), the comment meets it. Pass if the repo states no AI policy, or the policy's requirements apply only to pull requests or code. | required |

## Verdict rule

Accept only if every `required` check passes. Any `required` check
graded `fail` or `unclear` makes the verdict reject: a plan that can't
be verified from the package isn't ready to build from. `preferred`
checks are reported but never change the verdict.
