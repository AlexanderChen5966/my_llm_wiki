---
title: "【實驗】連接 ECS 服務之間的 6 種連接方式，實際上速度差多少？"
source: "https://codelove.tw/@tony/post/ayo4Ox"
author:
published:
created: 2026-09-07
description: "[](#1-%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)1. 前言--------------------------------------------------前陣子參加了 JAWS-UG 容器支部 × NW-JAWS 合作活動「一起學容器網路！！」，實際用..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月05日](https://codelove.tw/@tony/post/ayo4Ox "2026-09-05 17:15:11")

## 【實驗】連接 ECS 服務之間的 6 種連接方式，實際上速度差多少？

## 1\. 前言

前陣子參加了 JAWS-UG 容器支部 × NW-JAWS 合作活動「一起學容器網路！！」，實際用實作方式建置了 ECS Service Connect 與 VPC Lattice。

在實作中，我體驗到了把各服務真的跑起來的流程。不過，我也很好奇不同連線方式會讓通訊速度差多少，所以決定自己也搭一個環境來量測看看。

本文會在東京區域內建置 ECS 服務間連線的多種方式，並在原則上將 task 配置在同一個 AZ 後，實測延遲。  
比較對象如下 6 種：

- 同一個 VPC・直接使用 Task IP（基準）
- VPC Peering・直接使用 Task IP
- VPC Peering・ECS Service Connect
- Transit Gateway・直接使用 Task IP
- AWS PrivateLink
- Amazon VPC Lattice

此外，我也做了補充實驗：使用同一個 NLB 與 server task，切出透過 PrivateLink 所增加的延遲；以及測試跨 AZ 時會慢多少的實驗。

另外，雖然這是實驗系列的第二篇，但由於前一篇文章（東京－大阪區域間的 3 種私網連線方式實測）在量測環境與方法上都不同，因此不做數值上的比較。

本文適合已經大致理解 ECS task 定義與 service 基本用法，並且想了解不同服務間通訊連線方式在實務上取捨的工程師。

大家也可以一邊看，一邊猜猜看哪一種會最快。

## 2\. 驗證全貌

### 2.1 用語整理

連線方式可以分成兩層：封包如何送達，以及如何找到對方。前者是 OSI 參考模型的網路層（L3）本身，因此以下會以「L3 可達性」來表達。  
後者則是指 DNS 名稱、直接 IP、代理程式、負載平衡器等，也就是 client 如何指定連線目的地，以及之後如何實際到達服務。本篇把這些稱為「目的地指定方式」。  
這次量測的 6 種方式，就是由這兩層的組合構成。

PrivateLink 與 VPC Lattice 是把 L3 可達性與目的地指定方式整合成單一服務，無法像其他方式那樣自由組合兩層。相同 VPC、VPC Peering、Transit Gateway 可以自由選擇直接使用 Task IP 或 Service Connect；相對地，PrivateLink 是透過在 client VPC 建立的 Interface VPC Endpoint 來連線，通常使用分配給 endpoint 的 DNS 名稱。Lattice 則使用 Lattice service 的 DNS 名稱。這些差異我也都納入量測時的考量。

### 2.2 比較對象路徑

先畫成圖會像下面這樣。老實說我自己做的時候也有點混亂，所以乾脆畫圖整理了一下。  
[![01_test-patterns.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F53839df9-0f3b-4216-954b-d4ad646312f0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bd2381870ac3bb1bb810b86c4298f768)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F53839df9-0f3b-4216-954b-d4ad646312f0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bd2381870ac3bb1bb810b86c4298f768)

| 路徑 | 組成 | L3 可達性 | 目的地指定方式 | 差異中可得知的事 |
| --- | --- | --- | --- | --- |
| A | 同一 VPC・直接使用 Task IP | 無 | 直接 IP | 基準值 |
| B | Peering + 直接使用 Task IP | Peering | 直接 IP | B−A = 跨 VPC 的基本成本 |
| C | Peering + Cloud Map DNS | Peering | Cloud Map DNS（未量測） | 解析名稱後的行為與 B 相同，因此略過 |
| D | Peering + Service Connect | Peering | Envoy sidecar | D−B = sidecar 兩跳成本 |
| E | Internal ALB | ALB DNS（未量測） | ALB 特有處理 | 含 ALB 的 L7 處理，從比較軸中排除，因此略過 |
| F | TGW + 直接使用 Task IP | TGW | 直接 IP | F−B = TGW hop 成本 |
| G | PrivateLink | PrivateLink endpoint DNS | Endpoint DNS | G−B = PrivateLink + NLB 結構相對於 Peering 直接連線的延遲差 |
| G′ | Peering + Internal NLB 直接連線（補充實驗） | Peering | Internal NLB DNS | G−G′ = 使用相同 NLB 架構時，經由 PrivateLink 所增加的延遲 |
| H | VPC Lattice | VPC Lattice | Lattice service 名稱 | H−B = 經由 VPC Lattice 與 Peering 直接連線的延遲差 |

- C 是把 Cloud Map 的 service name 做 DNS 解析後，直接對回傳的 task IP 建立 TCP 連線的路徑。keep-alive ON/OFF 等量測條件的細節會在第 5 章說明，不過在 keep-alive ON 的條件下，一旦建立過連線就會重複使用，因此 DNS 解析只會發生第一次。連線建立後的通訊路徑會與 B 完全相同，所以在這次的規模下，以所有 request 的平均值來看，單次 DNS 解析時間會被淹沒，我判斷無法有效評估 B 與 C 的差異，因此未量測。
- E 的 Internal ALB 因為 ALB 本身的 private IP 可能會變動，所以無法像 G 那樣直接指定 IP，然後透過 PrivateLink 或 Peering 連線。Internal ALB 必須透過 DNS 名稱存取。雖然也可以透過 VPC Peering 從其他 VPC 連入，但若把 ALB 特有的 L7 處理也納入，會讓比較軸變多，因此這次排除。另外，連 L3 連線性的部分我也在量測前就先排除，所以上表中也以「-」表示。

C 與 E 雖然是排除量測的路徑，但因為路徑 ID 在文章、S3 與 CSV 中都共用作為 key，所以就保留缺號。

- G′ 是補充實驗用的路徑，G 與 G′ 使用同一個 task，只改變 client 到 NLB 的連線路徑。取 G−G′ 就能把透過 PrivateLink 所增加的時間單獨抽出來。

另外，除了連線方式比較之外，我也從另一個角度測了 AZ 配置的影響。量測前我原本預期，A～H 的連線方式造成的延遲差異應該不大。另一方面，我也想知道相同架構下跨 AZ 會慢多少，所以把它作為另一個比較軸來量測。

| 路徑 | 組成 | 差異中可得知的事 |
| --- | --- | --- |
| L1 | 同一 VPC・同一 AZ（與路徑 A 相同架構） | 基準值 |
| L2 | 同一 VPC・跨 AZ | L2−L1 = 跨一個 AZ 的成本 |

### 2.3 統一條件

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745929&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929315&amp;bpp=1&amp;bdt=1507&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4530&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1998&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=1&amp;fsb=1&amp;dtd=24" title="Advertisement" aria-label="Advertisement"></iframe>

為了只抽出連線方式的差異，我把以下條件在所有路徑都統一。  
※前一次量測時，中途曾出現不行的情境並思考過處理方式，所以一開始就先確認並定好方式很重要。

將各 ECS service 的 Desired Count 固定為 1，並避免一台 Managed Instance 上同時配置多個量測對象 task（這個方法會在後面說明）。所有路徑的 task CPU 與 memory 都相同，只有路徑 D 因為多了 Envoy sidecar，所以額外加上 sidecar 的資源。  
負載產生用的 client task 與量測對象的 server task 分開放在不同 instance 上。路徑 A 的對應 task 也一樣放在不同 instance 上。因為同一 instance 上 task 之間的通訊，可能會與其他路徑走不同的網路路徑（不知道通訊會在哪裡折返）。  
※如果有人知道同一 instance 上 task 之間通訊的機制，還請告訴我。

## 3\. 各方式的技術細節

以下說明這次量測的各路徑。把剛剛的表轉成實體架構後如下。  
[![02_test-patterns.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fd9a20344-720f-4e9f-8d4b-055d4203390e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=56c33b62502a754c33fa098b65e4305e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fd9a20344-720f-4e9f-8d4b-055d4203390e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=56c33b62502a754c33fa098b65e4305e)

### 3.1 同一 VPC・直接使用 Task IP（路徑 A）

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745960&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929293&amp;bpp=1&amp;bdt=1485&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C1634x905%2C1005x271&amp;nras=5&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5984&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2390&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc%2CAOrYGsmo9EOiwCJriL799ZVNX0ix24CDsyFKFBEx54m-hGerSK1HsxICoGgqWIHwwBhVfkrao3UfI6t5tFapn7dmbQlLtvoCTxKesJOuQvbl7MgEHke5cT6qOlaRYPIbPGeswlc%2CAOrYGsmjMgDEMOty5iQ94p6VeOVeSWv1WIFG4RlKvlv8V6ZxR1KoOQCo8el_7D8Hoz-pkg0HPosu5R3MRAKc4gNB5ZatQttuf7bsRSWAH7CfPJZ4Lcs&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=31625" title="Advertisement" aria-label="Advertisement"></iframe>

這是在同一個 VPC 內，client task 直接對 server task 的 private IP 送出 HTTP request 的架構。這是最單純的方式，不經過 VPC 或中繼服務，因此作為其他路徑比較的基準。

### 3.2 VPC Peering・直接使用 Task IP（路徑 B）

將兩個 VPC 以一對一方式連接，並透過在路由表新增靜態路由讓其可達，之後 client 直接存取 server 的 task IP。  
雖然這次不直接相關，但 VPC Peering 不是可傳遞的，因此 VPC 數量增加時需要 full mesh 的限制，前一篇文章也有提到。

### 3.3 Peering + ECS Service Connect（路徑 D）

ECS Service Connect 會自動把 Envoy sidecar 注入到 task 中，並提供透過 Cloud Map 的 service name 解析，以及經由 proxy 的通訊。  
sidecar 不需要明寫在 task definition 裡，只要在 service 端設定 `ServiceConnectConfiguration` ，ECS 就會自動幫你加上。

client 會先連到自己 task 內的 Envoy，接著由 Envoy 轉送到 server 端的 Envoy，再到 server 端應用程式，因此是兩跳架構。App Mesh 預定於 2026 年 9 月 30 日終止支援，而 AWS 也有提供依使用情境移轉到 ECS Service Connect 或 VPC Lattice 的資訊。這次驗證中，我們沒有使用 App Mesh，而是把這些替代方案納入比較對象，也順便補充一下。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745962&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929296&amp;bpp=1&amp;bdt=1488&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C1634x905%2C1005x271%2C952x280&amp;nras=6&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6929&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3320&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc%2CAOrYGsmo9EOiwCJriL799ZVNX0ix24CDsyFKFBEx54m-hGerSK1HsxICoGgqWIHwwBhVfkrao3UfI6t5tFapn7dmbQlLtvoCTxKesJOuQvbl7MgEHke5cT6qOlaRYPIbPGeswlc%2CAOrYGsmjMgDEMOty5iQ94p6VeOVeSWv1WIFG4RlKvlv8V6ZxR1KoOQCo8el_7D8Hoz-pkg0HPosu5R3MRAKc4gNB5ZatQttuf7bsRSWAH7CfPJZ4Lcs%2CAOrYGsl6y4QKPcdppJiwgQVsiHhmQnpq5EllhHz1wjhhXiaYIPjd-WEqXituqdjhP8h0oxtnDB1sE1RcfWbFp0BvvXrXDZsYHDlNp6XyLJvgDfGR&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=33401" title="Advertisement" aria-label="Advertisement"></iframe>

### 3.4 Transit Gateway・直接使用 Task IP（路徑 F）

Transit Gateway 是 hub-and-spoke 型的連線。這次只有東京區域內的 2 個 VPC，所以並不是 TGW 最典型的用途（集中管理大量 VPC），但這次是為了單純量測經由 TGW 的 hop 成本。  
我們沿用與路徑 B 相同的 client/server 配對，只把路由從 Peering 切換成 TGW，讓 F−B 成為純粹的 TGW hop 差異。

### 3.5 AWS PrivateLink（路徑 G）

將 NLB 後方的服務以 endpoint service 形式公開，並從使用側 VPC 中建立的 Interface VPC Endpoint 以 private 方式連線。這不是用來由 service 側主動連到使用側，而是由使用側連到 service 側。client 端建立 Interface 型 VPC Endpoint 後，透過其 DNS 名稱存取。

這次是在同一區域內連線，因此也可以啟用 `PrivateDnsEnabled` ；但因為 endpoint service 端沒有準備自訂網域，所以我將其設為 `false` ，直接使用 AWS 自動發行的 DNS 名稱。

### 3.6 Amazon VPC Lattice（路徑 H）

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745963&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929299&amp;bpp=1&amp;bdt=1491&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C1634x905%2C1005x271%2C952x280%2C952x280&amp;nras=7&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7744&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=4161&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc%2CAOrYGsmo9EOiwCJriL799ZVNX0ix24CDsyFKFBEx54m-hGerSK1HsxICoGgqWIHwwBhVfkrao3UfI6t5tFapn7dmbQlLtvoCTxKesJOuQvbl7MgEHke5cT6qOlaRYPIbPGeswlc%2CAOrYGsmjMgDEMOty5iQ94p6VeOVeSWv1WIFG4RlKvlv8V6ZxR1KoOQCo8el_7D8Hoz-pkg0HPosu5R3MRAKc4gNB5ZatQttuf7bsRSWAH7CfPJZ4Lcs%2CAOrYGsl6y4QKPcdppJiwgQVsiHhmQnpq5EllhHz1wjhhXiaYIPjd-WEqXituqdjhP8h0oxtnDB1sE1RcfWbFp0BvvXrXDZsYHDlNp6XyLJvgDfGR%2CAOrYGsmO0DDzYZkI_MvRSVUnoabPp4V-CQsO2i0QFaYWEolbGmxiUOPv2p5bnH-Ab-u_VY9HwtoGb5rUtG4BnmlVpTK7y6gIg95YkC-KpJq65EYwOxY&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=5&amp;fsb=1&amp;dtd=34118" title="Advertisement" aria-label="Advertisement"></iframe>

VPC Lattice 是一項較新的服務，以受管方式提供類似 service mesh 的功能。只要把 VPC Lattice 的 target group 綁到 ECS service 上，ECS 就會在 task 啟動時自動註冊 IP，不需要像 NLB 那樣手動註冊 target。

VPC Lattice 可選的 service listener protocol 有 HTTP、HTTPS、TLS。TLS listener 雖然可以將加密的 TCP traffic pass through 到 target，但不是那種可任意設定明文 TCP listener 的方式。這次為了統一所有條件，測試一律使用 HTTP（會在第 5 章再說明）。

### 3.7 補充

#### CIDR 是否可重疊

VPC Peering 與 TGW 不支援 CIDR 重疊的 VPC 之間連線。相對地，PrivateLink 與 VPC Lattice 是透過 endpoint 或 service name 來達成連線，因此不需要在 L3 可達性上特別考慮 CIDR 設計，即使兩個 VPC 的 CIDR 重疊也可以架構。這次我有刻意避免 CIDR 重疊，不過在考慮使用哪種服務時，這會成為一個重要觀點：是優先 CIDR 設計的自由度，還是優先單純的 L3 可達性。

#### 關於 App Mesh

App Mesh 預定於 2026 年 9 月 30 日停止支援，因此這次沒有列入比較對象。AWS 也已針對不同使用情境提供移轉到 ECS Service Connect 或 VPC Lattice 的資訊，因此本驗證把兩者都納入比較。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745965&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929301&amp;bpp=1&amp;bdt=1493&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C1634x905%2C1005x271%2C952x280%2C952x280%2C952x280&amp;nras=8&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=8601&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=5023&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc%2CAOrYGsmo9EOiwCJriL799ZVNX0ix24CDsyFKFBEx54m-hGerSK1HsxICoGgqWIHwwBhVfkrao3UfI6t5tFapn7dmbQlLtvoCTxKesJOuQvbl7MgEHke5cT6qOlaRYPIbPGeswlc%2CAOrYGsmjMgDEMOty5iQ94p6VeOVeSWv1WIFG4RlKvlv8V6ZxR1KoOQCo8el_7D8Hoz-pkg0HPosu5R3MRAKc4gNB5ZatQttuf7bsRSWAH7CfPJZ4Lcs%2CAOrYGsl6y4QKPcdppJiwgQVsiHhmQnpq5EllhHz1wjhhXiaYIPjd-WEqXituqdjhP8h0oxtnDB1sE1RcfWbFp0BvvXrXDZsYHDlNp6XyLJvgDfGR%2CAOrYGsmO0DDzYZkI_MvRSVUnoabPp4V-CQsO2i0QFaYWEolbGmxiUOPv2p5bnH-Ab-u_VY9HwtoGb5rUtG4BnmlVpTK7y6gIg95YkC-KpJq65EYwOxY%2CAOrYGslMx5g_qIYlSDwf9MsAWzZ04QEF2XzPS_Kd8vMlcL5SH6FnUPRV4SsFZAsB5ngyGj8pC5Y7kfOg5vquBW2fVzvC8lV9aBnP5n4KHeJjwmMaCw&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=6&amp;fsb=1&amp;dtd=35833" title="Advertisement" aria-label="Advertisement"></iframe>

本文的驗證是基於 2026 年 8 月底各服務的規格。支援的 protocol 與限制等，未來可能因更新而改變。

## 4\. 驗證環境建置

基礎架構的一部分是手動建好，再把內容轉成 CloudFormation 重現，最後完成整體建置。

### 4.1 Managed Instances 的建置

#### 建立 IAM 角色

首先建立 4 個 IAM 角色。2 個給 Managed Instances 使用，另外 2 個給其上執行的 task 使用。

##### ecs-conn-cluster-infra-role

讓 ECS 在底層啟動與管理 EC2 instance 的角色。路徑 H 對 VPC Lattice 的 target 註冊也是由這個角色執行。之所以另外用 inline policy 補 `iam:PassRole` ，是因為 AWS managed policy 內的 PassRole 許可只限定在 `arn:aws:iam::*:role/ecsInstanceRole*` 這種名稱上，像我這次自己命名的 instance role 並不涵蓋，所以後來才補上。  
[![ecs-conn-cluster-infra-role.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fff3f889a-b3fa-45f2-8a00-b2d94e7f2ef7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a6a769c6b15f2b1745d6c8709511eb17)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fff3f889a-b3fa-45f2-8a00-b2d94e7f2ef7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a6a769c6b15f2b1745d6c8709511eb17)

##### ecs-conn-cluster-instance-role

讓已啟動的 EC2 instance 本身加入 ECS cluster 的角色。透過 instance profile 帶到 instance 上。  
[![ecs-conn-cluster-instance-role.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Ffca1344c-4129-44ae-8450-faa9e7d12672.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fe7aa883c509f7a8c7379e54e87b8561)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Ffca1344c-4129-44ae-8450-faa9e7d12672.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fe7aa883c509f7a8c7379e54e87b8561)

##### ecs-conn-app-common-exec-role

task 啟動時 ECS agent 使用的角色。負責從 ECR 取得 image，以及將 log 輸出到 CloudWatch Logs。  
[![ecs-conn-app-common-exec-role.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F4c7fc712-5a53-4e09-93a5-e06dc1b9a16b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7dca96e435570d9e630d50085b48078e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F4c7fc712-5a53-4e09-93a5-e06dc1b9a16b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7dca96e435570d9e630d50085b48078e)

##### ecs-conn-app-common-task-role

container 內應用程式本身使用的角色。授予 ECS Exec 進入 container 的權限，以及將量測結果放到 S3 的權限。  
[![ecs-conn-app-common-task-role.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fb720d8ed-82a0-4dc6-921a-9909d0a50d9c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f05bd048c99492ebcc9fae0067075619)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fb720d8ed-82a0-4dc6-921a-9909d0a50d9c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f05bd048c99492ebcc9fae0067075619)

#### 建立 ECS cluster

我只建立 1 個 cluster，並在其中掛 3 個 capacity provider。VPC 明明只有 2 個，卻用了 3 個 capacity provider，是因為 capacity provider 指定的 security group 只會在單一 VPC 內有效。單一 capacity provider 無法同時涵蓋 VPC-A 與 VPC-B，因此必須拆成 VPC-A 用與 VPC-B 用。第 3 個則是用於 AZ 配置比較（L2）時，把 server 放到另一個 AZ 的 VPC-A 內另一個 subnet。  
[![cp00.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F015fab33-eba0-44db-b19e-9a07ddefa233.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b96e20ddde0f6a7b3283c94307e9df16)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F015fab33-eba0-44db-b19e-9a07ddefa233.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b96e20ddde0f6a7b3283c94307e9df16)

我本來想把 capacity provider 名稱用 `ecs-` 開頭，結果被擋下來了。 `ecs` 開頭的名稱是保留字。  
[![cp01.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F21a84766-c6f8-4b67-9dfa-03c5575ff312.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2b92920d35d39c4ec2e3d0aa9330621b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F21a84766-c6f8-4b67-9dfa-03c5575ff312.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2b92920d35d39c4ec2e3d0aa9330621b)

接著把剛剛建立的 IAM 角色馬上設定進去。  
[![cp02.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fea50b61c-dd6d-44bc-adbe-2d6c7af265e9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e6bf1b843755e35c67b12bf89a9534d6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fea50b61c-dd6d-44bc-adbe-2d6c7af265e9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e6bf1b843755e35c67b12bf89a9534d6)

instance 的條件是把 vCPU 與 memory 的最小值與最大值都設成同樣的值，固定為 `c7g.xlarge` 。這是為了後面那個「1 instance 1 task」的硬做法，所以必須先把 instance 容量固定。  
[![cp03.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fd7f9e9e9-4a11-4d57-afc7-8e37852bf18a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0fe1841074e5a2f20fee5279ee5e73e7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fd7f9e9e9-4a11-4d57-afc7-8e37852bf18a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=0fe1841074e5a2f20fee5279ee5e73e7)  
[![cp04.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fb5a72a67-61b8-4fba-896d-7646b3d58bf9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c11cd83115077713696f556f9615b602)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Fb5a72a67-61b8-4fba-896d-7646b3d58bf9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c11cd83115077713696f556f9615b602)

#### Task 定義

Task 定義準備了 client 用與 server 用兩份。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745973&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929303&amp;bpp=1&amp;bdt=1495&amp;idt=0&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C1634x905%2C1005x271%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=9&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=14166&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=10618&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc%2CAOrYGsmo9EOiwCJriL799ZVNX0ix24CDsyFKFBEx54m-hGerSK1HsxICoGgqWIHwwBhVfkrao3UfI6t5tFapn7dmbQlLtvoCTxKesJOuQvbl7MgEHke5cT6qOlaRYPIbPGeswlc%2CAOrYGsmjMgDEMOty5iQ94p6VeOVeSWv1WIFG4RlKvlv8V6ZxR1KoOQCo8el_7D8Hoz-pkg0HPosu5R3MRAKc4gNB5ZatQttuf7bsRSWAH7CfPJZ4Lcs%2CAOrYGsl6y4QKPcdppJiwgQVsiHhmQnpq5EllhHz1wjhhXiaYIPjd-WEqXituqdjhP8h0oxtnDB1sE1RcfWbFp0BvvXrXDZsYHDlNp6XyLJvgDfGR%2CAOrYGsmO0DDzYZkI_MvRSVUnoabPp4V-CQsO2i0QFaYWEolbGmxiUOPv2p5bnH-Ab-u_VY9HwtoGb5rUtG4BnmlVpTK7y6gIg95YkC-KpJq65EYwOxY%2CAOrYGslMx5g_qIYlSDwf9MsAWzZ04QEF2XzPS_Kd8vMlcL5SH6FnUPRV4SsFZAsB5ngyGj8pC5Y7kfOg5vquBW2fVzvC8lV9aBnP5n4KHeJjwmMaCw%2CAOrYGsmB_qYCXfalkkBbi_07FH-FUvROtYV2kc5xQGlyo6TeVIGlvJLY6eJoZZGkn8kdLR4oCBXbbWhahVouX8rBIGRClIIZY7AgB2YTyGe-ULPnRg&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=7&amp;fsb=1&amp;dtd=44497" title="Advertisement" aria-label="Advertisement"></iframe>

對應啟動類型的 `RequiresCompatibilities` 指定 `MANAGED_INSTANCES` 。網路模式用 `awsvpc` ，CPU 設為 4096，memory 設為 7168。 `c7g.xlarge` 是 4 vCPU / 8192 MiB，所以我把 1024 MiB 留給 host OS 與 ECS agent。CPU architecture 是 Graviton，因此明確指定 `ARM64` 。

在 service 端我遇到一個坑。 `AvailabilityZoneRebalancing` 預設是啟用的，但啟用後，每次部署都會需要一段時間同時存在新舊 task 的容量。即使只有 1 個 task 的 service，也會被要求準備 2 倍容量。  
如前所述，這次我希望同時執行的 task 數量越少越好，因此我把 `AvailabilityZoneRebalancing` 關掉，並將 `MinimumHealthyPercent` 設為 0、 `MaximumPercent` 設為 100，控制成先停止再重建的部署方式。

另外，因為要使用 ECS Exec，所以也啟用了 `EnableExecuteCommand` 。

具體定義檔寫在下一節的 CloudFormation 定義中。

#### CloudFormation 定義

這是我先手動建立、確認內容後，再交由 AI agent 整理成 CloudFormation 的版本。  
因為很長，所以折疊起來。

ecs-conn-cluster.yaml（cluster・capacity provider・IAM role）\`\`\`  
AWSTemplateFormatVersion: "2010-09-09"  
Description: >-  
ECS service connectivity benchmark: cluster layer. One ECS cluster, three  
Managed Instances capacity providers, and the IAM roles they need.

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788745975&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745929305&amp;bpp=1&amp;bdt=1497&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280%2C1634x905%2C1005x271%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=1329362303022&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=15153&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=11619&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;psts=AOrYGslT2Lje3xBNCn-V8r1EfidwlIdyV-WHZ0aPsBsiVyZi2af2syw4O3jW5QdlJftP9gmbviHRLYm6R39vInZsElN6DX1hHrrcgAkKMgkltp_zWsc%2CAOrYGsmo9EOiwCJriL799ZVNX0ix24CDsyFKFBEx54m-hGerSK1HsxICoGgqWIHwwBhVfkrao3UfI6t5tFapn7dmbQlLtvoCTxKesJOuQvbl7MgEHke5cT6qOlaRYPIbPGeswlc%2CAOrYGsmjMgDEMOty5iQ94p6VeOVeSWv1WIFG4RlKvlv8V6ZxR1KoOQCo8el_7D8Hoz-pkg0HPosu5R3MRAKc4gNB5ZatQttuf7bsRSWAH7CfPJZ4Lcs%2CAOrYGsl6y4QKPcdppJiwgQVsiHhmQnpq5EllhHz1wjhhXiaYIPjd-WEqXituqdjhP8h0oxtnDB1sE1RcfWbFp0BvvXrXDZsYHDlNp6XyLJvgDfGR%2CAOrYGsmO0DDzYZkI_MvRSVUnoabPp4V-CQsO2i0QFaYWEolbGmxiUOPv2p5bnH-Ab-u_VY9HwtoGb5rUtG4BnmlVpTK7y6gIg95YkC-KpJq65EYwOxY%2CAOrYGslMx5g_qIYlSDwf9MsAWzZ04QEF2XzPS_Kd8vMlcL5SH6FnUPRV4SsFZAsB5ngyGj8pC5Y7kfOg5vquBW2fVzvC8lV9aBnP5n4KHeJjwmMaCw%2CAOrYGsmB_qYCXfalkkBbi_07FH-FUvROtYV2kc5xQGlyo6TeVIGlvJLY6eJoZZGkn8kdLR4oCBXbbWhahVouX8rBIGRClIIZY7AgB2YTyGe-ULPnRg%2CAOrYGsmhM8pHpKODKsL5OVoddjkKl6pxnWpHy2PsL0oH6spUZAjIiqRhnSpRIAgotKi4W1uDJwufLw_en3AJXNO6BIDGAufLnfgmFXOwr-XAO9Hc6dU&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=1&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=8&amp;fsb=1&amp;dtd=45796" title="Advertisement" aria-label="Advertisement"></iframe>

Parameters:  
VpcAId:  
Type: String  
SubnetAPrimaryId:  
Type: String  
SubnetASecondaryId:  
Type: String  
SubnetBPrimaryId:  
Type: String  
SgTaskAId:  
Type: String  
SgTaskBId:  
Type: String  
InstanceType:  
Type: String  
Default: c7g.xlarge  
InstanceVCpuCount:  
Type: Number  
Default: 4  
Description: Must match InstanceType's vCPU count exactly (single-task sizing trick).  
InstanceMemoryMiB:  
Type: Number  
Default: 8192  
TaskMemoryReservationMiB:  
Type: Number  
Default: 7168  
Description: InstanceMemoryMiB minus headroom for the host OS and ECS agent.  
ServiceConnectNamespaceName:  
Type: String  
Default: [ecs-conn.local](http://ecs-conn.local/)

Resources:  
Cluster:  
Type: AWS::ECS::Cluster  
Properties:  
ClusterName:!Sub "${AWS::StackName}"  
ServiceConnectDefaults:  
Namespace:!Ref ServiceConnectNamespace  
ClusterSettings:

- Name: containerInsights  
	Value: enhanced
	## Service Connect requires a Cloud Map namespace to exist before the
	## cluster references it as a default. Not an ECS-managed namespace
	## (that only happens via the console "create cluster" flow).
	ServiceConnectNamespace:  
	Type: AWS::ServiceDiscovery::PrivateDnsNamespace  
	Properties:  
	Name:!Ref ServiceConnectNamespaceName  
	Vpc:!Ref VpcAId
	## \---------------- IAM ----------------
	## The AWS managed policy's PassRole statement is scoped to
	## arn:aws:iam:::role/ecsInstanceRole -- it does NOT cover an
	## arbitrarily-named instance role like the one below.
	InfrastructureRole:  
	Type: AWS::IAM::Role  
	Properties:  
	RoleName:!Sub "${AWS::StackName}-infra-role"  
	AssumeRolePolicyDocument:  
	Version: "2012-10-17"  
	Statement:
	- Effect: Allow  
		Principal:  
		Service: [ecs.amazonaws.com](http://ecs.amazonaws.com/)  
		Action: sts:AssumeRole  
		ManagedPolicyArns:
- arn:aws:iam::aws:policy/AmazonECSInfrastructureRolePolicyForManagedInstances
- arn:aws:iam::aws:policy/AmazonECSInfrastructureRolePolicyForVpcLattice  
	Policies:
- PolicyName: PassInstanceRole  
	PolicyDocument:  
	Version: "2012-10-17"  
	Statement:
	- Effect: Allow  
		Action: iam:PassRole  
		Resource:!GetAtt InstanceRole.Arn  
		Condition:  
		StringLike:  
		iam:PassedToService: ec2.\*
	InstanceRole:  
	Type: AWS::IAM::Role  
	Properties:  
	RoleName:!Sub "${AWS::StackName}-instance-role"  
	AssumeRolePolicyDocument:  
	Version: "2012-10-17"  
	Statement:
	- Effect: Allow  
		Principal:  
		Service: [ec2.amazonaws.com](http://ec2.amazonaws.com/)  
		Action: sts:AssumeRole  
		ManagedPolicyArns:
- arn:aws:iam::aws:policy/AmazonECSInstanceRolePolicyForManagedInstances
	InstanceProfile:  
	Type: AWS::IAM::InstanceProfile  
	Properties:  
	Roles: \[!Ref InstanceRole\]
	## \---------------- Capacity providers ----------------
	CapacityProviderVpcAMain:  
	Type: AWS::ECS::CapacityProvider  
	Properties:  
	Name: conn-cp-vpc-a-main # CP names cannot start with "ecs"  
	ClusterName:!Ref Cluster  
	ManagedInstancesProvider:  
	InfrastructureRoleArn:!GetAtt InfrastructureRole.Arn  
	InstanceLaunchTemplate:  
	Ec2InstanceProfileArn:!GetAtt InstanceProfile.Arn  
	NetworkConfiguration:  
	Subnets: \[!Ref SubnetAPrimaryId\]  
	SecurityGroups: \[!Ref SgTaskAId\]  
	InstanceRequirements:  
	VCpuCount:  
	Min:!Ref InstanceVCpuCount  
	Max:!Ref InstanceVCpuCount  
	MemoryMiB:  
	Min:!Ref InstanceMemoryMiB  
	Max:!Ref InstanceMemoryMiB  
	AllowedInstanceTypes: \[!Ref InstanceType\]  
	Monitoring: BASIC
	## VPC-A, L2 (apne1-az2). L2's peer task only.
	CapacityProviderVpcAL2:  
	Type: AWS::ECS::CapacityProvider  
	Properties:  
	Name: conn-cp-vpc-a-l2  
	ClusterName:!Ref Cluster  
	ManagedInstancesProvider:  
	InfrastructureRoleArn:!GetAtt InfrastructureRole.Arn  
	InstanceLaunchTemplate:  
	Ec2InstanceProfileArn:!GetAtt InstanceProfile.Arn  
	NetworkConfiguration:  
	Subnets: \[!Ref SubnetASecondaryId\]  
	SecurityGroups: \[!Ref SgTaskAId\]  
	InstanceRequirements:  
	VCpuCount:  
	Min:!Ref InstanceVCpuCount  
	Max:!Ref InstanceVCpuCount  
	MemoryMiB:  
	Min:!Ref InstanceMemoryMiB  
	Max:!Ref InstanceMemoryMiB  
	AllowedInstanceTypes: \[!Ref InstanceType\]  
	Monitoring: BASIC
	## VPC-B, main path (apne1-az1). Server tasks for A/B/D/F/G/H.
	CapacityProviderVpcBMain:  
	Type: AWS::ECS::CapacityProvider  
	Properties:  
	Name: conn-cp-vpc-b-main  
	ClusterName:!Ref Cluster  
	ManagedInstancesProvider:  
	InfrastructureRoleArn:!GetAtt InfrastructureRole.Arn  
	InstanceLaunchTemplate:  
	Ec2InstanceProfileArn:!GetAtt InstanceProfile.Arn  
	NetworkConfiguration:  
	Subnets: \[!Ref SubnetBPrimaryId\]  
	SecurityGroups: \[!Ref SgTaskBId\]  
	InstanceRequirements:  
	VCpuCount:  
	Min:!Ref InstanceVCpuCount  
	Max:!Ref InstanceVCpuCount  
	MemoryMiB:  
	Min:!Ref InstanceMemoryMiB  
	Max:!Ref InstanceMemoryMiB  
	AllowedInstanceTypes: \[!Ref InstanceType\]  
	Monitoring: BASIC
	ClusterCapacityProviderAssociations:  
	Type: AWS::ECS::ClusterCapacityProviderAssociations  
	Properties:  
	Cluster:!Ref Cluster  
	CapacityProviders:
- !Ref CapacityProviderVpcAMain
- !Ref CapacityProviderVpcAL2
- !Ref CapacityProviderVpcBMain  
	DefaultCapacityProviderStrategy: \[\]

Outputs:  
ClusterArn:  
Value:!GetAtt Cluster.Arn  
InfrastructureRoleArn:  
Value:!GetAtt InfrastructureRole.Arn  
Description: Used by path H's Service.VpcLatticeConfigurations.RoleArn.  
TaskMemoryReservationMiB:  
Value:!Ref TaskMemoryReservationMiB

```
ecs-conn-app-a.yaml（路徑 A 的 task 定義與 ECS service）\`\`\`
AWSTemplateFormatVersion: "2010-09-09"
Description: >-
  ECS service connectivity benchmark: path A (same VPC, direct task IP).
  Client and server are two independent services on the SAME capacity
  provider; single-task-mode-by-sizing guarantees they land on different
  instances automatically.

Parameters:
  ClusterArn:
    Type: String
  CapacityProviderName:
    Type: String
  SubnetId:
    Type: String
  SecurityGroupId:
    Type: String
  RepositoryUri:
    Type: String
  ImageTag:
    Type: String
    Default: latest
  ExecutionRoleArn:
    Type: String
  TaskRoleArn:
    Type: String
  TaskCpu:
    Type: String
    Default: "4096"
  TaskMemory:
    Type: String
    Default: "7168"
  AppPort:
    Type: Number
    Default: 8080
  AzId:
    Type: String
    Default: apne1-az1
    Description: >-
      Passed as an env var, not resolved at runtime -- task metadata
      returns an AZ NAME, not an AZ ID.

Resources:
  ClientTaskDefinition:
    Type: AWS::ECS::TaskDefinition
    Properties:
      Family: ecs-conn-a-client
      RequiresCompatibilities: [MANAGED_INSTANCES]
      NetworkMode: awsvpc
      Cpu: !Ref TaskCpu
      Memory: !Ref TaskMemory
      RuntimePlatform:
        CpuArchitecture: ARM64
        OperatingSystemFamily: LINUX
      ExecutionRoleArn: !Ref ExecutionRoleArn
      TaskRoleArn: !Ref TaskRoleArn
      ContainerDefinitions:
        - Name: app
          Image: !Sub "${RepositoryUri}:${ImageTag}"
          Essential: true
          Command: ["/app/measure-app", "-port", !Sub "${AppPort}", "-procs", "4"]
          Environment:
            - Name: AZ_ID
              Value: !Ref AzId
          PortMappings:
            - Name: app
              ContainerPort: !Ref AppPort
              Protocol: tcp
          LogConfiguration:
            LogDriver: awslogs
            Options:
              awslogs-create-group: "true"
              awslogs-group: /ecs-conn/a-client
              awslogs-region: !Ref AWS::Region
              awslogs-stream-prefix: app
          SystemControls:
            # TIME_WAIT socket buildup found the hard way: --disable-keepalive
            # conditions open a fresh connection per request, exhausting
            # ephemeral ports. tcp_tw_reuse lets a TIME_WAIT socket be reused
            - Namespace: net.ipv4.tcp_tw_reuse
              Value: "1"
            - Namespace: net.ipv4.ip_local_port_range
              Value: "1024 65535"

  # ServerTaskDefinition is identical except Family / awslogs-group.

  ClientService:
    Type: AWS::ECS::Service
    Properties:
      ServiceName: ecs-conn-a-client
      Cluster: !Ref ClusterArn
      TaskDefinition: !Ref ClientTaskDefinition
      DesiredCount: 1
      EnableExecuteCommand: true
      CapacityProviderStrategy:
        - CapacityProvider: !Ref CapacityProviderName
          Weight: 1
      NetworkConfiguration:
        AwsvpcConfiguration:
          Subnets: [!Ref SubnetId]
          SecurityGroups: [!Ref SecurityGroupId]
          AssignPublicIp: DISABLED
      # AvailabilityZoneRebalancing defaults to ENABLED and forbids
      # MaximumPercent<=100, which forces every deploy to briefly need
      # 2x capacity even for a single desiredCount service. Found the
      # hard way: 8 services force-deployed at once burned through the
      # whole vCPU quota mid-rollout.
      AvailabilityZoneRebalancing: DISABLED
      DeploymentConfiguration:
        MinimumHealthyPercent: 0
        MaximumPercent: 100
        DeploymentCircuitBreaker:
          Enable: true
          Rollback: true

Outputs:
  ClientServiceName:
    Value: !GetAtt ClientService.Name
```

### 4.2 CloudFormation stack 結構上的巧思

在用 CloudFormation 建置時，我做了幾個安排。

#### 相衝突路由的處理

VPC Peering 與 TGW 會把同一個對端 VPC CIDR 當成目的地，因此不能在同一張路由表中同時存在。這和前一篇區域間連線時遇到的限制是一樣的。兩個互相衝突的路由項目沒有寫進 CloudFormation，而是用 CLI 處理。

#### 成本對策

中途我把網路層與 endpoint 層分開了。原因是我的技術不足與成本考量。測試比預期更不順，重跑了幾次，變成跨好幾天量測。14 個 VPC endpoint 會持續累積時間費用，但 VPC、subnet、security group 這些基礎資源是免費的。我採取的方式是，把便宜且適合常駐的資源，與量測結束後應該每次刪除的高價資源，按 stack 分開，盡可能壓低費用。同時也手動把 task 數量降到 0，以降低成本並加快下次量測開始的速度。

### 4.3 為什麼選 ECS Managed Instances

這次我使用的是 ECS Managed Instances。理由很單純：Fargate 以前在這類驗證文章裡已經用過很多次了，所以這次想試試 ECS 啟動類型中最後 GA 的 Managed Instances。

我沒有和 Fargate 做速度比較。因為底層本身就是不同的東西（host、ENI 的生成方式、配置都不同），無法直接比較。

Managed Instances 在「放入 task 後會自動準備 instance」這種體驗上很像 Fargate，但本質上是受管理的 EC2。host OS 固定為 Bottlerocket，SSH 無效，也不能指定自訂 AMI 或 placement group。所有量測都透過 ECS Exec 在 container 內執行。

### 4.4 卡住的點：如何實現 1 instance 1 task

文件裡有這段：「如有需要可使用 single task mode－若工作負載需要更強的隔離，可以把 Amazon ECS Managed Instances 設成使用 single task mode。」但我找不到對應的參數。

同一份文件也寫著：「multi-task placement－預設下，Amazon ECS Managed Instances 會為了最佳化成本與使用率，在一個 instance 上放多個 task，因此相較於 Fargate，對工作負載隔離的限制較寬鬆。」這讓我擔心，若多個路徑的 task 共住在同一台 instance 上，比較前提就不成立了。

最後我只好用硬做法：讓 task 的 CPU 與 memory 幾乎等於 instance 容量（ `c7g.xlarge` 4 vCPU / 8192 MiB 對應 task 4 vCPU / 7168 MiB）。只要 task 幾乎把 instance 容量用滿，第二個 task 在物理上就放不進去。我實機確認了把兩個 service 同時丟到同一個 capacity provider 時不會共住，因此就用這個方式往下做。  
※如果有人知道正確作法，請告訴我。

### 4.5 卡住的點：私有子網路的 VPC endpoint 不足

Managed Instances 的 task 放在私有子網路中，而且沒有使用 NAT gateway。一開始我只準備了 ECR、CloudWatch Logs、SSM 相關 endpoint 就部署，結果 `Task timed out after 300` 的錯誤一直出現，連一台 container instance 都沒有註冊成功。

原因是少了讓 instance 本身連到 ECS control plane 所需的 `ecs` 、 `ecs-agent` 、 `ecs-telemetry` endpoint。只有 ECR、logs、SSM 的 endpoint 不夠，instance 無法加入 cluster，因此失敗了。後來我和 AI agent 一起追查，補上這 3 個 endpoint 後就解決了。

這 3 個 endpoint 是 Managed Instances 的特有需求，因為它們是讓實際在 EC2 instance 上運作的 ECS agent 使用的。Fargate 沒有使用者需要自己去意識的 EC2 instance 概念，所以以前只用 Fargate 時不會遇到這種需求。這是我第一次建 Managed Instances 時才注意到的地方。

## 5\. 量測方法

### 5.1 為何把指標統一成 HTTP

這次連線方式比較只用 HTTP 來量。一開始我本來也打算連 TCP connect 建立時間一起量，但後來發現對於中間有 proxy 的路徑沒辦法統一條件，所以作罷。

路徑 D 中，client app 連的是自己 task 裡的 Envoy，因此若量 TCP connect 時間，量到的其實是連到本機 proxy 的時間（預期幾乎為零），數字可能會比路徑 A 還漂亮。路徑 H 的 VPC Lattice service listener 可以選 HTTP、HTTPS、TLS，但沒辦法在與其他方式相同條件下，比較任意的明文 TCP 連線時間。如果只把某些路徑能量到的指標放進比較表，會造成誤解，因此我統一成所有路徑都成立的 HTTP 量測。

request 的協定也固定為所有路徑都是明文 HTTP/1.1。Lattice 雖然可以在 listener 端受管理地終止 TLS，讓應用程式不用改就能選 HTTPS/TLS listener，但這次使用的驗證 app 不支援 TLS，所以無法讓所有路徑都改成 HTTPS。最後就統一維持明文。

### 5.2 量測條件

量測條件如下：

| 項目 | 設定 |
| --- | --- |
| 每個條件的 request 數 | 10,000 |
| 每個條件的試行次數 | 3 |
| keep-alive | ON / OFF 都量 |
| 並行度 | 1 / 16 |
| payload | 所有路徑 1KB；1MB 只量 B、D、G、H |
| 協定 | 所有路徑皆為明文 HTTP/1.1 |
| 統計 | p50 / p95 / p99 |

負載產生使用 oha（Rust 製 HTTP 壓力測試工具）。JSON 輸出則是用 `--output-format json` 。

以下是一個實際執行的 command 範例（路徑 A、1KB payload）。因為 keep-alive ON/OFF 與並行度 1/16 組合起來有 4 種條件，所以會根據 `--disable-keepalive` 是否加入，以及 `-c` 的值不同，執行 4 次。並行 16 時就指定 `-c 16` 。

```
OHA="oha --no-tui --output-format json --worker-threads 4"
URL=http://<server-ip>:8080/bytes/1024

$OHA -n 10000 -c 1  --disable-keepalive $URL > A_ka-off_c1_1k.json
$OHA -n 10000 -c 1                      $URL > A_ka-on_c1_1k.json
$OHA -n 10000 -c 16 --disable-keepalive $URL > A_ka-off_c16_1k.json
$OHA -n 10000 -c 16                     $URL > A_ka-on_c16_1k.json
```

其中 `A_ka-on_c1_1k.json` （keep-alive ON・並行 1）的輸出，只摘錄 metrics 部分如下。

```
{
  "metrics": {
    "success_rate": 1.0,
    "requests_per_sec": 15352.41,
    "latency_ms": {
      "min": 0.052,
      "mean": 0.064,
      "p50": 0.064,
      "p95": 0.07,
      "p99": 0.08,
      "max": 0.329
    }
  }
}
```

同樣格式的輸出，會以 4 種條件 × 3 次試行 × 9 條路徑重複取得。上面貼的是其中一次執行結果。它和第 6 章的表格（3 次平均）會有些微差異，請注意。

### 5.3 卡住的點：百分位數與量測次數

一開始我打算比照前一篇文章做 300 次量測，但實際測下來波動非常大。查了一下後發現，oha 的 p99 實作方式，是把全部 request 由快到慢排序後，直接採用第 0.99×N 個實測值（300 筆就是第 297 筆）。也就是說，300 筆時，p99 其實就是倒數第 3 慢那一筆的單次實測值。只要 runtime 的 GC 或 OS scheduling 造成的瞬間延遲，剛好落在那一筆，p99 就會大幅震盪。  
試跑時我把次數提高到 3,000，並對同一條件重複 3 次，但 p99 相對平均值最大仍偏到 17%，還是不夠。再提高到 10,000 次並重複 3 次後，波動才收斂到約 4%，因此最後所有模式都以 10,000 次來量。

### 5.4 卡住的點：keep-alive OFF 時的 TIME\_WAIT 耗盡

在持續重測的過程中，我碰到在 keep-alive OFF 條件下，約每 3～4 次就會有 1 次 p50 突然飆高 3～25 倍的現象。p99 更是偏差更大。原因不明之下，我也考慮了資源不足的可能性，因此啟用 ECS Container Insights 檢查資源，結果發現 client 端 CPU 使用率在異常發生時會暴增接近 5 倍。

因為無法立刻定位原因，我請 AI agent 協助分析並繼續追查，最後找到解答。使用 `--disable-keepalive` 的量測會在每個 request 建立新的 TCP 連線，因此 10,000 次 request 會在 client 端消耗大量 port，而在連線結束後，最多還會以 TIME\_WAIT 狀態殘留在 kernel 中約 60 秒。由於量測重複 3 次，如果兩次量測間隔太短，TIME\_WAIT 還沒自然釋放，port 就會一路累積，到了累積大約 14,000 筆之後，新 port 的取得成本就會突然升高。

我在 task 定義的 `SystemControls` 中設定 `net.ipv4.tcp_tw_reuse=1` ，允許立即重用 TIME\_WAIT socket 後，即使同一條件連續執行 6 次，p50 也不再飄動。  
這個設定是在驗證環境中直接套用，但是否適合正式環境，會受通訊路徑、kernel 版本、是否有 NAT 或 load balancer 等網路相關元件影響，因此需要個別驗證。本文只是把它當成穩定量測環境的設定來使用。  
另外，在追查過程中，我也想起以前曾經發生過因為 port 耗盡而無法建立新 session 的事件。

在把這個對策套用到所有路徑後，重新進行 keep-alive OFF 的量測，p99 的波動（3 次量測平均值的最大偏差）從對策前最大 249% 改善到對策後最大 33.7%。第 6 章的結果就是採用這個對策後的資料。

## 6\. 結果

以下進入實測結果。單位全部都是毫秒。

### 6.1 連線方式比較（1KB、10,000 request × 3 次平均）

#### keep-alive ON 時（1KB，單位：毫秒）

| 路徑 | p50 | p95 | p99 | 並行 16 p50 | 並行 16 p99 |
| --- | --- | --- | --- | --- | --- |
| A（同一 VPC 直接） | 0.065 | 0.072 | 0.085 | 0.104 | 0.249 |
| B（Peering 直接） | 0.077 | 0.082 | 0.091 | 0.105 | 0.209 |
| D（Peering + Service Connect） | 0.463 | 0.508 | 0.564 | 1.614 | 2.792 |
| F（TGW 直接） | 0.257 | 0.271 | 0.285 | 0.212 | 0.395 |
| G（PrivateLink） | 0.509 | 0.559 | 0.609 | 0.527 | 0.718 |
| H（VPC Lattice） | 1.330 | 1.712 | 1.872 | 1.221 | 1.858 |

在 keep-alive ON、並行 1 的情況下，可以看出速度依序變慢的順序是 A→B→F→D→G→H。無論看 p50、p95 或 p99，順序都一樣。以下差異都以 p50 的差為準。  
同一 VPC 直接連線（A）與 Peering 直接連線（B）幾乎相同，B−A 的差約 0.012 ms，非常小。Service Connect（D）因為多了 Envoy sidecar 兩跳，因此比 B 慢約 0.39 ms。PrivateLink（G）與 VPC Lattice（H）則更慢，其中 H 最慢。

雖然順序不變，但差距大小會隨百分位數而變化。主要差異整理如下（keep-alive ON、並行 1，單位毫秒）。

#### 各路徑百分位數整理

| 差異 | 意義 | p50 | p95 | p99 |
| --- | --- | --- | --- | --- |
| B−A | 跨 VPC 的基本成本 | 0.012 | 0.010 | 0.006 |
| F−B | TGW hop 成本 | 0.180 | 0.189 | 0.195 |
| D−B | sidecar 兩跳成本 | 0.387 | 0.426 | 0.473 |
| G−B | PrivateLink + NLB 經由結構相對於 Peering 直接連線的差異 | 0.432 | 0.477 | 0.518 |
| H−B | VPC Lattice 成本 | 1.253 | 1.630 | 1.781 |
| G−G′ | 在相同 NLB 架構下，經過 PrivateLink 所增加的時間 | 0.108 | 0.134 | 0.139 |

如果把各自的 p50 設為 100 來畫成相對值圖，趨勢會更清楚。

[![percentile-gap-trend.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F0a4e4b6c-7fc6-4564-a126-11a405acec3c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=989f04c23bcfb953384badc68429c9ee)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2F0a4e4b6c-7fc6-4564-a126-11a405acec3c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=989f04c23bcfb953384badc68429c9ee)

VPC Lattice（H）在較慢的 request 上，與其他方式的差距更大。相較於 Peering 直接連線，H−B 在 p50 是 1.25 ms，但到 p99 變成 1.78 ms，差距擴大到 1.4 倍。與 PrivateLink 比較也有同樣傾向，p50 是 0.82 ms，而 p99 變成 1.26 ms，約 1.5 倍。若要選擇 Lattice，最好不要只看 p50 的印象，也要把慢 request 的差異納入考量。

補充實驗的 G−G′（PrivateLink 所增加的部分）也一樣，p50 是 0.108 ms，到 p99 擴大到 0.139 ms，約 1.3 倍。這次量測的 PrivateLink 與 VPC Lattice，都呈現出相較於基準構成的差異在 p99 比 p50 更大的傾向。

相反地，只有 B−A 在較慢的 request 上反而差距縮小了（p50 0.012 ms、p99 0.006 ms）。同一 VPC 與 VPC Peering 的差異在慢端幾乎可以說很小。

上述順序是並行 1 時的結果。當並行度提高到 16 時，順序改變了，出現 Service Connect（D）比 VPC Lattice（H）更慢的逆轉。把並行 1 與並行 16 的 p50 排在一起如下。

#### 並行度造成的 p50 變化（keep-alive ON・1KB）

| 路徑 | 並行 1 | 並行 16 | 倍率 |
| --- | --- | --- | --- |
| A（同一 VPC 直接） | 0.065 | 0.104 | 1.6 倍 |
| B（Peering 直接） | 0.077 | 0.105 | 1.4 倍 |
| F（TGW 直接） | 0.257 | 0.212 | 0.8 倍 |
| D（Service Connect） | 0.463 | 1.614 | 3.5 倍 |
| G（PrivateLink） | 0.509 | 0.527 | 1.0 倍 |
| H（VPC Lattice） | 1.330 | 1.221 | 0.9 倍 |

[![concurrency-shift.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Ffce8d31e-21cf-432d-b379-e944d65a9ffd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f793f8aef2fe39b8504ca0332bd9318b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Ffce8d31e-21cf-432d-b379-e944d65a9ffd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f793f8aef2fe39b8504ca0332bd9318b)

即使並行度拉高到 16，大多數路徑的 p50 幾乎都沒有變。TGW（F）與 VPC Lattice（H）在並行 16 時數值較小，但以這次的試行次數來看，還不到足以斷言改善的程度。其中只有 Service Connect（D）惡化 3.5 倍，並且超過 PrivateLink（G）與 VPC Lattice（H），變成最後一名。

這只是推測，但我認為 Service Connect 因為 client 與 server 兩端 task 內都要經過 Envoy sidecar，所以當並行度提高時，task 內有限的 CPU 與 memory 會被應用容器與 Envoy 搶用，較容易形成瓶頸。另一方面，Lattice 因為經過的是 AWS 管理的外部 data plane，較不受單一 task 資源限制影響，所以並行度升高時不像 D 那麼慢。以上只是我的推測，請留意。

以上是 keep-alive ON 的結果。把它關掉後，趨勢就改變了。

**keep-alive OFF 時（1KB，單位：毫秒）**

| 路徑 | p50 | p95 | p99 | 並行 16 p50 | 並行 16 p99 |
| --- | --- | --- | --- | --- | --- |
| A（同一 VPC 直接） | 0.222 | 0.272 | 0.293 | 0.357 | 0.645 |
| B（Peering 直接） | 0.235 | 0.293 | 0.300 | 0.371 | 0.652 |
| D（Peering + Service Connect） | 0.589 | 0.651 | 0.707 | 2.403 | 4.224 |
| F（TGW 直接） | 0.572 | 0.931 | 1.104 | 0.623 | 1.156 |
| G（PrivateLink） | 2.021 | 2.594 | 2.800 | 1.909 | 2.706 |
| H（VPC Lattice） | 2.621 | 3.710 | 4.051 | 2.256 | 3.366 |

[![keepalive-shift.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Ff56a7399-e64b-4989-baa9-a5805de715f2.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fb9432c46bf3b19bf718a49e4c949013)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3925746%2Ff56a7399-e64b-4989-baa9-a5805de715f2.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fb9432c46bf3b19bf718a49e4c949013)

關掉 keep-alive 後所有路徑都會變慢，但各路徑受到的影響差很多。影響最大的是 PrivateLink（G），從 0.509 ms 增加到 2.021 ms，惡化 4.0 倍。接著是同一 VPC 直接（A）變成 3.4 倍、Peering 直接（B）變成 3.1 倍。

相反地，受影響最小的是 Service Connect（D），只到 1.3 倍。我推測是因為 client 連的是自己 task 內的 Envoy，所以即使關掉 keep-alive，斷掉的也只是本地 proxy 的連線，而 Envoy 仍維持與 server 端的連線。VPC Lattice（H）只有 2.0 倍，介於中間，也可能是因為它同樣經由受管理的 data plane。不過這只是尚未驗證的假說。

PrivateLink 在 keep-alive OFF 時受影響特別大的原因，可能是每次新連線都會經過 Interface VPC Endpoint 與 NLB 的連線處理。不過本次驗證沒有直接觀測內部處理，因此無法確定原因。這次量測顯示 keep-alive ON 時影響較小，但如果是每次都建立新連線的使用方式，就要把這個差異納入考量。

### 6.2 補充實驗：在 NLB 直接連線上加上 PrivateLink 會多幾 ms

G 與 G′（Peering + Internal NLB direct）共用同一個 server task，只是 client 的目的地不同。取 G−G′ 就能把透過 PrivateLink 所增加的時間單獨抽出來。

| 路徑 | keep-alive ON p50 | keep-alive OFF p50 |
| --- | --- | --- |
| G′（NLB 直接） | 0.401 | 1.532 |
| G（PrivateLink） | 0.509 | 2.021 |

以 p50 來看，keep-alive ON 時 PrivateLink 多約 +0.11 ms，OFF 時多約 +0.49 ms。我推測 PrivateLink 的 Interface Endpoint 不是單純的路由，而是包含能支援 CIDR 重疊的雙向 NAT 類型服務連線機制，因此才會出現這個差異。

### 6.3 AZ 配置的影響（L2−L1）

除了連線方式比較外，我也把 server task 放到不同 AZ，測了跨 AZ 時的影響。指標是 TCP connect 建立時間。

| 路徑 | p50 | p99 |
| --- | --- | --- |
| L1（同一 AZ，= 路徑 A） | 0.260 | 0.318 |
| L2（跨 AZ） | 1.151 | 1.237 |

跨一個 AZ 後，TCP connect 建立時間在 p50 約增加 0.89 ms。雖然與第 6.1 的 HTTP latency 不是同一種指標，不能直接比較，但至少可以看出，對於需要頻繁建立新連線的工作負載來說，AZ 配置會有不能忽視的影響。

### 6.4 吞吐量（1MB、並行 16）

看 1MB payload、並行 16 時的 RPS，B、D、G、H、G′ 都收斂在約 1,470～1,480 RPS。換算下來，1MB（1,048,576 bytes）× 1,477 RPS ≒ 12.4 Gbps。  
我又另外用 iperf3 重新測了單一 stream 與 16 並行 stream 的頻寬上限，單一 stream 是 9.53 Gbps，16 並行 stream 是 12.40 Gbps，與 oha 在 16 並行下的實測一致。也就是說，在這個條件下，上限不是連線方式，而是驗證所用 instance 型號（ `c7g.xlarge` ，官方標示網路效能 12.5 Gbps）本身的網路頻寬。  
最後我放棄了吞吐量評估。

## 7\. 考察

結果大致整理完了，下面把前面零散寫下的觀察與看法彙整一下。

### 7.1 與基準的差異

若以 keep-alive ON・並行 1 的 p50 來看，大致可以這樣理解。以下差異都以 p50 之間的差為準。

- A→B（約 +0.01 ms）：在這次環境與條件下，同一 AZ 內經由 VPC Peering 的延遲增加非常小。
- B→D（約 +0.39 ms）：這是多了 Envoy sidecar 兩跳的成本。因為 client 與 server 端各自都有 local proxy，出現這樣的增加算合理。
- B→F（約 +0.18 ms）：這是經過 TGW 的成本。比 Peering direct 慢。前一篇區域間驗證也得到 TGW 比 Peering 更慢的結果，這次同一 AZ 內也有相同趨勢。
- B→G（約 +0.43 ms）：這是 Peering 直接連到 task IP，與經由 PrivateLink 及 NLB 的構成之間的差異。PrivateLink 增加的部分，會用同一個 NLB 與 server task 的 G−G′ 來確認。
- G→H（約 +0.82 ms）：VPC Lattice 比 PrivateLink 更慢。我推測是因為 Lattice 會經過 service 級路由等比 PrivateLink 更多的處理。
- B→H（約 +1.25 ms）：經由 VPC Lattice 的構成，比 Peering 直接連 task IP 更慢，也是這次比較中差異最大的。絕對值約增加 1.25 ms，但因為 B 原本只有 0.077 ms，所以相對上差很多。老實說，我原本預期它會更接近其他方式。

### 7.2 連線方式差異雖是毫秒級，但會因條件而改變趨勢

在這次環境中，連線方式造成的 p50 絕對差大致都落在毫秒級。另一方面，相對值上不同方式之間差很多，而且並行度與 keep-alive 有無也會讓排名和差距大小改變。  
如果工作負載可以接受幾毫秒的增加，那就不必過度重視速度，應該把連通性、認證與授權、可觀測性、營運負擔、成本等一起考量。反過來說，若是微秒級延遲很重要的工作負載，在串連多次 request、差異會逐步累積時，各方式之間的差距就不能忽視。  
老實說，我原本認為 VPC Lattice 跟其他方式幾乎沒差，所以實測後發現它比預期更明顯地慢於其他服務，這點是這次做完才知道的。

## 8\. 注意事項

這份驗證是個人實作，不能保證絕對嚴謹，請注意。

- 每個條件都是 10,000 次 request 測 3 次後的平均值。和前一篇一樣，沒有做時間帶、星期等統計層面的量測與確認。
- 在同一次量測 session 中，instance 是固定的，因此無法涵蓋不同 instance 之間的個體差異。

## 9\. 總結

這次我在東京區域建置了 ECS 服務間通訊的 6 種方式：同一 VPC 的直接連線、VPC Peering 的直接連線、ECS Service Connect、Transit Gateway、AWS PrivateLink、Amazon VPC Lattice，並在同一 AZ 配置下實測延遲。

就延遲而言，在 keep-alive ON、並行 1 的 p50 中，可以確認速度依序變慢為：同一 VPC 直接（0.065 ms）→ Peering 直接（0.077 ms）→ TGW 直接（0.257 ms）→ Service Connect（0.463 ms）→ PrivateLink（0.509 ms）→ VPC Lattice（1.330 ms）。絕對差距雖然是毫秒級，但相對上差異很大，而且隨著並行度與 keep-alive 有無，趨勢也會改變。

另一方面，在跨 AZ 的配置下，TCP connect 建立時間的 p50 約增加 0.89 ms。雖然這和 HTTP latency 不是同一個指標，不能直接比較，但對於大量建立新連線的工作負載來說，除了連線方式之外，AZ 配置也是重要的評估要素，這點再次被證實了。

在驗證過程中，我也遇到了不少回頭重做的情況，包括 Managed Instances 的 1 instance 1 task 化、私有子網路 VPC endpoint 不足、百分位數與 request 數的關係、網路頻寬上限、keep-alive OFF 時 TIME\_WAIT 耗盡等。這些都是實際動手後才會看見的問題，因此我盡量記錄下來，希望能提供同樣做這類驗證的人參考。除此之外還有不少小失誤，所以比想像中花了更多時間。

從這次結果來看，選擇連線方式不能只看速度，還必須一起考量目標延遲、並行度、連線重用、CIDR 設計、認證與授權、營運管理便利性、成本等。接下來我想再整理一下 ECS 服務間通訊方式的選用指引，包含 task 擴縮容與 rolling deployment 時的行為。

希望這篇對正在考慮 ECS 服務間通訊方式的你有所幫助。

---

原文出處： [https://qiita.com/sh\_fukatsu/items/524a76ad04930e774829](https://qiita.com/sh_fukatsu/items/524a76ad04930e774829)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/ayo4Ox) [14瀏覽](https://codelove.tw/@tony/post/ayo4Ox)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/ayo4Ox)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝21

324

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788745927&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayo4Ox&amp;pra=5&amp;aiof=11&amp;asro=0&amp;itsi=40&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788745927903&amp;bpp=1&amp;bdt=95&amp;idt=48&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788743511%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788743511%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788743511%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1329362303022&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2569&amp;eid=95399629%2C95400775%2C95401045%2C122880828&amp;oid=2&amp;pvsid=2282960364032298&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=53" title="Advertisement" aria-label="Advertisement"></iframe>