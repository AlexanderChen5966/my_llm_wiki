---
title: "Day 7｜有計劃才有行動：Planning 讓 LLM 安排更周全 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10385063"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：從會「做事」到會「安排事」 在 昨天 (Day 6) 我們讓 LLM 學會使用工具，能查天氣、看文件、甚至寄出 email。 這樣的 AI 已經會「動手」，但它還缺少一個關鍵能力：安排順序。 想..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 7

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 7 篇

## Day 7｜有計劃才有行動：Planning 讓 LLM 安排更周全

- 分享至

## 前言：從會「做事」到會「安排事」

在 [昨天 (Day 6)](https://ithelp.ithome.com.tw/articles/10384461) 我們讓 LLM 學會使用工具，能查天氣、看文件、甚至寄出 email。  
這樣的 AI 已經會「動手」，但它還缺少一個關鍵能力： **安排順序** 。

想像你交代 AI：「幫我規劃今天去美泉宮、美景宮、聖史蒂芬大教堂。」  
如果它只是單純查詢資訊、逐一回覆，最後你拿到的仍然是一堆零散結果，而不是一個完整的行程表。

這就是 **Planning（規劃）** 的價值：  
能把一個目標拆解成一系列可執行步驟，並考量前後順序與限制，讓 AI 的行動更有條理。

---

## Planning Pattern 是什麼？

### 核心概念

Planning Pattern 的精神是 **Plan → Execute** ：先規劃，再執行。

### 流程步驟

1. **Define goals** （定義目標）：清楚知道要完成什麼，例如一天內參觀三個景點。
2. **Analyze constraints** （分析限制）：考量時間、交通、資源等條件。
3. **Generate plan** （生成計劃）：AI 一次產出完整的步驟清單，像藍圖一樣。
4. **Execute & adjust** （執行與調整）：系統依照計劃逐步執行，必要時依情境微調。

### 特色

- 計劃與執行分開。
- 計劃在一開始就靜態生成，不會中途重排。
- 外部世界的變化，只會在執行階段影響調整（例如下雨延誤交通）。

  
*圖：Planning Pattern 流程圖，展現「先規劃、再執行」的典型模式。計劃一次生成，不會在過程中重排，只能在執行階段根據情況微調。*

---

## Demo 1：AI 先規劃，再執行 (Plan-and-Execute)

以下用「一日三景點行程」來展示最小可行範例。

> 規劃今天從上午 9 點出發，參觀維也納三個景點：  
> 美泉宮、美景宮、聖史蒂芬大教堂。  
> 條件：
> 
> - 每個景點至少 2 小時
> - 景點之間交通 30 分鐘
> - 必須在 18:00 前結束

```python
import google.generativeai as genai
import json
from datetime import datetime, timedelta

# ---- 初始化 LLM ----
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

# Step 1: 請 AI 生成計劃（要求 JSON 格式，含 place）
query = """
請規劃今天從上午 9 點出發，參觀維也納三個景點：
美泉宮、美景宮、聖史蒂芬大教堂。
條件：
- 每個景點至少 2 小時
- 景點之間交通 30 分鐘
- 必須在 18:00 前結束
請只輸出 JSON 格式，每個步驟包含：
title, place, minutes, indoor (是否室內, true/false)。
"""
response = llm.generate_content(query).text
print("AI 原始輸出：", response)

# Step 2: 清理 JSON（避免 LLM 輸出帶有 \`\`\`json ... \`\`\`）
raw = response.strip()
if raw.startswith("\`\`\`"):
    raw = raw.split("\`\`\`")[1]
    raw = raw.replace("json", "", 1).strip()

plan = json.loads(raw)

# Step 3: Tool (模擬天氣 API)
def get_weather(city: str):
    return "rain"  # 假設今天下雨

# Step 4: 執行計劃
weather = get_weather("Vienna")
print("\n今日天氣：", weather)

time = datetime.strptime("09:00", "%H:%M")
end = datetime.strptime("18:00", "%H:%M")

for task in plan:
    duration = task["minutes"]
    if weather == "rain" and "前往" in task["title"]:
        duration = int(duration * 1.5)  # 下雨天交通延長 1.5 倍
        task["title"] += "（因下雨延誤）"

    finish = time + timedelta(minutes=duration)
    print(f"{time.strftime('%H:%M')}–{finish.strftime('%H:%M')} {task['title']}")
    time = finish

print("行程可行！" if time <= end else "行程超時！")
```

---

### 輸出結果

![Demo 1 輸出結果](https://ithelp.ithome.com.tw/upload/images/20250921/20165544gUvK05bOTC.png)  
*圖：Demo 1 執行結果。行程表完全依照 AI 事先產生的靜態計劃執行，下雨只影響交通時間，但仍能在限制時間內完成行程。*

你可能會注意到：這份計劃裡 **沒有「查天氣」這個步驟** 。這不是錯誤，而是 **Planning Pattern 的特性** 。

在 **Plan-and-Execute** 模式中：

- **計劃階段** ：AI 先生成一份完整的「靜態藍圖」（行程），不會把外部資訊考慮進來。
- **執行階段** ：系統在實際執行時，才會透過工具（例如天氣 API）檢查環境，並在必要時調整（例如下雨天戶外交通時間就會延誤）。

換句話說，計劃本身是靜態的，外部世界的變化只會在執行時影響最終結果。

這也是為什麼在後續的章節，我們會介紹 ReAct，讓 AI 不僅能規劃，還能邊想邊查。

---

## Demo 2：遇到突發狀況，調整既有計劃

假設今天臨時發現 **美景宮休館** ，在 **Planning Pattern** 下，AI 不會自動幫你「重排新計劃」，而是系統在執行階段偵測到問題後， **刪掉無法執行的步驟** ，然後繼續照原本的計劃執行下去。

```python
import google.generativeai as genai
import json
from datetime import datetime, timedelta

# ---- 初始化 LLM ----
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

# Step 1: 請 AI 生成計劃（包含 place 欄位）
query = """
請規劃今天從上午 9 點出發，參觀維也納三個景點：
美泉宮、美景宮、聖史蒂芬大教堂。
條件：
- 每個景點至少 2 小時
- 景點之間交通 30 分鐘
- 必須在 18:00 前結束
請只輸出 JSON 格式，每個步驟包含：
title, place, minutes, indoor (是否室內, true/false)。
"""
response = llm.generate_content(query).text

# Step 2: 清理 JSON（避免 LLM 輸出帶有 \`\`\`json ... \`\`\`）
raw = response.strip()
if raw.startswith("\`\`\`"):
    raw = raw.split("\`\`\`")[1]
    raw = raw.replace("json", "", 1).strip()

plan = json.loads(raw)

# Step 3: Tool (模擬天氣 API)
def get_weather(city: str):
    return "rain"  # 假設今天下雨

# Step 4: Tool (檢查景點是否開放)
def check_open(place: str):
    closed_places = ["美景宮"]  # 假設美景宮休館
    return place not in closed_places

# Step 5: 執行計劃
weather = get_weather("Vienna")
print("\n今日天氣：", weather)

time = datetime.strptime("09:00", "%H:%M")
end = datetime.strptime("18:00", "%H:%M")

for task in plan:
    # 如果是參觀景點，先檢查是否開放
    if "參觀" in task["title"]:
        if not check_open(task["place"]):
            print(f"{task['place']} 今日休館，跳過此步驟")
            continue

    duration = task["minutes"]

    # 下雨 → 戶外交通延長 1.5 倍
    if weather == "rain" and "前往" in task["title"]:
        duration = int(duration * 1.5)  # 交通延長 1.5 倍
        task["title"] += "（因下雨延誤）"

    finish = time + timedelta(minutes=duration)
    print(f"{time.strftime('%H:%M')}–{finish.strftime('%H:%M')} {task['title']}")
    time = finish

print("行程可行！" if time <= end else "行程超時！")
```

---

### 輸出結果

![Demo 2 輸出結果](https://ithelp.ithome.com.tw/upload/images/20250921/20165544FrcgEXG1Bu.png)  
*圖：Demo 2 執行結果。遇到美景宮休館時，AI 僅刪除該步驟，沒有重新安排行程。*

可以看到，在 **Planning Pattern** 下：

- AI 不會主動幫你「想新的景點替代方案」。
- 它只是根據既有的靜態計劃， **把無法執行的部分刪掉** ，再繼續完成剩下的步驟。

這就是 **Plan-and-Execute 的限制** ：計劃雖然清楚，但缺乏靈活性。

如果要讓 AI 能夠「臨時改去其他景點」或「重新規劃完整路線」，就需要更進階的方法，例如 **ReAct** 或 **Multi-Agent** 。

---

## 小結

- **Planning Pattern** 的核心是 **Plan → Execute** ，計劃與執行分開，計劃靜態而清晰。
- 它適合明確、可預期的任務（例如流程自動化、排程）。
- 但遇到突發狀況，AI 只能刪掉步驟， **無法自動替代或重排** ，彈性不足。
- 後續會看到的 **ReAct** ，則能讓 AI 邊想邊查，具備更即時的應變能力。

---

![夜晚的卡爾教堂](https://ithelp.ithome.com.tw/upload/images/20250921/20165544dsCPJKo1xr.jpg)  
*圖：維也納卡爾教堂（Karlskirche）。夜色下的巴洛克建築展現嚴謹的對稱與秩序，如同 Planning Pattern 所強調的「先規劃、再執行」：AI 不只是行動，更需要結構化的安排，才能讓複雜任務井然有序。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10384461)

[

Day 6｜行動是關鍵：Tool Use 讓 LLM 突破回答的侷限

](https://ithelp.ithome.com.tw/articles/10384461)[下一篇](https://ithelp.ithome.com.tw/articles/10385814)

[

Day 8｜邊想邊做更靈活：ReAct 讓 LLM 學會臨機應變

](https://ithelp.ithome.com.tw/articles/10385814)

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