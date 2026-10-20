---
title: "Fara：Microsoft 開源的電腦操作 AI，讓 Agent 真正幫你使用瀏覽器完成任務 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3946"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-16
created: 2026-06-16
description: "Fara 是 Microsoft 推出的開源 Computer Use Agent 模型，能透過螢幕截圖理解介面並執行點擊、輸入與瀏覽操作，讓 AI 從回答問題進化為真正完成任務的數位代理。"
tags:
  - "clippings"
status: processed
---
Fara 是 Microsoft Research 推出的開源 Computer Use Agent（CUA，電腦操作代理）模型與執行框架，核心目標是讓 AI 不只是回答問題，而是真正能夠「操作電腦完成任務」。與一般大型語言模型專注於文字生成不同，Fara 被設計成能夠觀看螢幕截圖、理解網頁內容，並直接執行點擊、輸入、搜尋、瀏覽等操作，進而完成訂餐、找工作、規劃旅遊、資料查詢等多步驟任務。

從 AI Agent 發展趨勢來看，Fara 屬於近年快速崛起的「Computer Use Agent」類別。這類系統的目標不再只是提供建議，而是能像真人使用電腦一樣完成工作流程。例如使用者只需輸入一句「幫我找下週東京機票」或「幫我比較三家飯店價格」，Agent 就會自行打開網站、搜尋資訊、分析結果並執行後續操作。Fara 正是 Microsoft 在這個領域的重要開源成果之一。

Fara 的核心模型為 **Fara-7B** ，這是 Microsoft 推出的首款 Agentic Small Language Model（SLM），模型規模僅 70 億參數，但專門針對電腦操作場景進行訓練。與許多需要數百億甚至上千億參數的大型代理系統相比，Fara-7B 強調以較小模型達到接近大型系統的 Computer Use 能力，同時具備更低的部署成本與本地執行潛力。

其運作方式與傳統聊天模型有明顯差異。Fara 的輸入包含：

- 使用者目標（Task）
- 當前螢幕截圖
- 過往操作歷史
- Agent 執行紀錄

模型會根據這些資訊推理下一步動作，例如：

- 點擊按鈕
- 輸入文字
- 捲動畫面
- 開啟連結
- 搜尋資訊
- 提交表單

它不是產生一段建議文字，而是直接輸出具體操作指令與座標位置。

從功能面來看，Fara 的主要特色可整理如下：

- 專為 Computer Use Agent 設計
- 使用螢幕截圖理解介面內容
- 透過點擊座標控制網頁
- 支援多步驟任務規劃與執行
- 可本機部署或雲端執行
- 開源模型權重與程式碼
- 支援 vLLM、LM Studio、Ollama 等部署方式
- 採用 MIT 授權釋出
- 可與 Browser Automation 工作流整合
- 支援高達 128K Context Length

這些設計讓 Fara 不只是模型，而是一個完整的 Agent 執行框架。

其中最值得關注的特色，是其訓練方式並非依靠大量真人操作資料，而是透過 Microsoft 提出的 **FaraGen** 系統自動產生訓練資料。由多個 Agent 組成的資料生成流程會自動建立任務、產生解法、驗證結果，再將成功案例納入訓練資料。這讓 Microsoft 能夠在缺乏大量真人電腦操作紀錄的情況下，快速建立大規模 Computer Use Dataset。

從技術架構來看，Fara 採用視覺語言模型（Vision-Language Model）路線。它不像傳統瀏覽器自動化工具直接讀取 DOM 結構，而是透過螢幕截圖理解介面內容。換句話說，它看到的是人類看到的畫面，而非網站底層程式碼。這種方法的優點是能適用於更多網站與應用程式，降低對特定網頁結構的依賴。

除了開源模型之外，Fara 也被整合進 Microsoft 的 **Magentic-UI** 生態中。官方文件指出，Magentic-UI 會搭配 Fara 作為瀏覽器操作模型，形成完整的 Agent 工作流程。這代表未來 Microsoft 的 Agent 系統將不只是聊天，而是逐漸發展成能夠執行實際工作的數位助手。

從使用情境來看，Fara 特別適合以下類型任務：

- 網頁資料搜尋與整理
- 飯店與機票比較
- 線上表單填寫
- 求職網站瀏覽
- 商品比價
- SaaS 系統操作
- 網站測試自動化
- 重複性瀏覽器工作流程

對於開發者而言，它也能作為 Browser Agent 或 Web Automation Agent 的基礎模型使用。

不過，Fara 目前仍屬於相對早期的 Computer Use Agent 技術。Microsoft 也在相關文件中強調，模型主要建議用於受控環境與低風險任務，涉及敏感帳號、金融交易或高風險操作時仍需人類監督。由於模型直接參與操作決策，因此安全性、權限管理與驗證機制仍是重要課題。

整體而言，Fara 不只是另一個開源語言模型，而是 Microsoft 對未來 Agent 時代的重要佈局。當生成式 AI 已逐漸從「回答問題」走向「完成任務」，Computer Use Agent 將成為下一波發展重點。Fara 透過小型化模型、開源權重與電腦操作能力，展示了 AI 從資訊提供者轉變為數位執行者的可能性，也讓更多開發者有機會在本機部署真正能操作電腦的 AI Agent。