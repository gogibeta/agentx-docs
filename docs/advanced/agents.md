# Agents & Subagents

The main agent can **delegate work to subagents** that run in the background while you keep chatting. Long research, multi-step builds, parallel investigations — the agent fans out and reports back.

## How it works

- **Ask for something long-running** — the agent decides when a subagent helps (or you can ask explicitly).
- **Background execution** — subagents work while the conversation continues; you get the result when it's ready.
- **Shared memory, single-writer** — subagents share the conversation's memory safely; only one writer at a time, so nothing corrupts.
- **Per-call model & tools** — each subagent call can pick its own **model** and **tool set**. Cheap fast model for research, strong model for the final answer, browser tools only where needed. This keeps costs and context under control.

## Why subagents matter

Without them, one long task blocks the whole chat and burns the main model's context on every step. With them:

- Research runs in parallel (three investigations at once, not one after another).
- The main conversation stays responsive and its context stays clean.
- Each subtask gets the right-sized model instead of the most expensive one for everything.

## Agent modes & workspace scoping

- **Plan / Build modes** can be scoped to a **project folder**: when you pick the mode, the app asks which folder inside the workspace it may touch.
- The agent scans the **whole workspace only when you explicitly ask** — scoped modes can't wander into unrelated projects.

## Context hygiene

- **Automatic Jev checkpoints** compact only model-visible context; the **full chat history is always retained** in the app.
- If context grows fast during normal chat, check **Settings → Diagnostics** — the log shows exactly what grew it, so the cause is traceable instead of mysterious.

## What's next

- [Skills](/advanced/skills) — playbooks subagents follow too
- [Shell](/advanced/shell) — where subagents execute
