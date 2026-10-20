---
title: mattpocock/skills：真實工程師的 Claude Skills 套件
date: 2026-05-20
tags: [ai/claude, ai/agent]
source: https://github.com/mattpocock/skills
source_2: https://muki.tw/skills-for-real-engineers-introduce/（Superpowers 整合方案，2026-08-14）
type: article
status: processed
asset_value: 45
---

Matt Pocock（TypeScript 大師，60,000+ newsletter 訂閱）開源的 Claude Skills 套件，針對 AI coding 的四大失敗模式設計解法：需求錯位 / 過度冗長 / 程式碼不能跑 / 架構變泥球。Small、composable、基於數十年工程師經驗，支援任何模型。

## T（核心論點）

GSD、BMAD、Spec-Kit 等框架試圖掌控整個開發流程，但這奪走了你的控制權，讓 bug 難以排查。Matt Pocock 的 Skills 設計哲學相反：small、easy to adapt、composable。你組合它們，不是被它們指揮。**軟體工程基本功在 AI 時代比以往更重要。**

## B（適用場景）

**適用：** 使用 Claude Code 做真實工程專案（非 vibe coding）的開發者；需要 TDD 紀律的 AI 開發工作流；想對齊 AI 理解需求再動工的場景
**不適用 / 注意：** 需要先用 `/setup-matt-pocock-skills` 初始化設定（issue tracker、標籤詞彙表）；部分 skills 依賴 CONTEXT.md 和 ADR 文件

## R（四大問題與對應 Skills）

### 問題 1：Agent 沒做我要的（需求錯位）
**解法**：每次改動前必用 `/grill-me` 或 `/grill-with-docs`
- `/grill-me`：強迫 AI 問你詳細問題，直到決策樹每個分支都解決
- `/grill-with-docs`：同上，額外幫你建立 CONTEXT.md（共同術語表）和 ADR

### 問題 2：Agent 太囉嗦（冗長）
**解法**：`/grill-with-docs` 建立 CONTEXT.md 共同語言
- 專案術語表讓 AI 用 1 個詞代替 20 個詞
- 副作用：變數/函式/檔案命名更一致，AI 思考 token 更少

### 問題 3：程式碼不能跑（回饋迴路不足）
**解法**：`/tdd` skill（紅-綠-重構）
- 先寫失敗測試，看它失敗，寫最少程式碼讓它通過，提交
- `/diagnose`：除錯用的結構化診斷迴圈（reproduce → minimise → hypothesise → fix → regression-test）

### 問題 4：架構成了泥球（軟體熵加速）
**解法**：每幾天跑一次 `/improve-codebase-architecture`
- `/zoom-out`：讓 AI 以整個系統的視角解釋陌生程式碼
- `/to-prd`：改動前先問你要動哪些模組，再建立 PRD

## C（關鍵行動）

- [ ] 安裝：`npx skills@latest add mattpocock/skills`，選取技能後執行 `/setup-matt-pocock-skills`
- [ ] 每次改動前先用 `/grill-me` 或 `/grill-with-docs` 對齊需求
- [ ] 建立 TDD 習慣：`/tdd` skill 作為預設開發模式

## 原始摘要

完整 Skills 分三類：Engineering（diagnose、grill-with-docs、triage、improve-codebase-architecture、setup-matt-pocock-skills、tdd、to-issues、to-prd、zoom-out、prototype）/ Productivity（caveman、grill-me、handoff、write-a-skill）/ Misc（git-guardrails、migrate-to-shoehorn、scaffold-exercises、setup-pre-commit）。

## 相關連結

- [[claude-skills-ecosystem]] — GitHub 高 star Skills 生態全覽
- [[superpowers-framework]] — 類似定位的完整 AI 開發工作流框架（更強制性）
- [[agent-skills-toolkit]] — agent-skills 22 個 Skill 開發完整生命週期
- [[addyosmani-engineering-skills]] — Addy Osmani 的工程師 Skills 套件（另一個工程師視角）

## 補充資料（來源：DeTools 工具翼零，《Skills For Real Engineers》，2026-05-18）

### Workflow 導向 vs 語言導向的本質差異

Matt Pocock Skills 的核心定位不是「TypeScript 工具」，而是「AI 開發工作流工具箱」：不侷限於特定語言或框架，而是把實務開發 workflow 封裝成可重複使用的技能。

**涵蓋場景**：程式碼重構、Debug 與錯誤修復、文件與 README 撰寫、專案分析與規劃、測試生成、UI 與 component 開發、API 與資料處理 workflow、Git 與開發流程操作。

**與 Karpathy Skills 的差異**：
- Andrej Karpathy Skills：工程思維與 AI **行為約束**（告訴 AI 不能做什麼）
- Matt Pocock Skills：開發 **workflow 模式**（告訴 AI 怎麼做事）

### AI 開發三層架構中的定位

DeTools 的整理讓 Matt Pocock Skills 在 AI 開發棧中有明確位置：

| 層次 | 工具 | 功能 |
|------|------|------|
| **Skills**（怎麼工作）| Matt Pocock / Superpowers | 定義 AI 如何完成任務的工作流程 |
| **Rules**（能做什麼）| CLAUDE.md / AGENTS.md rules | 定義 AI 的邊界和禁止事項 |
| **Style**（應該怎麼寫）| Impeccable Style | 定義程式碼風格與設計語言 |

三者互補，可同時使用：Skills 提供工作流模式 → Rules 設定護欄 → Style 確保一致性。

**Skills 與一次性 prompt 的核心差異**：Skills 是跨任務、跨 session 可重複使用的工作模式，AI 在不同任務中自動載入對應 Skill 並按規則執行，而非每次重新教導。

## 補充資料（來源：https://muki.tw/skills-for-real-engineers-introduce/ ，2026-08-14）

第三來源，補上**與 Superpowers 併用的具體整合方案**與若干原條目未收的細節。⚠️ 該篇記 repo star 數為 **89k**（2026-08 觀察值，原條目未記 star 數）。

### 🔑 定位對比：一條龍 vs 工具箱

- **Superpowers** — 一條龍，從「需求 → 設計 → 拆任務 → TDD 實作 → review → merge」完全接管，強調**一整套一致的流程**
- **Skills for Real Engineers** — 軟體工具箱，強調**關鍵環節的品質與軟體工程**

作者對「大流程框架」（GSD／BMAD／Spec-Kit）的批評很具體：**正因為太大，出問題時只知道流程卡卡的，卻很難精準知道是哪一步出事；就算知道也很難只改那一小塊，因為它是一條龍幫你弄好的。**

🔑 **本篇最好的一句類比：這套 skills 像是 AI 開發版的 Unix——一堆小工具各自做好一件事，再讓我們用自己的方式拼裝起來。**

### 四點整合方案（Superpowers × mattpocock）

1. **在 Superpowers 前面加一層 `grill-with-docs` 做對標** — Superpowers 的 `brainstorming` 重點是「把 spec 挖出來 → 切成可執行 plans」，與 `grill-with-docs` 那種**瘋狂問問題**的能力不同。先跑 grill-with-docs 長出 `CONTEXT.md` 與 ADR，再交給 Superpowers 的 `brainstorming`／`writing-plans`，**規劃時就有穩定的專案語言與決策背景可參考，不用每次從 session 對話了解起**
2. **讓 Superpowers 接手大流程，實作細節補 TDD 準則** — 不硬插手它的 `test-driven-development`／`writing-plans`／`executing-plans`／`subagent-driven-development`，但補三條要求：測試盡量用 `CONTEXT.md` 的用語命名／優先寫像規格的 **integration-style 測試**／**以一測一實作的 vertical slice 心態調整 plan 顆粒度，防止一次寫一整片假規格的測試**。也可在呼叫時直接補一句「遵守 mattpocock/skills 的 `tdd` 準則」
3. **迭代前後做體檢** — 碰到解不開的 bug 暫時離開主流程用 `/diagnose`（reproduce → minimize → hypothesize → instrument → fix → regression-test）；每個迭代收尾前跑一次 `/improve-codebase-architecture` 掃一遍
4. **心態** — Superpowers 掌控大框架流程，matt 的 skills 是穿插其中的 plugins（可獨立運作也可整合）。🔑 **有了這個分層，就不會被任何一個大框架控制住——維持開發者對設計與架構的主導權，同時讓 agent 穩定在框架下執行**

### 一鍵安裝的代價

⚠️ 作者明確反對無腦一鍵安裝：**「一鍵安裝帶來的熵增也高，你沒辦法很好地領略每個 Skill 代表的意義，就很難有效直擊痛點，或呼叫對的 Skill。」** 建議安裝前先評估「它能優化我現有工作流的哪一階段」。（同 [[superpowers-skill-bloat-risk-tiering]]、[[skill-decision-what-to-automate]] 的取捨）

### CONTEXT.md 的共享語言：一個具體例子

`grill-with-docs` 產出的術語表威力，README 的例子最清楚：

- **Before**：「當課程裡某一個章節的某一堂課被變成『真正的』課程（在檔案系統有實體位置）時會出問題」
- **After**：「**materialization cascade** 出問題」

🔑 **從此 AI 看到這幾個字就知道你指的是前面那整段描述。** 這解決的是原條目四大問題中的「沒有共同語言」——腦中想的「使用者」、DB schema 的 user table、程式碼裡的 User／Member class 可能是不同東西；**名字相同的不一定是同一個概念，名字不同的也可能在指同一件事**。沒有共同詞彙表時，AI 只能自己猜。

另一半產出是 **ADR（Architecture Decision Records）**：只記錄那些「難以回頭、需要上下文解釋、且是多方案中做過取捨」的決策——選了什麼、為什麼、捨棄了哪些替代做法。

⚠️ `grill-with-docs` 與 `grill-me` 功能相同但更強大，**擇一安裝即可**（後者是非程式版，適合策略討論與產品構想）。其他日常推薦：`to-prd`（對話轉 PRD）／`to-issues`（PRD 拆 issue 並打上 `ready-for-agent` 等標籤）／`caveman`（極度壓縮語句省 token）／`handoff`（對話收斂成交接文件）。

### 收束

> **AI 的加速，除了加速寫 code 之外，可能還會加速我們做錯事情；但同時透過 AI，也可以幫我們把對的習慣變得更容易堅持。**

作者的觀察：**Matt Pocock 並沒有把人類工程師從流程中摘除，而是把「好的工程習慣」拆成一個個獨立的小 skill，讓 AI 幫我們守住底線。**
