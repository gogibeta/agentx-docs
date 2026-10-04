# Browser Setup

The agent's browser runs in the cloud (headless Chromium on a GitHub runner), relayed through a Cloudflare Worker to your phone. You talk to one stable Worker URL; runners come and go behind it.

## Architecture

```
Phone/app ──https/wss + CLIENT_TOKEN──▶ Cloudflare Worker (stable URL)
                                              │  Durable Object
                                              ▼
                              BrowserRelay DO (singleton "browser-main")
                                              ▲  one outbound WSS
                      GitHub runner: relay.py ─┴─▶ Chromium --headless
```

- **No ngrok / cloudflared / pinggy** — all third-party tunnels were tried and dropped.
- **No KV usage** — the Durable Object holds runner state itself.
- Runners self-chain: each run dispatches its successor at ~5h50m, before GitHub's 6-hour kill. Expect a brief reconnect roughly every 6 hours.

## What you need (all free)

1. A **Cloudflare account** — https://dash.cloudflare.com/sign-up (no card).
2. A **spare/dummy GitHub account** — never the account that builds AgentX.
3. Two secrets you generate (32+ random characters each):
   - `RELAY_SECRET` — runner ↔ Worker shared secret.
   - `CLIENT_TOKEN` — the token you paste into the AgentX app.

## Quick path

Full step-by-step: [Cloud Tunnel (agentx-cloud-browser)](/browser/cloud-tunnel).

Already have a Worker URL? Paste it in the app's **browser tunnel settings** with your client token, then use the app's connect/validate action.

## What's next

- [Cloud Tunnel](/browser/cloud-tunnel) — the complete deployment walkthrough.
- [Multi-Tab & Takeover](/browser/tabs-takeover) — using the browser in the app.
