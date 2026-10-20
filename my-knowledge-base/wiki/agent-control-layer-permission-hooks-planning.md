---
title: Agent 的控制層：Capability/Permission 三道防線、Hooks 生命週期、Planning 的執行狀態
date: 2026-08-07
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw（《30 天從零拆解 AI Agent》Day 4–6）
type: article
status: processed
asset_value: 78
---

## T（核心論點）

同系列 Day 4–6 三篇的共同主題是**控制層**——Agent 能呼叫工具之後，還缺三件事才能算可用：

1. **Day 4｜權限**：`Capability`（有沒有這個工具）與 `Permission`（能不能對這個對象用）不是同一件事；🔴 **Prompt 不是安全邊界**
2. **Day 5｜Hooks**：把 Logging／Metrics／Validation／Notification／Evaluation／Business Rules 掛在生命週期事件上，而不是塞進 prompt
3. **Day 6｜Planning**：**列出步驟不等於擁有計畫**——計畫必須有可查詢的執行狀態

## B（適用場景）

**適用：** 自建 agent 的權限與稽核設計；判斷某條規則該寫進 prompt 還是做成 hook；評估 agent 的「計畫」是真的還是只是一段文字

**不適用 / 注意：** 通用架構層拆解；Claude Code 的具體 hook 實作見 [[claude-code-hooks-memory]]（`exit 2` 才擋、matcher 只比對工具名）

## R（三個主題）

### 一、Day 4：三個不同的問題，需要三道防線

| 問題 | 機制 |
|---|---|
| 這個 Agent **原則上**可以做什麼？ | **Permission** |
| 這一次**高風險操作**是否需要人類確認？ | **Approval** |
| 即使操作出錯，**影響範圍**能否被限制？ | **Sandbox** |

#### Capability 與 Permission 不是同一件事

| **Capability**（有沒有這個能力）| **Permission**（能不能對這個對象用）|
|---|---|
| 是否有讀檔工具 | 可以讀專案目錄，但**不能讀家目錄** |
| 是否能執行 Shell | 可以建立 Email 草稿，但**不能直接寄出** |
| 是否能呼叫 Email API | 可以修改測試檔案，但**不能改 Production 設定** |
| 是否能寫入資料庫 | 可以查詢資料庫，但**不能刪除資料** |
| 是否能存取網路／建立雲端資源 | 可以呼叫內部 API，但**不能對外公開資料** |

> **給了工具不等於給了授權範圍。** 多數事故發生在「工具沒問題、對象錯了」。

#### 🔴 Prompt 不是安全邊界

「不要刪除重要檔案」「不要存取使用者的敏感資料」「在進行危險操作前先詢問使用者」——這些寫在 prompt 裡都**只是期待**：模型可能誤解什麼叫「重要檔案」、可能判斷錯什麼算「危險」。

> 這與 [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] 的「Prompt 描述期待而非保證」是同一命題；對應 Claude Code 就是 [[claude-code-hooks-memory]] 記載的「CLAUDE.md 是 advisory，Hook 才是 mandatory」。

### 二、Day 5：Hooks 該承接哪些關注點

**生命週期事件**：任務開始前／模型呼叫前／模型回應後／工具執行前／工具執行後。

掛在上面的橫切關注點：**Logging、Metrics、Validation、Notification、Output Processing、Evaluation、Business Rules**。

> 判準：**凡是「每次都要發生、且不該依賴模型記得」的事，都不該寫在 prompt 裡，應該掛成 hook。**

### 三、Day 6：列出步驟不等於擁有計畫

叫 agent「請先制定完整計畫，再逐步執行」，它會產出一份漂亮清單（理解需求→收集資料→分析問題→執行修改→驗證結果→回覆使用者）——**但這只是文字，不是計畫**。

真正的計畫必須讓系統能回答：
- 知道目前做到哪裡
- 知道哪些步驟已完成 / 哪些失敗
- 知道哪些步驟**依賴其他結果**
- 知道**新資訊是否讓原計畫失效**
- 知道什麼時候需要重新規劃
- 知道什麼條件下可以宣告完成

🔑 **因此計畫必須拆成兩個東西**：

| | 內容 |
|---|---|
| **Plan** | 接下來準備做什麼 |
| **Execution State** | 每個步驟目前是 **Pending / Running / Blocked / Failed / Completed** |

原文提出的三個真問題：什麼才算真正的 Agent Planning？Planning 應該存在於 **Prompt、State 還是 Control Flow**？**什麼任務根本不需要 Planning？**

> 最後一問值得記住——不是所有任務都需要計畫，這與 [[superpowers-skill-bloat-risk-tiering]] 的「儀式成本應與任務風險成比例」同向。

## C（關鍵行動）

- [ ] 分清 Capability 與 Permission：給了工具之後，另外定義**能對哪些對象用**
- [ ] 高風險操作走 Approval（人工確認），別靠 prompt 提醒
- [ ] 🔴 **任何安全相關的約束都不要只寫在 prompt 裡**——它不是安全邊界
- [ ] 把 Logging／Validation／Business Rules 掛成 hook，而非塞進 prompt 期待模型記得
- [ ] 檢查你的 agent「計畫」有沒有可查詢的 **Execution State**（Pending/Running/Blocked/Failed/Completed）；只有清單就不算有計畫
- [ ] 先問「這個任務需要 Planning 嗎」——不是所有任務都需要

## 相關連結

- [[synthesis/synthesis-agent-engineering-architecture]] — **本篇為其來源之一**（2026-09-03 重跑，8→16 篇）：Agent 工程架構精粹，含 Capability scaffolding vs Control plane 二分法

- [[agent-tool-runtime-seven-layers]] — 同系列 Day 3；本篇的 Permission 對應其第 3 層 Permission Gate、Hooks 對應其第 7 層 Audit/Trace
- [[subagent-four-boundaries]] — 同系列 Day 7；本篇的 Capability/Permission 區分是其第三層邊界的細節
- [[claude-code-hooks-memory]] — Claude Code 的具體 hook 實作：四種事件、`exit 2` 才會擋、matcher 只比對工具名稱、資料只走 stdin
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — Prompt/Policy/Tool/Workflow 四層；本篇「Prompt 不是安全邊界」是其 Policy 層存在的理由
- [[obligations-as-code-tedium-is-stability]] — 每條義務接可判定檢查；本篇的 hook 是其 sensor 的一種實作
- [[harness-engineering]] — Guides × Sensors 骨架理論；本篇三個控制層都屬 harness 範疇
- [[task-list-single-source-of-truth]] — 分開「已實作」與「已完成」；與本篇 Execution State 五種狀態同一需求
- [[agent-context-management]] — **同系列 Day 09**：「Pinned Context 由程式決定、不交給模型自由 Evict」是本篇「Prompt 不是安全邊界」在 Context 層的對應
- [[decision-model-ecosystem]] — 決策模型的決策層：信心值不能替代操作權限，權限檢查由程式執行（2026-10-08 補）
- [[prompt-injection-vs-jailbreak]] — 威脅模型面：不可信任的 Source × 高權限的 Sink，說明本篇三道防線要防的是什麼
