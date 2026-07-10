<p align="center">
  <a href="https://zillapi.com">
    <img src="assets/logo-512.png" width="160" height="160" alt="Zillapi" />
  </a>
</p>

<h1 align="center">Zillow Plugin — Live Property Data for AI Coding Agents</h1>

<p align="center">
  <b>Zestimates, full property records, and listing search on 160M+ U.S. homes — inside Cursor, Claude Code, and any Open Plugins-compatible agent.</b><br/>
  One install. OAuth sign-in. No API key to manage. Free tier, no card.
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

First property question you ask, Cursor prompts a Zillapi OAuth sign-in (create a free account if you don't have one — 100 credits, no card). Done.

### Claude Code

```bash
/plugin marketplace add ZeroPointRepo/zillow-plugin
/plugin install zillow@zillapi
```

### Any Open Plugins-compatible agent

This repo follows the [Open Plugins](https://open-plugins.com) standard (`.plugin/plugin.json`, `skills/`, `commands/`, `.mcp.json`) — install it with your tool's plugin mechanism, pointed at this repository.

---

## Authentication — OAuth, not keys

The bundled MCP config is exactly this:

```json
{
  "mcpServers": {
    "zillapi": {
      "url": "https://api.zillapi.com/mcp"
    }
  }
}
```

No token, no env var. The server implements MCP OAuth 2.1 with Dynamic Client Registration: on the first (unauthenticated) request it answers `401` with discovery metadata, your client opens the sign-in flow, and from then on calls run under your Zillapi account. Sign up free at [zillapi.com/signup](https://zillapi.com/signup) — 100 credits, no card.

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
├── .cursor-plugin/plugin.json    # Cursor manifest
├── .plugin/plugin.json           # Open Plugins (vendor-neutral) manifest
├── .claude-plugin/plugin.json    # Claude Code manifest (+ marketplace.json)
├── marketplace.json              # marketplace index (single-plugin)
├── mcp.json / .mcp.json          # hosted MCP server, OAuth (no keys)
├── skills/zillow/SKILL.md        # when + how to use the tools
├── commands/                     # /zillow:property · zestimate · search · comps
└── assets/                       # logo
```

The three manifests are identical — one per tool-specific discovery path.

## Related

- **REST API + code examples:** [ZeroPointRepo/zillow-api](https://github.com/ZeroPointRepo/zillow-api)
- **Standalone MCP server docs:** [ZeroPointRepo/zillow-mcp](https://github.com/ZeroPointRepo/zillow-mcp)
- **Agent skills (API-key based, for OpenClaw / Hermes):** [ZeroPointRepo/zillow-skills](https://github.com/ZeroPointRepo/zillow-skills)

## Support

Email [hello@zillapi.com](mailto:hello@zillapi.com) — we answer fast.

## Trademark

Zillapi is an independent service and is not affiliated with, endorsed by, or sponsored by Zillow Group, Inc. "Zillow" and "Zestimate" are registered trademarks of Zillow Group, Inc.
