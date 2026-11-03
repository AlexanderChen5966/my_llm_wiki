---
title: bitwize-music（claude-ai-music-skills）：用 Claude Code plugin 做專輯製作 pipeline 的架構參考
date: 2026-11-03
tags: [ai/claude, ai/agent, ai/mcp]
source: https://github.com/bitwize-music-studio/claude-ai-music-skills
type: reference
status: processed
asset_value: 24
---

> 註：asset_value 24（頻率 2×耗時 3×複雜度 4），剛好在合併門檻。音樂生成本身屬領域邊緣，**只取架構面**：plugin／skills／MCP／多模型分層／品質閘門；音樂生成細節（曲風、母帶、Suno 提示詞）從簡。
> **來源性質**：GitHub README 快照（2026-10，作者空白、非第三方評測）。數字（53 skills、80+ tools、4,412 測試等）皆為 README 自述，**未實際安裝驗證**。README 末尾的 X／Discord 邀請屬作者社群導流，不收。
> **時效／限制**：
> - 最適合 **Claude Max**；**Pro 在多曲目工作階段會碰速率限制**（README 自述）
> - 開發在 **`develop` 分支**，**`main` 只收測試過的穩定版**——要穩定版請用 main
> - 音樂由使用者在 Suno 平台自行生成（無完整自動化）；版權與平台條款責任在使用者；Suno 商用須付費方案
> - 授權 CC0（公共領域）

## T（核心論點）

bitwize-music 是一個 **Claude Code plugin**：把一段對話轉成從概念、歌詞、Suno 提示詞、母帶處理到發行準備的專輯製作流程。作者自述動機是拿音樂當領域，深入練習 Claude Code plugin 架構、agentic workflow、多模型編排與 MCP 工具。對本庫有價值的是它的**工程骨架**：53 個 skills、80+ tools 的 MCP server、依任務分 Opus／Sonnet／Haiku 三層、以及「沒過閘門就不往下走」的品質閘門與人工簽核。

## B（適用場景）

**適用：**
- 想看「一個大型 Claude Code plugin 能做到多複雜」的具體案例：skills 數量、MCP 與 skills 的分工、模型分層、CI 規模
- 設計自己的 plugin／skills 時，參考多模型 tier 別名、品質閘門與人工簽核點的寫法

**不適用 / 注意：**
- 不是音樂生成的評測；README 無任何產出品質的第三方驗證
- 需要 Max 方案才順手；沒有 Max 時不要期待多曲目連續跑
- 不要把 develop 分支當穩定版

## R（架構面，依 README 轉述）

### 安裝（plugin marketplace）
1. `/plugin marketplace add bitwize-music-studio/claude-ai-music-skills`
2. `/plugin install bitwize-music@bitwize-music`
3. `/bitwize-music:setup` 偵測環境並安裝相依；`/bitwize-music:configure` 設定藝名與工作路徑
- 支援 macOS、Linux、WSL2、原生 Windows；MCP server 與音訊工具需 Python 3.11+

### Skill 系統（53 個）
- 每個 skill 是一份帶 YAML frontmatter 的 markdown，frontmatter 宣告**用哪個模型、描述、何時啟用**
- Claude 依情境自動路由，或用 `/bitwize-music:<name>` 直接呼叫
- 角色式 skill 例：lyric-writer（韻律、押韻、發音風險）、mastering-engineer（各平台響度目標）、researcher（協調 10 個領域子代理並行）

### 多模型分層（以 tier 別名而非固定版本）

| 層 | `model:` | skills 數 | 用途 |
|---|---|---|---|
| Creative | `opus` | 7 | 歌詞、Suno 提示詞、專輯概念、法務／查證研究——產出品質決定音樂品質 |
| Reasoning | `sonnet` | 30 | 研究協調、發音分析、多數工作流 |
| Mechanical | `haiku` | 16 | 匯入、驗證、剪貼簿、說明——速度優先 |

- skills 宣告**層級別名**，不鎖版本，新一代 Claude 出來時不用逐個 skill 修改（README 自述）

### MCP server（80+ tools，Python）
- 作為外掛的「神經系統」：skills 呼叫 MCP tools 而不是直接讀檔，**維持回應快速與狀態一致**
- 類別：狀態管理（專輯／曲目查詢、session、快取重建）、歌詞分析、音訊處理（母帶、分軌、QC、宣傳影片）、資料庫（PostgreSQL 管理推文／宣傳內容）

### 研究系統（人工簽核後才能寫歌詞）
- 主研究者派給 10 個領域子代理並行；驗證代理交叉檢查所有主張，再交人工審查
- 流程：蒐集來源 → 驗證引用 → **人工簽核** → 才允許產生歌詞

### 品質閘門
- 歌詞：13 點檢查表
- 生成前：來源已驗證、旗標設定、風格提示完整、藝人名稱已清查
- 音訊：7 點 QC（響度、削波、靜音、相位、立體寬度、頻率平衡、動態範圍）
- 結構：專輯目錄驗證、檔案位置與內容完整性

### 規模與 CI（自述）
- 53 skills、72 個曲風目錄、46+ 參考文件、4,412 個測試（14 類）；6 個 GitHub Actions workflows（ubuntu／macOS／Windows 測試、lint、安全掃描、真實服務整合、夜間深度測試、自動發版、PR 目標分支檢查、版本同步）
- 覆蓋率在三個 OS 合併後才過閘門，避免平台專屬分支在 Linux 上被看不見

## C（關鍵行動）

- [ ] 要參考架構：讀 README 的 Architecture 與 `docs/skills.md`、`reference/model-strategy.md`（每個 skill 的模型理由）
- [ ] 設計自己的 skills 時，試用「tier 別名＋任務性質分層」與「MCP 承載狀態查詢」兩個做法
- [ ] 要實際安裝前先看 main 與 develop 的差異，並確認自己的方案額度
- [ ] **未來整合建議（本批不處理）**：`raw/` 另有未處理的 Suno 相關檔——zhihu「用 Claude 和 Suno 生成音乐！MCP 一键集成指南」與 `junhao-yesuno-lyrics-skill`，建議之後與本條一起評估成「Suno／音樂生成」群組

## 原始摘要

bitwize-music-studio/claude-ai-music-skills（GitHub README，2026-10 快照）：Claude Code plugin，把對話轉成專輯製作流程；53 skills、Python MCP server 80+ tools、opus 7／sonnet 30／haiku 16 的模型分層、歌詞 13 點／音訊 7 點品質閘門、10 個領域子代理並行研究加人工簽核；最適合 Claude Max，Pro 多曲目會碰速率限制；develop 為開發分支、main 為穩定版；CC0 授權。

## 相關連結

- [[agent-skill-design-patterns]] — Skill 設計模式，可對照本案的角色式 skills 與品質閘門
- [[claude-code-plugins-guide]] — Plugin 六元件系統與 Marketplace，本案是大型實例
- [[agent-skills-vs-mcp]] — Skills 與 MCP 的分工，本案以 MCP 承載狀態、skills 負責流程
- [[google-lyria-3-5-music-generation]] — 音樂生成的競品對照（Google Lyria 與 Suno）
- [[ideas/claudia-claude-code-ai-music-video-pipeline]] — 同為 Claude＋Suno 的案例（單源個案 vs 本條的 plugin 架構）
