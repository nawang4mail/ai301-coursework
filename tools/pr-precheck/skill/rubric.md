# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks, read against the plan's scope (files it will change, what it will change, its not-in-scope line) and its deviation notes | Every hunk does something the plan or a deviation note names, and every change the plan promises is in the diff. Pass if a planned item is missing but deferred openly in a plan deviation note or the description. Fail if the diff adds anything outside the plan with no deviation note (a new option, flag, or setting; a rename or refactor pass; an edit to a file the plan never names; reworded unrelated text), or silently leaves out a promised change. "Adjacent" or "small" extra work still fails if it is unannounced. | required |
| description-honesty | Every claim in the PR title and description (what changed, "exactly as planned", "no changes beyond X", "docs now cover Y", stated deferrals), read against the diff | Each claim about what the PR changes or does not change is true of the diff. Fail if the description claims something the diff does not contain, or denies something the diff does contain. Grade claims against the diff, never against how confident or polished they sound. | required |
| decisive-evidence | The test-evidence section (and any before/after in the description), read against the plan's test plan and the issue's repro steps | The evidence shows the plan's repro re-run on the branch with an observable result that differs from before and matches the plan's expected-after (a before/after pair, or an after shown next to the stated broken behavior). The run must exercise the changed path. The issue's reported repro must be shown with observable output. Every other item the test plan names (a second failure mode, a control, a secondary case) must at least be reported as run with its result stated, or openly deferred; a stated result counts for these without pasted output. Fail on "tested locally", "works now", "verified on my machine", a control-only or unchanged-path run, the issue's repro never shown, or a test-plan item the evidence never mentions at all. | required |
| repo-checks-run | The test-evidence section, read against the test command or suite named in the plan's test plan and the repo-facts block (contributing-guide test commands, template "tests passed" asks) | If the plan or the repo names a test suite or check command, the evidence shows it run on the branch: the command named and its result stated. Pass if neither the plan nor the repo names any suite or check. Fail if a named suite is never mentioned as run. A bare "tests pass" with no command named fails. | required |
| diff-hygiene | Every hunk in the diff | The diff contains only the change and what it needs (the fix, its tests, docs or changelog the repo asks for, and explanatory comments). Fail if any hunk carries debris: debug prints or logging left in, commented-out code or earlier attempts, dead functions or `allow(dead_code)`-style suppressions, leftover TODOs, or churn: a hunk or block whose only change is formatting, re-indentation, import reordering, or lines removed and re-added identically. Lines a diff re-shows as removed and re-added inside a statement whose content the fix changes (for example the other arguments of a call where one argument changes) are part of the fix, not churn. | required |
| template-asks | The description, commits, and diff, read against the repo-facts block's PR-template and contribution-docs asks | Every concrete ask the repo states for a PR is visibly met or explicitly marked not applicable with a reason: required sections present and filled, an issue reference ("closes/fixes #N") where asked, checklist items answered, and any changelog, whatsnew, or release-notes entry the repo requires present in the diff. Pass if the repo states no PR asks. Asks that cannot be shown inside a PR (a CLA, a vouch flow, the review process) are out of this check. | required |
| ai-disclosure | The description, read against the repo-facts block's contribution policy (AI policy, AI_POLICY.md) | Treat every package as AI-assisted work. If the repo's stated AI policy asks something of a PR (disclose AI use, name the tool or extent, confirm a human understands the work, write in own words), the description meets every part of it. Pass if the repo states no AI policy, or its policy places no requirement on PRs. | required |
| commit-messages | The commit list | Each commit message says what that commit changes. Fail on placeholder messages such as "wip", "fix", "misc cleanups", or "fmt". | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only if every `required` check passes. Any `required` check
graded `fail` or `unclear` makes the verdict reject: a PR whose
fidelity, evidence, or compliance cannot be verified from the package
is not ready to submit. `preferred` checks are graded and reported
but never change the verdict.

An honest shortfall is not a failure. A PR that delivers less than
everything the issue asks can be accepted when the gap is openly
deferred in the plan's deviation notes or the description, and the
work it does deliver passes every `required` check. Reject the silent
gap, not the disclosed one.
