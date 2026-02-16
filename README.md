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

## MCP Operations

The MCP server publishes six Teller-specific tools. Each returns:
- A concise human summary (text)
- `structuredContent.payload` with the raw JSON for automation

| Tool | Description | Common Inputs | Output Highlights |
| --- | --- | --- | --- |
| `get-delta-neutral-opportunities` | Surfaces delta-neutral arbitrage pairs by comparing Teller borrow APR vs. perp funding APRs. | `chainId`, `coin`, `limit`, `minNetAprPct` (all optional) | Sorted opportunity list with `netAprPct`, principal available, perp venue metadata. |
| `get-borrow-pools` | Enumerates Teller borrow pools + enrichment. | `chainId`, `collateralTokenAddress`, `borrowTokenAddress`, `poolAddress`, `ttl` (optional cache override) | Pool stats: collateral ratios, available liquidity, fees, payment cycle. |
| `get-borrow-terms` | Calculates per-wallet borrow capacity for a specific pool + collateral token. | `wallet`, `chainId`, `collateralToken`, `poolAddress` | `maxBorrowUsd`, `ltvPct`, collateral balances, principal available. |
| `build-borrow-transactions` | Generates the transaction sequence to borrow (approvals + `acceptSmartCommitment`). | `walletAddress`, `collateralTokenAddress`, `chainId`, `poolAddress`, `collateralAmount`, `principalAmount`, `loanDuration` | Ordered transactions with calldata, plus summary flags (`needsApproval`, `needsForwarderApproval`). |
| `get-wallet-loans` | Fetches all Teller loans for a wallet (active + historical). | `walletAddress`, `chainId` | Loan list with status, APR, schedule, collateral info. |
| `build-repay-transactions` | Builds approval + repay transactions for full or partial loan payoff. | `bidId`, `chainId`, `walletAddress`, optional `amount` | Repayment calldata, total owed, lending token metadata, full vs. partial flag. |

## Agent Integration

To register the MCP server with mcporter/OpenClaw:

```jsonc
{
  "name": "tellermcp",
  "command": "npm",
  "args": ["start"],
  "cwd": "/absolute/path/to/skills/tellermcp-mcp/scripts/tellermcp-server"
}
```

Once mcporter refreshes, any Codex agent can call the six tools above to reason about Teller opportunities, borrowing, and repayment flows.
