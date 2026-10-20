---
title: "Cloudflare DNS 設定教學：如何用免費版 (Free Plan) 代管網域、改 Nameserver 與啟用基本防護"
source: "https://blog.cloudmax.com.tw/cloudflare-dns-free-plan/"
author:
  - "[[Cherry Lan]]"
published: 2026-03-09
created: 2026-10-06
description: "Cloudflare 免費方案（Free Plan）中，提供了 DNS 代管（Cloudflare DNS） 等基礎功能，讓大家可以把網域的解析交由 Cloudflare 管理，同時也能視需求啟用 Proxy/CDN 與 SSL/TLS 等基本防護設定。"
tags:
  - "clippings"
status: "inbox"
---
DNS 設定是許多網站管理者在「網站要上線、要搬家、要接第三方服務」時最常卡關的一步。原因很簡單：DNS 就像網域的「總機」，它決定使用者輸入網址時要被導向哪台主機、信件要交給哪個郵件伺服器、以及 CDN/防護服務要如何介入。只要 DNS 或 Nameserver 設錯，輕則解析延遲、設定看似完成卻不生效，重則網站打不開、SSL 憑證異常、信件收發中斷等問題，影響往往立刻發生，卻又不容易第一時間定位原因。

Cloudflare 免費方案（Free Plan）中，提供了 **DNS 代管（Cloudflare DNS）** 等基礎功能，讓大家可以把網域的解析交由 Cloudflare 管理，同時也能視需求啟用 Proxy/CDN 與 SSL/TLS 等基本防護設定。

本文以 Cloudflare DNS 為範例，採用「新手照做就能成功」的方式，帶您走一遍 DNS 設定的核心流程：從加入網域、切換 Nameserver、檢查/匯入 DNS 紀錄（A / CNAME），到完成生效檢查與常見問題排除。完成後，即使你未來改用其他 DNS 服務商，這套觀念與流程也能直接沿用。

## ▌Cloudflare 介紹

Cloudflare 是一個雲端網路與資安平台，提供 DNS 代管與全球邊緣節點（Edge Network）服務，常被用來管理網域解析、改善連線體驗，並在流量進入網站前提供基本的安全防護與流量處理機制；Cloudmax 匯智所提供的 Cloudmax M-WAF（Managed Web Application Firewall） [CF-Shield 方案](https://www.cloudmax.com.tw/product/cf-shield-cloudflare) ，就是建構於 Cloudflare 的基礎設施之上。

以 Cloudflare 進階資安與流量處理能力為基礎，Cloudmax 匯智提供的 CF-Shield 方案差異不在於「功能列出來更多」，而是強調由匯智資深網路工程師負責架構規劃與安全策略代管與調校，做成可落地、可維運的防護架構：包含依網站架構與流量型態完成整體規劃、設定安全策略與防護規則，並在上線前後進行調校與驗證，降低誤擋與設定不生效等常見風險。對客戶而言，不需要自行研究複雜細節，也能更穩定地導入企業級的網站防護與服務品質

## ▌Cloudflare Free Plan 提供哪些功能？

Cloudflare Free Plan（免費方案）提供基本的 DNS 代管功能，可供用戶在 Cloudflare 上管理網域解析紀錄（例如 A／CNAME／MX／TXT），並可視需求啟用全球節點的基礎加速（CDN/Proxy）、SSL/TLS（HTTPS）與基礎防護能力，同時也能查看基本的流量與安全分析資訊，適合一般形象網站或流量較低的應用情境

- **DNS 代管** ：在 Cloudflare 上管理你的網域解析紀錄（A/CNAME/MX/TXT 等）。
- **基本加速** ：透過全球節點提供 CDN 快取與連線效能提升。
- **HTTPS/SSL** ：可啟用 SSL/TLS，讓網站用 HTTPS 正常上線。
- **基礎防護** ：包含基本的 DDoS 緩解與部分安全防護功能。
- **流量分析** ：可查看基本流量與安全事件等分析資料。
![](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/blog-888.png)

## ▌Cloudflare DNS 設定步驟操作教學：

### 【事前準備】

- 請先查詢網域所使用的 DNS 設定值有哪些；如何查詢需依您目前使用的 DNS 廠商所提供的方式進行查詢。  
	以 Cloudmax 匯智用戶為例：
	- 若使用匯智網域註冊所提供的 ns1.ix1000.com 系列 DNS，可至會員中心查詢（ [https://domain.cloudmax.com.tw/](https://domain.cloudmax.com.tw/) ）
		- 若是使用匯智虛擬主機所提供的 ns1.wt500.com 系列DNS，請登入主機 Plesk 管理介面確認。
- 請準備網域註冊商管理介面的帳密。

### 【開始設定】

**1\. 登入 Cloudflare**

- 前往 Cloudflare 官方網站 https://www.cloudflare.com/zh-tw/
- 使用 Email 註冊帳號（或登入既有帳號）
![Cloudflare 新增網站：輸入網域並選擇匯入 DNS 記錄方式（快速掃描/手動/上傳）](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image.png)

Cloudflare 新增網站：輸入網域並選擇匯入 DNS 記錄方式（快速掃描/手動/上傳）

![Cloudflare 新增網站：輸入網域並選擇匯入 DNS 記錄方式（快速掃描/手動/上傳）](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image-1.png)

Cloudflare 新增網站：輸入網域並選擇匯入 DNS 記錄方式（快速掃描/手動/上傳）

**2\. 進入網域設定頁面**

- 登入後進入管理後台（Dashboard），點選左側選單列表「網域」
- 於右側畫面點選「+讓網域上線」
![Cloudflare 設定DNS：輸入網域](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image-7.png)

Cloudflare 設定DNS：輸入網域

**3\. 新增要設定的網域**

於下圖畫面的「輸入現有網域或登入新網域」中，輸入您要設定的網域，並：

- 點選第一個項目「快速掃描 DNS 記錄」  
	Cloudflare 會自動讀取您目前 DNS 服務商上的解析紀錄（例如 A / CNAME / MX / TXT），並匯入到 Cloudflare 當作初始設定。
- 封鎖 AI 訓練機器人（依需求設定）  
	這部份是設定是否允許「AI 訓練/資料蒐集用的爬蟲」抓取你網站內容。

這部份設定完成後點選繼續即可；下一步會出現選擇方案。

![Cloudflare 設定DNS：設定DNS 規則](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image-8.png)

Cloudflare 設定DNS：設定DNS 規則

**4\. 選擇方案**

於下圖畫面選擇免費 USD$0 方案即可。

![Cloudflare 設定DNS：選擇方案 Free Plan](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image-9.png)

Cloudflare 設定DNS：選擇方案 Free Plan

**5\. 確認 DNS 記錄是否完整並匯入**

接著 Cloudflare 會檢查您於前面輸入的網域既有 DNS 紀錄；自動檢查部份可能有遺漏，請務必再次檢查是否完整，若有缺漏請按「+新增記錄」手動編輯，將缺漏的網域 DNS 設定值都新增上去。  
\*一般來說，原紀錄類型為 NS，正常情況下不用在Cloudflare 介面上設定。

確認完整後，按下方「繼續以啟用」按鈕，進行 DNS 匯入作業。

![Cloudflare 設定DNS：匯入DNS 值](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image-10.png)

Cloudflare 設定DNS：匯入DNS 值

**6\. 回「原」網域註冊商管理介面更改 DNS 主機資料**

啟用後，會出現下圖畫面，請依照 Cloudflare 的說明回到 **原本** 的 **網域註冊商管理介面** 將 DNS 主機改為下方畫面中所提供 DNS 主機。

\* **提醒!! 若網域有啟用 DNSSEC，修改 DNS 主機前，需先洽原註冊商關閉DNSSEC。**

![Cloudflare 設定DNS：回到原本的網域註冊商管理介面將 DNS 主機改為下方畫面中所提供 DNS 主機。](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/image-11.png)

Cloudflare 設定DNS：回到原本的網域註冊商管理介面將 DNS 主機改為下方畫面中所提供 DNS 主機。

**7\. 完成設定**

於原 **網域註冊商管理介面** 完成設定後，回到此畫面點選「 **我已更新我的名稱伺服器** 」，此時 Cloudflare 會檢驗設定；由於 DNS 有生效時間差（約 4 到 8 小時），若確認已正確完成設定，可等待 4 小時候再來點選。

## ▌Cloudflare Free Plan 補充注意事項 – Proxy、SSL/TLS（完成後請檢查）

完成 DNS 匯入與 Nameserver 切換後，建議再確認兩個設定是否符合您的使用目的。 **若您只用到 Cloudflare DNS 代管服務，則可忽略以下設定。**

- **Proxy（橘雲）是否開啟** ：到「DNS ⭢ 記錄」，將網站會用到的 `@` ／ `www` A 或 CNAME 設為 **Proxied（橘雲）** ，流量才會先經過 Cloudflare，如此才能啟用基本加速與防護（CDN 與 WAF）；若 Proxy 狀態為 **DNS only（灰雲）** ，就只有解析功能。
![Cloudflare 設定DNS：確認 Proxy（橘雲）是否開啟](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/%E5%9C%96%E7%89%872.jpg)

Cloudflare 設定DNS：確認 Proxy（橘雲）是否開啟

- **SSL/TLS 模式是否正確** ：到「SSL/TLS ⭢ 概觀」，依您的主機是否已有 SSL 憑證選擇模式。
	- 關閉 (不安全)：指不使用 SSL 數位憑證，不建議使用此項目
		- 彈性： 若您的網站尚未安裝 SSL 數位憑證時，可選此項目
		- 完整、完整(嚴格)：若您的網站已具備 SSL 數位憑證，則選擇此項目
![Cloudflare 設定DNS：確認 SSL/TLS 設定](https://blog.cloudmax.com.tw/wp-content/uploads/2026/03/%E5%9C%96%E7%89%873.jpg)

Cloudflare 設定DNS：確認 SSL/TLS 設定

完成以上檢查後，即可開始使用 Cloudflare Free Plan 提供的 DNS 代管功能，並在需要時搭配基本加速與基礎防護能力。

完成上述設定後，您就能使用 Cloudflare 免費方案（Free Plan）所提供的 DNS 代管、基本 CDN 加速與基礎防護功能。

## ▌Cloudflare FAQ

**1\. Cloudflare Free Plan 可以做到哪些 DNS 相關設定？**

Cloudflare Free Plan（免費版）可以用來代管 DNS，並設定常見的 DNS 記錄，包含 A、CNAME、MX、TXT、CAA 等。這些設定足以支援大多數網站上線（指向主機）、信件服務（MX、SPF/DKIM/DMARC 相關 TXT），以及各種第三方服務的網域驗證（TXT/CNAME）。換句話說，一般網站需要的 DNS 設定，免費版通常就夠用。

**2\. 使用 Cloudflare 代管 DNS，需要改 Nameserver 嗎？會影響多久？**

需要。在 Cloudflare 設定完 DNS 後，需回到您原本註冊網域的「網域註冊商」把 Nameserver（NS）改成 Cloudflare 提供的兩組 NS。  
改完後，DNS 生效時間取決於全球快取更新，通常在 24 小時內會完成（常見 4-8 小時，少數情況可能更久）。  
降低風險的方法是：在切換前先確認 Cloudflare 已匯入完整 DNS 紀錄，切換後再用不同網路或工具檢查解析是否正確。

**3\. 什麼情況只用免費版不夠，建議升級或找代管？**

通常發生在「安全策略需要更細、網站不能出錯」的場景。例如：電商/會員登入網站、需要更進階 WAF 規則與例外處理、常遇到誤擋/要調校、同時管理多網域多系統（API/後台/第三方服務）、或內部沒有專人長期維運與排查。這類情境建議評估升級付費方案，或交由代管團隊協助，避免因設定或調校不足造成停機、誤擋或安全風險。  
  
如果您遇到以下情況，通常就不建議只靠免費版自己摸索：  
**a. 網站是商務/電商/會員登入** ，對可用性與安全要求高（怕誤擋、怕設定出錯造成停機）。  
**b. 需要更進階的 WAF 規則、Bot 管理、細緻的安全策略與例外處理** （單靠預設規則不夠或常誤擋）。  
**c. 有多網域、多系統、混合架構** （例如 API、後台、第三方金流、CDN/反代搭配），設定複雜且需要一致的策略。  
**d. 內部沒人可以長期維運** （包含調校、驗證、事件排查與持續優化）。  
  
這時可以評估升級付費方案，若不確定哪種方案適合自己，歡迎與 Cloudmax 匯智服務團隊諮詢。

Image by AI-generated via ChatGPT / DALL·E

歡迎轉載！請見： [**轉載原則**](https://blog.cloudmax.com.tw/copyright/) 。