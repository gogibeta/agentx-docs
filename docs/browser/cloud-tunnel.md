# Cloud Tunnel (agentx-cloud-browser)

Deploy your own cloud browser: a headless Chromium on a GitHub runner, reachable from the AgentX app through your own Cloudflare Worker.

**Source repo:** https://github.com/polironi10/agentx-cloud-browser

## Step 1 — Generate secrets

Pick two long random strings (32+ characters, from a password generator):

- `RELAY_SECRET` — shared between the runner and the Worker.
- `CLIENT_TOKEN` — pasted into the AgentX app later.

Write them down. Each gets entered in two places.

## Step 2 — Deploy the Cloudflare Worker

1. Log in at https://dash.cloudflare.com and open **Workers & Pages** once — this creates your `workers.dev` subdomain.
2. Create a Cloudflare **API token** at https://dash.cloudflare.com/profile/api-tokens (needs Workers-script edit permission).
3. Clone the repo and run the deploy script:
   ```bash
   git clone https://github.com/polironi10/agentx-cloud-browser.git
   cd agentx-cloud-browser
   python3 worker/deploy-worker.py
   ```
   This uploads `worker.js`, binds the `BrowserRelay` Durable Object (SQLite backend), and sets the `RELAY_SECRET` and `CLIENT_TOKEN` secrets.
4. Note your **Worker URL**: `https://agentx-browser.<your-subdomain>.workers.dev`. It never changes.

::: tip Wrangler alternative
If you prefer wrangler over the script: script name `agentx-browser`, Durable Object binding `RELAY` → class `BrowserRelay`, migration `new_sqlite_classes: ["BrowserRelay"]` (required on the free plan).
:::

## Step 3 — Set GitHub repo secrets

On the dummy account's repo fork: **Settings → Secrets and variables → Actions → New repository secret**:

- `WORKER_URL` = your Worker URL from Step 2.
- `RELAY_SECRET` = the exact same value you gave the Worker.

(The workflow also uses the automatic `GITHUB_TOKEN` — its `contents: write` and `actions: write` permissions are declared in the YAML.)

## Step 4 — Start the runner (once)

1. **Actions → cloud-browser → Run workflow → Run workflow** (it only triggers manually).
2. It starts headless Chromium, launches `relay.py`, verifies the uplink (`/health` → `"ok":true`), then holds.
3. At ~5h50m it dispatches its successor and exits. The new runner swaps in with ~5 min overlap. **You never touch it again** — just glance at the Actions page occasionally for an unbroken chain of ~5h50m runs.

## Step 5 — Connect the AgentX app

In the app's **browser tunnel settings**:

- **Tunnel/Worker URL:** your Worker URL (e.g. `https://agentx-browser.<subdomain>.workers.dev`)
- **Client token:** your `CLIENT_TOKEN`

Then run the app's connect/validate action.

## Step 6 — Test

```bash
curl https://<worker>/health
# {"ok":true,"ts":...} — "ok":false means no runner is connected

curl "https://<worker>/json/version?token=<CLIENT_TOKEN>"
# Chromium descriptor JSON (e.g. Chrome/154.0.8037.57)
```

## Caveats

- **Experimental PoC** — not production-hardened.
- **Dummy GitHub account only** — ToS risk on your main account.
- **Handover blips** — every ~6h the DO drops client CDP sockets during runner swap; the app reconnects automatically.
- **Rotate secrets if exposed** — anyone with `RELAY_SECRET` can attach a rogue browser; anyone with `CLIENT_TOKEN` can drive yours.

## What's next

- [Multi-Tab & Takeover](/browser/tabs-takeover) — drive the browser from the app.
