---
name: creator-deep-dive
description: Deep-dive analysis of a specific creator (platform + username) using Virlo.
---

# Creator deep dive

Given a platform (`youtube` / `tiktok` / `instagram`) and a username:

1. Call `lookup_creator` with that platform and username, including `videos,outliers`.
2. From the results, identify top-performing videos (outliers), common themes/hashtags in successful content, posting frequency/consistency, and engagement patterns (views vs likes vs comments).
3. Ask the user before calling `track_creator` to start ongoing daily tracking.
4. Summarize: strengths, content strategy insights, and growth trajectory.
