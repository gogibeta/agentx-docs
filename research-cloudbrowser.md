I have everything needed. Here is the complete research report.

---

# agentx-cloud-browser — Complete Setup Guide (research findings)

## 1. What is it?

**Repo:** https://github.com/polironi10/agentx-cloud-browser (public, 70 commits, created 2026-09-30, self-described as EXPERIMENTAL PoC)

It gives the AgentX Android app a **cloud-hosted headless Chromium browser** reachable at **one stable URL**, with no third-party tunnel service (ngrok, cloudflared, pinggy, bore, tunnelmole, localhost.run were all tried and dropped).

**How it works (plain language):**
- A **GitHub Actions runner** (on a spare/dummy GitHub account) launches headless Chromium with remote debugging on `127.0.0.1:9222`.
- A tiny Python script (`runner/relay.py`) on that runner opens **one outbound WebSocket** to a **Cloudflare Worker**.
- The Worker hands everything to a **Durable Object** singleton named `browser-main` (`BrowserRelay` class), which relays Chrome DevTools Protocol (CDP) traffic between the app and the runner.
- The phone app talks only to the Worker URL, authenticating with a **client token**. GitHub kills jobs before 6 hours, so each run **dispatches its own successor** via the GitHub API at ~5h50m; the new runner's uplink replaces the old one in the DO with ~5 min overlap. The Actions page shows 42 consecutive runs, each ~5h50m — the chain is live and self-sustaining.

**Traffic diagram (from README):**
```
Phone/app ──https/wss + CLIENT_TOKEN──▶ Cloudflare Worker (stable URL)
                                              │  Durable Object binding
                                              ▼
                              BrowserRelay DO (singleton "browser-main")
                                              ▲  one outbound WSS (/relay?secret=RELAY_SECRET)
                      GitHub runner: relay.py ─┴─▶ Chromium --headless (CDP 127.0.0.1:9222)
```

**Repo layout:**
| Path | Purpose |
|---|---|
| `.github/workflows/browser.yml` | The runner workflow: starts Chromium, connects `relay.py`, self-chains a successor |
| `runner/relay.py` | Bridges local CDP (`127.0.0.1:9222`) to the DO over one outbound WSS; auto-reconnects with backoff |
| `worker/worker.js` | The Worker + `BrowserRelay` Durable Object (CDP HTTP `/json/*` + WS `/devtools/*` relay, token gating) |
| `worker/deploy-worker.py` | Deploys the Worker via Cloudflare API (ES-module upload + DO binding with `new_sqlite_classes` migration, then sets secrets) |
| `.heartbeat` | Touched by every run to keep the repo active |

## 2. The secret/token system (exact names)

There are exactly **two secrets**, used in two places:

| Secret | Where it lives | What it guards |
|---|---|---|
| `RELAY_SECRET` | (a) Cloudflare Worker secrets, (b) GitHub repo → Settings → Secrets → Actions | The runner's uplink: `wss://<worker>/relay?secret=RELAY_SECRET`. Must be identical in both places. |
| `CLIENT_TOKEN` | (a) Cloudflare Worker secrets, (b) pasted into the AgentX app's tunnel settings | Every client request: `?token=CLIENT_TOKEN` on `/json/*` and `/devtools/*` WebSockets. This is the "client credential" the app user holds. |
| `WORKER_URL` | GitHub repo → Settings → Secrets → Actions (only) | The Worker's public URL, e.g. `https://agentx-browser.<subdomain>.workers.dev` — the workflow uses it for its health check. |

**Worker endpoints (from `worker.js`):**
- `GET /health` → `{"ok":true,"ts":...}` — public, no token; `ok` is true only while a runner uplink is connected.
- `GET /json/version?token=CLIENT_TOKEN`, `PUT /json/new?token=...`, `GET /json/list?token=...` — CDP HTTP, proxied to Chromium; Chromium's `ws://127.0.0.1:9222` URLs are rewritten to `wss://<worker-host>` so the app can open them with the token.
- `WSS /devtools/...?token=CLIENT_TOKEN` — CDP WebSocket sessions, piped through the runner.
- `WSS /relay?secret=RELAY_SECRET` — runner uplink only; a successor runner connecting here replaces the old uplink and the DO 1001-closes old client sockets (clients reconnect).

## 3. Step-by-step setup (non-technical order)

### Step 0 — What you need (all free)
1. A **Cloudflare account** (free tier is enough) — https://dash.cloudflare.com/sign-up
2. A **spare/dummy GitHub account** — ⚠️ the README is explicit: this pattern is against GitHub Actions ToS for production use. **Never run it on the account that builds/releases AgentX.** Public repos get unlimited Actions minutes, which is why a throwaway account works.
3. The repo on that dummy account: fork or push https://github.com/polironi10/agentx-cloud-browser to it.

### Step 1 — Generate the two secrets
Pick two long random strings (e.g. from a password generator, 32+ characters):
- `RELAY_SECRET` — runner ↔ Worker shared secret
- `CLIENT_TOKEN` — the token you will later paste into the AgentX app
Write them down; you will enter each in two places.

### Step 2 — Deploy the Cloudflare Worker
1. Log in at https://dash.cloudflare.com and open **Workers & Pages** once — this creates your `workers.dev` subdomain (the deploy script returns no URL until it exists).
2. Create a Cloudflare **API token** at https://dash.cloudflare.com/profile/api-tokens (needs permission to edit Workers scripts and read the account). The repo's `deploy-worker.py` was written for an agent credential store; a human replaces that part with their own token (or use `wrangler` with an equivalent `wrangler.toml`: script name `agentx-browser`, DO binding `RELAY` → class `BrowserRelay`, migration `new_sqlite_classes: ["BrowserRelay"]` — required on the free plan for the SQLite backend).
3. What the deploy does (via `PUT /accounts/{id}/workers/scripts/agentx-browser` as a multipart ES-module upload, then `PUT .../secrets`):
   - uploads `worker.js` with the `RELAY` Durable Object namespace binding,
   - sets the `RELAY_SECRET` and `CLIENT_TOKEN` secrets on the Worker,
   - prints your **Worker URL**: `https://agentx-browser.<your-subdomain>.workers.dev`
4. Save that URL — it never changes, even as runners come and go.

### Step 3 — Set the GitHub repo secrets
On the dummy account's repo: **Settings → Secrets and variables → Actions → New repository secret**, add:
- `WORKER_URL` = the URL from Step 2
- `RELAY_SECRET` = the exact same value you gave the Worker
(The workflow also uses the automatic `GITHUB_TOKEN`; its needed permissions `contents: write` (heartbeat commit) and `actions: write` (self-dispatch) are declared in the YAML.)

### Step 4 — Start the runner (once — then it self-chains)
1. Go to **Actions → cloud-browser → Run workflow → Run workflow** (the workflow triggers only on `workflow_dispatch`).
2. It will: start headless Chromium (`--headless=new --no-sandbox --disable-gpu --remote-debugging-port=9222`), `pip install websockets`, launch `runner/relay.py`, verify the uplink (`/health` returns `"ok":true`), then hold.
3. At ~5h50m it dispatches its successor via the API and exits before GitHub's 6h kill. The successor boots, opens a new uplink, the DO swaps it in (~5 min overlap; client CDP sockets get 1001-closed and reconnect). If relay *and* Chromium both die mid-run, it dispatches a successor immediately.
4. You never need to touch it again — just check the Actions page occasionally; you should see an unbroken chain of ~5h50m runs.

### Step 5 — Connect the AgentX app
In the AgentX app's **browser tunnel settings** (the tunnel settings group), enter:
- **Tunnel/Worker URL**: your Worker URL from Step 2 (e.g. `https://agentx-browser.<subdomain>.workers.dev`)
- **Client token**: your `CLIENT_TOKEN` from Step 1
Then use the app's connect/validate action. (For reference, the operator's own live instance is `https://agentx-browser.pcagent.workers.dev/`.)

### Step 6 — Test it
```bash
curl https://<worker>/health
# expect: {"ok":true,"ts":...}   ("ok":false means no runner is currently connected)

curl "https://<worker>/json/version?token=<CLIENT_TOKEN>"
# expect: Chromium descriptor JSON (verified build: Chrome/154.0.8037.57)
```
Full E2E verified 2026-10-01: `GET /json/version` → 200, `PUT /json/new` → 200, CDP WebSocket navigated to example.com and captured a PNG screenshot. One gotcha from testing: Cloudflare's edge 403-blocks Python's stdlib `Python-urllib/3.12` user agent — use curl/a browser-like UA (the app's okhttp/Dart stack is unaffected).

## 4. Free vs paid

| Piece | Cost |
|---|---|
| Cloudflare Workers | **Free**: 100k requests/day — normal use is far below |
| Cloudflare Durable Objects (free plan, SQLite backend) | **Free**: 100k requests/day, 13,000 GB-s/day; the DO hibernates while idle so duration billing is tiny; CDP traffic is small |
| Cloudflare KV | **Not used at all** (the DO holds runner state in memory) |
| GitHub Actions | **Free** on a public repo (unlimited minutes) — but ⚠️ against ToS for production; dummy account only |
| Signups needed | Cloudflare (free) + GitHub (free). No tunnel-service signup, no payment method anywhere (ngrok was dropped specifically for demanding one) |

## 5. Caveats / things the doc page should warn about

1. **EXPERIMENTAL PoC** — the repo's own words. Not production-hardened.
2. **Dummy GitHub account only** — never the account that builds/releases AgentX (ToS risk + the workflow force-pushes heartbeats and self-dispatches).
3. **Runner handover blips**: when a successor takes over (~every 5h50m), the DO 1001-closes active client CDP sockets; clients must reconnect (the app handles this, but expect a brief reset roughly every 6 hours).
4. **Secrets handling**: `RELAY_SECRET` appears in the runner's uplink query string (`/relay?secret=`); anyone with it can attach a rogue browser. `CLIENT_TOKEN` gates all client access. Rotate both if ever exposed (re-deploy Worker secrets + update repo secret + re-paste into the app).
5. **No wrangler.toml in repo** — deployment is via `worker/deploy-worker.py` (Cloudflare API directly). A docs page should present the wrangler alternative for humans, since the script's `dynamic_credentials` import is agent-tooling, not something a user will have.
6. The workflow file at dispatch time is what successors run ("dispatches always run the workflow file as it exists on the branch AT DISPATCH TIME, so successors self-heal to the newest code") — pushing fixes to `main` propagates automatically at the next handover.