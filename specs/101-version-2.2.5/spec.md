# 101 — 版本更新：2.2.5 功能规格（website）

> **Module:** release/docs ｜ **Status:** Implementing ｜ **Last Updated:** 2026-10-31
> **需求编号:** 101-version-2.2.5 ｜ **需求名称:** 版本更新：2.2.5
> **git 分支:** 不改动（沿用 `main`）
> **前置依赖:** hos / ark / android 三端 v2.2.5 构建（与测试状态）均完成后再更新页面

## 1. 目的

三端统一基线推进到 opencode **v2.2.5**（`9bbb8ce18c`）后，把产品官网静态页
（`index.html` 英文、`index_zh.html` 中文、`README.md`、`script.js` 如涉及）的版本信息
与状态口径更新到 2.2.5 轮事实，提交并在本地打 tag `v2.2.5`（不 push，push 由用户执行）。

## 2. 范围

**In scope**

- 将页面中所有 `opencode v2.1.2` / `v2.1.2` 版本锚点更新为 **v2.2.5**（hero、各 edition 卡片、baseline 行等）。
- 更新各版「状态」措辞，与本轮事实严格一致：
  - hos（Electron on HarmonyOS）：**v2.2.5 构建链完成；无鸿蒙真机，真机验证待设备**（不得写"真机验证通过"）。
  - ark（HarmonyOS）：同上（构建链完成、真机待设备；维持无 Web UI 品牌化等既有事实）。
  - android：**v2.2.5 构建链完成；真机测试结果以 `TEST_REPORT.md` 为准**（待真机轮结束后回填结论）。
- 补丁条数/工具链若页面有述及，按各端 `patches/v2.2.5/README.md` 口径核对（ark 5 / hos 8 / android 7 不变）。
- README 内容约束（已有"不得夸写真机状态"纪律）继续遵守。
- 提交（Conventional Commits `docs:`）+ 本地 tag v2.2.5，不 push。

**Out of scope**

- 不做视觉重设计（仅版本/状态信息更新）。
- 不改部署（GitHub Pages 由用户 push 后发布）。
- 不收录任何测试 Key / URL 内嵌凭证 / 账号密码。

## 3. 需求条目

- **FR-web-101**（MUST）：全站无 `v2.1.2` 版本锚点残留（历史/changelog 语境除外，须可解释）。
- **FR-web-102**（MUST）：三端状态与各端 TEST_REPORT 事实一致，不夸写真机结论。
- **FR-web-103**（MUST）：中英双页同步、措辞一致。
- **FR-web-104**（MUST）：本地静态渲染无误（可 file:// 或简单 http server 打开核对）。
- **FR-web-105**（MUST）：敏感信息零命中。
- **FR-web-106**（SHOULD）：`docs:` 前缀提交 + 本地 tag v2.2.5，不 push。

## 4. 验收标准

| # | 标准 | 证据 |
|---|---|---|
| AC-01 | 无旧版本锚点 | grep 记录 |
| AC-02 | 状态口径真实一致 | 与三端 TEST_REPORT 对照 |
| AC-03 | 中英一致 | 双页 diff 对照 |
| AC-04 | 页面可正常打开 | 本地打开/截图 |
| AC-05 | 敏感信息零命中 | 扫描记录 |
| AC-06 | docs 提交 + 本地 tag | git log/tag |

## 5. 敏感信息纪律

不收录 API Key、测试 URL 内嵌凭证、账号密码；截图/示例一律占位。
