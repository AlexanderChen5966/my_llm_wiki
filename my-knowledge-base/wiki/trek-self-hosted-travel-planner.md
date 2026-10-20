---
title: TREK：自架式即時協作旅遊規劃平台
date: 2026-07-03
tags: [dev/architecture, type/concept]
source: https://tools.wingzero.tw/article/sn/3965
type: article
status: processed
asset_value: 42
---

TREK 是開源、可自架的旅遊規劃工具（self-hosted, real-time collaborative travel planner），把每日行程、互動地圖、旅費分帳、行李清單、文件管理、旅遊日誌整合成同一套私有系統。定位不是訂房/推薦網站，而是「自架版 Trip Planner」，保有資料掌控權。

> 註：旅遊工具非本知識庫核心焦點，但其內建 MCP Server（供 AI 助理操作）值得作為 self-hosted + MCP 整合的案例備查。

## T（核心論點）

相較 Google Maps 清單、Notion 行程表或共用試算表，TREK 重視完整旅遊流程管理：即時多人協作、互動地圖、預算分攤、文件集中——把旅遊規劃的地圖/安排/協作/財務/文件/日誌集中到同一個可自架的私有系統。

## B（適用場景）

- 多人旅行、自由行、自駕、長期旅程的協作規劃
- 重視資料自主、喜歡自架工具（NAS/VPS/家庭伺服器/私有雲）

## R（主要內容）

**核心功能**：拖放式每日行程（跨天移動景點）、Leaflet/Mapbox GL 互動地圖（3D 建築/地形/照片標記）、Google Places/OSM 搜尋、匯入 Google/Naver Maps 清單與 GPX/KML/GeoJSON、航班住宿餐廳訂位管理、Splitwise 風格分帳、行李清單/待辦/文件附件/PDF 匯出、WebSocket 即時同步、角色權限、OIDC SSO/2FA/Passkeys、PWA 安裝。

**進階模組**：Atlas（去過國家/bucket list/統計）、Vacay（假期管理）、AirTrail（航班匯入）、Journey（旅遊日誌）。

**內建 MCP Server**：讓 AI 助理透過 OAuth 2.1 存取 TREK，用工具建立旅程、規劃路線、產打包清單、整理預算（但只是自動化能力之一，非瀏覽器 Agent）。

**技術棧**：Node.js、NestJS、SQLite、React、Vite、TypeScript、Tailwind、Leaflet、WebSocket；Docker/Compose/Helm 部署。

## C（對自身的啟發）

- [ ] TREK 內建 MCP Server 讓 AI 對它下工具指令，是「應用程式暴露 MCP 供 Agent 操作」的實例（對照 OpenSEO/kit 的 MCP 設計）
- [ ] self-hosted + OAuth 2.1 的 MCP 存取模式，可作為安全整合參考

## 相關連結

- [[openseo-ai-seo-mcp]] — 應用封裝 MCP 供 Agent 呼叫
- [[stirling-pdf-toolkit]] — 自架開源工具
