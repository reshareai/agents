---
name: swap-and-post
description: Swap one of the user's ReShare AI characters into a reel or uploaded video, wait for it, and post or schedule it to their connected socials. Use when the user asks to make, swap, recast or post an AI character video with ReShare AI.
---

# Swap a character into a video and post it (AI swap mode)

Uses the `reshare` MCP tools. Everything runs as the user's ReShare AI account and counts against their plan.

1. **Check the plan.** `whoami`: if `can_swap` is false (Sharing plans and the Free plan include no AI swaps), say so and show `upgrade_url`; the user can still post their own video (share-my-video).
2. **Pick the character.** `list_characters`; use one with `usable: true`. If none, tell the user to create one at https://www.reshare.ai/characters.
3. **Get the source video.**
   - A public reel link (Instagram, TikTok, YouTube Shorts): `add_video_link`.
   - A local file: `create_upload` (purpose `swap`, exact `size` in bytes, `content_type`), then PUT the bytes yourself:
     `curl -sS -T <file> -H 'Content-Type: <content_type>' '<put_url>'`, then `commit_upload`.
   - Poll `get_upload` every 5–10 s until `status` is `ready` (note `duration_s`).
4. **Start the swap.** `start_swap` with `character_id` and `source_id` (or `source_url`), optional `start`/`end` seconds and `subject` ("the man in the blue shirt"). A `plan_limit` error means no AI swaps are left: show the message and `upgrade_url`; do not retry.
5. **Wait.** Poll `get_video` every 20–30 s until `ready` (about 8 minutes; `remaining_seconds` is an estimate) or `failed` (show `error`).
6. **Post.**
   - `list_profiles`; pick account ids with `postable: true` (X is coming soon and cannot be posted to yet).
   - For TikTok, ask the user who can view it: the account's `tiktok.privacy_level_options` from `list_profiles` (e.g. `PUBLIC_TO_EVERYONE` everyone, `MUTUAL_FOLLOW_FRIENDS` friends, `FOLLOWER_OF_CREATOR` followers, `SELF_ONLY` only me), passed as `tiktok.privacy_level`. TikTok requires the user's own choice: never pick one for them, not even everyone. Optional: `tiktok.disable_comment`, `tiktok.disable_duet` or `tiktok.disable_stitch`, or `tiktok.post_mode: "MEDIA_UPLOAD"` to send it to their TikTok inbox instead.
   - Draft a caption with the user. Call `preview_post` with the video, profile, accounts, caption and mode (`now`, `schedule` with `scheduled_at` `YYYY-MM-DDTHH:mm` + `timezone`, or `queue`).
   - Show the preview: accounts, time, issues, `posts_needed` / `posts_left`. Fix any `level: "error"` issue first.
   - Only after the user clearly says yes: `publish_post` with the same arguments, `confirm: true` and a fresh `idempotency_key` (e.g. a UUID). Reuse that key only to repeat a call that got no answer (timeout); a post that came back `failed` needs a new key.
7. **Report** the post status (`get_post`) and the app link.

Never publish without an explicit yes from the user. Never invent account ids.
