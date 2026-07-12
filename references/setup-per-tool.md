# Per-tool setup (where the setting lives)

All of these want the same two things: a **base URL** and a **key**. Only the
location differs.

## Claude Code (CLI)
Reads environment variables:
```bash
export ANTHROPIC_BASE_URL="https://<relay>/api/ai"   # Anthropic-compatible
export ANTHROPIC_AUTH_TOKEN="<key>"
export ANTHROPIC_MODEL="<big-model-id>"              # optional
export ANTHROPIC_SMALL_FAST_MODEL="<small-model-id>" # optional
claude
```
Put the exports in your shell profile (`~/.zshrc` / `~/.bashrc`) to persist.

## Cursor
Settings → **Models** → enable "Override OpenAI Base URL" (or the OpenAI/Anthropic
API-key section):
```
Base URL: https://<relay>/v1
API Key:  <key>
Model:    <the exact model id your relay exposes>
```
Add a custom model id if Cursor doesn't list yours. Then click **Verify**.

## Cline (VS Code extension)
Cline settings → API Provider → **OpenAI Compatible**:
```
Base URL: https://<relay>/v1
API Key:  <key>
Model ID: <model id>
```

## Codex CLI / other OpenAI-SDK tools
Point the SDK at the base URL:
```bash
export OPENAI_BASE_URL="https://<relay>/v1"
export OPENAI_API_KEY="<key>"
```

## Cherry Studio / Chatbox / LobeChat (GUI clients)
Settings → Model Provider → **OpenAI** (or "custom") → set **API Host / Base URL**
to `https://<relay>/v1` and paste the key. Pick the model from the list or add it.

## Notes
- OpenAI-compatible endpoints almost always end in `/v1`. Anthropic-compatible
  endpoints for Claude Code usually do **not** — check your relay's docs.
- The **model id** is relay-specific. Using the wrong id → `model not found`
  (see troubleshooting).
- Example ids for the disclosed cocodot endpoint: `mco-6` / `mcs-5` / `mch-1`;
  yours will differ.
