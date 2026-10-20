---
title: Tool Call 只是一張工作單：Tool Runtime 的七層責任
date: 2026-08-05
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw（《30 天從零拆解 AI Agent》Day 3）
type: article
status: processed
asset_value: 80
---

## T（核心論點）

很多框架把「執行工具」包成一行 API，讓人以為模型產生 Tool Call 後工具就會自然完成。但 **Tool Call 只是模型提出的結構化意圖——它不是執行結果，不是安全邊界，也不是完整的 Tool Runtime**。真正把這張工作單轉換成環境變化的是 **Tool Runtime**，而**模型只參與整條鏈的第一步，其餘七層全屬 Harness 的責任**。

## B（適用場景）

**適用：** 自建 agent 框架；評估第三方框架的工具執行層是否夠完整；除錯「agent 明明呼叫了工具卻沒效果／出錯後就崩掉」

**不適用 / 注意：** 本篇是通用架構層的拆解，Claude Code 的具體實作見 [[claude-code-harness-internals]]（兩者互為抽象與實例）

## R（七層責任）

### Tool Call 到底是什麼

```
Tool Call = 工具名稱 + 模型產生的參數 + 模型希望系統執行的意圖
```

模型回傳 `{"name":"read_file","arguments":{"path":"config.yaml"}}` 時，**檔案還沒有被讀取**。系統此刻仍不知道：`read_file` 是否真的存在、`path` 是否符合 Schema、`config.yaml` 是否在允許目錄、模型是否有權限、能否在時限內完成、回傳結果是否太大、是否需要記錄、出錯要怎麼回傳。

> **Tool Call 更接近一張工作單，而不是實際工作。**

### 完整執行鏈

```
模型產生 Tool Call        ← 模型只參與這一步
        ↓
Tool Registry 尋找工具
        ↓
Schema Validation 驗證參數
        ↓
Permission Gate 檢查權限
        ↓
Runtime 執行工具
        ↓
Timeout / Error Handling
        ↓
Output Normalization
        ↓
Tool Result 回到 Agent Loop
```

| 層 | 責任 | 關鍵設計 |
|---|---|---|
| **1. Tool Registry** | 依名稱找到工具實作 | 🔑 **它決定模型實際能看見哪些能力**——模型不應能呼叫任何沒被明確註冊的函式。工具不存在時**不該直接 Crash**，要產生結構化錯誤 `{"ok":false,"error":{"type":"unknown_tool"}}` 交回模型，讓它能修正名稱／換工具／向使用者說明 |
| **2. Schema Validation** | 驗證模型產生的參數 | 模型參數可能缺欄位／名稱錯／型別錯／值超範圍／多帶不接受的欄位／字串含危險內容 |
| **3. Permission Gate** | 檢查這次操作是否被允許 | 權限判斷是**系統責任，不是模型責任** |
| **4. Runtime 執行** | 實際呼叫工具 | — |
| **5. Timeout / Error** | 控制逾時、正規化錯誤 | 錯誤要能讓模型再決定：改用較小的查詢／換工具／詢問使用者／結束任務 |
| **6. Output Normalization** | 限制輸出大小 | ⚠️ 幾萬行 Log／大型 JSON／Binary／DB Cursor／敏感資訊／無法序列化的資料，**全部塞進 `messages[]` 可能立刻耗盡 Context Window**。手段：截斷／摘要／分頁／存成 Artifact／只回索引或檔案位置，讓模型再選要讀哪段 |
| **7. Audit / Trace** | 記錄執行全程 | 呼叫了哪個工具、什麼參數、權限是否通過、執行多久、回傳什麼、是否產生副作用、哪一輪出錯、最後為什麼停止。🔴 **沒有 Trace，agent 失敗時只剩最終輸出，很難知道問題在哪一層** |

### 🔑 責任邊界

**模型可以提出**：「我想讀取這個檔案。」
**模型無法判斷**：自己是否有權限、參數是否可信、工具要執行多久、輸出能否完整放進 Context、這次操作是否需要 Audit。

> **這些全是系統責任。** 把它們誤當成模型的責任，就會寫出「直接相信模型產生的參數」這類常見錯誤設計——**模型輸出是非確定性的**。

## C（關鍵行動）

- [ ] 檢查自己的 agent：工具不存在時是 Crash 還是回結構化錯誤給模型？
- [ ] **絕不直接相信模型產生的參數**——Schema 驗證要在執行前，不是靠工具自己擋
- [ ] 為工具輸出設大小上限，超過就改回索引／Artifact 參照，別整包塞進 `messages[]`
- [ ] 補上 Audit/Trace——否則 agent 出錯只能看到最終輸出，無法定位是哪一層失敗
- [ ] 評估第三方框架時，逐層對照這七層它幫你做了哪些、哪些要自己補

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[claude-code-harness-internals]] — **本篇的具體實例**：Claude Code 的工具結果包裝與 `<persisted-output>` 就是第 6 層 Output Normalization 的實作；「工具結果以 user 角色訊息回傳」則是第 8 步回到 Agent Loop
- [[synthesis-claude-code-internals]] — Claude Code 內部機制精粹；本篇是同一主題的**通用架構層**，可對照該精粹的三層架構（本篇七層全在 Harness 層內）
- [[claude-code-api-request-reverse-engineering]] — 攔到的 83 個工具定義即第 1 層 Tool Registry 的內容；`Read` 工具 1,782 字元 description 對應第 2 層的 Schema
- [[agent-loop-explained]] — Agent Loop 概念；本篇補上「執行工具」這一步實際包含七層
- [[ai-agent-core-loop]] — Agent 核心迴圈約 80 行；本篇說明那 80 行裡「執行工具」該展開多少
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — 企業 AI 四層；本篇的 Permission Gate 對應其 Policy 層
- [[agent-control-layer-permission-hooks-planning]] — **同系列 Day 4–6 續篇**：本篇第 3 層 Permission Gate 在該篇展開為 Permission/Approval/Sandbox 三道防線（含 Capability vs Permission 之分、「Prompt 不是安全邊界」）；第 7 層 Audit/Trace 在該篇對應 Hooks 生命週期
- [[subagent-four-boundaries]] — **同系列 Day 7**：本篇的七層是「單一 agent 怎麼執行工具」，該篇是「什麼時候該把工作切給另一個 agent」（四層邊界＋Tool vs Subagent 判準）
- [[agent-context-management]] — **同系列 Day 09**：Tool Result 回到 Context 之後的生命週期管理（不同 Observation 類型分流保留策略）
- [[handcraft-agent-shell-tool]] — 第 5 層（Timeout/Error）與輸出治理的完整手刻實作：process group 收行程、輸出雙上限、結束碼三分法、`| tail` 會吃掉失敗
