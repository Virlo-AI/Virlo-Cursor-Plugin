---
name: trend-scout
description: Discover what is trending right now across TikTok, YouTube Shorts, and Instagram Reels using Virlo.
---

# Trend scout

Scout current trends across the requested platform (default: all platforms) using the Virlo MCP tools:

1. Call `get_emerging_trends` for momentum-ranked early-stage trends and `get_trends_digest` for today's curated editorial digest.
2. Call `get_trending_videos` (filtered by platform if one was given) for the top viral videos from the last ~48 hours, paired with `get_breakout_sounds` for audio accelerating off a small base.
3. Call `get_hashtag_performance` for the top 3-5 trending topics found above (per-hashtag detail; `search_hashtags` only returns the overall ranked list for a date range, not a specific topic).
4. Present a trend report: what is emerging right now, today's curated top trends, hottest videos (ranked by weighted virality, not raw views), and rising hashtags and breakout sounds to watch, plus actionable content ideas.
