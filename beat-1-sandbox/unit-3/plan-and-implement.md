# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

nawang4mail

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-6008145607

> My plan for #73, built on my repro above (fork at `2f4e82f`)
>
> The repro shows the template is the file that's out of date:
> README.md:24 says to set `OPENROUTER_API_KEY` (since the repro I also found docs/SETUP.md:47 says the same), and `core/config.py` (lines 20-22) already reads it, but `grep -n "OPENROUTER_API_KEY" .env.example` comes back empty, so a
> copied `.env` has no line for it.
>
> Plan: one line in `.env.example`. In the `# LLM provider` block, add an empty `OPENROUTER_API_KEY=` with a comment pointing to README's Quick Start. I'm keeping `OPENAI_API_KEY`, since the embeddings provider still
> uses it.
>
> On #85: it makes the same one-line fix with `sk-or-your-key-here` as the value. I'm leaving the value empty so a copied `.env` doesn't carry a fake-looking key.
>
> Not changing: `core/config.py`, README, SETUP, or the base URL/model settings (config already has defaults for those). I'm also not adding "openrouter" to the `LLM_PROVIDER` options. The only provider switch I found (`get_embedding_provider`) accepts `mock` and `openai`, so listing it would describe an option that doesn't exist.
>
> Test: re-run the repro on the branch. Before, `grep OPENROUTER_API_KEY` on `.env.example` and the copied `.env` finds nothing. After, both show the new line, and with a dummy value in `.env`, `Settings()` loads it.
>
> Open question: I couldn't find any code that reads `openrouter_api_key` outside `core/config.py`, so I'm not sure "required for AI features" in SETUP.md is accurate yet. Leaving that alone unless someone knows otherwise.

---

## Your branch

**Branch**

fix/73-env-example-openrouter-key

**Evidence**

The test plan from `plan.md` (my Unit 2 repro steps, plus a `Settings()` load check),
run on my fork before and after the change.

Before, on `main` at `2f4e82f`:

```
+ git log --oneline -1
2f4e82f chore: track five more manifest entries against the tracker
+ cp .env.example .env
+ grep -n 'OPENROUTER_API_KEY\|LLM_PROVIDER' README.md .env.example
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
.env.example:18:LLM_PROVIDER=mock
+ grep -n OPENROUTER_API_KEY .env
+ echo '(no match)'
(no match)
+ grep -n OPENROUTER_API_KEY docs/SETUP.md
47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
+ sed -i '' 's/^OPENROUTER_API_KEY=$/OPENROUTER_API_KEY=test-key-73/' .env
+ python -c 'from core.config import Settings; print(repr(Settings().openrouter_api_key))'
''
```

After, on `fix/73-env-example-openrouter-key`, with the change made to `.env.example`
(the output below was captured before committing, so `git log` still shows `2f4e82f`;
the change is commit `7c9ab78`):

```
+ git log --oneline -1
2f4e82f chore: track five more manifest entries against the tracker
+ cp .env.example .env
+ grep -n 'OPENROUTER_API_KEY\|LLM_PROVIDER' README.md .env.example
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
.env.example:18:LLM_PROVIDER=mock
.env.example:21:OPENROUTER_API_KEY=
+ grep -n OPENROUTER_API_KEY .env
21:OPENROUTER_API_KEY=
+ grep -n OPENROUTER_API_KEY docs/SETUP.md
47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
+ sed -i '' 's/^OPENROUTER_API_KEY=$/OPENROUTER_API_KEY=test-key-73/' .env
+ python -c 'from core.config import Settings; print(repr(Settings().openrouter_api_key))'
'test-key-73'
```

Before the fix, the copied `.env` has no `OPENROUTER_API_KEY` line and `Settings()`
reads `''`. After it, `.env` line 21 is `OPENROUTER_API_KEY=` and a value set there
loads as `'test-key-73'`.

## Eval iterations

**Run history**

1. `--limit 3` smoke run: 1/3. `system-requirements` failed pkg-02 and `capture-age`
   failed pkg-03, both clear accepts.
2. Full run, saved with `--save-run`: 19/20 (bar: 18/20: PASS).

**Package analysis**

pkg-14 (clear-accept). My rubric decided `reject`; the gold label is `accept`. The
note column says `failed: scope, stranger-can-start`. The plan names areas rather
than files ("the client attach/reattach path in `zellij-server` ... and
`zellij-client`'s terminal query issuance") and says "exact functions to be pinned in
the PR after tracing the query issuance with debug logs". My `stranger-can-start`
check needs "where the change goes (file, function, or a uniquely identifiable code
area)", and my `scope` check needs the plan to name "each file it will change", so a
plan that defers picking its functions fails both. The gold note itself calls the
plan "arguable on the deferral, ready as scoped".

**Check rationale**

| scope | Where the plan says what it will change, and its not-in-scope line, read against the issue's reported bug | The plan names each file it will change and says what it won't touch, and every change listed is needed to fix the reported bug (the fix, the same fix at sites with the identical defect, and tests for it). Fail if it also includes a refactor, rename, migration, dependency bump, new feature, or redesign the fix does not require. | required |

My first version of this check was "The plan names each file it will change and says
what it won't touch." That only looks at whether the plan lists files and has a
not-in-scope line, so a plan that names its files and then also bundles a refactor or
redesign would still pass, and that is exactly what the scope-creep packages do. I
added the second half, that every change listed must be needed for the reported bug,
and named the kinds of extra work that fail it. I kept "the same fix at sites with the
identical defect, and tests for it" inside the allowed list so a fix applied at two
call sites, or a fix with its test, doesn't count as creep.

**Trade-offs**

`stranger-can-start` costs me pkg-14, a clear accept. I accept that miss rather than
loosen the check: allowing "functions to be pinned later" is the same wording an
unbuildable plan uses, so loosening it risks the three unbuildable packages that it
currently rejects (3/3 in the final run). The `scope` check also showed a trade-off on
my own plan: when I added a comparison with PR #85, a live plan-check run graded
`scope` as `unclear` because my extra `OPENROUTER_BASE_URL`/`OPENROUTER_MODEL` lines
weren't needed for the bug, and I dropped them to get back to accept.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
