---
title: "Mobile Next MCP 是什麼？用 Claude、Codex 自動操作 iOS 與 Android 的安裝、功能與風險"
source: "https://www.aiposthub.com/mobile-next-mcp-ios-android-automation/"
author:
  - "[[Philo]]"
published: 2026-09-29
created: 2026-09-29
description: "Mobile Next Mobile MCP 讓 Claude、Codex 等 Agent 控制 iOS、Android 模擬器與真機。本文整理安裝、工具、限制與安全設定。"
tags:
  - "clippings"
status: processed
---
西班牙開發者 Miguel Ángel Durán 在 2026 年 9 月 27 日於 X 分享 Mobile Next 的 Mobile MCP，影片示範 AI Agent 操作 iPhone 模擬器、重現 App 問題，再回到程式碼找出錯誤。這個開源專案也支援 Android 模擬器與 iOS、Android 真機。

MCP 是 Model Context Protocol 的縮寫，可以把外部工具以統一方式提供給 AI Agent。Mobile MCP 像一座橋梁，在 Agent 與手機系統之間翻譯操作。Agent 不直接理解 Xcode 或 Android Debug Bridge 的每個指令，而是呼叫「列出裝置」、「讀取畫面元素」、「點擊」、「滑動」、「啟動 App」等結構化工具。

我的判斷是謹慎偏多。它很適合讓開發者快速探索 App、重現錯誤、跑資料輸入和建立測試草稿。若流程會碰到正式帳號、付款、訊息或個人資料，則必須使用專用測試裝置、限制網路與帳號權限，並人工確認 Agent 呼叫的工具。

這篇文章會說明它能控制哪些裝置、和傳統 XCUITest／Espresso 的差別、如何安裝，以及為什麼「同一套工具跨 iOS、Android」仍不等於零設定或零風險。

## Mobile Next Mobile MCP 是什麼？

Mobile MCP 是 Mobile Next 在 GitHub 公開的 MCP Server，採 Apache 2.0 授權。MCP Server 是把一組能力包裝成 Agent 可呼叫工具的服務。它可連接 Claude Code、Codex、Gemini、GitHub Copilot、Cursor 等相容 MCP 的客戶端。

它的主要用途是行動 App 自動化與開發。Agent 可以列出裝置、取得螢幕尺寸與方向、讀取畫面元素、點擊、長按、滑動、輸入文字、啟動或停止 App、開啟 deep link、錄影和擷取 crash log。

Deep link 是直接開啟 App 特定頁面的網址或連結格式。Crash log 則是 App 當機時留下的錯誤紀錄。把這些能力交給 Agent 後，可以用自然語言描述完整任務，例如登入測試帳號、開啟設定、修改選項，再確認畫面結果。

## 支援哪些裝置？模擬器、仿真器和真機都可以

官方列出四種主要目標：iOS Simulator、iOS 真機、Android Emulator 與 Android 真機。Simulator 和 Emulator 都是在電腦上模擬手機環境的軟體，本文統稱模擬裝置。

| 目標 | 官方支援 | 本機前置條件 |
| --- | --- | --- |
| iOS Simulator | 是 | Xcode command line tools 與已啟動的 Simulator |
| iOS 真機 | 是 | USB 連接、信任裝置與必要驅動 |
| Android Emulator | 是 | Android SDK、ADB 與已啟動的 Emulator |
| Android 真機 | 是 | ADB、USB debugging 與裝置授權 |

ADB 是 Android Debug Bridge，開發者用它在電腦和 Android 裝置之間執行指令。Mobile MCP 把兩個平台的常見操作整理成同一套工具，但底層平台工具仍要先安裝並能正確辨識裝置。

專案也提供 Mobile Next Cloud，可以租用遠端 iOS 與 Android 真機。雲端裝置使用相同工具，不用在本機接線，但帳號、費用、資料位置和測試內容要另外評估。

## Accessibility-first：先讀元件，再用截圖座標補位

Mobile MCP 的主要設計是優先讀取原生 accessibility tree。Accessibility tree 是系統提供給輔助功能的結構化介面，會描述按鈕、文字欄位、標籤、可點擊狀態與畫面階層。

Agent 取得這些元素後，可以依名稱或參照值操作，比只看截圖猜座標更穩定。按鈕在不同螢幕尺寸移動幾個像素，只要語意和元件仍存在，測試就不一定失效。官方也表示，這種方式不需要每一步都送圖片給視覺模型，可以減少圖片 token 和延遲。

遇到自繪畫面、遊戲或沒有良好 accessibility 資訊的 App 時，Mobile MCP 仍可退回截圖加座標操作。這類操作更容易受解析度、動畫、鍵盤和版面變化影響，應加上畫面驗證與重試限制。

![AI Agent 操作 iPhone Simulator 並分析 Swift App 問題](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/mobile-next-mcp-device-demo.jpg)

Miguel Ángel Durán 的 X 示範讓 Agent 操作 iPhone Simulator、重現問題，再回到 Swift 程式碼提出修正。 圖片來源： Miguel Ángel Durán X 。

## X 影片示範了什麼？從重現問題到修改程式

Miguel Ángel Durán 的 X 影片展示一個行動開發流程。Agent 取得 GitHub issue 後，在 iPhone 模擬器開啟 App，檢查城市與料理資訊，找出文字和圖片問題，再回到 Swift 程式碼提出修正。

這段影片能證明 MCP 把 IDE、程式碼與模擬器串進同一個 Agent 工作流。它不能證明每個 App 都能一次完成，也不能當作跨裝置成功率 benchmark。影片使用的是特定示例、環境與模型，實際結果仍受 App accessibility、畫面狀態、網路和 Agent 判斷影響。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/09/mobile-next-mcp-x-demo.mp4" width="1280" height="720" controls=""></video>

59 秒示範呈現 Agent 取得 issue、操作 iPhone Simulator、找出畫面問題並回到程式碼。 影片來源： Miguel Ángel Durán X 。

## 和 XCUITest、Espresso、Appium 有什麼差別？

XCUITest 是 Apple 的 iOS 自動化測試框架，Espresso 是 Android 的原生 UI 測試框架。Appium 則提供跨平台 WebDriver 介面。這些工具通常由工程師寫出明確測試程式，適合持續整合和固定回歸測試。

Mobile MCP 的優勢是探索速度。你可以先用自然語言讓 Agent 嘗試一段使用者旅程，查看畫面結構、錄影、收集 crash，再逐步修正。對還不確定測試步驟的錯誤重現或臨時驗證，比先寫完整測試程式更快。

它的弱點是 Agent 行為可能不穩定。相同提示在不同畫面狀態下，可能選擇不同操作順序。官方把 mobilewright 定位為下一步：當探索流程成熟後，轉成可重複、較確定的測試。正式回歸測試仍應有明確輸入、斷言、版本和失敗證據。

## 如何安裝？Claude Code、Codex 都能接

官方要求 Node.js 20 以上版本，以及對應平台的 Xcode command line tools 或 Android Platform Tools。最常見的 MCP 設定會讓客戶端執行：

```json
{
  "mcpServers": {
    "mobile-mcp": {
      "command": "npx",
      "args": ["-y", "@mobilenext/mobile-mcp@latest"]
    }
  }
}
```

Claude Code 可使用 `claude mcp add` ，Codex 可使用 `codex mcp add` 加入相同 npm 套件。安裝後先啟動模擬裝置，再請 Agent「list available devices」。若沒有列出裝置，先檢查 Xcode、ADB、USB 授權與模擬器狀態。

`@latest` 會抓取當下最新版本，方便入門，但團隊測試通過後應固定版本並閱讀 changelog。Changelog 是版本更新紀錄。官方在 2026 年 9 月 23 日列出的最新 release 為 1.0.5，版本更新包含 iOS、Android、錄影與裝置狀態修正。

## 可以做哪些工作？

最適合的工作是結果容易觀察、風險可限制、而且需要多步操作的行動流程。

- 錯誤重現：依 issue 操作 App、錄影、擷取 crash 和畫面狀態。
- 探索測試：走過註冊、搜尋、購物車或設定流程，找出卡住位置。
- 測試資料輸入：在測試環境填寫重複表單或建立多組帳號資料。
- 行銷素材：在指定裝置打開功能頁並擷取畫面或錄影。
- 開發驗證：修改程式後重新啟動 App，確認特定 UI 是否改變。

不適合直接自動執行的工作包括正式付款、刪除使用者資料、讀取私人訊息、修改雙重驗證或操作個人主要手機。這些流程的不可逆性和資料敏感度太高，應使用模擬環境、假資料和人工核准。

## 安全與隱私：它拿到的是整台裝置能力

Mobile Next 的 SECURITY 文件明確提醒，Agent 可在裝置或模擬器的安全範圍內操作，應查看它正在呼叫哪些工具，並使用專為此用途準備的裝置。

這句話很重要。只要測試手機已登入 Email、社群、銀行或公司帳號，Agent 就可能看見通知、開啟 App、讀取剪貼簿或執行其他動作。最安全的做法是使用乾淨的專用模擬器或測試機，只放假資料和低權限測試帳號。

官方 README 也說明，專案會透過 PostHog 與 Scarf 收集匿名使用 telemetry。Telemetry 是軟體回傳使用與錯誤資訊的機制。若政策不允許，可設定 `MOBILEMCP_DISABLE_TELEMETRY=1` 關閉。

使用 `--listen` 啟動 HTTP 模式時，預設綁定 localhost，並提供 Host header 防護。若改成對外網路介面，應設定 `MOBILEMCP_AUTH` Bearer token、限制防火牆和來源。未設定驗證時，Server 會接受未驗證連線並顯示警告，不應直接暴露到公司或公開網路。

## 版本安全不能忽略：過去問題已修，但仍要固定與更新

官方 changelog 顯示，0.0.49 修正截圖與錄影路徑 traversal，0.0.50 將開啟網址限制為 HTTP／HTTPS，除非使用者主動允許不安全 scheme。0.0.51 加入 HTTP Server 的 Bearer token 驗證，0.0.52 也強化跨來源請求和未驗證警告。

Path traversal 是惡意路徑讓程式讀寫預期資料夾以外檔案的問題。URL scheme 則可能觸發撥號、簡訊或其他裝置功能。這些修正說明行動自動化工具的權限很高，也提醒使用者不要長期停留在舊版本。

更新也不能完全自動。合理流程是固定已驗證版本、追蹤安全公告與 changelog，在隔離裝置重跑核心流程後再升級。每次都使用 `@latest` ，可能在沒有審查的情況下改變工具行為。

## 用 5 條流程判斷值不值得導入

先選五條不同難度的測試流程：單頁點擊、表單輸入、跨頁導覽、需要鍵盤的流程，以及一條包含錯誤狀態的流程。分別在 iOS 與 Android 模擬裝置重跑三次。

記錄成功率、平均步驟、人工介入次數、總耗時，以及失敗時是否留下截圖、錄影和 crash。若使用 accessibility tree，也要記錄哪些畫面只能靠座標，因為那通常是穩定性風險。

我會把導入門檻設為：低風險流程成功率高、失敗能重現、Agent 沒有超出允許工具、且總時間低於人工操作。成熟後再把固定流程轉成 mobilewright、XCUITest、Espresso 或既有測試框架。

## Mobile Next MCP 常見問題

### 可以同時控制 iOS 和 Android 嗎？

同一套 MCP tools 支援兩個平台，但每次要先選定可用裝置。本機仍需安裝對應的 Xcode 或 Android SDK。

### 一定要真機嗎？

不用。iOS Simulator 和 Android Emulator 都在官方支援範圍，也可以使用 USB 真機或 Mobile Next Cloud 遠端真機。

### 完全不需要懂 XCUITest 或 Espresso 嗎？

用自然語言探索流程時可以不直接撰寫這些框架的測試。但正式回歸、除錯和平台設定仍需要測試與行動開發知識。

### 它只靠截圖辨識嗎？

不是。官方設計優先使用 accessibility tree 的結構化元素，無法取得元素時才退回截圖與座標。

### 可以拿主要手機直接測嗎？

不建議。官方安全文件建議使用專用裝置，因為 Agent 可以在裝置權限範圍內操作。應使用測試帳號、假資料和隔離裝置。

## 結語：先用 Agent 探索，再把成熟流程固定下來

Mobile Next Mobile MCP 把 iOS、Android 模擬裝置和真機包成一套 Agent 可呼叫工具，讓錯誤重現、探索測試和 UI 操作能從自然語言開始。Accessibility-first 也比每一步只靠截圖更有效率。

它沒有消除平台設定、測試工程或安全責任。Xcode、Android SDK、裝置授權、資料隔離與版本管理仍然存在。Agent 可以控制整台測試裝置，便利和風險來自同一項能力。

最穩健的用法，是先在乾淨模擬器用五條低風險流程測成功率，人工查看工具呼叫，再把成熟工作轉成可重複測試。這樣 Mobile MCP 才是行動開發的加速器，而不是新的不確定來源。