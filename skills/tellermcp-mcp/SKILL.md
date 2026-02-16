---
name: tellermcp-mcp
description: Teller borrowing MCP skill (no liquidation) that runs the Tellermcp server so agents can quote max borrow, build borrow/repay tx, and optionally scan delta-neutral funding spreads.
---

# Tellermcp MCP Skill

## Overview
Teller lets agents borrow stablecoins against altcoins with **no margin-call liquidations**. This skill bundles a ready-to-run MCP server (`scripts/tellermcp-server/`) focused on Teller’s borrowing surface—pool discovery, wallet-specific terms, borrow transaction builders, loan lookups, and repayment helpers—with optional delta-neutral funding scans. Load it whenever you must:
- Deploy or modify the Tellermcp MCP server
- Re-run `npm install`, build, or tests for the server
- Register Tellermcp with mcporter/OpenClaw so agents can tap Teller borrowing + repay endpoints (and optionally delta-neutral insights)

## Quick Start
1. `cd scripts/tellermcp-server`
2. `npm install`
3. (Optional) Configure env vars:
   - `TELLER_API_BASE_URL` (defaults to `https://delta-neutral-api.teller.org`)
   - `TELLER_API_TIMEOUT_MS` (defaults to `15000` ms)
4. `npm run build` → TypeScript type-check.
5. `npm start` → launches `tellermcp` MCP server over stdio.

## Repo Layout (`scripts/tellermcp-server/`)
- `package.json` / `package-lock.json` – Node 20+ project metadata
- `tsconfig.json` – ES2022/ESNext build targets
- `src/client.ts` – Typed Teller REST client (fetch wrapper + filters)
- `src/types.ts` – TypeScript interfaces for all Teller responses
- `src/index.ts` – MCP server wiring (McpServer + StdioServerTransport) registering tools:
  1. `get-delta-neutral-opportunities`
  2. `get-borrow-pools`
  3. `get-borrow-terms`
  4. `build-borrow-transactions`
  5. `get-wallet-loans`
  6. `build-repay-transactions`

Each tool returns: short text summary + `structuredContent.payload` containing the raw JSON for downstream automation.

## Runbook
### Installing dependencies
```bash
cd scripts/tellermcp-server
npm install
```
The project intentionally omits `node_modules/` & `dist/`; `npm install` and `npm run build` regenerate them.

### Local testing
- `npm run build` → TypeScript compile.
- `npm start` → STDIO MCP server. Use `gh pr checks` or `npm test` (placeholder) if additional tests are added later.

### Registering with mcporter / OpenClaw
Add an entry to your `mcporter` (or agent transport) config:
```jsonc
{
  "name": "tellermcp",
  "command": "npm",
  "args": ["start"],
  "cwd": "/absolute/path/to/scripts/tellermcp-server"
}
```
Once mcporter restarts, Codex agents can invoke the six Teller tools directly.

### Deploying updates
1. Edit TypeScript files inside `src/`.
2. `npm run build` locally.
3. Commit + push via GitHub skill (if syncing to `teller-protocol/teller-mcp`).
4. Restart the mcporter process pointing at this repo to pick up changes.

## References
- [references/delta-neutral-api.md](references/delta-neutral-api.md) – condensed Teller API cheat sheet (endpoints, params, caching behavior). Load when you need exact REST contract details.

## Packaging This Skill
When ready to ship a `.skill` bundle:
```bash
python3 /usr/local/lib/node_modules/openclaw/skills/skill-creator/scripts/package_skill.py /data/workspace/skills/tellermcp-mcp
```
The packager validates SKILL.md + resources and emits `tellermcp-mcp.skill` (zip) for distribution.
