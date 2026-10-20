---
title: "Day 26｜AI 一定會失敗：逾時（Timeout）、重試（Retry）、備援（Fallback）怎麼設計？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408173"
author:
  - "[[iThome]]"
published:
created: 2026-09-08
description: "到了 Day 26，Data Machi 的核心能力其實已經逐漸完整。我們可以透過 RAG 查詢文件、利用 Tool 取得結構化數據，讓 Coordinator 選擇適合的工具，也開始使用 LangG..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 27

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 27 篇

## Day 26｜AI 一定會失敗：逾時（Timeout）、重試（Retry）、備援（Fallback）怎麼設計？

- 分享至

到了 Day 26，Data Machi 的核心能力其實已經逐漸完整。我們可以透過 RAG 查詢文件、利用 Tool 取得結構化數據，讓 Coordinator 選擇適合的工具，也開始使用 LangGraph 將原本藏在 Agent Loop 裡的流程拆成 State、Node 與 Edge。從架構來看，一個企業 Agent 的主要骨架已經慢慢形成。

但如果把目前的版本直接放到正式環境，很快就會碰到另一個比 Prompt 或 Agent 邏輯更現實的問題： **外部服務一定會失敗。**

模型 API 可能突然超過等待時間，Google Sheets 或其他第三方 API 可能遇到 Rate Limit，服務可能暫時回傳 503，網路連線也可能短暫中斷。甚至服務正常回應，也不代表得到的資料一定完整。因此正式產品不能把這些情況視為罕見的「例外」，而應該從一開始就把失敗視為 Workflow 的正常分支。

可靠性的核心問題不再只是「服務有沒有成功」，而是： **失敗發生之後，系統知不知道接下來該怎麼做？**

## Timeout：不要讓一個外部服務拖住整條 Workflow

第一個最基本的可靠性機制是 **Timeout（逾時）** 。任何需要呼叫外部服務的 Tool，都不應該允許無限等待。如果 Google Sheets API、模型服務或 Project Tool 長時間沒有回應，系統應該在合理時間後主動結束這次呼叫，把目前狀態標記為失敗，再由 Workflow 判斷下一步。

沒有 Timeout 的問題並不只是「使用者需要多等一下」。假設一個 LangGraph Workflow 已經依序完成資料查詢、文件搜尋與結果整合，最後卡在其中一個外部 API，如果沒有等待上限，整條流程可能持續佔用資源，前端也不知道現在到底仍在運算，還是服務其實已經沒有回應。

因此 Timeout 本質上是一條很清楚的系統邊界： **我願意等待這個服務多久？超過之後，我就把它視為這一次執行失敗，而不是繼續無限等待。**

不過 Timeout 並沒有一個適合所有任務的固定數字。像 Router 判斷「這個問題應該查數據還是文件」，通常應該是快速任務，可以設定較短等待時間；但長篇文件摘要、複雜資料分析或最終報告生成，本來就需要更多運算時間，因此可以允許較長的 Timeout。可靠性設計不是替所有 Node 設定同一個 30 秒，而是根據任務特性、服務正常延遲與使用者等待成本來決定。

## Retry：失敗之後，不是所有情況都應該再試一次

Timeout 發生後，很自然會想到第二個機制： **Retry（重試）** 。但 Retry 最重要的設計原則不是「失敗就再試一次」，而是先判斷： **這個錯誤重新執行之後，有沒有合理機會恢復？**

例如短暫網路不穩、Timeout、429 Rate Limit，或部分 5xx Server Error，通常屬於暫時性錯誤。第一次呼叫失敗，不代表第二次一定會失敗，因此可以有限度 Retry。

相反地，如果 API 回傳 401 認證錯誤、403 權限不足，或 Request Input 本身格式錯誤，這些問題通常不會因為多呼叫五次就突然消失。如果 Credential 不正確，系統真正需要的是修正 Credential；如果使用者沒有權限，系統需要的是說明權限問題，而不是繼續消耗 API Call。

因此 Retry Policy 應該建立在 Error Classification 上，而不是用一條通用的 `try again` 處理所有失敗。

| 錯誤類型 | 是否適合 Retry | 建議處理 |
| --- | --- | --- |
| Timeout | 是 | 有限制地重試 |
| 429 Rate Limit | 是 | 等待後重試 |
| 暫時性 5xx | 視情況 | 有限制地重試 |
| 401 Authentication Error | 否 | 停止並檢查 Credential |
| 403 Permission Denied | 否 | 停止並說明權限不足 |
| Invalid Input | 否 | 修正輸入或要求使用者補充 |
| No Result | 通常不是單純 Retry | 改寫 Query、補查或回報沒有結果 |

這樣做的好處是，系統開始知道「失敗的原因不同，下一步也應該不同」，而不是把所有問題都當成暫時性故障。

## Retry 不能太積極：加入 Exponential Backoff

即使錯誤適合 Retry，也不代表應該立刻連續重試。例如 API 因流量過高回傳 429，如果系統在幾毫秒內立刻重新送出相同 Request，很可能只是繼續收到相同錯誤，甚至進一步加重服務負擔。

因此常見做法是搭配 **Exponential Backoff（指數退避）** 。第一次失敗後先等一小段時間，第二次等待更久，再進行下一次嘗試。概念上可能是第一次等待 1 秒，第二次 2 秒，第三次 4 秒，而不是一直以固定高頻率重新請求。

實務上通常還會加入一點隨機延遲，也就是 Jitter，避免大量使用者同時遇到故障後，又在完全相同的時間重新發送 Request，形成另一波瞬間流量。

不過最重要的仍然是要有 **Retry Limit** 。例如最多嘗試兩次或三次，超過之後就必須決定 Fallback、Partial Success 或停止，而不是讓 Retry 自己變成另一個無限循環。

## Fallback：主要服務失敗時，是否還有第二條路？

當 Retry 仍然失敗，下一個可以思考的是 **Fallback（備援）** 。

以 LLM 為例，主要模型暫時不可用時，可以切換到另一個適合的模型；如果某項工作不需要最高等級的模型能力，也可能使用較小、較快的模型完成。這時 Fallback 的目的不是讓使用者完全感覺不到故障，而是盡可能讓核心任務仍然可以完成。

資料來源也可能設計 Fallback。例如即時來源暫時不可使用時，如果系統保存了最近一次已驗證的結果，可以在特定情境下使用 Cache。但這裡需要非常小心：如果使用者問的是「現在庫存多少」，卻偷偷拿三小時前的 Cache 當成即時資料回答，這不叫可靠性，而只是把服務錯誤藏起來。

因此如果使用舊資料作為 Fallback，至少應該清楚保留資料時間，例如告訴使用者目前即時來源無法存取，以下結果來自最近一次成功查詢。 **Fallback 的目的，是降低服務中斷造成的影響，不是讓系統假裝什麼事情都沒有發生。**

如果沒有可信的替代模型、替代來源或可接受的 Cache，那最可靠的選擇反而是停止並清楚說明目前無法完成哪一部分。

## Timeout、Retry、Fallback 和 Verification 解決的是不同問題

這幾個可靠性機制很容易被混在一起，但它們處理的其實是不同層次。

| 機制 | 解決的核心問題 |
| --- | --- |
| Timeout | 一次外部呼叫最多等多久 |
| Retry | 暫時性失敗是否值得重新嘗試 |
| Fallback | 主要服務失敗後是否有替代路徑 |
| Verification | 成功取得結果後，內容是否真的可靠可用 |

例如 Gemini 呼叫超過 Timeout，系統可以先 Retry；Retry 兩次仍失敗，就切換 Fallback Model。但備援模型成功產生結果之後，仍然不能直接假設答案一定正確，後面還要進入 Verification。

這也表示可靠性不是一條「只要 API 回 200 就成功」的判斷，而是一連串不同層次的品質控制。

## Verification：有 Response，不代表真的成功

前面 Day 22 與 Day 25 已經多次介紹 Verification。到了正式產品環境，它同樣是 Reliability 的一部分。

Tool 即使成功回傳，也可能出現很多「技術上成功、業務上失敗」的情況。例如 API 回傳空陣列、來源 Schema 剛好修改、欄位消失、JSON 被截斷，或者模型只生成了一半答案。

假設我們要求模型輸出固定 JSON：

```json
{
  "summary": "...",
  "decisions": [],
  "action_items": []
}
```

服務確實回傳 HTTP 200，但內容只有：

```json
{
  "summary": "..."
```

這仍然是一次失敗，因為 Output 不符合下游 Workflow 需要的結構。

因此 Verification Node 可以檢查回傳格式、必填欄位、資料來源、數字一致性，以及回答是否超出 Tool Result 可以支持的範圍。當發現不完整輸出時，可以依任務性質決定 Retry、Repair，或直接回報資料不完整。

可靠性真正應該關心的是： **這個結果能不能安全地被下一個 Node 使用？**

## 不同 Node 可以有不同的 Reliability Policy

到了 LangGraph Workflow，Reliability Policy 也不必全系統只有一套。

例如 Router 只是判斷問題屬於 Data、Document 還是 Project。這是一個快速、低成本，而且即使失敗也容易重新執行的任務，因此可以使用較快模型、較短 Timeout，失敗後快速 Retry 一次。

另一方面，最終回答可能已經整合多個 Tool Result，需要較長 Context，甚至生成較完整的分析。這時可以允許較長 Timeout，並在模型失敗時使用 Fallback Model。

如果是 Send Email 或 Update Database 這類 Action Node，可靠性要求又不同。這類操作不應該因 Timeout 就直接重新執行，因為第一次 Request 有可能其實已經成功，只是 Response 沒有正常回到我們手上。如果無條件 Retry，就可能重複寄兩封信或建立兩筆 Task。

因此企業 Workflow 的 Reliability 不只和「什麼服務」有關，也和「這個 Node 正在做什麼」有關。

## Read Tool 和 Action Tool 的 Retry 策略不能完全相同

這是一個很重要的差異。

例如查 Google Sheets 發生 Timeout，重新查一次通常問題不大，因為它只是 Read Operation。但如果 `create_task()` 發生 Timeout，情況就變得更複雜：我們不知道 Task 是完全沒有建立，還是其實已經建立成功，只是回應在網路中途遺失。

這時如果直接 Retry：

`create_task() → Timeout → create_task() again`

就可能得到兩個一模一樣的 Task。

因此 Action Tool 通常還需要考慮 **Idempotency（冪等性）** ，也就是同一個操作即使因為網路問題重新送出，也不會產生重複副作用。例如每一個 Action 帶一個唯一 Request ID，外部系統如果已經處理過，就不再重複建立。

這也再次提醒我們：Retry 不是一條可以套在所有 Tool 外面的簡單規則。Read、Write 與高風險 Action，需要不同策略。

## 錯誤訊息本身也是產品體驗

Reliability 不只是 Backend 的事情。當某個 Tool 真正失敗時，使用者看到什麼，也會直接影響他對產品的信任。

最沒有幫助的訊息通常是：

`Something went wrong.`

使用者不知道出了什麼問題、不知道是不是自己做錯，也不知道接下來該做什麼。

比較好的錯誤訊息應該回答三件事： **哪一部分失敗、哪些部分仍然可用，以及使用者接下來能做什麼。**

例如 Google Sheets 暫時無法讀取，但 PDF RAG 正常，可以說明目前無法取得最新數字，但文件查詢仍然可用。如果是 Permission Denied，可以告知目前沒有該資料來源的讀取權限；如果只是使用者少填了一個必要條件，就應該請他補充，而不是顯示技術錯誤。

錯誤訊息因此也應該跟前面的 Error Classification 對應，而不是所有異常最後都被轉換成相同的紅色 Error Banner。

## Partial Success：一個來源失敗，不代表所有成果都要丟掉

多來源 Agent Workflow 特別需要 Partial Success。

假設使用者問：「哪個市場問題最多？正式定義是什麼？改善專案目前做到哪裡？」系統同時需要 Data Tool、Document Tool 與 Project Tool。如果前兩個都成功，只有 Project Tool Timeout，全部回覆「系統錯誤」其實浪費了已經取得的可信資訊。

更合理的方式，是保留已成功結果，告訴使用者目前已確認哪個市場問題最多以及正式定義，但專案系統暫時無法取得最新進度。

因此 Workflow State 不應該只有一個：

`status = success / failed`

而可以保留每個 Tool 的 Status，讓 Coordinator 最後判斷這次任務是 Complete、Partial Success，還是完全無法交付。

這種設計對多來源企業 Agent 特別重要，因為 Tool 越多，「全部服務永遠同時正常」就越不應該被當成前提。

## Log 要能幫忙除錯，但不能變成另一個風險

當系統開始加入 Timeout、Retry 與 Fallback，後端 Log 就非常重要。否則使用者只說「剛剛失敗了」，工程師很難知道到底是第一個模型 Timeout、Google API 回 429，還是 Fallback Model 也失敗。

至少可以記錄 Request ID、Node、Tool Name、Error Type、HTTP Status、Retry Count、Duration，以及是否使用 Fallback。這些資訊已經足以回答很多 Reliability 問題。

但 Log 也不能為了方便除錯，就把所有內容完整留下。API Key、Access Token、Credential、完整 Prompt、使用者敏感資料，都不應該直接寫入普通 Log。尤其 Agent 可以存取企業資料後，Observability 本身也必須受到資料治理限制。

因此好的 Log 要做到兩件看似矛盾的事： **足以讓工程師找到問題，但又不保存不必要的敏感資訊。**

## 怎麼測試 Reliability？不要等正式環境真的壞掉

可靠性不能只等真實事故發生才驗證。開發階段就可以刻意製造各種失敗情境。

例如故意使用錯誤 API Key，確認系統收到 Authentication Error 後立即停止，而不是不斷 Retry；模擬 API Timeout，確認 Loading 可以正常結束，而且 Workflow 真的走向 Retry 或 Fallback；讓主要模型持續失敗，確認備援模型會被啟用；或者讓 Tool 回傳不完整 JSON，確認 Verification 能攔下結果，而不是直接傳給下一個 Node。

可以整理一套 Reliability Test：

| 測試案例 | 預期行為 |
| --- | --- |
| 模型 Timeout | 結束等待 → 有限制 Retry |
| 429 Rate Limit | Backoff 後 Retry |
| 401 Invalid API Key | 不 Retry，回報認證問題 |
| 403 Permission Denied | 不 Retry，說明權限不足 |
| 主要模型持續失敗 | 切換 Fallback |
| Tool 回傳空結果 | Verification 判斷是否可接受 |
| JSON 被截斷 | Validation 失敗，不送往下游 |
| 一個來源失敗 | 保留其他結果，回傳 Partial Success |
| Action Timeout | 確認 Idempotency，不盲目重新執行 |

可靠性測試的目標不是證明「系統不會失敗」，而是證明：

**每一種已知失敗發生時，Workflow 都有合理而可預期的路徑。**

## Circuit Breaker：當服務明顯故障時，不要每個人都重新撞一次

如果某個外部 API 已經連續失敗很多次，還有一個常見機制叫 **Circuit Breaker（斷路器）** 。

可以把它想成家裡的電路跳閘。發現系統持續異常之後，並不是讓每一台電器一直重新嘗試通電，而是先暫時切斷，避免問題擴大。

套到 Data Machi，假設 Project API 已經連續多次回 503。如果沒有 Circuit Breaker，每一個新使用者都會經歷一次 Timeout、Retry、再 Timeout，不只體驗很差，也會繼續增加已經故障服務的負擔。

Circuit Breaker 可以暫時把這個來源標記為 unavailable，在一段時間內直接走 Fallback 或回傳清楚狀態。等健康檢查確認服務恢復後，再重新開放。

這個機制在小型 Demo 裡可能還不是必要，但當使用者數量與外部服務數量增加後，就會逐漸變得重要。

## Data Machi 的可靠性應該放在哪一層？

做到這裡，可以把 Reliability Responsibility 分成兩層。

第一層是 **Tool Layer** 。每個 Tool 應該負責最基本的 Timeout、API Response Parsing、Error Classification 與必要的資料格式驗證。Tool 不應該把所有異常都包成一個 `None` ，否則 Workflow 根本不知道發生的是 Permission Error、Timeout 還是 No Result。

第二層則是 **Workflow Layer** 。LangGraph 根據 Tool 回傳的 Status 決定是否 Retry、Fallback、進入 Verification、回傳 Partial Success，或直接停止。這裡同時管理 Retry Count、Fallback Path 與整體 Task Status。

這種分工可以避免每個 Tool 都自行決定整個使用者體驗，也避免 Workflow 必須理解每一個第三方 API 的底層細節。

Tool 負責回答：「這一次呼叫發生了什麼？」

Workflow 則負責回答：「知道發生什麼之後，整個任務接下來應該怎麼辦？」

## 從「正常流程」開始設計「失敗流程」

前一天我們建立 LangGraph 時，主要關心的是 Router、Tool、Verification 與 Answer 怎麼連接。到了 Day 26，可以開始替每一個外部 Node 多問幾個問題：它會不會 Timeout？哪些錯誤值得 Retry？Retry 幾次？主要服務壞了之後有沒有 Fallback？沒有 Fallback 時能不能提供 Partial Success？如果完全不能完成，使用者應該看到什麼？

這些問題最後可能形成類似下面的可靠性流程：

**External Call → Error Classification → Retry / Stop → Fallback → Verification → Success / Partial Success / Actionable Error**

這張流程真正重要的不是箭頭本身，而是每一條失敗路徑都有明確的出口，不會因為外部服務異常就讓 Agent 開始自由猜測。

## Reliability 的目標不是讓錯誤消失

做到這裡，很容易把 Reliability 理解成「想辦法讓成功率變成 100%」。但只要系統依賴網路、第三方 API 與模型服務，就不存在真正永遠不失敗的產品。

更實際的目標是：當失敗發生時，系統能快速知道發生什麼、不要讓問題無限擴大、盡可能保存已完成的工作，而且讓使用者知道接下來能做什麼。

這也是企業 AI 從 Demo 走向產品的一個重要差異。Demo 通常只展示 Happy Path；正式產品真正決定信任感的，反而常常是 Failure Path。

---

> **今天的重點：**  
> 正式環境的可靠性，不是期待 AI、API 或外部服務永遠成功，而是預先把失敗納入 Workflow。Timeout 負責限制等待時間，Retry 處理可能恢復的暫時錯誤，Fallback 在主要服務失效時提供替代路徑，Verification 確保即使服務成功回應，結果仍然真的可以使用。當這些機制都失敗時，系統也應該清楚提供 Partial Success 或可行動的錯誤訊息，而不是讓模型自己把缺少的部分補完。

下一篇，我們會從後端 Reliability 轉向使用者真正感受到的前端體驗。當一個 Agent Workflow 需要依序查資料、搜尋文件、Verification，甚至執行 Retry，整段工作可能需要十幾秒甚至更久。這時候使用者到底應該看到什麼，才不會以為系統卡住了？

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407967)

[

Day 25｜實作：把協調者（Coordinator）改造成可控的 LangGraph 工作流程

](https://ithelp.ithome.com.tw/articles/10407967)[下一篇](https://ithelp.ithome.com.tw/articles/10408179)

[

Day 27｜代理（Agent）在工作時，使用者為什麼不能只看到轉圈圈

](https://ithelp.ithome.com.tw/articles/10408179)

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

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/8/44265d37-4976-42f0-820c-15d996fea475_small.jpg)

[雲端資安如何合規: 5 種常見曝險情境](https://itplus.ithome.com.tw/vod-page/45?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cymetrics |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/2c664646-4da6-4ed6-a652-7f75e561cf79_small.png)

[Review AI from LINE EC NLP](https://itplus.ithome.com.tw/vod-page/1083?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/26/189cf344-a977-48ac-9ffc-b9db69fb9a5d_small.png)

[透過開源 Device Farm 提升 App 測試效率](https://itplus.ithome.com.tw/vod-page/1185?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

41 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/b79a2ccc-cb72-410a-b319-3603c90679c5_small.png)

[在 Kubernetes 環境中進行開源軟件治理的實踐](https://itplus.ithome.com.tw/vod-page/1315?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

32 分