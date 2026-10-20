---
title: "Day 10 - 用 Vite 渲染 SEO - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10413861"
author:
  - "[[iThome]]"
published:
created: 2026-09-23
description: "前言 現在做全端專案，預設答案幾乎就是 Next.js。我沒有用，用的是 Vite 打包的純前端 SPA，配一台 Express。 這個決定只有一筆債：SEO，得自己動手還。 理由一：MCP 與 OA..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 10

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 10 篇

## Day 10 - 用 Vite 渲染 SEO

- 分享至

## 前言

現在做全端專案，預設答案幾乎就是 Next.js。我沒有用，用的是 Vite 打包的純前端 SPA，配一台 Express。

這個決定只有一筆債：SEO，得自己動手還。

## 理由一：MCP 與 OAuth 要跟前端同源

我的伺服器不只服務瀏覽器，它同時是一台 MCP 伺服器，還要當 OAuth 授權伺服器。這些端點有嚴格的位置要求：

```bash
/mcp                                    MCP 端點
/authorize  /token  /register  /revoke  OAuth 端點
/.well-known/oauth-authorization-server OAuth 探測
/.well-known/oauth-protected-resource/mcp
```

它們必須跟網頁在同一個網域的根路徑上。client 拿到 `https://你的網域/mcp` 之後，第一站一定是同網域的 `.well-known` ：那份探測文件不能放在子路徑，也不能分到另一個服務。

這些是長連線、串流、標準協定的東西。用一般的 Express 路由寫最直接，SDK 也是照這個假設設計的。把它們塞進框架的 API 路由，順序就反了：得先看框架允許我做什麼，協定的要求往後排。

## 理由二：只想要一個服務

部署的時候我只想要一個容器： `npm run build` 把前端打包成靜態檔，Express 一邊提供那些靜態檔（加上單頁應用的 fallback），一邊處理 API、MCP、OAuth。

一個服務、一份日誌、一個健康檢查、一次部署。對一個人維護的產品，這個簡單性值很多錢。

攤開來看，那一支 `createApp()` 同時掛著這些東西：

| 掛在哪 | 是什麼 |
| --- | --- |
| `express.static(webDist)` ＋ `*splat` fallback | 前端打包出來的 SPA，任何路徑都回 `index.html` |
| `/api` | 給 SPA 用的 REST API |
| `/mcp` | 給外部 AI 工具用的 MCP 伺服器 |
| `mcpAuthRouter(...)` | OAuth 2.1 授權伺服器，讓 Claude.ai 這類客戶端自己完成註冊與授權 |
| `/` 、 `/robots.txt` 、 `/sitemap.xml` 、 `/llms.txt` | 行銷頁是建置時預先渲染的，給爬蟲的那三份檔案是請求進來時現組的 |
| `/healthz` | 監控用的健康檢查 |

*出處： [`src/app.ts:28-215`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/app.ts#L28-L215)*

同源帶來的好處就藏在這張表裡。SPA 打 `/api` 不用處理 CORS，MCP 客戶端探測到的 OAuth 中繼資料跟 `/mcp` 在同一個網域，行銷頁跟應用程式共用同一張憑證。這些如果拆成兩三個服務，每一條都要另外接一次。

要小心的是 `*splat` 那條 fallback：它把所有沒對到靜態檔的路徑都回 `index.html` ，所以 `/api/` 與 `/mcp` 要先被排除掉，否則打錯的 API 路徑會拿到一頁 HTML，拿不到 404。

## 理由三：這個應用本來就是後台

登入之後的介面是三欄式的工作區：側欄檔案樹、中間編輯器、右邊對話面板。使用者會在裡面待很久，切頁全部是前端路由，資料靠 API 拿。

這種應用從伺服器端渲染得到的好處很有限，因為內容都是私人的、不需要被搜尋引擎看到，首屏也不是關鍵指標，使用者一天登入一次，之後就一直待著。

## SEO 這筆債自己還

公開的頁面就不一樣了，它們需要被搜尋引擎和 AI 爬蟲看到。純 SPA 在這件事上接近零分：爬蟲拿到的是一個空的 `<div id="root">` ，Google 會晚一步試著渲染，AI 爬蟲跟社群預覽則根本不執行 JavaScript。

這筆債比想像中小，因為需要被索引的頁面只有十幾頁，不是整個應用。Next.js 使用者不用想這件事，我得自己補。

## 只有幾頁需要被看見

這個產品的頁面分兩種：

**私人的** ：工作區、編輯器、設定、圖譜。這些不但不需要被索引，還應該 **主動擋掉** 。使用者的知識庫內容絕對不該出現在搜尋結果裡。

**公開的** ：首頁、說明頁的五個分頁、三個比較頁、隱私政策與服務條款。這些需要被搜尋引擎跟 AI 爬蟲看到。

所以要做的只有一件事：把這十幾頁變成靜態 HTML。範圍比整站 SSR 小很多。

## 建置時預先渲染

做法是在建置流程的最後加一步：用 `react-dom/server` 把那幾頁渲染成完整的 HTML 檔。

```markdown
npm run build
  ├── vite build            前端打包
  ├── tsc                   後端編譯
  └── prerender             把公開頁面渲染成靜態 HTML
```

產出的每一份 HTML 都帶完整的 head：title、description、canonical、hreflang（中英雙語互指，加 x-default）、OpenGraph、以及 JSON-LD 結構化資料（軟體應用的類型，含價格；方案頁的常見問題另外標成 FAQPage）。

Express 這邊做路由：公開頁面回預先渲染的檔案，其他一律回 SPA 的殼。

```ts
app.get('/help/:page', (req, res) => {
  const en = req.query.lang === 'en' || preferEnglish(req.headers['accept-language']);
  res.set('Vary', 'Accept-Language');
  res.sendFile(\`help.${req.params.page}${en ? '.en' : ''}.html\`);
});
```

這是簡化過的示意，實際那支還檢查 slug 是不是純小寫英文、檔案不存在就交給下一個路由，並且記一次瀏覽（ [`src/app.ts:180-189`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/app.ts#L180-L189) ）。

首頁多一個判斷：有 session cookie 就走 SPA（登入的人要看工作區），沒有才回預先渲染的行銷頁，並且 `Vary` 要加上 `Cookie` 。

## 建置階段的值要當建置參數管

上線第一天我去看預先渲染的頁面，發現所有 canonical 都寫著 `example.com` 。

原因是預先渲染在 **建置階段** 跑，而它要知道正式網址才能產生正確的 canonical 與 OpenGraph URL。容器建置時沒有 `APP_URL` ，於是它用了預設值。修法是把它當成建置參數傳進去：

```dockerfile
ARG APP_URL
ENV APP_URL=$APP_URL
RUN npm run build && npm prune --omit=dev
```

*出處： [`Dockerfile:14-16`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/Dockerfile#L14-L16)*

這代表 **換網域要重新建置映像** ，不能只改環境變數重啟。這件事違反直覺，因為大部分環境變數都是執行階段的事，所以後來寫進了部署文件。

一般化的說法是： **任何在建置階段被寫進產物的值，都要當成建置的一部分來管** 。canonical URL、版本號、feature flag 的預設值，都屬於這一類。

## robots 與 sitemap

`robots.txt` 用白名單的心態寫：明確允許公開頁面，明確擋掉 `/n/` （筆記）、 `/settings` 、 `/api/` 、 `/mcp` 、 `/s/` （公開分享連結）。

使用者可以把某一頁產生一個公開連結分享出去，但那不代表他想要它出現在 Google 上。所以那些頁面除了 robots 擋，回應還帶 `X-Robots-Tag: noindex` ，而且 `no-store` 。 **「可以被連結」跟「可以被索引」是兩件事** ，使用者預期的是前者。

`sitemap.xml` 就列公開頁面，跟 robots 一樣是請求進來時現組的。

## 順手做 llms.txt

現在除了搜尋引擎，還有 AI 在讀你的網站。使用者問「有沒有什麼工具可以做 X」，模型會去翻。

`llms.txt` 是一份放在根目錄的純文字摘要，講清楚這個產品是什麼、有哪些功能、怎麼收費、文件在哪裡。花二十分鐘寫，成本很低。

我在裡面也放了 Karpathy 那則 gist 的連結，因為這個產品的來歷本身就是說明的一部分。

## 什麼情況我會選 Next.js

誠實一點：如果這個產品的公開內容佔比更高（比如說有一個內容站、有部落格、有大量需要被索引的頁面），我應該會選 Next.js，然後把 MCP 和 OAuth 另外拆一個服務出去。

分界線大概是這樣： **公開內容是主體、後台是配角，選框架；後台是主體、公開內容只有幾頁，選 SPA 加自己補預先渲染。**

## 小結

這個產品的主體在登入之後，公開給爬蟲看的只有十幾頁門面。所以前端是 Vite 打包的 SPA 配一台 Express，協定端點與 API 都掛在同一台服務上；那十幾頁在建置時預先渲染成完整的 HTML，爬蟲拿到的不再是空殼。要記住的是：正式網址在建置階段就燒進了那些頁面，換網域要重新建置映像，改環境變數重啟是不夠的。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10413279)

[

Day 09 - 用資料庫存 wiki 連結的邊

](https://ithelp.ithome.com.tw/articles/10413279)[下一篇](https://ithelp.ithome.com.tw/articles/10414163)

[

Day 11 - 用 Canvas 視覺化圖譜

](https://ithelp.ithome.com.tw/articles/10414163)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 13 篇

目錄

1. 9
	[Day 09 - 用資料庫存 wiki 連結的邊](https://ithelp.ithome.com.tw/articles/10413279)
2. 10
	[Day 10 - 用 Vite 渲染 SEO](https://ithelp.ithome.com.tw/articles/10413861)
3. 11
	[Day 11 - 用 Canvas 視覺化圖譜](https://ithelp.ithome.com.tw/articles/10414163)
4. 12
	[Day 12 - i18n 與深色模式](https://ithelp.ithome.com.tw/articles/10414909)
5. 13
	[Day 13 - 抓取網頁來源，匯入知識庫](https://ithelp.ithome.com.tw/articles/10415534)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/11/12/8ddb4075-cb3c-4b80-be90-78a42e0bc603_small.png)

[Citrix 多工虛擬化解決方案協助企業增進生產力](https://itplus.ithome.com.tw/vod-page/882?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/8/8/acad899b-f06a-4f28-9c8e-4ba4f62eb2c7_small.jpg)

[從遠古到現代：Linux 系統管理的大躍進之路](https://itplus.ithome.com.tw/webinar-page/269?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/4/9/696ceba6-85e0-4c32-890d-083cdd48cc48_small.png)

[低碳關鍵：建構線上碳流資訊平台的重要性](https://itplus.ithome.com.tw/vod-page/1136?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位永續高峰會 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/aa6d8015-e4f4-4b47-854e-7edd38eac52d_small.png)

[高效治理新解方：電子簽章、AI 智慧文件與數據驅動決策](https://itplus.ithome.com.tw/vod-page/1250?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

27 分