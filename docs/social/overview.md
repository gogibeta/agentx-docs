# Social Media — Overview

AgentX talks to social networks through **your own** Cloudflare Worker — not the developer's. You deploy it, you own it, your keys stay yours.

**Supported:** X/Twitter, Bluesky, Mastodon/ActivityPub, Threads, Instagram, TikTok.

## How it works

```
AgentX app ──▶ your fxembed-worker ──▶ X / Bluesky / Mastodon / …
                (your Cloudflare account)
```

The worker turns posts into clean JSON + Markdown for the agent. No X API key needed — it runs logged-out.

## Setup path

1. [Deploy fxembed-worker](/social/fxembed-worker) — ~10 minutes, free Cloudflare tier.
2. [Connect to AgentX](/social/connect) — paste your worker URL in the app.

## Cost

**Free.** Cloudflare Workers free tier (100k req/day) is plenty for personal use — responses are edge-cached, so repeated queries often don't even hit the Worker. No X API key, no signup, no tokens.
