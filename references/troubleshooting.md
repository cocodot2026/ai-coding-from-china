# Troubleshooting the usual errors

## 401 / 403 Unauthorized
- Wrong key, or key pasted with whitespace. Re-copy.
- Wrong endpoint for the tool: Claude Code wants the **Anthropic-compatible** base
  URL (`ANTHROPIC_BASE_URL`); Cursor/Cline/Codex want the **OpenAI-compatible** one
  (usually `/v1`). Mixing them → 401/403 or 404.
- Key not activated / no balance on the relay.

## `model not found` / 404 on model
- The **model id** is relay-specific. Use the exact id your relay exposes, not the
  official name (e.g. a relay may call Opus `mco-6`, not `claude-opus-4`).
- The tool sent a default id your relay doesn't map. Set the model explicitly.

## Timeouts / streaming fails / truncated output
- The relay's **streaming** endpoint may be flaky — some tools let you disable
  streaming; try non-streaming to isolate it.
- Under load a relay may throttle. If it only happens at peak, that's a quality
  signal — re-run the verify probes (verify-no-downgrade.md).
- Network path unstable → try with/without 梯子 depending on your path.

## `context length exceeded`
- Your prompt + history exceeds the model's window, **or** the relay advertises a
  bigger window than it actually serves. Trim history; if a modest context still
  errors, run the needle-in-haystack probe — the relay may be under-serving context.

## Cursor account banned / IP blocked
- Cursor bans usually target the **account/environment** (shared IP pool, payment
  vs. login mismatch, one account across many devices), not you personally.
- Switching editors alone doesn't fix it if the environment is unchanged. Decoupling
  editor from model — run the model via an API key in Cline / Claude Code instead of
  a Cursor subscription — sidesteps subscription-account bans (there's no account to
  ban; an API key isn't a login).

## "It works but feels dumber than yesterday"
- Not always in your head. Run the verify probes. Peak-hour downgrade/throttling is
  real on cheap relays. If it reproduces, switch relays — and pick one you can verify.
