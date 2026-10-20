---
title: 雙工具策略：Gemini CLI 探索 + Claude Code 執行
date: 2026-05-14
tags: [ai/claude, ai/workflow]
source: https://www.termdock.com/zh/blog/dual-tool-strategy-gemini-cli-claude-code
type: article
status: processed
asset_value: 65
---

把 AI 工具費用節省 60-70% 的策略：Gemini CLI 免費方案負責低風險的探索性任務，Claude Code 負責需要深度推理的複雜執行。核心判斷：這個任務需要深度多步驟推理，還是明確的單步操作？

> ⚠️ **2026-05-19 更新**：Google 在 Google I/O 2026 宣布 Gemini CLI 整併為 **Antigravity CLI**，指令從 `gemini` 改為 `agy`，遷移截止 2026-06-18。策略方向不變，但所有 `gemini "..."` 指令需換為 `agy "..."`。詳見 [[google-antigravity-platform]]。

## T（核心論點）
Claude Code 是手術刀（精準貴），Gemini CLI（現改名 Antigravity CLI）是廚房菜刀（免費日用）。每天 60-70% 的任務可以用 Antigravity CLI 完成，省下 Claude Code 額度用在真正需要的地方。

## B（適用場景）
**適用：** 重度 Claude Code 使用者（Max 方案 $100-200/月）的成本最佳化
**不適用：** 只偶爾用的輕度用戶（直接用 Claude Code 就好）

## R（判斷框架）
| 任務類型 | 用哪個工具 |
|---------|-----------|
| 程式碼庫探索 / Code review / 寫測試 / 文件 | Gemini CLI |
| 3 個檔案以內的小 bug 修復 | Gemini CLI |
| 跨多檔案架構重構 / 複雜 debug | Claude Code |
| 10+ 檔案的新功能實作 / 安全相關 | Claude Code |

灰色地帶（3-5 個檔案重構）先用 Gemini CLI 試，不行再切 Claude Code——損失 2-3 分鐘但省付費額度。

## C（關鍵行動）
- [ ] Gemini CLI 安裝：`npm install -g @google/gemini-cli`，Google 帳號即可，每天免費 1,000 次請求
- [ ] 任何唯讀探索任務優先用 Gemini CLI

## 相關連結
- [[claude-code-context-management]] — Claude Code Context 管控策略
- [[ai-pair-skill]] — 多模型協作的進階玩法（Claude + GPT + Gemini）
- [[gemini-cli-subagent]] — Gemini CLI 接入 Claude Code 的具體設定步驟與安全配置

## 補充資料（來源：cross-tool-ai-setup.md）

### 跨工具 AI 協作：AGENTS.md 作為單一維護點

本補充來源是從 B2B Manager 專案實戰蒸餾的跨工具協作指南，核心原則是「單一維護點」。

**各工具原生讀取的 Context 檔案**

| 工具 | 原生讀取檔案 | 支援 @import |
|------|------------|------------|
| Claude Code | `CLAUDE.md`、`.claude/CLAUDE.md` | ✅ `@path/to/file` |
| Gemini CLI | `GEMINI.md` | ✅（最多 5 層遞迴）|
| OpenAI Codex CLI | `AGENTS.md` | ❌ |
| VS Code Copilot | `.github/copilot-instructions.md`、`AGENTS.md` | ❌ |
| Cursor | `.cursorrules`、`.cursor/rules/*.mdc` | ❌（有 ~2000 tokens 上限）|
| Windsurf | `.windsurfrules`、`AGENTS.md` | ❌ |

> 趨勢：`AGENTS.md` 正成為跨工具事實標準，Windsurf、Codex、VS Code Copilot 均支援。

### 推薦標準架構

```
AGENTS.md               ← 單一維護點：完整共用規則
GEMINI.md               ← @./AGENTS.md（+ Gemini 專屬補充）
CLAUDE.md               ← @AGENTS.md（+ Claude 專屬補充）
.claude/rules/          ← Claude Code 自動載入的分類規則（路徑專屬）
.github/
  └── copilot-instructions.md  ← 精簡版
.cursorrules            ← 精簡版（有 token 限制）
```

**日常修改流程**：
- 修改共用規則 → 只改 `AGENTS.md`，其他工具透過直接讀取或 @import 自動同步
- 修改 Claude 專屬規則 → 只改 `.claude/rules/` 或 `CLAUDE.md` 的專屬補充區塊

### @import 機制重要說明

| 特性 | 說明 |
|------|------|
| Token 用量 | **不節省 token**：imported 檔案在啟動時展開進入 context |
| 用途 | 避免複製貼上 + 確保同步，不是 lazy load |
| 路徑基準 | 相對路徑相對於「包含 import 的檔案本身」|

### .claude/rules/ 路徑專屬規則

在 rule 檔案頂端加 YAML frontmatter，讓規則只在 Claude 操作匹配路徑的檔案時才載入：

```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API 相關規則（只在處理上述路徑時才載入）
```

**何時加 `paths`**：規則只在特定目錄有意義時加；禁止事項和安全規則不加（常駐警戒）。

### 驗收自我測試（Self-Verification Test）

建立 AGENTS.md 時一併在文件末尾加入驗收題目（8-12 題），涵蓋：平台限制、禁止事項、高風險操作、架構核心元件、構建指令、輸出語言。

**價值**：強迫撰寫者確認「規則是否真的寫清楚了」，也作為新成員或新工具 onboard 時的快速校驗。

B2B Manager 實測數據：改架構後 Claude Code 總 context 從 ~315 行降到 ~250 行，維護點從 3 份降為 1 份。

## 補充資料（來源：〈調查發現 76% 工程師更愛 Claude Code 勝過 Codex，關鍵差異是什麼？〉，TechOrange，2026-09-04，https://techorange.com/2026/09/04/claude-code-openai-codex-ai-developers/；原始調查為 ZDNET David Gewirtz 138 位開發者自願受訪）

### 雙刀流的實務證據：把分工軸從「省額度」擴到「能力互補」

本條目的雙工具分工原本是**成本**驅動（貴的手術刀留給難題）。這份 138 人調查顯示，
另一組工具對（Claude Code × Codex）的分工是**能力**驅動，但判斷方式相同：先問任務性質，再決定派誰。

**採用率**：Claude Code 76%、Codex 35%、**同時使用兩者 22%**（另有 22 人刻意讓兩款工具互相對比／互相審查）。

**Claude Code 被信任的具體行為**（可作為「派給誰」的判準，而非品牌偏好）：

- **編輯任何程式碼前先讀整個 repo**，而不是憑空猜測就動手（NVIDIA 受訪者的信任理由）
- 大型／複雜程式碼庫的上下文處理較佳，**跨多檔案的變更仍能保持一致性**（＝本條目 R 層把「跨多檔案架構重構」派給 Claude Code 的外部佐證）
- 編輯前先規劃並推演多個變更方案，複雜重構所需的修正次數較少
- **破壞性操作前先徵求許可**，提供可稽核性與人工監督
- 涉及金錢／支付系統的任務，多次重構後仍能正確執行

**Codex 的實測優勢**：相同價格提供更多使用量、**沒有每週上限**（重度使用者最痛的限制）、
運作較可預測、變更後的清理工作較少、上下文管理與壓縮較好、介面較清晰、**審查程式碼時更擅長抓罕見問題**、
可無縫接上既有 ChatGPT 工作流。

**已被實踐的配對方式**（比單用任一工具效果更好）：

| 配對 | 分工 |
|------|------|
| Claude Code 設計 ＋ Codex 審查 | 設計端要判斷力，審查端要抓罕見問題（Novo 創辦人） |
| Claude Code 日常 ＋ Codex 第二意見 | 卡住時換一顆腦（行銷營運服務創始人） |
| Fable 規劃 ＋ Codex 實作 ＋ 兩者互審 | Fable 擅長創意／接近人類層級的問題，Codex 成本效益高；互審後人工挑選要採用的修改 |

🔑 **最有價值的反直覺結論：「框架」比模型本身更重要。** 測試方式、上下文情境、權限設定與限制條件，
都左右最終輸出——這意味本條目的分工策略若沒有配套的框架（AGENTS.md 單一維護點、權限邊界、驗收測試），
換哪個模型都救不了結果。同一份調查也提醒：AI 產出的程式碼仍會出現
**隱性故障（Silent Failure）**——表面看似正確執行、實際暗藏錯誤，因此 14 位受訪者主張人工審查不可妥協，
每項 AI 變更都必須過審。成本與用量追蹤困難（每週上限、Token 消耗、費用難追）是兩邊共同的抱怨。

⚠️ 樣本限制：138 位自願受訪者，非隨機抽樣；且使用 Codex 的受訪者中無人來自大型科技業者，
不宜直接讀成「大型企業不選 Codex」。

### 相關連結（本節）

- [[chatgpt-chat-work-codex-comparison]] — Codex 自身的 Chat／Work 模式分工
- [[four-vendor-ai-tool-selection]] — 四家廠商工具選型的全景對照
- [[codex-best-practices-guide]] — Codex 側的實務做法
- [[fable-5-delegation-cost-benchmark]] — Fable 委派與成本基準（本節「Fable 規劃 ＋ Codex 實作」的量化依據）
