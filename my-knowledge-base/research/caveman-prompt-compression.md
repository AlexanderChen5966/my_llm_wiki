---
title: Caveman：穴居人模式 Prompt 壓縮技術
date: 2026-05-19
tags: [ai/prompt-engineering, ai/workflow]
source: https://cyberq.tw/2026/05/11/caveman-ai-token-saver/
type: article
status: processed
asset_value: 45
---

Caveman 是一個開源 Claude Code Skill，強迫 AI 用極端壓縮的「穴居人式」縮略英文回答，自報可減少 65-75% 輸出 Token 消耗。GitHub：[JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)（57k stars）。

## 核心機制

**Prompt Compression 技術**：省略冠詞、刪除修飾語、避免客套話，用關鍵詞與箭頭取代完整句子。

原本的完整敘述 → 只剩幾個關鍵字與流程指示。

## 適用範圍與限制

**適合用在**：
- 機械式 coding 任務（產 boilerplate、改變數名、寫測試）
- 長時間 Agent 工作流中的 context 節省
- 快速掌握關鍵資訊的 debug 工作流

**不適合用在**：
- 需要 chain-of-thought 的複雜推理任務（過度壓縮可能導致邏輯跳步）
- 複雜演算法、跨檔案重構（影響思考品質）
- 高風險的程式設計工作（連作者本人也不敢完全依賴）

**建議策略**：該省 token 時 caveman 上，該想清楚時關掉。

## 相關生態

Caveman 已演變成完整 Token 壓縮生態：
- **Cavemem**：長效記憶管理
- **Cavekit**：自動化工作流程與規格導向開發

官方展示：21,000 Token 的工作流多層壓縮後可降至 4,800 Token（節省約 77%）。

## 支援工具

Claude Code、Gemini CLI、Cursor、Copilot、Cline、Windsurf 等 40+ 種 AI 開發工具。

還提供**文言文模式**，利用中文古典語法高度濃縮的特性進一步降低 Token 使用量。

## 2026-07-13 補充：中文支援疑慮與 Ponytail 比較

社群反映「Caveman 對中文支援不理想」，此說法方向正確，三個結構性原因：

1. **壓縮句法建立在英文 tokenizer 特性上**：英文單字≈1 token，刪冠詞/修飾語/填充詞的收益直接；中文 1 字約 1-2 token，「刪字」的邊際收益天生低於英文
2. **官方 benchmark 全部是英文任務**：65% 平均節省（範圍 22-87%）沒有任何中文任務的實測佐證
3. **中文電報式縮寫的歧義成本**：壓縮後的中文指令容易產生語意歧義讓模型誤解，返工的 token 成本可能抵銷節省

`wenyan` 文言文模式雖是官方對中文的解法嘗試（利用古典漢語濃縮特性），但同樣缺乏實測數據，且文言文在現代開發語境（框架名/API/錯誤訊息混排）的理解風險未被驗證。

**與 Ponytail 的軸線區分**：Caveman 壓縮「對話文字」（語言相依性高），Ponytail 減少「生成程式碼」（語言中立，中文對話下指令效果不變）——兩者不是替代關係。**中文工作環境、輸出以程式碼為主時，Ponytail 是更可靠的選擇**；完整比較表見 [[github-copilot-token-reduction]]，開發流程整合見 [[dev-workflow-optimization-tools]]。

## 關聯條目

- [[claude-code-context-management]] — 正規的 Context 管控策略（手動存檔 + /clear）
- [[claude-skills-ecosystem]] — Caveman 在 Skills 生態的位置（第二檔明星框架：57k stars）
- [[dual-tool-strategy]] — Claude + Gemini 的成本分工策略（另一種節省 Token 方式）
- [[ponytail-lazy-senior-dev-skill]] — 語言中立的程式碼減量方案（中文環境優先選項）
- [[github-copilot-token-reduction]] — Caveman vs Ponytail 完整軸線比較表

## 補充資料：GitHub 官方 README 詳細資訊（JuliusBrussee/caveman）

來源：https://github.com/JuliusBrussee/caveman

### 實測 Benchmark 數據

平均 65% 輸出 Token 減少（10 個任務，範圍 22-87%）：

| 任務 | 正常模式 | Caveman | 節省 |
|------|---------|---------|------|
| React re-render bug 解釋 | 1180 | 159 | 87% |
| Auth middleware 修復 | 704 | 121 | 83% |
| PostgreSQL 連線池設置 | 2347 | 380 | 84% |
| Git rebase vs merge 解釋 | 702 | 292 | 58% |
| Refactor callback→async | 387 | 301 | 22% |
| **平均** | **1214** | **294** | **65%** |

**重要限制**：Caveman 只影響 output tokens，thinking/reasoning tokens 不受影響。「讓嘴巴更小，不是讓大腦更小」。

### 四個壓縮層級

| 層級 | 描述 |
|------|------|
| `lite` | 只刪除填充詞 |
| `full` | 預設穴居人模式 |
| `ultra` | 電報式極度精簡 |
| `wenyan` | 文言文模式（利用古典漢語濃縮特性）|

### 完整工具集

| 指令/工具 | 功能 |
|---------|------|
| `/caveman [lite\|full\|ultra\|wenyan]` | 壓縮每次回覆，持續到 session 結束 |
| `/caveman-commit` | Conventional Commit 訊息，≤50 字標題 |
| `/caveman-review` | 單行 PR 評論格式 |
| `/caveman-stats` | 顯示 session token 用量 + 終生節省 + USD |
| `/caveman-compress <file>` | 重寫記憶檔案（CLAUDE.md 等），每次 session 啟動節省約 46% input tokens |
| `caveman-shrink` | MCP 中間件，壓縮 tool descriptions |
| `cavecrew-*` | Caveman 子代理（investigator/builder/reviewer），比 vanilla 少 60% tokens |

### 安裝指令

```bash
# macOS / Linux / WSL
curl -fsSL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.sh | bash

# Windows PowerShell 5.1+
irm https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.ps1 | iex
```
約 30 秒。需要 Node ≥18。觸發：輸入 `/caveman` 或說「talk like caveman」。

### Caveman 生態系

| Repo | 功能 |
|------|------|
| **caveman** | 輸出壓縮（output tokens）|
| **cavemem** | 跨代理記憶（cross-agent memory）|
| **cavekit** | 規格驅動建構迴圈（spec-driven build loop）|
| **cavegemma** | Gemma 4 31B 微調版（把壓縮能力直接燒進 weights）|

### 學術佐證

2026 年 3 月論文「Brevity Constraints Reverse Performance Hierarchies in Language Models」發現：強制大型模型給簡短回覆，在某些 benchmark 上**準確率提升 26 分**。簡短不一定更差，有時候更正確。

## 補充資料（來源：JuliusBrussee/caveman GitHub README）

### 最新版本功能更新

原版報導記載約 65% token 節省，GitHub README 官方數據顯示：
- 平均 **65% 輸出 token 減少**（10 個任務，範圍 22-87%）
- 宣傳數字有時寫「75%」——這是最佳案例（React re-render bug 達 87%），平均值是 65%

### Caveman Ecosystem 第四個工具（新增）

| Repo | 功能 |
|------|------|
| **cavegemma** | Gemma 4 31B 微調版——把壓縮能力直接燒進 model weights，無需每次 prompt |

完整生態：caveman（輸出壓縮）+ cavemem（跨代理記憶）+ cavekit（規格驅動建構）+ cavegemma（fine-tuned 模型）

### OpenClaw 整合細節

caveman 已支援一鍵安裝到 OpenClaw（自托管 AI Gateway）：
```bash
curl -fsSL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.sh | bash -s -- --only openclaw
```
- Skill 放在 `~/.openclaw/workspace/skills/caveman/SKILL.md`
- SOUL.md 自動注入，每個 turn 都啟用 caveman 模式，無需每 session 輸入 `/caveman`
- 可用 `--uninstall` 乾淨移除
