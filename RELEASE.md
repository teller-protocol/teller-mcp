# Release / ClawHub Submission Guide

Use this checklist whenever you cut a new Tellermcp skill release for ClawHub or the official OpenClaw registry.

## 1. Prep Environment
- Ensure Node.js 20+ and npm are installed.
- Install PyYAML (required by OpenClaw packager):
  ```bash
  python3 -m pip install --user pyyaml
  ```

## 2. Bump Version + Update Changelog (if any)
- Update version references (e.g., Git tag, release notes).
- Note major changes in `README.md` or a separate CHANGELOG if you maintain one.

## 3. Rebuild + Test MCP Server
```bash
cd skills/tellermcp-mcp/scripts/tellermcp-server
npm install
npm run build
npm start   # optional smoke test
```

## 4. Package the Skill
```bash
SKILL_DIR=skills/tellermcp-mcp
python3 /usr/local/lib/node_modules/openclaw/skills/skill-creator/scripts/package_skill.py "$SKILL_DIR"
```
- Output: `tellermcp-mcp.skill` in repo root (move to `dist/`)
- Example:
  ```bash
  mv tellermcp-mcp.skill dist/tellermcp-mcp.skill
  ```

## 5. Verify Integrity
```bash
shasum -a 256 dist/tellermcp-mcp.skill
```
Record the checksum for release notes / ClawHub form.

## 6. Commit + Tag
```bash
git add dist/tellermcp-mcp.skill README.md RELEASE.md
git commit -m "chore: cut v0.x.y release"
git tag v0.x.y
```
Push branch + tag:
```bash
git push origin <branch>
git push origin v0.x.y
```

## 7. Create GitHub Release
```bash
gh release create v0.x.y dist/tellermcp-mcp.skill \
  --title "Tellermcp v0.x.y" \
  --notes "- <highlights>\n- SHA-256: <checksum>"
```

## 8. Submit to ClawHub
Provide:
- Release URL (GitHub tag)
- Download link to `tellermcp-mcp.skill`
- SHA-256 checksum
- Brief description + install instructions

Once approved, users can install via `openclaw skills install <download-url-or-path>`.
