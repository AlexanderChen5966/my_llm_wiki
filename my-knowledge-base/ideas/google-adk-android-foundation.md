---
title: Google ADK for Android 0.1.0 基礎（Juejin 文章）
date: 2026-06-17
tags: [dev/flutter, ai/agent]
source: https://codelove.tw/@tony/post/3VzjD3
type: idea
status: processed
asset_value: 40
---

Google ADK for Android / ADK for Kotlin 0.1.0 版本的初始介紹文章，涵蓋 LlmAgent 基礎架構、@Tool/@Param 工具標注系統、Session state + Memory service 設計。更完整的進階版本（v0.2.0）見 [[android-adk-agent-tool-calling]]。

**0.1.0 核心概念摘要：**

**LlmAgent 架構：**
```kotlin
val orchestrator = LlmAgent(
    name = "trip_orchestrator",
    model = Gemini(apiKey = apiKey, name = MODEL_NAME),
    instruction = Instruction("..."),
    tools = listOf(GetTripDetailsTool(tripId)),
    subAgents = listOf(carRentalPipeline, hotelPipeline)
)
```

**@Tool/@Param 工具標注（KSP 生成）：**
```kotlin
class TripService {
    @Tool
    fun getBookingStatus(
        @Param("Booking id from the user's trip document")
        bookingId: String
    ): String { ... }
}
// 使用：TripService().generatedTools()（需要 google-adk-kotlin-processor）
```

**Session 與 Memory 區別：**
- Session state：短期，服務當前會話的上下文共享
- Memory service：長期，可保存跨會話資訊
- ⚠️ 不要誤解為 SharedPreferences 或聊天紀錄

**Android 特有注意事項：**
工具呼叫會碰到執行緒和生命週期問題；不要在主執行緒做 DB 查詢；要處理檔案不存在、讀取失敗、權限拒絕等分支。

**版本狀態：** 0.1.0 為實驗版，適合嘗鮮，不適合接正式業務。

完整生產級用法見 [[android-adk-agent-tool-calling]]（v0.2.0）。
