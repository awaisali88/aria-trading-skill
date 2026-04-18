# ARIA Journal & Performance Tracker

Load this file when:
- The user asks to see their recommendation history (*"show journal"*, *"show my picks"*, *"show the log"*)
- The user asks for a performance/status check (*"status check"*, *"how are my picks doing"*, *"check all my recommendations"*)
- The user asks for aggregate stats (*"journal stats"*, *"how's my strategy performing"*, *"my ARIA hit rate"*)
- You finish rendering a Phase 10 Action Summary and need to auto-append new entries

The journal is a persistent, append-only record of every actionable recommendation ARIA has produced. It lets the user audit past signals against reality without re-reading every report.

---

## Storage: `reports/journal.jsonl`

**Format:** JSON Lines — one recommendation per line, UTF-8 encoded, newline-separated. Never rewritten in place; updates create new `status_history` entries appended inside the row, and row updates replace the entire line (read-modify-write-line).

**Location:** `reports/journal.jsonl` in the repo root. The `reports/` directory is already untracked by default (see `.gitignore`), so the journal stays on the user's machine unless they explicitly commit it.

**Created lazily:** If the file doesn't exist when auto-appending, create it. No pre-seeding required.

---

## Entry schema

Every journal line is a complete JSON object with these fields:

```json
{
  "id":                    "YYYY-MM-DD-<token>-<signal>-<seq>",
  "created_at":            "ISO-8601 timestamp",
  "token":                 "yn",
  "symbol":                "$YN",
  "mint":                  "<Solana mint address OR null if CEX>",
  "venue":                 "PumpSwap / Raydium / Binance / Bybit / MEXC / Hyperliquid / Jupiter",
  "chain":                 "solana / ethereum / base / bsc / arbitrum / cex",
  "signal":                "BUY-NOW | BUY-WATCH | HOLD | SELL-NOW | SKIP",
  "conviction":            "HIGH | MEDIUM | LOW",
  "horizon":               "intraday (1h-4h) | swing (1-7d) | position (>7d)",
  "entry_zone":            { "low": 0.068, "high": 0.072 },
  "trigger_condition":     "break above $0.080 with vol >= 1.5x avg",
  "sl":                    0.062,
  "tp1":                   0.080,
  "tp2":                   0.095,
  "tp3":                   0.120,
  "trailing_pct":          18,
  "trailing_activate_pct": 20,
  "position_size_pct":     5,
  "position_size_usd":     70,
  "position_size_sol":     0.5,
  "r_r":                   3.8,
  "composite_score":       55,
  "source_report":         "reports/pumpfun-graduated-top5-2026-04-17-v2.md",
  "status":                "OPEN",
  "price_at_signal":       0.0788,
  "status_history":        [
    {
      "checked_at":        "ISO-8601 timestamp",
      "price":             0.0788,
      "pct_change":        0.0,
      "status":            "OPEN",
      "notes":             "entry zone not touched yet"
    }
  ]
}
```

**ID format:** `YYYY-MM-DD-<token>-<signal-lowercase>-<seq>` where `<seq>` is a 3-digit zero-padded counter unique to that (date, token, signal) tuple. First entry of the day for $YN BUY-WATCH → `2026-04-18-yn-buy-watch-001`.

**Null handling:** Use `null` for any field not applicable (e.g. `mint: null` for CEX tokens, `trigger_condition: null` for BUY-NOW, `position_size_sol: null` for CEX trades).

### Signal values

| Signal | Meaning |
|---|---|
| `BUY-NOW` | Enter immediately, no trigger required |
| `BUY-WATCH` | Entry armed; wait for `trigger_condition` (breakout / pullback / retest) |
| `HOLD` | Existing position — target-based exit plan |
| `SELL-NOW` | Hard exit required (thesis broken, rug flag, etc.) |
| `SKIP` | Scanned but not recommended; logged for post-mortem |

### Status values (state machine)

```
                                  ┌──→ TP1_HIT  ──→ TP2_HIT  ──→ TP3_HIT
                                  │                                 │
OPEN  ──→ ENTRY_NOT_TRIGGERED     │                                 ↓
  │     (signal was BUY-WATCH)    │                             CLOSED_WON
  │       │                       │
  │       └──→ IN_POSITION   ─────┤
  │                  │            └──→ SL_HIT    ──→  CLOSED_LOST
  │                  │
  │                  └──→ CLOSED_MANUAL (user override)
  │
  └──→ EXPIRED  (7d for intraday, 14d swing, 30d position — no trigger, no SL, no TP hit)
```

Transitions are driven by `Status check` comparing current price to the entry zone / SL / TP levels.

---

## Auto-append behavior (integrated with Phase 10)

After ARIA renders any Phase 10 Action Summary block, it must **append one journal entry per actionable line** to `reports/journal.jsonl`. Specifically:

| Action Summary line | Journal entry signal |
|---|---|
| 🟢 `BUY NOW` | `BUY-NOW`, status `OPEN` |
| 🟢 `BUY` with trigger condition (e.g. "on dip $X-$Y") | `BUY-WATCH`, status `ENTRY_NOT_TRIGGERED` |
| 🟡 `HOLD` (user already holds) | `HOLD`, status `IN_POSITION` |
| 🔴 `SELL NOW` | `SELL-NOW`, status `CLOSED_MANUAL` on the prior OPEN entry (if any) |
| ⚪ `SKIP` | `SKIP`, status `EXPIRED` (logged for post-mortem only, no price-tracking) |

### Deduplication

Before appending, scan the last 48 hours of entries in `journal.jsonl`. If a row exists matching `(token + YYYYMMDD + signal)`, **update** the existing row (bump `created_at`, replace `entry_zone` / `sl` / `tp*` / `source_report`) instead of creating a duplicate. This prevents the journal from being flooded if the user re-runs the same scan multiple times in one day.

### When NOT to append

- SKIP entries are optional — append only if user explicitly asks or if composite_score ≥ 50 (skips below 50 are noise).
- Hypothetical or educational ARIA outputs (e.g. "what would I have done yesterday") do NOT auto-append.
- If the Phase 8 balance call returned empty AND no real trade is actionable, still append BUY-WATCH entries but mark `position_size_usd: null` and `position_size_sol: null`.

---

## Command 1 — `Show journal`

**Triggers:** `show journal`, `show my journal`, `show the log`, `show my recommendations history`, `show my picks`

**Behavior:** No external tool calls. Read `reports/journal.jsonl`, group by current `status`, sort by `created_at` desc, render a Markdown table.

**Grouping:**
- 🟢 **OPEN / ACTIVE** — statuses `OPEN`, `ENTRY_NOT_TRIGGERED`, `IN_POSITION`
- ✅ **WINS** — statuses `TP1_HIT`, `TP2_HIT`, `TP3_HIT`, `CLOSED_WON`
- ❌ **LOSSES** — statuses `SL_HIT`, `CLOSED_LOST`
- ⏳ **EXPIRED** — statuses `EXPIRED`, `CLOSED_MANUAL`

**Output format:**
```
═══════════════════════════════════════════════════════════════
📓 ARIA JOURNAL · <N> entries · last updated <YYYY-MM-DD HH:MM UTC>
═══════════════════════════════════════════════════════════════

🟢 OPEN / ACTIVE (N=<count>)
| Date       | Ticker | Venue    | Signal    | Entry        | SL     | TP1   | Status    | Source |
|------------|--------|----------|-----------|--------------|--------|-------|-----------|--------|
| [2026-04-17](reports/pumpfun-graduated-top5-2026-04-17-v2.md) | $YN | PumpSwap | BUY-WATCH | 0.068-0.072 | 0.062 | 0.080 | OPEN | ↗ |
...

✅ WINS (N=<count>)
(same columns, plus "Realized %" column)

❌ LOSSES (N=<count>)
(same columns, plus "Realized %" column)

⏳ EXPIRED (N=<count>)
(truncated — most recent 10 only; user can ask for more)

─────────────────────────────────────────────────────────────
Quick stats: Win rate <X>% · Avg winner +<X>% · Avg loser -<X>% · Open positions <N>

→ Run "Use ARIA. Status check." to refresh prices and update statuses.
→ Run "Use ARIA. Journal stats." for full aggregate metrics.
═══════════════════════════════════════════════════════════════
```

**Date column is a Markdown link** — `[YYYY-MM-DD](source_report path)` — so clicking in Claude Code opens the `.md` report file.

---

## Command 2 — `Status check`

**Triggers:** `status check`, `check all my picks`, `how are my picks doing`, `status check my recommendations`, `refresh my journal`

**Behavior:**
1. Read `reports/journal.jsonl`.
2. For every entry with status in `{OPEN, ENTRY_NOT_TRIGGERED, IN_POSITION}`:
   - Determine data source from `venue` / `chain`:
     - Solana DEX: `web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=5&limit=1` (latest 5m close) — look up pool via `web_fetch https://api.geckoterminal.com/api/v2/networks/solana/tokens/<mint>/pools` if pool not cached
     - Binance / Bybit / MEXC: `web_fetch https://api.binance.com/api/v3/ticker/price?symbol=<SYM>USDT` (current spot) — or `clodds_<venue>_spot_price` if connected
   - Compare current price to entry zone / SL / TP levels:
     - `price <= sl` → status `SL_HIT`, realized loss = (sl - price_at_signal) / price_at_signal
     - `price >= tp3` → status `TP3_HIT`, realized gain to TP3
     - `price >= tp2` (and not tp3) → status `TP2_HIT`
     - `price >= tp1` (and not tp2+) → status `TP1_HIT`
     - `entry_zone.low <= price <= entry_zone.high` → status `IN_POSITION` (entry triggered)
     - Neither SL nor TP nor entry zone touched → keep status, just log the check
   - Append new `status_history` entry: `{checked_at, price, pct_change, status, notes}`
   - Update entry's `status` field to new status if state changed.
3. Also check expiry — if `now - created_at > horizon-specific threshold` (7d intraday / 14d swing / 30d position) and no trigger/TP/SL hit → status `EXPIRED`.
4. Rewrite `reports/journal.jsonl` with the updated entries.
5. Render a **time-series table** — one row per entry, one column per historical check. This matches the user's mental model of a spreadsheet where each new check appears as a new column to the right of the same row.

### Time-series rendering format (MANDATORY for Status-check output)

For each entry, render a row with these columns in this order:

```
| Date      | Ticker | Venue     | Signal    | Entry       | SL    | TP1   | Signal $  | [MM-DD #1] | [MM-DD #2] | [MM-DD #3] | Now (YYYY-MM-DD HH:MM) | Δ from signal | Status   | 🔔 |
```

Column logic:
- **Signal $** — `price_at_signal` (the price when the recommendation was created)
- **Middle columns** — one column per historical `status_history` check, column header labeled with the check date (e.g. `Apr 18 14:20`). Show up to **4 most-recent historical checks** (excluding the one just added this run). If fewer than 4 checks exist, leave the empty columns blank. If more than 4 exist, show first-recorded check + last 3 (so the user sees the full progression without the row getting unreadably wide).
- **Now** — the price fetched in THIS Status-check run, with the timestamp in the header
- **Δ from signal** — `(Now - Signal $) / Signal $ × 100%`
- **Status** — current status (may have just transitioned this run)
- **🔔** — emoji column, filled only on rows where status changed THIS run

Example row (entry created 2026-04-17, checked on 04-17, 04-18, 04-19, and now 04-20):
```
| Date              | Ticker | Venue    | Signal     | Entry       | SL    | TP1   | Signal $ | Apr 17 18:30 | Apr 18 09:15 | Apr 19 11:40 | Now (2026-04-20 14:20) | Δ     | Status       | 🔔 |
| [2026-04-17](...) | $YN    | PumpSwap | BUY-WATCH  | 0.068-0.072 | 0.062 | 0.080 | $0.0788  | $0.0788      | $0.0812      | $0.0775      | $0.0831                | +5.5% | IN_POSITION  | 🔔 |
```

If this is the FIRST Status check on a set of entries (no prior history), render only: `Signal $ | Now | Δ | Status` — no middle check columns yet.

### Data-density guard

If any entry has >4 historical checks, compress to `First · 2nd-latest · Latest-1 · Now` (4 columns max). Never render more than 4 historical columns even if 20 checks exist — the journal file still contains the full history; the rendering is just the summary.

### Alternative compact mode

If the table is being rendered for a large number of entries (>15 rows), or the user explicitly asks for `Use ARIA. Status check — compact.`, fall back to the single-`Now` rendering:
- Columns: `Signal $ | Now | Δ | Status | 🔔`
- This is the original Command 1 "Show journal" format augmented with Δ and Status transitions

### R:R realized (optional trailing column on wins/losses only)

For entries in WINS / LOSSES groups, add a `R realized` column:
`(exit_price - entry_mid) / (entry_mid - sl)` where `exit_price` is the price at the state transition (TP hit or SL hit).

**Rate-limiting:** If >20 OPEN entries exist, batch fetches in parallel but space Binance/GeckoTerminal calls to avoid 429s. Tag any fetch failures with ⚠ and keep the previous price/status.

**Summary line below the table:**
```
Status check summary — checked <N> open entries at <timestamp>:
  🔔 <X> state transitions (TP1 hit: A · TP2 hit: B · SL hit: C · entry triggered: D · expired: E)
  📈 <N> still open · 📊 Mark-to-market: +<X>% unrealized / -<Y>% unrealized
  Win rate (all-time): <X>% · Avg R realized: <X>
```

---

## Command 3 — `Journal stats`

**Triggers:** `journal stats`, `how's my strategy performing`, `my ARIA hit rate`, `performance summary`

**Behavior:** Read the journal, compute aggregates, render a dashboard.

**Metrics to compute:**
- Total picks (ever)
- Win rate % (wins / (wins + losses), excluding expired)
- Avg R realized on winners (TP1 → 1R, TP2 → ~2R, TP3 → ~3R weighted)
- Avg % gain on winners
- Avg % loss on losers
- Expectancy = win_rate × avg_winner_% + loss_rate × avg_loser_%
- Best pick (by %), Worst pick (by %)
- Avg hold time (days/hours, winners vs losers separately)
- Per-venue breakdown (pump.fun / Raydium / Binance / etc. — N picks, win rate, expectancy)
- Per-signal-type breakdown (BUY-NOW vs BUY-WATCH hit rate)
- Per-conviction breakdown (HIGH / MEDIUM / LOW — do HIGH conviction picks actually outperform?)
- Recent trend — last-30-day win rate vs all-time

**Output format:**
```
═══════════════════════════════════════════════════════════════
📊 ARIA JOURNAL STATS · <total_picks> picks · <first_date> to <last_date>
═══════════════════════════════════════════════════════════════

Overall performance:
  Win rate:           <X>%  (<W> wins, <L> losses, <E> expired)
  Avg winner:         +<X>% / +<X>R
  Avg loser:          -<X>% / -<X>R
  Expectancy:         +<X>% per trade
  Best:               $<TICKER> +<X>% on <DATE>
  Worst:              $<TICKER> -<X>% on <DATE>
  Avg hold:           <X> days (winners <Xd>, losers <Xd>)

By venue:
  pump.fun:      <N> picks · <X>% WR · expectancy +<X>%
  Raydium:       <N> picks · <X>% WR · expectancy +<X>%
  Binance spot:  <N> picks · <X>% WR · expectancy +<X>%
  ...

By signal type:
  BUY-NOW:       <N> picks · <X>% WR · expectancy +<X>%
  BUY-WATCH:     <N> picks · <X>% WR · expectancy +<X>% (of those that triggered)
  HOLD→target:   <N> picks · <X>% WR

By conviction:
  HIGH:          <N> picks · <X>% WR  ← should be highest
  MEDIUM:        <N> picks · <X>% WR
  LOW:           <N> picks · <X>% WR  ← should be lowest

Recent trend:
  Last 30 days:  <X>% WR · <N> picks · <+/-X>% vs all-time
═══════════════════════════════════════════════════════════════
```

If the journal has fewer than 5 closed picks, render a compact version and note `(not enough data for meaningful stats — need 5+ closed positions)`.

---

## Phase 2 forward-compat note

The JSON-Lines format and schema above are chosen to be stable. When the user is ready to build a Railway web dashboard (a single-screen SPA with sortable columns, equity-curve chart, clickable links), the app reads this exact file — no migration, no schema change. ARIA can keep writing to the local file, and/or POST new entries to the app's API. Both can coexist.

When the user asks "give me the Railway app prompt", provide a complete build spec for:
- Next.js / HTMX frontend
- Express / FastAPI backend reading `journal.jsonl` (or a synced copy)
- Endpoints: `GET /entries`, `POST /entries`, `POST /status-check`, `GET /stats`
- Deployed on Railway with Postgres (optional — SQLite file also works)
