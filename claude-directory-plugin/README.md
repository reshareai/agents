# ReShare AI for Claude Code

Post your own videos to TikTok, Instagram, YouTube and more, now or on a schedule, and read how they did, from
Claude Code. Skills: `share-my-video`, `schedule-week`, `check-analytics`.

## Install

1. `/plugin marketplace add reshareai/agents`
2. `/plugin install reshare-sharing@reshare`
3. Run `/mcp`, pick `reshare` and sign in with your ReShare AI account. You choose what Claude may do: read, make
   videos, post. No API key is needed.
4. Ask: "Post my video ~/Movies/launch.mp4 to TikTok and Instagram tomorrow at 9am" or "How did my posts do this
   month?".

Every post is shown as a preview first, and nothing is published, rescheduled or cancelled until you say yes.

The plugin connects to `https://www.reshare.ai/api/mcp/sharing`. Docs: https://www.reshare.ai/docs/agents. Privacy:
https://www.reshare.ai/privacy. MIT licence (see `LICENSE`).
