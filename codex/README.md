# ReShare AI for Codex CLI (API key)

The quickest setup with a personal key. For the plugin (skills + OAuth sign-in), see `../openai-plugin`.

1. Create a key at https://www.reshare.ai/settings/agents and keep it in your shell, not in a file you commit:
   `export RESHARE_API_KEY=rsk_…`
2. Add the server:
   `codex mcp add reshare --url https://www.reshare.ai/api/mcp --bearer-token-env-var RESHARE_API_KEY`
   and set `tool_timeout_sec = 300` under `[mcp_servers.reshare]` in `~/.codex/config.toml` (publishing uploads the
   video first), or paste `config.toml` from this folder into `~/.codex/config.toml`.
3. Optional skills (share your video, swap and post, schedule a week, check analytics):
   `mkdir -p ~/.agents/skills && cp -R skills/* ~/.agents/skills/`
4. Optional: add `AGENTS.md` from this folder to your project or `~/.codex/AGENTS.md`.

Try: `codex "List my ReShare AI videos that are ready to post"`. Tools: https://www.reshare.ai/docs/agents.
