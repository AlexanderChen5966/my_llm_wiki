---
title: notebooklm-skill：零成本研究到多平台內容的自動化管線
date: 2026-05-20
tags: [ai/workflow, ai/mcp]
source: https://claude-world.com/zh-tw/articles/notebooklm-skill-open-source-research-to-content-pipeline/
type: article
status: processed
asset_value: 15
---

notebooklm-skill 是唯一串接「趨勢發現 → NotebookLM 深度研究 → AI 內容創作 → 多平台發布」完整流程的開源工具。基於 notebooklm-py 建構，利用 NotebookLM 的免費基礎設施（有引用、9 種產出物、零 API 費用），解決 AI 內容工作流的兩大痛點：幻覺引用與按次付費。

## T（核心論點）

Google NotebookLM 提供業界最低幻覺的研究能力（基於真實來源，可驗證引用）和豐富產出物生成，且完全免費。缺的只是自動化。notebooklm-skill 填補這個缺口，讓整個「研究 → 產出 → 發布」流程可以一個命令觸發。

## B（適用場景）

**適用：** 需要大量產出有引用依據內容的創作者；自動化內容管線（部落格 / 社群 / 電子報）；受夠 Perplexity 等按次付費 API 的開發者；Claude Code 使用者想整合 NotebookLM 能力
**不適用 / 注意：** 依賴 Google 帳號 session，需一次性瀏覽器登入；Google 服務條款禁止自動化存取（有潛在封號風險）；session 需定期更新（通常可持續數週）

## R（操作步驟）

### 認證（一次性）
```bash
pip install -r requirements.txt
python3 -m playwright install chromium
python3 -m notebooklm login  # 開啟瀏覽器登入一次
```

### 雙模式安裝

**作為 Claude Code Skill：**
```bash
mkdir -p .claude/skills/notebooklm
cp -r notebooklm-skill/SKILL.md .claude/skills/notebooklm/
cp -r notebooklm-skill/scripts/ .claude/skills/notebooklm/scripts/
```
然後直接告訴 Claude：「用 NotebookLM 研究 X 並寫一篇文章。」

**作為 MCP Server：**
```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "python3",
      "args": ["/path/to/notebooklm-skill/mcp-server/server.py"]
    }
  }
}
```

### 5 個 Pipeline 工作流

| 工作流 | 輸入 | 輸出 |
|--------|------|------|
| research-to-article | URLs | 1,000-2,000 字引用文章 |
| research-to-social | URLs | Threads / Instagram / Facebook 貼文 |
| trend-to-content | 地區 + 數量 | 每個熱門趨勢的內容 |
| batch-digest | RSS feed | 電子報摘要 |
| generate-all | URLs | 全部 9 種產出物 |

### 數字一覽

| 類別 | 數量 |
|------|------|
| CLI 指令 | 11 個 |
| MCP 工具 | 13 個 |
| Pipeline 工作流 | 5 個 |
| 產出物類型 | 9 種（全由 Google 伺服器生成，免費）|
| API 費用 | $0 |

## C（關鍵行動）

- [ ] 評估現有內容工作流是否能用 notebooklm-skill 自動化（特別是需要有來源引用的研究內容）
- [ ] 考慮組合 trend-pulse（發現趨勢）→ notebooklm-skill（深度研究）→ cf-browser（擷取內容）完整管線

## 原始摘要

獨特定位：有引用的研究 + 產出物生成 + 完整自動化 + 零成本，這個組合目前無競品。相關開源生態：trend-pulse（Google Trends / HN / Reddit 等 7 個來源即時趨勢）、cf-browser（Cloudflare Browser Rendering，9 個瀏覽器工具）。GitHub：claude-world/notebooklm-skill，MIT 授權。

## 補充資料（來源：notebooklm-skill MCP Servers，lobehub.com）

### MCP Server 完整工具列表（13 個）

| 工具 | 說明 |
|------|------|
| `nlm_create_notebook` | 建立筆記本並加入來源 |
| `nlm_list` | 列出所有筆記本 |
| `nlm_delete` | 刪除筆記本 |
| `nlm_add_source` | 加入來源到現有筆記本 |
| `nlm_ask` | 對筆記本提問（回傳答案 + 引用） |
| `nlm_summarize` | 取得筆記本摘要 |
| `nlm_generate` | 生成產出物（10 種類型）|
| `nlm_download` | 下載已生成的產出物 |
| `nlm_list_sources` | 列出筆記本中的來源 |
| `nlm_list_artifacts` | 列出已生成的產出物 |
| `nlm_research` | 執行深度網頁研究 |
| `nlm_research_pipeline` | 完整研究 Pipeline |
| `nlm_trend_research` | 趨勢 → 研究 Pipeline |

### 多客戶端支援設定

Claude Code（`.mcp.json`）、Cursor（`~/.cursor/mcp.json`）、Gemini CLI 皆使用相同的 MCP Server 設定格式，只需調整 `cwd` 路徑：

```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "python",
      "args": ["-m", "mcp-server"],
      "cwd": "/path/to/notebooklm-skill"
    }
  }
}
```

## 相關連結

- [[notebooklm-py-cli-tool]] — notebooklm-py 底層 CLI 工具詳解
- [[agent-skills-vs-mcp]] — Claude Code Skill 與 MCP 雙模式選擇邏輯
- [[dual-tool-strategy]] — Claude + 其他工具的成本分工策略
