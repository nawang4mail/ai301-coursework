# Procedure: how this skill grades a plan package

## Read order

1. Read the issue first: its title and body. Note the reported bug
   and the expected behavior.
2. Read the thread highlights. Note any maintainer comments.
3. Read the repo-facts block and the repro evidence. Note what
   behavior the repro pins down.
4. Read the candidate plan last, so it is judged against the issue
   rather than the other way round. Note its stated cause, its list of
   changes and files, its not-in-scope line, and its test plan.
5. Read the candidate plan comment.

The issue comes first because every check asks whether the plan fits
the issue it belongs to; reading the plan first lets its framing stand
in for the issue.

## Evidence gathering

For each check in `rubric.md`, record the quote or fact named here
before grading anything:

- **diagnosis**: quote the sentence in the plan that states the cause
  of the bug (often under "Cause" or "Diagnosis"). Then list every repro
  step, control run, and Expected/Actual line, and mark each one
  "explained", "contradicts", or "not addressed" by that cause. Note
  whether the cause first appears as a claim in the thread.
- **scope**: quote the files the plan names as changing and its
  not-in-scope line. List every change in the plan and mark each one
  "needed for the bug" or "extra" (refactor, rename, migration,
  dependency bump, new feature, redesign).
- **stranger-can-start**: quote the location (file, function, or code
  area) and the concrete edit for each change. Record "goal only" for a
  change that states an aim but no edit.
- **test**: quote the test plan. Record the observable outcome it names,
  and whether that outcome would differ between the broken and fixed
  code, using the repro's Actual line as the broken result.
- **thread-direction**: quote every thread comment from an OWNER,
  MEMBER, or COLLABORATOR that gives direction (where the cause is,
  which approach, what to test), and any linked or open PR. Then quote
  where the plan comment follows it or explains diverging, or record
  "not engaged". If there is no such comment or PR, record "none".
- **ai-policy**: quote the AI part of the repo-facts contribution
  policy, or record "no AI policy". Record whether its requirements
  apply to issue comments or only to pull requests or code. If they
  apply to comments, quote the part of the plan comment that meets
  them, or record "missing".

If a check's evidence is not in the package, record "not found" for
that check. Do not look anywhere outside the package.

## Check execution

1. Grade the checks in the order they appear in `rubric.md`'s table.
2. Grade each check only from the evidence recorded for it above,
   against its pass condition:
   - **P (pass)**: the recorded evidence meets the pass condition.
   - **F (fail)**: the recorded evidence does not meet it.
   - **? (unclear)**: the evidence was "not found", or it could be read
     either way.
3. Write one line per check: its grade and the quote or fact that
   decided it.
4. Do not change a grade because the plan feels good or bad overall;
   only the pass condition decides.

## Verdict assembly

1. Look only at the checks weighted `required`.
2. If every required check is P, the verdict is `accept` (ready).
3. If any required check is F or ?, the verdict is `reject` (hold).
   `?` counts as a fail.
4. `preferred` checks are reported but never change the verdict.
5. In the summary, name the first required check that was not P and
   quote the evidence that decided it, then emit the JSON block that
   SKILL.md specifies, mapping P/F/? to `pass`/`fail`/`unclear`.
