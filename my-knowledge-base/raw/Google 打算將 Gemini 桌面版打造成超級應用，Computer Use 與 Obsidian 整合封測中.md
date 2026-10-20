---
title: "Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中"
source: "https://www.kocpc.com.tw/archives/667941"
author:
  - "[[達小編]]"
published: 2026-09-06
created: 2026-09-08
description: "Gemini Spark 的概念最早在 5 月 Google I/O 2026 上亮相，這是一個持續在雲端運作、連結 Workspace 並使用 Gemini 3.5 與 Antigravity 框架的 AI 代理。換句話說，目前的 Gemini 桌面版已經具備基本的本地檔案操作與 Google Workspace"
tags:
  - "clippings"
status: processed
---
**[Google](https://www.kocpc.com.tw/archives/tag/google-2)** 正在將 **[Gemini 桌面版](https://www.kocpc.com.tw/archives/tag/gemini-%e6%a1%8c%e9%9d%a2%e7%89%88)** 應用逐步打造成一款真正的超級應用程式（superapp），目標直指 OpenAI 的 Codex 與 Anthropic 的 Claude Desktop。根據 [TestingCatalog](https://www.testingcatalog.com/google-keeps-transforming-gemini-desktop-into-superapp/) 的獨家揭露，最新的封閉測試版本加入了全新的 Ask 與 Assign 雙模式切換開關、Obsidian 筆記整合、Finder 資料夾捷徑以及跨電腦連線功能。這些變化讓 Gemini 從單純的對話機器人，進化成具備完整本地端操作能力的 AI 代理平台。以下先釐清哪些功能已上線、哪些仍在測試中。  

## Google 打算將 Gemini 桌面版打造成超級應用：Gemini 桌面版與 Spark

Gemini 桌面版應用本身早在今年 [上半年](https://www.kocpc.com.tw/archives/639218) 就已推出，目前 macOS 用戶可以透過官方管道下載使用。6 月底，Google 將 Gemini Spark 個人代理正式帶到 macOS 平台，讓美國地區的 AI Ultra 訂閱用戶能夠自動化處理本地檔案任務，例如將下載資料夾中的 PDF 分類整理到指定位置，或是結合 Google Workspace 建立預算試算表與定期排程更新。  
![Gemini 正式推出 Mac 版桌面應用程式，支援視窗分享、一鍵呼叫等功能 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/04/20260416102901_0_e2f39c.jpg "Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達")

Gemini Spark 的概念最早在 5 月 Google I/O 2026 上亮相，這是一個持續在雲端運作、連結 Workspace 並使用 Gemini 3.5 與 Antigravity 框架的 AI 代理。換句話說，目前的 Gemini 桌面版已經具備基本的本地檔案操作與 Google Workspace 整合能力，但以下這些新功能才是真正將 Gemini 推向超級應用的關鍵。

### Ask 與 Assign：借鏡 Codex 的雙模式設計（封閉測試中）

在最新的封閉測試版本中，Gemini 桌面版加入了一個模式切換開關，讓用戶能在 Ask 與 Assign 兩種模式之間自由切換。這個設計邏輯與 OpenAI Codex 的 Chat 和 Work 雙模式如出一轍：Ask 是標準的 Gemini 對話介面，而 Assign 則類似 Google 先前發表的 Gemini Spark 個人代理，但具備更強大的擴展能力。  
![Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260906154300_0_f94769.jpg "Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達")

Assign 模式的核心特色在於檔案夾層級的操作範圍控制，用戶可以設定一個預設工作資料夾、回到上一次使用的資料夾，或為特定任務選擇全新資料夾，將代理的操作嚴格限定在指定的範圍內。這種設計與 Codex 的 project-scoped 工作模式相似，確保 AI 代理不會越權存取不應觸及的檔案。

### 跨電腦連線與 Computer Use（封閉測試中）

除了資料夾層級的控制，測試版本還包含一個連接其他 Gemini 桌面版電腦的選項。雖然具體的連線機制尚未明朗，但很可能類似 Codex 的遠端控制架構（Remote），讓用戶能透過一台電腦操控另一台電腦上的 Gemini 工作。  
![Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260906154833_0_4136d5.jpg "Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達")

Computer Use 功能的加入是一大亮點。用戶可以標記 computer use 技能，從 Gemini 桌面版直接觸發操作本地應用程式的任務。這與 Anthropic Claude Desktop 的 computer use 能力直接對標，讓 AI 代理能夠實際操控桌面的各種應用程式，而不只是輸入文字框。舉例來說，用戶可以要求 Gemini 自動開啟瀏覽器、操作特定網頁並回傳結果，整個流程不需手動介入。  
![Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260906154705_0_ff2a84.jpg "Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達")

### Obsidian 整合：筆記工具成為知識庫入口（封閉測試中）

另一個值得關注的整合是 Obsidian，Google 正在測試 Gemini 與 Obsidian 筆記工具的連線功能。Obsidian 作為以 Markdown 為基礎的本地端筆記應用，儲存在名為 vault 的資料夾中。這項整合可能指向更廣泛的知識庫概念：讓 Gemini 能直接讀取用戶的個人筆記庫來提供更精準的回應。  
![Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260906154944_0_741abf.jpg "Google 打算將 Gemini 桌面版打造成超級應用，Computer Use 與 Obsidian 整合封測中 - 電腦王阿達")

目前尚不清楚 Google 是打算直接支援 Obsidian 作為長期的知識庫整合方案，還是內部先用 Obsidian 進行測試，待自家等效功能開發完成後再取代。但對於重度使用 Obsidian 的使用者來說，這項整合意味著個人知識管理與 AI 代理之間將產生更緊密的連結，可以直接對個人筆記提問、摘要與分析。

![](https://x.com/i/status/2096218506622579058)

### 自訂標籤頁與 Finder 整合（封閉測試中）

Google 還在開發一個 Customize 標籤頁，讓用戶可以在其中選擇技能、應用程式、連接器或外掛。不過，目前這個標籤頁的功能仍相當有限，主要提供的還是 Google 自家應用或現有 Gemini 介面中已有的擴充功能。外界預期 Google 將會像競爭對手那樣，後續加入完整的 MCP（Model Context Protocol）支援與第三方外掛生態系。

在 macOS 端，Finder 整合將讓用戶能直接將任何資料夾傳送給 Gemini 進行檢查或執行指定任務。這項功能進一步強化了 Gemini 在 macOS 上的本地操作能力，與已上線的 Gemini Spark macOS 版形成互補：Spark 負責持續性代理任務，而 Finder 整合提供即時的檔案操作捷徑。

### Gemini 4 與 Nano Banana 2.5 Flash 的程式碼線索（未發表）

程式碼追蹤顯示，Google 正在準備讓 Gemini 桌面版支援下一代主要模型，可能是 Gemini 4。同時，程式碼中也出現了 Nano Banana 2.5 Flash 的升級跡象。Nano Banana 系列是 Google DeepMind 的多模態生成模型，2.5 Flash 版本預期將帶來更快的推理速度與更強的多模態處理能力，涵蓋圖像生成、影片編輯與即時媒體處理等應用場景。

### 超級應用競賽：三巨頭各自布局

目前測試中的 Ask/Assign 雙模式、Obsidian 整合、Computer Use 與跨電腦連線，將 Gemini 桌面版的定位從「個人代理」推向「超級應用平台」。這個路徑與 OpenAI Codex 從程式碼助手擴展為全能代理、Anthropic Claude Desktop 從對話介面進化為桌面操作中樞的發展方向高度一致，三家巨頭正在各自打造不同風格的 AI 作業系統。

### 結語

總結來說，Gemini 桌面版已經上線並可正常使用，Gemini Spark 的本地檔案操作與 Workspace 整合也已開放給 AI Ultra 用戶。但這篇報導揭露的 Ask/Assign 雙模式、Obsidian 整合、Computer Use、跨電腦連線與 Finder 捷徑等功能，目前都處於封閉測試階段，公開釋出的時間表仍然不明。從程式碼中的功能完整度來看，Google 對 Gemini 桌面版的野心相當明確：它不再只是一個 AI 聊天機器人，而是一個能直接操作本地檔案、連接第三方工具、跨裝置協作且即將搭載最新模型的超級應用平台。