# 101 — 版本更新：2.2.5 技术方案（website）

> **回引:** `spec.md` ｜ **最后更新:** 2026-10-31 ｜ **日志:** `logs/20261031-1/`
> **前置:** hos/ark/android 的 v2.2.5 构建与（android）真机测试结论已出。

## 1. 决策

| # | 决策 | 依据 |
|---|---|---|
| D1 | 仅改版本/状态信息，不做视觉重设计 | 需求范围 |
| D2 | 真机状态如实写：hos/ark「构建完成、真机待设备」；android 以 TEST_REPORT 回填 | 无鸿蒙真机约束 |
| D3 | 补丁条数口径不变（ark 5 / hos 8 / android 7），仅基线版本号改 v2.2.5 | 三端 patches 目录 |
| D4 | 中英双页同步；README 约束沿用 | FR-web-103 |
| D5 | `docs:` 前缀提交；本地 tag v2.2.5；不 push | 用户 |

## 2. 步骤

| 步 | 动作 | 出口判据 |
|---|---|---|
| S1 | grep 全量定位 v2.1.2 / 旧状态措辞 | 清单完整 |
| S2 | 更新 index.html / index_zh.html / README.md | AC-01/02/03 |
| S3 | 本地打开核对渲染（http.server / file://） | AC-04 |
| S4 | 敏感信息扫描 | AC-05 |
| S5 | 代码评审（随父流程） | REVIEW_REPORT |
| S6 | docs 提交 + 本地 tag v2.2.5（不 push） | AC-06 |

## 3. 改动点（初步定位，构建完成后复核）

- `index.html` / `index_zh.html`：hero baseline（约 105 行）、三卡片 baseline（约 182/203/224 行）、
  hos 状态行（约 208 行）、section lede（约 276 行）。
- `README.md`：统一基线段（约 38 行）、状态纪律段（约 82 行）、可信行口径（约 94 行）。

## 4. 风险

| # | 风险 | 缓解 |
|---|---|---|
| R1 | 真机状态夸大 | 以三端 TEST_REPORT 为准，措辞评审 |
| R2 | 中英不一致 | 双页同条改、对照 |
| R3 | 漏改锚点 | 提交前全量 grep |
