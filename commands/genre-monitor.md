---
name: genre-monitor
description: Stand up a recurring TikTok genre or scene monitor and read discovery signals (sounds, hashtags, rising creators) using Virlo.
---

# Genre monitor

Given a genre, scene, or sub-culture (e.g. "cottagecore", "deep house DJs"):

1. Draft an intent for monitoring it and call `suggest_keywords`; include synonyms and sub-scenes of the same concept. Hashtag-style tokens are fine.
2. Call `create_niche_monitor` with platforms `["tiktok"]`, cadence `weekly`, and the suggested keywords. Confirm the per-run cost with the user first.
3. Poll `get_niche_monitor_data` (`data_type: "overview"`) until `finalized: true` (~15-20 min) — do not loop tightly, hand back the id and check later.
4. Read discovery signals (all free):
   - `sounds` (`order_by: rising`) — sounds breaking out in the genre right now.
   - `hashtags` (`order_by: growth`) — hashtag momentum and top creators per tag.
   - `outliers` (`order_by: rising`, filter by `follower_tier`) — creators gaining velocity, by tier.
   - `benchmarks` — median engagement, followers, and posting frequency per tier.
   - `affinity` — adjacent topics, hashtags, and sounds to expand into.
5. Deliver a genre brief: the sound to ride this week, hashtags heating up, rising creators to watch (with tier), how a creator compares to genre norms, and the recurring monitor now in place.

Check `get_credit_balance` first. Creating the agent costs $0.50/run; the discovery reads above are free.
