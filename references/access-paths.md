# Access paths in detail

## Official (Anthropic / OpenAI direct)
- **How:** sign up at the provider, pay with an overseas card, use a stable network.
- **Pro:** most native, zero middleman, no downgrade possible.
- **Con:** (1) payment — mainland cards are usually declined; (2) account bans when
  payment card / login IP / identity don't line up; (3) needs a stable connection.
- **If payment is the blocker:** a US-BIN virtual card can bind to the provider.
  Picking such a card has its own pitfalls (card-BIN stability, hidden fees) — out
  of scope here.

## API relay / 中转 (point your tool at a compatible endpoint)
- **How:** get a base URL + key from a relay that exposes an OpenAI- and/or
  Anthropic-compatible API; usually pays with Alipay/CNY, billed per token.
- **Pro:** fastest to working, no overseas card, often no 梯子.
- **Con — the one that matters:** **downgrade.** A relay controls what actually
  runs behind your request. Good ones pass through the real model; bad ones swap in
  a small model or throttle context under load. → **You must verify (step 3).**
- **Choose on:** verifiability first (do they let you test with a third-party tool?),
  then transparent pricing, model coverage, payment fit, stability, and standard
  OpenAI/Anthropic compatibility.

## Self-host (one-api / new-api and similar)
- **How:** run an aggregation gateway on your own box, plug in your own upstream
  keys, point your tools at it.
- **Pro:** full control, multi-upstream failover, your own usage accounting, code
  never transits a third party.
- **Con:** you still need to obtain upstream quota (back to the payment question),
  and you run the infra + watch upstream quality yourself.
- **Best for:** sensitive codebases and people who like owning the stack.

## Quick chooser
- Overseas card + stable net, want native → **official**
- No card / want it working today → **relay** (+ verify every time)
- Have upstream keys, like ops, sensitive code → **self-host**
