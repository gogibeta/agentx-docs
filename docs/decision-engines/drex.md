# Drex

Drex is the alternative decision engine for browser automation — same role as Jev (fast micro-decisions per browser action), different provider.

## Get access

Drex is invite-based. Use this invite link:

**https://drex.nace.ai/invite/kc6cz8pu**

Sign up through the invite, then create an API key in the Drex dashboard.

## Setup in AgentX

1. In AgentX, open the **decision engine settings**.
2. Paste your **Drex base URL** and **model name**.
3. Paste your **API key** (multiple keys, one per line, are supported — the agent rotates them).
4. Select **Drex** as the active decision engine.

## Jev vs Drex

| | Jev | Drex |
|---|---|---|
| Role | Fast browser micro-decisions | Fast browser micro-decisions |
| Access | Bring your own key | Invite link above |
| Fallback | Main model on failure | Main model on failure |

Pick whichever is working better for you — the agent uses the selected engine, and the main model only steps in when the engine is unavailable or errors.

## What's next

- [Browser Setup](/browser/setup) — the tunnel the engine drives.
