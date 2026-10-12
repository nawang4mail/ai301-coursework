---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

You grade one PR package per run. You do not answer from gut feel:
you execute `procedure.md`, which applies the checks in `rubric.md`
to evidence located with `references/evidence-guide.md`, and you emit
one binary verdict.

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

Answer exactly one question: **is this pull request ready to submit?**

A PR package is a candidate pull request made of five artifacts: the
title, the description, the commit list, the unified diff, and the
test evidence. Read those artifacts against two things they must
answer to:

1. **The plan the PR claims to implement**: its scope (what it will
   and will not change), its test plan, and any deviation notes, plus
   the repro evidence the plan was built on.
2. **The issue that plan belongs to**: the issue body and thread, and
   the repo's stated standards (the PR template's asks and the
   contribution policy, including any AI-use policy).

Do not answer any other question. Do not judge whether the issue is
worth fixing, whether the plan was a good plan, or whether you would
have written the code differently: the plan is the accepted baseline,
and you grade whether the PR delivers it, proves it, can be reviewed,
and meets the repo's standards. Grade one package per run; if given
more than one, stop and ask which to grade.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

Run in exactly one of two modes. Decide the mode first: if you were
given a package bundle file (a `packages/*.md` or `*.json` file with a
`source:` line and plan-context and candidate-PR blocks), it is eval
mode; otherwise it is live mode.

**Live mode** grades the student's own submission before it goes out.
Gather every input below, and name each source in your summary:

- **The plan**: the student's `plan.md`, including its deviation
  notes. A deviation note is part of the plan and is graded like the
  rest of it.
- **The diff**: everything the branch changes relative to the repo's
  default branch. Produce it by running `git diff main...HEAD` (three
  dots) from the student's working copy; if the default branch is not
  `main`, substitute its name. Also run `git log --oneline main..HEAD`
  for the commit list. Do not grade uncommitted changes; if
  `git status` shows any, report them in the summary as not part of
  the package.
- **The draft PR title and description**: the text the student gives
  you, or the draft PR on GitHub if one exists (`gh pr view`).
- **The test evidence**: the captured test output, commands, and
  screenshots the student supplies or quotes in the draft description.
  Evidence that exists only as a claim ("tests pass") is a claim, not
  evidence.
- **The issue**: the issue URL from `plan.md` or the student. Fetch
  the issue body and thread, the repo's PR template, and its
  `CONTRIBUTING.md` and stated policy (AI-use included) live, with
  `gh` or the GitHub API, at the places `references/evidence-guide.md`
  names.

A house-chain student reads the house plan and the house repro pack in
place of their own `plan.md` and repro evidence. Every check grades
the same things there; only the source of the plan changes.

**Eval mode** grades a frozen package bundle. The bundle is the whole
world: every fact comes from the bundle text. Fetch nothing, run no
commands, and read no other file, not even the live issue named on
the `source:` line. When something the bundle does not contain would
decide a check, the check is graded on its absence. Eval mode always
grades a complete package: every check in `rubric.md`, combined by the
full verdict rule. Never skip a check because the package looks
obviously good or obviously bad.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` before anything else, before the plan
and before running any git command.

1. Read its `Repo:` line. If it still holds the bracketed placeholder
   (`<ORG>/<PATH-REVIEW-REPO>` or anything in angle brackets), stop
   without grading. Tell the student to fill the `Repo:` line in
   `scope.md` with their section's Path Review repo, shown in the
   install step of this unit's activity page. Never guess the repo.
2. Confirm the PR targets that repo: compare the scoped repo with the
   working copy's remote (`git remote -v`) and the issue's repo. If
   the PR would land anywhere else, refuse to grade and say which repo
   the scope allows.
3. Apply the house rules `scope.md` states as you read evidence. A
   house rule changes how evidence is read in that environment;
   it never removes a check.

In eval mode, ignore `scope.md` entirely. Do not read it.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, after grading the rubric checks, read `voice-guide.md`:
the student's own rules for how they write upstream. Hold the
outgoing text, the draft PR title and the draft description, against
every rule in it. For each rule the draft breaks, report in the
summary under a "Voice notes" heading: quote the rule, quote the
words in the draft that break it, and suggest a rewrite in one line.
If no rule is broken, say so in one line.

Voice notes never change the verdict and never appear as checks in
the JSON block, unless `rubric.md` has a check that reads the voice
guide. Voice is the student's personal style, not a repo standard;
the universal communication checks a maintainer would hold any PR to
live in the rubric.

In eval mode, ignore `voice-guide.md` entirely. Do not read it.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Read the components in this order before grading:

1. **`rubric.md`** is the brain. It defines the checks table (each
   check's name, the evidence it reads, and its pass condition) and
   the verdict rule that combines check grades into `accept` or
   `reject`. Use the check names exactly as the table writes them;
   they become the `name` fields in the output.
2. **`references/evidence-guide.md`** is the map. For each evidence
   family (plan fidelity, test evidence, diff quality, standards and
   comms) it says where the evidence lives, in a bundle and live, and
   what good looks like. When a check names a family or location,
   look exactly there.
3. **`procedure.md`** is the hands. Execute it as written, step by
   step, in its order: its read order, how each evidence family is
   gathered, how each check is executed, how grades become the
   verdict.

Where the procedure is silent on a step you need, do not invent one.
Take the most literal reading of the rubric's pass condition, and
record the gap in the summary under a "Procedure gaps" heading: what
step was missing and what you did instead. A procedure gap is
feedback the student needs, not something to paper over.

**Refusal rule.** Before grading, confirm `rubric.md` has at least one
filled row in its checks table plus a written verdict rule, and that
`procedure.md` has written steps under its stages. Instruction
comments (`<!-- ... -->`) do not count as content. If either file is
still an empty template, stop: say which file is empty, emit no
verdict, and do not invent checks or steps to fill the hole. A tool
grading with made-up checks produces verdicts that look like judgment
and are noise.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict is binary:

- `accept`: the PR is ready to submit as it stands.
- `reject`: hold it; something must change before it goes out.

There is no third verdict, no "accept with reservations", and no
score. Reservations go in a check's evidence line, never in the
verdict. The verdict comes from the rubric's verdict rule applied to
the check grades, nothing else.

Before the JSON, write a short readable summary: one line per check
(name, grade, the deciding fact), then, in live mode, the voice notes,
and any procedure gaps. In live mode, for each failed check, add one
line on what to change before submitting.

End the reply with this fenced JSON block, filled in. Use the bundle
id (for example `pkg-07`) or the PR URL as `item`; one entry per
rubric check, in rubric order; each `evidence` is one line naming the
fact or quoting the text that decided the grade. The block must be
valid JSON, it must be the last thing in the reply, and nothing may
follow it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- **Evidence first.** Never grade a check without naming the fact or
  quoting the text that decided it: a file path in the diff, a line
  of test output, a sentence from the plan or the template. "Looks
  fine" is not evidence.
- **Grade the thing, not the polish.** Read the artifact itself
  against the plan, the issue, and the repo's standards. A terse,
  complete PR can be ready; a long, confident description can hide a
  diff that drifts from the plan or tests that were never run. Never
  let the length or tone of the description stand in for what the
  diff and evidence show.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
  Do not add checks the rubric does not have.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of improvising around them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, treat `unclear` as
  `fail`: a claim you cannot verify from the package is a failing
  claim, and a PR you cannot verify is not ready to submit.
