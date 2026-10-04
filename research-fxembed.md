Research complete. Here is the full structured setup guide:

---

# fxembed-worker — Complete Setup Guide

## 1. What is it?

**fxembed-worker** (https://github.com/gogibeta/fxembed-worker, MIT license) is a fork of [FxEmbed](https://github.com/FxEmbed/FxEmbed) — a Cloudflare Worker that turns social-media posts into rich embeds plus JSON APIs. It covers **X/Twitter, Bluesky, Mastodon/ActivityPub, Threads, Instagram, and TikTok**.

What this fork adds over upstream:
- **`/ai` realm** — Markdown twins of every API route, optimized for LLM/agent consumption (`src/realms/ai/`)
- **`/llms.txt`** — a complete agent manual served live from the worker (this is what AgentX reads to learn the API)
- **X search relay** — brings back X keyword search *without any credentials* (RSS discovery + per-post hydration via `packages/atmosphere/src/providers/twitter/rssSearch.ts`)
- Ours-only per-user search: `GET /api/2/profile/{handle}/search`
- Bluesky `api.bsky.app` retry (public AppView edge-403s search from datacenter IPs)

Live reference deployment: `https://fixtweet.twapiworker.workers.dev` · live agent guide: `https://fixtweet.twapiworker.workers.dev/llms.txt` · test console: `https://fxembed-console.pages.dev`

**What it does NOT need:** no X API key, no signup, no tokens. Everything runs logged-out by default. (A small set of routes — X followers/following/media tabs/typeahead/trends, IG/Threads proxy routes — need an optional credential pool; they answer 404/500/501 by design without one.)

## 2. Prerequisites (non-technical checklist)

1. A **Cloudflare account** — free, sign up at https://dash.cloudflare.com/sign-up (no credit card required for the free tier)
2. **Node.js 24 LTS** installed on your computer — download from https://nodejs.org (pick the LTS version)
3. **Wrangler** (Cloudflare's CLI) — installed via npm in step 3 below

## 3. Deploy — exact step by step

Open a terminal (Command Prompt / PowerShell on Windows, Terminal on Mac/Linux) and run:

**Step 1 — Log in to Cloudflare via Wrangler**
```
npm install -g wrangler
wrangler login
```
This opens a browser window; approve the Cloudflare login.

**Step 2 — Get your Cloudflare Account ID**
Go to https://dash.cloudflare.com → click your domain/account → on the right sidebar find **Account ID** → copy it. (It's a 32-character hex string.)

**Step 3 — Clone and install**
```
git clone https://github.com/gogibeta/fxembed-worker.git
cd fxembed-worker
npm install
```

**Step 4 — Create `wrangler.toml`**
```
cp wrangler.example.toml wrangler.toml
```
Open `wrangler.toml` in a text editor and replace `[CLOUDFLARE_ACCOUNT_ID]` with your Account ID from Step 2. The file looks like:
```toml
name = "fixtweet"
account_id = "PASTE_YOUR_ACCOUNT_ID_HERE"
main = "./dist/worker.js"
compatibility_date = "2026-04-11"
send_metrics = false
```
(You can change `name` to anything — it becomes part of your URL.)

**Step 5 — Create `.env`**
```
cp .env.example .env
```
Defaults work out of the box. The important keys and their defaults:
| Variable | Default | What it does |
|---|---|---|
| `API_HOST_LIST` | `api.fxtwitter.com,api-canary.fxtwitter.com` | Hosts serving the JSON API realm |
| `TWITTER_ROOT` | `https://x.com` | Canonical X origin |
| `X_SEARCH_RELAY_ROOT` | `https://nitter.kareem.one` | Volunteer RSS relay for X search (override with your own Nitter to self-host) |
| `SENTRY_DSN` | *(empty = off)* | Error reporting, optional |

**Step 6 — Build (order matters: atmosphere first, then bundle)**
```
npm run build:atmosphere
npm run build-local
```

**Step 7 — Deploy**
```
npm run deploy
```
Wrangler prints your live URL, e.g.:
```
https://fixtweet.<your-account>.workers.dev
```
**Save this URL — it's what you paste into AgentX.**

**Verify it works:** open `https://<your-worker-url>/llms.txt` in a browser — you should see the agent manual. Also try `https://<your-worker-url>/twitter/version` as a health check.

**Live logs (optional):** `npx wrangler tail fixtweet --format json`

## 4. Connect it to the AgentX app

In the AgentX app, go to the **social / web-search settings** (the settings page that asks for your own social worker URL — the app does not use the developer's test setup).

- **Field:** your social worker URL
- **Value to paste:** your deployed worker base URL, e.g. `https://fixtweet.<your-account>.workers.dev`
- **No trailing path needed** — the app appends `/llms.txt` itself to learn the API, and calls `/api/2/...`, `/ai/2/...` etc. from there.
- **No API key to paste** — the worker needs none.

Two behaviors to know about (from the worker's own manual):
- API calls (`/api`, `/ai`) **require a `User-Agent` header** — the app already sends one; missing UA returns HTTP 401.
- Embed HTML routes need a *bot* User-Agent (`Discordbot/2.0` etc.); with a browser UA they 302-redirect to the original site instead. This only matters for raw embed testing, not the app.

## 5. API keys / secrets needed

- **None for normal use.** The worker runs fully logged-out; X search works via the relay with zero credentials.
- **Optional only:** a credential pool (`credentials.enc.json`, or `wrangler secret put CREDENTIAL_KEY`) unlocks pool-gated routes (X media/followers/following/typeahead/trends; Instagram/Threads proxy-only routes). Skip this unless you need those specific routes — the build works fine with an empty pool.
- **Cloudflare auth** is via `wrangler login` (OAuth) — no tokens to copy/paste.

## 6. Free vs paid (Cloudflare Workers tiers)

| | Workers Free | Workers Paid |
|---|---|---|
| Cost | **$0, no credit card** | $5/month |
| Requests | 100,000/day | 10M/month included, then $0.30/M |
| CPU per request | 10 ms | 30 s default (up to 5 min) |
| Memory | 128 MB | 128 MB |

**For personal AgentX use, the free tier is plenty:** the worker is read-heavy and responses are edge-cached (single posts/profiles/search ~30 days; threads/timelines ~7 days), so repeated queries often don't even hit the Worker. 100k requests/day is far above what one user's social searches will consume. Upgrade only if you share the worker publicly and hit the daily cap.

---

**Sources:** repo README (Setup/Architecture/Endpoint matrix sections), `wrangler.example.toml`, `.env.example`, `llms.txt` (sections A–N), Cloudflare Workers limits/pricing docs (verified 2026-10-04: 100k req/day, 10ms CPU free tier).