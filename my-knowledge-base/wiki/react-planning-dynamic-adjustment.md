---
title: ReAct × Planning 結合：讓 Agent 先規劃再動態調整
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10393899
type: article
status: processed
asset_value: 58
---

單靠 ReAct 缺乏長遠目標，單靠 Planning 又無法應對突發狀況。結合兩者，Agent 能先定方向，再依工具回傳結果即時調整——像真正的導遊「先排好行程，再隨時根據現場狀況調整」。

## T（核心論點）

Planning 的靜態藍圖在遇到突發狀況（景點休館、天氣驟變）時只能刪除步驟，無法替代規劃；純 ReAct 又缺乏全局視野。ReAct × Planning 結合讓 Agent 同時具備「先規劃」與「邊執行邊修正」的雙重能力。

## B（適用場景）

**適用：**
- 多步驟任務且環境會變化（景點開放狀態、天氣、庫存）
- 需要「大致方向確定但細節需現場調整」的場景

**不適用：**
- 環境完全靜態可預測（用純 Planning 即可）
- 單步任務（用純 ReAct 即可）

## R（實作模式）

### 新增營業狀態檢查工具

延續多 MCP 架構，加入 `check_open_status` 工具模擬景點營業狀態：

```python
@mcp.tool()
def check_open_status(place: str, period: str) -> dict:
    """檢查景點在上午、下午或晚上是否營業（模擬用）"""
    open_status = random.choice([True, False])
    return {
        "place": place,
        "period": period,
        "open": open_status,
        "note": "營業中" if open_status else "暫時關閉（維修或週休）"
    }
```

### system_prompt 驅動動態調整邏輯

```python
agent = create_agent(
    model=llm,
    tools=tools,
    system_prompt=(
        "你是一位智慧旅遊助理，要先規劃維也納一日行程（早上、下午、晚上）。"
        "每個時段都需檢查景點是否營業（使用 check_open_status 工具）。"
        "若景點關閉，請使用 get_attractions 重新挑選並更新行程。"
        "請在過程中輸出使用的工具與輸入參數。"
    )
)
```

**關鍵設計：不是程式碼裡寫死「如果關閉就怎樣」，而是在 system_prompt 中描述決策邏輯，讓 LLM 自行執行 ReAct 循環來達成。**

### 執行歷程範例

```
先安排上午：美泉宮、下午：皇宮、晚上：音樂會。
[TOOL: check_open_status, INPUT: {'place': '美泉宮', 'period': '上午'}]
→ 美泉宮暫時關閉，重新查找其他景點。
[TOOL: get_attractions, INPUT: {'city': '維也納'}]
→ 改為上午參觀阿爾貝蒂娜博物館。
[TOOL: check_open_status, INPUT: {'place': '皇宮', 'period': '下午'}]
→ 皇宮營業中。
...

最終建議行程：
- 上午：阿爾貝蒂娜博物館
- 下午：皇宮
- 晚上：史蒂芬大教堂登塔
```

### 與純 Planning / 純 ReAct 的比較

| 模式 | 全局視野 | 應變能力 | 適合場景 |
|------|---------|---------|---------|
| 純 Planning | 有 | 無（只能刪步驟）| 環境穩定的多步驟任務 |
| 純 ReAct | 無 | 有 | 單次推理+行動 |
| Planning × ReAct | 有 | 有 | 多步驟且環境會變化 |

## C（關鍵行動）

- [ ] 任務有「整體方向」但「細節會變」時，優先考慮 Planning × ReAct 組合
- [ ] 在 system_prompt 中描述決策邏輯（而非寫死程式碼判斷），讓 LLM 自主執行調整循環
- [ ] 替代方案查詢工具（如 get_attractions）需與營業檢查工具搭配，形成完整的「失敗→替代」閉環

## 相關連結

- [[react-multi-mcp-agent]] — Day 20：純 ReAct + Multi-MCP 基礎
- [[langgraph-cot-visible]] — LangGraph 把 CoT 步驟可視化的方法
- [[langgraph-react-planning-visualization]] — Day 22：用 LangGraph 把這套決策流程節點化
- [[supervisor-multi-agent-langchain]] — Day 24：多 Agent 協作的進一步延伸
- [[ai-agent-architecture-patterns]] — Agent 四大架構模式（ReAct / Plan&Execute）
