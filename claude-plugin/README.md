# ReShare AI plugin for Claude Code

Post your own videos or AI character swaps to your connected socials, now or scheduled, and read post analytics,
from Claude Code. Skills: `share-my-video`, `swap-and-post`, `schedule-week`, `check-analytics`.

1. Create a key at https://www.reshare.ai/settings/agents (Settings → Connect your agent) and export it where you start
   Claude Code: `export RESHARE_API_KEY=rsk_…`
2. Install:
   ```
   /plugin marketplace add reshareai/agents
   /plugin install reshare@reshare
   ```
3. Ask: "Post my video ~/Movies/launch.mp4 to TikTok and Instagram tomorrow at 9am" or "Swap my character Mia into
   this reel and schedule it for Friday 6 pm."

The MCP server is `https://www.reshare.ai/api/mcp` (override with `RESHARE_MCP_URL` for a local server). The key is read
from your environment; it is never stored in the plugin. Without the plugin:
`claude mcp add --scope user --transport http reshare https://www.reshare.ai/api/mcp --header "Authorization: Bearer $RESHARE_API_KEY"`.

Privacy: https://www.reshare.ai/privacy · Terms: https://www.reshare.ai/terms · Support: https://www.reshare.ai/support
