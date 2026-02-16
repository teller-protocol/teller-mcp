# ClawHub Submission Notes

**Skill Name:** Teller MCP – Borrow USDC & Altcoins (no margin calls)
**Version:** v0.2.0

## Submission Summary
- **Description:** Teller lets onchain agents borrow stablecoins against altcoins with no margin-call liquidations. This MCP skill focuses on the borrowing workflow—pool discovery, per-wallet terms, borrow transaction builders, and loan repayment helpers—with delta-neutral scanning mentioned only after the core borrow stack.
- **Primary Use Cases:**
  1. Discover Teller borrow pools (USDC or altcoin collateral) across chains.
  2. Compute per-wallet max borrow + collateral requirements and build borrow transaction calldata (approvals + accept commitment).
  3. Inspect active/historic loans and generate repayment transactions (full or partial).
  4. *(Optional, secondary)* Scan delta-neutral opportunities once the loans are live.

## Release Artifacts
- **GitHub Release:** https://github.com/teller-protocol/teller-mcp/releases/tag/v0.2.0
- **Download URL:** https://github.com/teller-protocol/teller-mcp/releases/download/v0.2.0/tellermcp-mcp.skill
- **SHA-256:** `80707f0bcc265f39e947dddef4694919627b0e90eefce41cb2f239b9f319d280`

## Installation Snippet
```bash
# Download + verify
curl -L -o tellermcp-mcp.skill https://github.com/teller-protocol/teller-mcp/releases/download/v0.2.0/tellermcp-mcp.skill
shasum -a 256 tellermcp-mcp.skill
# expect: 80707f0bcc265f39e947dddef4694919627b0e90eefce41cb2f239b9f319d280

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
