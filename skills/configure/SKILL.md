---
name: configure
description: Set up ARIA Trading — save your Clodds MCP server URL and authentication token. Use when the user asks to configure ARIA, set up trading, connect to Clodds, change their MCP endpoint, or says "configure".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Bash(ls *)
  - Bash(mkdir *)
  - Bash(echo *)
  - Bash(grep *)
  - Bash(cat *)
---

# /aria-trading:configure — ARIA Trading Setup

Manages the Clodds MCP connection credentials so ARIA can reach the trading tools.

Arguments passed: `$ARGUMENTS`

---

## Dispatch on arguments

### No args — status check

Check the current connection state and guide the user:

1. **MCP URL** — check if `CLODDS_MCP_URL` is set in the environment. If set, show the URL. If not, show "not configured".

2. **MCP Token** — check if `CLODDS_MCP_TOKEN` is set. If set, show first 8 characters masked (e.g., `abc12345...`). If not, show "not configured".

3. **Execution mode** — check `ARIA_EXECUTION_MODE` env var. If set, show its value (`paper` or `live`). If unset, show `paper (default — first-install safe mode)`. Explain how to change it:
   ```
   To default to LIVE:  export ARIA_EXECUTION_MODE=live   (then restart Claude Code)
   To default to PAPER: export ARIA_EXECUTION_MODE=paper  (or just unset the var)
   Per-trade override always wins: say "paper trade X" or "live trade X" in any message.
   ```
   This command does NOT manage Alpaca API credentials — those are set at the user's Claude Code level (outside this plugin).

4. **Alpaca MCP presence** — check whether any `mcp__alpaca__*` tool appears in the current session's tool list. If yes, show `Alpaca MCP: connected (paper-trading ready)`. If no, show `Alpaca MCP: not detected — install & connect the Alpaca MCP at the Claude Code level to enable paper trading`.

5. **Next steps** based on state:
   - Nothing configured → tell the user:
     ```
     Run: /aria-trading:configure <your-clodds-url> <your-token>
     
     Example:
     /aria-trading:configure https://clodds-mcp.up.railway.app sk-abc123...
     ```
   - Both set → show "Ready. ARIA is connected to Clodds." and confirm the URL.
   - Partial → show what's missing and how to fix it.

### `<url> <token>` — save credentials

The user provides their Clodds MCP server URL and authentication token.

1. Parse the arguments: first argument is the URL, second is the token.
2. Validate the URL starts with `http://` or `https://`.
3. Tell the user to set these as environment variables so the MCP config can read them:

   **Option A — Shell environment (recommended):**
   ```bash
   # Add to your shell profile (~/.bashrc, ~/.zshrc, or Windows environment variables)
   export CLODDS_MCP_URL="<url>"
   export CLODDS_MCP_TOKEN="<token>"
   ```

   **Option B — Claude Code settings:**
   Tell the user they can add env vars in their Claude Code settings file (`~/.claude/settings.json`) under the `env` key, or set them as system environment variables.

4. Confirm what was provided and remind the user to restart their Claude Code session for the MCP config to pick up the new env vars.

### `clear` — remove credentials

1. Tell the user to remove `CLODDS_MCP_URL` and `CLODDS_MCP_TOKEN` from their shell profile or environment settings.
2. Confirm the credentials should be cleared and that ARIA will not be able to reach Clodds until reconfigured.

### `test` — test connection

1. Attempt to call any simple Clodds tool (e.g., `clodds_market_index` or `clodds_signals`) to verify the connection works.
2. If it succeeds, confirm "Connection verified — ARIA is ready."
3. If it fails, show the error and suggest the user check their URL and token.

---

## Important notes

- The `.mcp.json` bundled with this plugin uses `${CLODDS_MCP_URL}` and `${CLODDS_MCP_TOKEN}` environment variables.
- These env vars must be available in the shell environment where Claude Code runs.
- After setting env vars, the user must restart their Claude Code session for changes to take effect.
- Never log or display the full token — always mask it after the first 8 characters.
