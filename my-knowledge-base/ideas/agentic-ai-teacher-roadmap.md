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

## 補充資料

**2026-11-19 補充：來源頁面改版（2026-09-03 剪存版；https://github.com/WenyuChiou/awesome-agentic-ai-zh/blob/main/branches/for-teacher.md）**

新版整頁重寫，定位改為「**AI 先做草稿，教師負責決定能不能用**」；起點從「Claude.ai＋NotebookLM 免 setup」改為「只需一個**學校核准的 AI 工具**」。上方的 Tier 表、3 個 prompt 範本、三大情境細項為 2026-06 舊版內容，新版已改動如下：

| 項目 | 舊版（2026-06） | 新版（2026-09） |
|---|---|---|
| 層級建議 | Tier 0／1／2+ 表 | 移除；改為「只做一次備課停在網頁工具、每週重跑才走 Track A 並先問校方 IT」 |
| Prompt 範本 | 教案大綱／Rubric／學生回饋整理（貼學生作業片段） | 第一個練習（虛構的國小影子活動，含 Learning Objective、兩層 Scaffolding、Formative Assessment、Exit Ticket）＋Rubric 草稿＋常見錯誤整理；全用虛構內容，不推測學生身分、能力、健康或特教需求 |
| 工具／資源 | superpowers、Claude Code、hf agents-course、llm-universe、awesome-chatgpt-prompts | 新增 Claude for Teachers、ChatGPT for Teachers（皆限通過驗證的美國 K-12）、Gemini Notebook（原 NotebookLM）；llm-universe 換 hello-agents（CC BY-NC-SA 4.0）；新增 microsoft/ai-agents-for-beginners、anthropics/skills（逐資料夾授權）；prompt 庫改為 f/prompts.chat |
| 必修閱讀 | Effortless Academic | UNESCO GenAI 教育指引、歐盟執委會教師倫理指引、TeachAI School Toolkit（先讀自己學校政策） |
| 使用情境 | 三分支細項＋Chen 2020／Mittal 2024 文獻 | 精簡為備課與教材／課堂與學習支援／行政與溝通；新增「不要讓 AI 假裝診斷學生」 |

- **新增五條安全線**：先看校方政策 → 不放學生資料（練習用虛構內容）→ 教師保留決定權（成績、紀律、升學、特教由合格人員決定）→ 每次 Human Review（事實、引用、偏見、年齡適切、無障礙、課程目標）→ 向學生說清楚可用範圍與揭露方式。
- **新增八個核心詞**：Learning Objective、Scaffolding、Rubric、Formative Assessment、AI Literacy、Student Data、Human Review、Academic Integrity。
- **進階自動化要可中斷**：`選取資料 → 移除不必要資訊 → AI 草擬 → 人工檢查 → 批准 → 發布`；課程 Agent 不要一開始就給 LMS 寫入權限；工具不支援所在地區時不繞過限制。
- **法規說法改寫**：舊版直接點名 FERPA／GDPR／《個資法》要注意；新版改為「是否適用由校方與合格人員判斷，本頁不是法律意見」。

相關：[[agentic-ai-researcher-roadmap]]
