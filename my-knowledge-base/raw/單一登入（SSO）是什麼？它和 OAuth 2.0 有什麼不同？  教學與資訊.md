---
title: "單一登入（SSO）是什麼？它和 OAuth 2.0 有什麼不同？ | 教學與資訊"
source: "https://tools.wingzero.tw/articles/4126"
author:
  - "[[赤紅翼零]]"
published: 2026-10-27
created: 2026-10-27
description: "單一登入（SSO）讓使用者透過一個身分提供者登入多個相關服務；OAuth 2.0 則是授權框架，兩者不是同一件事。本文說明 SSO 的運作方式、OIDC 與 SAML 的角色，以及常見的安全與帳號管理考量。"
tags:
  - "clippings"
status: "inbox"
---
每天早上先登入公司入口網站，接著打開郵件、工時系統和雲端文件，不必每個服務都重新輸入帳號密碼——這就是單一登入（Single Sign-On，SSO）帶來的體驗。不過，SSO、OAuth 2.0 與「使用 Google 登入」常被放在一起談，實際上它們並不是同一種東西。

## SSO 是一種登入體驗與架構

SSO 的目標是讓使用者在一個信任架構中完成身分驗證後，能使用多個相關應用程式，而不必在每個服務分別登入。負責驗證帳號的系統通常稱為身分提供者（Identity Provider，IdP）；需要確認使用者身分的應用程式則可稱為服務提供者（Service Provider，SP），在 OIDC 語境中常稱為 Relying Party（RP）。

登入時，應用程式將使用者導向 IdP。IdP 驗證帳號，可能再要求多因素驗證；之後透過標準化的回應向應用程式證明使用者身分。應用程式驗證回應後，仍會建立自己的登入狀態。使用者再開啟另一個已信任同一 IdP 的應用程式時，IdP 可能已經有有效登入狀態，因此不必再次輸入密碼。

因此，SSO 並不是把同一組密碼傳給每個網站，也不代表所有服務共用同一個瀏覽器 Session。每個應用程式通常仍有自己的 Session、權限與登出期限。

## OAuth 2.0 解決的是授權，不是單一登入

OAuth 2.0 是授權框架，讓使用者可以允許某個用戶端在限定範圍內存取資源。例如，照片服務的使用者可以授權另一個應用程式讀取特定相簿，而不用交出照片服務的密碼。

OAuth 2.0 本身沒有定義一套通用的使用者登入身分資料。如果網站要用 Google 等身分提供者確認「登入的人是誰」，常見做法是在 OAuth 2.0 之上使用 OpenID Connect（OIDC）。OIDC 定義身分驗證層，並提供 ID Token 等機制。

可以這樣記：SSO 描述「登入一次後，可順暢進入多個服務」的體驗或架構；OAuth 2.0 描述「使用者授權應用程式存取哪些資源」；OIDC 則補上「使用者是誰」的身分驗證。常見的「使用 Google 登入」會用 OIDC，但單一使用 Google 登入按鈕，不一定就代表企業多應用程式的 SSO。

## SSO 常見的標準：SAML 與 OIDC

SSO 不是單一傳輸協定，常見實作包括：

- **SAML 2.0：** 常見於企業與組織的瀏覽器式 SSO。IdP 會產生帶有身分聲明的 SAML Response，應用程式依信任設定驗證簽章、發行者、接收對象、期限等條件，再建立登入狀態。SAML 訊息通常以 XML 表示。
- **OpenID Connect（OIDC）：** 建立在 OAuth 2.0 之上，常見於新式 Web、行動應用程式與 API 生態。應用程式透過 OIDC 流程取得並驗證 ID Token，以確認登入者身分；若還要呼叫外部 API，則另外依授權取得相應的 Access Token。

實際選擇要看 IdP、應用程式支援、組織政策與整合需求，不能只用「哪個比較新」來決定。

## 一個簡化的 SSO 流程

1. 使用者開啟應用程式 A；A 發現使用者尚未登入，便將瀏覽器導向組織的 IdP。
2. IdP 驗證使用者，並依政策執行多因素驗證或其他檢查。SAML 流程會向應用程式傳送 SAML Response；OIDC 授權碼流程則把 authorization code 導回應用程式，讓應用程式向 Token 端點交換 ID Token 與所需的 Access Token。
3. 應用程式驗證回應的簽章、發行者、對象、期限及流程關聯資訊，通過後建立自己的 Session。
4. 使用者接著開啟應用程式 B。若 B 也信任同一 IdP，且 IdP 的登入狀態仍有效，使用者可能不必再輸入密碼；B 仍會建立自己的 Session，並按自身規則檢查權限。

SSO 的核心是各方事先建立信任關係與正確驗證回應，不是單純把某個 Token 複製到不同網站使用。

## SSO 和 OAuth 2.0 的差異整理

- **關注問題：** SSO 關注使用者如何跨多個應用程式登入；OAuth 2.0 關注用戶端獲准存取哪些受保護資源。
- **典型角色：** SSO 常有 IdP 與多個應用程式；OAuth 2.0 常有使用者、用戶端、授權伺服器與資源伺服器。
- **常見標準：** SSO 可使用 SAML 或 OIDC；OAuth 2.0 是授權框架，OIDC 則是在其上增加身分驗證。
- **實務例子：** 公司員工登入一次後開啟郵件與人資系統，是 SSO 體驗；允許行事曆工具讀取使用者的 Google Calendar，是 OAuth 授權；點選「使用 Google 登入」通常是 OIDC 身分驗證。

## SSO 的好處與安全考量

SSO 可以減少重複登入與密碼管理負擔，也讓組織更容易集中執行多因素驗證、登入政策和帳號停用流程。不過，IdP 同時成為重要的信任中心：若 IdP 帳號或管理設定遭入侵，影響可能擴及多個服務。

- 為 IdP 啟用多因素驗證，優先採取符合組織風險的強驗證方式。
- 只授權必要的應用程式與資料，並定期檢查仍有效的整合與權限。
- 使用者離職或帳號停用時，應同步撤銷應用程式存取；不能只依賴使用者自行登出。
- 正確驗證 SAML Response 或 OIDC Token 的簽章、issuer、audience、有效期與流程關聯值；不要只解碼內容就相信。
- 設計好 Session 期限、重新驗證與登出行為。從一個應用程式登出，不一定會同時登出 IdP 或所有其他應用程式；全域登出需要相關系統支援並妥善設定。
- 保護 IdP 管理者帳號與金鑰、憑證、回呼網址等整合設定，並監控異常登入和授權活動。

## 總結

SSO 是讓使用者透過一個身分提供者登入多個信任服務的架構與體驗；它可透過 SAML 或 OIDC 等標準實作。OAuth 2.0 的核心是授權存取資源，不等同於 SSO，也不單獨定義登入身分；「使用 Google 登入」通常使用 OIDC。理解這些差異，才能在整合登入、API 授權和網站權限時採用正確的安全流程。

<iframe frameborder="0" src="https://b596e04ecbc49d87ab983eec7ecbbd49.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe><iframe frameborder="0" src="https://b596e04ecbc49d87ab983eec7ecbbd49.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>