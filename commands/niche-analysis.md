---
name: niche-analysis
description: Full niche analysis (research agent + creators + recurring monitor) for a given niche, using Virlo.
---

# Full niche analysis

Given a niche or topic:

1. Draft a concrete one-sentence intent (goal + content type + exclusions). Call `suggest_keywords` with that intent; if `quality.passes` is false, sharpen the intent and retry. Never invent intent from a keyword list.
2. Call `search_keywords` with the same intent and the suggested keywords (one-shot Content Research Agent). Do not pass `min_views` or `time_period` — filter at read time. Expect ~15-20 minutes; poll until `finalized: true`.
3. Call `get_keyword_search_results` to review videos, creator outliers (prefer `order_by=weighted_score`), AI analysis, and trend themes.
4. Call `lookup_creator` on the top 2-3 creator outliers for full profiles and video analytics.
5. Call `create_niche_monitor` with the same intent and best keywords, cadence `weekly`, for ongoing tracking. Confirm the per-run cost with the user first.
6. Summarize: top trending keywords and hashtags, most promising creators, content patterns, and the recurring monitor now in place.

Check `get_credit_balance` before starting.
