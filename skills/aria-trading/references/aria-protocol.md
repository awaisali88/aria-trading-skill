# ARIA Protocol — 9-Phase Analysis Pipeline

Run all phases in order for every analysis request. Show tool output from each phase before conclusions. Do not skip phases.

> 🔁 **Fallback rule (applies to every phase below):** If a Clodds tool returns empty, unconfigured, or off-topic output (e.g. `clodds_news` returning only political feeds, `clodds_signals` with no sources, `clodds_whale_tracking` with 0 wallets, `clodds_edge` returning prediction-market edges only, `clodds_analytics` with 0 opportunities, `clodds_feeds` empty), **silently fall back** to `web_search` + `web_fetch` on CoinDesk / CoinGecko / The Block / DexScreener / Birdeye / Solscan / X.com and keep the pipeline moving. Never halt or shrink the final report because one source was unconfigured — cross-reference, synthesize, and deliver the full 9-phase analysis every time. `clodds_x_research` is denied at the permission layer; always use `web_search` for X/Twitter.

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

## PHASE 3: TECHNICAL ANALYSIS — MULTI-TIMEFRAME CHART + INDICATORS

**This is the default Phase 3 behavior for every market/token analysis** — not just day trading. Every time ARIA is asked to analyze a market, scan for opportunities, or evaluate a token, it must run the full multi-timeframe chart analysis with the complete indicator suite on **1m · 5m · 15m · 1h · 4h**. Only extend to swing/position timeframes (1d / 3d / 1w) when the user explicitly says "swing", "long-term", "HODL", or specifies a holding window > 3 days — and even then, the 5 intraday timeframes still run.

**Always load `references/indicators.md` at the start of Phase 3.** That file contains the exact computation formula, interpretation rules, and chart-link format for every indicator below. Do not skip it — precise calculation matters, and the model should show the math path, not guess.

### Data sources per asset class

**CEX-listed tokens (Binance / Bybit / MEXC — e.g. SOL, XRP, TAO, BTC, ETH):**
Primary source is Binance's public klines endpoint (no auth required). Fetch each interval with `web_fetch`:
```
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=1m&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=5m&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=15m&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=1h&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=4h&limit=200
```
Each response is an array of `[openTime, open, high, low, close, volume, closeTime, quoteVolume, trades, takerBuyBase, takerBuyQuote, ignore]`. Calculate RSI, MACD, EMA, BB, VWAP, ATR from these arrays directly.

Supplement with `clodds_binance_spot_history <SYMBOL>USDT` for the most recent live trade tape (bid/ask imbalance).

**Solana tokens — decision tree (covers every Solana pair, not just pump.fun):**

1. **Token currently on pump.fun bonding curve or freshly graduated PumpSwap** → primary source:
   ```
   clodds_pumpfun chart <mint> --interval 1m
   clodds_pumpfun chart <mint> --interval 5m
   clodds_pumpfun chart <mint> --interval 15m
   clodds_pumpfun chart <mint> --interval 1h
   clodds_pumpfun chart <mint> --interval 4h
   ```

2. **Any other Solana token (Raydium / Orca / Meteora / Jupiter-only / fully graduated / never-on-pump.fun)** → GeckoTerminal OHLCV API (free, no auth):
   ```
   # Find the top pool for the mint:
   web_fetch https://api.geckoterminal.com/api/v2/networks/solana/tokens/<mint>/pools

   # Then pull OHLCV on all 5 intraday timeframes using the top pool address:
   1m:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=1&limit=200
   5m:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=5&limit=200
   15m: web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=15&limit=200
   1h:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/hour?aggregate=1&limit=200
   4h:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/hour?aggregate=4&limit=200
   ```
   Response: `data.attributes.ohlcv_list[]` where each entry is `[timestamp, open, high, low, close, volume]`. Feed directly into every indicator formula in `indicators.md` — no conversion needed.

3. **Try #1 first; on empty/error, fall back to #2.** If a token has *both* a pump.fun pool and a later Raydium/Meteora pool after graduation, `clodds_pumpfun chart` will only reflect the pump.fun pool's activity — prefer GeckoTerminal's top pool (highest liquidity) for post-graduation tokens.

4. **Visual confirmation (always include as a chart link, regardless of data source):**
   - `https://birdeye.so/token/<mint>?chain=solana` — embedded TradingView chart with default indicators, works for any Solana token
   - `https://dexscreener.com/solana/<mint>` — price + volume, all pools aggregated
   - `https://solscan.io/token/<mint>` — on-chain activity
   - `https://pump.fun/coin/<mint>` — only for pump.fun-origin tokens

**Last-resort fallback if GeckoTerminal also fails:** `web_fetch https://api.coingecko.com/api/v3/coins/<coin-id>/ohlc?vs_currency=usd&days=1` (≤1d returns 30m candles) and `...&days=7` (7d returns 4h candles). CoinGecko has no 1m/5m granularity, so day-trading precision degrades — flag this to the user if it happens.

### Per-timeframe block (run this full block for each of 1m, 5m, 15m, 1h, 4h)

For every timeframe, compute all indicators using the formulas in `references/indicators.md`, then emit:

```
═══ [TIMEFRAME] — $[TICKER] ════════════════════════════════════════
Trend:           UP / DOWN / RANGE  (HH-HL / LH-LL / oscillation)
Candle pattern:  [bull engulf / bear engulf / doji / hammer / shooting star / inside bar / none]
Chart structure: [flag / triangle / wedge / double top/bottom / H&S / inv H&S / channel]

Support & Resistance (algorithmic — see indicators.md S/R section):
  R3  $X.XX  (+X.XX%)  ← [why: prior swing high / ATH / liquidity pool]
  R2  $X.XX  (+X.XX%)
  R1  $X.XX  (+X.XX%)
      ─── CURRENT $X.XX ───
  S1  $X.XX  (-X.XX%)
  S2  $X.XX  (-X.XX%)
  S3  $X.XX  (-X.XX%)  ← [invalidation level]

Indicators (every one — no skipping; formulas in indicators.md):
  RSI(14):         XX.X  [OB >70 / neutral / OS <30]   divergence: [none/bull/bear]
  MACD(12,26,9):   line $X.XX  signal $X.XX  hist $X.XX  cross: [bull X now / bear X now / none]
  BB(20,2):        upper $X.XX  mid $X.XX  lower $X.XX  width X.X%  state: [squeeze/expand/riding upper/lower]
  VWAP (session):  $X.XX  — price [above / below] VWAP by X.X%
  EMA(9/21/50):    $X.XX / $X.XX / $X.XX  stack: [bull / bear / mixed]  price: [above all / below all / mixed]
  ATR(14):         $X.XX  (X.X% of price) — SL breathing room reference
  Stoch RSI:       K XX · D XX  zone: [OB/OS/mid]  cross: [bull / bear / none]  divergence: [none/bull/bear]
  OBV:             rising X% / flat / falling X% over last 20 candles — [accumulation / distribution]
  Volume:          last candle Xm vs 20-candle avg Xm = X.Xx ratio  → [spike / elevated / normal / fade]

Key observation: [one-line read specific to this timeframe]
```

### Multi-Timeframe Confluence Matrix (always produce)

After the five per-timeframe blocks, emit the confluence matrix:

```
┌─────────┬─────────┬──────────┬──────────┬──────────┬──────────────┐
│ TF      │ Trend   │ RSI      │ MACD     │ Vol      │ Signal       │
├─────────┼─────────┼──────────┼──────────┼──────────┼──────────────┤
│ 4h      │ UP/DN/R │ XX       │ BUL/BER  │ ↑/↓/→   │ 🟢/🟡/🔴      │
│ 1h      │ ...     │ ...      │ ...      │ ...      │ ...          │
│ 15m     │ ...     │ ...      │ ...      │ ...      │ ...          │
│ 5m      │ ...     │ ...      │ ...      │ ...      │ ...          │
│ 1m      │ ...     │ ...      │ ...      │ ...      │ ...          │
└─────────┴─────────┴──────────┴──────────┴──────────┴──────────────┘

Confluence score: X/5 bullish · X/5 bearish · X/5 mixed
Higher-TF bias (4h+1h): [BULLISH / BEARISH / NEUTRAL]
Entry-TF trigger (15m+5m): [ARMED / WAITING / INVALID]
Micro-TF timing (1m):      [HOT / COOLING]
```

### Day-trading role of each timeframe

Use this framework to translate the matrix into a trade decision:

| TF | Role | What it controls |
|----|------|---------------|
| **4h** | Macro bias | Are we swimming with or against the tide? If 4h is DOWN, only counter-trend scalps — no trend longs |
| **1h** | Structure | Where are the swing S/R levels? This anchors the trade plan's SL and TP2/TP3 |
| **15m** | Setup | Is the pattern forming (flag, pullback, breakout retest)? Entry zone comes from here |
| **5m** | Trigger | The actual entry candle — wait for bull/bear engulf, breakout close, or rejection wick at S/R |
| **1m** | Timing | Fine-tune fill — avoid wicks, chase less, use for scaling in/out |

### Day-trading entry rules (must all be true to enter long; invert for short)

1. **Higher-TF bias aligned:** 4h and 1h both trending in trade direction (or at least 1h trending + 4h range-neutral).
2. **Setup confirmed:** 15m shows a recognizable continuation or reversal pattern at a meaningful S/R level.
3. **Trigger fired:** 5m closed in trade direction through the setup level (not a wick — a close).
4. **Momentum healthy:** 15m RSI between 40–70 (longs) or 30–60 (shorts) — not already extended.
5. **Volume confirms:** 5m or 15m volume on the trigger candle ≥ 1.3× the 20-candle average.
6. **No immediate resistance:** next higher-TF resistance ≥ 1.5× the stop-loss distance (minimum 1:1.5 R:R before TP1).

If any of the six fails, mark the setup "WAITING" and tell the user what needs to happen to arm it.

### Stop-loss placement (day-trading)

- **Primary SL:** below the 15m swing low (long) or above the 15m swing high (short) — minus 1× ATR(14) of the 15m chart for breathing room.
- **Hard SL:** never wider than 1h S2 (long) or 1h R2 (short).
- **Invalidation SL (thesis broken):** 1h S3 (long) or 1h R3 (short) — if this breaks, exit even if the primary SL hasn't fired.

### Take-profit ladder (day-trading)

- **TP1:** nearest 15m resistance (long) / support (short) — sell 30–40%, move SL to breakeven.
- **TP2:** nearest 1h resistance / support — sell 30–40%.
- **TP3:** 1h R2 or 4h mid-structure — trail the final 20–30% at 1× ATR(1h) below peak.

### Day-trader momentum checks

- Volume spike with no price follow-through → distribution, fade the move.
- Price move on declining volume → weak, don't chase.
- 5m vs 15m direction — aligned = ride; opposing = wait for resolution.
- Failed 15m retest of prior breakdown → long reversal setup; failed retest of prior breakout → short reversal setup.
- First 1h candle of a new session (London/NY open) often sets the day's range — mark its high and low as key intraday levels.

### Output order for Phase 3

1. Five per-timeframe blocks (4h → 1h → 15m → 5m → 1m — top-down read)
2. Confluence matrix
3. Chart links block (always — for visual verification):
   ```
   📊 Charts — verify indicators visually:
     TradingView:  https://www.tradingview.com/chart/?symbol=BINANCE:[SYMBOL]USDT&interval=240
                   (intervals: 1 / 5 / 15 / 60 / 240 / D)
     Birdeye:      https://birdeye.so/token/[mint]?chain=solana   (Solana assets only)
     DexScreener:  https://dexscreener.com/solana/[mint]           (Solana assets only)

   On TradingView, open Indicators → add: RSI, MACD, Bollinger Bands, VWAP,
   EMA 9, EMA 21, EMA 50, ATR, Stoch RSI, OBV, Volume. Values above should
   match within ±2% of what you see on-chart.
   ```
4. Verdict line: `Setup: ARMED / WAITING / INVALID — [one-sentence reason, includes day-trade framing if intraday bias is decisive]`

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
