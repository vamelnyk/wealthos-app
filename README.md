# Wealthos

**See your whole financial picture in one place — and where it's heading.**

Wealthos pulls every account you own into a single net-worth view, tracks
your goals and plans against it, and lets you ask *"what if I…"* and watch
the answer play out. It's built for people whose money doesn't fit one bank,
one currency, or one country.

**Website:** [wealthos.cc](https://wealthos.cc)

This repository is the public home for **downloads**, **AI-tool (MCP) setup
docs**, and **issues**. The application source is private.

---

## What it does

- **Net worth, everything included.** Bank, brokerage, cash, property, crypto,
  pensions — any account, in any currency, converted to one display currency
  at live rates. See the total, and the breakdown by category.
- **Goals with live progress.** Set a target ("€100k by 2029"), link it to an
  account, and Wealthos tracks the progress for you as balances change.
- **Plans and reference amounts.** Sketch out a purchase, a move, or a project
  as a plan with line items, then promote any plan into a tracked goal.
- **Forecast simulator.** Model income, expenses, and contributions forward in
  time — *"if I save €800/month, when do I hit my number?"* — and save the
  scenarios you want to compare.
- **Custom metrics.** Define your own wealth metrics (savings rate, runway,
  liquid vs. illiquid) and read them alongside the built-ins.
- **Your AI, connected.** A built-in MCP server lets Claude, Cursor, Codex and
  others work with all of the above in plain language.

## Two editions, one product

Wealthos comes in two editions built from a single codebase, so they stay
feature-comparable. You choose where your data lives.

|  | **Wealthos Cloud** | **Wealthos Local** |
|---|---|---|
| Where your data lives | Hosted by us, synced across devices | Plain files on your own computer |
| Access | Any browser | macOS desktop app |
| Price | Subscription | One-time purchase |
| Account required | Yes | No — no sign-up at all |
| Works offline | No | Yes, entirely |
| AI tools (MCP) | [Hosted server](./mcp/remote.md) | [Local server](./mcp/local.md) |
| Get it | [wealthos.cc](https://wealthos.cc) | [wealthos.cc/local](https://wealthos.cc/local) |

**Cloud** is the fastest way to start: open a browser, sign in, and your data
follows you across devices. **Local** trades sync for full ownership — no
account, no server, and it keeps working with no internet at all.

## Wealthos Local: it's just a folder

Most finance apps lock your data in a database you'll never see. Wealthos Local
writes plain **YAML** and **Markdown** into a folder *you* pick:

```
Wealthos/                  ← your folder, anywhere you like
  wealthos.vault.yaml
  accounts/
    savings-account.yaml
    brokerage.yaml
  goals/
    house-deposit.yaml
  income.jsonl
  expenses.jsonl
  OVERVIEW.md              ← generated, human-readable
```

Open it in any editor. Search it. Put it in Dropbox or a git repo. Copy the
folder to back it up. If Wealthos ever disappears, your data is still there —
and still readable. Nothing leaves your machine, not even to us.

### Download

**[⬇ Download for macOS](https://github.com/vamelnyk/wealthos-app/releases/latest/download/Wealthos-macOS.dmg)**
· Apple Silicon · macOS 12+ · signed and notarised by Apple

1. Download and open the `.dmg`, then drag **Wealthos** to Applications.
2. Launch it and pick a folder for your vault (or point it at an existing one).
3. Add your accounts — that's it, you're tracking.

**7-day free trial**, then a **one-time purchase ($50 / €45)**. No subscription,
no renewal. One licence activates **3 machines** and includes free updates
within the major version. Full details and screenshots:
**[wealthos.cc/local](https://wealthos.cc/local)**.

Older versions and release notes are on the
[Releases](https://github.com/vamelnyk/wealthos-app/releases) page.

## Connect your AI tools

Wealthos speaks the [Model Context Protocol](https://modelcontextprotocol.io),
so any MCP-compatible assistant — Claude, Cursor, Codex, VS Code, and others —
can work with your money data directly:

> *"What's my net worth?"* · *"Add my brokerage account with €12,400."*
> *"If I save €800 a month, when do I reach €100k?"* · *"Create a house-deposit
> goal for 2029 and link it to my savings."*

The same **46 tools** are available on both editions (accounts, goals, plans,
income, expenses, forecasts, metrics, currency). Destructive actions require an
explicit confirmation, so an assistant can't delete your data on a
misunderstanding.

- **Local** launches the server over stdio — no port, no URL, no API key.
  Settings → *Connect an AI assistant* gives you a ready-made command per tool.
- **Cloud** exposes a hosted server at `https://mcp.wealthos.cc` with OAuth or
  an API token.

→ **[Full MCP setup guide](./mcp/)** — covers both servers, tool by tool.

## Support & issues

Bug reports and feature requests are welcome in
[Issues](https://github.com/vamelnyk/wealthos-app/issues). For anything
involving your account or personal financial data, please email us instead of
posting publicly — don't include balances, statements, or vault contents in a
public issue.
