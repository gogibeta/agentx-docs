# All Providers Explained

AgentX ships with **built-in providers** plus **custom providers** for anything OpenAI-compatible. This page explains every one: what it is, where to get a key, what it costs, and what it's best for.

> Pricing and free tiers change often. Figures below are approximate as of October 2026 — always confirm on the provider's official pricing page before spending money.

## Built-in providers

These appear directly in **Settings → Providers**. Tap one, paste your API key(s), pick models, done. You can paste **multiple keys per provider** (one per line) — AgentX rotates between them automatically to spread load and dodge rate limits.

### Google (Gemini)

- **What:** Google's Gemini models (e.g. Gemini 2.x Flash/Pro) via Google AI Studio.
- **Get a key:** [aistudio.google.com](https://aistudio.google.com) → Get API key. Free Google account, no credit card.
- **Free tier:** Generous — roughly ~1,500 requests/day on Flash-class models. Enough for heavy daily use.
- **Paid:** Pay-as-you-go through Google Cloud billing once free limits are exceeded.
- **Best for:** The best free starting point. Fast, smart, multimodal (text + images + audio).

### OpenAI

- **What:** GPT models via `api.openai.com/v1`.
- **Get a key:** [platform.openai.com](https://platform.openai.com) → API keys. Requires a paid account with billing set up.
- **Free tier:** None permanent (new accounts occasionally get small trial credits).
- **Paid:** Per-token, pay-as-you-go. The most expensive mainstream option, but top-tier quality.
- **Best for:** Maximum model quality when cost doesn't matter; also the reference OpenAI-compatible API everything else imitates.

### Anthropic (Claude)

- **What:** Claude models via Anthropic's API.
- **Get a key:** [console.anthropic.com](https://console.anthropic.com). Requires billing.
- **Free tier:** None permanent (occasional promotional credits).
- **Paid:** Per-token, pay-as-you-go. Generally cheaper than OpenAI for similar quality.
- **Best for:** Long-context reasoning, careful instruction-following, coding agents.

::: tip Heads-up
If Claude fails inside AgentX but works elsewhere, double-check the key saved **in the app's provider settings** — it's a common gotcha to test one key and paste a different one into the app.
:::

### DeepSeek

- **What:** DeepSeek's own API (`api.deepseek.com`) — the open-weight reasoning models as a hosted service.
- **Get a key:** [platform.deepseek.com](https://platform.deepseek.com).
- **Free tier:** Occasional promotions; normally paid but extremely cheap.
- **Paid:** Per-token — among the cheapest reasoning-capable models available.
- **Best for:** Cheap deep reasoning and coding on a budget.

### Qwen (Alibaba)

- **What:** Alibaba's Qwen models via DashScope international (`dashscope-intl.aliyuncs.com/compatible-mode/v1`, OpenAI-compatible).
- **Get a key:** Alibaba Cloud DashScope console.
- **Free tier:** Trial quotas for new accounts.
- **Paid:** Per-token, competitively priced.
- **Best for:** Strong multilingual (especially Chinese/English) models at low cost.

### Groq

- **What:** Ultra-fast inference (LLaMA, Qwen, Mixtral and more) on Groq's LPUs via `api.groq.com/openai/v1`.
- **Get a key:** [console.groq.com](https://console.groq.com). Free account, no credit card for the free tier.
- **Free tier:** Generous — roughly 30 requests/min and ~14,400 requests/day on popular models.
- **Paid:** Pay-as-you-go for higher limits.
- **Best for:** The fastest free responses anywhere. Great default for quick agents and browser narration.

### Ollama (local)

- **What:** Models running on your own computer via [Ollama](https://ollama.ai) (`http://localhost:11434/v1`).
- **Get a key:** None needed — just run Ollama on a PC on the same network and point AgentX at it.
- **Free tier:** Completely free — your hardware is the limit.
- **Paid:** Never (just your electricity).
- **Best for:** Privacy (nothing leaves your network), offline use, unlimited experimentation.

### OpenRouter

- **What:** One API for hundreds of models (`openrouter.ai/api/v1`) — OpenAI, Anthropic, Google, Meta, DeepSeek and more behind a single key.
- **Get a key:** [openrouter.ai](https://openrouter.ai).
- **Free tier:** Models tagged `:free` are free — ~50 requests/day, rising to ~1,000/day after a one-time $10 credit purchase.
- **Paid:** Per-token billing across all models with one balance.
- **Best for:** Trying many different models with one key; free-tier experimentation.

### OpenCode Go

- **What:** OpenCode's Zen model gateway (`opencode.ai/zen/go/v1`, OpenAI-compatible).
- **Get a key:** From your OpenCode account — see [opencode.ai](https://opencode.ai) for current plans.
- **Best for:** If you already use OpenCode, reuse the same model access inside AgentX.

### Local (on-device)

- **What:** Models running directly on the phone.
- **Get a key:** None.
- **Cost:** Free.
- **Best for:** Offline quick tasks. Limited by phone RAM — small models only.

### TypeSafe (Jev decision engine)

- **What:** Not a chat provider — this is the **Jev ultrafast decision engine** used for browser automation. It lives on its own settings page (**Settings → Jev**), not in the providers list.
- **Get a key:** From your Jev provider; paste the base URL and model name yourself.
- **Cost:** Depends on the Jev key you bring.
- **Best for:** Fast browser clicking — ~1–2 seconds per action instead of a full model round-trip. See [Jev](/decision-engines/jev).

## Custom providers (bring anything)

Any service that speaks the **OpenAI-compatible** chat API can be added as a custom provider: **Settings → Providers → Add custom**. You choose the protocol:

| Protocol | Use for |
|---|---|
| OpenAI | NaraRouter, OpenRouter-compatible gateways, local servers (LM Studio, llama.cpp server), any `/v1/chat/completions` endpoint |
| Google | Gemini-compatible endpoints |
| Anthropic | Claude-compatible endpoints |

Popular custom setups:

- **NaraRouter** — free API keys for popular models. Full walkthrough: [Free API Keys (NaraRouter)](/providers/nararouter).
- **Your own gateway** — e.g. a Cloudflare Worker that shims tools or routes between keys.

## Which provider should you start with?

<div class="rec-box">

**Recommended free starter stack:**
1. **Google (Gemini)** — your main smart model, generous free tier.
2. **Groq** — your fast model for quick tasks and agent loops.
3. **OpenRouter `:free` models** — for trying anything else at zero cost.
4. **NaraRouter** — extra free keys when you need more quota.

**When to pay:** when you consistently hit free limits, or you want frontier quality (Claude/GPT) for important work. Start free, pay only for what the free tiers can't cover.

</div>

## Next steps

- [Free API Keys (NaraRouter)](/providers/nararouter) — step-by-step free key setup
- [Add a Provider](/providers/add-provider) — exact fields and what they mean
- [Choose Models](/providers/models) — picking and switching models
- [Free vs Paid](/providers/free-vs-paid) — every cost in the whole stack, compared
