# Free vs Paid — The Whole Stack

Every piece of the AgentX stack, what costs money and what doesn't. **Free tiers are marked 🟢, paid-only 🔴, freemium 🟡.**

> Figures are approximate as of October 2026 and change often. Confirm on official pricing pages before spending.

## Chat providers

| Provider | Free | Paid | Notes |
|---|---|---|---|
| Google (Gemini) 🟢 | ~1,500 req/day, no card | Pay-as-you-go via Cloud billing | Best free start |
| Groq 🟢 | ~30 RPM / ~14.4K req/day, no card | Pay-as-you-go for higher limits | Fastest free responses |
| OpenRouter 🟡 | `:free` models — 50 req/day (1,000/day after one-time $10 credit) | Per-token billing, one balance | Try hundreds of models |
| NaraRouter 🟢 | ~6M tokens/day free, no card | Fixed-price subscriptions for more | Extra free quota |
| Ollama 🟢 | Unlimited — your hardware | — | Local, private, offline |
| Local (on-device) 🟢 | Unlimited | — | Small models, phone RAM limits |
| DeepSeek 🟡 | Occasional promos | Very cheap per-token | Cheapest reasoning |
| Qwen 🟡 | Trial quotas | Cheap per-token | Strong multilingual |
| OpenAI 🔴 | None permanent | Per-token, premium pricing | Top quality, priciest |
| Anthropic 🔴 | None permanent | Per-token | Great reasoning/coding |
| OpenCode Go 🟡 | Depends on OpenCode plan | See opencode.ai | Reuse OpenCode access |

**Cheapest path to a working app:** Google + Groq + OpenRouter `:free` — genuinely $0/month for heavy daily use.

## Browser automation

### Backends (where the browser runs)

| Backend | Free | Paid |
|---|---|---|
| System WebView 🟢 | Always free — built into Android | — |
| Cloud Tunnel 🟢 | Free tiers cover it: Cloudflare Workers 100k req/day, Durable Objects 13,000 GB-s/day, GitHub Actions minutes on public repos | Cloudflare $5/mo if you exceed free tiers |

### Decision engines (who decides the clicks)

| Engine | Free | Paid |
|---|---|---|
| Jev (TypeSafe) 🟡 | Depends on the Jev key/endpoint you bring | Paid Jev keys for higher limits |
| Drex 🟡 | Invite-based access: https://drex.nace.ai/invite/kc6cz8pu | Paid tiers per Nace AI |
| Main model fallback 🟢 | Costs whatever your chat provider costs | — |

::: warning
Running the browser runner on GitHub Actions' free tier for 24/7 production workloads conflicts with GitHub's Terms of Service (Actions is for CI/CD, not servers). For always-on use, prefer a **self-hosted runner** on your own machine or a small paid VPS. A spare GitHub account only isolates risk for experiments — it doesn't make ToS-violating usage compliant.
:::

## Social (fxembed-worker)

| Piece | Free | Paid |
|---|---|---|
| Cloudflare Workers 🟢 | 100k req/day, no card | $5/mo → 10M req/mo |
| X/Twitter API key | **Not needed** — the worker runs logged-out | — |

No X API key, no signup, no tokens for normal use.

## The app itself

AgentX is **free and open source** ([github.com/gogibeta/AgentX](https://github.com/gogibeta/AgentX)). No account, no subscription, no ads — forever.

## Docs site hosting

| Piece | Free | Paid |
|---|---|---|
| Cloudflare Pages 🟢 | Unlimited static hosting on `*.pages.dev`, generous bandwidth | — |
| This docs site | $0 — built with VitePress, deployed via API | — |
