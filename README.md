# Wealthos

**See your whole financial picture in one place — and where it's heading.**

Wealthos pulls every account you own into a single net-worth view, tracks
your goals and plans against it, and lets you ask *"what if I…"* and watch
the answer play out. It's built for people whose money doesn't fit one bank,
one currency, or one country.

**Website:** [wealthos.cc](https://wealthos.cc)

This repository is the public home for **downloads**, **AI-tool (MCP) setup
docs**, the **[command-line tool](#command-line-npx-wealthos)**, and
**issues**. The application source is private; the
[vault file format](#wealthos-local-its-just-a-folder) is open.

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
- **Ask before you buy.** *"Can I afford a €1,200 flight?"* gets a verdict
  from your real numbers, plus the trade-offs: a goal pushed back,
  investments to sell, a safety buffer breached.
- **Country money rules.** Sourced, dated rules for the cost of buying a home
  in 16 European countries, the Dutch 30% ruling and German freelancer net
  income, calculated for your situation.
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
  wealthos.vault.yaml      ← marks the folder as a vault
  settings.yaml
  accounts/
    3f9c1e2a-….yaml        ← one file per account, named by its id
  goals/
    8b0d4f71-….yaml
  plans/  simulations/  metrics/  …
  income.jsonl             ← one entry per line
  expenses.jsonl
  snapshots.jsonl          ← net worth over time
  OVERVIEW.md              ← generated, human-readable
  AGENTS.md                ← generated: the rules, for AI agents
```

Open it in any editor. Search it. Put it in Dropbox or a git repo. Copy the
folder to back it up. If Wealthos ever disappears, your data is still there —
and still readable. Nothing leaves your machine, not even to us.

### An open format

The folder layout is published as the **Open Wealth Format**, with a JSON
Schema for every file type, so any program can read and write a vault without
Wealthos:

- **Specification:** [wealthos.cc/open-wealth-format/v1/README.md](https://wealthos.cc/open-wealth-format/v1/README.md)
- **JSON Schemas:** [wealthos.cc/open-wealth-format/v1/index.json](https://wealthos.cc/open-wealth-format/v1/index.json)

Every vault also carries an `AGENTS.md`, so an AI agent with file access
(Claude Code, Codex, Cursor) learns the rules before it edits anything: money
is a decimal string, files are written whole, and generated files are never
edited by hand.

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

## Command line: `npx wealthos`

[`wealthos`](https://www.npmjs.com/package/wealthos) runs every Wealthos tool
from a terminal, for scripts and for AI agents that work through a shell
(Claude Code, Codex CLI, cron jobs). Output is JSON. It needs Node 20+ and is
MIT-licensed.

```sh
npx wealthos init ~/Documents/Wealthos --currency EUR          # create a vault
npx wealthos call ~/Documents/Wealthos wealth_get              # net worth, as JSON
npx wealthos call ~/Documents/Wealthos affordability_check amount=1200 what="flight" --text
npx wealthos doctor ~/Documents/Wealthos                       # validate every file
npx wealthos schema account                                    # JSON Schema for a file type
npx wealthos rules nl.expat_tax_regime gross_annual_salary=70000   # no account needed
```

- **On a Local vault**, pass the folder. If the desktop app has it open,
  commands go through the app, so they never collide with your edits. The
  app ships the same tool as `wealthos-core`.
- **On Wealthos Cloud**, pass `https://mcp.wealthos.cc` instead of a folder,
  with an API token in `WEALTHOS_TOKEN` (Settings → *Connect to AI tools via
  MCP*).

Full command list: the [npm page](https://www.npmjs.com/package/wealthos).

## Connect your AI tools

Wealthos speaks the [Model Context Protocol](https://modelcontextprotocol.io),
so any MCP-compatible assistant — Claude, Cursor, Codex, VS Code, and others —
can work with your money data directly:

> *"What's my net worth?"* · *"Add my brokerage account with €12,400."*
> *"If I save €800 a month, when do I reach €100k?"* · *"Create a house-deposit
> goal for 2029 and link it to my savings."* · *"Can I afford a €3,000 sofa
> this month?"*

The same **50 tools** are available on both editions (accounts, goals, plans,
income, expenses, forecasts, metrics, currency, the affordability check and
country rules). Destructive actions require an
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
