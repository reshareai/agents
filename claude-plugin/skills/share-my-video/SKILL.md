---
name: share-my-video
description: Post the user's own video (a local file) to their connected socials with ReShare AI, whole, trimmed or stitched from up to 3 clips, now or at a scheduled time. Use when the user asks to share, post, publish, cross-post or schedule a video they already have.
---

# Share your own video (Sharing mode)

Uses the `reshare` MCP tools. Everything runs as the user's ReShare AI account and counts toward what is left this period.

1. **Check the account.** `whoami`: posts left this period, permissions. Posting needs the `post` permission; uploading needs `jobs`.
2. **Upload the file.** `create_upload` with purpose `own`, the exact `size` in bytes and `content_type` (e.g. `video/mp4`), up to the size the tool states. Then PUT the bytes yourself to the returned `put_url` within 10 minutes:
   `curl -sS -T <file> -H 'Content-Type: <content_type>' '<put_url>'`
   Then `commit_upload`, and poll `get_upload` every 5–10 s until `status` is `ready` (note `duration_s`).
3. **Make the postable video.**
   - Whole or trimmed: `use_own_video` with the upload id, optional `start`/`end` seconds.
   - Several clips joined (up to 3, in order): `stitch_videos` with each clip's id and `start`/`end`, and a new `request_id`.
   - Poll `get_video` every 15–30 s until `ready` (a short content check runs first) or `failed` (show `error`).
4. **Choose where.** `list_profiles`; use only accounts with `postable: true`. If an account is not postable, tell the user `not_postable_reason` (reconnect it in the app; X is coming soon and cannot be posted to yet). Never invent account ids.
5. **Preview.** Draft the caption with the user (per-network caption or YouTube title through `overrides` when they want). `preview_post` with the video, profile, accounts, caption and mode: `now`, `schedule` (`scheduled_at` `YYYY-MM-DDTHH:mm` + `timezone`) or `queue`. Show the user the accounts, time, issues and `posts_needed` / `posts_left`; fix any `level: "error"` issue first.
6. **Publish only after a clear yes.** `publish_post` with the same arguments, `confirm: true` and a fresh `idempotency_key` (e.g. a UUID). Reuse that key only to repeat a call that got no answer; a post that came back `failed` needs a new key.
7. **Report.** `get_post` for each account's status and live URL. A scheduled post can be moved with `reschedule_post` or cancelled with `cancel_post`, again only after the user agrees.

A `plan_limit` error is final: show its message, do not retry.
