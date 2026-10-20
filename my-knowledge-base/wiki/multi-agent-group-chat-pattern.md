---
title: Multi-Agent Group Chat（討論型）：透過辯證收斂共識
date: 2026-06-15
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10388549
type: article
status: processed
asset_value: 65
---

Group Chat（討論型）Multi-Agent 是 Supervisor 模式的對立面：沒有主管拍板，所有 Agent 平等發言，在共享對話歷史中多輪交流、互相辯證、自然收斂——最適合創意發想、觀點辯論、跨專業協作。

## T（核心論點）

**Supervisor vs Group Chat 的本質差異是「決策路徑」**：

| | Supervisor（統籌型）| Group Chat（討論型）|
|--|------------------|--------------------|
| 決策方式 | 自上而下，主管拍板 | 自下而上，透過討論收斂 |
| 角色定位 | 有一個 Coordinator 統籌 | 所有角色平等參與 |
| 溝通方式 | 先分工再整合 | 在共享空間輪流發言 |
| 適合情境 | 有明確分工的任務 | 需要辯證的複雜決策 |

Group Chat 的三個核心機制：
1. **共享對話歷史**：每個 Agent 都能看到前面討論
2. **輪流發言（round-robin）**：確保所有角色都有參與
3. **逐步收斂**：邊討論邊調整，不是「一次到位」

## B（適用場景）

- 創意發想（需要腦力激盪、跨視角）
- 觀點辯論（需要正反交流補足盲點）
- 跨專業協作（避免單一視角偏誤）
- 模擬人類會議決策過程的 Agent 系統

## R（完整內容）

### 四角色旅行規劃示例

```python
# 四個角色：Planner / Foodie / Transport / Reviewer
# 共享記憶（使用者偏好）
MEMORY = {"diet": "不吃牛肉", "pref": "維也納豬排"}

class GroupChat:
    def __init__(self, agents, rounds=2):
        self.agents = agents
        self.rounds = rounds

    def run(self):
        history = "規劃維也納二日行程，09:00 出發，18:00 前結束\n"
        for r in range(self.rounds):
            for agent in self.agents:
                reply = agent.act(history)          # 每個 agent 看完整歷史後回應
                history += f"{agent.name}: {reply}\n---\n"
        return history
```

### Group Chat 典型過程

1. **分散建議**：Planner（行程）/ Foodie（餐廳）/ Transport（交通）各自提初稿
2. **辯證修正**：Reviewer 發現衝突並提出修正方向
3. **反覆調整**：Planner / Transport 根據回饋不斷調整
4. **自然收斂**：Reviewer 確認無衝突，形成最終方案

### Reviewer 角色設計

Reviewer 不是主管，而是「檢查與整理者」：
- 有衝突 → 條列出來，讓下一回合修正
- 沒有衝突 → 整合成完整方案

```python
# Reviewer 的 role_description
"""
你是檢視者。若有衝突（交通超時或餐廳不合適），請條列出來讓下一回合修正。
若沒有衝突，嘗試整合成完整行程 JSON 與文字版方案。
注意：你不是主管，只是多人對話的一份子。
"""
```

### 模型選擇建議

Group Chat 有多輪對話，token 消耗較高，建議：
- 快速原型驗證 → `gemini-2.5-flash-lite`（免費額度多）
- 生產環境 → 設計 token 預算或 early-stop 條件

## C（關鍵行動）

- [ ] 區分使用情境：有明確分工選 Supervisor，需要辯證選 Group Chat
- [ ] Reviewer Agent 的 description 強調「非主管，只負責檢查整理」
- [ ] 設定輪次上限（rounds），避免無限迴圈消耗 token
- [ ] 把 Memory（使用者偏好）注入每個 Agent 的 role_description

## 相關連結

- [[ideas/multi-agent-day11]] — Multi-Agent Supervisor / Pipeline 模式（Day 11）
- [[llm-memory-pattern]] — Memory Pattern：讓 Agent 記住偏好和錯誤（Day 10）
- [[ai-agent-architecture-patterns]] — AI Agent 四大架構模式
- [[ai-coworker-vs-ai-agent]] — AI 同事（多 Agent 平行）vs AI Agent 比較
- [[multi-agent-papers-three-elements]] — 40 篇論文歸納：討論型正是「匯流排型通訊拓樸 + 收斂集約規則」，本篇是它的具體實作案例
- [[synthesis-multi-agent]] — Multi-Agent 系統知識精粹（本主題 7 篇整合）
