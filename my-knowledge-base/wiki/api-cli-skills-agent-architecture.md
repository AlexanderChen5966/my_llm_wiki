---
title: API + CLI + Skills：讓產品對 AI Agent 友善的三層架構
date: 2026-06-15
tags: [ai/agent]
source: https://blog.wu-boy.com/2026/04/api-cli-skills-architecture-for-ai-agents-zh-tw/
type: article
status: processed
asset_value: 65
---

AI Agent 時代，產品的架構策略正在收斂：**API + CLI + Skills** 是讓任何產品對 Agent 友善的標準三層架構。Basecamp 和 Google Workspace 都走上了這條路。

## T（核心論點）

每一層各司其職，缺一不可：

```
Agent Skills（智慧層）
        ↓
CLI 工具（橋接層）
        ↓
API Layer（資料與動作層）
```

- **API Layer**：所有功能透過 API 暴露，是一切的基礎
- **CLI 工具**：把 API 包成原子命令，是連結遠端與本地的橋樑
- **Agent Skills**：編碼「何時用、怎麼用、用完後做什麼」的工作流知識

**Skill 不是取代 CLI，而是包裝 CLI。** 沒有好 CLI，Skill 只是空洞的 prompt。

## B（適用場景）

- 評估自己的產品/工具是否「Agent-accessible」
- 選擇 CLI 還是 MCP Server 的技術決策
- 替既有 CLI 寫第一個 Agent Skill

## R（完整內容）

### 為什麼 CLI 是最關鍵的中間層

**vs 直接呼叫 API：**
- API 沒有本地狀態，不能讀取 git repo、不能跑測試
- CLI 混合本地操作（讀寫檔案、操作 Git）+ 遠端 API，正是 Agent 日常所需

**MCP vs CLI 務實比較：**

| 面向 | MCP Server | CLI 工具 |
|------|-----------|---------|
| 部署需求 | 需要 sidecar 程序 | 單一 binary |
| 功能發現 | 需要 MCP 客戶端 | `--help` 即可 |
| 錯誤處理 | 協議層錯誤處理 | exit code + stderr |
| 組合性 | 需要程式碼串接 | pipe + shell script |
| 偵錯 | 需要 MCP inspector | 直接在 terminal 測試 |

Peter Steinberger（OpenClaw 作者）："every MCP would be better as a CLI"——在大多數「讓 Agent 操作你的產品」場景，CLI 更簡單、更通用、更好維護。

**例外**：需要即時雙向通訊（瀏覽器內 Agent、串流資料）才用 MCP。

### CLI 的四大優勢（Agent 視角）

**1. 確定性執行迴圈**
```bash
$ tea issues create --title "Fix bug"
# 成功：exit code 0，stdout 回傳 Issue URL
$ tea issues --repo nonexistent
# 失敗：exit code 1，stderr 說明原因
```
Agent 據此自我修正：exit code ≠ 0 → 讀 stderr → 調整下一步。

**2. 零整合成本**
`--help` 就能發現所有功能，不需要 schema 註冊。加上 `--json` flag 讓輸出可程式化解析。

**3. 天然組合性**
```bash
# pipe + jq 組合多個操作
tea issues list --state open --output json | jq '.[] | select(.labels | split(" ") | any(. == "kind/bug"))'
```

**4. 混合本地與遠端操作**
Gitea 的 `tea` CLI：讀 `.git/config` → 呼叫 Gitea API → 本地 git checkout，Agent 一個指令完成。

### Agent-Friendly CLI 設計原則

- 提供 `--json` flag，輸出可程式化解析
- 有意義的 exit code（0=成功，1=一般錯誤，2=參數錯誤）
- 清楚的 `--help` 說明
- 命令具有**冪等性**（重複執行不造成問題）
- 錯誤訊息寫到 stderr，正常輸出寫到 stdout

### 替既有 CLI 寫 Agent Skill

```markdown
---
name: deploy
description: >-
  Deploy the application to staging or production.
  Use when the user says "deploy", "release", or "push to production".
---

## Steps
1. 確認當前 branch（git branch --show-current 應為 main）
2. 跑測試（make test）
3. 部署（deploy --env <environment> --json）
4. 驗證（deploy status --env <environment> --json）
5. 回報結果
```

Skill 編碼的不是「如何執行」（那是 CLI 的工作），而是**何時執行、執行前要做什麼、執行後要確認什麼**。

### Go 是 CLI 工具的首選語言

優勢：
- 單一靜態 binary，無 runtime 依賴
- 跨平台編譯：`GOOS=linux GOARCH=amd64 go build`
- 啟動速度極快（每次 CLI 呼叫都是新 process）
- 豐富生態：cobra / urfave/cli / bubbletea

現實案例：Google Workspace CLI（Rust）、Gitea `tea`（Go）、GitHub `gh`（Go）、Docker CLI（Go）

## C（關鍵行動）

- [ ] 評估自己的工具是否有 `--json` 輸出和有意義的 exit code
- [ ] 替最常用的 CLI 工具寫一個 Agent Skill（描述何時用、流程步驟）
- [ ] 新工具優先考慮 CLI 而非 MCP Server，除非有即時雙向通訊需求

## 📎 2026-10-19 補充視角：LLM／Agent／Skill 三層（數位時代）

來源：數位時代〈Skills技能包是什麼？LLM、Agent、Skill新3層架構，讓企業ERP、CRM接上AI代理〉，2026-10-06，<https://www.bnext.com.tw/article/92500/ai-agent-skills-llm-enterprise-app-sop>（文末署名「責任編輯：陳祈安」，實際作者不明）。**全篇作者觀點**，無數據、無具體產品版本或實作；🔴 原文**沒有討論權限、驗證與錯誤責任**。

- 以 CPU／OS／App 類比提出 **LLM（理解推理）→ Agent（拆解目標、規劃步驟）→ Skill（可被 Agent 調用的工作流）**；Skill 要呼叫企業既有的 ERP／CRM／POS／WMS，**不取代 App**，但 App 要改成面向 Agent（裝 MCP 或開放 API）
- ⚠️ **兩套分層的分類軸不同，不合併**：本條的 API／CLI／Skills 是「**產品端工具層**」（產品怎麼讓 Agent 用）；bnext 的 LLM／Agent／Skill 是「**AI 堆疊層**」（能力從哪一層來）。兩者唯一交集是「Skill 包裝既有工具、不取代它」
- 案例（**作者示意**）：業務問「某客戶能否追加訂單」→ Skill 串 CRM 客戶紀錄＋ERP 應收帳款後給建議
- ⚠️ **與 [[sop-to-skill-4-steps]] 觀點並列，不擇一**：
  - bnext：Skill「**不需要 SOP，只要指引（guideline）**」，靠 LLM 判斷例外、必要時回頭問人；但同文也說「**SOP 還在**」，Skill 是讓工作流能處理例外
  - sop-to-skill-4-steps：SOP 多半缺判斷標準與例外處理，要**先補齊範疇、判斷標準、計算邏輯、例外處理**再轉成 SKILL.md

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — Agent 工程架構知識精粹（本主題 8 篇整合）
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills / MCP / Plugins / Agents 四概念比較
- [[ai-agent-architecture-patterns]] — AI Agent 架構模式（ReAct / Plan&Execute 等）
- [[skill-md-9-types-and-gotchas]] — SKILL.md 怎麼寫（Skill 的格式與實踐）
- [[loop-engineering-6-components]] — Loop Engineering（CLI + Skills 在更大系統中的位置）
