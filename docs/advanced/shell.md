# Shell — A Real Linux Environment

The agent runs **real shell commands** in a Linux environment (Alpine-based, via proot) right on your phone. This isn't a simulation — it's a working shell with a filesystem, package manager, and scripting.

## What it can do

- **Run commands and scripts** — one-liners, pipelines, shell scripts, cron-style automation.
- **Manage the workspace** — create, edit, move, organize, and search files.
- **Install packages** — `apk` packages, Python packages, npm tools, CLIs.
- **Build and test** — compile code, run test suites, process data, convert files.
- **Network** — `curl` APIs, download files, probe endpoints.

## Environment rules (Alpine)

The shell is Alpine Linux, which is minimal by design:

1. **First command in a fresh environment:** run `apk update` before installing anything.
2. **Python packages:** prefer pure-Python or wheels-only packages — there's no compiler toolchain, so anything needing `gcc` won't build.
3. **Long-running commands:** start them detached (background) and poll the output file instead of blocking the chat.

## Secrets are redacted automatically

API keys and tokens in the environment are **never shown to the model** — tool results replace them with `[REDACTED_SECRET]`. The agent can *use* secrets (e.g. call an API) without ever *seeing* their values. This is enforced centrally, not per-tool.

## Practical examples

- "Download this CSV and tell me the top 10 rows by revenue" — the agent fetches, parses, and summarizes.
- "Clone this repo and run its tests" — full dev workflow on your phone.
- "Every morning, check this site and summarize changes" — combine with scheduled tasks.

## What's next

- [MCP](/advanced/mcp) — plug in external tool servers
- [Skills](/advanced/skills) — save reusable shell workflows as skills
