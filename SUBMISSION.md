# ClawHub Submission Notes

**Skill Name:** Tellermcp MCP
**Version:** v0.1.0

## Submission Summary
- **Description:** Teller lets on-chain borrowers tap stablecoin liquidity against altcoins with no margin-call liquidations. This MCP skill packages the Teller borrowing workflows (pool discovery, terms, borrow/repay transactions) plus optional delta-neutral scouting into a single installable asset.
- **Primary Use Cases:**
  1. Discover Teller borrow pools across chains/collateral pairs.
  2. Compute per-wallet borrow capacity + LTV and generate borrow transaction calldata (approvals + accept commitment).
  3. Inspect active/historic loans and build repayment transactions (full or partial).
  4. (Optional) Scan delta-neutral opportunities by matching Teller borrow APRs with negative perp funding rates.

## Release Artifacts
- **GitHub Release:** https://github.com/teller-protocol/teller-mcp/releases/tag/v0.1.0
- **Download URL:** https://github.com/teller-protocol/teller-mcp/releases/download/v0.1.0/tellermcp-mcp.skill
- **SHA-256:** `edfb892245bb082a642bad5c2657fd0ed67596e0f5a4a2bb0c92a57ef47b1e44`

## Installation Snippet
```bash
# Download + verify
curl -L -o tellermcp-mcp.skill https://github.com/teller-protocol/teller-mcp/releases/download/v0.1.0/tellermcp-mcp.skill
shasum -a 256 tellermcp-mcp.skill
# expect: edfb892245bb082a642bad5c2657fd0ed67596e0f5a4a2bb0c92a57ef47b1e44

# Install
openclaw skills install tellermcp-mcp.skill
```

## MCP Transport Config Example
```jsonc
{
  "name": "tellermcp",
  "command": "npm",
  "args": ["start"],
  "cwd": "/absolute/path/to/skills/tellermcp-mcp/scripts/tellermcp-server"
}
```

## Contact / Maintainer
- **Team:** Teller Protocol
- **Contact:** Idea Guy / rbcp18
- **Repository:** https://github.com/teller-protocol/teller-mcp
