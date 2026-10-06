# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->
I am Nawang Gurung, a full stack developer experience in React, Node, and database architecture. I am contributing to this repository as part of an open-source capstone project to deepen my systems-level debugging skils. Maintainers can expect me to provide clear reproduction steps, communicate directly, and ask bounded questions when navigating unfamiliar core internals.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->
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

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
- Deadlines or timeline quarantees for when my PR will be ready.
- Robotic trasition phrases like "Based on my investigation.." or "In conclusion..".
- Assertions that a bug is "easy" or "simple" to fix before I have run the test suite.
- "Me too" comments on an issu without providing a fresh, verified reproduction report.
- Something sounds like AI, too many use of ! and emojis.