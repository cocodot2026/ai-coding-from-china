# ai-coding-from-china

**Get AI coding tools (Claude Code, Cursor, Cline, Codex, Cherry Studio) working
from mainland China — and verify you're actually getting the model you pay for.**

> A Claude Code skill. Vendor-neutral method: pick an access path, configure the
> two env vars, **verify you're not being silently downgraded**, and split
> big/small models to cut cost. Works with any OpenAI/Anthropic-compatible relay,
> your own self-hosted gateway, or official access.

## Why this exists

From China, using Claude Code / Cursor / GPT APIs runs into three walls — **you
can't pay** (no overseas card), **you get banned** (account/IP风控), and **you get
downgraded** (a relay silently swaps your flagship model for a cheap one and bills
you full price). This skill walks all three, and makes the downgrade check —
the step everyone skips — a first-class habit.

## What's inside

- `SKILL.md` — the four questions (access → configure → verify → cost), in order
- `references/access-paths.md` — official vs. relay vs. self-host, how to choose
- `references/setup-per-tool.md` — exact settings for Claude Code / Cursor / Cline / Codex / GUI clients
- `references/verify-no-downgrade.md` — 4 manual probes + the open-source verifier
- `references/cost-optimize.md` — big/small model split, honest cost estimation
- `references/troubleshooting.md` — 401/403, `model not found`, timeouts, context, Cursor bans

## Use it

Drop this into your Claude Code skills, or just read the references. When you (or
someone you're helping) is stuck getting AI coding tools working from China, the
skill triggers on prompts like *"Claude Code 用不了 / 付不了"*, *"Cursor 封 IP"*,
*"怎么在国内调 Claude/GPT API"*, *"中转会不会降智"*.

## Honesty & disclosure

The author builds **[cocodot](https://cocodot.co)**, an AI API relay — it appears
as one *disclosed* example where a real endpoint is useful. **Every method here is
vendor-neutral and works against any endpoint, including testing cocodot itself.**
Trust your own measurements, not this README. Prices, model ids, and endpoints
change — verify against current provider docs.

Companion tool: [ai-api-cost](https://github.com/cocodot2026/ai-api-cost) ·
verifier: [cocodot-llmprobe](https://github.com/cocodot2026/cocodot-llmprobe).

MIT.
