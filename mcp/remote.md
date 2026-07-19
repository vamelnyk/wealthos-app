# Hosted MCP server (Wealthos Cloud)

```
https://mcp.wealthos.cc
```

Transport: JSON-RPC 2.0 over Streamable HTTP. For **Wealthos Cloud**
accounts — if you use the desktop app, see [local.md](./local.md) instead.

---

## Two ways to connect

### 1. OAuth — nothing to set up first

Most modern clients (claude.ai, and anything supporting Dynamic Client
Registration) need only the URL:

1. In your AI tool, add a custom connector / MCP server and paste
   `https://mcp.wealthos.cc`.
2. The tool registers itself and opens the Wealthos consent screen.
3. Sign in, approve, and you're connected.

A matching entry appears under **Settings → Connect to AI tools via MCP**
in Wealthos, which you can revoke at any time.

### 2. API token — for tools that want a static header

Generate one in Wealthos: **Settings → Connect to AI tools via MCP → New
connection**. The token is shown **once** — store it like a password.

**Claude Desktop** — `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "wealthos": {
      "url": "https://mcp.wealthos.cc",
      "headers": { "Authorization": "Bearer wos_<your-token>" }
    }
  }
}
```

**Cursor** — `~/.cursor/mcp.json`: same shape as above.

**Claude Code**:

```bash
claude mcp add --transport http wealthos https://mcp.wealthos.cc \
  --header "Authorization: Bearer wos_<your-token>"
```

Restart the tool afterwards; Wealthos tools then appear in its tool list.

## What the tools can do

| Area | Examples |
|---|---|
| Net worth | Total across accounts, per-category breakdown, in your currency |
| Accounts | List, add, update, delete; custom categories |
| Goals | Create, update progress, link to an account for auto-tracking |
| Plans | Build plans, line items, reference amounts, promote to goals |
| Income & expenses | Recurring and one-time entries |
| Forecasts | Run a simulation, or run and save it as a scenario |
| Metrics | Define and read custom wealth metrics |
| Preferences | Read/change display currency (migrates goal targets at live rates) |

Destructive tools require an explicit `confirm: true`, so an AI can't
delete your data on a misunderstanding.

## Security

- A token grants the same access as your web session — treat it like a
  password, and revoke anything you suspect has leaked (revocation takes
  effect on the next request).
- Rate limit: 60 requests/minute per token.
- Sensitive amounts stay encrypted at rest and are decrypted only for the
  calling user.

## More

Product page with the full tool catalogue: **[wealthos.cc/mcp](https://wealthos.cc/mcp)**
