# ARIA — Example Prompts

When the user asks for examples, usage help, or "what can you do", load this file and present the examples grouped by category. Tell them every prompt is copy-paste-ready and can be edited — the `[SYMBOL]` / `[MINT]` / `[AMOUNT]` placeholders should be replaced with real values.

Every example starts with `Use ARIA.` to guarantee skill activation on Claude Code.

---

## 1. Market Scanning — find something to trade

```
Use ARIA. Scan the market and give me the top 3 buys for my balance today.
```
*Runs `clodds_market_index` + pumpfun trending/hot/gainers + `clodds_signals` + `web_search` → ranks top 3 with full ARIA scores.*

```
Use ARIA. What are the hottest pump.fun plays right now?
```
*Pulls pump.fun trending / hot / new-hot / graduating / KOTH → ranks with risk flags.*

```
Use ARIA. Any signals today?
```
*`clodds_signals` + market index + `clodds_edge` + `web_search` → live opportunity list.*

```
Use ARIA. Find me asymmetric setups — small cap, high R:R, explain the edge.
```
*Heavy `clodds_edge` + `clodds_opinion` + `web_search` — asymmetric-bet bias.*

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

---

## Quick-reference — minimum viable prompts

For every session, these 10 one-liners cover 80% of use cases:

1. `Use ARIA. Scan the market.`
2. `Use ARIA. Analyze $[TICKER].`
3. `Use ARIA. Is $[TICKER] a buy?`
4. `Use ARIA. Multi-TF chart read on $[TICKER].`
5. `Use ARIA. What's my portfolio?`
6. `Use ARIA. Check my alerts.`
7. `Use ARIA. Buy $[AMOUNT] of $[TICKER] on [VENUE].`
8. `Use ARIA. Close my $[TICKER] position.`
9. `Use ARIA. Macro check.`
10. `Use ARIA. Show me examples.`

When in doubt, start with `Use ARIA.` + a plain-English description of what you want. The skill's Standard Invocations table in `SKILL.md` maps most phrasings to the right protocol path.
