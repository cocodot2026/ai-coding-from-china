# Don't overpay

## Split big and small models
Coding agents make many cheap calls (autocomplete, first-pass edits, summaries,
file reads) and a few expensive ones (planning, hard debugging). Route accordingly:

- **Claude Code:** `ANTHROPIC_MODEL` = flagship (hard work),
  `ANTHROPIC_SMALL_FAST_MODEL` = small/fast model (everything else). Claude Code
  auto-routes between them, so most of your token volume runs at the cheap rate.
- **Cursor / Cline:** pick the cheaper model as your default and switch up to a
  flagship only for the hard prompt.

## Pay-as-you-go vs. subscription
For solo / lower-frequency use, per-token billing usually beats a flat monthly
subscription — you only pay for what you run. Estimate before you commit to a plan.

## Estimate cost honestly (no guessed prices)
Cost = **tokens × rate**. Don't trust a memorized price — pull the current per-token
rate from your provider's pricing page, then:

```
estimated_cost = (input_tokens/1e6 * input_rate_per_M)
               + (output_tokens/1e6 * output_rate_per_M)
```

Count tokens locally so you're not guessing volume either. A ready-made estimator
is the companion tool **ai-api-cost** (counts tokens for your real prompts and
multiplies by a rate you supply): https://github.com/cocodot2026/ai-api-cost

## Sanity-check what you're billed
After a session, compare the provider's reported usage to your own token count
(the estimator prints both). Persistent over-reporting = billing inflation; treat
it like a downgrade signal and re-verify (see verify-no-downgrade.md).
