---
title: Web 登入與授權基礎：Cookie／Session／Token、OAuth 2.0＋OIDC、JWT 驗證
date: 2026-11-19
tags: [dev/architecture, type/concept]
source: https://tools.wingzero.tw/articles/4124（DeTools 工具翼零／赤紅翼零〈OAuth 2.0 是什麼？「使用 Google 登入」背後怎麼運作？〉，2026-10-26）
source_2: https://tools.wingzero.tw/articles/4125（同系列〈JWT 是什麼？Token 如何攜帶與驗證使用者資訊？〉，2026-10-26）
source_3: https://tools.wingzero.tw/articles/4092（同系列〈Cookie、Session、Token 是什麼？網站如何記住登入狀態〉，2026-10-08；文中引 OWASP Session Management Cheat Sheet、RFC 6750、RFC 7519、MDN）
type: article
status: processed
asset_value: 70
---

同一作者的三篇入門教學合併成一條：網站怎麼記住登入（Cookie／Session／Token）、「使用 Google 登入」背後的 OAuth 2.0＋OIDC、以及 JWT 怎麼驗證。三篇是**觀念整理**，沒有程式碼或實測；文中規格引用（RFC、OWASP）本庫未逐條回原文核對。文末廣告 iframe 已忽略。

## T（核心論點）

> **「讀得到」不等於「可信」，「外部身分已驗證」不等於「本站已授權」。**

三個分層要分開想：

1. **Cookie／Session／Token 是三種不同層次的東西**：Cookie 是瀏覽器**保存與傳送**資料的機制；Session 是網站**維持狀態**的方法（常把 Session ID 放進 Cookie）；Token 是**表達身分或授權的憑證**（常放 `Authorization: Bearer`）。三者不互斥，只看名稱判斷不了整個登入架構
2. **OAuth 2.0 管「這個 App 獲准做什麼」，OIDC 管「使用者是誰」**：口語的「Google OAuth 登入」精確說是「Google 的 OIDC 登入流程，底層用 OAuth 2.0 授權碼流程」
3. 🔑 **JWT 的 Base64URL 只是編碼、簽章不是加密**：只解碼就相信 `"role":"admin"` 等於讓攻擊者自己改權限；驗簽＋驗聲明（`iss`／`aud`／`exp`…）之後，**還要再跑本站自己的權限規則**

## B（適用場景）

**適用：**
- 接第三方登入（Google、公司 auth server）時，釐清前端、後端、授權伺服器各自負責什麼
- 判斷某個 Token 該怎麼存、怎麼驗、多久過期、登出／停權時怎麼辦
- 讀別人的登入架構時，先分清「哪個是 Cookie、哪個是 Session ID、哪個是 Access Token／ID Token」

**不適用 / 注意：**
- 三篇是入門觀念，不是安全稽核清單；正式實作以 RFC、OWASP 與所用身分服務的官方文件為準
- Access Token、ID Token **不一定是 JWT**，也可能是不透明字串，要看發行方文件
- 公開用戶端（瀏覽器 SPA、手機 App）**不能把 client secret 當秘密**——這也是 SPA 與手機 App 要用授權碼＋PKCE 的原因

## R（操作步驟）

### 1. 網站怎麼記住登入（Cookie／Session／Token）

- 伺服器端 Session：登入驗證成功 → 產生難猜的 Session ID → `Set-Cookie` 交給瀏覽器 → 之後瀏覽器自動帶回，伺服器用 ID 找狀態。**Session ID 本身就是敏感憑證**，登出或逾期要由伺服器讓它失效
- 「Session Cookie」另指沒設 `Expires`／`Max-Age` 的 Cookie，描述的是瀏覽器保存期限，不保證伺服器端 Session 關瀏覽器就結束（瀏覽器的工作階段還原也可能保留它）
- Cookie 安全屬性各管一種風險，**不能互相替代**：

| 屬性 | 作用 | 限制 |
|---|---|---|
| `Secure` | 只走 HTTPS | — |
| `HttpOnly` | JS 讀不到 `document.cookie`，降低 XSS 直接偷憑證 | XSS 仍能以受害者身分操作網站 |
| `SameSite` | 控制跨站請求是否帶 Cookie，降低 CSRF | 不是 CSRF 防護的全部；`None` 須搭 `Secure` |

- Token 不一定比較安全：存在 JS 可讀的位置怕 XSS，存在 Cookie 又要顧 CSRF；**依威脅模型取捨**。Bearer Token 拿到就能用，當密碼保護：HTTPS、不進網址與日誌、最小權限

### 2. 「使用 Google 登入」的授權碼流程（OAuth 2.0＋OIDC）

1. 網站產生一次性 `state`（防 CSRF／錯置），適用時產生 **PKCE** verifier＋challenge，把瀏覽器導向 Google，帶用戶端 ID、**預先登記**的回呼網址、scope（OIDC 含 `openid`，可能加 `profile`、`email`）
2. 使用者在 **Google 的頁面**輸入密碼並決定是否同意
3. Google 導回回呼網址，附**短效一次性授權碼**＋原 `state`；網站先比對 `state`
4. 網站拿授權碼到 Token 端點換 Token，PKCE 流程一併送 verifier；機密用戶端（伺服器端）另驗用戶端身分
5. 驗證 ID Token（簽章、`iss`、`aud`、期限、`nonce`）→ **建立本站自己的 Session**

三種 Token 不要混用：

| Token | 用途 | 注意 |
|---|---|---|
| Access Token | 向資源伺服器呼叫 API，能做什麼看 scope | 不是通用的身分證明 |
| ID Token | OIDC 向用戶端表達「誰登入了」 | 要驗，不能只解碼；不能拿去呼叫 Google API |
| Refresh Token | 換新的 Access Token | 效力長、要更嚴格保護；不需離線存取就別要 |

**登入之後仍是本站的事**：Google 只確認外部身分；這個身分對應哪個本站帳號、能不能自動建帳號、能讀寫哪些資料，由本站授權規則決定。**不要只憑未驗證的 email 字串就把 Google 身分併到既有帳號**。

### 3. JWT 怎麼驗

- 結構：JWS 緊湊格式 `header.payload.signature` 三段（JWE 加密格式通常五段）；前兩段只是 Base64URL，**誰拿到都能解碼**，不放密碼與機密個資
- 驗證五步：
  1. 確認預期用途與簽發者，不同服務／用途的 Token 不混收
  2. 金鑰與演算法**由伺服器事先設定**，不盲信 Header 裡的演算法、金鑰網址或金鑰 ID
  3. 驗簽，失敗就拒絕，**不降級成略過驗證**
  4. 依協定檢查 `iss`、`aud`、`exp`，需要時 `nbf`、`iat`、`sub`
  5. 再跑本站的角色、資源歸屬與商業規則
- JWT vs Session：JWT 方便多服務在共同信任規則下各自驗證，但帶來**金鑰管理、撤銷、資料更新**問題；簽發後到期前可能一直有效 → 用短效存取 Token、撤銷／輪替，或必要時查伺服器端狀態。**每次請求都要即時撤銷的系統，完全無狀態不一定適合**

## C（關鍵行動）

- [ ] 檢查手上專案的登入流程：`state`、PKCE、回呼網址白名單、ID Token 驗證四項是否都有
- [ ] 確認後端驗 JWT 時演算法是寫死允許清單，而不是讀 Token Header
- [ ] 列出登出／停權／權限變更後，既有 Token 最長還能用多久

## 原始摘要

DeTools 工具翼零三篇入門教學：〈Cookie、Session、Token〉說明網站記住登入狀態的三個概念與 Cookie 安全屬性；〈OAuth 2.0〉拆解「使用 Google 登入」的 OAuth 2.0＋OIDC 分工、授權碼流程、三種 Token 與 `state`／PKCE；〈JWT〉說明三段結構、簽章不等於加密、五步驗證與常見陷阱。共同結論：憑證要當密碼保護、解碼不等於可信、外部身分通過後仍要做本站授權。

**合併判斷（2026-11-19）**：既有 auth 相關條目都是特定情境（MCP 伺服器的 OAuth 探測、代理身分 OIDC、Claude 連接器授權），沒有通用的 Web 登入基礎教學，因此合併三篇新建。同系列〈單一登入（SSO）是什麼？它和 OAuth 2.0 有什麼不同？〉仍在 inbox，日後可補進本條。

## 相關連結

- [[mcp-server-stateless-streamable-http-oauth]] — MCP 伺服器實作中的 OAuth 探測；本條是其底層的 OAuth／Token 觀念
- [[agentmail-agentid-agent-identity-oidc]] — 代理以 OIDC 取得獨立身分；身分層之後的授權同樣要另做
- [[claude-connector-auth-and-long-task-tasks-elicitations]] — 連接器授權的產品面案例
- [[agent-tool-authorization-gatekeeper]] — 「可達性≠授權」的代理版，對應本條「外部身分已驗證≠本站已授權」
- [[mcp-stateless-architecture]] — 無狀態架構「每次請求自帶內容」，與 JWT 無狀態驗證的撤銷取捨相通
