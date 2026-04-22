# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2026-04-22

### Changed — BREAKING

Renamed the MCP server integration from `clodds` to `aria` in lockstep
with the backend rename at `https://github.com/awaisali88/aria-mcp`.
Every surface that referenced the old name has been rewritten:

- **MCP server key** in `.mcp.json`: `clodds` → `aria`.
- **Environment variables**: `CLODDS_MCP_URL` → `ARIA_MCP_URL`,
  `CLODDS_MCP_TOKEN` → `ARIA_MCP_TOKEN`.
- **Tool names**: `clodds_*` → `aria_*` (e.g. `clodds_signals` →
  `aria_signals`, `clodds_solana_balance` → `aria_solana_balance`).
- **Namespaced permissions**: `mcp__plugin_aria-trading_clodds__clodds_*`
  → `mcp__plugin_aria-trading_aria__aria_*`.
- **Prose**: "Clodds MCP" → "ARIA MCP" across README, SKILL files,
  references, and the Claude Desktop system prompt.

### Migration

Users on 1.x must:

1. Rename their local env vars: `CLODDS_MCP_URL` → `ARIA_MCP_URL`,
   `CLODDS_MCP_TOKEN` → `ARIA_MCP_TOKEN`.
2. Point `ARIA_MCP_URL` at the renamed ARIA endpoint.
3. Re-run the `/configure` skill to pick up the new prompts if desired.

Without these changes, every tool call will fail with "Unknown tool"
because the server-side names no longer match.

## [1.0.0]

Initial release under the `clodds` backend.
