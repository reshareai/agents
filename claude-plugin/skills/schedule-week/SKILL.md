---
name: schedule-week
description: Plan and schedule a week of ReShare AI posts from the user's ready videos, one preview per post and a single confirmation. Use when the user asks to fill their calendar, schedule a week or batch-schedule posts.
---

# Schedule a week of posts

1. `list_videos` → keep `status: "ready"` videos (page with `next_cursor` if needed). `list_posts` with `status: "upcoming"` to see what is already scheduled; do not double-book a video on the same account.
2. `list_profiles` → accounts with `postable: true` (X is coming soon and cannot be posted to yet). `whoami` → posts left this period: each account of each post uses one. If what is left cannot cover the week, say how many fit and suggest fewer accounts.
3. Propose a table to the user: date, local time, timezone, video, accounts, caption, and for TikTok who can view each post. Default to one post a day at a sensible local hour unless the user gives times (`get_analytics` `best_time` is a good hint when present).
   For TikTok, ask the user who can view it: the account's `tiktok.privacy_level_options` from `list_profiles` (e.g. `PUBLIC_TO_EVERYONE` everyone, `MUTUAL_FOLLOW_FRIENDS` friends, `FOLLOWER_OF_CREATOR` followers, `SELF_ONLY` only me), passed as `tiktok.privacy_level`. TikTok requires the user's own choice: never pick one for them, not even everyone. Optional: `tiktok.disable_comment`, `tiktok.disable_duet` or `tiktok.disable_stitch`, or `tiktok.post_mode: "MEDIA_UPLOAD"` to send it to their TikTok inbox instead.
4. For each row call `preview_post` (mode `schedule`, `scheduled_at` `YYYY-MM-DDTHH:mm`, `timezone`). Collect issues and fix them with the user.
5. After one explicit yes for the whole table, call `publish_post` per row with `confirm: true` and a distinct `idempotency_key` per row (e.g. `week-<date>-<video_id>`), so a retry never doubles a post.
6. Summarise what was scheduled with `list_posts` (`upcoming`). A post can be moved with `reschedule_post` or cancelled with `cancel_post`, each only after the user agrees.
