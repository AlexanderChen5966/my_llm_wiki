---
title: "Day 11｜合奏的樂曲更精彩：Multi-Agent（統籌型）讓 LLM 學會分工合作 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10387823"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：一個 AI 不夠用的時候 在前幾天（CoT → RAG → Tool Use → Planning → ReAct → Reflection → Memory），我們讓單一 AI 擁有「思考、查..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 11

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 11 篇

## Day 11｜合奏的樂曲更精彩：Multi-Agent（統籌型）讓 LLM 學會分工合作

- 分享至

## 前言：一個 AI 不夠用的時候

在前幾天（CoT → RAG → Tool Use → Planning → ReAct → Reflection → Memory），我們讓單一 AI 擁有「思考、查資料、用工具、規劃、反思與記憶」等能力。  
看起來已經很強大，但在現實世界的任務中，一個 AI 往往還是會捉襟見肘。

例如：要規劃一趟維也納旅行，不只要考慮 **景點順序** ，還要顧及 **交通時間** 、 **餐廳選擇** 、 **個人偏好** （不吃牛肉、喜歡維也納豬排）與 **結束時間限制** 。  
如果全部交給單一 AI，容易顧此失彼。

這時候，就需要 **Multi-Agent** 。  
就像交響樂團一樣，樂手各司其職、彼此呼應，最後由指揮統籌整合 — 合奏的樂曲更精彩，正如音樂之都維也納所孕育的和諧篇章。

---

## Multi-Agent Design Pattern

### 為什麼需要 Multi-Agent？

如果一個 AI 要同時處理「規劃景點、挑餐廳、估交通、顧好使用者偏好」，它的 Prompt 會越來越龐雜，容易出錯。  
**Multi-Agent Design Pattern** 的精神，就是把任務拆分，讓 **多個角色各司其職** ，再透過協作把結果整合。這樣更容易理解、維護，也更貼近人類團隊合作。

---

### 三大核心元素

1. **分工（Specialization）**  
	每個 Agent 有明確的角色定位，例如：規劃師、交通顧問、美食顧問。  
	*好處：Prompt 精準、輸出專一。*
2. **協作（Collaboration）**  
	角色之間互相傳遞資訊、補足彼此盲點。  
	*好處：任務更完整，不會因單一角色的限制而失效。*
3. **統籌（Orchestration）**  
	最後需要一個「總召」來整合大家的意見，並檢查是否符合限制（像是必須在 18:00 前結束）。  
	*好處：結果一致、能符合全局條件。*

  
*圖：Multi-Agent Design Pattern 的三層結構 —— ① 分工（Specialization）：各角色各司其職；② 協作（Collaboration）：角色之間資訊交換，互補盲點；③ 統籌（Orchestration）：由 Coordinator 整合檢查，確保最終結果符合需求。*

這張圖呈現了完整的三層次流程：

- 使用者需求 先交給 分工角色（Planner、Foodie、Transport）
- 他們彼此之間會有 協作交換，互相補充資訊
- 最後由 Coordinator 統籌整合，輸出完整可行的結果

### 常見模式

- **Supervisor（統籌型）** ：一個主導 Agent 分派任務，其他角色各自回報。→ *最直觀也最常用。*
- **Pipeline（串接型）** ：像生產線一樣，一個 Agent 的結果傳給下一個。→ *適合有固定流程的任務。*
- **Group Chat（討論型）** ：多個 Agent 輪流對話，最後收斂共識。→ *適合創意發想或需要辯論的場景。*

> 在今天的 Demo，我們採用 **Supervisor** 模式：Planner、Foodie、Transport 各自輸出，最後由 Coordinator 統籌整合。

---

### 對照今天的旅行案例

- **Planner** ：規劃景點 → 分工
- **Foodie** ：根據記憶挑餐廳 → 分工 + 使用記憶
- **Transport** ：估算交通 → 分工
- **Coordinator** ：統合時間表、檢查超時 → 統籌

這就是最簡單的 **Multi-Agent Design Pattern** ： **分工 → 協作 → 統籌** 。

這種設計模式不只適用在旅遊行程，未來你會看到它在 軟體開發、商業決策、研究助理 等場景同樣發揮威力，讓 AI 團隊真正成為我們的協作夥伴。

---

## Naive Demo：維也納旅行

為了簡單掌握 Multi-Agent 的精神，我們做一個 **Naive Demo** ：  
由四個代理人分工合作完成「兩日行程規劃」。

- **Planner Agent** ：規劃每天上午與下午的景點
- **Foodie Agent** ：根據 **Memory** （不吃牛肉、偏好維也納豬排）挑餐廳
- **Transport Agent** ：呼叫 **Tool Use** 估算交通時間
- **Coordinator Agent** ：整合行程，檢查是否超時

---

### 系統架構

![系統架構](https://ithelp.ithome.com.tw/upload/images/20250925/20165544NFGMwNL3lj.png)  
*圖：Multi-Agent 系統架構。Planner 負責行程規劃（分工）、Foodie 依據 Memory 選餐廳並回饋（協作）、Transport 呼叫工具取得交通時間（協作），最後 Coordinator 統籌整合並檢查超時（統籌）。完整展現了「分工 → 協作 → 統籌」的流程。*

---

### 程式碼實作

以下程式先用最簡單的方式幫助大家理解這四個代理如何協作：Planner 規劃景點、Foodie 挑餐廳、Transport 呼叫工具估算交通、Coordinator 統籌檢查是否超時。  
之後的主題還會介紹好用的技術框架如（LangChain/LangGraph）可以實作。

```shell
pip install google-generativeai python-dateutil
```
```python
import os, json, random
from datetime import datetime, timedelta
import google.generativeai as genai
import re, json

def safe_json_loads(text, fallback=None):
    """
    嘗試解析 JSON，會自動移除 \`\`\`json ... \`\`\` code fence。
    如果失敗，就回傳 fallback。
    """
    if not text:
        return fallback
    # 移除 Markdown code block 標記
    clean = re.sub(r"^\`\`\`json\s*|\s*\`\`\`$", "", text.strip(), flags=re.IGNORECASE|re.MULTILINE)
    try:
        return json.loads(clean)
    except:
        return fallback

# ===== 0) 初始化 =====
os.environ["GEMINI_API_KEY"] = "YOUR_API_KEY"
genai.configure(api_key=os.environ["GEMINI_API_KEY"])
LLM = genai.GenerativeModel("gemini-2.5-flash")

# ===== 1) Memory (使用者偏好) =====
MEMORY = {
    "diet": "不吃牛肉",
    "pref": "維也納豬排"
}

# ===== 2) Tool Use 模擬 =====
def travel_time_tool(from_place, to_place):
    """
    模擬呼叫外部 API 查交通時間。
    這裡用隨機數 20–40 分鐘。
    """
    return random.randint(20, 40)

# ===== 3) Agents 定義 =====
def planner_agent():
    prompt = """
    請規劃維也納 2 日行程，每天上午與下午各安排一個景點，輸出 JSON 格式。
    範例：
    {
        "Day1": {"am": "美泉宮", "pm": "聖史蒂芬大教堂"},
        "Day2": {"am": "奧地利國家圖書館", "pm": "維也納歌劇院"}
    }
    """
    resp = LLM.generate_content(prompt)
    try:
        return safe_json_loads(resp.text)
    except:
        return None

def foodie_agent(plan):
    prompt = f"""根據以下行程，挑選午餐餐廳。
需符合條件：{MEMORY['diet']}，並偏好 {MEMORY['pref']}。
輸出 JSON 格式，
範例：{{"Day1": "Figlmüller（維也納豬排）", "Day2": "Gasthaus Pöschl"}}
每天一間餐廳。行程: {plan}"""
    resp = LLM.generate_content(prompt)
    try:
        return safe_json_loads(resp.text)
    except:
        return None

def transport_agent(plan, food):
    """
    Transport Agent：呼叫 travel_time_tool 取得所有需要的時間，
    再交給 LLM 整理成 {DayX: {am_to_lunch, lunch_to_pm}} 的格式。
    """
    # Step 1: 先收集原始資料
    raw_data = []
    for day, schedule in plan.items():
        am_to_lunch = travel_time_tool(schedule["am"], food[day])
        lunch_to_pm = travel_time_tool(food[day], schedule["pm"])
        raw_data.append({
            "day": day,
            "am": schedule["am"],
            "lunch": food[day],
            "pm": schedule["pm"],
            "am_to_lunch": am_to_lunch,
            "lunch_to_pm": lunch_to_pm
        })

    # Step 2: 請 LLM 整理成結構化 JSON
    prompt = f"""
    以下是交通估算的原始資料，請整理成 JSON 格式：
    {raw_data}

    格式範例：
    {{
      "Day1": {{"am_to_lunch": 25, "lunch_to_pm": 30}},
      "Day2": {{"am_to_lunch": 22, "lunch_to_pm": 35}}
    }}
    """
    resp = LLM.generate_content(prompt)
    return safe_json_loads(resp.text, {})

def coordinator_agent(plan, food, transit):
    """
    Coordinator Agent：統籌行程，檢查是否超時 (09:00 ~ 18:00)
    """
    day_start = datetime.strptime("09:00", "%H:%M")
    end_limit = datetime.strptime("18:00", "%H:%M")

    result = {}
    for day, schedule in plan.items():
        cur = day_start
        steps = []

        # 上午景點
        steps.append(f"{cur.strftime('%H:%M')}–'%H:%M'
 {schedule['am']}")

        # 上午 → 午餐
        cur += timedelta(minutes=transit[day]["am_to_lunch"])
        steps.append(f"{cur.strftime('%H:%M')} 午餐：{food[day]} (90 分鐘)")
        cur += timedelta(minutes=90)

        # 午餐 → 下午
        cur += timedelta(minutes=transit[day]["lunch_to_pm"])
        steps.append(f"{cur.strftime('%H:%M')}–'%H:%M'
 {schedule['pm']}")

        feasible = "行程可行！" if cur <= end_limit else "行程超時！"
        result[day] = {"timeline": steps, "status": feasible}

    return result

# ===== 4) 執行 Demo =====
if __name__ == "__main__":
    print("=== Multi-Agent + Tool Use Demo ===")

    plan = planner_agent()
    print("Planner:", plan)

    food = foodie_agent(plan)
    print("Foodie:", food)

    transit = transport_agent(plan, food)
    print("Transport (Tool Use):", transit)

    final = coordinator_agent(plan, food, transit)
    print("\n=== 最終統籌結果 ===")
    for day, info in final.items():
        print(f"\n{day}")
        for s in info["timeline"]:
            print(" ", s)
        print(" ", info["status"])
```

---

### 執行結果

![執行結果](https://ithelp.ithome.com.tw/upload/images/20250925/20165544UEMeUVtwtX.png)  
*圖：Multi-Agent + Tool Use Demo 的實際輸出結果。四個代理各司其職：Planner 負責規劃景點（分工）、Foodie 依據 Memory 選擇符合偏好的餐廳（協作）、Transport 呼叫工具取得交通時間（協作），最後 Coordinator 統籌檢查是否超時（統籌）。結果成功輸出可行的完整行程。*

這個範例完整展示了這四個 Agent 的分工合作：

- **Planner** ：安排行程（上午 / 下午景點）
- **Foodie** ：依據 **Memory** （不吃牛肉、偏好維也納豬排），挑選合適餐廳
- **Transport** ：呼叫 **Tool Use** ，估算移動時間
- **Coordinator** ：整合時間軸，檢查是否超過 18:00

結果：

- **Day 1** ：美泉宮 → 午餐 *Figlmüller（維也納豬排）* → 霍夫堡宮 ✅
- **Day 2** ：聖史蒂芬大教堂 → 午餐 *Gasthaus Pöschl* → 藝術史博物館 ✅

![Figlmüller 維也納豬排](https://ithelp.ithome.com.tw/upload/images/20250926/20165544ou4bp6nus5.png)  
*圖： [Figlmüller](https://www.figlmueller.at/en/) 的維也納豬排（Wiener Schnitzel）。這塊金黃酥脆的豬排大到覆滿整個餐盤，第一眼就讓人驚艷。薄薄的麵衣、樸實的調味，卻意外耐吃，正是維也納經典美食之一；而 Figlmüller 也正是維也納豬排的招牌百年老店。（攝影：作者自攝）*

---

## 小結：Multi-Agent 的價值

今天的範例展示了從 **單一 AI** 到 **多代理協作** 的演進：

- **分工（Specialization → 更專精）** ：每個 Agent 的 Prompt 可以針對專業角色調整
- **協作（Collaboration → 更高效）** ：角色彼此交換資訊、互補盲點，避免單一模型「全包」而出錯
- **統籌（Orchestration → 更真實）** ：由統籌 Agent 整合並檢查條件，模擬真實團隊合作情境

而且這個 Demo 不只是 Multi-Agent，本身還用到了 **Day 10 的記憶** ，讓美食顧問能避開「牛肉」並推薦「維也納豬排」。

這代表 AI 不只是「多角色」，還能「共享經驗」。

到這裡，我們已經讓 AI 進化成能夠 **分工合作的隊伍** 。

正如維也納交響樂的精神：合奏更能交織出真正的和諧與力量。

---

![卡爾教堂維瓦爾第《四季》音樂會](https://ithelp.ithome.com.tw/upload/images/20250925/201655440oHFXPmdJx.jpg)  
*圖：維也納卡爾教堂（Karlskirche）：韋瓦第《四季》音樂會（Vivaldi *The Four Seasons* Concert）。巴洛克華麗的祭壇與穹頂壁畫下，在音樂之都維也納，樂手們分工演奏、彼此呼應，最後在指揮的統籌下交織出和諧樂章。這正是 Multi-Agent 的精神：各角色專精分工、互相協作，最終由統籌整合，讓結果比單打獨鬥更完整、更精彩。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10387323)

[

Day 10｜有了記憶才懂成長：Memory 讓 LLM 不再重蹈覆轍

](https://ithelp.ithome.com.tw/articles/10387323)[下一篇](https://ithelp.ithome.com.tw/articles/10388549)

[

Day 12｜思想碰撞才有火花：Multi-Agent（討論型）讓 LLM 學會共識決策

](https://ithelp.ithome.com.tw/articles/10388549)

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