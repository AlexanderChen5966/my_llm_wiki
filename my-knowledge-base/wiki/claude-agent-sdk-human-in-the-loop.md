---
title: 【學習筆記】Claude Agent SDK：概觀與 Human-in-the-loop 機制
date: 2026-07-16
tags: [ai/agent, ai/claude]
source: https://hackmd.io/@Heidi-Liu/claude-agent-sdk
type: note
status: processed
asset_value: 62
---

調查 Claude Agent SDK 是否支援 Human-in-the-loop（人機協作）與不同工作階段 Agent workflow 的學習筆記，聚焦於 `canUseTool` callback 機制的深入解析。

## T（核心論點）

Claude Agent SDK 中的 Agent 控制分成兩層：Capability Control（systemPrompt + tools，決定 Agent 能做什麼）與 Decision Control（Human-in-the-loop，決定 Agent 何時需要人類介入）。所有人機互動——無論是工具審核還是 Agent 主動提問（AskUserQuestion）——都通過**同一個** `canUseTool` callback 處理，不需要分別實作機制；`permissionMode` 是內建的預設策略，`canUseTool` 則提供更細緻的自訂控制，兩者可疊加使用（SDK 先套用 permissionMode，再讓 canUseTool 做最終決策）。

## B（適用場景）

**適用：** 在 Web App / API Server 中嵌入 Claude Agent 能力，需要自訂審核 UI 或更細緻工具權限控制的場景。
**不適用 / 注意：** CLI 互動式開發或 CI/CD 管線不需要自訂 canUseTool（用 permissionMode 或 SDK 預設行為即可）。

## R（HITL 兩種觸發情境）

1. **工具審核（Tool Approval）**：Agent 要用工具（如寫入檔案）時，SDK 自動呼叫 `canUseTool(toolName, input)`，應用程式顯示審核 UI，使用者選擇 Approve/Deny/Modify input，回傳 `{behavior: "allow"|"deny", updatedInput?, message?}`
2. **AskUserQuestion（Agent 主動提問）**：Agent 判斷資訊不足時呼叫 `AskUserQuestion` 工具，同樣觸發 `canUseTool`，但角色不同——此時應用程式需顯示問題給使用者、收集答案後回傳給 Agent（如「你想用哪個測試框架？」附選項清單）

**permissionMode 四種值**：`"default"`（標準需審核）、`"plan"`（唯讀工具自動通過）、`"acceptEdits"`（自動接受檔案編輯）、`"bypassPermissions"`（全部放行，謹慎使用）。

**Web App 典型架構**：前端 React UI 透過 SSE 接收 Agent 串流回覆，透過 REST POST 回傳使用者答案（雙通道，因 SSE 為單向通信）；當 `canUseTool` 觸發需要使用者回覆的情境時，系統暫停 Agent 執行等待輸入。

## C（關鍵行動）

- [ ] 需要自訂審核 UI 或攔截特定工具（如 Bash 高風險指令）時，實作 `canUseTool` 而非只依賴 `permissionMode`
- [ ] `AskUserQuestion` 與一般工具審核共用同一個 callback，實作時需先判斷 `toolName` 再分流處理邏輯
- [ ] Web 應用中設計 SSE + REST 雙通道架構處理 Agent 串流回覆與使用者互動回傳

## 相關連結

- [[claude-code-agent-sdk-complete-guide]] — 同主題互補篇：涵蓋 SDK 4 大核心功能與 5 個完整實戰範例（CI/CD/多 Agent/生產部署），本篇則聚焦 HITL/canUseTool 單一機制的深入解析
- [[claude-code-tools]] — Claude Code 內建 Tools 使用邏輯，可對照 SDK 版工具存取控制
