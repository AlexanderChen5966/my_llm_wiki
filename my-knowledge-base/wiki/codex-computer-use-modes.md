---
title: Codex 操作電腦的四種方式：Computer Use / Chrome / Browser / Appshot
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://tools.wingzero.tw/article/sn/3949
type: article
status: processed
asset_value: 58
---

Codex 提供四種操作方式，核心原則：**優先使用範圍最小、權限最少的工具，只有在需要時才提升到更高權限**。有 MCP/Plugin 就用結構化工具，而非視覺化控制。

## T（核心論點）

**四種模式從低到高：**
- Appshot（告訴 Codex「我在看什麼」）
- Browser（操作開發中的網站）
- Chrome（操作已登入的瀏覽器環境）
- Computer Use（直接操作整台電腦）

簡單記法：視覺化控制是「最後一哩路」，不是第一選擇。

## B（適用場景）

- 了解在 Codex workflow 中何時應該用哪種操作方式
- 建立涉及已登入帳號（Gmail/Salesforce/企業系統）的自動化
- 本地開發網站的 UI 除錯
- 操作沒有 API 的桌面軟體

## R（完整內容）

### 選擇決策表

| 情境 | 建議使用 |
|------|---------|
| 操作桌面應用程式（Spotify、財務軟體）| Computer Use |
| 修改系統設定 | Computer Use |
| 控制 iPhone Mirroring 或 iOS 模擬器 | Computer Use |
| 使用 Gmail、LinkedIn 等已登入網站 | Chrome |
| 處理企業內部系統（Salesforce/客服後台）| Chrome |
| 多分頁研究與資料比對 | Chrome |
| 本機網站開發（localhost）| Browser |
| 前端 UI 除錯、響應式版面測試 | Browser |
| 提供目前畫面給 Codex 分析 | Appshot |

### Computer Use：最高權限，最廣泛

- 透過視覺辨識控制 macOS/Windows 的視窗、選單、鍵盤
- 在 macOS 上可在背景執行，不干擾使用者操作
- 可控制 Xcode、iOS 模擬器、iPhone Mirroring、系統設定
- 跨多個應用程式的複雜工作流程
- 安裝：Settings → Computer Use，提示詞中用 `@Computer` 觸發

### Chrome：已登入的瀏覽器環境

- 使用你目前 Chrome 的 Cookie、帳號狀態、已開啟分頁
- 能同時理解多個分頁間的關聯（跨分頁比對）
- 適合：Gmail、LinkedIn、Salesforce、企業系統、社群媒體追蹤
- 如果整個任務都在瀏覽器內且需要登入，Chrome 比 Computer Use 更精準

### Browser：隔離的開發測試環境

- Codex 對話內建的瀏覽器，使用者與 Codex 共同看同一頁面
- **完全隔離**：不使用你的 Chrome 帳號/Cookie/擴充功能
- 適合本機開發（Vite、React、Vue）、前端 Bug 重現、UI 標注
- 開發者可直接在畫面上標注問題，Codex 修改後重新驗證

### Appshot：提供上下文（不是控制）

- macOS 快捷鍵擷取最前方視窗（截圖 + 可讀取文字）
- 讓 Codex 理解你在看什麼，無需授權它控制該程式
- 適合：分享錯誤訊息、Email 內容、UI 設計稿、設定頁面

### 優先原則

1. **有 API/MCP/Plugin？優先用結構化工具**（Slack Plugin > 模擬點擊 Slack 網頁）
2. **整個任務在瀏覽器 + 需要登入？** Chrome
3. **開發中的本機網站？** Browser
4. **跨應用程式 + 沒有 API？** Computer Use
5. **只是要給 Codex 看螢幕內容？** Appshot

## C（關鍵行動）

- [ ] 建立涉及已登入服務的工作流前，先確認有沒有對應的 Plugin/MCP
- [ ] 本地開發 UI 除錯優先用 Browser，不要用 Computer Use 截全螢幕
- [ ] 設定 Computer Use：Settings → Computer Use，了解 `@Computer` 的使用方式

## 補充資料

**2026-11-19 補充（來源：aiposthub〈OpenCode Browse 讓 AI 使用現有瀏覽器：分頁交接、登入與允許清單怎麼運作？〉，https://www.aiposthub.com/opencode-browse-real-browser-mcp-handoff-allowlist/ ，2026-11-09）**：依官方 README 整理，站方未實測。對應本條「Chrome（已登入帳號）」那一級的開源實作。

- **架構**：代理 → MCP 伺服器 → 本機中繼 → 擴充功能（Chrome 除錯介面）→ 已登入的分頁；代理用 Playwright 程式操作，不是看截圖猜座標。支援 Chrome、Edge、Brave、Arc、Vivaldi、Opera 等 Chromium 系
- 🔴 **沿用登入＝沿用該帳號的全部權限**，Browse 不會替代理建較低權限的帳號
- **分頁所有權**：代理只用自己開的或人主動借出的分頁（工具列按鈕借出／收回，收回會中止操作）；代理不能把背景視窗拉到前景、不能關人借的分頁；自開分頁閒置 10 分鐘清掉
- **交接要人的真實點擊**：遇到登入／2FA 呼叫交接卡片，只有使用者在卡片或工具列的實際點擊才算完成，網頁程式無法假裝同意
- 🔑 **允許清單管「分頁能去哪」，不是防火牆**：會擋清單外的新分頁、導覽、重新導向與彈窗，但圖片、網頁程式發出的請求、iframe、背景工作者等子資源不在封鎖範圍；送給模型供應商的網頁內容也是另一條資料路徑
- **本機配對**：只收本機連線＋本機權杖；一次性配對碼 10 分鐘有效、錯 5 次作廢，之後各瀏覽器用金鑰挑戰驗證、可撤銷。受限執行環境與中繼同一程序，官方不把它定位成可跑敵意程式的沙箱

## 相關連結

- [[codex-plugins-12-guide]] — Codex 12 個必裝 Plugin（結構化工具優先於視覺化控制）
- [[codex-advanced-tips-11]] — Codex 11 個進階技巧（Durable Threads / Goals）
- [[codex-plugin-workflow]] — Codex Plugin 可重用工作流
