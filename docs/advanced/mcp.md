# MCP — External Tool Servers

MCP (Model Context Protocol) is an open standard for plugging **external tools** into the agent — databases, APIs, file systems, dev tools, and your own custom integrations. If a capability exists as an MCP server, AgentX can use it.

## How it works

1. Open **Settings → MCP**.
2. Add a server: give it a **name** plus the server command (local `stdio` server) or **URL** (remote SSE/streaming server).
3. The server's tools appear in the agent's tool list automatically — no restart needed.
4. The agent picks them up like any built-in tool when the task matches.

## What you can connect

- **Databases** — query Postgres, SQLite, etc. through MCP database servers.
- **Dev tools** — GitHub, GitLab, Docker, Kubernetes MCP servers.
- **Files & cloud** — Google Drive, S3, local filesystem servers.
- **Custom** — write your own MCP server (any language) exposing exactly the tools your workflow needs.

## Tips

- **Start with one server** and confirm its tools show up in the tool list before adding more.
- **Prefer narrow toolsets** — every tool definition costs context on every turn. A server with 50 tools you never use is a context tax.
- **Disable, don't delete** — if a server misbehaves, toggle it off here; the config stays for later.
- **Secrets** — MCP servers run with the same secret redaction as the shell: credentials in tool results are masked.

## stdio vs remote servers

| | Local (stdio) | Remote (URL) |
|---|---|---|
| Runs | As a process on the phone | On your server / a hosted endpoint |
| Setup | Command + args in settings | Just the URL (+ auth header if needed) |
| Best for | Bundled tools, offline use | Shared team tools, heavy dependencies |

## What's next

- [Skills](/advanced/skills) — reusable playbooks the agent follows
- [Shell](/advanced/shell) — the local execution environment
