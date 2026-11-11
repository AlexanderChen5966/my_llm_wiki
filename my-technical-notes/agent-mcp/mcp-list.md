---
title: Claude MCP 完整列表
date: 2026-10-21
tags: [ai/agent, ai/claude, ai/mcp]
type: reference
status: processed
source: ~/.claude.json、~/Library/Application Support/Claude/claude_desktop_config.json、各專案 .mcp.json、Claude Code plugins
---

> 範圍：Claude Code（CLI）與 Claude Desktop 目前設定中的 MCP server
> 最後更新：2026-11-10（新增 `GitLab` 官方 MCP，Code 與 Desktop 皆設定並完成 OAuth 授權，server 19.6.0-pre 實測可讀 slc-dev 專案）
> 前次更新：2026-10-21（新增 `blender`；claude.ai 連接器改版：新增 Slack、Microsoft 365，Supabase／Canva／Google 系列已不在清單；`godot-mcp-toolkit` 多一個 `失語者_2.5D` 專案）
> 初版：2026-10-08
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
| `blender` | **MCP for Blender**：驅動使用者開著的 Blender，`execute_blender_code` 以 bpy 執行任意 Python、`look` 截圖檢視成果、`get_scene_info`／`get_addon_status` 讀狀態；素材可從 Poly Haven／Sketchfab／Poly Pizza 匯入（`search_assets`→`import_asset`），或 `generate_3d` 文字／圖片生成單一模型（1–3 分鐘，可能扣點數）。需 Blender 端安裝 add-on 並啟動 | 2.2.2 | stdio：`uvx mcp-for-blender`；`DISABLE_TELEMETRY` 已設 | ✅ user | ✅ |
| `pixelorama` | 像素畫編輯器 Pixelorama 的 MCP：畫布／圖層／影格／調色盤操作、繪圖、特效、動畫、匯出（GIF、Godot SpriteFrames／TileSet、Aseprite JSON） | 0.2.1 | stdio：`node ~/Tools/pixelorama-mcp/mcp-server/dist/index.js` | ✅ user | — |
| `GitLab` | GitLab 官方 MCP Server（Beta）：專案、issue、MR、pipeline 等；OAuth 授權（Code 用 `/mcp` → Authenticate，Desktop 首次啟動開瀏覽器）。官方標示需 Premium／Ultimate＋GitLab Duo，`slc-dev` 實測可用（2026-11-10）；另有 Duo flow 工具，會耗 GitLab credits，啟動前要先問使用者 | Beta | Code：http `https://gitlab.com/api/v4/mcp`；Desktop：stdio `npx -y mcp-remote https://gitlab.com/api/v4/mcp` | ✅ user | ✅ |
| `godot` | Godot 引擎操作（`@coding-solo/godot-mcp`） | latest | stdio：`npx -y @coding-solo/godot-mcp`；需 `GODOT_PATH` | ✅ project（`~`） | — |
| `godot-mcp-toolkit` | Godot 引擎操作（`@npgamedev/godot-mcp-server`），**失語者**遊戲專案用；工具呼叫次數最多的 MCP | latest | stdio：`npx -y @npgamedev/godot-mcp-server` | ✅ project（`失語者_Godot/`、`失語者_2.5D/`） | — |

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
| `Atlassian MCP` | Jira（查詢／建立／編輯 issue、轉狀態、留言）、Confluence（頁面、CQL 搜尋）、Loom、Teamwork Graph；改為三層架構：主要工具直接呼叫，其餘先 `discover` 再依風險用 `executeRead`／`executeWrite`／`executeDestructive`（舊名 `Atlassian Rovo`） | ✅ 已授權 | ✅ | ✅ |
| `Slack` | Slack 工作區 | ⏸ 未授權 | ✅ | ✅ |
| `Microsoft 365` | Outlook／Teams／OneDrive 等 | ⏸ 未授權 | ✅ | ✅ |

> 2026-10-08 盤點時還在、本次已不在清單：`Supabase`（原已授權）、`Canva`、`Gmail`、`Google Calendar`、`Google Drive`（原皆未授權）。若仍需要，到 claude.ai 連接器重新加入

---

## 內建整合

| 名稱 | 描述 | 狀態 | Code | Desktop |
|------|------|------|------|---------|
| `claude-in-chrome` | 透過 Chrome 擴充功能操作使用者自己的 Chrome（沿用登入狀態）：點擊、填表、截圖、讀 console／network、錄 GIF；先呼叫 `tabs_context_mcp`，避免觸發 alert 對話框 | ✅ 使用中 | ✅ | — |
| `Filesystem`（Desktop 擴充） | Anthropic 官方檔案系統擴充：讀寫、編輯、搜尋、目錄樹 | ⏸ 已安裝但**停用**（`isEnabled: false`） | — | ⏸ |

---

## Claude Code 使用次數（對話紀錄統計）

> 2026-10-21 重算：以 `~/.claude/projects/` **目前保留**的對話紀錄（含 subagent）計 `tool_use`，依 id 去重。舊對話會被自動清理，因此與 10-08 版數字不可直接相比（例如 Claude Docs 從 262 降到 4，是紀錄被清掉而非少用）

| MCP | 工具呼叫次數 |
|-----|------------|
| godot-mcp-toolkit | 4210 |
| pixelorama | 975 |
| claude-in-chrome | 306 |
| blender | 48 |
| Atlassian（`Atlassian_MCP` 37＋舊名 `Atlassian_Rovo` 12） | 49 |
| godot | 32 |
| playwright | 24 |
| figma | 8 |
| Claude Docs | 4 |

> media-downloader、evaluate、telegram 在保留的紀錄中沒有呼叫（Desktop 使用紀錄未統計）

---

## 設定檔位置

| 位置 | 內容 |
|------|------|
| `~/.claude.json` → `mcpServers` | Claude Code user 範圍（media-downloader、evaluate、pixelorama、playwright、blender、GitLab） |
| `~/.claude.json` → `projects.<路徑>.mcpServers` | Claude Code project 範圍（`~` 下的 godot） |
| `<專案>/.mcp.json` | 專案共用設定（DownloadVideoPythonProject、失語者_Godot、失語者_2.5D） |
| `~/.claude/settings.json` → `enabledPlugins` | Plugin 開關（figma、telegram 帶 MCP；gopls／rust-analyzer 為 LSP） |
| `~/Library/Application Support/Claude/claude_desktop_config.json` | Claude Desktop（GitLab、evaluate、kyoto-assistant、media-downloader、blender） |
| `~/Library/Application Support/Claude/Claude Extensions/` | Desktop 擴充（Filesystem） |

---

## 統計

| 分類 | 數量 |
|------|------|
| 自建 | 2 |
| 第三方本機套件 | 7 |
| Plugin 帶入 | 2 |
| claude.ai 連接器 | 4（已授權 2、未授權 2） |
| 內建整合 | 2（Filesystem 停用） |
| **總計** | **17** |
| 兩邊都有設定 | evaluate、media-downloader、blender、GitLab（＋所有 claude.ai 連接器） |
