---
title: Agentic AI 教師學習路線（awesome-agentic-ai-zh）
date: 2026-06-25
tags: [ai/agent]
source: https://github.com/WenyuChiou/awesome-agentic-ai-zh/blob/main/branches/for-teacher.md
type: reference
status: processed
---

教師用 AI 的學習路線，摘自 WenyuChiou/awesome-agentic-ai-zh 三語學習地圖。多數教師 Tier 0-1（Claude.ai + NotebookLM）就夠，Tier 2+ 才需 Claude Code。

## 三大使用情境

1. **備課與素材製作**：教案生成、Quiz/rubric 建立、多媒體素材、學生回饋整理
2. **教學現場輔助**：沉浸式情境演練、蘇格拉底式追問、即時批改、適性教學
3. **其他場景**：行政管理、親師溝通、特殊教育支援、隱私保護

## 3 個可直接複製的 Prompt 範本

詳見原始來源。核心是教案大綱生成、Rubric 草稿生成、學生回饋整理三個標準 Prompt。

## 隱私倫理硬性規則

- **不要把學生個資丟進公開 LLM**（先匿名化）
- **AI 輔助 ≠ AI 評分**，最終評分一定人工把關
- 學生作品著作權：在台灣需注意《個資法》與校方政策

## 層級建議

| Tier | 工具 |
|------|------|
| Tier 0 | Claude.ai 網頁版 |
| Tier 1 | Claude Desktop + NotebookLM |
| Tier 2+ | Claude Code（批次自動化需求） |

相關：[[agentic-ai-researcher-roadmap]]
