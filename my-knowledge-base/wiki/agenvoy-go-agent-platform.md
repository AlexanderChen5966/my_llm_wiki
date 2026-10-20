---
title: Agenvoy：Go 打造的個人 AI Agent 平台（自建工具 + 沙箱 + 異步 ask_user）
date: 2026-06-22
tags: [ai/agent, ai/mcp]
source: https://github.com/pardnchiu/Agenvoy
type: reference
status: processed
asset_value: 66
---

Agenvoy 是台灣開發者邱敬幃（Pardn Chiu）用 Go 打造的開源 Agentic AI 平台（Apache-2.0，Alpha），受 OpenClaw 啟發。一行指令安裝、單一檔案，把 Copilot / Claude / OpenAI / Gemini / Nvidia NIM / 本地 Ollama 七大後端統一在一個介面，由 Planner LLM 自動路由到最適合的模型。最大特色：**自己會建工具**（缺工具時自建並保存，所有 Agent 共用）+ **OS 原生沙箱**執行。整合自 3 篇來源（RepoInside 介紹 / 官方 README / 作者 ask_user 異步設計深度文）。

## T（核心論點）

Agenvoy 不只是聊天機器人，而是「跑在你電腦上、會自己造工具並自我修復的個人 Agent」。它同時也是 **MCP server**——Claude Code / Codex / OpenCode 一行設定連上，就能用 Agenvoy 所有沙箱工具、找不到工具時自動建立、且建好的工具所有 Agent 共用（Tool Market 共享）。定位介於 [[hermes-agent]]（個人長效 Agent）與 [[opensquilla-intelligent-model-router]]（智能模型路由）之間，把「多模型調度 + 安全執行 + 自建工具」三者打包。

## B（適用場景）

**適用：**
- 想用單一 CLI 統一管理多個 AI 訂閱（已有 Copilot / OpenAI / Claude 等多帳號）
- DevOps / SRE 需要安全的自動化排程與指令執行（沙箱隔離）
- 想研究多模型調度、Agentic 架構、工具呼叫迴圈、MCP server 實作的工程師
- Discord 社群管理（內建 Discord Bot 模式）

**不適用 / 注意：**
- 目前是 **Alpha** 狀態，生產環境須謹慎
- 受 OpenClaw 啟發但更重「自建工具 + 一鍵安裝」，與 Hermes（pip+docker）取向不同

## R（核心能力與架構）

### 自我宣稱的十項核心能力
| 能力 | 說明 |
|------|------|
| 自動工具生成 | 缺工具時自行建立並保存 |
| 自我排程 | 一句話建立定時任務 |
| 長期記憶 | 每輪結束自動產生結構化摘要，下次注入歷史 |
| 文件搜尋 | 語意＋關鍵字，從本機文件回答 |
| Sub-Agent | 多 Agent 協作 |
| MCP client / server | 連外部 MCP，也讓任何 AI Agent 用你的沙箱工具 |
| Tool Market | 分享與安裝工具 |
| 自我改進 | 執行失敗後自動修正 |

### 安全與工程設計
- **OS 原生沙箱**：Linux `bubblewrap` / macOS `sandbox-exec` 隔離所有外部指令
- **API 金鑰**：存 OS 原生 Keychain（macOS Keychain / Linux Secret Service），不放 `.env`
- **SKILL.md 宣告式**：Markdown 統一定義任務流程與工具權限
- **Token 透明**：跨所有迭代累計追蹤即時顯示
- **聊天驗證**：6 碼驗證碼（vs OpenClaw / Hermes 的人工核准）

## C（關鍵行動 — 重點學習：ask_user 異步設計）

作者一篇深度文揭露了長效自主 Agent 的關鍵設計改造，**比產品本身更值得學**：

- [ ] **問題**：`v0.25.13` 前，Agent 問用戶問題是**阻塞**的（記憶體裡等回覆，加 10 分鐘超時）。這逼用戶必須緊盯訊息，否則要重跑——除了 `always-allow` 外根本無法長久自主運行
- [ ] **解法（受 Claude 網頁版啟發）**：`ask_user` 前先記錄**目標 / 已執行 / 下一步**三件事，然後**結束當前執行迴圈**，等用戶回覆才重新觸發
- [ ] **核心轉變**：把原本存記憶體的狀態**釋放到磁碟**。記憶體狀態隨生命週期消失，磁碟狀態永遠在那直到明確刪除——所以用戶一天後才回答也沒問題
- [ ] **Resume 流程**：回覆後讀「狀態紀錄 + 用戶答案」組成完整 context 當新輸入，**不依賴對話歷史**。對齊了 [[react-multi-mcp-agent]] 的 Human-in-the-Loop 模式：中斷前重整狀態反而幫 Agent 對齊需求與目標

## 原始摘要

Agenvoy 官方對照表（自評）：vs OpenClaw（pnpm monorepo，手動切模型，純文字 UI，不能自建工具）、vs Hermes-agent（pip+docker，手動切，僅 skill 能擴充）。Agenvoy 主打一行安裝單檔、自動選模型、按鈕/選單/modal 對話 UI、自己生成工具、跨 session 推送、語意+關鍵字文件搜尋。API 擴充只需丟一個 JSON 檔（傳統要寫 Go 程式）。MacBook 建議 `sudo pmset -c sleep 0` 避免休眠影響排程。

## 相關連結
- [[hermes-agent]] — 個人長效 Agent（Agenvoy README 直接對標的對象）
- [[opensquilla-intelligent-model-router]] — 智能模型路由（Agenvoy Planner LLM 自動選模型同理）
- [[mcp-fastmcp-server]] — MCP server 實作（Agenvoy 本身即 MCP server）
- [[react-multi-mcp-agent]] — ReAct × Multi-MCP（ask_user 異步呼應其 Human-in-the-Loop）
- [[concepts/agent-workflow-taxonomy]] — Agenvoy 橫跨 ①框架 / ②協定 / ④記憶 多層
