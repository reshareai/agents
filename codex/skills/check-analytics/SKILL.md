---
name: check-analytics
description: Read the user's ReShare AI post analytics (views, likes, comments per network, best time to post, usage) and explain what is working. Use when the user asks how their posts or videos are doing.
---

# Check analytics

Read-only: these tools change nothing, so they need only the `read` permission.

1. `get_analytics` with a `range` (`7d`, `30d`, `90d`, `all`; default 30d) and `sort` (`views`, `engagement`, `recent`).
2. For one post the user cares about: `get_post_analytics` with its id (`list_posts` with `status: "history"` finds it).
3. `whoami` for what is left this period; `get_usage` for this month's usage events.
4. Answer in plain words: top posts and why (network, time, caption), totals vs the previous pattern, the best posting window (`best_time`, only present from 5 published posts), and one or two concrete next steps. Numbers can lag up to 30 minutes; `pending: true` means not fetched yet.
