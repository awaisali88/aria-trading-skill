# ARIA Protocol — 9-Phase Analysis Pipeline

Run all phases in order for every analysis request. Show tool output from each phase before conclusions. Do not skip phases.

---

## PHASE 1: IDENTITY & SECURITY CHECK

**Tools:** `clodds_token_security` → `clodds_pumpfun token <mint>` → `clodds_research "[token]"` → `web_search "[token name] solana"` → `web_fetch [solscan or pump.fun page]`

Determine:
- Full name, ticker, mint address, chain, launch date
- Bonding curve or graduated? Which DEX?
- Creator wallet — anon or doxxed? Prior rug history?
- Narrative: meme / AI / celebrity / political / DeFi / RWA / other

**🚨 STOP if any rug flag triggers — do not continue to recommendation:**
- Mint authority not revoked
- LP not locked or locked <30 days
- Top 5 wallets hold >50% of supply
- Creator wallet shows prior rug history
- Honeypot detection positive

---

## PHASE 2: LIVE MARKET DATA SNAPSHOT

**Tools:** `clodds_pumpfun stats <mint>` → `clodds_pumpfun bonding <mint>` → `clodds_pumpfun trades <mint>` → `clodds_binance_spot_price` (if CEX-listed) → `clodds_jupiter_quote` (slippage) → `web_fetch dexscreener.com/solana/<mint>`

Pull and report every field:
- Price: current · 5m · 1h · 6h · 24h change
- Market cap and FDV
- 24h volume and 1h volume — accelerating or decelerating?
- Volume-to-market-cap ratio — flag >100% suspicious, >50% high activity
- Liquidity pool total USD depth
- Buy/sell transaction ratio (24h and 1h)
- Bonding curve: % filled · SOL in curve · SOL needed to graduate
- Graduation status: bonding / PumpSwap / Raydium / other DEX

**Slippage table (always include):**
```
Entry $500   → ~X.X%  |  Exit $500   → ~X.X%
Entry $2,000 → ~X.X%  |  Exit $2,000 → ~X.X%
Entry $5,000 → ~X.X%  |  Exit $5,000 → ~X.X%
```
*(impact ≈ tradeSize / (liquidityDepth × 2) × 100)*

---

## PHASE 3: TECHNICAL ANALYSIS

**Tools:** `clodds_pumpfun chart <mint> --interval 1h` → `clodds_pumpfun chart <mint> --interval 15m` → `clodds_ticks` → `web_fetch dexscreener chart page`

**Trend identification:**
- Higher highs + higher lows → Uptrend
- Lower highs + lower lows → Downtrend
- Oscillating between levels → Range/Consolidation

**Chart patterns to identify:**
Double top/bottom · Head and shoulders · Inverse H&S · Bull/Bear flag
Ascending/Descending triangle · Rising/Falling wedge · Cup and handle · Pennant

**Support & resistance map (always all 6 levels):**
```
R3  $X.XX  (+XX%)  ← ATH / extreme resistance
R2  $X.XX  (+XX%)  ← Major resistance / prior swing high
R1  $X.XX  (+XX%)  ← Near-term resistance
    ─── CURRENT $X.XX ───
S1  $X.XX  (-XX%)  ← Near-term support
S2  $X.XX  (-XX%)  ← Critical support / last defense
S3  $X.XX  (-XX%)  ← Death zone
```

**Indicators (calculate from returned price data):**
- **RSI (14):** >70 overbought / 30–70 neutral / <30 oversold · note divergences
- **MACD:** fast vs slow EMA position · histogram direction · crossover signal
- **Bollinger Bands:** above/below mid-band · band width (expanding/contracting)
- **VWAP:** above = bullish bias / below = bearish bias
- **OBV:** rising = accumulation / falling = distribution
- **EMA alignment:** 20 vs 50 — golden cross (bullish) or death cross (bearish)
- **Stochastic RSI:** >80 overbought / <20 oversold · divergence = reversal signal

**Momentum check:**
- Volume spike, no price follow-through → distribution (bearish)
- Price move on declining volume → weak momentum, likely to fade
- 1h direction vs 24h direction — aligned or diverging?
- Failed bounces in recent candles? → bearish confirmation
- Failed breakdowns? → bullish confirmation

---

## PHASE 4: ON-CHAIN INTELLIGENCE

**Tools:** `clodds_whale_tracking` → `clodds_metrics` → `clodds_divergence` → `clodds_analytics` → `web_fetch birdeye.so/token/<mint>?chain=solana`

Report:
- Whale direction: net accumulating (bullish) or net distributing (bearish)?
- Top 10 wallet concentration — flag if >40%
- Creator wallet holdings — has creator sold? What % remain?
- On-chain health — growing or shrinking active wallets?
- Price-vs-onchain divergence: price down but accumulating = hidden bullish signal
- LP lock status details

---

## PHASE 5: SOCIAL & SENTIMENT

**Tools:** `web_search "$[TICKER] crypto"` → `web_search "[token name] twitter pump.fun"` → `web_search "[token name] telegram"` → `web_fetch [project X page]` → `web_fetch opinion.trade` → `clodds_news [token]` → `clodds_feeds` → `clodds_opinion "[token] — buy or sell?"` → `clodds_edge "[token] — any asymmetric opportunity?"`

Report:
- X/Twitter: post volume 24h · sentiment · KOL mentions
- Community: active or ghost town?
- Opinion.trade: market sentiment pricing?
- Upcoming catalysts: listings · partnerships · unlocks · events
- Media coverage: any mainstream press?
- Shill risk: coordinated hype signals?

```
X/Twitter sentiment:  🟢 Bullish / 🟡 Neutral / 🔴 Bearish
Community activity:   Active / Moderate / Dead
KOL mentions:         Yes ([name]) / None
Catalyst pipeline:    Yes ([describe]) / None
Shill/manipulation:   Low / Medium / High
```

---

## PHASE 6: MACRO CONTEXT

**Tools:** `clodds_market_index` → `clodds_polymarket_markets "crypto"` → `clodds_polymarket_orderbook` → `clodds_binance_spot_price BTCUSDT` → `clodds_binance_spot_price SOLUSDT` → `web_search "crypto market today"` → `web_fetch opinion.trade`

Report:
- Fear/greed index and what it signals
- BTC 24h and 7d trend
- SOL 24h and 7d trend (critical for Solana tokens)
- Altcoin cycle position
- Memecoin sector temperature: hot / cooling / cold
- Polymarket/Kalshi macro events (rate decisions, regulatory, ETF flows)
- Does macro support or oppose this trade?

---

## PHASE 7: ARIA SCORE & PROBABILITY

Using data from all phases + `clodds_opinion "[token] — buy or sell?"`:

```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $[TICKER]                 │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  X/10 │                   │
│ 2.  RSI momentum            │  X/10 │                   │
│ 3.  MACD signal             │  X/10 │                   │
│ 4.  Volume momentum         │  X/10 │                   │
│ 5.  On-chain health         │  X/10 │                   │
│ 6.  Whale activity          │  X/10 │                   │
│ 7.  Social sentiment        │  X/10 │                   │
│ 8.  Liquidity & safety      │  X/10 │                   │
│ 9.  Narrative strength      │  X/10 │                   │
│ 10. Macro alignment         │  X/10 │                   │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ XX/100│                   │
└─────────────────────────────┴───────┴───────────────────┘

📈 Probability UP    (24h): XX%
📉 Probability DOWN  (24h): XX%
➡️ Probability SIDEWAYS:    XX%
```

Score: 80–100 Strong Buy · 65–79 Buy · 50–64 Speculative · 35–49 Avoid · 20–34 Sell · 0–19 Exit

---

## PHASE 8: PORTFOLIO CHECK & TRADE PLAN

See `references/trade-execution.md` for full format.

**Balance check tools by venue:**
- Solana: `clodds_solana_balance` + `clodds_pumpfun_balance`
- Binance: `clodds_binance_spot_balance`
- Bybit: `clodds_bybit_spot_balance`
- MEXC: `clodds_mexc_spot_balance`
- Hyperliquid: `clodds_hyperliquid_balance`
- All venues: `clodds_portfolio_summary` + `clodds_bags` + `clodds_risk`

---

## PHASE 9: ALERT & AUTOMATION SETUP

See `references/event-system.md` for full Tier 1 + Tier 2 configuration.

Immediately after every trade:
- Wire Tier 1: SL auto-close + TP1 auto-sell + trailing stop via `clodds_automation`
- Wire Tier 2: TP2/volume/whale alerts via `clodds_alerts`
- Activate: `clodds_monitoring` for continuous position health
