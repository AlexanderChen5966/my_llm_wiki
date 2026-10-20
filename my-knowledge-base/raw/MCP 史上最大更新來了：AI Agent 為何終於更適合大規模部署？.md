---
title: "MCP 史上最大更新來了：AI Agent 為何終於更適合大規模部署？"
source: "https://techorange.com/2026/07/29/mcp-update-ai-agent/"
author:
  - "[[廖紹伶]]"
published: 2026-07-29
created: 2026-07-30
description: "這一年，越來越多企業開始嘗試導入 AI Agent，卻大多卡在同一關：試點跑得動，正式環境卻上不了。很多人以為問題出在模型不夠聰明或找不到商業價值，但真正的癥結可能在更底層：串起 AI Agent 與各種軟體的那套協定：MCP（Model Context Protocol），本身還沒準備好承接企業級的規模。"
tags:
  - "clippings"
status: processed
---
這一年，越來越多企業開始嘗試導入 AI Agent，卻大多卡在同一關：試點跑得動，正式環境卻上不了。很多人以為問題出在模型不夠聰明或找不到商業價值，但真正的癥結可能在更底層：串起 AI Agent 與各種軟體的 MCP（Model Context Protocol）協定，本身還沒準備好承接企業級的規模。

如今這道關卡開始鬆動。MCP 本週推出問世以來最大一次改版，架構核心從「有狀態」（stateful）全面轉向「無狀態」（stateless），規格由 Linux Foundation 旗下的 Agentic AI Foundation（AAIF）主導。這個工程轉向聽來抽象，但值得關注的是究竟為何拿掉「有狀態」這塊地基，反而讓 AI Agent 更適合大規模部署呢？

## 卡住企業的，從來不是 AI 本身

根據《VentureBeat》報導，AAIF 執行董事 Mazin Gilbert 表示，自己接觸過打算部署上萬個 AI Agent 的公司，若不朝這個方向改，根本做不到規模化，而真正的障礙不在技術，也不在商業案例，而在協定底層必須先動的一些基本設計。

舊架構的運作方式就是問題之一。過去，MCP 客戶端得和特定一台伺服器維持連線，伺服器會把後續每個請求「釘」在發出連線的那台機器上。這在本機執行的桌面應用還行得通，但一旦搬上雲端、要靠多台可隨時增減的伺服器橫向擴展，就成了麻煩：只要那台機器當掉，AI Agent 正在做的工作也跟著中斷。為了維持連續性，企業得額外架設把請求導回同一台的機制，或讓流量閘道逐一拆解請求內容才知道該送往哪裡。主要維護者 Den Delimarsky 形容，舊版本下「只要一個運算節點掛掉，請求就會開始失敗」。

換句話說，企業得額外替協定處理分散式部署的問題，而不是直接利用既有的雲端架構。對想把 AI Agent 從試點推進正式環境的團隊來說，卡關處可能根本不在 AI，而在它能不能跑上企業現成的基礎設施。

## MCP 從實驗室專案，跨進企業級基礎設施

Gilbert 認為，一套協定要真正進入企業正式環境，需要同時具備三個條件：開放標準、可擴展的 Stateless 架構，以及中立治理。而這些在半年前都還沒到齊，這次改版正是要補上缺口。

其中最關鍵的，是可擴展的 Stateless 架構本身。改版後，MCP 伺服器可以像一般網路服務一樣，直接部署在負載平衡器後面，用企業手上現成的雲端維運工具就能撐起規模，不必再為它另建一套特殊機制。狀態並沒有憑空消失，而是從協定層交回應用層自行管理，開發者可依自己的環境決定怎麼保存。Gilbert 以網際網路作比：當年 HTTP 能贏得全球信任，靠的正是開放標準、無狀態擴展與中立治理這三件事同時成立。

架構之外，企業把協定放進正式環境還有兩個顧慮：穩定與安全，這次改版也一併補強。針對穩定性，這次首度訂出正式的功能棄用政策，保證任何功能從宣告淘汰到真正移除，至少間隔 12 個月。對要把 MCP 寫進長期系統的大型企業而言，這是一份不會被無預警推翻的承諾。《The New Stack》補充，這個 12 個月下限只有在出現公開安全通報時才可縮短，且硬底線是 90 天。

安全方面，新版強制客戶端驗證授權回應的來源，藉此封堵一類名為「混淆攻擊」（mix-up attack）的手法。在這種攻擊裡，客戶端可能被誘導，把授權回應誤認為來自另一個身分伺服器。據《VentureBeat》報導，Delimarsky 強調這是預防性的工程強化，並非因為 MCP 實際遭到攻擊。此外，新版與身分服務商 Okta 合作推出「企業受管授權」擴充，讓企業能以既有的公司身分系統，統一控管誰能存取哪些 MCP 伺服器，避免資料流向未經授權的來源。

Gilbert 因此形容，MCP 走到這一步，「已經從開放實驗室的實驗，變成我們所謂的『企業就緒』（enterprise ready）」。

## 長任務、互動式介面正式納入協定，開發者終於不用再各自造輪子

這次更新另一個容易被忽略的重點，是 MCP 首次把 Tasks 與 MCP Apps 納入正式規格。乍看之下，這只是新增兩項功能，但真正改變的是：許多原本每家公司都得自己實作的能力，開始變成協定的一部分。

以 Tasks 為例。過去如果 AI Agent 要轉錄一段一小時的會議錄音、分析大型程式碼庫，或等待 ERP 完成批次作業，開發者通常只能一直維持連線等待結果，或另外設計一套背景任務（Background Job）與通知機制。一旦 Client 中途關閉、網路中斷，還得自行處理任務恢復、重試與狀態同步。

新版 MCP 則把這類長時間任務納入標準流程。Server 可以先回傳 Task Handle，Client 日後再查詢執行結果，不需要一直維持連線。對開發者而言，這代表背景工作、重試機制與任務狀態管理，不再需要每個團隊各自設計一套做法。

另一項 MCP Apps 也有類似意義。過去，AI Agent 即使知道該請使用者確認資料、填寫表單或查看報表，也只能先輸出一段文字，再由開發者自行建立 Web 頁面或前端介面串接。新版 MCP Apps 則允許 Server 直接回傳互動式介面，把表單、儀表板等 UI 納入協定，讓不同 AI Client 能以一致方式呈現。

換句話說，這次更新並不是讓 AI Agent「可以顯示介面」，這件事本來就做得到，而是把原本每家公司各自實作的能力，逐漸變成整個生態系共用的標準。

## 從一家公司的開源專案，到多廠商共治的代理網際網路

MCP 由 Anthropic 於 2024 年 11 月創建，並在 2025 年 12 月捐給 Linux Foundation 旗下的 AAIF。如今核心維護者橫跨 Anthropic、微軟、OpenAI、Google 與 Amazon 五大科技公司；MCP 共同創建者暨主要維護者 David Soria Parra 坦言，自己在技術上仍握有否決權，卻從未真正動用，Anthropic 的貢獻占比也已降到半數以下。

更值得注意的是社群組成的變化。據《VentureBeat》報導，AAIF 成員已從去年 12 月成立時的約 40 家，成長到如今的 240 家，且早已不限科技廠商，擴散到零售、金融、電信等領域，這些「使用者」出現在名單上，代表他們要的不只是採用協定，更是進場影響協定的走向。

Soria Parra 表示，MCP 各語言 SDK 的下載量在過去半年翻倍，目前每週約達 2.5 億次；作為對照，2025 年 12 月捐贈當時，光是 Python 與 TypeScript 兩個 SDK 的下載量為每月約 9,700 萬次。因此，這次更新真正重要的，不只是 Stateless、OAuth 或 Tasks 等新功能，而是補齊了一套開放協定走向企業級基礎設施所需要的工程能力。對希望將 AI Agent 從試點推向正式環境的企業來說，這或許才是 MCP 最大一次更新真正的意義。

> 【推薦閱讀】
> 
> ◆ [Anthropic Mythos 找到密碼攻擊新方法：量子電腦之外，AI 成加密安全新變數？](https://techorange.com/2026/07/29/anthropic-mythos-cryptographic-weaknesses/)
> 
> ◆ [人人都會用 AI 後，真正的績效差距從哪來？德州大學、KPMG 實驗點出關鍵](https://techorange.com/2026/07/28/why-some-workers-turn-ai-into-results-and-others-dont/)
> 
> ◆ [4 個人，服務 75 萬名客戶：研究揭 AI 原生企業正在改寫「請更多人」老規則](https://techorange.com/2026/07/21/ai-native-firms-startup/)

＊本文開放合作夥伴轉載，資料來源： [《VentureBeat》](https://venturebeat.com/infrastructure/mcp-just-got-its-biggest-update-ever-heres-what-changes-for-ai-agents) 、 [Model Context Protocol](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/) 、 [《The New Stack》](https://thenewstack.io/mcp-release-candidate-rewrite/) 、 [Agentic AI Foundation](https://aaif.io/blog/mcp-is-growing-up?utm_source=chatgpt.com) ，首圖來源：AI 工具生成

![](https://sslcode.adgeek.com.tw/public/images/popad_close_button.png)