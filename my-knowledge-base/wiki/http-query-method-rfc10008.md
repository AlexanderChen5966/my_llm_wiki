---
title: HTTP QUERY 方法（RFC 10008）：帶 body 的安全冪等 GET，含 Hono+Bun 實作
date: 2026-07-09
tags: [dev/architecture, type/reference]
source: 合併自 https://codelove.tw/@tony/post/x0nZDa（原文 dev.to/tykok）與 https://codelove.tw/@tony/post/x8OJZq（原文 qiita.com/maaaashi）
type: article
status: processed
asset_value: 50
---

2026-06 標準化的新 HTTP 方法 `QUERY`（RFC 10008），是自 `PATCH`（2010）以來 16 年首個新標準 HTTP 動詞，定位為「可帶 request body 的安全版 GET」——解決複雜查詢條件塞不進 URL、又不想用非冪等 `POST` 的長年痛點。

## T（核心論點）

`GET` 只能靠 URL query string 傳條件（有長度限制、暴露在 log/URL 裡、無法表達巢狀結構）；改用 `POST` 又犧牲了冪等性與快取能力（網路中斷不能安全重送、代理伺服器不知道能不能安全重試）。`QUERY` 同時具備 `GET` 的「安全、冪等、可快取」與 `POST` 的「可用 body 傳遞複雜資料」，專為搜尋類端點設計，讓 `GET /api/users/active`、`GET /api/users/inactive`、`GET /api/users/premium` 這類分裂端點可以收斂成一個 `QUERY /api/users`。

## B（適用場景）

設計需要複雜篩選/搜尋條件的 REST API 端點；評估是否該用 `POST /resource/search` 這種語意不協調的 workaround；理解 HTTP 標準的最新演進。**注意部署現狀**：2026-06 才標準化，伺服器/用戶端實作還很少，預估採用時程在 2027-2028 年，**正式環境使用前應等待實作更普及**，目前建議與 `POST` 並用。

## R（重點整理）

**GET / POST / QUERY 特性對照**：

| 特性 | GET | POST | QUERY |
|------|-----|------|-------|
| 不改變資源狀態（安全）| ✅ | ❌ | ✅ |
| 冪等 | ✅ | ❌ | ✅ |
| 可用 request body | ❌（規範上不應賦予意義）| ✅ | ✅ |
| 可快取 | ✅ | ⚠️ 複雜 | ✅ |
| 可安全重放 | ✅ | ❌ 有風險 | ✅ |
| 請求的 URI（可存成永久網址重放）| ✅ 預設 | ❌ | ✅ 可選 |

**「請求的 URI」是 QUERY 的獨特能力**：傳統 POST 回傳的是「結果的 URI」（`Content-Location: /results/xyz789`，指向這次查詢結果快照）；QUERY 可回傳「請求的 URI」（`Location: /saved-queries/42`），重放這個網址會用**最新資料**重新執行同一個查詢——這讓「分享查詢連結」「將搜尋加入書籤」「定期重放而不用每次重送 body」成為可能。

**Hono + Bun 實作要點**：
- 路由用 `app.on("QUERY", path, handler)` 註冊（Hono 的通用 method 註冊介面，非新增的專屬方法）
- `OPTIONS` 端點應回傳 `Allow: QUERY, OPTIONS` + `Accept-Query: application/json`（類似 `Accept-Post` 的 QUERY 版本），讓客戶端能先確認端點是否支援
- Content-Type 非 JSON → 回 415；body 不符合 schema（用 zod 驗證）→ 回 400
- `curl -X QUERY` 可直接測試——curl 本質上把 method 名稱當字串送出，不需要框架特別支援新方法即可呼叫

```bash
curl -s -X QUERY localhost:3000/blogs \
    -H "Content-Type: application/json" \
    -d '{"query":"入門","limit":2}'
```

## C（關鍵行動）

- [ ] 設計搜尋類 API 端點時，把 QUERY 方法納入長期架構選項評估，但短期內仍搭配 POST 並行
- [ ] 正式環境暫緩直接依賴 QUERY 方法，先確認目標平台（瀏覽器/CDN/Proxy/API Gateway）的實作進度
- [ ] 用 Hono 等支援任意 method 註冊的框架時，記得補上 `OPTIONS` 回傳 `Allow`/`Accept-Query`，讓客戶端探測支援狀況

## 相關連結

（暫無相關條目，HTTP 標準演進首篇記錄）
