---
title: MCP
description: Use Westlake Drawer's index from your own agent through a local MCP server.
sidebar_label: MCP
slug: /explore/mcp
---

# MCP server

A small local server that gives your agent the index and the learning paths. It answers
**where to look**, not what the material says. No API key, no network call, no telemetry.

## What it provides

| Tool | Returns |
| :--- | :--- |
| `search_resources` | Notes and tools matching a query: title, URL, subject, course, repository |
| `get_learning_path` | A curated path: ordered steps with a one-line reason |
| `get_resource` | One resource by site path or title |
| `list_majors` | Majors and subjects in the index, with counts |

## Install

Needs Node.js 18 or newer; there are no dependencies.

1. Clone the site repository (or download the `mcp/` folder):

```bash
git clone https://github.com/westlake-drawer/westlake-drawer.github.io.git
cd westlake-drawer.github.io
```

2. Register it with your client. Claude Code:

```bash
claude mcp add westlake-drawer -- node "$(pwd)/mcp/index.js"
```

Cursor or another client using JSON config:

```json
{
  "mcpServers": {
    "westlake-drawer": {
      "command": "node",
      "args": ["/absolute/path/to/mcp/index.js"]
    }
  }
}
```

3. Ask your agent to use it, for example: *"Use westlake-drawer to find notes for a maths
foundation and give me a path."*

## Keep the data current

The server reads `mcp/data/`, a snapshot taken at build time. Refresh it after the index changes:

```bash
npm install
npm run mcp:sync
```

## Privacy

It reads only `mcp/data/search-index.json` and `mcp/data/paths.json` — public site metadata.
It writes nothing, sends nothing, and keeps no logs. Your model stays on your side.

Source and full notes: [mcp/README.md](https://github.com/westlake-drawer/westlake-drawer.github.io/tree/main/mcp).
