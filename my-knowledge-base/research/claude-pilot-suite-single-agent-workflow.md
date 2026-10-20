---
title: claude-pilot-suite 單一 Agent 開發流程設計
date: 2026-06-01
tags: [ai/claude, ai/agent, ai/workflow]
source: https://github.com/zeuikli/claude-pilot-suite
type: reference
status: processed
---

評估 claude-pilot-suite 在「只用 Claude 一個 agent」情境下，如何透過 model tier 智慧路由（Haiku/Sonnet/Opus）搭配 ZeroSpec（Layer 0 上下文整備）優化現有三模式開發流程，達成省額度 + 品質保證的雙重目標。

## claude-pilot-suite 是什麼

一套 Claude Code 的執行框架（MIT 授權），核心理念：**prompt structure + sub-agent delegation > 盲目用最強模型**。不靠多 provider 協作，而是在 Claude 生態內用三種 model tier 智慧切換。

研究根據：AgentOpt（arxiv 2604.06296）實驗顯示，Opus 單獨跑 HotpotQA = 31.71%，但弱規劃器 + 強求解器的分層委派 = 74.27%。

## 三種 Pilot 模式

### Haiku Pilot（省錢優先，預設模式）

**哲學**：弱規劃器 + 強委派 > 強規劃器包辦一切

- 預設用 Haiku 4.5 執行所有任務
- 遇到量化觸發條件才升級：
  - 同問題失敗 ≥ 3 次 → Sonnet
  - 任務涉及 ≥ 10 個檔案 → Sonnet
  - 架構/設計/安全關鍵字 → Opus 或 `advisor()`
  - 程式碼產出 > 300 LoC 未經要求 → 暫停，重新分配給 Sonnet
- **預期省 70-85%**（假設 80/15/5 任務分佈）

**品質護欄**：
1. Reference Pattern（指向具體檔案，不用抽象描述）
2. Think-Before-Coding（假設揭露）
3. Diff-Review Pledge（`git diff --stat` 逐檔說明）
4. Citation Anchor（結構化引用，hard task ≥ 5 anchors/段落）
5. Source-Verify Loop（`grep` 驗證每個引用數字）

### Sonnet Pilot（品質優先）

**哲學**：深度 context engineering + 按需 Opus advisor ≈ Opus 品質

- 7 項 pre-flight 強制檢查（Reference Pattern、Decision-Log、Reasoning Chain、Self-Review Loop、Intermediate Checkpoint、Mid-Write Outline Verification、Line-Level Source-Verify）
- Sonnet 的高信心偏差是雙面刃——容易跳過自我審查，pre-flight 是外掛的「懷疑機制」
- 升級到 Opus 的門檻比 Haiku 更嚴（因為 Opus 是 5 倍成本）
- **預期省 55-65%**

### Opus Pilot（天花板提升）

**哲學**：Opus 是天花板，不往上升級，只往下委派

- 5 個天花板提升機制：
  1. Reverse-Advisor Loop — Opus 先自我批評再定稿
  2. Parallel Hypotheses Synthesis — 產生 N 個競爭假說，合併最強元素
  3. CAR Explicitness — 每個主張錨定 Context/Action/Result
  4. Decision-Log Externalization — 決策日誌外化
  5. Meta-Harness Filesystem Loop — agentic 任務寫計畫檔 + 逐步驗證
- 3 個 Opus 專屬 pre-flight：歧義浮現、先簡後繁、向下委派評估

## Sub-Agent 委派策略

不是跨 provider 委派，而是 **Claude 內部 model tier 委派**：

| 場景 | Agent 類型 | 建議 Model |
|------|-----------|-----------|
| 搜尋 / > 10 檔案 / 定位符號 | `Explore` | Haiku 4.5 |
| 單檔手術式修改 ≤ 30 LoC | `general-purpose` | Haiku 4.5 |
| 跨模組 / > 30 LoC / 需設計判斷 | `general-purpose` | Sonnet 4.6 |
| 測試撰寫 | `general-purpose` | Sonnet 4.6 |
| 架構決策 / 實作計畫 | `Plan` | Opus 4.7 |

委派觸發條件：
- 讀取 ≥ 10 檔案
- 預期工具呼叫 > 20 次
- 可分解為 ≥ 3 個獨立子任務
- 任務類型 ∈ {research, security review, architecture decision}

## 安裝方式

```bash
git clone https://github.com/zeuikli/claude-pilot-suite.git
ln -s "$(pwd)/claude-pilot-suite/rules" ~/your-project/.claude/rules-pilot
ln -s "$(pwd)/claude-pilot-suite/skills/haiku-pilot" ~/your-project/.claude/skills/haiku-pilot
ln -s "$(pwd)/claude-pilot-suite/skills/sonnet-pilot" ~/your-project/.claude/skills/sonnet-pilot
ln -s "$(pwd)/claude-pilot-suite/skills/opus-pilot" ~/your-project/.claude/skills/opus-pilot
```

需自訂兩處：
1. `rules/core.md` — Production Safety Red Line（替換為實際的 production 標記）
2. `rules/subagent-strategy.md` — Sub-Agent Dispatch Table（替換為實際的 agent）

## 與現有開發流程的搭配

### 前提：Claude Pro $20 的額度特性

Claude Pro 訂閱的額度是「對話次數」而非「token 計費」，但不同 model tier 消耗的額度權重不同。claude-pilot-suite 的價值在於：
- 預設 Haiku → 低權重消耗
- 只在必要時升級 → 減少高權重消耗次數
- Sub-agent 委派 → 把大任務拆小，每個子任務用最便宜能勝任的 tier

### 現有三模式 → 單一 Claude 優化版

> **實際使用**：日常以模式 1 和模式 2 為主。模式 3 為參考備案，僅在必要時啟用。

#### 模式 1：純手寫（不變，主力）

直接手動修改，不消耗 Claude 額度。

#### 模式 2：需求驅動開發（主力）

```
Before:
/requirements → Claude 開發 → /stack-review → /doc-update

After（ZeroSpec + DESIGN.md + Haiku Pilot + planning-with-files）:
〔UI 需求才有〕figma-to-requirements（全域 skill，讀專案 DESIGN.md
       → token 對應表 + 狀態矩陣 + 待新增 token 清單 → UI 需求草稿）
ZeroSpec AGENTS.md ＋ DESIGN.md（Layer 0，專案上下文 + UI 設計上下文常駐）
→ /requirements（Layer 1，補功能面；UI 需求接續上面 figma-to-requirements 的草稿）
→ /plan（Layer 2，建立 task_plan.md）
→ haiku-pilot 開發（預設 Haiku，讀 AGENTS.md 任務路由 + DESIGN.md token/元件對應找對入口）
  → hooks 自動注入計畫 + 追蹤進度（progress.md）
  → 觸發升級條件時自動切 Sonnet
  → /stack-review → /doc-update
```

| 環節 | Pilot 模式 | ZeroSpec + planning-with-files 角色 |
|------|-----------|-----------------------------------|
| 專案上下文 | — | ZeroSpec AGENTS.md 常駐（任務路由 + 架構邊界 + 版本真相） |
| UI 設計上下文 | — | DESIGN.md 常駐（色彩/文字/間距 token + Figma→Flutter 元件對應）|
| UI 前置（UI 需求才有） | Haiku Pilot | figma-to-requirements 讀 DESIGN.md → token 對應+狀態矩陣 → UI 需求草稿 |
| requirements 產出 | Haiku Pilot | AI 讀 AGENTS.md 理解專案後產出更精準的需求（UI 需求接續草稿補功能面） |
| 建立計畫 | Haiku Pilot | `/plan` 建立 task_plan.md + findings.md + progress.md |
| 功能實作 | Haiku Pilot → 自動升級 | UserPromptSubmit hook 每次注入計畫；PostToolUse 提醒更新進度 |
| context 壓縮時 | — | PreCompact hook 保存狀態，壓縮後自動恢復 |
| 錯誤處理 | — | 3-Strike Error Protocol 追蹤嘗試，不重複失敗 |
| stack-review | Sonnet Pilot | reviewer 可讀 progress.md 了解完整開發過程 |
| doc-update | Haiku Pilot | — |

**ZeroSpec + planning-with-files + pilot-suite 三層互補**：
- ZeroSpec 負責「AI 看得懂專案」（上下文整備，Layer 0）
- pilot-suite 負責「用哪個 model tier」（成本控制）
- planning-with-files 負責「開發狀態不遺失」（執行追蹤，Layer 2）
- 三者沒有衝突——ZeroSpec 是靜態 Markdown，pilot-suite 的 pre-flight 和 planning-with-files 的 hooks 各管各的

**預估額度節省**：比全程 Sonnet/Opus 省 60-70%

#### 模式 3：複雜開發（參考備案）

> 原本模式 3 使用 `/ai-pair dev` 進行多模型交叉審查（GPT + Claude + Gemini），但單一 Claude 環境下無法呼叫其他 provider。此處用 pilot-suite 的結構化自我審查機制替代多模型審查，作為備案參考。

> **ai-pair v4.3 註記（2026-06-18 生效）：** gemini-reviewer 底層 CLI 已從 `gemini` 遷移至 **Antigravity CLI（`agy`）**（Google 停止 Gemini CLI 免費服務）。角色名稱 `gemini-reviewer` 保留，模型走 `Gemini 3.1 Pro (High)`（深審）/ `Flash (Low)`（快掃）。三個 reviewer 共讀 Team Lead 組的**單一 REVIEW_FILE**（不各自讀 source），是 token 節省的關鍵設計。詳見 b2b-manager `docs/ai-collaboration/dev-workflow-handoff-token-strategy.md`。

```
原本多 Agent 版（ai-pair v4.3，gemini 底層為 agy）:
/requirements → /ai-pair dev → GPT(copilot) + Gemini(agy) + Claude review → /doc-update

單一 Claude 替代版（ZeroSpec + DESIGN.md + Sonnet Pilot + planning-with-files）:
〔UI 需求才有〕figma-to-requirements（讀專案 DESIGN.md → token 對應+狀態矩陣 → UI 需求草稿）
ZeroSpec AGENTS.md ＋ DESIGN.md（Layer 0，架構邊界 + 版本真相 + 治理規則 + UI 設計上下文常駐）
→ /requirements（補功能面，UI 需求接續草稿）→ /plan（建立 task_plan.md）
  → sonnet-pilot 開發（含 Decision-Log + Reasoning Chain，讀 AGENTS.md 遵守治理規則、對著 DESIGN.md token 實作 UI）
  → hooks 持續追蹤進度 + 發現寫入 findings.md
  → Self-Review Loop（git diff + 逐檔說明）
  → advisor()（Opus 做架構審查，可參考 AGENTS.md 架構邊界 + findings.md）
  → /stack-review
  → /doc-update
```

| 環節 | Pilot 模式 | planning-with-files 角色 |
|------|-----------|------------------------|
| requirements 產出 | Sonnet Pilot | — |
| 建立計畫 | Sonnet Pilot | `/plan` 建立多階段計畫，含 Key Questions + Decisions Made |
| 架構決策 | Opus（via advisor） | advisor 可讀 findings.md 取得完整研究脈絡 |
| 功能實作 | Sonnet Pilot | hooks 追蹤每個 phase 完成度，errors 表格記錄所有失敗 |
| 自我審查 | Sonnet Pilot Self-Review | progress.md 提供完整變更歷史供 review 參考 |
| doc-update | Haiku Pilot | — |

**品質保證方式差異**：從「三家模型各做一次，人工判斷誰對」變成「結構化 7 項 pre-flight 自我檢查 + Opus 顧問 + 檔案式進度追蹤」。

**預估額度節省**：比全程 Opus 省 55-65%

**啟用時機**：涉及跨模組架構重構、安全關鍵修改、或模式 2 反覆失敗需要更高品質保證時。

#### 模式 4（可選）：深度分析/架構設計

```
opus-pilot → Reverse-Advisor + Parallel Hypotheses
→ 子任務委派給 Sonnet/Haiku sub-agent
→ Opus 整合結論
```

僅用於：架構決策、counter-factual 推理、跨多來源知識整合、威脅建模。

### 完整分層架構：ZeroSpec + planning-with-files + pilot-suite

三個工具分別解決不同層級的問題，組合成完整的單一 Agent 開發堆疊：

```
Layer 0：ZeroSpec（上下文整備）— AGENTS.md 常駐，AI 進 repo 就懂專案
        ＋ DESIGN.md（UI 設計上下文 — token / Figma→Flutter 元件對應，UI 工作必讀）
Layer 0.5（UI 需求才有）：figma-to-requirements — 截圖讀 DESIGN.md → token 對應+狀態矩陣 → 餵 Layer 1
Layer 1：/requirements（需求規格）— 基於 Layer 0 的理解產出精準需求
Layer 2：/plan + planning-with-files（執行追蹤）— task_plan.md + progress.md + findings.md
Layer 3：pilot-suite（成本控制 + 品質保證）— model tier 路由 + pre-flight 檢查
Layer 4：/stack-review → /doc-update（收尾）
```

| 層級 | 工具 | 負責什麼 | 性質 |
|------|------|---------|------|
| 上下文整備層 | ZeroSpec | AI 看得懂專案（架構邊界、任務路由、版本真相、治理規則） | 靜態，一次設定 |
| 成本控制層 | claude-pilot-suite | 用哪個 model tier（Haiku/Sonnet/Opus） | 動態，自動切換 |
| 執行追蹤層 | planning-with-files | 開發狀態不遺失（task_plan.md + progress.md + findings.md） | 動態，hooks 驅動 |
| 品質保證層 | pilot-suite pre-flight | 結構化自我檢查（7 項 pre-flight + Self-Review Loop） | 每次輸出觸發 |

**三層互補**：ZeroSpec 是靜態基底（Markdown，不消耗額度），pilot-suite 的 pre-flight 檢查專注「輸出品質」，planning-with-files 的 hooks 專注「過程追蹤」。三者的作用機制不同（AGENTS.md 自動載入 / rules 層 / skill hooks），不會衝突。

**單一 Agent 中 ZeroSpec 的特殊價值**：多 Agent 方案有 dotbabel 同步 AGENTS.md、有 agentmemory 共享記憶，但單一 Agent 只有自己——ZeroSpec 的任務路由表讓 Haiku Pilot 在預設低 tier 時也能快速找對入口檔案，減少因「找不到入口而觸發升級」的額度浪費。

**單一 Agent 中 planning-with-files 的特殊價值**：沒有其他 agent 可以幫你記住進度，context 壓縮後只有 planning files 能恢復狀態。相比多 Agent 方案（可以靠 agentmemory 共享記憶），單一 Agent 更依賴 planning-with-files 的 PreCompact hook。

### 與多 Agent 方案的比較

| 維度 | 多 Agent（dotbabel + CCB） | 單 Claude（pilot-suite） |
|------|--------------------------|------------------------|
| Layer 0 | ZeroSpec + dotbabel 同步 AGENTS.md 到所有 CLI | ZeroSpec AGENTS.md（Claude 獨享） |
| 額度來源 | 分散到 Codex/Antigravity（免費） | 集中在 Claude，但用 tier 路由省錢 |
| 額度節省 | 60-70%（靠免費 agent 吃重活） | 55-85%（靠預設 Haiku + 升級門檻） |
| 品質保證 | 交叉驗證（多模型各做一次） | 結構化 pre-flight + Self-Review Loop |
| Handoff 複雜度 | 高（需要 context 傳遞，但 ZeroSpec 降低門檻） | 低（同一 Claude 生態，sub-agent 共用 context） |
| 執行追蹤 | planning-with-files（多 agent 共享） | planning-with-files（單 agent 自用） |
| 設定成本 | 高（dotbabel + CCB + 多 CLI） | 低（symlink 即裝好） |
| 適合場景 | 大量實作 + 閱讀密集 | 品質敏感 + 推理密集 |

### 混合策略建議

兩種方案不互斥，可以混合使用。ZeroSpec + planning-with-files 是共通基礎層：

```
所有開發模式共通基底 → ZeroSpec（Layer 0）+ planning-with-files（Layer 2）
  AGENTS.md 提供靜態專案上下文，/plan 建立動態執行追蹤

日常開發（量大、token 密集）→ 多 Agent 方案
  Codex 實作 + Antigravity 搜尋 + Claude review
  （AGENTS.md = 跨 agent 共同基礎知識，planning files = 跨 agent 進度同步）

品質關鍵任務（推理密集）→ claude-pilot-suite
  Sonnet Pilot 主導 + Opus advisor
  （AGENTS.md = Haiku 也能精準找對入口，planning files = context 壓縮後狀態恢復）

知識庫整理（本 vault）→ claude-pilot-suite
  Haiku Pilot 處理 raw-pipeline（結構化任務）
  Sonnet Pilot 處理 knowledge-synthesizer（需品質保證）
```

## 2026-06-23 更新：UI 開發前置（figma-to-requirements + DESIGN.md）

單一 Claude 環境下，UI 類需求（來自 Figma 截圖）同樣新增前置階段，插在 `/requirements` 之前：

```
docs/assets/<功能>/*.png
  → figma-to-requirements（全域 skill，讀專案 DESIGN.md → token 對應+狀態矩陣）
  → /requirements
  → haiku-pilot / sonnet-pilot 開發（對著 DESIGN.md 實作）
  → /stack-review → /doc-update
```

| 變更 | 說明 |
|------|------|
| **DESIGN.md = Layer 0 設計上下文** | 與 AGENTS.md 並列的靜態環境檔。對 Haiku Pilot 尤其有價值——預設低 tier 時，DESIGN.md 直接給出 token 詞彙與元件對應，**減少因「不確定用哪個元件/顏色」而觸發升級**的額度浪費（同 ZeroSpec 任務路由表「讓 Haiku 也能精準找對入口」的邏輯）|
| **figma-to-requirements** | 全域 skill，讀截圖所屬 repo 的 DESIGN.md，跨專案通用 |
| **取代 figma-to-flutter** | 「截圖→需求→實作」三段；精確度來自 DESIGN.md token 而非工具。figma-to-flutter 已退役歸檔 |
| **pilot tier 對應** | figma-to-requirements 屬視覺對應任務，Haiku Pilot 即可勝任；複雜多畫面 flow 可升 Sonnet |

> 與 pilot-suite 三層的關係：DESIGN.md 補進 Layer 0（上下文整備）的「UI 設計」這一塊，與 AGENTS.md 同為靜態基底；figma-to-requirements 是 Layer 1 的視覺前處理器。兩者都不消耗額外推理額度（靜態檔 + 結構化對應）。

## 相關連結

- [[zerospec]] — ZeroSpec Layer 0 上下文整備框架（AGENTS.md + 任務路由）
- [[spec-driven-ai-workflow]] — 規格驅動 AI 開發工作流（spec-kit / OpenSpec）
- [[dev-workflow-optimization-tools]] — 多 Agent 方案（dotbabel/CCB/Matt Pocock Skills/planning-with-files）
- [[dual-tool-strategy]] — Claude + Antigravity 雙工具分工
- [[llm-cost-optimization-strategies]] — LLM 成本優化策略總覽
- [[archive/figma-to-requirements-ui-dev-workflow]] — UI 截圖→需求→實作工作流完整設計（已實作歸檔）
- [[concepts/agent-handoff-memory-patterns]] — HANDOFF/MEMORY 五模式 + ai-pair REVIEW_FILE token 機制
