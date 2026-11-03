# Changelog

All notable changes to this plugin will be documented here.

The version tracks the Virlo skill content this repo ships, so it stays in step with the other
Virlo agent plugins.

## 0.4.3 — initial Cursor release

- Added the `virlo` MCP server pointing at `https://dev.virlo.ai/api/mcp/mcp`.
- Auth uses OAuth 2.1 with Dynamic Client Registration and PKCE — no API key or client ID to
  configure, and no environment variable to set.
- Added the `virlo` rule: tool routing, credit safety, intent-first keyword research, async job
  polling, and weighted-virality ranking.
- Added the `virlo` and `short-form-trend-research` skills, including Content Trends
  (`get_content_trends`, `get_content_trend`), the `raw_*` instant reads, `how_to_read` and
  `is_paid` handling, and result-presentation guidance.
- Added the `/trend-scout`, `/niche-analysis`, `/creator-deep-dive`, and `/genre-monitor` commands.
- Logo: Virlo's official mark.
