---
name: ai-coding-from-china
description: >-
  Set up and verify AI coding tools (Claude Code, Cursor, Cline, Codex,
  Cherry Studio) from mainland China — pick an access path, configure the two
  env vars, verify you're getting the real model (not a silent downgrade), split
  big/small models to cut cost, and fix the common connection errors. Use when a
  developer in China says "Claude Code 用不了 / 付不了", "Cursor 封 IP", "怎么在国内调
  Claude/GPT API", "中转会不会降智", or is configuring an OpenAI/Anthropic-compatible
  relay. Vendor-neutral method; verify everything yourself.
---

# AI coding from China

Getting Claude Code / Cursor / Cline / Codex working from mainland China comes
down to four questions, in order. This skill walks each one, tool-agnostic. It
uses **cocodot** (an AI API relay, disclosed: the author's product) as a concrete
example where a real endpoint helps — but every step works with **any**
OpenAI/Anthropic-compatible relay, your own self-hosted gateway, or official
access. **The whole point is that you verify it yourself; don't trust any vendor,
including the example.**

## The four questions

1. **How do I get access?** (official vs. relay vs. self-host) → decide the path
2. **How do I point my tool at it?** → configure (usually two env vars)
3. **Am I getting the real model?** → verify no silent downgrade
4. **How do I not overpay?** → split big/small models

Work them in order. Most people only need 2 and 3.

---

## 1. Pick an access path

| Path | Pay with | Needs 梯子? | Downgrade risk | Who it's for |
|---|---|---|---|---|
| **Official** (Anthropic/OpenAI direct) | overseas card | yes | none | has a working overseas card + stable network |
| **API relay / 中转** | often Alipay/CNY | usually no | **yes — must verify** | no overseas card, wants it working fast |
| **Self-host** (one-api / new-api) | your own upstream keys | depends | depends on upstream | has upstream keys, runs infra |

**Decision rule:** overseas card + stable network → official. Otherwise → relay
(and do step 3 religiously). Already have upstream keys and like ops → self-host.

Full path details: `references/access-paths.md`.

---

## 2. Configure your tool (the two env vars)

Almost every tool reads a **base URL** + **key**. Point them at your chosen
endpoint and you're done.

**Claude Code** (reads `ANTHROPIC_*`):
```bash
export ANTHROPIC_BASE_URL="https://<your-relay>/api/ai"   # Anthropic-compatible endpoint
export ANTHROPIC_AUTH_TOKEN="<your-key>"
# optional: split models to save money (see step 4)
export ANTHROPIC_MODEL="<big-model-id>"
export ANTHROPIC_SMALL_FAST_MODEL="<small-model-id>"
claude
```

**Cursor / Cline / Cherry Studio / Codex** (read an OpenAI-compatible base URL):
```
Base URL: https://<your-relay>/v1        # OpenAI-compatible
API Key:  <your-key>
Model:    <the model id your relay exposes>
```

Per-tool exact fields (where the setting lives in each app): `references/setup-per-tool.md`.

> Concrete example (cocodot, disclosed — author's product): Anthropic endpoint
> `https://cocodot.co/api/ai`, OpenAI endpoint `https://cocodot.co/api/ai/v1`,
> model ids like `mco-6` (Opus), `mcs-5` (Sonnet), `mch-1` (Haiku). Swap in any
> other relay's values identically — nothing here is cocodot-specific.

---

## 3. Verify you're getting the real model (do NOT skip)

This is the step almost everyone skips and later regrets. A cheap or overloaded
relay can silently route your "Opus" request to a small model, or truncate your
context — you pay flagship price for budget output. **Never trust a "no
downgrade" promise; test it.**

Fast manual checks (no tools needed) and the automated open-source verifier
(`LLMprobe`) are both in `references/verify-no-downgrade.md`. The short version:

- **Don't** ask the model "what model are you?" — a one-line system prompt can
  make anything claim to be Opus. Test **behavior**, not self-report.
- **Do** run a fixed set of hard, constraint-following prompts at `temperature=0`
  against both a trusted baseline and your relay, and compare pass rates.
- **Do** bury a "needle" in a long context to catch silent truncation.
- Or just run `LLMprobe` (open source, key stays local) for a 0–100 score across
  6 probes: identity, capability (LLM-judged), latency, context, rate-limit,
  consistency.

Pick a relay only if it lets you verify — and if it holds up when you do.

---

## 4. Don't overpay: split big and small models

Coding agents fire a lot of cheap calls (autocomplete, first-pass edits, summaries)
and a few expensive ones (planning, hard debugging). Route them:

- Claude Code: `ANTHROPIC_MODEL` = a flagship for hard work,
  `ANTHROPIC_SMALL_FAST_MODEL` = a small model for the rest — it auto-splits.
- Pay-as-you-go beats a flat subscription for most solo/低频 usage; estimate before
  you commit. Method + a token-cost estimate approach: `references/cost-optimize.md`.

---

## Troubleshooting the usual errors

`401/403` (key/endpoint), `model not found` (id mismatch), timeouts/`stream`
failures, `context length` errors, Cursor account bans — causes and fixes in
`references/troubleshooting.md`.

---

## Honesty & disclosure (read this)

- The author builds **cocodot**, a relay; it appears as a disclosed example. Every
  method here is vendor-neutral and works against any endpoint — **including
  testing cocodot itself**. Trust your own measurements, not this doc.
- Don't paste real production keys into unknown relays; prefer self-host for
  sensitive code (step 1).
- Prices, model ids, and endpoints change — verify against each provider's current
  docs before relying on a number.
