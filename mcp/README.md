# Wealthos MCP

Wealthos exposes its financial operations over the
[Model Context Protocol](https://modelcontextprotocol.io), so any
MCP-compatible AI tool can work with your money data directly:

> *"What's my net worth?"* · *"Add my new brokerage account with €12,400."*
> *"If I save €800 a month, when do I hit €100k?"* · *"Create a goal for a
> house deposit by 2029 and link it to my savings account."*

**46 tools** across accounts, goals, plans, income, expenses, forecasts,
metrics, and currency — the same set on both servers.

---

## Which server do I want?

That depends on which edition of Wealthos you use.

|  | **Hosted (remote)** | **Local (desktop)** |
|---|---|---|
| For | Wealthos Cloud users | Wealthos Local users |
| Endpoint | `https://mcp.wealthos.cc` | stdio — your AI tool launches the process |
| Transport | Streamable HTTP (JSON-RPC 2.0) | stdio |
| Auth | API token, or OAuth 2.1 + PKCE | **None needed** — no port is opened |
| Where data lives | Your Wealthos account | The vault folder on your machine |
| Works offline | No | Yes |
| Setup guide | **[remote.md](./remote.md)** | **[local.md](./local.md)** |

Not sure which edition you have? If you sign in at
[wealthos.cc](https://wealthos.cc) in a browser, you're on Cloud. If you
downloaded the desktop app, you're on Local.

## A note on the local server's security model

The local server is **stdio-only by design**. Your AI tool spawns the
`wealthos-core` process and talks to it over standard input/output — there
is no listening port, no URL, and no API key. Nothing else on your machine
(or network) can reach it, and no credential exists that could leak.

That's a deliberate trade-off against the more convenient
"paste this URL" style of local MCP server. This is financial data; we'd
rather it not be one stray `curl` away.

## Registry

The hosted server is listed in the official MCP registry as
`cc.wealthos/mcp`, so registry-aware clients can discover it without you
pasting a URL.
