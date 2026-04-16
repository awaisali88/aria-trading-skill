# ARIA Event System — Two-Tier Clodds Automation

No TradingView. No external webhooks. Everything runs inside Clodds.

---

## ARCHITECTURE

```
TIER 1 — CLODDS AUTO-EXECUTES (no Claude needed, runs 24/7)
  Configured via: clodds_automation + clodds_triggers
  Fires:  Stop loss · TP1 sell · Trailing stop · DCA schedule
  Notifies: Slack after execution

TIER 2 — NOTIFY + ARIA DECIDES (requires Claude session)
  Configured via: clodds_alerts
  Fires:  TP2/TP3 · Volume spikes · Whale moves · New signals
  Flow:   Alert → Slack notification → open Claude → "check my alerts" → ARIA analyzes → Awais confirms → executes
```

---

## TIER 1 AUTOMATION RULES (configure at every trade entry)

```
SL_AUTO:
  trigger: price ≤ SL_PRICE
  action:  close 100% of position
  notify:  "🔴 SL hit on $TOKEN at $PRICE. Closed. Loss: $AMOUNT."

TP1_AUTO:
  trigger: price ≥ TP1_PRICE
  action:  sell 30% of position, move SL rule to ENTRY_PRICE (breakeven)
  notify:  "🟡 TP1 hit on $TOKEN. 30% sold. SL moved to breakeven."

TRAILING_AUTO:
  trigger: price drops TRAIL_PCT% below current peak
  activate_when: price reaches TRAIL_ACTIVATE_PRICE (entry + X%)
  action:  close remaining position
  notify:  "🟡 Trailing stop hit on $TOKEN. Remainder sold at $PRICE."

DCA_AUTO (if DCA active):
  trigger: on schedule (hourly/daily/weekly)
  action:  buy DCA_AMOUNT of TOKEN
  notify:  "✅ DCA executed: bought X SOL of $TOKEN at $PRICE."
```

---

## TIER 2 ALERT RULES (configure at every trade entry)

```
TP2_ALERT:
  trigger: price ≥ TP2_PRICE
  notify:  "📊 TP2 hit on $TOKEN at $PRICE. Open Claude: 'check my alerts'."

VOL_ALERT:
  trigger: 1h volume exceeds VOL_THRESHOLD
  notify:  "📊 Volume spike on $TOKEN. Open Claude for breakout check."

WHALE_ALERT:
  trigger: wallet >SIZE moves into/out of token
  notify:  "🐋 Whale move on $TOKEN. Open Claude to verify thesis."

SIGNAL_ALERT:
  trigger: price crosses key support or resistance level
  notify:  "📊 Price crossed $LEVEL on $TOKEN. Open Claude for updated signal."

NEW_OPP (for watchlist tokens, no position):
  trigger: token meets ARIA entry criteria (score >65 + volume surge)
  notify:  "🟢 Entry signal: $TOKEN at $PRICE. Open Claude for full analysis."
```

---

## WHEN "CHECK MY ALERTS" IS TRIGGERED

**Step 1 — Pull all state:**
```
clodds_monitoring        → all alerts fired since last session
clodds_alerts            → current alert states
clodds_portfolio_positions → all open positions and status
clodds_portfolio_pnl     → current P&L per position
```

**Step 2 — Categorize each fired alert:**
- NEW signal (no position) → run full ARIA Protocol → recommend entry
- TP2/TP3 on existing position → re-analyze → recommend partial exit or hold
- VOLUME/WHALE on existing → re-analyze full thesis → recommend add/hold/exit
- SL APPROACHING (within 10%) → warn immediately 🚨

**Step 3 — For each existing position with fired alert:**
```
clodds_pumpfun stats <mint>  → fresh price/volume
clodds_whale_tracking        → new whale activity?
clodds_metrics               → on-chain health changed?
clodds_divergence            → price vs on-chain diverging?
web_search "$TOKEN news"     → new catalyst or negative news?
clodds_signals               → new platform signal?
clodds_opinion "TOKEN — hold, add, or exit?"
```

**Step 4 — Output position review:**
```
┌──────────────────────────────────────────────────────────────┐
│      ALERT TRIGGERED — $TOKEN POSITION REVIEW               │
│      Alert: [TP2 / Volume / Whale / Signal]                 │
├──────────────────────────────────────────────────────────────┤
│  Alert fired at:     $X.XX                                  │
│  Current price:      $X.XX                                  │
│  Entry price:        $X.XX                                  │
│  Current P&L:        +/- X% ($X.XX)                        │
│  Position value:     $X.XX SOL / $X,XXX                    │
├──────────────────────────────────────────────────────────────┤
│  UPDATED SCORE:      XX/100  (was XX at entry)              │
│  BIAS CHANGED:       No / Yes → now [Bullish/Bearish]       │
│  THESIS VALID:       Yes / No — [reason]                    │
├──────────────────────────────────────────────────────────────┤
│  DECISION:           HOLD / SELL PARTIAL / CLOSE ALL / ADD  │
│  REASONING:          [1–2 line reason]                      │
│  ACTION:             [what and how much]                    │
│  NEW SL:             $X.XX                                  │
│  NEW TRAIL:          $X.XX (updated trailing level)         │
└──────────────────────────────────────────────────────────────┘

Say "make the trade" / "execute" / "yes" to act.
Say "hold" to keep current position.
```

---

## POSITION HEALTH DASHBOARD (for "monitor my positions")

```
┌────────────────────────────────────────────────────────────────────┐
│                 OPEN POSITIONS HEALTH CHECK                        │
│                 [DATE] [TIME]                                      │
├──────────┬────────┬────────┬─────────┬─────────┬──────────────────┤
│ Token    │ Entry  │ Now    │  P&L    │ SL      │ Status           │
├──────────┼────────┼────────┼─────────┼─────────┼──────────────────┤
│ $TOKEN1  │ $X.XX  │ $X.XX  │ +XX%   │ $X.XX   │ ✅ Healthy       │
│ $TOKEN2  │ $X.XX  │ $X.XX  │ -X%    │ $X.XX   │ ⚠️  Near SL (-8%)│
│ $TOKEN3  │ $X.XX  │ $X.XX  │ +XX%   │ $X.XX   │ 🟡 TP1 soon     │
└──────────┴────────┴────────┴─────────┴─────────┴──────────────────┘
```

---

## CLOSING A POSITION — AUTOMATION CLEANUP

When any position is fully closed (SL hit, manual exit, TP3):
1. Confirm close executed and price
2. Deactivate ALL automations for that token: `clodds_automation` + `clodds_alerts` + `clodds_monitoring`
3. Report realized P&L
4. Update portfolio state
