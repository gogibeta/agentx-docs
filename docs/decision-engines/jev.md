# Jev — The Fast Decision Engine

Jev is the decision engine that drives browser actions **fast**: one small-model decision per action, no full-model round-trip per click, no screenshots in the decision loop — just the accessibility tree.

## The problem Jev solves

The naive browser loop works like this: screenshot the page → send it to your big main model → wait for it to decide → click → repeat. In real sessions this was painfully slow and context-heavy — tool-result tokens ballooned past **290K** in a single session, and every click cost a full frontier-model call.

Jev flips the loop into [jev-ultrafast](https://github.com/browser-use/jev-ultrafast) style:

1. **Snapshot** — the browser produces a compact accessibility tree (buttons, links, inputs as `@e1`, `@e2`, …).
2. **One Choice call** — a small, fast model sees the element table + your goal and returns exactly **one action** (~1–2 seconds).
3. **Execute** — AgentX performs the CDP action (click, type, scroll…), waits briefly for the page to settle, and repeats.

Your expensive main model never touches micro-decisions. It only sees the summary when the task finishes.

## How decisions stay safe

- **Structured element table** — the engine picks from indexed elements (`@e1`, `@e2`), not raw coordinates, so clicks land on real controls.
- **Action-specific targets** — each action type (click, type, select…) gets the right element kind.
- **Quick post-action waits** — ≤200ms settle after typing, so the next snapshot reflects reality.
- **Loop protection** — an action that changes nothing is blacklisted and never retried; the same action 3× in a row stops the task instead of spinning forever.
- **Confidence gate** — below a minimum confidence threshold the engine stops as "uncertain" rather than clicking blindly.
- **Fail-open** — if Jev isn't configured or errors mid-task, the main model takes over. Jev never breaks a user-visible flow.

## Setup

1. Get a Jev-compatible API key from your Jev provider.
2. In AgentX, open **Settings → Jev** (its own settings page — separate from model providers, like web search).
3. Paste the **base URL** and **model name** yourself; no model picker needed.
4. Paste your **API key** — multiple keys, one per line, are supported and the agent rotates across them to spread load and avoid rate limiting.
5. Make sure **Jev** is the selected decision engine.

## When the main model steps in

The main model is used **only** as a fallback — when Jev is not configured, the key is missing, or a decision call fails. If Jev is healthy, the model never touches browser micro-decisions. You can watch which engine is active in the browser watch panel.

## Jev vs the alternatives

| | Jev | Drex | Main model |
|---|---|---|---|
| Speed per action | ~1–2s | ~1–2s | Slow (full round-trip) |
| Cost per action | Tiny | Tiny | Full model call |
| Page context | Compact element table | ~4× larger table | Whole conversation context |
| Setup | Bring your own key | Invite-based | None |

See the full comparison with recommendations: [Browser Automation Options](/browser/options).

## What's next

- [Drex](/decision-engines/drex) — the higher-context alternative engine
- [Browser Automation Options](/browser/options) — backends vs engines, what to pick
