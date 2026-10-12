# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2 and carried it through week 3; paste your filled voice-guide.md
here, whole. It is not re-authored and it is not graded as new work
this week.

Then reread it with the PR in mind. Your comments promised, reported,
and committed to an approach; a PR title and description ask a
maintainer to spend review time on your work. If your rules do not
cover that register (for example: how a title earns its thirty
seconds, how a description promises exactly what the diff contains,
how a disclosed shortfall is worded so it reads as honesty rather
than apology), extend the guide with what it needs. Extending is
allowed and encouraged; starting over is not required.

Live mode reads this file and holds your draft PR title and
description against it, reporting any rule your draft breaks. Eval
mode ignores it entirely, because your voice is yours and carries no
gold labels.
-->

## Who I am in threads

<!-- Paste your section here. -->
I am Nawang Gurung, a full stack developer experience in React, Node, and database architecture. I am contributing to this repository as part of an open-source capstone project to deepen my systems-level debugging skils. Maintainers can expect me to provide clear reproduction steps, communicate directly, and ask bounded questions when navigating unfamiliar core internals.

## Rules I write by

<!-- Paste your rules here, wrong/right pairs and all. Add any rule
the PR register needs that your comments did not. -->
### Rule: No promised timelines
I never commit to a delivery date or promise a fix will land by a specific time. I say what's actually done or about to happen instead.
- Wronng: I will fix this by tomorrow, primise!
- Right: Repro report on the way.

### Rule: Nme the version and the behavior
Every claim names the exact version I tested and the exact behavior I observed, never a vague "this bug" or "it".
- Wrong: I can confirm this bug happens.
- Right: v1.2 ignores --style, exactly as described.

### Rule: Say it like I'd say it out loud
No exclamation-point enthusiasn, no bot voice. I write the sentence I'd actually say to someone.
- Wrong: Hi!! Amazing project! Please assign me!
- Right: Picking this up --style is ignored on v1.2 exactly as described.

### Rule: Name what I'm still unsure of
If I'm not fully sure something is the actual cause, I say so in plain words instead of dropping the caveat to sound more finished.
- Wrong: This is caused by category: this.
- Right: This looks like it is caused by the category: this - I did not test every combination, but removing it alone stopped the error.

## Things I never post

<!-- Paste your list here; extend it if PR descriptions tempt you
toward new ones (claiming coverage the evidence does not show is the
classic). -->
- Deadlines or timeline quarantees for when my PR will be ready.
- Robotic trasition phrases like "Based on my investigation.." or "In conclusion..".
- Assertions that a bug is "easy" or "simple" to fix before I have run the test suite.
- "Me too" comments on an issu without providing a fresh, verified reproduction report.
- Something sounds like AI, too many use of ! and emojis.

