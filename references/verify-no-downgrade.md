# Verify you're getting the real model (not a silent downgrade)

A relay controls what actually runs behind your request. It can return
`"model": "claude-opus-..."` in the response while a small model did the work.
The response format is identical, so you can't eyeball it. Test **behavior**.

## First: why "what model are you?" is useless
- A one-line system prompt ("You are Claude Opus") makes any model claim to be Opus.
- Real models also often get their own version wrong (no self-knowledge in training).
So identity must be inferred from **behavior fingerprints**, never self-report.

## Four manual probes (a coffee's worth of time)

### 1. Capability canary — a *set* of hard prompts, not one
Flagship vs. small models separate most reliably on **multi-constraint following**
and **long-chain reasoning**. Single prompts are noisy; run ~10 and compare pass rates.
- Multi-constraint example (swap the specifics so a relay can't hard-code a pass):
  "Write one English sentence of exactly 12 words that contains 'orbit' but not
  'planet' and ends in a question mark. Output only the sentence."
- Run the same set at `temperature=0` against a **trusted baseline** (official API,
  or a provider you trust) and against your relay. A flagship's pass rate collapses
  visibly when swapped for a small model. No baseline? A low absolute pass rate
  still tells you "this isn't flagship."

### 2. Context length — needle in a haystack
Truncation is the sneakiest downgrade: short chats feel fine, long docs fail.
```python
filler = "The weather is fine today. " * 15000
secret = "The passphrase is: violet-42."
prompt = filler[:len(filler)//2] + secret + filler[len(filler)//2:] \
         + "\n\nWhat passphrase appears above? Answer only the passphrase."
```
Bury the needle in the **middle** (catches both head- and tail-truncation). No
"violet-42" back → your long prompt never fully reached the model.

### 3. Consistency — temperature=0, run 10 times
At `t=0` the same input should give near-identical output. If a relay load-balances
across a **pool of different upstreams/models**, the 10 outputs drift — phrasing,
conclusions, even language. Eyeball them side by side.

### 4. Speed fingerprint — suspiciously fast is a symptom
Small models generate tokens noticeably faster than flagships. Ask for ~500 words,
time TTFT and total. **Much faster than the official model?** Suspect a swap before
you celebrate. Also compare the response's `usage` token count to your local
tokenizer — over-reported usage is billing inflation.

## Or automate it: LLMprobe (open source)
[LLMprobe](https://github.com/cocodot2026/LLMprobe) runs 6 probes (identity,
capability with an LLM judge, latency, context, rate-limit, consistency) against any
OpenAI-compatible endpoint and prints a 0–100 score. Key stays on your machine;
code is auditable.
```bash
pip install openai
python llmprobe.py --base-url https://<relay>/v1 --api-key <key> --model <model>
```
Use the **same ruler on every relay** — including the disclosed example (cocodot),
which the author measured at 87/100. The point is the method, not the number: run
it yourself.

## Reading a bad score
A low score isn't always downgrade. Once, a relay scored 15/100 because its
**streaming endpoint was down** and every probe timed out — not a downgrade. Check
*which* probe failed before you conclude. Bad and bad-in-what-way are different.
