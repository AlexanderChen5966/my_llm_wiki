---
title: "Day 25｜實作：把協調者（Coordinator）改造成可控的 LangGraph 工作流程 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407967"
author:
  - "[[iThome]]"
published:
created: 2026-09-08
description: "前一天我們已經理解 LangGraph 的四個核心概念：狀態（State）負責保存目前工作流程知道的資訊，節點（Node）負責執行一項明確工作，連線（Edge）決定固定的下一步，而條件連線（Condi..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 26

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 26 篇

## Day 25｜實作：把協調者（Coordinator）改造成可控的 LangGraph 工作流程

- 分享至

前一天我們已經理解 LangGraph 的四個核心概念：狀態（State）負責保存目前工作流程知道的資訊，節點（Node）負責執行一項明確工作，連線（Edge）決定固定的下一步，而條件連線（Conditional Edge）則根據 State 決定流程要走哪一個分支。

今天要把這些概念真正放回 Data Machi，將原本比較集中、甚至帶有一些黑箱特性的 Coordinator，改造成一條可以觀察、測試、重試，也能在需要時加入人工介入的工作流程。

這次實作真正重要的不是「有沒有成功使用 LangGraph 套件」，而是開始把前面二十多天累積的規則變成明確的 Workflow：哪些事情可以讓模型判斷，哪些步驟不能被跳過；失敗後應該去哪裡，以及什麼情況下必須停下來。

## 先把 Coordinator 過度集中的責任拆開

前面的 Coordinator 已經負責不少事情：理解使用者問題、決定使用哪個 Tool、呼叫工具、觀察結果、判斷是否需要補查，最後再產生回答。這樣的設計在功能還少的時候很方便，但當 Verification、Memory、Retry 與 Clarification 都加進來之後，一個 Coordinator Function 很容易越寫越大。

例如原本的邏輯可能逐漸變成：先理解問題，如果需要數據就查 Sheets，如果需要文件就查 RAG，如果兩個都需要就決定順序；Tool 回來後再檢查結果，失敗時重試，資訊不足時詢問使用者，最後才整理答案。

每個規則單獨看都不複雜，但全部放在一起後，就很難回答一個最基本的除錯問題：

> 「這一題到底是哪一步做錯了？」

LangGraph 的做法不是讓 Coordinator 更強，而是反過來把責任拆開。例如第一版可以先拆成幾個主要 Node：

| Node | 主要責任 |
| --- | --- |
| Router | 理解問題需要哪些能力 |
| Data Tool | 查詢結構化資料 |
| Document Tool | 搜尋文件與 RAG |
| Verification | 確認結果是否足以回答 |
| Clarification | 向使用者補問必要條件 |
| Approval | 高風險 Action 前取得人工確認 |
| Answer | 使用已驗證結果產生最終回答 |

這樣 Router 不需要自己查資料，Tool 不需要自己決定是否回答，Verification 也不負責重新理解整個使用者問題。每個 Node 只負責一件相對明確的事情，再透過 State 把資料傳遞下去。

## 先定義 Workflow 需要哪些 State

把 Node 拆開之後，下一步不是立刻寫 Edge，而是先問：「這些 Node 之間到底需要共享哪些資訊？」

例如第一版 State 可以包含：

| State | 用途 |
| --- | --- |
| `current_question` | 使用者目前問題 |
| `conversation_context` | 前文與必要的多輪對話資訊 |
| `required_capabilities` | Router 判斷需要哪些能力 |
| `query_conditions` | 日期、市場、Category 等查詢條件 |
| `tool_results` | 各 Tool 回傳的原始結果 |
| `sources` | 資料來源與引用資訊 |
| `verification_status` | Verification 是否通過 |
| `missing_information` | 目前仍然缺少的資訊 |
| `retry_count` | 已經重試幾次 |
| `pending_action` | 尚未執行的高風險操作 |
| `approval_status` | 人工是否批准 |
| `errors` | Tool 或 Workflow 錯誤 |
| `final_answer` | 最終回答 |

State 的目的不是把所有資料無限制保存，而是讓不同 Node 清楚知道「目前這項任務已經進展到什麼程度」。

例如 Router 執行後，只需要把 `required_capabilities` 寫回 State；Google Sheets Tool 執行後，再加入 `tool_results` 與 `sources` ；Verification 則讀取這些結果，最後產生 `verification_status` 與 `missing_information` 。

如此一來，Node 之間不需要靠一大段 Prompt 猜測前面發生了什麼，而是可以直接讀取明確的資料欄位。

## Router Node 只負責分流，不要讓它重新變成萬能 Agent

第一個真正需要調整的，是 Router。

Router 的責任應該是理解：

> 「這個問題需要哪些能力？」

例如使用者問：

> 「今年共有多少筆需求？」

Router 可以判斷：

`required_capabilities = data`

如果問：

> 「需求工單的正式定義是什麼？」

則是：

`required_capabilities = document`

如果使用者問：

> 「哪一類需求最多？那一類正式怎麼定義？」

Router 可以判斷需要：

`required_capabilities = data + document`

重要的是，Router 做完這件事之後就應該結束自己的責任。它不需要直接查 Google Sheets、不需要自己搜尋文件，更不應該順便產生最終答案。

這樣當使用者問題被送錯來源時，我們只需要測 Router；數字計算錯誤時，則檢查 Data Tool Node。不同問題可以被分開定位，而不是全部歸類成「模型回答怪怪的」。

## 複合問題不只要知道用哪些 Tool，還要知道資料依賴

Router 判斷出 `data + document` 之後，也不代表兩個 Tool 一定可以同時執行。這時還需要延續 Day 20 提過的 Data Dependency。

例如：

> 「今年總工單數是多少？工單的正式定義是什麼？」

兩個問題彼此獨立，所以 Sheets 與 Document Tool 可以平行執行。

但：

> 「今年哪一類工單最多？這一類的正式定義是什麼？」

第二個問題中的「這一類」必須先等 Data Tool 找到 Top Category，因此一定要先查數據，再搜尋文件。

所以 Workflow 的重點不是單純把每個 Tool 畫成 Node，而是要把真正不能顛倒的資料依賴寫進 Edge。模型可以幫我們理解問題，但某一步明確依賴另一個結果時，Graph 應該確保流程順序不會被跳過。

## Tool Node 只負責可靠地執行能力

Tool Node 的設計可以延續前面建立 Tool 時的原則：清楚 Input、Output、Source 與 Error。

例如 Data Tool Node 接收查詢條件後，應該回傳的不只是「台灣有 328 筆」這句自然語言，而是保留結構化結果，包括實際 Query、Raw Result、資料來源與查詢時間。

Document Tool 也是相同概念。除了文件內容之外，還應該保存 Filename、Page、Section 或其他可追蹤資訊。

這些結果會被寫回 State，交給後面的 Verification Node，而不是 Tool 自己決定：

> 「這些資訊應該已經夠了，所以我直接回答使用者。」

Tool 回傳資料；是否足夠回答，是下一個 Node 的工作。

## Verification Node 是企業 Workflow 很重要的一層

Tool 執行成功，不代表任務已經完成。

假設使用者問：

> 「為什麼 Conversion Rate 下降？」

Data Tool 成功回傳：

> 4.8% → 4.3% → 3.9%

這只能證明 Conversion Rate 的確下降，不能直接證明「為什麼」。

因此 Tool Result 完成後，可以統一進入 Verification Node，檢查幾件事情：目前所有子問題是否都有證據、數字是否真的存在於 Tool Result、來源是否完整、動態資料是否已過期，以及目前結果是否足以支持回答中的結論。

| Verification Result | 下一步 |
| --- | --- |
| `passed` | 進入 Answer |
| `missing_evidence` | 補查另一來源 |
| `missing_input` | 進入 Clarification |
| `stale_data` | 重新查 Source of Truth |
| `tool_failed` | 依 Error Policy Retry 或停止 |
| `conflict` | 處理來源衝突 |

這就是 Conditional Edge 開始真正有價值的地方。模型不用自己「記得」驗證失敗後不能回答，因為 Graph 根本不提供直接前往 Answer 的路。

## Retry 也應該成為 Workflow 規則

前面 Day 23 已經談過，不是所有失敗都適合 Retry。

例如 Timeout 可以有限次重試；Permission Denied 重試再多次通常也沒有意義；Missing Input 應該進入 Clarification，而不是重新打 API；如果 RAG 沒找到文件，則可能需要調整 Query 後再搜尋一次。

因此 State 可以保存 `retry_count` ，再由 Conditional Edge 控制。例如 Timeout 時允許最多重試兩次，第三次就停止並回傳 Partial Success。

真正重要的是 Retry Limit 應該由程式控制，而不是在 Prompt 裡寫：

> 「請不要重試超過兩次。」

可以把錯誤策略整理成：

文字Markdown

CSVExcel

統計圖表

| Error | 下一步 |
| --- | --- |
| Timeout | 有限制 Retry |
| Rate Limit | 等待或 Fallback |
| Permission Denied | 停止並說明權限問題 |
| Missing Input | Clarification |
| No Result | 視情況改寫 Query 或回報 |
| Verification Failed | 回到相關 Tool 或補查 |
| 超過 Retry Limit | Stop / Partial Answer |

這樣 Error Handling 本身就變成 Workflow 的一部分。

## Clarification Node 是一條正式分支

如果 Router 或 Verification 發現問題資訊不足，也不要讓 Agent 在任何地方臨時插一句問題。

更清楚的做法，是讓 Workflow 進入 Clarification Node。

例如：

> 「幫我看最近的表現。」

目前只有：

- Market = Taiwan
- Metric = Conversion Rate
- Period = Missing

這時 State 可以記錄：

`missing_information = period`

接著進入 Clarification Node，只向使用者詢問：

> 「你是想看本月，還是最近 90 天的轉換率？」

等使用者補充後，再更新 State 並從適合的位置繼續。

這也讓「追問」從 Prompt 裡的一個模糊行為，變成真正可以被 Trace 的 Workflow Path。

## Human-in-the-loop 應該放在高風險 Action 前

前面的 Google Sheets、PDF RAG 都屬於 Read-only Tool，通常可以讓 Agent 自動執行。但如果未來加入：

- 寄送 Email
- 建立正式 Task
- 修改 Project Status
- 寫入 Database
- 刪除資料

風險就完全不同。

這類 Action 比較適合先準備 Payload，再進入 Approval Node：

```markdown
準備 Action
    ↓
Human Approval
   /          \
Approve      Reject
  ↓             ↓
Execute        Cancel
```

人工介入不是代表 Agent 不夠聰明，而是某些業務風險本來就不應該只靠模型判斷。

例如 Agent 可以決定：

> 「這個異常值得產生一封通知 Email。」

但真正的寄送動作，可以要求：

`approval_status = approved`

才允許走向 `send_email` Node。

Graph 沒有 Approval → Execute 的其他捷徑，這就比在 Prompt 中要求「寄信前記得問我」可靠得多。

## 權限可以逐步開放，不必一步到全自動

企業 Agent 也不需要一開始就追求完全自主。

比較合理的演進可以分成幾個成熟階段：

文字Markdown

CSVExcel

統計圖表

| 階段 | Agent 可以做什麼 |
| --- | --- |
| Read | 只查資料、不修改外部系統 |
| Draft | 可以準備 Email、Task 或變更內容 |
| Approve-to-Execute | 人工確認後才真的執行 |
| Limited Automation | 特定低風險 Action 可以自動執行 |
| Broader Automation | 治理、稽核與回滾成熟後再逐步擴大 |

例如第一版的 Email Tool 不必直接寄信，而可以只產生 Draft；下一階段再加入 Approval；等未來權限管理、Audit Log 與錯誤恢復都成熟之後，才開放特定低風險通知自動寄送。

> **成熟的 Agent 不是一開始就什麼都能做，而是隨著信任、可觀察性與治理能力逐步增加權限。**

## Parallel Execution 也只是 Graph 裡的一種模式

如果使用者問：

> 「今年共有多少工單？公司的工單定義是什麼？」

Router 判斷兩個問題互不依賴，就可以從同一個節點分成兩條路，同時執行 Data Tool 與 Document Tool，完成之後再到 Merge / Verification Node。

但如果使用者問的是：

> 「哪一類工單最多？這一類正式怎麼定義？」

就必須先 Data Tool，再 Document Tool。

所以 Parallel、Sequential、Retry、Clarification、Approval 都不需要各自建立一套新的 Agent Architecture。它們只是同一張 Workflow Graph 中不同的 Edge 與執行模式。

這也是 Graph 的優勢之一：我們開始用「工作依賴」設計系統，而不是用「有幾個 Agent」設計系統。

## 不要為了 Multi-Agent 而拆 Agent

做到這裡，很容易把每個 Node 都想成一個 Agent。

例如：

- Router Agent
- Sheets Agent
- Document Agent
- Verification Agent
- Approval Agent

但這通常沒有必要。

Router 可能只需要一次 LLM Classification；Sheets Node 是程式與 API；Verification 有些檢查甚至只是 Rule；Approval 的決策者則是人類。

因此很多 Node 並不代表 Multi-Agent。

如果目前使用單一 Coordinator 加多個 Tool 就能穩定理解與處理任務，沒有必要因為導入 LangGraph 就把架構強制拆成多個 Agent。

真正比較適合 Multi-Agent 的情況，是不同角色需要不同 Goal、Context、Tool Set、Model 或 Permission，或者業務流程本身存在正式角色交接。例如 Research → Review → Revision 這種工作流，才比較有理由拆成不同 Agent。

## 階段實作五｜把 Agent 設計轉成 Workflow 規格

現在可以拿出 Day 20 保存的「Agent 決策邊界」，將裡面的規則真正轉成 State、Node 與 Edge。

這次成果不一定要立刻變成完整 LangGraph 程式碼，也可以先完成一份工程師看得懂的 Workflow Specification。

例如：

文字Markdown

CSVExcel

統計圖表

| Node | 讀取哪些 State | 產生什麼輸出 | 下一步條件 | 失敗路徑 |
| --- | --- | --- | --- | --- |
| Router | Question、Context | Required Capabilities | 單一或多來源 | Clarification |
| Data Tool | Query Conditions | Result、Source、Timestamp | 完成後 Verification | Timeout／Permission |
| Document Tool | Search Query | Chunks、Source | 完成後 Verification | No Result |
| Verification | Question、Tool Results | Passed／Missing／Conflict | Answer 或補查 | Retry Limit 後停止 |
| Clarification | Missing Information | User Input | 更新 State 後返回 | 等待使用者 |
| Approval | Pending Action、Risk | Approved／Rejected | Execute 或 Cancel | 保存狀態等待 |
| Answer | Verified Results | Final Answer、Sources | END | Partial Answer |

接著再把主要 Edge 寫清楚。例如：

- START → Router
- Router → Data Tool / Document Tool / 多來源
- Tool → Verification
- Verification Passed → Answer
- Verification Missing Evidence → 對應 Tool
- Verification Missing Input → Clarification
- High-risk Action → Approval
- Approval Approved → Execute
- Approval Rejected → Cancel
- Answer → END

做到這裡，原本寫在 Prompt 裡的大量規則，就開始變成真正的 Workflow Specification。

## 至少走查五條不同路徑

完成 Graph 之後，不要只測最順利的 Happy Path。至少可以設計五種固定測試路徑。

| 測試路徑 | 要驗證的能力 |
| --- | --- |
| 單一來源 | Router 是否選對 Tool |
| 跨來源 | Sequential / Parallel 是否正確 |
| 資訊不足 | 是否進入 Clarification |
| Tool 失敗 | Retry / Failure Path 是否正確 |
| 高風險 Action | 是否真的停在 Approval |

例如跨來源測試：

> 「哪一類需求最多？它的正式定義是什麼？」

應該可以 Trace 到：

`Router → Data Tool → Document Tool → Verification → Answer`

而不是先查 Document，再自己猜一個 Category。

資訊不足測試：

> 「幫我看最近的表現。」

如果缺少必要 Period，就應該走：

`Router → Clarification`

而不是直接執行 Data Tool。

Action 測試則要確認，即使前面的結果全部正確，只要尚未取得 Approval，Workflow 就不能進入真正的 Execute Node。

## 測試時不只看答案，也要看走了哪條 Edge

在一般 LLM Application 裡，我們常只看最終答案正不正確。但到了 Graph Workflow，更應該把執行路徑本身也視為測試結果。

例如可以建立：

文字Markdown

CSVExcel

統計圖表

| ID | 問題 | 預期 Path | 實際 Path | 結果 |
| --- | --- | --- | --- | --- |
| GRAPH-01 | 今年工單多少？ | Router → Data → Verify → Answer |  |  |
| GRAPH-02 | 工單如何定義？ | Router → Document → Verify → Answer |  |  |
| GRAPH-03 | 哪類最多？它如何定義？ | Router → Data → Document → Verify → Answer |  |  |
| GRAPH-04 | 幫我看最近表現 | Router → Clarification |  |  |
| GRAPH-05 | 建立改善任務 | ... → Verify → Approval → Execute |  |  |

這樣如果最終答案碰巧正確，但 Workflow 走了不必要的 Tool 或跳過 Verification，仍然可以視為架構測試沒有通過。

這也是從「測模型答案」進一步走向「測整個 AI 系統」。

> **階段成果：**  
> 保存這份 Workflow 圖、State 定義、Node 規格，以及至少五條測試路徑。Day 30 會把它和前四階段成果、資料來源、Agent 決策邊界、權限與可靠性規則整合成最終的 **Enterprise AI Blueprint** 。

## 人工介入真正困難的是：暫停之後還能繼續

在簡單 Demo 裡，Human-in-the-loop 看起來只是一個按鈕。真正進入產品後，最困難的問題其實發生在按鈕出現之後。

假設 Workflow 目前已經完成：

- Data Query
- Verification
- Email Draft

接著停在 Approval。

如果使用者五分鐘後才批准，Backend 需要知道這次 Approval 對應哪一條 Workflow、哪一版 Email、前面用的是哪些 Tool Result，以及接下來應該從哪一個 Node 繼續。

如果使用者隔天才批准，伺服器甚至可能已經重新啟動過。

因此不能只把目前狀態留在一次 Request 的記憶體中，而需要真正把 State 保存起來。

這時就會開始遇到三個很重要的概念： **Checkpointer（檢查點）、Interrupt / Resume（中斷與恢復）與 Durable Execution（持久執行）。**

基本流程可以理解成：

```markdown
Workflow 執行
    ↓
到達高風險 Action
    ↓
保存 State / Checkpoint
    ↓
Interrupt
    ↓
等待使用者
    ↓
Approve
    ↓
載入先前 State
    ↓
Resume
    ↓
執行下一個 Node
```

這也是 LangGraph 類型 Workflow 和一般聊天 Agent 很重要的一個差異： **流程本身開始成為需要保存與管理的產品狀態，而不只是某一次 LLM 呼叫裡的 Context。**

## Pause / Resume 也帶來新的資料新鮮度問題

這裡還有一個很容易被忽略的問題。

假設系統早上 9 點查到：

> Hong Kong Conversion Rate 下降 12%。

因此產生一封通知 Email，等待主管批准。

但主管下午 3 點才按 Approve。

問題是：

> **早上 9 點的數據到下午 3 點還有效嗎？**

有些 Action 可以直接沿用，例如根據一份固定政策產生文件；但有些 Action 和即時資料高度相關，Resume 前可能還需要重新 Verification。

因此 Approval 後的流程也可以設計成：

`Resume → Freshness Check → Verification → Execute`

而不是永遠：

`Resume → Execute`

這再次說明 State Persistence 並不是單純把資料存起來，而是讓 Workflow 在恢復之後仍然有能力判斷：

> **先前的狀態現在還能不能安全使用？**

## 可控 Workflow 不代表每一步都寫死

完成今天的 Graph 之後，也不要誤以為 LangGraph 的目標是把 Agent 變成傳統 RPA。

例如：

> 「這個問題應該查 Sheets 還是文件？」

仍然可以交給 Coordinator / LLM 判斷。

> 「第一次文件搜尋結果不足，應該怎麼改 Query？」

也可以保留 Agentic Decision。

真正被 Graph 固定的是：

> 「Verification 沒通過不能進 Action。」

> 「Retry 不能超過兩次。」

> 「高風險 Action 一定要經過 Approval。」

也就是：

> **Graph 控制邊界，Agent 在邊界內做判斷。**

這就是前一天提過的「可控自主性」真正落到實作上的樣子。

## Data Machi 到 Day 25 發生了什麼變化？

回頭看整個系列，Data Machi 一開始只是：

`Question → LLM → Answer`

接著加入 RAG、Google Sheets Tool、Coordinator、Memory 與 Verification。到了 Day 25，這些能力不再只是全部堆在 Agent 身上，而開始被重新整理成一條有責任、有狀態、有分支的 Workflow。

現在它比較接近：

**Question → Router → Tool → Verification → Clarification / Retry / Approval → Answer / Action**

真正的差異不是功能突然增加，而是我們開始知道：

- 每一項責任在哪一個 Node
- 每一份資料存在 State 的哪裡
- 什麼條件會走哪一條 Edge
- 哪一類錯誤可以 Retry
- 哪一種資訊需要 Clarification
- 哪些 Action 必須人工批准
- Workflow 什麼時候應該停止

這些能力才讓 Agent 從 Prototype 逐漸變成可以被管理的系統。

---

> **今天的重點：**  
> Data Machi 不再只是一個「會自己決定下一步」的 Agent，而開始成為一條可以觀察、測試、重試、暫停與人工介入的 Workflow。LangGraph 真正帶來的價值，是把原本藏在 Prompt 與 Agent Loop 裡的規則，轉換成清楚的 **State、Node、Edge 與 Failure Path** ，同時保留模型在需要語意判斷之處的自主性。

下一篇開始，我們會從「架構正確性」走向「產品可靠性」。因為即使 Workflow 設計得再漂亮，真實世界的 API 仍然一定會遇到 **Timeout、Rate Limit、暫時失敗與第三方服務不可用** 。下一步要處理的，就是這些問題發生時，系統怎麼失敗得更可靠。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407728)

[

Day 24｜LangGraph 是什麼？用狀態（State）、節點（Node）、連線（Edge）把代理變成工作流程

](https://ithelp.ithome.com.tw/articles/10407728)[下一篇](https://ithelp.ithome.com.tw/articles/10408173)

[

Day 26｜AI 一定會失敗：逾時（Timeout）、重試（Retry）、備援（Fallback）怎麼設計？

](https://ithelp.ithome.com.tw/articles/10408173)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 28 篇

目錄

1. 24
	[Day 23｜代理（Agent）越自由越好嗎？為什麼最後會變得難以控制？](https://ithelp.ithome.com.tw/articles/10407401)
2. 25
	[Day 24｜LangGraph 是什麼？用狀態（State）、節點（Node）、連線（Edge）把代理變成工作流程](https://ithelp.ithome.com.tw/articles/10407728)
3. 26
	[Day 25｜實作：把協調者（Coordinator）改造成可控的 LangGraph 工作流程](https://ithelp.ithome.com.tw/articles/10407967)
4. 27
	[Day 26｜AI 一定會失敗：逾時（Timeout）、重試（Retry）、備援（Fallback）怎麼設計？](https://ithelp.ithome.com.tw/articles/10408173)
5. 28
	[Day 27｜代理（Agent）在工作時，使用者為什麼不能只看到轉圈圈](https://ithelp.ithome.com.tw/articles/10408179)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/27/2059752f-bdce-47ac-89d0-c84153186ce9_small.jpg)

[TS-x64：Intel Celeron N5095 4 核心 NAS，內建雙埠 2.5GbE、M.2 NVMe SSD 插槽、HDMI 輸出埠及 PCIe Gen 3 擴充槽，高性價比國民機上市](https://itplus.ithome.com.tw/vod-page/102?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

QNAP Systems, Inc. |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/289ff207-9389-4424-b179-53828e3ada76_small.jpg)

[從小團隊的 Scrum 到大團隊的 LESS](https://itplus.ithome.com.tw/vod-page/146?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/17/247eaa1e-398a-4311-ab39-2797a7c7633f_small.png)

[AI 時代的 DevSecOps 變革：安全開發流程的重塑與實踐](https://itplus.ithome.com.tw/vod-page/1464?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/78a90480-ee9e-42e7-bc6e-ca5a3b212024_small.png)

[以趨勢科技為例，如何實現低成本高效率的 Observability2.0](https://itplus.ithome.com.tw/vod-page/1484?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

35 分