# Virlo

Cursor plugin that connects agents to [Virlo](https://virlo.ai) through Virlo's official remote
[Model Context Protocol](https://modelcontextprotocol.io/) server.

Research short-form video without leaving the editor: what is trending on TikTok, YouTube Shorts,
and Instagram Reels, which videos are breaking out, why they worked, who is making them, and which
sounds, hashtags, and hooks are carrying them.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **Virlo**.
3. Click **Install**, then complete the Virlo sign-in prompt.

Or run `/add-plugin virlo` in chat.

## MCP

```json
{
  "mcpServers": {
    "virlo": {
      "type": "http",
      "url": "https://dev.virlo.ai/api/mcp/mcp"
    }
  }
}
```

Auth is OAuth 2.1 with Dynamic Client Registration and PKCE. Cursor registers itself and prompts
for Virlo sign-in when the plugin connects — there is no API key or client ID to configure, and
the plugin never reads an environment variable.

## Before you connect

You need a Virlo account. You can create one during sign-in at [virlo.ai](https://virlo.ai).

Virlo is pay-as-you-go against a prepaid credit balance. Reads of research you already paid for
are free; starting new work is not. Tool calls run as the Virlo user who authorizes the connection
and draw down that account's balance.

## What agents can do

| Category | Capabilities |
| --- | --- |
| Trends | Emerging trends ranked by momentum, a daily curated digest, and trending videos from the last ~48 hours |
| Videos | Analyze why a single video broke out, and rank results by weighted virality instead of raw views |
| Creators | Creator profiles with outlier videos, posting cadence, engagement, and audience demographics and geography |
| Sounds & hashtags | Breakout sounds accelerating off a small base, established trending sounds, and hashtag performance over time |
| Hooks | Trending opening hooks, hook search, and a hook library by niche |
| Research agents | One-shot niche research, plus recurring monitors that keep watching a niche and report what changed |
| Tracking | Track a creator or video over time and read back a tracking report |

Coverage is TikTok, YouTube Shorts, Instagram Reels, and Meta Ads. The hosted runtime is the
source of truth for tool names and schemas.

## Skills and commands

The plugin ships guidance so the agent uses the data well, not just the raw tools.

**Skills**

- `virlo` — routes a question to the right tool, quotes what each call costs, polls async research
  agents correctly, and ranks results by weighted virality.
- `short-form-trend-research` — turns live trend data into a content plan: which trends and sounds
  to ride, which hooks and formats are working, which creators to study.

**Commands**

- `/trend-scout` — what is trending right now across all three platforms.
- `/niche-analysis` — full niche workup: research agent, creators, recurring monitor.
- `/creator-deep-dive` — one creator's outlier videos, cadence, and audience.
- `/genre-monitor` — stand up a recurring monitor for a genre or scene.

## Notes

- **Read-only against social platforms.** Virlo reads public short-form video data. It does not
  post, comment, follow, or connect to your own TikTok, YouTube, or Instagram accounts, and it has
  no write access to any social platform.
- **No local code.** The plugin ships one remote MCP server declaration, one rule, two skills, and
  four commands. It contains no hooks, no scripts, and no executables, and it does not read or
  write files on your machine.
- **Credits.** Research agents, creator lookups, video analysis, and some trend, hashtag, and sound
  reads bill per call. The bundled rule tells the agent to check your balance and quote the
  expected cost before any paid run, and to get your go-ahead before creating a recurring monitor,
  because a recurring monitor bills on every run. The `X-Cost` response header is the authoritative
  charge for any call. Credits are purchased on virlo.ai, never inside Cursor.
- **Async by design.** Research agent runs take about 15-20 minutes. Tools return a `job_id` and
  the agent reads the result later rather than blocking. `finalized: true` is the done signal.
- Rank by weighted virality — `ln(views / followers) × ln(followers)` — rather than raw views, and
  never compare raw views across platforms.
- The URL must have no trailing slash.

## Docs

- Virlo MCP server and tool reference: https://dev.virlo.ai/docs/mcp
- Product: https://virlo.ai
- Support: https://virlo.ai/support
- Privacy: https://virlo.ai/legal/privacy-policy
- Terms: https://virlo.ai/legal/terms-of-service
- Server URL: https://dev.virlo.ai/api/mcp/mcp

Logo is Virlo's official mark.

## License

MIT. See [LICENSE](./LICENSE).
