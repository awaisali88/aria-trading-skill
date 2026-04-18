# ARIA Pre-Pump Scan — Forward-Looking Predictive Scanner

Load this file when the user asks for: *"top N coins about to go up"*, *"what's about to pump"*, *"pre-pump scan"*, *"predictive scan"*, *"hunt for next-hour movers"*, *"rescan"*, or any phrasing that asks for forward-looking momentum picks over a 15min-to-4h horizon.

**This is NOT the standard 9-phase ARIA Protocol.**
- **9-Phase Protocol** = deep analysis of 1–5 known tokens the user named
- **Pre-Pump Scan** = fast predictive sweep across 50–150 candidates, ranked by the Pre-Pump Signal Score, producing top N actionable setups for the next hour

Default N = 5 if the user doesn't specify.

---

## Step 1: Build the candidate universe (run all four surfaces in parallel)

```
1. pump.fun surface (Solana memecoins):
   clodds_pumpfun trending            → top 20 by 24h vol
   clodds_pumpfun hot                 → top 20 by 1h tx activity
   clodds_pumpfun gainers             → top 20 24h gainers
   clodds_pumpfun new-hot             → top 10 hottest fresh launches
   clodds_pumpfun graduating          → bonding curve >60%
   clodds_pumpfun koth                → King of the Hill candidates
   clodds_pumpfun volatile            → top volatility plays

2. Solana DEX broader surface:
   web_fetch https://api.geckoterminal.com/api/v2/networks/solana/trending_pools?page=1

3. CEX surface (for BTC/alt momentum):
   clodds_signals                     → live breakout/reversal signals
   Binance 24hr ticker sweep (prefer MCP, fall back to REST — see tool-inventory.md):
     1. Preferred: Binance MCP get_24hr_ticker / get_ticker_24hr (returns all symbols)
     2. Fallback: web_fetch https://api.binance.com/api/v3/ticker/24hr
     → sort by priceChangePercent, take top 30 and bottom 30 (reversal candidates)

4. Clodds intelligence:
   clodds_edge "what's about to move next hour crypto"
   clodds_ai_strategy "short-term momentum setups right now"
```

Merge and deduplicate → raw candidate universe (typically 100–200 tokens).

---

## Step 2: Pre-filter gates (disqualify immediately, don't score)

Remove any candidate that fails ANY gate:

| Gate | Threshold |
|---|---|
| Liquidity too thin | <$500K for memecoins · <$3M for CEX alts |
| Already too hot | Up >40% in last 1h (move is halfway done) |
| Manipulation risk | Vol/mcap ratio > 300% |
| Too new | Token age < 1h (insufficient data) |
| Known rug | flagged by `clodds_token_security` or `rugcheck.xyz` |
| Security failure | Mint authority not revoked · top 5 holders >50% |

Post-filter universe: typically 30–60 candidates.

---

## Step 3: Score each survivor on the Pre-Pump Signal (100 points, weighted)

Pull fresh data for each survivor:
```
GeckoTerminal pool OHLCV at 5m, 15m, 1h, 4h
GeckoTerminal pool trades (last 1h, filter for >$1K whales)
Binance klines (if CEX-listed) at 5m, 15m, 1h
```

Compute 9 factors. Weights are chosen so no single hot factor can carry the score — **confluence is required**:

| # | Factor | Weight | "Primed" = full points when |
|---|--------|--------|---------------------------------|
| 1 | Technical setup | 20 | BB squeeze on 15m pressing R1 · OR ascending triangle · OR bull flag · OR oversold-bounce basing — AND EMA9>EMA21>EMA50 stack — AND RSI 45–65 (room to run, not extended) |
| 2 | Multi-TF confluence | 15 | 1h trend bull + 4h bull bias + 15m pulling back to support OR 5m bull-reversal candle |
| 3 | Volume acceleration | 15 | 1h vol ≥ 1.5× of preceding-6h avg AND OBV rising (not just price) |
| 4 | Whale net buy (1h) | 15 | Net USD from trades >$1K is positive AND ≥ $10K |
| 5 | Momentum divergence | 10 | Bullish RSI divergence on 15m/1h (price made lower low, RSI made higher low) |
| 6 | Narrative ignition | 10 | X post velocity last 1h ≥ 3× rolling-24h avg · OR fresh catalyst news in last 4h (listing, KOL pick, partnership, token-2.0) |
| 7 | Social acceleration | 5 | Active community + ≥1 KOL mention (>100K followers) in last 24h |
| 8 | Holder health | 5 | Top 10 <40% · mint/freeze renounced · creator hasn't sold >20% in last 7d |
| 9 | Macro tailwind | 5 | BTC/SOL green on 1h · F&G rising vs yesterday's close |

Scoring thresholds:
- **≥ 75 → 🟢 BUY-NOW** (high conviction, move expected in next 30–60 min)
- **55–74 → 🟡 WAIT-FOR-TRIGGER** (setup armed but not fired; define specific trigger + wait time)
- **< 55 → ⚪ SKIP** (include in the "surfaced for transparency" tail)

---

## Step 4: Rank and emit Top N

Sort survivors by score descending. Take top N (user-specified; default 5).

**For each top-N result, emit this condensed block — NOT the full 9-phase walk.** Scanning 5 tokens with the deep protocol is slow and blows the 1-hour window:

```
─────────────────────────────────────────────────────────────
① $[TICKER] — Score XX/100 · [BUY-NOW / WAIT-FOR-TRIGGER] · [VENUE]
   Price now:  $X.XX       ·  1h Δ: ±X%   ·  24h Δ: ±X%
   Liquidity:  $X.XM       ·  Vol/MCap: X%
   
   Technical:    [pattern] on [TF] · R1 $X · RSI XX (room to run / OB / OS)
   Multi-TF:     4h [bull/bear] · 1h [bull/bear] · 15m [setup type]
   Volume:       1h vol X.X× last-6h avg · OBV [rising / flat / falling]
   Whale (1h):   +/- $XK net · N trades >$1K (buys:N, sells:N)
   Divergence:   [bullish on 15m / bearish on 1h / none]
   Catalyst:     [KOL @handle (XK followers) at HH:MM / news / listing / none]
   Social:       X posts last 1h vs 24h avg Y — Z× acceleration
   Holder:       top10 X% · mint/freeze [✓/✗] · creator [held/sold]
   Macro:        [supports / neutral / opposes]
   
   Entry:        [BUY NOW @ $X] OR [trigger: break >$X with vol ≥ 1.5× avg]
   Size:         [X SOL / $XXX based on live balance from Phase 8]
   SL:           $X (-X%)
   1h target:    $X (+X%)                 ← primary TP window
   2-4h target:  $X (+X%)                 ← extension if momentum holds
   
   → Expected move window: next XX minutes
   → Confidence: [HIGH / MEDIUM / LOW]
─────────────────────────────────────────────────────────────
```

Keep each block to ~15 lines. The scan is for fast decision-making, not deep analysis. If the user wants the deep 9-phase read on a specific winner, they can follow up with `Use ARIA. Full analysis on $[TICKER].`

---

## Step 5: Tail — transparency list

After the top N, list the next 5–10 tokens that scored 40–54 with their one-line skip reason. This helps the user see what was considered and build intuition for the scanner's judgment:

```
Also surfaced (scored 40-54, not in top N):
  $AAA — 52/100 — volume fading, 15m EMA bear
  $BBB — 48/100 — whales net sell $12K last 1h
  $CCC — 41/100 — RSI already 78, overextended
  $DDD — 40/100 — bull catalyst priced in Friday
```

---

## Step 6: Render the Phase 10 Action Summary

Same as the standard 9-phase protocol (see `aria-protocol.md` Phase 10). Pull live balances from every connected venue and render:

- 🟢 **BUY / ADD** = all BUY-NOW scored candidates, with venue, size, entry, SL, TP
- 🟡 **HOLD-WATCH** = all WAIT-FOR-TRIGGER candidates, with trigger level + size ready to deploy
- 🔴 **SELL NOW** = any existing position whose thesis just broke (detected via cross-reference with `clodds_bags`)
- ⚪ **SKIP** = top tail from Step 5
- Deployment % + aggregate SL risk + **Top next-action** one-liner

The Action Summary is the tl;dr the user acts on. Make the Top next-action unambiguous.

---

## Continuous-run mode (rescan / delta)

When the user invokes the scanner repeatedly (e.g. every 20–30 minutes), produce a delta block at the top of each new report comparing to the previous saved scan.

**Save every scan to:** `reports/pre-pump-scan-YYYY-MM-DD-HHMM.md`

**On every rerun, compare to the most recent prior scan file and render at the top:**

```
📊 Δ from last scan ([HH:MM UTC · N minutes ago]):
  ↑ NEW in top N:    $AAA (entered at 82), $BBB (entered at 78)
  ↓ DROPPED out:     $XXX (was 81, now 52), $YYY (was 76, now 48)
  ↗ GAINING score:   $ZZZ (72 → 84)   — momentum building, consider size-up
  ↘ LOSING score:    $WWW (78 → 63)   — fading, tighten stops or exit early
  = STABLE:          $VVV (75 → 77)   — thesis intact
```

**Suggested cadence:** re-run every **20–30 minutes** during active market hours. NOT every 5 minutes — 1h-horizon setups don't change that fast, and you'll burn API quota (and the user's attention).

**Note to the user on "continuous":** Claude Code cannot run background tasks on its own. "Continuous" means the user re-invokes the scan on a cadence. To make that easy:
- Single-word rerun: `Use ARIA. Rescan.`
- Alternative: the user can set up a schedule via the `/loop` or `/schedule` skills to trigger the scan automatically at an interval.

---

## Invocation phrases (all load this file)

- `Use ARIA. Find me top 5 coins about to go up next hour.`
- `Use ARIA. Pre-pump scan — top 10 setups.`
- `Use ARIA. What's about to pump?`
- `Use ARIA. Predictive scan, 1-hour horizon, top N.`
- `Use ARIA. Hunt for next-hour movers.`
- `Use ARIA. Rescan.` → triggers delta mode vs last saved `reports/pre-pump-scan-*.md`
- `Use ARIA. Pre-pump top 3 on Solana only.` → restrict universe to Solana DEX
- `Use ARIA. Pre-pump top 5 on Binance only.` → restrict to CEX candidates

---

## Rules for the predictive scan

**Always:**
- Run all four universe surfaces in parallel (Step 1) — single message, parallel tool calls
- Apply the pre-filter gates strictly (Step 2) — no score, no analysis on disqualified candidates
- Show the Pre-Pump Signal Score breakdown for every top-N result so the user can audit the reasoning
- Ground all sizes in the real `clodds_*_balance` output (Phase 8 rules apply)
- Include both a BUY-NOW list and a WAIT-FOR-TRIGGER list — most scan runs will have a mix
- Save the report to `reports/pre-pump-scan-YYYY-MM-DD-HHMM.md` before delivering so the next rescan can produce a delta
- End with the Phase 10 Action Summary block

**Never:**
- Render the full 9-phase per-token walk inside a scan (that's what the dedicated `Use ARIA. Analyze $X.` invocation is for)
- Recommend chasing a candidate already up >40% in last 1h (late, risk/reward broken)
- Rank by 24h change or raw pump.fun volume — those are backward-looking; score the *setup*, not the already-happened move
- Score higher than 75 without confluence across at least 4 of the 9 factors
- Skip the Action Summary because the scan is "just a scan" — the summary is where the user acts
