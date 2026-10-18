# Local MCP server (Wealthos Local)

The desktop app ships its own MCP server. Your AI tool launches it
directly over stdio — **no port, no URL, no API key** — and it reads and
writes the same vault folder the app uses. Everything stays on your
machine.

For **Wealthos Cloud** accounts, see [remote.md](./remote.md) instead.

---

## Easiest path: copy it from the app

Open **Settings → Connect an AI assistant** in Wealthos Local, pick your
tool, and copy the ready-made command or config. Both machine-specific
paths are already filled in for you, and there's a one-click **Add to
Cursor** button.

Everything below is the same thing, for reference.

## The two values

| Placeholder | Value |
|---|---|
| `<binary>` | `/Applications/Wealthos.app/Contents/MacOS/wealthos-core` (standard install) |
| `<vault>` | Your data folder — shown in **Settings → Data folder** |

## Per tool

**Claude Code**

```bash
claude mcp add --scope user wealthos -- "<binary>" mcp-stdio "<vault>"
```
Verify with `/mcp`.

**Cursor** — click *Add to Cursor* in Settings, or add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "wealthos": {
      "command": "<binary>",
      "args": ["mcp-stdio", "<vault>"]
    }
  }
}
```
Verify under Settings → Tools & MCP.

**Claude Desktop** — same JSON as Cursor, via Settings → Developer → Edit
Config (`~/Library/Application Support/Claude/claude_desktop_config.json`).
Restart afterwards.

**Codex**

```bash
codex mcp add wealthos -- "<binary>" mcp-stdio "<vault>"
```
Or add `[mcp_servers.wealthos]` with `command` / `args` to
`~/.codex/config.toml`. Verify with `/mcp`.

**VS Code**

```bash
code --add-mcp '{"name":"wealthos","command":"<binary>","args":["mcp-stdio","<vault>"]}'
```

**OpenClaw**

```bash
openclaw mcp add wealthos --command "<binary>" --arg mcp-stdio --arg "<vault>"
```
Verify with `openclaw mcp doctor wealthos --probe`.

**Hermes** — in `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  wealthos:
    command: "<binary>"
    args: ["mcp-stdio", "<vault>"]
```
Restart `hermes chat`; servers are discovered at startup.

**OpenCode** — in `opencode.json` (project, or `~/.config/opencode/`):

```json
{
  "mcp": {
    "wealthos": {
      "type": "local",
      "command": ["<binary>", "mcp-stdio", "<vault>"],
      "enabled": true
    }
  }
}
```

**Any other MCP client** — three settings, whatever the tool calls them:

```
Command:    <binary>
Arguments:  mcp-stdio <vault>
Transport:  stdio
```

## Notes

- **Works whether Wealthos is open or not.** If the app is running, the
  MCP server routes through it; if not, it opens the vault directly.
- **Nothing is exposed.** No listening port, no token — only a process
  your tool starts and stops.
- The same **50 tools** as the hosted server (accounts, goals, plans,
  income, expenses, forecasts, metrics, currency, the affordability check,
  country rules). Destructive ones require an explicit `confirm: true`.
- **Scripts and terminal agents** can call the same tools without MCP:
  `<binary> call <vault> wealth_get` (or `npx wealthos call …`). Run
  `<binary> tools <vault> --text` for the list.

## Troubleshooting

| Problem | Fix |
|---|---|
| Tool can't find the server | Check `<binary>` points *inside* `Wealthos.app`, and that `<vault>` exactly matches Settings → Data folder |
| "No such file or directory" | Quote both paths — they usually contain spaces |
| Tools don't appear | Restart the AI tool; most only read MCP config at startup |

Get the app: **[wealthos.cc/local](https://wealthos.cc/local)**
