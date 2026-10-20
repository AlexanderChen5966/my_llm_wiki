---
title: "Day 27｜代理（Agent）在工作時，使用者為什麼不能只看到轉圈圈 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408179"
author:
  - "[[iThome]]"
published:
created: 2026-09-08
description: "傳統聊天機器人的互動方式很簡單：使用者送出一個問題，等待幾秒鐘，接著看到模型產生答案。在這種情境下，一個簡單的 Loading Spinner 通常就已經足夠，因為等待時間短，而且後端實際上可能也只有..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 28

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 28 篇

## Day 27｜代理（Agent）在工作時，使用者為什麼不能只看到轉圈圈

- 分享至

傳統聊天機器人的互動方式很簡單：使用者送出一個問題，等待幾秒鐘，接著看到模型產生答案。在這種情境下，一個簡單的 Loading Spinner 通常就已經足夠，因為等待時間短，而且後端實際上可能也只有一次模型呼叫。

但當 Data Machi 開始從 Chatbot 逐漸變成 Agent Workflow，情況就完全不同了。一個問題可能要先經過 Router 理解任務，再查詢 Google Sheets、搜尋 PDF、整合不同來源，接著做 Verification；如果中間遇到 Timeout，甚至還可能進入 Retry 或 Fallback。原本只需要三、五秒的對話，現在可能變成十幾秒甚至更久的工作流程。

如果這段時間畫面從頭到尾只顯示一個不斷旋轉的 Loading Icon，使用者很快就會開始懷疑：系統現在真的有在工作嗎？是不是卡住了？我要不要重新整理？剛剛送出的問題到底有沒有收到？

因此 **Agent UX（代理使用者體驗）真正要解決的，不是怎麼讓等待動畫更漂亮，而是降低使用者在等待期間的不確定感。** 使用者不需要知道 Agent 背後有幾個 Node、用了哪一個 Framework，但他至少應該知道系統目前正在做什麼、任務是否仍然往前進，以及自己現在需不需要採取任何行動。

## Agent 的等待時間和一般 Chatbot 不太一樣

一般 Chatbot 的等待大多發生在同一件事情上：模型正在產生回答。但 Agent Workflow 的等待是由很多不同階段組成的。例如使用者問：「哪個市場的問題最多？正式定義是什麼？相關改善專案現在做到哪裡？」後端可能先判斷需要 Data Tool、Document Tool 與 Project Tool，再依照資料依賴安排查詢。

實際流程可能經過「理解問題」、「查詢數據」、「搜尋文件」、「確認專案狀態」、「驗證資料」與「整理回答」。如果 Project Tool 第一次 Timeout，還可能再多出一次 Retry。從使用者的角度來看，這整段時間如果都只叫做「Loading」，其實完全看不出系統到底有沒有前進。

這也是為什麼 Agent UX 需要開始從「等待答案」轉向「理解任務進度」。使用者不一定需要知道所有技術細節，但應該可以感受到工作流程正在往下一步移動。

## 進度提示應該描述「工作」，而不是技術實作

最直接的改善方式，是把 Workflow 目前執行到哪個階段，用人類能理解的方式顯示出來。

例如後端真正的 Node 可能叫做 `route_request` 、 `query_google_sheets` 、 `search_documents` 、 `verify_results` 與 `generate_answer` 。這些名稱很適合 Log 或 LangGraph Trace，但不一定適合直接顯示給一般使用者。

前端可以把它們轉換成人類比較容易理解的文字：

| Workflow 狀態 | 使用者看到的提示 |
| --- | --- |
| `planning` | 正在理解你的問題 |
| `retrieving_data` | 正在查詢數據 |
| `searching_documents` | 正在搜尋相關文件 |
| `checking_project` | 正在確認專案狀態 |
| `verifying` | 正在確認資料是否完整 |
| `generating` | 正在整理回答 |
| `completed` | 任務完成 |
| `failed` | 任務未完成 |

這裡很重要的一點是： **進度提示應該來自 Workflow 的真實狀態，而不是讓模型自己生成一句「請稍等，我正在查詢」。**

如果 LangGraph 現在真的進入 `search_documents` Node，Backend 就把狀態更新成 `searching_documents` ；Node 完成後進到 `verify_results` ，再更新成 `verifying` 。這樣前端看到的狀態和後端真正正在執行的事情是一致的。

這比要求模型每隔幾秒產生一句「我還在努力」可靠很多。

## 不要把 Chain-of-Thought 當成進度畫面

談到 Agent 執行過程，很容易想到把模型的 Reasoning 全部展示給使用者，例如顯示「我正在想應該使用 Google Sheets 還是文件」、「我覺得下一步可能需要查專案」。

但 Agent UX 需要的不是把模型內部推理公開，而是提供 **工作層級的可觀察狀態** 。

使用者真正需要知道的是：

- 正在理解問題
- 正在查詢數據
- 正在搜尋文件
- 正在重新嘗試資料來源
- 正在驗證結果
- 正在整理回答

這些都是 Workflow 真正發生的事件，而且不需要暴露模型內部的詳細推理過程。

可以把兩者區分成：

| 類型 | 是否適合顯示 |
| --- | --- |
| 正在搜尋 PDF | 適合 |
| 正在重新連線資料來源 | 適合 |
| 已完成 2 / 3 個來源 | 適合 |
| 模型完整內部推理 | 不需要 |
| 隱藏 Prompt | 不應顯示 |
| API Key、Tool 內部參數 | 不應顯示 |

因此「透明」並不代表把所有技術資訊都攤開，而是讓使用者看得懂和自己任務真正相關的狀態。

## Progress State 最好直接和 LangGraph Node 對應

Day 25 已經把 Coordinator 拆成明確的 Node，這時前端 UX 其實可以直接從 Graph 受益。

例如 Workflow 是：

**Router → Data Tool → Document Tool → Verification → Answer**

當 Router 開始時，前端顯示「正在理解你的問題」；Data Tool 開始後改成「正在查詢數據」；Document Tool 開始時顯示「正在搜尋相關文件」；Verification 時顯示「正在確認結果是否完整」，最後才進入「正在整理回答」。

這種設計的好處，是 UX 狀態不需要和 Agent 邏輯分開維護。Workflow 增加新 Node 時，只要定義它對應的使用者可讀狀態即可。

甚至後端可以回傳比較結構化的事件，例如：

`status = verifying`

前端自己負責將它翻成：

「正在確認數據與文件結果是否一致。」

這種方式也比讓 LLM 每次自由產生 Progress Message 更穩定，因為同一個 Workflow State 不會今天顯示「正在分析」、明天又變成「正在思考下一步」。

## 不一定需要告訴使用者百分比

看到 Progress UI，很容易想到顯示：

`70% 完成`

但 Agent Workflow 不一定適合百分比。

如果 Workflow 是一條固定五步驟流程，百分比還比較容易計算；但 Agent 可能在 Verification 後發現資料不足，再回去補查另一個 Tool。原本已經顯示 80%，下一秒卻因為新增一個查詢步驟變成 60%，反而會讓使用者困惑。

因此對動態 Workflow，比起硬算百分比，更適合顯示「目前階段」與「已完成的工作」。

例如：

- 已完成數據查詢
- 已找到相關文件
- 正在確認專案狀態
- 接著會整理結果

這種 Milestone-based Progress 往往比一個虛假的 73% 更可信。

## Streaming 可以減少等待感，但不代表任務已經完成

另一個常見的 UX 技術是 **Streaming（串流輸出）** 。模型不需要等完整答案產生完才一次送回前端，而可以逐步顯示文字。

這確實可以降低使用者感受到的等待時間，但在 Agent Workflow 裡，Streaming 也有一個新的風險： **畫面上已經開始出現文字，不代表整個任務真的完成。**

假設使用者問一個跨來源問題，Data Tool 已經查到結果，因此模型開始生成：「目前 Hong Kong 的問題量最高……」。但 Project Tool 還沒回來，Verification 也尚未完成。如果使用者看到文字開始出現，很可能以為這已經是最終答案。

因此 Agent UX 最好把「內容正在產生」和「任務已經完成」分開。

例如 Backend 可以維持幾個明確狀態：

| 狀態 | 意義 |
| --- | --- |
| `planning` | 正在理解任務 |
| `retrieving` | 正在取得資料 |
| `verifying` | 正在驗證結果 |
| `generating` | 正在整理最終內容 |
| `completed` | Workflow 已完整結束 |
| `partial` | 部分完成 |
| `failed` | 無法完成 |

即使畫面已經有部分文字，只要 `status != completed` ，前端仍然可以標示「正在完成剩餘步驟」。

這個差異對 Agent 特別重要，因為使用者要知道自己現在看到的是 Intermediate Output 還是 Final Result。

## 不要太早生成看似確定的答案

Streaming 還會帶來另一個產品問題：如果最終回答必須經過 Verification，就不應該在 Verification 前先把未確認的結論當成正式答案 Streaming 出去。

例如 Data Tool 查到 Conversion Rate 下滑後，模型可能先產生：

「這主要是 Paid Traffic 增加造成的。」

結果 Verification 後發現目前根本沒有足夠證據支持這個原因，即使後面再修正，使用者已經先看到了錯誤結論。

因此比較穩定的做法，是把 Agent Workflow 分成「進度事件」和「最終內容」兩條 UX。

Verification 以前，可以顯示：「已完成數據查詢，正在確認可能原因。」Verification 完成後，才正式生成或顯示完整答案。

如果真的要串流最終文字，也最好從資料已經通過主要 Verification 之後才開始。

## Error Message 應該告訴使用者：現在能做什麼

Day 26 已經談過 Backend Error Classification。到了前端，這些錯誤狀態應該進一步轉換成使用者真正能採取行動的訊息。

例如：

| Backend Error | 使用者看到的訊息 |
| --- | --- |
| 403 Permission Denied | 目前沒有 Google Sheets 的讀取權限，請確認共享設定 |
| Timeout | 資料來源暫時沒有回應，你可以重新嘗試 |
| No Result | 目前沒有找到符合條件的資料，可以調整查詢條件 |
| Missing Input | 還缺少時間範圍，請選擇本月或最近 90 天 |
| Document Search Failed | 文件搜尋暫時無法使用，其他資料來源仍可繼續 |
| Rate Limit | 服務目前繁忙，系統稍後可以重新嘗試 |

好的 Error UX 不只是把 Backend Error 翻譯成中文，而是要讓使用者理解： **是我的問題、權限問題、資料不存在，還是系統暫時失敗？我現在能不能做什麼？**

例如「找不到資料」和「資料來源目前無法存取」看起來都沒有結果，但意義完全不同。前者可能代表查詢條件真的沒有資料，後者代表系統根本沒有成功查看來源。

如果畫面最後都只寫「查詢失敗」，使用者就沒有辦法判斷下一步。

## Partial Success 在 UI 上也應該保留下來

多來源 Workflow 更需要避免「其中一個來源失敗，整頁結果消失」。

例如使用者問：

「哪個市場問題最多？它的正式定義是什麼？改善專案目前做到哪裡？」

執行結果是：

- Google Sheets：Success
- PDF RAG：Success
- Project API：Timeout

這時畫面可以正常顯示前兩部分，再在專案狀態位置標示「目前無法取得最新專案進度」。

對使用者而言，這比只看到：

`Something went wrong`

有價值得多。

甚至可以把結果呈現成：

| 工作項目 | 狀態 |
| --- | --- |
| 找出問題最高市場 | 已完成 |
| 查詢正式定義 | 已完成 |
| 確認改善專案 | 暫時無法取得 |

這也讓 Partial Success 從 Backend State 真正變成使用者可以理解的產品狀態。

## Retry 發生時，也不應該讓使用者以為系統停住

Day 26 加入 Retry 後，Workflow 可能因為第一次 Tool Call 失敗而多等待幾秒鐘。

如果前端完全沒有更新，使用者可能在 Retry 期間誤以為系統卡住，按下 Refresh 或重複送出相同任務，反而造成更多 Request。

因此 Retry 發生時，可以提供適度的狀態，例如：「資料來源暫時沒有回應，正在重新嘗試。」如果 Retry Limit 已經用完，才轉成：

「目前無法連線到資料來源，已停止重新嘗試。」

使用者不需要知道「現在是 Retry 2 / 3、Exponential Backoff 4 秒」，除非這是工程工具；一般產品只需要讓他知道系統仍然有控制地往下一步處理，而不是陷入無限 Loading。

## Agent UX 要明確區分「系統在等」和「系統在等你」

Human-in-the-loop 加入後，還會出現一個新的 UX 狀態。例如 Workflow 已經完成 Email Draft，但必須等使用者 Approval。

這時候不能繼續顯示：「正在處理……」。因為系統其實已經沒有在處理，而是在等使用者做決策。

這時畫面應該非常明確，可能是正在 **等待你的確認** ，並顯示：

- 即將執行什麼操作
- 會影響哪個系統
- 使用哪些資料
- Approve / Reject

例如：「已完成 Email 草稿。確認後將寄送給 Project Team。」這和「正在整理 Email」是完全不同的狀態。

因此 Agent Workflow 至少需要區分：

`running`

和：

`waiting_for_user`

否則使用者會不知道為什麼 Loading 一直沒有結束。

## 完成狀態要明確，不能只靠「看起來有答案」

傳統 Chatbot 通常只要訊息出現在畫面上，就可以視為這輪完成。但 Agent 可能做的不只是產生文字，還包括建立 Task、更新資料、寄信，或完成多個子工作。

因此 Agent UX 最好有明確的 Completion State。

例如：

**任務完成**

並告訴使用者：

- 已查詢哪些來源
- 哪些步驟成功
- 是否有 Partial Success
- 是否真的執行 Action
- 結果資料時間

例如使用者要求建立 Task，最終不應該只回：「已幫你處理。」

而應該清楚區分：「已建立 Task」和「已產生 Task 草稿，尚未建立」兩者差異。兩者對使用者來說完全不同。

這也是 Agent UX 很重要的一個原則： **Intent、Draft、Approval、Execution 與 Completion 都應該有不同狀態。**

## 從 Demo UX 到 Workflow UX

做到這裡，使用者和 Data Machi 的互動方式也開始改變。

最早期 Chatbot 比較像：

**送出問題 → Loading → Answer**

現在開始變成：

**送出任務 → 顯示目前階段 → 執行多個步驟 → 必要時詢問或等待批准 → 顯示完成或部分完成結果**

這兩種 UX 的差別不只是多了 Progress Bar，而是產品心智模型從「聊天」開始轉向「委派工作」。

使用者不是在問一句話而已，而是把一項 Task 交給 Agent。

既然是一項 Task，就應該有：

- 任務狀態
- 執行進度
- 等待原因
- 錯誤狀態
- 完成結果

這就是 Workflow UX。

## Agent UX 也可以有成熟度

如果想判斷目前產品做到哪裡，可以把 Agent UX 粗略分成五個成熟階段。

| 成熟度 | 使用者看到的體驗 |
| --- | --- |
| Level 1 | 從頭到尾只有 Loading |
| Level 2 | 有進度，但全部是 Node、API、Tool 等技術術語 |
| Level 3 | 可以看懂目前正在理解、搜尋、驗證或整理 |
| Level 4 | Error 可行動、Partial Success 與 Completion 明確 |
| Level 5 | 長任務可以離開頁面後繼續追蹤與恢復 |

Level 1 常見於 Prototype。只要 Agent Workflow 稍微變長，使用者就容易覺得系統壞掉。

Level 2 雖然開始有 Observability，但如果畫面顯示：

`Executing node: document_retriever`

一般業務使用者仍然不一定知道這代表什麼。

到了 Level 3，狀態開始被翻譯成真正的工作語言；Level 4 則開始把 Error、Partial Success 與 Completion 做完整。

Level 5 的差異最大：Agent 不再假設使用者一定要停留在原頁面等待任務完成。

## 長任務不應該綁住使用者的瀏覽器

假設未來 Data Machi 要分析 50 份文件、整理一小時會議錄音，或跑一個需要數分鐘的研究 Workflow，要求使用者一直留在同一個 Browser Tab 等待並不合理。

這時 Task 本身需要一個獨立識別，例如：

`task_id = task_20260906_001`

後端持續保存：

- Task Status
- Current Step
- Created At
- Updated At
- Result
- Error
- Checkpoint

使用者可以離開頁面，之後再回到：

「我的任務」

查看目前狀態。

例如：

文字Markdown

CSVExcel

統計圖表

| 任務 | 狀態 |
| --- | --- |
| 分析八月客訴 | 已完成 |
| 整理 Product Meeting | 等待確認 |
| 分析 50 份文件 | 執行中 |
| 更新 Project Tasks | 部分完成 |

這時 Agent 才真正開始從一次性的聊天回覆，轉變成可以承接工作任務的產品。

## 可恢復的 UX 需要後端真的支援恢復

前端有一個「繼續任務」按鈕，不代表系統真的能繼續。

Day 25 提過 Checkpointer、Interrupt / Resume 與 Durable Execution。這些 Backend 能力到了 Agent UX 裡，才會真正變成使用者看到的「稍後繼續」。

例如 Workflow 停在 Approval：

**Status：Waiting for Approval**

使用者隔天再回來，可以看到當時的 Email Draft、Data Source 與 Verification Result，再選擇 Approve。

Backend 從保存的 State Resume，而不是重新把整條 Workflow 跑一次。

這也是為什麼 Agent UX 和 Agent Architecture 很難完全分開設計。前端能呈現多少可靠狀態，取決於後端到底有沒有真正保存 State。

## 千萬不要讓模型假裝自己還在背景工作

這裡還有一個非常重要的 UX 原則。

模型可能回答：

「請稍等，我正在幫你查詢。」

但如果這句話送出之後，Backend 根本沒有任何 Tool Call、Workflow 或 Task 在執行，那這只是一段自然語言，並不代表系統真的正在工作。

因此所有：

- 正在查詢
- 正在分析
- 正在重新嘗試
- 正在建立任務
- 已完成

這類產品狀態，都應該來自 Backend 的真實 Workflow State。

模型可以負責把狀態翻譯成人類容易理解的語言，但不能自己憑空宣告一個不存在的執行狀態。

這也是 Agent UX 和普通聊天介面最大的差異之一： **畫面顯示的不只是 AI 說了什麼，而是系統現在真的發生了什麼。**

## 實務上可以先做到哪裡？

Data Machi 現階段不需要一口氣做到完整的 Long-running Task Platform。

如果第一版要改善 UX，可以先完成四件事情。

首先，把 LangGraph 主要 Node 對應成幾個人類可讀的 Progress State，例如「理解問題」、「查詢資料」、「搜尋文件」、「驗證結果」與「整理回答」。

第二，讓 Success、Partial Success、Failed 與 Waiting for User 有不同的視覺與文字狀態，不要全部使用同一個 Loading。

第三，把 Day 26 的 Error Classification 轉換成 Actionable Error Message，讓使用者知道哪個來源失敗，以及自己能不能處理。

第四，確認畫面上的「完成」真的來自 Workflow 的 `completed` State，而不是模型開始輸出文字就算完成。

做到這四件事，即使還沒有複雜動畫、Progress Bar 或 Task Center，Agent 的可信度通常就會比只有 Spinner 的 Prototype 提升很多。

## Agent UX 的目的不是展示系統有多複雜

設計 Agent UX 時，很容易因為後端有 LangGraph、RAG、Coordinator、Retry、Fallback，就想把每一步全部展示在畫面上。

但使用者通常不在乎：

「目前正在執行 Conditional Edge。」

他在乎的是：

「我的數據查到了嗎？」

「文件找到了嗎？」

「現在是不是在等我？」

「有一部分失敗嗎？」

「這個任務到底完成了沒有？」

因此 Agent UX 最後仍然應該回到工作本身。後端可以很複雜，但前端的任務是把這些複雜度轉換成少量、清楚而可信的狀態。

真正好的 Agent UX 不是讓使用者理解 Agent Architecture，而是讓他在整段任務執行期間都不需要猜測系統現在發生什麼。

---

> **今天的重點：**  
> Agent UX 的核心不是增加更多 Loading Animation，而是降低等待期間的不確定性。進度提示應該來自真實 Workflow State，而不是模型假裝正在工作；Streaming 不等於任務已完成；錯誤訊息要讓使用者知道下一步能做什麼；多來源任務則應保留 Partial Success。當 Workflow 需要更長時間時，任務還應該逐步具備可追蹤、可離開、可回來與可恢復的能力。使用者真正需要知道的始終是兩件事： **系統現在在做什麼，以及我現在需不需要做什麼。**

下一篇，我們會處理正式上線前另一個不能跳過的問題：當 Agent 已經可以存取 Google Sheets、文件、Project Tool，甚至開始具備 Action 能力後， **API Key、Service Account、資料權限與資產所有權到底應該怎麼管理？**

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408173)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/16/e04c7b02-307c-46c8-969b-4954d20f6dcc.jpg)

[Angular 17 全新檢測變更機制 - Signal](https://itplus.ithome.com.tw/webinar-page/219?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/16/34ce3c67-3bbf-4374-992a-c2d4355e82fe_small.png)

[公務人員如何善用 AI 提昇其個人與團隊績效](https://itplus.ithome.com.tw/vod-page/942?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/28a0a365-144b-4bcd-9462-ab36ed8991e0_small.png)

[雲原生環境下的資料安全實踐：Kubernetes 中的加密、機密管理與策略控管](https://itplus.ithome.com.tw/vod-page/1313?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/c91fe2fa-5311-4e64-8f3d-d3cf830b83d4_small.png)

[HPE ProLiant Compute Gen 12 全新 AI 伺服、智勝未來](https://itplus.ithome.com.tw/vod-page/1240?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

32 分