# Add a Provider

Providers live in **Settings → Providers**. Each provider needs two things: a **base URL** and an **API key**.

## Steps

1. Open **Settings → Providers**.
2. Tap **Add provider** (or pick an existing entry to edit).
3. Fill in:
   - **Name** — anything, e.g. `NaraRouter`.
   - **Base URL** — e.g. `https://router.bynara.id/v1`.
   - **API key** — paste your key (e.g. `sk-nry-…`).
4. Save. The app validates the key with a cheap models-list call.

## Multiple keys per provider

You can paste **multiple keys, one per line**, for the same provider. The agent rotates across them automatically to spread load and dodge rate limits. Useful if you have 2–3 free keys.

## What's next

- [Choose Models](/providers/models) — assign models to agent roles.
