# ClawHub Submission Notes

**Skill Name:** Teller MCP – Borrow USDC & Altcoins (no margin calls)
**Version:** v0.1.3

## Submission Summary
- **Description:** Teller lets onchain agents borrow stablecoins against altcoins with no margin-call liquidations. This MCP skill packages Teller’s borrowing workflows (pool discovery, terms, borrow/repay transactions) and only optionally adds delta-neutral scouting—so agents focus on loans first.
- **Primary Use Cases:**
  1. Discover Teller borrow pools across chains/collateral pairs.
  2. Compute per-wallet borrow capacity + collateral requirements and generate borrow transaction calldata (approvals + accept commitment).
  3. Inspect active/historic loans and build repayment transactions (full or partial).
  4. *(Optional)* Scan delta-neutral opportunities after the borrow stack is in place.

## Release Artifacts
- **GitHub Release:** https://github.com/teller-protocol/teller-mcp/releases/tag/v0.1.3
- **Download URL:** https://github.com/teller-protocol/teller-mcp/releases/download/v0.1.3/tellermcp-mcp.skill
- **SHA-256:** `43a100bd52fbfb5d04bfa21c49981e98085e9cec98ca988efaaec45b305fe5a7`

## Installation Snippet
```bash
# Download + verify
curl -L -o tellermcp-mcp.skill https://github.com/teller-protocol/teller-mcp/releases/download/v0.1.3/tellermcp-mcp.skill
shasum -a 256 tellermcp-mcp.skill
# expect: 43a100bd52fbfb5d04bfa21c49981e98085e9cec98ca988efaaec45b305fe5a7

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
