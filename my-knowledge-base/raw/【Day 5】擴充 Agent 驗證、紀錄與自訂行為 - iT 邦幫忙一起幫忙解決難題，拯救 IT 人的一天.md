---
title: "【Day 5】擴充 Agent 驗證、紀錄與自訂行為 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10401629"
author:
  - "[[iThome]]"
published:
created: 2026-08-10
description: "前幾天我們逐步建立了一個最小 Agent 架構。 目前系統已經有： Agent Loop Tool Runtime Schema Validation Permission Approval Sand..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 5

1

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 《30 天從零拆解 AI Agent：從 Tool Calling 到多 Agent 協作》系列 第 5 篇

## 【Day 5】擴充 Agent 驗證、紀錄與自訂行為

- 分享至

前幾天我們逐步建立了一個最小 Agent 架構。

目前系統已經有：

- Agent Loop
- Tool Runtime
- Schema Validation
- Permission
- Approval
- Sandbox

接下來很容易出現一個問題。

每當我們想加入新功能，就直接修改 Agent Loop。

想記錄工具使用，就在 Loop 裡加 Log。

想阻擋危險操作，就在 Loop 裡加條件判斷。

想在任務完成前執行驗證，就再加入一段流程。

想記錄 Token、延遲與成本，又繼續修改核心程式。

最後，原本簡單的 Loop 會同時負責：

- 流程控制
- 安全政策
- Logging
- Metrics
- Validation
- Notification
- Output Processing
- Evaluation
- Business Rules

每一個功能單獨看都合理。

但如果全部寫進核心 Loop，系統會越來越難理解、測試與修改。

今天要介紹一個常見的擴充方法：

> Hooks 與 Lifecycle Events。

它們讓我們可以在 Agent 執行的特定時間點插入自訂行為，而不需要一直改動 Loop 本身。

---

## 什麼是 Hook？

Hook 是一個預先定義好的擴充點。

Agent 執行到某個階段時，Harness 會觸發對應事件，讓外部邏輯在那個時間點執行。

常見事件包括：

- 任務開始前
- 模型呼叫前
- 模型回應後
- 工具執行前
- 工具執行後
- 任務準備結束前
- 任務確定結束後

這些時間點可以稱為 Lifecycle Events。

Hook 則是掛在 Event 上的處理邏輯。

可以把它理解成：

> Lifecycle Event 定義「什麼時候」，Hook 定義「要做什麼」。

例如，工具執行前可以觸發：

- 記錄工具名稱與參數
- 檢查 Permission
- 要求 Approval
- 移除 Secret
- 阻擋危險操作

工具執行後則可以：

- 記錄結果與延遲
- 截斷過大的輸出
- 正規化錯誤
- 更新 Metrics
- 建立 Audit Trace

核心 Loop 只需要負責觸發事件。

具體功能則由各自的 Hook 處理。

---

## 為什麼不直接寫進 Loop？

假設一開始只有一個需求：

> 在每次工具執行前記錄工具名稱。

直接加進 Loop 看起來非常簡單。

但很快又會出現：

- 記錄參數
- 遮蔽 Secret
- 計算執行時間
- 傳送 Trace 到監控系統
- 阻擋特定工具
- 對輸出做截斷
- 高風險操作前通知使用者
- 錯誤發生後保存診斷資訊

如果所有邏輯都在同一個函式中，會造成幾個問題。

### 1\. 核心流程難以閱讀

你很難快速看出 Agent 真正的控制流程。

到底是什麼讓它繼續？

又是什麼讓它停止？

答案會被淹沒在大量周邊邏輯中。

### 2\. 每次新增功能都可能破壞 Loop

即使只是加入一個 Log，也可能意外影響錯誤處理、Retry 或停止條件。

### 3\. 測試變得困難

要測試一個小功能，卻必須啟動整個 Agent。

### 4\. 不同環境難以組合

開發環境可能需要完整 Trace。

Production 需要嚴格 Security Hook。

Evaluation 環境則需要保存每一輪輸入輸出。

如果全部寫死在 Loop，就會出現大量環境判斷。

Hooks 的目的，就是把這些跨模組功能拆出去。

---

## 常見的 Lifecycle Events

不同 Framework 的命名不一定相同，但概念通常接近。

### Agent Start

任務開始時觸發。

適合：

- 建立 Trace ID
- 初始化 Metrics
- 載入任務設定
- 檢查 Budget
- 建立暫存 Workspace

### Before Model

每次呼叫模型前觸發。

適合：

- 組裝動態 Context
- 加入目前任務狀態
- 計算 Token
- 套用 Context Policy
- 移除不應送入模型的資料

### After Model

模型回應後觸發。

適合：

- 記錄模型輸出
- 檢查結構是否合法
- 計算延遲與成本
- 偵測異常回應
- 收集 Evaluation Data

### Before Tool

工具真正執行前觸發。

適合：

- Permission Check
- Approval Gate
- 參數清理
- Secret Redaction
- Audit Log
- 阻擋危險操作

### After Tool

工具執行完成後觸發。

適合：

- 記錄結果
- 計算執行時間
- 截斷大型輸出
- 正規化錯誤
- 更新 Metrics
- 建立 Artifact

### Agent Stop

Agent 準備停止時觸發。

適合：

- Completion Check
- 結果驗證
- 判斷是否允許結束
- 保存 Summary
- 觸發 Human Handoff

### Agent End

任務確定結束後觸發。

適合：

- 寫入 Audit
- 清理暫存資源
- 發送通知
- 保存 Evaluation Record
- 更新任務狀態
- 計算總成本

Hooks 的價值不只是讓「任何功能都能插入」。

而是每個擴充功能都有清楚的執行時機。

---

## Hook 可以觀察、修改，也可以控制流程

Hooks 可以大致分成三類。

### 1\. Observe

只觀察，不修改 Agent 行為。

例如：

- 記錄工具名稱
- 計算模型延遲
- 統計 Token
- 保存 Trace
- 收集 Evaluation Data

這類 Hook 風險最低。

即使非關鍵 Metrics 暫時失敗，通常也不應該讓主要任務停止。

### 2\. Transform

修改輸入或輸出。

例如：

- 在模型呼叫前加入動態 Context
- 截斷過大的 Tool Result
- 移除敏感欄位
- 正規化錯誤格式
- 將資料轉換成模型容易理解的格式

這類 Hook 會改變 Agent 看到的資訊，所以必須留下 Trace。

否則 Debug 時會出現一個問題：

> 為什麼工具原始輸出和模型實際看到的內容不同？

### 3\. Control

阻擋、暫停或改變流程。

例如：

- 拒絕工具執行
- 要求人類批准
- 超過 Budget 後停止
- 阻止 Agent 過早結束
- 驗證失敗後要求下一輪修正

這類 Hook 最強，也最危險。

因為它不只是觀察，而是直接改變 Agent 的控制流程。

---

## Hook 結果應該是結構化的

如果 Hook 只能觀察，回傳值可能不重要。

但如果 Hook 可以控制流程，就需要定義清楚結果。

例如：

- Continue：繼續執行
- Modify：使用修改後的資料
- Block：阻擋目前操作
- Pause：暫停並等待 Approval
- Retry：重新執行某個步驟
- Stop：終止任務

不要把所有政策決定都表示成 Exception。

因為 Exception 通常表示程式發生非預期錯誤。

但 Permission Hook 回傳 Block，可能是完全正常的系統行為。

> Hook Exception 代表 Hook 可能壞了，Hook Block 代表 Hook 正常做出阻擋決定。

兩者不應混為一談。

---

## Hook 的執行順序很重要

假設工具執行前有三個 Hook：

1. Redaction Hook
2. Audit Hook
3. Permission Hook

不同順序可能產生不同結果。

如果先記錄再 Redact，Audit Log 可能包含 Secret。

如果先修改參數再檢查 Permission，Policy 看到的是修改後請求。

如果 Permission 已經拒絕，後續 Hook 是否還需要執行？

因此 Hook System 必須定義：

- 執行順序
- 是否可以平行
- 某個 Hook Block 後是否立即停止
- Transform 是否影響後續 Hook
- 多個 Hook 同時修改資料時如何處理

最簡單的方式是設定明確 Priority。

例如：

1. Input Sanitization
2. Permission Check
3. Approval Check
4. Audit
5. Execution

順序應該是架構的一部分，而不是依賴註冊先後的巧合。

---

## Hook 失敗時怎麼辦？

Hook 自己也可能出錯。

例如：

- Logging Service 無法連線
- Metrics Backend Timeout
- Security Hook 無法取得 Policy
- Redaction Hook 發生錯誤
- Notification 發送失敗

不同 Hook 的失敗策略應該不同。

### Fail Open

Hook 失敗，但主要任務繼續。

適合：

- 非必要 Metrics
- 開發環境 Logging
- 次要通知

### Fail Closed

Hook 失敗，操作必須停止。

適合：

- Permission
- Security Policy
- Approval
- Secret Protection

### Record and Continue

記錄 Hook 錯誤，但不阻擋任務。

適合不影響安全的觀察型 Hook。

因此每個 Hook 都應該聲明：

> 這是關鍵控制，還是附加觀察？

否則一個 Metrics Service 故障，可能讓所有 Agent 停擺。

反過來，Permission Hook 故障後仍然繼續，也可能造成安全問題。

---

## Hooks 不是 Business Logic 垃圾桶

Hooks 很方便，所以也很容易被濫用。

任何不想放進 Loop 的邏輯，都可能被塞進 Hook。

最後 Hook System 反而變成另一個看不見的複雜流程。

### 適合放在 Hook 的功能

- Logging
- Metrics
- Permission
- Redaction
- Trace
- Notification
- 通用驗證
- Evaluation Data Collection

這些通常是 Cross-cutting Concerns，也就是會跨越多個工具或流程的共同能力。

### 不適合放在 Hook 的功能

- 任務主要步驟
- 核心 Business Workflow
- 必須按照明確順序完成的流程
- 失敗後有複雜補償邏輯的流程

例如：

1. 取得訂單
2. 計算退款
3. 建立退款
4. 更新帳務
5. 通知客戶

這是主要 Workflow，不應該偷偷藏在 Hooks 中。

> Hook 適合擴充控制平面，不適合隱藏主要業務流程。

---

## Hook 和 Middleware 有什麼差別？

Hook 與 Middleware 很像，但思考方式略有不同。

### Hook

在某個事件發生時觸發。

例如：

- Before Tool
- After Tool
- Agent End

適合事件導向的擴充。

### Middleware

通常包住一段完整執行流程。

適合：

- Authentication
- Retry
- Timeout
- Rate Limit
- Caching
- Transaction

實務上兩者常會混合。

重點不在名稱，而在責任是否清楚。

---

## Hooks 如何幫助 Evaluation？

Hooks 很適合收集 Evaluation Data。

模型呼叫後可以記錄：

- 模型名稱
- Token 使用量
- 延遲
- Tool Call
- Stop Reason

工具執行後可以記錄：

- 工具名稱
- 成功或失敗
- 執行時間
- 結果大小
- Retry 次數

任務結束後可以記錄：

- 是否完成
- 是否通過驗證
- 總成本
- 總輪數
- Human Handoff
- 最終 Failure Mode

這些資料可以回答：

- 哪個工具最常失敗？
- Agent 最常在哪一輪卡住？
- 哪種 Permission 最常被拒絕？
- 加入 Verification Hook 後，錯誤完成率是否下降？
- 某個 Hook 增加了多少延遲？

架構機制是否有效，最後還是要用 Evaluation 證明。

---

## 如何設計第一版 Hook System？

第一版不需要支援所有事件。

可以先從四個最有價值的 Event 開始：

- Before Model
- After Model
- Before Tool
- After Tool

再定義三種行為：

- Observe
- Transform
- Block

每個 Hook 至少要有：

- 名稱
- Event
- Priority
- Failure Policy
- 是否可以修改資料
- 執行時間限制

這樣已經足以支援：

- Audit
- Metrics
- Permission
- Redaction
- Output Truncation
- 基本 Evaluation

等到真的有 Failure Mode，再加入 Agent Start、Agent Stop 或 Approval Resume 等更細事件。

---

## 今天新增了什麼能力？

Day 4 的系統已經有 Permission、Approval 和 Sandbox。

但這些邏輯仍然可能直接寫在 Agent Loop 裡。

今天加入：

- Lifecycle Events
- Hook Manager
- Observe Hook
- Transform Hook
- Control Hook
- Priority
- Failure Policy

因此，我們可以在不一直修改核心 Loop 的情況下，加入：

- Logging
- Metrics
- Permission
- Approval
- Redaction
- Verification
- Notification
- Evaluation

核心 Loop 保持簡單。

擴充行為則可以獨立開關、測試與組合。

---

## 今天的結論

Agent Loop 應該負責核心控制流程。

它不應該同時承擔每一個安全、觀察、通知與商業需求。

Hooks 提供明確的 Lifecycle Events，讓系統能在特定時機加入自訂行為。

最重要的原則是：

> 核心 Loop 決定 Agent 如何前進，Hooks 負責在關鍵節點觀察、修改或阻擋。

但 Hooks 也不能無限制使用。

如果主要任務流程全部藏進 Hook，系統只會從「複雜的 Loop」變成「看不見的複雜 Hook」。

下一篇會進入 Planning：

> Agent 為什麼需要先拆解任務？一份看起來完整的計畫，真的能讓執行更可靠嗎？

**完整系列與範例收錄於：https://github.com/hardness1020/awesome-agent-architecture**

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10401436)

[

【Day 4】Agent 能使用工具，不代表它應該擁有所有權限

](https://ithelp.ithome.com.tw/articles/10401436)[下一篇](https://ithelp.ithome.com.tw/articles/10401557)

[

【Day 6】Agent 的計畫如何真正影響執行？

](https://ithelp.ithome.com.tw/articles/10401557)

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