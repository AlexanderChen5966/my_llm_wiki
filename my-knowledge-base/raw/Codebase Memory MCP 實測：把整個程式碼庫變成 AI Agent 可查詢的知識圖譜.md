---
title: "Codebase Memory MCP 實測：把整個程式碼庫變成 AI Agent 可查詢的知識圖譜"
source: "https://blog.shuochen.me/articles/codebase-memory/"
author:
  - "[[Shuo Chen]]"
published: 2026-06-29
created: 2026-07-09
description: "Codebase Memory MCP 是一個本機運行的程式碼知識圖譜引擎，能讓 Claude Code、Codex、Cursor 等 AI Agent 直接查詢函式、呼叫鏈、架構與影響範圍。本文實測安裝、索引、3D Graph UI 與 CLI 查詢流程。"
tags:
  - "clippings"
status: processed
---
## 前言

如果你有認真用過 AI Coding Agent，應該很快就會遇到一個問題：

AI 很會寫程式，但它不一定真的「知道」你的 codebase 長什麼樣子。

它可以用 `grep` 找字串，可以讀幾個檔案，也可以靠你貼脈絡給它猜。但當專案變大之後，這種探索方式很容易變成「一邊猜、一邊翻、一邊漏」。

今天要介紹的 [Codebase Memory MCP](https://github.com/DeusData/codebase-memory-mcp) ，就是專門解決這件事的工具。

它會在本機把整個 repository 解析成 knowledge graph，包含 functions、classes、imports、call chains、HTTP routes、cross-service links 等資訊。然後透過 MCP 提供給 Claude Code、Codex、Cursor、Gemini CLI、VS Code 等 Agent 使用。

換句話說，它不是讓 AI 再多讀幾個檔案，而是讓 AI 可以直接查「這個函式誰呼叫」、「這段改動會影響哪裡」、「整個專案有哪些模組」、「這個 route 對應到哪段程式碼」。

我自己實測下來，最有感的是：它把 codebase discovery 從翻檔案，變成查圖譜。

---

## 實測畫面

這次我直接拿自己的部落格專案做測試。索引完成後，Codebase Memory MCP 把整個 repo 轉成可以互動的 3D knowledge graph。影片裡可以看到我直接拖動畫面、縮放圖譜，並用節點之間的連線快速觀察程式碼關聯。

索引結果如下：

- 掃描檔案：326 個
- Graph nodes：3995 個
- Graph edges：8243 條
- UI 位址： `http://localhost:9749`

這不是單純畫一張漂亮的圖而已。背後的 nodes/edges 真的可以被 MCP 工具查詢，包含架構概覽、函式呼叫路徑、程式碼片段、語意搜尋與 Cypher-like query。

---

## Codebase Memory MCP 是什麼？

官方對它的定位是「給 AI Coding Agent 用的 code intelligence engine」。

它的核心流程大概是這樣：

1. 用 tree-sitter 解析專案裡的程式碼
2. 把 functions、classes、modules、routes、imports 等結構變成 graph
3. 用 SQLite 在本機儲存索引
4. 透過 MCP tools 讓 Agent 查詢這些結構
5. 如果安裝 UI 版本，還能在瀏覽器看到 3D graph visualization

官方 README 提到它支援 158 種語言，並針對 Python、TypeScript、JavaScript、PHP、C#、Go、C/C++、Java、Kotlin、Rust 等語言加入 Hybrid LSP semantic type resolution。

它也不是雲端服務，不需要 API key，不需要 Docker。整個過程都在本機跑，資料會存在 `~/.cache/codebase-memory-mcp/` 。

這點對公司內部專案或私有 repo 很重要，因為你的 codebase 不需要上傳到第三方服務。

---

## 它和 grep 最大的不同

一般 Agent 找 code，大多是這樣：

```bash
grep -R "ProcessOrder" .
```

找到幾個字串後，再一個一個檔案讀，接著推測呼叫關係。

Codebase Memory MCP 的思路不同。它先把 codebase 建成結構化圖譜，所以 Agent 可以直接問：

```bash
codebase-memory-mcp cli trace_path '{"function_name":"ProcessOrder","direction":"inbound"}'
```

這代表它查的不是「哪裡出現這個字」，而是「誰真的呼叫它」。

這個差異在大專案裡非常明顯。因為字串搜尋很容易找到註解、測試資料、文件、同名變數，但 graph query 可以回到程式結構本身。

---

## 安裝方式

macOS 或 Linux 可以直接使用官方安裝腳本：

```bash
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash
```

如果你想要 3D Graph UI，建議直接安裝 UI 版本：

```bash
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash -s -- --ui
```

我這次實測安裝後，它會自動偵測目前機器上有哪些 coding agent，並寫入對應設定。

我這邊偵測到的包含：

- Claude Code
- Codex CLI
- Gemini CLI
- OpenCode
- VS Code
- Cursor
- Kiro

安裝完成後可以檢查版本：

```bash
codebase-memory-mcp --version
```

---

## 啟動 3D Graph UI

如果你安裝的是 UI variant，可以用以下指令啟動：

```bash
codebase-memory-mcp --ui=true --port=9749
```

然後打開：

```
http://localhost:9749
```

UI 會先顯示目前已索引的 projects。

Projects 頁面會列出已索引的 repository、節點數與邊數

點擊 `View Graph` 後，就能進入 3D graph 畫面。

![Codebase Memory MCP 3D graph visualization](https://blog.shuochen.me/day_assets/codebase-memory/view.jpg)

3D Graph UI 可以用來觀察整個 codebase 的模組、檔案與呼叫關係

這個畫面很適合拿來快速理解大型專案的形狀。你可以旋轉、縮放、點擊節點，看某個 module 或 function 在整個系統裡的位置。

---

## 索引目前專案

如果你想手動索引目前 repo，可以使用 CLI：

```bash
codebase-memory-mcp cli index_repository '{"repo_path":"/path/to/your/repo"}'
```

我在自己的 `tech-blog` repo 實測，輸出結果是：

```json
{
  "project": "Volumes-ssd-1-tech-blog",
  "status": "indexed",
  "nodes": 3995,
  "edges": 8243
}
```

速度也很快。這個 repo 不是巨型專案，但裡面有 Nuxt、Vue、TypeScript、Python scripts、Markdown content、skills 等混合內容，索引起來仍然非常順。

---

## 實際查詢：搜尋 code graph

索引完成後，就可以直接查 graph。

例如我想找 repo 裡跟 `twitter webhook blog` 相關的程式結構：

```bash
codebase-memory-mcp cli search_graph '{"project":"Volumes-ssd-1-tech-blog","query":"twitter webhook blog","limit":8}'
```

它回傳的不是一般 grep 結果，而是已經被結構化的 symbol。

我這次查到的結果包含：

- `_expand_twitter_discussion`
- `TwitterScraper.fetch`
- `TwitterPlaywrightScraper.fetch`
- `test_twitter._tweet`

這種結果對 Agent 很好用。它不需要先讀一堆檔案，就能知道哪些 symbols 可能跟任務相關。

---

## 實際查詢：追蹤函式呼叫關係

更有感的是 `trace_path` 。

例如我想知道 `_expand_twitter_discussion` 這個 method 的上下游呼叫關係：

```bash
codebase-memory-mcp cli trace_path '{"project":"Volumes-ssd-1-tech-blog","function_name":"_expand_twitter_discussion","direction":"both","depth":2,"mode":"calls","include_tests":false}'
```

它可以直接回傳 callers 和 callees。

![Codebase Memory MCP trace_path 終端機查詢結果](https://blog.shuochen.me/day_assets/codebase-memory/search.jpg)

在終端機執行 trace\_path 後，可以直接看到 callers、callees 與 hop 深度

在我的實測裡，它找到：

**誰呼叫它：**

- `HorizonOrchestrator.run`
- `src.mcp.server.main`
- `src.services.webhook_cli.main`
- `src.main.main`

**它呼叫誰：**

- `TwitterScraper.fetch_replies_for_item`
- `TwitterScraper.append_discussion_content`
- `ContentAnalyzer.analyze_batch`
- `TwitterScraper._fetch_dataset`
- `ContentAnalyzer._get_concurrency`

這就是我覺得它真正有魅力的地方。

很多時候我們不是不知道要搜尋什麼，而是不知道某段 code 在整個系統裡的上下游影響。Codebase Memory MCP 直接把這件事變成一個 graph query。

---

## 常用 MCP Tools

官方目前提供的工具包含：

- `index_repository` ：索引 repository
- `search_graph` ：搜尋 graph 裡的 function、class、route、variable
- `query_graph` ：執行 Cypher-like query
- `trace_path` ：追蹤呼叫、資料流或跨服務路徑
- `get_code_snippet` ：用 qualified name 讀取原始碼片段
- `get_graph_schema` ：查看 graph schema
- `get_architecture` ：產生架構概覽
- `search_code` ：graph-augmented code search
- `detect_changes` ：偵測 git diff 的影響範圍
- `manage_adr` ：管理 Architecture Decision Records

對我來說最常用的會是這幾個：

```bash
codebase-memory-mcp cli get_architecture '{"project":"your-project","aspects":["all"]}'
```

```bash
codebase-memory-mcp cli search_graph '{"project":"your-project","query":"auth login token","limit":20}'
```

```bash
codebase-memory-mcp cli trace_path '{"project":"your-project","function_name":"handleLogin","direction":"both","depth":3}'
```

這些查詢都很適合交給 AI Agent 自己跑。你只要問「幫我找登入流程的入口」、「這個函式被誰呼叫」、「改這裡會影響哪裡」，Agent 就可以用這些 tools 找答案。

---

## 團隊共享 Graph Artifact

Codebase Memory MCP 還有一個很實用的設計：可以把 graph artifact 放進 repo。

它會產生：

```
.codebase-memory/graph.db.zst
```

這是一個壓縮後的 SQLite graph snapshot。

如果團隊願意把它 commit 進 repo，其他人 clone 下來後就不用每次都完整重建索引，可以直接從 artifact bootstrap，再針對本機 diff 做增量更新。

這點對大型 monorepo 會很有幫助，因為它把「每個人都重新掃一次」變成「共享一份 graph snapshot」。

---

## 適合誰使用？

我覺得 Codebase Memory MCP 特別適合這幾種情境：

1. 你常用 Claude Code、Codex、Cursor 這類 AI Coding Agent
2. 你的專案已經大到 grep 不夠直覺
3. 你需要快速理解陌生 codebase
4. 你常常要追函式呼叫鏈或影響範圍
5. 你在維護多語言、多服務或 monorepo
6. 你希望 code intelligence 在本機完成，不想上傳私有程式碼

如果你只是寫很小的單檔腳本，它可能有點殺雞用牛刀。

但只要專案開始有多個模組、測試、API route、背景任務或 shared package，它的價值就會變得很明顯。

---

## 我實測後的感想

我一開始以為它只是「幫 Agent 多一個搜尋工具」。

實際跑完之後，我覺得它更像是幫 AI Agent 裝了一份本機 codebase memory。

過去 AI 要理解專案，常常會先搜尋、讀檔、再搜尋、再讀檔。這個過程不只耗 token，也很容易漏掉真正重要的結構關係。

Codebase Memory MCP 的做法是先把 codebase 轉成圖，再讓 Agent 查圖。這讓 Agent 的探索方式更接近資深工程師：先看架構，再追依賴，再讀關鍵片段。

尤其搭配 3D Graph UI 後，它很適合拿來做 onboarding。新成員進 repo 時，不用從資料夾樹慢慢猜，可以直接先看整體 graph，再點進關鍵 node。

---

## 總結

Codebase Memory MCP 是我近期覺得很值得裝給 AI Coding Agent 的工具。

它不是取代 Agent，而是幫 Agent 補上一層本機 code intelligence。

它能把 codebase 變成可查詢的 knowledge graph，支援 3D visualization、CLI 查詢、MCP tools、call graph、architecture overview、impact analysis，而且整個流程都可以在本機完成。

如果你已經在用 Claude Code、Codex、Cursor 或其他支援 MCP 的 coding agent，我會建議至少裝一次 UI variant 試試看。

真的看到自己的 repo 被轉成一張可以查詢、可以旋轉、可以追呼叫關係的圖之後，你會很快理解它的價值。

**相關連結：**