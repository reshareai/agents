## ReShare AI

The `reshare` MCP server acts as my ReShare AI account (https://www.reshare.ai). Two modes: Sharing (post my own
video) and AI swap (swap one of my AI characters into a reel). Both end in posting to my connected social accounts.

- Swaps and posts count against my plan. Check `whoami` when unsure what is left or whether AI swaps are included.
- Posting is always two steps: `preview_post`, show me the result, and only after I say yes, `publish_post` with
  `confirm: true` and a new `idempotency_key` (reuse it only to repeat a call that got no answer; a failed post needs a new key).
- Never reschedule (`reschedule_post`) or cancel (`cancel_post`) a post without asking me first.
- Local videos: `create_upload`, then PUT the file with `curl -T` to the returned `put_url`, then `commit_upload`.
- Poll `get_upload` / `get_video` instead of guessing; a swap takes about 8 minutes.
- X (Twitter) is coming soon: do not try to post there.
- A `plan_limit` error is final: tell me the message and the `upgrade_url`, don't retry.
