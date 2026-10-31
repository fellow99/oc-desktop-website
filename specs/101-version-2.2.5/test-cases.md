# 101 — 版本更新：2.2.5 测试用例（website）

> **回引:** `spec.md` / `plan.md` ｜ **最后更新:** 2026-10-31

| 用例 | 关联 | 步骤 | 期望 | 结果 |
|---|---|---|---|---|
| TC-01 无旧版本锚点 | AC-01 | `grep -rn "v2\.1\.2\|2\.1\.2" index.html index_zh.html README.md` | 仅历史/changelog 可解释残留，基线表述全部 v2.2.5 | ⬜ |
| TC-02 状态真实 | AC-02 | 对照三端 TEST_REPORT 审读三卡措辞 | hos/ark=构建完成待真机；android=报告口径；无夸大 | ⬜ |
| TC-03 中英一致 | AC-03 | 双页同段对照 | 含义/版本/状态一致 | ⬜ |
| TC-04 渲染正常 | AC-04 | 本地 http.server/file:// 打开中英两页 | 无布局破坏、版本显示正确 | ⬜ |
| TC-05 敏感信息 | AC-05 | grep Key 前缀/凭证/URL 内嵌账密 | 零命中 | ⬜ |
| TC-06 提交与 tag | AC-06 | git log / git tag | docs 前缀；本地 v2.2.5；未 push | ⬜ |
