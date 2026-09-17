# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every `###` section. Each is graded on its own; content placed under the
wrong heading is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.


## issue-select — live mode: issue #73

Scope check (scope.md): in bounds — codepath/pathreview-ai301-fa26-s3 matches the
scoped repo. No house-rule conflicts apply here (no claim comments to override).

| Check | Grade | Evidence |
|---|---|---|
| Maintainer alive | pass | Last 5 default-branch commits are all human-authored (Andrew Burke), dated today (2026-09-16), well within 90 days |
| Repo in active use | pass | Not archived; last push to any branch is 2026-09-16T21:50:20Z (today) |
| Scope fits a newcomer | pass | Bounded, single deliverable: make .env.example agree with README.md/core/config.py on OPENROUTER_API_KEY; verified both files myself — .env.example only lists mock/openai while config.py defines openrouter_api_key too. Labeled good first issue/tier-1, maintainer's own 1-2 hour estimate, no umbrella/tracking structure, no unresolved design |
| Unclaimed | pass | assignees: [], 0 comments, no linked PRs found via timeline or PR search |
| Allowed under contribution policy | pass | CONTRIBUTING.md has no AI-related policy language at all (no AGENTS.md/AI_POLICY.md either) — silence passes |
| Recently released (preferred) | fail | No releases published in this repo at all — doesn't affect verdict |

Fit note: this is a good match for the stated fit profile — it's plain-text/config
editing (no frontend, no specialized domain knowledge) that still forces reading
across three files (README.md, .env.example, core/config.py) to find the mismatch,
which is exactly the "reading an unfamiliar codebase" practice the profile names.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
  "checks": [
    {"name": "Maintainer alive", "grade": "pass", "evidence": "last 5 default-branch commits all by Andrew Burke, dated 2026-08-24 to 2026-09-16 (today)"},
    {"name": "Repo in active use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16T21:50:20Z"},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "issue asks only to reconcile README.md/.env.example with core/config.py's field names; maintainer-estimated 1-2 hours; good first issue/tier-1 labels; no sub-items to split, no unresolved design"},
    {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: []; comments: 0; no linked PRs found"},
    {"name": "Allowed under contribution policy", "grade": "pass", "evidence": "CONTRIBUTING.md contains no AI-related policy statement; no AGENTS.md or AI_POLICY.md in repo root"},
    {"name": "Recently released", "grade": "fail", "evidence": "gh release list returns no releases for this repo"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Run 1 (original rubric): `agreement: 9/20 scored items  (bar: 18/20: below the bar;
category floor unmet: no match in claimed, policy)`.

Run 2 (after rewriting the rubric — added Scope, Unclaimed promoted to required,
added the contribution-policy check): `agreement: 17/20 scored items  (bar: 18/20:
below the bar)` — category floor cleared (`categories: claimed 4/4  clear-accept
6/8  dead-repo 3/3  policy 1/1  scope 3/4`), but still one short of the bar. All 3
misses traced to the wording of the "Scope fits a newcomer" check.

Partial re-check (`--only issue-01,issue-19,issue-20,issue-05,issue-10,issue-15`,
after tightening that check's wording): `agreement: 6/6 scored items` — a cheap
$0.20-class run to confirm the fix before spending a full run.

Run 3 (final, confirming): `agreement: 19/20 scored items  (bar: 18/20: PASS)`.
This is the score in the committed `eval-run.txt`.

**Issue analysis**

`issue-03` (pylint-dev/pylint#9143). Gold label: `reject` (category: `claimed`).
My rubric's verdict: `reject` — match.

Reasoning: the comment thread contains a collaborator's comment, "We are **not**
looking for any other contributions other than @hamza-mobeen's PR:
`https://github.com/pylint-dev/pylint/pull/10985`." The bundle's repo-facts line
also lists a currently open linked PR (`pylint-dev/pylint#10985 (open)`). My
`Unclaimed` check's pass condition is "no assignee, AND no currently open linked
PR working the issue" — the open linked PR fails that condition, `Unclaimed` is a
`required` check, so the issue is rejected. (My first-draft rubric had missed
this: it weighted `Unclaimed` as `preferred`, which never gates the verdict, so
run 1 graded this issue `accept` against gold `reject` — promoting it to
`required` is what fixed it.)

**Check rationale**

Quoted as currently written in `rubric.md`:

> Scope fits a newcomer | the issue body, the comment thread, and the "linked
> PRs" line | the issue is one bounded deliverable a single contributor could
> finish in one PR — a checklist of related edits toward one feature/page still
> counts as bounded, but an umbrella issue that explicitly invites separate
> contributors to split off independent sub-items (e.g. a list of other
> issue/PR links to divide up) does not. Fails if: it is a pure usage/support
> question; a maintainer states in the thread that it touches core internals or
> that the design is still unsettled; the issue's own scope is left open by its
> author (marked TBD / "possibly" / "likely" with no maintainer confirmation of
> what's wanted); or it carries a history of multiple closed, unmerged linked
> PRs that already attempted it | required

Reasoning: my first-draft rubric had no check for this family at all (the lecture's
third family, "does the scope fit a newcomer"), so run 1 scored `scope 1/4`. I
wrote this check to cover the concrete failure patterns the eval set actually
tests: a doc "megaissue" listing 32 sub-issues to divide up (`issue-10`), a
tracking issue with 106 comments and dozens of contributors each claiming a
different file (`issue-05`), and an issue with two prior closed/unmerged PRs
against it (`issue-15`).

**Trade-offs**

The first version of this check was too blunt: it false-rejected two
gold-`accept` clear-accept issues (`issue-01`, a docs reorg written as a
numbered checklist for one contributor to complete in one PR; `issue-19`, a bug
report with two numbered causes and follow-up suggestions) because a numbered
list in the body read like an "umbrella" even though it wasn't one meant to be
split across contributors. It also missed a gold-`reject` scope issue
(`issue-20`, a bot-filed feature request with no maintainer engagement and its
own body admitting "Logo asset TBD" / "possibly app wiring... if needed").

I re-ran just those 6 issues with `--only issue-01,issue-19,issue-20,issue-05,
issue-10,issue-15` after rewording the check to explicitly separate "a checklist
toward one feature" (bounded) from "an umbrella that invites separate
contributors to split off sub-items" (not bounded), and to add the "issue's own
scope is left open by its author" clause. That canary came back `6/6`. The
trade-off I accept: on the full 20-issue run, `issue-19` still came back a false
reject (`failed: Scope fits a newcomer`) even though the canary got it right —
Sonnet's grading of this specific check isn't perfectly stable run-to-run on
that issue's wording, and I chose not to chase that last point further rather
than risk overfitting the check to one run's randomness, since the full run
still cleared both the 18/20 bar and every category floor.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to my interests and time available**: I want to get better at reading unfamiliar codebases and want to avoid heavy frontend work. This one is a config/doc fix, which is not an heavy frontend job. It include reading the code, evaluating the claude's output and learning to use it. So, it match with my interest and time available.
2. **What the verdict identified correctly, and what you weighed that the rubric could
   not.**: The verdict mechanically confirmed the checks that matter for any issue- maintainer alive, repo active, unclaimed, no AI-contribution ban - and it flagged the scope check as passing because the issue is one bounded deliverable with a concrete effort estimate. what it couldn't weigh is the as I'm coming from Java not python, and this repo is mostly python. I judge myself that the language gap doesnt matter here because the fix touches no application code at all - it's two text file so my unfamiliarity with the codebase's is non-issue for this particular case, even though the rubric has no check for "does this match my language background."
3. **The anticipated difficulty in claiming it.**: I already verified the actual mismatch myself, so there's no ambiguity about what "done" looks like. the friction I anticipate is procedural, not technical: 'CONTRIBUTING.md' requires all five CI jobs ('lint', 'typecheck', test-unit'. 'test-integration', 'frontend') to pass even for a two-file changes, and notes that a first PR from a new GitHub account sits at 'waiting for approval to run workflows' until a maintainer releases it - so the actual turnover may be slower than the 1-2 hour estimate suggests.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
