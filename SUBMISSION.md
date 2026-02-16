# ClawHub Submission Notes

**Skill Name:** Tellermcp MCP
**Version:** v0.1.1

## Submission Summary
- **Description:** Teller lets onchain agents borrow stablecoins against altcoins with no margin-call liquidations. This MCP skill packages the Teller borrowing workflows (pool discovery, terms, borrow/repay transactions) plus optional delta-neutral scouting into a single installable asset.
- **Primary Use Cases:**
  1. Discover Teller borrow pools across chains/collateral pairs.
  2. Compute per-wallet borrow capacity + LTV and generate borrow transaction calldata (approvals + accept commitment).
  3. Inspect active/historic loans and build repayment transactions (full or partial).
  4. (Optional) Scan delta-neutral opportunities by matching Teller borrow APRs with negative perp funding rates.

## Release Artifacts
- **GitHub Release:** https://github.com/teller-protocol/teller-mcp/releases/tag/v0.1.1
- **Download URL:** https://github.com/teller-protocol/teller-mcp/releases/download/v0.1.1/tellermcp-mcp.skill
- **SHA-256:** `832c0bbf9c537c3a9f68efe62da12948977147e9b46fab2031464b542767e674`

## Installation Snippet
```bash
# Download + verify
curl -L -o tellermcp-mcp.skill https://github.com/teller-protocol/teller-mcp/releases/download/v0.1.1/tellermcp-mcp.skill
shasum -a 256 tellermcp-mcp.skill
# expect: 832c0bbf9c537c3a9f68efe62da12948977147e9b46fab2031464b542767e674

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
