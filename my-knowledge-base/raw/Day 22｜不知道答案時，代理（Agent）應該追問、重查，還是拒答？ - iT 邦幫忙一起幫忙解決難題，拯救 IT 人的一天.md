---
title: "Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407137"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "前一天我們談到 Memory，讓 Agent 知道哪些上下文可以沿用、哪些資料必須重新取得。但當多輪對話開始變得更自然之後，還會遇到另一個更棘手的問題：當手上的資訊不足時，Agent 到底應該自己繼續..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 23

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 23 篇

## Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？

- 分享至

前一天我們談到 Memory，讓 Agent 知道哪些上下文可以沿用、哪些資料必須重新取得。但當多輪對話開始變得更自然之後，還會遇到另一個更棘手的問題： **當手上的資訊不足時，Agent 到底應該自己繼續查、向使用者確認，還是直接說目前無法回答？**

這個判斷如果做不好，很容易走向兩個極端。一種是什麼事情都問使用者，明明前文已經提供市場、時間與指標，Agent 還是不斷重新確認；另一種則是什麼都不問，只要缺少條件就自己猜，最後產生一個看起來合理、實際上沒有證據支持的答案。

因此可靠的企業 Agent 並不是「永遠都有答案」，而是需要先判斷目前缺的是哪一種資訊。  

整個判斷的核心可以先記成三件事： **問題不清楚就 Clarify、資料可能過期就 Re-query、問題清楚但證據不足就 Verification 或補查。** 如果完成這些步驟後仍然沒有足夠證據，才應該明確說明目前無法回答，而不是用模型常識把缺口補滿。

## Clarification：問題本身不夠清楚時才問

假設使用者突然說：

> 「幫我看一下最近的表現。」

這句話對人來說也很模糊。什麼叫「表現」？是銷售、轉換率、需求工單還是專案進度？「最近」又代表本週、本月還是最近 90 天？如果前文也沒有提供足夠 Context，Coordinator 根本還不知道應該使用哪一個 Tool。

這時候真正缺少的是 **使用者意圖與查詢條件** ，所以應該進入 Clarification，而不是先隨便挑一個資料來源。

但好的 Clarification 不應該把整張表單重新丟給使用者。例如已經從前文知道：

```markdown
Market = Taiwan
Metric = Conversion Rate
```

目前只缺 Period，就沒有必要再問：

> 「請提供市場、指標與時間範圍。」

比較合理的問題是：

> 「你是想看本月的轉換率，還是最近 90 天的趨勢？」

也就是：

```markdown
已知：
Market = Taiwan
Metric = Conversion Rate

缺少：
Period

→ 只確認 Period
```

這裡可以先記住 Clarification 的三個實用原則： **一次只問真正阻擋下一步的關鍵問題、能提供具體選項就不要只問開放式問題，以及讓使用者知道為什麼需要確認。**

## 不要為了保險而什麼都問

Clarification 並不是越多越安全。

假設使用者前面已經問：

> 「2026 年台灣哪一類工單最多？」

系統完成查詢後，下一句是：

> 「那它的定義呢？」

這裡其實已經知道：

```markdown
「它」
=
上一輪的 Top Category
```

如果 Coordinator 又問：

> 「請問你想查哪一類工單？」

反而代表 Memory 沒有真正發揮作用。

因此 Clarification 應該發生在：

> **目前資訊真的不足以安全決定下一步。**

而不是：

> 「只要句子裡沒有把每個條件重新寫一次，就重新問使用者。」

可以把它理解成可靠同事的行為：已經知道的事情不重問，真正不確定而且會影響結果的事情才確認。

## Clarification 最好提供可以直接選擇的方向

另一個常見例子是名稱模糊。

使用者說：

> 「ATL 專案現在怎麼樣？」

假設 Project Tool 找到：

```markdown
ATL Dashboard Revamp
ATL Customer Journey Improvement
```

這時候 Agent 不應該自行選其中一個，也不需要只問：

> 「ATL 是什麼？」

因為系統其實已經取得更多資訊。

比較好的追問是：

> 「目前找到兩個名稱相近的 ATL 專案： `ATL Dashboard Revamp` 和 `ATL Customer Journey Improvement` 。你想查哪一個？」

這就是讓 Tool Result 參與 Clarification。

流程可以是：

```markdown
使用者：
ATL 專案現在怎麼樣？
      ↓
Project Tool
      ↓
找到兩個候選項目
      ↓
無法安全判斷是哪一個
      ↓
Clarification
```

Agent 並不是一碰到模糊就立刻把問題丟回使用者，而是可以先利用低風險的查詢縮小範圍，最後只讓使用者確認真正需要人類判斷的地方。

## Re-query：資料會變，就不能只相信 Memory

另一種情況不是問題不清楚，而是 **使用者問得很清楚，但現有資料可能已經過期** 。

例如前一輪已經查過：

```markdown
Project Status:
In Progress

Queried At:
2026-09-01 10:00
```

兩天後使用者問：

> 「現在專案進度呢？」

這時候不需要 Clarification，因為 Project 和問題都很明確。真正的問題是：

> **舊 Tool Result 還能不能代表「現在」？**

答案通常是否定的，因此應該重新呼叫 Project Tool。

```markdown
Current Question:
「現在專案進度呢？」

Existing Memory:
Status = In Progress
Queried At = 2 days ago

Freshness Check:
Expired

→ Re-query Project Tool
```

同樣的邏輯也適用於：

- 今天銷售
- 即時庫存
- 工單狀態
- 最新留言
- 物流進度
- 專案 Deadline
- 當前任務 Owner

這些資料的共同特色是會隨時間改變。

## 使用者明確要求重新確認時，要跳過 Cache

甚至有時候舊 Result 還非常新，但使用者已經明確告訴我們：

> 「你確定嗎？我剛剛更新資料。」

這時即使資料只在一分鐘前查過，也應該把使用者的語句理解為：

```markdown
force_refresh = true
```

重新查 Source of Truth。

例如：

```markdown
Old Result:
328

User:
「我剛更新 Sheet，再確認一次。」

→ Re-query

New Result:
331
```

這時最後回答可以說：

> 「重新查詢後，目前結果是 331 筆；上一輪查詢結果是 328 筆。」

這樣 Memory 反而可以幫忙比較新舊結果，而不是成為阻止重新查詢的理由。

因此可以延續 Day 21 的原則：

> **記得資料，不代表資料仍然有效。**

## Verification：Tool 有回結果，也不代表可以直接回答

另一種情況更容易被忽略：Tool 執行成功，也取得了資料，但這些資料不一定能支持使用者真正問的問題。

假設使用者問：

> 「為什麼轉換率下降？」

Google Sheets Tool 回傳：

```markdown
April: 4.8%
May: 4.3%
June: 3.9%
```

我們可以確認：

> **轉換率確實下降。**

但使用者問的是：

> **為什麼下降？**

光靠這三個數字，沒有辦法證明原因。

如果模型直接回答：

> 「因為流量品質下降。」

即使這個解釋在商業上很合理，仍然是沒有證據的推測。

因此 Tool Result 回來後，Coordinator 還需要問：

> **目前證據真的足以回答使用者原本的問題嗎？**

這就是 Verification。

## Verification 檢查的是「證據與結論是否匹配」

可以把 Verification 理解成回答送出去前的一道品質檢查。

例如：

```markdown
使用者：
為什麼 Conversion Rate 下降？

目前證據：
Conversion Rate 確實下降

可以回答：
「Conversion Rate 從 4.8% 降到 3.9%。」

不能直接回答：
「下降原因是流量品質惡化。」
```

如果使用者需要原因，就可能要進一步查：

- Traffic Source
- Campaign Change
- UX Experiment
- Project Log
- Incident Record
- Business Report

最後可能得到：

```markdown
Conversion Rate ↓
+
Paid Traffic Share ↑
+
Paid Traffic CVR 明顯較低
```

即使如此，也要小心區分「相關性」與「因果」。

比較可靠的說法可能是：

> 「目前資料顯示轉換率下降期間，低轉換的 Paid Traffic 占比同步上升，這可能是其中一個原因，但目前證據仍不足以證明單一因果關係。」

這就是 Verification 對回答邊界的影響。

## Verification 第一版不需要全部交給另一個 LLM

談到 Verification，很容易又想再增加一個「Verifier Agent」。

但第一版其實很多檢查可以直接使用程式規則完成，而且通常更穩定。

例如至少可以檢查：

| Verification | 要確認什麼 |
| --- | --- |
| Number Check | 回答中的數字是否存在於 Tool Result |
| Filter Check | 日期、市場與條件是否和 Query 一致 |
| Source Check | 文件敘述是否有來源 |
| Tool Status Check | Tool 失敗時是否假裝成功 |
| Freshness Check | 動態資料是否仍在有效期限 |
| Secret Check | 回答是否包含 Secret 或敏感設定 |
| Error Sanitization | 是否洩露內部 Stack Trace 或系統細節 |

例如 Tool Result 是：

```markdown
Revenue = 1,487,320
```

Final Answer 卻產生：

```markdown
Revenue = 1,478,320
```

這種問題不一定需要另一個模型判斷，程式就可以比對並攔下。

同樣地：

```markdown
Tool Status = FAILED
```

如果 Final Answer 卻寫：

> 「根據最新 Project Tool 資料……」

Verification 就應該阻止這個回答。

這也是企業 AI 一個很實用的設計原則： **能用 deterministic rule 驗證的事情，就不需要全部交給另一層 AI。**

## Tool 失敗時，不要讓模型自己補答案

假設 Project Tool 回傳：

```markdown
Permission Denied
```

這和：

```markdown
No Project Found
```

完全不是同一回事。

第一個代表：

> 系統目前沒有權限知道答案。

第二個才可能代表：

> 在目前查詢條件下沒有找到專案。

另外還可能有：

```markdown
Timeout
```

表示服務目前無法回應。

或：

```markdown
Missing Parameter
```

表示使用者的條件不足。

因此錯誤狀態最好保留明確類型：

| 狀態 | 代表意思 | 下一步 |
| --- | --- | --- |
| No Result | 查詢成功，但沒有資料 | 改查條件、補查或回報沒有結果 |
| Permission Denied | 無權限取得資料 | 說明權限限制 |
| Timeout | 來源暫時不可用 | 可重試有限次或部分回答 |
| Missing Input | 缺少必要條件 | Clarification |
| Invalid Source | 資料來源設定錯誤 | 回報系統問題，不自行猜答案 |

最危險的做法，是把所有錯誤最後都變成：

> 「沒有資料。」

因為這會把「不知道」偽裝成「不存在」。

## 什麼時候才應該拒答？

如果問題清楚、來源也已經查過，但所有可用資料都沒有提供足夠證據，這時就不應該繼續猜。

例如使用者問：

> 「這個專案延遲的真正原因是什麼？」

目前資料只有：

```markdown
Status:
Delayed

Due Date:
2026-08-31
```

沒有 Meeting Note、Comment、Incident Record 或其他原因說明。

最可靠的回答不是：

> 「可能是跨部門溝通問題。」

而是：

> 「目前專案紀錄只顯示狀態為 Delayed，沒有提供延遲原因，因此目前無法從現有資料確認原因。」

如果需要，也可以接著提供下一步：

> 「如果要進一步確認，可以查詢專案留言、最近一次會議紀錄或向負責人確認。」

這裡的重點不是用一句「不知道」結束，而是清楚區分：

```markdown
目前已知什麼
+
目前不知道什麼
+
還需要什麼資料才能回答
```

這會比硬給出一個漂亮答案可靠得多。

## Partial Answer：不是所有資料不足都只能拒答

另一個需要區分的情況是：有些問題雖然沒有完整答案，但部分內容已經可以確認。

例如：

> 「哪個市場的問題最多？為什麼？改善專案如何？」

目前取得：

```markdown
Top Market:
Hong Kong

Project Status:
In Progress

Root Cause:
No evidence
```

這時候沒有必要整題拒答。

比較合理的是：

> 「目前數據顯示 Hong Kong 的問題量最高，相關改善專案正在進行中；但現有資料沒有足夠證據確認問題量較高的原因，因此原因部分目前只能視為待驗證。」

也就是：

```markdown
Verified Part
→ 正常交付

Missing Evidence Part
→ 清楚標記資料不足
```

這就是 Partial Answer。

企業環境裡，這通常比「全部成功或全部失敗」更符合實際工作方式。

## 企業 Agent 最危險的幻覺，是把推論寫成事實

談到 Hallucination，很容易只想到模型完全憑空產生不存在的內容。但企業 Agent 更常發生的，其實是一些 **看起來非常合理的補完** 。

例如：

| 類型 | 危險例子 |
| --- | --- |
| 數字捏造 | 沒查資料卻回答「轉換率是 23.7%」 |
| 日期錯誤 | 把另一版本的 Launch Date 當成本專案日期 |
| 名稱混淆 | 把兩個相似專案的 Owner 混在一起 |
| 邏輯填補 | 只知道 Delayed，卻補成「跨部門溝通造成」 |
| 狀態推測 | 沒找到專案，就回答「目前沒有改善計畫」 |

最後一項尤其容易發生。

```markdown
Project Search:
No Result
```

只能證明：

> 「目前這次搜尋沒有找到。」

不能直接推論：

> 「公司沒有這個專案。」

可能的原因還包括名稱不一致、權限不足、資料尚未同步，甚至 Search Query 寫錯。

因此好的 Verification 不只是檢查文字有沒有錯，也要檢查：

> **結論是不是超過了證據本身可以支持的範圍。**

## Verified Fact 與 Analytical Hypothesis 要分開

如果模型確實可以提供分析價值，也不是所有推論都必須禁止。更合理的方式，是把「事實」與「假設」明確區分。

例如目前資料：

```markdown
Conversion Rate:
4.8% → 3.9%

Paid Traffic Share:
25% → 40%

Paid Traffic CVR:
2.1%
```

可以回答：

> **已確認事實：** 最近三個月整體轉換率由 4.8% 下降至 3.9%，同期 Paid Traffic 占比由 25% 上升至 40%，而 Paid Traffic 的轉換率為 2.1%。

接著才說：

> **待驗證假設：** 流量組合改變可能是整體轉換率下降的其中一項原因，但仍需要進一步控制其他因素後才能確認。

可以把兩層理解成：

```markdown
Verified Fact
必須來自：
Tool / RAG / Database / Source

Analytical Hypothesis
可以由：
LLM 推論

但必須：
清楚標記為假設
```

這比只在 Prompt 裡寫一句：

> 「Please do not hallucinate。」

實際得多。

## Clarification、Re-query、Verification 與拒答其實是不同問題

到這裡，可以把幾種容易混在一起的行為整理清楚。

| 情況 | 正確行為 |
| --- | --- |
| 使用者問題缺少必要條件 | Clarification |
| 問題清楚，但要求最新動態資料 | Re-query |
| Tool 有結果，但不足以支持完整結論 | Verification / 補查 |
| 來源互相衝突 | 顯示衝突並驗證 |
| 所有來源都沒有足夠證據 | 拒答該部分 |
| 部分問題有證據、部分沒有 | Partial Answer |
| 模型只有分析推論 | 標示 Analytical Hypothesis |

真正成熟的 Coordinator 不應該只有：

```markdown
Answer
or
Error
```

而應該可以選：

```markdown
Answer
Clarify
Re-query
Verify
Partial Answer
Insufficient Evidence
```

這些行為本身就是 Agent Policy 的一部分。

## 實務驗收｜故意用資訊不足的問題測試

Day 22 最適合測試的，不是「AI 答案寫得好不好」，而是：

> **當條件或證據不足時，它有沒有做出正確行為？**

可以建立下面這組固定測試。

| 測試情境 | 預期行為 |
| --- | --- |
| 「幫我看一下最近的表現。」 | 條件不足時，只追問真正缺少的指標或期間 |
| 前文已明確是 Taiwan，下一句「那最近呢？」 | 不重問 Market，只補時間條件 |
| 「請告訴我最新 Project Status。」 | 不沿用舊 Memory，重新查 Project Tool |
| 「為什麼 Conversion Rate 下降？」但只有下降數字 | 不把相關性寫成因果，應補查或標記 Hypothesis |
| PDF 與 Confluence 的政策版本不同 | 顯示來源、日期與版本衝突 |
| 所有來源都沒有答案 | 清楚說明目前缺少證據 |
| 「你確定嗎？我剛更新資料。」 | 強制 Re-query 並比較最新結果 |
| Project Tool 回 Permission Denied | 說明沒有權限，而不是說沒有專案 |
| Tool 查不到專案 | 說明目前搜尋沒有結果，不推論專案不存在 |

這類測試不適合只用「答對 / 答錯」判斷，可以拆成幾個維度：

| ID | 測試情境 | 應 Clarify | 應 Re-query | 證據足夠 | 是否誠實表達不確定性 | 通過 |
| --- | --- | --- | --- | --- | --- | --- |
| VERIFY-01 | 為什麼轉換率下降？目前只有下降數字 | 否 | 視情況 | 否 |  |  |
| VERIFY-02 | 最新 Project Status | 否 | 是 | 視 Tool Result |  |  |
| VERIFY-03 | 幫我看最近表現 | 是 | 否 | 否 |  |  |

這樣測試的目標不是要求系統每一題都產生答案，而是確認：

> **需要回答時回答，需要問時問，需要重新查時真的查，沒有證據時也願意停下來。**

## 「你確定嗎？」應該是一個可以測試的行為

最後可以特別測一個很常見的真實對話。

第一輪：

> 「目前台灣有多少筆需求？」

系統查詢：

```markdown
Result:
328
```

第二輪：

> 「你確定嗎？」

這句話不一定代表資料有更新，Coordinator 可以根據 Policy 決定是否重新檢查 Tool Result 與 Source。

第三輪：

> 「我剛剛更新 Sheet，再查一次。」

這次就非常明確：

```markdown
force_refresh = true
```

重新查詢後：

```markdown
New Result:
331
```

最後回答：

> 「重新查詢後目前是 331 筆，和上一輪的 328 筆相比增加 3 筆。」

這裡 Memory、Re-query 與 Verification 三個能力其實同時發生：系統記得舊值，但沒有把它當成永遠正確；重新取得最新資料後，也能比較兩次結果。

## 從「一定要回答」轉向「知道什麼時候不能回答」

做到 Day 22，Data Machi 的可靠性開始不只是來自能使用多少 Tool，而是開始有能力判斷自己的證據狀態。

```markdown
問題
 ↓
我理解問題嗎？
 ↓
資料夠新嗎？
 ↓
證據足夠嗎？
 ↓
來源一致嗎？
 ↓
可以交付嗎？
```

如果其中任何一層出現問題，下一步可能是 Clarification、Re-query、補查、Partial Answer，或明確告訴使用者資料不足。

這種行為有時候看起來沒有「什麼都能回答」那麼神奇，但它才比較接近真正可以放進企業工作流程中的 AI。

---

> **今天的重點：**  
> 企業 Agent 的可信度不是來自「永遠都有答案」，而是它能區分問題不清楚、資料過期、證據不足與來源衝突，並在不同情況選擇 **Clarification、Re-query、Verification、Partial Answer 或明確說明資料不足** 。真正可靠的系統，不只知道怎麼回答，也知道什麼時候不應該猜。

下一篇，我們會開始面對目前 Agent 架構本身的限制：當 **Clarification、Re-query、Verification、Retry、Partial Success 與停止條件** 全部塞在同一個 Agent Loop 裡，流程為什麼會越來越難理解、測試與控制？這也會正式把我們帶到 **LangGraph** 。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406864)

[

Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？

](https://ithelp.ithome.com.tw/articles/10406864)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 23 篇

目錄

1. 19
	[Day 18｜路由器（Router）與協調者（Coordinator）：會分流，不代表會完成任務](https://ithelp.ithome.com.tw/articles/10405571)
2. 20
	[Day 19｜實作：建立 Data Machi 協調者（Coordinator），讓 AI 自己選工具](https://ithelp.ithome.com.tw/articles/10405573)
3. 21
	[Day 20｜一個問題要查三個來源，代理（Agent）應該怎麼安排？](https://ithelp.ithome.com.tw/articles/10405767)
4. 22
	[Day 21｜代理（Agent）為什麼需要記憶（Memory）？哪些資訊該記，哪些不該記？](https://ithelp.ithome.com.tw/articles/10406864)
5. 23
	[Day 22｜不知道答案時，代理（Agent）應該追問、重查，還是拒答？](https://ithelp.ithome.com.tw/articles/10407137)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/20/0ac0a5b1-6150-475d-a775-4a622e682061_small.png)

[架構師也要 DevOps - 談服務模型的持續交付](https://itplus.ithome.com.tw/vod-page/767?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

51 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/4030b9f5-13ea-4f0d-becf-ea140471545b_small.png)

[在 Nutanix 混合多雲平台運行任何應用程式、資料、AI、Kubernetes](https://itplus.ithome.com.tw/vod-page/878?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/2f39ec89-fae9-41ea-8a96-bc97d11be2b7_small.png)

[用六頂思考帽挑戰敏捷框架，思考 AI 世代的敏捷未來](https://itplus.ithome.com.tw/vod-page/1069?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/3c32986b-66c3-4896-b66a-d2e0234cf09b.png)

[Kubernetes for Data Engineers: Building Scalable, Reliable Data Pipelines](https://itplus.ithome.com.tw/vod-page/1117?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

28 分