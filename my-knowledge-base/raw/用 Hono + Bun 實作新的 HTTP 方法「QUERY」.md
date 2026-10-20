---
title: "用 Hono + Bun 實作新的 HTTP 方法「QUERY」"
source: "https://codelove.tw/@tony/post/x8OJZq"
author:
published:
created: 2026-07-07
description: "先前，我看到了這樣的一則貼文。> ¡Histórico! 新的 HTTP 標準方法。> 名稱叫 QUERY，是 GET 和 POST 的替代方案> （新的 HTTP 方法成為標準。名字是 QUERY，會成為 GET 和 POST 的替代）這是在說新的 HTTP 方法 **`QUERY`*..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月05日](https://codelove.tw/@tony/post/x8OJZq "2026-07-05 08:15:08")

先前，我看到了這樣的一則貼文。

> ¡Histórico! 新的 HTTP 標準方法。  
> 名稱叫 QUERY，是 GET 和 POST 的替代方案  
> （新的 HTTP 方法成為標準。名字是 QUERY，會成為 GET 和 POST 的替代）

這是在說新的 HTTP 方法 **`QUERY`** 終於成為了「Proposed Standard」。  
規格已經以 [RFC 10008](https://www.rfc-editor.org/info/rfc10008/) 的形式公開。

或許有人會想：「有 GET 和 POST 不就夠了嗎？」但其實在想同時兼顧兩者優點的情境下，會有一些不起眼卻很困擾的場面。  
這次我會整理一下 `QUERY` 方法到底是什麼，並實際用 [Hono](https://hono.dev/) + [Bun](https://bun.sh/) 來實作並跑看看 API。

## QUERY 方法是什麼

簡單來說，就是 **「可以帶 request body 的、安全版 GET」** 。

和既有的 `GET` / `POST` 相比，定位大致如下：

GETPOST **QUERY** 不會改變資源✅❌✅可使用 request body❌(※)✅✅冪等✅❌✅回應可快取✅❌✅也就是說：

- 像 `GET` 一樣 **不會改變資源狀態**
- 像 `POST` 一樣 **可以透過 body 傳遞請求**
- 而且 **回應還可以快取**

是一個幾乎就是為了搜尋（query）而設計的 method。

技術上來說， `GET` 也不是不能加 body，但規範上是「GET 的 body 不應該賦予意義」，而且可能會被 proxy 或快取系統直接忽略。因此實務上幾乎不能用，這就是 (※) 的意思。

### 為什麼需要它

來想像一個要把複雜搜尋條件傳給伺服器的情境。

如果用 `GET` 來做，條件就必須全部放在 query string 裡。

```
GET /blogs?query=入門&limit=2&tags=TypeScript,Bun&sort=id
```

這樣雖然可以運作，但會有這些問題：

- 條件一旦變成巢狀結構（陣列或物件）就很難表達
- 可能會撞到 URL 長度限制
- 搜尋條件會直接暴露在 URL 上

反過來如果做成 `POST /blogs/search` ，又會有 **「明明是搜尋，語意上卻看起來像會改變狀態的 POST」** 這種不協調感，而且也沒辦法快取回應。

`QUERY` 正是用來填補這個落差的 method： **想用 body 傳複雜條件，但本質上仍是安全的讀取操作。**

## 實作看看

前面鋪陳有點長，接著實際動手。  
這次 runtime 用 Bun，Web framework 用 Hono。

另外，這次的程式碼放在以下 repository：

- [https://github.com/maaaashi/query-method-api](https://github.com/maaaashi/query-method-api)
```
bun create hono@latest query-method-api
cd query-method-api
bun add hono zod @hono/zod-validator
```

### 路由

Hono 除了 `app.get` / `app.post` 這些熟悉的方法之外，也提供了可以註冊任意 method 的 `app.on(method, path, handler)` 。  
像 `QUERY` 這種尚未普及的新 method，也可以很自然地註冊。

src/index.ts

```
import { Hono } from "hono";
import { queryBlogsHandlers } from "./handlers/blogs";

const app = new Hono();

app.get("/ping", (c) => c.text("pong"));

app.options("/blogs", (c) => {
  return c.body(null, 204, {
    Allow: "QUERY, OPTIONS",
    "Accept-Query": "application/json",

    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "QUERY, OPTIONS",
    "Access-Control-Allow-Headers": "Content-Type, Accept-Query",
  });
});
app.on("QUERY", "/blogs", ...queryBlogsHandlers);

export default app;
```

重點是在 `app.options` 回傳 `OPTIONS` 。

`QUERY` 還是個很新的 method，客戶端自然會想先知道「這個 endpoint 接不接受 `QUERY` ？」  
因此我們在 `OPTIONS` 的 response 中回傳 **`Allow: QUERY, OPTIONS`** ，並且再用 **`Accept-Query: application/json`** 宣告「body 會以 JSON 接收」。

`Accept-Query` 是用來表示伺服器可接受之 query 格式的 response header。可以把它想成 `Accept-Post` 的 QUERY 版本。

### 處理器

這裡是搜尋本體的 handler。使用 `@hono/zod-validator` 一併做 body validation。

src/handlers/blogs.ts

```
import { zValidator } from "@hono/zod-validator";
import { createFactory } from "hono/factory";
import * as z from "zod";
import { allBlogs } from "../data";

const RequestJsonSchema = z
  .object({
    query: z.string().optional(),
    limit: z.number().int().optional(),
  })
  .strict();

const factory = createFactory();

export const queryBlogsHandlers = factory.createHandlers(
  async (c, next) => {
    const contentType = c.req.header("content-type") || "";

    if (contentType && !contentType.includes("application/json")) {
      return c.json({ error: "Unsupported Media Type" }, 415);
    }
    await next();
  },
  zValidator("json", RequestJsonSchema, (result, c) => {
    if (!result.success) {
      return c.json({ error: "Invalid request body" }, 400);
    }
  }),
  async (c) => {
    const { query, limit } = c.req.valid("json");

    return c.json(allBlogs.search(query).sortById().take(limit), 200, {
      "Accept-Query": "application/json",
    });
  },
);
```

從上往下看，流程是：

1. 如果 `Content-Type` 不是 JSON，就回傳 **415（Unsupported Media Type）**
2. 如果 body 不符合 schema，就回傳 **400（Bad Request）**
3. 沒問題的話就用 `query` 篩選 → 用 `id` 排序 → 用 `limit` 限制數量後回傳

`QUERY` 可以帶 body，因此能像這樣 **用 JSON 接收搜尋條件** ，這正是它和 `GET` 最大的差異。

### 領域層

搜尋邏輯放在 domain 層。沒有做什麼特別的事，就是很單純的 immutable 實作。

src/domains/blog.ts

```
export class Blog {
  constructor(
    public readonly id: number,
    public readonly title: string,
    public readonly content: string,
  ) {}
}

export class Blogs {
  constructor(private readonly blogs: Blog[]) {}

  search(query?: string): Blogs {
    if (!query) return this;

    return new Blogs(this.blogs.filter((b) => b.title.includes(query)));
  }

  sortById(): Blogs {
    return new Blogs([...this.blogs].sort((a, b) => a.id - b.id));
  }

  take(limit?: number): Blogs {
    if (!limit) return this;

    return new Blogs(this.blogs.slice(0, limit));
  }
}
```

`search` / `sortById` / `take` 各自回傳新的 `Blogs` ，所以可以像 `allBlogs.search(query).sortById().take(limit)` 這樣串接 method chain，寫起來很順。

資料則是使用 `src/data.ts` 裡硬編碼的 10 筆 blog。

## 動起來看看

啟動伺服器。

```
bun run dev
# open http://localhost:3000
```

### 先用 OPTIONS 確認

先用 `OPTIONS` 問看看這個 endpoint 是否接受 `QUERY` 。

```
$ curl -s -X OPTIONS localhost:3000/blogs -i
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: QUERY, OPTIONS
Access-Control-Allow-Headers: Content-Type, Accept-Query
Allow: QUERY, OPTIONS
Accept-Query: application/json
Content-Length: 0
```

有收到 `Allow: QUERY, OPTIONS` 和 `Accept-Query: application/json` 。  
這表示「這個 endpoint 接受 QUERY，且 body 格式是 JSON」。

### 用 QUERY 搜尋

重頭戲來了。用 `curl` 的 `-X QUERY` 指定 method，並用 `-d` 帶入 body。

```
# 取得全部資料（傳空 JSON）
$ curl -s -X QUERY localhost:3000/blogs \
    -H "Content-Type: application/json" \
    -d '{}'
{"blogs":[{"id":1,"title":"Clojure入門",...},{"id":2,"title":"TypeScriptの型システム",...}, ...]}
```
```
# 搜尋「入門」，只取 2 筆
$ curl -s -X QUERY localhost:3000/blogs \
    -H "Content-Type: application/json" \
    -d '{"query":"入門","limit":2}'
{"blogs":[
  {"id":1,"title":"Clojure入門","content":"こんにちは。Clojureの世界へようこそ。"},
  {"id":5,"title":"GraphQL入門","content":"GraphQLはAPIの新しい形です。"}
]}
```

原本在 `GET` 下不太好處理的 **「用 JSON body 傳搜尋條件」** ，現在可以非常自然地做到。

在 `curl` 中使用自訂 HTTP method，要用 `-X` （ `--request` ）。 `QUERY` 雖然是相對新的 method，但 `curl` 本質上只是把 method 名稱當字串送出，所以不需要特別支援也能直接送。

### 也確認一下異常情況

驗證也有正常運作。

```
# 傳入 schema 以外的 key → 400
$ curl -s -o /dev/null -w "%{http_code}\n" -X QUERY localhost:3000/blogs \
    -H "Content-Type: application/json" -d '{"foo":1}'
400

# Content-Type 不是 JSON → 415
$ curl -s -o /dev/null -w "%{http_code}\n" -X QUERY localhost:3000/blogs \
    -H "Content-Type: text/plain" -d 'hi'
415
```

因為有加上 `.strict()` ，所以只要混入未知 key 就會被擋下來。

## 總結

- `QUERY` 是一種新的 HTTP 方法，定位為 **「可以帶 body 的、安全且冪等的 GET」**
- 它結合了 `GET` 的「不改變狀態、可快取」與 `POST` 的「可用 body 傳遞複雜條件」優點
- 在 Hono 裡可以用 `app.on("QUERY", path, handler)` 很自然地實作
- 用 `OPTIONS` 回傳 `Allow` / `Accept-Query` ，能讓客戶端清楚知道支援狀況，做法很貼心
- 不過目前瀏覽器、proxy、CDN 的支援還在發展中，因此短期內與 `POST` 並用仍是比較務實的做法

能看到新的 method 納入標準，老實說寫起來還挺令人興奮的。  
未來在設計搜尋型 endpoint 時，這會是值得放在腦中的一個選項。

## 參考

- [本次程式碼（GitHub） - maaaashi/query-method-api](https://github.com/maaaashi/query-method-api)
- [RFC 10008 - The HTTP QUERY Method](https://www.rfc-editor.org/info/rfc10008/)
- [Hono](https://hono.dev/)
- [Bun](https://bun.sh/)

---

原文出處： [https://qiita.com/maaaashi/items/6eefccb7361f64607865](https://qiita.com/maaaashi/items/6eefccb7361f64607865)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x8OJZq) [21瀏覽](https://codelove.tw/@tony/post/x8OJZq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x8OJZq)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385972&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8OJZq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385972819&amp;bpp=1&amp;bdt=85&amp;idt=60&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8071482004861&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395661%2C42533293%2C95393485&amp;oid=2&amp;pvsid=2466622370076911&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=83" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>