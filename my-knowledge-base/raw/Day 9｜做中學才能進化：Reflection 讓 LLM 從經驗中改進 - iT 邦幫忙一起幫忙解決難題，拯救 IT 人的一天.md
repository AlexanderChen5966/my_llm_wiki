---
title: "Day 9｜做中學才能進化：Reflection 讓 LLM 從經驗中改進 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10386571"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：從「臨機應變」到「學會改進」 在 昨天 (Day 8)，我們介紹了 ReAct Pattern：AI 能邊思考邊行動，像個導遊一樣在旅程中隨機應變。例如旅行途中，臨時發現 美景宮休館，它就能刪掉..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 9

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 9 篇

## Day 9｜做中學才能進化：Reflection 讓 LLM 從經驗中改進

- 分享至

## 前言：從「臨機應變」到「學會改進」

在 [昨天 (Day 8)](https://ithelp.ithome.com.tw/articles/10385814) ，我們介紹了 **ReAct Pattern** ：AI 能邊思考邊行動，像個導遊一樣在旅程中隨機應變。  
例如旅行途中，臨時發現 **美景宮休館** ，它就能刪掉該行程、改排其他景點；下雨時，會自動延長交通時間。

這讓 AI 不再只是「一次性規劃」的 Planning，而是真正能隨情境調整。

然而 ReAct 的應變只停留在「當下」。任務結束後，AI 並不會回頭檢討：

- 是否繞了遠路？
- 是否少了備案？
- 下次遇到同樣情境，有沒有更好的處理方式？

這就像旅人每天出門都能隨機應變，但從不記錄經驗，隔天還是會掉進相同的坑。  
為了讓 AI 不只是「臨時解決」，更能「累積經驗、持續成長」，我們需要 **Reflection Pattern** 。

---

## Reflection Pattern 是什麼？

### 核心概念

**Reflection（反思）** 就是讓 AI 在完成任務後，回頭檢視自己的思考與結果，找出不足並提出改進建議。

通常包含三個步驟：

1. **Review（回顧）**  
	檢視剛剛的推理、行動與輸出。
2. **Critique（檢討）**  
	找出錯誤、疏漏或效率不佳的地方。
3. **Refine（改進）**  
	給出更好的答案，或把經驗記錄下來，留待未來重用。

  
*圖：Reflection 的三步驟流程 —— Review（回顧）、Critique（檢討）、Refine（改進）。AI 不僅完成任務，還能透過這個循環不斷優化輸出。*

---

### 範例（旅遊情境）

假設 AI 規劃了一份行程：

```json
[
  {"place": "美泉宮", "minutes": 120},
  {"place": "聖史蒂芬大教堂", "minutes": 120}
]
```

看似合理，但透過 **Reflection** ，AI 會檢討：

```markdown
Review: 行程包含兩個主要景點，各停留 2 小時。
Critique: 沒有計算交通時間，也缺乏下雨時的室內備案。
Refine: 應加入交通時間，並多提供一個博物館作為備選。
```

改進後的結果：

```json
[
  {"place": "美泉宮", "minutes": 120},
  {"place": "交通（美泉宮 → 聖史蒂芬大教堂）", "minutes": 45},
  {"place": "聖史蒂芬大教堂", "minutes": 120},
  {"place": "維也納博物館 (備選)", "minutes": 90}
]
```

差別很明顯：

- **ReAct** → 解決「當下問題」
- **Reflection** → 解決「下次更好」

---

## Demo：反思讓 AI 修正超時行程

以下的範例展示了 Reflection 的威力：

- **初始行程** → 依照要求排滿景點與午餐，但總時間超過 18:00。
- **模擬檢查** → 發現超時。
- **Reflection** → AI 自我檢討，並縮短時間、加入調整。
- **改進後行程** → 可行且更合理。

---

### 程式碼範例

```python
import google.generativeai as genai
import json
from datetime import datetime, timedelta

# ---- 初始化 LLM ----
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

# ---- 工具函式：清理 LLM 輸出的 JSON ----
def clean_json_str(raw: str) -> str:
    raw = raw.strip()
    if raw.startswith("\`\`\`"):
        raw = raw.split("\`\`\`")[1]
        raw = raw.replace("json", "", 1).strip()
    return raw

# ---- Step 1: 初始行程（模擬 ReAct 輸出，但會超時） ----
init_prompt = """
請幫我規劃今天從上午 9 點出發的行程，包含：
- 上午參觀美泉宮（Schönbrunn Palace）
- 下午參觀聖史蒂芬大教堂（St. Stephen's Cathedral）
- 午餐在市中心的餐廳用餐

條件：
- 每個景點建議 5 小時
- 午餐建議 2 小時
- 景點與餐廳之間交通各 30 分鐘
- 必須在 18:00 前結束

請只輸出 JSON 格式，欄位為 place, minutes。
"""

init_response = llm.generate_content(init_prompt).text
raw = clean_json_str(init_response)

try:
    plan = json.loads(raw)
except Exception as e:
    print("初始行程 JSON 解析失敗：", e)
    plan = []

print("=== 初始行程 ===")
print(json.dumps(plan, ensure_ascii=False, indent=2))

# ---- 行程模擬器 ----
def simulate_schedule(plan, title="行程模擬"):
    print(f"\n=== {title} ===")
    time = datetime.strptime("09:00", "%H:%M")
    end = datetime.strptime("18:00", "%H:%M")

    for task in plan:
        duration = task["minutes"]
        finish = time + timedelta(minutes=duration)
        print(f"{time.strftime('%H:%M')}–{finish.strftime('%H:%M')} {task['place']}")
        time = finish

    feasible = time <= end
    print("\n行程可行！" if feasible else "\n行程超時！")
    return feasible

# ---- Step 2: 模擬初始行程 ----
if plan:
    feasible = simulate_schedule(plan, "初始行程模擬")
    if not feasible:
        print("\n初始行程超時，進入 Reflection 檢討...\n")

# ---- Step 3: Reflection 讓 LLM 檢討與改進 ----
refined_plan = []
if plan:
    reflection_prompt = f"""
你剛完成以下一日行程規劃：
{plan}

規劃今天從上午 9 點出發的行程，包含：
  - 上午參觀美泉宮（Schönbrunn Palace）
  - 下午參觀聖史蒂芬大教堂（St. Stephen's Cathedral）
  - 午餐在市中心的餐廳用餐

任務原始條件：
- 每個景點建議 5 小時
- 午餐建議 2 小時
- 景點與餐廳之間交通各 30 分鐘
- 必須在 18:00 前結束

請進行 Reflection：
1. 回顧這份計劃的優點與缺點。
2. 找出可能的問題（例如是否超時，時間分配是否合理）。
3. 提供改進後的版本（JSON 格式，結構與原本相同）。
"""
    reflection_response = llm.generate_content(reflection_prompt).text
    print("=== Reflection Output ===")
    print(reflection_response)

    if "[" in reflection_response and "]" in reflection_response:
        try:
            refined_str = reflection_response.split("[", 1)[1]
            refined_str = "[" + refined_str.split("]", 1)[0] + "]"
            refined_str = clean_json_str(refined_str)
            refined_plan = json.loads(refined_str)
        except Exception as e:
            print("\n改進後行程 JSON 解析失敗：", e)

# ---- Step 4: 模擬改進後的行程 ----
if refined_plan:
    simulate_schedule(refined_plan, "改進後行程模擬")
```

---

### 輸出結果

#### 初始行程模擬超時：進入 Reflection

![初始行程模擬結果](https://ithelp.ithome.com.tw/upload/images/20250923/201655448nxBuPsdya.png)  
*圖：初始行程模擬結果。因為每個景點都分配了 5 小時，加上交通與午餐，共計 13.5 小時，最終到晚上 22:30 才能結束，明顯超過「18:00 前結束」的限制，觸發 Reflection 檢討。*

在初始行程模擬中，雖然 AI 正確規劃了美泉宮、聖史蒂芬大教堂與午餐，也包含交通，但因為我們 **刻意給了過長的景點時間（各 5 小時）** ，導致總長度高達 13.5 小時，最終要到晚上 22:30 才能結束，明顯超過「18:00 前結束」的條件，因此被判定為超時。這樣的結果正好說明了 LLM 雖然能生成看似完整的答案，但仍可能在約束條件下失敗。

#### Reflection 檢討修正：調整後行程可行

![經過 Reflection 檢討後的改進結果](https://ithelp.ithome.com.tw/upload/images/20250923/201655448lwQJO9rms.png)  
*圖：經過 Reflection 檢討後的改進結果。AI 主動縮短景點遊覽時間並調整交通分配，最終行程在 18:00 前結束，從「超時失敗」轉為「可行」。*

進入 Reflection 後，AI 主動檢討並提出改善：縮短美泉宮至 4 小時、聖史蒂芬大教堂至 1.75 小時，並微調午餐與交通時間。最終行程在 18:00 前剛好結束，從「失敗」轉為「可行」。這不只是單純的重算，而是透過 **回顧 → 分析 → 改進** 的過程，讓 AI 展現出學習與調整的能力。

---

## 小結：Reflection 的價值

- **ReAct → 當下靈活** （避免即時出錯）
- **Reflection → 事後學習** （避免重蹈覆轍）
- **ReAct + Reflection → 學習循環** （從經驗中持續改進）

在今天的 Demo 裡，我們刻意設計了「景點時間過長 → 導致超時」的問題。雖然初始行程看似完整，但不可行；透過 Reflection，AI 主動檢討並調整，最終行程成功在 18:00 前完成。這證明了 Reflection 不只是格式修飾，而是能夠提升 **可行性與可靠性** 。

這個能力不只用在旅遊情境：無論是程式 Debug、知識問答，或決策流程優化，Reflection 都能幫助 AI 在任務後自我檢討，持續進步。

這正是讓 AI 從「助手」進化為「隊友」的關鍵一步。

下一步，我們將探討 **Memory（記憶）** —— 讓 AI 不僅會反思，還能把經驗保存下來，未來遇到相似情境能直接調用，真正做到「越用越聰明」。

---

![維也納中央咖啡館](https://ithelp.ithome.com.tw/upload/images/20250924/20165544kmeqxcVczl.png)  
*圖：維也納中央咖啡館（Café Central）。 高聳拱頂下的燈火與人聲，營造出既熱鬧又沉思的氛圍。這裡曾是許多思想家與文人的聚會場所，他們在交流與對話中反思過去、構想未來。就像 Reflection Pattern 的精神：AI 在完成任務後，不僅要回顧經驗，更要提煉改進，才能持續成長。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10385814)

[

Day 8｜邊想邊做更靈活：ReAct 讓 LLM 學會臨機應變

](https://ithelp.ithome.com.tw/articles/10385814)[下一篇](https://ithelp.ithome.com.tw/articles/10387323)

[

Day 10｜有了記憶才懂成長：Memory 讓 LLM 不再重蹈覆轍

](https://ithelp.ithome.com.tw/articles/10387323)

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