---
name: virlo
description: >-
  Looks up live short-form video data on TikTok, YouTube Shorts, and Instagram Reels through the
  Virlo MCP tools: emerging trends, viral videos, creator profiles and audience demographics,
  trending and breakout sounds, hashtags, and opening hooks, plus one-shot and recurring niche
  research agents. Use when the user asks what is trending or going viral, why a video popped,
  how a creator, sound, or hashtag is performing, which hooks or formats work in a niche, or asks
  to research, monitor, or keep watching a niche, topic, or competitor on short-form video. Also
  covers Virlo credit costs, async job polling, and ranking by weighted virality rather than raw
  views.
---

# Virlo: short-form video intelligence

Virlo indexes short-form video across TikTok, YouTube Shorts, Instagram Reels, and Meta Ads for
every niche. This skill tells the agent which Virlo tool answers which question, what each call
costs, and how to read the results.

> **What this skill does to the user's account.**
>
> - **Spends Virlo credits.** Creation tools (research agents, creator lookups, video analysis)
>   and some trend, hashtag, and sound reads bill per call. Check `get_credit_balance` and quote
>   the expected cost before any paid run. Get a yes before creating a recurring agent, because
>   it bills on every run.
> - **Talks to one endpoint only:** Virlo's hosted MCP server at `https://dev.virlo.ai/api/mcp/mcp`,
>   under the user's own Virlo account, which they sign in to with OAuth. The plugin never asks
>   for, stores, or reads an API key.
> - **Reads public social data only.** It does not post, comment, follow, or touch the user's
>   social accounts. It does not run local commands and does not write files.

## Setup

The Virlo tools must be connected before this skill can do anything. Call `get_credit_balance`
first. If the tool does not exist, the MCP server is not connected. Tell the user:

Open **Cursor Settings → Plugins**, install **Virlo**, and sign in when Virlo prompts. Sign-in is
OAuth, so there is no key to create, paste, or store. A user with no Virlo account can create one
at [virlo.ai](https://virlo.ai) during sign-in.

## Route the question to the right tool

Every tool below comes from the `virlo` MCP server this plugin bundles. If a bare tool name does
not resolve, qualify it with the server name, as in `virlo:get_emerging_trends`. The server
decides its own tool list, so read the tools it actually offers rather than assuming one exists
because it is named here.

| The user asks | Call | Cost |
|---|---|---|
| "What's trending right now?" | `get_emerging_trends`, `get_trends_digest`, `get_trending_videos` | per-call reads |
| "What sounds are blowing up?" | `get_breakout_sounds` (early), `get_trending_sounds` (established) | per-call reads |
| "What works in niche X?" | `suggest_keywords` -> `search_keywords` (one-shot research agent) | $0.50 (+$1.00 with Data Intelligence) |
| "Keep watching niche X" | `suggest_keywords` -> `create_niche_monitor` (recurring agent) | $0.50 per run (+$1.00 with Data Intelligence) |
| "Tell me about @creator" | `lookup_creator` (`batch_lookup_creators` for many) | paid |
| "Why did this video pop?" | `analyze_video` | paid |
| "What about this sound / hashtag?" | `lookup_sound`, `lookup_hashtag`, `get_hashtag_performance` | per-call reads |
| "Give me hooks that work" | `get_trending_hooks`, `search_hooks`, `get_hook_library` | per-call reads |
| "Track this creator/video over time" | `track_creator`, `track_video` -> `get_tracking_report` | paid |
| "Where's my old research?" | `list_keyword_searches`, `list_niche_monitors`, `list_tracked_items` | free |

Before you create new paid work, list existing searches and monitors. Re-reading an existing
result (`get_keyword_search_results`, `get_niche_monitor_data`, `get_satellite_run`) is always
free. Filter by date, platform, or `min_views` at read time instead of paying to re-run.

The `X-Cost` response header is the authoritative charge for any call. Do not assume a read is
free. When unsure, compare `get_credit_balance` before and after.

## Write the intent first

Never turn a topic straight into keywords. Write one concrete sentence first: goal + content type
+ exclusions. Example: "Find short-form videos where home cooks show 15-minute weeknight meals,
excluding restaurant reviews and mukbangs." Pass it to `suggest_keywords`. If `quality.passes` is
false, sharpen the intent and retry. Then pass the same intent and the suggested keywords to
`search_keywords` or `create_niche_monitor`. Do not pass `min_views` or `time_period` at creation.

## Async jobs

- Creation tools auto-poll for about 25 seconds, then return a `job_id`.
- Research agent runs take about 15-20 minutes median, up to 45. Hand the id back to the user and
  check later with `check_job_status` or the matching read tool. Do not busy-wait in a loop.
- `finalized: true` is the done signal. `status: "completed"` alone can still have AI analysis
  pending. A null analysis field means "not yet", not "no data".
- `partial_failure` is usable data: one platform or keyword failed, the rest succeeded.

## Read results correctly

- **Rank by weighted virality, never raw views:** `ln(views / followers) x ln(followers)`.
  >= 35 exceptional, 25-35 very strong, 18-25 strong, 10-18 promising.
- Never compare raw views across platforms. A TikTok view and a YouTube Shorts view are not the
  same unit.
- Prefer `order_by=weighted_score`, `rising`, or `growth` where a tool offers it. Raw view sorts
  over-favor accounts that are already big.
- For non-trivial research, read the MCP resource `virlo://docs/agent-playbook` first. It has the
  canonical routing and interpretation guidance for the current server version.

## Example prompts

- "What's trending on TikTok in the fitness niche this week?"
- "Why did this video go viral? https://www.tiktok.com/@.../video/..."
- "Look up @mrbeast on YouTube and show his outlier videos."
- "Which sounds are breaking out right now that I should use?"
- "Set up a weekly monitor for skincare content on TikTok and Instagram."
- "Give me 10 hooks that are working for SaaS founders on short-form."

## Troubleshooting

| Symptom | Fix |
|---|---|
| Virlo tools missing | The MCP server is not connected. See Setup. |
| 401 / 403 | The Virlo sign-in expired or was revoked. Reinstall or reconnect Virlo in **Cursor Settings -> Plugins** and sign in again. |
| Out of credits | Top up at dev.virlo.ai. Tell the user the balance before retrying. |
| Search returns 0 videos | Usually an intent or keyword problem, not an outage. Re-run `suggest_keywords` with a sharper intent. |
| Result fields are null | The job is not finalized yet. Check again later. |

## Keeping this skill updated

**Source:** [github.com/Virlo-AI/Virlo-Cursor-Plugin](https://github.com/Virlo-AI/Virlo-Cursor-Plugin)
**MCP docs and tool reference:** [dev.virlo.ai/docs/mcp](https://dev.virlo.ai/docs/mcp)

Installed from the Cursor Marketplace, the plugin updates itself when a new version is published.
