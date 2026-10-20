---
title: Prompt 寫再長也不會變成可靠工作流：企業 AI 系統的四個層次
date: 2026-08-05
tags: [ai/agent, ai/prompt-engineering, dev/architecture]
source: https://ithelp.ithome.com.tw（《從零開始打造企業 AI 知識工作流》Day 04）
type: article
status: processed
asset_value: 72
---

## T（核心論點）

把「先查 Google Sheets、找不到再查 Confluence、必須附來源、資料不足要說明不可推測」這種完整流程寫進 Prompt，看似合理卻暗藏根本問題：

> **Prompt 描述的是「期待的行為」，而不是「被保證的行為」。**
> 大型語言模型是機率性的系統，同一段指令在不同問題與上下文中，可能產生不同的執行結果。

可靠的企業 AI 系統要分四層：**Prompt（描述期待）／Policy（定義不能違反的邊界）／Tool（提供真正的執行能力）／Workflow（控制整個執行流程）**——寫再長的 Prompt 都無法取代後三層。

## B（適用場景）

**適用：** 設計企業內部 AI 知識工作流；判斷某條需求該寫進 Prompt 還是該做成 Policy/Workflow；審視「為什麼它有時照做有時不照做」

**不適用 / 注意：** 這是架構分層的概念框架，不含具體實作技術選型

## R（四個層次）

| 層 | 定位 | 範例 |
|---|---|---|
| **1. Prompt** | **描述期待**——可以要求，但不保證 | 用繁體中文回答／先結論再分析／避免過度技術化／整理成表格／資料不足要主動說明限制 |
| **2. Policy** | **定義不能違反的邊界** | 🔴 沒有工具查詢結果時**不得**提供具體營運數字／使用者沒有權限時**不得**讀取敏感文件／高風險操作**必須**經人工確認／資料來源**必須**可追溯／找不到資料時**不得**自行補上推測答案 |
| **3. Tool** | **提供真正的執行能力** | Google Sheets、Confluence、Trello/Jira、SQL 資料庫、搜尋引擎、Python 分析環境、Email 與行事曆、企業內部 API |
| **4. Workflow** | **控制整個執行流程** | 哪一步先執行／什麼條件下呼叫哪個工具／工具失敗是否重試／何時改用其他來源／什麼情況可以產生最終答案／什麼情況必須停止並回報錯誤 |

### 🔑 為什麼這個分層重要

四層的差別**不是強度，是性質**：

- Prompt 層的規則靠模型**願意**遵守（機率性）
- Policy 層的規則靠系統**強制**執行（確定性）

把「不得提供未經查詢的數字」寫在 Prompt 裡，它大部分時候會照做——但「大部分時候」在企業場景不夠。這類規則必須下沉到 Policy 層，由系統在模型輸出後（或工具呼叫前）攔下。

同理，「先查 A 找不到再查 B」這種**條件分支**寫在 Prompt 裡是期待，寫在 Workflow 裡才是保證。

## C（關鍵行動）

- [ ] 🔴 逐條檢視現有 Prompt：**哪些其實是「不可違反的邊界」？那些該搬到 Policy 層**
- [ ] 條件分支與重試邏輯（先查 A 再查 B、失敗重試幾次）從 Prompt 移到 Workflow 層
- [ ] 「必須附來源」這類要求要有機制驗證，不能只寫在 Prompt 裡當期待
- [ ] 高風險操作一律走 Policy 層的人工確認，不靠 Prompt 提醒
- [ ] Prompt 層只留真正屬於「表達偏好」的內容（語言、格式、詳略）

## 相關連結
- [[synthesis/synthesis-ai-dev-workflow-frameworks]] / [[synthesis/synthesis-spec-driven-workflow]] — **本篇為兩份精粹的來源**（2026-09-03 重跑）：🔑「Prompt 描述期待不是保證」是「架構才是瓶頸」與「規格必須接上機器可判定驗證」兩個結論的共同機制依據
- [[synthesis-claude-code-internals]] — **強制力光譜**（CLAUDE.md 可忽略 → Skill 被說服 → Plan Mode 硬保證 → Hook mandatory）與本篇的 Prompt/Policy 分界是同一命題；本篇是企業系統版，該精粹是單一工具版
- [[obligations-as-code-tedium-is-stability]] — 「每條義務接上機器可判定的檢查」＝本篇 Policy 層的落地方法
- [[agent-tool-runtime-seven-layers]] — 本篇 Policy 層的 Permission Gate 在該篇是第 3 層；Tool 層的實際執行對應其七層責任
- [[claude-code-hooks-memory]] — CLAUDE.md 是 advisory、Hook 才是 mandatory；同一分界在 Claude Code 的具體對應
- [[synthesis-spec-driven-workflow]] — 規格驅動精粹；本篇「Prompt 描述期待而非保證」與其「錨點必須接上可判定驗證」同源
- [[enterprise-rag-smb-architecture]] — 企業 RAG 架構；本篇的 Tool 層是其資料接入面
- [[enterprise-ai-five-stage-evolution]] — **同系列 Day 05**：五段演進路徑；🔑 其「問題二：模型說正在查詢卻沒真的查」是本篇「關鍵行為必須由 Workflow 保證」的實際故障案例
- [[multimodal-document-processing-three-layers]] — **同系列 Day 9**：文件處理三層策略（原生文字／OCR／多模態）
