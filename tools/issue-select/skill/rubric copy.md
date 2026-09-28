# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | the "last 5 default-branch commits" list in the repo-facts block | at least one of the 5 commits (human-authored, or a bot merging a human's pull request) is dated within 90 days of the bundle's capture date | required |
| Repo in active use | "last push to any branch" and the "archived:" flag in the repo-facts block | the repo is not archived AND the last push to any branch is within 90 days of the capture date | required |
| Scope fits a newcomer | the issue body, the comment thread, and the "linked PRs" line | the issue is one bounded deliverable a single contributor could finish in one PR — a checklist of related edits toward one feature/page still counts as bounded, but an umbrella issue that explicitly invites separate contributors to split off independent sub-items (e.g. a list of other issue/PR links to divide up) does not. Fails if: it is a pure usage/support question; a maintainer states in the thread that it touches core internals or that the design is still unsettled; the issue's own scope is left open by its author (marked TBD / "possibly" / "likely" with no maintainer confirmation of what's wanted); or it carries a history of multiple closed, unmerged linked PRs that already attempted it | required |
| Unclaimed | the "assignees:" and "linked PRs:" line, and claim comments in the thread | no assignee, AND no currently open linked PR working the issue (a closed/unmerged linked PR, or an old claim comment nobody followed through on, does not count against it) | required |
| Allowed under contribution policy | the "contribution policy" line in the repo-facts block | the policy does not state an outright ban on AI-generated contributions (disclosure, personal-understanding, testing, or human-review conditions still pass; a policy that says nothing about AI passes) | required |
| Recently released | "latest release" in the repo-facts block | the latest release is within 180 days of the capture date | preferred |

## Verdict rule

Accept only if every required check passes. Reject if any required check
fails. `unclear` on a required check counts as a fail (a first issue you
cannot verify is not a first issue you should take); `unclear` on a
preferred check is just noted, since preferred checks never change the
verdict. Preferred checks only rank the issues that are accepted.