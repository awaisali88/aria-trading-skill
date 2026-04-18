# ARIA — Example Prompts

When the user asks for examples, usage help, or "what can you do", load this file and present the examples grouped by category. Tell them every prompt is copy-paste-ready and can be edited — the `[SYMBOL]` / `[MINT]` / `[AMOUNT]` placeholders should be replaced with real values.

Every example starts with `Use ARIA.` to guarantee skill activation on Claude Code.

---

## 1. Market Scanning — find something to trade

### 1a. Backward-looking (what's moving NOW)

```
Use ARIA. Scan the market and give me the top 3 buys for my balance today.
```
*Runs `clodds_market_index` + pumpfun trending/hot/gainers + `clodds_signals` + `web_search` → ranks top 3 with full ARIA scores. Shows what's already moving.*

```
Use ARIA. What are the hottest pump.fun plays right now?
```
*Pulls pump.fun trending / hot / new-hot / graduating / KOTH → ranks with risk flags.*

```
Use ARIA. Any signals today?
```
*`clodds_signals` + market index + `clodds_edge` + `web_search` → live opportunity list.*

### 1b. Forward-looking predictive scanner (what's ABOUT to move)

```
Use ARIA. Find me top 5 coins about to go up in the next hour.
```
*Loads `references/predictive-scan.md` → sweeps 100+ candidates → pre-filters by liquidity/freshness/safety → scores each on the 9-factor Pre-Pump Signal (technical setup, multi-TF confluence, volume acceleration, whale net buy, divergence, narrative ignition, social acceleration, holder health, macro) → ranks top N → emits condensed per-token block with 1h + 2-4h targets + entry + SL + size → Phase 10 Action Summary.*

```
Use ARIA. Pre-pump scan — top 10 setups.
```
*Same pipeline, N=10 instead of default 5.*

```
Use ARIA. What's about to pump?
```
*Same pipeline, default N=5.*

```
Use ARIA. Pre-pump top 3 on Solana only.
```
*Restrict candidate universe to Solana DEX (pump.fun + GeckoTerminal trending pools). Skip CEX surface.*

```
Use ARIA. Pre-pump top 5 on Binance only.
```
*Restrict candidate universe to Binance-listed tokens (clodds_signals + Binance 24hr ticker top gainers/losers). Skip Solana DEX surface.*

```
Use ARIA. Rescan.
```
*Re-runs the predictive scanner and compares to the most recent prior scan file in `reports/pre-pump-scan-*.md`. Renders a **Δ block** at the top showing which tokens entered/left the top N, which are gaining/losing score, and which are stable. Recommended cadence: every 20-30 minutes during active hours.*

---

## 2. Single-Token Analysis — full 9-phase protocol

```
Use ARIA. Analyze $SOL.
```
*Complete 9-phase protocol → scorecard + ARIA Signal Block.*

```
Use ARIA. Is $XRP a buy right now?
```
*Full protocol → explicit YES / NO / WAIT verdict.*

```
Use ARIA. Full analysis on $TAO — I'm thinking of getting in.
```
*Full protocol with portfolio-aware trade plan.*

```
Use ARIA. Analyze Solana mint [MINT_ADDRESS] — is it safe and worth buying?
```
*Security check first (mint authority, LP, top holders) → if clean, full protocol.*

```
Use ARIA. Check the security of pump.fun token [MINT].
```
*`clodds_token_security` + pump.fun token profile + whale tracking + solscan fetch → rug pass/fail.*

---

## 3. Multi-Timeframe Chart + Indicators (this is the default now — also works standalone)

```
Use ARIA. Full multi-TF chart read on $BTC with every indicator.
```
*Pulls 1m/5m/15m/1h/4h Binance klines → computes RSI, MACD, BB, VWAP, EMA(9/21/50), ATR, Stoch RSI, OBV, volume for each TF → confluence matrix → TradingView link.*

```
Use ARIA. Day-trade chart check on $ETH — am I armed to enter long?
```
*Runs the 6-point entry checklist: higher-TF bias / setup / trigger / momentum / volume / R:R.*

```
Use ARIA. Multi-TF on BONK using GeckoTerminal — is this trending or chopping?
```
*Routes Solana DEX token through GeckoTerminal OHLCV → full indicator suite.*

```
Use ARIA. Chart + indicator analysis on $SOL, $XRP, $TAO — which has the cleanest setup?
```
*Three-token side-by-side with confluence matrices → ranks by setup quality.*

```
Use ARIA. Swing-trade read on $BTC — I want to hold 2-4 weeks.
```
*Runs intraday TFs + extends to 1d / 3d / 1w for the swing bias.*

---

## 4. Portfolio — understand what you hold

```
Use ARIA. What's my portfolio?
```
*`clodds_portfolio_summary` + PnL + bags + risk → full report across all venues.*

```
Use ARIA. What's my PnL today?
```
*`clodds_portfolio_pnl` + bags → realized + unrealized breakdown.*

```
Use ARIA. Show me my open positions and their SL/TP distances.
```
*`clodds_portfolio_positions` + `clodds_monitoring` → dashboard with health flags.*

```
Use ARIA. Check my $SOL position — should I add, hold, or exit?
```
*Fresh re-analysis on SOL → compare vs entry → updated action plan.*

```
Use ARIA. What's my Binance USDT balance?
```
*`clodds_binance_spot_balance` → live balance.*

---

## 5. Trade Execution — build plan, wait for confirmation

```
Use ARIA. Buy 0.5 SOL worth of [MINT] on pump.fun.
```
*Balance check → quote → full trade plan (SL/TP1/TP2/TP3/trailing) → confirmation format → waits for "go".*

```
Use ARIA. Buy $50 of $XRP on Binance spot.
```
*`clodds_binance_spot_balance` → price check → confirmation format → on "go" → execute + wire automation.*

```
Use ARIA. Long $ETH with 5× leverage on Hyperliquid, size it conservatively.
```
*Balance + liq-price calc → plan with SL/TP/trailing → confirmation → on "go" → execute + wire Tier 1.*

```
Use ARIA. Short $DOGE on Bybit — I think this is topping.
```
*Short plan with liq price → confirmation → execute on "go".*

```
Use ARIA. Swap all my BONK back to SOL.
```
*`clodds_jupiter_quote` → show net receive + price impact → confirmation → execute.*

```
Use ARIA. Close my $XRP position.
```
*Show current PnL → confirm → close → deactivate automations → report realized PnL.*

```
Use ARIA. DCA $20/day into $BTC for 2 weeks.
```
*Confirm amount / frequency / ceiling → `clodds_dca` + `clodds_automation` → confirm schedule live.*

---

## 6. Alerts, Automation & Monitoring

```
Use ARIA. Check my alerts.
```
*`clodds_monitoring` + `clodds_alerts` + positions → re-analyze each fired alert → present decisions.*

```
Use ARIA. Set a price alert on $SOL at $100.
```
*Ask: Tier 1 (auto-execute) or Tier 2 (notify only)? → configure.*

```
Use ARIA. Monitor all my positions every few minutes.
```
*Activates `clodds_monitoring` loop + reports status dashboard.*

```
Use ARIA. Update the trailing stop on my $XRP trade to 5%.
```
*Pull current price → recalc → update `clodds_automation` → confirm.*

```
Use ARIA. Alert me if [MINT] volume spikes 2× or a whale sells more than $50k.
```
*Registers volume + whale alerts via `clodds_alerts`.*

---

## 7. Macro & Market Health

```
Use ARIA. Macro check — what's the market doing?
```
*Fear/greed + BTC + SOL + Polymarket/Kalshi macro events + opinion.trade → regime read.*

```
Use ARIA. What's BTC doing right now?
```
*Live Binance price + 1d/7d trend + ETF flow context.*

```
Use ARIA. Is memecoin season on or off?
```
*pump.fun metas + sector rotation + volume aggregates → hot / cooling / cold.*

---

## 8. Research & Deep-Dives

```
Use ARIA. Research Fartcoin — narrative, holders, who's behind it, why anyone would hold.
```
*`clodds_research` + `web_search` + `web_fetch` on project page and socials → comprehensive report.*

```
Use ARIA. What's the Alpenglow upgrade and is it priced into $SOL?
```
*`clodds_research` + `web_search` → catalyst deep-dive + price-impact read.*

```
Use ARIA. Compare $XRP vs $ADA — which is the better commodity-framework bet today?
```
*Side-by-side 9-phase protocol → ranked recommendation.*

---

## 9. Power-User / Testing Prompts

```
Use ARIA. I'm day trading today. Run the full analysis on SOL, XRP, and TAO on Binance with my ~$161 USDT — I want the complete multi-timeframe chart read (1m/5m/15m/1h/4h) with S/R + RSI + MACD + VWAP + EMA alignment on every timeframe, the confluence matrix, and ARMED/WAITING/INVALID verdicts. Then give me day-trade plans with 15m-anchored SLs, 15m/1h TP ladders, and ATR-based trailing stops. Do not execute — wait for "go".
```
*The stress-test prompt. Exercises every phase, the fallback protocol, and the full indicator suite.*

```
Use ARIA. Same analysis but assume every Clodds data source fails — show me the web_search / web_fetch fallbacks working.
```
*Explicit fallback-path test — forces the model to route via CoinGecko / DexScreener / Birdeye / Solscan / X.*

```
Use ARIA. Give me a 5-line summary instead of the full report on $SOL.
```
*Terse-mode — only the signal block, no phase bodies.*

---

## 10. Configuration & Meta

```
Use ARIA. What tools do you have access to?
```
*Load `references/tool-inventory.md` → summarize by category.*

```
Use ARIA. Show me examples.
```
*Load this file (`references/examples.md`) → present the categorized list.*

```
Use ARIA. What's your protocol?
```
*Load `references/aria-protocol.md` → summarize the 9 phases.*

```
Use ARIA. How do you compute the indicators?
```
*Load `references/indicators.md` → walk through formulas for any indicator the user names.*

```
Use ARIA. Re-grade my last report against the protocol — show me the checklist and tell me what was skipped.
```
*Load `references/report-checklist.md` and `references/aria-protocol.md`, read the most recent report in `reports/`, walk every checklist item, mark each ✅ or ❌, and produce a phase-by-phase grade with concrete fixes for any gaps.*

```
Use ARIA. Run the compliance checklist on this report: [path or paste content].
```
*Same as above but for an arbitrary report — useful for grading historical analyses or comparing two reports.*

```
Use ARIA. Action summary only — what should I buy, sell, or hold right now based on my live balances?
```
*Skip the per-token 9-phase walk and go straight to Phase 10. Pulls live balances from every connected venue (Solana + Binance + Bybit + MEXC + Hyperliquid), cross-references existing positions in `clodds_bags`, and renders only the Action Summary block — BUY/HOLD/SELL/SKIP with venue + size + price targets + Top next-action. Best for quick daily check-ins.*

---

## 11. Journal & Performance Tracking

Every report ARIA produces auto-appends its recommendations to `reports/journal.jsonl` — a persistent log of every BUY/HOLD/SELL/SKIP signal with date, entry, SL, TP, and a link back to the source report. These three commands let you review, refresh, and analyze that history.

```
Use ARIA. Show journal.
```
*Read `reports/journal.jsonl`, render a Markdown table grouped by status (🟢 Open · ✅ Wins · ❌ Losses · ⏳ Expired), sorted newest-first. Date column is a clickable link to the source report — click to open the `.md` file. No external calls; just displays what's been logged.*

```
Use ARIA. Status check.
```
*For every open entry, pull current price (Binance klines for CEX, GeckoTerminal pool OHLCV for Solana), detect SL / TP1 / TP2 / TP3 hits, auto-update statuses, append a new status-check to each entry's history, and rewrite the journal. Renders a **time-series table** with columns per check date (Signal $ → Apr 17 → Apr 18 → … → Now → Δ → Status), so each row shows the price progression from signal creation through every prior check. 🔔 marks rows whose status changed this run. Up to 4 historical check columns; compresses to First + prior 2 + Now once >4 checks exist.*

```
Use ARIA. Status check — compact.
```
*Same command but renders the compact `Signal $ · Now · Δ · Status` format — useful when you have many open entries and want a narrow table.*

```
Use ARIA. Journal stats.
```
*Aggregated performance dashboard — overall win rate, average winner/loser %, expectancy per trade, best/worst pick, avg hold time, and breakdowns by venue (pump.fun / Raydium / Binance / etc.), signal type (BUY-NOW vs BUY-WATCH), and conviction (HIGH vs MEDIUM vs LOW — testing whether conviction correlates with outperformance). Also shows 30-day trend vs all-time.*

```
Use ARIA. Show my journal for pump.fun only.
```
*Filtered view — only entries where `venue` matches the filter. Works with any venue, ticker, signal type, or date range.*

```
Use ARIA. How did my last 5 picks do?
```
*Narrow status-check on the most recent 5 entries only — useful when you don't want to refresh 50+ entries at once.*

---

## Quick-reference — minimum viable prompts

For every session, these 12 one-liners cover 85% of use cases:

1. `Use ARIA. Scan the market.`
2. `Use ARIA. Find me top 5 about to go up next hour.` ← predictive scanner
3. `Use ARIA. Analyze $[TICKER].`
4. `Use ARIA. Multi-TF chart read on $[TICKER].`
5. `Use ARIA. What's my portfolio?`
6. `Use ARIA. Check my alerts.`
7. `Use ARIA. Buy $[AMOUNT] of $[TICKER] on [VENUE].`
8. `Use ARIA. Close my $[TICKER] position.`
9. `Use ARIA. Macro check.`
10. `Use ARIA. Show journal.` ← review past picks
11. `Use ARIA. Status check.` ← refresh performance on open picks
12. `Use ARIA. Show me examples.`

When in doubt, start with `Use ARIA.` + a plain-English description of what you want. The skill's Standard Invocations table in `SKILL.md` maps most phrasings to the right protocol path.
