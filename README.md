<p align="center">
  <a href="https://zillapi.com">
    <img src="assets/logo-512.png" width="160" height="160" alt="Zillapi" />
  </a>
</p>

<h1 align="center">Zillow Plugin — Live Property Data for AI Coding Agents</h1>

<p align="center">
  <b>Zestimates, full property records, and listing search on 160M+ U.S. homes — inside ChatGPT, Codex, Cursor, GitHub Copilot, Kiro, VS Code and Claude Code.</b><br/>
  One install. OAuth sign-in. No API key to manage. Free tier, no card.
</p>

<p align="center">
  <a href="https://agent-plugins.org/specification"><img src="https://img.shields.io/badge/Agent_Plugins-1.0.0-000000?style=for-the-badge" alt="Agent Plugins 1.0.0"/></a>
</p>

<p align="center">
  <a href="https://zillapi.com"><img src="https://img.shields.io/badge/Website-zillapi.com-0061FF?style=for-the-badge" alt="Website"/></a>
  <a href="https://zillapi.com/api/properties/"><img src="https://img.shields.io/badge/Docs-API_Reference-06B6D4?style=for-the-badge&logo=readthedocs&logoColor=white" alt="Docs"/></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-4CAF50?style=for-the-badge" alt="MIT License"/></a>
</p>

> **160M+ U.S. properties · 300+ fields per home · hosted MCP · free tier, no card**
> [Zillapi](https://zillapi.com) is an independent service for Zillow-sourced data — not affiliated with Zillow.

---

## What you get

One plugin, three kinds of superpowers, zero configuration:

| Component | What it does |
|---|---|
| **MCP server** (`zillapi`) | 4 hosted tools — `lookup_property_by_address`, `lookup_property_by_zpid`, `get_zestimate`, `search_listings` — over streamable HTTP with OAuth 2.1. No key handling: your agent signs you in on first use. |
| **Skill** (`zillow`) | Teaches the agent when to reach for property data, which tool is cheapest, and how to build search bounding boxes — so plain-English questions just work. |
| **Slash commands** | `/zillow:property` · `/zillow:zestimate` · `/zillow:search` · `/zillow:comps` — one-line workflows for the things you do all day. |

**Just ask, in plain English:**

```txt
What's the Zestimate on 17 Zelma Dr, Greenville SC?
Find 3-bed homes for sale under $600k in Greenville SC.
Run comps for https://www.zillow.com/homedetails/…_zpid/
```

**Or use a command:**

```txt
/zillow:comps 17 Zelma Dr, Greenville, SC 29617
```

…and the agent pulls the subject property, searches recently sold homes around it, and hands back a comp table with an indicated value.

---

## Install

### Cursor

- **Marketplace:** browse [cursor.com/marketplace](https://cursor.com/marketplace) or search "zillow" in the **Customize** panel, or
- **In the editor:** type `/add-plugin` and enter this repo:

```txt
https://github.com/ZeroPointRepo/zillow-plugin
```

**Add to Cursor** — MCP server only, no plugin. Paste this deeplink into your browser:

```txt
cursor://anysphere.cursor-deeplink/mcp/install?name=zillapi&config=eyJ1cmwiOiJodHRwczovL2FwaS56aWxsYXBpLmNvbS9tY3AifQ==
```

First property question you ask, Cursor prompts a Zillapi OAuth sign-in (create a free account if you don't have one — 100 credits, no card). Done.

### Claude Code

```bash
/plugin marketplace add ZeroPointRepo/zillow-plugin
/plugin install zillow@zillapi
```

### VS Code

Command Palette → **Chat: Install Plugin From Source**, then paste:

```txt
https://github.com/ZeroPointRepo/zillow-plugin
```

Or register a local clone in `settings.json`:

```json
"chat.pluginLocations": { "/absolute/path/to/zillow-plugin": true }
```

### ChatGPT, Codex, GitHub Copilot, Kiro

This repository root is a conformant **[Agent Plugins 1.0.0](https://agent-plugins.org/specification)** package (`plugin.json`, `skills/`, `mcp.json`). Install it with your client's plugin mechanism pointed at this repository, or at a local clone.

> Agent Plugins 1.0.0 standardizes the *package format*, not installation — each client owns its own install flow.

### Any client

The plugin is just a directory. Clone it wherever your client scans for plugins:

```bash
git clone https://github.com/ZeroPointRepo/zillow-plugin
```

---

## Authentication — OAuth, not keys

The bundled MCP config is exactly this:

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/mcp.schema.json",
  "mcpServers": {
    "zillapi": {
      "type": "streamable-http",
      "url": "https://api.zillapi.com/mcp"
    }
  }
}
```

No token, no env var — Agent Plugins 1.0.0 forbids embedded credentials, and authorization is client-managed by design. The server implements MCP OAuth 2.1 with Dynamic Client Registration: on the first (unauthenticated) request it answers `401` with discovery metadata, your client opens the sign-in flow, and from then on calls run under your Zillapi account. Sign up free at [zillapi.com/signup](https://zillapi.com/signup/) — 100 credits, no card.

---

## Credits & pricing

| Tool | Cost |
|---|---|
| `lookup_property_by_address` | 3 credits |
| `lookup_property_by_zpid` | 1 credit (24 h cache) |
| `get_zestimate` | 1 credit |
| `search_listings` | 1 credit **per listing returned** |

| Plan | Price | Credits | Rate limit | Top-ups |
|---|---|---|---|---|
| Free | $0 | 100 (one-time) | 20/min | not available |
| Monthly | $5/mo | 1,000/month | 200/min | $4 per 1,000 |
| Annual | $54/yr | 12,000 upfront | 300/min | $3 per 1,000 |

Failed calls don't consume credits. The skill and commands are written to keep spend low (zpid over address, tight bounding boxes, confirm before broad searches).

---

## Repo layout

```
zillow-plugin/
├── plugin.json                   # Agent Plugins 1.0.0 manifest  ← the portable one
├── mcp.json                      # Agent Plugins 1.0.0 MCP config (streamable-http, OAuth)
├── skills/zillow/SKILL.md        # when + how to use the tools
├── commands/                     # /zillow:property · zestimate · search · comps
├── assets/                       # logo
│
├── .cursor-plugin/plugin.json    # Cursor plugin manifest (+ marketplace.json)
├── .plugin/plugin.json           # legacy Open Plugins manifest
├── .claude-plugin/plugin.json    # legacy Claude Code manifest (+ marketplace.json)
├── marketplace.json              # marketplace index (single-plugin)
└── .mcp.json                     # legacy MCP config
```

`plugin.json` + `mcp.json` + `skills/` at the repo root are the portable [Agent Plugins 1.0.0](https://agent-plugins.org/specification) package. `.cursor-plugin/` is the [Cursor plugin](https://cursor.com/docs/reference/plugins) format, validated against Cursor's published schemas; it points back at the same root `mcp.json` and `skills/`, so there is one source of truth. The remaining dot-directories are retained so existing installs on older client-specific formats keep working.

Validate the package against the published JSON Schemas at any time:

```bash
curl -sO https://agent-plugins.org/schemas/1.0.0/plugin.schema.json
curl -sO https://agent-plugins.org/schemas/1.0.0/mcp.schema.json
npx ajv-cli@5 validate --spec=draft2020 -s plugin.schema.json -d plugin.json
npx ajv-cli@5 validate --spec=draft2020 -s mcp.schema.json    -d mcp.json
```

## Related

- **REST API + code examples:** [ZeroPointRepo/zillow-api](https://github.com/ZeroPointRepo/zillow-api)
- **Standalone MCP server docs:** [ZeroPointRepo/zillow-mcp](https://github.com/ZeroPointRepo/zillow-mcp)
- **Agent skills (API-key based, for OpenClaw / Hermes):** [ZeroPointRepo/zillow-skills](https://github.com/ZeroPointRepo/zillow-skills)
- **Other plugins on the same open standard:** [ZeroPointRepo/awesome-agent-plugins](https://github.com/ZeroPointRepo/awesome-agent-plugins). Our directory of verified Agent Plugins, this one included.

## Related Zillapi projects

- [zillow-api](https://github.com/ZeroPointRepo/zillow-api): REST API docs and examples
- [zillow-mcp](https://github.com/ZeroPointRepo/zillow-mcp): MCP server for Claude, ChatGPT, Cursor and Claude Code
- [zillow-skills](https://github.com/ZeroPointRepo/zillow-skills): agent skills (one-command install)
- [zillow-plugin](https://github.com/ZeroPointRepo/zillow-plugin): plugin for Claude Code and Cursor
- [n8n-nodes-zillapi](https://github.com/ZeroPointRepo/n8n-nodes-zillapi): n8n community node
- [crewai-zillapi](https://github.com/zillapi/crewai-zillapi): CrewAI tools
- Examples: [zillapi-vercel-ai-sdk-example](https://github.com/ZeroPointRepo/zillapi-vercel-ai-sdk-example) (Vercel AI SDK), [zillapi-openai-agents-example](https://github.com/ZeroPointRepo/zillapi-openai-agents-example) (OpenAI Agents SDK)
- Demos: [property-fair-value](https://github.com/ZeroPointRepo/property-fair-value), [property-scanner](https://github.com/ZeroPointRepo/property-scanner)

Learn more: [Zillow API guide](https://zillapi.com/zillow-api/) · [For AI agents](https://zillapi.com/ai-agents/) · [Pricing](https://zillapi.com/pricing/) · [Zillow API alternatives](https://zillapi.com/alternatives/zillow-api/) · [Zillow's Claude connector vs Zillapi](https://zillapi.com/compare/zillow-claude-connector-vs-zillapi/) · [Zillow API pricing explained](https://zillapi.com/blog/zillow-api-pricing/) · [YouTube](https://www.youtube.com/@zillapi)

## Support

Email [hello@zillapi.com](mailto:hello@zillapi.com) — we answer fast.

## Trademark

Zillapi is an independent service and is not affiliated with, endorsed by, or sponsored by Zillow Group, Inc. "Zillow" and "Zestimate" are registered trademarks of Zillow Group, Inc.
