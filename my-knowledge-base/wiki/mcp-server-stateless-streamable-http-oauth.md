---
title: MCP 伺服器實作：無狀態 Streamable HTTP、中介層順序與 OAuth 探測
date: 2026-09-23
tags: [ai/mcp, dev/architecture, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10411041; https://ithelp.ithome.com.tw/articles/10413861; 程式碼 https://github.com/wikibrain-app/wikibrain（AGPL-3.0）
type: article
status: processed
asset_value: 64
---

WikiBrain 系列 Day 06（主體）與 Day 10（選型補充），作者為 iT 邦幫忙使用者 20184129（WikiBrain 作者）。WikiBrain 用一台 remote MCP 伺服器讓 Cursor、Claude Code、Claude.ai、ChatGPT 四個 client 讀寫同一座知識庫。本條目收的是這台伺服器的**可直接套用的實作細節**：Express 路由、三段中介層、無狀態 transport、401 探測標頭、兩條接入路線。

## T（核心論點）

**MCP 是 client 發起的拉取模型，伺服器叫不動 client**——所以網頁上「幫我編纂」這類主動觸發的按鈕，必須另開一條伺服器端 agent 的路（使用者自備 API key），和 MCP 共用同一層資料存取。transport 設成無狀態，每個請求新建 server 實例，換來零負擔的水平擴展；代價只是不能做伺服器推送。**401 回應少了 `WWW-Authenticate` 的 `resource_metadata`，Claude.ai 只會顯示「連線失敗」，沒有下文。**

> ⚠️ **別和協定層的 stateless 混為一談**：本篇的「無狀態」是 **SDK transport 的設定**（`sessionIdGenerator: undefined`，伺服器不發 session id、不在記憶體保存連線），屬於在既有協定內的實作選擇；2026-07-28 RC 則是**協定規格本身**拿掉 `initialize`／`initialized`／`Mcp-Session-Id`（見 [[mcp-stateless-architecture]]、[[mcp-stateless-migration-state-ownership]]）。前者是「我這台 server 選擇不記」，後者是「規格不再要求任何 server 記」，兩者層級不同。

## B（適用場景）

**適用：**
- 自建 remote MCP server，要同時接 CLI 類 client（Cursor／Claude Code，貼 token）與網頁類 client（Claude.ai／ChatGPT，走 OAuth）
- 工具以讀寫資料為主、不需要伺服器主動推送通知的場景（知識庫、CRUD 類工具）
- 既有 Node/Express 服務想直接掛一支 `/mcp` 路由，不另開服務

**不適用 / 注意：**
- 需要伺服器推送（progress 通知、訂閱）的工具：無狀態 transport 做不到
- 只在本機用 stdio 的 server：本篇的認證、限流、OAuth 都用不到，參考 [[self-built-mcp-server-237-lines]]（Python SDK 版本）即可
- 接到活 IDE 的有狀態 bridge 是另一種東西，見 [[xcode-mcp-hidden-state-and-authorization]]

## R（操作步驟）

1. **一支路由就是一台 MCP 伺服器**：Streamable HTTP 就是普通 HTTP，直接 `app.post('/mcp', byIp, bearerAuth, byToken, handler)` 掛在既有 Express app，不用 WebSocket。handler 內：
   ```ts
   const server = createMcpServer(auth);
   const transport = new StreamableHTTPServerTransport({ sessionIdGenerator: undefined });
   res.on('close', () => { transport.close(); server.close(); });
   await server.connect(transport);
   await transport.handleRequest(req, res, req.body);
   ```
2. **`res.on('close')` 一定要收**：每個請求都會生出一組 server＋transport，client 中途斷線是常態，不清掉就是穩定的記憶體洩漏。
3. **中介層順序有理由**：`byIp`（認證前依來源 IP 限流，擋匿名洪水）→ `bearerAuth`（驗身分）→ `byToken`（認證後依 token 限流，擋單一使用者跑太兇）。兩種限流要分開算。
4. **GET／DELETE 先認證再回 405**：`app.get('/mcp', bearerAuth, methodNotAllowed)`。先驗證再回「方法不允許」，沒 token 的人就無法從回應差異推測路徑存在。
5. **錯誤一律回 JSON-RPC 格式**：未預期例外回 `500` ＋ `{ code: -32603, message: 'Internal server error' }`（`!res.headersSent` 才寫）；未授權回 `401` ＋ `{ code: -32001, message: 'Unauthorized' }`。
6. **401 必須帶探測標頭**：`WWW-Authenticate: Bearer realm="wikibrain", resource_metadata="<APP_URL>/.well-known/oauth-protected-resource/mcp"`。支援 OAuth 的 client 收到 401 會去讀這個網址、發現可動態註冊、自己跑完授權。使用者從頭到尾只貼了一個網址。
7. **token 存放**：資料庫只存雜湊，明文只在產生時顯示一次；查詢時檢查 `revoked_at IS NULL` 與到期；`last_used_at` 最多一分鐘寫一次、而且不擋請求（那只是給使用者看「token 還活著嗎」，不值得多一次同步寫入）。
8. **兩條接入路線**：Cursor／Claude Code 用 Bearer token 貼進 `mcp.json`；Claude.ai／ChatGPT 網頁版走 OAuth 2.1＋動態用戶端註冊。OAuth 存取 token 24 小時到期、client 自動續期 90 天。第三條路（只用瀏覽器或手機）由伺服器端 agent 拿使用者的 API key 跑。
9. **工具註冊**：`server.registerTool(name, { title, description, inputSchema }, handler)`，`inputSchema` 用 zod，SDK 自動轉 JSON Schema。`.describe()`、`min/max/default` 同時是驗證規則與給模型的提示；`title` 是 Claude.ai 連接器介面顯示給使用者看的名稱，使用者**逐個工具**選「允許／每次詢問／拒絕」，名字取得好不好直接影響他敢不敢按允許。描述怎麼寫見 [[tool-description-and-error-message-as-prompt]]。
10. **上線前三個 curl 驗證**：
    ```bash
    curl -i -X POST https://你的網域/mcp          # 無 token → 401，且要有 WWW-Authenticate
    curl -i https://你的網域/mcp                  # GET → 405
    curl -s -X POST https://你的網域/mcp \
      -H 'authorization: Bearer <token>' -H 'content-type: application/json' \
      -H 'accept: application/json, text/event-stream' \
      -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'   # 列出全部工具
    ```

### 補充：為什麼是 Vite SPA＋單一 Express，而不是 Next.js（Day 10）

- **(a) 同源根路徑是硬需求**：`/mcp`、`/authorize`、`/token`、`/register`、`/revoke`，以及 `/.well-known/oauth-authorization-server`、`/.well-known/oauth-protected-resource/mcp` 必須和前端同源、掛在根路徑。這是作者不用 Next.js、改用單一 `createApp()` 同時掛靜態檔與 API 的理由一。
- **(b) 判斷線**：公開內容是產品主體就選框架；登入後的後台是主體，就選 SPA，再自己補建置期預渲染（WikiBrain 只預渲染十幾頁公開頁）。`*splat` SPA fallback 必須先排除 `/api/` 與 `/mcp`，否則打錯的 API 會拿到 HTML 而不是 404。
- **(c) dev 心得**：建置期寫進產物的值（canonical、版本號、feature flag 預設值）要當**建置參數**管理——作者踩過建置時沒有 `APP_URL`、所有 canonical 被寫成 example.com 的坑，修法是 Dockerfile `ARG APP_URL`，代價是換網域就得重建映像。Flutter 的 `--dart-define` 屬於同一類。

SEO 細節（meta、hreflang、JSON-LD、robots）不收，見 [[aeo-implementation-llms-txt]]（llms.txt）與 [[vite-frontend-build-tool]]。

## C（關鍵行動）

- [ ] 自建 remote MCP server 時，把「401 帶 `resource_metadata`」列為 Claude.ai 接不上時的第一個檢查點
- [ ] 檢查每請求建立物件的 handler 是否都有 `res.on('close')` 清理
- [ ] 限流分認證前（IP）與認證後（token）兩段，順序照 byIp → auth → byToken
- [ ] Flutter 專案盤點哪些 `--dart-define` 值實際被寫進產物，換環境時是否需要重建

## 原始摘要

Day 06：選 MCP 是為了「實作一次、四個 client 共用同一座知識庫」；因為 MCP 是拉取模型，網頁按鈕另開伺服器端 agent 的路，兩條路共用同名同介面的六個工具與同一層資料存取（這部分設計已收在 [[wikibrain-llm-wiki-implementation-patterns]] R 第 6 點，本條目不重寫）。其餘篇幅是 `/mcp` 路由、transport、中介層、401 標頭、token 存放、兩種接法與 curl 驗證。

Day 10：捨 Next.js 改用 Vite SPA＋單一 Express，主因是 MCP／OAuth 端點必須與前端同源掛在根路徑、只想維護一個服務、產品主體是登入後的後台；SEO 用建置期預渲染十幾頁公開頁補回。

## 相關連結

- [[wikibrain-llm-wiki-implementation-patterns]] — 同系列 Day03+04；「伺服器端 agent 與 MCP 共用同名同介面的六個工具」已在該條 R 第 6 點，本條目只談 MCP 端點本身
- [[wikibrain-compounding-knowledge-base-demo]] — 同系列 Day05，網頁上跑三個操作的實測；本篇接續讓外部 AI 工具直接讀寫
- [[tool-description-and-error-message-as-prompt]] — 同系列 Day07：本篇註冊的六個工具，描述與錯誤訊息怎麼寫
- [[human-agent-shared-data-layer-optimistic-locking]] — 同系列 Day08：MCP 與伺服器端 agent 共用的那一層資料存取
- [[wikilink-as-database-edges-and-graph]] — 同系列 Day09+11：links 表與圖譜
- [[mcp-stateless-architecture]] — 協定層 stateless（2026-07-28 RC）；與本篇 SDK transport 層的無狀態不同層級
- [[mcp-stateless-migration-state-ownership]] — 協定層拿掉 session 後 state 的責任歸屬
- [[xcode-mcp-hidden-state-and-authorization]] — **對照組**：本篇是刻意無狀態的 Streamable HTTP server，Xcode MCP 是接到活 IDE 行程的有狀態 stdio bridge
- [[self-built-mcp-server-237-lines]] — Python SDK、stdio 版本的自建 MCP server 對照
- [[aeo-implementation-llms-txt]] — Day 10 提到的 llms.txt
- [[vite-frontend-build-tool]] — Vite 建置工具
