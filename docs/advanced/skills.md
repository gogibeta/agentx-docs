# Skills — Reusable Playbooks

Skills are **reusable playbooks** the agent follows: a name, a description of *when* to use it, and step-by-step instructions. The app ships with built-in skills (browser, social, Jev); you can add your own for anything you do repeatedly.

## How skills work

When you give the agent a task, it scans skill trigger descriptions and loads the matching skill's instructions into context. The skill then guides the whole task — exact endpoints, exact field names, exact steps, learned lessons.

Think of skills as **distilled experience**: instead of re-explaining your workflow every time, you write it once and the agent applies it forever.

## Add a skill

1. Open **Settings → Skills**.
2. Create a skill with three parts:
   - **Name** — short, e.g. `deploy-fxembed`.
   - **Trigger description** — "use when…", written like a rule the agent matches against your request.
   - **Instructions** — the concrete steps, commands, and gotchas.
3. Skills activate automatically when a task matches the trigger — no manual invocation needed.

## Writing good skills

- **One job per skill.** "Deploy the fxembed worker" beats "handle everything about social".
- **The trigger description is everything** — it's the only thing the agent sees when deciding. Write it as a matching rule: *"Use when the user asks to deploy/update the fxembed worker, or mentions Cloudflare social embeds."*
- **Keep instructions concrete** — exact endpoints, exact field names, exact commands. Vague advice ("be careful with auth") doesn't survive contact with a real task.
- **Record gotchas** — every time something fails once, add the fix to the skill so it never fails the same way twice.
- **Test it** — ask the agent something that should trigger the skill and check it actually loads.

## Built-in skills

| Skill | Trigger |
|---|---|
| Browser | Web automation, CDP, tunnel setup |
| Social | X/Twitter embeds, fxembed worker |
| Jev | Browser decision-engine setup, pruning context |

## What's next

- [Agents & Subagents](/advanced/agents) — delegating work to background agents
- [Shell](/advanced/shell) — the execution environment skills often drive
