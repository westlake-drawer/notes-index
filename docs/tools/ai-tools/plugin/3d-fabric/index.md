---
title: 3D Fabric
description: A print-quality gate for 3D modelling and manufacturing workflows, with 30 Blender skills included.
sidebar_label: 3D Fabric
slug: /tools/3d-fabric
---

# 3D Fabric

<ResourceFacts />

[**3d-fabric**](https://github.com/JiamingShi-JsPhys/3d-fabric) is a print-quality gate for 3D modelling and manufacturing workflows. Every path a mesh takes — a request written in natural language and built through the local SolidCraft CLI, a design someone sent you, an STL from a scanner, a mesh out of Blender — ends at the same gate: watertightness, winding, non-manifold edges, and each closed shell checked on its own, so an inside-out part cannot hide behind a correct one.

Seven CAD actions write the same gate record and use the same exit codes. Thirty Blender skills cover the 3D work that never has a dimensioned part. Agent hosts get a typed gate for every action, `--json` output byte-identical to the file on disk, and an explicit `UNVERIFIED` when SolidCraft is missing instead of a silent pass.

---

> **Notice.** A community plugin by Jiaming Shi, not affiliated with the SolidCraft project or Westlake University. It runs with your own user privileges once installed — review the code before use. See [Security](/legal/security).
>
> 社区插件，与 SolidCraft 项目及西湖大学无关；安装后以你的用户权限运行，使用前请阅读源码。
