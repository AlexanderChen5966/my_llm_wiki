---
title: "Day 6｜行動是關鍵：Tool Use 讓 LLM 突破回答的侷限 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10384461"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：為什麼需要工具？ 昨天 (Day 5)，我們透過 RAG 讓 LLM 不再只憑記憶或幻覺，而是能「開書考」，從文件或網路找到依據，再生成答案。但即使如此，RAG 仍有明顯的侷限： 需要即時資訊：..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 6

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 6 篇

## Day 6｜行動是關鍵：Tool Use 讓 LLM 突破回答的侷限

- 分享至

## 前言：為什麼需要工具？

[**昨天 (Day 5)**](https://ithelp.ithome.com.tw/articles/10383804) ，我們透過 **RAG** 讓 LLM 不再只憑記憶或幻覺，而是能「開書考」，從文件或網路找到依據，再生成答案。  
但即使如此，RAG 仍有明顯的侷限：

- **需要即時資訊** ：今天的天氣、即時股價、航班狀態
- **需要行動能力** ：寄送 Email、建立行事曆事件、下單買票

這些問題光靠「搜尋」仍然不夠，因為它們需要 **呼叫外部工具或 API** ，才能獲得答案或完成任務。

換句話說：昨天我們讓模型「找到依據」，但它還不能真的動手。今天要跨出的，就是讓它具備 **行動力** 。

而這正是 **Tool Use（工具使用）** 的核心價值：  
**讓 LLM 從「語言回答者」進化成「行動代理人」。**

---

## Tool Use 是什麼？

所謂「工具使用」，就是讓 LLM 在對話過程中， **能夠調用外部函式、API 或程式** ，並將結果再帶回來繼續推理或回應。  
它的典型流程是：

1. 使用者提出問題
2. 模型判斷是否需要外部工具
3. 呼叫工具並取得結果
4. 根據結果生成最終回答

**換句話說：LLM 負責決定何時用工具，工具負責精準執行。**

> 本篇先把 Tool Use 的地基打好——清楚的工具合約、流程骨架與簡單案例。至於「邊思考邊行動」的完整節奏（常見做法之一是 ReAct），我們之後會再回來細看。

---

## Tool Use Design Pattern

### 核心概念

LLM 雖然很強大，但它的知識與能力都被限制在訓練資料裡。要處理即時資訊或實際動作，就必須借助 **工具（Tools）** 。這種模式常被稱為 **Tool Use Pattern** 或 **Function Calling** ：

- 我們先定義可用的工具（名稱、描述、輸入結構）。
- 當 LLM 發現問題需要外部幫助時，就會輸出結構化參數（例如 JSON），由系統實際呼叫工具。
- 工具執行完後，把結果回傳給 LLM。
- LLM 再基於這個結果，生成最後的自然語言回覆。

---

### 設計流程（簡單 4 步）

1. **識別任務** ：LLM 判斷問題需要外部工具（例如「查今天維也納的天氣」）。
2. **呼叫工具** ：依工具的輸入結構產生參數（JSON 等），並執行 API/函式。
3. **接收結果** ：取得結構化回傳資料（例如「雷陣雨，22°C」）。
4. **整合回覆** ：將結果轉換成人類能理解的自然語言，或接續後續規劃。

  
*圖：Tool Use 的四步驟設計流程——從任務識別、呼叫工具、接收結果，到整合回覆。*

這四個步驟就是 **Tool Use Pattern 的核心骨架** ，幾乎所有框架（LangChain、OpenAI Function Calling、MCP）都脫離不了這個模式。

---

## Demo 1：最小可行 Tool Use（查天氣）

以下用「查今天維也納的天氣」當例子。

**步驟說明**

1. 使用者問：「今天維也納天氣如何？」
2. LLM 判斷：這需要即時資訊 → 呼叫 `get_weather` 工具。
3. 工具查詢並回傳「雷陣雨，22°C」。
4. LLM 整合成自然語言回覆：「今天維也納的天氣是雷陣雨，氣溫 22°C。」

![查今天維也納的天氣](https://ithelp.ithome.com.tw/upload/images/20250920/20165544sgEhUPWYkb.png)  
*圖：最小可行的 Tool Use 範例——LLM 呼叫天氣 API，查詢並回覆維也納的即時天氣。*

**範例程式**

```python
import google.generativeai as genai

# 1. 假設外部工具（API）
def get_weather(city: str):
    weather_data = {
        "Vienna": "雷陣雨，氣溫 22°C",
        "Taipei": "多雲，氣溫 30°C",
    }
    return weather_data.get(city, "查無資料")

# 2. 初始化 LLM
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

# 3. 使用者查詢
query = "請告訴我今天維也納的天氣"

# 4. 模擬 LLM 判斷需要呼叫工具
if "維也納" in query and "天氣" in query:
    result = get_weather("Vienna")
    answer = f"今天維也納的天氣是：{result}。"
else:
    answer = llm.generate_content(query).text

print(answer)
```

**輸出結果**  
![最小可行的 Tool Use 範例輸出結果](https://ithelp.ithome.com.tw/upload/images/20250920/20165544V2Za0dtOLQ.png)  
*圖：Demo 1 的輸出結果——模型呼叫天氣工具後，回覆維也納的即時天氣。*

如果沒有工具，LLM 可能會「憑記憶亂猜」天氣狀況，甚至答非所問。但透過 Tool Use，模型能直接查詢 API，把即時正確的資料帶回來，再生成自然語言回覆。這展示了「讓模型能動手」的第一步價值。

---

## Demo 2：RAG + Tool Use 結合（美泉宮下雨怎麼辦？）

把 **RAG（背景知識）** 與 **Tool Use（即時資訊）** 結合，就能讓模型既有「知識深度」又有「行動能力」。

**步驟說明**

1. 使用者問：「美泉宮值得排一天嗎？如果下雨怎麼辦？」
2. LLM 先用 **RAG** 查背景知識：美泉宮通常要花一天參觀。
3. LLM 同時呼叫 **天氣工具** ：明天預測雷陣雨。
4. LLM 整合兩者 → 回覆：「美泉宮值得排一天，但因天氣不好，建議改參觀室內展覽，或延後。」

![RAG + Tool Use 結合](https://ithelp.ithome.com.tw/upload/images/20250920/20165544YCDCfFJp9w.png)  
*圖：RAG + Tool Use 結合案例——LLM 同時檢索背景知識與查詢即時天氣，再整合為行動建議。*

**範例程式**

```python
from sentence_transformers import SentenceTransformer
import faiss
import google.generativeai as genai

# 1. 初始化編碼模型
encoder = SentenceTransformer("BAAI/bge-base-zh-v1.5")

# 2. 建立文件庫（旅行知識庫）
docs = [
    "美泉宮（Schönbrunn Palace）有 1,441 個房間、花園、動物園，通常需要花上一整天參觀。",
    "美景宮（Belvedere Palace）以藝術收藏聞名，通常半天足夠。",
]

# 3. 建立向量索引
embeddings = encoder.encode(docs)
index = faiss.IndexFlatL2(embeddings.shape[1])
index.add(embeddings)

# 4. 定義工具（假想天氣 API）
def get_weather(city: str):
    weather_data = {
        "Vienna": "雷陣雨，氣溫 22°C",
        "Taipei": "多雲，氣溫 30°C",
    }
    return weather_data.get(city, "查無資料")

# 5. 使用者問題
query = "美泉宮值得排一天參觀嗎？如果明天下雨要怎麼安排？"

# 6. RAG：檢索知識
q_emb = encoder.encode([query])
D, I = index.search(q_emb, k=1)
retrieved = docs[I[0][0]]

# 7. Tool Use：查天氣
weather = get_weather("Vienna")

# 8. 呼叫 LLM 整合
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

prompt = f"""
根據以下資訊回答問題：

[知識庫資料]
{retrieved}

[天氣資訊]
{weather}

問題：{query}
請用中文簡潔回答。
"""
response = llm.generate_content(prompt)

print("搜尋結果:", retrieved)
print("天氣結果:", weather)
print("LLM 回答:", response.text)
```

**輸出結果**

![RAG + Tool Use 結合輸出結果](https://ithelp.ithome.com.tw/upload/images/20250920/201655449hWMN3NHeC.png)  
*圖：Demo 2 的輸出結果——RAG 提供背景知識，Tool Use 提供即時天氣，LLM 整合後給出行動建議。*

這個範例展現了 **RAG 與 Tool Use 的互補** ：

- **RAG** 確保答案有「知識基礎」（美泉宮值得花一天參觀）。
- **Tool Use** 提供「即時資訊」（明天下雨）。
- **LLM** 才能整合兩者，給出貼近真實場景的規劃建議。

如果少了其中任何一個，回答都會不完整：只靠 RAG，模型無法知道天氣；只靠 Tool Use，它又不知道美泉宮的背景價值。

---

## 工具使用的挑戰

1. **設計成本** ：需要先定義清楚工具介面（名稱、輸入、輸出），否則模型無法正確調用。
2. **安全性** ：工具能做的事越多，風險也越高，例如誤刪檔案、亂下單。
3. **可靠性** ：工具可能失效（API down）、輸出異常，LLM 需要能處理例外狀況。
4. **使用者信任** ：模型說「已寄信」或「已訂票」，用戶要能驗證，否則體驗會崩壞。

---

## 小結

- **RAG 解決「答案可靠」** ， **Tool Use 解決「能不能行動」** ，兩者結合才能讓模型既有知識深度，又能即時落地。
- **Tool Use Pattern 的四步驟** （識別 → 呼叫 → 接收 → 整合）是所有框架的基礎，也是今天打下的核心地基。
- 工具使用雖然強大，但也帶來 **設計、安全與可靠性** 的挑戰，需要謹慎規劃。
- 今天我們讓模型真的能「動手」，接下來會進一步學習 **規劃（Planning）** ，讓 AI 能完成更複雜的多步驟任務；至於 **ReAct** ，則會在之後回來更完整地討論。

---

![藝術史博物館](https://ithelp.ithome.com.tw/upload/images/20250920/20165544Dpp39Sm95C.jpg)  
*圖：維也納藝術史博物館（Kunsthistorisches Museum）。穹頂壁畫與大廳雕塑展現的不只是藝術之美，更是「知識化為行動」的象徵：如同 LLM 若只會回答，便止步於欣賞；透過 Tool Use，則能真正「動手」，在現實世界中留下痕跡。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10383804)

[

Day 5｜接上真實世界：RAG 讓 LLM 有憑有據

](https://ithelp.ithome.com.tw/articles/10383804)[下一篇](https://ithelp.ithome.com.tw/articles/10385063)

[

Day 7｜有計劃才有行動：Planning 讓 LLM 安排更周全

](https://ithelp.ithome.com.tw/articles/10385063)

系列文

[踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友](https://ithelp.ithome.com.tw/users/20165544/ironman/8801) 共 30 篇

目錄

1. 26
	[Day 26｜AI 的守護靈獸：Guardrails 讓 Agent 劃出安全邊界](https://ithelp.ithome.com.tw/articles/10395588)
2. 27
	[Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴](https://ithelp.ithome.com.tw/articles/10396238)
3. 28
	[Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作](https://ithelp.ithome.com.tw/articles/10396549)
4. 29
	[Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！](https://ithelp.ithome.com.tw/articles/10396972)
5. 30
	[Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅](https://ithelp.ithome.com.tw/articles/10397208)
[完整目錄](https://ithelp.ithome.com.tw/users/20165544/ironman/8801)

[![.](https://itadstatic.ithome.com.tw/B9/1779447171_6a10358375783.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2605B9004)