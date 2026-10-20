---
title: "Day 12｜思想碰撞才有火花：Multi-Agent（討論型）讓 LLM 學會共識決策 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10388549"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：從統籌到討論 在 昨天 (Day 11)，我們看到了 Supervisor（統籌型） 的 Multi-Agent 模式：Planner、Foodie、Transport 各自分工，最後由 Coo..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 12

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 12 篇

## Day 12｜思想碰撞才有火花：Multi-Agent（討論型）讓 LLM 學會共識決策

- 分享至

## 前言：從統籌到討論

在 [昨天 (Day 11)](https://ithelp.ithome.com.tw/articles/10387823) ，我們看到了 **Supervisor（統籌型）** 的 Multi-Agent 模式：  
Planner、Foodie、Transport 各自分工，最後由 Coordinator 統籌整合。

這種模式清楚高效，但也偏向「一言堂」：所有決定由統籌拍板。

然而在真實世界裡，許多情境需要 **討論與辯證** ：

- 不同角色彼此挑戰、補充、質疑
- 在來回對話中逐步收斂共識
- 透過「思想碰撞」產生新火花，而不是只聽從單一權威

這就是 **Multi-Agent Conversation** 的另一種典型做法 —— **Group Chat（討論型）模式** 。

---

## Group Chat（討論型）模式

昨天的 **Supervisor 模式** ，Coordinator 是「主管」，大家分工完成後再交給他統籌拍板，結果乾淨俐落，但過程偏單向。

今天的 **Group Chat 模式** ，則是所有角色在同一個對話空間裡 **平等發言** ，透過互相質疑、補充與修正，在對話中 **逐步形成共識** 。

### 核心概念

1. **共享對話歷史**
	- 每個角色都能看到前面討論，並基於完整脈絡來回應。
2. **輪流發言**
	- 透過多回合 round-robin 發言，確保所有角色都有參與。
3. **逐步收斂**
	- 過程不是「一次到位」，而是邊討論邊調整，直到自然形成共識。

  
*圖：Group Chat 模式。所有角色（Planner、Foodie、Transport、Reviewer）都在同一個對話空間中平等發言，經過多回合互動，逐步收斂成共識行程。*

### 特點

- **平等互動** ：沒有主管，大家都是會議參與者。
- **互相辯證** ：Planner 提議 → Foodie 補充 → Transport 挑戰 → Reviewer 整理。
- **動態收斂** ：最後結果不是誰拍板，而是在交流中長出來。

---

### Supervisor vs Group Chat

| 模式 | 角色定位 | 溝通方式 | 決策產生 |
| --- | --- | --- | --- |
| **Supervisor（統籌型）** | 有一個主管（Coordinator）統籌拍板 | 各角色先完成分工，再交給主管整合 | **自上而下，由主管拍板** |
| **Group Chat（討論型）** | 所有角色平等參與 | 在同一對話空間中輪流發言、互相辯證 | **自下而上，透過討論逐步收斂共識** |

---

### 用旅行案例來比喻

假設我們還在規劃 **維也納二日遊** ：

- **Planner** 提出：「第一天上午去美泉宮，下午去聖史蒂芬大教堂。」
- **Foodie** 立刻回應：「午餐要不要去 Figlmüller？那裡有維也納豬排，而且符合『不吃牛肉』的需求。」
- **Transport** 插話：「等等，美泉宮到 Figlmüller 要 30 分鐘，再去聖史蒂芬可能會壓縮時間。」
- **Reviewer** 接著整理：「交通時間可能太趕，建議把景點縮減，或改選更近的餐廳。」
- 下一回合， **Planner** 調整：「那下午就改去霍夫堡宮，比較靠近市中心，行程更順。」

這樣一來，大家在 **對話中直接互動、逐步調整** ，最後得到一個更合理的行程。

沒有誰是最終「主管」，而是靠來回交流自然收斂。

---

## Naive Demo：小型 Group Chat

接著我們用程式碼來模擬一個簡單的 **Naive Group Chat** ，讓大家看清楚：多代理如何在「多人對話」中互動，逐步收斂。

(這裡 LLM 模型使用 `gemini-2.5-flash-lite` 是因為 Group Chat 有多輪對話會需要比較多用量， **lite** 版本免費額度較多，比較不會對話到一半超額而造成失敗)

在這個 Demo 中有四個角色：

- **Planner** ：旅行規劃師，提出景點行程。
- **Foodie** ：美食顧問，依據使用者偏好（不吃牛肉、喜歡維也納豬排）推薦餐廳。
- **Transport** ：交通顧問，檢查行程是否可行，提醒可能超時。
- **Reviewer** ：檢視者，觀察討論，若有衝突提出調整建議；若無衝突則嘗試收斂成最終方案。

### 程式碼

```python
import google.generativeai as genai
import os

# ===== 0) 初始化 =====
os.environ["GEMINI_API_KEY"] = "YOUR_API_KEY"
genai.configure(api_key=os.environ["GEMINI_API_KEY"])
LLM = genai.GenerativeModel("gemini-2.5-flash-lite") # Group Chat 有多輪對話需要比較多用量，故使用 lite 版本

# ===== 1) Memory（使用者偏好） =====
MEMORY = {
    "diet": "不吃牛肉",
    "pref": "維也納豬排"
}

# ===== 2) Agent 基底類別 =====
class BaseAgent:
    def __init__(self, name, role_description):
        self.name = name
        self.role_description = role_description

    def act(self, history):
        prompt = f"{self.role_description}\n目前討論紀錄：{history}\n請以 {self.name} 身份回應，簡短發言。"
        resp = LLM.generate_content(prompt)
        return resp.text.strip()

# ===== 3) 四個角色 =====
class PlannerAgent(BaseAgent):
    def __init__(self):
        role_description = """你是旅行規劃師，提出景點方案。
          輸出 JSON 格式，
          範例：
          {
              "Day1": {"am": "美泉宮", "pm": "聖史蒂芬大教堂"},
              "Day2": {"am": "奧地利國家圖書館", "pm": "維也納歌劇院"}
          }
        """
        super().__init__("Planner", role_description)

class FoodieAgent(BaseAgent):
    def __init__(self):
        role_description = f"""你是美食顧問，根據偏好（{MEMORY['diet']}、偏好 {MEMORY['pref']}）建議餐廳。
          輸出 JSON 格式，
          範例：{{"Day1": "Figlmüller（維也納豬排）", "Day2": "Gasthaus Pöschl"}}
        """
        super().__init__("Foodie", role_description)

class TransportAgent(BaseAgent):
    def __init__(self):
        role_description = """你是交通顧問，自行評估交通方式與所需時間並檢查行程是否交通合理，有無超時風險。
          輸出 JSON 格式，
          範例：
          {
            "Day1": {"am_to_lunch": 25, "lunch_to_pm": 30},
            "Day2": {"am_to_lunch": 22, "lunch_to_pm": 35}
          }
        """
        super().__init__("Transport", role_description)

class ReviewerAgent(BaseAgent):
    def __init__(self):
        role_description = """你是檢視者 (Reviewer)，負責觀察 Planner、Foodie、Transport 的建議。
          你的任務：
          1) 如果有衝突（例如交通超時或餐廳不合適），請條列出來，讓下一回合修正。
          2) 如果沒有衝突，嘗試整合成完整行程 JSON 與文字版方案。
          注意：你不是主管，只是多人對話的一份子，主要任務是檢查和整理。
          3) JSON 格式如下：
          {
              "Day1": {
                  "am": "美泉宮及其花園 (Schloss Schönbrunn)",
                  "lunch": "Figlmüller（維也納豬排）",
                  "pm": "霍夫堡宮 (Hofburg Palace)",
                  "transport": {"am_to_lunch": 39, "lunch_to_pm": 37}
              },
              "Day2": {
                  "am": "聖史蒂芬大教堂 (Stephansdom)",
                  "lunch": "Gasthaus Pöschl",
                  "pm": "維也納藝術史博物館 (Kunsthistorisches Museum Wien)",
                  "transport": {"am_to_lunch": 36, "lunch_to_pm": 38}
              }
          }
        """
        super().__init__("Reviewer", role_description)

# ===== 4) Group Chat 模擬 =====
class GroupChat:
    def __init__(self, agents, rounds=2):
        self.agents = agents
        self.rounds = rounds

    def run(self):
        history = "我們要規劃維也納二日行程，09:00 出發，18:00 前結束。\n"
        for r in range(self.rounds):
            for agent in self.agents:
                reply = agent.act(history)
                history += f"{agent.name}: {reply}\n---\n"
        return history

# ===== 5) 主程式 =====
if __name__ == "__main__":
    agents = [PlannerAgent(), FoodieAgent(), TransportAgent(), ReviewerAgent()]
    chat = GroupChat(agents, rounds=2)
    history = chat.run()
    print("=== Group Chat Demo ===")
    print(history)
```

---

### 執行結果

#### 1\. 各角色分別提出建議

![https://ithelp.ithome.com.tw/upload/images/20250926/20165544LZVQLOXgh5.png](https://ithelp.ithome.com.tw/upload/images/20250926/20165544LZVQLOXgh5.png)  
*圖：在多人對話初期，各角色分別提出建議：Planner 給行程、Foodie 推薦餐廳、Transport 檢查交通。這階段像「初稿」，之後會逐步收斂。*

#### 2\. Reviewer 審視所有角色的輸出後，提出潛在衝突

![https://ithelp.ithome.com.tw/upload/images/20250926/20165544T0YVwEc0MP.png](https://ithelp.ithome.com.tw/upload/images/20250926/20165544T0YVwEc0MP.png)  
*圖：Reviewer 審視所有角色的輸出後，提出潛在衝突（例如交通時間可能不足、餐廳距離過遠），並提供修正建議。這角色雖然平等，但在多人對話裡扮演「檢查與整理」的重要作用。*

#### 3\. 個角色根據 Reviewer 的建議調整安排

![https://ithelp.ithome.com.tw/upload/images/20250926/20165544o3pxZosPCn.png](https://ithelp.ithome.com.tw/upload/images/20250926/20165544o3pxZosPCn.png)  
*圖：Planner 根據 Reviewer 的建議，對 Day 2 的景點安排做出微調，並更新行程 JSON。這裡可以看到 Planner 不是單向輸出，而是會被多人對話中的回饋影響。*

![https://ithelp.ithome.com.tw/upload/images/20250926/20165544dFZveRw6xP.png](https://ithelp.ithome.com.tw/upload/images/20250926/20165544dFZveRw6xP.png)  
*圖：Transport 參考 Planner 與 Foodie 的更新方案，重新計算各段交通時間，並回報 JSON 結果。顯示出多人對話中角色會隨討論進展不斷修正自己的專業建議。*

#### 4\. Reviewer 檢視各角色的最新建議後，確認已無衝突，並整理出最終行程方案

![https://ithelp.ithome.com.tw/upload/images/20250926/20165544b0RAqdYFY8.png](https://ithelp.ithome.com.tw/upload/images/20250926/20165544b0RAqdYFY8.png)  
*圖：Reviewer 檢視 Planner、Foodie、Transport 的最新建議後，確認已無衝突，並整理出最終行程方案。這顯示出 Group Chat 模式能透過多輪互動，自然收斂成完整結果。*

這個 Demo 呈現了 Group Chat 模式的典型過程：

1. **分散建議** ：每個角色先給出各自的專業意見（行程、美食、交通）。
2. **辯證修正** ：Reviewer 發現衝突並提出修正方向。
3. **反覆調整** ：Planner、Transport 根據回饋不斷調整。
4. **自然收斂** ：最後 Reviewer 確認沒有衝突，形成完整且合理的最終方案。

和昨天的 **Supervisor 模式** 不同，這裡沒有單一主管拍板，而是讓角色在多人對話中 **平等交流 → 逐步修正 → 收斂共識** 。

這正是 **Group Chat 模式的價值** ：透過討論與互動，結果會更貼近真實世界的「會議決策」過程。

---

## 小結：Group Chat 的價值

和昨天的 **Supervisor 型** 相比， **Group Chat（討論型）模式** 更接近真實人類會議：

- **多角色平等** ：沒有唯一主管，所有角色都能自由發言。
- **互動辯證** ：透過交流補足盲點，甚至挑戰彼此假設。
- **自然收斂** ：最終結果不是自上而下拍板，而是經過互動逐步演化出來。

這是 Multi-Agent **常見的互動模式** ，特別適合：

- **創意發想** （需要腦力激盪）
- **觀點辯論** （需要正反交流）
- **跨專業協作** （避免單一視角偏誤）

Group Chat 讓 Multi-Agent 不只是「分工合作」，更能「思想碰撞」，在對話中長出新的答案。

正如維也納的沙龍文化與音樂廳氛圍，許多靈感與旋律，都是在 **交流與辯證** 中誕生的。

---

![維也納工業大學](https://ithelp.ithome.com.tw/upload/images/20250926/20165544ZQfOn4Navz.jpg)  
*圖：維也納工業大學（Technische Hochschule Wien）。 古典立面與雕像象徵科學與工藝的積累，而屋頂上的群像雕塑則呈現多個角色共同協作的場景。正如 Group Chat 模式，每一個角色都平等參與討論，透過交流與辯證逐步收斂成共識。沒有唯一的權威，而是集體智慧的展現，這也是學術與創新的真正精神。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10387823)

[

Day 11｜合奏的樂曲更精彩：Multi-Agent（統籌型）讓 LLM 學會分工合作

](https://ithelp.ithome.com.tw/articles/10387823)[下一篇](https://ithelp.ithome.com.tw/articles/10388740)

[

Day 13｜旅途中繼站：回顧 Agentic 覺醒足跡，邁向實戰新篇章

](https://ithelp.ithome.com.tw/articles/10388740)

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