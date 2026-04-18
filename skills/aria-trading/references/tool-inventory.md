# ARIA Tool Inventory — Complete Reference

All Clodds MCP tools available to ARIA. Load this file when you need the full parameter details for any tool category.

---

## CLODDS AI INTELLIGENCE (call directly — no API key cost)

| Tool | Purpose |
|------|---------|
| `clodds_research` | Deep AI research on any token, protocol, or narrative |
| `clodds_opinion` | AI market opinion on a trade thesis or token setup |
| `clodds_edge` | AI alpha discovery — underpriced setups, narrative gaps |
| `clodds_ai_strategy` | AI-generated trading strategy for specific conditions |

---

## LIVE SIGNALS & MARKET FEEDS

| Tool | Purpose |
|------|---------|
| `clodds_signals` | Live trading signals — momentum, breakout, reversal alerts |
| `clodds_news` | Crypto news filtered by token, protocol, or keyword |
| `clodds_feeds` | Raw data feeds — volume spikes, liquidations, anomalies |
| `clodds_divergence` | Price-vs-onchain divergence — leading reversal indicator |
| `clodds_metrics` | On-chain metrics — TVL, active wallets, txn count, fees |
| `clodds_analytics` | Token/portfolio analytics — wallet behavior, PnL, exposure |
| `clodds_market_index` | Fear/greed, BTC dominance, altcoin cycle, sector rotation |

---

## ON-CHAIN & WALLET INTELLIGENCE

| Tool | Purpose |
|------|---------|
| `clodds_whale_tracking` | Large wallet moves — accumulation, distribution, smart money |
| `clodds_token_security` | Rug check — mint authority, LP lock, holder concentration, honeypot |
| `clodds_solana_balance` | Live SOL + all SPL token balances on connected wallet |
| `clodds_solana_wallet` | Active wallet address |
| `clodds_portfolio_summary` | Full portfolio across all connected venues |
| `clodds_portfolio_pnl` | Real-time P&L per position |
| `clodds_portfolio_positions` | All open positions across all exchanges |
| `clodds_portfolio_sync` | Sync portfolio data across all venues |
| `clodds_bags` | Holdings with entry prices and unrealized P&L |
| `clodds_risk` | Portfolio risk — concentration, drawdown, correlation |

---

## PUMP.FUN INTELLIGENCE

All called as `clodds_pumpfun <subcommand>`:

| Subcommand | Returns |
|------------|---------|
| `trending` | Top tokens by 24h volume |
| `hot` | Most active by 1h transaction count |
| `gainers` | Top 24h price gainers |
| `losers` | Top 24h price losers |
| `new-hot` | Hottest new token launches by volume |
| `new` | Most recently created tokens |
| `graduated` | Tokens migrated from bonding curve to PumpSwap |
| `graduating` | Tokens >60% bonding curve filled — near graduation |
| `volatile` | Highest volatility tokens |
| `koth` | King of the Hill candidates (30–35K mcap zone) |
| `metas` | Trending narratives and meta plays |
| `search <query>` | Search by keyword or narrative |
| `stats <mint>` | Price, mcap, volume, liquidity, txns, price changes |
| `token <mint>` | Full profile — description, socials, creator wallet, links |
| `holders <mint>` | Top holder distribution and concentration |
| `trades <mint> --limit N` | Recent trade history — buys vs sells |
| `bonding <mint>` | Bonding curve state — % filled, SOL in curve, SOL to graduate |
| `chart <mint> --interval Xm/Xh` | OHLCV data for technical analysis |
| `similar <mint>` | Related or copycat tokens |
| `best-pool <mint>` | Best execution venue — PumpSwap vs Jupiter vs Raydium |
| `quote <mint> <amount> <buy/sell>` | On-chain accurate price quote before trading |
| `user-coins <address>` | All tokens created by a wallet |
| `sol-price` | Current SOL/USD price |
| `latest-trades --limit N` | Platform-wide recent trades |

---

## CEX PRICE & MARKET DATA

| Tool | Purpose |
|------|---------|
| `clodds_binance_spot_price` | Live Binance spot price (e.g. SOLUSDT, BTCUSDT) |
| `clodds_bybit_spot_price` | Live Bybit spot price |
| `clodds_mexc_spot_price` | Live MEXC spot price |
| `clodds_binance_spot_history` | Binance recent trade history |
| `clodds_bybit_spot_history` | Bybit recent trade history |
| `clodds_mexc_spot_history` | MEXC recent trade history |
| `clodds_binance_spot_orders` | Open orders on Binance account |
| `clodds_bybit_spot_orders` | Open orders on Bybit account |
| `clodds_mexc_spot_orders` | Open orders on MEXC account |
| `clodds_binance_spot_balance` | Binance wallet balances |
| `clodds_bybit_spot_balance` | Bybit wallet balances |
| `clodds_mexc_spot_balance` | MEXC wallet balances |
| `clodds_hyperliquid_balance` | Hyperliquid account balance |
| `clodds_hyperliquid_positions` | Open Hyperliquid perpetual positions |
| `clodds_binance_futures_positions` | Open Binance futures positions |
| `clodds_bybit_futures_positions` | Open Bybit futures positions |
| `clodds_mexc_futures_positions` | Open MEXC futures positions |
| `clodds_arbitrage` | Cross-exchange price discrepancy scanner |

---

## PREDICTION MARKETS & AI OPINION — first-class intelligence signal

These tools are **actively used in Phase 5 (social) and Phase 6 (macro) of every token analysis** — not optional, not reference-only. Prediction-market implied probabilities are smart-money probability estimates and fold directly into the Phase 7 scorecard (factor #7) and the Pre-Pump Signal Score (factor #6).

| Tool | Purpose | When to use |
|------|---------|-------------|
| `clodds_polymarket_markets "<query>"` | Search Polymarket — deep liquid crypto event odds | Query by ticker, project name, or narrative on every token analysis + every macro check |
| `clodds_polymarket_orderbook <market_id>` | Live orderbook — extract implied probability | Run on every market_id surfaced by `_markets` — `impliedProb = bestBid_YES / 100` or mid-market of bid+ask |
| `clodds_polymarket_positions` | Your open Polymarket positions | Only for portfolio / trade execution |
| `clodds_polymarket_balance` | Polymarket account balance | Only for trade execution |
| `clodds_polymarket_buy` / `_sell` | **[TRADE]** Polymarket execution | Only on explicit user execution request |
| `clodds_kalshi_markets "<query>"` | Kalshi regulated US binary markets — regulatory/macro events | Fed rate decisions, CPI prints, SEC/ETF approvals, macro triggers |
| `clodds_kalshi_orderbook <market_id>` | Kalshi orderbook depth | Extract implied prob on every relevant market |
| `clodds_kalshi_positions` / `_balance` / `_buy` / `_sell` | Portfolio + execution | Only when user is trading Kalshi directly |
| `clodds_metaculus "<query>"` | Metaculus community forecasts | Long-horizon crypto/macro question consensus — use alongside Polymarket to spot retail-vs-analyst divergences |
| `clodds_trading_manifold "<query>"` | Manifold play-money forecasts (retail-weighted) | Cross-check vs Polymarket — divergence >10pp = tradeable signal |
| `clodds_predictit` | PredictIt political/economic markets | US political events with crypto spillover (regulation, administration changes) |
| `clodds_predictfun` | PredictFun alt-data | Optional cross-check |
| `clodds_opinion "<TICKER> — buy/sell given [context]?"` | **AI-synthesized trade opinion** on the specific setup | MANDATORY on every token analysis — folds into Phase 5 sentiment block + Phase 7 factor #9 |

**Implied-probability extraction rule:**
```
bestBid_YES / 100           (most common, polymarket YES-side quote)
(bestBid + bestAsk) / 2 / 100   (mid-market, more stable)
```
If the orderbook is thin (spread >10pp), flag the prob as `LOW-CONFIDENCE` in the rendered block.

**Query patterns that work across all these tools:**
- By ticker: `"BTC"`, `"SOL"`, `"XRP"`
- By project name: `"Bitcoin"`, `"Solana"`, `"Ripple"`
- By narrative: `"ETF approval"`, `"rate cut"`, `"CPI"`, `"halving"`
- By specific event: `"SOL ETF"`, `"BTC 100k"`, `"Ripple vs SEC"`

**Fallback if any Clodds prediction-market tool returns help-text or no results:**
```
web_fetch https://polymarket.com/markets/crypto
web_fetch https://kalshi.com/markets/crypto
web_fetch https://manifold.markets/browse?topic=crypto
web_fetch https://opinion.trade
web_search "polymarket <TICKER> odds today"
```

---

## SOLANA DEX EXECUTION

| Tool | Purpose |
|------|---------|
| `clodds_jupiter_quote` | Live Jupiter swap quote — price impact and slippage |
| `clodds_solana_quote` | Alternative DEX quote — Raydium, Orca, PumpSwap routing |
| `clodds_pumpfun_balance` | pump.fun token holdings in wallet |
| `clodds_pumpfun_buy` | **[TRADE]** Buy pump.fun bonding curve token |
| `clodds_pumpfun_sell` | **[TRADE]** Sell pump.fun token |
| `clodds_jupiter_swap` | **[TRADE]** Execute Jupiter swap |
| `clodds_solana_swap` | **[TRADE]** Alternative Solana swap execution |

---

## CEX SPOT EXECUTION

| Tool | Purpose |
|------|---------|
| `clodds_binance_spot_buy` | **[TRADE]** Market or limit buy on Binance spot |
| `clodds_binance_spot_sell` | **[TRADE]** Market or limit sell on Binance spot |
| `clodds_bybit_spot_buy` | **[TRADE]** Buy on Bybit spot |
| `clodds_bybit_spot_sell` | **[TRADE]** Sell on Bybit spot |
| `clodds_mexc_spot_buy` | **[TRADE]** Buy on MEXC spot |
| `clodds_mexc_spot_sell` | **[TRADE]** Sell on MEXC spot |

---

## FUTURES EXECUTION

| Tool | Purpose |
|------|---------|
| `clodds_hyperliquid_long` | **[TRADE]** Open Hyperliquid long (market) |
| `clodds_hyperliquid_short` | **[TRADE]** Open Hyperliquid short (market) |
| `clodds_hyperliquid_close` | **[TRADE]** Close Hyperliquid position |
| `clodds_hyperliquid_leverage` | Set leverage for Hyperliquid symbol |
| `clodds_binance_futures_long` | **[TRADE]** Open Binance futures long |
| `clodds_binance_futures_short` | **[TRADE]** Open Binance futures short |
| `clodds_binance_futures_close` | **[TRADE]** Close Binance futures position |
| `clodds_binance_futures_leverage` | Set leverage on Binance |
| `clodds_bybit_futures_long` | **[TRADE]** Open Bybit long |
| `clodds_bybit_futures_short` | **[TRADE]** Open Bybit short |
| `clodds_bybit_futures_close` | **[TRADE]** Close Bybit position |
| `clodds_bybit_futures_leverage` | Set leverage on Bybit |
| `clodds_mexc_futures_long` | **[TRADE]** Open MEXC long |
| `clodds_mexc_futures_short` | **[TRADE]** Open MEXC short |
| `clodds_mexc_futures_close` | **[TRADE]** Close MEXC position |
| `clodds_mexc_futures_leverage` | Set leverage on MEXC |

---

## STRATEGY, ALERTS & AUTOMATION

| Tool | Purpose |
|------|---------|
| `clodds_alerts` | Register price/event alert thresholds (Tier 2 — notify) |
| `clodds_automation` | Configure auto-execution rules (Tier 1 — SL/TP/trailing) |
| `clodds_triggers` | Conditional event triggers on price, volume, on-chain |
| `clodds_monitoring` | Continuous position monitoring — check fired alert states |
| `clodds_strategy` | Build and manage rule-based trading strategies |
| `clodds_backtest` | Backtest strategy against historical data |
| `clodds_dca` | DCA scheduling and execution |
| `clodds_sizing` | Position sizing calculator |
| `clodds_risk` | Full portfolio risk assessment |

---

## SOLANA DEFI PROTOCOLS

| Tool | Purpose |
|------|---------|
| `clodds_drift` | Drift Protocol — Solana perpetuals and margin |
| `clodds_marginfi` | MarginFi — Solana lending and borrowing |
| `clodds_kamino` | Kamino Finance — Solana liquidity and yield |
| `clodds_orca` | Orca DEX — concentrated liquidity pools |
| `clodds_raydium` | Raydium AMM — Solana's primary DEX |
| `clodds_meteora` | Meteora — dynamic liquidity pools |

---

## CLAUDE NATIVE TOOLS (web intelligence — no API cost)

| Tool | Use For |
|------|---------|
| `web_search` | X/Twitter sentiment, social media, news, opinion.trade, price cross-reference, project research |
| `web_fetch` | DexScreener, Birdeye, Solscan, pump.fun, CoinGecko, CoinMarketCap, GitHub, audit reports |

**X/Twitter search formats:**
```
"$[TICKER]" site:x.com                    → recent X posts about a token
"[token name]" crypto twitter 2026        → broader social discussion
[token name] solana pump.fun              → pump.fun community chatter
[creator handle] [token] announcement    → creator/team mentions
"[token name]" whale buy OR sell         → whale social signals
[token name] rug OR scam                 → negative sentiment check
```

**Key fetch targets:**
```
https://dexscreener.com/solana/[mint]
https://birdeye.so/token/[mint]?chain=solana
https://solscan.io/token/[mint]
https://pump.fun/coin/[mint]
https://opinion.trade
```

## Data-source preference order (Binance + CoinGecko)

For **any Binance call** (klines, ticker, price, 24hr stats) and **any CoinGecko call** (global data, OHLC, market data, simple price), follow this preference chain. Check the current session's available tool list before choosing:

### Binance
1. **Preferred — Binance MCP** (any tool matching `mcp__*binance*__*` that the user has connected locally). Common function names to look for: `get_klines`, `klines`, `get_ticker`, `get_24hr_ticker`, `get_price`, `get_symbol_price_ticker`, `get_ticker_24hr`, `get_symbol_ticker`. Use whichever the installed server exposes.
2. **Fallback A — Clodds** `clodds_binance_spot_price` for simple price queries (Clodds does not expose klines, so klines skip this tier).
3. **Fallback B — `web_fetch` public REST** `https://api.binance.com/api/v3/...` (no auth). Only used if tiers 1 and 2 are unavailable or errored.

### CoinGecko
1. **Preferred — CoinGecko MCP** (any tool matching `mcp__*coingecko*__*`). Common function names: `get_coin_data`, `get_coin_ohlc`, `get_global`, `get_simple_price`, `get_coin_market_chart`, `get_coins_markets`. Use whichever the installed server exposes.
2. **Fallback — `web_fetch` public REST** `https://api.coingecko.com/api/v3/...` (no auth). Only used if tier 1 is unavailable or errored.

### Detection and fallback rule

- **If a matching MCP tool IS in the available tool list** → attempt the MCP call first. On error (timeout, auth, rate limit, unexpected shape), fall back to the next tier and note the degradation to the user in the Tool-call audit section at the end of the report.
- **If no matching MCP tool is in the list** → skip directly to the web_fetch tier. Do not try ToolSearch for a Binance/CoinGecko MCP inside a single analysis run — that's configuration, not runtime fallback.
- **Never dual-call** — don't hit both the MCP and the REST API for the same data. Pick one tier, use it, move on.

All URLs listed below in this file remain valid as the web_fetch fallback tier.

---

**Multi-timeframe OHLCV — primary data sources (all free, no auth):**

1. **CEX-listed tokens (Binance):** prefer Binance MCP `get_klines` / `klines` (tier 1 per preference chain above); fall back to the public REST endpoint:
```
https://api.binance.com/api/v3/klines?symbol=[SYMBOL]USDT&interval=1m&limit=200
https://api.binance.com/api/v3/klines?symbol=[SYMBOL]USDT&interval=5m&limit=200
https://api.binance.com/api/v3/klines?symbol=[SYMBOL]USDT&interval=15m&limit=200
https://api.binance.com/api/v3/klines?symbol=[SYMBOL]USDT&interval=1h&limit=200
https://api.binance.com/api/v3/klines?symbol=[SYMBOL]USDT&interval=4h&limit=200
```
Response per candle: `[openTime, open, high, low, close, volume, closeTime, quoteVolume, trades, takerBuyBase, takerBuyQuote, _]`.

2. **pump.fun bonding-curve / freshly graduated tokens:**
```
clodds_pumpfun chart <mint> --interval <1m|5m|15m|1h|4h>
```

3. **Any other Solana DEX token (Raydium / Orca / Meteora / Jupiter / fully graduated / never-on-pump.fun)** — GeckoTerminal OHLCV API:
```
# Find top pool:
https://api.geckoterminal.com/api/v2/networks/solana/tokens/[mint]/pools

# Pull OHLCV per timeframe:
1m:  https://api.geckoterminal.com/api/v2/networks/solana/pools/[pool]/ohlcv/minute?aggregate=1&limit=200
5m:  https://api.geckoterminal.com/api/v2/networks/solana/pools/[pool]/ohlcv/minute?aggregate=5&limit=200
15m: https://api.geckoterminal.com/api/v2/networks/solana/pools/[pool]/ohlcv/minute?aggregate=15&limit=200
1h:  https://api.geckoterminal.com/api/v2/networks/solana/pools/[pool]/ohlcv/hour?aggregate=1&limit=200
4h:  https://api.geckoterminal.com/api/v2/networks/solana/pools/[pool]/ohlcv/hour?aggregate=4&limit=200
```
Response: `data.attributes.ohlcv_list[]` where each entry is `[timestamp, open, high, low, close, volume]` — direct-feed into every indicator formula in `indicators.md`.

GeckoTerminal also supports `ethereum`, `base`, `bsc`, `arbitrum`, `polygon_pos`, and others — swap the network slug in the URL to cover non-Solana DEX tokens when needed.

All three sources feed the same indicator pipeline in `references/indicators.md`.

`clodds_x_research` is denied at the permission layer on this setup — use `web_search` for X/Twitter instead.

**Fallback for other unconfigured Clodds tools:** if `clodds_news`, `clodds_signals`, `clodds_whale_tracking`, `clodds_edge`, `clodds_analytics`, or `clodds_feeds` return empty/unconfigured responses, use `web_search` + `web_fetch` (CoinDesk, CoinGecko, The Block, DexScreener, Birdeye, Solscan) to fill the gap. Never halt analysis on a single missing source.
