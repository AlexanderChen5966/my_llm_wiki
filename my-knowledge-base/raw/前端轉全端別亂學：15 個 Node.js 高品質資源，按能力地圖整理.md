---
title: "前端轉全端別亂學：15 個 Node.js 高品質資源，按能力地圖整理"
source: "https://codelove.tw/@tony/post/am2dXq"
author:
published:
created: 2026-06-08
description: "前端轉全端別亂學：15 個 Node.js 高品質資源，按能力地圖整理  ======================================    前端轉全端別亂學：15 個 Node.js 高品質資源，按能力地圖整理--------------------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月06日](https://codelove.tw/@tony/post/am2dXq "2026-06-06 17:15:11")

## 前端轉全端別亂學：15 個 Node.js 高品質資源，按能力地圖整理

## 前端轉全端別亂學：15 個 Node.js 高品質資源，按能力地圖整理

```
前端轉全端別亂學：15 個 Node.js 高品質資源，按能力地圖整理
```

---

[![cover-node-fullstack-map.png](https://i.imgur.com/xLQMJKD.jpeg)](https://i.imgur.com/xLQMJKD.jpeg) 前端轉全端，最容易踩的坑不是資源不夠。而是學習順序錯了。 **也許有小夥伴說 ai 寫程式還有必要看這個地圖嗎？ 我的回答是有必要，ai 雖然可以寫程式，但是它需要你指揮它用什麼！怎麼寫好！**

我這邊也有一些 AI Coding 和 Node 技術交流交流群，感興趣的可以加我的私人微信 **ikoala520** 進群，一起學習，共同進步。

很多人一上來就去學某個框架， `Next.js` 、 `NestJS` 、 `Fastify` 都收藏了一遍，結果真的要做專案時才發現：API 會寫，但參數驗證沒做；資料庫能連，但表關係不清楚；本地能跑，但上線後程序掛了沒人管；快取、日誌、部署、錯誤處理更是一團亂。

所以前端轉全端，不是「多學幾個後端框架」。更準確地說，是補齊一張能力地圖：

- 你要理解 `Node.js` 執行環境
- 你要會寫 API 和後端服務
- 你要懂資料庫和 ORM
- 你要知道介面輸入怎麼驗證
- 你要能接快取
- 你要讓服務穩定跑起來
- 你還要逐步建立工程化意識

這篇文章整理了 15 個高品質 `Node.js` 學習資源，不按「誰更紅」排序，而是按前端轉全端最自然的學習路徑來分組。

如果你正在從前端往全端走，建議直接收藏。

[![ability-map.png](https://i.imgur.com/8yKnJil.jpeg)](https://i.imgur.com/8yKnJil.jpeg)

---

### 一、先補 Node.js 和後端基礎

很多前端寫後端時，最大的問題不是語法不會，而是還停留在瀏覽器 JavaScript 的心智裡。

瀏覽器裡你關心的是 DOM、事件、元件、狀態和渲染。

到了後端，你開始面對的是程序、檔案系統、網路請求、環境變數、非同步 I/O、錯誤處理、套件管理和服務生命週期。

所以第一步不是直接上框架，而是先把 `Node.js` 和伺服器端基礎補上。

---

### 1\. Node.js Learn

簡介： `Node.js Learn` 是官方學習入口，適合系統補 `Node.js` 基礎。從執行環境、套件管理、非同步程式設計，到檔案系統、HTTP、除錯和最佳實踐，都有對應內容。

亮點：它最大的價值是幫你理解 `Node.js` 和瀏覽器 JavaScript 的差異。前端轉全端，不能只把 JavaScript 換個地方跑，而是要理解它在伺服器端到底承擔什麼角色。

建議看法：不要一口氣從頭刷到尾，先重點看 `Asynchronous work` 、 `File system` 、 `HTTP` 、 `Debugging` 這些和後端開發直接相關的部分。

訪問地址  
[nodejs.org/en/learn](http://nodejs.org/en/learn)

---

### 2\. MDN Express / Node Tutorial

簡介： `MDN` 的 `Express / Node` 教學適合第一次系統寫伺服器端應用。它會帶你從環境建置、路由、控制器、資料庫，到表單處理和部署，完整走一遍後端專案。

亮點：它講得比較慢，但這正適合前端補後端語感。尤其是請求、回應、路由、樣板、模型這些概念，第一次學不要貪快。

建議看法：把它當成「後端入門專案」來學，而不是當文件查。跟著做一遍，你會更容易理解一個伺服器端應用是怎麼組織起來的。

訪問地址  
[developer.mozilla.org/en-US/docs/Learn\_web\_development/Extensions/Server-side/Express\_Nodejs](http://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)

---

### 二、再進入框架選擇

學完基礎之後，才適合進入框架。

這裡不要被「哪個框架最好」帶偏。框架沒有絕對最好，只有適合的情境不同。

- 想從 `React` 平滑過渡到全端應用，可以看 `Next.js`
- 想寫輕量 API 和邊緣服務，可以看 `Hono`
- 想寫高效能 Node.js API，可以看 `Fastify`
- 想學更工程化的後端架構，可以看 `NestJS`

這四個放在一起看，才不容易被單一框架綁住視野。

---

### 3\. Learn Next.js

簡介： `Learn Next.js` 是 `Next.js` 官方教學，適合已經會 `React` 的前端，用一個 dashboard 專案學習資料獲取、路由、表單、Server Actions、驗證和部署。

亮點：它是前端轉全端最順的過渡入口。因為你不用一上來離開熟悉的 `React` 生態，而是在原有基礎上逐步接觸伺服器端能力。

建議看法：重點看資料獲取、Server Actions、Authentication 和 Deployment。它們正是前端開始接觸全端時最容易模糊的地方。

訪問地址  
[nextjs.org/learn](http://nextjs.org/learn)

---

### 4\. Hono Documentation

簡介： `Hono` 是一個輕量 Web 框架，適合寫 API、BFF、邊緣函式和跨執行環境服務。它的設計更貼近現代 Web Standards，可以執行在 `Node.js` 、 `Cloudflare Workers` 、 `Bun` 、 `Deno` 等環境裡。

亮點：如果你不想一上來就進入很重的後端框架， `Hono` 是一個很舒服的入口。它足夠輕，但又不是玩具，適合寫小型服務、介面層和邊緣場景。

建議看法：先看 Routing、Middleware、Validation、RPC，再看不同執行環境的部署方式。

訪問地址  
[hono.dev/docs](http://hono.dev/docs)

---

### 5\. Fastify Documentation

簡介： `Fastify` 是一個高效能 `Node.js` Web 框架，文件圍繞路由、外掛、hook、schema 驗證、日誌和 TypeScript 支援展開。

亮點：它能幫你理解「輕量但工程化」的後端服務應該怎麼組織。不是所有專案都需要上重框架，但也不能把所有邏輯都塞進幾個路由函式裡。

建議看法：重點看 Plugins、Hooks、Validation and Serialization。理解這幾塊之後，你會更容易寫出可擴充的 API 服務。

訪問地址  
[fastify.dev/docs/latest](http://fastify.dev/docs/latest)

---

### 6\. NestJS Documentation

簡介： `NestJS` 是一個更工程化的 `Node.js` 後端框架，核心概念包括模組、控制器、服務、依賴注入、守衛、管道、中介軟體和攔截器。

亮點：它適合中大型專案和團隊協作，也適合前端開發者建立後端架構邊界。你會開始理解：控制器負責入口，服務負責業務，模組負責組織，驗證和授權也應該有清晰位置。

建議看法：先看 Controllers、Providers、Modules、Pipes、Guards。不要一開始就陷進微服務、GraphQL、複雜部署這些高階主題。

訪問地址  
[nestjs.inode.club/](http://nestjs.inode.club/)

---

### 三、用一門系統課程串起來

只看文件容易碎。

你知道了路由，知道了資料庫，也知道了測試，但這些東西怎麼放進一個真實專案裡，還是需要一門系統課程幫你串起來。

這時候可以看 `Full Stack Open` 。

---

### 7\. Full Stack Open

簡介： `Full Stack Open` 是一門完整的全端課程，涵蓋 `React` 、 `Node.js` 、 `Express` 、資料庫、測試、GraphQL、TypeScript、CI/CD 和容器化。

亮點：它不是只教一個 demo，而是把前端、API、資料、測試和交付串成一條真實開發鏈路。對前端轉全端的人來說，這種「把碎片知識接起來」的價值很大。

建議看法：如果你時間有限，可以先看 Node.js 後端、資料庫、測試、TypeScript 相關章節。不要為了追求完整度，把自己卡在每個細節裡。

訪問地址  
[fullstackopen.com](http://fullstackopen.com/)

---

### 四、補資料庫與 ORM

前端轉全端，遲早繞不開資料庫。

很多人一開始會被 ORM 迷惑，以為會調幾個 API 就等於會資料庫。其實不是。

你至少要理解：

- 表結構怎麼設計
- 關係怎麼表達
- 查詢怎麼寫
- migration 怎麼管理
- ORM 到底幫你隱藏了什麼
- 什麼時候應該回到 SQL 本身

所以這裡建議把 `PostgreSQL` 、 `Prisma` 和 `TypeORM` 放在一起學。

[![資料庫與 ORM 學習關係圖轉存失敗，建議直接上傳圖片檔案](https://codelove.tw/@tony/post/images/fullstack-node/png/data-layer-choice.png)](https://codelove.tw/@tony/post/images/fullstack-node/png/data-layer-choice.png)

[![data-layer-choice.png](https://i.imgur.com/vpysAqy.jpeg)](https://i.imgur.com/vpysAqy.jpeg)

---

### 8\. PostgreSQL Tutorial

簡介： `PostgreSQL Tutorial` 適合補關聯式資料庫基礎，從建表、查詢、關聯、彙總，到交易、檢視表和索引都有入口。

亮點：前端轉全端，不要只會 ORM。ORM 再好，底層還是資料庫。你越懂 SQL，越不容易寫出低效查詢，也越能理解資料模型的邊界。

建議看法：先把 SELECT、JOIN、GROUP BY、交易、索引這些基礎概念學穩。後面再看 ORM，會清楚很多。

訪問地址  
[postgresql.org/docs/current/tutorial.html](http://postgresql.org/docs/current/tutorial.html)

---

### 9\. Prisma Documentation

簡介： `Prisma` 是 TypeScript 專案裡很常見的資料存取工具，重點是 schema、migration、Prisma Client 和型別安全查詢。

亮點：它對前端開發者很友好。你定義資料模型之後，可以獲得比較好的型別提示和查詢體驗，適合快速搭建業務專案。

建議看法：重點看 Schema、Migrate、Client、Relations。不要只停留在 CRUD 範例，要理解 schema 和資料庫結構之間的關係。

訪問地址  
[docs.prisma.io](http://docs.prisma.io/)

---

### 10\. TypeORM

簡介： `TypeORM` 是一個面向 `TypeScript` 和 `JavaScript` 的 ORM，支援多種資料庫，常見寫法包括實體、裝飾器、Repository、QueryBuilder 和 migration。

亮點：相比 `Prisma` ， `TypeORM` 更接近傳統 ORM 風格，在一些企業專案、舊專案和 `NestJS` 生態裡仍然很常見。學它的價值不只是「多會一個工具」，而是理解另一類 ORM 心智模型。

建議看法：重點看 Entities、Relations、Repository、QueryBuilder、Migrations。如果你後面學 `NestJS` ，也可以順便看它和 `TypeORM` 的整合方式。

訪問地址  
[typeorm.io](http://typeorm.io/)

---

### 五、介面驗證與型別安全

很多前端剛寫後端時，會預設「前端傳過來的資料是對的」。

這是一個危險習慣。

到了伺服器端，任何輸入都不應該被直接相信。請求 body、query、params、環境變數、第三方回呼資料，都需要驗證。

這就是 `Zod` 的價值。

---

### 11\. Zod

簡介： `Zod` 是 TypeScript-first 的 schema 驗證函式庫，可以用來驗證請求參數、表單資料、環境變數、設定檔和介面回傳值。

亮點：它很適合前端轉全端，因為你可以用一份 schema 同時獲得執行期驗證和 TypeScript 型別推導。也就是說，它不只是「判斷資料對不對」，還可以讓型別系統跟著資料規則走。

建議看法：先看 String、Object、Array、Union、Refine、Transform，再把它接到 API 入參驗證裡。學會之後，你寫介面會穩很多。

訪問地址  
[zod.dev](http://zod.dev/)

---

### 六、補快取與 Redis

很多全端專案一開始不需要 Redis。

但只要專案稍微複雜一點，你很快會遇到這些問題：

- 熱點資料要不要快取
- 登入態和 session 怎麼存
- 短時間限流怎麼做
- 驗證碼、暫時 token、排行榜、計數器怎麼處理
- 後台任務和佇列能不能借助 Redis

所以前端轉全端，不一定一開始就深入 Redis，但至少要知道 Node.js 專案怎麼接 Redis。

---

### 12\. node-redis

簡介： `node-redis` 是 Node.js 生態裡常用的 Redis 用戶端，可以讓你的 Node.js 服務連接 Redis，執行字串、雜湊、列表、集合、發布訂閱等操作。

亮點：它適合補「快取層」這塊能力。很多前端寫全端應用時，只會資料庫，不知道快取應該放在哪、怎麼用、什麼時候不該用。 `node-redis` 是一個很好的實作入口。

建議看法：先看連接、基本命令、過期時間、Hash、Pub/Sub。不要一開始就把 Redis 當萬能資料庫，它更適合快取、暫時狀態和高頻存取場景。

訪問地址  
[github.com/redis/node-redis](http://github.com/redis/node-redis)

---

### 七、部署與工程化

本地能跑，只是第一步。

真正進入全端之後，你還要考慮：

- 服務怎麼打包
- 環境變數怎麼管理
- 程序掛了怎麼重啟
- 日誌怎麼看
- 怎麼容器化
- 怎麼避免把 demo 寫成一次性腳本

這一組資源，就是把專案從「能跑」推向「能交付、能維護」。

[![Node.js 專案上線前工程化鏈路轉存失敗，建議直接上傳圖片檔案](https://codelove.tw/@tony/post/images/fullstack-node/png/production-chain.png)](https://codelove.tw/@tony/post/images/fullstack-node/png/production-chain.png)

[![production-chain.png](https://i.imgur.com/Dh4A3Tc.jpeg)](https://i.imgur.com/Dh4A3Tc.jpeg)

---

### 13\. Docker Node.js Guide

簡介： `Docker Node.js Guide` 適合學習如何把 Node.js 應用容器化，並串上前端、資料庫、測試和 CI/CD。

亮點：容器化會逼你把很多邊界想清楚：依賴怎麼安裝，環境變數怎麼傳，伺服器埠怎麼暴露，資料庫怎麼連接，測試怎麼跑，生產映像怎麼建置。

建議看法：先看單個 Node.js 應用的 Dockerfile，再看 compose、多服務、資料庫和 CI/CD。不要一開始就追求複雜編排。

訪問地址  
[docs.docker.com/guides/nodejs](http://docs.docker.com/guides/nodejs)

---

### 14\. PM2

簡介： `PM2` 是 Node.js 常用的程序管理工具，可以用來啟動服務、守護程序、自動重啟、查看日誌，也支援 cluster 模式。

亮點：它補的是「服務怎麼穩定跑起來」這塊能力。很多全端新手把專案部署到伺服器後，只會 `node app.js` ，終端一關服務就沒了。 `PM2` 能讓你理解程序守護、日誌、重啟和生產執行的基本概念。

建議看法：先看 Quick Start、Process Management、Logs、Startup Script。學會之後，至少能把一個 Node.js 服務比較穩地跑在伺服器上。

訪問地址  
[pm2.keymetrics.io/docs/usage/quick-start](http://pm2.keymetrics.io/docs/usage/quick-start)

---

### 15\. Node.js Best Practices

簡介： `Node.js Best Practices` 是一份長期維護的工程實踐清單，涵蓋專案結構、錯誤處理、測試、生產環境、安全、效能和可維護性。

亮點：當你已經能寫 API、連資料庫、做部署之後，它可以幫你檢查自己的專案是不是還停留在 demo 水平。很多問題不是「不會寫」，而是「寫得能不能長期維護」。

建議看法：不要當成普通教學從頭看。更適合在做專案時對照檢查：錯誤處理有沒有統一？日誌有沒有規範？設定有沒有隔離？安全有沒有基本防護？測試有沒有覆蓋關鍵路徑？

訪問地址  
[github.com/goldbergyoni/nodebestpractices](http://github.com/goldbergyoni/nodebestpractices)

---

### 推薦學習路線

如果你不想一次看太多，可以按目標來選。

[![Node.js 全端推薦學習路線轉存失敗，建議直接上傳圖片檔案](https://codelove.tw/@tony/post/images/fullstack-node/png/learning-routes.png)](https://codelove.tw/@tony/post/images/fullstack-node/png/learning-routes.png)

[![learning-routes.png](https://i.imgur.com/NLNRz45.jpeg)](https://i.imgur.com/NLNRz45.jpeg)

#### 1\. 想快速做一個全端專案

建議路線：

`Learn Next.js` → `Prisma` → `Zod` → `Docker Node.js Guide` → `PM2`

這條路線適合已經會 `React` ，想盡快做出一個可以上線的小專案的人。先用 `Next.js` 把前後端串起來，再用 `Prisma` 管資料，用 `Zod` 做介面驗證，最後補部署。

#### 2\. 想補真正的後端基本功

建議路線：

`Node.js Learn` → `MDN Express / Node Tutorial` → `PostgreSQL Tutorial` → `Fastify` → `Node.js Best Practices`

這條路線適合不想只做「會寫頁面的全端」，而是想把伺服器端基礎補穩的人。它會慢一點，但基礎會更紮實。

#### 3\. 想走工程化後端路線

建議路線：

`Node.js Learn` → `NestJS` → `TypeORM` → `PostgreSQL` → `PM2` → `Node.js Best Practices`

這條路線適合未來可能參與中大型專案、團隊專案、企業專案的人。重點不是追求輕，而是建立模組化、分層、依賴注入、資料建模和生產執行的意識。

#### 4\. 想補全專案交付能力

建議路線：

`Full Stack Open` → `Zod` → `node-redis` → `Docker Node.js Guide` → `PM2`

這條路線適合已經能寫一些 API，但發現專案一複雜就失控的人。它會幫你補輸入驗證、快取、容器化和程序管理。

---

### 最後

前端轉全端，不是把頁面後面隨手接一個 API。

真正的全端能力，是你能從使用者介面一路理解到伺服器端、資料庫、快取、部署和工程品質。

所以不要只問：

> 我該學哪個 Node.js 框架？

更應該問：

> 我現在缺的是哪一塊能力？

如果你還沒有伺服器端基礎，就先看 `Node.js Learn` 和 `MDN Express / Node Tutorial` 。

如果你想從前端自然過渡，就看 `Learn Next.js` 。

如果你想寫更清晰的後端服務，就看 `Fastify` 或 `NestJS` 。

如果你開始碰資料庫，就把 `PostgreSQL` 、 `Prisma` 、 `TypeORM` 放在一起看。

如果你已經開始寫真實 API，就一定要補 `Zod` 。

如果專案需要快取，就接觸 `node-redis` 。

如果準備上線，就不要跳過 `Docker` 、 `PM2` 和 `Node.js Best Practices` 。

學習全端最重要的不是收藏最多資源，而是知道每個資源幫你補哪一塊短板。

我這邊也有一些 AI Coding 和 Node 技術交流交流群，感興趣的可以加我的私人微信 **ikoala520** 進群，一起學習，共同進步。

這 15 個資源，按階段慢慢吃透，比東學一個框架、西看一個教學，要穩得多。

---

原文出處： [https://juejin.cn/post/7647086721050525705](https://juejin.cn/post/7647086721050525705)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/am2dXq) [294瀏覽](https://codelove.tw/@tony/post/am2dXq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/am2dXq)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝6 💬2

337

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780888498&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fam2dXq&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780888498229&amp;bpp=3&amp;bdt=252&amp;idt=347&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780888487%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3328268669018&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668&amp;oid=2&amp;pvsid=1939209616796133&amp;tmod=473573085&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=402" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>