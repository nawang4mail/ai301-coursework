# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

Each family says where to look in an eval bundle, where to look in
live mode, and what good looks like. The `rubric.md` checks that read
each family are named in brackets.

An eval bundle has these sections, in this order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Plan context` (the repro
evidence, then the plan with its files, not-in-scope line, test plan,
and any deviation or deferral note), and `## Candidate PR` (with
`### Title`, `### Description`, `### Commits`, `### Diff`, and
`### Test evidence`). The `.json` twin holds the same content as
fields; read either.

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

[checks: `plan-fidelity`, `description-honesty`]

- Where it lives (eval): the plan's boundary is in `## Plan context`:
  the `Files:` line, the change it describes, the `Not in scope:` line,
  and any deferral or deviation note (often phrased "if X proves messy
  it will be deferred", "known limit", or "deviation"). What actually
  changed is in `### Diff`: every `--- a/` / `+++ b/` file header and
  every `@@` hunk under it. The fidelity claims are in `### Title` and
  `### Description` ("per the plan", "exactly as planned", "no changes
  beyond", "docs now cover").
- Where it lives (live): `plan.md`'s scope, files, and not-in-scope
  sections plus its deviation notes; `git diff main...HEAD` for what
  changed (`git diff --stat main...HEAD` for the file list); the draft
  PR title and description.
- What good looks like: every changed file appears in the plan's
  `Files:` line or a deviation note, every hunk does something the
  plan describes, and every change the plan promises shows up in the
  diff. A deferral counts as honest when the plan or the description
  names the dropped item and says it is not handled. Every claim the
  description makes about what changed is true of the diff.
- Drift, more than the plan: a new option, flag, or config setting; a
  rename or refactor pass; an edit to a file the plan never names; a
  reworded help string or doc line unrelated to the bug. It counts
  even when it is small or "adjacent".
- Drift, less than the plan: a promised item (a test, a docs update,
  a second fix site) missing from the diff with no note, especially
  when the description says it is there.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

[checks: `decisive-evidence`, `repo-checks-run`]

- Where it lives (eval): the proof is in `### Test evidence`, plus any
  before/after block in `### Description`. What it must prove is in
  `## Plan context`: the `Test plan:` (each repro, failure mode, and
  control it names, and the expected-after), and the repro evidence's
  broken behavior. The suite to run is named in the `Test plan:` and
  in `## Repo facts` (test commands in the contribution policy, a
  "tests added and passed" template ask).
- Where it lives (live): the student's captured terminal output and
  screenshots (pasted into the draft description or saved alongside
  `plan.md`), read against `plan.md`'s test plan; the suite commands
  in the repo's CONTRIBUTING.md and PR template.
- What good looks like: the plan's own repro command is shown re-run
  on the branch, with the output after the fix and either the output
  before it or the broken behavior it replaces, and the after matches
  the plan's stated expected-after. The run goes through the code the
  diff changed. Every other repro, failure mode, or control the test
  plan lists is at least reported as run with its result ("same-key
  redefine prints the one-time warning"), or openly deferred. The named suite is shown by command with
  its result ("`pytest tests/ -q` passes (1462 passed)").
- Not decisive: "tested locally", "works now", "verified on my
  machine", "cargo test passes" standing in for the repro; a run of
  only the control case (the path that never broke); a run of a
  different path than the one the issue reports; a test whose
  assertion would pass on the unfixed code; one of two named repros
  shown and the other never mentioned at all.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

[checks: `diff-hygiene`, `commit-messages`]

- Where it lives (eval): every `+` and `-` line in `### Diff`, and the
  messages in `### Commits`.
- Where it lives (live): `git diff main...HEAD` and
  `git log --oneline main..HEAD`.
- What good looks like: every changed line is part of the fix, its
  tests, an explanatory comment, or a docs or changelog entry the repo
  asks for. A reviewer can find the fix without stepping over anything
  else. Each commit message says what that commit changes.
- Debris tells: a debug print or log line (`eprintln!("DEBUG`,
  `console.log`, `print(`) added; a commented-out line of code or an
  earlier attempt; a function nothing calls, or one kept alive with
  `#[allow(dead_code)]` or an `_unused` name; a new `TODO` or `FIXME`;
  a hunk or block of re-indented, reformatted, or re-ordered lines
  (imports included) whose content does not change; a block of lines
  removed and re-added identically with nothing else changed in it. Commit tells: "wip", "fix", "fmt", "misc cleanups".
- Not debris: unchanged lines a diff shows as removed and re-added
  around a changed line in the same statement (the other arguments of
  a call where one argument changes); a comment that explains the fix (including one naming
  what is deliberately not handled), a new test, a required changelog
  line.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

[checks: `template-asks`, `ai-disclosure`]

- Where it lives (eval): the asks are in `## Repo facts`: the
  `pull requests:` line (template sections, checklist items, "closes
  #N", changelog or whatsnew entry) and the `contribution policy:`
  line (AI policy, AI_POLICY.md). Where the PR honors them: the
  section headings and checklist answers in `### Description`, the
  issue reference in the description or title, and any changelog or
  whatsnew file in `### Diff`.
- Where it lives (live): the repo's `.github/PULL_REQUEST_TEMPLATE.md`
  (or `.github/PULL_REQUEST_TEMPLATE/`), CONTRIBUTING.md, and any
  AI_POLICY.md, fetched with `gh` or the GitHub API; read against the
  draft description and the branch diff.
- What good looks like: each stated ask is visible in the PR. A
  required section has real content under it, a checklist item is
  answered or marked not applicable with a reason, a requested issue
  reference names the issue number, and a required changelog or
  whatsnew entry is a file in the diff (a description saying "added"
  is not enough). Where a repo states an AI policy for PRs, the
  description says AI was used and covers each part the policy asks
  for (tool or extent, human understanding, own words). Every package
  is AI-assisted work, so a missing disclosure is a miss, never "not
  applicable".
- Not a template ask: process steps a PR text cannot show (a CLA, a
  vouch flow, review turnaround). A repo with no template and no
  stated asks, or with no AI policy, passes by default.
- Thread direction (no check in `rubric.md` reads this yet): explicit
  maintainer direction lives in `## Thread highlights` (OWNER, MEMBER,
  COLLABORATOR comments). A good description engages it by name when
  the PR follows or departs from it; record it in the summary.
