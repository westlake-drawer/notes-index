---
title: Assistants
description: Hand this index to your own model — by prompt or through a local MCP server.
sidebar_label: Assistants
slug: /explore/assistants
---

# Assistants

Two ways to use Westlake Drawer with your own model. Neither needs an account here, and
neither sends your data anywhere: the site only supplies the resource list and the rules.

## Copy a prompt

Pick a direction and a goal, copy the prompt, and paste it into the assistant you already use.
The prompt carries the resource list, an ordering task, and the limits: no explaining the
material, no resources outside the list.

<PromptBuilder />

## Connect an agent over MCP

A small local server that gives your agent the index and the curated paths. It answers
**where to look**, not what the material says. No API key, no network call, no telemetry.

| Tool | Returns |
| :--- | :--- |
| `search_resources` | Notes and tools matching a query: title, URL, subject, course, repository |
| `get_learning_path` | A curated path: ordered steps with a one-line reason |
| `get_resource` | One resource by site path or title |
| `list_majors` | Majors and subjects in the index, with counts |

### Install

Needs Node.js 18 or newer; there are no dependencies.

Clone the site repository (or download the `mcp/` folder):

```bash
git clone https://github.com/westlake-drawer/westlake-drawer.github.io.git
cd westlake-drawer.github.io
```

Then register the server with your agent:

<McpSetup />

### Keep the data current

The server reads `mcp/data/`, a snapshot taken at build time:

```bash
npm install
npm run mcp:sync
```

### Credit

MCP server and prompt design: [JiamingShi-JsPhys](https://github.com/JiamingShi-JsPhys).

### Privacy

It reads only `mcp/data/search-index.json` and `mcp/data/paths.json` — public site metadata.
It writes nothing, sends nothing, and keeps no logs.
Source: [mcp/README.md](https://github.com/westlake-drawer/westlake-drawer.github.io/tree/main/mcp).
