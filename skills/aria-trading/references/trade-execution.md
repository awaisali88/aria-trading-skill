# ARIA Trade Execution — Rules & Formats

Load this file for the complete trade plan format, position sizing, SL/TP/trailing rules, and confirmation templates.

---

## MANDATORY TRADE SEQUENCE

Every trade follows this exact sequence — no shortcuts:

1. **Check balance** on the target venue
2. **Calculate position size** from available capital
3. **Build full trade plan** — entry, SL, TP1/TP2/TP3, trailing stop
4. **Show confirmation format** — WAIT for explicit approval
5. **Execute** on "make the trade" / "execute" / "go" / "yes"
6. **Wire automation** — ALL Tier 1 + Tier 2 rules immediately after fill
7. **Report fill** — price, fees, balance, automation status

---

## STEP 1: BALANCE CHECK BY VENUE

```
Solana:      clodds_solana_balance + clodds_pumpfun_balance
Binance:     clodds_binance_spot_balance
Bybit:       clodds_bybit_spot_balance
MEXC:        clodds_mexc_spot_balance
Hyperliquid: clodds_hyperliquid_balance
All:         clodds_portfolio_summary + clodds_bags + clodds_risk
```

Output:
```
Available balance on [venue]:  X.XX SOL / $X,XXX
Already deployed:              X.XX SOL / $X,XXX (in open positions)
Free capital:                  X.XX SOL / $X,XXX
```

---

## STEP 2: POSITION SIZING RULES

| Market cap | Standard max | High conviction max |
|-----------|-------------|---------------------|
| <$100K micro | 1–2% of free capital | 3% |
| $100K–$1M | 2–5% | 7% |
| $1M–$10M | 5–10% | 15% |
| $10M–$100M | 10–20% | 25% |
| >$100M | 20–35% | 40% |

**Reductions:**
- -50% if: LP not locked · creator not doxxed · top-10 wallets hold >40% supply
- -30% if: liquidity <$50K · slippage >5% on target size

**Hard limits:**
- Max 20% in any single memecoin
- Max 40% total memecoin exposure across portfolio

**If Awais specifies an amount:** use it, but still check it's available, recalculate SL/TP from that amount, show full plan.

---

## STEP 3: SL / TP / TRAILING RULES

**Stop loss:**
- Default (memecoins): 15–25% below entry
- Near resistance entry: 8–12% (tight — failed breakout risk)
- Thin liquidity, multi-day thesis: 30%
- Futures: SL = liquidation price minus 20% buffer

**Trailing stop:**
- Activate when position is +30% from entry
- Trail 20% below the current price peak
- When TP1 hits: move SL to breakeven, activate trailing on remaining position
- On Solana DEXs: use `clodds_alerts` + `clodds_automation` (no native trailing order support)

**Profit ladder:**
- TP1 (30% of position): first resistance or +40–60% — sell 30%
- TP2 (40% of position): second resistance or +100–150% — sell 40%
- TP3 (final 30%): moon bag — hold until thesis breaks or trailing stop hits

---

## STEP 4: TRADE CONFIRMATION FORMAT

Always use this exact format. Never execute before showing it.

```
⚡ TRADE PLAN — waiting for your go-ahead

  Action:         BUY / SELL / LONG / SHORT / CLOSE
  Asset:          $[TICKER or SYMBOL]
  Venue:          [Binance / Bybit / MEXC / Hyperliquid / pump.fun / Jupiter]
  ────────────────────────────────────────────────────
  Available:      X.XX SOL / $X,XXX on [venue]
  Trade size:     X.XX SOL / $X,XXX  (X% of available capital)
  Entry price:    ~$X.XX  (live quote)
  Slippage:       ~X%
  Fees:           ~$X.XX
  ────────────────────────────────────────────────────
  STOP LOSS:      $X.XX  (-X%)  → max loss: $X.XX on this trade
  TAKE PROFIT 1:  $X.XX  (+X%) → sell 30%
  TAKE PROFIT 2:  $X.XX  (+X%) → sell 40%
  TAKE PROFIT 3:  $X.XX  (+X%) → trail final 30%
  TRAILING STOP:  Trail XX% below peak — activates at +XX% from entry
  ────────────────────────────────────────────────────
  Risk/Reward:    1:X
  Portfolio risk: X% of total at risk if SL hits
  Liq. price:     $X.XX  (futures only)
  ────────────────────────────────────────────────────

Say "make the trade" / "execute" / "go" / "yes" to confirm.
Say "no" / "cancel" or adjust any parameter to change the plan.
```

---

## STEP 6: POST-EXECUTION AUTOMATION (run immediately after fill)

```
# TIER 1 — Clodds auto-executes (SL / TP1 / trailing)
clodds_automation → SL rule: price ≤ SL_PRICE → close 100%, notify Slack
clodds_automation → TP1 rule: price ≥ TP1_PRICE → sell 30%, move SL to breakeven, notify
clodds_automation → trailing: trail TRAIL_PCT% below peak, activate at TRAIL_ACTIVATE_PRICE

# TIER 2 — Notify for re-analysis (TP2 / volume / whale)
clodds_alerts → TP2: price ≥ TP2_PRICE → notify Slack
clodds_alerts → volume: 1h vol > THRESHOLD → notify Slack
clodds_alerts → whale: large wallet move detected → notify Slack

# Monitoring
clodds_monitoring → activate continuous health check, interval 30min
```

---

## STEP 7: FILL REPORT FORMAT

```
✅ TRADE EXECUTED

  Filled:           X.XX SOL / X tokens at $X.XX
  Fees paid:        $X.XX
  ──────────────────────────────────────────────
  TIER 1 ACTIVE:
    SL auto-close:  $X.XX  (-X%)  ✅
    TP1 auto-sell:  $X.XX  (+X%)  ✅
    Trailing stop:  XX% below peak, activates at $X.XX  ✅
  TIER 2 ALERTS:
    TP2 alert:      $X.XX  ✅
    Volume alert:   $X,XXX threshold  ✅
    Monitoring:     Active, every 30min  ✅
  ──────────────────────────────────────────────
  Remaining balance: X.XX SOL / $X,XXX on [venue]
  Open position:     X.XX SOL / $X,XXX at avg entry $X.XX
```

---

## CLOSING A POSITION

When closing any position (manual, SL, TP3):
1. Check balance: `clodds_bags` → show current holdings + unrealized P&L
2. Get quote for exit size
3. Show: current P&L + expected net receive + slippage
4. Wait for confirmation
5. Execute close
6. Deactivate ALL automations: `clodds_automation` (cancel SL/TP/trailing) + `clodds_alerts` (cancel all alerts) + `clodds_monitoring` (deactivate)
7. Report: realized P&L + updated portfolio balance

---

## FUTURES SPECIFIC RULES

Additional fields for futures/perps trades:
- Always show liquidation price in confirmation format
- Always set leverage explicitly via `clodds_*_futures_leverage` before opening
- SL should be at least 20% above the liquidation price as buffer
- Recommended leverage for memecoins/high-vol: max 3–5x
- Recommended leverage for BTC/ETH: max 10x
- Never use maximum available leverage
