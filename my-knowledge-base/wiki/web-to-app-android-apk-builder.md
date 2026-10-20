---
title: Web-to-App — 手機端 on-device Android APK 封裝器
date: 2026-06-17
tags: [dev/flutter]
source: https://github.com/shiaho777/web-to-app
type: article
status: processed
asset_value: 52
---

多數「網站轉 App」工具只是用 WebView 包一個 URL 就結束了。Web-to-App 更接近一個口袋型 APK 工作坊：在 Android 手機上直接完成 APK 建構、簽名、安裝、分享，完全不需要 PC、Android Studio 或 Build Server。

## T（核心論點）

**Web-to-App 的差異化是「手機即開發環境」加上「真正在裝置上跑 server runtime」**。核心關鍵是刻意保留 `targetSdk = 28`，這讓 generated APK 可以 fork 並執行本機 Node.js、PHP、Python、Go binary（同 Termux 的做法），而不只是包一層 WebView。純 WebView 封裝的工具做不到這件事。

Play Store 發布另有對策：on-device AAB exporter 建構時將 `targetSdk` 改寫至 Play Store 要求的版本（目前 35），本機簽名後可直接上架。這個架構讓靜態/前端應用走 Play Store，需要 server runtime 的應用走 APK sideload。

## B（適用場景）

**最適合：**
- 已有網站/React/Vue/HTML 產品，想快速推出 Android App 測試市場
- SaaS 後台、內部工具、文件網站需要 Android 入口點
- 想驗證 MVP 可行性的獨立開發者或小型團隊
- 教育機構或企業內網應用，不一定要上 Play Store

**不適合：**
- 需要深度 Android 原生功能（BLE 藍牙、AR、高效能圖形）
- 追求完全 Native 體驗（應考慮 Flutter 或 React Native）
- 複雜的背景服務、push notification 精細控制

## R（完整內容）

### 支援的內容類型

| 輸入類型 | 說明 |
|---------|------|
| Website URL | WebView 包 URL（Landing page、後台、工具） |
| HTML/靜態前端 | React、Vue、Vite build，離線運作 |
| Node.js/PHP/Python/Go | 帶 on-device local server，裝置上直接跑 |
| WordPress | 本地 PHP + SQLite-integration 跑 WordPress |
| 圖片/影片/Gallery | 媒體展示應用 |
| Multi-Web | 多網站 tab/card/feed/drawer 集成 |

### APK 建構流程（5步）

1. **Create**：從 URL、資料夾、媒體集或 runtime template 建 App
2. **Customize**：設定 WebView、toolbar、splash、模組、permission、簽名、runtime 行為
3. **Preview**：手機上預覽
4. **Build & Sign**：on-device 透過 `com.android.tools.build:apksig` 簽名
5. **Install/Share/Export**：安裝、分享或備份 APK

### 關鍵技術特性

- **APK 二進位修補**：binary AXML/ARSC patching、resource injection、permission pruning、V1/V2/V3 簽名
- **WebView 精細控制**：User-Agent、desktop mode、JS/CSS injection、DNS-over-HTTPS、proxy、PWA cache
- **GeckoView**：可選 Firefox 風格渲染引擎（預設用系統 WebView）
- **Module Market**：GitHub-backed 社群 JS/CSS 延伸模組市集（`registry.json` + PR flow）
- **AI Coding 整合**：可提示生成 web apps、extension modules、runtime projects
- **資安**：PBKDF2 + AES-256-GCM 資源加密、28 向量瀏覽器指紋偽裝、Hosts-rule 廣告封鎖

### 技術棧

- Kotlin + Jetpack Compose + Material 3
- Koin（DI）、Room 2.7.2（persistence）、OkHttp 4.12.0
- `com.android.tools.build:apksig 8.3.0`（APK 簽名）
- `protobuf-javalite`（AAB metadata）
- GeckoView（可選）、Coil（圖片載入）、ZXing（QR）

### targetSdk=28 的設計決策

Host app 保留 targetSdk=28 是刻意選擇，不是限制——這是讓 generated app 能從 app storage fork 並執行 Node.js/PHP/Python/Go native binary 的前提條件，跟 Termux 相同機制。代價是 host app 只能透過 GitHub Releases 發布（Play Store 限制）。

## C（關鍵行動）

- [ ] 從 GitHub Releases 下載安裝 Web-to-App
- [ ] 先用靜態 HTML/React 專案測試整個 APK 建構流程
- [ ] 需要上 Play Store：使用 AAB exporter，會自動將 targetSdk 改寫為 35
- [ ] 需要 Node.js/PHP/Python runtime：走 APK sideload 路線（不上 Play Store）
- [ ] 瀏覽 Module Market 查看可用的社群擴充模組

## 相關連結

- [[flutter-patrol-e2e-testing]] — Flutter 的 e2e 測試，與 Web-to-App 代表兩種不同 Android 開發路線
- [[agentic-ai-learning-roadmap]] — AI Agent 工具生態與 7 階段學習地圖
