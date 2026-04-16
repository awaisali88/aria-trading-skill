# ARIA Trading Skill

**ARIA** (Autonomous Research & Intelligence Analyst) is a personal crypto analyst and active trader skill for Claude Code and Claude Desktop.

Built on the Clodds MCP tool suite, ARIA handles the full trading lifecycle:
- Deep market research and token analysis
- Technical, on-chain, social, and macro analysis
- Portfolio-aware position sizing
- Trade execution on Solana DEXs and CEXs
- Automated SL/TP/trailing stop management via Clodds
- Event-driven monitoring and alert re-analysis

---

## What ARIA Does

### Analysis
- Full 9-phase ARIA Protocol on any token or market
- Pump.fun memecoin intelligence (trending, graduating, hot, security checks)
- On-chain: whale tracking, holder concentration, divergence detection
- Social: X/Twitter sentiment via web search, community health
- Macro: fear/greed, BTC/SOL trend, prediction market odds
- Composite ARIA score (0–100) with UP/DOWN/SIDEWAYS probability

### Trading
- Executes on: **Binance · Bybit · MEXC · Hyperliquid · pump.fun · Jupiter**
- Portfolio-first: always checks available balance before recommending size
- Every trade includes: Stop Loss + TP1/TP2/TP3 + Trailing Stop — no exceptions
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

## File Structure

```
aria-trading-skill/
├── SKILL.md                          ← Core skill (triggers + workflow)
├── README.md                         ← This file
├── references/
│   ├── tool-inventory.md             ← Full Clodds tool reference
│   ├── aria-protocol.md              ← 9-phase analysis pipeline
│   ├── event-system.md               ← Two-tier automation architecture
│   └── trade-execution.md            ← Trade formats, SL/TP/trailing rules
└── prompts/
    └── claude-desktop-system-prompt.md ← System prompt for Claude Desktop Projects
```

---

## Requirements

- **Claude Code** or **Claude Desktop** (Pro or higher)
- **Clodds MCP** connected with your exchange API keys configured
  - Supported: Binance, Bybit, MEXC, Hyperliquid, Solana wallet
  - Optional: Polymarket, Kalshi accounts
- **Slack** (optional but recommended — Clodds sends trade notifications via Slack)

---

## Installation

### Claude Code

Clone the repo into your workspace:
```bash
git clone https://github.com/[your-username]/aria-trading-skill.git
```

Then in your Claude Code session, Claude will automatically detect and load `SKILL.md` when you ask anything trading-related. Or explicitly activate with:
```
Load the ARIA trading skill
```

### Claude Desktop — Projects

1. Open Claude Desktop → Projects → New Project → name it "ARIA Trader"
2. Open `prompts/claude-desktop-system-prompt.md`
3. Copy the full contents and paste into Project Instructions
4. Connect Clodds as your MCP server in Claude Desktop settings
5. Done — ARIA is active for every conversation in that project

---

## How to Use

### Market scanning
```
Scan the market
Any signals?
What's trending on pump.fun?
```

### Token analysis
```
Analyze [token name or mint address]
Is [token] a buy?
Check security on [mint address]
```

### Trading
```
Buy 0.5 SOL of [token]          → ARIA checks balance, builds plan, asks to confirm
Sell my [token]                 → ARIA shows P&L, asks to confirm
Long SOL with 3x leverage       → ARIA builds futures plan with liquidation price
DCA 0.1 SOL into [token] daily  → ARIA sets up automated DCA
```

### Monitoring
```
Check my alerts                 → ARIA reviews all fired Clodds alerts, re-analyzes
Monitor my positions            → ARIA shows position health dashboard
What's my portfolio?            → Full P&L across all venues
What's my PnL?
```

### Macro
```
Macro check
What's SOL doing?
What's BTC doing?
```

---

## Key Design Decisions

**Why no TradingView?**
Clodds `clodds_alerts`, `clodds_automation`, and `clodds_monitoring` handle all price threshold monitoring natively. Tier 1 rules (SL/TP/trailing) execute automatically within Clodds without needing Claude active. TradingView can be added later for indicator-based alerts (RSI crossovers, MACD flips) via webhook → n8n if needed.

**Why explicit confirmation on every trade?**
ARIA is an analyst-trader, not a fully autonomous bot. You stay in control of judgment calls — adding to positions, timing entries, deciding when a thesis has changed. The automation handles the risk management (SL/TP) so your capital is protected even when you're not watching.

**Why portfolio-first?**
Never trading more than available capital. ARIA always checks the live balance on the specific venue being used before recommending a size, so recommendations are always actionable.

---

## Disclaimer

This skill is for personal use. All trading carries risk. ARIA's analysis is informational — every trade requires your explicit confirmation. Not financial advice.
