I have everything needed. Here is the structured research report.

---

# NaraRouter Research Report

## 1. What is NaraRouter?

**NaraRouter** is a unified AI model gateway/router operated out of Indonesia (bynara.id). It exposes 39–53+ AI models through a single API with both **OpenAI-compatible** and **Anthropic-compatible** endpoints. It is a *reseller/aggregator*, not an inference provider — it routes to backend models (Agnes, Tencent HY3, DeepSeek, Mistral, Qwen, OpenAI "Fams", Anthropic, etc.) behind one base URL and one API key.

- **Official website:** https://router.bynara.id/
- **Model directory:** https://router.bynara.id/ai-models
- **Docs:** https://router.bynara.id/docs
- **Signup URL:** https://router.bynara.id/register

**Trust caveat (verified by independent research, abdelilah-elaziri/free-llm-hub):** it lists "Claude Fams" family-plan access, several paid tiers show "Out of stock" (unusual for an API — suggests pooled accounts), model IDs are rebranded (e.g. `deepseek-v4-pro-0813-bynara`), and support is Telegram-only. Treat it as a **bonus free tier, never a sole dependency**. On the plus side, it sells fixed-price subscriptions (IDR/day), not per-token billing — so an out-of-tier model call simply *errors* rather than silently billing you. Structurally safe in that one respect.

## 2. Signup — step by step

1. Go to **https://router.bynara.id/register**
2. Sign up with **email + password** (min 8 chars) **or** continue with **Google**. No credit card required.
3. **Link your Telegram account** in the dashboard — one integration reference (omniroute) notes this is *required before `/v1` endpoints answer*. If calls 401/403 after signup, this is the likely cause.
4. Open the dashboard → **API Keys** → **Create key**.
5. Keys start with **`sk-nry-`** and are **shown only once** — copy it immediately. You can rotate/revoke from the dashboard later.
6. Verify the key works with curl:
```bash
curl -s -X POST https://router.bynara.id/v1/chat/completions \
  -H "Authorization: Bearer sk-nry-YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"agnes-2.5-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":10}'
```
7. List exactly which models your plan includes:
```bash
curl -s https://router.bynara.id/v1/models \
  -H "Authorization: Bearer sk-nry-YOUR_KEY"
```
(Not all 53 models are on every tier — always check before configuring.)

## 3. Free-tier models (verified by community testing)

| Model ID | Notes |
|---|---|
| `agnes-2.5-flash` | Best free default; fastest, reliable, vision-capable |
| `agnes-2.0-flash` | Free tier, older generation |
| `tencent-hy3` | Tencent HY3; strong reasoning default on OpenRouter too |
| Mistral Large (free listing) | Marked "Free" on the model directory |
| Kimi K2.7 Code Free | Free coding model |
| Poolside models | Listed at Rp0/$0 (free) |
| NVIDIA models | Listed at Rp0/$0 (free) |

Models that are **NOT** free (need top-up/credits): `deepseek-v4-pro-0813-bynara`, `deepseek-v4-flash-free` (name is misleading — not on free tier), Anthropic/OpenAI/DxAI flagship IDs.

Free-tier response times can be slow (5–20s) on shared infrastructure — normal.

## 4. App configuration (exact values)

| Field | Value |
|---|---|
| **Base URL** | `https://router.bynara.id/v1` |
| **API key header** | `Authorization: Bearer sk-nry-...` |
| **Chat endpoint** | `POST https://router.bynara.id/v1/chat/completions` (OpenAI format) |
| **Anthropic endpoint** | `POST https://router.bynara.id/v1/messages` (Anthropic Messages format) |
| **Responses endpoint** | `POST https://router.bynara.id/v1/responses` (OpenAI Responses API) |
| **Embeddings** | `POST https://router.bynara.id/v1/embeddings` |
| **Image generation** | `POST https://api-images.bynara.id/v1/images/generations` (different subdomain) |
| **Image editing** | `POST https://api-images.bynara.id/v1/images/edits` |
| **Model list** | `GET https://router.bynara.id/v1/models` |

Request body is standard OpenAI chat-completions: `{"model":"agnes-2.5-flash","messages":[...],"max_tokens":4096,"temperature":0.7}` → response at `choices[0].message.content`. For an Anthropic-style client: set `ANTHROPIC_BASE_URL=https://router.bynara.id/v1`, `ANTHROPIC_AUTH_TOKEN=<key>`.

## 5. Free-tier rate limits

- **~6M tokens/day** (community-verified live; marketing claims 7–10M — treat 6M as the real number), resets **07:00 WIB (00:00 UTC)**
- **10–15 requests/minute**
- **No credit card, free forever** on the free tier
- Paid tiers are fixed-price IDR/day subscriptions (Freemium / FreeMium Max / FreeMium Ultra) that unlock more models

## 6. Alternatives worth listing in the docs

1. **OpenRouter** — https://openrouter.ai — one key, `:free` model suffix (e.g. `tencent/hy3:free`, `qwen3-coder:free`, `nvidia/nemotron-3-ultra:free`). Base URL `https://openrouter.ai/api/v1`. Limits: 20 req/min, **50 req/day** (1,000/day after a one-time $10 credit purchase). No card for free tier. Free model set **rotates** — verify live. Good fallback router.
2. **Google Gemini API (AI Studio)** — https://aistudio.google.com/ — ~1,500 req/day free, generous token allowance, no card. Direct provider (more trustworthy than resellers).
3. **Groq** — https://console.groq.com/ — 30 RPM / ~14.4K req/day free, very fast inference (Llama, Qwen, Kimi K2). No card.
4. **Hugging Face Inference** — serverless, few-hundred req/hr free for <10B models. No card.
5. **Cerebras** — historically ~30 RPM free; recent reports say now a $5 trial behind a verified payment method — flag as uncertain.
6. **GitHub Models** — **retired July 30, 2026** — do not list as working.

**Recommended doc guidance:** NaraRouter as the primary free lane (generous 6M/day token budget suits an agent's heavy usage), OpenRouter `:free` as fallback lane, Gemini API as the trustworthy direct-provider lane. Warn users free tiers are dev-grade: rate caps, slower responses, and reseller risk make them unsuitable as a single dependency.

---

**Sources:** NaraRouter official site (router.bynara.id, /register, /ai-models — fetched live 2026-10-04); v4time/v4skill `nararouter-api.md`; inyogeshwar/claude-code-free `docs/providers/nararouter.md`; abdelilah-elaziri/free-llm-hub commit 9f2ad58 (live verification + trust analysis); YoannDev90/awesome-free-ai-api PR #16; mosgaragedev/omniroute PROVIDER_REFERENCE.md; mvalentsev/awesome-free-ai-coding (OpenRouter free, verified 2026-10-01); gost-co/modelatlas Appendix E.