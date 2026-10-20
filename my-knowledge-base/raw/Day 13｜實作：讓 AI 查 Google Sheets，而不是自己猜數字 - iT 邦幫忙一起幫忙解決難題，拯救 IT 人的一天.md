---
title: "Day 13｜實作：讓 AI 查 Google Sheets，而不是自己猜數字 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10404930"
author:
  - "[[iThome]]"
published:
created: 2026-08-28
description: "前兩天我們已經把檢索增強生成（RAG）與工具使用（Tool Use）的邊界講清楚：文件適合尋找「已經存在的知識」，但精確數字、最新狀態與計算結果，應該直接從結構化資料來源取得。今天就把這個觀念真正落地..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 14

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 14 篇

## Day 13｜實作：讓 AI 查 Google Sheets，而不是自己猜數字

- 分享至

前兩天我們已經把檢索增強生成（RAG）與工具使用（Tool Use）的邊界講清楚：文件適合尋找「已經存在的知識」，但精確數字、最新狀態與計算結果，應該直接從結構化資料來源取得。今天就把這個觀念真正落地，讓 Data Machi 第一次透過工具查詢 Google Sheets。

這一篇的重點不是把整張試算表貼進 Prompt，再請大型語言模型幫忙算答案。剛好相反，我們希望把「資料存取與計算」交給程式，把「理解使用者意圖與整理答案」留給模型。這樣不但可以降低模型自行計算錯誤的風險，也讓每一個結果都能重新驗證。

整體流程可以先理解成：

```markdown
使用者問題
    ↓
LLM 理解需求
    ↓
Google Sheets Tool
    ↓
讀取結構化資料
    ↓
Pandas / 程式篩選與計算
    ↓
精確結果
    ↓
LLM 整理成自然語言回答
```

## 為什麼需要服務帳號（Service Account）？

如果後端程式需要自動讀取 Google Sheets，就不適合每一次查詢都等待某個使用者手動登入 Google 帳號。比較穩定的方式，是建立一個 **Service Account（服務帳號）** ，讓後端以一個獨立的「機器身分」存取指定的試算表。

可以把它想成替 Data Machi 建立一個專門的系統帳號。這個帳號不需要像一般使用者一樣登入 Google Workspace，而是由後端使用憑證證明自己的身分，再依照被授予的權限存取資料。

基本流程可以整理成：

```markdown
Google Cloud Project
        ↓
啟用 Google Sheets API
        ↓
建立 Service Account
        ↓
建立 Credentials
        ↓
取得 Service Account Email
        ↓
將 Google Sheet 分享給該 Email
        ↓
Backend 使用憑證讀取 Sheet
```

建立時，可以先到 Google Cloud Console 選擇或新增一個 Project，接著到 API Library 啟用 Google Sheets API。如果程式未來還需要搜尋或列出 Google Drive 中的檔案，再依需求啟用 Google Drive API，不需要一開始把所有 API 都打開。

接著建立一個容易辨識用途的 Service Account，例如：

```markdown
data-machi-sheets-reader
```

第一版的目標只是讀取指定的測試試算表，因此不需要給它 Project Owner 或其他過大的管理權限。建立完成後，可以取得 Service Account 的 Email，再把匿名測試試算表分享給這個 Email；如果目前只需要讀取資料，Google Sheets 權限先設定成 Viewer 即可。

這一步很容易被忽略。即使後端已經持有有效憑證，如果試算表本身沒有分享給 Service Account，程式依然沒有權限讀取內容。

## Service Account 憑證同樣屬於 Secret

建立 Service Account 後，後端需要一組憑證才能證明自己的身分。無論實際採用哪一種憑證管理方式，都應該遵守和 Gemini API Key 相同的原則： **Secret 不應該直接進入 GitHub Repository。**

本機開發時，如果暫時使用 JSON Credential File，至少應該將檔案加入 `.gitignore` ，避免不小心 Commit。不過部署到 Render 時，比較方便的方式之一，是把需要的憑證內容透過 Environment Variables 提供給 Backend。

例如：

```markdown
GOOGLE_SERVICE_ACCOUNT_JSON={"type":"service_account",...}
GOOGLE_SHEET_KEY=your_sheet_id
```

Backend 啟動時再從環境變數讀取：

```markdown
Environment Variables
        ↓
Backend
        ↓
建立 Google Credentials
        ↓
Google Sheets API
```

這樣本機與正式環境可以使用不同的 Secret，而程式碼本身不需要跟著修改。

另外也要延續 Day 10 的安全原則： **Google Service Account Credential 只能存在後端。** 不要把它放進 React 或 Vercel 前端，因為前端程式最終會執行在使用者瀏覽器中。

## 先用匿名資料驗證，不要直接碰正式資料

第一次建立 Google Sheets Tool 時，不需要直接連接公司的正式營運資料。比較好的做法，是先建立一份結構清楚、內容可以公開的匿名測試資料，確認權限、欄位與計算流程都正確之後，再考慮接上真正的企業資料來源。

例如可以建立一張簡單的需求工單資料：

| Request ID | Created Date | Market | Category | Status | Owner |
| --- | --- | --- | --- | --- | --- |
| R001 | 2026-07-01 | Taiwan | Dashboard | Completed | User A |
| R002 | 2026-07-03 | Hong Kong | Data Query | In Progress | User B |
| R003 | 2026-07-04 | Taiwan | Dashboard | Completed | User A |
| R004 | 2026-07-08 | Taiwan | Analysis | In Progress | User C |
| R005 | 2026-07-12 | Hong Kong | Dashboard | Completed | User B |

這時候先不要問 AI「今年台灣哪一類需求最多」，而是先確認最基本的資料讀取是否成功。例如程式是否真的讀到 5 筆資料、欄位名稱是否正確，以及日期欄位有沒有被正確解析。

這裡仍然延續前面的原則：

> **一次只驗證一層。**

整個測試順序可以按照：

```markdown
Service Account 可以驗證
        ↓
Google Sheet 有正確授權
        ↓
Backend 可以讀到資料
        ↓
欄位型態正確
        ↓
Pandas 可以篩選與計算
        ↓
最後才加入 LLM
```

不要一開始就把 LLM、Tool Calling、Google 權限、資料計算與前端全部混在一起除錯。否則當回答出錯時，很難知道問題究竟發生在哪一層。

## 常見的權限問題怎麼判斷？

Google Sheets Tool 第一次串接時，最常遇到的通常不是 AI 問題，而是身分與資源權限問題。

| 現象 | 優先檢查 |
| --- | --- |
| 找不到 Spreadsheet | Sheet ID 是否正確 |
| Permission Denied | Sheet 是否分享給 Service Account |
| Credentials Error | Service Account Credential 是否正確載入 |
| API 未啟用 | Google Sheets API 是否已啟用 |
| 可以連線但沒有資料 | Worksheet 名稱、Range 或欄位設定 |

其中最常見的情況是：Service Account 已經建立成功，Backend 也可以完成驗證，但 Google Sheet 忘記分享給 Service Account Email。這時候並不是憑證失效，而是「你知道自己是誰，但這份文件沒有授權給你」。

把 **Authentication（你是誰）** 和 **Authorization（你能不能存取這份資料）** 分開理解，後面串接其他企業系統時也會非常有幫助。

## 不要讓大型語言模型自己做精確計算

當 Google Sheets 已經可以正確讀取後，下一步才是把自然語言問題轉換成資料查詢。

假設使用者詢問：

> 「今年哪一個市場的工單最多？」

比較可靠的方式不是把所有 Rows 全部塞給模型，再請它自己數，而是先把使用者問題轉換成程式可以理解的條件，再由 Pandas、SQL 或其他資料工具執行真正的篩選與計算。

```markdown
自然語言問題
「今年哪一個市場的工單最多？」
        ↓
LLM 理解需求
        ↓
轉換成查詢條件
Year = 2026
Group By = Market
Metric = Request Count
        ↓
Pandas / 程式計算
        ↓
Taiwan = 3
Hong Kong = 2
        ↓
LLM 整理回答
```

模型可以理解「今年」、「市場」與「最多」代表什麼，但真正的 Count、Sum、Average、Group By、Sort 或比例計算，應該盡可能交給可重現的程式邏輯。

例如 Pandas 可以執行：

```python
result = (
    df[df["Created Date"].dt.year == 2026]
    .groupby("Market")["Request ID"]
    .nunique()
    .sort_values(ascending=False)
)
```

程式得到精確結果之後，再把結果交給模型：

```markdown
Taiwan: 3
Hong Kong: 2
```

最後由模型整理成：

> 2026 年目前台灣的工單數最多，共有 3 筆；香港則有 2 筆。

這個分工和 Day 12 提過的原則完全一致：

> **模型負責理解意圖，程式負責執行事實。**

## 為什麼不直接把整張 Sheet 丟給模型？

如果資料只有五列，看起來把整張表貼進 Prompt 好像也沒有什麼問題。但當資料增加到數千或數萬筆之後，這種方式會開始面臨幾個明顯限制：Context 會快速膨脹、Token 成本增加，而且模型仍然需要自己從大量文字中篩選與計算。

更重要的是，企業分析通常希望相同條件能產生相同結果。假設「Active Customer」在系統中有明確定義，就應該把計算邏輯保留在 SQL、Python 或資料工具中，而不是每一次都讓 LLM 根據文字描述重新理解一次。

因此比較穩定的架構是：

```markdown
Google Sheets
      ↓
Data Tool
      ↓
程式篩選 / 計算
      ↓
小而精確的結果
      ↓
LLM
      ↓
自然語言回答
```

大型語言模型拿到的不需要是整張資料表，而是完成問題所需要的結果與必要 Context。

## 欄位別名：讓使用者不需要知道資料 Schema

真實企業資料的欄位名稱，通常和人類平常說話的方式不完全一樣。例如資料表可能使用：

```markdown
request_market
created_at
ticket_category
request_status
owner_name
```

但使用者實際上可能會問：

> 「今年台灣哪一種需求最多？」

他不會說：

> 「請 group by `ticket_category` ，然後 filter `request_market = TW` 。」

因此 Tool Layer 通常需要處理一層 **Alias Mapping（欄位別名對應）** ，將人類使用的業務語言轉換成真正的資料 Schema。

例如：

| 使用者語言 | 真實欄位 |
| --- | --- |
| 市場、國家 | `request_market` |
| 建立日期、日期 | `created_at` |
| 類型、需求類別 | `ticket_category` |
| 狀態、進度 | `request_status` |
| 負責人 | `owner_name` |

當使用者詢問：

> 「今年台灣哪一類需求最多？」

系統就可以整理成：

```markdown
時間 → created_at
市場 → request_market
需求類別 → ticket_category
```

接著再交給程式執行。

這一層做得好有兩個好處。第一，使用者不需要學習資料庫的欄位名稱，仍然可以使用平常工作的語言；第二，如果未來資料 Schema 改名，只需要修改 Mapping，而不需要重寫所有 Prompt 或 Tool Description。

## 日期也應該轉成明確條件

自然語言中的日期特別容易產生歧義，例如：

- 今天
- 昨天
- 上個月
- 最近三個月
- 第一季
- 去年同期

模型可以協助理解這些文字，但真正送進資料工具前，最好轉換成明確的日期範圍。

例如：

```markdown
使用者：
「最近三個月台灣有多少工單？」

        ↓

轉換成：

start_date = 2026-06-01
end_date = 2026-08-31
market = Taiwan
metric = request_count
```

接下來程式只需要按照明確條件執行，不需要再次猜測「最近三個月」到底代表什麼。

尤其當企業報表存在 Fiscal Year、Business Week 或特殊 Campaign Period 時，更不能假設每個人的「一季」與「一個月」定義完全相同。這些規則最後都應該進入可以驗證的 Business Logic。

## 快取不是為了偷懶，而是控制成本與延遲

如果 Google Sheet 有幾千筆資料，而且一天只更新幾次，就沒有必要每一次使用者問問題時，都重新完整下載一次資料。這時候就可以加入 **Cache（快取）** 。

例如：

```markdown
第一次查詢
    ↓
Google Sheets API
    ↓
載入資料
    ↓
保存 Cache
    ↓
執行計算

下一次查詢
    ↓
Cache 還在有效時間內？
    ↓
是 → 使用 Cache
否 → 重新讀取 Google Sheets
```

Cache 可以降低 API 呼叫次數，也能縮短回答等待時間，但它同時帶來另一個問題： **快取中的資料不一定是最新的。**

因此 Cache Duration 不應該單純設定成「越久越好」，而應該根據業務資料的更新頻率決定。一份每天更新一次的資料，和每五分鐘更新一次的營運 Dashboard，本來就不應該使用相同策略。

如果使用者明確說：

> 「我剛剛更新了 Sheet，請重新確認。」

或：

> 「請查目前最新狀態。」

系統也應該能選擇跳過既有 Cache，重新取得資料，而不是盲目使用舊結果。

## 回答最好告訴使用者資料有多新

當系統開始使用 Cache 或外部資料來源後，「這個數字是什麼」和「這個數字是什麼時間取得的」會同樣重要。

因此 Tool Result 可以順便保留：

```markdown
data_source = Google Sheets
retrieved_at = 2026-08-24 15:30
row_count = 1520
```

最後回答就可以呈現：

> 截至 2026/08/24 15:30 取得的資料，台灣目前共有 328 筆工單，其中 Dashboard 類型最多，共 124 筆。

這樣使用者才知道目前看到的數字是否符合他對「最新」的期待。如果來源資料一天前才更新，也可以誠實呈現，而不是讓語言模型營造出「這就是現在即時數字」的錯覺。

## 不存在的欄位，不要自己猜一個替代品

還有一個非常容易被忽略的問題：使用者可能會問資料裡根本不存在的欄位。

假設目前 Sheet 只有：

```markdown
Request ID
Created Date
Market
Category
Status
Owner
```

使用者卻詢問：

> 「哪一個工單的處理成本最高？」

資料裡根本沒有 `Cost` 欄位。這時候 Tool 不應該看到 `Category` 或其他看起來相關的欄位，就自行推測使用者可能想問別的東西。

比較正確的回覆是：

> 目前這份資料沒有「處理成本」欄位，因此無法根據現有資料回答。如果有成本資料來源，需要另外連接或加入對應欄位。

這個設計看起來比較保守，但對企業 AI 反而更重要，因為：

> **明確承認資料不存在，比使用錯誤欄位產生一個看似合理的答案更可靠。**

## 今天要怎麼驗證 Data Tool 真的成功？

完成工具之後，不要只測一個問題就宣告成功。至少可以從三個層級開始驗證。

第一類是簡單查詢，例如：

> 「目前共有多少筆工單？」

第二類是篩選與彙總，例如：

> 「2026 年台灣有多少筆工單？」

第三類是組合條件與排序，例如：

> 「2026 年台灣哪一類工單最多？」

可以整理成：

| 測試類型 | 問題 | 要驗證的能力 |
| --- | --- | --- |
| 基礎計數 | 共有多少筆資料？ | 資料是否完整讀取 |
| 條件篩選 | 今年台灣有多少工單？ | 日期與 Market 篩選 |
| Group By | 哪一類工單最多？ | 分組、計數與排序 |
| 組合條件 | 今年台灣哪一類最多？ | 多條件查詢 |
| 不存在欄位 | 哪一個工單成本最高？ | 能否正確拒答 |

每一次測試都應該把 Data Tool 的結果和原始 Google Sheet 人工核對，而不是只判斷模型最後說得是否通順。如果原始資料算出 Taiwan 是 3 筆，而 AI 說 4 筆，即使回答文字寫得再漂亮，這個工具仍然沒有通過驗證。

最好可以同時保存：

```markdown
原始資料
+
使用者問題
+
Tool Parameters
+
程式計算結果
+
最終回答
```

這樣發生錯誤時，就能清楚知道到底是模型理解錯問題、參數 Mapping 錯誤、程式計算錯誤，還是最後回答整理錯誤。

## 實務踩坑｜最新資料、日期與不存在的欄位

實際把 Google Sheets Tool 放進企業工作流後，有三個問題值得再整理一次。

第一個是 **資料新鮮度** 。Cache 可以提升速度，但回答最好保留資料取得時間；如果使用者明確要求重新確認，就應該重新讀取來源。

第二個是 **時間條件** 。「上個月」、「最近三個月」、「Q1」等自然語言應該先轉成明確日期範圍，再交給資料工具執行。

第三個是 **Schema 邊界** 。如果資料來源根本沒有使用者要求的欄位，就應該明確回報無法回答，而不是自己選擇一個相似欄位替代。

這三件事情其實都指向同一個設計原則：

> **模型可以協助理解使用者在問什麼，但真正影響事實結果的條件，最好轉換成可以驗證的程式規則。**

從今天開始，Data Machi 已經不只有文件搜尋能力，也開始能從結構化資料中取得精確結果。這也是從「企業知識問答」走向「企業資料工作流」很重要的一步。

---

> **今天的重點：**  
> Data Machi 第一次擁有可驗證的結構化資料工具。模型不再負責「猜數字」，而是判斷什麼時候需要查資料，再由 Google Sheets Tool 與程式完成精確的篩選、計算與驗證。

下一篇，我們會把視角再拉大。企業知識從來不只存在於 Google Sheets 或 PDF，而可能散落在文件、資料庫、專案工具與各種 SaaS 系統中。先畫出企業的 **Knowledge Map（知識地圖）** ，才能決定下一個工具究竟應該接什麼。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10404726)

[

Day 12｜什麼是工具使用（Tool Use）？讓 AI 從「回答」開始學會「使用工具」

](https://ithelp.ithome.com.tw/articles/10404726)[下一篇](https://ithelp.ithome.com.tw/articles/10405115)

[

Day 14｜企業知識散落各處：先畫出你的企業知識地圖（Knowledge Map）

](https://ithelp.ithome.com.tw/articles/10405115)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 17 篇

目錄

1. 13
	[Day 12｜什麼是工具使用（Tool Use）？讓 AI 從「回答」開始學會「使用工具」](https://ithelp.ithome.com.tw/articles/10404726)
2. 14
	[Day 13｜實作：讓 AI 查 Google Sheets，而不是自己猜數字](https://ithelp.ithome.com.tw/articles/10404930)
3. 15
	[Day 14｜企業知識散落各處：先畫出你的企業知識地圖（Knowledge Map）](https://ithelp.ithome.com.tw/articles/10405115)
4. 16
	[Day 15｜實作：讓 AI 同時查數字與文件，完成第一次跨來源回答](https://ithelp.ithome.com.tw/articles/10405380)
5. 17
	[Day 16｜什麼時候工具使用（Tool Use）才真正變成代理（Agent）？](https://ithelp.ithome.com.tw/articles/10405442)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/20/50ae04e1-d67c-4784-a2ed-a4a3ea66301c_small.jpg)

[拒絕再寫無效規格，來學學實例化需求！](https://itplus.ithome.com.tw/vod-page/324?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Agile Summit 敏捷高峰會 |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/dd49af2b-a0cd-45b6-8fc4-0f62af89a554_small.png)

[Developer Success x Developer Portal](https://itplus.ithome.com.tw/vod-page/1038?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

20 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/17/5365ca68-469a-47e6-acba-aa791ff7c188_small.png)

[Kubernetes Self-Healing 背後的維運辛苦談](https://itplus.ithome.com.tw/vod-page/1320?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

44 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/b141104c-71f2-4587-8458-affaa819eb9a_small.png)

[混合雲環境與容器安全](https://itplus.ithome.com.tw/vod-page/1202?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分