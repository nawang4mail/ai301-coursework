# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: week 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; your operator swap showed you what that feels
like. Write the map you wish your executor had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
- <b>Where it lives:</b> In the eval bundle, look within the "Repro report" block. In live mode, look at the draft reproduction comment under the "Environment" heading.
- <b>What good looks like:</b> The report explicitly list the OS and key dependency versions. These versions must either match the ones reported in the original issue, or the author must explicitly state why they are using a different version.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
- <b> Where it lives: </b>"Repro report" block or the live draft comment under "Steps to reproduce".
- <b> What good looks like: </b>A step describing a file's exact required contents in words counts as followable without a literal code block, as long as it's clear which elements are necessary and sufficient to trigger the behavior. When the bug's trigger is structural rather than value-specific, naming that structural element is enough- it does not need literal file contents, since any valid values would trigger the same behavior. Only fail this when the description leaves ambiguity about whether the trigger condition itself would actually occur.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
- <b> Where it lives: </b>In the "Repro report" block or live draft commnent, under "Observed behavior", "Logs" or attached screenshots.
- <b> What good looks like: </b>The report includes raw terminal output, full stack traces, or visual proof of the failure state. It does not just summarize the failure (eg. "the app crashed"); it proves it by showing the exact error. For an honest cannot - reproduce, the artifact does not need to show the issue's failure - it needs to show real, verifiable output from the actual attempt that lets a reader see what happened and how it differs from what the issue describes. A cannot- reproduce with no artifacts at all - just an assertion "I couldn't reproduce it" - still fails the check; the proof requirement shifts from "shows the failure" to "show the real attempt", it doesn't disappear.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
- <b> Where it lives: </b>Cross-reference the "Oserved behavior" artifacts in the repro report against the "Issue context" description.
- <b> What good looks like: </b>The provided logs or screenshots actually match the steps taken. A "cannot reproduce" report is accepted as honest as long as they provide the exact environment and steps they tried. They do not claim to have a fix ready if they haven't submitted a PR.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
- <b> Where it lives: </b>Look at the "Claim comment" and "Repro report". Cross-reference this with the "contribution policy" line in the "repo-facts" block.
- <b> What good looks like: </b> Treat every candidate comment as AI-assisted work, but read what the repo-facts policy actually asks for - don't apply same rule to every repo: if the policy demands disclosure of AI use, this check passes only if the comment contains an explicit disclosure statement; absence if a fail. If the policy demands human-authored voice, this check passes if the comment reads as natural human writing- specific, personal, non-boilerplate- with no explicit disclosure statement required. If the policy says nothing about AI, or only asks contributors to review/ understant AI-assisted content responsibly (not disclose it), this check passes automatically.