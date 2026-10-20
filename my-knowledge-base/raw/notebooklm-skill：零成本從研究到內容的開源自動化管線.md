---
title: "notebooklm-skill：零成本從研究到內容的開源自動化管線"
source: "https://claude-world.com/zh-tw/articles/notebooklm-skill-open-source-research-to-content-pipeline/"
author:
  - "[[Claude World]]"
published: 2026-03-14
created: 2026-05-15
description: "串接趨勢發現、NotebookLM 深度研究、AI 內容創作與多平台發布的唯一開源工具。支援 Claude Code Skill 與 MCP Server 雙模式，零 API 費用。"
tags:
  - "clippings"
status: processed
---
如果你能從「我想寫關於 X 的文章」到發布完成的文章、Podcast、投影片和社群貼文——全部自動化、有引用出處、而且零 API 費用呢？

這就是 **notebooklm-skill** 做到的事。

## AI 內容管線的痛點

多數 AI 內容工作流有兩個痛點：

1. **AI 生成的內容缺乏真實引用。** LLM 會幻覺出不存在的來源。即使是 RAG 管線也需要昂貴的 embedding 基礎設施。
2. **研究工具按次收費。** Perplexity、You.com 等服務每次搜尋都要付費，規模化後成本急遽上升。

Google 的 NotebookLM 同時解決了這兩個問題——它基於真實來源進行研究並提供可驗證的引用，能生成 9 種可下載的產出物（Podcast、投影片、報告、測驗等），而且完全免費。

缺少的是什麼？ **自動化。** NotebookLM 沒有公開 API，只能在瀏覽器中手動操作。

直到現在。

## notebooklm-skill 做什麼

**notebooklm-skill** 是唯一串接完整內容創作流程的開源工具：

```plaintext
趨勢發現 → NotebookLM 深度研究 → AI 內容創作 → 多平台發布
```

基於 [notebooklm-py](https://github.com/nichochar/notebooklm-py) v0.3.4 建構，它封裝了 NotebookLM 的內部 HTTP API。只需一次瀏覽器登入，後續所有操作都是純 HTTP 呼叫——沒有每次操作的瀏覽器自動化開銷。

### 雙模式架構

此工具支援兩種運行模式：

| 模式 | 運作方式 | 相容工具 |
| --- | --- | --- |
| **Claude Code Skill** | 將 `SKILL.md` 放入 `.claude/skills/` ，自動偵測觸發詞 | Claude Code |
| **MCP Server** | 執行 `python3 mcp-server/server.py` ，支援 stdio 或 HTTP 模式 | Claude Code、Cursor、Gemini CLI、任何 MCP 客戶端 |

不管你是 Claude Code 使用者、Cursor 使用者，還是正在建構自己的 MCP 相容代理，同一套工具都能用。

## 數字一覽

| 類別 | 數量 |
| --- | --- |
| CLI 指令 | 11 個命令 |
| MCP 工具 | 13 個工具 |
| Pipeline 工作流 | 5 個端到端管線 |
| 產出物類型 | 9 種可下載（全由 Google 伺服器生成） |
| 來源類型 | 11 種（URL、YouTube、PDF、Google Drive 等） |
| API 費用 | $0 |

## 9 種可下載產出物——全部免費

每種產出物都由 Google 的基礎設施生成，對你完全零成本：

| 產出物 | 格式 | 用途 |
| --- | --- | --- |
| Podcast（音訊概覽） | M4A | 深度探討、簡報、評論、辯論 |
| 影片 | MP4 | 視覺內容 |
| 投影片 | PDF | 簡報展示 |
| 報告 | Markdown | 長篇研究報告 |
| 學習指南 | Markdown | 學習指南格式 |
| 測驗 | JSON | 教學評量 |
| 閃卡 | JSON | 學習教材 |
| 心智圖 | JSON 樹狀結構 | 視覺化知識映射 |
| 資訊圖表 | PNG | 視覺摘要 |
| 數據表 | CSV/JSON | 結構化資料擷取 |

光是 Podcast 就支援四種格式： **深度探討** （15-30 分鐘）、 **簡報** （3-5 分鐘）、 **評論** （10-20 分鐘）和 **辯論** （兩個對立觀點，10-20 分鐘）。

## 5 個 Pipeline 工作流

以下是預建的端到端工作流：

### 1\. 研究 → 文章

輸入 URL → NotebookLM 建立筆記本 → 提出 5 個研究問題 → Claude 撰寫 1,000-2,000 字的引用文章。

### 2\. 研究 → 社群貼文

URL → 筆記本 → 摘要 → 針對 Threads、Instagram、Facebook 的平台特定貼文。

### 3\. 趨勢 → 內容

整合 [trend-pulse](https://github.com/claude-world/trend-pulse) 發現熱門趨勢，在 NotebookLM 中研究，自動生成內容。

### 4\. 批次摘要

RSS feed → 筆記本 → 分類電子報摘要與問答。

### 5\. 生成所有產出物

URL → 筆記本 → 一次生成並下載全部 9 種產出物。

## YouTube 影片合成

最令人印象深刻的功能之一： **將投影片 + Podcast 合成為 YouTube 影片** 。

內建的 `make_video.sh` 腳本：

1. 從 NotebookLM 生成投影片（PDF）和 Podcast（M4A）
2. 用 `pdftoppm` 將 PDF 轉換為 PNG 逐頁圖
3. 用 `ffmpeg` 將投影片與音訊合成
4. 輸出 H.264 影片 + AAC 音訊， `+faststart` 支援網路串流

實際效果：

- [中文示範](https://youtu.be/6M3K4sxahdE)
- [英文示範](https://youtu.be/q1kj_OccaVE)

## 認證方式：不需要 OAuth，不需要 API Key

設定過程非常簡單：

```bash
pip install -r requirements.txt
python3 -m playwright install chromium
python3 -m notebooklm login  # 開啟瀏覽器，登入一次
```

就這樣。瀏覽器 session 會儲存在本地。後續所有呼叫都是純 HTTP。不需要 Google Cloud 專案、不需要 OAuth 應用、不需要 API Key。Session 可持續數週。

## 快速開始

**作為 Claude Code Skill：**

```bash
mkdir -p .claude/skills/notebooklm
cp -r notebooklm-skill/SKILL.md .claude/skills/notebooklm/
cp -r notebooklm-skill/scripts/ .claude/skills/notebooklm/scripts/
```

然後直接告訴 Claude： *「用 NotebookLM 研究 Claude Code 最佳實踐並寫一篇文章。」*

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

**CLI 使用：**

```bash
# 從 URL 建立筆記本
python3 scripts/notebooklm_client.py create \
  --title "研究主題" \
  --urls "https://example.com/article1" "https://example.com/article2"

# 生成 Podcast
python3 scripts/notebooklm_client.py podcast --notebook "研究主題"

# 完整研究到文章管線
python3 scripts/pipeline.py research-to-article \
  --urls "https://..." --questions 5 --format markdown
```

## 與其他工具比較

| 工具 | 研究 | 產出物 | 自動化 | 費用 |
| --- | --- | --- | --- | --- |
| **notebooklm-skill** | NotebookLM（有引用） | 9 種 | 完整管線 | 免費 |
| Perplexity API | 按次搜尋 | 僅文字 | 手動 | $5-200/月 |
| LangChain/LlamaIndex | 自建 RAG | 自建 | 需自寫程式 | LLM API 費用 |
| Zapier + NotebookLM | 無 | 無 | 部分 | $20+/月 |
| NotebookLM UI | 手動 | 9 種 | 無 | 免費 |

獨特定位： **有引用的研究 + 產出物生成 + 完整自動化 + 零成本** 。

## 相關開源專案

notebooklm-skill 是我們正在建構的更大開源生態系的一部分：

- **[trend-pulse](https://github.com/claude-world/trend-pulse)** — 從 7 個免費來源（Google Trends、Hacker News、Reddit 等）即時發現熱門趨勢。為 notebooklm-skill 的 `trend-to-content` 管線提供趨勢數據。可作為 MCP Server 使用。
- **[cf-browser](https://github.com/claude-world/cf-browser)** — 透過 Cloudflare Browser Rendering 提供 9 個瀏覽器工具給 Claude Code。截圖、爬取、抓取任何網頁內容。一個指令部署完成。

這三個工具組成完整的內容管線： **發現趨勢** （trend-pulse）→ **深度研究** （notebooklm-skill）→ **擷取網頁內容** （cf-browser）→ **創作與發布** （notebooklm-skill）。

## 接下來

專案採用 MIT 授權，持續維護中。計畫新增更多 MCP 工具（ `nlm_generate` 、 `nlm_download` 、 `nlm_list_artifacts` ）、更多管線配方，以及更深入的內容管理系統整合。

**GitHub**: [claude-world/notebooklm-skill](https://github.com/claude-world/notebooklm-skill) **示範（中文）**: [youtu.be/6M3K4sxahdE](https://youtu.be/6M3K4sxahdE) **Demo（English）**: [youtu.be/q1kj\_OccaVE](https://youtu.be/q1kj_OccaVE)

如果你正在建構內容管線，受夠了按次付費的 API 呼叫，試試看吧。如果覺得好用，在 GitHub 上給顆星幫助更多人發現這個專案。光是零成本這點就值得探索——完整的自動化管線讓它成為真正的遊戲規則改變者。