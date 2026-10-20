---
title: "2026 年 Google I/O 大會的 15 項更新：透過 Chrome 的新功能、工具和特色，打造代理程式型網路  |  Blog"
source: "https://developer.chrome.com/blog/chrome-at-io26?hl=zh-tw"
author:
published:
created: 2026-05-21
description: "瞭解 2026 年 Google I/O 大會的重大公告。"
tags:
  - "clippings"
status: processed
---
代理程式正在改變各處的開發作業，而這項轉變在網路上發生的速度最快。重新定義我們建構的內容、建構方式和建構者。隨著代理式網路時代來臨，我們看到一項轉變，可縮小複雜的開發人員工作流程、基礎平台功能和日常使用者體驗之間的差距。

在 2026 年 Google I/O 大會上，我們揭示了這個時代的願景。這些構想將網頁生態系統的三個核心領域連結在一起： **賦予 AI 代理程式能力** ，透過新功能建構網站並與網站互動； **突破網頁 UI 和效能的極限** ；以及 **將瀏覽器轉變為強大的主動式助理** ， **透過 Chrome 內建 Gemini 為日常使用者提供協助** 。我們將高效的內建 AI 模型直接整合到瀏覽器，並在 Chrome 中提供自動瀏覽等強大的自動化工具，讓所有人都能更聰明、快速且輕鬆地使用網路。

以下是我們在 2026 年 Google I/O 大會上發布的 15 項重大更新，協助您在代理式網路瀏覽時代中建構及蓬勃發展。

## 為網路提供 AI 代理

代理程式正在改變我們與軟體的互動方式，我們認為網頁必須具備引導代理程式的能力。我們即將推出強大的新功能和工具，例如 WebMCP 和 Modern Web Guidance，讓您更清楚且快速地打造現代網頁體驗。我們也提供 AI 輔助工具，協助您以前所未有的速度和準確度，建構、偵錯及最佳化程式碼。

### 1\. WebMCP：將網站轉換為具備代理功能的工具包

我們將搶先介紹 [WebMCP](https://developer.chrome.com/docs/ai/webmcp?hl=zh-tw) ，這項提議的開放網路標準可讓您向瀏覽器型代理程式公開 JavaScript 函式和 HTML 表單等結構化工具。定義這些工具後，您就能明確指示代理程式與網站互動的方式和位置。因此，現在，代理程式可以呼叫機器友善函式，在幾秒內完成複雜工作，而且更可靠、精準，並提供個人化服務。假設使用者正在規劃多個城市的假期行程，不必看著服務專員填寫旅遊表單，而是授權服務專員直接查詢後端 API，立即建立個人化的行程，並根據天氣狀況調整行程，讓使用者核准。

實驗性 WebMCP 來源試用將在 Chrome 149 中啟動。Chrome 內建 Gemini 即將支援 WebMCP API。我們已看到全球消費者品牌開始試用 WebMCP，為使用者打造更令人滿意且引人入勝的體驗。

![Expedia、Booking.com、Shopify、Credit Karma、TurboTax、Redfin、Etsy、Instacart、Target 標誌](https://developer.chrome.com/static/blog/chrome-at-io26/image/logo-wall_2880.png?hl=zh-tw)

### 2\. 現代網頁指南：引導程式碼代理建構現代網頁的藍圖

[現代網頁指南](https://developer.chrome.com/docs/modern-web-guidance?hl=zh-tw) 現已推出搶先體驗版，這是一組經過專家審查的常青技能，可引導程式碼代理程式處理許多常見用途，打造最容易存取、效能最佳且最安全的現代網頁體驗。它會直接與 [Baseline](http://web.dev/baseline?hl=zh-tw) 整合，讓您專注於想建構的內容，工具則會自動找出要在所選 Baseline 目標中使用的正確功能和備援。在 Google Antigravity 中按一下即可安裝，或透過 npx 安裝，也可以在程式碼編寫代理中安裝為擴充功能。新版網頁指引功能支援數十項最新功能，適用於超過 100 種用途，且會定期新增更新。

### 3\. 使用 Chrome 開發人員工具 (代理專用) 自動執行偵錯作業

[代理程式專用的 Chrome 開發人員工具](https://developer.chrome.com/blog/devtools-for-agents-v1?hl=zh-tw) 可讓您擴展工作流程，即時查看、偵錯及最佳化程式碼。讓代理程式直接存取開發人員工具的功能 (例如控制台記錄、網路流量和無障礙樹狀結構)，即可驗證及自動修正問題，不需手動監督。 [代理程式專用的 Chrome 開發人員工具](https://developer.chrome.com/docs/devtools/agents?hl=zh-tw) 今天推出，適用於 Antigravity 和其他 20 多個程式碼編寫代理程式。

![綜合圖表，顯示成效提升情形。](https://developer.chrome.com/static/blog/chrome-at-io26/image/ly-corporation1_2880.png?hl=zh-tw)

LY Corporation 的代理商使用 Chrome 開發人員工具，建構了以 AI 為基礎的自動化成效稽核系統，減少 96% 至 98% 的手動分析作業，並為每個團隊提供隨選稽核報告。

### 4\. 在 Chrome 開發人員工具中使用 AI 輔助偵錯功能，深入瞭解問題

[Chrome 開發人員工具中的 AI 輔助功能](https://developer.chrome.com/docs/devtools/ai-assistance/get-started?hl=zh-tw) 現在可以存取 Lighthouse 資料，並自動搜尋背景資訊，回答比以往更開放式的主題。此外，小工具可讓你全面掌握 Gemini 的推論過程，有助於進行偵錯。

Chrome 開發人員工具中的 AI 輔助功能和互動式小工具可大幅降低效能偵錯的複雜度，同時實現無縫的人機協作。

### 5\. 免除伺服器、預算和繁文縟節：使用內建 AI 解鎖 AI 功能

內建 AI 完全在瀏覽器中執行，可部署個人化主動式功能，這類功能在伺服器上執行成本高昂。略過代幣帳單和其他障礙，全心專注於獨特的使用者價值。最棒的是，瀏覽器會管理並在網站間共用最佳化模型，讓更多使用者在網路上享受 AI 體驗。

為協助您輕鬆建構這些 AI 強化功能，我們擴充了網頁 AI 工具包：

- **[提示 API](https://developer.chrome.com/docs/ai/prompt-api?hl=zh-tw) 現已穩定：** Chrome 148 使用 Gemini Nano，支援多模態輸入和結構化輸出， 提供豐富體驗、可靠的 JSON 格式，實現無縫整合，並支援更多語言。
- **Gemma 197M：** 這款超高效率的專家模型可透明地支援特定工作 API，例如 *摘要產生器* ，自動將功能擴展到更多裝置。

[探索完整的內建 AI 套件](https://developer.chrome.com/docs/ai/built-in-apis?hl=zh-tw) ，包括現有的 Translator 和 Language Detector API，並 [加入搶先體驗方案](https://goo.gle/chrome-ai-dev-preview-join) ，測試即將推出的 API。

**Trip.com** ：使用內建 AI 在裝置端生成個人化旅遊摘要，可避開伺服器負擔，並無限次查詢，完全不必擔心預算。

---

## 突破網頁 UI 和效能的界限

我們正在開發新一代平台功能，進一步模糊網站和原生應用程式之間的界線。全新的宣告式 API (例如 Canvas 中的 HTML 和宣告式部分更新) 會為您處理複雜的算繪和效能工作，讓您輕鬆在網路上建構美觀、現代、高保真度、高效能且互動式的體驗。

### 6\. 畫布中的 HTML 和元素範圍的檢視畫面轉換：運用新一代 UI 打破界線

全新的 [HTML-in-Canvas API](https://developer.chrome.com/blog/html-in-canvas-origin-trial?hl=zh-tw) 和 [元素範圍檢視區塊轉換](https://developer.chrome.com/docs/css-ui/view-transitions/element-scoped-view-transitions?hl=zh-tw) ，可實現先前無法達成的 UI，為網頁帶來高保真度的應用程式式互動。透過 HTML-in-Canvas API，將實際 DOM 元素直接整合至 WebGL 和 WebGPU 的畫布，打造 [沉浸式 3D 體驗](https://github.com/GoogleChromeLabs/css-web-ui-demos/blob/main/html-in-canvas/awesome-html-in-canvas.md) ，方便搜尋、存取、以原生方式翻譯，並與內建瀏覽器功能順暢互動。搭配檢視區塊轉場效果 (例如元素範圍，目前已在 Chrome 147 中推出，以及目前正在測試的雙階段轉場效果)，即可建立分層 UI 動畫，並為中間狀態製作動畫，不會阻礙網頁互動。將複雜的互動轉換為宣告式 API，預設提供高保真效能。 [HTML-in-Canvas API 來源試用](https://developer.chrome.com/origintrials?hl=zh-tw#/register_trial/3478467762190286849) 現已開放。

透過 Canvas 中的 HTML API，您現在可以將 HTML 元素和 CSS 樣式合併到高保真度的多維度環境中，建立新的廣告素材版面配置和效果。

### 7\. 效能和 UI 改善：單頁應用程式的 Core Web Vitals 等

Chrome 推出新方法，可提升新式應用程式風格網頁體驗的效能。新版更新包括即將在 Chrome 版本中推出的 [Soft Navigations API](https://developer.chrome.com/docs/web-platform/soft-navigations?hl=zh-tw) ，可將 Core Web Vitals 評估功能帶入單頁應用程式。我們也推出了新的 [宣告式部分更新](https://developer.chrome.com/blog/declarative-partial-updates?hl=zh-tw) 基本類型，將原生無序 HTML 更新帶入平台，以及新的串流 API，方便您將 HTML 插入網頁，不必進行大量 DOM 操作。這些 API 現已開放測試。

### 8\. 使用「立即 UI」模式更新驗證機制

為配合身分識別更新，即時 UI 模式會將密碼和密碼金鑰整合為單一登入流程，並由瀏覽器管理。當使用者在您的網站上按一下「登入」時，Chrome 會自動顯示可用的憑證，讓使用者透過已儲存的密碼或密碼金鑰順利完成驗證。請參閱 [即時 UI 模式導入指南](https://developer.chrome.com/docs/identity/immediate-ui-mode?hl=zh-tw) ，瞭解如何開始使用。

### 9\. 根據實際流量資料規劃基準目標

您不必再使用匯出的 TSV 檔案來重新排列資料！使用全新的 [基準檢查工具](https://baseline-checker.chrome.dev/) ，直接連結至更新後的 Google Analytics API，準確掌握實際使用者支援現代化功能的百分比。選擇 Baseline 目標，放心地將最新功能發布給使用者，同時瞭解何時該使用備援。

---

## 運用 Chrome 內建 Gemini 大幅提升瀏覽體驗

我們在桌機、iOS 和 Android 裝置上推出 Chrome 內建 Gemini，為使用者提供強大的全新瀏覽、創作和工作方式。從透過自動瀏覽功能自動執行複雜的多步驟工作，到使用游標或語音進行直覺的多模態互動，Chrome 內建 Gemini 可讓使用者直接運用強大的生產力工具。

### 10\. Android 版 Google Chrome 內建 Gemini：手機上的瀏覽助理。

我們將於 6 月推出 Android 版 Chrome 內建 Gemini，這項個人瀏覽助理可協助你更深入瞭解網頁內容。你可以歸納長篇文章重點、提出特定問題，以及取得詳細說明，不必切換應用程式。除了回答問題，Gemini 還能與 Google 日曆、Keep 和 Gmail 等 Google 應用程式連結，協助你快速完成工作，是功能多元的效率提升工具。此外，有了 [個人化智慧服務](https://blog.google/products-and-platforms/products/search/personal-intelligence-expansion/?hl=zh-tw) ，只要選擇連結 Gmail 和 Google 相簿等應用程式，這個安全且能感知脈絡的瀏覽助理甚至可以根據你的獨特興趣、嗜好等提供量身打造的回覆。

### 11\. 使用自動瀏覽功能處理繁瑣工作

[Android 版自動瀏覽功能](http://blog.google/products-and-platforms/products/chrome/bringing-chrome-ai-to-android/?hl=zh-tw) 現已推出，可自動處理數位雜事，讓你充分運用 Chrome 內建 Gemini，專注於更重要的工作。有了自動瀏覽功能，你就能在 Android 手機上輕鬆完成各種工作，例如預約服務、規劃派對、尋找有現貨的商品等等。舉例來說，假設您即將前往觀賞喜劇表演，但忘了預訂停車位，自動瀏覽功能就能派上用場。只要在 Chrome 中問問 Gemini，Gemini 就會從票券中收集活動詳細資料，為你尋找停車位。

在電腦上，我們將於近幾個月內將自動瀏覽功能整合至 Gemini Spark，讓全天候個人 AI 代理在瀏覽器中為您採取行動。

### 12\. 使用 Nano Banana 隨時隨地改造圖像

使用 Android 裝置瀏覽網頁時，可以透過 Nano Banana 即時建立或自訂圖片。只要在學習時要求 Chrome 內建 Gemini「將這個網頁轉成資訊豐富的資訊圖表」，或是在瀏覽公寓時要求「修改圖片，加入現代客廳必備用品」，即可輕鬆完成。

### 13\. Chrome 中的「技能」：將最實用的 AI 提示變成一鍵工具

在 Chrome 中使用「技能」，即可儲存最實用的 AI 提示，並在電腦版 Chrome 內建 Gemini 中重複使用。儲存多個分頁的工作流程，例如在購物時並列比較規格，或掃描長篇文件以尋找重要資訊，然後隨時按一下滑鼠即可立即再次運作執行。

### 14\. 選取畫面內容，向 Chrome 內建 Gemini 提問

現在只要將滑鼠游標移到網頁的特定部分，就能問問 Chrome 內建 Gemini 相關問題，不必再費心描述具體內容。舉例來說，你可以在頁面上選取兩項產品，立即比較主要功能。如要使用 Nano Banana 編輯圖片，可以選取要變更的圖片部分

![圖片](https://developer.chrome.com/static/blog/chrome-at-io26/image/cursor-gemini_2880.png?hl=zh-tw)

使用指標比較網頁上的圖片。

### 15\. 在網路上使用語音

不久後，你就能在桌機版 Chrome 的網站上使用語音輸入文字。有了語音功能，撰寫留言、填寫表單中的長欄位或撰寫電子郵件等作業都會更加輕鬆自然。這項功能會使用 Gemini 模型清理轉錄稿，移除「嗯」和「啊」等語助詞，並根據脈絡調整內容，同時保留你的聲音，或按照你的要求填入欄位。

## 後續步驟

我們正處於邁向代理式網路的轉型期。我們將強大的基礎 AI 功能與日常網頁開發作業連結起來，消除過去阻礙創新進展的摩擦。

我們正從需要您完成所有繁重工作的網路，轉向主動 *為* 您工作的網路。無論您是想與瀏覽器型代理程式無縫整合、突破網頁的視覺極限，還是簡化自己的偵錯工作流程，我們都能提供工具，協助您打造網路的未來。

如要查看完整指南和深入技術說明，請前往 [developer.chrome.com](https://developer.chrome.com/?hl=zh-tw) 和 [web.dev](https://web.dev/?hl=zh-tw) 。我們很期待您運用這些新功能打造的內容。歡迎分享你的專案，並透過 [X](https://twitter.com/ChromiumDev) 、 [LinkedIn](https://www.linkedin.com/showcase/chrome-for-developers/) 和 [YouTube](https://www.youtube.com/user/ChromeDevelopers?hl=zh-tw) 與我們交流。

我們在下屆 Google I/O 大會再見！