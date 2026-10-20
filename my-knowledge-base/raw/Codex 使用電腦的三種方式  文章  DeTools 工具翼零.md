---
title: "Codex 使用電腦的三種方式 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3949"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-18
created: 2026-06-22
description: "深入解析 Codex 的 Computer Use、Chrome、Browser 與 Appshot 差異，了解各自適用情境與權限範圍，幫助開發者選擇最合適的 AI 電腦操作方式，提升工作流程效率。"
tags:
  - "clippings"
status: processed
---
目前 Codex 提供三種不同的電腦操作方式： **Computer Use（電腦控制）** 、 **Chrome 擴充功能** 以及 **內建 Browser（瀏覽器）** 。三者功能有部分重疊，因此容易讓人混淆，但實際上各自適合不同的使用情境。

簡單來說，如果有現成的 API、外掛程式（Plugin）或 MCP 工具可用，通常優先使用這些結構化工具會更有效率。例如透過 Slack 外掛直接讀取討論串，會比模擬滑鼠點擊 Slack 網頁來得快速且精準；GitHub 外掛執行操作，也比直接控制 GitHub 網站更容易檢查與追蹤。視覺化控制最適合用於結構化工具無法涵蓋的最後一哩路。

## Computer Use：直接操作電腦

Computer Use 是功能最完整的操作方式，能讓 Codex 透過視覺辨識與介面操作來控制 macOS 或 Windows 上的應用程式，包括視窗、選單、鍵盤輸入與剪貼簿等功能。

由於它必須像真人一樣觀看畫面、判斷位置、點擊操作並等待介面回應，因此速度通常比直接呼叫 API 慢。然而，它最大的優勢是即使沒有 API 的軟體，也能進行操作。

在 macOS 上，Computer Use 還能在背景執行工作，不一定會干擾使用者當前的操作。它可以控制許多桌面應用程式，例如 Spotify、Xcode、系統設定、iOS 模擬器，甚至透過 iPhone Mirroring 操作手機畫面。若工作流程涉及多個應用程式，它也能在不同程式之間切換完成任務。

適合使用 Computer Use 的情境包括：

- 桌面應用程式操作（如 Spotify、財務軟體）
- iOS 模擬器或 iPhone Mirroring
- 系統與應用程式設定調整
- 沒有 API 的資料來源
- 跨多個應用程式的工作流程
- 現有工具缺少某個特定功能時的補足方案

安裝方式是在 Codex 的「Settings → Computer Use」中進行安裝，使用時只需在提示詞中提及 **@Computer** 即可。

## Chrome：使用已登入的瀏覽器環境

Chrome 擴充功能讓 Codex 可以使用你目前登入中的 Chrome 瀏覽器環境，包括 Cookie、帳號登入狀態、瀏覽器設定以及已開啟的分頁。

這非常適合需要身份驗證的工作，例如：

- Gmail
- LinkedIn
- Salesforce
- 客服後台系統
- 企業內部網站
- 已登入的研究與資料查詢
- 依賴瀏覽器擴充功能的表單或系統

Chrome 模式最大的優勢在於能同時理解多個分頁之間的關聯。它不只是記錄畫面上的位置，而是真正理解瀏覽器中的工作流程，因此能在不同分頁之間讀取資訊、比較內容並持續執行任務。

例如：

- 比對客戶資料與客服工單
- 檢查 Gmail 郵件內容
- 查看 LinkedIn 資訊
- 長期追蹤社群媒體私訊與通知
- 處理已登入系統中的工作流程

如果整個任務都發生在瀏覽器內，而且需要登入身分，Chrome 通常會比 Computer Use 更合適。

## Browser：適合開發與測試網站

內建 Browser 是一個直接存在於 Codex 對話中的瀏覽器，使用者與 Codex 可以共同查看同一個網頁畫面，因此特別適合網站開發與除錯工作。

常見用途包括：

- 本機開發伺服器（localhost）
- HTML 預覽頁面
- 不需登入的公開網站
- 重現前端 Bug
- 響應式設計測試
- UI 與版面設計審查

Browser 最大的特點是隔離性。它不會使用你的 Chrome 帳號、Cookie、擴充功能或現有分頁，因此非常適合作為安全的測試環境。

例如：

- 測試 Vite、React、Vue 專案
- 修正版面溢位問題
- 驗證手機版與桌面版畫面
- 調整字體、間距與色彩配置
- 針對頁面元素進行標註與評論

在設計流程中，開發者可以直接在畫面上標註問題，例如：

- 「這裡的資訊層級不對」
- 「這個按鈕空間太小」
- 「整體看起來太像卡片設計」
- 「這套字體比例應該統一」

Codex 會根據標註內容修改程式碼並重新開啟頁面進行驗證，形成快速迭代的設計流程。

## Appshots：快速提供畫面上下文

Appshot 並不是第四種控制方式，而是一種快速將目前畫面內容提供給 Codex 的方法。

在 macOS 上，可以按下指定快捷鍵擷取目前最前方視窗的內容。Codex 會取得該畫面的截圖以及可讀取的文字資訊。

可以用來分享：

- 錯誤訊息
- 電子郵件內容
- UI 設計稿
- 系統設定頁面
- 不熟悉的操作介面

Appshot 的用途是讓 Codex 理解你目前正在看的內容，而不需要立即授權它控制該程式。

| 情境 | 建議使用 |
| --- | --- |
| 操作桌面應用程式 | Computer Use |
| 修改系統設定 | Computer Use |
| 控制 iPhone Mirroring 或模擬器 | Computer Use |
| 使用 Gmail、LinkedIn 等已登入網站 | Chrome |
| 處理企業內部系統 | Chrome |
| 多分頁研究與資料比對 | Chrome |
| 本機網站開發 | Browser |
| 前端 UI 除錯 | Browser |
| 響應式版面測試 | Browser |
| 提供目前畫面給 Codex 分析 | Appshot |

簡單記憶方式：

- **Appshot** ：告訴 Codex「我正在看什麼」
- **Browser** ：操作開發中的網站
- **Chrome** ：操作你已登入的瀏覽器環境
- **Computer Use** ：直接操作整台電腦

這也是官方建議的原則： **優先使用範圍最小、權限最少的工具，只有在需要時才提升到更高權限的操作方式。**

原文整理自 [X Three Ways Codex Can Use a Computer](https://x.com/jxnlco/status/2066970432855581052)