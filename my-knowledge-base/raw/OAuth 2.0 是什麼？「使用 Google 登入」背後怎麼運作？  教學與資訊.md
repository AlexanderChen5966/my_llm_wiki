---
title: "OAuth 2.0 是什麼？「使用 Google 登入」背後怎麼運作？ | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4124"
author:
  - "[[赤紅翼零]]"
published: 2026-10-26
created: 2026-10-27
description: "OAuth 2.0 是讓應用程式在使用者授權下取得有限資源存取權的框架；「使用 Google 登入」通常會搭配 OpenID Connect（OIDC）確認身分。本文拆解兩者分工、授權碼流程、Access Token 與 ID Token 的差異，以及 state、PKCE 等安全機制。"
tags:
  - "clippings"
status: "inbox"
---
許多網站都有「使用 Google 登入」按鈕。按下之後，使用者通常會被帶到 Google 確認帳號，再回到原網站；網站並沒有取得你的 Google 密碼。這個過程背後常同時用到 OAuth 2.0 與 OpenID Connect（OIDC），但兩者解決的問題並不相同。

## OAuth 2.0：代表使用者授權存取資源

OAuth 2.0 是一套授權框架，讓使用者可以允許某個應用程式，在限定範圍內存取受保護的資源。例如，照片服務可以授權一個編輯工具讀取指定相簿，而不必把帳號密碼交給該工具。

流程中常見的角色包括：使用者（資源擁有者）、想取得授權的應用程式（用戶端）、負責登入與核發授權的 Google（授權伺服器），以及提供資料或 API 的服務（資源伺服器）。使用者可以同意或拒絕要求的權限；應用程式取得的權限應限制在核准的範圍內。

## 「登入」為什麼還會用到 OIDC？

OAuth 2.0 的核心目標是授權存取，並沒有定義一套通用的使用者身分驗證結果。若網站要確認「現在登入的是哪個 Google 使用者」，通常會使用建立在 OAuth 2.0 之上的 OpenID Connect（OIDC）。OIDC 定義了身分識別資訊與 ID Token 等機制。

簡單記：OAuth 2.0 著重「這個應用程式獲准做什麼」；OIDC 著重「使用者是誰」。因此，口語上的「Google OAuth 登入」，在現代常見架構中更精確地說是「使用 Google 的 OIDC 登入流程」，而底層授權流程使用 OAuth 2.0。

## 使用 Google 登入，大致會經過哪些步驟？

1. **網站準備登入請求：** 網站產生一次性 `state` ，並在適用的用戶端使用 PKCE 產生 verifier 與 challenge，接著把瀏覽器導向 Google。請求會帶上用戶端識別碼、預先登記的回呼網址、回應類型與所需範圍；OIDC 登入範圍通常包含 `openid` ，也可能有 `profile` 、 `email` 。
2. **使用者在 Google 驗證並授權：** 使用者在 Google 頁面登入，檢視並決定是否同意網站要求的權限。密碼是在 Google 的頁面輸入，不應交給第三方網站。
3. **Google 導回網站：** 同意後，Google 導回已登記的回呼網址，並附上短效的一次性授權碼與原先的 `state` 。網站先比對 `state` ，避免登入流程遭到跨站請求偽造或被錯置。
4. **網站交換授權碼：** 網站把授權碼送到 Google 的 Token 端點換取 Token；使用 PKCE 的流程還會提交 verifier。伺服器端的機密用戶端也會依流程驗證用戶端身分。手機或瀏覽器等公開用戶端不能把 client secret 當成秘密寫進應用程式。
5. **驗證身分並建立本站登入：** OIDC 流程可能回傳 ID Token。網站必須依規格驗證簽章與必要欄位，例如簽發者、預期的 audience、有效期限，以及流程中使用的 nonce；通過驗證後，再建立自己網站的登入 Session。

## Access Token 和 ID Token 有什麼不同？

- **Access Token：** 用來向特定資源伺服器呼叫 API，能做什麼取決於核准的 scope 與服務規則。它不是拿來當作使用者身分證明的通用資料。
- **ID Token：** OIDC 用來向用戶端表達驗證結果的 Token，包含使用者識別資訊及簽發、對象、期限等聲明。網站應驗證它，而不是只把內容解碼後就當成可信資料。
- **Refresh Token：** 若流程與授權條件允許，可能用來取得新的 Access Token。它通常效力較長，應以更嚴格方式保護；不需要離線存取時就不要索取。

Token 常是持有者憑證：拿到的人可能就能使用它所代表的權限。不要把 Token 放進網址、公開程式碼、錯誤訊息或日誌，也不要將 ID Token 當成呼叫 Google API 的 Access Token。

## Google 認證，不等於本站所有權限都已確認

Google 回報某個帳號完成驗證，只能協助網站確認外部身分。網站仍要決定這個身分是否對應既有本站帳號、是否允許建立帳號，以及使用者在本站能讀取或修改哪些資料。角色與功能權限應由本站自己的授權規則判斷，不能因為「已用 Google 登入」就一律給予管理權限。

帳號綁定也要小心：不要只憑未驗證或來源不明的電子郵件字串，就把 Google 身分合併到既有帳號。應依 OIDC 驗證結果和本站明確的帳號連結流程處理。

## 實作時要注意的安全重點

- 使用 HTTPS，並精確限制已登記的回呼網址；不要接受任意導向網址。
- 每次登入流程都驗證不可預測且一次性的 `state` ；使用 OIDC 時也妥善處理 `nonce` 。
- 採用授權碼流程並使用 PKCE，避免授權碼被攔截後遭冒用；不要把 client secret 放在瀏覽器或手機 App。
- 只要求目前功能真正需要的最小 scope，並清楚向使用者說明用途。
- 妥善保護 Token、限制保存時間與存取權，並避免寫入日誌；登入成功後建立本站自己的 Session。
- 不要自行設計流程去收取使用者的 Google 密碼，也不要只解碼 Token 就相信其內容。

## 總結

「使用 Google 登入」不是把 Google 密碼交給網站，而是讓 Google 負責驗證帳號，再透過安全的授權流程把結果交回網站。OAuth 2.0 提供授權框架，OIDC 補上身分驗證層；網站驗證回傳資料後，仍須建立本站 Session，並自行執行本站的權限檢查。

<iframe frameborder="0" src="https://0a73876efa482b8d7151737069ec27cc.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe><iframe frameborder="0" src="https://0a73876efa482b8d7151737069ec27cc.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>