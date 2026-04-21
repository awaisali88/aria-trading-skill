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

ARIA uses a **tiered MCP strategy** — required, recommended, and optional. The skill detects MCPs at runtime using a use-if-present glob pattern (`mcp__*<name>*__*`), so install only what you need. Missing optionals degrade gracefully — the skill falls through to slower or less accurate routes but never halts.

### 🔴 Required

#### Clodds MCP Server

All trading-venue execution (Binance · Bybit · MEXC · Hyperliquid · pump.fun · Jupiter) plus on-chain, portfolio, alerts, and automation tools. Without this, ARIA cannot place trades or read live balances.

You need:
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

Configure API keys inside Clodds for any combination of:
- **CEX:** Binance, Bybit, MEXC, Hyperliquid
- **DEX:** Solana wallet (for Jupiter, pump.fun, Drift, etc.)
- **Prediction markets:** Polymarket, Kalshi (optional)
- **Notifications:** Slack (optional)

### 🟡 Strongly Recommended

These MCPs materially improve analysis accuracy. Skipping them means the skill falls through to slower or less reliable `web_fetch` routes with lower-quality data.

#### Alpaca MCP — paper-trading default

Makes the default execution mode **paper** (safe dry-run), routes US equity and supported-crypto trades to Alpaca's paper account. Unsupported assets (pump.fun/alt tokens) fall through to a journal-only simulated row. Set `ARIA_EXECUTION_MODE=live` to flip the default to live venues.

```bash
claude mcp add --scope user -e ALPACA_API_KEY=<key> -e ALPACA_SECRET_KEY=<secret> alpaca -- uvx alpaca-mcp-server
```
Get keys at [alpaca.markets/docs/api-documentation/paper-trading](https://alpaca.markets/docs/api-documentation/paper-trading/) (free paper account).

#### TwitterAPI.io MCP — tier-1 X/Twitter data

Replaces the flaky public Nitter rotation with a paid proxy that actually returns tweet JSON reliably. Used for the §1.4 X-status handler (tweet fetch + replies + engagement), post-velocity calculation, KOL tier tagging, and creator-handle legitimacy. **Cost: ~$0.01 per full memecoin analysis** (15 credits per tweet, pay-as-you-go).

```bash
claude mcp add-json --scope user twitterapi '{"command":"uvx","args":["--with","mcp==1.6.0","twitterapi-mcp"],"env":{"TWITTER_API_KEY":"<key>"}}'
```
Get key at [twitterapi.io](https://twitterapi.io). Note the `mcp==1.6.0` pin — the upstream package passes a `settings=` kwarg that newer `mcp` versions rejected.

#### Binance MCP — preferred CEX klines source

Tier-1 for any Binance call per `references/tool-inventory.md`'s data-source preference order. Without it, ARIA falls back to the public REST endpoint (works but unauthenticated and slower on busy intervals).

Install any Binance MCP implementation you trust — ARIA auto-detects any `mcp__*binance*__*` match in session.

#### CoinGecko MCP — preferred global/OHLC data source

Tier-1 for global market data, OHLC, and simple prices.

```bash
claude mcp add --transport sse --scope user coingecko https://mcp.api.coingecko.com/mcp
```

### 🔵 Optional — nice-to-have fallbacks

| MCP | When it helps | How to install |
|---|---|---|
| **Perplexity** (`mcp__*perplexity*__*`) | Last-resort tier-3 fallback when direct `web_fetch` returns 402/403/ECONNREFUSED and every other alternative fails. Social/meta/site-content only — **never used for price, OHLCV, balances, or execution quotes.** | Install any Perplexity MCP server — requires a Perplexity AI API key. Example: `claude mcp add --scope user -e PERPLEXITY_API_KEY=<key> perplexity -- npx -y server-perplexity-ask` |
| **Helius** (`mcp__*helius*__*`) | Alternative on-chain Solana RPC when solscan/gmgn return 403. Tier-2 fallback for token metadata and wallet lookups. | Any Helius MCP server — requires a Helius API key from [helius.dev](https://helius.dev) |
| **Prediction-market MCPs** (Polymarket, Kalshi, Metaculus, Manifold) | First-class intelligence signals in Phase 5 and Phase 6 when configured via Clodds. Each adds a data-point to the composite score. | Configured inside Clodds, no separate MCP install |

### Detection and graceful degradation

The skill's lazy-loaded `references/link-resolution.md` defines the full error-code dispatch table with ordered fallback tiers. When an optional MCP is missing, the next tier in the chain runs automatically. You can verify which MCPs are detected in your session with:
```bash
claude mcp list
```
Look for ARIA-relevant names (`clodds`, `alpaca`, `twitterapi`, `coingecko`, any `*binance*`, any `*perplexity*`, any `*helius*`).

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
│   │   └── references/                    ← Lazy-loaded on demand (token-minimal)
│   │       ├── aria-protocol.md           ← 9-phase analysis pipeline
│   │       ├── alpaca-paper.md            ← Paper-trading playbook (Alpaca)
│   │       ├── event-system.md            ← Two-tier automation architecture
│   │       ├── examples.md                ← Copy-paste prompt library
│   │       ├── indicators.md              ← RSI/MACD/BB/VWAP/EMA/ATR/Stoch/OBV formulas
│   │       ├── journal-system.md          ← Signal journal + status/stats commands
│   │       ├── link-resolution.md         ← 402/403/404 fallback dispatch + MCP tier routing
│   │       ├── predictive-scan.md         ← Forward-looking pre-pump scanner
│   │       ├── pumpfun-social-playbook.md ← Creator audit, KOL tiers, X-status handler §1.4
│   │       ├── report-checklist.md        ← Per-phase compliance checklist
│   │       ├── tool-inventory.md          ← Full tool reference (100+ tools, MCP preference chains)
│   │       └── trade-execution.md         ← Trade formats, SL/TP/trailing rules
│   └── configure/
│       └── SKILL.md                       ← Setup skill (/aria-trading:configure)
├── prompts/
│   └── claude-desktop-system-prompt.md    ← System prompt for Claude Desktop/Mobile
├── reports/                               ← Generated analysis reports + journal.jsonl
└── README.md
```

---

## Disclaimer

All trading carries risk. ARIA's analysis is informational — every trade requires your explicit confirmation. Not financial advice.
