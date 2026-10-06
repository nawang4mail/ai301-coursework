# Evidence guide: where evidence lives in a plan package

Each section says where to look in an eval bundle, where to look in
live mode, and what good looks like. The rubric check that reads each
family is named in brackets.

## Diagnosis and grounding

[check: `diagnosis`]

- Where it lives (eval): the candidate plan's cause statement, usually
  a `Cause:` line or a `Diagnosis` section. The behavior it must
  explain is in the repro-evidence block's steps and its `Expected:` /
  `Actual:` lines.
- Where it lives (live): the cause section of the student's `plan.md`,
  read against the student's posted repro comment on the issue.
- What good looks like: the plan names the mechanism that produces the
  bug (what goes wrong, and where), and that cause explains every repro
  step. A control run is the strongest evidence: if the repro shows the
  bug still happening with the blamed part bypassed, the cause is
  wrong, however confident the plan sounds. A cause copied from a
  thread comment counts only if the repro backs it.

## Scope

[check: `scope`]

- Where it lives (eval): the candidate plan's change list and any
  `In:` / `Out:` or `In scope` / `Not in scope` lines.
- Where it lives (live): the change and scope sections of `plan.md`.
- What good looks like: every file the change touches is named by
  path, there is an explicit line saying what the plan will not touch,
  and every change is needed for the reported bug. A fix bundled with
  a refactor, migration, or redesign is not bounded, even if the
  plan calls the extra work "cleanup".

## Executability

[check: `stranger-can-start`]

- Where it lives (eval): the candidate plan's change list or `Change:`
  line, and the files, functions, or code areas it names.
- Where it lives (live): the change section of `plan.md`.
- What good looks like: a stranger could open the named file and
  start the edit without asking the author what to do.

## Test plan

[check: `test`]

- Where it lives (eval): the candidate plan's `Test:` line or `Test
  plan` section, read against the repro-evidence block's steps.
- Where it lives (live): the test section of `plan.md`.
- What good looks like: the test names an observable result that
  differs between broken and fixed code, usually the repro steps
  re-run with the expected result stated. A manual re-run of the
  repro counts; "verify it works" does not.

## Honesty

[no check in `rubric.md` reads this family yet]

- Where it lives (eval): any risks, unknowns, or "to check" lines in
  the candidate plan, and the claims in the candidate plan comment.
- Where it lives (live): the risks or unknowns section of `plan.md`,
  and its `Deviations` section after the build.
- What good looks like: anything the repro evidence did not establish
  is named as an unknown, not stated as fact.

## Comms

[checks: `thread-direction`, `ai-policy`]

- Where it lives (eval): the candidate plan comment, read against the
  thread highlights (especially OWNER, MEMBER, or COLLABORATOR
  comments and any linked PR) and the repo-facts block's contribution
  policy, including any AI-use policy.
- Where it lives (live): the student's draft plan comment, read
  against the live issue thread and the repo's CONTRIBUTING.md and any
  AI policy file.
- What good looks like: the comment responds to what maintainers have
  already said in the thread, and meets any AI-use rule the repo
  states for comments. An AI policy that only covers pull requests or
  code asks nothing of an issue comment.
