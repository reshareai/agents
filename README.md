# ReShare AI for AI agents

Connect [ReShare AI](https://www.reshare.ai) to your AI agent and run both modes from a conversation:

- **Sharing:** upload your own video, trim it or stitch up to 3 clips, preview, then post now or schedule it to your
  connected social accounts (TikTok, Instagram, YouTube, Facebook, LinkedIn, Threads and more; X is coming soon), and
  read analytics.
- **AI swap:** swap one of your AI characters into a reel, then post the result.

Everything goes through one remote MCP server: **`https://www.reshare.ai/api/mcp`** (Streamable HTTP). The OpenAI
package (`openai-plugin/`) uses its Sharing-only endpoint **`https://www.reshare.ai/api/mcp/sharing`** (no AI swap tools,
no plan offers). Full guide and
tool list: https://www.reshare.ai/docs/agents.

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

License: not chosen yet (see LICENSE once added).

## License

MIT © Optijara AI LLC. The ReShare AI name and logo are trademarks of Optijara AI LLC and are not covered by the MIT license.

Support: support@reshare.ai
