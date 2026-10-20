---
title: "Cookie、Session、Token 是什麼？網站如何記住登入狀態 | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4092"
author:
  - "[[赤紅翼零]]"
published: 2026-10-08
created: 2026-10-08
description: "Cookie、Session 與 Token 常一起出現在登入流程中，卻不是同一種東西。本文用實際請求流程說明三者的差異，以及網站如何安全地記住登入狀態。"
tags:
  - "clippings"
status: "inbox"
---
登入網站後切換頁面，不必每次都重新輸入帳號密碼；隔天再回到某些網站，甚至仍然保持登入。這些服務如何記得「剛才登入的人是誰」？

答案通常跟 Cookie、Session 或 Token 有關。它們常在同一個登入流程裡合作，但各自扮演不同角色：Cookie 是瀏覽器保存及傳送資料的機制；Session 是網站維持使用者狀態的方法；Token 則是一種可用來表達身分或授權的憑證。

## 為什麼網站需要記住登入狀態？

HTTP 請求本身通常彼此獨立。使用者登入時送出帳號密碼，伺服器驗證成功後，如果沒有其他機制，下一次請求不會自動帶著「這是剛才那位使用者」的資訊。

網站必須在登入成功後，建立一種能辨認後續請求的方式。Cookie、Session 與 Token 就是常見做法。HTTP Cookie 是伺服器要求瀏覽器保存少量資料，並在符合條件的後續請求中送回的機制。

## Cookie 是什麼？

Cookie 是瀏覽器保存的一小段資料。伺服器可以在回應中透過 `Set-Cookie` 要求瀏覽器儲存 Cookie；之後瀏覽器會依照網域、路徑、安全屬性等規則，在請求中附上 Cookie。

```
HTTP/1.1 200 OK
Set-Cookie: session_id=random-value; Path=/; Secure; HttpOnly; SameSite=Lax
```

後續請求可能會帶上：

```
GET /account HTTP/1.1
Cookie: session_id=random-value
```

Cookie 不一定代表登入狀態。網站也可能用它記住語言偏好、購物車或其他設定。即使 Cookie 裡放的是登入憑證，它也只是瀏覽器保存與傳送資料的方式；伺服器仍須判斷這份資料是否有效。

## Session 是什麼？

Session（工作階段）是一種讓網站把多次請求關聯到同一位使用者的方法。常見的伺服器端 Session 流程如下：

1. 使用者登入，伺服器驗證帳號密碼。
2. 伺服器建立 Session，並產生難以猜測的 Session ID。
3. 伺服器把 Session ID 放進 Cookie，交給瀏覽器保存。
4. 瀏覽器之後自動附上 Cookie；伺服器用 Session ID 找到對應的登入狀態。

這種設計下，Cookie 裡通常是識別 Session 的值，而使用者資料與登入狀態由伺服器管理。Session ID 本身仍是敏感憑證：別人如果取得有效的 ID，可能就能冒用該登入狀態。因此 Session ID 應使用安全的亂數產生、透過 HTTPS 傳送，並在登出或逾期時由伺服器使其失效。 [OWASP：Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

另外，「Session Cookie」也常指沒有設定 `Expires` 或 `Max-Age` 的 Cookie。這描述的是瀏覽器 Cookie 的保存期限，不等於伺服器端的 Session 必定在關閉瀏覽器時結束；瀏覽器的工作階段還原功能也可能保留原本的工作階段 Cookie。因此，登入逾期與登出仍應由伺服器妥善管理。 [OWASP：Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

## Token 是什麼？

Token（權杖）是系統核發的資料或憑證，可用來表示使用者身分、授權範圍或其他資訊。用戶端呼叫 API 時，常見作法是把 Token 放在 `Authorization` 標頭：

```
GET /api/profile HTTP/1.1
Authorization: Bearer example-token
```

Bearer Token 的特性是：持有它的人就可能使用它所授予的權限。因此，Token 必須當作密碼一樣保護，使用 HTTPS 傳輸，避免放進網址或日誌，也不應賦予超出需求的權限。 [RFC 6750：OAuth 2.0 Bearer Token Usage](https://www.rfc-editor.org/rfc/rfc6750.html)

JWT（JSON Web Token）是常見的 Token 格式之一，但 Token 不等於 JWT。JWT 可以包含以 JSON 表示的 Claims，並依規格進行簽章或加密；常見的簽章 JWT 不代表內容已加密，解碼後的內容可能可以讀取，所以不應把密碼或其他機密直接放進去。 [RFC 7519：JSON Web Token](https://www.rfc-editor.org/rfc/rfc7519.html)

## Cookie、Session、Token 有什麼不同？

- **Cookie：** 瀏覽器保存、傳送資料的機制，常隨符合條件的 HTTP 請求送到網站。
- **Session：** 網站維持多次請求之間使用者狀態的方法，常由伺服器管理，瀏覽器保存 Session ID。
- **Token：** 表示身分或授權的憑證或資料，常由用戶端在 API 請求中提供。

三者不是互斥選項。網站完全可能把 Session ID 放在 Cookie 裡；API 用的 Token 也可能由 Cookie 保存。只看到 Cookie 或 Token 這個名稱，還不足以判斷整個登入架構。

## Cookie 登入有哪些安全設定？

如果網站使用 Cookie 保存 Session ID，常見安全設定包括：

- `Secure` ：Cookie 只透過 HTTPS 傳送。
- `HttpOnly` ：JavaScript 無法透過 `document.cookie` 讀取 Cookie，可降低部分 XSS 情境下憑證被直接竊取的風險。
- `SameSite` ：控制 Cookie 是否隨跨站請求送出，有助於降低 CSRF 風險；設定為 `None` 時通常必須同時搭配 `Secure` 。

這些設定各自處理不同風險。 `HttpOnly` 不代表 XSS 無法以受害者身分操作網站； `SameSite` 也不應被當成所有 CSRF 防護的替代品。伺服器仍須妥善驗證請求、設定 Session 期限，並在登出時使 Session 失效。 [MDN：安全 Cookie 設定](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/Cookies) 、 [OWASP：Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

## 使用 Token 就一定比較安全嗎？

不一定。安全性取決於 Token 怎麼簽發、保存、傳送與撤銷，而不是只看它叫不叫 Token。

例如，把 Token 存在可供 JavaScript 讀取的位置，若網站發生 XSS，惡意腳本可能有機會讀取它。若把 Token 放在 Cookie 中，瀏覽器可能會自動隨請求傳送，則仍須注意 CSRF 等風險。不同架構有不同取捨，應依網站用途與威脅模型設計。

無論採用哪種方式，都應限制憑證的有效期限與權限、透過 HTTPS 傳送、避免把憑證放進網址，並在登出或偵測到風險時讓憑證失效。

## 簡單總結

Cookie 是瀏覽器保存和傳送資料的機制；Session 是網站維持使用者狀態的方法；Token 是可用來表示身分或授權的憑證。常見的登入流程會把 Session ID 放進 Cookie，讓瀏覽器在後續請求中帶回；API 則常透過 `Authorization` 標頭接收 Token。

理解這三個概念後，就能看懂網站如何記住登入狀態，也能更清楚判斷 Cookie、Session ID 與 JWT 為什麼都必須受到妥善保護。

<iframe frameborder="0" src="https://3b14c56554cbaa8c3511ce54257ddf6a.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>