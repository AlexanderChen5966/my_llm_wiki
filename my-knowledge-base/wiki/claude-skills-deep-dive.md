---
title: Claude Skills 深度解析：Progressive Disclosure 與四機制比較
date: 2026-05-14
tags: [ai/claude]
source: https://kaochenlong.com/claude-code-skills
type: article
status: processed
asset_value: 72
---

Claude Skills 最核心的設計是 Progressive Disclosure（漸進式揭露）：啟動時只預載 ~100 token 的 Metadata，判斷相關才載入完整 Instructions，需要時才讀 Resources——理論上可打包無限量知識而不撐爆 context。這與 CLAUDE.md（永遠全量載入）形成根本差異。

## T（核心論點）
Skills 的護城河不是「功能多」，而是「按需載入」設計讓知識模組化無上限，搭配跨平台開放標準（agentskills.io），讓你在 Claude Code 建的 Skills 理論上能在 Cursor、Gemini CLI、GitHub Copilot 等工具復用。

## B（適用場景）
**適用：** 想理解 Skills 底層機制、需要決定何時用 Skills vs Custom Commands vs MCP 的開發者
**不適用：** 只需要快速安裝現成 Skills 的用戶（直接看 [[claude-skills-ecosystem]]）

## R（三層載入機制）

```
Layer 1：Metadata（~100 tokens）
  → Claude 啟動時預載所有 Skill 的 name + description
  → 用來判斷「這個任務跟哪個 Skill 有關」

Layer 2：Instructions（建議 ≤5000 tokens）
  → 判斷相關後才載入 SKILL.md 主體
  → 包含操作步驟、判斷邏輯、範例

Layer 3：Resources（無限量）
  → scripts/、references/、assets/ 按需讀取
  → SKILL.md 必須明確引用，Claude 才知道它們存在
```

## 四機制比較表

| 機制 | 觸發方式 | 載入時機 | 適用場景 |
|------|---------|---------|---------|
| **Skills** | Claude 自動判斷（根據 description） | 動態載入 | 重複性專業工作流、需要 AI 主動判斷的情境 |
| **Custom Commands** | 使用者輸入 `/command` | 使用者觸發 | 固定格式的重複操作，使用者明確知道要做什麼 |
| **MCP** | 工具呼叫 | 啟動時載入 | 連接外部 API、資料庫、檔案系統 |
| **Subagents** | Claude 自動委派 | 需要時建立 | 需要獨立 context、平行處理、複雜任務 |

> Skills = 腦袋裡的知識（教 Agent 怎麼做）
> MCP = 手上的工具（讓 Agent 能存取外部資源）
> 四者不互斥，可同時運作

## C（關鍵行動）
- [ ] **description 是最重要的欄位**：決定 Skill 什麼時候被啟用。壞例：`Helps with PDFs.` 好例：`Extracts text and tables from PDF files... Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.`
- [ ] **SKILL.md 控制在 500 行以內**，詳細資料放 references/，在 SKILL.md 中明確引用
- [ ] **安全注意**：`scripts/` 裡的東西可以被執行，安裝前必須檢查有無惡意腳本（呼叫外部 URL、修改系統檔）

## SKILL.md 進階 frontmatter

```yaml
---
name: my-deploy-check
description: When to use this skill
disable-model-invocation: false   # true = 禁止 Claude 自動觸發
user-invocable: true              # false = 隱藏於 / 選單
allowed-tools: Read, Grep, Bash   # 精細權限控制
model: claude-sonnet-4-6           # 強制使用特定模型
context: fork                      # 在 isolated subagent 跑，不污染主 context
---
```

`context: fork` 是最強欄位——讓整個 Skill 跑在獨立 subagent，主對話 context 不被吃掉。

## 補充資料（來源：Anthropic 官方文件《Extend Claude with skills》，code.claude.com，2026-05）

### 官方架構新資訊：Custom Commands 已合併進 Skills

> A file at `.claude/commands/deploy.md` and a skill at `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way. Your existing `.claude/commands/` files keep working. Skills add optional features: a directory for supporting files, frontmatter to control whether you or Claude invokes them, and the ability for Claude to load them automatically when relevant.

**實際意義：** 不需遷移舊的 Custom Commands；Skills 是超集，多了支援目錄、frontmatter 控制、自動觸發。

### 官方 Frontmatter 完整欄位表（最新版）

| 欄位 | 功能 | 注意 |
|------|------|------|
| `name` | 顯示名稱（小寫字母+數字+連字號，最長 64 字）| 省略則用目錄名 |
| `description` | 觸發判斷依據，與 `when_to_use` 合計截斷於 **1,536 字元** | 最重要欄位 |
| `when_to_use` | 補充觸發時機說明，計入 1,536 字元上限 | — |
| `argument-hint` | autocomplete 提示（如 `[issue-number]`）| — |
| `arguments` | 具名位置參數，對應 `$name` 佔位符 | — |
| `disable-model-invocation` | `true` = 只能手動觸發，從 context 中隱藏 | 部署/提交等有副作用的流程 |
| `user-invocable` | `false` = 隱藏於 `/` 選單，只 Claude 可觸發 | 背景知識型 Skill |
| `allowed-tools` | 無需每次確認即可使用的工具清單 | 不限制可用工具，只是預批准 |
| `model` | 覆寫本次對話的模型，下一個 prompt 恢復 | 接受與 `/model` 相同的值 |
| `effort` | 覆寫 effort 等級（low/medium/high/xhigh/max）| — |
| `context` | `fork` = 在獨立 subagent 跑，不污染主 context | 最強隔離機制 |
| `agent` | `context: fork` 時指定 subagent 類型 | `Explore`/`Plan`/`general-purpose`/自訂 |
| `hooks` | Skill 生命週期 hooks | — |
| `paths` | Glob 路徑限制，只在操作匹配路徑時才自動啟用 | monorepo 場景 |

### 動態 Context 注入：`!` 語法

```yaml
---
name: pr-summary
context: fork
agent: Explore
allowed-tools: Bash(gh *)
---
PR diff: !`gh pr diff`
PR comments: !`gh pr view --comments`
```

`` !`command` `` 在 Skill 送給 Claude 之前執行，輸出替換佔位符——Claude 收到的是真實數據，不是命令本身。這是 **preprocessing**，不是 Claude 執行的工具呼叫。

### Skill 內容生命週期

- Skill 觸發後，`SKILL.md` 內容進入對話成為單一 message，**整個 session 期間持續存在**
- Auto-compaction 後，Skill 最多保留 **5,000 tokens**，所有活躍 Skill 共用 **25,000 tokens** 的總預算
- 最近調用的 Skill 優先保留；session 中調用過多 Skill 可能導致舊 Skill 被丟棄

### Skill 數量限制與 `skillListingBudgetFraction`

所有 Skill 的 description 載入 context 有 token 預算上限（預設 = 模型 context 視窗的 1%）。溢出時，使用頻率最低的 Skill 的 description 最先被截斷。

解法：
- `skillListingBudgetFraction: 0.02`（提升到 2%）
- 低優先 Skill 設為 `"name-only"` 於 `skillOverrides`
- 縮短 description 文字（key use case 放最前面）

### `skillOverrides` 四種可見度狀態

| Value | Claude 知道 | `/` 選單顯示 |
|-------|------------|------------|
| `"on"` | 名稱 + 描述 | 是 |
| `"name-only"` | 僅名稱 | 是 |
| `"user-invocable-only"` | 隱藏 | 是 |
| `"off"` | 隱藏 | 否 |

## 相關連結
- [[claude-skills-guide]] — 兩種 Skill 類型：能力包型 vs 軟編排型
- [[claude-skills-asset-building]] — MAPS 框架的 Skills 資產化策略
- [[claude-skills-ecosystem]] — GitHub 生態全景
- [[agent-skills-vs-mcp]] — Skills vs MCP 深入比較

## 補充資料（來源：Anthropic 官方《Skill 撰寫最佳實踐》，platform.claude.com，2026-05）

### 三大核心原則

**1. 簡潔原則**：Claude 已經非常聰明，只加 Claude 尚未擁有的上下文。對每條資訊質問「Claude 真的需要這個嗎？」簡潔版（50 tokens）vs 過度冗長版（150 tokens）輸出品質相近，但 token 成本差 3 倍。

**2. 自由度設定**：匹配任務的脆弱性與可變性：
- **高自由度**（文字描述）：多種方法都有效、決策取決於上下文 → 程式碼審查、研究綜合
- **中等自由度**（帶參數偽程式碼）：有首選模式但允許變化 → 報告生成
- **低自由度**（精確腳本、無參數）：操作脆弱、一致性關鍵 → 資料庫遷移

類比：窄橋上只有一條安全路線（低自由度）vs 開闊田野中許多路徑都能成功（高自由度）。

**3. 跨模型測試**：Opus 完美有效的內容可能需要為 Haiku 提供更多細節。同一個 Skill 應在 Haiku / Sonnet / Opus 都測試。

### 結構最佳實踐

- **description 欄位必須第三人稱**（「處理 Excel 文件並生成報告」，而非「我可以幫您…」）
- **SKILL.md 主體 ≤500 行**，超過時拆分為獨立參考文件
- **參考深度只有一層**：SKILL.md → reference files，不能 reference files 再引用其他 files
- **超過 100 行的參考文件要有目錄（TOC）**，確保 Claude 部分讀取時能看到完整範圍

### 工作流程與反饋循環

**清單追蹤模式**：複雜工作流程讓 Claude 複製清單並勾選，可防止跳過驗證步驟：
```
任務進度：
- [ ] 步驟 1：分析表單（執行 analyze_form.py）
- [ ] 步驟 2：驗證映射
- [ ] 步驟 3：填寫表單
```

**驗證迴圈模式**：執行驗證器 → 修復錯誤 → 重複，確保品質。只有驗證通過才繼續。

### 評估驅動開發（核心創新）

**先建立評估再寫 Skill**，確保解決真實問題而非假設問題：

1. 在沒有 Skill 的情況下完成任務，記錄具體失敗點
2. 建立 3 個測試場景作為 eval
3. 測量 Claude 的基準表現
4. 撰寫最少指令（剛好通過 eval）
5. 迭代：跑 eval → 比較基準 → 改進

**Claude A + Claude B 迭代法**：
- Claude A：幫助設計和改進 Skill 的專家（在同一次對話中產出 SKILL.md）
- Claude B：新對話中用真實任務測試 Skill 的代理人
- 觀察 Claude B 的行為 → 帶著具體問題返回 Claude A → 迭代

### 反模式清單

| 反模式 | 正確做法 |
|-------|---------|
| Windows 風格路徑（`scripts\helper.py`）| 一律使用正斜線（`scripts/helper.py`）|
| 提供太多選項（pdfplumber / PyMuPDF / pdf2image…）| 提供一個預設值 + 特殊情況說明 |
| 時效性資訊（「2025年8月之前用舊API」）| 用「舊模式」折疊區塊 |
| 巫毒常數（`TIMEOUT = 47`）| 每個值都說明理由 |
| 深度嵌套參考（A → B → C）| 只從 SKILL.md 一層引用 |

## 補充資料（來源：Claude Skills 深度实测能力包与软编排的完整指南.md）

### 三週實測數據

Axton Liu 深度使用 Claude Skills 三週的實測結果：

**能力包型（discussion-organizer）**：
- 50 多條碎片化學習筆記 → 結構化 Markdown，分三個清晰分類
- 四層判斷邏輯：價值評分（⭐×5）+ 可信度標記（✅/⚠️/❌）+ 提取粒度 + 措辭風格統一
- 用時：數分鐘（手工：3-4 小時）

**軟編排型（srt-workflow）**：
- 74 條字幕 → 2500 字結構化文章（中英對照版）
- 自動修正 6 個語音識別錯誤、添加 1 個 H1 + 5 個 H2 標題
- 四階段：字幕分段 → 質量審查 → 定稿 → 翻譯（條件觸發）
- 用時：約 2 分鐘（手工：1-2 小時）

### 軟編排 vs 官方編排的關鍵差異

**官方編排型（官方設計）**：
- Skill 定義流程：第一步做 A，第二步做 B，第三步做 C
- Claude 自己執行：A → B → C
- 所有操作在**單個 Claude 實例的上下文內**執行

**軟編排型（Axton 的擴展用法）**：
- Skill 只定義 SOP，主 Claude 讀取 SOP 後用 Task 工具調度多個獨立 Subagent
- Subagent 間透過**文件傳遞**上下文，不走主 Claude 的 Context
- 軟編排**只能在 Claude Code 使用**（依賴 Task 工具）

### 使用前置條件

- 需 **Pro、Team 或 Enterprise** 計劃（Free 用戶無法使用）
- 必須在設定中開啟 **Code Execution**
- 軟編排（Subagent 調度）只支援 Claude Code，基礎能力包型支援 Claude.ai Web/Desktop

### 常見問題與解法

| 問題 | 解法 |
|------|------|
| Skill 沒被自動觸發 | 在 description 中補充明確觸發詞和排除詞 |
| 多個 Skill description 重疊 | 加入「不適用場景」區分，或手動指定 Skill 名稱 |
| 軟編排並行度不夠 | 官方上限 10 個並行 Subagent；需要更高並行度改用 Make/N8N 硬編排 |
| 一次性任務 | 不要做成 Skill，直接對話即可 |

---

## 補充資料

來源：Anthropic 官方文件《Agent Skills Overview》（platform.claude.com，2026-05）

### 官方定義的精確架構

**為什麼叫 Agent Skills（官方說法）**：
> Skills are reusable, filesystem-based resources that provide Claude with domain-specific expertise: workflows, context, and best practices that transform general-purpose agents into specialists. Unlike prompts (conversation-level instructions for one-off tasks), Skills load on-demand and eliminate the need to repeatedly provide the same guidance across multiple conversations.

**三層 Progressive Disclosure（官方標準）**

| Level | 載入時機 | Token 成本 | 內容 |
|-------|---------|-----------|------|
| Level 1：Metadata | 啟動時（永遠） | 每個 Skill 約 100 tokens | YAML frontmatter 的 name + description |
| Level 2：Instructions | Skill 觸發時 | < 5k tokens | SKILL.md 主體（工作流、最佳實踐） |
| Level 3+：Resources | 按需讀取 | 有效無限制 | 打包的附加文件，透過 bash 執行而不進 context |

**關鍵機制：腳本執行不消耗 Context**
> When Claude runs `validate_form.py`, the script's code never loads into the context window. Only the script's output consumes tokens.

這讓腳本比讓 Claude 即時生成程式碼高效得多。

### 跨平台可用性與限制

**可用平台**：Claude API、Claude Code、claude.ai（需 Pro/Max/Team/Enterprise + code execution）

**共享範圍差異**：
- claude.ai：個人專屬，每位成員須各自上傳
- Claude API：Workspace 級別共享，所有成員可存取
- Claude Code：個人（`~/.claude/skills/`）或專案（`.claude/skills/`）

**重要限制：Custom Skills 不跨平台同步**
- claude.ai 上傳的 Skills 不會自動出現在 API
- Claude Code 的 Skill 是 filesystem-based，獨立於 claude.ai 和 API

### 官方內建 Skills（Pre-built）

- **PowerPoint (pptx)**：建立簡報、編輯投影片、分析內容
- **Excel (xlsx)**：建立試算表、分析資料、生成圖表報告
- **Word (docx)**：建立文件、編輯內容、格式化
- **PDF (pdf)**：生成格式化 PDF 文件和報告
- **Claude API Skill**（開源）：最新 API 參考資料、SDK 文件、8 種語言最佳實踐

### 安全性注意事項（官方警告）

> Treat like installing software: Only use Skills from trusted sources.

- 來自不受信任來源的 Skill 可能執行惡意指令（資料外洩、未授權系統存取）
- 使用外部 URL 的 Skill 有特別高的風險（外部依賴可能被污染）
- 安裝前必須審計所有打包的檔案：SKILL.md、scripts/、images/ 等
