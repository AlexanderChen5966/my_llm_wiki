---
title: Google 電腦／手機代理：Gemini Computer Use API GA、ARTEMIS 與 Antigravity Android 流程（2026-11 快照）
date: 2026-11-05
tags: [ai/agent, dev/testing, ai/mcp]
source: https://www.aiposthub.com/gemini-computer-use-ga-browser-mobile-desktop-agents/；https://www.aiposthub.com/google-artemis-android-automation-testing-guide/；https://www.aiposthub.com/google-antigravity-stitch-android-real-device-demo/（皆 aiposthub／Philo）
type: article
status: processed
asset_value: 27
---

> 註：asset_value 27（頻率 3×耗時 3×複雜度 3），**保守估 18（耗時 2）**，屬邊界；單篇分數 27／18／8，合併才成條目。
> 🔴 **來源性質**：三篇皆 aiposthub（作者 Philo）**單一作者的二手整理，站方皆未實測**；一手來源（Gemini API 官方公告與文件、ARTEMIS README、官方影片）**未核對**。
> ⏰ **快照式、保鮮期短**：Computer Use 官方公告 2026-11-04（文章與模型清單快照 **2026-11-05**）；ARTEMIS 文章 2026-10-19；Antigravity Android 影片 2026-10-06（文章 10-07）。模型清單、支援範圍會變。
> 邊界：本條講 **Gemini API 層的 Computer Use 工具**；Gemini 桌面版 app 的 Computer Use 封測（見 [[gemini-desktop-assign-mode-obsidian-computer-use]]）是另一個層級，本篇未涉及其狀態。
> 已排除：各篇訂閱導流與站內延伸閱讀。

## T（核心論點）

「截圖→模型提動作→執行器動手→回傳新截圖」是這類代理共通的迴圈；真正決定能不能放心用的是**確認路徑、資料外送與測試環境隔離**，不是成功率數字。官方沒有公布 Computer Use 任務成功率，ARTEMIS 的高完成率是 Google 自報、本站未重跑，**都不能外推到你的 App**。

## B（適用場景）

要用 Gemini 的電腦操作工具做瀏覽器／桌面／Android 自動化，或想評估開源的 ARTEMIS 做 Android 真機測試時，先看迴圈、安全面與資料流。只支援 Android，**不涵蓋 iPhone**（ARTEMIS）。

## R（關鍵細節）

### 1. Gemini Computer Use（API，已 GA，快照 2026-11-05）

- 從預覽轉 GA，二手轉述稱**無破壞性介面變更**
- **迴圈**：截圖 → 模型提出 function call（含 intent 欄位）→ 執行器操作 → 新截圖回傳。**執行器要自己寫**
- **點擊座標為 0–999 標準化**，執行器要依實際視窗大小換算
- 環境分瀏覽器／桌面／Android；**Android 標為最佳化**，其他作業系統的支援**原文未確認，不推論**
- **安全判斷三態**：允許／需確認／阻擋；類別含交易、帳號建立、隱私同意等。可對照 [[codex-computer-use-modes]] 的最小權限思路
- 🔴 **截圖提示注入偵測是選擇啟用、預設關閉**（支援 Gemini 3.5 Flash-Lite 以上）：畫面上的文字可能是攻擊指令，沒開就沒有這層防護
- 文件推薦 Gemini 3.8 Flash；另列 Gemini 4 Argon、3.5 Flash-Lite、3 Flash Preview 支援（**模型清單為 11-05 快照**）
- 未量化：截圖與重試的成本、任務成功率

### 2. ARTEMIS（Google 開源，2026-10-19 文章）

- 以**自然語言經 ADB** 操作 Android 真機或模擬器（需 USB 偵錯），支援跨 App 任務
- 定位**優先用元素索引，失敗時退回座標／視覺**——與 [[agent-device-cross-platform-testing]] 的「accessibility tree 優先、截圖退回」是同構的另一個實作
- 可經 **MCP** 接 Antigravity、Claude Code、Codex，並收集截圖與 **Logcat**
- 兩種模式：**Flash**（反應式，專案自稱約每步 3–5 秒，無完整計畫、執行前安全檢查與最終報告）／**Pro**（規劃者＋操作＋檢查，自稱約每步 15–40 秒）；🔴 秒數為專案自稱典型值，未驗證
- 🔴 **AndroidWorld（20+ App、100+ 任務）完成率 99% 以上是 Google 自報（README），本站未重跑，不可外推到真實 App**
- 資料流：裝置端輔助服務在本機處理介面資訊，**但使用雲端模型時，畫面與任務內容會送出**；輕量裝置端 VLM 僅列為後續方向
- 風險：ADB／偵錯權限高 → 用**專用測試機與測試帳號**（見 [[agent-permission-and-os-sandbox]]、[[agent-operated-tools-permission-and-acceptance-snapshot-2026-10]]）

### 3. Antigravity 的 Android 流程（快照 2026-10-07，一小節）

- 官方影片展示：Antigravity 經 **Stitch MCP** 取設計 → 轉 **Jetpack Compose** 原生元件 → 用 Android CLI 外掛建置 → 模擬器驗證 → 送到實機（Stitch 見 [[design-md-google-format]]）
- 🔴 **只有這兩段是影片事實**；後半（保留文字／互動／狀態、模擬器與實機分開留證據、權限與鍵盤情境、要求代理說明每次修改原因）是**作者自承非影片項目的通用驗收建議，沒實測**
- 這是流程能力展示，不證明任意應用都能自動完成；模擬器與實機的證據邊界見 [[devin-macos-vm-ios-simulator-foldable-testing]]
- Compose 是原生路線，與以 Flutter 為主的專案相關度有限

## 三者對照

| 項目 | Computer Use API | ARTEMIS | Antigravity Android |
| --- | --- | --- | --- |
| 層級 | 模型 API 工具（你寫執行器） | 開源 Android 代理框架 | IDE 代理流程展示 |
| 操作對象 | 瀏覽器／桌面／Android | Android 真機／模擬器（ADB） | 設計稿到 App 與裝置 |
| 關鍵風險 | 注入偵測預設關、確認路徑 | ADB 高權限、雲端模型外送畫面 | 展示≠通用能力 |
| 數字出處 | 無成功率 | 99%、3–5／15–40 秒皆自報 | 無 |

## C（關鍵行動）

- [ ] 接 Computer Use 前確認：執行器座標換算、三態確認怎麼處理、是否開啟注入偵測
- [ ] 試 ARTEMIS 用專用測試機與測試帳號，先確認用的是本機或雲端模型再決定能跑什麼畫面
- [ ] 引用 99%、秒數、模型清單時保留「自報／自稱／快照」標註，一個月後回一手核對

## 原始摘要

三篇 aiposthub 整理：Gemini API 的 Computer Use 工具轉 GA（截圖迴圈、標準化座標、三態安全判斷、選擇啟用的注入偵測）；Google 開源 ARTEMIS 以自然語言經 ADB 操作 Android（Flash／Pro 兩模式、可接 MCP、AndroidWorld 自報 99% 以上）；Antigravity 官方影片展示從 Stitch 設計到 Compose 到實機的流程，作者補充的驗收建議未經實測。

## 相關連結

- [[gemini-desktop-assign-mode-obsidian-computer-use]] — 桌面版 app 的 Computer Use 封測；本條是 API 層 GA，不同層級
- [[agent-device-cross-platform-testing]] — 元素優先、截圖退回的同構做法
- [[fara-computer-use-agent]] — 視覺優先電腦操作模型，同屬截圖座標迴圈
- [[codex-computer-use-modes]] — 最小權限與確認模式對照
- [[google-antigravity-platform]] — 平台總覽
- [[devin-macos-vm-ios-simulator-foldable-testing]] — 模擬器證據邊界
- [[agent-permission-and-os-sandbox]] — 權限與沙箱通則
- [[agent-operated-tools-permission-and-acceptance-snapshot-2026-10]] — 前批同型快照條目
- [[design-md-google-format]] — Stitch 與設計規格
