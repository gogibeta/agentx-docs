# Agents & Subagents

The main agent can delegate work to subagents that run in the background while you keep chatting.

## How it works

- Ask for something long-running — the agent can spawn a subagent for it.
- Subagents share the conversation's memory (single-writer) and report back when done.
- Each subagent call can pick its own **model** and **tool set** — e.g. a cheap fast model for research, the strong model for the final answer.

## Agent modes & workspace scoping

- **Plan / Build modes** can be scoped to a project folder: when you pick the mode, the app asks which folder inside the workspace it may touch.
- The agent scans the whole workspace only when you explicitly ask.

## Context hygiene

- Automatic Jev checkpoints compact only model-visible context; the full chat history is always retained.
- If context grows fast during normal chat, check **Settings → Diagnostics** — the log shows exactly what grew it.
