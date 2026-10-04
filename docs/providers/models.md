# Choose Models

After adding a provider, pick which model does which job. Different jobs want different strengths.

## Recommended free setup (NaraRouter)

| Role | Model | Why |
|---|---|---|
| Main agent | `agnes-2.5-flash` | Fast, reliable, vision-capable, free |
| Coding | Kimi K2.7 Code Free | Free coding specialist |
| Reasoning | `tencent-hy3` | Stronger reasoning when the flash model struggles |

## Per-model context windows

AgentX lets you set a **separate context window per model** — switching models switches the active window automatically. Set this in the model entry, not globally. Match it to what the provider actually allows (check `/v1/models` or the provider's model directory).

## What's next

- [Free vs Paid](/providers/free-vs-paid) — the full cost picture.
- [Decision Engines](/decision-engines/jev) — Jev/Drex for browser speed.
