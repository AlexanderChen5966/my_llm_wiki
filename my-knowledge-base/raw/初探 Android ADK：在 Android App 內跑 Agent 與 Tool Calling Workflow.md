---
title: "初探 Android ADK：在 Android App 內跑 Agent 與 Tool Calling Workflow"
source: "https://medium.com/ddsakura-blog/%E5%88%9D%E6%8E%A2-android-adk-%E5%9C%A8-android-app-%E5%85%A7%E8%B7%91-agent-%E8%88%87-tool-calling-workflow-67dd6e35545b"
author:
  - "[[ddsakura]]"
published: 2026-05-31
created: 2026-06-09
description: "今年 Google I/O 期間，Google 推出了 Agent Development Kit（ADK）for Kotlin 與 ADK for Android。"
tags:
  - "clippings"
status: processed
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*2q10r4RIX7EukACWphkRqA.jpeg)

今年 Google I/O 期間，Google 推出了 Agent Development Kit（ADK）for Kotlin 與 ADK for Android。

看完相關介紹後，我趁著週末做了一個小型 spike project，想實際了解 Android ADK 的開發體驗，以及它能幫我們處理多少 agent workflow。

這次的目標很單純：  
讓使用者輸入購物需求，例如：

> 我想找 3000 元以下適合通勤的藍牙耳機

接著由 Android App 內的 agent 自行判斷需要呼叫哪些 tools，最後回傳推薦商品與理由。

過去如果要做類似的 agent workflow，我大多會自己實作。  
流程通常長這樣：

```c
使用者輸入
↓
呼叫 LLM
↓
解析 JSON 或 tool call
↓
dispatch 到本地 function
↓
把 tool result 再送回 LLM
↓
顯示 final answer
```

但當 tool 開始變多、多輪對話開始出現、context state 需要管理時，自己維護這套流程就會越來越複雜。

因此這次 spike 最大的目的是想看看 Android ADK 能否使用並幫忙處理這些 agent workflow。

這次刻意把 shopping domain 做得非常簡單。  
商品資料直接 hardcode：

```c
data class Product(
    val id: String,
    val name: String,
    val category: String,
    val price: Int,
    val tags: List<String>,
    val description: String,
)
```

主要只有兩個 tools：

- SearchProductsTool: 搜尋商品。
- CompareProductsTool: 以及比較商品並產生推薦理由。

**ADK Tool 定義**

ADK 提供 annotation 與 KSP code generation。  
開發者只需要宣告 function：

```c
@Tool(
    name = "search_products",
    description = "Search hardcoded shopping products by category and optional maximum price.",
)
fun searchProductsForAgent(
    @Param("Product category to search. Use earphone for Bluetooth earbuds, or any for all categories.")
    category: String,
    @Param("Maximum product price in local currency. Use 0 if there is no budget.")
    maxPrice: Int,
): String
```

ADK 會透過 KSP 產生對應的 tool wrapper。  
Agent 在執行期間便能透過這些 generated tools 完成 tool calling workflow。  
這部分的開發體驗其實不錯，有點類似 Retrofit annotation 或 Room annotation 的感覺。  
整個 spike 的流程大致如下：

```c
Compose UI
    ↓
ViewModel
    ↓
Android ADK
(InMemoryRunner / LlmAgent)
    ↓
HttpRemoteModel
    ↓
OpenAiCompatibleJsonMapper
    ↓
LM Studio
    ↓
Model Tool Calls
    ↓
ADK Generated Function Tools
    ↓
SearchProductsTool
CompareProductsTool
    ↓
Tool Responses
    ↓
Model Final Answer
```

從 Android ADK 的角度來看：  
它只知道自己在呼叫一個 Model。  
至於後面接的是 Gemini、OpenAI、LM Studio，還是其他 provider，其實不重要。

## Get ddsakura’s stories in your inbox

Join Medium for free to get updates from this writer.

一開始其實有考慮直接串 Gemini API。  
但馬上遇到一個很務實的問題：  
要處理 API Key，  
有點麻煩。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*FAh35Zwd7btsZrLaQb6YOQ.png)

因此最後選擇使用 LM Studio 跑 local model。  
Android Emulator 可以直接透過：

```c
http://10.0.2.2:1234/v1/chat/completions
```

存取 host machine 上的 LM Studio。  
這樣就能讓 Android App 直接與 local LLM 溝通。  
這次使用的模型是：

```c
google/gemma-4-e4b
```

ADK 使用自己的 model interface：

```c
Model.generateContent(
    request: LlmRequest,
    stream: Boolean
): Flow<LlmResponse>
```

而 LM Studio 接受的是 OpenAI-compatible API：

```c
/v1/chat/completions
```

包含：

- messages
- tools
- tool\_calls
- role=tool

因此中間需要做一層轉換：

```c
ADK LlmRequest
    ↓
OpenAI-compatible JSON
    ↓
LM Studio
    ↓
ADK LlmResponse
```

我把這層抽象成：

```c
interface RemoteModelJsonMapper {
    fun toRequestBody(
        request: LlmRequest,
        modelName: String
    ): String
    fun toLlmResponse(
        responseBody: String
    ): LlmResponse
}
```

負責處理：

- ADK system instruction → OpenAI system message
- ADK user content → OpenAI user message
- ADK function declarations → OpenAI tools
- OpenAI tool\_calls → ADK FunctionCall
- ADK FunctionResponse → OpenAI role=tool
- Assistant content → ADK text response

最後 Android 端的 model provider 長這樣：

```c
object LmStudioModelProvider : ModelProvider {
    override val label: String =
        "LM Studio at http://10.0.2.2:1234/v1/chat/completions"
    override fun getModel(): Model =
        HttpRemoteModel(
            endpoint =
                "http://10.0.2.2:1234/v1/chat/completions",
            modelName = "google/gemma-4-e4b",
            mapper = OpenAiCompatibleJsonMapper(),
        )
}
```

最後在 Android Emulator 中以 Remote Model Mode 執行。  
Debug Summary 顯示：

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*NhYSd83bTW-K6pCvCnTYQw.png)

```c
provider = LM Studio at http://10.0.2.2:1234/v1/chat/completions
adkEvents = 5
modelFunctionCalls = 2
androidToolResponses = 2
```

代表：

- LM Studio 回傳了 2 次 tool call
- Android ADK 執行了 2 次 tool
- Tool 結果再回送給 model

整條路徑完整跑通：

```c
Android ADK
    ↓
Local LLM
    ↓
Tool Call
    ↓
Android App-side Tool
    ↓
Tool Result
    ↓
Local LLM
    ↓
Next Tool Call
    ↓
Android App-side Tool
    ↓
Final Answer
```

這次 spike 下來，我覺得可以整理以下結論。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*vwjCkxdH_5rGhhXEQhRKQQ.png)

第一，Android ADK 確實能在 App 內 host agent workflow  
LlmAgent、InMemoryRunner、KSP generated tools、event stream 都能正常運作。  
Android App 可以直接成為 agent runtime。

第二，Model Provider 可以替換  
這次接的是 LM Studio。  
但對 Android ADK 來說，它只看到一個 Model。  
只要 adapter 實作得出來，後面可以換成：

這部分彈性很高。

目前這個 spike 還非常陽春。  
接下來如果有時間，我可能會繼續嘗試：

- 多種 user intent
- 多輪對話
- 多個 tools 協作
- 不同 model provider 替換
- App-side state 與 model context 整合
- RAG 與本地資料查詢
- Streaming response UI

目前 Android ADK 仍然相當新，我這次使用的版本是 0.2.0。  
現階段我會把它定位成：  
適合 spike、prototype、學習 agent architecture。  
至於 production 是否適合導入，還需要再觀察版本成熟度、模型成本、安全性以及整體維運方式。