---
title: 開發流程優化工具評估：dotbabel、Matt Pocock Skills、Mem0、Claude Squad、CCB
date: 2026-06-01
updated: 2026-08-05
tags: [ai/agent, ai/workflow, ai/claude]
source: https://github.com/kaiohenricunha/dotbabel, https://github.com/mattpocock/skills, https://github.com/mem0ai/mem0, https://github.com/smtg-ai/claude-squad, https://github.com/SeemSeam/claude_codex_bridge
type: reference
status: processed
---

評估多個開源工具在「Claude Pro $20 + Copilot Pro $10 + Codex Free + Antigravity Free」預算限制下，如何優化現行開發流程（見 [[decision-framework]] 的五情境），核心目標是額度分散與 agent 間 handoff。

## 工具定位總覽

| 工具 | 本質 | 授權 | 適用度 |
|------|------|------|--------|
| ZeroSpec | Layer 0 上下文整備框架（AGENTS.md + 任務路由） | MIT | ⭐⭐⭐⭐⭐ |
| dotbabel | 跨 CLI 治理框架（統一規則、Skills、handoff） | MIT | ⭐⭐⭐⭐ |
| Matt Pocock Skills | Claude Code 高品質 Skill 集合（TDD、診斷、handoff） | 開源 | ⭐⭐⭐⭐ |
| Mem0 | AI 跨 session 持久記憶層 | Apache 2.0 | ⭐⭐ |
| Claude Squad | 多 agent 平行執行管理器（tmux + worktree） | AGPL-3.0 | ⭐⭐⭐ |
| claude_codex_bridge (CCB) | 多 provider 混合團隊工作空間（tmux pane） | 開源 | ⭐⭐⭐⭐ |

## 各工具詳細分析

### 0. ZeroSpec — Layer 0 上下文整備框架

**來源**：https://github.com/corey924/ZeroSpec（MIT）

**做什麼**：在任何 AI workflow 開始前，把最小必要的專案上下文固定到可預測位置。核心命題：**AI 答案「幾乎對但不完全對」，66% 原因是上下文不完整，不是模型不夠強**。換模型解決不了隱性規則和團隊默契的缺口。

**四類最小必要上下文**：

| 類別 | 說明 | 解決什麼問題 |
|------|------|------------|
| 專案定位與架構邊界 | 哪些模組做什麼、邊界在哪 | AI 改到不該改的地方 |
| 版本真相來源 | 哪份文件是最新依據 | AI 引用舊 API 或廢棄寫法 |
| 領域→程式碼導航 | 任務路由（「新增 API → 看哪個檔案」） | AI 找不到入口亂猜 |
| 文件治理規則 | 哪些是底線 vs 哪些是習慣 | AI 不知道什麼可以改、什麼不能改 |

**層次定位**：
- ZeroSpec = **Layer 0**（Context Readiness，所有 workflow 開始前）
- spec-kit / OpenSpec / `/requirements` = Layer 1（SDD 規格驅動，在 ZeroSpec 之上）
- planning-with-files = Layer 2（執行追蹤，在規格之上）
- ZeroSpec 不取代 Layer 1 工具，而是讓 Layer 1 更容易接上

**與 dotbabel 的關係**：
- 重疊：都涉及 AGENTS.md 維護
- 差異：ZeroSpec 偏「專案特定上下文」（架構邊界、任務路由、版本真相），dotbabel 偏「跨 CLI 規則同步」（統一規則地板 + handoff）
- 互補：ZeroSpec 整理好 AGENTS.md 內容，dotbabel 負責把它同步到所有 CLI

**對現有流程的價值**：
- `/requirements` 產出的需求品質取決於 AI 對專案的理解深度——ZeroSpec 保證這個基礎
- Codex handoff 時，Codex 讀到的 AGENTS.md 包含任務路由表，不需要額外說明「改這個功能要看哪些檔案」
- 一次設定持續受益，不消耗額度，純 Markdown

### 1. dotbabel — 跨 CLI 治理框架

**做什麼**：model-agnostic 的治理工具包，為 Claude Code、Codex、Gemini（Antigravity）、Copilot 提供統一的規則層。核心理念是「全域規則地板」——所有 agent 共用一套最佳實踐。

**核心功能**：
- **跨 CLI 規則同步**：維護一份 AGENTS.md，自動同步到所有 CLI 工具
- **Skills 庫**：`/ground-first`、`/fix-with-evidence`、`/aws-specialist` 等可重用工作流
- **Handoff 機制**：Claude ↔ Codex 之間的上下文傳遞，支援安全上傳
- **治理 CLI**：`dotbabel doctor` 診斷、指令漂移偵測、spec 驗證
- **User-scope overlays**：`~/.config/dotbabel/local-rules.md` 個人規則疊加

**安裝**：兩條路徑——`./bootstrap.sh`（零 npm 依賴）或 `npm i -g @dotbabel/dotbabel`

**對現有流程的價值**：
- 解決多 agent 規則不一致問題（現在 CLAUDE.md 規則只有 Claude 看得到）
- handoff 機制讓模式 2（需求驅動）可以安全地把實作交給 Codex

### 2. Matt Pocock Skills — 工程實戰 Skill 集合

**做什麼**：TypeScript 教育者 Matt Pocock 打造的 Claude Code Skills，針對 AI 開發常見失敗模式設計。

**核心 Skills**：

| 類別  | Skill                            | 用途                    |
| --- | -------------------------------- | --------------------- |
| 工程  | `/tdd`                           | 紅-綠-重構循環的測試驅動開發       |
| 工程  | `/diagnose`                      | 結構化 debug 循環，針對困難 bug |
| 工程  | `/grill-with-docs`               | 需求釐清 + 領域語言文件化        |
| 工程  | `/to-prd`、`/to-issues`           | 計畫轉 GitHub Issues     |
| 工程  | `/improve-codebase-architecture` | 識別架構改善機會              |
| 生產力 | `/caveman`                       | 超壓縮溝通，省 75% token     |
| 生產力 | `/handoff`                       | 對話壓縮，用於 agent 間交接     |
| 生產力 | `/grill-me`                      | 訪談式規劃釐清               |

**安裝**：`npx skills@latest add mattpocock/skills`，然後 `/setup-matt-pocock-skills` 設定

**對現有流程的價值**：
- `/caveman` 直接省 Claude Pro 額度（75% token 壓縮）
- `/tdd` 可作為模式 2 的開發方法升級
- `/handoff` 讓 Claude → Codex 的上下文傳遞更有效率
- `/grill-with-docs` 可強化 `/requirements` 前的需求釐清

### 3. Mem0 vs agentmemory — AI 持久記憶層比較

兩者都是跨 session、跨 agent 的持久記憶方案，但架構和適用場景差異很大。

#### Mem0

- **來源**：https://github.com/mem0ai/mem0（Apache 2.0）
- 多層記憶（user / session / agent state）
- Hybrid search（語意 + BM25 + 實體匹配）
- 支援 Claude Code、Cursor、Windsurf 的 agent skill
- CLI 工具 + 瀏覽器擴充套件
- 自架或雲端部署（app.mem0.ai）
- 需要外部向量資料庫（Qdrant / pgvector）

#### agentmemory

- **來源**：https://github.com/rohitg00/agentmemory（MIT）
- 基於 Karpathy LLM Wiki 模式的實作，擴展了信心評分、生命週期管理、知識圖譜
- 完全本地（SQLite + iii-engine），**零外部資料庫依賴**
- 12 個自動 hooks（零人工介入捕捉記憶）+ 53 個 MCP tools
- 4 層記憶整合 + 衰減 + 自動遺忘（生命週期管理）
- 即時檢視器（port 3113）
- 原生支援 Claude Code（外掛 + hooks + MCP）、Codex CLI、Gemini CLI、Cursor、Windsurf 等
- **所有 agent 共享同一個記憶伺服器**

#### 對比表

| 維度 | agentmemory | Mem0 | Claude Code 內建 |
|------|-------------|------|-----------------|
| 檢索 R@5 | **95.2%** | 68.5%（LoCoMo） | N/A（grep） |
| 自動捕捉 | 12 hooks（零人工） | 手動 `add()` | 手動觸發 |
| 搜尋方式 | BM25 + 向量 + 圖（RRF 融合） | 向量 + 圖 | 全文載入 context |
| 多 agent 共享 | MCP + REST + 租約 + 訊號 | API（無協調） | 每個 agent 獨立 |
| 外部依賴 | 無（SQLite） | Qdrant / pgvector | 無 |
| Token 效率 | ~1,900 tokens/session（$10/年） | 依整合方式 | 240 條 ≈ 22K+ tokens |
| 記憶生命週期 | 4 層整合 + 衰減 + 自動遺忘 | 被動擷取 | 手動清理 |
| 即時檢視器 | 是（port 3113） | 雲端儀表板 | 無 |
| 自架 | 預設（本地優先） | 選用 | 是 |
| 安裝 | `npm i -g @agentmemory/agentmemory` | `pip install mem0ai` | 內建 |

#### 評估結論

- **agentmemory 明顯優於 Mem0**：本地優先、零外部依賴、自動捕捉、檢索準確率高 39%
- ⚠️ 但仍需評估是否與現有系統重疊：Claude Code 已有 auto memory，Obsidian 知識庫已是持久記憶
- ✅ agentmemory 的獨特價值：**多 agent 共享記憶**——Claude 在 session 1 發現的架構決策，Codex 在 session 2 直接知道，無需 handoff 傳遞
- ✅ 額外價值：session 重播功能，可匯入 Claude Code JSONL 紀錄回顧過去對話
- **結論**：如果導入多 Agent 方案（CCB/dotbabel），agentmemory 是比 Mem0 更好的記憶層選擇。但若日常只用模式 1+2（純手寫 + 需求驅動），現有 auto memory + Obsidian 已夠用，可列為 Phase 3 待評估

### 4. Claude Squad — 多 Agent 平行管理器

**做什麼**：tmux-based TUI，同時管理多個 AI coding agent（Claude Code、Codex、Gemini、Aider），每個任務獨立 git worktree。

**核心功能**：
- 背景執行 + auto-accept（yolo mode）
- Review diff 後再 apply 變更
- Session 暫停/恢復
- 多 profile 支援不同 AI 助手
- 直接從介面 commit & push

**安裝**：Homebrew 或 curl，指令為 `cs`

**評估**：
- ✅ 適合「同時跑多個獨立任務」
- ⚠️ Claude Pro $20 額度有限，多實例會加速消耗
- ⚠️ 功能與 CCB 部分重疊
- **結論**：搭配 Codex（免費）平行跑時有價值；純 Claude 多實例不建議

### 5. claude_codex_bridge (CCB) — 混合 Provider 工作空間

**做什麼**：在一個 tmux workspace 裡同時開多個 AI provider CLI，每個 agent 是真實的 CLI session，不是模擬。

**核心功能**：
- **Real CLI panes**：每個 agent 是實際的 provider CLI
- **可見 sidebar**：agent 狀態、視窗、通訊一目了然
- **Multi-window topology**：main/work/review/research 邏輯分組
- **獨立 worktree**：實作 agent 在獨立 git branch 工作
- **混合 model/API**：每個 agent 可用不同 provider
- **明確協作**：`/ask` 路由、callback、silence mode
- **專案設定**：`.ccb/ccb.config` 定義團隊配置

**安裝**：Python 3.10+、tmux、至少一個 agent CLI

**對現有流程的價值**：
- 最契合「agent 間 handoff」需求
- 一個 workspace 裡 Claude（判斷）+ Codex（實作）+ Antigravity（搜尋）
- 明確的角色分工讓額度分散有結構性保證

### 6. planning-with-files — 檔案式執行追蹤

**來源**：https://github.com/OthmanAdi/planning-with-files（MIT，v2.43.0）

**做什麼**：Manus 風格的「磁碟工作記憶」，用三個 markdown 檔案追蹤開發執行過程，並透過 hooks 自動注入 context。

**核心機制**：

| 檔案 | 用途 | 更新時機 |
|------|------|---------|
| `task_plan.md` | 分階段計畫 + 決策紀錄 + 錯誤追蹤 | 每個 phase 完成後 |
| `findings.md` | 研究發現 + 外部資料 | 任何發現時立即寫入 |
| `progress.md` | session 日誌 + 測試結果 | 持續更新 |

**關鍵特性**：
- **5 個 hooks 自動運作**：UserPromptSubmit（注入計畫）、PreToolUse（每次工具呼叫前重讀）、PostToolUse（提醒更新進度）、Stop（檢查完成度）、PreCompact（壓縮前保存狀態）
- **Context 壓縮存活**：長 session 壓縮後，hooks 自動重新注入 task_plan.md
- **3-Strike Error Protocol**：失敗 3 次自動升級策略，不重複同樣的錯誤
- **SHA-256 plan attestation**：防止 plan 被竄改（prompt injection 防護）
- **跨 agent 支援**：同時支援 Claude Code、Codex CLI、Gemini CLI、Cursor 等
- **session 恢復**：`session-catchup.py` 在新 session 自動比對上次進度
- **`/plan-goal` + `/plan-loop`**：搭配 Claude Code 原生 `/goal` 和 `/loop`，實現「自動跑到完成」

**安裝**：`npx skills add OthmanAdi/planning-with-files`

**對現有流程的價值**：
- 補上 `/requirements`（WHAT）到實際開發之間的「執行追蹤」缺口
- 跨 agent handoff 時，所有 agent 讀同一份 planning files，不需要另外傳遞 context
- 長 session 不再因 context 壓縮而遺失進度狀態

## 推薦搭配方案

### 導入優先順序

| 順序  | 工具                  | 層級       | 理由                                              |
| --- | ------------------- | -------- | ----------------------------------------------- |
| 0   | ZeroSpec            | Layer 0  | 一次設定永久受益，AGENTS.md 整理好所有後續工具都更準確，零額度消耗          |
| 1   | Matt Pocock Skills  | 工作流      | 零設定成本，`/caveman` 立即省 token，`/handoff` 強化交接      |
| 1+  | planning-with-files | Layer 2  | 補上執行追蹤層，所有模式通用，跨 agent 共享進度                     |
| 2   | dotbabel            | Layer 0+ | 把 ZeroSpec 整理好的 AGENTS.md 同步到所有 CLI，跨 CLI 規則一致性 |
| 3   | CCB                 | 執行層      | 需要平行開發時再引入，定義 Claude + Codex + Antigravity 團隊   |
| 3+  | agentmemory         | 記憶層      | 導入多 Agent 方案後，作為跨 agent 共享記憶層（取代 Mem0）          |
| 暫緩  | Claude Squad        | 執行層      | 功能與 CCB 重疊，額度風險較高                               |
| 不採用 | Mem0                | 記憶層      | agentmemory 在檢索準確率、自動捕捉、零外部依賴全面勝出               |

### 額度分散策略

```
Claude Pro ($20)  → 架構決策、final review、複雜推理
Copilot Pro ($10) → IDE 內即時補全、小範圍修改
Codex (Free)      → 主要實作、測試撰寫、文件生成
Antigravity (Free)→ 搜尋、大檔案閱讀、交叉驗證
```

## 現有流程 vs 優化後流程

### 現行流程（五情境）

> ⚠️ **權威來源為 [[decision-framework]]「優先順序判斷 → 開發流程（五情境）」**。本節與其保持同步，2026-07-29 校準。

| 情境 | 條件 | 流程 | Claude 額度消耗 |
|---|---|---|---|
| 1 | 簡單改動 | 直接手動修改 | 無 |
| 2 | 需求明確（**無 UI**）| 〔讀〕**現況層** → `/requirements` → agent 開發 → `/stack-review` → `/doc-update` | 中（審查為主）|
| 3 | 需求明確（**有 UI**）| `figma-to-requirements` → 〔讀〕**現況層** → `/requirements` → agent 開發 → `/stack-review` → `/doc-update` | 中 |
| 4 | 複雜／高風險（**無 UI**）| 〔讀〕**現況層** → `/requirements` → `/ai-pair dev`（多模型協作審查）→ `/doc-update` | 高（多模型審查）|
| 5 | 複雜／高風險（**有 UI**）| `figma-to-requirements` → 〔讀〕**現況層** → `/requirements` → `/ai-pair dev` → `/doc-update` | 高 |

> **〔讀〕現況層**（2026-07-30 加入五情境）= 讀取 `project-documentation` 產出的架構／API／變更歷程文件
> （含**上游 repo**）。**是讀取而非執行步驟**，不產生交付物。詳見文末 2026-07-30 章節。

**與舊「三模式」的差異**（本文早期版本）：
- 「Claude 開發」已改為「**agent 開發**」——實作主力移出 Claude（詳見下方「核心策略轉變」）
- UI 維度從「附加前置」正式**拆成獨立情境**（情境 3 / 5）
- 舊版模式編號沿用至今，因為五情境本質是**3 個複雜度層級 × UI 維度**，而 `figma-to-requirements` 是前置步驟而非另一條管線

### 本文「模式 N」與五情境對照

> 本文後續章節（工具選用建議、導入順序等）沿用**複雜度層級**編號，對應關係如下：

| 本文模式 | 對應情境 | 說明 |
|---|---|---|
| **模式 1** 純手寫 | 情境 1 | 完全相同 |
| **模式 2** 需求驅動 | 情境 2 + 3 | 有 UI 時前置 `figma-to-requirements` |
| **模式 3** 複雜開發 | 情境 4 + 5 | 有 UI 時前置 `figma-to-requirements` |
| **模式 4** TDD | ❌ **未納入** decision-framework | 本文提案，尚未採用（見該節說明）|

### 優化後各模式

#### 模式 1：純手寫（不變）

直接手動修改，搭配 Copilot Pro inline 補全。不吃 Claude 額度。

#### 模式 2：需求驅動開發（中等複雜）

```
Before:
/requirements → Claude 開發 → /stack-review → /doc-update

After（完整分層）:
〔UI 需求才有〕figma-to-requirements（全域 skill，讀專案 DESIGN.md
       → token 對應+狀態矩陣 → UI 需求草稿）
ZeroSpec AGENTS.md ＋ DESIGN.md（Layer 0，專案上下文 + UI 設計上下文常駐）
→〔讀〕現況層（Layer 0′：本 repo 與上游 repo 的 ARCHITECTURE / API_DOCUMENTATION /
       UPDATE_RECORD；⚠️ 先判斷可信度——標未驗證／無 source-commit 者翻原始碼）
→ /requirements（Layer 1，需求規格；UI 需求接續 figma-to-requirements 草稿補功能面）
→ /plan（Layer 2，建立 task_plan.md）
→ /handoff 壓縮需求 → Codex 開發（免費）
  （Codex 讀 AGENTS.md 任務路由 + DESIGN.md token/元件對應 + hooks 自動讀取 task_plan.md，更新 progress.md）
→ Claude /stack-review → /doc-update
```

| 改動                             | 效果                                               |
| ------------------------------ | ------------------------------------------------ |
| 加 figma-to-requirements（UI 需求才有）| 截圖讀 DESIGN.md → token 對應+狀態矩陣，先產精確 UI 草稿再進 /requirements |
| DESIGN.md 併入 Layer 0           | UI 設計上下文常駐，agent 對著 token/元件對應實作，不 hardcode |
| **加〔讀〕現況層（Layer 0′）** | `/requirements` 不必每次從零翻原始碼；**但必須先判斷可信度**——標未驗證／無 `source-commit` 者仍翻原始碼。省 token 的前提是文件可信 |
| 加 `/plan`（planning-with-files） | 建立 task_plan.md + findings.md + progress.md 追蹤執行 |
| 加 `/handoff`（Matt Pocock）      | 把 requirements 壓縮成 Codex 能直接消化的格式                |
| 實作交給 Codex                     | Claude 從「開發者+審查者」退到「純審查者」                        |
| 日常加 `/caveman`                 | Claude 回覆壓縮 75%，延長額度壽命                           |
| hooks 自動追蹤                     | PostToolUse 提醒更新 progress.md，context 壓縮後自動恢復計畫   |

> **三層協作**：ZeroSpec 的 AGENTS.md 讓 Codex 一進 repo 就知道架構邊界和任務路由（Layer 0）；`/plan` 建立結構化計畫（Layer 2）；hooks 自動注入 task_plan.md 到每個 session。三層加起來，handoff 幾乎不需要額外的人工 context 傳遞。

#### 模式 3：複雜開發（CCB 多 Agent 協作）

```
Before:
/requirements → /ai-pair dev（含 review）→ /doc-update

After（完整分層）:
〔UI 需求才有〕figma-to-requirements（讀專案 DESIGN.md → token 對應+狀態矩陣 → UI 需求草稿）
ZeroSpec AGENTS.md ＋ DESIGN.md（Layer 0，所有 agent 共讀同一份專案上下文 + UI 設計上下文）
→〔讀〕現況層（Layer 0′，含上游 repo；⚠️ 高風險改動最禁不起契約猜錯，此層不可略過）
→ /grill-with-docs → /requirements（Layer 1，UI 需求接續草稿補功能面）
→ /plan（Layer 2，建立 task_plan.md）→ CCB workspace：
  Codex 實作（讀 AGENTS.md 任務路由 + DESIGN.md token/元件 + task_plan.md，寫 progress.md）
  + Antigravity 搜尋（讀 AGENTS.md 架構邊界，結果寫入 findings.md）
  + Claude 架構判斷（讀 findings.md 做決策）
→ Claude /stack-review 或 /ai-pair review-only
→ /doc-update
```

| 改動 | 效果 |
|------|------|
| 加 figma-to-requirements（UI 需求才有）| 截圖讀 DESIGN.md 產 UI 草稿，CCB 各 agent 對著同一份 DESIGN.md token 實作 UI |
| **加〔讀〕現況層（Layer 0′）** | 多 agent 共讀**同一份已驗證的契約**，避免各自推導出不同結論；高風險改動不可略過此層 |
| 加 `/grill-with-docs`（Matt Pocock） | requirements 前先釐清需求，減少後面來回修改 |
| 加 `/plan`（planning-with-files） | 三個 agent 共享同一份 planning files 作為協作介面 |
| CCB 取代 ai-pair 開發階段 | 多 agent 各自 worktree 平行工作，免費 agent 做重活 |
| ai-pair 改為 review-only | Claude + GPT(copilot) + Gemini(agy) 三方交叉審查，Claude 不再承擔開發；三方共讀單一 REVIEW_FILE（不各自讀 source）省 token |

> **ZeroSpec + planning-with-files 在 CCB 中的互補**：ZeroSpec 的 AGENTS.md 提供「靜態上下文」（架構邊界、任務路由、版本真相來源）——每個 agent 進入 repo 時自動讀取，不需要人工說明專案結構。planning-with-files 提供「動態上下文」（task_plan.md、progress.md、findings.md）——隨開發過程持續更新。靜態 + 動態兩層讓三個 agent 的協作既有共同基礎知識，又有即時進度同步。

#### 模式 4：TDD 驅動開發（⚠️ 本文提案，未採用）

> **狀態（2026-07-29 確認）**：**暫不納入** [[decision-framework]] 的五情境，維持提案狀態。
> 實際開發流程仍以五情境為準；`/tdd` 需要時可在情境 2/4 的 agent 開發階段直接套用，不必獨立成一個模式。
> 若日後決定正式採用，**須先更新 decision-framework**（權威來源），再回頭同步本文。

```
/grill-with-docs → /tdd（紅綠重構循環）
→ /diagnose（遇 bug 時）→ /stack-review → /doc-update
```

適合核心模組、需要高品質保證的開發。Matt Pocock `/tdd` 確保測試覆蓋率。

**暫不納入的理由**：TDD 屬於情境 2/4 內部的**開發方法選擇**，不是另一個並列情境——把它拉成第四個模式會讓「情境」這個維度混入兩種不同的分類基準（複雜度 vs 開發方法）。

### 核心策略轉變

**Claude 角色重新定位**：從「開發者 + 審查者」退到「審查者 + 決策者」

| 角色 | Agent | 成本 |
|------|-------|------|
| 需求釐清 + 架構決策 | Claude | 付費（但用量少） |
| 主要實作 + 測試撰寫 | Codex | 免費 |
| 搜尋 + 大檔案閱讀 | Antigravity | 免費 |
| IDE 即時補全 | Copilot Pro | $10/月（固定） |
| Final review + 交叉驗證 | Claude + ai-pair | 付費（審查比開發省） |

### 導入順序與時程建議

| 階段 | 動作 | 預估時間 |
|------|------|---------|
| Phase 1 | 安裝 Matt Pocock Skills + planning-with-files，模式 2 開始用 `/caveman` + `/plan` | 30 分鐘 |
| Phase 2 | 安裝 dotbabel，建立 AGENTS.md 跨 CLI 規則同步 | 1 小時 |
| Phase 3 | 安裝 CCB，設定 `.ccb/ccb.config` 定義三 agent 團隊 | 2 小時 |
| 觀察期 | 跑兩週，觀察 Claude 額度消耗是否降低 60%+ | 2 週 |
| 視需要 | agentmemory（跨 agent 記憶）、Claude Squad（平行獨立任務） | 按需 |

## 完整分層架構

```
Layer 0  規範層（常駐・人工維護・不會失真）
         ZeroSpec AGENTS.md — 任務路由、架構邊界、版本真相
       ＋ DESIGN.md — 色彩/文字/間距 token、Figma→Flutter 元件對應
Layer 0′ 現況層（on-demand 讀取・程式碼衍生・⚠️ 會漂移）        ← 2026-07-30 加入
         project-documentation 產出：ARCHITECTURE / API_DOCUMENTATION / UPDATE_RECORD
         含**上游 repo** 的產出（b2b-manager 的上游是 b2b-backend）
Layer 0.5（UI 需求才有）：figma-to-requirements — 截圖讀 DESIGN.md → token 對應+狀態矩陣 → 餵 Layer 1
Layer 1：/requirements 或 spec-kit（需求規格）— 功能範圍、驗證規則、API 設計
Layer 2：/plan + planning-with-files（執行追蹤）— task_plan.md、progress.md、findings.md
Layer 3：執行（多 Agent: CCB + dotbabel 或 單 Agent: pilot-suite）
Layer 4：/stack-review（品質保證）
Layer 5：/doc-update（文件同步）
```

> **Layer 0 與 0′ 是兩種性質，不是兩個階段**：0 是**規範性**（「該怎麼做」，定義規則所以不會錯）、
> 0′ 是**描述性**（「現況是什麼」，會與程式碼脫節）。0′ 因此需要 `source-commit` 與
> 可信度標記——0 完全不需要。

每一層解決不同的問題，缺一層就會在下游產生連鎖效應：
- 缺 Layer 0 → AI 不懂專案結構，`/requirements` 產出的需求可能偏離
- **缺 Layer 0′ → `/requirements` 每次從零翻原始碼（貴）**
- 🔴 **Layer 0′ 存在但失真 → 比缺席更糟**：下游照抄錯誤契約，**runtime 才炸**
  （2026-07-30 實測某 API 文件通用章節 13 處錯誤，前端照抄後整頁解析失敗）
- 缺 Layer 1 → AI 不知道要做什麼，直接衝去寫 code
- 缺 Layer 2 → 長 session 或 handoff 時遺失進度
- Layer 3-5 → 現有流程已覆蓋

## 2026-06-23 更新：UI 開發前置（figma-to-requirements + DESIGN.md）

UI 類需求（來自 Figma 截圖）新增一個前置階段，插在**所有模式的 `/requirements` 之前**：

```
docs/assets/<功能>/*.png（一組功能流程截圖）
  → figma-to-requirements（全域 skill，讀「截圖所屬 repo 的 DESIGN.md」
       → 產 token 對應表 + 狀態矩陣 + 待新增 token 清單）
  →〔讀〕現況層（Layer 0′，2026-07-30 加入；UI 需求若涉及 API 串接才需要）
  → /requirements（補功能邏輯 / 影響範圍 / 風險，原流程）
  →（以下接各模式原流程不變：Codex/CCB 實作 → stack-review / ai-pair → doc-update）
```

| 變更 | 說明 |
|------|------|
| **DESIGN.md = Layer 0 設計上下文** | 與 AGENTS.md 並列：AGENTS.md 講「怎麼開發」，DESIGN.md 講「UI 用哪些 token/元件」。靜態環境檔，一次設定持續受益（同 ZeroSpec Layer 0 精神，零額度消耗） |
| **figma-to-requirements**（全域 skill） | 讀截圖所屬 repo 的 DESIGN.md，跨專案通用（b2b-manager / app01-double 各用各的 token） |
| **取代 figma-to-flutter** | 「截圖→code」一步到位 → 改「截圖→需求→實作」三段；精確度來自 DESIGN.md token 詞彙而非工具。figma-to-flutter 已退役歸檔 |
| **適用範圍** | 主要模式 2/3（需求驅動 / 複雜開發）；不影響下游 stack-review / ai-pair / doc-update |

> 與本文工具的關係：DESIGN.md 補上 ZeroSpec（Layer 0）缺的「UI 設計上下文」這一塊；figma-to-requirements 則是 Layer 1（/requirements）的視覺前處理器。多 Agent handoff 時，DESIGN.md 與 AGENTS.md 一樣是所有 agent 共讀的靜態基礎。

## 2026-07-13 更新：Token 成本工具四件套與選用情境

來源：raw/〈Fable 5 延長到 7/12，這些是我省 Token 的方法〉社群分享 + 各工具官方 README（已整理進 wiki，詳見文末連結）。此節把四個省 token 工具對應到現行開發流程（[[decision-framework]] 五情境，本文以「模式 N」對照，見〈本文「模式 N」與五情境對照〉），並釐清選用邏輯。

### 四工具作用軸線對照（互不重疊，可疊加）

| 工具 | 省的是什麼 | 語言相依性 | 導入成本 | 對應 wiki |
|------|-----------|-----------|---------|----------|
| **Ponytail** | AI **生成程式碼的量**（六梯級決策樹，少寫不必要的 code） | **低**——省的是 code，中文對話照樣有效 | 低（plugin 一鍵裝，建議改寫規則） | [[ponytail-lazy-senior-dev-skill]] |
| **codebase-memory-mcp** | **探索/理解程式碼的 input token**（知識圖譜結構查詢取代 grep+整檔讀取） | 無 | 低（單一 binary，一次 install 配置 11 種 agent） | [[codebase-memory-mcp]] |
| **Caveman** | **對話輸出文字的 token**（電報式壓縮回覆） | **高**——壓縮句法建立在英文 tokenizer 特性上 | 低 | [[github-copilot-token-reduction]] / research/caveman-prompt-compression |
| **pxpipe** | **大塊 context 的 input token**（文字渲染成圖片走視覺通道計費） | **高**——官方自承 CJK 保守支援 | 中（proxy 架設） | [[pxpipe-image-token-compression]] |

### 對應現有開發模式的選用建議

**模式 1（純手寫）**：全部不需要，Copilot inline 補全已足夠。

**模式 2（需求驅動：requirements → Codex 開發 → Claude review）**：
- ✅ **Ponytail（改寫版）優先導入**——作用在 Codex/Claude 實作階段，少寫的 code 同時減輕 `/stack-review` 的審查負擔，是「一份投資兩處受益」。做法依原分享作者的經驗：**不直接照裝，讓 AI 把六梯級規則改寫進自己的 CLAUDE.md**（避免「AI 變笨/失去創意」副作用），可只取前四梯級（需要存在嗎→stdlib→原生能力→已裝依賴）
- ✅ **codebase-memory-mcp 掛在 b2b-manager 等中大型專案**——在 AGENTS.md 加一句「查看專案前先用該 MCP 了解結構關聯」才會被主動使用；小型個人專案（Python 工具類）不需要，grep 已夠
- ⚠️ Caveman 對中文工作流效益打折（見下方軸線釐清），可跳過

**模式 3（複雜開發：CCB 多 Agent）**：
- ✅ codebase-memory-mcp 效益最大——多個 agent 共用同一份知識圖譜索引，每個 agent 都省下重複探索的 input token
- ✅ Ponytail 規則放進共讀的 AGENTS.md，所有實作 agent 一致遵循
- 補充：[[fable5-subagent-model-routing-design]] 的 judge/builder/worker 模型分級與 ai-pair 架構同源，「審查用高階模型、實作/調查用次階模型」可直接沿用

**模式 4（TDD）**：Ponytail 與 TDD 相容良好（測試本身是安全邊界，Ponytail 明文不刪測試覆蓋），無衝突。

### pxpipe 暫不採用的理由

1. **Fable 5 特化 + 免費期已結束（7/12）**：成本效益需重算，且允許清單外模型（Opus 4.7/4.8、GPT-5.5）讀圖錯誤率明顯較高
2. **CJK 保守支援**：官方 README 自承「ASCII/Latin-1 well tested; CJK works but conservatively」，本工作流大量使用繁體中文，逐字精確回憶（識別碼/檔名/中文人名）的 confabulation 風險更高
3. 若未來要重新評估：先在自己的 workload 跑 `~/.pxpipe/events.jsonl` 實測，勿直接採用官方 59-70% 數字

### Caveman vs Ponytail 軸線釐清（常見混淆）

兩者**不是同一類工具、不是二選一**：Caveman 壓縮「對話文字」（語言相依性高），Ponytail 減少「生成程式碼」（語言中立）。網路上「Caveman 對中文支援不理想」的說法方向正確——電報式壓縮句法建立在英文 tokenizer 特性上（英文單字≈1 token，刪冠詞/修飾語收益直接），中文 1 字約 1-2 token、電報式中文省得有限，且中文縮寫易產生語意歧義造成返工。Caveman 雖有 `wenyan` 文言文模式試圖利用古典漢語濃縮特性，但理解風險與歧義問題仍在，實測數據（65% 平均節省）全部來自英文任務。

**結論：中文工作環境下，程式碼為主要輸出時 Ponytail 是更可靠的選擇**——其節省機制（少寫 code）與對話語言無關。完整比較見 [[github-copilot-token-reduction]] 的補充章節。

### 更新後的省 token 優先順序（中文工作環境）

```
1. Ponytail（改寫版規則進 CLAUDE.md）  ← 語言中立，作用在輸出 code
2. Prompt Caching（固定指示放頂部）     ← 語言中立，長 session 效益最高
3. codebase-memory-mcp（中大型專案）    ← 語言中立，作用在輸入探索
4. Sub-agent Isolation（探索委派）      ← 語言中立
5. Caveman                             ← 中文環境效益打折，可跳過
6. pxpipe                              ← CJK 風險 + Fable 免費期結束，暫不採用
```

## 相關連結

- [[decision-framework]] — **開發流程五情境的權威來源**（本文的模式定義以其為準）
- [[zerospec]] — ZeroSpec Layer 0 上下文整備框架
- [[spec-driven-ai-workflow]] — 規格驅動 AI 開發工作流（spec-kit / OpenSpec）
- [[dual-tool-strategy]] — Claude + Antigravity 雙工具分工
- [[gemini-cli-subagent]] — Antigravity CLI 接入 Claude Code
- [[ai-pair-skill]] — 多模型協作（ai-pair v4.3：gemini-reviewer 底層改用 Antigravity CLI `agy`，三方共讀單一 REVIEW_FILE 省 token）
- [[concepts/agent-handoff-memory-patterns]] — HANDOFF/MEMORY 五模式 + 跨工具記憶能力對照
- [[archive/figma-to-requirements-ui-dev-workflow]] — UI 截圖→需求→實作工作流完整設計（已實作歸檔）
- [[ponytail-lazy-senior-dev-skill]] — Ponytail 六梯級決策樹完整說明（含 54% 修正數據與社群批評）
- [[codebase-memory-mcp]] — 知識圖譜 MCP 完整技術細節（arXiv 實測 10 倍省 token）
- [[pxpipe-image-token-compression]] — pxpipe 完整失效模式分析（CJK 保守支援出處）
- [[github-copilot-token-reduction]] — 四種 Token 削減法 + Caveman vs Ponytail 軸線比較
- [[fable5-subagent-model-routing-design]] — judge/builder/worker 模型分級路由

---

## 2026-07-29 更新：project-documentation 定位為「Layer 0′ 現況層」

來源：b2b-manager 報表寄送設定功能（SSGS-12853）的實際故障。此節釐清 `project-documentation` 在分層中的位置，以及它與 Layer 0 既有內容的**本質差異**。

### 對五情境的定位：不是新情境，是情境 2-5 的**共同前置**

先釐清它在權威來源（[[decision-framework]]「開發流程（五情境）」）中的位置：

| 情境 | 是否需要現況層 |
|---|---|
| 1 簡單改動（直接手動）| ❌ 不需要——沒有 `/requirements` |
| 2 / 3 需求明確（無 UI / 有 UI）| ✅ 需要 |
| 4 / 5 複雜高風險（無 UI / 有 UI）| ✅ **最需要**——高風險改動最禁不起契約猜錯 |

→ **不新增第六情境**。它不是管線上的一步，是情境 2-5 的 `/requirements` 讀取的**上下文**，與 `figma-to-requirements`（前置步驟、會產出交付物）性質不同。

### ⚠️ 依框架的「功能/工具是否採用」檢核

| 條件 | 判定 | 說明 |
|---|---|---|
| 解決目前有感的痛點 | ✅ | 已實際造成 P0，非「有趣」 |
| 單人使用可以跑通 | ✅ | 單人單機生成，不需團隊配合 |
| **成本可接受** | ⚠️ **本項是唯一疑慮** | 見下 |
| 可以漸進採用 | ✅ | 可只先做 b2b-backend 的 API 契約一份 |
| 實驗後再導入全域 | ⚠️ **尚未實驗** | 見下 |

🔴 **成本結構與框架內其他工具不同**：Ponytail、codebase-memory-mcp 等都是**一次性設定**，之後零維護。現況層是**描述性**的，必須**定期重生成**才保有價值——這是**持續成本**。若不重生成，它會退化成「看起來權威但已過期」的文件，比沒有更危險（正是這次的故障模式）。

→ 導入時**必須同時定義重生成時機**，否則不該導入。建議綁在後端 `UPDATE_RECORD.md` 新增章節時。

### 📌 依「實驗後再導入全域」：當時先不寫入權威來源

> ✅ **已於 2026-07-30 完成導入**，本小節保留為決策過程記錄。
> 現況層已寫入 [[decision-framework]] 的五情境（情境 2-5 的 `/requirements` 前加〔讀〕現況層節點）。

當時的判斷：框架明訂「先在專案或功能層級實驗，確認有效再加進全域環境」。而 b2b-backend 那三份文件是**早期 ad-hoc AI 生成、未經 `project-documentation`**，等於**這個流程還沒真正跑過一次**，所以先維持提案狀態（同 模式 4 TDD 的處理方式）。

→ 2026-07-30 實跑後確認有效（抓到 13 處錯誤），依框架規則解除提案狀態、寫入權威來源。

### 💡 框架原則的直接對應

「**遇到不確定時，讀比寫便宜**」原本是知識庫的整理原則，但它正是待處理第 1 項的依據：`/requirements` Step 2 應**先讀現況層產出，不夠再翻程式碼**——翻程式碼是「寫」（重新推導一次），讀現況層是「讀」。這次故障的成本結構完全吻合：因為現況層不可信，每次都被迫從零推導。

⚠️ 但前提是**現況層本身可信**。所以「已驗證／推導未驗證」標記（見下）不是加分項，是這條原則能成立的必要條件。

### 為什麼它不屬於現有任何一層

`project-documentation` 產出的是**給 `/requirements` 參考的依據**（不夠再翻程式碼），位置在 Layer 1 上游。但它與 Layer 0 既有內容性質不同：

| | **規範層**（既有 Layer 0）| **現況層**（project-documentation 產出）|
|---|---|---|
| 內容 | AGENTS.md「怎麼開發」、DESIGN.md「用哪些 token」 | ARCHITECTURE.md「架構長怎樣」、API_DOCUMENTATION.md「有哪些端點」 |
| 性質 | **規範性**（prescriptive）| **描述性**（descriptive）|
| 來源 | 人工維護 | **從程式碼衍生** |
| 會不會失真 | ❌ 不會——它定義規則，規則即真相 | ⚠️ **會漂移**——程式碼變了文件沒跟上，或生成時就猜錯 |
| 需要什麼特殊處理 | 無 | **標記驗證狀態 + 定期重生成** |

→ 記為 **Layer 0′（現況層）**，與 **Layer 0（規範層）** 並列而非取代（命名已於 2026-07-30 統一為 `0′`，見〈完整分層架構〉）。

### 🔴 實際故障：描述層失真會沿整條鏈往下污染

b2b-backend 的三份文件是**早期直接用 AI 生成、未經原始碼驗證**（非經 `project-documentation`）。結果：

```
AI 生成的 API_DOCUMENTATION.md（JSON 範例憑慣例撰寫）
  ↓ /requirements 與實作照它寫前端 DTO
runtime 解析失敗（新增看似成功但 Drawer 不關、列表不更新）
  ↓ 被迫回頭讀原始碼
發現 §3.1/§5.1 的 `SearchResult<T>` 五個欄名全錯
```

實測該文件的偏差程度：

| 項目 | 文件寫的 | 實際原始碼 | 出現次數 |
|---|---|---|---|
| JSON 鍵命名 | camelCase | **snake_case**（`DaoConfig` 的 `@Primary` ObjectMapper）| camelCase **396** : snake_case **1** |
| `SearchResult<T>` | `{results, totalElements, totalPages, currentPage, pageSize}` | **`{data, page, pages, total}`** | 5 個欄名全錯 |
| `Response<T>` | `{data, message}` | **`{data}`**（無 `message`）| — |
| 400 錯誤 | `{"error":{"message"}}` | **`{"message"}`**（單層）| — |

⚠️ **錯誤集中在「通用規範」章節（§3.1/§5.1），再被複製到每個分頁端點**——不是散落的 396 個獨立錯誤，是一個錯誤模板擴散全文。

### 💡 關鍵設計：強制標記「已驗證 / 推導未驗證」

這次真正的傷害不是「文件有錯」，而是**錯得看不出來**——文件語氣肯定，下游沒有理由懷疑。

> 若範例旁有一行「⚠️ 本範例為依慣例推導，未從原始碼驗證」，就會立刻去讀 `SearchResult.java`，整條故障鏈不會發生。

| 標記 | 適用內容 | 下游（`/requirements`）行為 |
|---|---|---|
| ✅ **已驗證** | 從原始碼確定性抽取：端點、`@PreAuthorize` 權限、DTO 欄位、enum 值、包裝類結構、ObjectMapper 命名策略 | 可直接採用 |
| ⚠️ **推導未驗證** | 憑慣例撰寫的 JSON 範例、說明性文字 | **必須翻程式碼確認** |

**要求 AI 標記不確定性，比要求 AI 不要猜更可行**——它判斷不出自己哪裡在猜，但可以被要求區分「我抽取的」與「我推導的」。

### 新增維度：跨 repo 消費

現有分層都假設**單一 repo**。但實際上 b2b-manager 的 `/requirements` 消費的是**另一個 repo（b2b-backend）的現況層產出**：

```
b2b-backend：project-documentation → ARCHITECTURE / API_DOCUMENTATION / UPDATE_RECORD
                                          ↓ 跨 repo 被消費
b2b-manager：/requirements → docs/shared/ 任務文件 →（不夠時）翻後端原始碼
```

由此衍生兩個需求：

1. **`UPDATE_RECORD.md` 應正式納入 `project-documentation` 的文件類型**（目前五種類型沒有它）。實測它是**跨 repo 協作效用最高**的一份——有 commit hash、⚠️ 行為變更標記、章節編號，是下游判斷「後端改了什麼」的唯一可靠來源
2. **任務文件需宣告「後端依賴」**（如 `API §4.18`、`Role.java`、`EntityType`），否則「後端更新 → 哪些任務文件要跟著改」只能靠人記得，無法自動化

### ✅ 已修復：兩個 skill 之間的交接斷點（2026-07-30）

**斷點本體**：`/requirements` 的 Step 2「探索相關程式碼」**直接 `ls` / `grep` 原始碼，完全沒有讀現況層產出這一步**。
所以「產出當依據、不夠再翻程式碼」這個設想在 skill 層**從未存在**——實際每次都從零翻程式碼。

兩端都已改完並實測：

| skill | 版本 | 改了什麼 |
|---|---|---|
| `project-documentation` | 2.0.0 → **2.1.0** | 核心原則 3 條、wire 契約推導階段 B、Step 0 更新（不覆蓋）、Step 5 自檢、`change-log` 第六型、命名配合實際 |
| `requirements` | 1.2.0 → **1.3.0** | **Step 2 拆為 2a（先讀參考文件＋可信度判斷）／2b（程式碼探索）**、風險清單加跨服務契約、metadata 加 `後端依賴`／`參考文件基準`、範本加「契約依據」章節 |

> ❌ **不加**「任務需求文件」類型——`docs/shared/` 屬 `/requirements` 的產出，兩個 skill 是獨立流程。

---

## 2026-07-30 實測：在 b2b-backend 跑一次，抓到 13 處錯誤

依框架的「實驗後再導入全域」，在 b2b-backend（Spring Boot）實跑新版流程，驗證是否真能抓到既有文件的錯誤。

### 結果：既有 AI 生成文件的通用章節，錯誤率接近 100%

| 項目 | 文件寫的 | 原始碼實際 |
|---|---|---|
| 鍵命名 | （未記載）| **3 個 ObjectMapper**：`@Primary`＝`SNAKE_CASE`，eip／uec＝`LOWER_CAMEL_CASE` |
| `Response<T>` | `{data, message}` | **`{data}`**（`message` 是幽靈欄位）|
| 分頁包裝 | `{results, totalElements, totalPages, currentPage, pageSize}` | **`{data, page, pages, total}`**（5 個欄名全錯）|
| 包裝層數 | §3.1 套在 `data` 內／§5.1 頂層（**文件自我矛盾**）| 搜尋**直接回傳無外層**；create／update 才多一層 |
| 錯誤格式 | `{error:{code,message,details}, timestamp, path}` | **`{message}`** 單層 |
| 日期時間 | `YYYY-MM-DDTHH:mm:ssZ (ISO 8601)` | **`yyyy-MM-dd HH:mm:ss`**（空格、無 `T`／`Z`）|
| `TransactionType` | 4 個值 | **17 個**，且其中 3 個文件值**根本不存在** |
| `DriverStatus` | 3 個值含幽靈 `PENDING` | 5 個值，缺 `INVITED`／`REJECTED`／`DELETED` |
| `TicketStatus` | `OPEN`／`IN_PROGRESS`／`CLOSED` | **`IN_PROGRESS`／`CLOSED` 都不存在** |
| `AccountType` | `MAIN`／`SUB` | **整個 enum 不存在** |
| §6 錯誤碼表 | 9 個錯誤碼 | **後端沒有 error code 概念**，整張表虛構 |

### 🔴 最重要的診斷：錯的不是推導能力，是「產出了流程沒要求的東西」

那份 JSON 範例**根本不在原流程的要求範圍內**——範本只要求寫回應的**型別名稱**（`Response<XxxDTO>`），
AI 為求文件完整**自行補上了整份 JSON 範例**。

→ **所以「補齊推導步驟」不足以解決問題**。只要「自行發揮」沒被禁止，AI 就會在其他未被規範處重演同一件事。
這是 v2.1.0 把「不得產出流程未要求的內容」列為**原則 1（最高優先）**的由來。

### 意外收穿：`source-commit` 規則在測試中自己證明了必要性

測試途中 b2b-backend **切換了分支**（`feat/SSGS-12855@db257366` → `main@52571b83`），enums 由 39 變 37。
重驗後核心 wire 契約檔案在兩個 commit 完全相同，結論不受影響——**但這暴露了真正的問題**：

`main` 的 `EntityType` 只有 10 個值（無 `REPORT_SCHEDULE`），`/report_schedules` 整組端點也不在 main 上。
**原文件沒有版本基準，根本無法判斷它描述的是哪個分支的現實。**

### 測試也暴露 skill 自身 5 個缺陷（已修）

1. `--include=*.java` **未加引號，zsh 直接報 `no matches found`**——第一次階段 B 全數失敗
2. 未排除 Maven `target/`
3. **`@Primary` 用 grep 抓不到**（獨立一行，與 `setPropertyNamingStrategy` 不同行）→ 必須 Read 檔案
4. `change-log.md` 範本的示例**含對該 repo 不成立的事實聲明**——我自己犯了原則 1 要防的錯
5. B4 缺 enum 範圍指引（實測 37～39 個）

> 💡 **範本裡的示例會被當成事實複製**。第 4 點是最反直覺的：連「防止虛構」的規範文件本身，
> 示例都必須寫成 `<placeholder>` 並標注虛構，否則它就是下一個錯誤來源。

### 延伸驗證：跨服務 enum 全量比對，查出一個線上缺口

順著同一方法比對 **39 個前端 enum × 36 個後端 enum**，查出前端 `AccountBalanceAdjustmentType`
缺 5 個後端已有的值，且該處用**非 nullable `$enumDecode`**
→ **帳戶調整記錄頁存在與上次 P0 完全同型的缺口，且正在線上**。

⚠️ 比對腳本踩了 3 個坑（Dart 最後一個常數被 `split(';')` 吃掉／`\w+` 匹配中文註解／同一行多常數），
**前兩個各產生過一輪全是誤報的結果**。→ **自動比對的結果必須人工抽驗再採信。**

### 導入狀態

| 機制 | 狀態 |
|---|---|
| 原則 1／2／3、`source-commit`、命名路徑、Step 0 路徑 4、Step 5 自檢 | ✅ 已實測生效 |
| Spring Boot 階段 B（wire 契約推導）| ✅ 已實測生效 |
| **Flutter 階段 B** | ⚠️ 未測 |
| **Step 0 路徑 3（更新流程）** | ⚠️ 未走到（這次命中路徑 4）|
| **`change-log` 產出** | ⚠️ 未測 |
| **重生成觸發機制** | ❌ **仍不存在**——skill 能判斷「該不該重生成」，但沒有東西會觸發它 |

→ **可以寫入權威來源（`decision-framework`）了**，但重生成觸發機制仍是開放項。

### 附帶發現：skill 規範與實際專案全面不符

| skill 規範 | b2b-manager | b2b-backend |
|---|---|---|
| `docs/project-architecture.md` | ✗（用 `docs/claude/claude-architecture.md`）| ✗（用 `ARCHITECTURE.md`）|
| `docs/api-reference.md` | ✗（用 `docs/api/`）| ✗（用 `API_DOCUMENTATION.md`）|
| `docs/page_architecture/[模組]-documentation.md`（kebab-case）| ⚠️ 目錄對，命名是 **snake_case** | — |
| `docs/bug_record/`（kebab-case）| ⚠️ 目錄對，命名是 **snake_case** | — |

前端 64 份文件中僅 **24 份**落在 skill 的五種類型內；檔案數最多的 `docs/shared/`（20 檔）屬 `/requirements`，`docs/claude/`（10 檔）、`docs/ai-collaboration/`（8 檔）則不屬任何 skill。

→ **skill 不被遵循不是紀律問題，是規範與實際脫節**。建議**改 skill 配合實際命名**（改 60+ 檔名的成本與風險都不划算）。


---

## 2026-08-05 評估：Superpowers 暫不採用（含風險登記）

起因：複查 [[superpowers-skill-bloat-risk-tiering]] 有效性時發現本文件從未評估過 Superpowers，而它是知識庫覆蓋度最高的框架之一（2 篇主條目 + 3 篇精粹提及）。**本次為決策紀錄，非新增工具建議。**

**現況核實（2026-08-05 上網查證）**：`obra/superpowers` v6.2.0、266.6k stars、`skills/` 14 個、當日仍有 push、支援 11 個平台。**本機未安裝**（`~/.claude/skills`、`~/.claude/plugins`、`notes/skills` 皆無，僅存在於官方市集可安裝清單）。

### ⚠️ 依框架的「功能/工具是否採用」檢核

| 條件 | 判定 | 說明 |
|---|---|---|
| 解決目前有感的痛點 | ❌ | 目前痛點是**額度分散與 agent 間 handoff**（本文件核心目標），不是缺工程紀律。Superpowers 解的是「AI 接到需求就衝去寫沒測試的 code」 |
| 單人使用可以跑通 | ✅ | 單人可跑，SessionStart hook 自動注入 |
| 成本可接受 | ❌ | 見下方風險 3、4 |
| **可以漸進採用** | ❌ **決定性否決項** | **Mandatory Workflow 本質不可漸進**——`using-superpowers` 明載「YOU DO NOT HAVE A CHOICE… This is not negotiable」；且 issue #2079 指出 brainstorming「**沒有輕量路徑，而且模型無法自己建構一條，因為該 skill 明確把『想要輕量路徑』列為 anti-pattern**」 |
| 實驗後再導入全域 | ❌ | SessionStart hook **天生全域**，無法只在單一專案實驗 |

→ **五條中三條不通過，其中「可以漸進採用」是硬性否決。**

### 🔴 與五情境的直接衝突（本次評估的決定性理由）

Superpowers 的骨幹是 **red/green TDD 強制流程**（`test-driven-development`：「NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST」／「Write code before the test? Delete it. Start over.」／小改動**明確不在**三項窄例外內）。

但 [[decision-framework]] 的五情境流程**完全沒有 TDD 步驟**：

```
〔讀〕現況層 → /requirements → agent 開發 → /stack-review → /doc-update
```

品質關卡走 `/stack-review`（程式審查）與 `/ai-pair dev`（多模型協作審查）。且 **TDD 已於 2026-07-29 明確決定暫不納入 decision-framework**（見本文件既有 TDD 註記）。

→ **裝一個以強制 TDD 為脊椎的框架，會直接推翻一個已經做過的決定。** 這比「框架重不重」的一般性爭論更關鍵。

### 風險登記（六項）

| # | 風險 | 嚴重度 | 依據 |
|---|---|---|---|
| 1 | **硬門檻主動壓制模型原生判斷力** | 🔴 高 | 上游 PR #2063 分類微實驗：現行入口決策文字把 bounded 任務 **5/5 推向 FULL ceremony**，而**完全無指引的 null prompt 反而能完美分類**。「絕對措辭正在主動壓制模型原生就會做的區辨」 |
| 2 | **儀式成本與任務風險不成比例** | 🔴 高 | 上游自家審計（2,240 session／24.21 GB rollout log）Finding 4：「process cost was not proportional to risk or uncertainty」。極端案例：一段話的規則變更產出 **3,061 行／14 commits／約 10 小時**，而承載邏輯只是一次 `git rev-parse --git-common-dir` 比對 |
| 3 | **子代理成本乘法** | 🟡 中（依規模） | 同一份需求被主代理＋實作代理＋規格審查＋品質審查各讀一次，重複隨任務數放大。作者自承「每任務開 3 個 subagent，50 任務燒 token 很快」。⚠️ **但有反向數據**：12 任務對照實驗顯示 token 反而**少 14%**——兩者量測不同規模區間，小量時「避免返工」勝出。上游 #2078（已合併 08-04）正在批次化小任務壓低此開銷 |
| 4 | **多 harness 維護成本 N 倍** | 🟡 中 | 官方明載「**If you use more than one, install Superpowers separately for each one**」。本工作流跨 Claude Code／Codex／Copilot／Antigravity，等於 N 份安裝、N 次更新，且各家機制不同（plugin／extension／手動 fetch）|
| 5 | **流程嚴格性 ≠ 正確性** | 🟡 中 | MikeWang 實測：完整跑完 brainstorming→writing-plans→TDD→自動審查，`isNumber` 對**全形數字**回傳 `true` 的漏抓**仍然沒抓到**。流程能保證「有做審查」，不能保證「審查抓到了」 |
| 6 | **Claude 5 世代受害更深，非更輕微** | 🔴 高 | 官方為 Claude 5 世代刪 8 成系統提示詞零下滑；官方 Opus 5 指南明指「交給子代理複查」反增工具呼叫與 token；強調詞已從技巧翻轉為負債，而 `using-superpowers` 前言正是 `EXTREMELY-IMPORTANT` 區塊＋全大寫封套＋12 列 Red Flags（約 450 字）。issue #2087 更指其「字面解讀甚至禁止模型在 skill 檢查前讀取附上的連結或提出釐清問題」|

**⚠️ 兩項必須並記的緩解事實**（避免本登記變成一面之詞）：
- 上游**已認可並在處理**：#2063 三路徑 router（`ceremony scales, approval never does`，仍在 `dev`）、#2078 批次化（已合併）、#2087 Claude 5 世代重構措辭（open）
- 強制措辭的必要性是**世代函數**——對舊世代模型它是合理設計；#2087 提議 per-model gating。**問題不是框架寫錯，是它為一個已過去的世代校準**

### 結論與若未來重新評估的條件

**暫不採用整套。** 但值得**摘用三個不依賴 TDD 的子模組**（對照 [[claude-code-skill-survivors-20-picks]]「90% 的人只用了 10%」的結論）：

| 子模組 | 為什麼值得 | 接到哪 |
|---|---|---|
| `verification-before-completion` | 🔑 **最高優先**：「宣稱完成但沒驗證是不誠實，不是有效率」＋ IDENTIFY→RUN→READ→VERIFY→ONLY THEN 五步；警告訊號＝出現「應該／大概／似乎」就停。**不依賴 TDD** | 接在 `/stack-review` 前面 |
| `systematic-debugging` | 四階段根因追蹤（root-cause-tracing／defense-in-depth／condition-based-waiting）| 情境 4/5 的除錯環節 |
| `writing-plans` | 極度詳細計畫、2–5 分鐘微任務、含驗證條件 | 可與 `/requirements` 對照取捨，不必兩套並行 |

> `verification-before-completion` 對本工作流特別對症——[[ai-era-five-irreplaceable-human-jobs]] 記錄過「信任 AI『全部測試通過』假報告」的翻車實例，這正是它針對的失敗模式。

**未來重新評估的觸發條件**：
- [ ] 上游 #2063 三路徑 router 合併進 `main`（屆時「不可漸進」這項否決可能解除）
- [ ] #2087 的 per-model gating 落地（屆時 Claude 5 世代的風險 6 可能解除）
- [ ] 或 decision-framework 正式納入 TDD（屆時最關鍵的衝突消失）

→ 三者任一成立時值得重評；在此之前**只摘子模組，不裝整套**。
