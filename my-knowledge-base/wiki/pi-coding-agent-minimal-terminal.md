---
title: Pi Coding Agent：4 個核心工具的極簡終端 agent
date: 2026-08-17
tags: [ai/agent, ai/workflow]
source: https://kamadiam.com/pi-coding-agent/（光輝咖碼：架構與選型）
source_2: https://vervecode.dev/ai/pi-coding-agent/（安裝配置與用 Codex 訂閱登入）
type: article
status: processed
asset_value: 60
---

兩篇合併。🔑 **與 [[deepseek-harness-plugin-architecture]] 是同一光譜的兩端**：pi 是 4 個工具的極簡主義，DSH 是 185 個外掛的組合主義。

## T（核心論點）

> **世界上有很多 Agent 框架，但這一個是「你的」——工具來適應你，不是你來適應工具。**

它要解決的痛點很具體：**很多 Coding Agent 硬要你走「先規劃 → 再開子代理 → 再審查」的流程，結果改一個小 bug 也得等它跑完一整串儀式**。所以 pi 走「極簡核心 ＋ 按需擴充」：

- **核心工具很少**：檔案讀寫、編輯、shell 執行（`read`／`write`／`edit`／`bash`）
- **不把工作流塞進核心**：子代理、網頁存取、MCP 都靠套件與 Extension 加入
- **規則與能力由你組合**：`AGENTS.md`、Skills、Extensions、Pi Packages 都能納入專案設定並**進 Git 版控**

## B（適用場景）

| 你的情境 | 建議 |
|---|---|
| 大型 monorepo、要靠自動索引 | Cursor / Claude Code |
| **Side project、終端一把梭** | **Pi** |
| **本地模型、低 VRAM、要控 token** | **Pi** |
| **Agent 設定要進 Git（`AGENTS.md`、`.pi/`）** | **Pi** |
| **自建產品要嵌 Agent（SDK）** | **Pi** |
| UI 調像素、邊預覽邊改 | Cursor |

**不適用 / 注意：**
- 🔴 **官方明言沒有內建權限系統，靠容器化**（見 [[deepseek-harness-plugin-architecture]] 的三 harness 對照表）——這點與 [[agent-permission-and-os-sandbox]] 的結論對照起來很關鍵：**沒有權限層就等於全開**
- ⚠️ 兩篇作者都定位它為「小眾選擇」（主流仍是 Codex App 與 Claude Code）
- ⚠️ `/compact` 壓縮後的摘要**不是逐字備份**，重要決策最好另外寫進專案檔案

## R（四層架構與四種運作模式）

| 層級 | 做什麼 |
|---|---|
| CLI / TUI | 終端互動、顯示狀態、**高風險操作的人工確認** |
| Agent 協調 | ReAct 迴圈：想 → 做 → 看結果 → 再修正 |
| 工具與上下文 | 核心工具 ＋ Extension 注入的額外上下文或工具 |
| 持久化 | **Session 樹狀歷史**、JSONL 日誌，可 fork、可匯出 |

四種運作模式（不只是「開 TUI 聊天」）：

- **Interactive** — 專案目錄執行 `pi`
- **Print / JSON** — `pi -p "分析此 repo"` 或 JSON event stream，供腳本自動化
- **RPC** — stdin/stdout JSON 協議，供非 Node 系統整合
- **SDK** — 嵌入自己的 TS 應用

🔑 **兩個即時控制很值得學**（多數 agent 沒有）：

- **Steering（`Enter`）** — Agent 還在工作時排入一則訊息糾偏，**目前這輪工具呼叫完成後、下次模型呼叫前送出**
- **Follow-up（`Alt + Enter`）** — 排程下一則訊息，等目前工作與待處理的 steering 訊息完成後再接上

長對話用 `/compact` 壓縮，或用 **`/tree` 回到某個節點、從那裡繼續另一條路**（樹狀 session 是它與線性 session 工具的主要差異）。

## C（關鍵行動）

- [ ] 若考慮採用：先確認你能接受「沒有內建權限系統，靠容器化」
- [ ] 想控 token 或用本機模型時，它的極簡核心提示是實質優勢
- [ ] 與其他 agent 併用時：**同一目錄、同一時間只讓一邊寫檔**，要並行請用 git worktree 隔離

## 補充資料

### 用既有訂閱登入（不必另付 API 費用）

pi 預設能使用 **Claude、Codex、GitHub Copilot 的訂閱**，也可用 API Key。流程：`/login` → Use a subscription → 選 provider（例如 ChatGPT Plus/Pro）→ **Browser login** → 開網頁登入已訂閱帳號。

基本操作：`/model` 選模型（**`Ctrl+P` 反覆點擊可直接切換**）、**`Shift+Tab` 反覆點擊調整推理等級**。

### 🔑 第二篇作者的實際用法（額度分攤策略）

> 「我能於 Pi 中連線公司提供的 Cursor，開發時規劃用 GPT-5.5 xhigh，實作時視複雜度派 GPT-5.4 或 Composer 2.5 的子代理，**可以分攤近期被嚴重閹割的 Codex 額度**。」

⚠️ 這是把 pi 當「多供應商 gateway」用的思路，與 [[cc-switch-provider-manager]]、[[claude-swap-account-rotation]] 的動機相同，但實作層次不同（前者換 provider 設定，pi 是在同一 agent 內派不同模型的子代理）。

### 與 Cursor 的分工建議

主專案用 Cursor 改 UI、做多檔 refactor；pi 負責終端批次任務、長跑腳本或低成本實驗。

⚠️ 原文的警語很傳神：**「兩個 Agent 同時改同一檔案，最後通常不是協作，是 merge conflict 的聯誼會。」**（Worktree 隔離的理由見 [[agent-task-system-background-scheduling]] 的 Day 16 章節）

### 上下文工程：Harness 保持極簡，上下文全部可組合可版控

啟動時會從目前目錄**往上尋找並載入指令檔**（`AGENTS.md`／`SYSTEM.md`，也支援 `CLAUDE.md`）。🔑 **「AI 回得好不好，八成看上下文怎麼塞」**——這與 [[agents-md-guide]]、[[claude-md-design-patterns]] 的主張一致，差別在 pi 把「哪些上下文要進去」完全交給使用者以檔案宣告。

### 把 AI 放在終端的實際好處

**不用再「複製錯誤訊息 → 貼到聊天窗 → 複製修復 code 回來」**——Linter 報錯、測試失敗，Agent 可以直接讀輸出並迭代修正。（⚠️ 對照 [[ai-tutor-withhold-answer-architecture]]：那篇正是在說**這個便利本身會讓人失去讀錯誤訊息的能力**——同一個機制的兩面）

## 相關連結

- [[deepseek-harness-plugin-architecture]] — 🔑 **光譜另一端**：Everything is a Plugin、194 個套件；並附三 harness 完整對照表與依賴血緣鑑定四步法
- [[agent-permission-and-os-sandbox]] — pi 缺少的那一層（權限分級與 OS 沙箱）
- [[claude-code-harness-internals]] — Claude Code 的 harness 機制
- [[agents-md-guide]] — `AGENTS.md` 的寫法
- [[agent-loop-explained]] — ReAct 迴圈與四層階梯
- [[cc-switch-provider-manager]] — 跨 CLI 的 provider 統合
- [[claude-code-session-handoff]] — session 壓縮與交接（pi 的樹狀 session 是另一種解法）
- [[ollama-local-llm-setup]] — 本機模型（pi 的適用場景之一）
