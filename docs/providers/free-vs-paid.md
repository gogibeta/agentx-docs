# Free vs Paid

Every piece of the AgentX stack, with what costs money and what doesn't.

## Models

| Provider | Free | Paid |
|---|---|---|
| NaraRouter | ~6M tokens/day, 10–15 RPM, no card | Fixed-price IDR/day subscriptions unlock more models |
| OpenRouter | `:free` models, 50 req/day (1,000/day after $10 one-time credit) | Per-token billing |
| Google AI Studio | ~1,500 req/day, no card | Pay-as-you-go |
| Groq | 30 RPM / ~14.4K req/day, no card | Pay-as-you-go |

## Browser (cloud tunnel)

| Piece | Free | Paid |
|---|---|---|
| Cloudflare Workers | 100k req/day, no card | $5/mo → 10M req/mo |
| Cloudflare Durable Objects | 100k req/day + 13,000 GB-s/day | Usage-based |
| GitHub Actions (public repo) | Unlimited minutes | — |

The whole tunnel runs on free tiers. No tunnel-service signup, no payment method anywhere.

::: warning
Running the browser runner on GitHub Actions is against GitHub's ToS for production use. Use a **spare/dummy GitHub account** — never the account that builds AgentX.
:::

## Social (fxembed-worker)

| | Free | Paid |
|---|---|---|
| Cloudflare Workers | 100k req/day, no card | $5/mo |
| X/Twitter API key | **Not needed** — the worker runs logged-out | — |

No X API key, no signup, no tokens for normal use.

## Decision engines

| Engine | Free | Paid |
|---|---|---|
| Jev | Depends on the Jev provider/key you bring | Paid Jev keys for higher limits |
| Drex | Invite-based: https://drex.nace.ai/invite/kc6cz8pu | Paid tiers on Drex |

## The app itself

AgentX is free and open source ([github.com/gogibeta/AgentX](https://github.com/gogibeta/AgentX)). No account, no subscription, no ads.
