---
title: "TREK：自架式旅遊規劃平台，把行程、地圖、分帳與旅遊日誌整合在一起 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3965"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-07-01
created: 2026-07-02
description: "TREK 是開源自架旅遊規劃工具，整合每日行程、互動地圖、旅費分帳、行李清單、文件管理與多人即時協作，支援 Docker 部署，打造完整的私有旅行管理平台。"
tags:
  - "clippings"
status: processed
---
TREK 是一個可自行部署的開源旅遊規劃工具，核心用途是讓使用者在自己的伺服器上建立旅行計畫、安排每日行程、管理地圖景點、預算、行李清單、訂位資料與旅遊日誌。它的定位不是一般旅遊推薦網站，也不是訂房或機票平台，而是偏向「自架版 Trip Planner」，讓個人、家庭或團體能在私有環境中共同規劃旅程，並保有資料掌控權。

相較於 Google Maps 清單、Notion 行程表或共用試算表，TREK 更重視完整旅遊流程管理。它支援即時協作、互動地圖、預算分攤、打包清單、文件管理、天氣預報、PWA 安裝與 SSO 登入，並可透過 Docker 快速部署。專案 README 將它描述為「self-hosted, real-time collaborative travel planner」，也就是一套可自架、支援即時多人協作的旅行規劃系統。

從功能面來看，TREK 的核心特色包括：

- 拖放式每日行程規劃，可跨天移動景點
- Leaflet 或 Mapbox GL 互動地圖，支援 3D 建築、地形、照片標記與路線視覺化
- Google Places 或 OpenStreetMap 景點搜尋
- 匯入 Google Maps／Naver Maps 清單，以及 GPX、KML、GeoJSON 地圖檔
- 航班、住宿、餐廳等訂位資料管理
- Splitwise 風格旅費追蹤與多人分帳
- 行李清單、待辦事項、文件附件與 PDF 匯出
- WebSocket 即時同步、多人邀請、角色權限與群組協作
- 支援 OIDC SSO、2FA、Passkeys 與管理後台
- 可作為 PWA 安裝到 iOS 或 Android 使用

這些功能顯示 TREK 並不是單純的行程表，而是把旅遊規劃、地圖、協作、財務與文件管理整合成同一套系統。

實際使用情境上，TREK 適合正在規劃多人旅行、自由行、自駕旅遊或長期旅程的人。例如一群朋友可以共同建立行程，把景點拖到不同日期中，根據地圖位置調整路線，紀錄每天備註，分攤住宿與餐費，並把機票、訂房確認信、門票 PDF 等文件集中管理。旅程結束後，也可以透過 Journey 模組建立旅遊日誌，整理照片、地圖與回憶紀錄。

TREK 也提供一些進階附加功能，例如 Atlas 可記錄去過的國家、bucket list 與旅行統計；Vacay 可管理假期與國定假日；AirTrail 則能連接自架 AirTrail 匯入航班資料。比較特別的是，它也內建 MCP Server，讓 AI 助理能透過 OAuth 2.1 存取 TREK，並用工具建立旅程、規劃每日路線、產生打包清單或整理預算，但這只是其自動化能力之一，不代表 TREK 本身是瀏覽器 Agent。

技術架構方面，TREK 使用 Node.js、NestJS、SQLite、React、Vite、TypeScript、Tailwind、Leaflet 與 Docker，並透過 WebSocket 提供即時同步。官方提供 Docker、Docker Compose 與 Helm/Kubernetes 部署方式，適合放在 NAS、VPS、家庭伺服器或私有雲中使用。

整體而言，TREK 是一套功能相當完整的開源自架旅遊規劃平台。它的價值不在於幫你訂票或推薦行程，而是在於把旅遊規劃中常見的地圖、每日安排、協作、預算、文件、行李與日誌集中到同一個私有系統。對於重視資料自主、喜歡自架工具，或經常和多人一起規劃自由行的使用者來說，TREK 是一個相當值得關注的開源選擇。

![](https://farm66.static.flickr.com/65535/55367373386_a0e7728469_b.jpg)

![](https://farm66.static.flickr.com/65535/55367373391_edbd4583cf_b.jpg)

![](https://farm66.static.flickr.com/65535/55367526728_25b68efb99_b.jpg)