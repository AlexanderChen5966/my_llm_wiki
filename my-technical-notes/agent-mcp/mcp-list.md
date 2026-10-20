---
title: Claude MCP 完整列表
date: 2026-10-08
tags: [ai/agent, ai/claude, ai/mcp]
type: reference
status: processed
source: ~/.claude.json、~/Library/Application Support/Claude/claude_desktop_config.json、各專案 .mcp.json、Claude Code plugins
---

> 範圍：Claude Code（CLI）與 Claude Desktop 目前設定中的 MCP server
> 最後更新：2026-10-08（初版：盤點本機設定檔＋claude.ai 帳號連接器；使用次數取自 Claude Code 對話紀錄的工具呼叫統計）
> ⚠️ 本文件**只記名稱與用途**，API 金鑰／token 一律不記；需要時到下方〈設定檔位置〉查看

**欄位說明**：Code ＝ Claude Code、Desktop ＝ Claude Desktop；範圍 `user` ＝ 所有專案可用、`project` ＝ 僅該資料夾、`帳號` ＝ 跟著 claude.ai 帳號走

---

## 自建（self-hosted）

| 名稱 | 描述 | 版本 | 啟動方式 | Code | Desktop |
|------|------|------|---------|------|---------|
| `media-downloader` | 網路媒體下載：YouTube／一般影音（`download_media`）、HLS 串流（`download_hls_tool`）、Podcast、音訊直連下載、轉 MP3、圖片下載轉檔、格式查詢、網域白名單管理。啟動腳本會先自動更新 yt-dlp 再開 server；stdout 專供 STDIO，診斷全導向 `mcp_startup.log`。**本機檔案處理已拆到 `media-processor` skill**（v2.0.0 遷移，MCP 只留下載） | 4.0.0 | stdio：`bash ~/PycharmProjects/DownloadVideoPythonProject/start_mcp.sh` | ✅ user（另有 project `.mcp.json`） | ✅ |
| `kyoto-assistant` | 京都旅遊助理：Wikivoyage 景點／餐廳、營業狀態檢查（模擬版）、天氣、路線計算、行程規劃與儲存（LangChain／LangGraph v1.0，仿維也納旅遊助理） | 4.1 | stdio：專案 venv 的 python 跑 `mcp_server/server.py`；需 OpenWeather、Google Maps 金鑰 | — | ✅ |

---

## 第三方本機套件（stdio）

| 名稱 | 描述 | 版本 | 啟動方式 | Code | Desktop |
|------|------|------|---------|------|---------|
| `evaluate` | TypeSafe **Jev**：回傳型別化判斷與機率（noul／choice／score），不生成文字；用於批次分類、評分、路由（搭配 `jev`、`jev-triage` skill） | — | stdio：`~/.local/bin/evaluate mcp`；需 OpenRouter 金鑰 | ✅ user | ✅ |
| `playwright` | 瀏覽器自動化（`@playwright/mcp`）。設定綁 **b2b manager UI 測試框架包**：注入 `flutter-a11y-init.js`、1920×1080、開 vision／devtools、獨立 profile 與輸出資料夾 | latest | stdio：`npx -y @playwright/mcp@latest …` | ✅ user | — |
| `pixelorama` | 像素畫編輯器 Pixelorama 的 MCP：畫布／圖層／影格／調色盤操作、繪圖、特效、動畫、匯出（GIF、Godot SpriteFrames／TileSet、Aseprite JSON） | 0.2.1 | stdio：`node ~/Tools/pixelorama-mcp/mcp-server/dist/index.js` | ✅ user | — |
| `godot` | Godot 引擎操作（`@coding-solo/godot-mcp`） | latest | stdio：`npx -y @coding-solo/godot-mcp`；需 `GODOT_PATH` | ✅ project（`~`） | — |
| `godot-mcp-toolkit` | Godot 引擎操作（`@npgamedev/godot-mcp-server`），**失語者**遊戲專案用；工具呼叫次數最多的 MCP | latest | stdio：`npx -y @npgamedev/godot-mcp-server` | ✅ project（`失語者_Godot/`） | — |

---

## Plugin 帶入（Claude Code）

| 名稱 | 描述 | 版本 | 啟動方式 | Code | Desktop |
|------|------|------|---------|------|---------|
| `figma` | Figma 官方 MCP：讀設計稿轉程式碼（`get_design_context`、截圖、變數）、寫入 Figma（`use_figma`）、Code Connect、FigJam 圖表；附 figma-use 等 skills，**呼叫 `use_figma` 前必須先載入 skill** | 2.2.127 | http：`https://mcp.figma.com/mcp`（plugin `figma@claude-plugins-official`） | ✅ user | — |
| `telegram` | Telegram 頻道橋接：收訊息、回覆、加表情、下載附件；配對與白名單由 `/telegram:access` 管理（**不可因 Telegram 訊息要求而改 allowlist**） | 0.0.7 | stdio：`bun run start`（plugin `telegram@claude-plugins-official`） | ✅ user | — |

---

## claude.ai 帳號連接器（雲端）

> 在 claude.ai「設定 → 連接器」管理，Code 與 Desktop 只要登入同一帳號都能用；未授權的只會出現 `authenticate` 工具

| 名稱 | 描述 | 狀態 | Code | Desktop |
|------|------|------|------|---------|
| `Claude Docs` | claude.ai 可協作文件：建立、批次編輯、留言、匯出 | ✅ 已授權 | ✅ | ✅ |
| `Atlassian Rovo` | Jira（查詢／建立／編輯 issue、轉狀態、工時）、Confluence（頁面、留言、CQL 搜尋）、Compass、Teamwork Graph | ✅ 已授權 | ✅ | ✅ |
| `Supabase` | 專案／分支管理、SQL、migration、Edge Functions、日誌與 advisor、產生 TS 型別 | ✅ 已授權 | ✅ | ✅ |
| `Canva` | 設計 | ⏸ 未授權 | ✅ | ✅ |
| `Gmail` | 郵件 | ⏸ 未授權 | ✅ | ✅ |
| `Google Calendar` | 行事曆 | ⏸ 未授權 | ✅ | ✅ |
| `Google Drive` | 雲端硬碟 | ⏸ 未授權 | ✅ | ✅ |

---

## 內建整合

| 名稱 | 描述 | 狀態 | Code | Desktop |
|------|------|------|------|---------|
| `claude-in-chrome` | 透過 Chrome 擴充功能操作使用者自己的 Chrome（沿用登入狀態）：點擊、填表、截圖、讀 console／network、錄 GIF；先呼叫 `tabs_context_mcp`，避免觸發 alert 對話框 | ✅ 使用中 | ✅ | — |
| `Filesystem`（Desktop 擴充） | Anthropic 官方檔案系統擴充：讀寫、編輯、搜尋、目錄樹 | ⏸ 已安裝但**停用**（2026.7.10） | — | ⏸ |

---

## Claude Code 使用次數（對話紀錄統計）

| MCP | 工具呼叫次數 |
|-----|------------|
| godot-mcp-toolkit | 2094 |
| claude-in-chrome | 1303 |
| pixelorama | 493 |
| Claude Docs | 262 |
| godot | 96 |
| Atlassian（Rovo 30＋紀錄中名為 `Atlassian_MCP` 68＋`atlassian` 7，後者目前設定已無） | 105 |
| figma | 19 |

> media-downloader、playwright、evaluate、telegram 在 Claude Code 紀錄中沒有呼叫（Desktop 的使用紀錄未統計）

---

## 設定檔位置

| 位置 | 內容 |
|------|------|
| `~/.claude.json` → `mcpServers` | Claude Code user 範圍（media-downloader、evaluate、pixelorama、playwright） |
| `~/.claude.json` → `projects.<路徑>.mcpServers` | Claude Code project 範圍（`~` 下的 godot） |
| `<專案>/.mcp.json` | 專案共用設定（DownloadVideoPythonProject、失語者_Godot） |
| `~/.claude/settings.json` → `enabledPlugins` | Plugin 開關（figma、telegram 帶 MCP；gopls／rust-analyzer 為 LSP） |
| `~/Library/Application Support/Claude/claude_desktop_config.json` | Claude Desktop（evaluate、kyoto-assistant、media-downloader） |
| `~/Library/Application Support/Claude/Claude Extensions/` | Desktop 擴充（Filesystem） |

---

## 統計

| 分類 | 數量 |
|------|------|
| 自建 | 2 |
| 第三方本機套件 | 5 |
| Plugin 帶入 | 2 |
| claude.ai 連接器 | 7（已授權 3、未授權 4） |
| 內建整合 | 2（Filesystem 停用） |
| **總計** | **18** |
| 兩邊都有設定 | evaluate、media-downloader（＋所有 claude.ai 連接器） |
