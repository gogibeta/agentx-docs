# First Launch

## Permissions

On first launch, AgentX asks for:

- **Storage** — for workspace files, downloads, and the diagnostics log.
- **Notifications** — for background task updates.

Grant both. The app is unusable without storage access.

## Setup order

Do things in this order — each step unlocks the next:

1. **[Providers & Models](/providers/nararouter)** — add at least one model provider with a free key. Without this, the agent can't think.
2. **[Decision Engine](/decision-engines/jev)** — pick Jev or Drex for fast browser automation (optional but recommended).
3. **[Browser](/browser/setup)** — connect the cloud tunnel so the agent can browse the web.
4. **[Social](/social/overview)** — deploy your worker for X/Twitter and more (optional).

::: tip
You can chat with the agent after step 1. Steps 2–4 add capabilities — the app works without them, just with fewer tools.
:::

## Diagnostics

AgentX keeps an always-on diagnostic log. If something breaks:

1. Go to **Settings → Diagnostics**.
2. Tap **Export / Share diagnostics**.
3. Send the file when asking for help — it contains no API keys (they're redacted automatically).
