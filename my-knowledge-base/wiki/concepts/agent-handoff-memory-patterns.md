---
title: Agent Handoff 與記憶模式：跨 Session 與跨工具的狀態傳遞地圖
date: 2026-06-23
tags: [ai/agent, ai/claude, pkm/workflow]
type: concept-synthesis
sources:
  - [[claude-code-handoff-memory-strategy]]
  - [[get-shit-done-gsd-framework]]
  - [[agentmemory]]
  - [[agenvoy-go-agent-platform]]
  - [[hermes-agent]]
  - [[reflection-memory-learning-loop]]
  - [[headroom-context-compression]]
  - [[agent-session-management]]
  - [[claude-code-context-management]]
  - [[claude-code-auto-memory]]
status: processed
---

## 概念定義

AI Agent 每次對話都是獨立記憶體。「Handoff（交接）」解決的是：**當一個 session 結束、或工作要換給另一個工具／Agent 繼續，怎麼讓下一個接手方不用從頭解釋一切。**

這個問題有三個維度，**第三個維度最容易踩 token 陷阱**：

| 維度 | 問題 | 狀態傳遞方式 |
|------|------|------------|
| **跨 session（同工具）** | 同一個 Claude Code 明天繼續，怎麼不失憶 | 讀回完整 HANDOFF |
| **跨工具・序列接力** | Claude Code 做完交給 Codex 繼續 | 下一棒讀完整 HANDOFF |
| **跨工具・並行協作** | 多個 Agent 同時審查／處理同一份產出（如 ai-pair 三方 review）| **協調者組一份精簡共用 context，各 Agent 不自讀** |

> ⚠️ **序列接力與並行協作的狀態傳遞剛好相反**：序列接力時下一棒需要**完整** HANDOFF（要接著開發）；並行協作時若讓每個 Agent 各讀完整 HANDOFF，token 成本 ×N 且破壞「分散帳單」設計（詳見模式一「並行協作的 token 陷阱」）。

## 五種模式總表

| 模式 | 持久化位置 | 跨工具？ | 自動化程度 | 適合情境 |
|------|-----------|---------|-----------|---------|
| **① 檔案型交接** | Markdown 檔（磁碟）| ✅ 任何工具都能讀 | 手動觸發 | 單人開發主力 |
| **② 提示詞摘要** | 人工複製貼上 | ✅ 工具無關 | 全手動 | 無任何設定時的備案 |
| **③ Framework 持久化** | checkpointer / DB | ❌ 限同一 Framework | 半自動（內建）| LangChain 系統 |
| **④ 外部記憶伺服器** | 獨立 server | ✅ 多工具共用 | 自動（接入後）| 多工具重度用戶 |
| **⑤ 異步狀態釋放** | 磁碟快照 | 視實作而定 | 自動 | 長跑 Agent + 人工審核 |

---

## 模式一：檔案型交接

**核心概念**：把 session 狀態寫成 Markdown 放在磁碟，任何工具下次只要讀這個檔案就能接上。

### HANDOFF.md + MEMORY.md（單專案）

```
HANDOFF.md  ← 給明天的我（每次覆蓋）
            ： 這次做了什麼、改了哪些檔案、TODO、下次從哪開始

MEMORY.md   ← 給未來所有對話的我（累積）
            ： 永遠有效的規則、技術選擇、白名單
```

收工時觸發選單更新兩個檔案；開工時說「讀一下 HANDOFF，我們繼續」。把觸發詞寫入 `~/.claude/CLAUDE.md` 可全自動。

### GSD 四檔案（大型任務）

```
ROADMAP.md  ← 所有階段的全貌（初始化時產出）
CONTEXT.md  ← 這個 Phase 的模糊點與邊界條件
PLAN.md     ← 原子化任務清單（XML 格式）
STATE.md    ← 目前在哪個 Wave、哪個 Task
```

每個 Phase 強制開全新 context，靠這四個檔案傳遞狀態。目的是讓主 session context 始終保持在 30-40%（GSD 稱為「Context 保鮮」——Claude 在 0-30% context 時表現最好，超過 70% 開始敷衍）。

### 多工具・序列接力（Markdown 作為通用介面）

```
Claude Code 開發 → 收工寫 HANDOFF.md
→ 開 Codex：「讀 HANDOFF.md，繼續」
→ Codex 完成 → 更新 HANDOFF.md
→ 下一個工具繼續
```

純 Markdown，工具無關。下一棒**需要完整 HANDOFF**（要接著做）。缺點：需要人工明確叫下一個工具去讀。

### 多工具・並行協作的 token 陷阱（ai-pair REVIEW_FILE 模式）

當多個 Agent **同時**處理同一份產出（最典型：ai-pair 三方 code review），狀態傳遞邏輯與序列接力**相反**——不能讓每個 Agent 各自讀完整 HANDOFF：

```
❌ 錯誤：GPT / Gemini / Claude 各自先讀完整 HANDOFF.md 再 review
   → 同一份文件付 3 次（copilot 走 GPT credits、agy 走 Google 額度各付一次）
   → token ×3，破壞「不同 reviewer 走不同帳單」的分散設計

✅ 正確：協調者（Team Lead）萃取 HANDOFF 精華 3-5 行
   → 組進「單一」REVIEW_FILE 的 header（[review brief] + 範圍化 diff）
   → 三個 Agent 共讀這一份，不自己讀任何 source
```

關鍵分辨：

| | 完整 HANDOFF | 精華（review brief）|
|---|---|---|
| 內容 | 做到哪、TODO、怎麼接續 | 為什麼這樣設計、已知 trade-off、重點看哪 |
| 給誰 | 序列接力的下一棒（要繼續開發）| 並行的 reviewer（只審查不開發）|

> ai-pair v4.3 的 Review Context Protocol 明文規定「reviewers must NOT read source files on their own — REVIEW_FILE is their only input」，正是為了這個 token 陷阱。實作細節見 b2b-manager `docs/ai-collaboration/dev-workflow-handoff-token-strategy.md`。

---

## 模式二：提示詞摘要

不需要任何設定，適用所有 AI 工具。

**標準交接摘要提示詞**（四元素缺一不可）：

> 請建立一份我可以貼到新聊天中的交接摘要。請包含：**我們想完成什麼**、**目前已做出的關鍵決定**、**有哪些資訊你如果用猜的可能會猜錯的**，以及**下一步該做什麼**。請保持精簡，讓人能在一分鐘內讀完。

為什麼不能只說「摘要 100 字」：太短的摘要只保留結論，丟失「為什麼這樣決定」和「哪些方案已排除」，新 session 仍需要重新猜。

---

## 模式三：Framework 持久化

LangChain / LangGraph 的原生記憶機制，**限同一 Framework 內部使用**。

```python
# checkpointer + thread_id = 跨 session 保留狀態
checkpointer = InMemorySaver()
agent = create_agent(..., checkpointer=checkpointer)
agent.invoke(..., {"configurable": {"thread_id": "project-001"}})
```

**Reflection × Memory 進化版**：五個節點的學習循環（Plan → Execute → Reflect → Memory → Summary），Reflect 節點把「這次犯的錯」提煉成「可重用決策準則」，下一個 session 直接注入，實現從互動中學習。

適合需要**從對話累積偏好與知識**的長期協作場景。

---

## 模式四：外部記憶伺服器

記憶不存在任一 Agent 的 context，而是在獨立伺服器，所有工具接入後共用。

### agentmemory（多工具共享後端）

```
Claude Code ──┐
Cursor       ──┤──→ agentmemory server（localhost）
Codex CLI    ──┘
```

- 接入方式：MCP server / REST API / hooks
- 自動捕捉：操作紀錄、技術決策、bug 修復
- 混合搜尋：BM25 + 向量 + graph
- 成本比較：LLM 摘要記憶 ~NT$16,000/年；agentmemory ~NT$320/年

**與 CLAUDE.md 的分工**：靜態規範（命名規則、程式碼風格）放 CLAUDE.md；動態工程知識（這個 bug 怎麼修、這個 API 的坑）交給 agentmemory。

### Hermes Agent（個人化長效記憶）

三層記憶架構：
- **Episodic**：這次互動發生了什麼
- **Skill**：學到了什麼可重用的能力
- **Honcho**：對這個用戶的長期偏好理解

越用越懂你，跨 session 不需要重新介紹自己。

---

## 模式五：異步狀態釋放

解決「Agent 需要人工輸入但不能無限等待」的問題。

**Agenvoy 的 ask_user 設計**：

```
傳統做法（阻塞）：
Agent 問問題 → 停在記憶體等 → 10 分鐘超時 → 中斷重來

異步做法（Agenvoy）：
Agent 問問題 → 記錄「目標/已執行/下一步」→ 狀態寫磁碟 → 結束執行迴圈
用戶一天後回覆 → 讀磁碟狀態 + 用戶答案 → 重組 context → resume
```

關鍵洞察：**記憶體狀態隨生命週期消失，磁碟狀態永遠在**。Human-in-the-Loop 不應該是阻塞點，而是一個非同步事件。

---

## 跨工具 Memory 能力對照

這是多工具接力最常踩的坑：

| Memory 類型 | 誰能讀 | 跨工具？ |
|-------------|--------|---------|
| Claude Code Auto Memory | 只有 Claude Code | ❌ 存在 `~/.claude/projects/`，Codex/Gemini 看不到 |
| HANDOFF.md / MEMORY.md | 任何能讀該資料夾的工具 | ✅ 純 Markdown，工具無關 |
| agentmemory server | 任何接了它的工具 | ✅ 需各工具設定一次 |
| LangChain checkpointer | 限同一 Framework | ❌ |

**結論**：Claude Code 的 Auto Memory 是私有的，不能當多工具交接的橋樑。真正跨工具共用記憶只有兩條路：**共享 Markdown 檔**（簡單但手動）或**外部記憶伺服器**（agentmemory，自動但需設定）。

---

## 選型決策樹

```
多個 Agent 同時處理同一份產出（並行 review / 並行處理）？
├─ 是 → 協調者組「單一」精簡 context，各 Agent 共讀不自取（ai-pair REVIEW_FILE 模式）
│        ⚠️ 絕不讓每個 Agent 各讀完整 HANDOFF — token ×N
└─ 否 → 需要跨工具序列接力（Claude → Codex → ...）？
   ├─ 是 → 需要自動共享，不想手動？
   │        ├─ 是 → agentmemory（模式四）
   │        └─ 否 → 共享完整 HANDOFF.md（模式一）
   └─ 否（同工具跨 session）
      ├─ 是大型任務（多 Phase）？ → GSD 四檔案（模式一進階）
      ├─ 是 LangChain 系統？     → checkpointer + Reflection（模式三）
      ├─ 需要長跑 + 人工審核？   → 異步狀態釋放（模式五）
      └─ 無任何設定/工具限制？   → 提示詞摘要（模式二）
```

---

## 底層共通邏輯

所有模式都在回答同一個問題：**哪些東西不能信任 AI 的記憶來保存？**

越重要的狀態，越要寫出來存成磁碟上的永久記錄（Markdown 或 DB），而不是依賴 AI 在同一個 context 裡「記著」。記憶體是脆弱的，磁碟是可靠的。

## 相關概念

- [[ai-pair-skill]] — ai-pair v4.3 多模型 review：並行協作的 REVIEW_FILE token 機制實例
- [[claude-code-handoff-memory-strategy]] — HANDOFF.md + MEMORY.md 三步驟實作
- [[get-shit-done-gsd-framework]] — GSD 四檔案 + Context 保鮮完整說明
- [[agentmemory]] — 多工具共享記憶伺服器深度指南
- [[hermes-agent]] — 三層個人化長效記憶架構
- [[agenvoy-go-agent-platform]] — ask_user 異步設計：狀態釋放到磁碟
- [[reflection-memory-learning-loop]] — LangChain Reflection × Memory 學習循環
- [[claude-code-auto-memory]] — Claude Code Auto Memory 機制詳解（2026-09-03 自 claude-code-context-management 拆出）
- [[concepts/agent-workflow-taxonomy]] — 六大 Agent 類型中的記憶層（④）定位

## 相關連結

- [[ai-memory-cross-agent-handoff]] — **第④類（外部記憶伺服器）的實測**：ai-memory 1.28.1 跨 Claude Code ↔ Codex 交接，⚠️ **機制成功但「摘要」不是摘要**
- [[ideas/githarness-versioned-requirement-memory]] — 📎 2026-10-19 新增：把「需求版本＋工作狀態」存成 Git 式版本，需求變更時回到語意相容的起點（arXiv 經 aiposthub 轉述，未核原文）
