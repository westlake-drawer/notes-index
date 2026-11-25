---
title: DSH Token Steward
description: Model-matrix router for DeepSeek Harness — one OpenAI-compatible gateway, automatic routing and failover.
sidebar_label: DSH Token Steward
slug: /tools/dsh-token-steward
---

# DSH Token Steward

<div className="repo-meta"><span>Plugin</span><span>TypeScript</span><span>v1.2.2</span><span>LLM Routing</span><span>MIT</span></div>

[**dsh-token-steward**](https://github.com/JiamingShi-JsPhys/dsh-token-steward) is a model-matrix router for DeepSeek Harness. Official APIs, free mirrors, self-hosted relays and expensive top-tier models can sit in one pool: the plugin runs an OpenAI-compatible gateway on `127.0.0.1`, and pointing `llm-pi-ai`'s `baseURL` at it is the only change a session needs.

Routing, downgrade, health breakers, exact caching and metering all happen inside the gateway. The matrix, provider keys and per-model pricing are maintained from the DSH settings panel, or changed through conversation.

---

> **Notice.** A community plugin by Jiaming Shi, not affiliated with DeepSeek, DeepSeek Harness, or Westlake University. It runs with your own user privileges once installed — review the code before use. See [Security](/legal/security).
