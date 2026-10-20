---
title: "新的 HTTP 方法：QUERY"
source: "https://codelove.tw/@tony/post/x0nZDa"
author:
published:
created: 2026-07-07
description: "你有在使用 REST API 嗎？你知道 `GET`、`POST`、`PUT`、`DELETE`……  但你知道有一個新的 HTTP 方法在 **2026 年 6 月** 標準化了嗎？它叫做 **`QUERY`**，而且它解決了一個開發者已經繞路處理了 16 年的問題。這是自 `PATCH`..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月06日](https://codelove.tw/@tony/post/x0nZDa "2026-07-06 11:06:59")

你有在使用 REST API 嗎？你知道 `GET` 、 `POST` 、 `PUT` 、 `DELETE` ……

但你知道有一個新的 HTTP 方法在 **2026 年 6 月** 標準化了嗎？它叫做 **`QUERY`** ，而且它解決了一個開發者已經繞路處理了 16 年的問題。

這是自 `PATCH` （2010）以來第一個新的標準 HTTP 方法。沒錯，你沒看錯：HTTP 動詞已經 **16 年** 沒有重大創新了。

## GET 方法的問題

你想在後端建立一個端點來取得訂單資訊，並加入篩選條件。因為你是在取得資源，而且這也是 REST 規範，所以這個端點會使用 `GET` 方法。

你在 URL 上加上不同參數來篩選結果，只回傳符合搜尋條件的訂單：

```
GET /orders?select=surname,givenname,email&limit=10&match="email=*@example.*"
```

**問題** ：

- URL 有長度限制（通常最多 `8000` 字元）
- 參數會出現在記錄檔中（安全性問題）
- 無法做複雜查詢（進階篩選、SQL 查詢、JSONPath）
- 編碼正確性很麻煩

## 那改用 POST 呢？

為了處理 `GET` 請求的各種問題，你可能會改用 `POST` 請求：

```
POST /orders HTTP/1.1
Host: api.example.org
Content-Type: application/x-www-form-urlencoded

select=surname,givenname,email&limit=10&match="email=*@example.*"
```

**問題** ：

- `POST` **不是冪等** 的 → 如果網路中斷，你不能安全地重新送出同一個請求
- 沒有標準的 HTTP 快取機制
- 代理伺服器不知道能不能安全地拒絕或重試
- 每個框架都會實作自己的一套「安全 POST」系統

## 解決方案：QUERY

`QUERY` 給你兩全其美的好處：

```
QUERY /orders HTTP/1.1
Host: api.example.org
Content-Type: application/x-www-form-urlencoded
Accept: application/json

select=surname,givenname,email&limit=10&match="email=*@example.*"
```

**優點** ：

- **沒有 URL 長度限制** – 請求本文可以包含不限大小的 SQL 查詢、JSONPath 或複雜篩選條件
- **冪等** – 執行這個請求 10 次 = 執行 1 次

> **什麼是冪等性？**
> 
> 如果一個操作被呼叫多次，產生的結果和呼叫一次相同，那它就是冪等的。
> 
> **具體例子** ：
> 
> - ✅ `GET /users/123` （冪等）→ 呼叫 1 次 = 呼叫 10 次 = 相同資料
> - ✅ `QUERY /users` （冪等）→ 重送 = 相同結果
> - ❌ `POST /users` （非冪等）→ 呼叫 1 次建立 1 位使用者，呼叫 2 次就建立 2 位使用者
> 
> **為什麼這很重要？** 如果網路中斷，你可以安全地重送，不會產生重複資料或資料損壞。

- **可快取** – 代理伺服器、CDN、瀏覽器都能快取回應（像 `GET` 一樣）
- **可在請求本文中傳資料** – 在本文送出複雜資料（像 `POST` 一樣），而不是放在 URL 裡
- **安全** – 絕不修改伺服器狀態（唯讀）
- **可安全重放** – 如果網路中斷，你可以有把握地重送請求
- **請求的 URI** – 伺服器可以建立一個永久網址來重放同一個請求

> **請求的 URI 與結果的 URI**
> 
> **傳統 `POST`** （結果的 URI）：
> 
> ```
> POST /orders → 200 OK
> 
> Content-Location: /results/xyz789
> ```
> 
> **`QUERY`** （請求的 URI）：
> 
> ```
> QUERY /orders → 200 OK
> 
> Location: /saved-queries/42
> ```
> 
> **神奇之處** ：重放 `/saved-queries/42` 會以 **更新後的資料** 重新執行同一個 SQL/JSONPath 查詢。
> 
> **具體來說** ：
> 
> ```
> # 之後...
> 
> GET /saved-queries/42
> 
> # → 伺服器重新執行原本的 SQL 查詢
> 
> # → 你會拿到最新結果
> ```

**它的用途** ：

- 分享查詢（把連結寄給同事）
- 將搜尋加入書籤
- 快取查詢（不只是快取結果）
- 定期重放，而不需要每次都重新送出本文

多虧了 `QUERY` ，我們終於能對複雜請求做出真正可用的 HTTP 快取。代理伺服器、CDN 和瀏覽器現在都能快取帶有請求本文的請求。這對效能來說非常重要。

我們也得到了 REST API 設計上的演進。與其寫成：

```
GET /api/users/active
GET /api/users/inactive
GET /api/users/premium
```

現在你可以：

```
QUERY /api/users
```

只要一個端點，就更容易 **維護** 、 **保護** 和 **監控** 。

## 總結：GET vs POST vs QUERY

| 功能 | GET | POST | QUERY |
| --- | --- | --- | --- |
| **安全** | ✅ 是 | ❌ 否 | ✅ 是 |
| **冪等** | ✅ 是 | ❌ 否 | ✅ 是 |
| **允許本文** | ❌ 否 | ✅ 是 | ✅ 是 |
| **可快取** | ✅ 是 | ⚠️ 複雜 | ✅ 是 |
| **可安全重放** | ✅ 是 | ❌ 有風險 | ✅ 是 |
| **請求的 URI** | ✅ 預設 | ❌ 否 | ✅ 可選 |

---

## 結論

`QUERY` 終於為我們提供了一種執行複雜請求的解決方案。這是一個影響很大的「小」變更。這個新方法解決了開發者多年來一直繞道處理的真實問題。

此外，這個方法才剛在最近（2026 年 6 月）標準化，因此真正普及還需要一段時間。

### ⚠️ 部署狀態

- ✅ **已標準化** ：RFC 10008（2026 年 6 月）
- ⏳ **實作** ：目前支援的伺服器／用戶端還很少
- 🚀 **採用時程** ：可能是 2027–2028 年

**在正式環境中使用之前** ，請先等實作更普及。現在最重要的是理解這個概念，並在未來的 API 架構中記住它。

---

## 下一步：實際應用中的 QUERY

這篇文章涵蓋了 QUERY 的理論與關鍵概念。但實際上要怎麼使用它呢？

在未來的文章中，我會探討 **在 REST API 中實作 QUERY 的實務作法** ，展示如何建立支援這個新 HTTP 方法的 REST API，以及如何將它整合到真實世界的架構中。

敬請期待！ 🚀

---

原文出處： [https://dev.to/tykok/the-new-http-method-query-2bec](https://dev.to/tykok/the-new-http-method-query-2bec)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x0nZDa) [61瀏覽](https://codelove.tw/@tony/post/x0nZDa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x0nZDa)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385939&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0nZDa&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385939272&amp;bpp=1&amp;bdt=86&amp;idt=42&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783049968%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783049968%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783049968%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3272169809603&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395661%2C95393485&amp;oid=2&amp;pvsid=2610553342351978&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=72" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>