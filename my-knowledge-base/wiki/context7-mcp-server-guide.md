---
title: Context7 MCP Server：用「use context7」讓 AI 讀到最新版官方文件
date: 2026-08-24
tags: [ai/mcp, ai/claude, type/how-to]
source: https://codelove.tw/@tony/post/3rPw7q
type: article
status: processed
asset_value: 60
---

## T（核心論點）

AI 編碼助理最常見的錯不是不會寫，而是「只知道訓練當時的 API 版本」——Context7 MCP Server 把 15,000+ 函式庫的官方文件即時餵給模型，導入約 5 分鐘，使用成本只是在提示詞尾端加一句「use context7」。

## B（適用場景）

**適用：**
- 使用訓練資料可能已過期的新版框架：Next.js 15、React 19、TypeScript 5.5 等
- 常被舊版 API 寫法坑到、得自己回頭翻官方文件重查的情境
- 需要指定特定版本的程式碼範例
- 想在團隊層級統一「AI 寫出來的是最新最佳實務」的基準

**不適用 / 注意：**
- 涵蓋範圍限於它收錄的 15,000+ 函式庫官方文件——冷門或內部函式庫仍無解
- 觸發是**手動**的：沒加「use context7」就不會自動去查，需要養成習慣或寫進 CLAUDE.md
- 它解的是「文件版本正確性」，不解「模型理解錯誤」——生成結果仍要驗證
- 每次觸發會把文件拉進 context，長對話中要留意 context 佔用

## R（操作步驟）

1. **Claude Code**（一行完成）：
   ```bash
   claude mcp add context7 -- npx -y @upstash/context7-mcp
   ```
   若上述失敗，改用 HTTP transport：
   ```bash
   claude mcp add --transport http context7 https://mcp.context7.com/mcp
   ```
2. **Cursor**：設定畫面 →「Cursor 設定」→「MCP」→「新增新的全域 MCP 伺服器」，或直接編輯 `~/.cursor/mcp.json`：
   ```json
   {
     "mcpServers": {
       "context7": {
         "command": "npx",
         "args": ["-y", "@upstash/context7-mcp@latest"]
       }
     }
   }
   ```
3. **Claude Desktop**：在 `claude_desktop_config.json` 加入相同的 `mcpServers` 設定（鍵名可用 `Context7`）。
4. 使用：在一般提示詞**結尾**加上 `use context7`。例如：
   - 「請使用 app 路由器建立一個基本的 Next.js 專案。use context7」
   - 「請使用 React 19 的新 use() Hook，實作非同步資料擷取。use context7」
   - 「請使用 TypeScript 5.5 的新功能，建立型別安全的 API 用戶端。use context7」
   - 「請使用 PostgreSQL 的認證資訊，建立一個刪除 city 為空白的資料列的腳本。use context7」
5. 驗證有效：比對生成的程式碼是否用了目標版本的新 API（例如 React 19 的 `use()` 而非舊寫法）；沒有差異多半是忘了加觸發語。

## C（關鍵行動）

- [ ] 「use context7」是手動觸發語——忘記加就等於沒裝，建議寫進 CLAUDE.md 或個人常用 prompt 樣板
- [ ] 安裝失敗時先切換 transport（npx stdio ↔ HTTP `https://mcp.context7.com/mcp`），這是最常見的分岔
- [ ] 它保證的是文件新鮮度，不是程式正確性——產出仍需自行驗證

## 原始摘要

Context7 由 **Upstash** 開發，是完全免費的開源 MCP 伺服器，支援**超過 15,000 個函式庫**的官方文件。定位比喻：把「只有舊參考書的圖書館」換成「永遠備有最新版本的圖書館」，而且圖書館員（AI）會自動挑最新的書。

**解決的問題**：傳統 AI 編碼助理只知道訓練當時的資訊——Next.js 15 已推出仍用舊版知識回答、不知道 TypeScript 5.5 新功能而提出舊寫法，最後使用者還是得自己回查官方文件。

**宣稱效益**：錯誤變少、除錯時間縮短、能寫出符合最新最佳實務的程式碼。導入約 5 分鐘。

**三種安裝路徑**：Claude Code 一行 `claude mcp add context7 -- npx -y @upstash/context7-mcp`（備援為 `--transport http https://mcp.context7.com/mcp`）；Cursor 走設定畫面或 `~/.cursor/mcp.json`；Claude Desktop 走 `claude_desktop_config.json`。三者共用的設定內容是 `npx -y @upstash/context7-mcp@latest`。

原文出處：Qiita ともだ（https://qiita.com/tomada/items/42537e0b14b38797d218），由 codelove.tw 翻譯轉載；作者另著《用 Claude Code 做中學：AI 驅動應用程式開發入門》（技術評論社，2026-09-08 上市），第 7 章涵蓋 MCP 運作原理與 Context7、Playwright MCP、Supabase MCP 的活用。

## 相關連結

- [[mcp-fastmcp-server]]
- [[agent-skills-vs-mcp]]
- [[claude-code-tools]]
- [[twinkle-hub-taiwan-mcp]]
