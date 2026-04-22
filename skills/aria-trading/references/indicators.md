# ARIA Indicator Reference — Compute, Interpret, Link

Load this file during Phase 3 for every market/token analysis. For each indicator below:
1. Pull the OHLCV data (Binance klines or pump.fun chart array).
2. Compute using the formula shown.
3. Report the value + interpretation in the per-timeframe block.
4. Cross-reference with the TradingView / Birdeye chart links at the end so the user can verify visually.

---

## Input data format

**Binance klines** — follow the Binance data-source preference chain in `tool-inventory.md`:
1. **Preferred**: Binance MCP `get_klines` / `klines` with `symbol=<SYM>USDT`, `interval=<tf>`, `limit=200`
2. **Fallback**: `web_fetch https://api.binance.com/api/v3/klines?symbol=<SYM>USDT&interval=<tf>&limit=200`

Array of candles, each candle is:
```
[openTime, open, high, low, close, volume, closeTime, quoteVolume, trades, takerBuyBase, takerBuyQuote, _]
      0      1     2    3    4      5
```

**pump.fun** (`aria_pumpfun chart <mint> --interval <tf>`)
Array of `{timestamp, open, high, low, close, volume}` objects. Works for bonding-curve tokens and freshly graduated PumpSwap tokens.

**Any Solana DEX pool** (GeckoTerminal — free public API, no auth — use for Raydium / Orca / Meteora / Jupiter / graduated pump.fun tokens, or any Solana token that `aria_pumpfun chart` fails on):
```
# Step 1 — find the top pool for a token mint:
web_fetch https://api.geckoterminal.com/api/v2/networks/solana/tokens/<mint>/pools

# Step 2 — pull OHLCV for the top pool (replace <pool> with address from step 1):
1m:  https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=1&limit=200
5m:  https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=5&limit=200
15m: https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=15&limit=200
1h:  https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/hour?aggregate=1&limit=200
4h:  https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/hour?aggregate=4&limit=200
1d:  https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/day?aggregate=1&limit=200
```
Response shape: `data.attributes.ohlcv_list[]` where each entry is `[timestamp, open, high, low, close, volume]` — same layout as pump.fun, so all indicator formulas below apply unchanged.

**GeckoTerminal also works for other chains** (Ethereum, Base, BSC, Arbitrum, etc.) — replace `solana` in the URL with the target network slug. This makes the indicator pipeline portable beyond Solana + the three CEXes.

Convention for the formulas below:
```
closes[]  = array of close prices, oldest → newest
highs[]   = array of highs
lows[]    = array of lows
volumes[] = array of volumes
```

---

## RSI (14) — Relative Strength Index

**Formula:**
```
1. For i in 1..len(closes):
     change[i] = closes[i] - closes[i-1]
     gain[i]   = max(change[i], 0)
     loss[i]   = max(-change[i], 0)

2. Seed (first RSI value at index 14):
     avg_gain = mean(gain[1..14])
     avg_loss = mean(loss[1..14])

3. Subsequent values (Wilder's smoothing):
     avg_gain = (avg_gain_prev * 13 + gain_current) / 14
     avg_loss = (avg_loss_prev * 13 + loss_current) / 14

4. RS  = avg_gain / avg_loss
5. RSI = 100 - (100 / (1 + RS))
```

**Interpretation:**
| RSI value | Read |
|---|---|
| > 70 | Overbought — pullback risk rising |
| 50–70 | Bullish momentum |
| 30–50 | Bearish momentum |
| < 30 | Oversold — bounce likely |

**Divergence (always check last 10 candles):**
- Bullish div: price makes lower low, RSI makes higher low → reversal up
- Bearish div: price makes higher high, RSI makes lower high → reversal down

**Report format:** `RSI(14): 42.3 — bearish momentum, no divergence`

---

## MACD (12, 26, 9) — Moving Average Convergence Divergence

**EMA helper (used throughout this file):**
```
k        = 2 / (period + 1)
ema[0]   = SMA(closes[0..period-1])
ema[i]   = closes[i] * k + ema[i-1] * (1 - k)
```

**Formula:**
```
1. ema12        = EMA(closes, 12)
2. ema26        = EMA(closes, 26)
3. macd_line[]  = ema12[] - ema26[]
4. signal_line  = EMA(macd_line, 9)
5. histogram    = macd_line - signal_line
```

**Interpretation:**
| State | Read |
|---|---|
| MACD > Signal, both > 0 | Strong bullish |
| MACD > Signal, both < 0 | Bullish reversal starting |
| MACD < Signal, both > 0 | Bearish reversal starting |
| MACD < Signal, both < 0 | Strong bearish |
| Histogram expanding | Momentum accelerating |
| Histogram contracting | Momentum fading — watch for cross |
| Fresh cross in last 1–3 candles | Actionable signal |

**Report format:** `MACD: line -0.12, signal -0.08, hist -0.04 — bearish, hist contracting (early reversal)`

---

## Bollinger Bands (20, 2)

**Formula:**
```
1. mid     = SMA(closes[-20:])
2. stddev  = standard deviation of closes[-20:]
3. upper   = mid + 2 * stddev
4. lower   = mid - 2 * stddev
5. width%  = (upper - lower) / mid * 100
```

**Interpretation:**
| State | Read |
|---|---|
| Price > upper | Overbought — short-term pullback likely |
| Price < lower | Oversold — short-term bounce likely |
| Riding upper band | Strong uptrend — don't fade |
| Riding lower band | Strong downtrend — don't fade |
| Width at 20-candle low (squeeze) | Breakout imminent, direction unclear |
| Width expanding | Volatility rising — trade the direction |

**Report format:** `BB(20,2): upper $89.42 / mid $86.31 / lower $83.20 (width 7.2%) — mid-range, squeeze forming`

---

## VWAP — Volume-Weighted Average Price

**Session reset:** UTC midnight for intraday; beginning of selected window for multi-TF analysis.

**Formula:**
```
For each candle i in the session:
  typical[i]  = (high[i] + low[i] + close[i]) / 3
  tp_vol[i]   = typical[i] * volume[i]

cumul_tp_vol = sum(tp_vol[])
cumul_vol    = sum(volume[])
VWAP         = cumul_tp_vol / cumul_vol
```

**Interpretation:**
| State | Read |
|---|---|
| Price > VWAP | Intraday bulls in control |
| Price < VWAP | Intraday bears in control |
| Price touching VWAP from above | Long entry zone (support) |
| Price touching VWAP from below | Short entry zone (resistance) |

**Report format:** `VWAP: $87.88 — price above, bullish intraday bias`

---

## EMA(9), EMA(21), EMA(50) — Moving Averages

Use the EMA helper above for each period.

**Interpretation:**
| Stack | Read |
|---|---|
| 9 > 21 > 50 | Bull stack — strong uptrend |
| 9 < 21 < 50 | Bear stack — strong downtrend |
| Mixed | Trend in transition |
| Price > all three | Bullish bias |
| Price < all three | Bearish bias |
| 9 crosses 21 up | Short-term bull kickoff |
| 21 crosses 50 up | Medium-term bull shift |

**Report format:** `EMA(9/21/50): $88.10 / $87.45 / $86.20 — bull stack, price above all`

---

## ATR(14) — Average True Range

**Formula:**
```
For each candle i:
  TR[i] = max(
    highs[i] - lows[i],
    abs(highs[i] - closes[i-1]),
    abs(lows[i]  - closes[i-1])
  )

ATR[14]    = mean(TR[1..14])                        (seed)
ATR[i]     = (ATR[i-1] * 13 + TR[i]) / 14           (Wilder)
ATR_pct    = ATR / current_price * 100
```

**Interpretation:**
- Express ATR as `$X.XX` and `X.X% of price`
- SL breathing room: 1× ATR of the 15m chart below swing low (longs) / above swing high (shorts)
- Conservative SL: 1.5–2× ATR
- Rising ATR: widen stops, volatility expanding
- Falling ATR: tighten stops, expect breakout

**Report format:** `ATR(14): $1.42 (1.6% of price) — normal volatility`

---

## Stochastic RSI (14, 14, 3, 3)

**Formula:**
```
1. rsi[]       = RSI(closes, 14)                       (rolling, see RSI section)
2. For i ≥ 28:
     stoch[i]  = (rsi[i] - min(rsi[i-13..i])) /
                 (max(rsi[i-13..i]) - min(rsi[i-13..i])) * 100
3. %K = SMA(stoch, 3)
4. %D = SMA(%K, 3)
```

**Interpretation:**
| State | Read |
|---|---|
| %K > 80 | Overbought |
| %K < 20 | Oversold |
| %K crosses %D up in oversold | Bullish trigger |
| %K crosses %D down in overbought | Bearish trigger |
| Divergence vs price | Reversal signal (same logic as RSI) |

**Report format:** `Stoch RSI: K 23, D 31 — oversold, K crossing D up (bullish trigger)`

---

## OBV — On-Balance Volume

**Formula:**
```
OBV[0] = 0
For each candle i ≥ 1:
  if closes[i] > closes[i-1]:  OBV[i] = OBV[i-1] + volumes[i]
  elif closes[i] < closes[i-1]: OBV[i] = OBV[i-1] - volumes[i]
  else:                          OBV[i] = OBV[i-1]
```

**Interpretation:**
| Pattern | Read |
|---|---|
| OBV↑ + price↑ | Healthy uptrend — confirmation |
| OBV↓ + price↓ | Healthy downtrend — confirmation |
| OBV↑ + price flat/↓ | Hidden accumulation — bullish divergence |
| OBV↓ + price flat/↑ | Hidden distribution — bearish divergence |
| OBV opposing price 20+ candles | Major reversal warning |

**Report format:** `OBV: rising +18% over last 20 candles — accumulation confirms uptrend`

---

## Volume ratio (every timeframe)

```
avg_vol_20  = mean(volumes[-20:])
vol_ratio   = volumes[-1] / avg_vol_20
```

| Ratio | Read |
|---|---|
| > 2.0 | Strong spike — signal weight ×2 |
| 1.3–2.0 | Elevated — tradeable confirmation |
| 0.7–1.3 | Normal |
| < 0.7 | Weak — do not chase |

**Breakout validation:**
- Breakout + vol_ratio > 1.5 → validated, enter
- Breakout + vol_ratio < 1.0 → suspected fake-out, wait for retest

**Report format:** `Vol: 2.3× avg (last candle 18.4M vs 20-candle avg 8.0M) — strong confirmation`

---

## Support & Resistance (algorithmic detection)

Use on every timeframe. Algorithm:

```
1. Identify swing highs:  candle i where highs[i] > highs[i-2..i-1] AND highs[i] > highs[i+1..i+2]
2. Identify swing lows:   candle i where  lows[i] <  lows[i-2..i-1] AND  lows[i] <  lows[i+1..i+2]
3. Cluster swings within 0.3% of each other — merge into a single level
4. Rank by:
     a. Number of touches (more touches = stronger)
     b. Recency (recent touches weighted higher)
     c. Volume at the touch (higher volume = stronger)
5. Top 3 resistance levels above current price = R1, R2, R3
6. Top 3 support levels below current price = S1, S2, S3
```

Add two structural levels if present:
- ATH / ATL of the visible window (extreme R3 / S3)
- Session open / prior day close (intraday magnets)

---

## Visual verification — chart links (always include)

**CEX-listed tokens (Binance / Bybit / MEXC):**
```
https://www.tradingview.com/chart/?symbol=BINANCE:[SYMBOL]USDT&interval=[1|5|15|60|240]
```
TradingView interval codes: `1` = 1m, `5` = 5m, `15` = 15m, `60` = 1h, `240` = 4h, `D` = 1d.
Default TradingView layout already includes volume + moving averages. Users can add RSI / MACD / Bollinger / VWAP from the chart's Indicators panel.

**Solana / pump.fun tokens:**
```
https://birdeye.so/token/[mint]?chain=solana          ← embedded TradingView chart with indicators
https://dexscreener.com/solana/[mint]                  ← price + volume
https://solscan.io/token/[mint]                        ← on-chain activity
https://pump.fun/coin/[mint]                           ← community + bonding curve
```

**Report format at the end of Phase 3:**
```
📊 Charts:
  4h/1h/15m/5m/1m:  https://www.tradingview.com/chart/?symbol=BINANCE:SOLUSDT&interval=240
  Birdeye (SOL):    n/a (CEX asset)

Verify the RSI, MACD, Bollinger Bands, VWAP, and EMA(9/21/50) overlays on the
TradingView link — the computed values above should match within ±2% of what
you see on-chart. Any material mismatch → tell me and I'll recompute.
```

---

## Reporting discipline

For every per-timeframe block in Phase 3 **under the Major Profile**, **all** of these must be present:
RSI(14) · MACD(12,26,9) · BB(20,2) · VWAP · EMA(9/21/50) · ATR(14) · Stoch RSI · OBV · volume ratio · R1/R2/R3 · S1/S2/S3 · candle pattern · chart structure · one-line key observation.

If a calculation fails (not enough candles, missing volume field, etc.) state so explicitly — never fabricate values.

---

## Memecoin Peak-Check Mode — simplified indicator set

For **Memecoin Profile** tokens (pump.fun / PumpSwap / fresh Solana DEX), Phase 3 uses a reduced indicator set on 15m + 1h only. The heavy multi-TF × multi-indicator suite above is **explicitly suppressed** — not because indicators break, but because on a <10-day-old token with <1,000 holders, the OHLCV series is dominated by noise and a handful of whale trades. Forcing RSI(14) on 30 bars of manipulated tape produces false confidence; the real signal lives in Phase 5 social data.

Full procedure in `aria-protocol.md` → `Phase 3 — Memecoin Peak-Check Mode`. This section documents the minimal indicator compute needed for the 4-question block.

### Data pulls (2 only)

```
15m: web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=15&limit=100
1h:  web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/hour?aggregate=1&limit=60
```

If 15m <20 bars OR 1h <10 bars → render `INSUFFICIENT DATA — n bars only` and skip the block entirely. Do not fabricate.

### Compute only these 4 things

**1. Peak status** (qualitative read of the 1h series)

Look at the last 10-20 1h bars and classify:
- **pre-peak base** — sideways range <10% spread for ≥8 bars with rising volume
- **still peaking** — last 3+ bars closed green with each close > prior high; volume rising
- **blow-off top confirmed** — one bar closed with long upper wick (upper wick ≥ 2× body) AND closed red AND volume spiked ≥3× then next bar volume collapsed
- **ranging post-peak** — ≥6 bars sideways after a peak bar, holding near peak ±15%
- **new leg forming** — clear higher-low printed above a prior pullback, next bar breaking the pullback high
- **broken down** — last 5 1h closes all red, each below prior low; volume confirming sell pressure

**2. VWAP position** (session VWAP only — no 20-bar/50-bar anchored variants)

```
session_vwap = sum(typical_price[i] × volume[i]) / sum(volume[i])
                where typical_price = (high + low + close) / 3
                    and the session = today's UTC bars on the 1h series
```

Render: `price $X.XX is [X.X%] above/below session VWAP $Y.YY`

Interpretation:
- price ≥3% above VWAP + rising trend → momentum extended but intact
- price near VWAP ±1% → neutral zone
- price ≥3% below VWAP + declining → exhausted, avoid longs

**3. Volume trend** (last 3 bars vs prior 10 bars on 1h)

```
vol_ratio = mean(volume[last 3 bars]) / mean(volume[bars -13 through -4])
```

Classify:
- ratio ≥ 1.5 on green bars → **expanding on rally** (healthy)
- ratio ≥ 1.5 on red bars → **expanding on drop** (capitulation or panic)
- ratio < 0.8 on green bars → **contracting on rally** (exhaustion — do not chase)
- ratio 0.8-1.5 → **flat-ranging**

**4. Structure (15m + 1h)**

Eyeball the swing highs/lows over the last 20 bars on each TF:
- **HH-HL uptrend** — each swing high > prior swing high, each swing low > prior swing low
- **HH-LH topping** — peak made a higher high but the pullback's swing low is now printing lower = topping structure
- **LH-LL downtrend** — classic lower-high lower-low sequence
- **range** — swing highs and lows roughly the same over 20 bars

Render as the TF-alignment line: `aligned-bull / aligned-bear / divergent`.

### Explicitly suppressed indicators (for Memecoin Profile)

- **RSI(14)** — noisy on 30-bar memecoin series; false overbought/oversold readings are common
- **MACD(12,26,9)** — requires ≥35 bars for the signal line; many fresh memecoins don't have that
- **Bollinger Bands(20,2)** — the band width on a vertical pump compresses to meaninglessness right at the peak
- **Stoch RSI** — noise-on-noise
- **ATR(14)** — unreliable on thin tape where a single whale print dominates the range
- **EMA(9/21/50)** — 50-bar EMA is impossible on a 30-bar token; shorter EMAs just hug price on impulsive moves
- **OBV** — meaningful on mature tokens but on a token with <1,000 holders, two whale trades flip the direction

The compute cost saved by skipping these (5 TFs × 10 indicators ≈ 50 compute-operations per token) is reallocated to the deeper social analysis in Phase 5.

### Chart-link block (Memecoin Profile)

```
📊 Charts — verify peak-status visually:
  Birdeye:       https://birdeye.so/token/[mint]?chain=solana
  DexScreener:   https://dexscreener.com/solana/[mint]
  GeckoTerminal: https://www.geckoterminal.com/solana/pools/[pool]
  pump.fun:      https://pump.fun/coin/[mint]

Look at the 1h chart. Does it match the "Peak status" classification above?
If yes, proceed with the trade plan. If no — the chart-read is wrong; re-examine.
```
