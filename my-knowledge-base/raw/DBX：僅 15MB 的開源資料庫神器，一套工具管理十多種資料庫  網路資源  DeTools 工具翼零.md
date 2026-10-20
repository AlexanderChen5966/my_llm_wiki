---
title: "DBX：僅 15MB 的開源資料庫神器，一套工具管理十多種資料庫 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3953"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-21
created: 2026-06-22
description: "DBX 是輕量級開源資料庫管理工具，支援 MySQL、PostgreSQL、MongoDB 等多種資料庫，內建 AI SQL 助手、ER 圖生成與資料分析功能，提供高效且現代化的開發體驗。"
tags:
  - "clippings"
status: processed
---
DBX 是一款開源、跨平台且輕量化的資料庫管理工具（Database Client），主打小體積、高效能與現代化介面設計。它的核心目標相當直接：提供一個能取代部分商業資料庫工具的選擇，讓開發者不必安裝數百 MB 的軟體，也能管理多種資料庫系統。根據專案介紹，DBX 安裝包僅約 15MB，但支援超過十多種主流資料庫。

近年資料庫管理工具市場幾乎被幾個大型產品主導，例如 DataGrip、DBeaver 與 TablePlus。這些工具功能完整，但往往伴隨較大的安裝體積、較高記憶體使用量，甚至需要訂閱付費。DBX 則選擇另一條路線，希望透過現代技術架構打造一款快速啟動、開源且免費的資料庫客戶端。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=828129217&amp;pi=t.aa~a.2909957178~i.5~rp.4&amp;abgtt=13&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1782104998&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3953&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782104998662&amp;bpp=1&amp;bdt=600&amp;idt=0&amp;shv=r20260617&amp;mjsv=m202606170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1782104868%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1782104868%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1782104868%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=825x280%2C0x0%2C255x600%2C255x600&amp;nras=2&amp;correlator=5707308448317&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=11&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=285&amp;ady=1307&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099258%2C95393485&amp;oid=2&amp;pvsid=1731713626148548&amp;tmod=2049643337&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Fweb%2Fpage%2F1&amp;fc=384&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=247x742_l&amp;bz=1.14&amp;pgls=CAs.&amp;ifi=16&amp;uci=a!g&amp;btvi=3&amp;fsb=1&amp;dtd=3" title="Advertisement" aria-label="Advertisement"></iframe>

從產品定位來看，DBX 並不只是 SQL 查詢工具，而是一套整合資料瀏覽、資料編輯、ER 圖生成與 AI 輔助能力的資料庫工作台（Database Workspace）。它支援的資料庫種類相當廣泛，包括：

- MySQL
- PostgreSQL
- SQLite
- MariaDB
- SQL Server
- Oracle
- Redis
- MongoDB
- DuckDB
- ClickHouse
- TiDB
- StarRocks
- Doris
- OceanBase
- openGauss

這種多資料庫支援能力，讓開發者不需要為不同資料庫安裝不同工具。

在功能設計上，DBX 最受關注的特色之一是內建 AI SQL Assistant。

使用者可以串接：

- OpenAI API
- Claude API

讓系統根據資料表結構、自動分析 Schema、欄位關係與索引資訊，再透過自然語言產生 SQL。

例如輸入：

> 查詢最近 30 天註冊且有下單紀錄的用戶

AI 可以直接生成對應 SQL。

除了產生查詢語法之外，也支援：

- SQL 解釋
- SQL 優化
- SQL 錯誤修正
- Query 分析

對於不熟悉 SQL 或需要快速分析資料的人來說相當實用。

另一個亮點是 ER Diagram 生成功能。

許多資料庫管理工具需要額外插件才能建立資料關聯圖，而 DBX 可以直接從資料庫結構產生：

- Table Structure Diagram
- Entity Relationship Diagram（ERD）

並支援：

- 拖曳瀏覽
- 縮放
- SVG 匯出

對於理解大型系統資料結構相當有幫助。

在資料操作方面，DBX 提供類似試算表的編輯體驗。

使用者可以直接於資料表中：

- 新增資料
- 修改資料
- 刪除資料

所有變更都會以顏色區分：

最後再統一提交，降低誤操作風險。

對資料分析人員而言，另一個實用功能是檔案預覽。

DBX 內建 DuckDB 引擎，因此可以直接拖曳：

到程式中瀏覽資料，而不需要先匯入資料庫。

這種設計讓它同時具備資料庫工具與輕量資料分析工具的特性。

安全性也是專案相當強調的一環。

DBX 提供：

例如執行：

```
DROP TABLE
DELETE
TRUNCATE
ALTER
```

等高風險指令時，系統會額外要求確認，避免誤刪資料。

整體而言，DBX 是近年相當值得關注的新興開源資料庫客戶端。它透過 Tauri 與 Rust 建構出輕量、高效且現代化的資料庫管理體驗，同時整合 AI SQL 助手、ER 圖生成、檔案分析與多資料庫支援能力。對於不想被商業授權綁定、又希望擁有現代開發體驗的使用者而言，DBX 提供了一個兼顧效能與功能性的開源選擇。