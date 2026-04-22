# ARIA Report Template — Canonical Structure

**Load this file at the START of every market-analysis report (single-token or multi-token).** The structure below is the canonical layout for every crypto/memecoin analysis ARIA produces. Deviating from it without an explicit reason is a Phase-10 compliance failure.

The template was established from the reference report `reports/analysis-pub-2026-04-21-1530.md`. If you're ever unsure what a section should look like, open that file and match it.

---

## Absolute rules

1. **Section order is fixed.** Do not reorder.
2. **Memecoin-Profile exception:** Phase 5 runs AFTER Phase 1 and BEFORE Phase 2 (social is a leading indicator for memecoins). For Major Profile, phases run 1→2→3→4→5→6→7→8→9→10 in natural order.
3. **Every phase header uses the documented emoji** — they are the visual rhythm the reader scans by. Do not swap them out.
4. **Executive Verdict is mandatory** for every report, even one-liner `WAIT` calls. A reader should be able to get the entire thesis from the top block in <20 seconds.
5. **Disclaimer is mandatory** at the bottom. Non-negotiable.
6. **Tool-Call Audit is mandatory** — no "UNKNOWN" label anywhere in the report without a corresponding row in the audit table showing what was tried and why it failed.

---

## Canonical section order

```
1.  Title block                           (H1 + metadata lines)
2.  🚨 EXECUTIVE VERDICT                   (blockquote summary, top of report)
3.  🌍 Macro Context Snapshot              (mandatory table — F&G, BTC/ETH/SOL, dominance)
4.  🧠 PHASE 1 — Identity & Security       (identity table + rugcheck + verdict)
5.  📱 PHASE 5 — Social Deep-Dive          ← Memecoin Profile ONLY; placed here by ordering rule
6.  📊 PHASE 2 — Live Market Snapshot      (metrics + slippage table + verdict)
7.  📈 PHASE 3 — Chart Analysis             (Peak-Check for Memecoin, 5-TF for Major)
8.  🔗 PHASE 4 — On-Chain Intelligence     (holders, whale flow, creator, wash-trade)
9.  📱 PHASE 5 — Social & Sentiment         ← Major Profile ONLY; placed here by ordering rule
10. 🧭 PHASE 6 — Macro Context             (consolidated reference to §3)
11. 📋 PHASE 7 — Scorecard                 (weighted factor table + probability distribution)
12. 💼 PHASE 8 — Trade Plan                (or SUPPRESSED block if HARD STOP fired)
13. 🔔 PHASE 9 — Alerts & Automation
14. 📊 PHASE 10 — Action Summary           (wallet + 🟢🟡🔴⚪ action lists + deployment)
15. 🧾 Tool-Call Audit                     (every tool call, result, fallback)
16. 🎯 ARIA Signal Block                   (the ASCII box)
17. 📓 Journal note                        (auto-append state + override hint)
18. ⚖️ Disclaimer                          (mandatory closing)
```

---

## §1 — Title block

```markdown
# ARIA Analysis — $TICKER (Token Name) · [Memecoin Profile | Major Profile]

**Date:** YYYY-MM-DD HH:MM UTC
**Mint:** `<mint>`         ← Solana tokens only; CEX majors use **Symbol:** instead
**Source URL:** <primary link — pump.fun coin page / Binance trade page>
**Profile classification:** **Memecoin** (reason) OR **Major** (reason)
**Pipeline applied:** <one-line summary of which profile ordering + scorecard was run>
```

---

## §2 — 🚨 Executive Verdict

**Always a blockquote block**, right after the title. Covers:
- Signal (BUY / SELL / HOLD / WAIT / AVOID / HARD STOP)
- Conviction (LOW / MEDIUM / HIGH / near-certain)
- Composite score (X/100)
- **Two to five sentences of synthesis** — the thesis in plain language
- Any HARD STOP triggers fired (reference the playbook § that triggered)
- Phase-suppression state (e.g. "Phase 8 SUPPRESSED")

Example:
```markdown
## 🚨 EXECUTIVE VERDICT — HARD STOP · DO NOT TOUCH

> **Signal: AVOID — SELL-IMMEDIATELY IF HELD**
> **Conviction: HIGH (near-certain)**
> **Composite score: 14/100**
>
> <2-5 sentence thesis explaining what happened and why it matters>
>
> Red flags firing:
> - **§7.1 — <rule name>**: <how it fires>
> - **§7.7 — <rule name>**: <how it fires>
>
> Per skill rules, **Phase 8 trade plan is SUPPRESSED**.
```

For a positive verdict (BUY/ENTER), the same structure applies — state conviction, score, thesis, and any risk-downgrades.

---

## §3 — 🌍 Macro Context Snapshot

**Mandatory table**, appears once near the top of every report. Minimum rows:

| Metric | Value | Read |
|---|---|---|
| Fear & Greed Index | N · <regime> | <what this means for risk>
| BTC | $price · 24h Δ · **7d Δ** | <trend read> |
| ETH | $price · 24h Δ · **7d Δ** | <trend read> |
| SOL | $price · 24h Δ · **7d Δ** | <trend read> |
| Total Crypto MCap | $X T · 24h Δ | <trend read> |
| BTC Dominance | X% | <alt/meme rotation read> |
| Polymarket crypto markets | <hit summary or "no hit"> | <derivative signal read> |
| ETF flows | <value or "not polled — reason"> | — |

Close with a **Macro verdict** line: `**Macro verdict:** <one sentence about whether this tape helps or hurts the subject token>`.

---

## §4 — 🧠 Phase 1: Identity & Security Check

Two tables:

### Identity fields (table 1)
| Field | Value |

Rows include: Token name · Ticker · Mint · Chain · Pool address · Pool created · Deployer wallet · Claimed X link (with fraud flag if fake attribution) · Website status · Discord · Telegram · Narrative as claimed.

### Security signals (table 2, via rugcheck/gopluslabs/honeypot.is)
| Check | Result | Read |

Rows: Risk score · Mint authority · Freeze authority · LP lock · LP providers count · Top holder (bonding curve vs real) · Top external holders · Rugged flag.

Close with a **Phase 1 verdict** line — PASS / FAIL / HARD STOP with one-line reasoning.

---

## §5 — 📱 Phase 5: Social Deep-Dive (Memecoin only, runs BEFORE Phase 2)

**Mandatory 15-line table** per `pumpfun-social-playbook.md`. Tag every field with its sourcing tier where ambiguous (e.g. `via TwitterAPI.io`, `via nitter.net`, `via Perplexity`).

Lines to include (see playbook for exact content):
1. Creator X handle (claimed)
2. Creator X handle (verified — via §1.4 X-STATUS handler if applicable)
3. Creator X profile metrics (followers, age, bio, post count)
4. Status tweet content + engagement (if X link was a status URL)
5. Attribution verdict (fraud / genuine / unverifiable)
6. Post velocity 1h
7. Post velocity 24h
8. KOL mentions by tier (🐋 / 🦈 / 🐠 / 🦐)
9. Competing / disputed endorsements
10. Competing "OG" mint (if narrative is split)
11. Telegram status
12. Discord status (verified via public API)
13. Website status (live / dead / archived)
14. Shill / bot-detection score with specific evidence
15. Social-signal classification (VIRAL / ACTIVE / MODERATE / DEAD / MANIPULATED)

Close with a **Sources block** (clickable URLs) and the **🚨 SOCIAL RED FLAG BANNER** if any §7 rule fires.

---

## §6 — 📊 Phase 2: Live Market Snapshot

Full metrics table + slippage table. See `aria-protocol.md § PHASE 2` for exact mandatory fields (5m/1h/6h/24h/7d price deltas; volumes per window; buy/sell tx split; MCap/FDV/liquidity; bonding curve progress).

**Slippage table is mandatory** for any asset with liquidity <$10M. Sizes: $50 · $100 · $500 · $1,000 · $5,000.

Close with a **Phase 2 verdict** line.

---

## §7 — 📈 Phase 3: Chart Analysis

**Memecoin Profile:** Peak-Check Mode (15m + 1h only, 4 questions, ≤6-line verdict, no RSI/MACD/BB).

**Major Profile:** Full 5-TF × 10-indicator block (1m · 5m · 15m · 1h · 4h, all indicators per `indicators.md`) + confluence matrix + ARMED/WAITING/INVALID verdict.

**Chart links block** (mandatory, both profiles): Birdeye · DexScreener · GeckoTerminal · TradingView (for majors) · pump.fun (for pump.fun tokens).

---

## §8 — 🔗 Phase 4: On-Chain Intelligence

Table with: Top-10 concentration · Bonding-curve contract holding · Creator wallet current holdings (MANDATORY — `link-resolution.md` dispatch before UNKNOWN) · Whale net direction 24h · LP lock · Wash-trading signature · Active-wallet trend · OBV divergence (Major only).

PARTIAL banner at top if 3+ items couldn't be filled.

---

## §9 — 📱 Phase 5 (Major Profile ONLY)

Standard 7-line sentiment block. For Memecoin Profile, this slot is empty because §5 already ran before Phase 2.

---

## §10 — 🧭 Phase 6: Macro Context (consolidated)

Short block — one line saying "Already rendered at top of this report" + 3–5 bullets pulling out the key macro reads specific to the subject token's asset class (e.g. BTC dominance for alts, SOL 7d for Solana memes, memecoin sector temperature).

Close with **Macro verdict for this token: POSITIVE / NEUTRAL / NEGATIVE / MIXED**.

---

## §11 — 📋 Phase 7: Scorecard

### Memecoin Profile — 8 factors, weighted /100
| # | Factor | Weight | Score | Signal | Rationale |

Factors (exact): 1 Social · 2 Creator · 3 Narrative · 4 On-chain · 5 Security · 6 Chart · 7 Liquidity · 8 Macro.
Weights: 25 · 15 · 15 · 15 · 10 · 10 · 5 · 5 = 100.

### Major Profile — 10 equal factors /100
(see `aria-protocol.md § PHASE 7`)

### Probability distribution (mandatory, both profiles)
| Outcome | Probability | Reasoning |
|---|---|---|
| ⬆ UP (+20% or more) | X% | <why> |
| ➡ SIDEWAYS (±20%) | X% | <why> |
| ⬇ DOWN (-20% or more) | X% | <why> |

Horizon: 24–72h for short-term signals, longer for explicit swing/position analyses.

---

## §12 — 💼 Phase 8: Trade Plan

**If Phase 1 HARD STOP or Phase 5 MANIPULATED fired** → render this block instead:
```
> **Per `<playbook §>` — trade plan is SUPPRESSED.**
> **<reason — red flags that fired>**.
```
Still render the wallet snapshot (real balance) and optionally a "what a trade plan WOULD look like" reference-only block.

**Otherwise** → full trade plan per `trade-execution.md`: Entry range, SL, TP1/TP2/TP3 + % of position at each, trailing stop, R/R, position size in native + USD, venue, `Mode: PAPER | LIVE | SIMULATED`.

---

## §13 — 🔔 Phase 9: Alerts & Automation

Per `event-system.md`. For SUPPRESSED trade plans, state "No alerts armed. No automation wired." + one-liner rationale. Optional tier-2 research-only alerts are allowed (clearly labeled `[Tier 2 — notify only]`).

---

## §14 — 📊 Phase 10: Action Summary

**Mandatory closing block.** Four sub-blocks in this order:

1. **Wallet snapshot (live)** — real balance calls from `aria_*_balance` tools
2. **Action lists** — one row per asset, grouped by priority:
   - 🟢 BUY / ADD
   - 🟡 HOLD
   - 🔴 SELL NOW
   - ⚪ SKIP / AVOID
3. **Deployment totals** — % of capital deployed, % reserve, aggregate SL risk, % recommended for this asset
4. **Top next-action (one-liner)** — the single sentence the user should act on from this report

---

## §15 — 🧾 Tool-Call Audit

Every tool call goes in this table. Columns: Tool / URL · Purpose · Result (✅/❌/⚠). For failures, cite the specific error code and the fallback path used.

Any `UNKNOWN` label anywhere in the report MUST have a corresponding audit row showing which tiers from `link-resolution.md §1` were attempted.

Close with a **Sources (research)** list of 5–10 clickable URLs for reader verification.

---

## §16 — 🎯 ARIA Signal Block

The ASCII box. Template in `SKILL.md`. For SUPPRESSED trade plans, use `—` in the ENTRY/SL/TP/TP2/TP3/TRAILING rows with `SUPPRESSED per §<rule>` note. Memecoin profiles suppress RSI/MACD/OBV rows (mark `n/a — suppressed`).

---

## §17 — 📓 Journal note

One short paragraph on whether the entry was auto-appended to `reports/journal.jsonl`. Note the composite-score threshold (50) — SKIPs below 50 are NOT auto-appended per `journal-system.md`. Include the override hint: `Use ARIA. Append <ticker> skip to journal.` for scores below threshold that the user wants tracked.

---

## §18 — ⚖️ Disclaimer

```markdown
## ⚖️ Disclaimer

This report is for research and educational purposes. Nothing here is financial advice. Crypto memecoins, especially fresh pump.fun bonding curves, are among the highest-loss-rate assets in existence — the base rate for total loss on a bonding curve that is still below 5% progress at 24+ hours old is well above 95%. The analysis above reflects on-chain + social data at the time of pull; state can change in minutes. Do your own verification before any action.
```

For Major-Profile (BTC/ETH/large-alt) reports, the second sentence can be softened to something like "All trading carries risk; leverage amplifies both direction and drawdown." The first, third, and fourth sentences are constant.

---

## Cross-references

- Full phase-by-phase content requirements: `aria-protocol.md`
- Per-section compliance checks: `report-checklist.md`
- Memecoin Phase 5 operational procedures: `pumpfun-social-playbook.md`
- Link-resolution fallbacks before rendering UNKNOWN: `link-resolution.md`
- Journal auto-append rules: `journal-system.md`
- Signal block format + trade plan rules: `trade-execution.md`
