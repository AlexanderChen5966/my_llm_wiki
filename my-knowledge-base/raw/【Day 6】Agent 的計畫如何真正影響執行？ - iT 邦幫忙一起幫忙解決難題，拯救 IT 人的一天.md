---
title: "【Day 6】Agent 的計畫如何真正影響執行？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401557"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "前幾天我們逐步建立了 Agent 的基本控制平面： Agent Loop Tool Runtime Permission Approval Sandbox Hooks 今天要進入另一個常被放大的能力：..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 6

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 6 篇

## 【Day 6】Agent 的計畫如何真正影響執行？

- 分享至

前幾天我們逐步建立了 Agent 的基本控制平面：

- Agent Loop
- Tool Runtime
- Permission
- Approval
- Sandbox
- Hooks

今天要進入另一個常被放大的能力：

> Planning。

很多 Agent Demo 會先要求模型：

> 請先制定完整計畫，再逐步執行。

模型很快就會產生一份看起來合理的清單：

1. 理解需求
2. 收集資料
3. 分析問題
4. 執行修改
5. 驗證結果
6. 回覆使用者

這份內容看起來像計畫。

但它可能完全沒有改變系統的執行方式。

如果 Agent 沒有保存進度、沒有追蹤每一步的狀態、沒有處理新資訊，也沒有在失敗後更新策略，那這份 Plan 很可能只是一段漂亮文字。

它看起來像 Planning，但沒有真正控制後續執行。

今天要回答三個問題：

1. 什麼才算真正的 Agent Planning？
2. Planning 應該存在於 Prompt、State 還是 Control Flow？
3. 什麼任務根本不需要 Planning？

---

## 列出步驟不等於擁有計畫

一份文字清單只是 Plan Artifact。

它描述模型當下認為可以採取的路徑。

但 Agent 真正需要的是：

- 知道目前做到哪裡
- 知道哪些步驟已完成
- 知道哪些步驟失敗
- 知道哪些步驟依賴其他結果
- 知道新資訊是否讓原計畫失效
- 知道什麼時候需要重新規劃
- 知道什麼條件下可以宣告完成

所以完整的 Planning 至少包含四層：

1. Plan：接下來準備做什麼
2. Execution State：每個步驟目前是 Pending、Running、Blocked、Failed 還是 Completed
3. Replanning：新資訊出現後，是否需要修改路徑
4. Verification：每個步驟完成後，如何確認結果真的有效

只有 Plan，沒有 State，系統不知道目前走到哪裡。

只有 State，沒有 Replanning，系統遇到變化時只能照舊執行。

只有 Replanning，沒有 Verification，系統可能一直換策略，卻不知道哪一步真的成功。

---

## Planning 的真正價值是壓縮決策

Planning 不是為了讓模型看起來更有條理。

它的價值是把一個模糊任務，轉換成一組可以追蹤與驗證的決策。

例如：

> 幫我改善這個專案的測試穩定性。

這個目標太大，Agent 很容易在不同方向之間反覆切換。

Planning 可以先拆出：

- 找出最常失敗的測試
- 區分環境問題與程式問題
- 只修正一個可重現失敗
- 執行局部驗證
- 再執行完整測試

這些步驟不是為了增加形式。

而是降低每一輪需要重新判斷的範圍。

Planning 的價值可以理解成：

> 把大問題拆成較小決策，再為每個決策設定可觀察結果。

如果 Plan 只是把大目標換成更多模糊句子，就沒有真正壓縮決策。

---

## Plan Artifact 應該是可操作的

一個可操作的 Plan，至少要回答：

- 目標是什麼
- 下一步是什麼
- 需要哪些輸入
- 預期產生什麼結果
- 完成條件是什麼
- 失敗後要怎麼處理
- 是否需要人類批准
- 是否存在依賴關係

例如「分析問題」太模糊。

更好的步驟是：

> 讀取最近三次失敗紀錄，找出重複出現的錯誤訊息，輸出最多三個候選原因。

這個步驟有明確輸入、行動與輸出。

Plan 的粒度不需要非常細。

但每一步都應該能被系統追蹤。

---

## 計畫需要狀態，不只是文字

如果系統只把計畫放在 Prompt 裡，模型可能下一輪就重新描述一份不同的計畫。

更可靠的方式，是把計畫視為任務狀態的一部分。

每個 Step 可以有：

- 名稱
- 狀態
- 依賴
- 嘗試次數
- 結果摘要
- 驗證結果
- 失敗原因

狀態可能包含：

- Pending：尚未開始
- Running：目前正在執行
- Blocked：等待輸入、權限或其他步驟
- Failed：已執行但未成功
- Completed：已完成並通過驗證
- Skipped：因為計畫改變而跳過

這些狀態讓 Harness 能回答：

- 下一步應該執行哪個 Step
- 哪些 Step 可以平行
- 哪些 Step 需要等待
- 哪些 Step 應該重試
- 哪些 Step 已經不再需要

如果所有進度都只存在模型的自然語言回覆裡，系統很難可靠恢復。

---

## 計畫不是不可修改的合約

真實環境會改變。

工具會失敗。

資料會缺失。

使用者會補充新需求。

原本合理的假設可能被新證據推翻。

所以 Plan 不應該被當成必須照表執行的固定腳本。

可以把 Planning 分成兩種模式。

### Static Plan

先產生完整步驟，再照順序執行。

適合：

- 環境穩定
- 任務路徑清楚
- 步驟少
- 依賴明確
- 失敗模式有限

### Adaptive Plan

先建立初始路徑，每得到新 Observation 後重新判斷是否需要調整。

適合：

- 研究
- Debug
- 搜尋
- 開放式分析
- 外部環境不穩定
- 多種路徑都可能成功

Adaptive Planning 比較靈活，但也更容易造成：

- 不斷改變方向
- 重複工作
- Plan Drift
- Token 與延遲增加
- 永遠無法收斂

因此 Replanning 也需要條件，而不是每一輪都重新規劃。

---

## 什麼時候應該 Replan？

Replanning 可以在特定事件發生時觸發。

### 關鍵假設被推翻

原本以為某個檔案存在，實際上不存在。

### 主要工具失敗

原本路徑無法執行，需要改用替代方式。

### 新資訊改變優先順序

使用者補充真正關心的範圍。

### Step 連續失敗

同一步驟已經多次無法完成。

### 預算不足

剩餘時間或 Token 無法支持原計畫。

### 驗證結果不通過

表面完成，但實際輸出不符合要求。

這些都是明確事件。

比起每輪都問模型「要不要重新規劃」，事件驅動的 Replanning 更容易控制。

---

## Planning 不能取代 Verification

一份完整 Plan 很容易讓人產生錯覺：

> 只要步驟完整，任務就會可靠完成。

但計畫只能描述意圖。

它不能證明執行結果。

例如 Plan 寫：

> 執行測試並確認全部通過。

模型也可能在沒有真正執行測試時，直接把這一步標記為完成。

所以每個重要 Step 都需要 Verification。

例如：

- 測試是否真的通過
- 指令 Exit Code 是否為零
- 預期檔案是否存在
- 輸出 Schema 是否正確
- API 是否真的回傳成功
- 使用者批准是否有效

Planning 決定要檢查什麼。

Verification 決定結果是否可信。

兩者不能互相取代。

---

## Planning 不能取代 Permission

模型可能計畫：

> 修改設定檔，然後部署到 Production。

即使這個計畫邏輯完整，也不代表 Agent 有權限執行。

Planning 處理任務分解。

Permission 處理操作是否允許。

Approval 處理高風險行動是否需要人類確認。

Sandbox 處理出錯時的影響範圍。

一份合理的計畫不能跳過任何控制層。

---

## Plan 需要 Budget

Planning 本身也有成本。

模型可能花很多 Token 產生一份非常詳細的計畫。

之後每輪又重新讀取、更新與解釋整份計畫。

如果任務本來只需要一次工具呼叫，Planning 反而增加延遲。

因此 Plan 應該有自己的限制：

- 最多幾個 Step
- 每個 Step 最多幾次嘗試
- 最多幾次 Replan
- 計畫可以使用多少 Token
- 什麼情況下直接執行
- 什麼情況下停止並要求人類介入

Planning 不是免費的。

只有當它降低錯誤、重工或決策複雜度時，才值得加入。

---

## 不是所有任務都需要 Planning

簡單任務通常不需要先建立完整 Plan。

例如：

- 查詢一個明確資訊
- 讀取一個檔案
- 修改一個已知欄位
- 執行一個固定工具
- 回覆一個短問題

對這些任務，Planning 可能只是額外的 Token 與延遲。

比較需要 Planning 的任務通常具有：

- 多步驟
- 長時間執行
- 多個依賴
- 多個工具
- 高成本操作
- 可恢復需求
- 人類批准節點
- 明確驗證要求
- 中途可能出現新資訊

可以先問：

> 如果不先建立狀態化計畫，Agent 最可能在哪裡迷路、重複或過早完成？

如果答案不明確，可能不需要 Planning。

---

## Planning Granularity

Plan 太粗，不能控制。

Plan 太細，維護成本過高。

例如：

### 太粗

- 分析專案
- 修正問題
- 驗證結果

每一步仍然需要大量臨場決策。

### 太細

- 打開第一個目錄
- 讀取第一個檔案
- 讀取第二個檔案
- 搜尋第一個關鍵字
- 搜尋第二個關鍵字

這會讓 Plan 很快失效，也增加狀態管理成本。

比較實用的粒度是：

> 每個 Step 對應一個清楚的中間結果，並且可以被驗證。

例如：

- 找出失敗最頻繁的測試
- 建立一個可重現案例
- 修正造成失敗的最小程式範圍
- 執行局部與完整驗證

---

## Plan 可以由誰產生？

不一定要由同一個模型完成所有 Planning。

### 同一個 Agent 先規劃再執行

最簡單，但規劃與執行可能共享相同盲點。

### Planner 與 Executor 分離

Planner 負責拆解，Executor 負責執行。

優點是責任清楚。

缺點是增加模型呼叫與協調成本。

### Harness 提供固定骨架

程式碼先定義必須經過的節點，模型只填寫不確定部分。

例如固定要求：

- 先收集證據
- 再提出修改
- 最後執行驗證

這能減少模型自由度，也更容易評估。

### 人類建立目標與限制

使用者提供重要步驟、優先順序或批准點，模型只負責補充執行細節。

Production 系統常常是混合這些方法。

---

## Planning 與 Graph 的差別

Planning 通常由模型根據任務產生或調整。

Graph 則通常由程式碼預先定義控制節點與路徑。

如果某個流程每次都必須先驗證，再允許部署，就不一定需要模型重新規劃。

可以直接寫進控制流程。

Planning 適合不確定部分。

Graph 適合已知且必須強制執行的部分。

這也是後面 Graph Engineering 會進一步討論的核心。

---

## 常見的錯誤設計

### 1\. 把漂亮清單當作 Planning

如果清單不影響 State 與執行，它只是文字。

### 2\. Plan 永遠不更新

新 Observation 已經讓原路徑失效，Agent 仍照舊執行。

### 3\. 每一輪都 Replan

造成方向漂移、重複工作與成本增加。

### 4\. 模型自行宣告 Step 完成

重要步驟應有外部 Verification。

### 5\. 簡單任務也強制產生完整 Plan

增加 Token 與延遲，沒有明顯收益。

### 6\. Plan 太細

環境稍微改變，整份計畫就失效。

### 7\. Plan 沒有 Budget

Agent 可能花大量時間規劃與改寫，而不是完成任務。

### 8\. Planning 跳過 Permission

合理計畫不代表擁有執行權限。

---

## 如何設計第一版 Planning？

第一版可以先加入五個欄位：

- Step
- Status
- Expected Result
- Verification
- Failure Reason

再加入三個限制：

- 最大 Step 數量
- 每個 Step 最大嘗試次數
- 最大 Replan 次數

Replan 只在明確事件發生時觸發：

- 關鍵假設失敗
- Step 多次失敗
- 使用者需求改變
- Verification 不通過
- Budget 不足

這已經比「先列出一份計畫」更接近真正的 Planning System。

可參考：https://github.com/hardness1020/awesome-agent-architecture/tree/main/sections/05-planning-todos

---

## 今天新增了什麼能力？

Day 5 的 Hooks 讓我們可以在關鍵 Lifecycle Event 插入自訂邏輯。

今天加入：

- Plan Artifact
- Step State
- Dependency
- Expected Result
- Verification
- Replanning Trigger
- Planning Budget

因此，Plan 不再只是模型輸出的一段文字。

它開始成為可以追蹤、恢復、修改與驗證的任務狀態。

---

## 今天的結論

Planning 的價值不是讓模型先說明自己準備做什麼。

而是把模糊目標轉換成可追蹤、可驗證、可調整的執行狀態。

真正的 Planning 至少包含：

- Plan：準備做什麼
- State：目前做到哪裡
- Replanning：什麼時候改變路徑
- Verification：如何證明每一步有效

最重要的原則是：

> 計畫必須能在執行中被追蹤，也必須能在現實改變時被修正。

下一篇會進入 Subagent：

> 把任務交給另一個 Agent，真的等於平行處理嗎？還是只是把 Context 與責任重新切分？

**完整系列與範例收錄於：https://github.com/hardness1020/awesome-agent-architecture**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401629)

[

【Day 5】擴充 Agent 驗證、紀錄與自訂行為

](https://ithelp.ithome.com.tw/articles/10401629)[下一篇](https://ithelp.ithome.com.tw/articles/10401890)

[

【Day 7】Subagent 不等於平行處理：先切 Context、責任與權限，再談 Multi-Agent

](https://ithelp.ithome.com.tw/articles/10401890)

系列文

[《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》](https://ithelp.ithome.com.tw/users/20183246/ironman/9121) 共 7 篇

目錄

1. 3
	[【Day 3】Agent 選了工具，誰負責真的執行？](https://ithelp.ithome.com.tw/articles/10401298)
2. 4
	[【Day 4】Agent 能使用工具，不代表它應該擁有所有權限](https://ithelp.ithome.com.tw/articles/10401436)
3. 5
	[【Day 5】擴充 Agent 驗證、紀錄與自訂行為](https://ithelp.ithome.com.tw/articles/10401629)
4. 6
	[【Day 6】Agent 的計畫如何真正影響執行？](https://ithelp.ithome.com.tw/articles/10401557)
5. 7
	[【Day 7】Subagent 不等於平行處理：先切 Context、責任與權限，再談 Multi-Agent](https://ithelp.ithome.com.tw/articles/10401890)
[完整目錄](https://ithelp.ithome.com.tw/users/20183246/ironman/9121)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/7659574e-4e9e-4ad3-bfcf-7843d2bb0936_small.jpg)

[透過Headless CMS讓前端與行銷團隊高效合作](https://itplus.ithome.com.tw/vod-page/151?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/06b37579-92a2-4be3-a8ea-9ca641c74caa_small.png)

[別人的 K8s 死袂了 - Kubernetes Failure Stories](https://itplus.ithome.com.tw/vod-page/562?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

22 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/dbfdba4a-8f61-4ea5-be4a-2c9e66e5471e_small.png)

[基於 BPMN 的微服務設計方法](https://itplus.ithome.com.tw/vod-page/886?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/17/d985abc8-a53e-4f64-91b6-a93d33a03af6_small.png)

[90 秒就裝進你的 CI——動手攔下一次真實的 npm 供應鏈攻擊（JFrog Curation 實作）](https://itplus.ithome.com.tw/vod-page/1460?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

85 分