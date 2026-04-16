# ARIA Trading Skill

## Project Overview

ARIA (Advanced Research & Intelligence Analyst) is a personal crypto analyst and active trader skill built for Claude Code and Claude Desktop. It gives Claude the ability to perform deep token analysis, execute trades across multiple exchanges, and manage automated monitoring — all through the Clodds MCP tool suite. ARIA combines a structured 9-phase analysis protocol with portfolio-aware trade execution and a two-tier automation system for 24/7 position management.

This is a personal tool built for Awais Ali. It is not a standalone application — it is a set of markdown documents that define Claude's behavior when activated as a skill. There is no code to compile, no server to run, and no tests to execute.

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Claude Code / Claude Desktop (Pro+ required) |
| Trading infrastructure | Clodds MCP server (100+ tools) |
| Exchanges (CEX) | Binance, Bybit, MEXC, Hyperliquid |
| DEX / On-chain | Solana (Jupiter, pump.fun, Drift, MarginFi, Kamino, Orca, Raydium, Meteora) |
| Prediction markets | Polymarket, Kalshi, PredictIt, Metaculus |
| Notifications | Slack (optional) |
| Social/sentiment | Claude native `web_search` (X/Twitter, news sites) |
| Data sources | DexScreener, Birdeye, Solscan, CoinGecko via `web_fetch` |

## Project Structure

```
aria-trading-skill/
├── CLAUDE.md                              # This file — project guide for Claude
├── README.md                              # User-facing intro and installation guide
├── SKILL.md                               # Core skill definition for Claude Code (trigger rules, workflow, rules)
├── .gitignore                             # Git ignore rules
├── references/                            # Detailed implementation guides (loaded on demand by SKILL.md)
│   ├── aria-protocol.md                   # 9-phase analysis pipeline methodology
│   ├── event-system.md                    # Two-tier automation architecture (Tier 1 auto-execute, Tier 2 alerts)
│   ├── tool-inventory.md                  # Complete Clodds MCP tool reference (100+ tools by category)
│   └── trade-execution.md                 # Mandatory trade sequence, position sizing, SL/TP/trailing rules
└── prompts/
    └── claude-desktop-system-prompt.md    # Standalone system prompt for Claude Desktop Projects mode (~42KB)
```

## How to Run

This is a documentation-only project — there is nothing to build, install, or test.

### Install as Claude Code Skill

```bash
# From within any project directory
claude skill add /path/to/aria-trading-skill
```

### Install for Claude Desktop

1. Open Claude Desktop → Projects
2. Create or open a project
3. Copy the contents of `prompts/claude-desktop-system-prompt.md` into the project's system prompt

### Prerequisites

- Claude Code or Claude Desktop with Pro+ subscription
- Clodds MCP server connected with exchange API keys configured
- Exchange accounts with API keys (Binance, Bybit, MEXC, Hyperliquid, and/or Solana wallet)
- Optional: Slack workspace for trade notifications

## Key Commands

Since ARIA is a skill (not a CLI), "commands" are natural-language invocations:

| Invocation | What ARIA Does |
|---|---|
| `"scan the market"` | Pull signals, news, feeds, top movers; summarize opportunities |
| `"analyze [TOKEN]"` | Run full 9-phase ARIA Protocol, output Signal Block |
| `"check pump.fun"` | Pull trending, hot, gainers, graduating tokens from pump.fun |
| `"trade plan for [TOKEN]"` | Build entry/SL/TP plan with position sizing from live balance |
| `"make the trade"` / `"execute"` | Execute the confirmed trade plan, wire all automation |
| `"check my alerts"` | Pull all fired alerts, re-analyze each, present updated signals |
| `"monitor my positions"` | Show Position Health Dashboard with P&L + automation status |
| `"close [TOKEN]"` | Close position, deactivate all automation for that token |
| `"what's the macro?"` | Fear/greed, BTC/SOL trends, prediction markets, cycle position |
| `"check my portfolio"` | Pull balances, positions, P&L across all connected venues |

## Architecture Notes

### File Relationship Model

```
SKILL.md (entry point for Claude Code)
  ├─ references/aria-protocol.md    → loaded when analysis is needed
  ├─ references/trade-execution.md  → loaded when trading is needed
  ├─ references/event-system.md     → loaded when setting up automation
  └─ references/tool-inventory.md   → loaded when tool lookup is needed

prompts/claude-desktop-system-prompt.md  → standalone (contains everything inline)
```

`SKILL.md` is the Claude Code entry point — it contains trigger keywords, identity, rules, and pointers to reference files. The reference files are loaded on demand. The Claude Desktop prompt is a self-contained ~42KB document that inlines all the same logic.

### Two-Tier Automation Model

- **Tier 1 (Auto-Execute):** Clodds runs 24/7 independently. Handles stop-loss, take-profit (TP1), and trailing stops. Sends Slack notifications on execution. No Claude session needed.
- **Tier 2 (Alert → Re-Analyze):** Clodds fires alerts (TP2 targets, volume spikes, whale moves) to Slack. User triggers `"check my alerts"` which causes ARIA to re-run analysis with fresh data and present updated recommendations.

### Mandatory Trade Sequence

Every trade follows this exact order — no shortcuts:

1. Check balance on target venue
2. Calculate position size from available capital (rules in `trade-execution.md`)
3. Build full trade plan (entry, SL, TP1/TP2/TP3, trailing stop)
4. Show confirmation format — **wait for explicit user approval**
5. Execute only on explicit confirmation
6. Wire ALL Tier 1 + Tier 2 automation immediately after fill
7. Report fill details

### 9-Phase ARIA Protocol

1. Identity & Security Check
2. Live Market Data
3. Technical Analysis (S/R levels, RSI, MACD, BB, VWAP, OBV, EMA)
4. On-Chain Intelligence (whale tracking, holder concentration, LP lock)
5. Social & Sentiment (X/Twitter, KOL mentions, shill risk)
6. Macro Context (fear/greed, BTC/SOL trends, cycle position)
7. ARIA Score & Probability (10-factor scorecard → 0–100 composite)
8. Portfolio Check & Trade Plan
9. Alert & Automation Setup

### Key Design Decisions

- **No TradingView dependency** — Clodds tools handle all price monitoring natively
- **Never autonomous trading** — every trade requires explicit user confirmation
- **Portfolio-first sizing** — always check live balance before recommending size
- **Fresh data always** — never use cached/memorized prices or balances
- **`web_search` for social** — never call `clodds_x_research` (not configured); use Claude's native `web_search`

## Important Files

| File | Purpose |
|---|---|
| `SKILL.md` | Core skill definition — trigger rules, identity, workflow, rules, communication style |
| `references/aria-protocol.md` | The 9-phase analysis methodology — the heart of ARIA's analytical capability |
| `references/trade-execution.md` | Position sizing tables, SL/TP rules, mandatory trade sequence, confirmation templates |
| `references/event-system.md` | Two-tier automation architecture, alert types, "check my alerts" workflow |
| `references/tool-inventory.md` | Complete catalog of 100+ Clodds MCP tools organized by category |
| `prompts/claude-desktop-system-prompt.md` | Standalone system prompt for Claude Desktop (all logic inlined, ~42KB) |
| `README.md` | User-facing installation guide and usage examples |

## Environment Variables

This project has no environment variables of its own. All configuration lives in:

- **Clodds MCP server** — exchange API keys (Binance, Bybit, MEXC, Hyperliquid), Solana wallet keys
- **Slack** — webhook/bot configuration for trade notifications (optional)
- **Claude Code / Desktop** — MCP server connection settings

## Conventions & Style

- **All files are Markdown** — no code, no config files, no build artifacts
- **SKILL.md uses frontmatter-style metadata** at the top (name, triggers, description)
- **Reference files are loaded on demand** — SKILL.md tells Claude when to load each one
- **Signal Block format** — every analysis must end with a structured ASCII signal block (template in `aria-protocol.md`)
- **Confirmation format** — every trade must show a structured confirmation box and wait for approval (template in `trade-execution.md`)
- **Position sizing is table-driven** — rules are based on market cap brackets, not arbitrary percentages
- **Hard limits enforced** — max 20% single memecoin, max 40% total memecoin exposure
- **Communication style** — lead with verdict, be direct, quantify everything, use emojis only for urgency flags

## What Not to Touch

- **`prompts/claude-desktop-system-prompt.md`** — This is a compiled standalone prompt. If you update reference files or SKILL.md, this file must be manually regenerated to stay in sync. Do not partially edit it.
- **Position sizing tables in `trade-execution.md`** — These are calibrated risk management rules. Changes affect real money.
- **Tier 1 automation rules in `event-system.md`** — These execute automatically without user confirmation. Incorrect changes could cause unintended trades.

## Current State & Known Issues

- **Single-user design** — All references, sizing rules, and risk limits are calibrated for one person (Awais Ali). Not designed for multi-user deployment.
- **Desktop prompt drift risk** — `prompts/claude-desktop-system-prompt.md` is a manual copy of logic from SKILL.md + references. There is no build step to keep them in sync. Edits to reference files must be manually propagated to the desktop prompt.
- **`clodds_x_research` is broken** — The tool exists in the Clodds suite but is not configured. All social/sentiment research must go through Claude's native `web_search`. This is documented but easy to forget.
- **No automated tests** — Being a pure-documentation skill, there are no tests. Correctness depends on manual review and live usage.
- **No versioning** — There is no VERSION file or changelog. The single git commit (`a5716a3`) represents v1.0.
