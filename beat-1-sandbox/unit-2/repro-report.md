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