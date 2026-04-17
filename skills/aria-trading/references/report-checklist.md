# ARIA Report Compliance Checklist

Run this checklist BEFORE delivering any analysis to the user. Tick every item. If ANY item is unchecked, either fix the gap (re-run the missing pull) or explicitly footnote it as `† skipped because [reason]` in the report itself.

This file is the structural fix for multi-token reports — it stops the model from triaging sections out under length pressure. Walk it every time, regardless of how big the report is.

---

## Per-token (multiply by number of tokens analyzed)

### Phase 1 — Identity & Security
- [ ] Mint, creator wallet, launch date, narrative, links
- [ ] Mint authority status (revoked / not revoked / unknown)
- [ ] LP lock status (locked / unlocked / X days remaining)
- [ ] Top 5 (or 10) holder concentration %
- [ ] Fallback chain run if `clodds_token_security` failed (rugcheck.xyz, geckoterminal token info, solscan holders, pumpfun token)
- [ ] STOP banner rendered if any rug flag fires (mint not revoked / LP unlocked / top5 >50% / insider cluster / honeypot)
- [ ] PRESUMED-RISK banner if 3+ fallbacks failed AND token <7 days old

### Phase 2 — Live Market Snapshot
- [ ] Price changes: 5m / 1h / 6h / 24h / 7d (all five intervals)
- [ ] MCap, FDV, liquidity (USD), vol/mcap ratio
- [ ] 1h volume + 24h volume (separate)
- [ ] Buy/sell tx split for last 1h (from GeckoTerminal trades endpoint)
- [ ] Bonding/graduation status
- [ ] Slippage table at $50 / $100 / $500 / $1000 / $5000 (mandatory if liquidity <$10M)

### Phase 3 — Multi-TF Chart + Indicators
- [ ] 1m block — COMPUTED from raw OHLCV (not derived)
- [ ] 5m block — COMPUTED from raw OHLCV (not derived)
- [ ] 15m block — COMPUTED
- [ ] 1h block — COMPUTED
- [ ] 4h block — COMPUTED
- [ ] All 10 indicators per TF: RSI, MACD, BB, VWAP, EMA9/21/50, ATR, StochRSI, OBV, Vol ratio
- [ ] 3R + 3S levels per TF
- [ ] Confluence matrix (5-row table)
- [ ] Chart links block (TradingView / Birdeye / DexScreener / GeckoTerminal)
- [ ] Verdict: ARMED / WAITING / INVALID with one-line reason
- [ ] Any TF with <30 candles labeled `INSUFFICIENT DATA — n bars` (not fabricated)

### Phase 4 — On-Chain Intelligence
- [ ] Top 10 holder concentration % (with >40% flag if applicable)
- [ ] Creator wallet status (% remaining + last sell date)
- [ ] Whale direction last 24h (net accumulation $ vs distribution $)
- [ ] LP lock confirmation
- [ ] Price-vs-OBV divergence read
- [ ] On-chain health (active wallets trend)
- [ ] PARTIAL banner if 3+ items couldn't be filled

### Phase 5 — Social & Sentiment
- [ ] Standardized 7-line sentiment block rendered
- [ ] X post count last 24h (estimated from web_search)
- [ ] KOL mentions enumerated by handle + follower count (or `None`)
- [ ] Catalyst pipeline (or `None`)
- [ ] Mainstream coverage (or `None`)
- [ ] Sources list with clickable URLs

### Phase 7 — ARIA Scorecard
- [ ] Full 10-factor table rendered (NOT just composite)
- [ ] Signal column populated for every factor
- [ ] Composite score 0–100
- [ ] Probability distribution: UP / DOWN / SIDEWAYS percentages
- [ ] Any neutral-by-default 5/10 score footnoted with reason

### Phase 8 — Trade Plan
- [ ] Real balance call result shown (NOT hypothetical)
- [ ] BALANCE UNKNOWN banner if balance tool errored
- [ ] Entry zone, SL, TP1/TP2/TP3, trailing stop activation
- [ ] Position size in actual units (SOL/USDT) AND % of available capital
- [ ] R:R ratio
- [ ] % of total portfolio at risk if SL hits
- [ ] Suppressed if Phase 1 STOP banner fired

### Phase 9 — Alerts
- [ ] Every command labeled `[Tier 1 — auto-execute]` or `[Tier 2 — notify only]`
- [ ] SL + TP1 + trailing as Tier 1
- [ ] TP2 + volume + whale as Tier 2

### ARIA Signal Block
- [ ] Rendered in the canonical box format with all rows populated

---

## Once per report (regardless of token count)

- [ ] Phase 6 macro block at top (fear/greed value, BTC+SOL 24h+7d, prediction markets, sector temp, macro verdict)
- [ ] Tool-call audit table (which tools succeeded / failed / were substituted) — at the bottom
- [ ] Final ranking + portfolio allocation table grounded in the real balance from Phase 8
- [ ] Disclaimer line at the end

---

## Failure mode

If 3+ items remain unchecked after the report is otherwise complete, the report is INCOMPLETE. Two options:
1. Re-run the missing pulls (preferred — most fallback URLs are listed in `aria-protocol.md` and `indicators.md`).
2. Surface the gaps to the user with this banner at the top of the delivered report:
   ```
   ⚠ Report incomplete — missing: [list checklist items]
   Run again with [specific fix, e.g. "Use ARIA. Re-run with full balance check"] to close.
   ```

Never silently drop a required section. Either include it, footnote why it's missing, or flag the report as incomplete.
