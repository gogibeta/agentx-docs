# Free API Keys (NaraRouter)

NaraRouter is a unified AI model gateway — one API key gives you access to 40+ models through a single OpenAI-compatible endpoint. Its free tier is generous enough to run an agent on.

- **Website:** https://router.bynara.id/
- **Sign up:** https://router.bynara.id/register
- **Model list:** https://router.bynara.id/ai-models
- **Docs:** https://router.bynara.id/docs

## Sign up — step by step

1. Go to **https://router.bynara.id/register**.
2. Sign up with **email + password** (min 8 characters) or **Continue with Google**. No credit card required.
3. **Link your Telegram account** in the dashboard — this is required before the `/v1` endpoints will answer. If your calls return 401/403 right after signup, this is almost always the cause.
4. Open the dashboard → **API Keys** → **Create key**.
5. Keys start with **`sk-nry-`** and are **shown only once** — copy it immediately. You can rotate or revoke it later from the dashboard.

## Test your key

```bash
curl -s -X POST https://router.bynara.id/v1/chat/completions \
  -H "Authorization: Bearer sk-nry-YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"agnes-2.5-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":10}'
```

A working key returns a JSON reply. To see exactly which models your plan includes:

```bash
curl -s https://router.bynara.id/v1/models \
  -H "Authorization: Bearer sk-nry-YOUR_KEY"
```

## Free-tier models

| Model ID | Notes |
|---|---|
| `agnes-2.5-flash` | Best free default — fastest, reliable, vision-capable |
| `agnes-2.0-flash` | Free tier, older generation |
| `tencent-hy3` | Strong reasoning model |
| Mistral Large (free listing) | Marked "Free" in the model directory |
| Kimi K2.7 Code Free | Free coding model |
| Poolside / NVIDIA models | Listed at Rp0/$0 (free) |

**Not free** (need credits): `deepseek-v4-pro-0813-bynara`, Anthropic/OpenAI flagship IDs. Note: `deepseek-v4-flash-free` has a misleading name — it's not on the free tier.

## Free-tier limits

- **~6M tokens/day**, resets 07:00 WIB (00:00 UTC)
- **10–15 requests/minute**
- Free responses can take 5–20s on shared infrastructure — normal.

## Values to paste into AgentX

| Field | Value |
|---|---|
| Base URL | `https://router.bynara.id/v1` |
| API key | Your `sk-nry-…` key |

The app uses the standard OpenAI chat-completions format, which NaraRouter speaks natively.

## Alternatives

- **OpenRouter** ([openrouter.ai](https://openrouter.ai)) — one key, free models with the `:free` suffix (e.g. `tencent/hy3:free`). Base URL `https://openrouter.ai/api/v1`. 20 req/min, 50 req/day free (1,000/day after a one-time $10 credit purchase). Good fallback lane.
- **Google AI Studio** ([aistudio.google.com](https://aistudio.google.com/)) — ~1,500 req/day free, direct provider, no card. The most trustworthy free option.
- **Groq** ([console.groq.com](https://console.groq.com/)) — 30 RPM / ~14.4K req/day free, very fast inference. No card.

::: warning Treat resellers as a bonus, not your only key
NaraRouter is an aggregator, not an inference provider. Keep a second provider (e.g. Google AI Studio) configured so one outage doesn't strand you.
:::

## What's next

- [Add a Provider](/providers/add-provider) — where the base URL and key go in the app.
- [Choose Models](/providers/models) — which model for which job.
