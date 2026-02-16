# Teller MCP Skill

This repository packages the **Tellermcp** Model Context Protocol server as an OpenClaw skill. It includes:

- `skills/tellermcp-mcp/SKILL.md` — skill metadata + runbook
- `skills/tellermcp-mcp/scripts/tellermcp-server/` — TypeScript MCP server source (Teller delta-neutral + lending tools)
- `skills/tellermcp-mcp/references/delta-neutral-api.md` — API cheat sheet for Teller endpoints

## Development

```bash
cd skills/tellermcp-mcp/scripts/tellermcp-server
npm install
npm run build
npm start   # runs MCP server over stdio
```

## Packaging for OpenClaw

```bash
SKILL_DIR=skills/tellermcp-mcp
python3 /usr/local/lib/node_modules/openclaw/skills/skill-creator/scripts/package_skill.py "$SKILL_DIR"
```

This produces `tellermcp-mcp.skill`, which you can upload to OpenClaw or ClawHub.

## MCP Tooling

The MCP server exposes six Teller-aware tools:
1. `get-delta-neutral-opportunities`
2. `get-borrow-pools`
3. `get-borrow-terms`
4. `build-borrow-transactions`
5. `get-wallet-loans`
6. `build-repay-transactions`

Each tool returns a short text summary plus structured JSON payload, making it easy for agents to consume.
