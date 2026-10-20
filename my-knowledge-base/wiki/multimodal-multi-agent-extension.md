---
title: Multimodal Agent：用 Gemini 讓 Agent 看懂圖片並擴充多 Agent 系統
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10395412
type: article
status: processed
asset_value: 65
---

純文字 Agent 看不懂菜單照片、地圖截圖。用 Gemini 2.5 Flash 的多模態能力，讓 Agent 直接讀圖片內容並推理，再把這個能力包裝成新的專家 Agent，插入既有的 Supervisor 多 Agent 系統。

## T（核心論點）

多模態（Multimodal）讓 Agent 從「只能讀文字」進化到「能感知圖片」，實作上只需把圖片以 base64 編碼塞進 `HumanMessage` 的 content list，搭配文字 prompt 一起送給支援視覺的模型（如 Gemini 2.5 Flash）。多模態能力可以像任何工具一樣，包裝成 `@tool` 插入既有的 Supervisor 架構。

## B（適用場景）

**適用：**
- 輸入包含圖片但需要文字化的推理輸出（菜單翻譯、截圖判讀、文件辨識）
- 既有 Multi-Agent 系統需要新增「讀圖」能力，且不想重構整體架構

**不適用：**
- 純文字任務（多模態模型通常比純文字模型貴且慢，沒必要）
- 需要精確像素級操作（如點擊座標）的場景，應考慮專門的 Computer Use Agent（參見 [[fara-computer-use-agent]]）

## R（實作模式）

### 多模態訊息格式

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_core.messages import HumanMessage
import base64

llm = ChatGoogleGenerativeAI(model="gemini-2.5-flash")

with open("menu.jpg", "rb") as f:
    encoded = base64.b64encode(f.read()).decode("utf-8")

message = HumanMessage(content=[
    {"type": "text", "text": "這是一張德國餐廳菜單，請翻譯成中文並推薦適合的菜品"},
    {"type": "image_url", "image_url": f"data:image/jpeg;base64,{encoded}"}
])

response = llm.invoke([message])
```

範例輸入為 Figlmüller 餐廳菜單照片，模型成功翻譯德文菜名並給出推薦。

### 擴充既有 Multi-Agent 系統：新增 CuisineAgent

延續 [[supervisor-multi-agent-langchain]] 的 Supervisor 架構，新增一個無工具、純多模態推理的專家 Agent：

```python
cuisine_agent_prompt = (
    "你是美食顧問，會讀取使用者提供的菜單圖片，"
    "並依照飲食偏好（如不吃牛肉）篩選推薦菜品。"
)

@tool("CuisineAgent", description="讀取菜單圖片並依飲食偏好推薦菜品")
async def call_cuisine_agent(image_path: str, dietary_preferences: str) -> str:
    with open(image_path, "rb") as f:
        encoded = base64.b64encode(f.read()).decode("utf-8")
    message = HumanMessage(content=[
        {"type": "text", "text": f"飲食偏好：{dietary_preferences}。{cuisine_agent_prompt}"},
        {"type": "image_url", "image_url": f"data:image/jpeg;base64,{encoded}"}
    ])
    result = await llm.ainvoke([message])
    return result.content

# 加入 Supervisor 的工具列表
supervisor = create_agent(
    model=llm,
    tools=[call_weather_agent, call_planner_agent, call_advisor_agent, call_cuisine_agent],
    system_prompt="你是旅遊規劃 Supervisor，可呼叫天氣、行程、建議、美食四位專家。"
)
```

**關鍵設計：CuisineAgent 不需要額外工具，本身的「讀圖能力」就是它的核心價值，包裝方式與其他純文字專家 Agent 完全一致**——這證明多模態能力可以無痛插入既有的 Tool Calling Pattern 架構，不需重新設計協調邏輯。

## C（關鍵行動）

- [ ] 圖片內容用 base64 編碼塞入 `image_url` content block，搭配文字 prompt 一起傳入多模態模型
- [ ] 新增多模態能力時，優先考慮包裝成獨立的專家 Agent（工具），而非把圖片邏輯硬塞進現有 Agent
- [ ] 多模態 Agent 不一定需要額外工具——「讀圖+推理」本身就可以是完整的職責

## 相關連結

- [[supervisor-multi-agent-langchain]] — Day 24：本篇擴充的 Supervisor 架構基礎
- [[fara-computer-use-agent]] — 需要精確操作座標時的替代方案（Computer Use Agent）
- [[n8n-multi-agent-workflow]] — Day 28：同系統在 n8n 中的視覺化版本
- [[synthesis-multi-agent]] — Multi-Agent 系統知識精粹（本主題 7 篇整合）
