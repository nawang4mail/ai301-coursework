# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

Read in this order before grading anything. In eval mode every part
is a section of the bundle; in live mode, use the sources SKILL.md
and `references/evidence-guide.md` name for each.

1. **The plan** (`## Plan context`; live: `plan.md`). Write down:
   the files it will change, the change it describes, its not-in-scope
   line, every deviation or deferral note, each item its test plan
   names (every repro, failure mode, control, and the expected-after),
   and any suite command it names.
2. **The issue and thread** (`## Issue`, `## Thread highlights`).
   Write down the reported broken behavior and its repro steps, and
   any maintainer direction (OWNER, MEMBER, COLLABORATOR).
3. **The repo's asks** (`## Repo facts`). Write down each PR-template
   ask and contribution-docs ask as a separate item, any test or check
   command, and the AI policy's PR requirements (or "no AI policy").
4. **The diff** (`### Diff`). List every changed file, and every hunk
   with a few words on what it does.
5. **The test evidence** (`### Test evidence`). List each command
   shown and its before and after output.
6. **The commits, then the title and description** (`### Commits`,
   `### Title`, `### Description`). List every claim the description
   makes about what changed, what was tested, and what was deferred,
   and every template section or disclosure it contains.

The plan comes first because it is the yardstick: the diff, the
evidence, and the description are each read against it, so its
boundary and test plan must be written down before anything they
are measured against. The description comes last on purpose: read
first, a confident "implements the plan exactly" frames the diff, and
drift slips past. The diff is read on its own terms before any claim
about it is.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

For each check in `rubric.md`, record the facts below before grading.
Quote or name the exact line, file, or hunk; write "not found" when
the package does not contain it. Look nowhere outside the package in
eval mode.

- **plan-fidelity**: make a two-column table. Left: each file and
  hunk from the diff. Right: the plan item or deviation note it
  belongs to, or "none". Then list each change the plan promises (fix
  sites, tests, docs, changelog) and mark it "in diff", "deferred:
  <quote of the note>", or "missing".
- **description-honesty**: take each claim listed from the
  description in read-order step 6 and mark it "true of diff",
  "contradicted by <file/hunk>", or "not in diff". Fidelity claims
  ("exactly as planned", "no changes beyond", "docs updated") are
  always checked against the plan-fidelity table.
- **decisive-evidence**: first, for the issue's reported repro,
  record the command, its after output, its before output or the
  broken behavior it replaces, and whether the after matches the
  plan's expected-after; mark it "shown with output" or "not shown".
  Then, for each other item the plan's test plan names, mark it
  "shown with output", "reported with result: <quote>", "deferred:
  <quote>", or "never mentioned". Note whether the runs go through
  the code the diff changed or only a control or unchanged path.
- **repo-checks-run**: list each suite or check command the plan or
  repo facts name. For each, quote the evidence line showing it run
  with a result, or record "not shown". If none are named, record
  "none named".
- **diff-hygiene**: go through the hunks listed in read-order step 4
  and mark each one "fix", "test", "explanatory comment", "required
  doc/changelog", or a debris kind from the evidence guide (debug
  print, commented-out code, dead code, TODO, formatting/import churn,
  identical re-add block). Before marking removed-and-re-added lines
  as churn, check whether another line in the same statement or call
  changes; if it does, mark them "fix". Quote each debris line.
- **template-asks**: for each ask listed in read-order step 3, quote
  where the PR meets it (section heading, checklist answer, "closes
  #N", changelog file in the diff), or record "marked n/a: <reason>",
  "missing", or "not showable in a PR" (CLA, vouch flow).
- **ai-disclosure**: quote the AI policy's PR requirements, or record
  "no AI policy" / "no PR requirement". If it has requirements, quote
  the description's disclosure for each part, or record "missing".
- **commit-messages**: list each commit message and mark it
  "describes change" or "placeholder".

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade the checks in the order they appear in `rubric.md`'s table.
2. Grade each check only from the evidence recorded for it above,
   against its pass condition:
   - **P (pass)**: the recorded evidence meets the pass condition.
   - **F (fail)**: the recorded evidence breaks it. One failing item
     (one drifting hunk, one debris line, one missing ask, the
     issue's repro not shown, one test-plan item never mentioned) is
     enough for F.
   - **? (unclear)**: the deciding evidence is "not found", or the
     recorded facts can honestly be read both ways.
3. When evidence is genuinely absent, do not search further and do not
   fill the gap with assumption: an absent required item (a missing
   before/after, a missing disclosure) is F when the pass condition
   requires it to be shown; it is ? only when the package is silent on
   whether it was needed at all.
4. A check may be graded from its recorded evidence alone without
   re-reading the package. Re-open the package only when a recorded
   fact is ambiguous, and then only the section the evidence guide
   names for that check.
5. Grade each check independently. A strong fix does not lift a
   failing evidence check, and a failing check does not drag down the
   others.
6. Write one line per check: its grade and the quote or fact that
   decided it.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Look only at the checks weighted `required`.
2. If every required check is P, the verdict is `accept` (submit).
3. If any required check is F or ?, the verdict is `reject` (hold).
   `?` counts as a fail.
4. `preferred` checks are reported but never change the verdict.
5. The deciding check is the first required check, in rubric order,
   graded F or ?. In the summary, name it and quote the evidence that
   decided it. For an `accept`, say that every required check passed.
6. Emit the JSON block that SKILL.md specifies: one entry per rubric
   check in rubric order, mapping P/F/? to `pass`/`fail`/`unclear`,
   with the recorded quote or fact as the evidence line, and the
   verdict from steps 2-3.
