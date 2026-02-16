# ClawHub Submission Notes

**Skill Name:** Tellermcp MCP
**Version:** v0.1.0

## Submission Summary
- **Description:** MCP skill exposing Teller delta-neutral opportunities, borrow pool discovery, borrower-specific terms, borrow transaction builders, loan listings, and repayment builders. Includes full Node/TypeScript source and ready-to-install `.skill` artifact.
- **Primary Use Cases:**
  1. Scan Teller markets for positive net APR delta-neutral plays.
  2. Discover and filter Teller borrow pools per chain/collateral.
  3. Compute per-wallet borrow capacity and LTV for any pool.
  4. Generate borrow transaction calldata (approvals + accept commitment).
  5. Inspect all loans for a wallet.
  6. Build repay transactions (full/partial) with approvals.

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
