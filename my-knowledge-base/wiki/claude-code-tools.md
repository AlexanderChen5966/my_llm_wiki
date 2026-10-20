---
title: Claude Code 內建 Tools 使用邏輯與 Prompt 優化
date: 2026-05-14
tags: [ai/claude]
source: https://israynotarray.com/ai/20260429/8473628195/
type: article
status: processed
asset_value: 72
---

Claude Code 有一套自己的工具選擇邏輯——了解工具的選擇原因，就能反推 Prompt 寫得哪裡不夠清楚。讀懂工具使用模式是調優工作流的核心技能。

## T（核心論點）
Prompt 越具體，Claude 選工具越精準，Token 消耗越低。看懂 Claude 選了什麼工具 → 反推 Prompt 哪裡沒說清楚 → 下次寫得更準。

## B（適用場景）
**適用：** 想優化與 Claude Code 的協作效率；設定 Hook/Skill/permissions 時需要知道工具名稱
**不適用：** 只做一次性問答的場景

## R（工具分類與選擇邏輯）

### 讀檔三劍客
| 工具 | 用途 | 選擇時機 |
|------|------|----------|
| **Read** | 讀取已知路徑的檔案（帶行號） | 你已知道確切路徑 |
| **Glob** | 用檔名 pattern 找檔案（`**/*.tsx`） | 你知道檔名長什麼樣，不知道在哪 |
| **Grep** | 用內容字串搜尋（底層 ripgrep） | 你只知道內容關鍵字 |

`Edit` 前必須先 `Read`（沒讀就 Edit 會被擋）。`Write` 是整檔覆寫，只用於建立全新檔案。

### Subagent 三種類型
| Subagent | 模型 | 工具 | 適合場景 |
|----------|------|------|----------|
| Explore | Haiku（快） | 唯讀 | 大範圍掃描 codebase |
| Plan | 同主對話 | 唯讀 | Plan Mode 研究 |
| general-purpose | 同主對話 | 全部 | 複雜任務邊讀邊改 |

需要讀很多檔案的任務 → 丟給 Explore Subagent，主對話 Context 不被塞爆。

### 其他工具速查
- **Bash**：跑測試/建置/git 指令；`run_in_background` 支援背景執行
- **Monitor**：持續餵 log 給 Claude（vs Bash background = 跑完才通知）
- **LSP**：型別跳轉、找引用；Edit 後自動回報型別錯誤
- **AskUserQuestion**：Claude 主動問多選題（在 CLAUDE.md 設定預設可減少問題）
- **WebFetch/WebSearch**：抓指定 URL 內容 / 搜尋關鍵字

## C（從工具使用模式反推 Prompt 問題）
| 觀察到的現象 | 代表的問題 | 修正方法 |
|-------------|-----------|----------|
| 一直 Read 不同檔案找不到 | 描述太抽象，Claude 在猜位置 | Prompt 直接給路徑 |
| Glob 一輪都找不到 | 關鍵字和實際檔名不匹配 | 告知命名慣例或改用 Grep |
| Edit 失敗後改用 Write | 你中間用 IDE 改過，記憶不一致 | 叫它先重新 Read 一次 |
| 大任務不用 Subagent | 沒判斷出是大任務 | Prompt 明寫「請用 Explore subagent」 |
| 繞道用 Bash | permissions.deny 擋了內建工具 | 檢查 deny 規則（deny 只擋內建，不擋 Bash） |

注意：`Read(./.env)` 的 deny 只擋 Read 工具，不擋 `Bash cat .env`，需要 OS sandboxing 才能真正擋住。

> ⚠️ 2026-10-08 並陳：[[agent-api-key-handoff-six-methods-risk]]（2026-10，依官方文件比對）稱 Claude Code 也會擋 `cat .env` 這類點名檔名的指令；但 `grep -r` 或腳本自行開檔仍擋不住，需要沙箱。兩說時間不同（本條 2026-05），可能是版本行為改變，未確認。兩者一致的部分：腳本讀檔擋不住，要 OS 沙箱。

## 相關連結
- [[claude-code-context-management]] — Context 管控與 Subagent 使用
- [[claude-code-file-management]] — permissions.deny 設定方式
- [[agent-api-key-handoff-six-methods-risk]] — 2026-10 依官方文件：`Read(./.env)` deny 也擋 `cat .env` 這類點名指令（與本條 2026-05 說法有時間差）

## 補充資料（來源：Claude Code 內建 Tools 在做什麼？看懂它怎麼選工具，Prompt 才寫得準）

### 完整工具清單（分類版）

| 分類 | 工具名稱 | 主要用途 |
|------|---------|---------|
| 讀檔 | Read | 讀取檔案內容（帶行號）|
| 讀檔 | Glob | 用檔名 pattern 找檔案（`**/*.tsx`）|
| 讀檔 | Grep | 用內容字串搜尋（底層是 ripgrep）|
| 改檔 | Edit | 對檔案做精確的字串替換 |
| 改檔 | Write | 整個檔案覆寫（只用於建立全新檔案）|
| 改檔 | NotebookEdit | 修改 Jupyter Notebook 的 cell |
| 執行 | Bash | 跑 Shell 指令（支援 `run_in_background`）|
| 執行 | PowerShell | 跑 PowerShell 指令（Windows）|
| 執行 | Monitor | 背景執行指令並把每行輸出即時回傳給 Claude |
| 委派 | Agent | 派出 Subagent 處理任務 |
| 程式碼 | LSP | 型別檢查、跳轉定義、找參考（透過 Language Server）|
| 任務 | TaskCreate / TaskUpdate / TaskList | 管理任務清單 |

### 為什麼要搞懂 Tools

三個實務原因：
1. **反推 Prompt 問題**：看到 Claude 連續開 7-8 個 Glob 找不到，代表你的描述太抽象（「登入邏輯」不等於檔名裡有 `login`），下次直接給路徑如 `src/auth/`
2. **設定 Hook 和 Skill 的 allowed-tools**：Hook 和 Skill 的 `allowed-tools` 設定全部建立在工具名稱上，不知道工具名稱就無法正確設定
3. **設定 permissions.deny**：deny 規則鎖的是工具名稱，不了解工具行為就無法判斷鎖哪個

### Bash vs Monitor 的關鍵差異

- **Bash**（背景模式）：`run_in_background=true`，指令跑完才通知 Claude
- **Monitor**：持續餵每行輸出給 Claude，適合需要即時觀察 log 的任務

### LSP 工具的隱藏能力

Edit 完成後，LSP 會自動回報型別錯誤——不需要等到 runtime 才發現問題。設定 `allowed-tools: LSP` 的 Skill 可以強制要求 Claude 在改完程式碼後確認型別正確。

### AskUserQuestion 的最佳實踐

在 CLAUDE.md 設定常見決策的預設值，可以大幅減少 Claude 問問題的頻率。例如：「如果我沒有明確說明語言，預設使用繁體中文」。
