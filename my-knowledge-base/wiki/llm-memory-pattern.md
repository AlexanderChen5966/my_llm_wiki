---
title: LLM Memory Pattern：讓 Agent 不再重蹈覆轍
date: 2026-06-15
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10387323
type: article
status: processed
asset_value: 62
---

Reflection 解決「事後修正」，Memory 確保「下次不再犯」——兩者結合才讓 AI 真正展現「學習與成長」的能力，從一次性助手升級為長期可靠隊友。

## T（核心論點）

**Memory 的雙重價值**：
1. **避免錯誤**：把 Reflection 產生的修正結論永久保存，下次自動套用
2. **保留偏好**：記住使用者習慣，未來互動中主動應用

三種記憶種類互補：

| 類型 | 說明 | 存活期 |
|------|------|-------|
| 短期記憶（Short-term）| 當前對話上下文 | 會話結束即消失 |
| 長期記憶（Long-term）| 跨多次互動的經驗 | 永久（形成知識庫）|
| 反思記憶（Reflection）| AI 檢討結論的持久化 | 永久（下次直接調用）|

## B（適用場景）

- AI Agent 需要跨 session 記住使用者偏好（飲食限制、工作習慣）
- 避免相同錯誤重複出現（例如 Agent 一直超出時間限制）
- 搭配 Reflection Pattern 實作完整的「學習迴圈」
- 旅遊規劃 / 行程管理 / 任何需要個人化的場景

## R（完整內容）

### Memory 核心循環

```
使用者輸入 / 任務結果
    ↓ Write
記憶庫（memory store）
    ↓ Retrieve（遇到相似任務時）
相關記憶
    ↓ Apply
更貼近需求的輸出
    ↓ → 回饋到記憶庫（Write）
```

### Reflection + Memory 完整組合

```
任務執行
    ↓
Reflection：當下檢討修正
    ↓
Memory Write：把檢討結論存入記憶
    ↓ （下次相似任務）
Memory Retrieve：自動套用之前的修正
```

### 示例：避免超時問題

```python
# 沒有 Memory → 每次都照原始設定（可能超時）
# 有了 Memory → 套用上次 Reflection 的結論

memory_store = {
    "schedule_rules": {
        "max_attraction_minutes": 240,  # 從 300 修正為 240
        "max_lunch_minutes": 90         # 從 120 修正為 90
    }
}
```

### 示例：使用者偏好記憶（Python）

```python
memory_store = {"diet": {}, "cuisine_pref": []}

def update_memory(user_input: str):
    if "不吃牛肉" in user_input:
        memory_store["diet"]["beef"] = False
    if "豬排" in user_input:
        memory_store["cuisine_pref"].append("schnitzel")

# 將 memory_store 注入 LLM prompt，讓 AI 根據記憶做決策
prompt = f"使用者的飲食習慣：{memory_store}\n候選餐廳：{candidates}\n請選最適合的..."
```

### 三種記憶在系統中的角色

- 短期記憶：`messages` 陣列（LLM 原生 context window）
- 長期記憶：外部資料庫（SQLite / JSON / 向量資料庫）
- 反思記憶：Reflection Pattern 的輸出寫入長期記憶

## C（關鍵行動）

- [ ] 為 Agent 設計記憶 schema（至少分：偏好 / 錯誤教訓 / 成功模式三類）
- [ ] Reflection 結論不只當下修正，必須 Write 進長期記憶庫
- [ ] 跨 session 的 Agent 必須在 session 開始時先 Retrieve 相關記憶

## 相關連結

- [[ideas/multi-agent-day11]] — Multi-Agent Supervisor / Pipeline / Group Chat 三模式
- [[multi-agent-group-chat-pattern]] — Group Chat 討論型（Day 12）
- [[ai-agent-architecture-patterns]] — AI Agent 四大架構模式
- [[agentmemory]] — agentmemory：跨 session 持久記憶伺服器實作
- [[context-engineering-fundamentals]] — Context Engineering 三部曲
