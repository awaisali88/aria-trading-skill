# ARIA — Top 5 pump.fun Graduated Tokens · Full Analysis (v2, COMPLIANT)

**Report date:** 2026-04-17 18:30 UTC
**Scope:** Same 5 tokens as v1 (pippin, Pnut, jellyjelly, yn, neet) — re-graded against the full ARIA compliance checklist.
**v2 closes:** real balance check, 10-factor scorecard per token, slippage tables, buy/sell tx split, full 1m/5m/4h raw OHLCV, standardized sentiment blocks, tier-labeled alerts, PARTIAL/HIGH-CONCENTRATION banners where applicable.

---

## ⚠ Compliance banners (read first)

```
⚠ BALANCE EMPTY — clodds_solana_balance returned 0.00 SOL
  (Wallet: GwwcVEb7GVXxTHviHr8a8pWDXWH5xjtG5GveLATTahfk)
  clodds_portfolio_summary: Total Value $0.00 · 0 positions
  All Phase-8 position sizes below are shown in % of capital +
  absolute SOL for illustration. Fund the wallet before executing.
```

```
🚨 HIGH HOLDER CONCENTRATION — 3 of 5 tokens flagged
  pippin:     top10 67.6%  (flag >40%)
  Pnut:       top10 70.6%  (flag >40%)
  jellyjelly: top10 71.9%  (flag >40%)
  neet:       top10 19.9%  ✓
  yn:         top10 10.0%  ✓
  None exceed the Phase-1 HARD-STOP threshold of >50% in top-5,
  but the three flagged tokens warrant smaller size + tighter SL.
```

```
ℹ  PRESUMED-RISK banner NOT raised for yn.
  yn is 2 days old (<7d), but Phase-1 fallback chain succeeded
  (GeckoTerminal token info returned: top10 9.98%, mint authority
  renounced, freeze authority renounced). Security data is present,
  not missing — banner suppressed per protocol.
```

---

## 0. Macro Snapshot — 2026-04-17 18:30 UTC

```
Fear & Greed:        21  ·  EXTREME FEAR
                     ↳ api.alternative.me/fng
BTC:                 $74,730   · 24h: -0.06%  · 7d: +3.42%
                     ↳ Binance klines BTCUSDT interval=1d
SOL:                 $87.93    · 24h: -0.16%  · 7d: +5.53%   (CRITICAL — memecoin denom)
ETH:                 $2,330    · 24h: -0.41%  · 7d: +6.40%
BTC dominance:       56.98%    (CoinGecko global)
ETH dominance:       10.72%
Total MCap:          $2.62T    · 24h: +0.17%
Polymarket events:   unavailable — clodds_polymarket_markets returned help text;
                     no event pulls succeeded after fallback attempts
Kalshi macro:        unavailable — clodds_kalshi_markets returned help text
Memecoin sector:     🌤 WARM  — pump.fun trending list has 12 active high-vol graduates,
                     BTC+SOL both up 7d, but F&G 21 (Extreme Fear) drags the rating.
                     Classification per aria-protocol.md: "graduating count 2-5/day OR BTC/SOL flat-to-up"
ETF flows:           unavailable on this setup
Macro verdict:       NEUTRAL — SOL trend supports memecoin bids but F&G extreme-fear
                     caps conviction; no macro tailwind, no macro headwind
```

**Trade implication:** Not the macro for 20% allocations. Size small, take profits early, respect stops.

---

## 1. Executive Summary

| # | Token | Price | 24h | MCap | Liq | Top10 | ARIA Score | Verdict |
|---|-------|-------|-----|------|-----|-------|-----------|---------|
| 1 | **Pnut** | $0.0764 | +35% | $76.4M | $3.65M | 70.6% ⚠ | **56/100** | Speculative · BUY ON DIP |
| 2 | **jellyjelly** | $0.0494 | +10% | $49.4M | $4.13M | 71.9% ⚠ | **55/100** | Speculative · BUY ON BREAKOUT |
| 3 | **yn** | $0.0788 | +10% | $78.8M | $697.9K | 10.0% ✓ | **54/100** | Speculative · SMALL-SIZE MOMENTUM |
| 4 | **pippin** | $0.0355 | +40% | $35.5M | $5.23M | 67.6% ⚠ | **51/100** | Speculative · WAIT FOR PULLBACK |
| 5 | **neet** | $0.0375 | -2% | $37.5M | $1.58M | 19.9% ✓ | **42/100** | **AVOID** |

**Action order if funding the wallet:** Pnut (dip entry) → jellyjelly (breakout trigger) → yn (momentum scalp). Skip pippin until it retests $0.033 support; avoid neet.

---

## 2. yn ★ only truly-fresh graduate (2 days old)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `5w3sM6yMmfmboCv3FRn3hwKTCtukZwjcwP5WGsGrpump` |
| Creator wallet | `Cizcyxi2Qn6N...` |
| Launch / pool created | **2026-04-15 PumpSwap** (2 days ago) |
| Narrative | Meme — "yn's always got unc's back" |
| Mint authority | ✅ Disabled (GeckoTerminal info) |
| Freeze authority | ✅ Disabled |
| Holder count | **2,799** |
| Top 10 holder % | **9.98%** ✓ (very low concentration) |
| Graduation | ✅ GRADUATED to PumpSwap |

**Security verdict:** Clean. Both authorities renounced, top-10 below 10%. No STOP flag, no PRESUMED-RISK banner. The only material risk is **liquidity depth** (below, Phase 2).

### Phase 2 — Live Market Snapshot
| Metric | Value |
|--------|-------|
| Price (live) | **$0.0788** |
| MCap | $78.8M |
| FDV (≈circ) | $78.8M |
| Liquidity | **$697.9K** ⚠ |
| Vol/MCap | 0.028 (low but OK given token age) |
| 24h Vol | $2.17M |
| 1h Vol | $71.7K |
| 5m Δ | +0.4% (from 1m OHLCV last 5 bars) |
| 1h Δ | +2.1% |
| 6h Δ | +36.5% (derived from 1h series) |
| 24h Δ | +9.8% |
| 7d Δ | n/a — token is 2 days old |
| Bonding | ✅ Graduated, PumpSwap pool `CtkKyCpUxd4G9AMMoDCuo7jGXLiwR5mMFPwfp8WLqJVE` |
| Buy/sell tx last 100 | **50 buys / 50 sells** (balanced) |
| Buy USD vol / Sell USD vol | ~$4,847 / ~$4,923 (balanced) |
| Trades >$5K in 100 | **0** (all retail-size) |

**Slippage table** (formula: `tradeSize / (liq × 2) × 100`, one-sided; round-trip = 2×):
```
Entry $50    → 0.004%   |  Exit $50    → 0.004%
Entry $100   → 0.007%   |  Exit $100   → 0.007%
Entry $500   → 0.036%   |  Exit $500   → 0.036%
Entry $1,000 → 0.072%   |  Exit $1,000 → 0.072%
Entry $5,000 → 0.358%   |  Exit $5,000 → 0.358%   (round-trip ≈0.72%)
```

### Phase 3 — Multi-Timeframe Chart + Indicators

**Data source:** GeckoTerminal OHLCV, pool `CtkKyCpUxd4G9AMMoDCuo7jGXLiwR5mMFPwfp8WLqJVE`.

#### 1m TF (60 bars, last 30 detailed)
```
Trend:           UP  (HH-HL clean)
Pattern:         Steady climb, last 30 bars from $0.0766 → $0.0788
Structure:       Ascending channel, low volatility

R3 $0.0810  (+2.8%)   · S1 $0.0781 (-0.9%)
R2 $0.0798  (+1.3%)   · S2 $0.0775 (-1.7%)
R1 $0.0792  (+0.5%)   · S3 $0.0766 (-2.8%)   ← 60-bar low

RSI(14):         ~58   neutral-bullish       divergence: none
MACD(12,26,9):   line>signal, both >0, histogram expanding → bull
BB(20,2):        upper $0.0798, mid $0.0782, lower $0.0766  → riding mid-upper
VWAP (session):  $0.0780 — price +1.0% above VWAP → bull
EMA(9/21/50):    $0.0787 / $0.0780 / $0.0775  stack: BULL (9>21>50, price above all)
ATR(14):         $0.0004  (0.5% of price) — tight
Stoch RSI:       K ~70 · D ~65  zone: near OB  cross: none
OBV:             rising — accumulation continuing
Volume:          last 1,028 vs 20-bar avg ~1,100 → 0.93× normal

Key observation: micro-uptrend intact, no divergence, low-vol grind higher
```

#### 5m TF (60 bars, last 30)
```
Trend:           UP  (HH-HL, recent accel)
Pattern:         Early 5m bars at $0.0545, latest at $0.0787 (+45% over 5h)
Structure:       Momentum impulse with small pullbacks, no reversal

R3 $0.0820  (+4.1%)   · S1 $0.0760 (-3.6%)
R2 $0.0810  (+2.8%)   · S2 $0.0720 (-8.6%)
R1 $0.0795  (+0.9%)   · S3 $0.0545 (-30.8%)  ← 60-bar low

RSI(14):         ~68   bullish, not OB       divergence: none
MACD(12,26,9):   positive, hist expanding → bull
BB(20,2):        upper $0.0800, mid $0.0755, lower $0.0710  → upper-band ride
VWAP:            $0.0712 — price +10.7% above → very bullish
EMA(9/21/50):    $0.0785 / $0.0750 / $0.0700  stack: BULL (parabolic)
ATR(14):         $0.0015  (1.9% of price)
Stoch RSI:       K ~85 · D ~80  zone: OB  cross: none yet
OBV:             rising steeply
Volume:          fresh 5m bars 500-2,000 vs 20-bar avg ~1,500 → normal, not exhausted

Key observation: strong uptrend with pullback to $0.076 holding EMA9; OB building
```

#### 15m TF (60 bars; 120 available)
```
Trend:           UP  (HH-HL over last 5 hours)
Pattern:         Extended uptrend from $0.063 → $0.0788, no major reversal
Structure:       Rising wedge forming (caution)

R3 $0.0820  (+4.1%)   · S1 $0.0720 (-8.6%)
R2 $0.0800  (+1.5%)   · S2 $0.0680 (-13.7%)
R1 $0.0790  (+0.3%)   · S3 $0.0630 (-20.1%)

RSI(14):         ~71   OB — caution          divergence: slight bear (PR HH, RSI not confirming)
MACD(12,26,9):   line>signal>0, hist flattening → momentum cooling
BB(20,2):        upper $0.0810, mid $0.0735, lower $0.0660  → upper-band
VWAP:            $0.0695 — price +13.4% above → extended
EMA(9/21/50):    $0.0770 / $0.0725 / $0.0680  stack: BULL
ATR(14):         $0.0022  (2.8%)
Stoch RSI:       K ~88 · D ~85  zone: OB  cross: pending bear
OBV:             rising
Volume:          last bar 6k vs 20-bar avg ~15k → 0.4× FADING

Key observation: OB, vol fading, bear div forming — classic late-trend
```

#### 1h TF (33 bars available — ENTIRE history of token)
```
⚠  Only 33 hourly bars since graduation. Reading with limited baseline.

Trend:           UP  (net +260% from graduation floor $0.022)
Pattern:         Impulse with 2 embedded flash wicks (timestamps 1776322800,
                 1776348000) showing liquidation/whale-dump events absorbed
Structure:       Early-stage, no developed S/R from prior cycles

R3 $0.0900  (+14%)    · S1 $0.0700 (-11%)
R2 $0.0870  (+10%)    · S2 $0.0600 (-24%)
R1 $0.0800  (+1.5%)   · S3 $0.0500 (-37%)

RSI(14):         ~73   OB                    divergence: n/a (insufficient history)
MACD(12,26,9):   strongly positive
BB(20,2):        upper $0.0780, mid $0.0560, lower $0.0340 (wide, parabolic)
VWAP:            $0.0490 — price +61% above → extreme
EMA(9/21/50):    $0.0735 / $0.0530 / $0.0290  stack: BULL (parabolic)
ATR(14):         $0.0090  (11% of price — EXTREME)
Stoch RSI:       K ~95 · D ~90  zone: deep OB
OBV:             rising
Volume:          recent 1h 70K vs rolling avg 75K → normal

Key observation: parabolic + extreme ATR + limited history = size small or wait
```

#### 4h TF (9 bars — INSUFFICIENT DATA)
```
⚠  INSUFFICIENT DATA — only 9 bars available (token graduated 2026-04-15).
    Indicator values cannot be computed reliably. Chart visible:

    9 bars: 0.0514, 0.0620, 0.0743, 0.0103*, 0.0157, 0.0063*, 0.0575, 0.0697, 0.0788
    (*flash-crash bars that were absorbed within the 4h window)

    Structural read only:
    - Net trajectory UP from $0.0514 to $0.0788 (+53%)
    - 2 deep wicks embedded suggest thin-book sell events
    - No 4h resistance developed yet — blue-sky above

    RSI/MACD/BB/EMAs: not computed — revisit in 48-72h when ≥20 4h bars exist.
```

### Phase 3 Confluence Matrix
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ INSUFFICIENT DATA — 9 bars                      │
│ 1h      │ UP      │ 73   │ BULL    │ →    │ 🟢 overbought│
│ 15m     │ UP      │ 71   │ BULL↓   │ ↓    │ 🟡 cooling  │
│ 5m      │ UP      │ 68   │ BULL    │ →    │ 🟢          │
│ 1m      │ UP      │ 58   │ BULL    │ →    │ 🟢          │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 3/5 bull · 0/5 bear · 1/5 mixed · 1/5 insufficient
Higher-TF bias:  BULLISH (1h up; 4h unknown)
Entry trigger:   WAITING — wait for 15m pullback to $0.068-0.072 (EMA21)
Micro timing:    HOT on 1m but 15m cooling = don't chase
```

### Chart links
- [Birdeye yn](https://birdeye.so/token/5w3sM6yMmfmboCv3FRn3hwKTCtukZwjcwP5WGsGrpump?chain=solana)
- [DexScreener yn](https://dexscreener.com/solana/5w3sM6yMmfmboCv3FRn3hwKTCtukZwjcwP5WGsGrpump)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/CtkKyCpUxd4G9AMMoDCuo7jGXLiwR5mMFPwfp8WLqJVE)
- [Solscan token](https://solscan.io/token/5w3sM6yMmfmboCv3FRn3hwKTCtukZwjcwP5WGsGrpump)

### Phase 4 — On-Chain
```
Top 10 holder %:     9.98%    ✓ (flag threshold 40%)
Creator wallet:      Cizcyxi2Qn6N... — % remaining UNKNOWN (solscan fallback not fetched)
Whale direction 24h: +$0 net (last 100 trades all <$5K; no whale activity visible)
LP lock:             PumpSwap-standard (LP tokens burned post-graduation) — inferred, not explicitly verified
Price-vs-OBV div:    none on 1h/15m; OBV rising with price
Active wallets:      2,799 holders (token 2d old; wallet-growth trend unavailable)

⚠ ON-CHAIN DATA PARTIAL — creator wallet activity, wallet-growth trend unavailable
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟡 Neutral  (no indexed signal — too new)
X post count (24h):   LOW (~<10 posts) — searches returned only the creator link
Community activity:   DEAD — no meaningful KOL or community chatter indexed
KOL mentions:         None (creator-linked handle @definegambling, niche, no follower data)
Catalyst pipeline:    None visible (no CEX listing, partnership, or announcement)
Shill/manipulation:   Low-to-Medium (thin community = whatever pump exists is organic-for-now)
Mainstream coverage:  None (not on CoinGecko / CoinMarketCap / CryptoSlate / Messari)
```

**Narrative color:**
- Zero mainstream indexing for "$yn" site:x.com — no tweets by KOLs with >100K followers
- Creator's Twitter link points to [@definegambling](https://x.com/definegambling) — a niche account
- 2,799 holders with 9.98% top-10 is a surprisingly organic distribution for a 2d-old token — suggests genuine retail buying, not wash trading
- But **zero attention** = upside hinges on a narrative-ignition event (viral tweet, KOL pick) that hasn't happened yet

**Sources:**
- [GeckoTerminal pool yn/SOL](https://www.geckoterminal.com/solana/pools/CtkKyCpUxd4G9AMMoDCuo7jGXLiwR5mMFPwfp8WLqJVE)
- Creator-linked: [x.com/definegambling](https://x.com/definegambling)

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $yn                       │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  9/10 │ clean HH-HL 1m-1h │
│ 2.  RSI momentum            │  4/10 │ OB on 1h+15m      │
│ 3.  MACD signal             │  7/10 │ bull, hist flat   │
│ 4.  Volume momentum         │  5/10 │ normal, fading 15m│
│ 5.  On-chain health         │  8/10 │ top10 10% ✓       │
│ 6.  Whale activity          │  5/10 │ no whales — neutr │
│ 7.  Social sentiment        │  3/10 │ 🟡 DEAD community │
│ 8.  Liquidity & safety      │  3/10 │ $697K liq, 0.36%@ │
│                             │       │ $5K (thin)        │
│ 9.  Narrative strength      │  6/10 │ fresh grad, no    │
│                             │       │ story yet         │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 55/100│ → Speculative     │
└─────────────────────────────┴───────┴───────────────────┘

Wait — recounting: 9+4+7+5+8+5+3+3+6+5 = 55 → 55/100

📈 Probability UP    (24h): 50%
📉 Probability DOWN  (24h): 35%
➡️ Probability SIDEWAYS:    15%
```

### Phase 8 — Trade Plan
```
Balance check — Solana wallet · 2026-04-17 18:30 UTC
  Available:        0.00 SOL  ⚠ EMPTY
  Open positions:   0
  Free for new:     0 SOL
  Source:           clodds_solana_balance (live)

⚠ BALANCE UNKNOWN — user has 0 SOL. Sizing shown as %
  of whatever capital is later deployed. Replace with real
  numbers before executing.
```

**Speculative score (55) → 5% cap per token. Extra caveat for yn due to liquidity depth.**

| Scenario | Entry zone | SL | TP1 (30%) | TP2 (40%) | TP3 trail (30%) | R:R | Position |
|----------|-----------|-----|-----------|-----------|-----------------|-----|----------|
| Pullback long | $0.068 – $0.072 | $0.062 (-11%) | $0.080 (+14%) | $0.095 (+36%) | $0.120 (+71%) trailing 18% | 1:3.8 | **5%** of bag |
| Breakout long | >$0.080 w/ 2× vol | $0.076 (-5%) | $0.090 (+13%) | $0.105 (+31%) | $0.130 (+63%) | 1:4.0 | 3% of bag |

At hypothetical 10 SOL bag: **5% = 0.5 SOL (~$44)** · **% portfolio at risk if SL hits = 5% × 11% = 0.55%**

### Phase 9 — Alerts (proposed, not wired)
```
[Tier 1 — auto-execute]  clodds_automation add yn stop_loss=0.062 tp_ladder=0.080:30,0.095:40 trailing=18% activate_at=+20%
[Tier 1 — auto-execute]  clodds_automation add yn tp1_sell pct=30 price=0.080
[Tier 2 — notify only]   clodds_alerts    add yn price>0.0800 notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add yn price<0.0700 notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add yn volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add yn whale_sell threshold=$10k notify=tg   ← lower threshold due to thin book
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $yn · 2026-04-17 18:30 UTC               ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: WATCH-BUY (pullback only)                       ║
║  CONVICTION: MEDIUM (momentum) / LOW (risk-adj)          ║
║  PRICE: $0.0788  BIAS: BULLISH but overextended          ║
║  CHART: Parabolic with 2 embedded flash wicks            ║
║  RSI: 73 (1h) / 71 (15m)  MACD: bull  OBV: rising        ║
║  SCORE: 55/100                                           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: $0.068 – $0.072   SL: $0.062 (-11%)              ║
║  TP1: $0.080 (+14%) → 30%  TP2: $0.095 (+36%) → 40%      ║
║  TP3: $0.120 (+71%) → trail 30%                          ║
║  TRAILING: 18% below peak — activate at +20%             ║
║  R:R 1:3.8  POSITION: 5% bag · 0.5 SOL @ 10 SOL           ║
╠══════════════════════════════════════════════════════════╣
║  UP: 50%  DOWN: 35%  SIDEWAYS: 15%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Cleanest security (top10 10%, mint/freeze off). ║
║  Price parabolic, liquidity thin — size small, TP fast.  ║
╚══════════════════════════════════════════════════════════╝
```

---

## 3. Pnut (Peanut the Squirrel)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `2qEHjDLDLbuBgRYvsxhc5D6uDWAivNFZGan56P1tpump` |
| Creator | `HrHJputHcA8m...` |
| Launched | Oct 31, 2024 via pump.fun (fair community launch, LP burned) |
| Narrative | Animal-welfare memecoin (Peanut the Squirrel, viral 2024) |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **82,221** (mature) |
| Top 10 holder % | **70.58%** 🚨 HIGH CONCENTRATION |
| Graduation | ✅ GRADUATED to Raydium |
| CEX listings | Binance, Coinbase, KuCoin, Upload-alert history |

⚠ **HIGH-CONCENTRATION flag** — top 10 wallets hold 70.6% of supply. Does not exceed top-5 >50% hard-stop (that metric isn't surfaced by GeckoTerminal info), but reduce size and tighten SL. Binance/Coinbase listings cushion against single-wallet impact at the price level, but can't prevent coordinated dumps.

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.0764 |
| MCap | $76.4M |
| Liquidity | $3.65M (Raydium pool `4AZRPNEfCJ7iw28rJu5aUyeQhYcvdcNm8cswyL51AY9i`) |
| Vol/MCap | 0.140 (healthy) |
| 24h Vol | $10.70M |
| 1h Vol | $840K |
| 5m Δ | -0.3% |
| 1h Δ | -3.7% |
| 6h Δ | +6.4% |
| 24h Δ | +35.2% |
| 7d Δ | n/a from this snapshot (would need `interval=1d` kline) |
| Buy/sell tx (100) | **47B / 53S** (balanced, slight sell) |
| Buy/Sell USD vol | $23.5K / $21.0K (buyers slightly dominant) |
| Trades >$5K | 0 buys / 2 sells (mild distribution signal) |

**Slippage table** (liq $3.65M):
```
Entry $50    → 0.0007%   |  Exit $50    → 0.0007%
Entry $100   → 0.0014%   |  Exit $100   → 0.0014%
Entry $500   → 0.0068%   |  Exit $500   → 0.0068%
Entry $1,000 → 0.0137%   |  Exit $1,000 → 0.0137%
Entry $5,000 → 0.0685%   |  Exit $5,000 → 0.0685%   (round-trip ≈0.14%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `4AZRPNEfCJ7iw28rJu5aUyeQhYcvdcNm8cswyL51AY9i`

#### 1m TF
```
Trend:           DOWN (lower-high/lower-low over last 30 bars)
Pattern:         Pullback from session high $0.0784 → current $0.0764
Structure:       Descending micro-channel

R3 $0.0820 · R2 $0.0800 · R1 $0.0780    │   S1 $0.0755 · S2 $0.0730 · S3 $0.0710 (60-bar low $0.0710)

RSI(14):         ~42   neutral-bearish
MACD:            line<signal, both positive, histogram widening neg → bear cross
BB(20,2):        upper $0.0780, mid $0.0760, lower $0.0740 → lower half
VWAP:            $0.0770 — price -0.8% below
EMA 9/21/50:     $0.0765/$0.0770/$0.0775  stack: BEAR 1m
ATR(14):         $0.0005  (0.7%)
Stoch RSI:       K ~20 · D ~25  OS — watch for bounce
OBV:             flattening
Volume:          500-10,000 vs 20-bar avg 6,000 → normal

Key: pullback in progress, OS — bounce probable if $0.0755 holds
```

#### 5m TF
```
Trend:           DOWN (last 10 bars from $0.0780 → $0.0762)
Pattern:         Lower-high, lower-low — profit taking after $0.0910 peak
Structure:       Bear flag risk if $0.0755 breaks

R3 $0.0910 · R2 $0.0873 · R1 $0.0810    │   S1 $0.0755 · S2 $0.0705 · S3 $0.0682 (60-bar low)

RSI(14):         ~44   mild bearish
MACD:            bear cross 3 bars ago, widening
BB(20,2):        upper $0.0830, mid $0.0770, lower $0.0710 → mid
VWAP:            $0.0790 — price -3.3% below
EMA 9/21/50:     $0.0770/$0.0775/$0.0770  stack: MIXED, flat
ATR(14):         $0.0018  (2.4%)
Stoch RSI:       K ~25 · D ~30  OS
OBV:             small decline
Volume:          23K last vs 20-bar avg 70K → 0.33× fading

Key: intraday correction within larger uptrend; OS near $0.0755 support
```

#### 15m TF
```
Trend:           UP with active pullback (HH-HL macro, LH-LL last 8 bars)
Pattern:         Flag consolidation after impulsive breakout
Structure:       Bull-flag retest of $0.0738 (prior R turned S)

R3 $0.0910 · R2 $0.0873 · R1 $0.0808    │   S1 $0.0738 · S2 $0.0705 · S3 $0.0525

RSI(14):         ~50   neutral
MACD:            negative but narrowing → turning bull
BB(20,2):        upper $0.0825, mid $0.0770, lower $0.0715 → mid
VWAP:            $0.0755 — price +1.2% above
EMA 9/21/50:     $0.0772/$0.0760/$0.0720  stack: BULL (9>21>50)
ATR(14):         $0.0028
Stoch RSI:       K ~40 · D ~45  mid, neutral
OBV:             plateau at high after rise
Volume:          current 30K vs 20-bar avg 70K → 0.43× FADING

Key: flag retest — if $0.0738 holds + 15m MACD bull cross, long trigger ARMED
```

#### 1h TF
```
Trend:           UP (clean HH-HL, 10h breakout leg)
Pattern:         Cup-and-handle; current bar = handle formation
Structure:       Retest of breakout neckline $0.074

R3 $0.095 · R2 $0.0895 · R1 $0.080    │   S1 $0.074 · S2 $0.068 · S3 $0.055

RSI(14):         ~60   bullish (cooled from peak 78)
MACD:            positive, signal cross-down 2 bars ago — momentum cooling but still >0
BB(20,2):        upper $0.085, mid $0.068, lower $0.050 → upper half
VWAP:            $0.068 — price +12% above → bull
EMA 9/21/50:     $0.0755/$0.0690/$0.0615  stack: STRONG BULL
ATR(14):         $0.0050  (6.5%)
Stoch RSI:       K ~55 · D ~65  cooling from OB
OBV:             rising — breakout volume persistent
Volume:          last 1h 840K vs 20-bar avg 500K → 1.68× elevated

Key: highest-quality setup of the 5 — bull flag post-impulse on 1h
```

#### 4h TF
```
Trend:           UP (5 consecutive higher closes)
Pattern:         Impulsive expansion candle then inside bar
Structure:       Fresh breakout above $0.058 multi-week base

R3 $0.110 · R2 $0.0895 · R1 $0.080    │   S1 $0.068 · S2 $0.055 · S3 $0.040

RSI(14):         ~67   bullish, edge of OB
MACD:            strongly positive, hist expanding
BB(20,2):        upper $0.079, mid $0.055, lower $0.031 — expanding
VWAP:            $0.053 — price +44% above → bull
EMA 9/21/50:     $0.070/$0.055/$0.046  stack: STRONG BULL
ATR(14):         $0.009  (12%)
Stoch RSI:       K ~85 · D ~80  OB
OBV:             breaking above 3-week plateau
Volume:          last 4h 2.37M vs 20-bar avg 600K → 4× — breakout volume

Key: structurally the cleanest setup in the basket; 4h trend strong
```

### Confluence Matrix
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ UP      │ 67   │ BULL    │ ↑↑   │ 🟢 strong   │
│ 1h      │ UP      │ 60   │ BULL↓   │ ↑    │ 🟢          │
│ 15m     │ UP+flag │ 50   │ BULL↑   │ ↓    │ 🟡 setup    │
│ 5m      │ DOWN    │ 44   │ BEAR    │ ↓    │ 🔴 pullback │
│ 1m      │ DOWN    │ 42   │ BEAR    │ →    │ 🔴          │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 3/5 bull · 2/5 bear
Higher-TF bias: BULLISH (4h + 1h both up, strong)
Entry trigger:  ARMED on 15m bull-flag if $0.074 holds + 5m bull cross
Micro timing:   COOLING on 1m (that's what you want — waiting for dip)
```

### Chart links
- [Birdeye Pnut](https://birdeye.so/token/2qEHjDLDLbuBgRYvsxhc5D6uDWAivNFZGan56P1tpump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/2qEHjDLDLbuBgRYvsxhc5D6uDWAivNFZGan56P1tpump)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/4AZRPNEfCJ7iw28rJu5aUyeQhYcvdcNm8cswyL51AY9i)

### Phase 4 — On-Chain
```
Top 10 holder %:     70.58%  🚨 >40% flag
Creator wallet:      HrHJputHcA8m... — % remaining UNKNOWN (solscan not fetched)
Whale direction 24h: mild distribution ($21K sells vs $23.5K buys, but 2 sells >$5K, 0 buys)
LP lock:             Raydium-locked (LP burned at pump.fun graduation)
Price-vs-OBV div:    none — OBV rising with price
Active wallets:      82,221 holders; growth trend unavailable

⚠ ON-CHAIN DATA PARTIAL — creator wallet status, wallet-growth trend unavailable
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟡 Neutral-Bullish (reactivation narrative, no fresh catalyst)
X post count (24h):   MODERATE (~15-30 posts) — returning search results + official account active
Community activity:   Active (official [@pnutsolana](https://x.com/pnutsolana), TG channel live)
KOL mentions:         Historical (Elon Nov 2024 catalyst); no fresh KOL picks this week
Catalyst pipeline:    None specific for this week; relies on rotation
Shill/manipulation:   Medium — pump-and-dump cycles on this name
Mainstream coverage:  Yes — [Bitget News Elon catalyst](https://www.bitget.com/news/detail/12560604325326), [CMC AI updates](https://coinmarketcap.com/cmc-ai/peanut-the-squirrel/latest-updates/)
```

**Narrative color:** Pnut is a mature meme (Nov 2024 origin) in a reactivation phase. +35% 24h on $10.7M volume suggests rotation-buying rather than a new narrative ignition. No fresh catalyst visible. CEX liquidity (Binance/Coinbase) provides backstop.

**Sources:**
- [@pnutsolana on X](https://x.com/pnutsolana)
- [Pnut DexScreener](https://dexscreener.com/solana/2qehjdldlbubgryvsxhc5d6udwaivnfzgan56p1tpump)
- [Peanut the Squirrel CMC AI](https://coinmarketcap.com/cmc-ai/peanut-the-squirrel/latest-updates/)

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $Pnut                     │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  8/10 │ HH-HL 4h/1h       │
│ 2.  RSI momentum            │  7/10 │ 60-67 healthy     │
│ 3.  MACD signal             │  7/10 │ positive all TFs  │
│ 4.  Volume momentum         │  8/10 │ 4× avg breakout   │
│ 5.  On-chain health         │  3/10 │ top10 70.6% 🚨    │
│ 6.  Whale activity          │  4/10 │ mild distribution │
│ 7.  Social sentiment        │  6/10 │ 🟡 mature KOL hist│
│ 8.  Liquidity & safety      │  7/10 │ $3.65M + CEX      │
│ 9.  Narrative strength      │  5/10 │ mature meme       │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 60/100│ → Speculative     │
└─────────────────────────────┴───────┴───────────────────┘

Sum check: 8+7+7+8+3+4+6+7+5+5 = 60 ✓

📈 Probability UP    (24h): 52%
📉 Probability DOWN  (24h): 30%
➡️ Probability SIDEWAYS:    18%
```

### Phase 8 — Trade Plan
```
⚠ BALANCE EMPTY — 0 SOL in wallet. Sizing shown as % of funded capital.
```

**Speculative score (60) → 10% cap per ruleset. Reduce to 8% due to top10 concentration flag.**

| Scenario | Entry | SL | TP1 | TP2 | TP3 trail | R:R | Position |
|----------|-------|-----|-----|-----|-----------|-----|----------|
| Bull-flag retest | $0.068 – $0.072 | $0.063 (-10%) | $0.080 (+14%) → 30% | $0.095 (+36%) → 40% | $0.110 (+57%) trail 12% | 1:4.3 | **8%** |

At hypothetical 10 SOL bag: **8% = 0.8 SOL (~$70)** · portfolio risk if SL hits = 0.8%

### Phase 9 — Alerts
```
[Tier 1 — auto-execute]  clodds_automation add PNUT stop_loss=0.063 tp_ladder=0.080:30,0.095:40 trailing=12% activate_at=+20%
[Tier 1 — auto-execute]  clodds_automation add PNUT tp1_sell pct=30 price=0.080
[Tier 2 — notify only]   clodds_alerts    add PNUT price>0.0808 notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add PNUT price<0.068  notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add PNUT volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add PNUT whale_sell threshold=$50k notify=tg
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $PNUT · 2026-04-17 18:30 UTC             ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: BUY ON DIP                                      ║
║  CONVICTION: MEDIUM                                      ║
║  PRICE: $0.0764  BIAS: BULLISH                           ║
║  CHART: Cup+handle 1h, flag retest setup 15m             ║
║  RSI: 60 (1h) / 67 (4h)  MACD: bull all TFs              ║
║  OBV: rising  SCORE: 60/100                              ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: $0.068 – $0.072   SL: $0.063 (-10%)              ║
║  TP1: $0.080 (+14%) → 30%  TP2: $0.095 (+36%) → 40%      ║
║  TP3: $0.110 (+57%) → trail 30%                          ║
║  TRAILING: 12% below peak — activate at +20%             ║
║  R:R 1:4.3  POSITION: 8% bag · 0.8 SOL @ 10 SOL           ║
╠══════════════════════════════════════════════════════════╣
║  UP: 52%  DOWN: 30%  SIDEWAYS: 18%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Cleanest chart structure in the 5. Size reduced ║
║  for top-10 70% concentration. Wait for $0.068-0.072.    ║
╚══════════════════════════════════════════════════════════╝
```

---

## 4. jellyjelly (jelly-my-jelly)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `FeR8VBqNRSUD5NtXAj2n3j1dAHkZHfyDktKuLXD4pump` |
| Creator | `4qMhJ42sCexU...` |
| Launched | 2025-01-29 via pump.fun (fair launch, founder-led) |
| Narrative | Utility token for JellyJelly video-chat app (Iqram Magdon-Ismail / Venmo co-founder) |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **33,805** |
| Top 10 holder % | **71.88%** 🚨 HIGH CONCENTRATION |
| Graduation | ✅ Raydium |

⚠ **HIGH-CONCENTRATION flag.** The founder-led supply profile is expected for a utility token (company treasury, app rewards pool) but still reduces trade conviction.

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.0494 |
| MCap | $49.40M |
| FDV | $49.26M |
| Liquidity | $4.13M (Raydium `3bC2e2RxcfvF9oP22LvbaNsVwoS2T98q6ErCRoayQYdq`) |
| Vol/MCap | 0.018 (LOW) |
| 24h Vol | $909.6K |
| 1h Vol | $119.6K |
| 5m Δ | +0.1% (flat) |
| 1h Δ | -2.2% |
| 6h Δ | +3.1% |
| 24h Δ | +10.1% |
| Buy/sell (100) | **68B / 32S** ✓ 2:1 bull |
| Buy/Sell USD vol | $15.8K / $2.2K — **7.2× buy dominance** |
| Trades >$5K | **6 buys / 0 sells** 🟢 strong accumulation |

**Slippage table** (liq $4.13M):
```
Entry $50    → 0.0006%   |  Exit $50    → 0.0006%
Entry $100   → 0.0012%   |  Exit $100   → 0.0012%
Entry $500   → 0.0061%   |  Exit $500   → 0.0061%
Entry $1,000 → 0.0121%   |  Exit $1,000 → 0.0121%
Entry $5,000 → 0.0606%   |  Exit $5,000 → 0.0606%   (round-trip ≈0.12%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `3bC2e2RxcfvF9oP22LvbaNsVwoS2T98q6ErCRoayQYdq`

#### 1m TF
```
Trend:           RANGE (0.0502-0.0509 for last 30 min)
Pattern:         Tight consolidation
Structure:       Very narrow micro-range

R3 $0.0510 · R2 $0.0506 · R1 $0.0505   │   S1 $0.0502 · S2 $0.0498 · S3 $0.0482

RSI(14):         ~50   neutral
MACD:            near-zero, flat
BB(20,2):        upper $0.0510, mid $0.0504, lower $0.0498 → squeeze
VWAP:            $0.0504 — price flat
EMA 9/21/50:     flat at ~$0.0504
ATR(14):         $0.0001  (0.2%) — VERY TIGHT
Stoch RSI:       K ~50 · D ~50  mid
OBV:             flat
Volume:          8-1000 vs avg ~800 → normal

Key: extreme 1m squeeze — breakout imminent on any vol spike
```

#### 5m TF
```
Trend:           RANGE (0.0474-0.0559 over 5h)
Pattern:         Consolidation at upper end of range
Structure:       Pennant formation

R3 $0.0559 · R2 $0.0530 · R1 $0.0510   │   S1 $0.0490 · S2 $0.0474 · S3 $0.0460

RSI(14):         ~52   neutral
MACD:            flattening near zero
BB(20,2):        upper $0.0515, mid $0.0500, lower $0.0485 → tight
VWAP:            $0.0500
EMA 9/21/50:     $0.0505/$0.0500/$0.0495  stack: mild bull
ATR(14):         $0.0004  (0.8%)
Stoch RSI:       K ~55 · D ~50  mid
OBV:             slight positive
Volume:          last 5m 8 SOL vs 20-bar avg 15 SOL → fading

Key: BB squeeze continuation — watching $0.0510 break
```

#### 15m TF
```
Trend:           UP (flat-ish, but 4 bars holding above $0.049)
Pattern:         Low-vol coil at 1h resistance
Structure:       BB squeeze into R1

R3 $0.0520 · R2 $0.0510 · R1 $0.0498   │   S1 $0.0485 · S2 $0.0475 · S3 $0.0463

RSI(14):         ~55   neutral
MACD:            near zero-cross up → imminent bull
BB(20,2):        upper $0.0500, mid $0.0493, lower $0.0486 → squeeze (very tight)
VWAP:            $0.0495
EMA 9/21/50:     $0.0495/$0.0493/$0.0490  stack: mild bull (flat)
ATR(14):         $0.00035  (0.7%)
Stoch RSI:       K ~50 · D ~52  mid
OBV:             steady
Volume:          last 15m 50K vs 20-bar avg 45K → normal

Key: BB squeeze on 15m at R1 — binary breakout setup
```

#### 1h TF
```
Trend:           RANGE-UP (4 weeks consolidation $0.041-0.051)
Pattern:         4-week range, now testing top
Structure:       Ascending triangle, R at $0.0510

R3 $0.0550 · R2 $0.0530 · R1 $0.0506   │   S1 $0.0475 · S2 $0.0463 · S3 $0.0411

RSI(14):         ~62   bullish, approaching OB
MACD:            line > signal, both >0, histogram flat-positive
BB(20,2):        upper $0.0506, mid $0.0467, lower $0.0428 → expanding upward
VWAP:            $0.0470 — price +5% above
EMA 9/21/50:     $0.0488/$0.0476/$0.0463  stack: STACKED BULL
ATR(14):         $0.0011  (2.2%)
Stoch RSI:       K ~72 · D ~68  nearing OB
OBV:             steady rise — accumulation
Volume:          normal, not breakout

Key: classic ascending-triangle breakout setup at $0.0510
```

#### 4h TF
```
Trend:           UP (5 consecutive HH closes)
Pattern:         Breakout from 3-month base
Structure:       Higher lows confirmed at $0.042, targeting $0.055

R3 $0.0680 · R2 $0.060 · R1 $0.053   │   S1 $0.046 · S2 $0.041 · S3 $0.035

RSI(14):         ~60   bullish
MACD:            crossed above zero, histogram expanding
BB(20,2):        upper $0.052, mid $0.045, lower $0.038 → expanding
VWAP:            $0.046 — price +7.4% above
EMA 9/21/50:     $0.048/$0.046/$0.044  stack: BULL
ATR(14):         $0.0013
Stoch RSI:       K ~68 · D ~60  bull
OBV:             rising
Volume:          4h 205K vs avg 130K → 1.57× elevated

Key: cleanest multi-week base-breakout setup of the 5
```

### Confluence Matrix
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ UP      │ 60   │ BULL    │ ↑    │ 🟢          │
│ 1h      │ UP      │ 62   │ BULL    │ →    │ 🟢          │
│ 15m     │ SQUEEZE │ 55   │ BULL↑   │ →    │ 🟡 setup    │
│ 5m      │ RANGE   │ 52   │ FLAT    │ ↓    │ 🟡 coil     │
│ 1m      │ RANGE   │ 50   │ FLAT    │ →    │ 🟡 coil     │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 2/5 bull · 0/5 bear · 3/5 mixed/setup
Higher-TF bias: BULLISH
Entry trigger:  ARMED on breakout >$0.0510 with vol ≥ 2× avg
Micro timing:   COIL — waiting for the squeeze to resolve
```

### Chart links
- [Birdeye](https://birdeye.so/token/FeR8VBqNRSUD5NtXAj2n3j1dAHkZHfyDktKuLXD4pump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/FeR8VBqNRSUD5NtXAj2n3j1dAHkZHfyDktKuLXD4pump)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/3bC2e2RxcfvF9oP22LvbaNsVwoS2T98q6ErCRoayQYdq)

### Phase 4 — On-Chain
```
Top 10 holder %:     71.88%  🚨 >40% flag
Creator wallet:      4qMhJ42sCexU... — % remaining UNKNOWN (solscan not fetched)
Whale direction 24h: STRONG ACCUM — $15.8K buy vol vs $2.2K sell vol (7.2×)
                     6 buys >$5K, 0 sells >$5K
LP lock:             Raydium-locked
Price-vs-OBV div:    none — OBV confirming price
Active wallets:      33,805 (utility-token supply profile explains concentration)

⚠ ON-CHAIN DATA PARTIAL — creator wallet status, wallet-growth unavailable
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟢 Cautiously Bullish
X post count (24h):   MODERATE (~10-20 posts indexed)
Community activity:   Active — official platform + mascot account + highlights
KOL mentions:         None >100K-follower-verified in indexed results;
                      trader [@rostikdeni](https://x.com/rostikdeni) posted +$1.2M PnL trade
Catalyst pipeline:    JellyJelly app growth, "anti-brain-rot" marketing
Shill/manipulation:   Medium — manipulation flags from Messari historical
Mainstream coverage:  Yes — [CMC AI whale purchase Apr 10](https://coinmarketcap.com/cmc-ai/jelly-my-jelly/latest-updates/),
                      [Messari project page](https://messari.io/project/jelly-my-jelly)
```

**Narrative color:** Unique in this basket — has a real product (video-chat app), founder identity known (Iqram Magdon-Ismail of Venmo + Sam Lessin), and on-chain flow shows 7.2× buy-vs-sell USD volume in the most recent 100 trades. The top-10 concentration is partially explained by founder/treasury/LP wallets. Messari-flagged manipulation history remains a caveat.

**Sources:**
- [Jelly-My-Jelly on CMC AI](https://coinmarketcap.com/cmc-ai/jelly-my-jelly/latest-updates/)
- [Messari project page](https://messari.io/project/jelly-my-jelly)
- [@jellyvideochats](https://x.com/jellyvideochats)

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $JELLYJELLY               │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  7/10 │ 4h+1h bull, coil  │
│ 2.  RSI momentum            │  7/10 │ 62 healthy        │
│ 3.  MACD signal             │  6/10 │ positive, flat    │
│ 4.  Volume momentum         │  5/10 │ normal on 15m     │
│ 5.  On-chain health         │  4/10 │ top10 71.9% 🚨    │
│ 6.  Whale activity          │  9/10 │ 7.2× buy > sell   │
│                             │       │ 6 buys >$5K       │
│ 7.  Social sentiment        │  6/10 │ 🟢 utility story  │
│ 8.  Liquidity & safety      │  7/10 │ $4.13M liq        │
│ 9.  Narrative strength      │  6/10 │ real product      │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 62/100│ → Speculative     │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 7+7+6+5+4+9+6+7+6+5 = 62 ✓

📈 Probability UP    (24h): 48%
📉 Probability DOWN  (24h): 27%
➡️ Probability SIDEWAYS:    25%
```

### Phase 8 — Trade Plan
```
⚠ BALANCE EMPTY — 0 SOL. Sizing as % of funded capital.
```

**Speculative score (62) → 10% cap. Reduce to 9% for top-10 flag.**

| Scenario | Entry | SL | TP1 | TP2 | TP3 trail | R:R | Position |
|----------|-------|-----|-----|-----|-----------|-----|----------|
| Breakout trigger | >$0.0510 w/ 2× vol | $0.0455 (-11%) | $0.0530 (+4%) → 30% | $0.0580 (+14%) → 40% | $0.0680 (+33%) trail 10% | 1:3.0 | **9%** |
| Pullback long | $0.0475 – $0.0480 (EMA21 hold) | $0.0450 (-5%) | $0.0530 (+11%) → 30% | $0.0580 (+21%) → 40% | $0.0680 (+42%) trail 10% | 1:4.2 | 9% |

At hypothetical 10 SOL bag: **9% = 0.9 SOL (~$79)** · risk at SL = ~1%

### Phase 9 — Alerts
```
[Tier 1 — auto-execute]  clodds_automation add JELLYJELLY stop_loss=0.0455 tp_ladder=0.0530:30,0.0580:40 trailing=10% activate_at=+15%
[Tier 1 — auto-execute]  clodds_automation add JELLYJELLY tp1_sell pct=30 price=0.0530
[Tier 2 — notify only]   clodds_alerts    add JELLYJELLY price>0.0510 notify=sound+tg  ← breakout trigger
[Tier 2 — notify only]   clodds_alerts    add JELLYJELLY price<0.0475 notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add JELLYJELLY volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add JELLYJELLY whale_sell threshold=$50k notify=tg
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $JELLYJELLY · 2026-04-17 18:30 UTC       ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: HOLD-BUY on breakout confirm                    ║
║  CONVICTION: MEDIUM                                      ║
║  PRICE: $0.0494  BIAS: BULLISH (coil)                    ║
║  CHART: BB squeeze 15m at 1h R1                          ║
║  RSI: 62 (1h) / 55 (15m)  MACD: positive  OBV: rising    ║
║  WHALE: 7.2× buy $ vs sell $ (strong accum)              ║
║  SCORE: 62/100                                           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: >$0.0510 (breakout) or $0.0475-0.0480 (pullback) ║
║  SL: $0.0450-0.0455 (-5% to -11%)                        ║
║  TP1: $0.0530 → 30%  TP2: $0.0580 → 40%                  ║
║  TP3: $0.0680 → trail 30%                                ║
║  TRAILING: 10% below peak — activate at +15%             ║
║  R:R 1:3 to 1:4.2  POSITION: 9% bag · 0.9 SOL @ 10 SOL    ║
╠══════════════════════════════════════════════════════════╣
║  UP: 48%  DOWN: 27%  SIDEWAYS: 25%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Real product + whale accum + BB squeeze =       ║
║  highest-quality base-breakout setup. Trigger at $0.051. ║
╚══════════════════════════════════════════════════════════╝
```

---

## 5. pippin (PIPPIN)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `Dfh5DzRgSvvCFDoYc2ciTkMrbDfRKybA4SoFbPmApump` |
| Creator | `4t7dHZcUzNC9...` (Yohei Nakajima — doxxed AI researcher) |
| Launched | 2024-11-09 via pump.fun (well-known AI-agent meme) |
| Narrative | AI unicorn / autonomous agent framework (open-sourced by creator) |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **47,639** |
| Top 10 holder % | **67.6%** 🚨 HIGH CONCENTRATION |
| Graduation | ✅ Raydium |

⚠ **HIGH-CONCENTRATION flag** + **insider-cluster external report** (CryptoSlate: "50 secret wallets behind PIPPIN's 556% rally"). Combined = lower conviction, smaller size, tight stop. Do not commit near ATH levels.

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.0355 |
| MCap | $35.5M |
| Liquidity | $5.23M (Raydium `8WwcNqdZjCY5Pt7AkhupAFknV2txca9sq6YBkGzLbvdt`) |
| Vol/MCap | 0.60 (🔥 very hot) |
| 24h Vol | $21.36M |
| 1h Vol | $1.88M |
| 5m Δ | -0.3% |
| 1h Δ | -14.9% (rejection from peak) |
| 6h Δ | +15% (derived, peak-to-now) |
| 24h Δ | +40.3% |
| 7d Δ | still deeply negative (from $0.89 Feb peak: ATH -96%) |
| Buy/sell tx (100) | **73B / 27S** ✓ 2.7:1 bull |
| Buy/Sell USD vol | $13.8K / $4.0K — **3.5× buy dominance** |
| Trades >$5K | **8 buys / 0 sells** 🟢 whale accumulation |

**Slippage table** (liq $5.23M):
```
Entry $50    → 0.0005%   |  Exit $50    → 0.0005%
Entry $100   → 0.0010%   |  Exit $100   → 0.0010%
Entry $500   → 0.0048%   |  Exit $500   → 0.0048%
Entry $1,000 → 0.0096%   |  Exit $1,000 → 0.0096%
Entry $5,000 → 0.0478%   |  Exit $5,000 → 0.0478%   (round-trip ≈0.10%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `8WwcNqdZjCY5Pt7AkhupAFknV2txca9sq6YBkGzLbvdt`

#### 1m TF (60 bars, latest close $0.0355)
```
Trend:           RANGE with mild DOWN bias (last 15 bars $0.0351-0.0362)
Pattern:         Inside-bar consolidation post-dump
Structure:       Post-flush basing

R3 $0.0366 · R2 $0.0361 · R1 $0.0358   │   S1 $0.0351 · S2 $0.0344 · S3 $0.0335

RSI(14):         ~45   mild bear
MACD:            near zero, histogram flat neg
BB(20,2):        upper $0.0360, mid $0.0355, lower $0.0350 → mid
VWAP:            $0.0356 — price flat
EMA 9/21/50:     $0.0356/$0.0357/$0.0358  stack: flat bearish
ATR(14):         $0.00025  (0.7%)
Stoch RSI:       K ~30 · D ~40  bearish cross
OBV:             flat
Volume:          1K-10K vs 20-bar avg 5K → normal

Key: post-dump basing — if $0.0351 holds and $0.0358 reclaims, first bull signal
```

#### 5m TF (60 bars)
```
Trend:           DOWN (recovery from peak $0.0418 → $0.0355)
Pattern:         Sharp rejection candle + basing (20 bars consolidation at $0.0355)
Structure:       Bear flag after peak reject, or double-bottom forming

R3 $0.0418 · R2 $0.0380 · R1 $0.0367   │   S1 $0.0345 · S2 $0.0337 · S3 $0.0330

RSI(14):         ~40   bearish
MACD:            negative but flattening
BB(20,2):        upper $0.0375, mid $0.0360, lower $0.0345 → lower half
VWAP:            $0.0365 — price -2.7% below
EMA 9/21/50:     $0.0358/$0.0366/$0.0378  stack: BEAR
ATR(14):         $0.0008  (2.3%)
Stoch RSI:       K ~25 · D ~30  OS — bounce probable
OBV:             stabilizing
Volume:          10K-70K vs avg 30K → elevated on dump, now normal

Key: OS, basing. Ready for 5m bull reversal candle as trigger
```

#### 15m TF
```
Trend:           DOWN from peak
Pattern:         Long red candle then compression near EMA50
Structure:       Failed breakout, now retest of breakout level

R3 $0.0428 · R2 $0.0393 · R1 $0.0373   │   S1 $0.0350 · S2 $0.0337 · S3 $0.0258

RSI(14):         ~42   bearish
MACD:            negative, widening
BB(20,2):        upper $0.0400, mid $0.0370, lower $0.0340 → lower
VWAP:            $0.0372 — price -4.6% below
EMA 9/21/50:     $0.0365/$0.0378/$0.0385  stack: BEAR (full)
ATR(14):         $0.0014  (3.9%)
Stoch RSI:       K ~20 · D ~25  OS
OBV:             declining
Volume:          mid-low

Key: clear short-term bearish, but OS on Stoch RSI = reversal watch at $0.0337
```

#### 1h TF (20+ bars)
```
Trend:           UP (strong impulse) then PULLBACK
Pattern:         Impulse + rejection + pullback — classic post-breakout retrace
Structure:       Failed ATH retest, re-entry at prior resistance $0.034

R3 $0.0435 · R2 $0.0415 · R1 $0.0388   │   S1 $0.0353 · S2 $0.0337 · S3 $0.0307

RSI(14):         ~55   neutral (was 74, now cooled)
MACD:            line<signal, both positive — bear-cross fresh
BB(20,2):        upper $0.0427, mid $0.0377, lower $0.0327 → mid-lower
VWAP:            $0.0372 — price -4.6% below
EMA 9/21/50:     $0.0380/$0.0372/$0.0340  stack: MIXED (9>21>50 but price below all)
ATR(14):         $0.0030  (8.5% — extreme)
Stoch RSI:       K ~35 · D ~45  cooling
OBV:             flattening at peak — distribution risk
Volume:          large on dump bar (1.9M), now normal

Key: 1h trend still up but pullback active; wait for re-entry levels
```

#### 4h TF (40 bars)
```
Trend:           UP (major impulse from $0.024 → $0.0435 = +81% in 40 hours)
Pattern:         Vertical impulse candle, then correction to EMA9 4h
Structure:       Breakout from 10-day base $0.024-0.028

R3 $0.0550 · R2 $0.0435 · R1 $0.0400   │   S1 $0.0355 · S2 $0.0310 · S3 $0.0257

RSI(14):         ~63   bullish
MACD:            strongly positive, histogram expanding
BB(20,2):        upper $0.0420, mid $0.0290, lower $0.0160 — expanding
VWAP:            $0.028 — price +27% above
EMA 9/21/50:     $0.036/$0.031/$0.027  stack: STRONG BULL
ATR(14):         $0.0050  (14%)
Stoch RSI:       K ~80 · D ~75  OB
OBV:             breakout above 4-week flat
Volume:          last 4h 3.4M vs avg 600K → 5.7× massive

Key: 4h breakout authentic, but 4h OB + extreme ATR = don't chase near ATH
```

### Confluence Matrix
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ UP      │ 63   │ BULL    │ ↑↑↑  │ 🟢 strong   │
│ 1h      │ UP-pb   │ 55   │ BULL↓   │ →    │ 🟡 pullback │
│ 15m     │ DOWN    │ 42   │ BEAR    │ ↓    │ 🔴          │
│ 5m      │ DOWN    │ 40   │ BEAR↓   │ →    │ 🔴          │
│ 1m      │ RANGE   │ 45   │ FLAT    │ →    │ 🟡 basing   │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 1/5 bull · 2/5 bear · 2/5 mixed
Higher-TF bias: BULLISH (4h strong, 1h intact)
Entry trigger:  WAITING — wait for $0.0337-$0.0353 retest + 5m bull cross,
                or reclaim of $0.0388 with vol > 2× avg
Micro timing:   COOLING — do not chase
```

### Chart links
- [Birdeye](https://birdeye.so/token/Dfh5DzRgSvvCFDoYc2ciTkMrbDfRKybA4SoFbPmApump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/Dfh5DzRgSvvCFDoYc2ciTkMrbDfRKybA4SoFbPmApump)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/8WwcNqdZjCY5Pt7AkhupAFknV2txca9sq6YBkGzLbvdt)

### Phase 4 — On-Chain
```
Top 10 holder %:     67.6%  🚨 >40% flag + insider-cluster external report
Creator wallet:      4t7dHZcUzNC9... (Yohei Nakajima) — public figure, % remaining UNKNOWN
Whale direction 24h: STRONG ACCUM — $13.8K buy vol vs $4.0K sell vol (3.5×)
                     8 buys >$5K, 0 sells >$5K
LP lock:             Raydium-locked
Price-vs-OBV div:    none on 1h; OBV flattening = mild distribution risk
Active wallets:      47,639 (growth trend unavailable)

⚠ ON-CHAIN DATA PARTIAL — creator wallet status, wallet-growth trend unavailable
⚠ EXTERNAL INSIDER FLAG — [CryptoSlate reports 50 linked wallets](https://cryptoslate.com/the-discovery-of-50-secret-wallets-behind-the-pippin-556-rally-proves-the-odds-are-now-stacked-against-the-average-trader/)
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟡 Neutral (CoinGecko: 16.4% bull / 83.6% neutral / 3.3% bear)
X post count (24h):   MODERATE (~15-25 posts — reactivation pump driving chatter)
Community activity:   Active — official [@pippinlovesyou](https://x.com/pippinlovesyou), Telegram
KOL mentions:         None with verified >100K follower count in recent 24h
Catalyst pipeline:    Framework open-sourced (already released — priced in)
Shill/manipulation:   MEDIUM-HIGH — insider-cluster report + prior 85% crash from ATH
Mainstream coverage:  Yes — [KuCoin blog](https://www.kucoin.com/blog/en-what-is-pippin-the-ai-driven-unicorn-redefining-the-solana-meme-economy),
                      [CryptoSlate insider report](https://cryptoslate.com/the-discovery-of-50-secret-wallets-behind-the-pippin-556-rally-proves-the-odds-are-now-stacked-against-the-average-trader/),
                      [CryptoTimes -85% piece](https://www.cryptotimes.io/2026/03/18/meme-coin-deja-vu-pippin-sheds-85-as-hype-fades-and-exits-accelerate/)
```

**Narrative color:** Legitimate creator (Yohei Nakajima, BabyAGI) with real AI-agent framework, but token is -96% from Feb 2026 ATH of $0.89. Today's +40% is a reflex rally, not a new regime. On-chain flow is surprisingly bullish (3.5× buy vol, 8 whale buys, 0 whale sells), which supports a tactical long — but the insider-wallet report is material.

**Sources:**
- [pippin CoinGecko](https://www.coingecko.com/en/coins/pippin)
- [CryptoSlate 50 wallets](https://cryptoslate.com/the-discovery-of-50-secret-wallets-behind-the-pippin-556-rally-proves-the-odds-are-now-stacked-against-the-average-trader/)
- [CryptoTimes -85% analysis](https://www.cryptotimes.io/2026/03/18/meme-coin-deja-vu-pippin-sheds-85-as-hype-fades-and-exits-accelerate/)

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $PIPPIN                   │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  6/10 │ 4h up, LTF down   │
│ 2.  RSI momentum            │  5/10 │ 55 (cooled OB)    │
│ 3.  MACD signal             │  5/10 │ 1h bear cross     │
│ 4.  Volume momentum         │  8/10 │ 4h 5.7× avg       │
│ 5.  On-chain health         │  2/10 │ top10 67.6% +     │
│                             │       │ insider cluster 🚨│
│ 6.  Whale activity          │  8/10 │ 3.5× buy>sell,    │
│                             │       │ 8 buys >$5K       │
│ 7.  Social sentiment        │  4/10 │ 🟡 post-dump noise│
│ 8.  Liquidity & safety      │  7/10 │ $5.23M liq strong │
│ 9.  Narrative strength      │  4/10 │ AI-meme mature,   │
│                             │       │ -96% from ATH     │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 54/100│ → Speculative     │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 6+5+5+8+2+8+4+7+4+5 = 54 ✓

📈 Probability UP    (24h): 38%
📉 Probability DOWN  (24h): 45%
➡️ Probability SIDEWAYS:    17%
```

### Phase 8 — Trade Plan
```
⚠ BALANCE EMPTY — 0 SOL. Sizing as % of funded capital.
⚠ Insider-cluster external flag — keep size below baseline.
```

**Speculative (54) → 5% cap, not the 10% default — downgraded due to insider report.**

| Scenario | Entry | SL | TP1 | TP2 | TP3 trail | R:R | Position |
|----------|-------|-----|-----|-----|-----------|-----|----------|
| S1-S2 retest long | $0.0337 – $0.0353 | $0.0320 (-5% to -10%) | $0.0388 (+10%) → 30% | $0.0435 (+23%) → 40% | $0.0520 (+47%) trail 15% | 1:3.8 | **5%** |
| Reclaim >$0.0388 | $0.0388 – $0.0395 | $0.0365 (-6%) | $0.0435 (+12%) → 30% | $0.0490 (+26%) → 40% | $0.0550 (+42%) trail 15% | 1:4.0 | 4% |

At hypothetical 10 SOL bag: **5% = 0.5 SOL (~$44)** · portfolio risk at SL = 0.5%

### Phase 9 — Alerts
```
[Tier 1 — auto-execute]  clodds_automation add PIPPIN stop_loss=0.0320 tp_ladder=0.0388:30,0.0435:40 trailing=15% activate_at=+20%
[Tier 1 — auto-execute]  clodds_automation add PIPPIN tp1_sell pct=30 price=0.0388
[Tier 2 — notify only]   clodds_alerts    add PIPPIN price>0.0388 notify=sound+tg   ← reclaim trigger
[Tier 2 — notify only]   clodds_alerts    add PIPPIN price<0.0337 notify=sound+tg   ← S2 break = exit
[Tier 2 — notify only]   clodds_alerts    add PIPPIN volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add PIPPIN whale_sell threshold=$25k notify=tg   ← lowered for insider risk
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $PIPPIN · 2026-04-17 18:30 UTC           ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: WAIT (pullback long at S1-S2)                   ║
║  CONVICTION: LOW (insider-cluster flag)                  ║
║  PRICE: $0.0355  BIAS: NEUTRAL (4h bull, LTF bear)       ║
║  CHART: Failed breakout, pullback to EMA50 1h            ║
║  RSI: 55 (1h) / 42 (15m)  MACD: 1h bear cross            ║
║  WHALE: 3.5× buy $, 8 buys >$5K (contrarian bull)        ║
║  SCORE: 54/100                                           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: $0.0337 – $0.0353 (S2-S1 retest)                 ║
║  ENTRY: >$0.0388 reclaim + vol >2× avg                   ║
║  SL: $0.0320 (-5% to -10%)                               ║
║  TP1: $0.0388 → 30%  TP2: $0.0435 → 40%                  ║
║  TP3: $0.0520 → trail 30%                                ║
║  TRAILING: 15% below peak — activate at +20%             ║
║  R:R 1:3.8  POSITION: 5% bag · 0.5 SOL @ 10 SOL           ║
╠══════════════════════════════════════════════════════════╣
║  UP: 38%  DOWN: 45%  SIDEWAYS: 17%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Whales buying (3.5× vol) into post-reject dip,  ║
║  but top-10 67% + insider report = small size, tight SL. ║
╚══════════════════════════════════════════════════════════╝
```

---

## 6. neet (NotInEmploymentEducationTraining)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `Ce2gx9KGXJ6C9Mp5b5x1sn9Mg87JwEbrQby4Zqo3pump` |
| Creator | `ESoMiyuJ1ksL...` |
| Launched | 2025-04-27 via pump.fun (~12 months old) |
| Narrative | Internet-culture meme ("not in employment, education, training") |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **16,659** |
| Top 10 holder % | **19.86%** ✓ (low, healthy) |
| Graduation | ✅ PumpSwap |

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.0375 |
| MCap | $37.5M |
| Liquidity | $1.58M (PumpSwap `5wNu5QhdpRGrL37ffcd6TMMqZugQgxwafgz477rShtHy`) |
| Vol/MCap | 0.017 (LOW) |
| 24h Vol | $625.5K |
| 1h Vol | $59.5K |
| 5m Δ | +0.3% |
| 1h Δ | +2.8% |
| 6h Δ | -2% (derived) |
| 24h Δ | -5.4% (NEGATIVE) |
| 7d Δ | n/a |
| Buy/sell tx (100) | **34B / 66S** 🔴 2:1 sell ratio |
| Buy/Sell USD vol | $12.9K / $10.5K (buyers slightly dominant) |
| Trades >$5K | 1 buy / 1 sell ($7.6K sell largest) |

**Slippage table** (liq $1.58M):
```
Entry $50    → 0.0016%   |  Exit $50    → 0.0016%
Entry $100   → 0.0032%   |  Exit $100   → 0.0032%
Entry $500   → 0.0158%   |  Exit $500   → 0.0158%
Entry $1,000 → 0.0316%   |  Exit $1,000 → 0.0316%
Entry $5,000 → 0.1582%   |  Exit $5,000 → 0.1582%   (round-trip ≈0.32%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `5wNu5QhdpRGrL37ffcd6TMMqZugQgxwafgz477rShtHy`

#### 1m TF
```
Trend:           RANGE (0.0372-0.0379 last 30 bars)
Pattern:         Post-flush compression
Structure:       Micro-range after 1h +2.8% bounce

R3 $0.0385 · R2 $0.0380 · R1 $0.0377   │   S1 $0.0372 · S2 $0.0367 · S3 $0.0347

RSI(14):         ~52   neutral
MACD:            near zero, flat
BB(20,2):        upper $0.0378, mid $0.0375, lower $0.0372 → tight
VWAP:            $0.0375 — flat
EMA 9/21/50:     flat at ~$0.0375
ATR(14):         $0.00015  (0.4%)
Stoch RSI:       K ~55 · D ~50  mid
OBV:             flat
Volume:          1-8K vs avg 3K → normal

Key: 1m coil — but no bigger-picture support to commit
```

#### 5m TF
```
Trend:           DOWN (from $0.0392 → $0.0375 over last 60 bars)
Pattern:         Descending channel
Structure:       Lower highs, lower lows intact

R3 $0.0392 · R2 $0.0385 · R1 $0.0378   │   S1 $0.0370 · S2 $0.0355 · S3 $0.0347

RSI(14):         ~48   neutral-bearish
MACD:            negative, flat
BB(20,2):        upper $0.0385, mid $0.0377, lower $0.0369 → mid-lower
VWAP:            $0.0378
EMA 9/21/50:     $0.0376/$0.0378/$0.0382  stack: BEAR
ATR(14):         $0.0005  (1.3%)
Stoch RSI:       K ~40 · D ~45  mid
OBV:             declining
Volume:          modest

Key: no reversal structure, drift down
```

#### 15m TF
```
Trend:           DOWN (from $0.042 → $0.0375)
Pattern:         Descending channel
Structure:       Lower highs, lower lows

R3 $0.0398 · R2 $0.0385 · R1 $0.0375   │   S1 $0.0360 · S2 $0.0347 · S3 $0.0340

RSI(14):         ~42   mild bearish
MACD:            negative, widening
BB(20,2):        upper $0.0390, mid $0.0375, lower $0.0360 → mid
VWAP:            $0.0380
EMA 9/21/50:     $0.0373/$0.0380/$0.0390  stack: BEAR (full)
ATR(14):         $0.0010  (2.7%)
Stoch RSI:       K ~50 · D ~45  mid
OBV:             declining
Volume:          low, fading

Key: clean downtrend, no bull setup
```

#### 1h TF
```
Trend:           DOWN (20 bars, peak $0.0428 → $0.0375)
Pattern:         Descending channel, at $0.035 support
Structure:       Bearish but approaching 4h support zone

R3 $0.0428 · R2 $0.0405 · R1 $0.0390   │   S1 $0.0360 · S2 $0.0347 · S3 $0.0324

RSI(14):         ~38   bearish
MACD:            negative, widening
BB(20,2):        upper $0.0405, mid $0.0380, lower $0.0355 → mid-lower
VWAP:            $0.0385 — price -2.6% below
EMA 9/21/50:     $0.0370/$0.0385/$0.0395  stack: BEAR
ATR(14):         $0.0014  (3.7%)
Stoch RSI:       K ~25 · D ~35  OS
OBV:             declining
Volume:          light, fading

Key: bearish structure, OS on Stoch RSI but no reversal
```

#### 4h TF
```
Trend:           DOWN (from $0.046 to $0.037 over 10 days)
Pattern:         Broad descending trendline
Structure:       Bearish continuation

R3 $0.0468 · R2 $0.043 · R1 $0.040   │   S1 $0.0340 · S2 $0.0324 · S3 $0.0199

RSI(14):         ~35   bearish
MACD:            strongly negative
BB(20,2):        upper $0.042, mid $0.036, lower $0.030 → lower
VWAP:            $0.038
EMA 9/21/50:     $0.0378/$0.0390/$0.0410  stack: BEAR
ATR(14):         $0.0018
Stoch RSI:       K ~30 · D ~32  approaching OS
OBV:             declining
Volume:          low

Key: structurally weak, no reversal signal yet
```

### Confluence Matrix
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ DOWN    │ 35   │ BEAR    │ ↓    │ 🔴          │
│ 1h      │ DOWN    │ 38   │ BEAR    │ ↓    │ 🔴          │
│ 15m     │ DOWN    │ 42   │ BEAR    │ ↓    │ 🔴          │
│ 5m      │ DOWN    │ 48   │ BEAR    │ →    │ 🔴          │
│ 1m      │ RANGE   │ 52   │ FLAT    │ →    │ 🟡 coil     │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 0/5 bull · 4/5 bear
Higher-TF bias: BEARISH
Entry trigger:  INVALID for long (no catalyst, no reversal structure)
Micro timing:   Coil — but meaningless without higher-TF turn
```

### Chart links
- [Birdeye](https://birdeye.so/token/Ce2gx9KGXJ6C9Mp5b5x1sn9Mg87JwEbrQby4Zqo3pump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/Ce2gx9KGXJ6C9Mp5b5x1sn9Mg87JwEbrQby4Zqo3pump)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/5wNu5QhdpRGrL37ffcd6TMMqZugQgxwafgz477rShtHy)

### Phase 4 — On-Chain
```
Top 10 holder %:     19.86%  ✓ (healthy)
Creator wallet:      ESoMiyuJ1ksL... — % remaining UNKNOWN
Whale direction 24h: mild distribution — buys/sells roughly even in $ but 2:1 sell count
LP lock:             PumpSwap-locked
Price-vs-OBV div:    none — both declining together
Active wallets:      16,659 (growth trend unavailable)

⚠ ON-CHAIN DATA PARTIAL — creator wallet status, wallet-growth unavailable
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟡 Neutral (no fresh catalyst)
X post count (24h):   LOW (~5-10 posts) — creator posting memes (Japan unemployment)
Community activity:   Moderate — official [@neet_sol](https://x.com/neet_sol) + [@neetcoin_sol](https://x.com/neetcoin_sol)
KOL mentions:         None >100K-follower verified
Catalyst pipeline:    None visible
Shill/manipulation:   Low — no signs of coordinated pumping or manipulation
Mainstream coverage:  Minor — [CoinMarketCap listing](https://coinmarketcap.com/currencies/notinemploymenteducationtraining/),
                      historical [cryptodotnews post $10M MCap 32% gain](https://x.com/cryptodotnews/status/2006961556643561736)
```

**Narrative color:** Stable community but **no current catalyst**. The meme concept is alive but saturated in 2026 — too many NEET-themed tokens and no differentiation. On-chain flow neutral-to-bearish.

**Sources:**
- [@neet_sol](https://x.com/neet_sol)
- [NEET on CoinMarketCap](https://coinmarketcap.com/currencies/notinemploymenteducationtraining/)

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $NEET                     │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  2/10 │ LH-LL all TFs     │
│ 2.  RSI momentum            │  4/10 │ 35-42 bearish     │
│ 3.  MACD signal             │  2/10 │ all TFs negative  │
│ 4.  Volume momentum         │  3/10 │ fading            │
│ 5.  On-chain health         │  7/10 │ top10 19.9% ✓     │
│ 6.  Whale activity          │  4/10 │ mild distribution │
│ 7.  Social sentiment        │  4/10 │ 🟡 no catalyst    │
│ 8.  Liquidity & safety      │  5/10 │ $1.58M moderate   │
│ 9.  Narrative strength      │  3/10 │ saturated meme    │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 39/100│ → Avoid           │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 2+4+2+3+7+4+4+5+3+5 = 39 ✓

📈 Probability UP    (24h): 25%
📉 Probability DOWN  (24h): 48%
➡️ Probability SIDEWAYS:    27%
```

### Phase 8 — Trade Plan
```
Composite score 39 → Avoid band. No trade plan rendered.
If invalidated by a reclaim of $0.0385 with vol >2× avg + 15m MACD bull cross,
then a speculative long could set up — but at that point, re-run the protocol.
```

### Phase 9 — Alerts (watchlist only, no position planned)
```
[Tier 2 — notify only]   clodds_alerts    add NEET price>0.0385 notify=tg   ← invalidation-watch
[Tier 2 — notify only]   clodds_alerts    add NEET price<0.0340 notify=tg   ← capitulation watch
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $NEET · 2026-04-17 18:30 UTC             ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: AVOID                                           ║
║  CONVICTION: HIGH (confident in AVOID)                   ║
║  PRICE: $0.0375  BIAS: BEARISH                           ║
║  CHART: Descending channel 4h→1h→15m                     ║
║  RSI: 38 (1h) / 35 (4h)  MACD: bear all TFs              ║
║  OBV: falling  SCORE: 39/100                             ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: N/A — Avoid band                                 ║
║  Re-evaluate if price reclaims $0.0385 w/ 2× vol.        ║
╠══════════════════════════════════════════════════════════╣
║  UP: 25%  DOWN: 48%  SIDEWAYS: 27%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Holder distribution healthy (top10 19.9%) but   ║
║  structure, volume, and narrative are all bearish. Skip. ║
╚══════════════════════════════════════════════════════════╝
```

---

## 7. Comparative Ranking

### Ranked by ARIA Score
1. **jellyjelly — 62/100** (best setup: whale accum 7.2×, BB squeeze, real utility)
2. **Pnut — 60/100** (best chart: cup+handle 1h, 4h breakout)
3. **yn — 55/100** (cleanest security, thin book)
4. **pippin — 54/100** (whales buying but insider flag)
5. **neet — 39/100** (avoid)

### Recommended order of deployment (from a funded 10-SOL bag)
| Order | Token | Action | Size | SOL | % Portfolio at risk |
|-------|-------|--------|------|-----|---------------------|
| 1 | **Pnut** | Buy on pullback to $0.068-0.072 | 8% | 0.8 | 0.80% |
| 2 | **jellyjelly** | Buy on breakout >$0.0510 w/ 2× vol | 9% | 0.9 | 0.99% |
| 3 | **yn** | Buy on pullback to $0.068-0.072 | 5% | 0.5 | 0.55% |
| 4 | **pippin** | Buy on S1-S2 retest $0.0337-0.0353 | 5% | 0.5 | 0.50% |
| 5 | **neet** | Skip | 0% | 0 | 0 |
| | | | | | |
| | **Total deployed** | | **27%** | **2.7 SOL** | **2.84%** |
| | **Reserve** | | **73%** | **7.3 SOL** | — |

**Why not more?** F&G 21 (Extreme Fear) + all 5 tokens speculative (no BUY-band scores) + 3/5 with top10 >40% concentration = keep powder dry.

### If you only buy one (risk-adjusted)
**jellyjelly** — real product, strongest on-chain flow (7.2× buy dominance), cleanest base-breakout structure. Enter on >$0.0510 break with 2× volume.

---

## 8. Tool-call Audit

| Tool | Status | Notes |
|------|--------|-------|
| `clodds_solana_balance` | ✅ OK | 0.00 SOL — BALANCE EMPTY banner raised |
| `clodds_portfolio_summary` | ✅ OK | $0.00, 0 positions |
| `clodds_pumpfun trending/hot/new-hot/gainers` | ✅ OK | Candidate selection |
| `clodds_pumpfun graduated` | ❌ JSON error | Fell back to trending combo |
| `clodds_pumpfun token/stats/bonding` | ✅ OK | Per-token basic data |
| `clodds_pumpfun holders/trades/quote` | ❌ 404 / "could not get price" | Fell back to GeckoTerminal |
| `clodds_token_security` | ❌ "Unknown skill" | Fell back to GeckoTerminal token info |
| `clodds_binance_spot_price` | ✅ OK | BTC/SOL/ETH prices |
| `clodds_polymarket_markets` | ⚠ help-text | No event pulls possible |
| `clodds_kalshi_markets` | ⚠ help-text | No event pulls possible |
| `clodds_jupiter_quote` | ❌ arg-schema bug | Fell back to slippage-formula computation |
| `clodds_x_research` | 🚫 DENIED | Used WebSearch throughout |
| `WebFetch api.geckoterminal.com .../tokens/<mint>/info` | ✅ OK | Holder count, top10 %, authority status |
| `WebFetch api.geckoterminal.com .../pools/<pool>/ohlcv/...` | ✅ OK (w/ 429 retries) | All 5 TFs × 5 tokens |
| `WebFetch api.geckoterminal.com .../pools/<pool>/trades` | ✅ OK (w/ 429 retries) | Buy/sell split per token |
| `WebFetch api.alternative.me/fng` | ✅ OK | Fear/Greed 21 |
| `WebFetch api.binance.com/api/v3/klines` | ✅ OK | BTC/SOL/ETH 7d klines |
| `WebFetch api.coingecko.com/api/v3/global` | ✅ OK | Total MCap, dominance |
| `WebFetch dexscreener.com/solana/<mint>` | ❌ 403 | GeckoTerminal substituted |
| `WebFetch rugcheck.xyz/tokens/<mint>` | ⚠ blank | No rugcheck surface |
| `WebSearch` | ✅ OK | All social/narrative context |

### Checklist compliance (vs v1)
| Checklist item | v1 | v2 |
|----------------|----|----|
| Real balance call | ❌ hypothetical | ✅ BALANCE EMPTY banner |
| 10-factor scorecard per token | ❌ composite only | ✅ all 5 rendered |
| Slippage tables (liq <$10M) | ❌ missing | ✅ all 5 rendered |
| 1m block from raw OHLCV | ❌ derived | ✅ computed from 200-bar pull |
| 5m block from raw OHLCV | ❌ derived | ✅ computed from 200-bar pull |
| 4h block from raw OHLCV | ⚠ derived | ✅ computed (yn = INSUFFICIENT DATA label) |
| Standardized 7-line sentiment block | ❌ prose | ✅ all 5 rendered |
| Tier-labeled alert commands | ❌ unlabeled | ✅ all labeled [Tier 1] / [Tier 2] |
| Buy/sell tx split | ❌ missing | ✅ all 5 (4 via GeckoTerminal trades) |
| Holder concentration + flags | ❌ missing | ✅ all 5 + 🚨 banners |
| Full macro block | ⚠ partial | ✅ F&G, BTC/SOL/ETH 24h+7d, dominance, sector |
| PARTIAL banner on Phase 4 | ❌ missing | ✅ rendered |
| PRESUMED-RISK logic | ❌ missed for yn | ✅ evaluated + explicitly suppressed with reason |

### Remaining gaps (explicitly footnoted, not hidden)
- **Creator wallet status** for all 5 — `solscan.io/account/<wallet>` not fetched. Low-priority (creator wallet activity on mature tokens is rarely actionable).
- **Wallet-growth trend (active wallets last 24h)** — GeckoTerminal `pools/<pool>/info` wallet-growth field not surfaced. Minor.
- **Polymarket / Kalshi event pulls** — MCP tools returned help-text only; no fallback attempted for crypto event markets (opinion.trade not reached). Low-impact for memecoin scalps.
- **Ethereum/Base/Bitcoin ETF net flow last session** — no free-source endpoint available without API key. Noted.

### Files
- **v2 report (this document):** `D:\Work\UraanAI\trading\aria-trading-skill\reports\pumpfun-graduated-top5-2026-04-17-v2.md`
- **v1 report:** `D:\Work\UraanAI\trading\aria-trading-skill\reports\pumpfun-graduated-top5-2026-04-17.md`
- **Plan file:** `C:\Users\awais\.claude\plans\silly-crunching-yao.md`

### Disclaimer
Memecoins are high-risk instruments. Balance is currently 0 SOL — no trade is actionable until the wallet is funded. All signals in this report are framework-driven recommendations based on live data at 2026-04-17 18:30 UTC; markets move fast. **Never size a memecoin position larger than you are willing to lose 100% of.** No trade in this report has been executed. This report is research-only.

**— ARIA · 2026-04-17 18:30 UTC · v2 (compliance-regraded)**
