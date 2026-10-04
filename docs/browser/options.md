# Browser Automation Options

AgentX's browser automation has **two independent choices** that people often mix up. Understanding them separately makes setup obvious:

1. **Backend — where the browser runs** (the *hands*): System WebView vs Cloud Tunnel
2. **Decision engine — who decides what to click** (the *brain*): Jev vs Drex vs main model

You can combine any backend with any decision engine.

---

## Part 1: Browser backends (the hands)

### Option A — System WebView ✅ Recommended

- **What:** Android's built-in System WebView, driven on-device through its remote-debugging socket. No downloads, no extra processes, **0 MB** added to the app.
- **Setup:** Nothing. It's the default — **Settings → Browser** already selects it.
- **Pros:**
  - Zero setup, zero cost, works offline
  - Visible and touchable right in the watch panel
  - Lightest on battery and RAM
  - Takeover taps work directly on the live view
- **Cons:**
  - Runs on your phone, so very heavy pages (huge SPAs, WebGL) can be slower than a desktop browser
  - Your phone's IP and user-agent — sites that block mobile/datacenter traffic see a mobile client
- **Best for:** Everyday browsing tasks, quick lookups, form filling, reading pages. **Start here.**

### Option B — Cloud Tunnel

- **What:** A full desktop Chromium running on a server (e.g. a GitHub Actions runner), connected to your phone through your own Cloudflare relay. The app talks to it over CDP (Chrome DevTools Protocol) tunneled over HTTPS/WebSocket.
- **Setup:** Deploy the relay worker + runner — full guide: [Cloud Tunnel](/browser/cloud-tunnel).
- **Pros:**
  - Desktop-class rendering and speed for heavy sites
  - Server IP — useful when your mobile IP is blocked or rate-limited
  - The browser keeps running even if the app is backgrounded
- **Cons:**
  - Real setup work (Cloudflare Worker + runner + tokens)
  - Needs internet; adds latency per action
  - You operate infrastructure — keep the client token secret
- **Best for:** Heavy automation, sites that block mobile clients, long unattended runs.

<div class="rec-box">

**Recommendation:** Use **System WebView** unless you have a concrete reason not to. It's the default for a reason — instant, free, and good enough for ~90% of tasks. Add the **Cloud Tunnel** when you hit a site that needs desktop rendering or when you want automation to keep running independently of your phone.

</div>

---

## Part 2: Decision engines (the brain)

Each browser step is: *look at the page → decide one action → do it → repeat*. The decision engine is what makes the "decide" step. AgentX tries them in order and falls back gracefully.

### Option A — Jev (TypeSafe) ✅ Recommended for speed

- **What:** [jev-ultrafast](https://github.com/browser-use/jev-ultrafast)-style automation. Instead of sending the whole page to your big main model, AgentX sends a compact element list to a small, fast **Choice** model that returns exactly one action (~1–2 seconds per step).
- **Setup:** **Settings → Jev** — paste your TypeSafe API key, base URL, and model name. Any Jev-compatible endpoint works.
- **Pros:**
  - ~1–2s per action — dramatically faster than model round-trips
  - Cheap — tiny decision model, not your frontier model
  - Fail-open: if Jev isn't configured or errors, the task falls back to the main model instead of breaking
- **Cons:**
  - Needs a Jev-compatible key/endpoint (you bring it)
  - Compact element table (~limited chars per step) — can miss context on extremely dense pages
- **Best for:** Everyday automation speed. The default choice once configured.

### Option B — Drex (Nace AI)

- **What:** Drex by Nace AI — a **wire-compatible** alternative to Jev (same API shape, drop-in replacement) that allows roughly **~4x the context** per decision step.
- **Setup:** **Settings → Jev** — select Drex as the provider, or paste its base URL. Invite: https://drex.nace.ai/invite/kc6cz8pu
- **Pros:**
  - Same fast one-action-per-step loop as Jev
  - ~4x context — sees much larger element tables, better on complex/dense pages
- **Cons:**
  - Invite-based access; pricing/tiers set by Nace AI
- **Best for:** Complex pages where Jev's compact view misses things — dashboards, dense tables, intricate forms.

### Option C — Main model fallback

- **What:** If neither Jev nor Drex is configured (or the decision call fails), the **chat's main model** makes the browser decisions.
- **Setup:** None — always available.
- **Pros:** Works out of the box with zero configuration.
- **Cons:** Much slower per step and burns your main model's tokens/context on every click.
- **Best for:** Getting started before you've set up Jev/Drex, or as an emergency fallback.

<div class="rec-box">

**Recommendation:** Configure **Jev** first (fastest, cheapest per action). Switch to **Drex** when pages get complex and Jev starts missing elements. The **main model fallback** stays as your safety net — you'll rarely notice it once Jev is set up.

</div>

---

## Putting it together

| Your situation | Backend | Decision engine |
|---|---|---|
| Just installed, want it to work | System WebView | Main model (then add Jev) |
| Daily driver, fastest automation | System WebView | Jev |
| Complex dashboards / dense pages | System WebView or Tunnel | Drex |
| Site blocks mobile / heavy WebGL | Cloud Tunnel | Jev or Drex |
| Long unattended runs | Cloud Tunnel | Jev |

## Related

- [Browser Setup](/browser/setup) — step-by-step backend configuration
- [Cloud Tunnel](/browser/cloud-tunnel) — deploy your own relay
- [Multi-Tab & Takeover](/browser/tabs-takeover) — tabs, the visible cursor, and taking over manually
- [Jev](/decision-engines/jev) · [Drex](/decision-engines/drex) — engine details
