# Connect the Produck MCP

The Produck MCP server is remote and token-authenticated. Connect it once, then `search_feedback` and
`get_feedback` are available to your agent.

> The dashboard at `https://tryproduck.com/app` is the source of truth for these snippets — it fills in
> your real token automatically. The formats below use a placeholder.

## 1. Mint a token

1. Open `https://tryproduck.com/app` → **MCP** section.
2. Generate a token (shown once). It looks like `mcp_live_…`.
3. Make it available to your agent in one of two ways:
   - `export PRODUCK_MCP_TOKEN=mcp_live_…` in your shell, **or**
   - write it to a file: `mkdir -p ~/.produck && echo "mcp_live_…" > ~/.produck/mcp_token`

Both plugin installs below read the token from `PRODUCK_MCP_TOKEN` (falling back to `~/.produck/mcp_token`).

## 2a. Install the plugin (Claude Code, Codex)

The plugin bundles the MCP **and** the `produck-feedback` + `user-alignment` skills in one install.

**Claude Code**
```
/plugin marketplace add tryproduck/produck-skills
/plugin install produck@produck
```

**Codex**
```
codex plugin marketplace add tryproduck/produck-skills
# then enable "produck" from the /plugins browser
```

## 2b. Add the MCP server (agents without a plugin system)

Use your agent's native MCP config. Replace `<YOUR_PRODUCK_MCP_TOKEN>` with the token from step 1.

**Cursor / generic JSON**
```json
{
  "mcpServers": {
    "produck": {
      "type": "http",
      "url": "https://tryproduck.com/api/mcp",
      "headers": { "Authorization": "Bearer <YOUR_PRODUCK_MCP_TOKEN>" }
    }
  }
}
```

**opencode** (`opencode.json`)
```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "produck": {
      "type": "remote",
      "url": "https://tryproduck.com/api/mcp",
      "enabled": true,
      "headers": { "Authorization": "Bearer <YOUR_PRODUCK_MCP_TOKEN>" }
    }
  }
}
```

**Codex** (manual, no plugin — `~/.codex/config.toml`)
```toml
# export PRODUCK_MCP_TOKEN=<YOUR_PRODUCK_MCP_TOKEN> first
[mcp_servers.produck]
url = "https://tryproduck.com/api/mcp"
bearer_token_env_var = "PRODUCK_MCP_TOKEN"
```

**Claude Code** (manual, no plugin)
```bash
claude mcp add --transport http produck https://tryproduck.com/api/mcp \
  --header "Authorization: Bearer <YOUR_PRODUCK_MCP_TOKEN>"
```

## 3. Confirm

Your agent should now expose `search_feedback` and `get_feedback` (e.g. `/mcp` in Claude Code lists the
`produck` server connected). If a plugin install reports a missing token, complete step 1.
