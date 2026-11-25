---
title: Chelate Shell
description: An orchestrator that turns scattered local agent skills into a deterministic scientific research workflow.
sidebar_label: Chelate Shell
slug: /tools/chelate-shell
---

# Chelate Shell

<div className="repo-meta"><span>Tool</span><span>Python</span><span>v1.4.0</span><span>Agent Orchestrator</span><span>14 Skills</span><span>MIT</span></div>

[**chelate-shell**](https://github.com/JiamingShi-JsPhys/chelate-shell) is a local, deterministic agent-skill orchestrator built by **Jiaming Shi** for scientific research and engineering workflows. 

It doesn't replace individual AI skills; instead, it acts as a **project manager**. It decomposes user intents, enforces a standardized workspace, and routes tasks to the appropriate skills in a strictly deterministic order.

**Why Chelate Shell?**
The agent ecosystem has plenty of excellent isolated skills—validating formulas, tracking videos, or organizing files. However, when facing complex research workflows, agents often struggle with the orchestration gap: **Which skill to call? In what order? Where should the outputs go?** 

Chelate Shell bridges this gap with a microkernel-based router. By reading each skill's `manifest.json`, it automatically builds a standardized pipeline, ensuring your research remains reproducible, auditable, and hallucination-free.

---

> **Notice.** A community plugin by Jiaming Shi, not affiliated with Westlake University. It runs with your own user privileges once installed — review the code before use. See [Security](/legal/security).
>
> 社区插件，与西湖大学无关；安装后以你的用户权限运行，使用前请阅读源码。
