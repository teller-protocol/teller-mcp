# Teller MCP Skill

This repository packages the **Teller MCP Borrow** server as an OpenClaw skill. Teller lets agents borrow USDC or altcoins against altcoin collateral with no margin-call liquidations; this skill gives agents a turnkey MCP interface for pool discovery, max-borrow quotes, borrow/repay transaction building, and only then (optionally) delta-neutral funding scans.

It includes:

- `skills/tellermcp-mcp/SKILL.md` — skill metadata + runbook
- `skills/tellermcp-mcp/scripts/tellermcp-server/` — TypeScript MCP server source
- `skills/tellermcp-mcp/references/delta-neutral-api.md` — API cheat sheet for Teller endpoints
- `dist/tellermcp-mcp.skill` — pre-built package ready for `openclaw skills install`

## Installation (OpenClaw)
1. Download `dist/tellermcp-mcp.skill` from the latest release (or this repo).
2. (Optional) Verify integrity:
   ```bash
   shasum -a 256 dist/tellermcp-mcp.skill
   # expected: 80707f0bcc265f39e947dddef4694919627b0e90eefce41cb2f239b9f319d280
   ```
3. Install into your agent:
   ```bash
   openclaw skills install dist/tellermcp-mcp.skill
   ```
4. Register the MCP server with mcporter/OpenClaw:
   ```jsonc
   {
     "name": "tellermcp",
     "command": "npm",
     "args": ["start"],
     "cwd": "/absolute/path/to/skills/tellermcp-mcp/scripts/tellermcp-server"
   }
   ```
5. Restart mcporter; the Teller tools become available immediately.

## Development

```bash
cd skills/tellermcp-mcp/scripts/tellermcp-server
npm install
npm run build
npm start   # runs MCP server over stdio
```

## Packaging for OpenClaw / ClawHub

Prerequisite: install PyYAML (required by OpenClaw's packager).
```bash
python3 -m pip install --user pyyaml
```

Then run:
```bash
SKILL_DIR=skills/tellermcp-mcp
python3 /usr/local/lib/node_modules/openclaw/skills/skill-creator/scripts/package_skill.py "$SKILL_DIR"
```
This produces `dist/tellermcp-mcp.skill` (already pre-built in this repo). Re-run whenever code changes to refresh the artifact.

## ClawHub Release Checklist
See [RELEASE.md](RELEASE.md) for the full submission workflow. Summary:
1. Bump version/tag (e.g., `v0.1.0`).
2. Repackage the skill (steps above) and recompute SHA-256.
3. Commit the updated artifact + metadata.
4. Create a GitHub release and attach `dist/tellermcp-mcp.skill`.
5. Submit the release URL + checksum to ClawHub.

## MCP Operations (Borrowing First)

The skill focuses on Teller’s borrow/repay rails (no liquidations), then layers on delta-neutral discovery. Each tool returns:
- A concise human summary (text)
- `structuredContent.payload` with the raw JSON for automation

| Tool | Description | Common Inputs | Output Highlights |
| --- | --- | --- | --- |
| `get-borrow-pools` | Enumerates Teller borrow pools + enrichment. | `chainId`, `collateralTokenAddress`, `borrowTokenAddress`, `poolAddress`, `ttl` | Pool stats: collateral ratios, available liquidity, fees, payment cycle. |
| `get-borrow-terms` | Calculates per-wallet borrow capacity for a specific pool + collateral token. | `wallet`, `chainId`, `collateralToken`, `poolAddress` | `maxBorrowUsd`, `ltvPct`, collateral balances, principal available. |
| `build-borrow-transactions` | Generates the transaction sequence to borrow (approvals + `acceptSmartCommitment`). | `walletAddress`, `collateralTokenAddress`, `chainId`, `poolAddress`, `collateralAmount`, `principalAmount`, `loanDuration` | Ordered transactions with calldata, plus summary flags (`needsApproval`, `needsForwarderApproval`). |
| `get-wallet-loans` | Fetches all Teller loans for a wallet (active + historical). | `walletAddress`, `chainId` | Loan list with status, APR, schedule, collateral info. |
| `build-repay-transactions` | Builds approval + repay transactions for full or partial payoff. | `bidId`, `chainId`, `walletAddress`, optional `amount` | Repayment calldata, total owed, lending token metadata, full vs. partial flag. |
| `get-delta-neutral-opportunities` | (Optional) Surfaces delta-neutral arbitrage pairs by comparing Teller borrow APR vs. perp funding APRs. | `chainId`, `coin`, `limit`, `minNetAprPct` | Sorted opportunity list with `netAprPct`, principal available, perp venue metadata. |

## Agent Integration Reminder
After installation, restart mcporter (or your agent runner) so it can discover the new MCP transport and tools.
