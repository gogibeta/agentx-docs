# Jev

Jev is the decision engine that drives browser actions **fast**: one small-model decision per action, no full-model round-trip per click, no screenshots in the loop — just the accessibility tree.

## Why Jev

The old loop sent every click back through the main model (slow, context-heavy — tool-result tokens ballooned past 290K in real sessions). Jev makes each micro-decision locally:

- One Jev request per action cycle.
- Structured indexed element table (`@e1`, `@e2`, …).
- Action-specific targets; quick post-action waits.
- Loop protection: an action that changes nothing is never retried; the same action 3× in a row stops the task instead of looping forever.

## Setup

1. Get a Jev API key from your Jev provider.
2. In AgentX, open the **decision engine settings** (the Jev/Drex settings page — separate from model providers, like web search).
3. Paste the **base URL** and **model name** yourself; no model picker needed.
4. Select **Jev** as the active decision engine.

You can add **multiple keys, one per line** — the agent rotates across them to spread load and avoid rate limiting.

## Fallback

The main model is used **only** when Jev is unavailable or errors. If Jev is healthy, the model never touches browser micro-decisions.

## What's next

- [Drex](/decision-engines/drex) — the alternative engine.
