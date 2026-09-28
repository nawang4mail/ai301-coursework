# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->
## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | Repro report (Environment section) | The report lists the operating system, language, and key dependency versions. | required |
| Steps followable | Repro report (Steps section) | The steps are a continuous, sequential list of copy-pasteable terminal commands or exact UI click-paths starting from a fresh state. | required |
| Behavior proven | Repro report (Observed behavior) | The report includes raw terminal output, stack traces, or screenshots that match the original issue's description. If the outcome is an honest connot-reproduce(see Honest outcome), artifacts from the real attempt - showing what was tried and what differed from the issue's trigger conditions- satisfy this check instead.| required |
| Honest outcome | Issue context, Repro report | The logs/ screenshots actually match the steps taken. If "cannot reproduce", the author provided the exact steps they tried that resulted in success. | required |
| AI Disclosure Compliant | repo-facts, claim comment, repro report | If the repository's contribution requires AI disclosure, the user must have disclosed it in their comments. (Passes automatically if the policy mentions nothing about AI). | required |
| Professional Claim | Claim comment | The comment explicitly names the issue and promises an investigation or attempt, without guaranteeing a fix or a timeline. | required |

## Verdict rule

Accept only if every required check passes. Reject if any required check
fails. `unclear` on a required check counts as a fail (a first issue you
cannot verify is not a first issue you should take); `unclear` on a
preferred check is just noted, since preferred checks never change the
verdict. Preferred checks only rank the issues that are accepted.