# Drex — The High-Context Decision Engine

Drex (by Nace AI) is the alternative decision engine for browser automation — the **same role as Jev** (fast micro-decisions per browser action, one Choice call per step), from a different provider, with one big advantage: roughly **~4× the context window** per decision.

## Why Drex exists

Jev's speed comes from a **compact** element table — only so many characters of the page's accessibility tree are sent per step. On simple pages that's plenty. On dense pages (dashboards, data tables, intricate multi-section forms), the compact view can crop out the very element the task needs, and the engine starts guessing or stalling.

Drex is **wire-compatible** with Jev (same API shape — it's a drop-in replacement in AgentX's settings), but it accepts a much larger element table per step. Same fast loop, far fewer "I can't see it" moments.

## Jev vs Drex — detailed

| | Jev | Drex |
|---|---|---|
| Role | Fast browser micro-decisions | Fast browser micro-decisions |
| API | Jev Choice API | Wire-compatible with Jev |
| Context per step | Compact element table | ~4× larger element table |
| Access | Bring your own key/endpoint | Invite-based |
| Key rotation | Multiple keys, one per line | Multiple keys, one per line |
| Fallback | Main model on failure | Main model on failure |
| Best for | Everyday speed | Complex, dense pages |

**Rule of thumb:** start with Jev. Switch to Drex when you notice the engine missing elements on complicated pages, or when tasks on dashboards and web apps keep stalling.

## Get access

Drex is invite-based. Use this invite link:

**https://drex.nace.ai/invite/kc6cz8pu**

Sign up through the invite, then create an API key in the Drex dashboard. Check Drex's own site for current pricing and tiers.

## Setup in AgentX

1. In AgentX, open **Settings → Jev** (the decision-engine settings page — Drex is selected there, not in the providers list).
2. Select **Drex** as the engine.
3. Paste your **Drex base URL** and **model name**.
4. Paste your **API key** — multiple keys, one per line, are supported and the agent rotates them automatically.
5. Run a browser task and watch the decision speed in the watch panel.

## Fallback behavior

Identical to Jev: the main model steps in **only** when Drex is unavailable or a decision call errors. The switch is automatic and the task continues.

## What's next

- [Jev](/decision-engines/jev) — the default fast engine
- [Browser Automation Options](/browser/options) — backends vs engines, what to pick
- [Browser Setup](/browser/setup) — the browser the engine drives
