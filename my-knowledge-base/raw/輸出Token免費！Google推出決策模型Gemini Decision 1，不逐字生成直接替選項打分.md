---
title: "輸出Token免費！Google推出決策模型Gemini Decision 1，不逐字生成直接替選項打分"
source: "https://www.bnext.com.tw/article/93032/google-gemini-decision-1-decisions-api"
author:
  - "[[李先泰]]"
published: 2026-11-12
created: 2026-11-13
description: "Google推出Gemini Decision 1，不逐字寫答案、只替選項打分，專攻是非、選擇與評分判斷，約0.25秒回傳，輸出token完全免費。"
tags:
  - "clippings"
status: "inbox"
---
如果只是要AI回答「是」或「不是」，其實不必讓它寫出一整段話。Google在美國時間11月11日推出Gemini Decisions API，搭配新模型Gemini Decision 1，專門處理是非、選擇與評分三種有固定選項的判斷題。模型不逐字生成回答，而是一次替所有選項打分，回傳機率與信心分數。

官方稱輸入在1.6萬個token以內時，從送出請求到收到結果約0.15至0.25秒。計費方式也跟著改變，每100萬個輸入token收0.10美元（約新台幣3.3元），輸出token完全不收費。

## 不逐字寫答案、只替選項打分，決策模型在做什麼？

token是模型處理文字的最小單位，即使只要簡短答案，一般生成式模型仍需逐token產生輸出。Gemini Decision 1的做法不同：開發者事先列好選項，模型只跑一次運算（官方稱single forward pass），就替每個選項算出機率。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

它能回答三種題型。是非題估算某個條件成立的機率；選擇題從2到255個選項中挑出答案，並列出每個選項的機率；評分題則依開發者訂的分級量表打分，例如把一則資安警示按嚴重程度分級。

每個答案都附上0到1的信心分數，分數偏低的案例，可以交給備援流程或人工審核。輸入資料可以混用文字、圖片、音訊、影片與PDF，上限25.6萬個token，一次請求最多能問1,000個問題。

官方舉例，可以把一段通話錄音和帳戶資料放在一起判讀，或把掃描的理賠申請PDF搭配損害照片一起審核。

## 輸出token為什麼可以免費？

這跟它的運作方式有關。官方說明，Gemini Decision 1不逐字生成答案文字，而是直接回傳各選項的機率與信心分數，輸出token一律0美元，也不另收思考token、語音轉文字與文字辨識（OCR）的費用。計費只看輸入：同一份輸入資料每次請求只算一次，同時問再多問題，每題也只加收題目與選項說明的token。

速度方面，在輸入1.6萬個token以內的情況下，模型運算時間的中位數約50至100毫秒，而且一次問1題和問100題，延遲幾乎沒有差別。Google表示，在官方的共用基準測試組上，用Decisions API呼叫Gemini Decision 1，成本約為用Interactions API呼叫入門款模型Gemini 3.5 Flash-Lite的五分之一。

攤開官方跑分表，9項測試中Gemini Decision 1拿下7項最高分，不過其中音訊（MMAU）與影片（Video-MME）2項，新創TypeSafe的Jev與OpenAI的GPT-6 Luna都沒有成績。三家都有成績的7項測試裡，Gemini Decision 1贏5項、Jev贏2項。

在這7項共用測試中，衡量「信心分數準不準」的校準誤差，以Gemini Decision 1最低。成本則是Jev略勝，跑完7項共用測試，Jev花0.68美元、Gemini Decision 1花0.88美元、GPT-6 Luna花0.93美元，Gemini 3.5 Flash-Lite則要4.14美元。

## Google內部已在用，決策模型成AI新戰場

Google自家產品已先拿Decisions API當「分流器」。Gemini Notebook用它即時判斷每個查詢該交給哪一級模型、用多深的思考模式，讓一般查詢的延遲最多降低40%；Google搜尋則把它當成MCP工具（讓AI連接外部工具與資料的標準協定）的安全層，偵測對抗性的行為與內容。

設計工具Stitch的新實驗功能rapid canvas，也用它在數秒內產生可操作的產品流程草圖；Google Labs的CC則用它從200多個選項中挑選表情符號回應。

開發者已可在Google AI Studio申請API金鑰使用，並有免費額度可做原型測試；OpenRouter、LangChain、Vercel等開發平台的串接陸續推出中。截至11月11日，Google Cloud上的Gemini尚未支援，官方標示為coming soon。

Google並不是第一個進場的玩家。《SiliconANGLE》指出，第一個決策模型是新創TypeSafe在9月中推出的Jev，上線數天用戶就破百萬，TypeSafe隨後在10月以約75億美元（約新台幣2,468億元）估值募得約8.7億美元（約新台幣286億元）。

OpenAI則在9月29日的開發者大會發表同類的Decisions API，《TechCrunch》報導，10月22日起開放給所有客戶。從新創、OpenAI到Google，約兩個月內，三家都推出了專做判斷題的模型。

> 延伸閱讀： [不能生成文字卻暴紅！ChatGPT技術推手打造AI決策模型Jev，為何讓開發者搶到當機？](https://www.bnext.com.tw/article/92319/typesafe-jev-decision-model-explainer)

資料來源： [Google官方部落格](https://blog.google/innovation-and-ai/technology/developers-tools/gemini-decisions-api/) 、 [《SiliconANGLE》](https://siliconangle.com/2026/11/11/google-enters-the-decision-model-market-with-gemini-decisions-api/) 、 [《TechCrunch》](https://techcrunch.com/2026/10/22/openai-makes-its-jev-competitor-widely-available-as-decision-model-competition-heats-up/)

本文初稿為AI編撰，整理．編輯/ 李先泰