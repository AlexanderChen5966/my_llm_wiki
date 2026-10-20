---
title: ZeroSpec：AI 開工前的技術交接單框架
date: 2026-05-14
tags: [ai/agent, dev/architecture]
source: https://github.com/corey924/ZeroSpec
type: article
status: processed
asset_value: 78
---

ZeroSpec 是零依賴的 Markdown 框架（v0.5.2），在 AI Agent 開始動手前，把最小必要的專案上下文固定到可預測的位置——架構邊界、模組導航、版本真相來源（source of truth）。問題的根本不是模型能力，而是上下文缺口。

## T（核心論點）

AI 給出「幾乎對但不完全對」的答案，66% 的原因是上下文不完整，而非模型不夠強。ZeroSpec 的核心命題：**先把專案整理到 AI 看得懂，再選用什麼工具都差不多**。換模型不能解決隱性規則和團隊默契的缺口。

ZeroSpec 是 **SDD-ready 的 Layer 0 基線**：用事件觸發維持 SPEC / ADR / SA 的持續更新，但不負責 phase gate、簽核流程與執行狀態轉換。需要嚴格流程編排時，再銜接 Layer 1 工具（OpenSpec / Spec Kit）。

## B（適用場景）

| 適合導入 | 建議跳過 |
|---------|---------|
| 需要一套可長期維護的 Agent 指引基線 | Repo 為一次性用途（POC、教學、短期腳本） |
| 希望零依賴且不綁定單一平台 | Repo 以內容筆記為主，沒有長期維護程式碼需求 |
| 希望文件能跟著開發節奏演進，不是一次性產物 | 架構仍在探索期，尚無法定義穩定硬規則 |
| 需要 Layer 0 的上下文基線，再銜接 Layer 1 工具 | 團隊既有 Layer 1 已完整處理上下文注入 |

## R（操作步驟）

### Layer 0 定位與分層

| 層級 | 職責 | 代表工具 |
|------|------|---------|
| **Layer 0** | 讓專案「可被 AI 讀懂」— 架構約束、導航、SoT | **ZeroSpec** |
| **Layer 1** | 讓 AI「照流程執行任務」— 工作流、phase gate | OpenSpec, Spec Kit |

ZeroSpec 不取代 Layer 1 工具，而是讓 Layer 1 更容易接上。如果想先試 SDD 輔助開發但還不想 Day-1 導入完整 workflow，ZeroSpec 很適合起步。

### 四類最小必要上下文

| 類別 | 說明 | 解決什麼問題 |
|------|------|------------|
| 專案定位與架構邊界 | 哪些模組做什麼、邊界在哪 | AI 改到不該改的地方 |
| 版本真相來源 | 哪份文件是最新依據 | AI 引用舊 API 或廢棄寫法 |
| 領域→程式碼導航 | 任務路由（「新增 API → 看哪個檔案」） | AI 找不到入口亂猜 |
| 文件治理規則 | 哪些是底線 vs 哪些是習慣 | AI 不知道什麼可以改、什麼不能改 |

### 內容模型（A/B/C 分工）

| 類別 | 誰負責 | 人工投入 |
|------|--------|---------|
| **(A) AI 自動產生** | AI 掃描 Repo | 零 |
| **(B) AI 草擬、人審核** | AI 歸納 + 人確認 | 審核 5 分鐘 |
| **(C) 人必須提供** | 無法從程式碼推導 | 回答幾個問題 |

### Prompt Pack 系統（事件觸發式）

ZeroSpec 的核心操作是一組 Prompt Pack，每個 Prompt 對應特定觸發事件：

**Bootstrap（一次性）：**

| 步驟 | Prompt | 用途 |
|------|--------|------|
| Step 1 | `INIT-SCAN.md` | 分析現況（不寫檔），產出結構化盤點 |
| Step 2 | `INIT-BUILD.md` | 產生 AGENTS.md + docs/README.md |
| Step 3 | 驗證 | 用真實小任務確認 Agent 遵守架構約束 |
| Step 4 | `SPEC.md` | 建立第一份 SPEC，進入持續運作 SDD 機制 |

**持續運作（事件觸發）：**

| 觸發事件 | Prompt Pack |
|---------|-------------|
| API 新增/變更 | `SPEC.md` |
| 架構決策 | `ADR.md` |
| 系統快照 | `SA.md` |
| 稽核 AGENTS.md 品質 | `AUDIT.md`（不寫檔）|
| 驗證既有 SPEC 是否仍跟程式碼一致 | `DRIFT.md`（不寫檔，v0.5.2 新增）|
| 多模組開發（3+ Controller/handler 或 2+ SPEC） | `IMPL.md`（實作並同步 SPEC，v0.5.2 新增）|
| 專案演進同步文件 | `UPDATE.md` |
| 未觸發以上事件 | **不建立任何文件** |

### 快速起步（30 分鐘內）

1. 在目標專案開啟 Agent 模式，貼上 `INIT-SCAN.md`
2. 在同一對話貼上 `INIT-BUILD.md`，回答 AI 的 2-3 個待確認問題
3. 審核產出的 `AGENTS.md` 與 `docs/README.md`，用真實小任務驗證
4. 根據 AI 建議，用 `SPEC.md` 建立第一份 SPEC

ZeroSpec 獨立存放即可，**不需要加入目標 Repo**。Bootstrap 完成後，目標 Repo 只新增 `AGENTS.md` 和 `docs/README.md` 兩個檔案。

### 支援的 AI 工具

| 工具 | 啟用方式 |
|------|---------|
| GitHub Copilot (VS Code) | Agent 模式（確認 `#codebase` 可用）；需 `@AGENTS.md` 引用或建立 `.github/copilot-instructions.md` |
| Cursor | Composer — Agent（非 Chat 模式）|
| Codex CLI | 在專案根目錄啟動，根據 AGENTS.md 慣例自動讀取 |
| Generic CLI | 在專案根目錄啟動，貼入 Prompt Pack 內容 |
| Claude Code | 預設即具備讀寫能力 |
| Windsurf | Cascade 模式 |
| JetBrains AI Assistant | 開啟 Attach project files |

不建議：無法完整存取本機 Repo 的環境（ChatGPT / Claude.ai 網頁版）。

### 可選 Adapter（三種，均非必要）

| Adapter | 功能 | 安裝位置 |
|---------|------|---------|
| **Prompt Files** | VS Code Prompt 介面的快捷入口 | `.github/prompts/`（從 `templates/prompts/*.prompt.md` 複製）|
| **Agent-Skill** | 依意圖路由到正確 Prompt Pack | 專案內：`.agents/skills/zerospec/`（Copilot）；個人全域：`~/.claude/skills/`（Claude Code，用 `sync-skills.sh` 安裝）|
| **Pointers** | 引用/連結 AGENTS.md 的平台進入點 | Repo 根目錄（從 `templates/pointers/` 複製）|

Skills 目錄結構（v0.5.2 新增）：
- `skills/zerospec/SKILL.md` — Skill-style Router（已用 Claude Code 驗證）
- `skills/zerospec/prompts/` — Prompt 子檔（由 `sync-skills.sh` / `sync-skills.ps1` 從 `prompts/` 同步）
- `scripts/sync-skills.sh` / `.ps1` — 同步 prompts/ → skills/、安裝至 Claude 全域、或檢查漂移（可作 CI 門檻）

### 範例與驗收

**範例目錄**（`examples/`）：Day-1 最小產出、.NET 雙 API、Java Library、Python Package、React + Nx Monorepo。

**驗收腳本**：`bash scripts/verify-zerospec.sh`（macOS/Linux）或 `powershell scripts/verify-zerospec.ps1`（Windows），檢查核心 Prompt 檔案完整性。附 GitHub Actions CI 範本。

### 建議採用指標

| 指標 | 目標 |
|------|------|
| Day-1 人工投入時間 | ≤ 30 分鐘 |
| 人工新寫內容比例（C 類） | ≤ 20% |
| 首次回合可合併率 | ≥ 70% |
| 架構硬規則違反率 | ≤ 10% |
| API 變更後 SPEC 草稿覆蓋率 | ≥ 90% |

## C（關鍵行動）

- [ ] 從「我的專案有沒有整理到 AI 看得懂？」這個問題開始，不是「該用哪個工具？」
- [ ] 先寫「任務路由」（新增 API → 看哪個檔案），而非目錄樹概覽
- [ ] 把 AI 反覆犯錯的規則加到 NEVER 區塊，持續補充
- [ ] 用 `INIT-SCAN` + `INIT-BUILD` 完成 Bootstrap，再用真實小任務驗證
- [ ] 建議每月快速回顧、每季完整回顧 AGENTS.md 內容

## 補充視角：作者第一手說明（ZeroSpec 誕生脈絡）

來源：ZeroSpec 作者 corey924 文章（2026-04-25）：

**為什麼叫 ZeroSpec：**「Zero」有兩層意思——處理的是所有 workflow 開始之前的第零層（Layer 0，Context Readiness），同時刻意做成 zero-dependency，不綁任何平台或 runtime。Spec 指的是整理專案規格與規則讓 AI 穩定讀到。

**數據依據：**
- Stack Overflow Developer Survey 2025：66% 開發者最大挫折是 AI 答案「幾乎對但不完全對」；45.2% 覺得 debug AI 產出的程式碼比自己寫更花時間
- METR 2025 研究：資深開發者在自己熟悉的成熟專案中，使用 AI 工具在特定情境下反而慢了約 19%（解讀：上下文整備不足的問題，非 AI 無用）。🔴 **更關鍵的對照**（[[ai-era-five-irreplaceable-human-jobs]]）：研究結束後，這 16 位受測開發者仍**主觀相信自己「快了 20%」**——連使用者自己對 AI 效益的感受都不可靠，這也是判斷「上下文整備夠不夠」不能只靠自我感覺的理由
- Simon Willison：當模型不了解你的程式庫或專案脈絡時，補對上下文通常比一味換模型更有效
- Anthropic Context Engineering：找到最小的一組高訊號 token，讓 Agent 每次進來都能拿到剛好夠用的地圖。ZeroSpec 是這個原則在 Repo 層級的實踐

**與其他工具的層次關係：**
- ZeroSpec = Layer 0（Context Readiness，所有 workflow 開始前）
- OpenSpec / Spec Kit = Layer 1（SDD 規格驅動，在 ZeroSpec 之上）
- ZeroSpec 不取代 Layer 1 工具，而是讓 Layer 1 更容易接上

**選擇 plain Markdown 的理由：** 人可直接讀、AI 讀得懂、可進 Git 走 PR 做 diff、不依賴任何 runtime、模型升級不失效。與業界趨勢一致：AGENTS.md 格式由 Linux Foundation 旗下 Agentic AI Foundation 維護，6 萬個開源專案採用。

## 相關連結

- [[agents-md-guide]] — AGENTS.md 完整設計原則（ZeroSpec 建構在此標準上）
- [[harness-engineering]] — Context Engineering 即 Harness 的子模組
- [[spec-driven-ai-workflow]] — 規格驅動 AI 開發工作流（Layer 1：spec-kit / OpenSpec）
- [[research/dev-workflow-optimization-tools]] — 多 Agent 開發流程優化（ZeroSpec 作為 Layer 0）
- [[research/claude-pilot-suite-single-agent-workflow]] — 單一 Agent 開發流程（ZeroSpec + pilot-suite + planning-with-files）
