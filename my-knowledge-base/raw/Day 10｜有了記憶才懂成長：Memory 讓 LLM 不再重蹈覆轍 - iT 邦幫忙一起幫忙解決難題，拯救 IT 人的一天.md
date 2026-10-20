---
title: "Day 10｜有了記憶才懂成長：Memory 讓 LLM 不再重蹈覆轍 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10387323"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：從反思到記憶 在 昨天 (Day 9)，我們看到 Reflection Pattern 如何讓 AI 事後檢討與修正。AI 能反思「景點時間太長 → 超時」，並提出改進方案，讓行程成功在 18:..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 10

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 10 篇

## Day 10｜有了記憶才懂成長：Memory 讓 LLM 不再重蹈覆轍

- 分享至

## 前言：從反思到記憶

在 [昨天 (Day 9)](https://ithelp.ithome.com.tw/articles/10386571) ，我們看到 **Reflection Pattern** 如何讓 AI 事後檢討與修正。  
AI 能反思「景點時間太長 → 超時」，並提出改進方案，讓行程成功在 18:00 前結束。

但是如果第二天我們再請 AI 排類似的行程，它是否還會重蹈覆轍？  
光有 Reflection，AI 只能「當下修正」，但下次仍可能忘記之前的錯誤。

這時候，我們需要 **Memory（記憶）** —— 讓經驗能被保存，避免重蹈覆轍。

---

## Memory（記憶）的核心概念

**Memory** 讓 AI 不只會臨時反思，還能「記住經驗」，未來遇到相似情境時自動套用。

### 核心概念：

1. **存放 (Write)** —— 把反思或使用者偏好記錄下來
2. **調用 (Retrieve)** —— 當再次遇到類似任務時，自動帶入相關記憶
3. **應用 (Apply)** —— 基於記憶直接調整輸出，避免重複錯誤

  
*圖：Memory 的核心循環 —— Write → Retrieve → Apply → Result → Write*

使用者的輸入或任務結果會被寫入 (Write)，存放到記憶庫。下一次遇到相似任務時，會先檢索 (Retrieve) 相關資訊並應用 (Apply)，讓輸出更貼近需求。結果再回饋到記憶中，形成持續成長的循環。

若再搭配昨天的 Reflection，AI 在輸出後會先檢討，再把檢討結論更新進記憶，讓循環更完整。

而在不同情境下，這些記憶可以以不同型態存在，常見的有以下三種：

### 記憶的種類

1. **短期記憶（Short-term Memory）** —— 記得上下文對話，會話結束即消失
2. **長期記憶（Long-term Memory）** —— 保存跨多次互動的經驗，形成「知識庫」
3. **反思記憶（Reflection Memory）** —— 將 AI 的檢討結果永久保存，下次直接調用

這三種記憶往往是同時存在並互相補充的：短期記憶處理上下文，長期記憶累積知識，而反思記憶則留存經驗教訓，三者結合才能讓 AI 真正展現『成長』的感覺。

---

## Demo 1：避免重蹈覆轍（超時問題）

延續 Day 9 案例，我們刻意給了：

- **美泉宮 (Schönbrunn Palace)** 5 小時
- **聖史蒂芬大教堂 (St. Stephen's Cathedral)** 5 小時
- 午餐 2 小時 + 交通各 30 分鐘

總長度高達 **13.5 小時** ，導致行程要到 **22:30** 才能結束 → 超時失敗。

### 沒有 Memory → 每次都犯同樣的錯

```json
[
  {"place": "美泉宮", "minutes": 300},
  {"place": "交通（美泉宮→餐廳）", "minutes": 30},
  {"place": "午餐", "minutes": 120},
  {"place": "交通（餐廳→聖史蒂芬大教堂）", "minutes": 30},
  {"place": "聖史蒂芬大教堂", "minutes": 300}
]
```

模擬結果：22:30 才結束 → 超時。

### 有了 Memory → 套用 Day 9 的反思記錄

昨天 Reflection 存下的記憶（延續 Day 9 檢討結論）：

```markdown
「美泉宮建議 4 小時、聖史蒂芬大教堂 1.75 小時，避免超時」
```

今天規劃時直接套用：

```json
[
  {"place": "美泉宮", "minutes": 240},
  {"place": "交通（美泉宮→餐廳）", "minutes": 30},
  {"place": "午餐", "minutes": 90},
  {"place": "交通（餐廳→聖史蒂芬大教堂）", "minutes": 30},
  {"place": "聖史蒂芬大教堂", "minutes": 105}
]
```
```makefile
輸出結果：**17:45** 結束 → 剛好在 18:00 前完成。
```

在這個案例中，若沒有記憶，AI 每次都會按照「5 小時景點 + 2 小時午餐」的原始設定，導致行程持續超時。  
即使 AI 具備 Reflection 能力，可以在當下檢討並修正，但如果缺乏 Memory，它下一次仍會重蹈覆轍。

有了 **Memory** 之後，AI 能記住昨天 Reflection 的結論：

- 「美泉宮 5 小時太長 → 建議縮短為 4 小時」
- 「聖史蒂芬大教堂建議縮短為 1.75 小時」

因此在新的行程規劃裡，自動套用這些調整，最終行程就能在 18:00 前準時結束。

這說明了 **Reflection 解決「事後修正」，而 Memory 則確保「下次不再犯」** 。  
兩者結合，才讓 AI 能逐步展現「真正會學習」的能力。

---

## Demo 2：尊重使用者偏好（不吃牛肉）

旅行不只要考慮時間，還要顧及個人偏好。這裡我們示範 AI 如何透過 **記憶 + LLM** ，記住使用者飲食習慣，並在推薦時自動套用。

### Step 1：使用者輸入 → AI 寫入記憶

```python
# ---- 記憶存放結構 ----
memory_store = {"diet": {}, "cuisine_pref": []}

def update_memory(user_input: str):
    if "不吃牛肉" in user_input:
        memory_store["diet"]["beef"] = False
    if "豬排" in user_input and "schnitzel" not in memory_store["cuisine_pref"]:
        memory_store["cuisine_pref"].append("schnitzel")

# 使用者輸入
user_input = "我不吃牛肉，但喜歡維也納豬排"
update_memory(user_input)

print("使用者記憶：", memory_store)
```

輸出：

```json
{
  "diet": {"beef": false},
  "cuisine_pref": ["schnitzel"]
}
```

### Step 2：候選餐廳清單

```python
candidates = [
    {"name": "市中心牛排館", "tags": ["beef", "steak"]},
    {"name": "市中心壽司店", "tags": ["japanese", "sushi"]},
    {"name": "市中心維也納豬排餐廳", "tags": ["schnitzel", "austrian"]}
]
```

### Step 3：呼叫 LLM → 根據記憶篩選餐廳

```python
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

prompt = f"""
使用者的飲食習慣：
{memory_store}

候選餐廳：
{candidates}

請依照使用者的飲食限制與偏好，選擇最合適的一間餐廳，
並只輸出餐廳的名稱。
"""

response = llm.generate_content(prompt)
print("AI 建議的餐廳：", response.text.strip())
```

### 輸出結果

```python
使用者記憶： {'diet': {'beef': False}, 'cuisine_pref': ['schnitzel']}
AI 建議的餐廳：市中心維也納豬排餐廳
```

### Step 4：整合到完整行程模擬

```makefile
=== 結合記憶後的完整行程 ===
09:00–09:30 前往美泉宮
09:30–13:30 美泉宮
13:30–14:00 前往餐廳
14:00–15:30 午餐吃市中心維也納豬排餐廳
15:30–16:00 前往聖史蒂芬大教堂
16:00–17:45 聖史蒂芬大教堂
```

在這個案例中，若沒有記憶，AI 很可能會安排「牛排館」，導致踩雷。  
但有了 **Memory** 之後，AI 能自動避開「牛肉」選項，並依照偏好推薦在地代表性的 **維也納豬排（Wiener Schnitzel）** 餐廳，例如 Figlmüller。

這不只是單純的關鍵字比對，而是 **結合記憶與 LLM 推理** 的能力：

- Memory 記住「不吃牛肉」與「喜歡豬排」的偏好
- LLM 根據餐廳候選清單做推理與篩選
- 最後輸出更貼近使用者需求的選擇

換句話說，AI 已經不只是單次對話的助手，而是能 **累積使用者偏好，並在未來互動中主動應用** 的「長期隊友」。

---

## 小結：Memory 的價值

在今天的練習中，我們看到 Memory 如何同時處理「壞的」與「好的」兩種經驗：

- **避免錯誤** → 透過記住昨天的反思，下次自動調整景點時間，避免再犯超時問題。
- **保留偏好** → 透過記住使用者「不吃牛肉、喜歡維也納豬排」的習慣，未來自動避開不合適的餐廳，並優先推薦更貼近需求的選項。

這凸顯了 Memory 的雙重價值：

- 它不只是幫助 AI **避免重蹈覆轍** （負面經驗），
- 也能讓 AI **強化並延續好的行為** （正面偏好）。

對照來看：

- **Reflection** → 幫助 AI 事後檢討與修正
- **Memory** → 幫助 AI 記住錯誤的教訓，也保留好的偏好
- **Reflection + Memory** → 讓 AI 不僅「知道自己錯了」，還能「延續做對的」，真正展現出「學習與成長」

有了這樣的能力，AI 就能從「一次性規劃的助手」逐步升級為「長期的可靠隊友」——會學習、會成長，越用越聰明。

---

![白水公寓](https://ithelp.ithome.com.tw/upload/images/20250924/20165544P6mVNGud4W.jpg)  
*圖：維也納百水公寓（Hundertwasserhaus）。這棟色彩斑斕、造型自由的建築，沒有一扇窗戶完全相同，每個細節都像被保存下來的片段記憶。綠樹從屋頂和牆面生長，讓舊有的空間與新的生命交織。就像 Memory 的作用：AI 把不同的經驗累積、保存，再在未來拼湊成更完整的理解與行動。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10386571)

[

Day 9｜做中學才能進化：Reflection 讓 LLM 從經驗中改進

](https://ithelp.ithome.com.tw/articles/10386571)[下一篇](https://ithelp.ithome.com.tw/articles/10387823)

[

Day 11｜合奏的樂曲更精彩：Multi-Agent（統籌型）讓 LLM 學會分工合作

](https://ithelp.ithome.com.tw/articles/10387823)

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