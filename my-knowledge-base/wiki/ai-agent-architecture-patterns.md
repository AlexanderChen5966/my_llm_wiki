---
title: AI Agent 架構模式完整指南：Workflow / ReAct / Plan&Execute / Dual-Agent
date: 2026-06-15
tags: [ai/agent]
source: https://codelove.tw/@tony/post/3K15Ya
type: article
status: processed
asset_value: 68
---

AI Agent 不是一種架構，而是多種模式的選擇題。從固定流程到自主規劃，不同架構對應不同場景；安全性與企業落地是另一個必須提前設計的維度。

## T（核心論點）

**Agent = LLM + Memory + Tools + 決策迴圈**

三個條件缺一不可，構成真正的 Agent：
1. 有**記憶**（短期 context + 長期 memory store）
2. 有**工具**（能執行動作，不只是回答）
3. 有**自主決策迴圈**（能根據工具結果調整下一步）

只有 LLM 回答問題 → 不是 Agent；有工具但沒有迴圈 → Function Calling，不是 Agent。

## B（適用場景）

| 架構模式 | 最適用場景 | 特點 |
|---------|-----------|------|
| Workflow | 流程固定、可預測任務 | 低風險、易部署、難應對例外 |
| ReAct | 需要即時推理+工具呼叫 | 靈活但消耗更多 token |
| Plan & Execute | 複雜長任務、需要拆解步驟 | 完成率高，但規劃錯誤難恢復 |
| Dual-Agent | 高品質輸出、需要驗證 | 準確率高，成本翻倍 |

## R（完整內容）

### 四種架構模式

**Workflow（工作流）**

```
Input → Step 1 → Step 2 → Step 3 → Output
```

- 流程預定義，LLM 只負責每個節點的處理
- 優點：可預測、可審計、成本低
- 缺點：遇到例外情況無法自適應
- 適合：文件生成、固定格式轉換、批次處理

**ReAct（Reason + Act）**

```
Thought → Action → Observation → Thought → Action → ... → Answer
```

- LLM 邊推理邊決定下一步行動
- 每次 Observation 後重新規劃
- 優點：能應對動態情況
- 缺點：token 消耗大，有時陷入死循環
- 適合：開放性問題、搜尋+分析任務

**Plan & Execute**

```
Plan: [Step 1, Step 2, Step 3, ...]
Execute: Step 1 → Step 2 → Step 3 → Result
```

- 先完整規劃，再逐步執行
- 優點：長任務完成率高，計畫可審查
- 缺點：初始規劃錯誤時難以中途修正
- 適合：開發任務、研究任務、需要前置設計的工作

**Dual-Agent（評審者 + 執行者）**

```
Executor Agent → Draft → Reviewer Agent → Feedback → Executor → Final
```

- 一個 Agent 做，另一個 Agent 審
- 優點：品質顯著提升，能抓到單一 Agent 的盲點
- 缺點：成本 2x，時間 2x
- 適合：程式碼審查、文件品質、高風險決策

### Agent 安全性（重要）

**Prompt Injection 攻擊**：惡意使用者把指令藏在工具回傳的資料中，劫持 Agent 行為。

研究資料（2024 年）：**94.4% 攻擊成功率** 對自主 Agent

防禦措施：
- **工具輸出沙箱**：工具回傳的內容不直接進入 system prompt
- **動作白名單**：Agent 只能執行預定義的動作集合
- **人工確認門檻**：高風險動作（刪除/發送/支付）必須人工確認
- **輸出驗證**：Agent 輸出的結果在執行前先驗證格式和邊界

### 企業落地三大挑戰

1. **可觀測性**：Agent 的決策鏈路需要完整 log，否則出錯無法 debug
2. **成本控制**：ReAct 和 Dual-Agent 在大量任務下成本快速膨脹
3. **錯誤恢復**：Agent 中途失敗的 checkpoint 和 retry 機制

## C（關鍵行動）

- [ ] 設計新 Agent 前先決定架構模式（Workflow / ReAct / Plan&Execute / Dual-Agent）
- [ ] 所有 Agent 必須實作動作白名單 + 高風險動作確認
- [ ] 建立 Agent 執行 log，記錄每個決策節點的 Thought + Action + Observation

## 相關連結

- [[loop-engineering-6-components]] — Loop Engineering（Agent 的外層設計框架）
- [[android-adk-agent-tool-calling]] — Android ADK（Agent 在 Android App 內的實作）
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills / MCP / Plugins / Agents 四概念比較
- [[multi-agent-group-chat-pattern]] — Multi-Agent 模式（Supervisor / Pipeline / Group Chat）
