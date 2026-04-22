# Link Resolution Playbook — HTTP-Error-Routed Fallback Chains

Load this file **only when a direct `web_fetch` returns a non-200** or when a ARIA data tool returns `"Unknown skill"` / empty help-text. If every fetch in the current analysis is succeeding, this file never needs to enter context.

**Why this file exists:** the `$PUB` report (2026-04-21) showed that `403` / `402` / `ECONNREFUSED` responses were consistently treated as "data UNKNOWN" with no alternative route attempted. That's a false negative — most of those pages are reachable via a public-API alt host, a mirror, an archive, or Perplexity. The correct behavior is: on any non-200, consult the §1 dispatch table, try tiers 1→3 in order (with the §4 stop-rule), and only label `UNKNOWN` after the chain is exhausted. Record every attempt in the Tool-call audit table so the user can see what was tried.

---

## §1 — Error-code dispatch table

For every URL pattern that is known to block anonymous automated fetches, the table below lists the first, second, and third-tier alternative sources. Try them in order. Stop the first time you get usable data.

| Source pattern | Typical error | Tier 1 fallback | Tier 2 | Tier 3 (Perplexity) | Give-up label |
|---|---|---|---|---|---|
| `solscan.io/token/<mint>` | 403 | `web_fetch https://public-api.solscan.io/token/meta?tokenAddress=<mint>` (no-auth JSON) | `mcp__*helius*__*` if present → or `web_fetch https://api.helius.xyz/v0/token-metadata?api-key=<key>` (needs user-supplied key) | Perplexity: `"token metadata for Solana mint <mint> — return supply, decimals, authorities, top 10 holders"` | `UNKNOWN (solscan 403 + 2 fallbacks attempted)` |
| `solscan.io/account/<wallet>` | 403 | `web_fetch https://public-api.solscan.io/account?account=<wallet>` | Helius MCP `getAsset` or `web_fetch https://api.helius.xyz/v0/addresses/<wallet>/balances?api-key=<key>` | Perplexity: `"wallet activity summary for Solana address <wallet> — labels, linked handles, prior token deployments"` | `UNKNOWN (solscan 403 chain exhausted)` |
| `gmgn.ai/sol/token/<mint>` | 403 (Cloudflare) | `web_fetch https://api.gmgn.ai/api/v1/token_info/sol/<mint>` (alt host, often un-shielded) | `web_fetch https://public-api.birdeye.so/defi/token_overview?address=<mint>` (header `x-chain: solana`) | Perplexity: `"gmgn profile for Solana token <mint> — insider cluster, sniper count, dev sold %"` | `UNKNOWN (gmgn 403 chain exhausted)` |
| `gmgn.ai/sol/address/<wallet>` | 403 | `web_fetch https://api.gmgn.ai/api/v1/wallet/sol/<wallet>` | `web_fetch https://api.cielo.finance/v1/wallet/<wallet>` (public pnl endpoint) | Perplexity: `"prior token launches by Solana wallet <wallet>"` | `UNKNOWN (creator wallet: 3 fallbacks failed)` |
| `aria_pumpfun holders/trades/chart <mint>` | 404 | `web_fetch https://api.geckoterminal.com/api/v2/networks/solana/tokens/<mint>/info` + `/pools/<pool>/trades?limit=100` | `web_fetch https://api.dexscreener.com/token-pairs/v1/solana/<mint>` | Perplexity: `"recent trades and holder concentration for <mint>"` (last-resort — GT usually suffices) | `INSUFFICIENT (GT + DexScreener empty)` |
| `rugcheck.xyz/tokens/<mint>` returns `risks:[], topHolders:null, tokenMeta:null` | sparse (not 404 — looks OK but empty) | `web_fetch https://honeypot.is/api/v2/scan?network=solana&address=<mint>` | `web_fetch https://api.gopluslabs.io/api/v1/token_security/solana?contract_addresses=<mint>` | Perplexity: `"honeypot / rug indicators for Solana mint <mint>"` | **Flag `SPARSE` — do NOT count as clean.** In Phase 1, treat sparse-rugcheck as no-data and insist on 2+ corroborating sources before passing the security gate. |
| `x.com/<handle>/status/<id>` | (not an error) — currently mis-handled as "status not account" | **`mcp__*twitterapi*__*` MCP if present** (TwitterAPI.io live proxy — returns full tweet JSON) → run `pumpfun-social-playbook.md §1.4 X-STATUS HANDLER` | `cdn.syndication.twimg.com/tweet-result?id=<id>` (free public JSON) → Nitter mirror rotation (§3) | Perplexity: `"tweet at <URL> — full text, likes, replies, top-3 reply handles"` | `UNKNOWN (MCP + syndication + nitter + Perplexity failed)` |
| `x.com/<handle>` | 402 / 403 | **`mcp__*twitterapi*__*` MCP if present** (`get_user_by_username` / `user_resource`) | Nitter mirror rotation (§3) → `web_search "@<handle> twitter followers"` | Perplexity: `site:x.com <handle> — follower count, bio, account creation date, post count` | `UNKNOWN (MCP + nitter + search failed)` |
| `x.com/i/communities/<id>` | 402 | **`mcp__*twitterapi*__*` MCP `search_tweets` with community filter** if present | `web_fetch https://nitter.net/i/communities/<id>` (plus next mirrors) → `web_search "x.com/i/communities/<id> members"` | Perplexity: `"X community <id> member count, activity level, associated tickers"` | `UNKNOWN (MCP + community 402 + fallbacks failed)` |
| `discord.gg/<invite>` (or `discord.com/invite/<invite>`) — masked invite HTML | invite HTML doesn't show counts | `web_fetch https://discord.com/api/v10/invites/<invite>?with_counts=true` → JSON with `approximate_member_count` + `approximate_presence_count` + `guild.name` + `guild.description` | `web_fetch` the invite landing page and parse meta tags (fallback) | Perplexity: `"Discord server discord.gg/<invite> — member count, activity level, topic"` | `Discord: unverifiable (3 tiers failed)` — **not** "dead link" |
| `t.me/<handle>` | preview thin | `web_fetch https://t.me/s/<handle>` (public web preview, shows last ~N messages + member count) | `web_search "<tg_link> telegram members"` | Perplexity: `"Telegram channel @<handle> — member count, post frequency, last message date"` | `Telegram: unverifiable (3 tiers failed)` |
| `<project-website>` | ECONNREFUSED / DNS fail / 404 | `web_fetch https://web.archive.org/web/2025*/<url>` and `https://web.archive.org/web/2026*/<url>` (wayback machine) | `web_search "<domain> crypto project"` + check any result with screenshot/cache | Perplexity: `"content of <domain> — is there a live version, cached snapshot, or public record of what this site was"` | `Website: DEAD (wayback empty)` — only mark DEAD after wayback confirms no archive |
| `farside.co.uk/btc-etf-flow-all-data/` | blocked / captcha | `web_fetch https://www.theblock.co/data/crypto-markets/spot-btc-etfs` | `web_fetch https://www.cointelegraph.com/tags/spot-bitcoin-etf` | Perplexity: `"BTC spot ETF net flows on <DATE> in USD millions — sum IBIT, FBTC, ARKB, BITB, BTCO, EZBC, BRRR, HODL, BTCW, GBTC"` | `ETF flows: unavailable (3 sources blocked)` |
| Any ARIA tool returning `"Unknown skill"` or help-text-only | — | Claude-native equivalent already listed in `SKILL.md` Data Fallback Protocol table | Perplexity research query equivalent to the tool's purpose | (no tier 4) | per existing SKILL.md behavior |

---

## §2 — TwitterAPI.io MCP (tier-1, preferred)

When the user has installed the [TwitterAPI.io MCP](https://pypi.org/project/twitterapi-mcp/) and any tool matching the glob `mcp__*twitterapi*__*` (or the configured server name from `claude mcp list`) appears in the session's tool list, **prefer it as tier 1 for every X/Twitter fetch** — it returns full live JSON (tweet text, author, likes, reposts, replies, quote count, bookmarks, timestamps) with a real SLA. Same use-if-present pattern as Binance/CoinGecko/Perplexity.

### Common operations → expected MCP tool / resource

| Need | MCP tool / resource | Notes |
|---|---|---|
| Tweet by ID (full JSON incl. engagement) | `get_tweet` / resource `tweet://{tweet_id}` | Primary endpoint for §1.4 X-status handler |
| Tweet replies (top N) | resource `tweet://{tweet_id}/replies` | Feeds shill-detection + reply sentiment |
| Tweet retweeters | resource `tweet://{tweet_id}/retweeters` | KOL-tier check (do any retweeters have >10K followers?) |
| User profile (followers, age, bio, post count) | `get_user_profile` / resource `user://{username}` | Step §1.2 creator handle legitimacy |
| User recent tweets | resource `user://{username}/tweets` | Post-velocity leading indicator (see pumpfun-social-playbook.md §2) |
| Search tweets | `search_tweets` | `$TICKER` cashtag search → KOL tier scan |

### Graceful degradation

- **No MCP installed** → skip silently, advance to tier 2 (syndication / nitter).
- **MCP installed but returns 401/403** → API key issue; log in audit table as `TwitterAPI.io auth failed`, advance to tier 2, do NOT retry (protect user's rate limit).
- **MCP installed and returns 429 rate-limit** → advance to tier 2 once, then back off and resume MCP on the next invocation.
- **Any MCP tool returns the tweet / profile data** → **stop the chain immediately**; nitter/Perplexity/search are not consulted.

### Cost awareness

TwitterAPI.io charges ~$0.00015 per API call (15 credits minimum). Per-analysis cost is trivial (<$0.01 for a 5–10 tweet memecoin dig), but **do not batch-fetch entire user timelines unless post-velocity measurement genuinely requires it.** Follow the token-minimalism cap in §5 below — same 4-call ceiling per URL applies to MCP calls.

### Attribution

Tag any datapoint sourced via the MCP as `(via TwitterAPI.io)` in the rendered Phase 5 block. This distinguishes paid-tier high-confidence data from the free-tier Nitter / Perplexity tiers, which carry a staleness / hallucination risk.

---

## §3 — Nitter mirror rotation (tier-2 free fallback)

When the TwitterAPI.io MCP is not available, or returns an error, rotate through these public Nitter hosts in order. On 5xx, connection-refused, or timeout from mirror *N*, advance to mirror *N+1* — don't abandon the query:

```
1. https://nitter.net/<path>
2. https://nitter.privacydev.net/<path>
3. https://nitter.poast.org/<path>
4. https://nitter.kavin.rocks/<path>
5. https://nitter.1d4.us/<path>
6. https://nitter.unixfox.eu/<path>
```

Nitter instances come and go; if all six are down on a given day, drop to Tier 3 (Perplexity) directly.

**What Nitter gives you that the X site blocks:**
- Full tweet text, likes, reposts, replies, bookmark counts on any public status
- Follower count, following count, bio, account creation date on any public profile
- Community member list (partial) on `/i/communities/<id>`
- Search results for `$TICKER` queries via `/search?q=%24TICKER`

Nitter cannot access private/locked accounts — those remain `UNKNOWN` regardless of tier.

---

## §4 — Perplexity fallback tier (tier-3 last resort)

Perplexity MCP (when the user has it installed) is the tier-3 "last resort" for pages/content that all direct fetches refuse to serve. The skill is **MCP-agnostic** — it looks for any tool matching the glob `mcp__*perplexity*__*` (or `mcp__*pplx*__*`) in the current session's tool list, same use-if-present pattern as the Binance/CoinGecko chain in `tool-inventory.md`.

### Prompt template

```
Summarize what is publicly known at <URL>. Include these specific fields: <list>.
If the page is behind auth, paywall, or rate limit, use cached copies, archive.org,
alternate hosts, or cite "no public record" explicitly. Return structured bullets,
not prose. Cite the retrieval source (e.g. "wayback 2026-01-15", "cache.google").
```

### Fields to request per URL class

| URL class | Fields to ask for |
|---|---|
| X profile | follower count, bio, account creation date, verified status, post count last 7d |
| X status | full text, posted date (UTC), likes, reposts, replies, top-3 reply handles with follower tiers |
| Discord/TG | member count (approximate is fine), activity level (posts/day), server topic/description, last visible message date |
| Project website | live/dead, current content summary, last known content if archived, any linked socials |
| ETF flows page | daily net flow per fund in USD millions, total net flow for the day, 7d cumulative |
| Token on solscan/gmgn | supply, decimals, mint/freeze authorities, top-10 holders %, creator wallet, prior tokens by creator |

### Guardrail — what Perplexity is NOT allowed to source

Perplexity returns prose and may paraphrase/hallucinate numeric data. **Never quote Perplexity as the primary source for:**
- Price, volume, liquidity, OHLCV (use GeckoTerminal / Binance klines / pump.fun directly)
- Live holder counts or balances (use `aria_pumpfun holders` / `public-api.solscan.io` / Helius)
- Trade execution quotes (use `aria_jupiter_quote` / `aria_solana_quote`)
- Current-moment prediction-market odds (use Polymarket/Kalshi MCPs or web_fetch)

Perplexity is for **social/meta/site-content signals only** — things that are either qualitative or archival. Tag any datapoint sourced via Perplexity as `(via Perplexity)` in the rendered block so the reader knows its confidence floor.

---

## §5 — When to stop (token-minimalism cap)

Per `memory/feedback_token_minimalism.md`, the skill must not burn context retrying forever. Hard cap per URL:

```
Tier 1  →  up to 2 attempts   (e.g. public-api alt host + one retry)
Tier 2  →  up to 1 attempt    (one alternative second-tier)
Tier 3  →  up to 1 Perplexity query
--------------------------------
Max 4 calls per URL before falling through to "UNKNOWN + cite attempts".
```

Every attempt — successful or not — must be appended as a row in the final **Tool-call audit** table at the end of the report, with the specific error code or "empty" marker. The user should be able to look at that table and see exactly which alternatives were tried and in what order.

Example audit row:
```
| 1 | solscan.io/account/<wallet>                | ❌ 403                  | fallback attempted |
| 1 | public-api.solscan.io/account?account=...  | ✅ 200                  | — |
```

---

## §6 — Integration points in the pipeline

- **Phase 1** (`aria-protocol.md § PHASE 1`) — if `aria_token_security` or rugcheck returns sparse, dispatch via §1 table. After 5 of 7 tiered sources fail, render the `PRESUMED-RISK` banner per aria-protocol.md.
- **Phase 4** (`aria-protocol.md § PHASE 4 On-chain`) — before labeling `Creator wallet: UNKNOWN`, dispatch via §1 rows for `solscan.io/account/*` and `gmgn.ai/sol/address/*`.
- **Phase 5 Memecoin** (`pumpfun-social-playbook.md`) — creator-handle chain §1.1 invokes the §1 X-row dispatch and the new §1.4 X-status handler.
- **Phase 6 Macro** (`aria-protocol.md § PHASE 6`) — ETF flows failover per the `farside.co.uk` row.
- **All phases** — anywhere a `web_fetch` returns non-200, dispatch via §1 before continuing.

---

## §7 — Extending this table

When new sources/errors are encountered in the wild, add a row to §1 with:
1. The exact URL pattern that fails
2. The error code / failure mode
3. Three ordered alternative sources (at least one public API, at least one Perplexity fallback)
4. The correct "give-up" label — something the user can read and know what was tried

Do not leave `UNKNOWN` without an audit-table trail of what was attempted.
