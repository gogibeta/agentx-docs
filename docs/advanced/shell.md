# Shell

The agent runs real shell commands in a Linux environment (Alpine-based, proot). Full toolset for file work, scripting, and automation.

## What it can do

- Run commands, scripts, and one-liners.
- Manage the workspace: create, edit, move, and organize files.
- Install packages (`apk update && apk add …` first on Alpine).
- Pure-Python or wheels-only for Python packages (no compiler toolchain).

## Notes

- The first command in a fresh environment should be `apk update` before installing anything.
- Long-running commands: start detached and poll the output file.
- Secrets in the environment are redacted from tool results automatically (`[REDACTED_SECRET]`) — the agent never sees raw values.

## What's next

- [MCP](/advanced/mcp) — external tool servers.
