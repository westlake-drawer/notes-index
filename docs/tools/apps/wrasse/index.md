---
title: wrasse
description: Deterministic, offline security audit for AI-written code — no model, no network, no API key.
sidebar_label: wrasse
slug: /tools/wrasse
---

# wrasse

<ResourceFacts />

[wrasse](https://github.com/JiamingShi-JsPhys/wrasse) audits the code an AI wrote, and every
finding comes with the exact line to replace. The rules are deterministic: no model, no
network by default, no API key.

It answers two questions. **Will this code leak something?** Hard-coded keys, secrets in
`NEXT_PUBLIC_*` variables, tokens in logs, stack traces returned to clients, upload paths
built from filenames. **What did the AI leave out?** Packages that do not exist, routes with
no authentication, queries with no ownership check, debug mode, wildcard CORS, unsafe
deserialisation, SQL built from values, and checks that only run inside one role branch.

---

> **Notice.** A community app by Jiaming Shi, not affiliated with any model provider or
> Westlake University. It runs with your own user privileges once installed — review the
> code before use. See [Security](/legal/security).
