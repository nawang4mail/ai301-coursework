# Plan: #73 README and `.env.example` disagree about which LLM API key to set

## Repro evidence this plan builds on

From my reproduction on issue #73 (fork at commit `2f4e82f`, macOS):

```
$ cp .env.example .env
$ grep -n "OPENROUTER_API_KEY\|LLM_PROVIDER" README.md .env.example
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
.env.example:18:LLM_PROVIDER=mock

$ grep -n "OPENROUTER_API_KEY" .env.example
(no match)

$ grep -n "openrouter" core/config.py
core/config.py:20:    openrouter_api_key: str = Field(default="")
core/config.py:21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
core/config.py:22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```

Since posting the repro I also checked docs/SETUP.md, which gives the
same instruction:

```
$ grep -n "OPENROUTER_API_KEY" docs/SETUP.md
47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
```

So README.md (line 24) and docs/SETUP.md (line 47) tell a newcomer to set
`OPENROUTER_API_KEY` in `.env`, `core/config.py` defines that setting, but
the `.env.example` template they copy never contains it.

## Diagnosis

The template is out of date, not the docs or the config. `core/config.py`
already reads `OPENROUTER_API_KEY`, `OPENROUTER_BASE_URL`, and
`OPENROUTER_MODEL` from `.env`, and README/SETUP already point people at
`OPENROUTER_API_KEY`. The only file that does not know about it is
`.env.example`, so copying it gives an `.env` with no line for the key the
docs ask for.

## Scope

In scope: `.env.example` only. In its `# LLM provider` block, add
one line, `OPENROUTER_API_KEY=` (empty placeholder), directly under
`OPENAI_API_KEY`, with a one-line comment that this is the key README's
Quick Start refers to.
Keep `OPENAI_API_KEY`, because `ingestion/embeddings/provider.py` still
uses it for the `openai` embedding provider.

Not in scope:
- `core/config.py`: no settings are added, renamed, or removed.
- README.md and docs/SETUP.md: once the template lists the key, their
  instruction is accurate as written.
- Adding `"openrouter"` to the `LLM_PROVIDER` options comment. No code I
  found accepts that value (the only provider switch,
  `get_embedding_provider`, supports `mock` and `openai`), so listing it
  would document an option that does not exist.
- `OPENROUTER_BASE_URL` and `OPENROUTER_MODEL`. `core/config.py` has
  working defaults for both, and the docs never ask anyone to set them,
  so they are not part of this bug.
- Wiring OpenRouter into any code path.

## Files

- `.env.example`, the `# LLM provider` block (lines 16-19).

## Approach

1. Branch `fix/73-env-example-openrouter-key` from `main` on my fork.
2. Edit the `# LLM provider` block of `.env.example` as described in Scope.
3. Re-run the repro steps (test plan below).
4. Commit with a Conventional Commit message (`docs: ...`), keeping
   `plan.md` and `comment.md` out of the commit.

## Test plan

Re-run my repro steps on the branch:

```
cp .env.example .env
grep -n "OPENROUTER_API_KEY\|LLM_PROVIDER" README.md .env.example
grep -n "OPENROUTER_API_KEY" .env
grep -n "OPENROUTER_API_KEY" docs/SETUP.md
```

Before the fix: `grep -n "OPENROUTER_API_KEY" .env.example` and `.env`
return no match. After the fix: both return the new
`OPENROUTER_API_KEY=` line, so README.md:24's instruction points at a
line that exists in the copied `.env`.

Also confirm the setting loads from the copied file, using a dummy value
so the check can tell "loaded from `.env`" apart from the empty default:

```
sed -i '' 's/^OPENROUTER_API_KEY=$/OPENROUTER_API_KEY=test-key-73/' .env
python -c "from core.config import Settings; print(repr(Settings().openrouter_api_key))"
```

Expected after the fix: `'test-key-73'`. Before the fix the `sed` has no
line to change, so it prints `''`. Then run the
existing test suite (`make test`) to confirm nothing else changed.

## Related work

PR #85 (a classmate's) is open against this issue and adds
`OPENROUTER_API_KEY=sk-or-your-key-here` as one line under
`OPENAI_API_KEY`. My plan makes the same one-line fix but leaves the
key empty, so a copied `.env` never carries a fake-looking key value,
and adds a comment tying the line to README's instruction. Under the course's house rules, #85 does not block
this plan.

## Risks and unknowns

- Unknown: whether any code path actually uses `openrouter_api_key` yet.
  I found nothing that reads it outside `core/config.py`. That does not
  change this fix, since the docs already ask for the key, but it means
  "required for AI features" in docs/SETUP.md may overstate things. I am
  leaving that sentence alone and noting it rather than guessing.

## Deviations

Nothing changed; the plan held. The build is the one change the plan
named: an empty `OPENROUTER_API_KEY=` line in the `# LLM provider` block of
`.env.example`, directly under `OPENAI_API_KEY`, with a one-line comment
pointing to README's Quick Start (commit `7c9ab78` on
`fix/73-env-example-openrouter-key`). No other file was touched. The test
plan ran as written: before the fix, the copied `.env` had no
`OPENROUTER_API_KEY` line and `Settings()` read `''`; after it, `.env`
line 21 is `OPENROUTER_API_KEY=` and a dummy value set there loads as
`'test-key-73'`. The one step I did not run is `make test`, because it
needs the project's full virtualenv; the change is a single line in a
template file that no test reads.
