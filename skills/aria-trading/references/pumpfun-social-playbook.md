# Pump.fun Social Playbook — Operational Procedures

Load this file when running **Phase 5 Memecoin Expanded Block** for any pump.fun / PumpSwap / fresh Solana memecoin. It defines the *how* of social analysis; the *what-to-render* lives in `aria-protocol.md` Phase 5.

**Why this file exists:** for fresh memecoins, Twitter is the leading indicator of price. Charts trail social narrative by 5-15 minutes. The predictive edge is in correctly reading creator-account legitimacy, post-velocity acceleration, and KOL-tier mention patterns — not in another RSI(14) print.

---

## 1. Creator audit procedure

**Goal:** resolve the creator's X (Twitter) handle, verify the account is legitimate, and flag prior-rug history.

### Step 1.1 — Resolve creator handle

Try the sources in this order. On any HTTP 402/403/404/ECONNREFUSED at any step, dispatch via `references/link-resolution.md § §1` for the public-API alt-host / Perplexity fallback before advancing.

```
1. clodds_pumpfun token <mint>
   → The response's "Links:" block lists Twitter / Telegram / Website if the deployer set them.
   → If Twitter link is present: extract @handle from the URL.

2. pump.fun coin page:
   web_fetch https://pump.fun/coin/<mint>
   → Look for an X / Twitter icon in the header block. Extract @handle.

3. IF the extracted X link matches x.com/<handle>/status/<id> (status URL, not profile):
   → Jump to §1.4 X-STATUS URL HANDLER below. Extract @handle from path segment [0]
     and proceed with §1.2 on that handle. Do NOT mark as "status not account" —
     the handle IS in the URL path.

4. Solscan creator wallet (403-routed):
   Tier 1:  web_fetch https://solscan.io/account/<creator_wallet>
   On 403:  web_fetch https://public-api.solscan.io/account?account=<creator_wallet>
   On 403:  try Helius MCP if present (mcp__*helius*__*) or Perplexity per link-resolution.md

5. GMGN creator profile (403-routed):
   Tier 1:  web_fetch https://gmgn.ai/sol/address/<creator_wallet>
   On 403:  web_fetch https://api.gmgn.ai/api/v1/wallet/sol/<creator_wallet>  (alt host)
   On 403:  Perplexity per link-resolution.md § §1

6. Nitter mirror search (when direct X.com is blocked):
   Per link-resolution.md § §2 mirror rotation:
   web_fetch https://nitter.net/search?q=%24<TICKER>
   → Scan top 20 results for posts that reference the mint or ticker with creator-style
     framing ("I just deployed", "new launch", "CA:"). Extract @handle of author.

7. Perplexity (if mcp__*perplexity*__* is available):
   "Who deployed Solana token <mint> with ticker <TICKER>? Return the creator's
    X handle if publicly known. Cite the retrieval source."

8. Direct search:
   web_search "<mint first 8 chars> OR <ticker> site:x.com"
   web_search "<ticker> creator site:x.com"
```

If all 8 fail → the creator X handle is **UNKNOWN**. Render `Creator X: UNKNOWN (8 fallbacks attempted)` in the expanded block, list which tiers were tried in the Tool-call audit table, and cap Phase 7 Factor 2 (Creator/KOL audit) at 3/15.

### Step 1.2 — Verify handle legitimacy

Prefer the TwitterAPI.io MCP when present (paid tier, live JSON), otherwise fall through Nitter rotation:

```
Tier 1 (preferred): mcp__*twitterapi*__* — call get_user_profile / resource user://<handle>
                    → returns followers, following, post count, creation_at,
                      profile_image, description (bio), verified flag, location

Tier 2:             web_fetch https://nitter.net/<handle>  (rotate mirrors per
                    link-resolution.md §3 on 5xx)

Tier 3:             web_fetch https://x.com/<handle>  (often 402 but try once)

Tier 4:             Perplexity / web_search for "@<handle> twitter followers"
```

Extract in this order of fields:
- **Follower count**
- **Account creation date** (compute age in days)
- **Post count** (total)
- **Profile photo + header present?** (empty = red flag)
- **Bio content** (empty or crypto-generic spam = yellow flag)

**Age thresholds (account-legitimacy tier):**
- ≥ 2 years (doxxed dev) → high trust (+15 Factor 2)
- 90 days – 2 years → normal trust (+10 Factor 2)
- 30 – 90 days → fresh account (+5)
- < 30 days → very fresh, high-risk (0 Factor 2)
- Account age UNKNOWN (rate-limited) → proceed with caveat; cap at +8

### Step 1.3 — Prior-rug check

```
web_search "<handle> rug OR scam OR exit OR prior token site:x.com"
web_search "<handle> creator history"
```

Also check via GMGN:
```
web_fetch https://gmgn.ai/sol/address/<creator_wallet>
→ Look for the "Previous tokens" / "Token history" block.
```

**Classification:**
- **No prior tokens found** → neutral (no evidence)
- **1-2 prior tokens, still alive with holders** → green (serial legit dev)
- **1+ prior token with rug-pull signature** (creator dumped >50%, LP pulled, chart went to zero) → **HARD STOP** — zero Factor 2, flag in report, suppress Phase 8 trade plan per aria-protocol.md rules
- **Handle recently renamed** (e.g. via X handle-history check) → yellow flag, document the prior handle

### Step 1.4 — X-STATUS URL HANDLER

**Trigger:** the creator-link resolution in §1.1 produced a URL of shape
`x.com/<handle>/status/<status_id>` or `twitter.com/<handle>/status/<status_id>`.

**The mistake to avoid:** treating it as "a status link, not an account" and dropping the handle. The `<handle>` segment IS the creator's X handle — it just happens to be embedded in a deep-link to a specific tweet. The tweet itself is ALSO a high-signal artifact (launch announcement engagement = narrative-ignition proxy).

**Procedure:**

```
a) Parse the URL:
   - handle      = path segment [0]  (strip "@" if present)
   - status_id   = path segment [2]

b) Run §1.2 (handle legitimacy) on the EXTRACTED handle.
   This gives follower count, account age, bio, post count — same as if
   §1.1 had returned x.com/<handle> directly.

c) Fetch the status itself (rotate tiers on 402/403/5xx):

   Tier 1:  TwitterAPI.io MCP — if mcp__*twitterapi*__* is in session:
            Call get_tweet (or resource tweet://{status_id}) to pull
            full JSON: text, author, posted_at, likes, reposts, replies,
            quote count, bookmarks. Then get resource tweet://{status_id}/replies
            (top 20) for the reply-sentiment + shill-detection signal.
            This is the preferred path — paid SLA, live data, no scraping.

   Tier 2:  cdn.syndication.twimg.com/tweet-result?id=<status_id>&token=<deterministic_token>
            Public Twitter syndication API used by embedded-tweet widgets
            — free, no auth. Returns text + author + timestamp + basic
            engagement counts. Less complete than the MCP but always on.

   Tier 3:  Nitter mirror rotation — advance through link-resolution.md §3:
            nitter.net → nitter.privacydev.net → nitter.poast.org →
            nitter.kavin.rocks → nitter.1d4.us → nitter.unixfox.eu.
            On "empty body" from one mirror, try the next mirror ONCE,
            then advance tiers — empty body usually means this tweet
            isn't indexed here, not that the mirror is down.

   Tier 4:  web_fetch https://x.com/<handle>/status/<status_id>
            Often 402 but worth 1 try — some IPs / times succeed.

   Tier 5:  Perplexity (if mcp__*perplexity*__* present):
            "Return the full text, posted UTC date, likes, reposts, replies,
             and top-3 reply handles for <URL>. If behind paywall, use
             nitter/cache/archive mirrors."

   Tier 6:  web_search "\"<status_id>\" site:x.com OR site:nitter.net"
            Last resort — surfaces any quote-tweets or archives.

   Stop the chain at the first tier that returns usable data.
   Tag the rendered Phase 5 block with the sourcing tier — e.g.
   "(via TwitterAPI.io)", "(via syndication)", "(via nitter.net)",
   "(via Perplexity)" — so the reader sees the confidence floor.

d) Extract from the response:
   - tweet_text                       (full, not truncated)
   - posted_at                        (UTC timestamp)
   - likes, reposts, replies, bookmarks
   - mentioned @handles and $CASHTAGS
   - top-3 replies: {handle, text first 100 chars, reply sentiment}

e) Feed into Phase 5 scoring:
   - Engagement ≥ 1,000 likes AND ≥ 100 replies within 24h of posting
     → counts as §6.3 narrative-ignition signal (thread-velocity spike)
     → +5 to Factor 1 (Social, /25)
   - Reply sentiment >60% bullish (positive language, emojis like 🚀📈💎)
     → +3 to Factor 1
   - Reply sentiment >80% bot-spam (3+ copy-pasted replies, low-follower authors,
     default profile photos)
     → trigger §4 red-flag pattern #1/#4 → MANIPULATED classification
   - Any mentioned handle with >100K followers → tier-tag per §3 and count as KOL
     mention for Factor 1.

f) Render as one sub-line in the 15-line Phase 5 block:
   "Creator status tweet: <N likes · N replies · N reposts> — <sentiment read>"
   Example: "Creator status tweet: 47 likes · 3 replies · 2 reposts — dead
             (engagement <1% of expected for a real launch announcement)"

g) Audit table: record every tier attempted in the final Tool-call audit with
   the specific error code and URL used.
```

**Edge cases:**
- If the URL uses `/status/` but path segment [0] is `i` or `intent` (e.g. `x.com/intent/tweet`) — that's a compose intent, not a real tweet. Mark as `malformed creator link` and continue §1.1 at step 4.
- If the tweet is from a different handle than the one pump.fun lists as creator (common when devs RT a KOL to inflate legitimacy) — flag `status author ≠ claimed creator handle`, and treat the claimed handle with extra suspicion.
- If the status returns "Tweet not found / deleted" via nitter — that's a **red flag per §7 item #3** (creator deleted old tweets).

---

## 2. Post-velocity calculation

**Goal:** measure how fast chatter about $TICKER is accelerating on X. Velocity is the #1 leading indicator for pre-pump memecoins.

### Formula

```
velocity_1h   = posts_last_1h    / (posts_last_24h / 24)
velocity_24h  = posts_last_24h   / (posts_last_7d  / 7)
```

Both must be computed. 1h velocity is the immediate ignition signal; 24h velocity confirms it isn't a single-candle anomaly.

### Data collection

Prefer the TwitterAPI.io MCP when present — it returns timestamped result arrays that can be bucketed exactly, not approximated from search snippets:

```
Tier 1 (preferred): mcp__*twitterapi*__* search_tweets query="$<TICKER>" or "<token name>"
                    → returns array of tweets with exact posted_at timestamps
                    → bucket directly: count where posted_at ≥ now-1h, ≥ now-24h, ≥ now-7d
                    → no approximation, no "Xm ago" parsing

Tier 2:             web_search "$<TICKER> site:x.com"
                    → Count results that reference the token in the last 1h, 24h, 7d.
                    → Engines don't filter by hour directly — apply these heuristics:
                      - Results with "just now" / "Xm ago" → last-1h bucket
                      - Results with "Xh ago" where X ≤ 23 → last-24h bucket
                      - Results with "Xd ago" where X ≤ 7 → last-7d bucket
                    → If search volume is too small to bucket reliably, fall back to:
                      web_search "<token name> OR <ticker> solana crypto" and count results
                      dated this week.
```

### Thresholds (velocity tiers)

| Velocity (1h) | Velocity (24h) | Classification | Factor 1 Score (of /25) |
|--------------|----------------|----------------|------------------------|
| ≥ 3×         | ≥ 2×           | **VIRAL**      | 22-25                 |
| ≥ 3×         | < 2×           | single-hour spike, suspicious | 14-18 (check shill) |
| 1.5-3×       | ≥ 1.5×         | **ACTIVE**     | 16-21                 |
| 1-1.5×       | 1-1.5×         | **MODERATE**   | 8-15                  |
| < 1×         | < 1×           | **DEAD**       | 0-7                   |

### Floor cases

- **Token too new for 7d avg** (<7 days old) → substitute with `posts_since_launch / hours_since_launch` and compare 1h to that. Label `velocity based on since-launch baseline (N hours)`.
- **Search engine returns <5 results total** → token is in "dark phase", score as DEAD unless direct X search on `x.com/search?q=%24<TICKER>` returns more.

---

## 3. KOL tier classification

**Goal:** identify which size of X accounts are mentioning the token, because a 🐋 KOL retweet moves price very differently from a 🦐 shill mention.

### Tier thresholds

| Emoji | Tier | Follower count | Scoring impact |
|-------|------|----------------|----------------|
| 🐋 | Whale KOL | > 1,000,000 | Full Factor 2 bonus if mention in last 4h |
| 🦈 | Shark KOL | 100,000 – 1,000,000 | +5 to Factor 2 per mention (cap at 3 unique) |
| 🐠 | Mid-tier | 10,000 – 100,000 | +1 per mention (cap at 5); qualifies ACTIVE classification |
| 🦐 | Shrimp | < 10,000 | Count only; zero scoring impact but flag if >20 mentions (possible coordinated shrimp-shill) |

### Data collection

```
Tier 1 (preferred): mcp__*twitterapi*__* search_tweets query="$<TICKER>"
                    → For each returned tweet: extract author.handle + author.followers_count
                      directly from the result object (no per-handle re-fetch needed)
                    → Tier-tag immediately by follower bucket

Tier 2:             web_search "$<TICKER> site:x.com"
                    → For each result: extract the @handle of the poster.
                    → web_fetch https://x.com/<handle> OR nitter mirror to resolve
                      follower count for tier tagging.
```

### Known KOL quick-reference

Keep this list up-to-date as you see recurring accounts. When any of these surface in `$TICKER` search, immediately tier-tag them without needing to re-check follower count:

```
🐋 (>1M):  @elonmusk · @cz_binance · @VitalikButerin · @aeyakovenko · @SBF_FTX-NOT-USED · [your live list]
🦈 (100K-1M): @Ansem · @MacnBTC · @CryptoCred · @Pentoshi · @AltcoinGordon · [your live list]
🐠 (10K-100K): commonly pump.fun caller accounts — name ends in "calls", "degen", "moons", "kings", etc.
```

### Coordination red flag

If 3+ 🦐 accounts post identical copy within 60 seconds of each other, that is **not** organic acceleration — it is coordinated shrimp-shilling, often paid. Flag as **MANIPULATED** in Phase 5 and zero Factor 1.

---

## 4. Shill / bot detection heuristics

**Goal:** distinguish genuine narrative ignition from coordinated shill or bot-swarm activity.

### Red-flag patterns (any 2+ hits → MANIPULATED, zero Factor 1)

1. **Copy-paste tweet text** — three or more posts with identical or near-identical wording (same emoji, same formatting, same call-to-action)
2. **Coordinated timing** — 5+ posts within 60 seconds of each other from different accounts, not replies to a single thread
3. **Account-age cluster** — 3+ accounts posting about the token all created in the same week
4. **Zero profile photos** — default X egg / no header / blank bio on 3+ of the top posters
5. **Engagement ratio mismatch** — posts have 500+ likes but 0 replies or all replies are from the same 3 accounts
6. **Handle-change history** — original poster recently renamed their account (hide prior rug/scam)
7. **Crypto-ticker-spray account** — account bio contains 10+ tickers, meaning they shill anything that pays
8. **Paid-promo markers** — disclaimer text like "Ad", "AD", "#sponsored", "partnership" (some are legit, but weight as paid not organic)

### Scoring

- Zero red flags → shill = **LOW**
- 1 red flag → shill = **MED**, partial penalty on Factor 1 (subtract 5 pts)
- 2+ red flags → shill = **HIGH**, Factor 1 goes to 0 and token is flagged MANIPULATED

---

## 5. Telegram / Discord verification

**Goal:** if the token page lists a Telegram or Discord, verify it's real and measure activity.

### Telegram procedure

```
1. Fetch the TG invite URL from clodds_pumpfun token <mint> Links block.
2. web_fetch <tg_link>
   → If the page returns "tg://... deep link" or the bare invite preview, extract:
     - Group name
     - Member count (usually visible as "XX,XXX members")
     - Description / topic
   → If page 404s or invite is expired → mark "Telegram: dead link" and flag as red
3. If page loads but shows no member count, try:
   web_search "<tg_link> telegram members"
```

### Activity thresholds

| Member count | Activity classification |
|---|---|
| > 10,000 | Large community (good if organic; red flag if 99% silent) |
| 1,000 – 10,000 | Active community (normal pump.fun graduate profile) |
| 100 – 1,000 | Small / niche community |
| < 100 | Dead or fresh; no signal |

**Activity quality check:** if member count is high but the group is set to read-only or only the admin posts, it's a held-community-hostage setup — red flag.

### Discord procedure (public-API tier)

The invite landing page at `discord.gg/<code>` is usually JS-rendered and the member count is hidden. Use Discord's own public API instead:

```
a) Extract invite code from discord.gg/<code> or discord.com/invite/<code>

b) Tier 1: web_fetch https://discord.com/api/v10/invites/<code>?with_counts=true
   → Returns public JSON including:
     - approximate_member_count
     - approximate_presence_count    (members online right now)
     - guild.name
     - guild.description
     - guild.verification_level
     - channel.name                  (which channel the invite leads to)
   → No auth required. Works even when the landing page is behind Cloudflare.

c) Tier 2: web_fetch https://discord.gg/<code>
   → Sometimes the HTML still exposes counts in <meta> tags; parse og:description.

d) Tier 3: Perplexity (if present):
   "Discord server discord.gg/<code> — member count, activity level, topic,
    any known associations with crypto tokens"

e) If all 3 tiers fail:
   → Label "Discord: unverifiable (3 tiers failed)" — NOT "dead link".
     Absence of confirmation ≠ evidence of death. Log attempts in audit table.
```

**Interpretation:**
- Large `approximate_member_count` but very low `approximate_presence_count` (< 0.5%)
  → red flag — bought/botted membership, no real activity.
- `verification_level: 0` and recent-creation guild → fresh low-effort server, weight lower.
- Discord is less common than Telegram on pump.fun tokens; absent Discord alone is **not** a red flag — only verified-dead or verified-botted servers are.

---

## 6. Narrative-ignition triggers

**Goal:** catch the exact moment when a viral narrative starts to attach to the token, which typically precedes price by 5-15 minutes.

### Ignition signals (any one = narrative igniting)

1. **🐋 KOL retweet** — a >1M-follower account with historical crypto authority retweets or quotes a $TICKER post. This is the strongest single signal. Price move within 15 min is near-certain (direction depends on the post's content).
2. **News-site pickup** — a coverage article appears on CoinDesk / Decrypt / The Block / CryptoSlate / CoinTelegraph / CMC AI / KuCoin blog mentioning the token. Sometimes precedes KOL pickup.
3. **Thread velocity spike** — a single X thread about the token gains >1,000 retweets in under 30 minutes. Check top 3 search results for thread ID; inspect engagement curve.
4. **CEX listing rumor** — an exchange (Bitget, MEXC, Gate.io, LBank) announces a pending listing. Verify via the exchange's official X account.
5. **Partnership announcement** — token collab with a well-known project (memecoin-partners-influencer / AI-project-partners-memecoin).
6. **Prediction-market market appears** — a new Polymarket / Kalshi / Metaculus question about the token opens with implied-bullish odds. Rare for memecoins, but when it happens it's a high-signal event.
7. **Chart-stock pickup** — appears on Binance Alpha watchlist, KuCoin Alpha alerts, or CoinMarketCap trending top 10.

When any ignition signal is detected, upgrade Factor 3 (Narrative ignition) toward full points in Phase 7 scoring.

### Detection procedure

```
web_search "<ticker> OR <token name> CEX listing OR announcement"
web_search "<ticker> partnership site:x.com"
web_search "<ticker> coindesk OR decrypt OR theblock"
clodds_polymarket_markets "<ticker>"
clodds_metaculus "<ticker>"
```

If any returns a hit dated in the last 4 hours, log it as the ignition timestamp and measure everything else relative to it.

---

## 7. Red flags — immediate disqualification triggers

Any of these fires → render `🚨 SOCIAL RED FLAG` banner on the report and SUPPRESS Phase 8 trade plan:

1. **Creator account deleted / suspended** since token launch
2. **Creator posts gone silent** for >48 hours after token launch (abandonment)
3. **Creator deleted old tweets** en masse (cleaning rug evidence)
4. **Handle changed post-launch** (distancing from the token they deployed)
5. **Follower count dropped >50%** in 24h (mass-unfollow after users caught on)
6. **Telegram shutdown** — group set to read-only or admins all left
7. **Linked website 404s** or points to unrelated content
8. **Token distribution announcement changed** (promised airdrop canceled, team allocation revised upward)
9. **External insider-cluster report** from CryptoSlate / RugCheck / on-chain forensics flagging the token
10. **Screenshot evidence of coordinated shill discussion in a private Discord** surfaces in replies

Red flags 1-5 are creator-related. Red flags 6-10 are project-related. Either category triggers the suppression.

---

## 8. Minimum-data ruleset (when full workflow isn't feasible)

If rate limits, token too obscure, or time pressure prevents the full procedure, use this degraded ruleset:

**Mandatory minimum for every Memecoin-Profile token:**
1. Resolve creator X handle (or render `UNKNOWN` with fallbacks attempted)
2. Creator account age (or `UNKNOWN`)
3. At least one post-velocity measurement (1h or 24h — at least one)
4. KOL-tier scan on the direct X search (top 10 results minimum)
5. Shill-detection red-flag scan on top results

**If even the minimum fails, render this banner:**
```
⚠ SOCIAL DATA INSUFFICIENT — could not run Phase 5 deep-dive
   Attempted: [list tools that were tried]
   Phase 7 social factors capped at 30% of available points.
   Treat composite score as upper bound; reduce size by 50%.
```

Never skip Phase 5 silently for a memecoin. If it can't run, say so in the report.

---

## 9. Output rendering rules

- The 15-line expanded block in Phase 5 must always render **even if most fields are UNKNOWN** — just populate each line with what was actually resolved. UNKNOWN is a data point, not a reason to skip.
- For multi-token reports, each memecoin's Phase 5 block is independent — don't batch-analyze socials across tokens (noise).
- Sources line at end of each token's Phase 5: list 3-5 clickable URLs — the X profile, the TG link, the best news article surfaced, the prediction-market (if any).
- When shill = HIGH and token was still selected for top-N based on technical factors alone, **add a standalone `⚠ SOCIAL MANIPULATION` banner at the top of that token's section** so it's visible before the reader sees the score.

---

## 10. When to fall back to Major Profile

**Memecoin → Major profile graduation criteria:**
- Token MCap crosses $500M AND CEX-listed on Binance/Bybit/MEXC
- AND mature token (age >6 months)
- AND creator fully exited + decentralized community

When a token crosses this line (rare but happens — think DOGE, SHIB, PEPE when they were first listed on Binance), switch to Major Profile scoring. Announce the reclassification in the report: `Reclassified from Memecoin → Major Profile (reason: Binance spot listing + $500M MCap + 6mo age)`.

---

## Cross-references

- Rendering format for the 15-line block: `aria-protocol.md` § PHASE 5: SOCIAL & SENTIMENT → Phase 5 Memecoin Expanded Block
- Phase 7 scoring weights for memecoins: `aria-protocol.md` § PHASE 7 → Memecoin Profile Scorecard
- Pre-pump scan integration: `predictive-scan.md` § Step 3 → Memecoin Profile 8-factor
- Hard-stop logic: `aria-protocol.md` § PHASE 1: IDENTITY & SECURITY CHECK → HARD STOP list (creator prior-rug extends it)
