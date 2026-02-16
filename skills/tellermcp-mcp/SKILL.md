---
name: tellermcp-mcp
description: Teller MCP borrow skill (USDC collateral, altcoin loans, no margin-call liquidations) plus optional delta-neutral funding scans.
---

# Teller MCP Borrow Skill

## Overview
Teller lets agents borrow stablecoins against altcoins **without margin-call liquidations**. This skill packages the Tellermcp MCP server so agents can:
- Discover Teller borrow pools across chains/collateral pairs
- Quote per-wallet max borrow + collateral requirements
- Build the on-chain transactions to borrow (approvals → accept commitment)
- List active/historic loans and generate repayment transactions
- *(Optional)* Scan delta-neutral funding spreads once the borrowing workflow is wired up

## Quick Start
1. `cd scripts/tellermcp-server`
2. `npm install`
3. Optional env vars:
   - `TELLER_API_BASE_URL` (default `https://delta-neutral-api.teller.org`)
   - `TELLER_API_TIMEOUT_MS` (default `15000` ms)
4. `npm run build` → TypeScript type-check
5. `npm start` → MCP server over stdio

## Repo Layout (`scripts/tellermcp-server/`)
- `package.json` / `package-lock.json`
- `tsconfig.json`
- `src/client.ts`, `src/types.ts`, `src/index.ts` (tool registration)

### Tools registered
1. `get-borrow-pools`
2. `get-borrow-terms`
3. `build-borrow-transactions`
4. `get-wallet-loans`
5. `build-repay-transactions`
6. `get-delta-neutral-opportunities` *(optional)*

Each returns a short text summary plus `structuredContent.payload` JSON for automation.

## Runbook
### Install deps
```bash
cd scripts/tellermcp-server
npm install
```

### Local testing
```bash
npm run build
npm start
```

### Register with mcporter/OpenClaw
```jsonc
{
  "name": "tellermcp",
  "command": "npm",
  "args": ["start"],
  "cwd": "/absolute/path/to/scripts/tellermcp-server"
}
```

### Updating
1. Edit `src/`
2. `npm run build`
3. Commit + push
4. Restart mcporter pointing to this repo

## References
- [references/delta-neutral-api.md](references/delta-neutral-api.md)

## Packaging
```bash
python3 /usr/local/lib/node_modules/openclaw/skills/skill-creator/scripts/package_skill.py /data/workspace/skills/tellermcp-mcp
```
Generates `tellermcp-mcp.skill` for distribution.
