---
title: "JWT 是什麼？Token 如何攜帶與驗證使用者資訊？ | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4125"
author:
  - "[[赤紅翼零]]"
published: 2026-10-26
created: 2026-10-27
description: "JWT（JSON Web Token）是一種以 JSON 表示並可簽章或加密的 Token 格式，常用於網站與 API 傳遞聲明。本文說明 JWT 的三段結構、簽章不等於加密、如何驗證，以及使用時常見的安全陷阱。"
tags:
  - "clippings"
status: "inbox"
---
登入網站後，伺服器或 API 常需要一種方式，讓後續請求知道「這是誰、允許做什麼」。JWT 是常見的 Token 格式之一，但它並不是加密的身分證，也不是拿到就能直接信任的資料。理解它如何編碼、簽章與驗證，才能避免把登入系統做得方便卻不安全。

## JWT 是什麼？

JWT 是 JSON Web Token 的縮寫，由一套標準定義如何以 JSON 表示一組聲明（claims），並依需求使用數位簽章／訊息驗證碼保護，或加密內容。它是一種 Token 格式，不是完整的登入協定，也不代表系統一定採用 OAuth。

網站有時會在登入成功後簽發 JWT，讓用戶端之後向 API 出示；但 OAuth 2.0 或 OIDC 中使用的 Access Token、ID Token 不一定都是 JWT，也可能是不透明字串。要看發行方與協定文件，不能只因為名稱叫 Token 就推定格式或用途。

## 常見的三段式 JWT 長什麼樣？

最常看到的是以 JWS 緊湊格式表示的簽章 JWT，外觀像這樣：

`xxxxx.yyyyy.zzzzz`

三段以句點分隔，分別是：

1. **Header（標頭）：** 通常包含 Token 類型與簽章演算法等資訊。
2. **Payload（負載）：** 放置聲明，例如使用者識別碼、發行者、對象與有效期限。
3. **Signature（簽章）：** 用來檢查前兩段是否被竄改，以及在適當的簽章方案下確認簽發來源。

前兩段通常只是 Base64URL 編碼，不是加密。任何拿到 Token 的人都可能解碼並讀取 Payload，因此不要把密碼、私密資料或不應外流的個資放進一般簽章 JWT。若使用加密 JWT（JWE），格式與結構不同；JWE 緊湊序列化通常是五段。看到三段式字串，也不代表它已經過驗證。

## 簽章能做什麼，不能做什麼？

簽章可以讓接收端檢查 Token 是否遭修改，並在使用受信任金鑰與正確驗證規則時，確認它由預期的簽發者產生。簽章本身不會隱藏內容，也不會自動保證 Token 沒有過期、適用於目前 API，或對應使用者仍有權限。

例如，伺服器若只把 JWT 解碼，看到 `"role":"admin"` 就放行，攻擊者可能自行改寫內容再送出。必須先以可信任的金鑰驗證簽章，再核對必要聲明和應用程式授權規則。

## JWT 通常如何驗證？

1. **確認預期用途與簽發者：** 接收端應知道自己要接受哪個 issuer、哪類 Token；不能把不同服務或用途的 Token 混用。
2. **安全地選擇金鑰與演算法：** 伺服器依事先設定或可信任來源取得金鑰，並只允許預期的演算法。不要盲目採信 Token Header 中的演算法、金鑰網址或金鑰識別資訊。
3. **驗證簽章：** 依指定演算法和正確金鑰驗證簽章；失敗就拒絕，不要降級成「略過驗證」。
4. **檢查聲明：** 依服務規則驗證 `iss` （簽發者）、 `aud` （接收對象）、 `exp` （到期時間），需要時也檢查 `nbf` （生效時間）、 `iat` （簽發時間）與 `sub` （主體）。不是每個欄位都必須出現在每種 JWT；應依協定和用途明確定義要求。
5. **再檢查本站權限：** Token 身分有效，不代表使用者當下仍有權限執行所有操作。仍要按照本站的角色、資源歸屬和商業規則授權。

## JWT 和 Session 有什麼不同？

傳統 Session 常由伺服器保存登入狀態，用戶端只持有 Session ID；JWT 常把部分聲明放在可由接收端驗證的 Token 中。JWT 可能方便多個服務在共同信任規則下驗證，但也帶來金鑰管理、撤銷與資料更新等問題。採用 JWT 不會自動讓系統更安全，也不一定比 Session 簡單。

JWT 一旦簽發，在到期前可能持續有效。若使用者登出、帳號停權或權限改變，系統要有相應策略，例如縮短存取 Token 期限、撤銷或輪替憑證，或在必要時查詢伺服器端狀態。若每次請求都必須即時撤銷，完全無狀態的設計可能不適合。

## 實作時常見的安全陷阱

- **把 Base64URL 當成加密：** 一般簽章 JWT 的 Payload 可被解碼閱讀，不要放入機密。
- **只解碼、不驗簽：** 解碼只代表讀得到內容，不代表內容可信。
- **不檢查 issuer、audience 或有效期：** 可能讓為其他服務簽發的 Token 被錯誤接受。
- **演算法設定過於寬鬆：** 依安全指引設定允許的演算法，不要由未受信任的 Token 自行決定驗證方式。
- **把長效 Token 暴露在用戶端：** Token 若被竊取，可能被重放。依應用架構選擇安全儲存方式，搭配 HTTPS、適當的有效期限與權限範圍，也避免把 Token 寫進網址或日誌。
- **忽略權限變動與登出：** 規劃 Token 到期、撤銷、金鑰輪替與帳號停用後的處理方式。

## 總結

JWT 是一種攜帶聲明的 Token 格式。常見三段式 JWT 由 Header、Payload 和 Signature 組成；Base64URL 只是編碼，簽章用來驗證完整性，不等於加密。伺服器必須依可信任的金鑰和演算法驗證簽章，再檢查 issuer、audience、期限與本站權限，不能只解碼內容就放行。

<iframe frameborder="0" src="https://890fc43d0eb6a2e964a444507fd510f5.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe><iframe frameborder="0" src="https://890fc43d0eb6a2e964a444507fd510f5.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>