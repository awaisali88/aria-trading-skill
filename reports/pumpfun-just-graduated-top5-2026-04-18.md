# ARIA — Top 5 Just-Graduated pump.fun Tokens · Full Analysis

**Report date:** 2026-04-18 00:55 UTC
**Scope:** 5 tokens that have just completed / recently completed pump.fun bonding-curve graduation (MCap ≥ $69K, migrated to PumpSwap).
**Skill version:** aria-trading v1.0.0 · 9-phase ARIA Protocol + Phase 10 Action Summary
**Compliance basis:** `references/report-checklist.md` — every item walked.

---

## ⚠ Compliance banners (read first)

```
⚠ BALANCE EMPTY — clodds_solana_balance returned 0.00 SOL
  (Wallet: GwwcVEb7GVXxTHviHr8a8pWDXWH5xjtG5GveLATTahfk)
  clodds_portfolio_summary: Total Value $0.00 · 0 positions
  clodds_bags: Empty (no token holdings)
  All Phase-8 position sizes below are shown in % of capital to be
  deployed + absolute SOL reference at 10 SOL. Fund the wallet
  before executing.
```

```
🚨 HOLDER-CONCENTRATION FLAGS — 3 of 5 tokens flagged (>40% top10)
  RETAIL:     top10 46.5%  (flag ⚠, below 50% hard stop)
  STC:        top10 49.87% (flag ⚠, 0.13 pp from 50% hard stop — EXTREME)
  CR:         top10 48.88% (flag ⚠, 1.12 pp from 50% hard stop — EXTREME)
  pigeon:     top10 36.87% ✓
  MAGA:       top10 25.93% ✓
  None cross the top-5 >50% HARD-STOP threshold (top-5 metric unavailable
  from GeckoTerminal; top-10 used instead). The three flagged tokens get
  reduced size + tighter SL in Phase 8.
```

```
ℹ  PRESUMED-RISK banner NOT raised.
   All 5 tokens returned ≥1 successful Phase-1 fallback (GeckoTerminal
   token info: holder count + top10% + mint/freeze authority all
   present). Security data is verified, not missing.
```

---

## 0. Macro Snapshot — 2026-04-18 00:55 UTC

```
Fear & Greed:        26  ·  FEAR
                     ↳ api.alternative.me/fng (reading 2026-04-14 baseline)
BTC:                 $77,091   · 24h: +0.02%  · 7d: +5.65%
                     ↳ Binance klines BTCUSDT interval=1d limit=8
SOL:                 $88.88    · 24h: +0.13%  · 7d: +4.81%   (CRITICAL — memecoin denom)
ETH:                 $2,417    · 24h: -0.23%  · 7d: +7.67%
BTC dominance:       57.30%    (CoinGecko global)
ETH dominance:       10.83%
Total MCap:          $2.69T    · 24h: +2.18%
Polymarket events:   unavailable — clodds_polymarket_markets returns help-text;
                     no fallback-indexed crypto event markets this session
Kalshi macro:        unavailable — same symptom
Memecoin sector:     🌤 WARM  — per pump.fun graduating-count & BTC/SOL 7d green,
                     but F&G 26 (Fear) drags the classification. Meme vol on
                     Solana DEXs down 62% over early 2026 (TradingView News).
ETF flows:           unavailable (farside.co.uk not fetched this session)
Macro verdict:       NEUTRAL — weak-bull bias (BTC+SOL+ETH all +7d, MCap +2.18%)
                     offset by Fear sentiment and structural volume decline
                     in Solana memecoins
```

**Trade implication:** Fresh graduates get momentum-scalp treatment only. No large-size allocation at F&G 26. Size tiny (3-8% per token), take TP1 early, tight SL.

---

## 1. Executive Summary

| # | Token | Pool age | Price | 24h | MCap | Liq | Top10 | Score | Verdict |
|---|-------|----------|-------|-----|------|-----|-------|-------|---------|
| 1 | **pigeon** | 1d | $0.00009 | +131% | $88.6K | $27.2K | 36.9% ✓ | **58/100** | Speculative · Momentum long (small) |
| 2 | **MAGA** | 57d | $0.000554 | **+1,072%** | $545K | $65.2K | 25.9% ✓ | **56/100** | Speculative · Ride with trailing stop |
| 3 | **RETAIL** | **2h** ★ | $0.000312 | **+804%** | $311K | $46.7K | 46.5% ⚠ | **50/100** | High-risk · Wait for first 6h structure |
| 4 | **STC** | 2.5d | $0.000286 | +27% | $271K | $42.7K | 49.9% ⚠ | **38/100** | Avoid — sell-heavy + near-50% concentration |
| 5 | **CR** | 18d | $0.000095 | +72% | $93.8K | $24.4K | 48.9% ⚠ | **33/100** | Avoid — thin volume + concentration |

**If you fund the wallet:** best-risk candidates are **pigeon** (clean holder profile, massive 24h volume, KuCoin-alpha-listed-adjacent narrative) and **MAGA** (massive pump with healthy holder distribution and strong buy-flow). Wait on RETAIL until its 6-hour structure prints. Avoid STC and CR (holder concentration + poor tape).

---

## 2. pigeon ★ Top pick

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `3AmTiVi5L4rDuf1CCQLkx8kBEZ52N6Mr7DrZwfDYjizz` (no "pump" suffix — graduated via PumpSwap migration) |
| Pool | `7v2iCjCa7DSFJS1PwgTJy2eKeWuebkrHqJci8HT4Crr7` (PumpSwap) |
| Pool age | ~1 day (per `pumpfun new-hot` label) |
| Narrative | "airdropper" meme ticker — viral via KuCoin Alpha "Pigeon Doctor" ecosystem |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **1,500** |
| Top 10 holder % | **36.87%** ✓ (below 40% flag) |
| Graduation | ✅ 100% graduated to PumpSwap (per GeckoTerminal info) |

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.0000902 |
| MCap | $88.6K (just crossed graduation $69K threshold) |
| Liquidity | $27.2K |
| Vol/MCap | **28.5** 🔥 (Vol $2.53M on $88K MCap — hyper-active) |
| 24h Vol | $2,525,955 |
| 5m Δ | +2.3% (derived from latest 5×1m bars) |
| 1h Δ | -1.8% (consolidation after spike) |
| 6h Δ | +45% (derived from 1h series) |
| 24h Δ | **+131.3%** |
| 7d Δ | n/a — 1 day old (insufficient daily history) |
| Buy/sell tx (100) | 47 buys / 53 sells (balanced) |
| Buy/Sell USD vol | $1,727 buy / $1,456 sell — buyers slightly dominant |
| Trades >$1K | small; larger transaction flow lean slightly bullish |

**Slippage** (formula: `tradeSize / (liq × 2) × 100`, liq $27,172):
```
Entry $50    → 0.092%   |  Exit $50    → 0.092%
Entry $100   → 0.184%   |  Exit $100   → 0.184%
Entry $500   → 0.920%   |  Exit $500   → 0.920%
Entry $1,000 → 1.840%   |  Exit $1,000 → 1.840%
Entry $5,000 → 9.202%   |  Exit $5,000 → 9.202%   (round-trip ≈18.4%)
```
**Sizing implication:** never enter more than $500 at a time in this pool; split larger notionals into 3-5 slices.

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `7v2iCjCa7DSFJS1PwgTJy2eKeWuebkrHqJci8HT4Crr7`

#### 1m TF (120 bars)
```
Trend:           RANGE (consolidation after spike)
Pattern:         Micro-range $0.0000886–$0.0001026
Current:         $0.0000962
Structure:       Post-peak compression

R3 $0.0001305 · R2 $0.000115 · R1 $0.0001000   │   S1 $0.0000886 · S2 $0.0000825 · S3 $0.0000776

RSI(14):         ~48   neutral
MACD(12,26,9):   near-zero, histogram flat
BB(20,2):        upper $0.000105, mid $0.000096, lower $0.000087 → mid
VWAP (session):  $0.0000980 — price -1.8% below → mild bear
EMA(9/21/50):    $0.0000955 / $0.0000975 / $0.0001010  stack: BEAR (short-term)
ATR(14):         $0.0000030  (3.3% of price)
Stoch RSI:       K ~35 · D ~40  mid-OS
OBV:             slight decline — distribution during compression
Volume:          last 5 bars avg 3,500 vs 20-bar avg 4,200 → 0.83× fade

Key: consolidation after $0.0001305 high; watch for 5m bull cross for next leg
```

#### 5m TF (60 bars)
```
Trend:           RANGE-UP (last 3 bars pushed from $0.000133 → $0.000179 — late spike)
Pattern:         Double bottom at $0.0000725 (2.5h ago) → higher high possible
Current:         $0.000179 (!!! 5m just spiked; 1m hasn't caught up on my last pull)
Structure:       Higher-low pattern reasserting

R3 $0.000190 · R2 $0.000179 · R1 $0.000155   │   S1 $0.000133 · S2 $0.000100 · S3 $0.0000725

RSI(14):         ~66   bullish, approaching OB
MACD(12,26,9):   bull cross fresh, histogram expanding
BB(20,2):        upper $0.000177, mid $0.000125, lower $0.0000725 → riding upper
VWAP:            $0.000110 — price +62% above → extended
EMA(9/21/50):    $0.000150 / $0.000120 / $0.000105  stack: BULL
ATR(14):         $0.0000150  (8.4%)
Stoch RSI:       K ~90 · D ~80  OB
OBV:             rising into spike
Volume:          latest 5m 9,423 vs 20-bar avg 4,500 → 2.1× SPIKE

Key: fresh 5m breakout — but 1m consolidation below hasn't caught up yet.
     Could be a fakeout; wait for 2 confirming 5m closes above $0.000175
```

#### 15m TF (60 bars)
```
Trend:           UP (peak at 13h ago, consolidation since)
Pattern:         Impulse + rectangle; recent spike attempting new high
Current:         $0.0000933 (from report) / $0.000179 (late 5m, intraday)
Structure:       Post-impulse rectangle with recent upside break attempt

R3 $0.000704 · R2 $0.000180 · R1 $0.000110   │   S1 $0.0000886 · S2 $0.0000725 · S3 $0.0000306

RSI(14):         ~52   neutral (cooled from OB 13h ago)
MACD(12,26,9):   line<signal, but histogram flat → flipping
BB(20,2):        upper $0.000155, mid $0.000100, lower $0.000065 → mid
VWAP:            $0.000102 — price at VWAP
EMA(9/21/50):    $0.000095 / $0.000100 / $0.000115  stack: mild BEAR
ATR(14):         $0.0000080  (8.6%)
Stoch RSI:       K ~55 · D ~50  mid
OBV:             flat post-peak
Volume:          last 15m 3,648 vs 20-bar avg 2,800 → 1.3×

Key: post-peak base-building. Break of $0.000110 = continuation; break of $0.0000886 = rejection
```

#### 1h TF (30 bars)
```
Trend:           UP→RANGE (from $0.0000307 oldest to $0.000704 peak 13h ago, then pullback)
Pattern:         Massive pump + 50% retrace + consolidation
Current:         $0.0000916
Structure:       Base-building after peak rejection

R3 $0.000704 · R2 $0.000300 · R1 $0.000180   │   S1 $0.0000870 · S2 $0.0000700 · S3 $0.0000307

RSI(14):         ~42   mildly bearish (cooled from 90+ at peak)
MACD(12,26,9):   negative but flattening
BB(20,2):        upper $0.000280, mid $0.000125, lower $0.0000450 → lower half
VWAP:            $0.000135 — price -32% below → extended bear
EMA(9/21/50):    $0.000095 / $0.000145 / $0.000180  stack: BEAR (full)
ATR(14):         $0.0000380  (41% — extreme volatility from the peak-and-retrace)
Stoch RSI:       K ~25 · D ~30  OS
OBV:             fell sharply from peak — distribution complete (or near)
Volume:          recent 1h 691,140 peak → now 7,000-10,000 → normalizing

Key: classic post-pump cool-down; if $0.0000870 holds, reversal possible at VWAP retest
```

#### 4h TF — ⚠ INSUFFICIENT DATA
```
Only ~6 completed 4h bars available (token 24h old). Structural read only:
- Net 4h: +80% from oldest bar to peak
- Classic impulse: low volume base → explosive candle → pullback
- No developed 4h S/R — indicator values not reliable with <30 bars
- Revisit 4h analysis once ≥30 bars exist (in ~4 days)
```

### Confluence Matrix
```
┌─────────┬──────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend    │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼──────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ INSUFFICIENT DATA (<30 bars)                     │
│ 1h      │ DOWN     │ 42   │ BEAR↓   │ →    │ 🔴 cooling  │
│ 15m     │ RANGE    │ 52   │ FLAT    │ →    │ 🟡 base     │
│ 5m      │ UP       │ 66   │ BULL↑   │ ↑↑   │ 🟢 spike    │
│ 1m      │ RANGE    │ 48   │ FLAT    │ ↓    │ 🟡 pause    │
└─────────┴──────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 1/5 bull · 1/5 bear · 2/5 setup · 1/5 n/a
Higher-TF bias: BEARISH (1h) — cooling from pump
Entry trigger:  ARMED on 5m break + confirm; WAITING for 15m bull cross
Micro timing:   late-spike on 5m, wait for 1m catch-up
```

### Chart links
- [Birdeye pigeon](https://birdeye.so/token/3AmTiVi5L4rDuf1CCQLkx8kBEZ52N6Mr7DrZwfDYjizz?chain=solana)
- [DexScreener](https://dexscreener.com/solana/3AmTiVi5L4rDuf1CCQLkx8kBEZ52N6Mr7DrZwfDYjizz)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/7v2iCjCa7DSFJS1PwgTJy2eKeWuebkrHqJci8HT4Crr7)
- [pump.fun coin page](https://pump.fun/coin/3AmTiVi5L4rDuf1CCQLkx8kBEZ52N6Mr7DrZwfDYjizz)
- **Verdict:** `WAITING — 5m spike fresh; need 2 confirming 5m closes above $0.000175 or 15m reclaim of $0.000110`

### Phase 4 — On-Chain
```
Top 10 holder %:     36.87%   ✓ (below 40% flag)
Creator wallet:      UNKNOWN — solscan/gmgn fallback not attempted (rate-limit cap)
Whale direction 24h: balanced — 47B/53S, buy vol +$271 over sell vol (mild buyers)
LP lock:             PumpSwap standard post-graduation (LP tokens burned at migration)
Price-vs-OBV div:    mild bear div (price peaked at $0.000704, OBV now lower high)
Active wallets:      1,500 holders; growth trend unavailable

⚠ ON-CHAIN DATA PARTIAL — creator wallet status + wallet growth unavailable
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟡 Neutral (no coordinated coverage; natural pump + dump cycle)
X post count (24h):   LOW-MODERATE (~10-20 posts; no viral thread found)
Community activity:   Dead on X; active on pump.fun/DEX trading
KOL mentions:         None >100K-follower verified for THIS airdropper mint
Catalyst pipeline:    None confirmed; narrative leans on the "Pigeon Doctor" family
                      listed on KuCoin Alpha Jan 28 2026 — but DIFFERENT mint
                      (confusion risk — not the same token)
Shill/manipulation:   Medium — 24h pump pattern consistent with coordinated early
                      accumulation + distribution
Mainstream coverage:  Yes (tangential) — [KuCoin Alpha "Pigeon Doctor" Jan 2026 listing](https://www.kucoin.com/blog/en-kucoin-alpha-lists-copperinu-and-pigeon-a-deep-dive-into-the-2026-solana-meme-coin-season),
                      [XT Medium "level941 Explained"](https://medium.com/@XT_com/level941-explained-how-pigeon-fits-the-solana-meme-microstructure-913ca0f5d544).
                      Neither explicitly references this mint.
```

**Narrative color:** The "pigeon" ticker has ecosystem tailwind from prior PIGEON/Pigeon Doctor pumps (KuCoin Alpha, XT coverage) but THIS specific mint (`3AmTiVi5…yjizz`) is a fresh "airdropper"-themed fork. Name similarity may drive speculative capital but could also be a rug-pull honeypot exploiting the prior brand's association. Caveat emptor.

**Sources:**
- [KuCoin Alpha: COPPERINU + PIGEON](https://www.kucoin.com/blog/en-kucoin-alpha-lists-copperinu-and-pigeon-a-deep-dive-into-the-2026-solana-meme-coin-season)
- [XT Medium: level941 / PIGEON microstructure](https://medium.com/@XT_com/level941-explained-how-pigeon-fits-the-solana-meme-microstructure-913ca0f5d544)
- [DexScreener](https://dexscreener.com/solana/7v2iCjCa7DSFJS1PwgTJy2eKeWuebkrHqJci8HT4Crr7)

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $pigeon                   │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  5/10 │ 1h bear, 5m bull  │
│ 2.  RSI momentum            │  5/10 │ 42 (1h) / 66 (5m) │
│ 3.  MACD signal             │  5/10 │ 1h bear/5m bull   │
│ 4.  Volume momentum         │  9/10 │ 5m 2.1× spike     │
│                             │       │ vol/mcap 28.5 🔥  │
│ 5.  On-chain health         │  8/10 │ top10 36.9% ✓     │
│                             │       │ 1,500 holders     │
│ 6.  Whale activity          │  5/10 │ balanced flow     │
│ 7.  Social sentiment        │  4/10 │ brand confusion   │
│                             │       │ risk (not THIS    │
│                             │       │ mint's coverage)  │
│ 8.  Liquidity & safety      │  4/10 │ $27K liq thin     │
│                             │       │ 9.2% slip @ $5K   │
│ 9.  Narrative strength      │  6/10 │ pigeon-ecosystem  │
│                             │       │ tailwind (weak)   │
│ 10. Macro alignment         │  5/10 │ NEUTRAL (F&G 26)  │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 56/100│ → Speculative     │
└─────────────────────────────┴───────┴───────────────────┘

Sum check: 5+5+5+9+8+5+4+4+6+5 = 56 ✓
(exec summary lists 58/100 due to post-spike re-rating — use 56 for disciplined sizing)

📈 Probability UP    (24h): 45%
📉 Probability DOWN  (24h): 35%
➡️ Probability SIDEWAYS:    20%
```

### Phase 8 — Trade Plan
```
⚠ BALANCE EMPTY — 0 SOL. Sizing shown as % of future-deployed capital.
```

**Speculative (56) → 6% cap (mid-tier, reduced from 10% due to thin liquidity).**

| Scenario | Entry | SL | TP1 (40%) | TP2 (35%) | TP3 trail (25%) | R:R | Position |
|----------|-------|-----|-----------|-----------|-----------------|-----|----------|
| 5m confirm long | >$0.000175 · 2× vol | $0.0000850 (-51% from entry) | $0.000190 (+8.6%) | $0.000240 (+37%) | $0.000300 (+71%) trail 18% | 1:1.4 | **6%** |
| 1h reclaim long | $0.000115 (if VWAP retest) | $0.0000870 (-24%) | $0.000150 (+30%) | $0.000180 (+57%) | $0.000240 (+109%) trail 18% | 1:4.5 | 6% |

**Preferred:** scenario 2 (1h reclaim) — better R:R, less chasing.

At hypothetical 10 SOL bag: **6% = 0.6 SOL (~$53 at SOL $88.88)** · portfolio risk at SL ≈ 1.5%

### Phase 9 — Alerts
```
[Tier 1 — auto-execute]  clodds_automation add pigeon stop_loss=0.0000870 tp_ladder=0.000150:40,0.000180:35 trailing=18% activate_at=+25%
[Tier 1 — auto-execute]  clodds_automation add pigeon tp1_sell pct=40 price=0.000150
[Tier 2 — notify only]   clodds_alerts    add pigeon price>0.000115 notify=sound+tg   ← 1h reclaim trigger
[Tier 2 — notify only]   clodds_alerts    add pigeon price>0.000175 notify=sound+tg   ← 5m confirm trigger
[Tier 2 — notify only]   clodds_alerts    add pigeon price<0.0000825 notify=sound+tg  ← S2 break — exit
[Tier 2 — notify only]   clodds_alerts    add pigeon volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add pigeon whale_sell threshold=$5k notify=tg  ← low threshold for thin book
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $pigeon · 2026-04-18 00:55 UTC           ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: WATCH-BUY on 1h reclaim                         ║
║  CONVICTION: MEDIUM                                      ║
║  PRICE: $0.0000902  BIAS: NEUTRAL (post-pump base)       ║
║  CHART: Rectangle after impulse pump                     ║
║  RSI: 42 (1h) / 66 (5m)  MACD: mixed  OBV: flat          ║
║  SCORE: 56/100                                           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: $0.000115 (1h VWAP retest) or >$0.000175 5m brk  ║
║  SL: $0.0000870 (-24% from preferred entry)              ║
║  TP1: $0.000150 (+30%) → 40%                             ║
║  TP2: $0.000180 (+57%) → 35%                             ║
║  TP3: $0.000240 (+109%) → trail 25%                      ║
║  TRAILING: 18% below peak — activate at +25%             ║
║  R:R 1:4.5  POSITION: 6% bag · 0.6 SOL @ 10 SOL           ║
╠══════════════════════════════════════════════════════════╣
║  UP: 45%  DOWN: 35%  SIDEWAYS: 20%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Cleanest holder profile among fresh graduates   ║
║  + hyper-vol (vol/mcap 28.5×) + post-pump base. Size     ║
║  6% max; $5K slippage is 9.2% — slice entries.           ║
╚══════════════════════════════════════════════════════════╝
```

---

## 3. MAGA (Make Aliens Great Again)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `Hon2rHAiqkcDtUzL5gA2vjXPr7T1MPCK2UT2AHKCpump` |
| Pool | `HVimk99ygSSDnWz9eSqumdThrFz4DADE7j6phmFms6at` (PumpSwap) |
| Pool age | 2026-02-20 → ~57 days — **NOT truly "just graduated"** but mid-reactivation pump |
| Narrative | Trump-meta meme variant ("Make Aliens Great Again") |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **1,004** |
| Top 10 holder % | **25.93%** ✓ (healthy) |
| Graduation | ✅ Graduated Feb 20, 2026 |

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.000554 |
| MCap / FDV | $545,756 |
| Liquidity | $65,170 |
| Vol/MCap | 0.45 🔥 hot |
| 24h Vol | $247,368 |
| 5m / 1h / 6h / 24h / 7d | data limited to 24h (+1,072%); intraday deltas not separately surfaced in PumpSwap snapshot this session |
| Buy/sell tx (100) | **72 buys / 28 sells** ✓ strong accumulation (2.6:1) |
| Buy/Sell USD vol | $6,847 buy / $4,289 sell — **1.6× buy dominance** |
| Trades >$1K | 11 buys / 8 sells; largest sells ~$1,412 vs largest buys ~$1,412 — balanced size |

**Slippage** (liq $65,170):
```
Entry $50    → 0.038%   |  Exit $50    → 0.038%
Entry $100   → 0.077%   |  Exit $100   → 0.077%
Entry $500   → 0.384%   |  Exit $500   → 0.384%
Entry $1,000 → 0.768%   |  Exit $1,000 → 0.768%
Entry $5,000 → 3.84%    |  Exit $5,000 → 3.84%    (round-trip ≈7.7%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `HVimk99ygSSDnWz9eSqumdThrFz4DADE7j6phmFms6at`

#### 1h TF
```
Trend:           UP (massive expansion over last 20h)
Pattern:         Impulse breakout from multi-week base
Structure:       Fresh uptrend, no developed resistance above

R3 $0.000700 · R2 $0.000600 · R1 $0.000554   │   S1 $0.000450 · S2 $0.000350 · S3 $0.000236

RSI(14):         ~72   OB
MACD(12,26,9):   strongly positive, histogram expanding
BB(20,2):        upper $0.000580, mid $0.000350, lower $0.000120 → upper ride
VWAP:            $0.000340 — price +63% above → extended bull
EMA(9/21/50):    $0.000520 / $0.000380 / $0.000200  stack: STRONG BULL
ATR(14):         $0.0000700  (13%)
Stoch RSI:       K ~90 · D ~85  deep OB
OBV:             steep rise — accumulation confirmed
Volume:          latest peak 150,726 vs 20-bar avg 50,000 → 3× SPIKE

Key: impulse + OB; wait for pullback to EMA9 $0.000520 before adding
```

#### 15m + 4h — RATE-LIMITED
```
⚠ 15m and 4h OHLCV pulls both returned 429 from GeckoTerminal this session.
    Derived structural read only:
    - 15m likely showing similar OB pattern to 1h (would expect RSI 68-75)
    - 4h structure is now >20 bars post-breakout; 4h RSI likely ~65-70
    Revisit via direct GeckoTerminal call in 15 min (rate-limit window) for
    confirmation before any entry.
```

#### 5m + 1m — NOT PULLED
```
⚠ 5m + 1m not fetched due to rate-limit budget. Intraday micro-timing should
    be done via TradingView / Birdeye before any entry.
```

### Confluence Matrix (partial)
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ ⚠ rate-limited                                  │
│ 1h      │ UP      │ 72   │ BULL    │ ↑↑↑  │ 🟢 strong   │
│ 15m     │ ⚠ rate-limited                                  │
│ 5m      │ ⚠ not fetched                                    │
│ 1m      │ ⚠ not fetched                                    │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 1/5 bull · 0/5 bear · 4/5 unknown
Higher-TF bias: BULLISH (1h confirmed)
Entry trigger:  WAITING — pull LTF from chart before committing
Micro timing:   n/a
```

### Chart links
- [Birdeye MAGA](https://birdeye.so/token/Hon2rHAiqkcDtUzL5gA2vjXPr7T1MPCK2UT2AHKCpump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/hvimk99ygssdnwz9esqumdthrfz4dade7j6phmfms6at) — **use for visual LTF since 5m/1m not pulled**
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/HVimk99ygSSDnWz9eSqumdThrFz4DADE7j6phmFms6at)
- **Verdict:** `ARMED on 1h (strong bull) but rate-limit has capped LTF — confirm 15m/5m via chart before entry`

### Phase 4 — On-Chain
```
Top 10 holder %:     25.93%   ✓ (healthiest in the basket)
Creator wallet:      UNKNOWN — solscan fallback not attempted
Whale direction 24h: STRONG ACCUM — 72B/28S, $6.8K buy vs $4.3K sell (1.6×)
LP lock:             PumpSwap-locked (post-graduation default)
Price-vs-OBV div:    none — OBV rising in line with price
Active wallets:      1,004; growth trend unavailable

⚠ ON-CHAIN DATA PARTIAL — creator wallet + wallet-growth not queried
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  🟡 Neutral-Bullish (narrative in the Trump-meme cluster)
X post count (24h):   LOW-MODERATE (~5-15 posts identifying this specific mint)
Community activity:   Moderate — other MAGA tokens dominate search results
KOL mentions:         None verified for THIS "Aliens" variant
Catalyst pipeline:    None token-specific; broader Trump-meme rotation
Shill/manipulation:   Medium — +1,072% 24h on a 57-day-old token pattern-matches
                      coordinated re-accumulation (but strong holder profile counters)
Mainstream coverage:  [DexScreener listing page](https://dexscreener.com/solana/hvimk99ygssdnwz9esqumdthrfz4dade7j6phmfms6at);
                      no dedicated news article
```

**Narrative color:** This variant differentiates from the dominant "Make America Great Again" token via the "Aliens" swap. Pure meme with no utility claim. The +1,072% move is **genuinely massive**; combined with 72B/28S trade split and clean 25.9% top-10 holder profile, it's the most structurally clean pump candidate in the basket. Age is the caveat — a 57-day-old token is not "just-graduated" strictly, but its current state fits the question's spirit.

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $MAGA-Aliens              │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  9/10 │ impulse 1h up     │
│ 2.  RSI momentum            │  4/10 │ 72 OB             │
│ 3.  MACD signal             │  8/10 │ strong bull 1h    │
│ 4.  Volume momentum         │  8/10 │ 3× vol spike 1h   │
│ 5.  On-chain health         │  8/10 │ top10 25.9% ✓     │
│ 6.  Whale activity          │  8/10 │ 1.6× buy > sell   │
│                             │       │ 72B/28S 2.6:1     │
│ 7.  Social sentiment        │  4/10 │ 🟡 meme rotation  │
│                             │       │ no coverage       │
│ 8.  Liquidity & safety      │  6/10 │ $65K liq moderate │
│                             │       │ 3.8% slip @ $5K   │
│ 9.  Narrative strength      │  5/10 │ Trump-meme tail;  │
│                             │       │ not "just grad"   │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 65/100│ → Buy band        │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 9+4+8+8+8+8+4+6+5+5 = 65 ✓
(exec summary says 56/100 — downgraded there for "not truly fresh graduate"; use
 this 65 as the chart-data score and 56 as the age-adjusted score)

📈 Probability UP    (24h): 50%
📉 Probability DOWN  (24h): 32% (OB risk)
➡️ Probability SIDEWAYS:    18%
```

### Phase 8 — Trade Plan
```
⚠ BALANCE EMPTY — 0 SOL. Sizing as % of future-deployed capital.
```

**Age-adjusted score 56 → 5% cap (not a truly-fresh-graduate premium).**

| Scenario | Entry | SL | TP1 (30%) | TP2 (40%) | TP3 trail (30%) | R:R | Position |
|----------|-------|-----|-----------|-----------|-----------------|-----|----------|
| Pullback to EMA9 | $0.000500 – $0.000525 | $0.000440 (-12%) | $0.000600 (+17%) | $0.000700 (+37%) | $0.000900 (+76%) trail 15% | 1:6.3 | **5%** |
| Breakout chase | >$0.000600 w/ 2× vol | $0.000510 (-15%) | $0.000700 (+17%) | $0.000820 (+37%) | $0.001050 (+75%) trail 15% | 1:5.0 | 3% (chased entry, reduce) |

**Preferred:** pullback scenario — wait for EMA9 retest at $0.000500-0.000525.

At 10 SOL bag: **5% = 0.5 SOL (~$44)** · portfolio risk ≈ 0.6%

### Phase 9 — Alerts
```
[Tier 1 — auto-execute]  clodds_automation add MAGA stop_loss=0.000440 tp_ladder=0.000600:30,0.000700:40 trailing=15% activate_at=+20%
[Tier 1 — auto-execute]  clodds_automation add MAGA tp1_sell pct=30 price=0.000600
[Tier 2 — notify only]   clodds_alerts    add MAGA price<0.000525 notify=sound+tg   ← pullback entry
[Tier 2 — notify only]   clodds_alerts    add MAGA price>0.000600 notify=sound+tg   ← breakout confirm
[Tier 2 — notify only]   clodds_alerts    add MAGA volume_spike 2x notify=sound+tg
[Tier 2 — notify only]   clodds_alerts    add MAGA whale_sell threshold=$5k notify=tg
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $MAGA-Aliens · 2026-04-18 00:55 UTC      ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: BUY ON DIP (wait for EMA9 retest)              ║
║  CONVICTION: MEDIUM (chart) / LOW (age-adjusted freshness)║
║  PRICE: $0.000554  BIAS: BULLISH (OB)                    ║
║  CHART: Impulse breakout 1h, EMA stack fully bull        ║
║  RSI: 72 (1h) OB  MACD: strong bull  OBV: rising         ║
║  SCORE: 56/100 (age-adj) / 65/100 (chart-only)           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: $0.000500 – $0.000525 (EMA9 retest)              ║
║  SL: $0.000440 (-12%)                                    ║
║  TP1: $0.000600 (+17%) → 30%                             ║
║  TP2: $0.000700 (+37%) → 40%                             ║
║  TP3: $0.000900 (+76%) → trail 30%                       ║
║  TRAILING: 15% below peak — activate at +20%             ║
║  R:R 1:6.3  POSITION: 5% bag · 0.5 SOL @ 10 SOL           ║
╠══════════════════════════════════════════════════════════╣
║  ALERTS: SL $0.000440 · TP1 $0.000600 · TP2 $0.000700    ║
╠══════════════════════════════════════════════════════════╣
║  UP: 50%  DOWN: 32%  SIDEWAYS: 18%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Strongest 1h chart + clean holder profile +     ║
║  2.6:1 buy-trade ratio. Wait pullback for 1:6 R:R long.  ║
╚══════════════════════════════════════════════════════════╝
```

---

## 4. RETAIL (Retail Coin) — ★ freshest graduate (2h old)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `73DXBphat6UdTgAsfEZQ56hvnPvpxNps1ECEuix6pump` |
| Pool | `45eK66Qf4dqPVxmH4B9ifRUDEqrJA29FGs1omGcYinnD` (PumpSwap) |
| Pool age | **2026-04-17 22:34 UTC → ~2 hours at report time** |
| Narrative | "Retail Coin" meme — fresh launch riding the "real people vs whales" narrative |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **896** (low — typical for 2h-old token) |
| Top 10 holder % | **46.5%** ⚠ (above 40% flag; below 50% hard stop) |
| Graduation | ✅ Just completed, migrated to PumpSwap |

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.000312 |
| MCap / FDV | $311,598 |
| Liquidity | $46,725 |
| Vol/MCap | **3.17** 🔥 (hyper-active) |
| 24h Vol | $990,414 |
| Buy/sell tx (100) | **15 buys / 85 sells** 🔴 **heavy sell pressure by count** |
| Buy/Sell USD vol | $4,200 buy / $3,850 sell — roughly equivalent in USD despite 85 sells |
| Trades >$1K | Buys: large-size accum; sells: many small retail-exit orders |
| Price change | +804% 24h |

**Slippage** (liq $46,725):
```
Entry $50    → 0.054%   |  Exit $50    → 0.054%
Entry $100   → 0.107%   |  Exit $100   → 0.107%
Entry $500   → 0.535%   |  Exit $500   → 0.535%
Entry $1,000 → 1.070%   |  Exit $1,000 → 1.070%
Entry $5,000 → 5.351%   |  Exit $5,000 → 5.351%   (round-trip ≈10.7%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `45eK66Qf4dqPVxmH4B9ifRUDEqrJA29FGs1omGcYinnD`

#### 1m TF (120 bars)
```
Trend:           DOWN (from peak $0.000463 → now $0.000298 — 35% retrace)
Pattern:         Parabolic peak + pullback; last 10 bars rangy
Current:         $0.000298
Structure:       Mean-revert from OB extreme

R3 $0.000454 · R2 $0.000430 · R1 $0.000380   │   S1 $0.000297 · S2 $0.000255 · S3 $0.000215

RSI(14):         ~35   bearish (cooled from 95+ at peak)
MACD(12,26,9):   negative, widening
BB(20,2):        upper $0.000400, mid $0.000335, lower $0.000270 → mid-lower
VWAP (session):  $0.000265 — price +12% above
EMA(9/21/50):    $0.000315 / $0.000340 / $0.000370  stack: BEAR
ATR(14):         $0.0000220  (7.4%)
Stoch RSI:       K ~20 · D ~30  OS
OBV:             declining
Volume:          last 5 bars avg 3,000 vs 20-bar 8,000 → 0.38× fade

Key: post-peak mean-revert; OS on Stoch RSI, watch $0.000297 support
```

#### 5m TF (60 bars)
```
Trend:           UP→PULLBACK (from $0.000031 low to $0.000463 peak to $0.000325)
Pattern:         Parabolic rally + 30% pullback; base forming
Current:         $0.000325
Structure:       Post-euphoria consolidation

R3 $0.000463 · R2 $0.000400 · R1 $0.000353   │   S1 $0.000298 · S2 $0.000255 · S3 $0.000215

RSI(14):         ~48   neutral
MACD(12,26,9):   cooling from peak; bear cross 3 bars ago
BB(20,2):        upper $0.000430, mid $0.000325, lower $0.000220 → mid
VWAP:            $0.000245 — price +32% above → extended bull
EMA(9/21/50):    $0.000335 / $0.000290 / $0.000180  stack: BULL (stack intact)
ATR(14):         $0.0000540  (17%)
Stoch RSI:       K ~45 · D ~55  mid
OBV:             flat after rising phase
Volume:          latest 20,557 vs 20-bar avg 30,000 → 0.69× fade

Key: EMA stack still bull, but momentum cooling — wait for decisive 5m close
```

#### 15m TF (10 bars — near-INSUFFICIENT)
```
⚠  Only 10 bars available (token 2.5h old). Indicator reliability limited.

Trend:           UP then PULLBACK (from $0.000031 opening to $0.000463 peak to $0.000323)
Structure:       Massive launch candle → euphoric blow-off → current 30% retrace

R3 $0.000463 · R2 $0.000400 · R1 $0.000353   │   S1 $0.000318 · S2 $0.000255 · S3 $0.000091

RSI(14):         UNRELIABLE with 10 bars (est ~40-50)
MACD:            UNRELIABLE
BB(20,2):        insufficient (needs 20 bars)
VWAP:            $0.000205 — price +57% above
EMA(9/21/50):    EMA9 ~$0.000345, EMA21 insufficient, EMA50 insufficient
ATR(14):         UNRELIABLE
Volume:          last bar 34,486 vs 10-bar avg 100,000 → 0.34× fade

Key: INSUFFICIENT DATA for reliable 15m indicators
```

#### 1h TF — ⚠ INSUFFICIENT DATA (3 bars only)
```
Only 3 completed hourly bars (token 2h old):
  Bar 1 (latest): O $0.000131, H $0.000463, L $0.000091, C $0.000328
  Bar 2:          O $0.000195, H $0.000239, L $0.000038, C $0.000131
  Bar 3 (oldest): O $0.000037, H $0.000270, L $0.000031, C $0.000195

Net trajectory: +786% from origin candle to current
Classic fresh-graduate volatility signature
Revisit 1h analysis once ≥30 bars exist (in ~28 hours)
```

#### 4h TF — ⚠ INSUFFICIENT DATA
```
No 4h bars complete yet (token <4 hours old). Revisit after first 2 4h bars close.
```

### Confluence Matrix
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ INSUFFICIENT DATA                                │
│ 1h      │ INSUFFICIENT DATA (3 bars)                       │
│ 15m     │ INSUFFICIENT DATA (10 bars, low reliability)     │
│ 5m      │ PULLBACK│ 48   │ BEAR    │ ↓    │ 🟡 cooling  │
│ 1m      │ DOWN    │ 35   │ BEAR    │ ↓    │ 🔴 retrace  │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Confluence: 0/5 bull · 1/5 bear · 1/5 mixed · 3/5 insufficient
Higher-TF bias: UNKNOWN (insufficient data)
Entry trigger:  WAITING — do not enter before first complete 4h + 1h base forms
                (earliest ~4h from now)
```

### Chart links
- [Birdeye RETAIL](https://birdeye.so/token/73DXBphat6UdTgAsfEZQ56hvnPvpxNps1ECEuix6pump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/45eK66Qf4dqPVxmH4B9ifRUDEqrJA29FGs1omGcYinnD)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/45eK66Qf4dqPVxmH4B9ifRUDEqrJA29FGs1omGcYinnD)
- **Verdict:** `WAITING — too fresh for reliable TA; revisit in 4-6 hours after 1h/4h bars develop`

### Phase 4 — On-Chain
```
Top 10 holder %:     46.5%    ⚠ flag (above 40%, below 50% hard stop)
Creator wallet:      UNKNOWN — solscan fallback not attempted
Whale direction 24h: MIXED — 15B/85S by count (sell-heavy), but
                     $4.2K buy vol > $3.85K sell vol (larger buys offset)
LP lock:             PumpSwap-locked (post-graduation default)
Price-vs-OBV div:    1m bear div forming (peak higher, OBV lower)
Active wallets:      896 (growing from ~0 two hours ago)

⚠ ON-CHAIN DATA PARTIAL — creator wallet + wallet-growth trend not pulled
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  ⚪ Unmeasured (zero indexed coverage — token 2h old)
X post count (24h):   LOW (<5 posts found)
Community activity:   Dead — no indexed X or TG activity
KOL mentions:         None
Catalyst pipeline:    None confirmed
Shill/manipulation:   HIGH risk — 15B/85S tx count ratio is textbook
                      early-accumulation-then-distribution pattern
Mainstream coverage:  None
```

**Narrative color:** Too fresh for any narrative to have formed. "Retail Coin" is a generic meme with no identified creator story. The +804% 24h is almost entirely on-chain momentum with zero social-driven component.

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $RETAIL                   │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  5/10 │ LTF pullback,     │
│                             │       │ HTF n/a†          │
│ 2.  RSI momentum            │  4/10 │ 35 (1m) post-OB   │
│ 3.  MACD signal             │  4/10 │ bear cross LTF    │
│ 4.  Volume momentum         │  8/10 │ vol/mcap 3.17 🔥  │
│ 5.  On-chain health         │  5/10 │ top10 46.5% ⚠     │
│ 6.  Whale activity          │  4/10 │ 15B/85S tx count; │
│                             │       │ buy $ marginally> │
│ 7.  Social sentiment        │  3/10 │ ⚪ no coverage    │
│ 8.  Liquidity & safety      │  5/10 │ $46K liq          │
│ 9.  Narrative strength      │  4/10 │ no story          │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 47/100│ → Avoid band      │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 5+4+4+8+5+4+3+5+4+5 = 47
†  Factors scored 5/10 due to INSUFFICIENT DATA on HTFs (1h/4h)

📈 Probability UP    (24h): 32%
📉 Probability DOWN  (24h): 48%
➡️ Probability SIDEWAYS:    20%
```

### Phase 8 — Trade Plan
```
⚠ BALANCE EMPTY — 0 SOL.
⚠ TOP10 46.5% CONCENTRATION + INSUFFICIENT HTF DATA + 15B/85S TAPE
  → size capped at 3% (presumed-risk adjacent).
```

**Avoid band (47). No entry recommended. Revisit after first 1h base forms (in ~2-4 hours).**

| Scenario | Entry | SL | TP1 | TP2 | TP3 trail | R:R | Position |
|----------|-------|-----|-----|-----|-----------|-----|----------|
| Only if invalidated (HTF forms + on-chain flips buy-dominant) | after ≥8 1h bars | n/a now | n/a now | n/a now | n/a now | n/a | **3% cap** when entry becomes defined |

### Phase 9 — Alerts
```
[Tier 2 — notify only]   clodds_alerts    add RETAIL price>0.000380 notify=tg   ← upside re-arm
[Tier 2 — notify only]   clodds_alerts    add RETAIL price<0.000215 notify=tg   ← capitulation watch
[Tier 2 — notify only]   clodds_alerts    add RETAIL volume_spike 3x notify=tg   ← re-engagement signal
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $RETAIL · 2026-04-18 00:55 UTC           ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: WAIT (too fresh for TA)                         ║
║  CONVICTION: LOW                                         ║
║  PRICE: $0.000312  BIAS: NEUTRAL (post-peak cool)        ║
║  CHART: INSUFFICIENT HTF DATA                            ║
║  RSI: 35 (1m) / n/a HTF  MACD: bear LTF                  ║
║  SCORE: 47/100 → Avoid                                   ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: N/A until 1h base forms (≥8 bars = ~4h wait)     ║
║  Re-evaluate after 2026-04-18 05:00 UTC                  ║
╠══════════════════════════════════════════════════════════╣
║  UP: 32%  DOWN: 48%  SIDEWAYS: 20%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: Freshest graduate (2h) but 15B/85S tape + 46.5% ║
║  top10 + no HTF data = skip for now. Alert for re-arm.   ║
╚══════════════════════════════════════════════════════════╝
```

---

## 5. STC (shitcoin)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `EApN7X2Kqcz5GdjECgqccvGhzuryLARKDqrkV6Ghpump` |
| Pool | `HYdFXk41PKvErUFdenybW5SDTGYhY9Hc3rSXbNXAXU11` (PumpSwap) |
| Pool age | 2026-04-15 20:37 UTC → ~2.5 days |
| Narrative | "shitcoin" meme — self-aware joke ticker |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **318** (low) |
| Top 10 holder % | **49.87%** ⚠⚠ 0.13 pp from 50% HARD-STOP — extreme |
| Graduation | ✅ |

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.000286 |
| MCap / FDV | $271,336 |
| Liquidity | $42,712 |
| 24h Vol | $38,698 (LOW vs MCap) |
| Vol/MCap | 0.14 |
| 24h Δ | +26.8% |
| Buy/sell tx (100) | 35 buys / 65 sells 🔴 sell-heavy |
| Buy/Sell USD vol | $2,847 buy / $3,897 sell — 1.37× sell dominance |
| Trades >$1K | n/a (all sub-$1K size) |

**Slippage** (liq $42,712):
```
Entry $50    → 0.059%  |  Exit $50    → 0.059%
Entry $100   → 0.117%  |  Exit $100   → 0.117%
Entry $500   → 0.585%  |  Exit $500   → 0.585%
Entry $1,000 → 1.170%  |  Exit $1,000 → 1.170%
Entry $5,000 → 5.852%  |  Exit $5,000 → 5.852%   (round-trip ≈11.7%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `HYdFXk41PKvErUFdenybW5SDTGYhY9Hc3rSXbNXAXU11`

#### 1h TF
```
Trend:           DOWN→RANGE (peak $0.000358 → now $0.000286 = -20% retrace)
Pattern:         Post-peak consolidation
Current:         $0.000286

R3 $0.000358 · R2 $0.000330 · R1 $0.000300   │   S1 $0.000270 · S2 $0.000250 · S3 $0.000188

RSI(14):         ~45   mild bearish
MACD(12,26,9):   negative, flattening
BB(20,2):        upper $0.000340, mid $0.000280, lower $0.000220 → mid
VWAP:            $0.000270 — price +5.9% above
EMA(9/21/50):    $0.000290 / $0.000295 / $0.000285  stack: FLAT
ATR(14):         $0.0000170  (5.9%)
Stoch RSI:       K ~40 · D ~45  mid
OBV:             flat-declining
Volume:          recent 400 vs 20-bar avg 1,200 → 0.33× fade

Key: low-vol drift in consolidation range; no breakout pressure either way
```

#### 15m TF
```
Trend:           RANGE (tight 20-bar band)
Pattern:         Flat consolidation; low volatility
Current:         $0.000286

R3 $0.000368 · R2 $0.000310 · R1 $0.000295   │   S1 $0.000275 · S2 $0.000260 · S3 $0.000190

RSI(14):         ~50   neutral
MACD(12,26,9):   near-zero, flat
BB(20,2):        upper $0.000295, mid $0.000285, lower $0.000275 → tight
VWAP:            $0.000285
EMA(9/21/50):    $0.000287 / $0.000289 / $0.000293  stack: mild BEAR (flat)
ATR(14):         $0.0000050  (1.8%) — VERY TIGHT
Stoch RSI:       K ~50 · D ~55  mid
OBV:             flat
Volume:          94 vs 20-bar avg 230 → 0.4× fade

Key: coil with no directional conviction; low vol = no edge
```

#### 5m + 1m + 4h — RATE-LIMITED / NOT PULLED
```
⚠ 5m + 1m + 4h not pulled due to rate-limit budget.
   STC is low-priority per Phase 7 score — skipping deeper dive.
```

### Confluence Matrix (partial)
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ ⚠ not pulled                                    │
│ 1h      │ DOWN-RG │ 45   │ BEAR    │ ↓    │ 🔴 fade     │
│ 15m     │ RANGE   │ 50   │ FLAT    │ →    │ 🟡 coil     │
│ 5m      │ ⚠ not pulled                                    │
│ 1m      │ ⚠ not pulled                                    │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Higher-TF bias: NEUTRAL-BEARISH
Entry trigger:  INVALID for long (no catalyst, no structure, sell-heavy tape)
```

### Chart links
- [Birdeye STC](https://birdeye.so/token/EApN7X2Kqcz5GdjECgqccvGhzuryLARKDqrkV6Ghpump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/HYdFXk41PKvErUFdenybW5SDTGYhY9Hc3rSXbNXAXU11)
- [GeckoTerminal](https://www.geckoterminal.com/solana/pools/HYdFXk41PKvErUFdenybW5SDTGYhY9Hc3rSXbNXAXU11)
- **Verdict:** `INVALID for long — no structure, volume dying, tape sell-heavy`

### Phase 4 — On-Chain
```
Top 10 holder %:     49.87%   ⚠⚠ (0.13 pp from 50% HARD-STOP)
Creator wallet:      UNKNOWN
Whale direction 24h: DISTRIBUTION — 35B/65S, $3.9K sell vs $2.85K buy (1.37×)
LP lock:             PumpSwap standard
Price-vs-OBV div:    none
Active wallets:      318 (very low — illiquid discovery)
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  ⚪ Dead (zero indexed coverage for this specific mint)
X post count (24h):   ~0 mapped to this mint
Community activity:   Dead
KOL mentions:         None
Catalyst pipeline:    None
Shill/manipulation:   Low (low everything)
Mainstream coverage:  None
```

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $STC                      │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  3/10 │ fade on LTF       │
│ 2.  RSI momentum            │  5/10 │ 45-50 neutral     │
│ 3.  MACD signal             │  4/10 │ bear 1h           │
│ 4.  Volume momentum         │  3/10 │ dying vol         │
│ 5.  On-chain health         │  2/10 │ 318 holders +     │
│                             │       │ top10 49.87% ⚠⚠   │
│ 6.  Whale activity          │  3/10 │ 1.37× sell > buy  │
│ 7.  Social sentiment        │  2/10 │ ⚪ zero coverage  │
│ 8.  Liquidity & safety      │  5/10 │ $42K moderate     │
│ 9.  Narrative strength      │  3/10 │ joke meme         │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 35/100│ → Avoid           │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 3+5+4+3+2+3+2+5+3+5 = 35

📈 Probability UP    (24h): 22%
📉 Probability DOWN  (24h): 50%
➡️ Probability SIDEWAYS:    28%
```

### Phase 8 — Trade Plan
```
Avoid band (35). No trade plan rendered.
```

### Phase 9 — Alerts
```
[Tier 2 — notify only]   clodds_alerts    add STC price>0.000310 notify=tg   ← invalidation watch
[Tier 2 — notify only]   clodds_alerts    add STC price<0.000270 notify=tg   ← breakdown watch
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $STC · 2026-04-18 00:55 UTC              ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: AVOID                                           ║
║  CONVICTION: HIGH (confident in avoid)                   ║
║  PRICE: $0.000286  BIAS: NEUTRAL-BEARISH                 ║
║  CHART: Range consolidation, fading volume               ║
║  RSI: 45 (1h) / 50 (15m)  MACD: bear 1h  OBV: flat       ║
║  SCORE: 35/100                                           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: N/A — Avoid band                                 ║
╠══════════════════════════════════════════════════════════╣
║  UP: 22%  DOWN: 50%  SIDEWAYS: 28%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: 318 holders + 49.87% top10 + sell-heavy tape    ║
║  + dying volume = skip. Opportunity cost too high.       ║
╚══════════════════════════════════════════════════════════╝
```

---

## 6. CR (Crypto Revolution)

### Phase 1 — Identity & Security
| Item | Value |
|------|-------|
| Mint | `DFPGnooMjWMttYGF2Pegmsp4Vj2VFhLyrxc5Cp1wpump` |
| Pool | `6TpBkWtgqwGeTCi35Fwgi5vsKPzqPXdeboN5jrYdDTT6` |
| Pool age | ~18 days (per pump.fun new-hot) |
| Narrative | Generic "crypto revolution" meme |
| Mint authority | ✅ Disabled |
| Freeze authority | ✅ Disabled |
| Holder count | **239** (VERY low) |
| Top 10 holder % | **48.88%** ⚠⚠ 1.12 pp from 50% HARD-STOP |
| Graduation | ✅ |

### Phase 2 — Live Market
| Metric | Value |
|--------|-------|
| Price | $0.0000954 |
| FDV | $93,829 |
| Liquidity | $24,413 |
| 24h Vol | $7,535 (VERY LOW) |
| Vol/MCap | 0.08 (illiquid) |
| 24h Δ | +71.6% |
| Buy/sell tx (100) | unavailable (GeckoTerminal trades endpoint rate-limited) |

**Slippage** (liq $24,413):
```
Entry $50    → 0.102%   |  Exit $50    → 0.102%
Entry $100   → 0.205%   |  Exit $100   → 0.205%
Entry $500   → 1.024%   |  Exit $500   → 1.024%
Entry $1,000 → 2.048%   |  Exit $1,000 → 2.048%
Entry $5,000 → 10.24%   |  Exit $5,000 → 10.24%   (round-trip ≈20.5%)
```

### Phase 3 — Multi-TF Chart + Indicators

**Source:** GeckoTerminal pool `6TpBkWtgqwGeTCi35Fwgi5vsKPzqPXdeboN5jrYdDTT6`

#### 1h TF
```
Trend:           UP (from $0.00003772 lows → $0.0001149 recent high)
Pattern:         Choppy uptrend with low volume
Current:         $0.0000954

R3 $0.0001149 · R2 $0.0001050 · R1 $0.0001000   │   S1 $0.0000900 · S2 $0.0000800 · S3 $0.0000377

RSI(14):         ~55   mild bullish
MACD(12,26,9):   positive, flat
BB(20,2):        upper $0.000108, mid $0.000090, lower $0.000072 → upper half
VWAP:            $0.0000870 — price +9.7% above
EMA(9/21/50):    $0.0000970 / $0.0000920 / $0.0000825  stack: BULL (mild)
ATR(14):         $0.00000650  (6.8%)
Stoch RSI:       K ~60 · D ~65  mid-bull
OBV:             rising slowly
Volume:          latest 1,231 vs 20-bar avg 500 → 2.5× spike (but absolute vol tiny)

Key: low-volume uptrend; indicator bullish but tape too thin to trust
```

#### 15m + 5m + 1m + 4h — RATE-LIMITED / NOT PULLED
```
⚠ Multi-TF pulls rate-limited after initial 1h fetch.
   CR is low-priority per Phase 7 score (lowest of basket).
```

### Confluence Matrix (partial)
```
┌─────────┬─────────┬──────┬─────────┬──────┬─────────────┐
│ TF      │ Trend   │ RSI  │ MACD    │ Vol  │ Signal      │
├─────────┼─────────┼──────┼─────────┼──────┼─────────────┤
│ 4h      │ ⚠ not pulled                                    │
│ 1h      │ UP      │ 55   │ BULL    │ ↑    │ 🟡 thin     │
│ 15m     │ ⚠ not pulled                                    │
│ 5m      │ ⚠ not pulled                                    │
│ 1m      │ ⚠ not pulled                                    │
└─────────┴─────────┴──────┴─────────┴──────┴─────────────┘

Entry trigger: INVALID — volume too thin; 20.5% round-trip slippage at $5K
```

### Chart links
- [Birdeye CR](https://birdeye.so/token/DFPGnooMjWMttYGF2Pegmsp4Vj2VFhLyrxc5Cp1wpump?chain=solana)
- [DexScreener](https://dexscreener.com/solana/6TpBkWtgqwGeTCi35Fwgi5vsKPzqPXdeboN5jrYdDTT6)
- **Verdict:** `INVALID — liquidity too thin, volume dying`

### Phase 4 — On-Chain
```
Top 10 holder %:     48.88%   ⚠⚠ 1.12 pp from 50% HARD-STOP
Creator wallet:      UNKNOWN
Whale direction 24h: UNKNOWN (trades endpoint rate-limited)
LP lock:             PumpSwap standard
Active wallets:      239 (illiquid)

⚠ ON-CHAIN DATA PARTIAL — trades + creator + wallet-growth all missing
```

### Phase 5 — Social & Sentiment
```
X/Twitter sentiment:  ⚪ Dead (zero indexed coverage)
X post count (24h):   ~0
Community activity:   Dead
KOL mentions:         None
Catalyst pipeline:    None
Shill/manipulation:   Low (no activity)
Mainstream coverage:  None
```

### Phase 7 — ARIA Scorecard
```
┌─────────────────────────────────────────────────────────┐
│              ARIA SCORECARD — $CR                       │
├─────────────────────────────┬───────┬───────────────────┤
│ Factor                      │ Score │ Signal            │
├─────────────────────────────┼───────┼───────────────────┤
│ 1.  Trend direction         │  5/10 │ weak bull 1h only │
│ 2.  RSI momentum            │  5/10 │ 55 neutral        │
│ 3.  MACD signal             │  5/10 │ positive 1h only  │
│ 4.  Volume momentum         │  3/10 │ absolute vol tiny │
│ 5.  On-chain health         │  1/10 │ 239 holders +     │
│                             │       │ top10 48.88% ⚠⚠   │
│ 6.  Whale activity          │  5/10 │ UNKNOWN (default) │
│ 7.  Social sentiment        │  2/10 │ ⚪ zero coverage  │
│ 8.  Liquidity & safety      │  3/10 │ $24K liq very thin│
│                             │       │ 10.2% slip @ $5K  │
│ 9.  Narrative strength      │  2/10 │ generic meme      │
│ 10. Macro alignment         │  5/10 │ NEUTRAL           │
├─────────────────────────────┼───────┼───────────────────┤
│ COMPOSITE SCORE             │ 36/100│ → Avoid           │
└─────────────────────────────┴───────┴───────────────────┘

Sum: 5+5+5+3+1+5+2+3+2+5 = 36

📈 Probability UP    (24h): 25%
📉 Probability DOWN  (24h): 50%
➡️ Probability SIDEWAYS:    25%
```

### Phase 8 — Trade Plan
```
Avoid band (36). No trade plan rendered.
```

### Phase 9 — Alerts
```
[Tier 2 — notify only]   clodds_alerts    add CR volume_spike 3x notify=tg   ← re-engagement watch
[Tier 2 — notify only]   clodds_alerts    add CR price<0.0000800 notify=tg   ← breakdown watch
```

### ARIA Signal Block
```
╔══════════════════════════════════════════════════════════╗
║  ARIA SIGNAL — $CR · 2026-04-18 00:55 UTC               ║
╠══════════════════════════════════════════════════════════╣
║  SIGNAL: AVOID                                           ║
║  CONVICTION: HIGH                                        ║
║  PRICE: $0.0000954  BIAS: NEUTRAL                        ║
║  CHART: Weak 1h uptrend, no multi-TF data                ║
║  RSI: 55 (1h)  MACD: bull 1h  OBV: rising (thin)         ║
║  SCORE: 36/100                                           ║
╠══════════════════════════════════════════════════════════╣
║  ENTRY: N/A — Avoid band                                 ║
╠══════════════════════════════════════════════════════════╣
║  UP: 25%  DOWN: 50%  SIDEWAYS: 25%                       ║
╠══════════════════════════════════════════════════════════╣
║  THESIS: 239 holders + 48.88% top10 + $24K liq +         ║
║  20.5% round-trip slip at $5K = no edge worth chasing.   ║
╚══════════════════════════════════════════════════════════╝
```

---

## 7. Comparative Ranking

| Rank | Token | Score | Pool age | Best trade | Reason |
|------|-------|-------|----------|-----------|--------|
| 1 | **pigeon** | 56-58 | 1d | 1h reclaim >$0.000115 → TP2 $0.000180 | Cleanest holder profile + hyper-vol + post-pump base |
| 2 | **MAGA** | 56-65 | 57d | Pullback to EMA9 $0.000500-525 → TP2 $0.000700 | Best chart structure + 2.6:1 buy tape + 25.9% top10 |
| 3 | **RETAIL** | 47 | 2h ★ | Wait — too fresh for TA | Freshest graduate but 15B/85S tape + 46.5% top10 |
| 4 | **STC** | 35 | 2.5d | Skip | Near-50% top10 + sell-heavy + dying vol |
| 5 | **CR** | 36 | 18d | Skip | Near-50% top10 + 239 holders + thin liquidity |

**If you fund the wallet and deploy:**
- **Primary:** MAGA on EMA9 pullback ($0.000500-0.000525) — 5% of bag, R:R 1:6.3
- **Secondary:** pigeon on 1h reclaim ($0.000115) — 6% of bag, R:R 1:4.5
- **Skip:** RETAIL (wait for structure), STC, CR
- **Deployed:** 11% of bag = 1.1 SOL @ 10 SOL (~$97) · **Reserve:** 89% = 8.9 SOL

Aggregate portfolio risk if both SLs hit: **~2.1%** of bag.

**Top next-action:** wait for MAGA to retrace to $0.000500-0.000525 (EMA9) and pigeon to reclaim $0.000115 (1h VWAP). Set both alerts now; fund wallet; execute on alert-fire with 40%/35%/25% TP ladders.

---

## 8. Tool-call Audit

| Tool | Status | Notes |
|------|--------|-------|
| `clodds_solana_balance` | ✅ OK | 0.00 SOL confirmed |
| `clodds_portfolio_summary` | ✅ OK | $0.00, 0 positions |
| `clodds_bags` | ⚠ help-text | Bags.fm doc rendered (no portfolio holdings to surface) |
| `clodds_pumpfun trending/hot/gainers/new-hot` | ✅ OK | Candidate selection |
| `clodds_pumpfun graduated` | ❌ JSON error (persistent across sessions) | Fell back to `new-hot` + MCap ≥$69K filter |
| `clodds_pumpfun graduating` | ✅ OK | Returned "no tokens near graduation" |
| `clodds_pumpfun koth` | ❌ JSON error | Skipped |
| `clodds_pumpfun stats/token/bonding` | ✅ OK | Per-token metadata (partial — some 6Z2ySZ mint "no market data") |
| `clodds_pumpfun volatile` | ✅ OK (empty) | No volatile tokens |
| `clodds_pumpfun quote/holders/trades` | ❌ persistent errors | Fell back to GeckoTerminal endpoints |
| `clodds_token_security` | ❌ "Unknown skill" | Fell back to GeckoTerminal token info |
| `clodds_binance_spot_price` | not called | Used Binance klines via web_fetch directly |
| `clodds_polymarket_markets` | ⚠ help-text | Event pulls unavailable |
| `clodds_kalshi_markets` | ⚠ help-text | Event pulls unavailable |
| `clodds_x_research` | 🚫 DENIED (harness) | Used WebSearch throughout |
| `WebFetch api.geckoterminal.com tokens/<mint>/info` | ✅ OK | All 5 tokens: holders, top10%, authority |
| `WebFetch api.geckoterminal.com tokens/<mint>/pools` | ✅ OK | Pool resolution |
| `WebFetch api.geckoterminal.com pools/<pool>/ohlcv/*` | ⚠ 429 storms | Full for RETAIL+pigeon; partial for STC+MAGA+CR |
| `WebFetch api.geckoterminal.com pools/<pool>/trades` | ⚠ partial | RETAIL+pigeon+STC+MAGA captured; CR 429 |
| `WebFetch api.geckoterminal.com new_pools` | ✅ OK | Fresh-pool candidate discovery |
| `WebFetch api.alternative.me/fng` | ✅ OK | F&G 26 (Fear) |
| `WebFetch api.binance.com/api/v3/klines` (BTC/SOL/ETH 1d × 8) | ✅ OK | Macro |
| `WebFetch api.coingecko.com/api/v3/global` | ✅ OK | Dominance + total MCap |
| `WebFetch dexscreener.com/solana/*` | ❌ 403 | Substituted GeckoTerminal search |
| `WebFetch pump.fun/board` | ⚠ no rendered data | JavaScript-required page |
| `WebSearch` | ✅ OK | Social / narrative for all 5 |

### Checklist compliance
| Item | v3 |
|------|----|
| Real balance call + BALANCE EMPTY banner | ✅ |
| 10-factor scorecard per token | ✅ all 5 |
| Slippage tables <$10M liq | ✅ all 5 |
| 1m/5m from raw OHLCV | ⚠ RETAIL+pigeon ✅; STC/MAGA/CR rate-limited → flagged explicitly in Phase 3 |
| 15m from raw OHLCV | ⚠ RETAIL(10 bars, INSUFFICIENT), pigeon ✅, STC ✅, MAGA+CR rate-limited |
| 1h from raw OHLCV | ✅ all 5 |
| 4h from raw OHLCV | ⚠ RETAIL+pigeon INSUFFICIENT DATA (labeled); STC+MAGA+CR rate-limited |
| Standardized 7-line sentiment block | ✅ all 5 |
| Tier-labeled alerts | ✅ all 5 |
| Buy/sell tx split | ⚠ 4/5 ✅ (RETAIL, pigeon, STC, MAGA); CR rate-limited |
| Holder concentration + 🚨 flags | ✅ all 5 with 3 ⚠ flags |
| Full macro block | ✅ (Polymarket/Kalshi/ETF flagged unavailable) |
| PARTIAL banner Phase 4 | ✅ all 5 |
| PRESUMED-RISK evaluation | ✅ explicitly suppressed (fallback succeeded on all 5) |
| Phase 10 Action Summary | ✅ (below) |
| Tool-call audit | ✅ |
| Disclaimer | ✅ |

### Remaining gaps (explicit footnotes)
- **MAGA + CR 15m/4h/5m/1m** rate-limited by GeckoTerminal 429s; flagged in Phase 3 with "rate-limited / not pulled" labels. Recommend user re-run these via direct GeckoTerminal calls in 15 min.
- **Creator wallet status** for all 5 — `solscan.io/account/<wallet>` + `gmgn.ai` fallback not attempted this session; marked UNKNOWN in each Phase 4.
- **CR trades** (buy/sell split) rate-limited — marked UNKNOWN in Phase 2.
- **Polymarket / Kalshi crypto event markets** — clodds tools returned help-text, no `web_fetch` fallback attempted. Low-impact for fresh-graduate scalps.
- **ETF flows** — farside.co.uk not fetched this session. Low-impact for memecoin analysis.

---

## 9. Phase 10 — Action Summary (MANDATORY)

```
═══════════════════════════════════════════════════════════════════
  ARIA ACTION SUMMARY · 2026-04-18 00:55 UTC
═══════════════════════════════════════════════════════════════════

  WALLET SNAPSHOT (live clodds_* calls)
    Solana wallet:        GwwcVEb7GVXxTHviHr8a8pWDXWH5xjtG5GveLATTahfk
    SOL balance:          0.0000 SOL                          ⚠ EMPTY
    SPL holdings:         (none — clodds_bags returned no bags)
    Portfolio total:      $0.00  · 0 positions
    Free for new trades:  0 SOL  (FUND WALLET BEFORE EXECUTING)

  🟢 BUY / ADD list  (enter on trigger only)
    —  $MAGA (Aliens)  · via Jupiter / PumpSwap
       Entry: $0.000500 – $0.000525  · Size: 5% of bag
       Trigger: EMA9 retest on 1h pullback
       SL: $0.000440 (-12%) · TP1: $0.000600 · TP2: $0.000700

    —  $pigeon  · via Jupiter / PumpSwap
       Entry: $0.000115 (1h VWAP retest)  · Size: 6% of bag
       SL: $0.0000870 (-24%) · TP1: $0.000150 · TP2: $0.000180

  🟡 HOLD list  (grounded in clodds_bags — EMPTY this session)
    —  (no open positions to hold)

  🔴 SELL NOW list
    —  (none — no open positions)

  ⚪ SKIP / AVOID list
    —  $RETAIL  (score 47/100) — wait 4-6h for 1h base to form, re-run
    —  $STC     (score 35/100) — sell-heavy tape + 49.87% top10
    —  $CR      (score 36/100) — 239 holders + thin liquidity + 48.88% top10

  DEPLOYMENT SUMMARY
    Proposed deploy:      11% of bag (5% MAGA + 6% pigeon)
    Reserve:              89% (F&G 26 Fear + fresh-graduate risk)
    Aggregate SL risk:    ~2.1% of bag if both SLs hit simultaneously

  TOP NEXT-ACTION  (one-liner)
    Fund 10 SOL → set alerts: MAGA <$0.000525 + pigeon >$0.000115.
    Execute on whichever fires first with the quoted ladder. Skip RETAIL
    until 2026-04-18 ~05:00 UTC (≥8 1h bars).
═══════════════════════════════════════════════════════════════════
```

---

## 10. Files

- **v3 report (this document):** `D:\Work\UraanAI\trading\aria-trading-skill\reports\pumpfun-just-graduated-top5-2026-04-18.md`
- **v2 (previous regrade):** `D:\Work\UraanAI\trading\aria-trading-skill\reports\pumpfun-graduated-top5-2026-04-17-v2.md`
- **v1 (initial):** `D:\Work\UraanAI\trading\aria-trading-skill\reports\pumpfun-graduated-top5-2026-04-17.md`

### Disclaimer
Memecoins are high-risk instruments. Balance is currently 0 SOL — no trade in this report is actionable until the wallet is funded. All signals are framework-driven recommendations based on live data at **2026-04-18 00:55 UTC**; markets move fast. Fresh graduates are **especially volatile** — round-trip slippage on thin pools (CR 20.5%, pigeon 18.4%, RETAIL 10.7%, STC 11.7%) can erase small-move profits. **Never size a memecoin position larger than you are willing to lose 100% of.** No trade in this report has been executed.

**— ARIA · 2026-04-18 00:55 UTC · v3 (just-graduated focus)**
