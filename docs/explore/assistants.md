---
title: Assistants
description: Hand this index to your own model — by prompt or through a local MCP server.
sidebar_label: Assistants
slug: /explore/assistants
---

# <Bi en="Assistants" zh="助手与 Agent" />

<Bi en="Two ways to use Westlake Drawer with your own model. Neither needs an account here, and neither sends your data anywhere: the site only supplies the resource list and the rules." zh="有两种方式把 Westlake Drawer 交给你的模型使用。都不需要在本站注册，也不会上传你的数据：站点只提供资源清单与规则。" />

## <Bi en="Copy a prompt" zh="复制提示词" />

<Bi en="Pick a direction and a goal, copy the prompt, and paste it into the assistant you already use. The prompt carries the resource list, an ordering task, and the limits: no explaining the material, no resources outside the list." zh="选好方向与目标，复制提示词，粘贴到你常用的助手。提示词里已经包含资源清单、排序任务和边界：不解释课程内容、不使用清单以外的资源。" />

<PromptBuilder />

## <Bi en="Connect an agent over MCP" zh="通过 MCP 连接 Agent" />

<Bi en="A small local server that gives your agent the index and the curated paths. It answers **where to look**, not what the material says. No API key, no network call, no telemetry." zh="一个本地小服务，把索引与预设路径交给你的 Agent。它回答的是**去哪里找**，而不是材料讲了什么。不需要 API Key、不联网、无遥测。" />

| Tool | Returns |
| :--- | :--- |
| `search_resources` | Notes and tools matching a query: title, URL, subject, course, repository |
| `get_learning_path` | A curated path: ordered steps with a one-line reason |
| `get_resource` | One resource by site path or title |
| `list_majors` | Majors and subjects in the index, with counts |

### <Bi en="Install" zh="安装" />

<Bi en="Needs Node.js 18 or newer; there are no dependencies." zh="需要 Node.js 18 或更高版本；没有任何依赖。" />

<Bi en="Clone the site repository (or download the `mcp/` folder):" zh="克隆站点仓库（或只下载 `mcp/` 目录）：" />

```bash
git clone https://github.com/westlake-drawer/westlake-drawer.github.io.git
cd westlake-drawer.github.io
```

<Bi en="Then register the server with your agent:" zh="然后在你的 Agent 里注册这个服务：" />

<McpSetup />

### <Bi en="Keep the data current" zh="保持数据最新" />

<Bi en="The server reads `mcp/data/`, a snapshot taken at build time:" zh="服务读取 `mcp/data/`，那是构建时生成的快照：" />

```bash
npm install
npm run mcp:sync
```

### <Bi en="Credit" zh="署名" />

<Bi en="MCP server and prompt design:" zh="MCP 服务与提示词设计：" /> [JiamingShi-JsPhys](https://github.com/JiamingShi-JsPhys).

### <Bi en="Privacy" zh="隐私" />

<Bi en="It reads only `mcp/data/search-index.json` and `mcp/data/paths.json` — public site metadata. It writes nothing, sends nothing, and keeps no logs." zh="它只读取 `mcp/data/search-index.json` 与 `mcp/data/paths.json` —— 都是站内公开元数据。不写文件、不发送数据、不保留日志。" />
