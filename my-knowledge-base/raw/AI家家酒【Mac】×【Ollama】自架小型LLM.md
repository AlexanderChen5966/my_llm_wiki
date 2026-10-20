---
title: "AI家家酒【Mac】×【Ollama】自架小型LLM"
source: "https://www.cc.ntu.edu.tw/chinese/epaper/home/20251220_007504.html"
author:
published:
created: 2026-06-02
description:
tags:
  - "clippings"
status: processed
---
### 前言

大家好，我是富田町47番地的社畜工程師柚子哥，很高興可以跟大家分享自己的心得，自從我開始玩n8n自動化流程搭配AI後，為了讓AI精確鎖定我心裡的想法上下文工程(Context Engineering)與提示詞( prompt )也開始越寫越長，我課金的AI服務實在燒了不少錢，在物價高漲的時代，一個月三萬多元的微薄薪水只能斤斤計較找出比較划算的方案，於是尋思自己架設小型的LLM，現在有很多開源的AI模型可以讓我們使用，不如把一些簡單低複雜度的任務就交給自架的LLM去處理就好，比較複雜的任務就交給課金的AI服務去處理。

### 一、我的自架LLM硬體選擇

我選擇硬體時想了不少方案，買傳統主機搭專業顯卡太貴買不下手，於是我看上了蘋果公司推出的Mac mini M4 16G Ram，蘋果的Unified Memory架構讓CPU和GPU共享同一塊記憶體，所以可以跑小型LLM模型，以教育版價格去買的話只需要1萬6千4百元，算是在我薪水可以支付得出來的範圍，而且蘋果ARM架構的晶片耗電量比較低，可以節省一點電費，未來退役之後就拿來當NAS跟軟路由都是不錯的選擇，還可以順便學習IOS APP的開發，拓展職涯之路，於是我就直接下訂單了。

![20251220_007504_01](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/30ddf3f9-2dbc-41ac-b187-b2bbf2d94d9e.png)

圖1. 蘋果官網教育價格。

入手後還是滿興奮的，因為是我第一次使用mac系統，操作體驗、軟體安裝、鍵盤快捷鍵都不熟，需要重新學習，但喜歡學習新東西、學習能力強是我的優點，所以花了一個禮拜的摸索期大致上也熟悉操作方法。

![20251220_007504_02](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/a687599f-ff50-4006-b615-6afaf6171e85.jpg)

圖2. 入手後的蘋果硬體

### 二、安裝Ollama

Ollama是一個輕量級的本地大型語言模型（LLM）運行框架，有簡易的對話式使用介面，也可以透過簡單的命令列或API，就能下載並執行各種熱門開源模型，安裝簡單、佔用資源較低。

支援透過HTTP API與其他應用程式整合，非常適合開發者將AI功能嵌入桌面或伺服器應用中。與課金的AI相比，Ollama不需要網路即可執行，能確保隱私與資料安全，滿適合企業內部使用或需要離線環境的人。

首先我們到Ollama的官網首頁https://ollama.com，就會有明確的下載圖示，點擊後下載並安裝。

![20251220_007504_03](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/cf6ab162-b5a4-4d40-be98-874228e02d3b.png)

圖3. Ollama官網

![20251220_007504_04](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/b7e12dee-a906-4304-9aca-d77c5fa264f2.png)

圖4. Ollama下載頁面

![20251220_007504_05](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/3beab5a6-c739-48a7-b3ba-a4f68ae3b8f2.png)

圖5. 下載

軟體安裝只要把.app拖到Applications資料夾即可使用。

![20251220_007504_06](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/82f1f5ac-ea8e-48b2-a5ca-afbee27904a1.png)

圖6. 安裝Ollama

### 三、刪除Ollama方式

若要從系統中完全移除Ollama的話，官方github也有提供指令。

[https://github.com/ollama/ollama/blob/main/docs/macos.md](https://github.com/ollama/ollama/blob/main/docs/macos.md "Js")

sudo rm -rf /Applications/Ollama.app

sudo rm /usr/local/bin/ollama

rm -rf "~/Library/Application Support/Ollama"

rm -rf "~/Library/Saved Application State/com.electron.ollama.savedState"

rm -rf ~/Library/Caches/com.electron.ollama/

rm -rf ~/Library/Caches/ollama

rm -rf ~/Library/WebKit/com.electron.ollama

rm -rf ~/.ollama

![20251220_007504_07](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/ae3f43da-00c4-474e-bc64-26ca4fd18245.png)

圖7. 刪除方式

### 四、查詢可用的公開模型

點擊官網左上角的Models可以觀看可以快速使用的模型清單。

清單上有很多開源模型可以使用，包含Meta的llama系列、Google的Gemma系列、中國的deepseel系列跟qwen系列，甚至現在連OpenAI也加入了開源模型戰場，推出了gpt-oss 20b跟120b模型(寫稿日2025/09/13)。

![20251220_007504_08](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/8a731e13-a0f2-421d-af1e-bd8328d084b8.png)

圖8. 查看語言模型

### 五、模型標籤

而每個模型底下都有數個標籤，每個標籤代表什麼意思，這邊簡單跟大家說明。

![20251220_007504_09](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/f19a9202-7e59-4ccd-ab85-3c64b351c1b5.png)

圖9. 標籤

**數字「b****」：**淡藍色的標籤，含有數字加上「b」，「b」代表十億，是模型的參數量，gpt-oss 20b就代表有200億個參數，數字越大通常對於問題的回答能力也越好，但相對的也更吃硬體的性能，以我目前硬體(mac mini m4、16G ram)的性能大致上可以跑得動20b的模型，但是很慢，運行14b以下的模型，速度都還可以接受。

**Turbo****：**Turbo標籤代表，該模型支援使用ollam的資料中心級硬體運行，加速模型推理，例如我的硬體去跑gpt-oss 20b很慢，就可以用Turbo去加速推理，但必須登入ollama的帳號並且課金就可以使用，目前這功能是Preview版本。

![20251220_007504_10](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/99d7ac83-b364-47b1-9ad4-4b0c8f1cee7d.png)

圖10. Turbo

**Vision****：**Vision標籤代表該模型有視覺能力，能夠處理和理解圖像或視覺輸入（例如圖片、圖表）與文字之間的關係。以官網部落格中使用Llama 3.2 Vision的範例可以得知，可以識別的範例中可以辨識手寫文字、光學字元判斷、讀取圖表、圖片問答等等。後續我也會在實作中示範看看圖辨識。

[https://ollama.com/blog/llama3.2-vision](https://ollama.com/blog/llama3.2-vision "Js")

![20251220_007504_11](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/36090fcd-cb64-403b-8be6-8e16d002e3fb.png)

圖11. Vision

**Thinking****：**Thinking標籤代表該語言模型支援思考模式，會把我們提出的問題用不同角度去回答，得出更精確的答案，因此思考模式也會花更多時間去推理問題，如果模型有Thinking標籤則思考模式預設是開啟，如果只是想快速得到答案可以關掉，在Ollama CLI下--think=false指令可以禁用思考模式；開啟思考模式則是用--think。

[https://ollama.com/blog/thinking](https://ollama.com/blog/thinking "Js")

![20251220_007504_12](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/26223b74-73e3-4c03-a92d-49753ee83c22.png)

圖12. Vision

**Tools****：**Tools標籤代表該模型支援調度使用外部工具來協作的能力，例如先寫好簡單的計算機、網頁搜尋工具，在對話中可以叫AI應用我們寫好的Python工具來達成任務。

但Tools跟Agent是不一樣的概念，我一開始也有點混淆，後來才搞清楚，差異整理如下。

|   |   |   |
|---|---|---|
|概念|定義與功能|兩者之間的關係|
|Tools（工具/函式呼叫）|1. 識別它需要使用外部資源。<br><br>2. 生成呼叫外部函式（如 API、計算器、網路搜尋）所需的 JSON 參數。<br><br>它提供執行動作和獲取即時資訊的能力。|是 Agent 運作的基礎。沒有工具，Agent 只能基於訓練數據回答。|
|Agent（代理）|1. 感知（Perception）： 接收使用者輸入或環境資訊<br><br>2. 推理（Reasoning）： 思考、制定計劃、決定要使用哪個工具。<br><br>3. 行動（Action）： 呼叫 Tools 執行步驟。<br><br>4. 記憶（Memory）： 維持會話上下文和過往經驗。|更高層次的結構，它利用 Tools 作為其行動能力，並透過「思考」來指導工具的使用。|

表1. Tools跟Agent差異

官方github也有提供工具的範例，是一個加法器。大家可以參考看看。

[https://github.com/ollama/ollama-python/blob/main/examples/tools.py](https://github.com/ollama/ollama-python/blob/main/examples/tools.py "Js")

![20251220_007504_13](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/822f94a5-bf44-420b-9de5-58431326c163.png)

圖13. Tools

**Embedding****：**Embedding標籤就代表這模型支持嵌入模型，這功能不是用來直接生成回答文字，而是可以用來把我們整理好的文字轉換成向量數值，以便後續進行語義相似度搜索、分類的工作，RAG（檢索增強生成）之用。這篇是初級篇，所以不深講，下次有機會再寫一篇RAG的範例。官方的部落格也有相關範例可以先看看。[https://ollama.com/blog/embedding-models](https://ollama.com/blog/embedding-models "Js")

![20251220_007504_14](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/ed227e71-fa11-470b-97b6-739693639277.png)

圖14. Embedding

### 六、開始使用AI

講完標籤後大家應該會於選擇要使用什麼模型上有一點概念，接下來我們就要開始實際使用AI了，如果你是一般人的話可以直接使用ollama的對話視窗來提問，再進階一點還自己載入自己的LLM模型，修改Model File文件並調整自己喜歡的參數等等，後續有機會再寫一篇跟大家分享。官方網站相關網址：[https://github.com/ollama/ollama/blob/main/docs/modelfile.md](https://github.com/ollama/ollama/blob/main/docs/modelfile.md "Js")。這篇先把基本的兩種使用ai方式介紹給大家。

第一種是CLI模式，我們到蘋果官網去瀏覽所有模型的時候都可以看到圖15標示的位置都有個類似ollama run的指令，請先複製下來。

![20251220_007504_15](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/ae579a44-5065-45f4-9710-4c08dcd80031.png)

圖15. 官網模型指令

再來我們開啟蘋果的終端機。

![20251220_007504_16](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/c6b1ffd7-dcd2-48a2-9684-0bd63317412c.png)

圖16. 蘋果終端機

把剛剛複製好的指令貼上，並按下enter執行。

![20251220_007504_17](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/5a3a90a5-7a2d-495c-b1e9-7db53e0ff4d3.png)

圖17. 執行指令

第一次執行指令會比較慢，因為會看我們有沒有下載這個模型，如果電腦主機裡面沒有這個模型，沒有的話他們會去自動下載模型。等一段時間下載完畢後他就會開啟對話模式。

![20251220_007504_18](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/569fb5f3-c216-47ad-9b80-65b5f9a69712.png)

圖18. 下載並開啟對話

這時候我們就可以在這邊輸入問題，我就問他是什麼AI模型，AI也回答了我的問題。

![20251220_007504_19](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/fb153006-8aab-4cbd-b60b-3f6cd3a9cc07.png)

圖19. CLI回答問題

第二種是UI模式，一般普通使用者一定不會用CLI模式，無疑是讓自己更累，所以ollama有UI模式，讓大家可以更輕鬆簡單的使用AI。

首先看蘋果電腦的右上角，有個可愛的羊駝符號點右鍵，就會出現選單。第一個選單是開始UI對話介面，第二個選單是開啟簡單的設定。

![20251220_007504_20](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/6e7806c5-1cb6-42ec-a40d-c58afa4fcb62.png)

圖20. 羊駝符號

我們點開open ollama，出現對話框UI。UI介面左上角有個New Chat可以開啟新的對話，使用起來就跟我們常見的AI服務一樣，右下角可以選擇要使用的模型，選好模型之後第一次提問他就會檢查是否有該模型，沒有的話一樣要花時間下載，下載過之後就直接開新對話就可以直接使用。

![20251220_007504_21](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/4e90a0ca-a36e-41e9-a4ba-bc39dac832f9.png)

圖21. UI介面

![20251220_007504_22](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/94d225a9-0b34-4bed-b4b8-5c46e022225f.png)

圖22. 語言模型選擇

我這邊示範一下不同語言模型所產生的答案差異，我第一個使用lama3.2:3b的模型，提問「你是擅長寫windows的power shell，可以請你幫我寫一個小程式，搜尋D磁碟機內的所有.docx檔案，並複製到E磁碟機的mydocx資料夾裡面」，看看AI給的答案。

![20251220_007504_23](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/7f817dae-67f2-47bd-83ee-cc2e27f9373f.png)

圖23. 對lama3.2:3b提問

![20251220_007504_24](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/e7ed8ba7-68ed-4909-a972-ebb4bdfe05bc.png)

圖24. lama3.2:3b回答

接下來，我用gpt-oss:20b去問同樣的問題，因為會思考所以明顯花費時間很長，花了64.1秒，但得到的答案很詳細。

![20251220_007504_25](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/f973ede7-cb1e-4c9a-8087-8c6eb5eb0a5a.png)

圖25. 對gpt-oss:20b提問

![20251220_007504_26](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/055135bf-d7da-4593-b732-b97e7d0d3564.png)

圖26. gpt-oss:20b回答

![20251220_007504_27](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/9eb2f17b-6442-4516-8320-f056875144fb.png)

圖27. gpt-oss:20b回答

接下來我用gemma3:12b模型測試他識別圖像的能力，我從AI那邊先產一張沙漠中的熊貓圖，再問AI這張有什麼，AI也確實解讀的圖片中的元素。

![20251220_007504_28](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/4ef1a9d1-d13d-4f27-aee8-9bb50fa1eb3b.png)

圖28. 要辨識的圖

![20251220_007504_29](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/d98633ad-ea2a-4ad2-bd7b-4f9e33b7267a.png)

圖29. 詢問AI

![20251220_007504_30](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/5bc87384-c74b-4822-9983-5804e4c3de96.png)

圖30. AI的回答

再來我們看看UI介面的settings裡面的設定。

**Ollama account**：登入Ollama帳號，就可以使用他們課金的功能例如網頁搜尋，推理加速。

**Expose Ollama to the network**：將 Ollama 暴露給網路，您的區域網路 (local network) 上的其他電腦、平板或應用程式就能夠連線到並使用您這台電腦上運行的 Ollama 服務。一般安全性考量是關閉，因為如果你的電腦有被駭客入侵的風險，他們也可能會利用這個服務，所以通常會用更安全的方式，例如ngrok 或 Tailscale 這類工具來建立加密連線，或是透過 Nginx 進行反向代理讓ollama串接到其他服務上。

**Model location**：模型儲存位置，想更改位置到其他地方可以在這邊更改。

**Context length**：上下文長度決定了本地 LLM 在產生回覆時，可以記住並使用的對話量，範圍從 4k (4,000) 到 128k (128,000) 個 Token，長度越長，模型能記住的對話歷史就會越多，回覆也會更連貫和貼合情境，但同時也會消耗更多的電腦記憶體 (RAM)，且處理速度會變慢，大家再自己慢慢微調到適合自己電腦的Context length。

**Airplane mode**：開啟後它會切斷 Ollama 的網路連線，禁用 Turbo 模式和網路搜尋。

![20251220_007504_31](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/7b0bf4f8-2007-4ef2-8380-ee525523f917.png)

圖31. Settings

七、Ollama REST API

最後我簡單說一下Ollama REST API，如果我有其他的系統想串AI的話Ollama也提供REST API，我這邊就簡單用Postman這款開源軟體測試API，Postman也是很值得開一篇文章介紹，Postman不過這邊先略過，我直接安裝好並開始使用。

![20251220_007504_32](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/f3b59309-0609-40b3-a012-535d25b2dd8d.png)

圖32. Postman

Postman有整套寫好的Ollama REST API測試，我們可以搜尋ollama，並新增到workspace，只需要修改參數就可以馬上測試API通不通，這邊我講幾個簡單的參數給大家參考。

![20251220_007504_33](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/fddeaf6e-a487-4978-b6c3-9b8a3b28d79e.png)

圖33. 搜尋ollama

![20251220_007504_34](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/f091a9b0-ebe7-48e9-8a63-ebb4fdec0502.png)

圖34. Ollama REST API

先開啟一個chat completion的測試頁，點開overview的設定，裡面會有model，這就是設定我們要使用的語言模形式哪一個。

![20251220_007504_35](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/99919add-3b97-4154-a93c-07a6a0ac1921.png)

圖35. 修改模型

第二個大家會想改參數是stream，他位於body裡面，stream就是AI的文字返回方式，如果大家想看的返回文字是像人一樣一個字一個字回覆，stream要設定true，但我們再串AI時想等AI全部生成好整串文字再一口氣回覆給我，我才可以再串到Agent裡面的話，stream要設定false。然後按下【Send】下面就會出現Ai產出的答案。

![20251220_007504_36](https://www.cc.ntu.edu.tw/chinese/epaper/home/001/Upload/354/ckfile/0909d9d3-6756-4083-9169-62381c2d6077.png)

圖36. stream

### 八、結語

我這邊簡單的AI家家酒就介紹到這邊，以後有機會在繼續跟大家深入介紹進階的功能，總結來說，Ollama在Mac mini M4上的體驗相當優秀。它不僅安裝簡單、操作直觀，更能充分發揮Apple Silicon架構的硬體優勢，帶來流暢的推理效能。對於需要在本地進行AI開發、保障隱私或快速建立原型的人來說，這是一個強大而高效的解決方案。雖然仍無法完全取代雲端的龐大算力，但作為日常實驗與開發工具，Ollama已經足以滿足大多數需求。

### 參考資料

- Apple官網：[https://www.apple.com/tw/](https://www.apple.com/tw/ "Js")
- Ollama：[https://ollama.com/](https://ollama.com/ "Js")
- Ollama github：[https://github.com/ollama/ollama](https://github.com/ollama/ollama "Js")