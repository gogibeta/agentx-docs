# Skills

Skills are reusable playbooks: a name, a description of when to use it, and instructions the agent follows. The app ships with built-in skills (browser, social, Jev); you can add your own.

## Add a skill

1. Open **Settings → Skills**.
2. Create a skill: name, trigger description ("use when…"), and the instructions.
3. Skills activate automatically when the agent's task matches the trigger.

## Writing good skills

- **One job per skill.** "Fetch X API" beats "handle everything about X".
- **Trigger description is everything** — it's how the agent decides to use it. Write it like a rule: "Use when the user asks about…".
- **Keep instructions concrete** — exact endpoints, exact field names, exact steps.
- Test by asking the agent something that should trigger it.

## What's next

- [Agents & Subagents](/advanced/agents) — delegating work.
