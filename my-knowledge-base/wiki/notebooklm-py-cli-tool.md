---
title: NotebookLM-py：Google NotebookLM 的完整 Python CLI 工具
date: 2026-05-20
tags: [ai/workflow, pkm/automation]
source: https://tenten.co/learning/notebooklm-py-skill/
type: article
status: processed
asset_value: 12
---

notebooklm-py 是 teng-lin 開發的 Python CLI 工具，透過逆向工程 NotebookLM 的未公開 API，讓使用者完整程式化存取所有功能。解決 NotebookLM 無公開 API 的痛點，將閃卡、腦圖、報告等內容從介面鎖定狀態解放。

## T（核心論點）

NotebookLM 是低幻覺的 AI 研究工具（基於使用者上傳文件，每句都有可追溯來源），但官方不提供 API。notebooklm-py 打破這個限制，讓 NotebookLM 可作為知識庫，被 Claude Code（透過 Skills 整合）或其他 AI 代理呼叫，實現「NotebookLM 作知識庫 + Claude Code 作執行引擎」的組合。

## B（適用場景）

**適用：** 需要將 NotebookLM 整合進自動化工作流的開發者；個人研究、原型開發與內部工具整合；學術文獻回顧（批次處理多篇 PDF）；企業知識庫建構（搭配 Claude Code Skills）；YouTube 創作者的內容情報收集
**不適用 / 注意：** Google 可能隨時修改內部 API（工具暫時失效風險）；使用條款禁止自動化存取（有封號風險，建議用專用帳號）；不適用於需要穩定 API 的生產環境

## R（操作步驟）

### 安裝
```bash
pip install notebooklm-py
pip install "notebooklm-py[browser]"
playwright install chromium
notebooklm login  # 一次性 Google 帳號認證
```

### 三種使用模式

**CLI 指令模式**（Shell 腳本整合）：
```bash
notebooklm create "Q1 市場分析"
notebooklm source add "https://example.com/report.pdf"
notebooklm generate audio --wait
notebooklm download audio ./podcast.mp3
```

**Python API**：異步架構（asyncio），適合整合進 Python 應用程式

**Claude Code Skills**：安裝 notebooklm skill 後，用自然語言驅動文件查詢，Claude 自動連接 NotebookLM 執行查詢

### 功能矩陣

| 功能類別 | 支援內容 |
|--------|---------|
| 筆記本管理 | 建立、列表、重命名、刪除、分享 |
| 來源匯入 | URL、YouTube、PDF/TXT/MD/DOCX、Google Drive |
| 內容生成 | 音訊 Podcast、影片、簡報、測驗、閃卡、報告、資訊圖表、腦圖 |
| 下載匯出 | 音訊、影片、簡報、資訊圖表、報告、腦圖、數據表、測驗、閃卡 |

## C（關鍵行動）

- [ ] 評估是否需要 notebooklm-py 或 notebooklm-skill（後者提供完整自動化管線，見 [[notebooklm-skill-research-pipeline]]）
- [ ] 使用專用 Google 帳號，降低封號風險

## 原始摘要

技術架構：Playwright 瀏覽器自動化（模擬真實用戶操作），低幻覺特性（NotebookLM RAG 架構，所有回答追溯到具體來源段落）。Tenten 作者定位：AI 工具互聯趨勢的代表性案例。

## 相關連結

- [[notebooklm-skill-research-pipeline]] — 基於 notebooklm-py 的完整自動化管線（趨勢發現 → 研究 → 發布）
- [[agent-skills-vs-mcp]] — Claude Code Skill 與 MCP 雙模式選擇邏輯

## 補充資料：notebooklm-mcp（MCP 方案）

來源：PleasePrompto/notebooklm-mcp GitHub README（2026-05）

### 定位對比

| 方案 | 技術架構 | 整合方式 | 適用場景 |
|------|---------|---------|---------|
| **notebooklm-py** | Playwright 瀏覽器自動化（Python）| Claude Code Skill / Python API / CLI | 批次處理、Python 工作流整合 |
| **notebooklm-mcp** | Patchright Chrome（stealth + persistent fingerprint，Node.js）| MCP server | 任何 MCP 相容客戶端（Claude Code、Codex 等）|

### notebooklm-mcp 技術細節

- v2.0.0（v1 已停止支援），支援 `stdio`（預設）和 Streamable-HTTP 兩種傳輸
- 驅動真實 Chrome（或 fallback Chromium），讀取 DOM 層級引用，零幻覺
- 支援平台：Linux / macOS / Windows（WSL2 + WSLg 完整支援，WSL1 不支援）

### 安裝與串接 Claude Code

```bash
npx notebooklm-mcp@latest  # 一行安裝

# 串接 Claude Code
claude mcp add notebooklm -- npx notebooklm-mcp@latest
```

或手動加入 `~/.claude.json`：
```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "npx",
      "args": ["notebooklm-mcp@latest"]
    }
  }
}
```

**選擇建議**：只需 Python 工作流整合 → notebooklm-py；需要與多種 MCP 客戶端整合或已有 MCP 架構 → notebooklm-mcp。兩者都靠瀏覽器自動化，都面臨 Google 可能隨時更改介面的風險。

## 補充資料：Antigravity + NotebookLM MCP 組合（大腦 + 雙手）

來源：Hei_Ai 成長日記（Patreon，2026-03-10）

### 核心架構思路

**NotebookLM 的容量優勢**：一個 Notebook 可放 50 個來源，每個來源最多 50 萬字，遠超任何 IDE 的 context window。

組合邏輯：
- **NotebookLM = 大腦**（研究 + 記憶）：負責 Deep Research、儲存大量資料
- **IDE（Antigravity / Claude Code）= 雙手**（執行 + 建立）：負責寫 code、建立網站、產出內容

透過 **notebooklm-mcp**（提供 31 個工具）把兩者連接起來，IDE 需要什麼資料直接從 NotebookLM 抓取。

### 兩個實戰案例

**案例 1：批量 Deep Research（自動化研究）**
1. Antigravity 用 Web Search 找出 10 個 AI 工具組合方向
2. 選定 5 個方向，透過 MCP 自動在 NotebookLM 建立 5 個 Notebook
3. 每個 Notebook 自動發出 Deep Research 請求（約 5 分鐘，引用 33 個來源）
4. 同時自動啟動多媒體任務：簡報、音頻、心智圖、資訊圖、影片生成
5. **全程不打開 NotebookLM，所有操作透過 Antigravity 完成**

**案例 2：財報分析互動網站**
1. 在 NotebookLM 上傳 NVDA / AMD / 美光三份季度財報 PDF
2. Antigravity 讀取指定 Notebook 內容，生成互動式財報分析網站
3. 網站包含：營收對比、淨利潤走勢、年增長率、規模比較（全部互動式圖表）
4. 數據來自真實財報 PDF，非 AI 幻覺；約 10 分鐘完成

⚠ 注意：提示詞裡的 Notebook 標題必須跟 NotebookLM 上的標題**完全一致**，因為 Antigravity 透過標題找筆記本。

### 為什麼 NotebookLM Deep Research > IDE 原生 Web Search

NotebookLM Deep Research 一次引用 30+ 個來源做深度分析，品質遠超 IDE 的一般搜尋結果。建議用 NotebookLM 替代 IDE 原生的 Web Search 功能處理需要高品質研究的任務。

### Claude Code 用戶安裝注意

用 Claude Code 安裝 notebooklm-mcp 時，可能不會彈出登入視窗。需根據 AI 指引手動取得 Cookies，複製貼上給 AI Agent 完成授權。
