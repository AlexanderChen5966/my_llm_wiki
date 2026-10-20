---
title: "Flutter 開發怎麼做 Agent？從工程實戰詳細給你解讀下"
source: "https://codelove.tw/@tony/post/3QKlEq"
author:
published:
created: 2026-07-09
description: "Flutter 開發怎麼做 Agent？從工程實戰詳細給你解讀下====================================    之前發 Angent 文章的時候，有人提過幾次說有沒有工程實戰的，這次就乾脆透過 Flutter 的 Agent 實現來聊一聊實戰實現。> 全文還..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月08日](https://codelove.tw/@tony/post/3QKlEq "2026-07-08 15:15:12")


## Flutter 開發怎麼做 Agent？從工程實戰詳細給你解讀下

```
之前發 Angent 文章的時候，有人提過幾次說有沒有工程實戰的，這次就乾脆透過 Flutter 的 Agent 實現來聊一聊實戰實現。
```

> 全文還是很長。

Flutter 開發做 Agent 幾乎繞不開 GenKit， **因為 Genkit-Dart 是 Google 官方針對 Flutter 實作的 Agent SDK** ，幾乎是唯一選擇。GenKit 的整體能力比較接近 LangChain，核心都是幫助應用開發把 *模型、Prompt、工具、RAG、結構化輸出、觀測、評測、部署* 串起來，當然能力相對 LangChain 的進階 LangGraph 全家桶肯定弱了不少。

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/f742576d7db341ef923bd9e602283ed9~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=PCxouisQ3838mp%2BJRKCpgaQrm8A%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/f742576d7db341ef923bd9e602283ed9~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=PCxouisQ3838mp%2BJRKCpgaQrm8A%3D)

不過 **GenKit 的核心能力頁不是 Chain，在 GenKit 裡一切都是圍繞 Flow 展開** ，Flow 的定位主要是：

> **把一段 AI 業務邏輯包裝成可型別校驗、除錯、追蹤、部署、評測的函式** 。

大概類似這樣，這個就是我自己的 GenKit Agent 專案裡單次 turn 的資料流圖，也是今天我們要聊的內容：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/7dd6fd2f360f4dc39793e983ae5b3529~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=YMy8wX2NCFVkSRWFLRCRsTeCJ10%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/7dd6fd2f360f4dc39793e983ae5b3529~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=YMy8wX2NCFVkSRWFLRCRsTeCJ10%3D)

> 比如在我這裡， ***整個 Agent 專案在這一整套 Flow 裡定義了 21 個 lifecycle step 編排時序** ，同時 trace 裡會看到對應的 `stepName` ，使用者的每個提交請求都會在整個 Flow 內進行狀態流轉* 。

因為 AI workflow 在 Agent 場景不是說一次模型呼叫就完事了，現在一個普通正常的 Agent，一般來說都需要：

- 檢索上下文
- 管理會話歷史
- 格式化輸入
- 校驗輸出
- 組合多個模型回應
- ····

這都還只是基礎，還沒有各種細節和工程管理， **而 GenKit 的 Flow 就是提供工程上的 *型別安全輸入輸出、串流輸出、Developer UI 除錯* 的能力** 。

更具體地說，GenKit 的關鍵能力可以分幾個層面：

- **模型統一呼叫** ：透過 GenKit 的 provider/plugin 體系統一呼叫，類似 LangChain 的 model abstraction，專案裡可以透過一份 `ai.generateStream(...)` 同時掛本地 gemma、LiteRT-LM 或者遠端 LLM 相容 endpoint 協議，切換只是換一個 model resolver
- **結構化輸出和型別安全** ：GenKit 很強調 schema，類似 JS/TS 裡用 Zod 定義 inputSchema、outputSchema，整個 Flow 不只是約束 LLM 輸出，同時也是在規範 Flow 變成一個穩定的可驗證 API， **實際上這個點對生產系統很重要，因為如果你開發過 Agent，就會發現真實業務裡最怕模型輸出 JSON 不穩定** ，比如 GenKit 專案裡，所有跨邊界的資料結構（TraceReport、ToolResultEnvelope、ClaimExtraction、QueryRewritePlan）都帶一個 `schema = "xxx.v1"` 版本號，方便未來 v2 breaking change 並存
- **工具呼叫** ：GenKit 支援流程化和結構化的 tool calling，在 GenKit 裡， **核心是怎麼把工具呼叫安全地接進業務後端** ，也就是工具怎麼在 Flow 裡流轉
- **RAG 抽象** ：GenKit 官方 RAG 抽象包括 Indexer、Embedder、Retriever 的支援，也是幫助構建 RAG flow，比如我的 Agent 專案裡做了 5 種 retriever 模式（hashLexical / bm25 / hybrid / embedding / hybridEmbedding）和 384 維 n-gram embedding + BM25 + RRF 融合，全部落在 GenKit 的 retriever 抽象上
- **開發除錯和觀測** ：可以在 Flow 裡新增和查看一系列 trace、除錯執行鏈路、看每一步模型呼叫和工具呼叫， **trace 這個如果沒有，基本上 Agent 找 Bug 是不可能**

> 而 GenKit 的核心作用就在於提供了這樣一套規範化的工程化能力，把 *呼叫模型、取業務資料、結構化輸出、工具呼叫、RAG 檢索、日誌/trace、除錯部署* 整合一體化的 SDK。

**所以 GenKit 也可以看成是 Agent 的協議化編排系統** ，它的定位類似 LangChain，但是沒有 LangChain 那麼重，也沒有 LangGraph 那麼豐富的狀態管理和協同能力，所以也有不少東西需要我們自己實作，比如狀態機、human on loop 這些就需要我們單獨實作。

那麼回到實戰專案裡，如下表格所示，是我用 GenKit 專案實作的一個簡單 Flutter Agent 的結構，也是企業級普通 Agent 需要具備的能力場景，也就是雖然已經有了 GenKit SDK，但是你要做的也不是就調調 API 就完事，更多是如何在 GenKit 上面設計一套完整的 Agent Flow 流轉機制：

分組功能和技術點 **Agent 內核** Agent LoopTrace 包裝 + TurnExecutor11 态 FSM + GenKit Flow會話可變性受控Turn Plan（意圖 + 工具集合）ReAct / Plan-Execute Planner **Tool** 11 個內建工具聲明執行、審批、超時、重試內建 handler + MCP client宿主注入的工具工具路由投影三種 provider 私有格式歸一化MCP 白名單策略結果 schema遠端主題轉發 **RAG** BM25 / Embedding / ANN / Reranker知識庫編排三類物料 → chunk5 類 query 改寫協議歷史摘要資料事實上下文資料消息持久化DDG / SearxNG / Open-Meteo **Prompt** 分段拼裝 + 預算裁剪 **能力/權限** 能力快照技能聲明意圖路由（中立） **審計** Grounding 審計Claim 抽取與核對 **可觀測** Trace 資料結構Eval dataset動態回歸版本對比觀測器介面 **存儲** 會話主庫長期記憶附件二進位 **安全** 脫敏取消權杖上下文溢出重試 **模型** Chat 訊息 & Model 定義OpenAI 相容客戶端本地推理執行階段多模態 content partToken 估算 **GenKit 資源** GenKit runtime/tool 註冊上面的那些功能在工程裡表達出來，大概就會是如下所示的一個工程結構形態和能力依賴，如果圖沒被壓小的話，應該可以看得清楚：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/199b09f8a4f8428ba071739d00eee5e8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=qo2SMBvce6XQOo6XEMV9Ubpz4lY%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/199b09f8a4f8428ba071739d00eee5e8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=qo2SMBvce6XQOo6XEMV9Ubpz4lY%3D)

從這張圖能看出來： **所有語義決策路徑最後都需要收斂到 GenKit runtime 上** ，而 Dart 側的模組只是往裡塞 schema、tool、prompt、evidence，然後透過模型基於這些"結構化輸入"去做決策，業務都需要在 Flow 裡流轉。

## Plan

所以在工程上一個可用的企業級 Agent 需要考慮的點很多， **比如 Turn Plan，你不可能直接就把使用者的話發給大模型去執行吧** ，你需要把使用者的自然語言拆分成真實意圖，需要分成幾步執行，需要呼叫哪些工具，有什麼前後因果關係：

```
dart 代碼解讀複製代碼class AgentTurnPlan {
  final String intent;                       // 意圖（由模型/routing plan 決定）
  final Set<String> requiredToolNames;       // 本輪必須用到的工具
  final Set<String> forbiddenToolNames;      // 本輪禁止用的工具（隱私/權限）
  final Set<String> candidateToolNames;      // 本輪候選工具
  final List<String> activeSkillIds;         // 啟動的技能
  final LocalAiModelCapabilities modelCapabilities;
  // ...
}
```

> 對於模型最終輸入來看得到的是：" *這幾個工具你必須至少調一個* "、" *這些工具你不能用* "、" *這些是可以用的* "，不需要模型去猜權限或隱私是否允許。

比如在我的 Agent 實作裡， **TurnPlan 用「三集合模式」表達工具邊界（required / forbidden / candidate），模型不需要在自然語言層面猜" *我這個場景是不是不能調 device\_control* "，因為它根本看不到 device\_control 這個工具** ，實際上模型看到的是：

- **能力快照** ：宿主注入的能力，比如目前模型支不支援 tool\_call、支不支援 image input
- **技能聲明** ：一個「客服 skill」可以聲明" *只允許 knowledge\_search + web\_search + memory\_get* "
- **隱私 / 審批策略** ：某些工具需要使用者點確認才能跑
- **意圖路由** ：預設回傳 `modelToolChoice` ，也就是"由模型基於 tool schema 自己決定"

**而且這個「意圖識別」也不是直接讓大模型拆分就完事了，更專業的需要分層設計** ，比如：

- 第一層透過關鍵字和正規表示式過濾出一些業務意圖，透過業務規則層，來處理高頻、明確、無歧義的請求
- 再經過小模型進行語意拆分，處理一些常規意圖
- 最後才讓大模型負責意圖整合和兜底

> **而且每一層還需要有置信度判斷、降級策略和澄清機制等等** 。

在 TurnPlan 之上，一般還會再展開出一層 `ExecutionPlan` ，把 TurnPlan 分解成一序列 `ExecutionStep` ，每一步都帶 `goal / toolName / required / readOnly / parallelSafe / reason` ，這時候整體會有三種策略：

- `directAnswer` ：不需要工具，直答
- `react` ：ReAct 模式（思考-行動交替），也就是這個問題需要推理
- `planAndExecute` ：先規劃再執行（複雜多工具場景）

**不過 ExecutionPlan 也不是"寫死的腳本"** ，它是給模型的 hint，主要是寫進 prompt 裡，GenKit 生成過程中，模型完全可能偏離這個 plan（比如臨時決定不用某工具），程式碼也不強制。

> **真正的"必須呼叫"是透過 `forceToolUse=true` 和後置的 `_ensureRequiredToolsWereUsed()` 校驗來保證** ，這種"soft plan + hard invariant"的組合，可以折衷做到既尊重了模型的推理自由度，又保證了工程約束不被破壞。

## prompts 模板

**接著是前面流程裡的 prompts 模板** ，在這裡是把不同的 Dotprompt 模板融合到 Flow 流程裡，主要提供這些場景：

- 主 system 指令
- 工具指導 block
- Query 改寫
- 路由計劃
- Claim 抽取
- Claim 核對
- 審計失敗後重生成
- 輸出品質重寫
- 工具結果綜合
- 直答提交
- 預填工具結果
- 缺失工具補選
- 截斷續寫
- 長期記憶行
- 歷史 turn 行
- 通用 section block
- 截斷標記
- 截斷 turn 區塊

**這些 prompt 全部走 GenKit Dotprompt（YAML frontmatter + 模板體），執行時透過 `AiPromptMessages` 介面注入 locale/上下文變數，不會在 prompt 裡寫死特定語言，同時也可以根據狀態對 prompt 進行組裝** ：

[![](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/6eb9b14bf7d849e19adf0a6db978a4a3~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=RE%2F4cAcTdfHb%2B%2F%2FSjydFxIxXlYs%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/6eb9b14bf7d849e19adf0a6db978a4a3~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oGL54yrZGXlsI_pg60=:q75.awebp?rk3s=f64ab15b&x-expires=1783904013&x-signature=RE%2F4cAcTdfHb%2B%2F%2FSjydFxIxXlYs%3D)

> **所以在 Agent 專案裡 prompt 模版管理也是很重要的模組** ，比如某個 locale 在流轉裡錯亂或者丟失了，可能就會出現 prompt 模版語言選錯，然後會話突然就變成了其他語言輸出。

Dotprompt 的寫法上，絕大多數模板體只有一句 `{{systemInstruction}}` 這種極簡注入，真正的字串在 Dart 端由 `PromptMessages.systemInstruction()` 生成後傳入，這麼做的原因主要是：

- **i18n 強制解耦** ：模板本身不含任何自然語言，locale 從外面注入，避免"寫死中文"這種反模式
- **讓 Dotprompt 保持宣告式** ：YAML frontmatter 宣告 schema 契約，模板體只是"格式化容器"，避免在模板裡塞邏輯
- **利於 GenKit Developer UI 使用** ：Dotprompt 可以在 UI 裡預覽、編輯、對比歷史版本

**這裡有個細節就是「雙預算裁剪」，因為字元和 token 不是線性關係** ，比如 CJK 單字元可能占 2-3 個 token，英文單字一個 token，所以 prompt builder 裡需要兩級處理，先跑一個字元預算做廉價的粗過濾（不跑 tokenizer），再跑一個 token 預算做終審（一定要跑 tokenizer），裁剪時的 fallback 優先級也是固定的：

```
kotlin 代碼解讀複製代碼conversation_turns → memory → web → knowledge → data → （user_turn 永不裁）
```

也就是， **目前使用者的 user\_turn 永遠保留完整** ，這是"使用者意圖不可以失真"的關鍵邊界：

> *你可以砍歷史，可以砍長期記憶，可以砍 web 搜尋結果，但絕對不能砍使用者這句話本身* 。

## 狀態機

**接著就是狀態機，又分粗狀態和細狀態** ，比如這裡我在 Agent 專案裡就定義了一個 9 態的粗粒度狀態機，主要用來處理：

- `notInitialized` ：進程剛起，尚未掃描本地 asset
- `missingModel / missingRuntime` ：模型是否就緒
- `downloading` ：本地模型二進位或 tflite 權重在下載
- `starting / ready` ：載入完成，可以接單
- `running / stopping` ：正在處理一次會話或正在關停
- `error` ：不可恢復的啟動失敗

[![](https://i.imgur.com/GyTkYdB.jpeg)](https://i.imgur.com/GyTkYdB.jpeg)

> 這個 9 態狀態機關心的是"進程能不能用"，比如 manifest 存在但對應二進位模型，就要觸發下載，遠端模型的 Provider 是否配置，健康檢查 OK 才能進 running。

**然後會有每個業務 Flow 的狀態機，11 個狀態按分成 3 個正常階段** 、 **2 個錯誤/中斷分支** ，同時用 `composite state` 標註 4 種終態的對應關係， **最重要的是每條遷移邊界都標註觸發源** （哪個方法/例外），這樣可以在出問題的時候透過 trace 回溯：

[![](https://i.imgur.com/7yZaxM6.jpeg)](https://i.imgur.com/7yZaxM6.jpeg)

11 態裡每個狀態都有明確的 Actor 標記（ `fsm / llm / genkit` ），這個標記極其關鍵：

> 它在 trace 裡顯式地告訴使用者" *這一步的失敗是模型問題還是工程問題* "，避免回歸時把責任不清晰，比如 `generateOrTool` 是 llm actor， `executeTools` 是 fsm actor， `regenerateFromEvidence` 又回到 llm actor。

**因為 Agent 是一個天然多態的物件，比如：answer 會邊跑邊增長，thinking 也是，tool result 也可能隨時加入 Flow** ，如果這些"可變"緩衝區如果不加約束，很容易就在 UI 層和持久化層之間發生資料競爭，比如 UI 拿到半截 answer 就直接輸出。

**所以需要給每個 Turn 都有獨立的任務管理，只有狀態到達 `commitTurn` 或 `interruptedCommit` 狀態時，透過統一的 committer 一次性把最終 session 回覆，才不至於輸出錯亂** 。

> 比如我專案裡的具體做法是「immutable session 物件 配合 replace assistantTurn」，也就是 `answerBuffer` / `thinkingBuffer` / `processSteps` 都是 controller 內部的追加式欄位，每次 mutation 都產生一個新的 session 物件，只在狀態到達 `commitTurn` 時由 committer 一次性把最終 session 輸出。

**還有一個非常重要的約束是終態的不變量校驗** ，比如四種終態（committed / interruptedCommitted / stopped / failed）有嚴格的不變量：

```
arduino 代碼解讀複製代碼switch (output.terminalOutcome) {
  case committed:            // 必須 !errorPresent && state==commitTurn
  case interruptedCommitted: // 必須 committed && errorPresent
  case stopped:              // 必須 !committed && stopped
  case failed:               // 必須 !committed
}
```

> **任一不變量違反直接 `throw StateError`** ，寧可讓上層看到明顯的 crash，也能有"看起來 committed 但其實有 error"這種沉默污染。

## Agent Loop

**接著就是 Agent Loop** ，它承擔的是「從收到 request 到吐出 response」的完整流程，核心就是把 Planner 的所有計劃都執行完，在 GenKit 裡就是：

> **"工具循環"透過 GenKit `generateStream(maxTurns:)` 內部完成** 。

[![](https://i.imgur.com/6l6LdvZ.jpeg)](https://i.imgur.com/6l6LdvZ.jpeg)

這裡其實有一個和很多" *自己寫 ReAct 循環* "實作完全不同的關鍵選擇：

> **Dart 侧不寫 `while (needsTool)` 循環** ，工具循環是透過 GenKit `generateStream(maxTurns: x)` 內部完成。

**這也是使用 Genkit 的核心，你要讓 Loop 都在 GenKit 裡流轉，能讓 GenKit 做的事絕不在本地重複實作** ，因為你自己寫 `while` 循環意味著你要重新處理 tool\_call parsing、tool\_result 回填、maxTurns 上限、cancellation、streaming chunking 等等，這其實就脫離了使用 GenKit 的意義。

另外，LLM 肯定會有執行出錯的時候，所以在我專案裡，我主要用了三條 recovery 路徑解決這三類問題， **所有 recovery 都透過「丟異常 → 上層再次呼叫 `_streamWithGenkitTools` 」完成鏈路循環** ：

- **Required tool missing** ：當模型輸出的文字裡檢測到 " *疑似 tool\_call 但沒走正規通道* "，比如把 `<execute_tool>` 塞在自然語言裡，那就丟 `_GenkitToolRequiredException` ，上層捕獲後透過 `forceToolUse=true` 遞迴重試，讓 GenKit 用 `toolChoice: required` 強制走 tool schema，這樣語意決策還保留在模型
- **Context overflow** ：捕獲到 model API " *context too long* " 時，觸發 `_runCompleteContextOverflowRetries` ，按 compactLevel 階梯遞增地壓縮 prompt 重試，每一層壓縮都對應 " *刪掉歷史 turn / 縮短 grounding / 剪掉 memory* " 這樣的結構性動作， **從不刪除目前使用者 question 或 grounding facts 的核心欄位**
- **Truncated answer（finishReason=length）** ：如果生成被最大 token 數截斷，就用 `stream_continuation_instruction` 讓模型繼續寫，如果已經有 tool result，就呼叫 `_synthesizeAnswerFromToolResults` 讓模型基於 tool result 重新總結， **這兩條路徑都是把控制權交還給模型** ，本地只做 prompt 組裝

這裡還有一個有意思的是 **Prefill 機制** ：

> **Agent Loop 開始之前就會把已經能確定的 required 工具（比如 knowledge\_search、current\_time 這類無副作用工具）預跑一遍，把結果作為"已完成的 tool\_call"塞進 prompt 裡** 。

這麼做的好處是：

- 減少 GenKit maxTurns 內部循環輪數，節省 latency
- 讓模型第一次生成就能看到 grounding facts，避免多輪反覆
- 對副作用工具（mutating / externalAction）絕不預跑，嚴格遵循" *工具副作用只能由明確的模型工具呼叫觸發* "的邊界

所以整個 Agent Loop 這要有幾個原則：

- **循環靠 GenKit，邊界靠 Dart** ：比如 `maxTurns=6` 由 GenKit 承擔，Dart 只做輸入準備、邊界後校驗、結構性錯誤重試
- **異常即控制流** ：三條 recovery 都透過" *異常 → 上層再次調* "完成，trace 裡能明確看到" *這一 turn 走了幾次重試* "
- **語意決策會回歸模型**

## 工具

**然後就是工具層，核心就是要定義好工具的描述，然後做好資料流向和信任分級** ，要定義好什麼工具有風險，什麼工具可以執行，工具的業務能力是什麼，從工具注入、工具執行和工具結果的流轉，要是可追溯而且是安全的：

[![](https://i.imgur.com/HNxaGSJ.jpeg)](https://i.imgur.com/HNxaGSJ.jpeg)

具體到專案裡，比如我的專案裡內建聲明了 11 個工具，比如 *本地資料摘要、知識庫檢索、目前時間、web 搜尋、URL 擷取、CLI 命令、MCP、裝置控制、長期記憶的讀/寫/遺忘* 等等，每個工具都不是說簡單寫個 「name 和 description」就行，我們需要的是定義一組完整的能力標籤：

維度欄位語義Schema `inputJsonSchema` + `validateInput()` 嚴格 `additionalProperties:false` 副作用 `sideEffectLevel` readOnly/idempotent/mutating/externalAction並行 `parallelSafe` + `canRunInParallel` 只有 readOnly + parallelSafe 才並行風險 `riskLevel` low/medium/high隱私 `resultMayContainPrivateData` 決定是否可回注遠端模型網路 `usesNetwork` 決定信任等級降級預算 `resultCharBudget` + `trimResultContent` 結果字元預算這些標籤是 **runtime 決策依據** ，ToolBroker 在執行前會讀這些欄位決定：

> 是否可以並行發起、結果是否要脫敏後回傳給模型、是否要走審批流、結果超出預算是 truncate 還是 error out，這就是 Agnet 裡常說的 "schema = 契約"， **所有語意決策都基於宣告式中繼資料，本地程式碼不用 hard-code 分支** 。

還可以再疊加一個 **信任 × 副作用矩陣** ：

Trust ＼ SideEffectreadOnlyidempotentmutatingexternalAction **localTrusted** ✅ 自動✅ 自動⚠️ 審批❌ 拒絕 **hostTrusted** ✅ 自動✅ 自動⚠️ 審批⚠️ 審批 **externalUntrusted** ✅ 自動⚠️ 審批❌ 拒絕❌ 拒絕> 信任等級其實也很重要，比如我把線上內建工具自動降級為 externalUntrusted ，那即便 `web_search` 是內建工具，只要它 `usesNetwork=true` ，對應的結果也需要被認為為"外部不可信"，必須先脫敏再返回給模型。

整個工具執行流程在 GenKit 類似如下所示，從模型 tool\_call 到落地 tool result 需要有全流程管理：

[![](https://i.imgur.com/N4FYdwr.jpeg)](https://i.imgur.com/N4FYdwr.jpeg)

這裡其實也有幾個點值得展開說：

- **Provider 格式歸一化** ：不同的 LLM provider 用不同的 tool-call 輸出格式，比如 OpenAI 標準 JSON、 `<execute_tool>...</execute_tool>` XML 區塊、 `<|tool_call|>call:` 前綴格式， **這些格式需要一個歸一化層，任一格式無法歸一化就直接丟異常，不能做字串關鍵字兜底** ，如果連 tool call 都解析不出來，就該讓上層看到 error
- **並行策略** ：只有 `sideEffectLevel=readOnly && parallelSafe=true` 的工具才能並行呼叫，寫操作 `mutating` / `externalAction` 強制串行，避免競態， **高危工具（ `cli` 、 `device_control` ）都強制非並行和高風險預警** ，且預設需要審批
- **三級 recovery 之外還可以有個 replan** ：當工具執行回傳 `status: failed` 時，可以從 `ExecutionPlan` 裡剔除該步驟，把失敗的 tool result envelope **作為 evidence 保留** （供審計和後續 replan 使用）， **但不重新生成 execution plan 的語意** ，因為語意需要由模型下一輪決定

## RAG

RAG 也是很重要的一環， **它決定了能否輸出有事實依據的答案** ，比如我專案裡的 RAG 有 4 條 grounding 通路 + 5 種檢索模式 + RRF 融合：

- **knowledge\_search** ：靜態語料檢索
- **local\_data\_summary** ：外部動態注入的即時資料（訂單、日誌等）
- **web\_search + fetch\_url** ：連網資訊
- **memory\_get** ：長期記憶

> 這四條通路完全解耦，各自維護自己的 provider 介面，Host 可以按業務替換任一路而不影響其他路。

舉個例子：一個客服場景可能關掉連網通路（隱私敏感），但打開 DataContext 通路把訂單表即時餵進來，而檢索器（Retriever）本身是多態的，有 5 種模式：

Mode打分方式場景 `hashLexical` 64 維 hash embedding 的 cosine小語料、快速回退 `bm25` 經典 BM25(k1=1.2, b=0.75)關鍵字匹配為主 `hybrid` hashLexical + normalized BM25 相加預設穩態 `embedding` 384 維 n-gram embedding 的 cosine語意匹配 `hybridEmbedding` embedding + 歸一化 lexical最高品質，最慢> **BM25 常量 `k1=1.2, b=0.75`** 是資訊檢索領域的經驗預設值，對短文件語料非常合適，但 BM25 天然對中文不友善，空格 split 對中文完全無效，所以對中文可以按 unigram/bigram/trigram 全部生成 token 來彌補， **實際上具體還是看你場景和需求** 。

**另外再往上一層還有 Query Rewrite** ，比如我就做了 5 類 query 變體：

Kind語義例子（原查詢："如何檢測大檔案讀寫卡頓"） `originalNaturalLanguage` 原查詢如何檢測大檔案讀寫卡頓 `keywordTerms` 關鍵字化大檔案 讀寫 卡頓 檢測 `technicalTerms` 技術術語file I/O latency detection benchmark `codeApiConfig` 程式碼/API/設定名稱Dart:File.readAsBytes / iostat / fsync `synonymExpression` 同義擴展檢查磁碟寫入延遲 觀測 IO 阻塞生成方式是讓模型輸出規範化 JSON，本地按 schema 校驗，這裡不寫死同義詞表，讓模型基於目前 query 動態生成，也就是"模型=路由器"的另一個表現，5 類變體分別查詢後用 RRF（Reciprocal Rank Fusion）融合：

```
less 代碼解讀複製代碼for each variant q:
    hits[q] = retriever.search(q, limit*2)
    for hit at rank r in hits[q]:
        mergedScore[hit.chunkId] += hit.score + 1/(r+1)
sort by mergedScore
```

> 這裡沒有純 RRF（純 RRF 只用 `1/(rank+k)` ），用的是 **混合了原始分數 + rank 倒數** ，避免完全丟失原來的語意相似度資訊。

Query Rewrite 配合 RRF 的組合是專案 RAG 的基礎品質能力，因為單查詢 BM25 對"術語錯位"（使用者說"卡頓"，文件說"latency"）無解，5 類變體基本能枚舉到檢索失敗的大部分情況。

**融合完之後還有一個 Source Diversity Reranker** ：每個 sourceId 最多貢獻 K 個 chunk，避免命中集中在一個文件，這個 reranker 不改變單條 chunk 的分數排序，只保證 **結果的多樣性** 。

> 這對 grounding 非常關鍵：模型看到 3 個相似 chunk 反而不利於推理，看到 3 個不同角度的 chunk 才能形成有效 context。

## Trace

然後就是 trace， **它是非常重要的標準，也是 Agent 出問題時的回放支援** ，透過 trace 可以知道" *Agent 做了什麼、為什麼這麼做* "，把整個鏈路變成完全可稽核的流程，比如我這裡整個 trace 棧有 5 層，視角是"資料從生產到消費的流水線"：

[![](https://i.imgur.com/h9QR2WJ.jpeg)](https://i.imgur.com/h9QR2WJ.jpeg)

> 比如 GenKit Flow 裡每次 `ai.run(name, fn)` 都會自動產生一個 span，所以 TurnFlow 定義了 21 個 span name。

具體到 TraceReport 的結構：

```
dart 代碼解讀複製代碼class AgentTraceReport {
  final String schema;                 // "local_agent.trace_v1"
  final String turnId;
  final String sessionId;
  final DateTime startedAt;
  final DateTime finishedAt;
  final List<AgentTraceSpan> spans;    // 21 個 lifecycle step
  final AgentTraceModelCall? modelCall;
  final List<AgentTraceToolCall> toolCalls;
  final AgentTraceAuditor? auditor;
  final Map<String, dynamic> metrics;
}
```

> schema 名字帶 `v1` 是為未來 breaking change 留後路（未來 v2 可以並存）， `attributes` 是 open map，每個 lifecycle step 可以塞自己關心的 attribute，不用改結構； `logs` 支援事件日誌，不只是 span 的 start/end。

然後在 Trace 上面還有可以用做幾層配套：

- **Replay Harness** ：把一次 turn 的所有依賴（user query、capability snapshot、tool schemas、KB 命中、模型配置）打包，可以在別的機器上完全離線重放，這樣使用者反饋"Agent 回答錯了"，我們只要拿到 support bundle（就是 replay 包），開發者可以在自己機器上復現
- **EvalDataset** ：把已標註過的 trace 轉成 dataset item，提交到 git，未來有任何改動都跑 dataset 回歸，dataset item 對比新 trace vs golden 時會看：tool calls 順序/名字/參數是否匹配、auditor 判定是否一致、答案是否語意等價（可選 LLM judge）

> 透過 **DynamicRegression 配合 ReleaseDashboard** ，定期跑 EvalDataset，輸出 pass rate 趨勢、各 tag 的分組通過率、slowest span 排行、變差的 case 列表和 trace diff，這樣在持續維護上才能一直推進。

## 脫敏

這是上面 Trace 的後續，因為 Trace 是雙刃劍，它記錄所有細節，也就意味著它可能洩露密鑰、路徑、個人資訊， **所以就必須要有一套統一脫敏機制，防止 support bundle、trace 落盤、tool result 回注模型等場景把敏感資訊帶出去** 。

比如我的專案裡用的是三層策略：

- **Key-based** ：命中"結構化敏感欄位"名（比如 apiKey / authorization / bearer / token / secret / password / cookie）就直接 mask value 為 `[secret]`
- **Text-based** ：無論 key 是否敏感， **對任何字串內容都跑一遍 6 條串行 replace 鏈** ，抓字串裡的隱式密鑰（ `api_key=xxx` / `sk-xxxx` / `ghp_xxxx` / `Authorization:` / 長 base64 / 本地路徑 / stack trace 等）
- **長度限制** ：單條字串預設最長 1200 字元，防止意外洩露的大 blob

除了資訊脫敏，還有兩類"執行階段安全"：

- **Cancellation** ：CancellationToken 全程貫穿，主要使用者點"停止"後所有 in-flight 的 tool call、model call、retriever query 都會立即丟 `CancelledException` ，避免"使用者已經放棄但 Agent 還在偷偷跑"
- **Context Overflow Guard** ：在檢測到 model API 返回 "context too long" 時，觸發預算收緊重試，不直接 fail

## Auditor

**最後還有 Auditor 審核** ，因為 LLM 在執行過程中，可能會出錯，會呼叫錯誤，會輸出錯誤結果，所以需要有對應的事實審核：

[![](https://i.imgur.com/BOnKeJc.jpeg)](https://i.imgur.com/BOnKeJc.jpeg)

Auditor 有一條 **極其重要的職責邊界** ：

> **Auditor 只能校驗和拒絕，不能生成領域答案。**

**Auditor 絕不會也不能改寫模型的答案** ，它只能判 pass/fail 並觸發 model regenerate，舉個例子：

> *模型說"訂單金額是 100"，evidence 顯示"訂單金額是 120"，那麼你只能報錯，不能直接修改* 。

**這個邊界看起來"低效"，但堅持這個邊界能保持系統的可追溯性** ，因為所有答案都是模型生成的，本地程式碼不參與語意決策，這樣 trace 裡的 answer 100% 反映模型能力，評估結果更可信。

**另外審計結論最好是用結構化列舉（IssueCode）表達** ：

Code語義 `emptyAnswer` 答案脫敏/去噪後為空 `missingFactReference` 需要 fact binding 但缺失 `missingGroundingFact` 有 trusted current-tool fact 但答案沒引用 `exactValueMismatch` 數值/精確值和 tool 返回不匹配 `numericOutOfRange` 數值超出 fact 允許範圍 `structurallyIncomplete` 答案結構性不完整（如 JSON 破損） `claimGroundingFailed` 模型抽取的 claim 無法在 evidence 中定位> 上層決策只看 `code` / `severity` / `factKind` ，不看 "rendered issue text"，text 只是給使用者展示用。

**Auditor 內部真正核心主要是 Claim Extraction & Grounding** ：

- 讓模型 A（answer model）先生成答案
- 讓模型 B（可能是同一個模型的另一次呼叫） **從答案中抽取每一條 factual claim** ，透過 tool call 提交結構化結果
- 讓模型 B **對每條 claim 在 evidence 裡定位** ，透過另一個 tool call 提交裁決
- 任一 `verdict != grounded` 就產出 `claimGroundingFailed`

> 這麼做不能保證 100% 無幻覺，但是可以顯著降低" *答案裡出現了 evidence 裡根本沒提到的事實* "的機率。

然後 Auditor 判 fail 後就可以進入 **Regenerate 循環** ：

- Engine 捕獲 audit issue
- 用 `audit_failure_regeneration_instruction` 組裝恢復 prompt，包含 **具體的 issue code + evidence**
- 讓模型基於回饋重新生成
- 重新走 audit
- 然後比如最多 3 次，都失敗則 hard fail（把 audit issues 結構化返回給上層）

> **Regenerate 是基於結構化回饋** ，不是"讓模型再試一次"這種單純的 prompt 指令，回饋裡會明確告訴模型" *你的 claim X 在 evidence 裡找不到，請修正或刪除* "，這樣模型就有了明確的改進方向。

## GenKit 的工程總結

到這裡就把 Genkit 的工程話裡需要了解的大模組聊完了，不過這裡沒有聊更多細節，因為篇幅已經很長，不過回過頭看， **前面這些模組能落地，其實很大程度是因為 GenKit 在專案裡的”工程地盤“作用** ，比如在我專案對 GenKit 的使用大致有幾個點是刻意的：

- **共享 Runtime + Registry** ：整個程序共享一個 `Genkit` 實例，所有 Flow、Tool、Prompt 都註冊在同一個 registry 裡，好處是 replay harness 和真實執行階段用 **同一份註冊表** ，避免"replay 時 tool 沒註冊"這種漂移
- **每個 Tool 都是 GenKit Tool** ：11 個內建工具都透過 `ai.tool()` 註冊為真實的 `genkit.Tool` ，帶完整的 `JSON Schema（input）` + `description（i18n）` ， `generateStream(tools: [...])` 直接把這些 tool 傳給模型，模型走標準的 tool\_calls 通道，GenKit 內部完成「call → execute → feedback」循環，Dart 側唯一要做的就是提供 tool executor 函式
- **每個 Prompt 都是 Dotprompt** ：所有 prompt 全部走 Dotprompt 格式，透過 `ai.prompt('local_agent/system_instruction')` 載入， **也就是 prompt 是版本化 asset** ，可以在不動程式碼的前提下改文案，同時保留可回歸性（asset 會打進 support bundle 供 replay 用），同時 prompt 有 YAML frontmatter，可以宣告 schema、model、config，讓 GenKit Developer UI 直接除錯
- **每個 Turn 都是 GenKit Flow** ：每次 turn 都註冊成 `agent_turn_flow` ，這樣對 Trace 天然可視化（Developer UI 打開就能看到 21 個 lifecycle step 時間線）、Eval 天然可跑（GenKit eval harness 可以直接對 Flow 跑 dataset）、Replay 天然可行（Flow 是純函式，只要輸入相同 + 凍結模型回應，輸出相同）
- **lifecycle step 都是真 span** ： **凡是 GenKit 可承載的，都需要做成了真實 GenKit step/tool/span** ，這樣發生問題時 trace 能精確指到"哪一步生命周期出現失敗"

然後一些設計原則也比較重要，前面也提到了：

- **Schema 即契約** ，從工具 inputJsonSchema 到 TraceReport 的 `schema="local_agent.trace_v1"` ，整個系統的每一個"跨邊界資料結構"都有 schema 名字，這樣 Trace 可以 replay（schema 變了舊資料不會被誤讀）、Tool 能做強驗證（模型輸出的 tool\_call 參數必須符合 schema）、Prompt 頁可以重用（同一個 schema 可以餵給不同模型）
- **模型即路由器** ，IntentRouter 中立不做關鍵字匹配，Query rewrite 透過模型生成 5 類變體，Tool 選擇由模型基於 schema 決定，Audit 結論由模型基於 evidence 判定，長期記憶的寫入由模型主動 memory\_write， **我們本地程式碼只提供能力、schema、權限、隱私邊界，語意決策全部交給模型**
- **Auditor 只校驗不生成** ，Auditor 有能力拒絕、有能力觸發 regenerate、有能力回傳結構化 issue，但不能替模型寫答案
- **所有觀測都是可 replay 的一等公民** ，TraceReport 需要可序列化的，Support bundle 打包所有依賴，Replay Harness 能離線重現，EvalDataset 能定期回歸
- **i18n** ，Dotprompt 不寫死自然語言，所有 label/description 走 messages 介面，locale 從外面注入，甚至 `<truncated>` 標記都按 locale 渲染

## 最後

所以，這就是一個很普通的、基於 GenKit 實作一個本地化 Agent 的常見工程化場合就是這樣，其實這已經是一個很基礎的企業 Agent 實作現場了。

> **用 GenKit 的好處是它支援的語言多，同時比如 GenKit Dart 既可以在客戶端，也可以在伺服器端，甚至可以 GenKit Dart 和 GenKit Python/TS 互動，透過 GenKit 用在 Flutter 也可以做全平台 App 場景，所以某種程度上，這也是 Agent 時代之後，Flutter 又成長了一些的原因** 。

**事實上也有人說，有 OpenCode 了為什麼還要自己寫 Agent** ？

因為場景不同啊， **企業內 Agent 最大的意義就是你的特殊業務 Flow、你的特殊流轉流程，你的意圖邊界不是通用的，你的工具集合不是通用的，你的隱私策略更不是通用的** ，你不能用一個通用產品去把這些"邊界"塞進去，或者說塞進去就變成了一堆開關、規則、if/else，這樣反而更難維護。

還有人說為什麼要 RAG，比如 Claude Code 為什麼不用 RAG？然後我們做 Agent 業務又一直提 RAG 的重要性？這裡就是場景的區分：

- 因為程式碼倉庫本身就是一個結構化、可搜尋、能執行和驗證的上下文環境，它可以直接 grep 檔案、讀程式碼、跑測試、看報錯，再一步步定位問題
- **而企業業務 Agent 面對的是分散在文件庫、資料庫、CRM、工單、權限系統裡這些私有知識和即時業務事實** ，這種情況下 RAG 就更有價值、有一套檢索增強層，把正確的資料在正確權限下取出來，再交給模型判斷，各種圖片，PPT，PDF，word，透過多層拆分，把 tag、摘要、原文、附件都規劃成一個知識庫系統

> 所以場景不同，Claude Code 的核心是“在程式碼環境裡搜尋和驗證”，而企業 Agent 的核心是“連接企業真實知識和業務系統”，所以企業 Agent 要 RAG， **但這個 RAG 不只是向量庫，還對應有文件檢索、SQL/API、權限過濾、規則校驗和證據引用組成的廣義外部知識系統** 。

**Agent SDK 給到的價值也正好在這裡** ，而 GenKit 它不像 LangGraph 那麼強的狀態編排，也不像 LangChain 那麼雜，它就是把 Flow / Tool / Prompt / Trace 這幾件核心用一套規範抽好了，你需要的就是圍繞它建構你自己的 FSM、Planner、Auditor、RAG 通路、Skill 系統、隱私邊界。

所以，Agent 的作用從來都不是讓模型變聰明，我們要做的是把 LLM 能力約束在我們的業務範圍內，同時可以回溯和驗證，這就是做獨立 Agent 的意義。

---

原文出處： [https://juejin.cn/post/7658865284564926518](https://juejin.cn/post/7658865284564926518)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3QKlEq) [7瀏覽](https://codelove.tw/@tony/post/3QKlEq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3QKlEq)

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

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝18 💬3 ❤️4

723

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

140

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783576470&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3QKlEq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783576470199&amp;bpp=1&amp;bdt=114&amp;idt=49&amp;shv=r20260706&amp;mjsv=m202607010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783567951%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783567951%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783567951%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4802093289486&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1486&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=31099551%2C95395869&amp;oid=2&amp;pvsid=6756675450479453&amp;tmod=475751733&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1726%2C1050%2C1486%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.16&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=57" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>