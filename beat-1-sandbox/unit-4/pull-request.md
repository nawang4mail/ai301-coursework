# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/157

**Branch**

fix/73-env-example-openrouter-key

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--only pkg-09,pkg-13,pkg-16` partial run: 3/3 agreed (no bar verdict on a partial run).
2. Full run, saved with `--save-run`: 18/20 (bar: 18/20: PASS). pkg-02 `failed: diff-hygiene`
   and pkg-05 `failed: decisive-evidence`, both clear accepts.
3. `--only pkg-02,pkg-05,pkg-07,pkg-12,pkg-14,pkg-15,pkg-18,calib-04 --include-calibration`
   partial run after revising two checks: 7/7 scored items agreed, and calib-04 agreed.
4. Full run, saved with `--save-run`: 20/20 (bar: 18/20: PASS).

**Package analysis**

pkg-05 (clear-accept). In run 2 my rubric decided `reject`; the gold label is `accept`.
The note column said `failed: decisive-evidence`, and the tool's evidence line was: "Issue
repro shown before/after, but the plan's second repro (same-name same-key: one row plus
warning) has no output, only the claim 'Same-key redefine prints the one-time warning.'"
The plan's test plan names two cases: "re-run the issue's script, expect both `atuin` rows;
re-run with two same-name same-key bindings, expect one row plus a warning". The PR shows a
full before/after table for the first case, but for the second only reports the result in a
sentence. My check at the time said "every failure mode or repro the test plan names must be
shown", so a reported-but-not-pasted second case failed it. In run 4, after I changed the
check to need pasted output only for the issue's own repro, it graded `accept`.

**Check rationale**

| diff-hygiene | Every hunk in the diff | The diff contains only the change and what it needs (the fix, its tests, docs or changelog the repo asks for, and explanatory comments). Fail if any hunk carries debris: debug prints or logging left in, commented-out code or earlier attempts, dead functions or `allow(dead_code)`-style suppressions, leftover TODOs, or churn: a hunk or block whose only change is formatting, re-indentation, import reordering, or lines removed and re-added identically. Lines a diff re-shows as removed and re-added inside a statement whose content the fix changes (for example the other arguments of a call where one argument changes) are part of the fix, not churn. | required |

My first version of this check ended with "formatting, re-indent, import-reorder, or
re-printed-line churn on lines the fix does not need." In run 2 that made the tool reject
pkg-02, a clear accept, with the evidence: "save_html/save_svg hunks remove and re-add
unchanged lines (`html = self.export_html(`, `theme=theme,` ...), which is identical re-add
churn." Those lines only show as removed and re-added because the fix changes `clear=clear`
to `clear=False` inside the same multi-line call. I rewrote churn as "a hunk or block whose
only change is formatting ... or lines removed and re-added identically", and added the
sentence saying lines re-shown inside a statement the fix changes are part of the fix. I did
not drop identical re-adds from the check altogether, because pkg-18's debris is exactly
that: blocks of identical re-printed lines with nothing else changed.

**Trade-offs**

Both revisions loosened a check, so before the confirming full run I re-ran canaries with
`--only`: for `diff-hygiene`, pkg-12, pkg-15 and pkg-18 (the three unreviewable packages,
pkg-18 being the one whose debris is re-printed lines); for `decisive-evidence`, pkg-07 and
pkg-14 (not-tested) plus calib-04 with `--include-calibration`. All stayed `reject`. calib-04
is the case the looser evidence check has to keep catching: its plan names two failure modes
and the evidence never mentions the `--exec` abort at all, which still fails because the
check only accepts a secondary item that is "reported as run with its result stated". The
cost is that a PR which states a secondary result without pasting output now passes, so my
tool trusts that one sentence in a way it did not before.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
