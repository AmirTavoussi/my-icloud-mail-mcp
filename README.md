# my-icloud-mail-mcp

Minimal MCP server for iCloud Mail. About 240 lines, one file, so you can read all of it.

## Tools
- `list_folders`, `list_messages`, `search_messages`, `read_message` (read-only)
- `save_draft` (writes to Drafts, never sends)
- `send_email` (only exists if `ALLOW_SEND=true`; needs `confirm: true`)
- No delete or move tools.

## Setup
1. Create an app-specific password: account.apple.com > Sign-In and Security > App-Specific Passwords.
2. Run it straight from GitHub (no install step):
   `ICLOUD_EMAIL=you@icloud.com ICLOUD_APP_PASSWORD=xxxx-xxxx-xxxx-xxxx npx -y github:YOUR_GITHUB_USERNAME/my-icloud-mail-mcp`
   Or locally: `npm install`, then `node index.js` with the same env vars.

## Client config (stdio clients, e.g. Claude Desktop)
```json
{
  "mcpServers": {
    "icloud-mail": {
      "command": "npx",
      "args": ["-y", "github:YOUR_GITHUB_USERNAME/my-icloud-mail-mcp"],
      "env": {
        "ICLOUD_EMAIL": "you@icloud.com",
        "ICLOUD_APP_PASSWORD": "xxxx-xxxx-xxxx-xxxx"
      }
    }
  }
}
```

Optional: add `"ALLOW_SEND": "true"` to `env` to enable sending.

## Security notes
- Revoke the app password at account.apple.com if anything looks off.
- Pin dependency versions and commit `package-lock.json`; run `npm audit` occasionally.
- Email bodies are untrusted: a malicious email can try to instruct your bot. Keep sending off, or keep a human approval step.
