---
title: Claude Code + Obsidian 知識庫建構指南
date: 2026-05-13
tags: [pkm/workflow, ai/claude, pkm/obsidian]
source: https://www.bnext.com.tw/article/90530/llm-knowledge-base-obsidian-claude-code
type: article
status: processed
asset_value: 72
---

## T（核心論點）
Claude Code 能直接進入本地資料夾讀寫，是比網頁版 AI 更強大的知識庫引擎——你把資料丟進 Obsidian，Claude Code 負責摘要、分類、交叉連結，讓 100 篇筆記 40 萬字仍然查得動。

## B（適用場景）
**適用：** 已有 Obsidian Vault，需要系統化整理大量碎片素材；有 Claude Pro 訂閱
**不適用：** 偶爾查詢的一次性場景；不想付費的情境（可考慮 NotebookLM 替代）

## R（操作步驟）
1. 安裝 Obsidian，建立 Vault，設定 PARA 資料夾結構（raw/wiki/outputs）
2. 安裝 Obsidian Web Clipper，設定存入路徑為 `raw/`
3. 安裝 Claude Code（claude.ai/code），指向 Vault 資料夾
4. 建立 CLAUDE.md，定義知識庫規則（資料夾用途、frontmatter 格式、整理邏輯）
5. 在 Claude Code 下指令：「讀取 raw/ 按 CLAUDE.md 規則整理成 wiki/」
6. 每次查詢結果回存 outputs/（觸發知識複利）

## C（關鍵行動）
- [ ] CLAUDE.md 是核心——沒有它 AI 不知道你的規則
- [ ] raw/ 只丟不整理，整理是 AI 的工作
- [ ] 查詢結果回存 outputs/，讓知識庫越用越厚

## 原始摘要
數位時代文章，以 Karpathy 的 LLM 知識庫為基礎，詳解如何用 Claude Code + Obsidian 建構可查可問的 AI 知識庫。強調 Claude Code 與網頁版 AI 的關鍵差異：前者可直接操作本地資料夾。需要 Claude Pro 帳號（每月 20 美元）。

## 補充資料（來源：eugeniughelbur/obsidian-second-brain，2026-05-18）

### obsidian-second-brain：Karpathy LLM Wiki 的進化版

這是一個跨 CLI 技能包，一套程式碼支援 Claude Code、Codex CLI、Gemini CLI 和 OpenCode 四個平台，讓 Obsidian Vault 成為自我更新的 AI-first 第二大腦。

**核心差異（vs Karpathy 原版方法）：**

| 面向 | Karpathy LLM Wiki | obsidian-second-brain |
|------|------------------|----------------------|
| 新素材處理 | 建立新頁面 | **改寫現有頁面**，更新相關人物/概念，解決矛盾 |
| 矛盾偵測 | 手動解決 | `/obsidian-reconcile` 自動解決 |
| 跨源模式 | 詢問時才浮現 | `/obsidian-synthesize` 主動尋找無名模式 |
| 運行時機 | 按需 | 4 個排程 Agent（夜間/週回顧/矛盾掃描/健康檢查）|
| 筆記格式 | 人類可讀 | AI-first：`## For future Claude` + frontmatter |

**33 個命令分四層架構：**
- **Layer 1（Operations，21 個）**：`/obsidian-save`、`/obsidian-ingest`、`/obsidian-synthesize`、`/obsidian-reconcile`、`/obsidian-daily` 等
- **Layer 2（Thinking，4 個）**：`/obsidian-challenge`（以你的歷史記錄反駁你的想法）、`/obsidian-emerge`（浮現30天內的無名模式）、`/obsidian-connect`（橋接兩個不相關領域）、`/obsidian-graduate`（把想法碎片轉為完整專案）
- **Layer 3（Context，1 個）**：`/obsidian-world`（漸進式 token 預算的身份與狀態載入）
- **Layer 4（Research，6 個）**：`/x-read`、`/x-pulse`、`/research`、`/research-deep`、`/notebooklm`、`/youtube`（需 API Key）

**研究工具雙軌設計：**
- **開放網路軌**：`/research-deep` via Perplexity + Grok，獲取外部最新信號
- **Vault 錨定軌**：`/notebooklm` via Gemini File Search，根植你已有的知識

**安裝（Claude Code）：**
```bash
git clone https://github.com/eugeniughelbur/obsidian-second-brain ~/.claude/skills/obsidian-second-brain
bash ~/.claude/skills/obsidian-second-brain/scripts/setup.sh "/path/to/your/vault"
# 然後執行 /obsidian-init
```

研究工具 API 費用參考（2026-04）：`/x-read` ~$0.05、`/research-deep` ~$0.40-0.80、`/notebooklm` ~$0.004（Flash）

## 相關連結
- [[obsidian-visual-skills]] — Visual Skills Pack：讓 Claude 自動生成 Canvas／Excalidraw／Mermaid，補上本篇純文字流程的視覺化一端
- [[karpathy-knowledge-base-method]] — 同一方法的深度介紹
- [[obsidian-web-clipper]] — raw/ 輸入端工具

## 補充資料：Ingest / Query / Lint 三模組實作（科技翰林院，2026-04-14）

來源：techhanlin.tw，Claude Code + Obsidian 完整建構心得。

### 三大核心模組

**Ingest（匯入）**：Claude Code 掃描 `raw/` 資料夾 → 讀取所有 Markdown → 自動歸納核心論點 → 寫入 wiki/ 對應頁面，並建立 wikilink 連結。支援指令：「把 raw 的所有文章整理進 wiki」。

**Query（查詢）**：直接對整個 vault 提問，Claude Code 讀取相關 wiki 頁面 → 彙整回答。適合用在「我之前存了什麼關於 X 的文章？」或「整理一下我對 Y 的所有理解」。

**Lint（維護）**：定期執行 vault 健康檢查：找出孤立筆記（無 inbound link）、過時條目（超過 90 天未更新）、格式不符規範的頁面，並提示修復建議。

### Scheduled Agents 自動化

Claude Code 2026 新增排程代理：
```
# 每天早上自動整理昨天的 raw/ 素材
claude schedule "每天 08:00 執行 /obsidian-ingest"
```
不需手動觸發，raw/ 積累到一定量自動消化。

### Claude Code 能力對照

| 能力 | 知識管理用途 |
|------|------------|
| 檔案讀寫 | 直接操作 Obsidian vault 的所有 .md |
| CLAUDE.md 持久記憶 | 記住 vault 結構、筆記規範、偏好設定 |
| Subagent 平行處理 | 同時掃描多個資料夾、批次加標籤 |
| Scheduled Agents | 每天自動 Ingest，無需手動觸發 |
- [[karpathy-no-obsidian-setup]] — 不用 Obsidian 的極簡版本
- [[llm-wiki-karpathy-vs-nashsu]] — Karpathy 方法論 vs 進階 App 實作比對
