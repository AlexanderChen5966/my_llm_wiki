---
title: "Day 23｜代理（Agent）越自由越好嗎？為什麼最後會變得難以控制？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407401"
author:
  - "[[iThome]]"
published:
created: 2026-09-04
description: "做到這裡，Data Machi 的 Agent 已經可以選工具、保留部分 Memory，也能在資訊不足時 Clarify、Re-query 或進行 Verification。能力看起來越來越完整，但同..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 24

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 24 篇

## Day 23｜代理（Agent）越自由越好嗎？為什麼最後會變得難以控制？

- 分享至

做到這裡，Data Machi 的 Agent 已經可以選工具、保留部分 Memory，也能在資訊不足時 Clarify、Re-query 或進行 Verification。能力看起來越來越完整，但同時也會出現另一個問題： **系統的行為開始變得越來越難預測。**

前面介紹 ReAct 時，我們讓 Agent 在每一次 Tool 執行後重新觀察結果，再決定下一步。這種動態決策確實帶來彈性，但如果所有判斷都藏在同一個 Agent Loop 裡，隨著 Tool、Retry、Memory、Verification、Clarification 與人工確認逐漸增加，開發者很快就會發現：明明每一項功能都合理，整體流程卻變得很難理解。

可以先把兩種架構簡單比較：  

兩者並不是誰一定比較好，而是隨著任務風險與複雜度提高，我們開始需要把某些原本藏在 Agent Loop 裡的規則「拉出來」，變成程式可以明確控制的流程。

## 第一個問題：流程順序不能只靠模型記得

有些企業流程不是「最好按照這個順序」，而是 **一定要按照這個順序** 。

例如：

```markdown
取得資料
    ↓
驗證資料
    ↓
確認結果
    ↓
建立正式任務
```

又或者：

```markdown
準備 Email
    ↓
人工確認
    ↓
寄出 Email
```

如果只在 System Prompt 裡寫：

> 「請務必先驗證資料，再建立任務。」

模型大多數時候可能會照做，但企業系統真正需要的是：

> **這個步驟在技術上就不能被跳過。**

兩者差異很大。

Prompt 比較像告訴同事：

> 「記得寄信前先讓我確認。」

Workflow 則像在系統裡直接設定：

```markdown
沒有 approval = true
→ send_email() 無法被執行
```

後者才是真正的流程控制。

## 「模型大多會遵守」和「系統保證」不是同一件事

假設我們允許 Agent 使用三個 Tool：

```markdown
query_data
verify_result
create_task
```

如果全部交給模型決定，理論上它應該：

```markdown
query_data
    ↓
verify_result
    ↓
create_task
```

但 Agent 也可能因為 Prompt 理解、Context 太長或模型決策不同，直接執行：

```markdown
query_data
    ↓
create_task
```

如果 `create_task` 只是建立測試 Todo，問題可能不大；但如果未來換成：

```markdown
update_database
send_email
publish_report
approve_request
```

跳過 Validation 就可能產生真正的業務風險。

因此可以把設計原則整理成：

> **需要語意判斷的地方交給模型；不能被跳過的規則交給程式。**

這會是後面 LangGraph 很重要的架構思想。

## 第二個問題：失敗與 Retry 需要有明確位置

前面我們已經遇到過很多不同失敗：

```markdown
Timeout
Permission Denied
No Result
Invalid Input
Model Error
```

但當整段流程都存在同一個 Agent Loop 中時，「失敗之後去哪裡」很容易變成 Prompt 裡的一堆自然語言規則。

例如：

> 如果 Google Sheets Timeout，請再試一次；如果仍然失敗就告訴使用者。如果 RAG 搜尋沒有足夠結果，請重新搜尋；如果還是找不到就詢問使用者。如果 Gemini 發生錯誤，改用另一個模型……

當規則增加之後，Prompt 很快會變成：

```markdown
如果 A → B
但 C 時 → D
如果 D 失敗 → E
如果 E 又符合 F → 回到 B
除非 G → H
```

這時即使人類自己重新閱讀 Prompt，也未必能快速回答：

> 「這個錯誤最後到底會走去哪裡？」

## Retry 不是只有「再試一次」

不同錯誤需要完全不同的處理方式。

例如 Google Sheets Tool 回：

```markdown
Timeout
```

合理策略可能是：

```markdown
Retry 1 次
    ↓
仍然 Timeout？
    ↓
停止 + Partial Answer
```

但如果回傳：

```markdown
Permission Denied
```

就不應該無限 Retry，因為再呼叫十次通常也不會突然取得權限。

合理流程應該是：

```markdown
Permission Denied
    ↓
停止 Retry
    ↓
回報權限問題
```

如果是：

```markdown
Missing Parameter
```

則可能要：

```markdown
Clarification
```

如果是：

```markdown
No Relevant Document
```

則可能：

```markdown
調整搜尋 Query
    ↓
Search Again
```

因此錯誤處理本身其實就是一張 Workflow。

可以整理成：

文字Markdown

CSVExcel

統計圖表

| 錯誤 | 合理下一步 |
| --- | --- |
| Timeout | 有限制地 Retry |
| Permission Denied | 停止並回報權限問題 |
| Missing Input | Clarification |
| No Result | 調整條件或補查 |
| Model Failure | Retry 或 Fallback |
| Verification Failed | 回到資料取得或重新計算 |

如果這些路徑全部只藏在 Agent Prompt 裡，流程會越來越難追蹤。

## 第三個問題：除錯時很難知道真正是哪一步錯了

假設使用者問：

> 「找出最近問題最大的市場，確認定義，再建立改善任務。」

最後系統建立了一個錯誤任務。

如果所有事情都在同一個 Agent Loop 裡，開發者可能只能看到：

```markdown
Agent chose Tool A
Agent got result
Agent chose Tool B
Agent chose Tool C
Final output...
```

但真正想知道的是：

```markdown
Step 1：Data Query 正確嗎？
Step 2：Top Market 判斷正確嗎？
Step 3：Document Search 找對定義嗎？
Step 4：Verification 通過嗎？
Step 5：Task Payload 正確嗎？
Step 6：為什麼最後允許建立 Task？
```

這就是 **Observability（可觀察性）** 的問題。

當一條 Workflow 有明確 Node 時，就比較容易紀錄：

```markdown
Node: query_data
Status: SUCCESS

Node: verify_metric
Status: SUCCESS

Node: search_definition
Status: SUCCESS

Node: verify_answer
Status: FAILED
```

看到這裡就知道，流程根本不應該繼續進到 Action。

相較之下，如果全部都只是一個長 Agent Loop，就需要重新翻整段 Trace 才能還原真正的決策路徑。

## 第四個問題：Human-in-the-loop 很難只是 Prompt 裡的一句話

當 Data Machi 還只有 Read Tool 時，Agent 做錯選擇的風險相對低。例如查錯一份文件，最多是回答錯誤。

但當系統開始加入 Action Tool，例如：

```markdown
create_task
update_status
send_email
write_database
```

就會出現新的需求：

> **某些操作必須先經過人工確認。**

例如 Agent 準備：

```markdown
Action:
Send Email

Recipient:
project-team@example.com

Content:
Project delay notification...
```

我們可能希望使用者先看到：

```markdown
即將寄出 Email
是否確認？
```

只有使用者選擇：

```markdown
Approve
```

才能真正執行。

這代表 Workflow 必須支援：

```markdown
準備 Action
    ↓
Pause
    ↓
等待使用者
    ↓
Approve / Reject
    ↓
Resume
```

這不是普通 Agent Loop 最自然的使用方式，因為執行可能不是幾秒鐘後繼續，而是幾分鐘、幾小時甚至隔天才得到確認。

## Pause 代表系統必須保存 State

假設流程停在：

```markdown
Task Draft 已建立
等待使用者確認
```

使用者半小時後按下：

> 「確認建立。」

系統必須知道：

```markdown
當時是哪一個 Task？
來源是什麼？
使用者確認的是哪個版本？
前面有哪些 Tool Result？
Workflow 現在停在哪一個 Step？
```

因此 Human-in-the-loop 很自然會帶出另一個需求：

> **State Persistence（狀態保存）。**

例如：

```markdown
workflow_id = wf_001

current_node = human_approval

pending_action = create_task

task_payload = {...}

verification_status = passed
```

等使用者確認後再從這個狀態繼續。

這種「Pause → 保存 → Resume」的能力，就是為什麼後面會需要更明確的 Workflow Engine，而不是只依賴一段 Agent Prompt。

## 第五個問題：Memory、Verification 和 Retry 開始互相影響

目前 Data Machi 已經有：

```markdown
Tool Selection
Memory
Re-query
Clarification
Verification
Retry
Partial Success
```

每一個功能單獨看都不複雜，但組合後很快就會產生大量分支。

例如：

```markdown
使用者問題
    ↓
Memory 有舊結果？
    ↓
資料還新鮮？
    ↓
需要 Re-query？
    ↓
Tool 執行成功？
    ↓
Verification 通過？
    ↓
需要另一個 Tool？
    ↓
是否達到 Max Steps？
    ↓
需要 Clarification？
```

如果再加入 Action：

```markdown
需要人工確認？
```

流程會變得更複雜。

這也是很多 Agent Prototype 在 Demo 階段看起來很好，但真正加入企業規則後開始難以維護的原因：不是模型突然變差，而是 **控制邏輯變多了，卻仍然全部藏在同一個自由循環裡。**

## Agent Loop 的優勢其實仍然存在

講到這裡，不代表 Agent Loop 本身不好。

如果任務只是：

```markdown
2–3 個 Tool
+
簡單搜尋
+
沒有高風險 Action
+
沒有複雜 Retry
```

ReAct 或 AgentExecutor 類型的 Agent Loop 其實非常方便。

例如：

> 「幫我找出公司政策裡和差旅住宿相關的規則。」

Agent 可以自己嘗試 Search Tool、修改 Query、再產生答案。這種任務如果硬畫成十幾個 Node，反而可能過度工程化。

因此真正的問題不是：

> 「Agent Loop 好不好？」

而是：

> **這個任務是否已經複雜到需要更明確的流程控制？**

## 什麼時候 Agent Loop 開始不夠用？

可以觀察幾個很實用的訊號。

第一個訊號是 Prompt 開始大量出現：

> 「一定要先 A，再 B。」

例如：

```markdown
一定要先查資料
一定要驗證
驗證失敗不能回答
寄信前一定要取得確認
```

這些規則如果越來越多，通常代表它們應該離開 Prompt，進入 Workflow。

第二個訊號是 Agent 偶爾會跳過必要步驟。例如十次裡九次都先 Verification，但有一次直接進到 Action。對高風險場景來說，「90% 會遵守」通常不夠。

第三個訊號是開始需要 Pause / Resume，也就是 Human-in-the-loop。

第四個訊號是 Retry 路徑變多，而且不同錯誤有不同處理方式。

第五個訊號是除錯時已經很難回答：

> 「這個 Request 到底經過了哪些節點？」

當這些現象開始出現，就代表架構可能需要從單純 Agent Loop 往 Graph Workflow 演進。

## 從 Agent Engineering 走向 Graph Engineering

可以把前面的 Agent 想成：

```markdown
Agent
  ↓
根據目前情況
自己決定下一步
```

而 Graph Engineering 更像：

```markdown
Workflow
  ↓
哪些路可以走？
哪些地方由模型決定？
哪些地方由程式保證？
```

兩者最大的差別不是「有沒有 LLM」，而是決策責任重新分配。

例如：

```markdown
理解使用者問題
→ LLM
```

這個步驟需要語意理解，很適合交給模型。

```markdown
驗證成功後才能進入 Action
→ 程式 Edge
```

這種規則則不需要讓模型判斷。

再例如：

```markdown
查不到文件後，應該改 Query 還是 Clarify？
→ LLM / Coordinator
```

可以保留彈性。

但：

```markdown
最多 Retry 2 次
→ 程式 Counter
```

更適合使用 deterministic rule。

因此可以整理成一個很重要的分工：

文字Markdown

CSVExcel

統計圖表

| 問題 | 比較適合誰決定 |
| --- | --- |
| 使用者真正想問什麼？ | LLM |
| 哪個 Tool 語意上最適合？ | LLM |
| 找不到資料後應改怎麼查？ | LLM / Policy |
| Verification 失敗能不能進下一步？ | 程式 |
| 最多 Retry 幾次？ | 程式 |
| Action 前是否一定需要 Approval？ | 程式 |
| 某個 User 是否有權限？ | 權限系統 |
| Workflow 接下來允許走哪些路？ | Graph |

這就是 Graph Engineering 最核心的價值： **不是消滅 Agent，而是替 Agent 畫出可以安全活動的道路。**

## 自由和控制不是二選一

企業 Agent 並不是只能在兩種極端中選擇：

```markdown
完全固定 Workflow
```

或者：

```markdown
完全自由 Agent
```

真正實用的架構通常是兩者混合。

例如：

```markdown
START
  ↓
理解問題
  ↓
Agent 決定使用哪個 Tool
  ↓
執行 Tool
  ↓
Verification
  ↓
┌──────────────┬──────────────┐
│ Passed       │ Failed       │
│              │              │
│繼續          │回到搜尋      │
└──────────────┴──────────────┘
  ↓
需要 Action？
  ↓
Human Approval
  ↓
Execute
  ↓
END
```

其中：

```markdown
選哪個 Tool
```

仍然可以讓 Agent 自主決定。

但：

```markdown
Verification Failed
→ 不允許 Execute
```

則由 Workflow 強制保證。

因此我們不是把 Agent「關起來」，而是讓它：

> **在需要彈性的地方自主，在不能出錯的地方受到明確控制。**

## 實務案例｜寄信前一定要經過 Verification 與 Approval

假設未來 Data Machi 可以自動產生異常通知 Email。

使用者說：

> 「如果香港的 Conversion Rate 下降超過 10%，幫我通知團隊。」

如果全部交給 Agent Loop，它可能需要自行完成：

```markdown
查數據
→ 計算下降幅度
→ 判斷是否超過 10%
→ 產生 Email
→ 寄信
```

企業版本可能更適合寫成：

```markdown
Data Query
    ↓
Calculate
    ↓
Threshold Check
    ↓
Conversion Rate ↓ > 10%？
    ↓
┌───────────────┬───────────────┐
│      否       │      是       │
│               │               │
│     END       │ Verification  │
└───────────────┴───────────────┘
                        ↓
                 Verification Passed？
                        ↓
               ┌────────────┬────────────┐
               │     否     │     是     │
               │            │            │
               │ Stop       │Draft Email │
               └────────────┴────────────┘
                                    ↓
                            Human Approval
                                    ↓
                            Send Email
```

這條流程裡 LLM 仍然可以負責：

```markdown
理解異常原因
整理 Email 內容
```

但數字計算、Threshold、Verification Gate 和 Approval Gate 都由 Workflow 控制。

這就是「Agent + Workflow」真正的混合設計。

## 為什麼這時候 LangGraph 才有必要？

如果 Day 06 就直接介紹 LangGraph，可能會很容易變成：

> 「因為大家都在用這個框架，所以我們也來學。」

但走到 Day 23，問題其實已經自然出現了。

目前我們需要處理：

```markdown
State
Tool Selection
Conditional Routing
Retry
Verification
Clarification
Memory
Human Approval
Partial Success
Pause / Resume
```

這些能力已經不是單一 Prompt 最適合管理的形式。

我們需要一種方式，把流程拆成幾個明確概念：

```markdown
State
目前系統知道什麼

Node
現在執行什麼

Edge
下一步去哪裡
```

例如：

```markdown
[Coordinator]
      ↓
[Tool]
      ↓
[Verification]
      ↓
   Passed?
   /     \
 Yes     No
 ↓        ↓
Answer   Retry
```

一旦流程被畫出來，就能開始回答：

- 哪個 Node 失敗？
- 哪個 Edge 被走過？
- State 現在是什麼？
- Retry 幾次了？
- 為什麼停在這裡？
- 哪裡需要人工批准？

這才是 LangGraph 真正要解決的問題。

## 不要因為 LangGraph 很熱門，就把所有流程 Graph 化

同樣地，Graph 也不是越多越好。

如果目前應用只有：

```markdown
User
 ↓
RAG
 ↓
Answer
```

根本沒有必要為了使用 LangGraph，硬拆成十個 Node。

如果只需要：

```markdown
User
 ↓
Agent
 ↓
2 個 Tool
 ↓
Answer
```

而且沒有 Human Approval、複雜 Retry 或高風險 Action，簡單 Agent Loop 可能仍然最合適。

架構應該跟著問題長大，而不是先選框架，再讓問題配合框架。

可以把演進理解成：

```markdown
簡單問題
    ↓
Direct LLM

需要外部能力
    ↓
Tool Use

需要多步驟
    ↓
Workflow

需要動態決策
    ↓
Agent

需要可控的動態流程
    ↓
Graph-based Agent Workflow
```

不是每個階段都必須走到最後一層。

## 實務踩坑｜不要把流程規則全部藏在 Prompt

如果 System Prompt 開始出現大量內容：

```markdown
先做 A
如果失敗做 B
如果還失敗就 C
除非 D
在 E 之前一定要 F
最多 Retry 兩次
沒有 Approval 不能 G
```

這通常就是一個訊號：

> **這些內容有一部分已經不是 Prompt Instruction，而是 Workflow Specification。**

Prompt 更適合處理：

```markdown
角色
語意判斷
回答方式
Tool 選擇原則
```

Workflow 則適合處理：

```markdown
順序
分支
Retry
Stop
Approval
Failure Path
```

把兩者分開後，不只 Agent 比較容易理解，開發者也更容易測試整套系統。

## 從「Agent 有多聰明」改成「系統有多可控」

到 Day 23，可以重新看整個 Data Machi 的演進。

一開始我們關心的是：

```markdown
LLM 能不能回答？
```

後來變成：

```markdown
能不能查企業資料？
```

接著變成：

```markdown
能不能自己選 Tool？
```

再來是：

```markdown
能不能根據結果調整下一步？
```

到了現在，真正重要的問題已經開始變成：

```markdown
它做錯時能不能知道錯在哪？
哪些步驟不能跳過？
什麼時候必須停止？
高風險 Action 能不能被攔住？
流程能不能被重現與稽核？
```

這也是企業 Agent 很關鍵的一次架構轉變：從追求「自主性」，開始轉向追求「 **可控的自主性** 」。

---

> **今天的重點：**  
> Agent 越自主，不代表系統就應該越黑箱。當流程開始出現必要順序、Verification、Retry、Human-in-the-loop、Pause / Resume 與高風險 Action 時，就不應只依靠 Prompt 要求模型「記得遵守」。更可靠的做法，是讓模型處理需要語意判斷的地方，讓 Workflow 用程式保證不能被跳過的規則。企業真正需要的是： **該自由的地方自由，該控制的地方明確控制。**

下一篇，我們會正式進入 **LangGraph** ，用 **State、Node 與 Edge** 三個核心概念，把目前散落在 Agent Loop 裡的 Tool Selection、Verification、Retry、Clarification 與 Human Approval，重新畫成一張可以觀察、測試與控制的 Workflow。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407137)

[

Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？

](https://ithelp.ithome.com.tw/articles/10407137)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 24 篇

目錄

1. 20
	[Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具](https://ithelp.ithome.com.tw/articles/10405573)
2. 21
	[Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？](https://ithelp.ithome.com.tw/articles/10405767)
3. 22
	[Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？](https://ithelp.ithome.com.tw/articles/10406864)
4. 23
	[Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？](https://ithelp.ithome.com.tw/articles/10407137)
5. 24
	[Day 23｜代理（Agent）越自由越好嗎？為什麼最後會變得難以控制？](https://ithelp.ithome.com.tw/articles/10407401)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/19/4aec590c-6116-4ec2-870a-5622a84efd0c_small.png)

[Pentaho VS. Apache Hop - ETL 圖形化開發工具的新戰場](https://itplus.ithome.com.tw/vod-page/673?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

歐立威科技 |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/13/93b8e33d-d874-4f2c-af55-6bbfbb602804_small.jpg)

[Stacking Insight](https://itplus.ithome.com.tw/webinar-page/192?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

57 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/368ce414-1126-4859-a8ce-ab679373a83b_small.png)

[容器化實現證券核心轉型](https://itplus.ithome.com.tw/vod-page/1027?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

51 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/30af4504-308b-4298-b98e-aeec666e08b3_small.png)

[老闆，您敏捷和我們不一樣 - 大型混合敏捷團隊 ( Scrum, Kanban ) 的踩坑經驗](https://itplus.ithome.com.tw/vod-page/1080?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

36 分