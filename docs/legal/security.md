---
title: Security · 安全须知与事故响应
description: 运行工具的风险、密钥处理、漏洞披露与事故响应。
sidebar_label: Security
slug: /legal/security
---



<Tabs>
<TabItem value="zh" label="中文" default>

**本站是静态站点。** 只提供文本与图片，不代替你执行代码、不设置分析 Cookie、
不索取账号凭据。

**工具是第三方软件。** [工具](/tools) 下的每个插件与应用存放在他人仓库中，安装后以你的
用户权限运行。被列出不等于经过审计。安装前请阅读源码、查看 `SECURITY.md` 与发布记录，
并优先放在虚拟环境、容器或独立账户中运行。

**密钥。** 任何插件都不需要你的账号密码。API Key 放在环境变量或系统凭据存储，
不要写进会提交的文件。

**组织侧的安全措施。** 组织要求 owner 启用双因素认证；仓库默认最小权限；
关键分支建议开启保护与必需审核；发布流程使用最小权限的 GitHub Actions。

**漏洞披露。** 请私下报告：在受影响的仓库使用 GitHub 的 *Report a vulnerability*，
或通过组织联系维护者。我们会在 72 小时内确认收到，并在修复后公开说明（不披露复现细节）。

**事故响应。** 发现账号被盗、恶意提交或内容被篡改时：立即移除受影响内容或撤销权限；
由维护者多数确认处置方案；在 [community](https://github.com/westlake-drawer/community)
公告影响范围与修复措施；涉及个人信息的按[隐私政策](/legal/privacy)处理；
保存事件记录 24 个月。

**备份。** 依赖 GitHub 的仓库历史；组织不另行维护离线副本，重要内容建议贡献者本地保留。

</TabItem>
<TabItem value="en" label="English">

**The site is static.** Text and images only; it runs no code for you, sets no
analytics cookies, and asks for no credentials.

**Tools are third-party software.** Everything under [Tools](/tools) lives in someone
else's repository and runs with your privileges. Listing is not an audit. Read the
source, check `SECURITY.md` and release history, and prefer a virtual environment,
container, or separate account.

**Secrets.** No plugin needs your password. Keep API keys in environment variables or
the system credential store, never in a committed file.

**Organization-side measures.** Owners are required to enable two-factor
authentication; repositories use least privilege; critical branches should be
protected with required review; deployment uses minimal-permission GitHub Actions.

**Vulnerability disclosure.** Report privately via GitHub's *Report a vulnerability*
on the affected repository, or contact a maintainer through the organization. We
acknowledge within 72 hours and publish a summary after the fix, without reproduction
details.

**Incident response.** On a compromised account, malicious commit, or tampered content:
remove the content or revoke access immediately; confirm the response by maintainer
majority; announce scope and remediation in
[community](https://github.com/westlake-drawer/community); handle personal data under
the [privacy policy](/legal/privacy); keep an incident record for 24 months.

**Backups.** Repository history on GitHub is the backup; the organization keeps no
separate offline copy, so contributors should keep local copies of important work.

</TabItem>
</Tabs>
