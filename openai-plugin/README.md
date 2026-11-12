# ReShare AI plugin for Codex and ChatGPT

An [Agent Plugins](https://agent-plugins.org) package (`plugin.json` + `mcp.json` + `skills/` + `assets/`), the
portable format OpenAI documents for Codex and the ChatGPT/Codex plugin directory (Cursor loads the same format).

- **MCP server:** `https://www.reshare.ai/api/mcp/sharing` (Streamable HTTP), the Sharing-only server: your own
  videos, posting and analytics, without the AI swap tools or plan offers (the OpenAI directory's rules). Signed in
  with OAuth (dynamic client registration, PKCE). On first use you sign in with your ReShare AI account and choose
  what the plugin may do: read, make videos (`jobs`), post.
- **Skills:** `share-my-video` (onboarding), `schedule-week`, `check-analytics`.

For AI swaps as well, use the full server `https://www.reshare.ai/api/mcp` (`../codex`, or any client's custom MCP
settings).

Install in Codex from the public repo:

```
codex plugin marketplace add reshareai/agents
codex plugin add reshare@reshare
```

Codex asks you to sign in to ReShare AI the first time it uses the server.

Prefer a personal API key instead of OAuth? Use `../codex` (`codex mcp add … --bearer-token-env-var`).

`plugin.json` also carries the OpenAI directory listing, five positive and three negative review cases and release
notes (`extensions.com.openai`). Reviewer credentials and the demo video URL are entered in the OpenAI dashboard,
never in this package. Note: the published Agent Plugins 1.0.0 `mcp.json` schema does not yet allow the
`extensions` object that OpenAI documents for auth; Codex 0.166 installs the package as is.
