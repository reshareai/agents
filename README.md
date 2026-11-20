<p align="center">
  <img src="https://www.reshare.ai/brand/logo/icon-512.png" alt="ReShare AI logo" width="128" height="128">
</p>

# ReShare AI for AI agents

[![MCP Badge](https://lobehub.com/badge/mcp/reshareai-agents)](https://lobehub.com/mcp/reshareai-agents)

Connect [ReShare AI](https://www.reshare.ai) to your AI agent and run both modes from a conversation:

- **Sharing:** upload your own video, trim it or stitch up to 3 clips, preview, then post now or schedule it to your
  connected social accounts (TikTok, Instagram, YouTube, Facebook, LinkedIn, Threads and more; X is coming soon), and
  read analytics.
- **AI swap:** swap one of your AI characters into a reel, then post the result.

Everything goes through one remote MCP server: **`https://www.reshare.ai/api/mcp`** (Streamable HTTP). The OpenAI
package (`openai-plugin/`) uses its Sharing-only endpoint **`https://www.reshare.ai/api/mcp/sharing`** (no AI swap tools,
no plan offers). Full guide and
tool list: https://www.reshare.ai/docs/agents.

## Connect

**Any MCP client with an API key** (Cursor, VS Code, Windsurf, Claude Code, scripts): the Streamable HTTP server
`https://www.reshare.ai/api/mcp/public`. Tool calls need your personal API key in the `Authorization` header; create
one at https://www.reshare.ai/settings/agents and keep it in the `RESHARE_API_KEY` environment variable.

```json
{
  "mcpServers": {
    "reshare": {
      "type": "http",
      "url": "https://www.reshare.ai/api/mcp/public",
      "headers": { "Authorization": "Bearer ${RESHARE_API_KEY}" }
    }
  }
}
```

**claude.ai, Claude Desktop and ChatGPT** use `https://www.reshare.ai/api/mcp` instead and sign in with your ReShare AI
account (OAuth): add a custom connector with that URL. No API key is needed there.

The `.mcp.json` at the root of this repo points at `https://www.reshare.ai/api/mcp/public` without a key, so anyone can
list the tools; add your key as an Authorization header for tool calls.

## Tools / Prompts / Resources

Anyone can see what the server offers, without signing in, at **`https://www.reshare.ai/api/mcp/public`**:
`initialize`, `tools/list`, `prompts/list` and `resources/list` answer with names, descriptions and input schemas only.
Every tool call, prompt and resource read needs a credential: on `/api/mcp/public` a personal API key
(`Authorization: Bearer rsk_…`); on `/api/mcp` and `/api/mcp/sharing` an API key or OAuth sign-in (below).

<!-- capabilities:start -->
Generated from `lib/agent/catalog.ts` (server version 0.4.0) by `scripts/agent-submissions.ts`. Do not edit by hand.

### Tools (21)

- `whoami`: Who am I (read-only)
- `list_characters`: List characters (read-only) *(not on /api/mcp/sharing)*
- `list_profiles`: List profiles and accounts (read-only)
- `list_videos`: List videos (read-only)
- `get_video`: Get a video (read-only)
- `get_upload`: Get an upload or link (read-only)
- `list_posts`: List posts (read-only)
- `get_post`: Get a post (read-only)
- `get_usage`: Get usage (read-only)
- `get_analytics`: Post analytics overview (read-only)
- `get_post_analytics`: Analytics for one post (read-only)
- `create_upload`: Start a video upload
- `commit_upload`: Finish a video upload
- `add_video_link`: Add a reel link *(not on /api/mcp/sharing)*
- `start_swap`: Start an AI character swap *(not on /api/mcp/sharing)*
- `use_own_video`: Make a postable video from your upload
- `stitch_videos`: Join up to 3 clips into one video
- `preview_post`: Preview a post (dry run) (read-only)
- `publish_post`: Publish or schedule a post
- `reschedule_post`: Reschedule or edit a scheduled post
- `cancel_post`: Cancel a scheduled post

### Prompts (3)

- `swap_character_into_reel` (video_url, character): Swap one of your AI characters into a public reel link, then show the result when it is ready. *(not on /api/mcp/sharing)*
- `post_video_everywhere` (video, when?): Preview a ready video on every connected account that can post, then publish it now or at a time once you say yes.
- `weekly_analytics_summary`: Summarise how your posts did over the last 7 days: views, likes and comments per network, the best post and the best time to post.

### Resources (2)

- `reshare://docs/getting-started` (text/markdown): How to connect (OAuth or an API key), the usual flows, and what each tool does.
- `reshare://plans` (application/json): The public plans and their monthly limits (posts, uploads, AI credits, profiles), as on the pricing page. *(not on /api/mcp/sharing)*
<!-- capabilities:end -->

## Sign in

| Method | Use it for | How |
|---|---|---|
| OAuth 2.1 (sign in with ReShare AI) | claude.ai / Claude Desktop connectors, ChatGPT, the Codex plugin, any client that supports MCP OAuth | Add the server URL; the client opens ReShare AI's sign-in and asks what it may do: `read`, `jobs` (uploads, videos, swaps), `post`. Revoke it any time in Settings → Connect your agent. |
| Personal API key `rsk_…` | Claude Code, Codex CLI, Cursor, VS Code, scripts | Create one at https://www.reshare.ai/settings/agents, keep it in an environment variable, send `Authorization: Bearer $RESHARE_API_KEY`. |

Every post is previewed first (`preview_post`) and published only with `confirm: true` after you say yes.

## What is in this repo

| Path | What |
|---|---|
| `.claude-plugin/marketplace.json` | Claude Code plugin marketplace (`/plugin marketplace add reshareai/agents`) |
| `claude-plugin/` | Claude Code plugin: the MCP server (API key from `RESHARE_API_KEY`) + skills |
| `.agents/plugins/marketplace.json` | Codex plugin marketplace (`codex plugin marketplace add reshareai/agents`) |
| `openai-plugin/` | Agent Plugins package for Codex and the ChatGPT/Codex plugin directory: MCP server with OAuth + skills |
| `codex/` | Codex CLI with an API key: `config.toml`, `AGENTS.md`, skills for `~/.agents/skills` |
| `.mcp.json` | The server at `/api/mcp/public` with no key, for scanners and listing the tools; add your key as an Authorization header for tool calls |
| `server.json` | The listing for the official MCP Registry (`ai.reshare/reshare`) |

Skills (the same text in every client): `share-my-video`, `swap-and-post`, `schedule-week`, `check-analytics` (the OpenAI package leaves out `swap-and-post`).

## Install

**Claude Code (plugin)**

```
export RESHARE_API_KEY=rsk_…
/plugin marketplace add reshareai/agents
/plugin install reshare@reshare
```

**Claude Code (server only)**

```
claude mcp add --scope user --transport http reshare https://www.reshare.ai/api/mcp --header "Authorization: Bearer $RESHARE_API_KEY"
```

**Codex (plugin, OAuth)**

```
codex plugin marketplace add reshareai/agents
codex plugin add reshare@reshare
```

**Codex (API key)**

```
codex mcp add reshare --url https://www.reshare.ai/api/mcp --bearer-token-env-var RESHARE_API_KEY
mkdir -p ~/.agents/skills && cp -R codex/skills/* ~/.agents/skills/
```

**claude.ai, Claude Desktop, ChatGPT:** add a custom connector with the URL `https://www.reshare.ai/api/mcp` and sign in.

**Any other MCP client:** a Streamable HTTP server at `https://www.reshare.ai/api/mcp` with either OAuth or the header
`Authorization: Bearer rsk_…`. Per-client snippets (Cursor, VS Code, Windsurf, Gemini CLI, Grok): https://www.reshare.ai/docs/agents.

## Support

Help: https://www.reshare.ai/support · Privacy: https://www.reshare.ai/privacy · Terms: https://www.reshare.ai/terms

## Maintainers

This folder is generated in the ReShare AI app repo (`integrations/`) and copied here as is. Skills are edited in
`claude-plugin/skills/` and copied to the other clients by `scripts/agent-submissions.ts` in the app repo.

## License

MIT © Optijara AI LLC. The ReShare AI name and logo are trademarks of Optijara AI LLC and are not covered by the MIT license.

Support: support@reshare.ai
