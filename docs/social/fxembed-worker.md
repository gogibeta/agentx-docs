# Deploy fxembed-worker

**fxembed-worker** ([github.com/gogibeta/fxembed-worker](https://github.com/gogibeta/fxembed-worker), MIT) is a Cloudflare Worker that turns social-media posts into rich embeds + JSON APIs for agents. A fork of FxEmbed, with agent-focused additions:

- **`/ai` realm** — Markdown twins of every API route, optimized for LLM consumption.
- **`/llms.txt`** — a complete agent manual served live (this is what AgentX reads to learn the API).
- **X search without credentials** — keyword search works with zero API keys.
- Covers **X/Twitter, Bluesky, Mastodon, Threads, Instagram, TikTok**.

## Prerequisites

1. A **Cloudflare account** — https://dash.cloudflare.com/sign-up (free, no card).
2. **Node.js 24 LTS** — https://nodejs.org.
3. Terminal access (Command Prompt / PowerShell / Terminal).

## Deploy — step by step

**Step 1 — Log in to Cloudflare**
```bash
npm install -g wrangler
wrangler login
```
Approve the login in the browser window that opens.

**Step 2 — Get your Account ID**

Go to https://dash.cloudflare.com → click your account → copy the **Account ID** from the right sidebar (32-character hex string).

**Step 3 — Clone and install**
```bash
git clone https://github.com/gogibeta/fxembed-worker.git
cd fxembed-worker
npm install
```

**Step 4 — Configure**
```bash
cp wrangler.example.toml wrangler.toml
```
Open `wrangler.toml` and replace `[CLOUDFLARE_ACCOUNT_ID]` with your Account ID. You can change `name` — it becomes part of your URL.

```bash
cp .env.example .env
```
Defaults work out of the box.

**Step 5 — Build** (order matters)
```bash
npm run build:atmosphere
npm run build-local
```

**Step 6 — Deploy**
```bash
npm run deploy
```
Wrangler prints your live URL, e.g. `https://fixtweet.<your-account>.workers.dev`.

**Save this URL — it's what you paste into AgentX.**

## Verify

- Open `https://<your-worker-url>/llms.txt` — you should see the agent manual.
- Health check: `https://<your-worker-url>/twitter/version`.

## What's next

- [Connect to AgentX](/social/connect) — paste the URL in the app.
