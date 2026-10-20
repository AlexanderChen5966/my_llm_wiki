---
title: Chat On Steroids：讓 ChatGPT 透過本機 MCP 變身 Coding Agent（不耗 Codex 額度）
date: 2026-09-13
tags: [ai/mcp, ai/agent]
source: https://tools.wingzero.tw/articles/4065
type: note
status: processed
asset_value: 48
---

Chat On Steroids（CoS）是開源 Local MCP 工具，讓 ChatGPT 對話直接取得本機專案讀寫、Terminal 執行、桌面控制（滑鼠/鍵盤/剪貼簿）能力，形成「ChatGPT → MCP → 本機專案 → 修改/測試 → 修正」的完整 Coding Agent 迴圈。核心可轉用判準：把部分原本交給 Codex 的工作（讀專案、改程式、跑測試）分流給 ChatGPT + MCP 處理，因為 CoS 消耗的是 ChatGPT Conversation 本身用量而非 Codex quota——但仍受 ChatGPT 帳號的 Model Availability/Usage/Context Limits 限制，並非「AI Token 變免費」。支援 Multi-Agent Workers（不同工作分派給不同 Worker 平行處理，各自保留獨立 Context）與 Compact & Resume（長任務可將狀態帶到新 Chat 繼續）。安裝流程：安裝 CoS → 授權 Project Folder → ChatGPT Developer Mode 接上 Core MCP → 載入 Chrome Companion Extension；目前仍為 Beta，Shell Command 以當前 OS User 權限執行，不建議一開始就授權整顆硬碟或重要資料目錄。若要深入查安裝細節或原始碼，見 GitHub（github.com/totec448-spec/chat-on-steroids）。
