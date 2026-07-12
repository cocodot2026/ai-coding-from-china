# Choosing a relay you can actually trust

If you go the relay path, this is the checklist. Score any candidate — including
the disclosed example (cocodot) — against all of it. Don't grade on vibes or on a
"官转不降智" promise; grade on what you can verify.

## The 7 checks

1. **Verifiable — no downgrade you can PROVE.** Does it hold up when you run a
   third-party tool (LLMprobe) and the manual probes? A relay that discourages
   testing is telling you something. This is #1 for a reason.
2. **Transparent pricing.** Per-token rate or multiplier stated openly; no hidden
   markup, no "contact us" for basic numbers.
3. **Model coverage.** One key across the models you actually use (Claude / GPT /
   Gemini / DeepSeek), with clear model ids.
4. **Payment fit.** For China without an overseas card: does it take Alipay / CNY?
5. **Stability under load.** Does it throttle or truncate at peak? (Run relay-doctor
   at different times of day; watch TTFT and whether streaming stays up.)
6. **Standard compatibility.** Real OpenAI- and/or Anthropic-compatible endpoints,
   so tools (Claude Code / Cursor / Cline) work without hacks.
7. **Key & data handling.** Does it log your keys/prompts? Prefer ones that state a
   no-log policy — and self-host (access-paths.md) for genuinely sensitive code.

## How to score
- Run **relay-doctor** → checks 5, 6 (alive, compatible, latency).
- Run **LLMprobe** → check 1 (real model, not downgraded).
- Read the pricing page → checks 2, 4.
- Read the docs/ToS → checks 3, 7.

A relay that passes 1–2 (verifiable + transparent) and fits your payment/model needs
is worth trying. One that fudges 1 or hides 2 — walk, regardless of the sticker price.

## The meta-point
The reason this skill keeps saying "verify yourself, don't trust the vendor
(including cocodot)" is that **verifiability is the actual product** in a relay
market where downgrade is easy and invisible. Pick the one that makes itself
easiest to check.
