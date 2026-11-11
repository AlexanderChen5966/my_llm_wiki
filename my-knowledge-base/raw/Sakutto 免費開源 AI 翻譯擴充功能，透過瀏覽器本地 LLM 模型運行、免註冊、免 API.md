---
title: "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API"
source: "https://www.kocpc.com.tw/archives/702653"
author:
  - "[[Rocky]]"
published: 2026-11-09
created: 2026-11-10
description: "LLM 模型的翻譯品質，幾乎都比 Google、微軟等等的老牌翻譯工具還要好，也因此對於很多人來說，一定會希望改用 LLM 模型。雖然現在有很多擴充功能可以串接本地的 LLM 模型，進而做到幫網頁翻譯，但用過的人應該都有這感覺：「速度還是太慢了」，久而久之還是繼續回去用 Google 翻譯。最近就發現到這款新的免費擴充功能 Sakutto，它是直接把 LLM 模型下載到 Chrome 瀏覽器裡，因此不僅無需填 API Key，翻譯速度也更快，我實測確實如此，真的快非常多，推薦給大家。"
tags:
  - "clippings"
status: "inbox"
---
LLM 模型的翻譯品質，幾乎都比 Google、微軟等等的老牌翻譯工具還要好，也因此對於很多人來說，一定會希望改用 LLM 模型。雖然現在有很多擴充功能可以串接本地的 LLM 模型，進而做到幫網頁翻譯，但用過的人應該都有這感覺：「速度還是太慢了」，久而久之還是繼續回去用 Google 翻譯。最近就發現到這款新的免費擴充功能 Sakutto，它是直接把 LLM 模型下載到 Chrome 瀏覽器裡，因此不僅無需填 API Key，翻譯速度也更快，我實測確實如此，真的快非常多，推薦給大家。

## Sakutto 免費 AI 翻譯擴充功能介紹

Sakutto 是一款免費、開源的 Chrome AI 網頁翻譯擴充功能，由日本開發者 jammaru 製作，利用瀏覽器裡的本地大型語言模型（LLM）處理翻譯工作，不必將網頁文字傳送到外部翻譯伺服器。

目前搭配的 LLM 模型是 PLaMo 3 Translate，透過瀏覽器內的運算環境執行翻譯，而不是呼叫雲端或本地 AI API。安裝後可以翻譯整個網頁，也能只選取感興趣的段落，或開啟文字翻譯頁面，貼上文字取得結果。閱讀方式可選擇讓譯文直接取代原文，或同時顯示原文與譯文。

語言支援方面，PLaMo 3 Translate 可處理 53 種語言，網頁來源語言會自動偵測，預設翻譯目標則依瀏覽器介面語言設定，也能針對目前網站調整。對於未涵蓋的語言組合，擴充功能會改用 Chrome 內建的本地翻譯能力。除了手動翻譯，Sakutto 還提供特定網站自動翻譯、用語詞彙表及翻譯快取等功能。

首次啟用時，Sakutto 會從 Hugging Face 下載 AI 模型，預設的 0.8B 模型約為 573 MB，也可選用體積更大的 2B 模型。模型下載並儲存完成後，就能在離線狀態下使用 PLaMo 翻譯。實際速度還是會受到電腦硬體、文章長度及選用模型影響。

隱私部分，開發者表示 Sakutto 不收集翻譯內容、不設廣告，也沒有使用行為分析或追蹤機制。翻譯原文、譯文、詞彙表及快取資料保留在使用者裝置上。

**主要特色**

- 完全免費使用：不需要付費訂閱，沒有廣告或付費進階功能。
- 免帳號、免 API Key：安裝 Chrome 擴充功能即可開始設定使用。
- 本地 AI 翻譯：透過 PLaMo 3 Translate 在使用者裝置上執行翻譯，支援 53 種語言。
- 整頁網頁翻譯：直接將外文網頁內容轉換成指定語言。
- 選取文字翻譯：只翻譯需要閱讀的文字段落。
- 支援手動輸入翻譯：可貼上文字，不必另外開啟翻譯網站。
- 原文與譯文對照：提供替換原文及雙語對照顯示模式。
- 支援離線翻譯：AI 模型下載完成後，可在沒有網路的情況下使用。
- 提供模型選擇：預設使用 0.8B 模型，也能選擇較大的 2B 模型。
- 支援鍵盤快捷鍵：快速翻譯整頁、選取文字及切換顯示模式。
- 開源透明：擴充功能程式碼公開於 GitHub，採 MIT 授權。

[取得 Sakutto](https://chromewebstore.google.com/detail/sakutto-local-ai-translat/mhlbjjijfkoghmcjndgdimhdlfkhadfl)

安裝好 Sakutto 後，會先跳出使用說明和同意條款：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130538_0_7a11a0.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

再來會預設下載比較小的 0.8B 模型，如果你想要用 2B 模型，之後可以到設定裡面切換：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130539_0_e230db.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

模型下載好後就能開始使用：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130539_0_831b90.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

使用方式很簡單，打開要翻譯的網頁後，可以直接用快捷鍵，或是按擴充功能圖示的 Translate this page 功能、右鍵選單的翻譯功能，就會進行翻譯：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130541_0_cbcd3e.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

接著你就會看到，文字從網頁最上方開始翻譯，畫面也會顯示 Token 速度、還剩多少沒有翻譯：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130541_0_7e6621.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

速度部分每台電腦都不太一樣，下方 GIF 圖檔是在我電腦運作的模樣：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109131811_0_807e55.gif "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

如果你想要保留原文，翻譯文在下方顯示的話，切換到 Bilingual 模式：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130543_0_9e9e97.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

就會變成這樣：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130544_0_75eb44.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")

設定裡面的 MODEL 選項，可以切換你要使用的模型：  
![Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/11/20261109130545_0_13dab8.jpg "Sakutto 免費開源 AI 翻譯擴充功能，透過瀏覽器本地 LLM 模型運行、免註冊、免 API - 電腦王阿達")