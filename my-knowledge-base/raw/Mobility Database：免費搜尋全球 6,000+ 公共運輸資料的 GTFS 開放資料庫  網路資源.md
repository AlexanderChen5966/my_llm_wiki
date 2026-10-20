---
title: "Mobility Database：免費搜尋全球 6,000+ 公共運輸資料的 GTFS 開放資料庫 | 網路資源"
source: "https://tools.wingzero.tw/articles/4026"
author:
  - "[[赤紅翼零]]"
published: 2026-08-15
created: 2026-08-17
description: "Mobility Database 是由 MobilityData 維護的全球公共運輸開放資料庫，收錄超過 6,000 個 GTFS、GTFS Realtime 與 GBFS Feed，涵蓋 99 個以上國家。"
tags:
  - "clippings"
status: "processed"
---
網站連結 [https://mobilitydatabase.org/](https://mobilitydatabase.org/)

Mobility Database 是一個專門收集全球公共運輸與共享運具開放資料的資料庫，目前收錄超過 6,000 個 GTFS、GTFS Realtime 與 GBFS Feed，涵蓋 99 個以上國家。使用者可以直接搜尋城市、交通營運商或 Feed，取得公車、捷運、鐵路、自行車共享等交通服務的標準化資料，進一步用於路線規劃 App、交通研究、資料分析與智慧城市應用。

它與 Google Maps、Moovit 等交通查詢服務的定位不同。Mobility Database 並不是直接告訴一般乘客「下一班公車幾點到」，而是提供產生這些服務所需的底層交通資料。對 App 開發者、研究人員、政府與交通業者而言，它比較像一個全球公共運輸 Open Data Catalog，將原本分散在各城市與營運商網站上的資料集中整理，並提供 API 與資料品質檢查工具。

Mobility Database 由非營利組織 MobilityData 維護。MobilityData 本身長期參與 GTFS、GBFS 等開放交通資料標準的發展與治理，因此這個網站並非單純把網路上的 CSV、JSON 連結集中在一起，而是與交通資料標準及驗證工具形成完整生態。

理解 Mobility Database 前，可以先認識它主要收錄的三種資料。GTFS（General Transit Feed Specification）是公共運輸資料的通用格式，可以描述路線、車站、班次與時刻表等資訊；GTFS Realtime 則處理即時資訊，例如車輛位置、班次更新與服務異常；GBFS（General Bikeshare Feed Specification）則主要用於共享運具，目前應用範圍已不只公共自行車，也包含 Scooter、Moped 與 Car Sharing 等服務。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="784" height="345" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=199514577&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786929547&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4026%23google_vignette&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786929543402&amp;bpp=7&amp;bdt=1896&amp;idt=7&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1786929525%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1786929525%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1786929525%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x908%2C784x280&amp;nras=3&amp;correlator=2388251552344&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=2931&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95399245%2C122880126%2C122880398%2C42533294&amp;oid=2&amp;pvsid=7521985188386789&amp;tmod=1626592820&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=2&amp;fsb=1&amp;dtd=3686" title="Advertisement" aria-label="Advertisement"></iframe>

換句話說，如果正在開發一個「顯示某城市所有公車站」的 App，GTFS 可以提供基礎資料；如果還要顯示車輛目前在哪裡，就需要 GTFS Realtime。若產品還想加入共享單車或其他共享運具資訊，GBFS 就能補上這部分。

目前 Mobility Database 比較重要的功能包括：

- 收錄超過 6,000 個全球交通與共享運具 Feed
- 涵蓋 99 個以上國家
- 支援 GTFS Schedule、GTFS Realtime 與 GBFS
- 可依交通營運商、Feed 名稱或地點搜尋
- 提供歷史與目前版本的交通資料
- 每日檢查 Feed 是否有新版本
- 提供 API，方便程式自動取得資料
- 整合 GTFS Schedule Validator 與 GBFS Validator
- 可查看 Feed 的 Bounding Box 與涵蓋範圍
- 開放交通業者與社群新增或更新 Feed。

其中「每日更新檢查」很實用。官方會在每天午夜 UTC 檢查資料提供者登記的 Feed URL，如果內容發生改變，就會自動加入新的 Feed Version；GBFS 則另外會配合 `systems.csv` Catalog 的變化同步。這能降低開發者自己逐一追蹤各交通局或營運商資料網址的工作量。

另一項容易被忽略的特色是 Feed Mirror。交通資料最麻煩的地方之一，是來源通常仍由各個營運商自行 Hosting。如果營運商網站暫時離線、網址變更或存在地區限制，依賴原始 URL 的服務就可能突然取得不到資料。Mobility Database 會保存營運商 GTFS Schedule Feed 的 Mirror Version，降低來源網站暫時故障或 Geoblocking 帶來的影響。

對開發者而言，真正讓 Mobility Database 從「資料目錄」變成基礎設施的則是 API。官方允許透過 API 搜尋及取得資料，並提供相對穩定的 URL。一般使用者瀏覽與下載資料不需要付費，但若要使用 API 或新增 Feed，則需要建立帳號。

這使它很適合拿來建立交通資料 Pipeline。例如定期透過 API 找到特定區域的 GTFS Feed，下載後寫入資料庫，再計算站點密度、路線覆蓋率或班次頻率。若結合地圖服務，也能製作自己的 Transit Map、Trip Planner 或 Mobility Dashboard。

資料品質是另一個值得注意的部分。Mobility Database 與 Canonical GTFS Schedule Validator、GBFS Validator 整合，因此部分 Feed 頁面可以直接看到 Validation Report，而不是下載資料後才發現格式存在問題。實際 Feed 頁面也會列出營運商、涵蓋地區、Feed URL、License、支援功能與品質報告等資訊。

對交通營運商來說，這些 Validator 也有另一層用途。業者可以先檢查自己發布的 GTFS 或 GBFS 是否符合規格，再交給 Trip Planner 或 Navigation App 使用，減少資料格式錯誤造成的來回修改。官方也鼓勵營運商搜尋其他 Feed，參考不同機構如何製作 GTFS Schedule、GTFS Realtime 與 GBFS。

Mobility Database 還承接了一個交通開放資料社群的重要歷史角色：TransitFeeds.com。官方 FAQ 說明，Mobility Database 在 2024 年 2 月推出並取代 TransitFeeds，而 TransitFeeds 的歷史資料已於 2025 年 12 月遷移至 Mobility Database。因此，對過去曾使用 TransitFeeds 尋找 GTFS 的開發者而言，Mobility Database 可以視為現在主要的延續平台。

從實際應用來看，它的使用範圍相當廣。交通 App 開發者可以取得各城市大眾運輸資料；研究人員能分析城市交通網路與服務覆蓋；資料工程師可以建立全球 GTFS Dataset；政府或都市規劃團隊則能比較不同地區的公共運輸服務。若是 AI 開發者，也可以將 GTFS 與 GBFS 資料整理後提供給 Agent 或 RAG 系統，建立能理解地方交通網路的應用。

GBFS 的加入也讓 Mobility Database 不再只處理傳統大眾運輸。GBFS 本身是一套面向共享運具的即時或近即時、唯讀開放資料標準，主要目的之一就是讓乘客端應用程式取得共享運具資訊。這意味著同一個交通產品可以同時整合公車、捷運與共享單車等不同交通方式。

若與各國政府 Open Data Portal 相比，Mobility Database 最大的差異是「跨國與標準化」。政府資料平台通常只處理自己轄區內的 Dataset，而且格式可能五花八門；Mobility Database 則集中在 GTFS、GTFS Realtime 與 GBFS 這些國際交通標準。對需要一次處理十個甚至數十個城市的服務而言，標準化比單純「有資料可以下載」更重要。

不過，Mobility Database 也不是所有交通資料問題的答案。它主要是 Feed Catalog 與資料取得入口，並不是完整的 Route Planning Engine，也不會直接替開發者完成路線演算法、地圖視覺化或乘客端 App。不同 Feed 的實際資料完整程度與品質仍取決於原始交通業者，因此正式導入產品前，仍需要查看 License、Validation Report 與資料涵蓋情況。

整體而言，Mobility Database 最有價值的地方，是把原本散落全球的公共運輸與共享運具資料集中到一個可搜尋、可驗證、可透過 API 使用的入口。對一般乘客而言，它可能不是每天會打開的交通網站；但對 GTFS 開發者、交通研究人員、智慧城市團隊與 Mobility App 公司來說，這類資料庫能省掉大量尋找 Feed、追蹤網址與確認格式的工作。

<iframe frameborder="0" src="https://c4a15179df83a30be66d2b336212c2fd.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe><iframe frameborder="0" src="https://c4a15179df83a30be66d2b336212c2fd.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>