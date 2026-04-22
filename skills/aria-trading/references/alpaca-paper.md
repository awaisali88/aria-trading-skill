# ARIA Alpaca Paper-Trading Playbook

Load this file **only** when the resolved execution mode is `paper` (see SKILL.md → Execution mode rules). Contains: tool map, supported-asset allowlist, asset-class routing rule, market-hours gate, order-type rules, fill-report format, simulated-fill branch for unsupported assets.

The Alpaca MCP (`mcp__alpaca__*`) is **not bundled** with this plugin. It is expected to be connected at the user's Claude Code level. If `mcp__alpaca__*` tools are not available in the current session, abort the paper flow and tell the user to connect the Alpaca MCP first (point them at the official Alpaca MCP docs).

---

## Tool map

| Purpose | Tool |
|---|---|
| Account overview (cash, buying power, equity, portfolio value) | `mcp__alpaca__get_account_info` |
| All open positions | `mcp__alpaca__get_all_positions` |
| One specific position | `mcp__alpaca__get_open_position` |
| Market clock (is equity market open?) | `mcp__alpaca__get_clock` |
| Trading calendar (next open / close) | `mcp__alpaca__get_calendar` |
| Latest equity quote | `mcp__alpaca__get_stock_latest_quote` |
| Latest equity bar (price+vol last candle) | `mcp__alpaca__get_stock_latest_bar` |
| Equity OHLCV history (charting) | `mcp__alpaca__get_stock_bars` |
| Latest crypto quote | `mcp__alpaca__get_crypto_latest_quote` |
| Latest crypto bar | `mcp__alpaca__get_crypto_latest_bar` |
| Crypto OHLCV history | `mcp__alpaca__get_crypto_bars` |
| All assets (filter by class=crypto for allowlist refresh) | `mcp__alpaca__get_all_assets` |
| Place equity order | `mcp__alpaca__place_stock_order` |
| Place crypto order | `mcp__alpaca__place_crypto_order` |
| Cancel single order | `mcp__alpaca__cancel_order_by_id` |
| Cancel all open orders | `mcp__alpaca__cancel_all_orders` |
| List orders (open / closed / all) | `mcp__alpaca__get_orders` |
| Close one position | `mcp__alpaca__close_position` |
| Close every position (use with care, even in paper) | `mcp__alpaca__close_all_positions` |
| Portfolio history (equity curve) | `mcp__alpaca__get_portfolio_history` |

**Data preference:** for price/OHLCV on Alpaca-listed assets, prefer the Alpaca MCP over Binance/CoinGecko — it gives fills that match what the paper order will actually execute against. Only fall back to Binance/CoinGecko/web_fetch if Alpaca bars error.

---

## Supported-asset allowlist (crypto)

Alpaca's crypto universe is narrow. Current reliable allowlist (as of 2026-Q1):

```
BTC/USD · ETH/USD · SOL/USD · LINK/USD · AAVE/USD · AVAX/USD · BCH/USD
DOGE/USD · DOT/USD · LTC/USD · MKR/USD · SHIB/USD · UNI/USD · XRP/USD · YFI/USD
USDC/USD · USDT/USD
```

On first paper-crypto trade in a session, verify the current list by calling `mcp__alpaca__get_all_assets` with `asset_class=crypto` and caching the symbols in conversation context. If the asset the user asked for isn't in the returned list → route to the **simulated-fill branch** below, do NOT try to place a real Alpaca order that will fail.

**Equities:** any US-listed ticker works on Alpaca paper (NASDAQ / NYSE / AMEX). No pre-allowlist needed; Alpaca will reject unknowns and ARIA should surface the error.

---

## Asset-class routing

```
User asset                              → Route to
────────────────────────────────────────────────────────────
Equity ticker (AAPL, NVDA, TSLA, ...)   → place_stock_order
Alpaca-allowlisted crypto (BTC, ETH...) → place_crypto_order
pump.fun mint (ends "pump")             → SIMULATED-FILL BRANCH (no Alpaca call)
PumpSwap / Raydium / Orca token         → SIMULATED-FILL BRANCH
Unlisted alt not on Alpaca              → SIMULATED-FILL BRANCH
```

Fall-through logic:
1. Check the user's ticker against the equity symbol format (1–5 uppercase letters, optionally `.A`/`.B` share class).
2. If it looks like crypto (BTC / ETH / etc. or `XXX/USD` syntax), check the allowlist cache.
3. If it looks like a Solana mint address (32–44 base58 chars) or the user mentioned pump.fun / PumpSwap / Raydium / Jupiter → simulated branch.
4. Otherwise: attempt `get_all_assets` lookup once; if no match → simulated branch.

---

## Market-hours gate (equities only — crypto skips this)

Before placing any equity paper order, call `mcp__alpaca__get_clock`:

| Clock state | Action |
|---|---|
| Open | Place `market` order as planned. |
| Closed (pre-market / after-hours) | Ask the user: (a) queue as `time_in_force: "day"` for next open (filled at 9:30 ET bell), or (b) switch to a `limit` order with `extended_hours: true` (fills 4 AM – 8 PM ET). Wait for answer before placing. |
| Closed (weekend / holiday) | Offer: (a) queue as `time_in_force: "gtc"` to fill on next trading day, or (b) park as BUY-WATCH journal entry (no order sent) until Monday. Wait for answer. |

Crypto trading runs 24/7 on Alpaca — no clock check required.

---

## Order-type rules

### Equities — use `bracket` order class (native SL+TP+parent in one call)

```
mcp__alpaca__place_stock_order
  symbol:            "AAPL"
  qty / notional:    <from Phase 8 sizing>
  side:              "buy" | "sell"
  type:              "market" | "limit"
  time_in_force:     "day" | "gtc"
  order_class:       "bracket"
  stop_loss:         { stop_price: <SL> }
  take_profit:       { limit_price: <TP1> }
```

TP2/TP3 and trailing go as separate follow-up orders once the entry fills (Alpaca bracket is single-TP).

### Crypto — no bracket; place parent + separate SL + TP children

Alpaca crypto does not support `order_class: "bracket"`. Sequence:

1. Parent market/limit buy via `place_crypto_order`.
2. After fill confirms (poll `get_open_position` until `qty > 0`), place:
   - `mcp__alpaca__place_crypto_order` side=sell, type="stop_limit", `stop_price=<SL>`, `limit_price=<SL * 0.995>`, `qty=<position qty>`, time_in_force=gtc  ← stop-loss
   - `mcp__alpaca__place_crypto_order` side=sell, type="limit", `limit_price=<TP1>`, `qty=<30% of position>`, time_in_force=gtc  ← TP1
   - Repeat for TP2 (40%) and TP3 (30%) with their respective limit prices.
3. Record all child order IDs in the journal entry for later cancel-on-SL-hit.

### Trailing stop — native Alpaca order type (stocks and crypto)

```
mcp__alpaca__place_stock_order   # or place_crypto_order
  symbol:          <ticker>
  qty:             <trailing portion size>
  side:            "sell"
  type:            "trailing_stop"
  trail_percent:   18   # or use trail_price for absolute $ offset
  time_in_force:   "gtc"
```

Activation-price semantics: Alpaca activates the trail as soon as the order is placed. If the user wants "activate trailing only at +30%", ARIA must poll `get_open_position` (via `aria_monitoring` or an ARIA-side status check) and submit the trailing order only after the unrealized PnL crosses +30%.

---

## Fill-report format (paper trades)

```
✅ PAPER TRADE EXECUTED  (Mode: PAPER · Alpaca paper account)

  Symbol:           $[SYMBOL]
  Filled:           <qty> at $<fill_price>   (paper)
  Order ID:         <alpaca_order_uuid>
  Submitted:        <iso_timestamp>
  ──────────────────────────────────────────────
  Child orders wired:
    SL  (stop):     $X.XX  order_id=...  ✅
    TP1 (limit):    $X.XX  order_id=...  ✅
    TP2 (limit):    $X.XX  order_id=...  ✅
    TP3 (limit):    $X.XX  order_id=...  ✅   (or: trailing stop armed @ XX%)
  ──────────────────────────────────────────────
  Paper buying power remaining: $X,XXX.XX
  Paper equity after order:     $X,XXX.XX
```

Then auto-append a journal row with `mode: "paper"` and `alpaca_order_id` populated. See `references/journal-system.md` for the full schema.

---

## Simulated-fill branch (unsupported assets)

When the user asks to paper-trade an asset Alpaca cannot execute (pump.fun mint, PumpSwap / Raydium token, any alt not on Alpaca's list), ARIA does **not** fall back to live execution. Instead, it produces a *simulated fill* — a journal-only record that captures what a paper fill would have looked like, so the user can still track the idea's performance.

Sequence:

1. **Quote the asset** at the current on-chain mid price:
   - pump.fun bonding curve: `aria_pumpfun quote <mint> <amount> buy` (returns exact SOL-per-token at current curve state).
   - PumpSwap / Raydium / Orca / Jupiter: `aria_jupiter_quote` or `aria_solana_quote`.
   - GeckoTerminal pool mid as fallback: `web_fetch https://api.geckoterminal.com/api/v2/networks/solana/pools/<pool>/ohlcv/minute?aggregate=1&limit=1` → use close price.
2. **Simulate the fill** at that quote price (zero slippage assumption; flag in the output).
3. **Render the confirmation** with `Mode: SIMULATED` and an explicit banner: *"(simulated fill — asset not on Alpaca paper; tracked in journal only, no order placed)"*.
4. **WAIT for user go-ahead** — simulated trades still follow the "make the trade" gate to prevent accidental journal pollution.
5. **Append journal row** with `venue: "alpaca-simulated"`, `mode: "simulated"`, `alpaca_order_id: null`, `position_size_usd` based on the quote, full SL/TP levels from the trade plan.
6. **Status-check compatibility** — the existing Solana DEX price-fetch path in `journal-system.md` Status check handles these rows without changes.

No `mcp__alpaca__*` calls happen in this branch. No order is ever sent. The row participates in journal stats just like any paper row but is grouped separately under `mode: simulated`.

---

## Closing a paper position

Same 7-step flow as live close (`trade-execution.md` → CLOSING A POSITION) but:

- Balance check: `mcp__alpaca__get_open_position <symbol>` instead of `aria_bags`.
- Cancel outstanding SL/TP/trailing orders for this symbol via `mcp__alpaca__cancel_order_by_id` (iterate the child order IDs stored in the journal row).
- Close via `mcp__alpaca__close_position <symbol>` (liquidates full qty) OR `mcp__alpaca__close_position <symbol> <percentage>` for partial.
- Log realized P&L and update the journal row's state to `CLOSED_MANUAL` or `CLOSED_WON`/`CLOSED_LOST` depending on context.

---

## Safety notes

- Even in paper, `place_*` / `close_*` / `cancel_*` tools should NOT be allowlisted in `.claude/settings.local.json` — always prompt before orders go out, so the user confirms the route is Alpaca paper and not an accidentally-flipped-to-live MCP.
- Before placing any paper order, verify in `get_account_info` that the returned account's `id` or `account_number` matches the user's expected paper account. If Alpaca returns a live-account shape, abort and warn the user.
- Never dual-book: if a live execution path fires for the same symbol in the same conversation, pick one venue and stick to it — dual orders across live + paper venues are not a feature, they're a bug.
