---
title: 規格驅動 AI 開發工作流：spec-kit / OpenSpec + Superpowers
date: 2026-05-14
tags: [ai/agent, dev/architecture]
source: https://nijialin.com/2026/02/18/superpowers-vs-spec-kit/
type: article
status: processed
asset_value: 68
---

解決 AI 開發兩大痛點的組合拳：「規格先行工具」（spec-kit 或 OpenSpec）負責在寫 code 前建立需求共識；「Superpowers」負責強制執行工程紀律（TDD、系統化除錯）。一個管「知道要做什麼」，另一個管「用正確方式做」。

## T（核心論點）
AI 開發的根本問題有兩個：(1) 需求沒講清楚就開工——缺乏規格共識；(2) 開工後缺乏工程紀律——不寫測試、不驗證完成。規格工具 + Superpowers 分別解決這兩個問題，兩者是互補而非替代關係。

## B（適用場景）
**適用：** 需要讓 AI 長時間自走、或功能複雜到需要先釐清需求的開發場景
**不適用：** 簡單的一次性 bug fix；已有完整規格的成熟功能維護

## R（兩種規格工具選擇）

### spec-kit（GitHub 官方出品）
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
specify init <project_name> --ai claude
```
五步驟 SDD 流程：
1. `/speckit.constitution` — 定義專案原則
2. `/speckit.specify` — 描述要做什麼功能
3. `/speckit.plan` — 產出技術實施計畫
4. `/speckit.tasks` — 把計畫拆成可執行任務
5. `/speckit.implement` — 執行任務產出 code

支援 Claude Code、GitHub Copilot、Cursor、Gemini CLI。

### OpenSpec（社群工具）
四大標準文件：`proposal.md`、`specs/`、`design.md`、`tasks.md`
```bash
/opsx:new <功能名稱>    # 初始化
/opsx:ff                # fast-forward，自動生成規格文件
/opsx:archive           # 功能完成後歸檔
```

### 搭配 Superpowers 的完整工作流
```
1. 規格階段（spec-kit 或 OpenSpec）
   → 用 brainstorming skill 釐清需求
   → 生成 proposal + tasks.md

2. 隔離環境（Superpowers）
   → using-git-worktrees：建立獨立功能分支

3. 任務拆解
   → writing-plans：把高階任務拆成 2-5 分鐘微任務
   → 每個微任務含檔案路徑、測試步驟、驗證條件

4. 紀律化實作（Superpowers）
   → subagent-driven-development：逐一執行
   → test-driven-development：Red → Green → Refactor

5. 審查與收尾
   → requesting-code-review
   → finishing-a-development-branch + /opsx:archive
```

**Context Hygiene 技巧**：OpenSpec 把需求固化為文件後，可以清空當前 context，讓 AI 重新讀取文件再繼續執行——大幅降低幻覺和忘記初衷的機率。

## C（關鍵行動）
- [ ] 工具定位：spec-kit = 「先把規格寫清楚」；superpowers = 「不讓 AI 直接衝去寫 code」
- [ ] 新專案初始化或維護既有專案（整理現況）優先用 spec-kit
- [ ] 長時間自走任務（goal/subagent）時，規格文件是 Context 的「真相來源」

## 補充視角：角色定位比喻

來源：《AI 寫程式再進化：如何完美結合 OpenSpec 與 Superpowers》（Josh，2026-02-25）

**OpenSpec = PM（專案經理）**：透過產出提案、規格、設計和任務清單等文件，確保開發者與 AI 在寫下任何一行程式碼前，對「要做什麼」有完美共識。

**Superpowers = Tech Lead（技術主管）**：更像是嚴格的資深技術主管，強制 AI 遵守軟體工程紀律——真正的 TDD、Git Worktree 隔離、系統化除錯、Subagent 執行微型任務。

> 一個管「知道要做什麼」，另一個管「用正確方式做」。

## 補充視角：OpenSpec + Superpowers 完整五階段實踐

來源：高見龍文章（2026-02-25），提供最詳細的 Step-by-step 整合流程：

**五階段實踐要點：**
1. **需求釐清**：`brainstorming` → `/opsx:new` → `/opsx:ff`，把對話固化為單一真相來源
2. **隔離環境**：`using-git-worktrees`，確保主幹整潔、測試基準線乾淨
3. **任務細化**：OpenSpec `tasks.md`（高階）→ `writing-plans`（2-5 分鐘微任務），不是每個任務都要展開
4. **紀律化實作**：`subagent-driven-development` + `test-driven-development`（RED→GREEN→REFACTOR）；規格文件是唯一依據，防止偏離
5. **審查與歸檔**：`requesting-code-review` → `finishing-a-development-branch` → `/opsx:archive`（更新全局規格）

**搭配彈性：** 需求清楚時可跳過 `brainstorming`；簡單 bug 連 `proposal` 都不需要，直接動手。OpenSpec 的 `tasks.md` 是高階任務清單，只在複雜任務時才用 `writing-plans` 進一步展開。

## 相關連結

- [[ai-native-sdlc-playbook-anthropic]] — Anthropic 官方版本：`intent.md` → `spec.md` → `plan.md` 產物鏈，提交鏈本身即審計記錄
- [[superpowers-framework]] — Superpowers 15 個 Skill 完整解析
- [[zerospec]] — ZeroSpec 零依賴 Markdown 框架（另一種規格先行方案）
- [[agents-md-guide]] — AGENTS.md 標準，AI 理解 repo 結構的基礎
- [[harness-engineering]] — 更高層次的 AI 開發系統設計

## 補充資料：OpenSpec v1.0 OPSX 十命令系統

來源：Cash Wu（2026-02-03）《OpenSpec v1.0 大改版：從 3 個命令到 10 個命令的 OPSX 工作流程》

OpenSpec v1.0 將原本**剛性三段式**（PLANNING→IMPLEMENTING→DONE，不能回頭）重構為**彈性動作式**，10 個 OPSX 命令可隨時回頭修改：

| 命令 | 用途 |
|------|------|
| `/opsx:new <name>` | 建立新 change（只建資料夾，不產生文件） |
| `/opsx:ff` | Fast-Forward：一次產生所有規劃文件（proposal/specs/design/tasks） |
| `/opsx:continue` | 逐步建立 artifact（每次一個，適合邊做邊 review） |
| `/opsx:apply` | 執行 tasks.md 中的任務（可指定 change 名稱，支援多 change 並行） |
| `/opsx:verify` | 驗證實作是否符合 spec（偵測 drift，例如 localStorage key 不一致） |
| `/opsx:sync` | 預覽 delta specs 合併到主 specs（archive 前可選） |
| `/opsx:archive` | 歸檔完成的 change（delta specs 合入主 specs，移至 archive/） |
| `/opsx:bulk-archive` | 批次歸檔多個 change（自動偵測 spec 衝突，按時間順序合併） |
| `/opsx:explore` | 探索模式（阻止實作，專心調查問題；適合需求不明確時） |
| `/opsx:onboard` | 新手引導（用真實 codebase 走一遍完整流程，約 15 分鐘） |

**常見工作流組合：**
- 快速開發（需求明確）：`new → ff → apply → archive`
- 完整流程（需驗證）：`new → ff → apply → verify → archive`
- 探索式開發：`explore → new → continue → ... → apply`
- 逐步審查（團隊協作）：`new → continue → (review) → continue → ...`

**與舊版比較：**

| 特性 | 舊版 | 新版 OPSX |
|------|------|----------|
| 命令數量 | 3 | 10 |
| 工作流程 | 剛性階段 | 彈性動作 |
| 能否回頭 | 不行 | 可以 |
| 探索模式 | 無 | 有（阻止過早實作）|
| 批次歸檔 | 無 | 有 |
| 實作驗證 | 無 | 有（/opsx:verify）|

---

## 補充資料：opsx 的本質——結構化 prompt + 手動 RAG（2026-07-30 併入）

來源：MikeWang〈讓 AI 寫 code 前先對齊規格 / Align Before You Build〉（Medium，2026-04-10，中英雙版同內容）。
上方章節講「opsx 怎麼用」，本節補的是**「opsx 本質上是什麼」**這個角度。

### 一、spec 文件 = 結構化的 prompt

問題根源一句話：**需求只活在對話框裡**——今天說「加登入功能」，明天說「改登入流程」，AI 已忘了上次的決策，甚至做出相反方向。

| | 一般 AI coding 互動 | 用 opsx 寫成 spec |
|---|---|---|
| 指令 | 「幫我清理這專案裡被註解掉的程式碼」 | `Requirement: 系統 SHALL 識別並刪除被註解掉的程式碼，保留一般說明性註解`＋ Given/When/Then scenarios |
| 結果 | AI 猜測意圖，可能清多也可能清少 | 邊界條件明確化 |

三個差異：
1. **消除歧義**——Given/When/Then 把邊界條件明確化，AI 不需要猜
2. **可驗證性**——每個 scenario 就是一個測試案例；**spec 和 test 說的是同一件事，天然對齊**
3. **可重現性**——spec 放在 codebase 裡，任何時候重跑結果一致

> **Given/When/Then 正是 BDD 的標準語法**——opsx 把 BDD 思維帶進 AI coding，寫 spec 和寫測試案例的思路幾乎相同，只是執行者從人變成 AI。

### 二、`config.yaml` 的 context = 手動 RAG

```yaml
# openspec/config.yaml
context: |
  Tech stack: Swift, UIKit, MVVM
  API conventions: RESTful, JSON
  Testing: XCTest for unit tests
rules:
  specs:
    - Use Given/When/Then format
```

這段 context 會**注入到每一個 artifact 的 prompt**，讓 AI 生成 spec 或執行任務時始終知道專案背景——與 RAG「回答前先撈相關資料注入 context」高度相似。

差別是**手動 vs 自動**：opsx 要自己維護，不是從文件自動向量化檢索。**但好處是版控、透明、可修改——你完全掌握 AI 看到的是什麼。**

### 三、企業 iOS 實戰案例（多 App + 共用核心模組）

清理累積的註解程式碼與多餘空行，流程中三個值得學的設計：

| 設計 | 內容 |
|---|---|
| **並行策略** | 三個獨立 App 目錄分給**三個 sub-agent 同時處理** |
| **模型分級寫進 design.md** | 決策：*sub-agents 用 Sonnet 而非 Opus*；理由：*這是模式匹配任務，不需高階推理，Sonnet 足夠且成本較低* ← **把選型理由落檔，不只是當下決定** |
| **自訂 skill 封裝** | 清除 skill（掃描並刪除）＋ 驗證 skill（確認變更沒超出 spec 範圍）；**skill 可指定更輕量模型（如 Haiku）**，省下的 token 留給真正需要推理的工作 |

收尾：`/opsx:verify` 確認 tasks 完成與實作符合規格 → 再檢查 git diff 確認只刪無用內容、未動邏輯 → 才封存。

> **archive 不只是封存，它是結構化的決策歷史**——新人看 archive 就能理解每個變更的 why、how 與邊界條件，**比翻 git log 更清楚**。

### 四、什麼時候「不」適合用 opsx

| 情境 | 原因 |
|---|---|
| 快速 prototype | 需求還在摸索，spec 寫了也會馬上改 |
| 一次性 script | 用完即丟，不需要可維護的規格文件 |
| 需求極度模糊的探索期 | 先用 `/opsx:explore` 想清楚，確定方向再進流程 |
| 極簡變更（改變數名、加一行 log）| 直接改比走流程快 |

**判準：當你的變更值得被記錄、被重現、被其他人理解，opsx 才發揮最大價值。**

---

## 補充資料：SDD 方法論深度解析

來源：Cash Wu（2026-02-15）《SDD — 從 TDD 到規格驅動開發：AI 時代的延伸》、徐培鈞（2026-04-01）《SDD：用規格驅動開發，讓 AI 寫出更可靠的程式碼》、高見龍（2026-03-21）演講筆記

### SDD 的根本問題意識

**AI 擅長 How，但不知道 What 的邊界。**「幫我實作使用者註冊功能」——AI 可能給出 OAuth、簡訊驗證、email 啟動等完整實作，但你只需要最基本的帳密登入。原因不是 AI 能力不夠，而是自然語言 prompt 本質上是模糊的。

### 規格文件的四要素

一份有效的 SDD 規格文件應包含：
1. **功能範圍**（Feature Scope）：這個功能做什麼
2. **驗證規則**（Validation Rules）：具體的邊界條件
3. **API 設計**（API Design）：class/method/return type
4. **不在範圍內**（Out of Scope）：**明確說清楚不做什麼**（防止 AI 腦補）

### 開發流程演化

```
傳統：需求 → 設計 → 實作 → 測試
TDD：需求 → 測試 → 實作 → 重構
SDD + TDD：需求 → 規格 → 測試 → AI 實作 → Review
```

SDD 是 TDD 的**前置步驟**，不是替代。規格在 AI 時代扮演「導航系統地址」的角色——不是「帶我去好餐廳」（太模糊），而是具體的目的地座標。

### SDD vs Plan Mode

| 面向 | Plan Mode | SDD |
|------|-----------|-----|
| 存在形式 | 對話內的暫時狀態 | 文件（落地在版本控制中）|
| Context 消耗完後 | 需重新說明 | 直接讀文件接著做 |
| 換 AI 工具 | 需重新說明 | 文件帶著走 |
| 任務切換 | 需要重建 | 規格錨定，隨時切換 |

**關鍵洞察**（高見龍）：套件和 Git 的寫入權限不要給 AI——讓 AI 管規格和實作，人管依賴和版本。

### 規格生命週期

每個規格的生命週期：**討論 → 提案 → 實作 → 更新 → 歸檔**

工具推薦：openspec（首選）、Kiro（規格管理）、speckit（GitHub 官方）、Spectra（向量 RAG + Claude 查詢）
