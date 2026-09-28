# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**
nawang4mail

---

## Posted upstream

**Claim comment**
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5863155285
<br>
This will be my first contribution to this repo - I'd like to pick this one up.
Looking at the issue: .env.example and README.md only document the openai key, but core/config.py also defines openrouter_api_key, so the docs and the actual config are out of sync.
I'll dig into the mismatch, reproduce it, and report back with what I find before opening a PR.


**Reproduction comment**
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5863589347

## Environment
- OS: macOS 27.0 (Build 26A428)
- git 2.51.0
- Repo: fork of codepath/pathreview-ai301-fa26-s3, commit 2f4e82f52efbcfcc57d65b3fa5348672163ca088
  ("chore: track five more manifest entries against the tracker")
- No Python/language runtime version is recorded because this issue is a static
  docs/config-template mismatch, not a running-code bug — nothing in the app is
  executed to reproduce it. (Another commenter on this issue already confirmed
  no application runtime environment is required.)

## Steps to reproduce
1. git clone https://github.com/nawang4mail/pathreview-ai301-fa26-s3.git
2. cd pathreview-ai301-fa26-s3
3. cp .env.example .env
4. grep -n "OPENROUTER_API_KEY\|LLM_PROVIDER" README.md .env.example
5. grep -n "openrouter" core/config.py

## Observed behavior
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
.env.example:18:LLM_PROVIDER=mock

grep -n "OPENROUTER_API_KEY" .env.example returns no match at all — the key
README tells you to set doesn't appear anywhere in the template.

core/config.py:20:    openrouter_api_key: str = Field(default="")
core/config.py:21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
core/config.py:22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")

## What this shows
This confirms the exact mismatch the issue describes: README.md's Quick Start
(line 24) instructs adding OPENROUTER_API_KEY to .env, but .env.example never
defines that variable, and its only LLM_PROVIDER line (18) is left at "mock"
with no mention of "openrouter" as a valid value. core/config.py (lines 20-22)
proves the app already has full settings support for openrouter — the
.env.example template just never surfaces it, so a newcomer following the
template alone would have no way to discover the key name and would have to
read the source to find it.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (original rubric): `agreement: 17/20 scored items  (bar: 18/20: below the bar;
category floor unmet: no match in disclosure)`. Misses: pkg-05 (failed: Steps followable),
pkg-09 (failed: Behavior proven), pkg-20 (graded accept against gold reject).

Run 2 (after adding the honest-cannot-reproduce carve-out to Behavior proven, and a first
attempt at loosening Steps followable): `agreement: 17/20 scored items  (bar: 18/20: below
the bar; category floor unmet: no match in disclosure)`. Fixed pkg-09, but the Steps wording
still didn't fix pkg-05 and newly broke a previously-agreeing package, pkg-12. Misses:
pkg-05, pkg-12, pkg-20 (all "failed: Steps followable" except pkg-20).

Run 3 (after rewriting the Comms/AI-disclosure check to distinguish disclosure-type from
voice-type repo policies, and refining Steps followable further): `agreement: 18/20 scored
items  (bar: 18/20: PASS)`. Fixed pkg-20 and pkg-12, but the disclosure rewrite newly broke
pkg-03. Misses: pkg-03 (failed: AI Disclosure Compliant), pkg-05 (failed: Steps followable).

Run 4 (after further refining Steps followable to allow structural, non-literal
descriptions of a trigger file's contents): `agreement: 19/20 scored items  (bar: 18/20:
PASS)`. Fixed pkg-05. Miss: pkg-03 only.

Run 5 (final, confirming — after adding the voice-vs-disclosure distinction to the Comms
check): `agreement: 20/20 scored items  (bar: 18/20: PASS)`. This is the score in the
committed `eval-run.txt`.

**Package analysis**

`pkg-09` (sharkdp/fd#2033). Gold label: `accept` (category: `clear-accept`), noted as "honest
cannot-reproduce: real attempt at the argument-size reordering with marker-order artifacts,
names what differed... and what a triggering setup likely needs."

My rubric's Run 1 verdict: `reject` — mismatch. My "Behavior proven" check unconditionally
required artifacts matching the issue's described failure ("raw terminal output, stack
traces, or screenshots that match the original issue's description"), with no exception for
an honest cannot-reproduce. The candidate's own log output showed the *correct* argument
order (`ONE ONE ONE TWO TWO TWO`), not the reordering bug — so my check read this as "no
proof of the failure" and failed it, even though the report was honest about not
reproducing and showed real evidence of the actual attempt (my "Honest outcome" check
already agreed it was honest). The rubric's own intro text says "an evidenced
cannot-reproduce is a pass," but the Behavior proven row had no clause making that possible.

I fixed this by adding an explicit alternate clause: when the outcome is an honest
cannot-reproduce, artifacts from the real attempt (showing what was tried and what differed
from the trigger conditions) satisfy the check instead of requiring the original failure to
be shown. From Run 2 onward, pkg-09 reads `accept`, matching gold.

**Check rationale**

Quoted as currently written in `rubric.md`:

> Behavior proven | Repro report (Observed behavior) | The report includes raw terminal
> output, stack traces, or screenshots that match the original issue's description. If the
> outcome is an honest connot-reproduce(see Honest outcome), artifacts from the real attempt
> - showing what was tried and what differed from the issue's trigger conditions- satisfy
> this check instead.| required

Reasoning: my first-draft rubric had this check judge only whether the artifact shows the
issue's failure, with no room for an honest cannot-reproduce — which is exactly the family
pkg-09 tests. I added the second sentence so a real, evidenced attempt that shows what
differed from the trigger conditions counts as proof, without opening the door to a bare
assertion ("I couldn't reproduce it") with no artifacts at all — that would still fail,
since it shows no real attempt.

**Trade-offs**

Loosening Steps followable to accept a structural (non-literal) description of a trigger
file's contents — rather than requiring the file's exact contents verbatim — is what fixed
pkg-05 (Run 4), but it's a real trade-off: a report that describes a trigger file only in
general terms could now pass even when the specific values *do* matter to whether the bug
fires, not just the structure. I accept this because pkg-05's actual trigger (conda's
`EnvironmentSectionNotValid`) is insensitive to the specific dependency values — only the
presence of a `category:` key matters — so a structural description is genuinely sufficient
proof for that family, even though the same looser wording would miss a report that hand-waved
a value that did matter. I chose not to write a more elaborate rule distinguishing the two
cases, since the eval set's `clear-accept` category came back 8/8 in the final run with the
simpler wording, and a more complicated rule risks overfitting to pkg-05 specifically.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
