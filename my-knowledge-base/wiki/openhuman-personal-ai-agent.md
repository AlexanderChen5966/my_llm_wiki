---
title: OpenHuman — 個人 AI 超級助手與 Memory Tree 長期記憶系統
date: 2026-06-17
tags: [ai/agent, pkm/workflow]
source: https://github.com/tinyhumansai/openhuman
type: article
status: processed
asset_value: 60
---

大多數 AI Agent 有「冷啟動問題」：每次開新 session 都要重新交代專案背景、工作脈絡、客戶資訊。OpenHuman 試圖解決這個問題，透過 118+ OAuth 整合自動同步 Gmail、GitHub、Notion、Slack 等服務，每 20 分鐘建立一次 Memory Tree，將你的工作知識壓縮成 Obsidian 相容的 Markdown 知識庫。

## T（核心論點）

**OpenHuman 的核心不是聊天機器人，而是「讓 AI 在幾分鐘內建立長期記憶」的個人 AI 作業系統。** 靈感來自 Karpathy 的 LLM Knowledgebase 概念：所有文件、郵件、程式碼、行事曆都壓縮成結構化 Markdown，存進本地 Obsidian 風格知識庫，讓 Agent 隨時可搜尋和引用個人上下文。

技術棧：Rust + Node.js + Tauri（桌面應用）+ SQLite（本地存儲）。Local-first 設計，記憶資料存在本機，使用者可直接閱讀、修改知識庫內容。

## B（適用場景）

**最適合：**
- 知識工作者、工程師、創業者，每天處理大量郵件、文件、GitHub commit、會議紀錄
- 希望 AI 助手不需要每次重新解釋背景
- 想把個人工作知識資產化、建立可搜尋知識庫的人

**謹慎考慮：**
- 目前處於 Beta 階段，部分整合服務與跨平台支援仍在改善
- 預設透過 OpenHuman 管理後端代理 OAuth 和模型路由，若需完全自主需配置 direct mode（Composio 自行託管）

## R（完整內容）

### 核心機制：Memory Tree + Obsidian Wiki

1. **Auto-fetch**：每 20 分鐘自動從所有連接的服務拉取新資料
2. **Memory Tree**：將大量資料壓縮成 ≤3k-token Markdown chunks，評分後折疊進層級摘要樹
3. **Obsidian Wiki**：chunks 存為 `.md` 檔，Obsidian 相容，使用者可瀏覽與編輯
4. **SQLite 本地存儲**：所有記憶在本機，加密存放

Memory Tree 架構受 Karpathy 的 obsidian-wiki workflow 啟發，同時支援 [[agentmemory]] 作為可選的進階記憶後端（`memory.backend = "agentmemory"`）。

### TokenJuice：成本壓縮技術

每個 tool call、郵件內容、搜尋結果進 LLM 前都經過壓縮：
- HTML → Markdown 轉換
- 長 URL 縮短
- 冗餘 tool output 去重與摘要
- CJK 與 emoji 保留（逐字素）
- 官方宣稱降低成本與延遲最多 80%

### 118+ 整合服務

透過 Composio connector layer 的 OAuth 一鍵連接，包含：
- 溝通工具：Gmail、Slack、Discord
- 開發工具：GitHub、Linear、Jira
- 生產力工具：Notion、Google Drive、Calendar、Stripe
- 其他 100+ 服務

### 多模型路由 + 本地 AI

預設透過 OpenHuman backend 路由（reasoning/fast/vision 三種 workload），也支援 Ollama 本地 AI（`memory.routing = "local"`）。

### 桌面助手特色（差異化）

- **Mascot 虛擬助手**：桌面浮動 AI 角色，有嘴型同步、可作為 Google Meet 真實參與者、背景持續思考
- **原生語音**：STT 輸入 + ElevenLabs TTS 輸出
- **完整 coder toolset**：filesystem、git、lint、test、grep 內建

### 與同類工具比較

| | OpenHuman | OpenClaw | Hermes Agent |
|--|-----------|---------|-------------|
| 記憶系統 | Memory Tree + Obsidian vault | Plugin 依賴 | 自我學習（Episodic/Skill） |
| 整合數量 | 118+ OAuth | BYO | BYO |
| Auto-fetch | ✅ 20 分鐘 | ❌ | ❌ |
| 本地優先 | ✅ SQLite | ✅ | ✅ |
| UI 易用性 | 桌面 GUI | Terminal-first | Terminal-first |

### 安裝

```bash
# macOS
brew tap tinyhumansai/core && brew install openhuman

# Linux (Debian/Ubuntu)
# 按照簽名 apt repo 安裝（見 GitHub README）

# Windows
# 下載 .msi 安裝檔
```

## C（關鍵行動）

- [ ] 安裝 OpenHuman，完成首次設定，連接最高頻使用的 2-3 個服務（Gmail + GitHub + Notion）
- [ ] 讓系統跑一次完整 auto-fetch，觀察 Memory Tree 的結構化結果
- [ ] 打開 Obsidian 連接本地 vault 路徑，確認可讀取 AI 建立的知識庫
- [ ] 若已有 [[agentmemory]]，設定 `memory.backend = "agentmemory"` 讓兩者共用記憶層
- [ ] Beta 階段：重要任務保留人工驗證，不完全依賴自動化記憶

## 相關連結

- [[agentmemory]] — 跨 session 持久記憶伺服器（OpenHuman 支援作為可選後端）
- [[hermes-agent]] — Hermes Agent 同樣解決 AI 長期記憶問題，偏向開發者 terminal 使用
- [[opensquilla-intelligent-model-router]] — OpenSquilla 同樣有模型路由，偏向 coding 場景成本控制
- [[headroom-context-compression]] — Headroom 與 TokenJuice 解決相似問題（context 壓縮），但方法不同
- [[synthesis-karpathy-llm-wiki-method]] — Karpathy LLM Knowledgebase 概念（OpenHuman 的靈感來源）
