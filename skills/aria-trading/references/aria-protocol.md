# ARIA Protocol — 9-Phase Analysis Pipeline

Run all phases in order for every analysis request. Show tool output from each phase before conclusions. Do not skip phases.

> 🔁 **Fallback rule (applies to every phase below):** If a Clodds tool returns empty, unconfigured, or off-topic output (e.g. `clodds_news` returning only political feeds, `clodds_signals` with no sources, `clodds_whale_tracking` with 0 wallets, `clodds_edge` returning prediction-market edges only, `clodds_analytics` with 0 opportunities, `clodds_feeds` empty), **silently fall back** to `web_search` + `web_fetch` on CoinDesk / CoinGecko / The Block / DexScreener / Birdeye / Solscan / X.com and keep the pipeline moving. Never halt or shrink the final report because one source was unconfigured — cross-reference, synthesize, and deliver the full 9-phase analysis every time. `clodds_x_research` is denied at the permission layer; always use `web_search` for X/Twitter.

---

## PHASE 1: IDENTITY & SECURITY CHECK

**Tools:** `clodds_token_security` → `clodds_pumpfun token <mint>` → `clodds_research "[token]"` → `web_search "[token name] solana"` → `web_fetch [solscan or pump.fun page]`

**MUST INCLUDE for every token (skipping any item fails the report):**
- Full name, ticker, mint address, chain, launch date
- Bonding curve or graduated? Which DEX?
- Creator wallet — anon or doxxed? Prior rug history?
- Narrative: meme / AI / celebrity / political / DeFi / RWA / other
- Mint authority status (revoked / not revoked / unknown)
- LP lock status (locked / unlocked / lock duration)
- Top 5 wallet concentration % (or top 10 if top 5 unavailable)

**Fallback chain when `clodds_token_security` errors / returns "Unknown skill":** run *every* step below, do not skip:
```
1. web_fetch https://rugcheck.xyz/tokens/<mint>            → rug-risk score + LP lock + mint authority
2. web_fetch https://api.geckoterminal.com/api/v2/networks/solana/tokens/<mint>/info
                                                           → supply, top holders %, freeze authority
3. web_fetch https://solscan.io/token/<mint>#holders       → top 10 holder distribution
4. clodds_pumpfun token <mint>                             → creator wallet, bonding state
```
If 3 of 4 fallbacks also fail AND the token is <7 days old, render this banner and cap recommended size to 1% of memecoin bag regardless of TA verdict:
```
⚠ INSUFFICIENT SECURITY DATA — TREAT AS PRESUMED-RISK
   Cannot verify mint authority, LP lock, or holder distribution.
   Position cap: 1% of memecoin bag.
```

**🚨 HARD STOP — DO NOT RECOMMEND if ANY of the following fires.** Replace the trade plan with a STOP banner; the report still runs all other phases for educational context, but Phase 8 produces no entry plan:
- Mint authority not revoked
- LP not locked or locked <30 days
- Top 5 wallets hold >50% of supply
- Creator wallet shows prior rug history
- Honeypot detection positive
- Insider-cluster flag from any external source (e.g. CryptoSlate / RugCheck wallet-cluster reports)

STOP banner format:
```
🚨🚨🚨 STOP — DO NOT RECOMMEND $[TICKER] 🚨🚨🚨
Trigger: [which flag(s) fired]
Source:  [tool / URL]
Phase 8 trade plan SUPPRESSED. Educational analysis continues below.
```

---

## PHASE 2: LIVE MARKET DATA SNAPSHOT

**Tools:** `clodds_pumpfun stats <mint>` → `clodds_pumpfun bonding <mint>` → `clodds_pumpfun trades <mint>` → `clodds_binance_spot_price` (if CEX-listed) → `clodds_jupiter_quote` (slippage) → `web_fetch dexscreener.com/solana/<mint>`

**MUST INCLUDE for every token (every item — skipping any one fails the report):**
- **Price changes — all five intervals:** 5m / 1h / 6h / 24h / 7d. Fallback chain if any interval is missing from the primary source:
  ```
  1. web_fetch https://api.geckoterminal.com/api/v2/networks/solana/tokens/<mint>/info
     → returns price_change_percentage for m5 / h1 / h6 / h24
  2. For 7d on any token (CEX or DEX), pull daily klines and compute:
     CEX: prefer Binance MCP `get_klines` (tier 1 per tool-inventory.md Data-source preference order); fallback `web_fetch https://api.binance.com/api/v3/klines?symbol=<SYM>USDT&interval=1d&limit=8`
     DEX: web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/day?aggregate=1&limit=8
     7d Δ = (close[latest] - close[7 days ago]) / close[7 days ago] × 100
  3. Only if the token is <7 days old AND daily klines return <7 bars, label 7d as "n/a (token <7d old)" — never say "n/a from this snapshot" without attempting the klines fallback.
  ```
- **Volume:** 1h and 24h, with vol/mcap ratio. Flag >100% as suspicious, >50% as high activity.
- **Buy/sell tx split (last 1h):** count buys vs sells. Pull from `web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/trades?limit=100` and count by `kind` field. Report as `Buys: N (X%) · Sells: N (X%)`.
- **Liquidity** (USD), **MCap**, **FDV**, and circulating-supply ratio.
- **Bonding curve %** filled / SOL in curve / SOL to graduate (if pre-graduation).
- **Graduation status:** bonding / PumpSwap / Raydium / Orca / Meteora / other DEX.

**Slippage table — MANDATORY for every memecoin with liquidity <$10M.** Render every row:
```
Slippage (impact ≈ tradeSize / (liquidityDepth × 2) × 100):
  Entry $50    → ~X.X%   |  Exit $50    → ~X.X%
  Entry $100   → ~X.X%   |  Exit $100   → ~X.X%
  Entry $500   → ~X.X%   |  Exit $500   → ~X.X%
  Entry $1,000 → ~X.X%   |  Exit $1,000 → ~X.X%
  Entry $5,000 → ~X.X%   |  Exit $5,000 → ~X.X%
```
For CEX-listed tokens (deep books), a single line "negligible at <$10K notional" is acceptable instead.

---

## PHASE 3: TECHNICAL ANALYSIS — MULTI-TIMEFRAME CHART + INDICATORS

**This is the default Phase 3 behavior for every market/token analysis** — not just day trading. Every time ARIA is asked to analyze a market, scan for opportunities, or evaluate a token, it must run the full multi-timeframe chart analysis with the complete indicator suite on **1m · 5m · 15m · 1h · 4h**. Only extend to swing/position timeframes (1d / 3d / 1w) when the user explicitly says "swing", "long-term", "HODL", or specifies a holding window > 3 days — and even then, the 5 intraday timeframes still run.

**Always load `references/indicators.md` at the start of Phase 3.** That file contains the exact computation formula, interpretation rules, and chart-link format for every indicator below. Do not skip it — precise calculation matters, and the model should show the math path, not guess.

**🚫 No "derived" timeframes.** Every one of the 5 timeframes must be COMPUTED from raw OHLCV pulled from the data source (Binance klines / pump.fun / GeckoTerminal). Phrases like "5m derived from 15m structure" or "1m: heavy noise, likely RSI 30↔70" are NOT acceptable — they are a Phase 3 failure. If GeckoTerminal returns fewer than 30 candles for a timeframe, render the per-TF block with header `INSUFFICIENT DATA — n bars only` and skip the indicator values for that TF (do not fabricate). Always pull explicitly:
```
1m:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=1&limit=200
5m:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=5&limit=200
```
For CEX assets, use the corresponding Binance klines URLs at `interval=1m` and `interval=5m` already documented in `indicators.md`.

### Data sources per asset class

**CEX-listed tokens (Binance / Bybit / MEXC — e.g. SOL, XRP, TAO, BTC, ETH):**

Follow the Binance data-source preference order in `tool-inventory.md`:
1. **Preferred — Binance MCP** (tool matching `mcp__*binance*__*` with a klines-type function such as `get_klines` / `klines`). Call it five times, one per interval `1m`, `5m`, `15m`, `1h`, `4h`, `limit=200`.
2. **Fallback — web_fetch public REST** (no auth). Use these URLs if no Binance MCP tool is available or it errored:
```
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=1m&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=5m&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=15m&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=1h&limit=200
https://api.binance.com/api/v3/klines?symbol=<SYMBOL>USDT&interval=4h&limit=200
```
Response shape is identical either way: array of `[openTime, open, high, low, close, volume, closeTime, quoteVolume, trades, takerBuyBase, takerBuyQuote, ignore]`. Calculate RSI, MACD, EMA, BB, VWAP, ATR from these arrays directly.

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

**Last-resort fallback if GeckoTerminal also fails:** follow the CoinGecko preference chain in `tool-inventory.md` — prefer CoinGecko MCP `get_coin_ohlc` (tier 1); fall back to `web_fetch https://api.coingecko.com/api/v3/coins/<coin-id>/ohlc?vs_currency=usd&days=1` (≤1d returns 30m candles) and `...&days=7` (7d returns 4h candles). CoinGecko has no 1m/5m granularity, so day-trading precision degrades — flag this to the user if it happens.

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

**MUST INCLUDE for every token (every item — use fallback chain when Clodds tools fail / return empty):**
- **Top 10 holder concentration % + flag if >40%.** Fallbacks:
  ```
  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/info     → top holders
  web_fetch https://solscan.io/token/<mint>#holders                                    → top 10 list
  web_fetch https://birdeye.so/token/<mint>?chain=solana                               → holder distribution
  ```
- **Creator wallet status — has creator sold? What % remains?** **MANDATORY fetch — do not mark UNKNOWN without running it:**
  ```
  1. web_fetch https://solscan.io/account/<creator_wallet>                             → wallet holdings + tx history
  2. If solscan 403s or renders empty: web_fetch https://api.solscan.io/account/tokens?account=<creator_wallet>
  3. If both fail: web_fetch https://gmgn.ai/sol/address/<creator_wallet>              → GMGN profiler often mirrors creator data
  ```
  Report as `Creator holds X% of supply, last sell N days ago` or `Creator fully exited N days ago`. Only mark `UNKNOWN` if all three fallbacks fail — and in that case, state which URLs were tried in the PARTIAL banner.
- **Whale direction (last 24h): net accumulation vs distribution.** Fallback:
  ```
  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/trades?limit=100
  ```
  Filter trades by USD size > $5K; sum buy USD vs sell USD; report `Net whale flow: +$XXk (accumulation)` or `-$XXk (distribution)`.
- **LP lock status:** locked / unlocked / X days remaining (cross-reference Phase 1 fallback chain output).
- **Price-vs-OBV divergence:** computed from Phase 3 OBV data — if price made a higher high while OBV made a lower high (bearish div) or vice versa (bullish div), report it explicitly.
- **On-chain health:** active wallets trend over last 24h (growing / shrinking / flat) — pull from GeckoTerminal pool info or Birdeye.

If three or more of the above cannot be filled even after the fallback chain, render `⚠ ON-CHAIN DATA PARTIAL — [list missing items]` at the top of the Phase 4 block.

---

## PHASE 5: SOCIAL & SENTIMENT

**Tools:** `web_search "$[TICKER] crypto"` → `web_search "[token name] twitter pump.fun"` → `web_search "[token name] telegram"` → `web_fetch [project X page]` → `web_fetch opinion.trade` → `clodds_news [token]` → `clodds_feeds` → `clodds_opinion "[token] — buy or sell?"` → `clodds_edge "[token] — any asymmetric opportunity?"`

**MUST RENDER the standardized sentiment block — never skip, never collapse, never replace with prose.** This block is mandatory for every token analyzed:

```
X/Twitter sentiment:  🟢 Bullish / 🟡 Neutral / 🔴 Bearish
X post count (24h):   ~XX posts (estimate from web_search results)
Community activity:   Active / Moderate / Dead
KOL mentions:         Yes ([@handle, ~XXk followers]) / None
Catalyst pipeline:    Yes ([describe, ETA]) / None
Shill/manipulation:   Low / Medium / High
Mainstream coverage:  Yes ([outlet, headline, date]) / None
```

**How to fill the block:**
- **X post count:** run `web_search "$<TICKER> site:x.com"` and `web_search "[token name] site:x.com"`. Count distinct results from the last 24h. If under 10, mark as `LOW (~N posts)`; if 10–50 mark `MODERATE`; if >50 mark `ACTIVE`. State the count.
- **KOL mentions:** scan the search results for accounts with >100K followers, blue checks, or known crypto-KOL handles. List by handle with follower count if visible. Report `None` only if you actually checked.
- **Sentiment color:** 🟢 if bullish posts > 60% of fresh chatter, 🔴 if bearish > 60%, 🟡 otherwise.
- **Catalyst pipeline:** listings / partnerships / unlocks / token-2.0 / events with stated ETA. If none surfaces in news + X search, mark `None`.

Then the long-form context (after the block):
- Community color (Telegram/Discord activity if checked)
- Recent narrative drivers (~3 bullets)
- Risk flags surfaced by search (e.g. CryptoSlate insider reports, Messari manipulation flags)
- Sources list (clickable URLs)

---

## PHASE 6: MACRO CONTEXT

**Tools:** `clodds_market_index` → `clodds_polymarket_markets "crypto"` → `clodds_polymarket_orderbook` → `clodds_binance_spot_price BTCUSDT` → `clodds_binance_spot_price SOLUSDT` → `web_search "crypto market today"` → `web_fetch opinion.trade`

**MUST RENDER the macro block once per report (top of the document, after the executive summary).** Every item is required — pull from the explicit URL/tool listed:

```
Macro snapshot — [DATE] [TIME UTC]
  Fear & Greed:        XX (Extreme Fear / Fear / Neutral / Greed / Extreme Greed)
                       ↳ web_fetch https://api.alternative.me/fng/?limit=1
  BTC:                 $XX,XXX  · 24h: ±X.X%  · 7d: ±X.X%
                       ↳ prefer Binance MCP (get_ticker_24hr + get_klines interval=1d limit=8);
                         fall back to clodds_binance_spot_price BTCUSDT and/or
                         web_fetch https://api.binance.com/api/v3/klines?symbol=BTCUSDT&interval=1d&limit=8
  SOL:                 $XXX     · 24h: ±X.X%  · 7d: ±X.X%
                       ↳ same preference chain with SOLUSDT (CRITICAL for Solana memecoin trades)
  ETH:                 $X,XXX   · 24h: ±X.X%  · 7d: ±X.X%
                       ↳ same preference chain with ETHUSDT
  BTC dominance:       XX.X%
                       ↳ prefer CoinGecko MCP (get_global);
                         fall back to web_fetch https://api.coingecko.com/api/v3/global
  Polymarket events:   [top 1-2 active crypto markets + odds]
                       ↳ clodds_polymarket_markets "crypto"
                       ↳ fallback if help-text returned:
                         web_fetch https://polymarket.com/markets/crypto
                         web_search "polymarket crypto odds today"
  Kalshi macro:        [rate decisions / CPI / SEC / ETF events with date]
                       ↳ clodds_kalshi_markets
                       ↳ fallback if help-text returned:
                         web_fetch https://kalshi.com/markets/crypto
                         web_search "Kalshi crypto rate decision market today"
  Memecoin sector:     🔥 HOT / 🌤 WARM / 🌧 COOLING / 🥶 COLD  — [one-line reason]
  ETF flows:           [BTC/ETH ETF net flow last session, $M]
                       ↳ free source:
                         web_fetch https://farside.co.uk/bitcoin-etf-flow-all-data/
                         web_fetch https://farside.co.uk/ethereum-etf-flow-all-data/
                         web_search "BTC spot ETF net flow today" if farside blocks
  Macro verdict:       SUPPORTS / NEUTRAL / OPPOSES the trade direction
```

**Memecoin sector classification:**
- 🔥 HOT — pump.fun graduating count >5/day, BTC + SOL both up 7d, fear/greed >55
- 🌤 WARM — graduating count 2-5/day OR BTC/SOL flat-to-up
- 🌧 COOLING — graduating count <2/day OR BTC/SOL down 24h
- 🥶 COLD — graduating count <1/day AND BTC/SOL down 7d AND fear/greed <30

The macro verdict line is what informs Phase 7 factor #10 (Macro alignment).

---

## PHASE 7: ARIA SCORE & PROBABILITY

**MANDATORY for EVERY token analysis.** Composite score alone is NOT acceptable. The full 10-factor table must be rendered for each token, with the **Signal column populated** (one short phrase per factor explaining the score). On multi-token reports, render the scorecard table once per token — never collapse to composites only.

**If a factor cannot be scored due to missing data**, score it 5/10 (neutral) and footnote the reason with `†`. Never omit the table or substitute a single composite number.

Using data from all prior phases + `clodds_opinion "[token] — buy or sell?"`:

```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $[TICKER]                 │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  X/10 │ [bull/bear/range] │
│ 2.  RSI momentum            │  X/10 │ [overbought/...]  │
│ 3.  MACD signal             │  X/10 │ [cross + dir]     │
│ 4.  Volume momentum         │  X/10 │ [spike/fade]      │
│ 5.  On-chain health         │  X/10 │ [holders/wallets] │
│ 6.  Whale activity          │  X/10 │ [accum/distrib]   │
│ 7.  Social sentiment        │  X/10 │ [🟢/🟡/🔴 + KOL]  │
│ 8.  Liquidity & safety      │  X/10 │ [$XM liq, slip]   │
│ 9.  Narrative strength      │  X/10 │ [theme + age]     │
│ 10. Macro alignment         │  X/10 │ [supp/neut/opp]   │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ XX/100│                   │
└─────────────────────────────┴───────┴───────────────────┘

📈 Probability UP    (24h): XX%
📉 Probability DOWN  (24h): XX%
➡️ Probability SIDEWAYS:    XX%
```

**Scoring guide (use consistent thresholds across all tokens):**
| Composite | Action |
|---|---|
| 80–100 | Strong Buy |
| 65–79 | Buy |
| 50–64 | Speculative |
| 35–49 | Avoid |
| 20–34 | Sell |
| 0–19 | Exit |

**Per-factor scoring anchors (so scores stay consistent token-to-token):**
- **Trend (1):** 10 = clean HH-HL on all 5 TFs · 5 = mixed · 0 = LH-LL on all 5 TFs
- **RSI (2):** 10 = 50–65 with bull divergence · 5 = neutral · 0 = >85 or <15 extreme
- **MACD (3):** 10 = fresh bull cross both 1h+4h · 5 = neutral · 0 = bear cross 4h
- **Volume (4):** 10 = ≥2× avg sustained 4h+ · 5 = normal · 0 = <0.5× fading
- **On-chain (5):** 10 = top10 <20% + growing wallets · 5 = neutral · 0 = top10 >50% or shrinking
- **Whale (6):** 10 = net accum >$50K/24h · 5 = mixed · 0 = net distrib >$50K/24h
- **Social (7):** 10 = 🟢 + named KOL + active community · 5 = 🟡 · 0 = 🔴 or dead community
- **Liquidity (8):** 10 = >$10M liq + <0.5% slip @ $1K · 5 = $1–10M · 0 = <$500K
- **Narrative (9):** 10 = fresh narrative + organic growth · 5 = mature meme · 0 = exhausted/no narrative
- **Macro (10):** 10 = SUPPORTS · 5 = NEUTRAL · 0 = OPPOSES (from Phase 6 verdict line)

---

## PHASE 8: PORTFOLIO CHECK & TRADE PLAN

See `references/trade-execution.md` for full format.

**MUST CALL a real balance tool BEFORE any allocation recommendation.** Hypothetical phrasings ("on a 10-SOL bag", "if you had $X", "assume a $200 portfolio") are NOT acceptable and constitute a Phase 8 failure.

**Balance check tools by venue (call the right one for the trade venue):**
- Solana memecoins / pump.fun: `clodds_solana_balance` + `clodds_pumpfun_balance`
- Binance: `clodds_binance_spot_balance`
- Bybit: `clodds_bybit_spot_balance`
- MEXC: `clodds_mexc_spot_balance`
- Hyperliquid: `clodds_hyperliquid_balance`
- Multi-venue / "what's my portfolio": `clodds_portfolio_summary` + `clodds_bags` + `clodds_risk`

**Render the balance result at the top of Phase 8:**
```
Balance check — [VENUE] · [DATE] [TIME UTC]
  Available:        X.XX SOL / $X,XXX USDT
  Open positions:   N (showing $X,XXX in $TICKER, $X,XXX in $TICKER...)
  Free for new:     X.XX SOL / $X,XXX
  Source:           clodds_<venue>_balance (live)
```

**If the balance tool errors or returns 0**, render this banner instead and continue with sizing as a percentage:
```
⚠ BALANCE UNKNOWN — clodds_<tool> returned [error/0].
  Sizing below shown as % of available capital.
  Replace with real numbers before executing.
```

**Position sizing rules (memecoin specific):**
- Per-token cap: 15% of memecoin bag for top-quality (score ≥75), 10% for mid (60–74), 5% for speculative (50–59), 1% if presumed-risk banner from Phase 1.
- Aggregate memecoin allocation cap: 40% of total portfolio.
- Reserve: always keep ≥30% USDT/SOL dry powder.

Then build the trade plan per `references/trade-execution.md` with all of: Entry zone, SL, TP1/TP2/TP3, trailing stop activation, R:R, % portfolio at risk if SL hits.

---

## PHASE 9: ALERT & AUTOMATION SETUP

See `references/event-system.md` for full Tier 1 + Tier 2 configuration.

Immediately after every trade:
- Wire Tier 1: SL auto-close + TP1 auto-sell + trailing stop via `clodds_automation`
- Wire Tier 2: TP2/volume/whale alerts via `clodds_alerts`
- Activate: `clodds_monitoring` for continuous position health

**Every alert command in the report MUST be labeled with its tier.** Use this exact format so the user can see at a glance what auto-executes vs what requires their attention:

```
[Tier 1 — auto-execute]  clodds_automation add <token> stop_loss=X tp_ladder=A:30,B:40 trailing=X%
[Tier 1 — auto-execute]  clodds_automation add <token> tp1_sell pct=30 price=X
[Tier 2 — notify only]   clodds_alerts     add <token> price>X notify=tg
[Tier 2 — notify only]   clodds_alerts     add <token> volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts     add <token> whale_sell threshold=$50k notify=tg
```

For analyses where no trade is executed (recommendation only), still render the proposed alert lines using the same `[Tier X]` labels — that way the user can see exactly what would be wired on a "go".

---

## PHASE 10: ACTION SUMMARY (MANDATORY closing block for every report)

Every report — single-token or multi-token — must end with this actionable summary block. This is the tl;dr the user reads first. It must be grounded in the **real wallet balances** pulled in Phase 8, not hypothetical framings.

**Pull all balances before rendering:**
```
clodds_solana_balance                    → SOL + SPL token balances
clodds_binance_spot_balance              → USDT + all spot holdings on Binance
clodds_bybit_spot_balance                → same for Bybit (if user trades there)
clodds_mexc_spot_balance                 → same for MEXC
clodds_hyperliquid_balance               → Hyperliquid perps account
clodds_portfolio_positions               → all open positions across venues
clodds_bags                              → held tokens with entry price + unrealized PnL
```

**Render this exact block at the very end of the report, before the Disclaimer:**

```
═══════════════════════════════════════════════════════════════
📋 ACTION SUMMARY — [DATE] [TIME UTC]
═══════════════════════════════════════════════════════════════

Wallet snapshot (live, from clodds_*_balance):
  Solana:      X.XX SOL (~$X)   · N SPL positions
  Binance:     $X,XXX USDT      · N spot positions
  Bybit:       $X,XXX USDT      · N positions          (omit row if 0)
  MEXC:        $X,XXX USDT      · N positions          (omit row if 0)
  Hyperliquid: $X,XXX           · N perps              (omit row if 0)
  ─────────────────────────────────────────────────
  Deployable:  $X,XXX total                            ← ready for new trades

🟢 BUY / ADD  (ordered by conviction, highest first):
  ① BUY $[PAIR] on [VENUE] — [X.X SOL / $XXX] on [dip $X-$X / break >$X]
     SL $X · TP1 $X (+X%) · TP2 $X (+X%) · TP3 $X (+X%) trail X%
  ② BUY $[PAIR] on [VENUE] — [size] on [trigger]
     SL $X · TP1 $X · TP2 $X · TP3 $X trail X%
  (list up to 5; omit section entirely if no buyable setups)

🟡 HOLD — sell at targets  (positions you already hold, per clodds_bags):
  • $[TICKER] (held X units @ $Y.YY avg on [VENUE], unrealized +/-X%):
    — sell 30% at $X (TP1, +X% from avg)
    — sell 40% at $X (TP2, +X%)
    — trail final 30% at X% below peak, activate at $X (TP3)
    — SL moved to $X (breakeven / trailing invalidation)
  (list every current holding that appeared in the report)

🔴 SELL NOW  (thesis broken — exit regardless of price):
  • SELL 100% of $[TICKER] on [VENUE] — [one-line reason: rug flag / trend broken / catalyst failed]
  (omit section entirely if nothing to sell)

⚪ SKIP / AVOID  (tokens scanned but not deployed):
  • $[TICKER] — composite X/100 — [one-line reason]

Deployment plan:
  Deploying:   X.X SOL / $X,XXX   (XX% of deployable)
  Reserve:     X.X SOL / $X,XXX   (XX% dry powder)
  Aggregate portfolio risk if every SL fires: X.XX%

Top next-action (if you can only do one thing today):
  → [one crisp line — the single highest-conviction action]
═══════════════════════════════════════════════════════════════
```

**Rules for the Action Summary:**
1. **Always render.** Even if balance is 0 (BALANCE EMPTY banner from Phase 8), still render the block with zero-balance rows and recommend sizing as % of future-deployed capital.
2. **Real numbers only.** Every SOL/USDT number must come from a real `clodds_*_balance` call. Hypothetical framings ("at 10-SOL bag") are a failure for this section.
3. **BUY lines must name the venue explicitly** — `on pump.fun` / `on Raydium` / `on Binance spot` / `on Hyperliquid perps`. The user needs to know where to click.
4. **HOLD lines must come from `clodds_bags`** — if the user holds a token mentioned elsewhere in the report, convert the Signal Block's TP/SL into sell-at-price instructions for the existing position. If they don't hold a token, it belongs in BUY/SKIP, not HOLD.
5. **SELL NOW is reserved for hard exits** — rug flag, trend-broken with SL hit, catalyst confirmed failed. Don't use it for "take some profit" (that goes in HOLD).
6. **Top next-action is the single most-decisive line** — the user will re-read this tomorrow morning. Make it unambiguous.

### Final step — auto-append to the journal

**After the Action Summary block is rendered, ALWAYS append new entries to `reports/journal.jsonl`** following the schema in `references/journal-system.md`. One entry per actionable line:

| Action Summary line | Journal signal | Initial status |
|---|---|---|
| 🟢 BUY NOW (no trigger) | `BUY-NOW` | `OPEN` |
| 🟢 BUY with trigger condition | `BUY-WATCH` | `ENTRY_NOT_TRIGGERED` |
| 🟡 HOLD (existing position) | `HOLD` | `IN_POSITION` |
| 🔴 SELL NOW | `SELL-NOW` | `CLOSED_MANUAL` (and also flip any prior OPEN entry for that token to `CLOSED_MANUAL`) |
| ⚪ SKIP (composite ≥ 50) | `SKIP` | `EXPIRED` |
| ⚪ SKIP (composite < 50) | do not append |

**Deduplication rule:** Before appending, check `journal.jsonl` for an entry matching `(token + YYYYMMDD + signal)`. If found, **update** that row (bump `created_at`, replace `entry_zone` / `sl` / `tp1` / `tp2` / `tp3` / `trailing_pct` / `source_report`) instead of writing a duplicate line.

**Never skip this step.** The journal is how the user measures ARIA's real-world performance over time. Missing entries break the downstream `Status check` and `Journal stats` commands. Every report's appends must complete successfully before delivering the report.
