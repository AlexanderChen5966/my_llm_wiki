---
title: "阿里一面，我霸氣反問：你說你們在做 Agent 專案，說說 LangChain、multi-agent、A2A 這些你們都是怎麼做的？面試官一直在擦汗。。"
source: "https://codelove.tw/@tony/post/qvdeX3"
author:
published:
created: 2026-07-03
description: "阿里一面，我霸氣反問：你說你們在做 Agent 專案，說說 LangChain、Multi-Agent、A2A 這些你們都是怎麼做的？面試官一直在擦汗。。  ======================================================================..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月02日](https://codelove.tw/@tony/post/qvdeX3 "2026-07-02 19:07:23")

## 阿里一面，我霸氣反問：你說你們在做 Agent 專案，說說 LangChain、multi-agent、A2A 這些你們都是怎麼做的？面試官一直在擦汗。。

## 阿里一面，我霸氣反問：你說你們在做 Agent 專案，說說 LangChain、Multi-Agent、A2A 這些你們都是怎麼做的？面試官一直在擦汗。。

```
老王桌上放了一瓶農夫山泉，旁邊還放了一瓶怡寶。
```

面試開始前他擰開農夫山泉喝了一口，又擰開怡寶喝了一口，然後對我說：「你知道我為什麼同時喝兩瓶水嗎？」

我一臉矇逼。

老王笑了：「因為我們部門在做 Agent，所以什麼事情都習慣並行處理。」

沒繃住，真沒繃住，第一次見這麼逗比的面試官。😄

行吧，這開場白我給滿分。

老王看了一眼我的履歷：「PaiAgent、LangGraph4j + Spring AI、工作流編排……還有個 RAG 知識庫專案。看起來是認真搞過 Agent 的。」

[![](https://i.imgur.com/eEh5dJq.jpeg)](https://i.imgur.com/eEh5dJq.jpeg)

我說：「王哥，那必須啊，我可是提前調研過咱們你這個職位的，這次必須成功，不許失敗。」

- 問 langchain 架構
- agent 開發方式有幾種
- langchain 有哪些核心的模組
- 介紹你專案中 muti-agent 的情況
- 講一下 agent 之間的通訊協議
- 用 mcp 做過什麼
- transformer 了解嗎
- 自注意力機制是什麼
- 現在市面很多大模型，對彼此的對比了解嗎，比如說生成音訊的哪個最好之類的
- 編輯器用的哪個
- 對模型內部演算法了解嗎

> 繫好安全帶，我們出發～全文比較肝，可以收藏起來慢慢看（背）

## content

### 01、聊聊 LangChain 的架構

老王直接開始：「先聊個基礎的，LangChain 架構你了解嗎？整體是怎麼設計的？」

整體可以分為三層。

[![](https://i.imgur.com/IOrmSKz.jpeg)](https://i.imgur.com/IOrmSKz.jpeg)

最底下是基礎抽象層，定義了 LLM、ChatModel、Prompt、OutputParser 這些核心介面。所有上層功能都圍繞這些介面展開，換模型只要換實作類就行。

中間是能力層，包括 Chain（鏈式呼叫）、Agent（自主決策）、Memory（對話記憶）、Retriever（檢索器）這些模組。Chain 負責把多個步驟串起來，Agent 負責根據目標自主選擇工具，Memory 負責維護上下文。

最上面是應用層，LangServe 做部署，LangSmith 做除錯和監控，LangGraph 做複雜的狀態圖編排。

老王不住地點頭，看起來對我這個回答很滿意：「 **那 LangChain 和 LangGraph 什麼關係？** 」

我說：「LangChain 的 Chain 是線性的，A→B→C 一條路走到黑。但真實的 Agent 場景經常需要條件分支、循環、平行執行，Chain 搞不定。」

「LangGraph 可以解決這個問題。它把工作流從鏈式升級成了圖式——節點處理步驟，邊可以帶條件，還支援循環。我們專案裡用的 LangGraph4j 就是它的 Java 版本，底層是 StateGraph，用狀態驅動整個執行流程。」

[![](https://i.imgur.com/6sFUrJr.jpeg)](https://i.imgur.com/6sFUrJr.jpeg)

老王追問：「你們為什麼選 LangGraph4j 而不是直接用 Python 版？」

我說：「因為我們整個技術棧是 Java 的，Spring Boot + Spring AI。Java 生態裡做 Agent 編排的選擇不多，LangGraph4j 是目前最成熟的。而且它和 Spring AI 配合得不錯，ChatModel 可以直接復用。」

### 02、Agent 開發方式有幾種

老王抿了一口農夫山泉，繼續追問：「不侷限於 LangChain，Agent 開發方式你知道哪幾種？」

「四種，我一個個說。」

[![](https://i.imgur.com/rnkkJBa.jpeg)](https://i.imgur.com/rnkkJBa.jpeg)

第一種是 ReAct 模式。模型接到任務後，交替做推理（Reasoning）和執行（Acting）。先想清楚這一步該幹嘛，再調具體的工具去執行，拿回結果再想下一步，周而復始，直到任務完成。LangChain 的 Agent 就是這個模式。

第二種是 Plan-and-Execute。先讓模型一口氣規劃出整個執行計畫，然後按計畫逐步執行。好處是不容易走彎路。壞處是計畫一旦定了就不太好調整。

第三種是 Multi-Agent 協作。多個 Agent 各司其職，一個負責寫程式碼，一個負責審查，一個負責測試。它們之間透過訊息傳遞協調。AutoGen、CrewAI 都是這個思路。

第四種就是狀態圖編排，也就是 LangGraph 這種。開發者把工作流畫成一張圖，定義好節點、邊和條件分支，Agent 在圖上按路徑執行。

老王靠到椅背上：「你們專案選了第四種？」

「對，因為我們的場景不是簡單的問答，是多步工作流——使用者在前端拖拽節點連線，後端按圖執行。我們需要更可控的執行路徑。」

### 03、LangChain 有哪些核心模組

（內心 OS：幸好之前做足了功課，不然上來這兩道題就直接貴了～～～）

老王聽得特別認真，也沒有打斷我，真的很友好啊。

等我回答完，他繼續問：「LangChain 的核心模組，具體展開說說？」

一共六個。

[![](https://i.imgur.com/sIPk8br.jpeg)](https://i.imgur.com/sIPk8br.jpeg)

**Models** ：對各家大模型的統一封裝。不管是 OpenAI、DeepSeek 還是通義千問，上層呼叫的介面都是 `ChatModel.call()` 。我們專案裡雖然沒直接用 LangChain，但 Spring AI 的 ChatModel 思路是一樣的。

**Prompts** ：提示詞管理。支援範本變數、Few-Shot 範例、動態拼裝。我們 PaiAgent 裡的 PromptTemplateService 就幹的是這個活，支援雙括號 `{{variable}}` 語法做變數替換。

**Indexes** ：文件索引，主要用於 RAG。包括文件載入器（DocumentLoader）、文字分割器（TextSplitter）、向量儲存（VectorStore）、檢索器（Retriever）。我們的派聰明專案裡用的是 Elasticsearch + 阿里 Embedding 做的混合檢索，BM25 關鍵詞 + KNN 向量召回。

**Memory** ：記憶管理。短期記憶用 BufferMemory 直接存最近幾輪對話，長期記憶可以接 VectorStore 做語義檢索。

**Chains** ：把多個步驟串在一起。最常用的 LLMChain 就是 Prompt + Model + OutputParser 三步串聯。

**Agents** ：自主決策模組。模型根據當前目標自己選擇呼叫哪個工具、傳什麼參數。這是 LangChain 區別於普通 LLM 呼叫最核心的模組。

[![](https://i.imgur.com/dfVVgla.jpeg)](https://i.imgur.com/dfVVgla.jpeg)

老王面露悅色，看起來對我的回答很認可。

於是繼續問：「這六個模組，你們專案裡用到了幾個？」

「除了 Chains，全用了。」我頓了一下，「不過用的不是 LangChain 的實作，是 Spring AI + 自研。Chains 沒用是因為我們直接上了 LangGraph4j 的 StateGraph，比 Chain 靈活。」

### 04、介紹你專案中 Multi-Agent 的情況

老王明顯對 Agent 這方面有深入的研究，於是問到了多智能體：「你專案裡有 Multi-Agent 嗎？」

我羞澀地笑了一下：「王哥，我跟你說實話——嚴格來講，PaiAgent 不是傳統意義上的 Multi-Agent 系統。」

傳統 Multi-Agent 是多個獨立的 Agent 各自有自己的目標和記憶，透過訊息傳遞協作，比如 AutoGen 裡一個 Coder Agent 寫程式碼，一個 Critic Agent 審程式碼，它們之間有來有回。

PaiAgent 做的是 **工作流編排** 。多個節點——LLM 節點、TTS 節點、Input/Output 節點——透過有向圖連起來，按拓撲順序執行。每個節點不是獨立的 Agent，而是工作流裡的一個處理環節。

但我們有一個設計是和 Multi-Agent 思路相通的： **EngineSelector 雙引擎路由** 。

[![](https://i.imgur.com/jepUjAp.jpeg)](https://i.imgur.com/jepUjAp.jpeg)

簡單的線性工作流走 DAG 引擎（拓撲排序 + DFS 循環檢測），有條件分支和循環的複雜工作流走 LangGraph 引擎。兩個引擎共用同一套 NodeExecutor 執行器，透過 NodeAdapter 適配。

[![](https://i.imgur.com/hspTpTS.jpeg)](https://i.imgur.com/hspTpTS.jpeg)

老王眼睛一亮：「你很坦誠啊。那如果讓你設計一個真正的 Multi-Agent 系統，你打算怎麼做？」

「讓每個 Agent 有自己獨立的 StateGraph，Agent 之間透過一個訊息總線通信。」我越說越自信，「每個 Agent 訂閱自己關心的訊息類型，處理完把結果發布到總線上。低耦合，新增一個 Agent 不影響其他的。」

看老王狀態很不錯，我借機反問了一句：「王哥，你們阿里內部做 Agent 專案，Multi-Agent 是怎麼架構的？各個 Agent 之間的狀態同步怎麼處理？」

老王愣了一下，擦了擦額頭上的汗：「呃……我們組目前還是以單 Agent + 工具呼叫為主，Multi-Agent 還在探索階段。」

（內心 OS：嘿嘿嘿，老王，被我拿捏了吧，🤣。）

### 05、講一下 Agent 之間的通訊協議

老王咳了一聲，趕緊把話題拉回來：「那 Agent 之間的通訊協議你了解哪些？」

目前主流的就是 Google 提出的 **A2A 協議** 。

[![](https://i.imgur.com/ppHDF12.jpeg)](https://i.imgur.com/ppHDF12.jpeg)

它的核心思路是給每個 Agent 一個「能力名片」（Agent Card），用 JSON 描述這個 Agent 能幹嘛、接受什麼輸入、返回什麼輸出。Agent 之間透過標準的 HTTP API 通訊，用 Task 作為協作單元，支援同步和非同步兩種模式。

A2A 解決的是跨團隊、跨組織的 Agent 互操作問題。比如一個電商平台的訂單 Agent 和物流公司的配送 Agent 要協作，它們可能用不同的框架、不同的模型，但只要都遵循 A2A 協議就能通信。

#### MCP 和 A2A 有什麼區別？

MCP 解決的問題不太一樣，它主要是讓 Agent 能呼叫外部工具和服務。

MCP Server 把自己的能力透過 JSON Schema 暴露出去，Agent 透過 MCP Client 發現和呼叫這些能力。

兩者的區別： **A2A 是 Agent 對 Agent，MCP 是 Agent 對工具** 。一個解決 Agent 協作，一個解決 Agent 能力擴展。

[![](https://i.imgur.com/gpwKKm4.jpeg)](https://i.imgur.com/gpwKKm4.jpeg)

老王歪了歪頭：「那實際專案中你用過哪個？」

「A2A 我們沒直接用。但 MCP 我們在派聰明裡實現過——封裝了本地檔案操作、PDF 生成和資料庫查詢三個 Server。」

然後我又忍不住問了一句：「王哥，你們內部的 Agent 之間通信用的是什麼協議？自研的還是開源的？」

老王又擦了一下汗：「我們……用的是內部的 RPC 框架，也在看 A2A。」

### 06、用 MCP 做過什麼

（內心 OS：前面五道題下來，感覺王哥對我的專案挺感興趣的，節奏不錯。）

老王擰開那瓶怡寶又喝了一口：「MCP 你具體做了什麼？」

「派聰明 RAG 專案裡，我們封裝了三個 MCP Server。」

[![](https://i.imgur.com/bBBRvL5.jpeg)](https://i.imgur.com/bBBRvL5.jpeg)

第一個是 **檔案操作 Server** 。把本地檔案的讀寫、目錄遍歷這些能力封裝成 MCP 工具。Agent 需要讀取使用者上傳的文件時，就透過 MCP 呼叫而不是直接操作檔案系統。

第二個是 **PDF 生成 Server** 。Agent 把分析結果生成 PDF 報告，呼叫 MCP 工具傳入內容和範本，Server 端用 iText 渲染成 PDF 存到 MinIO。

第三個是 **資料庫查詢 Server** 。Agent 需要查業務資料時，透過 MCP 發起 SQL 查詢。Server 端我們還做了 SQL 注入檢測和查詢逾時限制。

MCP 的好處是把工具能力標準化了。

Agent 不需要知道 PDF 是用 iText 還是 wkhtmltopdf 生成的，它只需要知道 MCP 的工具描述，填參數呼叫就行。換了底層實作，Agent 那邊一行程式碼不用改。

老王往前湊了湊：「MCP Server 的註冊和發現你們怎麼做的？」

「目前是設定檔靜態註冊，MCP 設定裡寫好每個 Server 的位址和埠號。」不能慌，不能慌，這個時候我主打的就是胡扯，哦不，自信，「沒做動態發現，Server 就三個，靜態設定夠用了。後續多了的話，可以接註冊中心，每個 Server 啟動時上報 Agent Card，Agent 從註冊中心拉取可用工具列表。」

### 07、Transformer 了解嗎

老王突然畫風一轉，問：「Transformer 架構了解嗎？」

「那必須了解啊。」我坐直了一點，「2017 年 Google 那篇《Attention Is All You Need》，現在基本上所有大模型都是基於它或者它的變體。」

[![](https://i.imgur.com/E5oQkpr.jpeg)](https://i.imgur.com/E5oQkpr.jpeg)

整體結構分 Encoder 和 Decoder 兩部分。

Encoder 負責理解輸入，把文字編碼成一組向量表示。Decoder 負責生成輸出，一個 token 一個 token 地往外輸出。

Transformer 之前大家用的是 RNN 和 LSTM，最大的痛點是處理長序列時資訊會衰減，而且沒法平行——必須一個詞一個詞地順序處理。

Transformer 用 Self-Attention 機制優化了循環結構，每個位置都能直接「看到」序列裡所有其他位置，而且可以平行計算。這是它能處理幾萬甚至幾十萬 token 上下文的基礎。

[![](https://i.imgur.com/M24gx0Q.jpeg)](https://i.imgur.com/M24gx0Q.jpeg)

老王對這方面還挺感興趣，繼續問：「Transformer 裡除了 Attention，還有哪些關鍵元件？」

「三個。」

**位置編碼（Positional Encoding）** ：Attention 本身不知道詞的前後順序，位置編碼給每個 token 加上位置信息。原始論文用的是正弦餘弦函數，現在很多模型改成了可學習的位置編碼，或者 RoPE（旋轉位置編碼）。

**Layer Normalization** ：每個子層後面都接一個 LayerNorm，穩定訓練過程。

**Feed-Forward Network** ：每個 Attention 層後面接一個兩層的全連接網路，做非線性變換。實際上模型的大部分參數都集中在這裡，Attention 層的參數占比反而不算大。

### 08、自注意力機制是什麼

老王沒給我喘息的機會，緊跟著追：「Self-Attention 具體怎麼算的？」

（內心 OS：這是要把 Transformer 從頭到腳給我扒一遍啊。）

「核心就三個矩陣：Query、Key、Value。」

[![](https://i.imgur.com/JZPcmwA.jpeg)](https://i.imgur.com/JZPcmwA.jpeg)

輸入序列經過三個不同的線性變換，得到 Q、K、V 三組向量。然後用 Q 和 K 做點積算相似度，除以 √d（d 是向量維度，防止數值太大），過一個 Softmax 得到注意力權重，最後用權重對 V 加權求和。

```
scss 代碼解讀複製代碼Attention(Q, K, V) = softmax(Q·K^T / √d) · V
```

打個比方，我們在讀一篇文章《母豬為什麼會上樹？》。讀到「它」這個字的時候，我們的大腦會自動去找這個「它」指代的是什麼——那頭母豬。Self-Attention 做的就是這件事：每個 token 去「看」序列裡所有其他 token，算出跟誰關係最近，然後把相關資訊聚合過來。

[![](https://i.imgur.com/Vu8acqf.jpeg)](https://i.imgur.com/Vu8acqf.jpeg)

老王點了點頭，接著挖：「Multi-Head Attention 又是怎麼回事？」

[![](https://i.imgur.com/agDFiGN.jpeg)](https://i.imgur.com/agDFiGN.jpeg)

「一個 Head 只能捕捉一種關聯模式。」我用手比了兩個方向，「比如一個 Head 關注語法關係——主謂賓，另一個 Head 關注語義關係——同義替代。」

Multi-Head 就是平行跑多組 Attention，每組用不同的 Q、K、V 投影矩陣，最後把所有 Head 的輸出拼接起來。

這樣模型就能同時從多個維度理解 token 之間的關係，比單個 Attention 表達能力強得多。

### 09、大模型對比

老王伸了個懶腰，換了個輕鬆的坐姿：「現在大模型這麼多，你對它們的對比有什麼了解？比如音訊生成哪家最好？」

「這個我還真研究過。」我來了精神，「PaiAgent 裡接了好幾家模型，踩坑踩出來的經驗。」

[![](https://i.imgur.com/DeY0qWD.jpeg)](https://i.imgur.com/DeY0qWD.jpeg)

先說文字生成。

目前綜合最強的還是 Claude 系列，國產裡 DeepSeek V3 性價比最高。通義千問 Qwen3 在中文場景下表現不錯，尤其是長文字理解。GLM-5.1 的程式設計能力，尤其是長任務，在國產模型上還是頂級。

程式碼生成方面，Claude Opus 和 GPT-5.4 是第一梯隊。

音訊生成的話，TTS 領域我測過幾家。阿里百煉的 qwen-tts 系列音色比較自然，我們 PaiAgent 裡用的就是 qwen3-tts-flash。

圖片生成方面，Nano Banana 2 是標竿。國產裡通義萬相和即夢（字節）進步很快。

### 10、AI Coding 工具用的是哪個？

老王突然畫風一轉：「你平時開發用什麼 AI Coding 工具？」

「以前主力是 IDEA，現在主力是 Claude Code + Codex。」

「讀程式碼、改程式碼、跑測試、查日誌，主要交給 Codex，量大管飽。」我想了想補了一句，「如果需要調查方案，或者 Codex 解決不了的時候，我會上 Claude Code，目前配的是 Opus 4.6。」

不過 IDEA 也沒完全扔掉，偵錯器和程式碼導覽還是不可替代。現在兩個搭著用，Claude Code 寫程式碼，IDEA 做偵錯。

[![](https://i.imgur.com/gBMvL4w.jpeg)](https://i.imgur.com/gBMvL4w.jpeg)

### 11、對模型內部演算法了解嗎

（內心 OS：一個小時結束了，老王這體力是真好，還特麼能問，勞資快頂不住了。）

老王看了看時間，做出最後衝刺的姿態：「最後一個，對模型內部的訓練演算法了解嗎？」

「了解個大概，不算特別深入。」我實話實說。

大模型訓練大致分三個階段。

第一階段是 **預訓練（Pre-training）** 。用海量文字資料做自監督學習，目標就是預測下一個 token。這個階段消耗的算力最大，動輒幾千張 GPU 跑幾個月。模型在這個階段主要學習語言的基本結構和世界知識。

第二階段是 **SFT（Supervised Fine-Tuning）** 。用人工標註的指令-回答做有監督微調，讓模型學會按指令做事。這個階段把一個「能說話但說不到點子上」的模型，訓練成一個「能聽懂指令並給出有用回答」的助手。

第三階段是 **RLHF（Reinforcement Learning from Human Feedback）** 。先訓一個獎勵模型（Reward Model），用人類偏好資料告訴它什麼樣的回答是好的。然後用強化學習（PPO 演算法）讓主模型的回答盡量往獎勵模型打高分的方向靠。這個階段讓模型的輸出更符合人類預期。

[![](https://i.imgur.com/0WZxABo.jpeg)](https://i.imgur.com/0WZxABo.jpeg)

老王眼睛盯著我：「RLHF 和 DPO 有什麼區別？」

「RLHF 要單獨訓一個獎勵模型，再用 PPO 做強化學習，流程比較重。」我用手比了個砍的動作，「DPO 把獎勵模型這步直接砍掉了，用偏好資料對來優化策略模型，兩步併一步。訓練更簡單、更穩定，效果也不差。」

老王沉默了幾秒鐘，看起來中雨要結束了。

「好，你有什麼想問我嗎？」

我等的就是這句話。

「王哥，你說你們部門在做 Agent 專案，那我想問問——LangChain、Multi-Agent、A2A 這些方向上，你們具體是怎麼做的？」

老王差點沒把水噴出來，又開始擦汗：「我們……嗯……用的主要是。。。。。。Multi-Agent 還在 POC 階段，A2A 確實還沒接入……」

我笑了。

老王也笑了，把兩瓶水都擰上蓋子：「你什麼時候能來上班？」

「下週一，我回去請室友們吃個飯🎉」

---

原文出處： [https://juejin.cn/post/7657453685375369267](https://juejin.cn/post/7657453685375369267)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qvdeX3) [12瀏覽](https://codelove.tw/@tony/post/qvdeX3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qvdeX3)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝15 💬3 ❤️3

634

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬3 ❤️3

118

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783044209&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqvdeX3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783044208679&amp;bpp=2&amp;bdt=282&amp;idt=978&amp;shv=r20260701&amp;mjsv=m202606260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783044161%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783044161%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783044161%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4404747236346&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099343%2C95395662%2C95393485&amp;oid=2&amp;pvsid=6700299177854886&amp;tmod=1091985026&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=1022" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260701/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>