# ARIA Trading Skill

**ARIA** (Autonomous Research & Intelligence Analyst) is a crypto analyst and active trader skill for AI coding agents.

Built on the Clodds MCP tool suite, ARIA handles the full trading lifecycle:
- Deep market research and token analysis (9-phase protocol)
- Technical, on-chain, social, and macro analysis
- Portfolio-aware position sizing
- Trade execution on Solana DEXs and CEXs
- Automated SL/TP/trailing stop management
- Event-driven monitoring and alert re-analysis

---

## Installation

### Claude Code Plugin (recommended)

```bash
claude plugin install awaisali88/aria-trading-skill
```

Then configure your Clodds MCP connection:
```
/aria-trading:configure https://your-clodds.up.railway.app YOUR_TOKEN
```

The plugin automatically bundles the Clodds MCP server config — no manual `.mcp.json` editing needed.

### Multi-Agent Install (Cursor, Windsurf, Copilot, etc.)

```bash
npx skills add awaisali88/aria-trading-skill
```

Works with [40+ agents](https://skills.sh). You'll need to configure the Clodds MCP server manually in your agent's MCP settings.

### Claude Desktop / Claude Mobile

1. Open Claude → Projects → New Project → name it "ARIA Trader"
2. Open `prompts/claude-desktop-system-prompt.md`
3. Copy the full contents and paste into Project Instructions
4. Connect Clodds MCP in Claude Desktop settings (Settings → MCP Servers)

---

## Prerequisites

### Clodds MCP Server (required)

ARIA uses the Clodds MCP server for all trading tools. You need:

1. A running Clodds MCP server (local or hosted on Railway/Fly.io/etc.)
2. Exchange API keys configured in Clodds

**If using the Claude Code plugin**, set these environment variables and restart your session:
```bash
export CLODDS_MCP_URL="https://your-clodds.up.railway.app"
export CLODDS_MCP_TOKEN="your-mcp-auth-token"
```

**If using `npx skills add`**, add the Clodds MCP server to your agent's `.mcp.json`:
```json
{
  "clodds": {
    "type": "http",
    "url": "https://your-clodds.up.railway.app",
    "headers": {
      "Authorization": "Bearer your-mcp-auth-token"
    }
  }
}
```

### Exchange Accounts

Configure API keys in Clodds for any combination of:
- **CEX:** Binance, Bybit, MEXC, Hyperliquid
- **DEX:** Solana wallet (for Jupiter, pump.fun, Drift, etc.)
- **Optional:** Polymarket, Kalshi (prediction markets)
- **Optional:** Slack (trade notifications)

---

## What ARIA Does

### Analysis
- Full 9-phase ARIA Protocol on any token or market
- Pump.fun memecoin intelligence (trending, graduating, hot, security checks)
- On-chain: whale tracking, holder concentration, divergence detection
- Social: X/Twitter sentiment via web search, community health
- Macro: fear/greed, BTC/SOL trend, prediction market odds
- Composite ARIA score (0-100) with UP/DOWN/SIDEWAYS probability

### Trading
- Executes on: **Binance · Bybit · MEXC · Hyperliquid · pump.fun · Jupiter**
- Portfolio-first: always checks available balance before recommending size
- Every trade includes: Stop Loss + TP1/TP2/TP3 + Trailing Stop
- Explicit confirmation required before any execution
- Full fill report with automation confirmation after every trade

### Automated Monitoring (Clodds-native, no TradingView needed)

**Tier 1** — Clodds auto-executes independently (runs 24/7):
- Stop loss hit → auto-close position + Slack notification
- TP1 hit → auto-sell 30%, move SL to breakeven + notification
- Trailing stop → auto-close remainder + notification

**Tier 2** — Alert fires, you review with ARIA:
- TP2/TP3 levels → Slack notification → open Claude → "check my alerts"
- Volume spikes, whale moves, signal changes → notify → re-analyze → decide

---

## Usage

### Market Scanning
```
Scan the market
Any signals?
What's trending on pump.fun?
```

### Token Analysis
```
Analyze [token name or mint address]
Is [token] a buy?
Check security on [mint address]
```

### Trading
```
Buy 0.5 SOL of [token]          → checks balance, builds plan, asks to confirm
Sell my [token]                  → shows P&L, asks to confirm
Long SOL with 3x leverage        → builds futures plan with liquidation price
DCA 0.1 SOL into [token] daily   → sets up automated DCA
```

### Monitoring
```
Check my alerts                  → reviews all fired alerts, re-analyzes
Monitor my positions             → position health dashboard
What's my portfolio?             → full P&L across all venues
```

### Macro
```
Macro check
What's SOL doing?
What's BTC doing?
```

---

## Project Structure

```
aria-trading-skill/
├── .claude-plugin/
│   └── plugin.json                        ← Plugin manifest
├── .mcp.json                              ← Clodds MCP config (auto-configured)
├── skills/
│   ├── aria-trading/
│   │   ├── SKILL.md                       ← Core skill (triggers + workflow)
│   │   └── references/
│   │       ├── aria-protocol.md           ← 9-phase analysis pipeline
│   │       ├── event-system.md            ← Two-tier automation architecture
│   │       ├── tool-inventory.md          ← Full Clodds tool reference (100+ tools)
│   │       └── trade-execution.md         ← Trade formats, SL/TP/trailing rules
│   └── configure/
│       └── SKILL.md                       ← Setup skill (/aria-trading:configure)
├── prompts/
│   └── claude-desktop-system-prompt.md    ← System prompt for Claude Desktop/Mobile
└── README.md
```

---

## Disclaimer

All trading carries risk. ARIA's analysis is informational — every trade requires your explicit confirmation. Not financial advice.
