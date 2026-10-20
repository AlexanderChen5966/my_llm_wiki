---
title: "Claude Code Dynamic Workflows：讓 AI 自己寫 AI 的排程腳本"
source: "https://www.oao.tw/ai-knowledge/claude-code-dynamic-workflows"
author:
  - "[[曉兆資訊服務有限公司 OAO Studio]]"
published: 2026-06-01
created: 2026-06-02
description: "Claude Code Dynamic Workflows（2026-05-28 發布）讓 Claude 自動生成 JS 編排腳本，同時派出最多 1,000 個平行 subagent 完成任務。研究預覽版；支援 Max / Team / Enterprise / API。"
tags:
  - "clippings"
status: processed
---
## 你現在可以做什麼？

想像你叫 Claude Code 幫你「分析這個 monorepo 裡每個 package 的依賴關係，找出循環引用，然後幫我寫修復建議」。在 Dynamic Workflows 之前，Claude Code 會一個 package 一個 package 地看，花很多時間；在 Dynamic Workflows 之後，它會寫一支 JavaScript 編排腳本，同時派出幾十個 subagent 各自負責幾個 package，並行分析，最後把結果匯集在一起。

同樣的邏輯可以套在任何「可以拆成獨立子任務」的工作上：大規模程式碼審查、多角度文獻研究、跨檔案重構、深度競品分析……以前需要你手動拆任務、依序等待；現在 Claude 自己寫腳本、自己拆、自己跑、自己驗。

```
關鍵概念：Dynamic Workflows 改變的不是 Claude 的能力，而是 Claude 的工作方式——從「聊天機器人逐輪回答」變成「AI 工程師寫編排腳本，管理一群平行 agent」。
```

這個功能在 2026 年 5 月 28 日以 Research Preview 形式推出，與 Claude Opus 4.8 同日發布，目前供 Max、Team、Enterprise 方案使用，也可以透過 API、Amazon Bedrock、Google Vertex AI 和 Microsoft Foundry 呼叫。

## 它的運作方式：JS 腳本 + 平行 Subagents

Dynamic Workflows 的核心是一個兩階段的執行模型：Claude 先生成一個 JavaScript 編排腳本，再由這個腳本控制多個 subagent 的派遣與協調。

Phase 01

Orchestration

### Claude 寫編排腳本

你下一個指令，Claude 先思考任務的結構，然後生成一支 JavaScript workflow 腳本。腳本定義了任務要怎麼拆、各子任務的依賴關係、要用什麼驗證邏輯，以及最後如何彙整。

Phase 02

Execution

### 平行派遣 Subagents

腳本執行時，根據任務需求同時派出多個 subagent，每個都是獨立的 Claude 實例，各自處理自己的子任務。上限是每個 workflow 1,000 個 subagent，同一時間最多 16 個並行執行。

Phase 03

Synthesis

### 驗證與彙整

可以在腳本中加入驗證用的 subagent——它們的工作是審查其他 subagent 的輸出、抓出矛盾或低品質結果。通過驗證的結果才被彙整成最終答案。

這個設計最重要的地方在於「Claude 寫腳本」這一步。以前的 agentic AI 是模型在每一輪根據對話歷史決定下一步；Dynamic Workflows 讓 Claude 在執行前就把整個計畫結構化為程式碼，讓執行路徑更確定、更可預測、也更容易 debug。

``// 編排腳本示意（Claude 自動生成，人不需要手寫） export const meta = {   name: 'repo-dependency-audit',   description: 'Audit circular dependencies across all packages', }  // 第一階段：平行掃描所有 packages const packages = await agent('List all packages in monorepo') const results = await parallel(   packages.map(pkg => () =>     agent(`Analyze dependencies for ${pkg}`)   ) )  // 第二階段：驗證結果，找出循環引用 const verified = await agent(   'Cross-check results, identify circular deps',   { schema: FINDINGS_SCHEMA } )  return verified``

Workflow 腳本可以儲存起來，之後在 Claude Code 的 `/` 自動補全裡直接叫出來重複使用——就像自訂的 slash command。

## 兩種觸發方式

不需要安裝任何東西。Dynamic Workflows 已內建在 Claude Code CLI、Desktop App 和 VS Code Extension 裡，有兩種方式可以啟用：

方式一

### Keyword 觸發

在 prompt 裡包含 `workflow` 這個詞，Claude Code 就會自動切換到 workflow 模式，生成編排腳本並派遣 subagents。

方式二

### Ultracode 模式

輸入 `/effort ultracode` 開啟 xhigh reasoning，Claude 會自己判斷任務是否適合用 workflow——不需要你明確說「用 workflow」。

```
注意：關鍵字觸發相當靈敏——只要 prompt 裡出現 workflow 這個詞（包含在上下文提問中），就會被識別為 workflow 任務。如果你只是在討論 workflow 概念而不想實際啟動編排，改用「multi-agent」或「parallel agents」等替代詞。
```

除了上述兩種外，也可以直接呼叫 Anthropic 提供的內建 workflow。目前最實用的是 `/deep-research` ：多角度搜尋、抓取原始資料、交叉驗證、輸出帶引用的報告。整個流程由 Claude 自動編排，不需要任何設定。

## 用量限制與 Token 成本

Dynamic Workflows 的運算規模遠超普通 Claude Code 使用，在開始之前，有幾個關鍵數字要知道：

上限

**1,000 subagents / workflow** ：單一 workflow 最多可以派出 1,000 個獨立 subagent，適用於超大規模任務（整個 codebase 審計、大量文件批次處理等）。

上限

**16 concurrent** ：同一時間最多 16 個 subagent 並行執行。超過的排隊等候，不會失敗——只是需要等空出 slot。

注意

**Token 消耗大幅增加** ：每個 subagent 都是一個完整的 Claude 呼叫，各自消耗 input + output token。10 個 subagent 大約是正常對話的 10 倍消耗。建議先用範圍較小的任務測試，確認結果再擴大規模。

建議

**從具體任務開始** ：第一次使用建議選一個範圍明確的任務（例如「審查這個目錄下的所有 API handler」），而不是開放式的大問題，避免 Claude 生成過於龐大的 workflow。

方案

**Max / Team / Enterprise** ：目前需要這三種方案之一。Enterprise 方案需要管理員在後台手動開啟。API、Amazon Bedrock、Google Vertex AI、Microsoft Foundry 也支援。

## 最適合的任務類型

Dynamic Workflows 在「可以橫向拆解成獨立子任務」的工作上效益最大。以下幾類任務特別適合：

大規模程式碼分析

### Codebase 審計與重構

讓每個 subagent 負責一個目錄或模組，同時分析安全漏洞、tech debt、依賴問題。最後匯整成一份有優先順序的報告。

研究與資訊彙整

### 多角度深度研究

內建的 `/deep-research` 就是典型案例——多個 subagent 從不同角度搜尋，交叉驗證，生成帶引用的報告。

批次生成

### 大量重複性任務

例如：為 500 個函數生成測試案例、把 100 篇文件翻譯成多語言、批次生成多個模組的文件。

驗證與品質控制

### 多角度交叉審查

派出 3 個 subagent 從不同視角（correctness、security、performance）分別審查同一份 PR，再由一個 subagent 匯整成統一的審查報告。

另一方面，Dynamic Workflows 不適合需要人在每個步驟介入的任務，也不適合單一線性流程（沒有可並行的子任務）。如果你的任務是「幫我修這個 bug」或「解釋這段程式碼」，普通的 Claude Code 對話就夠了，不需要 workflow。