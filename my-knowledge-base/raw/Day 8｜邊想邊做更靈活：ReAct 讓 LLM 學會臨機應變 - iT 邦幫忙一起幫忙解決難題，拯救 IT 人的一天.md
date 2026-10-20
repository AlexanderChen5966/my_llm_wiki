---
title: "Day 8｜邊想邊做更靈活：ReAct 讓 LLM 學會臨機應變 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10385814"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：從「先規劃」到「邊查邊做」 昨天 (Day 7) 我們介紹了 Planning Pattern：AI 會先生成一份完整的計劃，再依序執行。這種模式雖然條理清楚，但有一個限制：計劃在一開始就固定，..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 8

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 8 篇

## Day 8｜邊想邊做更靈活：ReAct 讓 LLM 學會臨機應變

- 分享至

## 前言：從「先規劃」到「邊查邊做」

[昨天 (Day 7)](https://ithelp.ithome.com.tw/articles/10385063) 我們介紹了 **Planning Pattern** ：AI 會先生成一份完整的計劃，再依序執行。  
這種模式雖然條理清楚，但有一個限制： **計劃在一開始就固定，無法靈活應變** 。  
例如景點臨時休館，AI 只能刪掉該步驟，而無法動態調整或找替代方案。

昨天的 Planning 屬於「先想完再做」；而今天要介紹的 **ReAct** ，則是「邊做邊想」。這樣的對比，正好補足 Planning 缺乏的彈性。

這就是今天的主角 **ReAct Pattern** ： **Reason + Act** 。

---

## ReAct Pattern 是什麼？

### 核心概念

ReAct 的精神是 **邊推理 (Reason)，邊行動 (Act)** 。

它不像 Planning Pattern 那樣一次性產出完整的靜態計劃（Planning），ReAct 會在每一步驟中交替進行「思考」與「行動」，直到找到最終答案。

這個循環通常被簡稱為 **TAO 流程** ：

1. **Thought（思考）**
	- AI 在自然語言中描述自己的推理，例如：「因為今天下雨，所以要先確認哪些景點是室內的。」
		- 這不是給使用者看的答案，而是模型的「內部思考」。
2. **Action（行動）**
	- AI 決定要執行什麼工具，並輸出結構化的動作，例如：「查詢開放狀態(美景宮)」。
		- 這時候由系統接手，呼叫相對應的 API 或函式。
3. **Observation（觀察）**
	- 工具回傳結果，例如：「美景宮今日休館」。
		- AI 會把這個資訊納入後續的推理，進一步決定下一步要怎麼做。
4. **Final Answer（最終答案）**
	- 當 AI 認為資訊足夠時，會輸出最後的回答，通常是給使用者的乾淨結果。

  
*圖：ReAct Pattern 流程圖。AI 交替進行「推理」與「行動」，直到得到最終答案。*

### 特色

- 思考過程是逐步展開的。
- 工具使用被納入推理流程。
- 能即時依據外部資訊動態調整。

### 舉例說明：一日行程規劃

1. **Thought** ：「我需要先查天氣，因為下雨可能影響交通。」
2. **Action** ：「查天氣(Vienna)」
3. **Observation** ：「今天維也納下雨」
4. **Thought** ：「因為下雨，所以要先確認美景宮是否開放。」
5. **Action** ：「查詢開放狀態(美景宮)」
6. **Observation** ：「美景宮今日休館」
7. **Final Answer** ：只安排美泉宮與聖史蒂芬大教堂的行程。

### 相關研究

Yao, et al. [*ReAct: Synergizing reasoning and acting in language models.*](https://iclr.cc/virtual/2023/poster/11003) (ICLR 2023)

> 這篇是 ReAct 模式的原始論文，重點概念就是將 **推理 (Reasoning)** 與 **行動 (Acting)** 融合在同一流程中，透過交替的 Thought → Action → Observation 循環，讓語言模型能夠一邊思考、一邊互動，最終得到更靈活與可靠的解答。

---

## Demo：AI 邊想邊查 (ReAct)

我們延續昨天的例子：規劃一日三景點（美泉宮、美景宮、聖史蒂芬大教堂）。  
不同的是，這次 AI 不會一次性輸出完整行程，而是透過 **ReAct 過程** 逐步完成。

```python
import google.generativeai as genai
import json
import re
from datetime import datetime, timedelta

# ---- 初始化 LLM ----
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

# ---- 模擬工具 ----
def get_weather(city: str):
    return "今天維也納是下雨天"  # 假設今天下雨

def check_open(place: str):
    closed_places = ["美景宮"]  # 假設美景宮休館
    return f"{place} 正常開放" if place not in closed_places else f"{place} 今日休館"

# ---- 工具路由表 ----
TOOLS = {
    "查天氣": get_weather,
    "查詢開放狀態": check_open,
}

# ---- ReAct 循環 ----
def react_loop():
    prompt = """你是一個旅行規劃助理，使用 ReAct 模式回答問題，思考請用繁體中文。
格式必須包含以下幾種：
Thought: 你的推理
Action: 你要執行的工具（格式：查天氣(city) 或 查詢開放狀態(place)）
Observation: 工具回傳的結果
Final Answer: 請用 JSON 格式輸出最終行程，例如：
[
  {"place": "美泉宮", "minutes": 120},
  {"place": "聖史蒂芬大教堂", "minutes": 120}
]

現在任務：幫我規劃今天去美泉宮、美景宮、聖史蒂芬大教堂的一日行程。"""

    for step in range(8):  # 最多 8 回合
        response = llm.generate_content(prompt).text
        print(response)

        # ---- 如果是最終答案 ----
        if "Final Answer" in response:
            try:
                json_str = response.split("Final Answer:")[-1].strip()
                if json_str.startswith("\`\`\`"):
                    json_str = json_str.split("\`\`\`")[1]
                    json_str = json_str.replace("json", "", 1).strip()
                plan = json.loads(json_str)
                print("\n最終解析後的行程：", plan)
                return plan
            except Exception as e:
                print("\nJSON 解析失敗：", e)
                return []

        # ---- 嘗試解析 Action ----
        match = re.search(r"Action:\s*([^\(]+)\((.+)\)", response)
        if match:
            action, arg = match.groups()
            action = action.strip()
            arg = arg.strip()
            if action in TOOLS:
                print(f"\n>> 執行工具：{action}({arg})")
                obs = TOOLS[action](arg)
                print(f">> 工具回傳：{obs}\n")
                prompt += f"\nObservation: {obs}"
            else:
                print(f"\n>> 無效的工具動作：{action}")
                prompt += "\nObservation: 無效的動作"
        else:
            print("\n>> 無法解析動作")
            prompt += "\nObservation: 無法解析動作"

# ---- 執行行程模擬 ----
def simulate_schedule(plan, weather="rain"):
    print("\n=== 行程時間模擬 ===")
    time = datetime.strptime("09:00", "%H:%M")
    end = datetime.strptime("18:00", "%H:%M")

    for i, task in enumerate(plan):
        duration = task["minutes"]
        finish = time + timedelta(minutes=duration)
        print(f"{time.strftime('%H:%M')}–{finish.strftime('%H:%M')} 參觀 {task['place']}")
        time = finish

        if i < len(plan) - 1:  # 景點之間交通
            travel_time = int(30 * (1.5 if weather == "rain" else 1))
            finish = time + timedelta(minutes=travel_time)
            if weather == "rain":
                print(f"{time.strftime('%H:%M')}–{finish.strftime('%H:%M')} 前往下一景點（因下雨延誤）")
            else:
                print(f"{time.strftime('%H:%M')}–{finish.strftime('%H:%M')} 前往下一景點")
            time = finish

    print("行程可行！" if time <= end else "行程超時！")

# ---- 主程式 ----
if __name__ == "__main__":
    plan = react_loop()
    if plan:
        simulate_schedule(plan, weather="rain")
```

---

### 輸出結果

![ReAct Demo 輸出結果](https://ithelp.ithome.com.tw/upload/images/20250922/20165544npFLITVNKo.png)  
*圖：ReAct Demo 實際輸出結果。AI 先逐步推理 (Thought)，再選擇行動 (Action) 呼叫工具，取得觀察結果 (Observation)，並根據外部資訊不斷修正計劃。最後產生 Final Answer（JSON 格式的行程），並進一步模擬完整時間表。*

1. **Thought → Action → Observation 循環**
	- 藍色區塊顯示 AI 的 **Action** （工具呼叫），例如「查詢開放狀態(美景宮)」。
		- 黃色標記部分是 AI 的 **推理過程 (Thought)** ，像是「因為下雨所以要優先考慮室內景點」。
		- 工具回傳結果 (Observation) 會直接影響下一步的推理與決策。
2. **動態應變**
	- AI 發現美景宮休館後，能即時排除該景點。
		- 交通因下雨延誤，也會被納入規劃。
3. **最終輸出**
	- **Final Answer** ：乾淨的 JSON 格式行程資料。
		- **行程模擬表（紅框區）** ：轉換成具體的時間軸，直觀顯示每個景點停留與交通時間。

執行結果清楚展現 **ReAct 與 Planning 的差異** ：  
*Planning 是先給定靜態藍圖，遇到變數只能刪掉步驟；而 ReAct 則能邊查邊想，讓行程動態調整。*

---

## 對比：Planning vs ReAct

- **Planning Pattern** ：AI 一開始就產生靜態藍圖，後續只能刪減，無法重排。
- **ReAct Pattern** ：AI 在過程中可以查詢、驗證，並根據新資訊即時調整計劃。

![Planning vs ReAct](https://ithelp.ithome.com.tw/upload/images/20250922/20165544xg41MG1XsC.png)  
*圖：Planning 與 ReAct 的對比。前者靜態、清晰；後者動態、靈活。*

---

## 小結

- **ReAct** 把「推理」與「工具使用」結合，讓 AI 能邊想邊查。
- 它比 Planning 更靈活，能即時處理外部變化。
- 特別適合需要高度互動與即時性的任務，例如助理問答、動態規劃。

接著我們會進一步探討 **Reflection（反思）** ，讓 AI 不只是當下靈活應變，還能記取經驗、避免重蹈覆轍。

如果 **ReAct** 是讓 AI 能在當下隨情境調整，那麼下一步的 **Reflection** ，則是讓 AI 從過去 **學會改進** 。

---

![維也納國立歌劇院](https://ithelp.ithome.com.tw/upload/images/20250922/20165544ViMck2QawT.jpg)  
*圖：維也納國立歌劇院（Wiener Staatsoper）。巴洛克華麗的階梯與穹頂燈光，是電影《不可能的任務：失控國度》著名的歌劇院段落中，阿湯哥與女主角在此相會並臨機應變尋找出路的經典場景。舞台上的情境變化莫測，需要臨場反應與即時調度，如同 ReAct Pattern 強調的「邊想邊做」：AI 不僅能規劃，更能在情境中不斷調整，學會臨機應變。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10385063)

[

Day 7｜有計劃才有行動：Planning 讓 LLM 安排更周全

](https://ithelp.ithome.com.tw/articles/10385063)[下一篇](https://ithelp.ithome.com.tw/articles/10386571)

[

Day 9｜做中學才能進化：Reflection 讓 LLM 從經驗中改進

](https://ithelp.ithome.com.tw/articles/10386571)

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

[![.](https://itadstatic.ithome.com.tw/B9/1780653847_6a229f1744d1f.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9001)