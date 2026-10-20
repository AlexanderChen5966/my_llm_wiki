---
title: Android ADK：在 Android App 內跑 AI Agent 與 Tool Calling
date: 2026-06-15
tags: [dev/flutter, ai/agent]
source: https://medium.com/ddsakura-blog/初探-android-adk-在-android-app-內跑-agent-與-tool-calling-workflow-67dd6e35545b
type: article
status: processed
asset_value: 65
---

Google I/O 2026 推出 Agent Development Kit（ADK）for Kotlin 與 ADK for Android，讓 Android App 本身成為 Agent Runtime。Spike 實驗目標：讓 App 內 Agent 自主判斷呼叫哪些 tools，返回購物推薦（不需人工 dispatch）。

## T（核心論點）

Android ADK 讓 Android App 直接成為 Agent Runtime，核心優勢：
- **KSP Annotation** 自動生成 Tool Wrapper（類似 Retrofit / Room），大幅降低 tool 整合成本
- **Model Provider 可替換**：ADK 只看 Model interface，後面接 Gemini / LM Studio / 任何 OpenAI-compatible API 都可以
- **完整事件流**：LlmAgent + InMemoryRunner + event stream 都能正常運作

## B（適用場景）

**適用：**
- 在 Android App 內嵌入具有 Tool Calling 能力的 AI Agent
- 需要 App-side 資料 + LLM 推理的組合（本地購物搜尋、本地文件 RAG）
- Spike / Prototype / 學習 Agent Architecture

**目前限制（版本 0.2.0）：**
- 生產環境成熟度待觀察（版本較新）
- 多輪對話 / 多個 tools 協作仍需實驗
- Streaming response UI 尚未完善

## R（詳細內容）

### 系統架構

```
Compose UI
    ↓
ViewModel
    ↓
Android ADK (InMemoryRunner / LlmAgent)
    ↓
HttpRemoteModel
    ↓
OpenAiCompatibleJsonMapper
    ↓
LM Studio / Gemini API
    ↓
Model Tool Calls
    ↓
ADK Generated Function Tools
    ↓
Tool Responses → Final Answer
```

### KSP Annotation Tool 定義

```kotlin
@Tool(
    name = "search_products",
    description = "Search products by category and max price.",
)
fun searchProductsForAgent(
    @Param("Product category. Use 'any' for all.")
    category: String,
    @Param("Max price. Use 0 if no budget.")
    maxPrice: Int,
): String
```

ADK 透過 KSP 自動產生 tool wrapper，開發體驗接近 Retrofit。

### Local LLM 接入（繞過 API Key）

```kotlin
// Android Emulator 可透過 10.0.2.2 存取 host machine LM Studio
object LmStudioModelProvider : ModelProvider {
    override fun getModel(): Model =
        HttpRemoteModel(
            endpoint = "http://10.0.2.2:1234/v1/chat/completions",
            modelName = "google/gemma-4-e4b",
            mapper = OpenAiCompatibleJsonMapper(),
        )
}
```

ADK LlmRequest ↔ OpenAI-compatible JSON 需要一層 RemoteModelJsonMapper 轉換。

### 實驗結果

```
adkEvents = 5
modelFunctionCalls = 2      ← LM Studio 回傳了 2 次 tool call
androidToolResponses = 2    ← ADK 執行了 2 次 tool
```

完整路徑：Android ADK → Local LLM → Tool Call → App-side Tool → Tool Result → Next Tool Call → Final Answer

## C（關鍵行動）

- [ ] 評估 Android ADK 在 Sanlong App 的潛在應用（例如自然語言查詢優惠活動）
- [ ] 比較 Android ADK vs 純 REST API call 的架構選擇
- [ ] 關注 ADK 版本成熟度（目前 0.2.0）

## 相關連結

- [[android-cli-ai-agent-workflow]] — Android CLI + AI Agent（開發自動化，非 App 內 Agent）
- [[research/android-ai-integration-appfunctions]] — Android AppFunctions（Android MCP）+ ADK 生態
- [[flutter-google-io-2026]] — Flutter 3.44 / A2UI（同屆 Google I/O 相關）
- [[ai-agent-architecture-10-modules]] — AI Agent 10 個核心模組（ADK 的底層概念）
