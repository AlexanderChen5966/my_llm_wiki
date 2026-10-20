---
title: AI 使用五層級：從聊天到 Agent 自動化
date: 2026-06-24
tags: [ai/agent, pkm/workflow]
source: https://codelove.tw/@tony/post/3Kp5Ya
type: note
status: processed
asset_value: 45
---

多數人對 AI 工具的使用停在前兩層（聊天/生成文件），但真正的生產力提升發生在第 3-5 層：透過設定檔、工具整合、自動化讓 AI 主動驅動工作流，而非被動回答問題。

## T（核心論點）

AI 使用方式有五個層級：

| 層級 | 名稱 | 做的事 | 代表工具/做法 |
|------|------|--------|--------------|
| **1** | 聊天層 | 一問一答，人主動 prompt | ChatGPT 對話 |
| **2** | 檔案生成層 | AI 輸出文件/程式碼/報告 | 指令式生成 |
| **3** | 設定檔層 | 透過 CLAUDE.md / system prompt 讓 AI 遵守規則 | `CLAUDE.md`、`AGENTS.md` |
| **4** | MCP + Skills 層 | AI 能呼叫外部工具、執行結構化工作流 | MCP Servers、`SKILL.md` |
| **5** | Agent 自動化層 | AI 自主完成多步驟任務，不需要人工介入每一步 | Loop Engineering、Telegram Connector |

## B（適用場景）

大多數個人生產力需求在第 3-4 層即可滿足。第 5 層（完全自動化）適合高重複性、有明確驗收條件的任務（參考 Loop Engineering 三關過濾）。

個人知識庫目前在第 3-4 層（`CLAUDE.md` + Skills），已具備相當的自動化能力。

## R（升到第 5 層的路徑）

從第 4 層升至第 5 層的可能做法：
- **Telegram Connector**：文章轉發 → 自動觸發 `knowledge-organizer` Skill
- **排程**：每週固定 `/schedule raw-pipeline` 批次處理
- **事件觸發**：GitHub Action 或 Cron 驅動 Loop

關鍵前提：任務必須可重複 + 有客觀驗收條件（否則第 5 層帶來的是失控而非省力）。

## C（關鍵行動）

- [ ] 評估知識庫 raw-pipeline 是否具備「第 5 層化」的條件（重複/可驗收/有價值）
- [ ] 若要升至第 5 層：先裝三道護欄（次數上限/diff/成本），再去掉手動觸發點

## 相關連結

- [[claude-cowork-setup-guide]] — Cowork 設定（第 5 層的實作基礎）
- [[loop-engineering-6-components]] — Loop Engineering 框架（第 5 層的設計方法論）
- [[concepts/agent-workflow-taxonomy]] — 六大 Agent 類型（第 3 層=Skills、第 4 層=MCP、第 5 層=Loop/自動化編排）
- [[ai-adoption-five-stages-boris-cherny]] — 組織「採用」5 階地圖（本篇是個人「使用」五層；兩張互補的地圖，對照參看）
