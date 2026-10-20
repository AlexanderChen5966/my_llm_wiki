---
title: "認識 Pi Coding Agent：極簡終端 AI 程式助手概覽 - 光輝咖碼"
source: "https://kamadiam.com/pi-coding-agent/"
author:
  - "[[undefined]]"
published:
created: 2026-08-17
description: "厭倦沉重 AI 工具？Pi Coding Agent 用四個工具 + 可擴充設計，讓終端 AI 融入你的既有流程。含安裝、上下文工程、套件選型與 Cursor 分工決策表。"
tags:
  - "clippings"
status: "processed"
---
你是不是也遇過這種情況：開了某個 AI 程式助手，它硬要你走「先規劃 → 再開子代理 → 再審查」的流程，結果改一個小 bug，也得等它跑完一整串儀式？咖啡都喝完了，bug 還在讀取中。

市面上很多 Coding Agent 越來越像「整套 IDE 替代品」，而不是「幫你省時間的夥伴」。如果你想要的是： **極簡、快、能嵌進自己習慣的終端工作流** ，那值得認識 **Pi Coding Agent** （ [pi.dev](https://pi.dev/) ）。

這篇會帶你從「它是什麼」一路走到「怎麼安裝、怎麼設定、怎麼跟 Cursor 分工」。讀完後，你應該能判斷 Pi 是否適合自己的工作流，也知道該從哪一種模式開始試。

## 什麼是 Pi Coding Agent？

**Pi Coding Agent** 是跑在終端機（Terminal）裡的 AI 程式開發助手，由 `@earendil-works` 維護。它的設計哲學可以濃縮成一句話：

> 世界上有很多 Agent 框架，但這一個是 **你的** ——工具來適應你，不是你來適應工具。

跟 Devin 這類自主型平台或重度多代理編排系統不同，Pi 走的是 **「極簡核心＋按需擴充」** ：

**Devin** 是 Cognition 推出的自主 AI 軟體工程師（ [devin.ai](https://devin.ai/) ）：接到任務後會自行拆解步驟、讀 repo、寫 code、跑測試、開 PR，人類多半只需審核結果。這類「大而全」的 agent 平台內建規劃模式與子代理編排，能獨立完成整個 feature，但工作流往往也由工具決定，而不是開發者自己拼裝。

- **核心工具很少** ：內建檔案讀寫、編輯與 shell 執行能力
- **不把所有工作流塞進核心** ：子代理、網頁存取或 MCP 等能力可透過套件與 Extension 加入
- **規則與能力由你組合** ： `AGENTS.md` 、Skills、Extensions、Pi Packages 都能納入專案設定

白話說：你在專案資料夾打 `pi` ，就能用自然語言叫 AI 讀檔、改 code、跑測試；要不要加 MCP、子代理、網頁搜尋，全由你決定。

### 四層架構（看懂 Pi 在幹嘛）

| 層級 | 做什麼 |
| --- | --- |
| **CLI / TUI** | 終端互動、顯示狀態，以及高風險操作的人工確認 |
| **Agent 協調** | ReAct 迴圈：想 → 做 → 看結果 → 再修正 |
| **工具與上下文** | 核心工具，加上 Extension 注入的額外上下文或工具 |
| **持久化** | Session 樹狀歷史、JSONL 日誌，可 fork、可匯出 |

把 AI 直接放在終端的好處很實際：不用再「複製錯誤訊息 → 貼到聊天窗 → 複製修復 code 回來」。Linter 報錯、測試失敗，Agent 可以直接讀輸出並迭代修正。

## Pi vs Cursor vs Claude Code：該選誰？

Pi 不是 Cursor 的平替，而是不同取捨。下面這張表先幫你快速抓到方向；實際功能仍會隨版本與已安裝套件改變：

| 維度 | Pi Coding Agent | Claude Code | Cursor Agent |
| --- | --- | --- | --- |
| **核心取向** | 極簡核心，靠套件與 Extension 擴充 | 終端工作流與 Anthropic 生態整合 | IDE 內的編輯、導覽與 Agent 工作流 |
| **上下文來源** | `AGENTS.md` ／ `CLAUDE.md` 與 Extension | 指令檔與對話中選取的內容 | IDE 專案內容與 `@` 等上下文功能 |
| **Sub-agents** | 核心不預設提供，可由套件加入 | 依目前版本與設定提供相關能力 | 依目前版本與方案提供相關能力 |
| **Plan mode** | 用 Prompt Template 或 Extension 組合 | 依目前版本提供相關工作流 | 依目前版本提供相關工作流 |
| **RAG / 索引** | 由 Extension 或套件自行加入 | 依工具與工作流而定 | 依目前產品設定與版本而定 |
| **Session** | **樹狀分支** + compact | 線性 + compact | 多 chat tab |
| **部署** | CLI / RPC / SDK 嵌入 | CLI only | Desktop IDE |
| **Token 取捨** | 核心提示較小，細節由你控制 | 依工作流與上下文而定 | 依專案上下文與模型而定 |
| **最適合** | 終端實驗、SDK 嵌入、可版本控制的 Agent 設定 | Anthropic 生態深度使用者 | 日常 IDE 多檔編輯 |

### 依場景選工具

| 你的情境 | 建議 |
| --- | --- |
| 大型 monorepo、要靠自動索引 | Cursor / Claude Code |
| Side project、終端一把梭 | **Pi** |
| 本地模型、低 VRAM、要控 token | **Pi** |
| Agent 設定要進 Git（ `AGENTS.md` 、`.pi/` ） | **Pi** |
| 自建產品要嵌 Agent（SDK） | **Pi** |
| UI 調像素、邊預覽邊改 | Cursor |

**實戰分工** 可以這樣想：主專案用 Cursor 改 UI、做多檔 refactor；Pi 負責終端批次任務、長跑腳本或低成本實驗。 **同一目錄、同一時間只讓一邊寫檔** ；若要並行，請用 Git worktree 隔離。兩個 Agent 同時改同一檔案，最後通常不是協作，是 merge conflict 的聯誼會。

## 四種運作模式

Pi 不只有「開 TUI 聊天」：

| 模式 | 怎麼用 | 適合 |
| --- | --- | --- |
| **Interactive（互動）** | 專案目錄執行 `pi` | 日常對話、除錯、改 code |
| **Print / JSON** | `pi -p "分析此 repo"` 或 JSON event stream | 腳本自動化、結構化事件流 |
| **RPC** | stdin/stdout JSON 協議 | 非 Node 系統整合 |
| **SDK** | 嵌入自己的 TS 應用 | 自建 Agent 產品 |

互動模式裡還有兩個很多人愛用的即時控制：

- **Steering（ `Enter` ）** ：Agent 還在工作時，排入一則訊息糾偏；目前這輪工具呼叫完成後，會在下一次模型呼叫前送出。
- **Follow-up（ `Alt + Enter` ）** ：排程下一則訊息，等目前工作與待處理的 steering 訊息完成後再接上。

長對話別硬撐——用 `/compact` 壓縮舊訊息，或用 `/tree` 回到某個節點、從那裡繼續另一條路。完整歷史仍保存在 JSONL session 檔案中，但壓縮後的摘要不是逐字備份，重要決策最好另外寫進專案檔案。

## 上下文工程：Pi 真正厲害的地方

AI 回得好不好，八成看上下文怎麼塞。Pi 的策略是： **Harness 保持極簡，上下文全部可組合、可版本控制。**

### 1\. Context Files（AGENTS.md / SYSTEM.md）

Pi 啟動時會從目前目錄往上尋找並載入指令檔：

| 載入順序 | 路徑 | 用途 |
| --- | --- | --- |
| 全域 | `~/.pi/agent/AGENTS.md` | 個人通用規範 |
| 父目錄 | 向上尋找的 `AGENTS.md` 或 `CLAUDE.md` | Monorepo 子專案繼承 |
| 當前目錄 | `./AGENTS.md` 或 `./CLAUDE.md` | 專案專屬規則 |

`SYSTEM.md` 可 **完全替換** 預設 system prompt； `APPEND_SYSTEM.md` 則用來追加內容。兩者都適合鎖定語言、安全邊界與角色人設。

若需要跨專案的系統提示，也可以在 `~/.pi/agent/` 放 `SYSTEM.md` 或 `APPEND_SYSTEM.md` ：前者取代預設 system prompt，後者追加內容。適合放「先讀本地、危險操作先問、不確定就停下來問人」這類共通規則；請把專案特有規範留在 `AGENTS.md` 。

範例（專案根 `AGENTS.md` ）：

```markdown
# Project Instructions
- 改 code 後跑 \`npm run check\`。
- 不要動 production 設定檔。
- 不確定時先問我。
```

### 2\. Skills：按需載入的能力包

Skills 是 Markdown 檔，frontmatter 有 `name` 和 `description` 。Pi 只在 system prompt 放 **摘要** ；Agent 要用 `read` 才載入全文——避免一次塞爆 context。

手動觸發： `/skill:git-workflow` 。發現路徑： `~/.pi/agent/skills/` 、`.pi/skills/` 、`.agents/skills/` （專案 skills 需 trust 專案）。

### 3\. Prompt Templates：可重用的 / 指令

帶參數的 Markdown 模板，例如 `/review src/auth.ts` 展開成完整審查 prompt。支援 `$1` 、 `$ARGUMENTS` 等替換。適合標準化 code review、部署 checklist。

### 4\. Extensions：動態注入 RAG

TypeScript Extension 可以訂閱 `context` 事件，在每次 LLM 呼叫前調整 message 陣列。這讓你能自行加入檔案檢索、額外規則或其他上下文來源；具體做法要依 Extension 的實作與文件為準。

### Project Trust（安全邊界）

當專案含有 `.pi/` 資源或專案層級的 `.agents/skills` 時，Pi 可能要求你先 **trust** 專案。未信任前不會載入需要信任的專案資源；信任後才會載入專案設定與 Extension。第一次打開陌生 repo，先看清楚它有哪些設定，再按下信任，這個確認不是裝飾品。

## 為什麼 Pi 刻意不內建 MCP、子代理、Plan mode？

這不是單純的「功能缺失」，而是把取捨交回使用者：

| 刻意省略 | 為什麼 | 需要時怎麼辦 |
| --- | --- | --- |
| MCP | 工具描述可能增加上下文負擔 | 安裝相容的 MCP Extension |
| Sub-agents | 不預設綁定特定編排方式 | 安裝支援子代理的套件 |
| Plan mode | 不強迫所有人採用同一套規劃儀式 | Prompt Template 或 Extension |
| IDE 整合 | 專注終端 Harness | 用 RPC 或 SDK 從外部嵌入 |

官方文件的精神很直白： **Ask Pi to build it, or install a package.**

套件安裝範例：

```bash
pi install npm:pi-mcp-adapter    # 社群 MCP adapter（請先確認套件來源）
pi install npm:pi-subagents      # 社群子代理套件（請先確認套件來源）
pi install npm:pi-web-access     # 社群網頁存取套件（請先確認套件來源）
```

## 套件選型：不要一開始裝滿排行榜

[pi.dev/packages](https://pi.dev/packages) 有社群套件目錄。 **比較穩的做法是從 Tier 0 起步，痛點出現再加。** 套件不是自助餐，不必看到排行榜就每盤夾一點。

| Tier | 組合 | 適合 |
| --- | --- | --- |
| **Tier 0** | 僅 `AGENTS.md` | 小 patch、學習四工具 |
| **Tier 1** | 依需求加入 MCP 或子代理套件 | 日常 feature、review 迴圈 |
| **Tier 2** | \+ `pi-web-access` / `pi-codex-goal` / `context-mode` | 長任務、context 膨脹 |

### pi-mcp-adapter：MCP 但不炸 context

不同 MCP server 的工具描述量差異很大。部分 adapter 會用單一代理工具，按需發現與呼叫，以減少一開始暴露給模型的工具描述；是否 lazy 啟動、是否支援直接暴露工具等細節，取決於所安裝 adapter 的版本與設定。這裡不要把某個套件的行為套到所有 MCP 整合上，安裝前先看套件文件。

### pi-subagents：子代理怎麼編排

部分子代理套件提供 `scout` 、 `planner` 、 `worker` 、 `reviewer` 等角色，以及鏈式或並行執行；角色名稱與模式由套件決定，安裝前請看它自己的文件。

實戰 prompt 範例：

```
Use scout to map the auth flow, then planner for an implementation plan.
Run parallel reviewers: correctness, tests, and unnecessary complexity.
```

注意：並行工作通常會增加模型呼叫與 token 成本；裝了套件也不代表一定會自動跑 reviewer，仍要在 prompt 或 `AGENTS.md` 裡 **明確要求** 。

## 五階段實戰工作流

Pi 沒有「唯一官方流程」，但下面這套在實戰中最穩：

```
Phase 0  設定層（一次性）
   AGENTS.md + APPEND_SYSTEM.md + 精選 packages
        ↓
Phase 1  任務釐清
   自然語言目標 → 大任務用 create_goal 或 planner
        ↓
Phase 2  探索（唯讀）
   scout / read+grep / web-access
        ↓
Phase 3  實作
   worker 或主 session 小步 edit
        ↓
Phase 4  驗證閉環
   bash 跑 test/lint → reviewer → 修正
        ↓
Phase 5  上下文維護
   /compact、/tree 分支、context-mode（長 session）
```

**三個反模式請避開：**

1. 一開始裝滿熱門榜——context 與認知負擔暴增
2. 長專案只靠對話記憶——應寫進 `AGENTS.md` 、goal 檔或 `plan.md`
3. 沒給驗證條件就「改到好」——一定要指定 test/lint 成功才算完

探索階段原則： **計畫寫進檔案** （ `plan.md` 、 `context.md` ），先讀本地，不足再 web，大改前先跟人確認。

## 安裝與第一次實戰（約 10 分鐘）

**前置** ：Node.js 版本請以 Pi 官方文件與目前套件的 `engines` 宣告為準；安裝前先確認自己的 Node.js 版本。

```bash
# 方式 A：官方 npm（方便鎖版本）
npm install -g --ignore-scripts @earendil-works/pi-coding-agent

# 方式 B：macOS / Linux 一鍵腳本
curl -fsSL https://pi.dev/install.sh | sh
```

確認：

```bash
node -v    # 確認版本符合官方文件與套件需求
pi --help
```

登入模型（憑證存 `~/.pi/agent/auth.json` ）：

```bash
cd /path/to/your-sandbox-project   # 先用可弄髒的 side project
pi
/login
```

可用的登入方式與模型供應商會隨版本更新，請以 `/login` 顯示的選項及官方文件為準。CI 若使用 API key，請透過環境變數或 CI secret 管理，不要把金鑰寫進 repo。

第一次驗證：

```
Summarize this repository and tell me how to run its checks.
```

進階試試： `@README.md` 附加檔案、`!npm run lint` 跑 shell、 `pi -p "Summarize"` 非互動模式。

### 新手常見踩坑

| 狀況 | 怎麼修 |
| --- | --- |
| `pi: command not found` | 確認 npm global `bin` 在 PATH |
| 沒 API key | 在 Pi 內 `/login` 或設 env |
| AI 亂改重要檔 | 用 sandbox repo + `AGENTS.md` 限制 |
| 聊越久越糊 | `/compact` 或 `/new` |
| 想要 MCP 但沒有 | `pi install npm:pi-mcp-adapter` |

## 推薦工具與資源

| 工具 | 適合誰 | 一句話 |
| --- | --- | --- |
| **[Pi Coding Agent](https://pi.dev/)** | 想控 context、終端 workflow、SDK 嵌入 | 極簡四工具 + 按需擴充， **本文明星推薦** |
| **Cursor** | 日常 IDE 多檔編輯、UI 調整 | 索引與預覽強，跟 Pi 互補而非互斥 |
| **Claude Code** | Anthropic 訂閱深度使用者 | 終端 Agent，內建功能較多、較 opinionated |
| **pi-mcp-adapter** | 要用 MCP 又怕 context 爆炸 | Pi 生態 MCP 首選 |
| **pi-subagents** | 需要 scout/reviewer 鏈 | 補 Pi 不內建子代理的缺口 |

延伸閱讀： [Pi Quickstart](https://pi.dev/docs/latest/quickstart) 、 [使用 Pi](https://pi.dev/docs/latest/usage) 、 [Extensions 文件](https://pi.dev/docs/latest/extensions) 、 [Pi Packages 文件](https://pi.dev/docs/latest/packages) 。

## 結語：回歸極簡，但別回歸低效

Pi Coding Agent 重新定義了「AI 助手該怎麼存在於開發流程裡」——不試圖接管整個 IDE，而是當一個 **快、可配置、可版本控制** 的終端夥伴。

三個 takeaway：

1. **極簡核心** （四工具）換來 token 效率與完整上下文控制權
2. **能力按需加** （Packages / Skills / Extensions），不要一開始裝滿
3. **跟 Cursor 分工** ：IDE 改 UI、Pi 跑終端批次與實驗——同一 worktree 別兩邊同時寫

若你厭倦了沉重、黑盒、硬塞工作流的 AI 工具，建議先用 sandbox 專案試一輪： **到 [pi.dev](https://pi.dev/) 安裝 → `/login` → 寫一份 `AGENTS.md` → 跑一個小任務驗證讀檔、編輯與 shell 執行的閉環** 。覺得順了，再按需加入社群套件。先把核心用熟，再加配料；不然最後可能只剩一鍋「設定燉湯」。

## 參考資料

- [Pi 官方文件](https://pi.dev/docs/latest) ：查閱安裝方式、核心概念與目前功能。
- [Using Pi](https://pi.dev/docs/latest/usage) ：查閱啟動方式、互動指令、模式與 context files。
- [Extensions](https://pi.dev/docs/latest/extensions) ：查閱 Extension 的載入位置與擴充方式。
- [Pi Packages](https://pi.dev/docs/latest/packages) ：查閱 npm／Git 套件的安裝與管理方式。
- [Pi Packages 目錄](https://pi.dev/packages) ：查閱社群套件與各套件自己的說明。
- [Pi GitHub Repository](https://github.com/badlogic/pi-mono) ：查閱原始碼與版本變更。