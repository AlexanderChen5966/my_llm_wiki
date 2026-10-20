---
title: "Claude Opus 5.5 能做互動教學頁嗎？從官方 GPS 影片到可驗收的創意編程指南"
source: "https://www.aiposthub.com/claude-opus-5-5-interactive-learning-page-guide/"
author:
  - "[[Philo]]"
published: 2026-09-27
created: 2026-09-29
description: "Claude Opus 5.5 的官方 GPS 影片展示了互動教學頁。本文解釋它做了什麼、影片不能證明什麼，並提供新手可直接使用的需求範本與驗收方法。"
tags:
  - "clippings"
status: processed
---
Claude 官方影片裡，一個 GPS 教學頁讓抽象的衛星定位變得可以操作。這是 Claude Opus 5.5 的創意編程示範：模型協助做出互動網頁，再由官方拍成影片展示。它不代表 Opus 5.5 能直接輸出影片檔，也不代表每次輸入一句話，都能得到同樣成熟的成品。

如果你想用它做自己的教學工具，關鍵不是模仿影片畫面，而是把「要教懂什麼、讀者能操作什麼、怎樣算做對」寫清楚。本文用官方示範拆解這三件事，並提供一份可以直接修改的需求範本。

![Anthropic 官方 Claude Opus 5.5 發布主視覺](https://www-cdn.anthropic.com/images/4zrzovbb/website/f4d37a1d1f582f53f4e89440062b649b6273a093-1200x630.jpg)

Anthropic 官方 Claude Opus 5.5 發布主視覺

*Claude Opus 5.5 官方發布主視覺。圖片來源：* [*Anthropic 發布頁*](https://www.anthropic.com/claude-opus-5-5?ref=aiposthub.com) *。*

## 官方 GPS 影片展示的是什麼？

在 [〈GPS, explained by Claude Opus 5.5〉官方影片](https://www.youtube.com/watch?v=K-pgPNFcAj4&ref=aiposthub.com) 中，Claude 展示了一個解釋衛星定位的互動頁。影片說明，衛星持續廣播時間訊號，手機利用四個訊號求出位置與時間。官方描述也特別指出，畫面是依公開 GPS 星曆製作的模擬，不是接收即時衛星無線電訊號。

![](https://www.youtube.com/watch?v=K-pgPNFcAj4)

*Claude 官方頻道展示 Opus 5.5 製作的 GPS 互動教學頁。影片來源：Claude 官方 YouTube 頻道。*

這個例子的價值在「可操作的解釋」。一張靜態圖只能畫出衛星與手機的位置。互動頁則可以讓讀者改變條件，觀察定位結果如何變化。對老師、內容團隊或產品設計師而言，這是拿來測試概念是否容易理解的原型，不等於已通過科學準確性、無障礙或跨裝置驗收。

官方另有 [〈Introducing Claude Opus 5.5〉](https://www.youtube.com/watch?v=1f13Bl1sYkw&ref=aiposthub.com) 與 [〈Using Claude Opus 5.5 as your daily driver〉](https://www.youtube.com/watch?v=jKRl_CSVxyI&ref=aiposthub.com) 。前者介紹模型定位，後者以日常 Claude Code 工作說明速度、額度與工作方式。影片是認識產品的入口。要判斷自己的題目能否完成，仍得用同一份需求與驗收條件親自測試。

![](https://www.youtube.com/watch?v=jKRl_CSVxyI)

*Claude 官方以日常 Claude Code 工作示範 Opus 5.5。影片來源：Claude 官方 YouTube 頻道。*

延伸閱讀： [Claude Opus 5.5 發布：創意編程亮眼，成本與評測該怎麼看？](https://www.aiposthub.com/claude-opus-5-5-creative-coding-showcase-deep-dive-2026/)

## 新手怎麼開始做第一個互動頁？

最省事的入口是 Claude 的 Artifacts。Artifact 是 Claude 放在對話旁邊、可以反覆編輯的小作品，能是單頁網站、圖表或互動工具。 [Claude 官方說明](https://support.claude.com/en/articles/17153992-what-are-artifacts-and-how-do-i-use-them?ref=aiposthub.com) 指出，Free、Pro、Max、Team 與 Enterprise 方案都能在聊天中建立 Artifact，但管理員設定與「雲端程式執行和檔案建立」功能會影響能否使用。

先挑一個可以用三十秒說明的題目，例如「讓國中生拖動光源，看影子長度怎麼變」。不要一開始就要求完整課程平台。第一版只需要一個可操作變數、一個會跟著變的畫面，以及一句解釋結果的文字。這樣你才能判斷互動是否真的幫助理解。

以下需求可以直接貼到 Claude，將方括號內的內容換成你的主題：

> 請製作一個繁體中文互動教學頁，對象是［讀者年齡或背景］，主題是［概念］。讀者操作［一個滑桿或按鈕］時，畫面必須即時顯示［可觀察的變化］，並用一句白話說明原因。請先列出你使用的假設與資料來源，再製作頁面。完成後檢查手機寬度、鍵盤操作、文字對比，以及操作到最小值和最大值時是否出錯。若某個數值只是示意，請直接在頁面標示「示意」。

這份需求刻意把「讀者要學到什麼」放在視覺效果之前。若模型做出漂亮動畫，卻沒有解釋操作與結果的關係，就要回到教學目標修改，而不是只增加更多特效。

## 怎麼分辨好看的展示與能交付的作品？

可以用四個問題驗收第一版。讀者能否在手機上操作？只用鍵盤能否完成主要互動？把控制項調到邊界時，數值與畫面是否仍合理？每個科學或財務數值是否有來源，或清楚標為示意？這些檢查比單看影片流暢度更能決定作品能否交給真正的讀者。

若你使用 Claude Code 開發網站，還要看程式碼與測試。請模型列出改了哪些檔案、如何執行、通過哪些測試，以及仍有哪些限制。Anthropic 把 Opus 5.5 定位於長時間編程與知識工作。官方發布的測試與客戶案例支持「值得試」的判斷，但不能替你的專案驗收。尤其有資料輸入、帳號或付費功能時，畫面完成只是開始。

延伸閱讀： [Claude Opus 5.5 是什麼？效能、價格、安全限制與升級建議完整解析](https://www.aiposthub.com/claude-opus-5-5-performance-price-safety/)

## 需要先買最貴的方案嗎？

不需要為了一頁原型先買 API。Claude 官方 Help Center 指出，免費方案也能在聊天中建立 Artifact。如果你的帳號沒有看到功能，先檢查能力設定與組織管理限制。當你需要反覆修改大型程式庫、讓模型使用工具或跑長時間任務時，再比較付費方案與 Claude Code 是否合適。

如果是開發者直接呼叫 API，Opus 5.5 的標準牌價截至 2026 年 9 月 27 日為每百萬輸入 token 4 美元、輸出 token 20 美元。 [Anthropic 官方模型目錄](https://platform.claude.com/docs/en/models/opus-5-5/overview?ref=aiposthub.com) 也列出 100 萬 token 上下文視窗。Token 是模型讀入與產生內容的計費單位。上下文視窗是一次可放進對話的內容容量，不代表內容越長越準。Anthropic 稱典型任務總成本較 Opus 5 低約 40%，那是特定工作負載的估計，不能直接套用到每個人的帳單。

## 常見問題

### Opus 5.5 可以直接生成影片嗎？

這次找到的官方 GPS 影片展示的是 Claude 製作的互動網頁，再以影片呈現。官方模型文件把 Opus 5.5 的輸入列為文字與圖片、輸出列為文字，因此不能把這段展示解讀為模型本身直接輸出影片檔。若你的目標是成品影片，仍需另外安排錄製、剪輯或影片工具。

### 沒有程式背景也能試嗎？

可以從 Claude 聊天中的 Artifact 開始。先選一個小主題，說清楚對象、可操作的變數與驗收條件，再逐輪修改。正式網站涉及部署、資料安全與維護，仍需要對應的工程檢查。

### 官方展示能當成產品效能保證嗎？

不能。展示只證明那個作品存在，沒有提供同一需求反覆生成的成功率。若要評估是否適合團隊，請用自己的題目記錄第一次可用版本所花的時間、人工修正次數與最終驗收結果。

## 結論：先做一個可檢查的小作品

Opus 5.5 的 GPS 示範最值得借鏡的，是把抽象知識轉成可操作的解釋。對新手來說，先用 Artifact 做一頁只有一個核心互動的原型，請一位真正的讀者試用，再檢查事實、手機操作與邊界狀態。若這一頁能穩定教懂概念，再決定是否擴成網站或投入更長的 Claude Code 專案。

## 資料來源

- [Claude 官方影片：GPS, explained by Claude Opus 5.5](https://www.youtube.com/watch?v=K-pgPNFcAj4&ref=aiposthub.com)
- [Claude 官方影片：Introducing Claude Opus 5.5](https://www.youtube.com/watch?v=1f13Bl1sYkw&ref=aiposthub.com)
- [Claude 官方影片：Using Claude Opus 5.5 as your daily driver](https://www.youtube.com/watch?v=jKRl_CSVxyI&ref=aiposthub.com)
- [Claude Help Center：What are artifacts and how do I use them?](https://support.claude.com/en/articles/17153992-what-are-artifacts-and-how-do-i-use-them?ref=aiposthub.com)
- [Claude Platform Docs：Opus 5.5 模型資訊](https://platform.claude.com/docs/en/models/opus-5-5/overview?ref=aiposthub.com)