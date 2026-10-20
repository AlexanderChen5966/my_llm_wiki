---
title: "Headroom：AI Agent 的情境壓縮神器，用懂懂壓縮節省 LLM 成本"
source: "https://vocus.cc/article/6a125377fd897800010c43a1"
author:
  - "[[Slow Signals]]"
published: 2026-05-27
created: 2026-06-05
description: "AI coding agent 帶來驚人的 API 費用？問題可能出在 context 裡塞了太多不必要內容。Headroom 提出「本地壓縮」策略，在 LLM API 呼叫前先進行 context 壓縮，針對 tool-heavy 任務，官方 benchmark 可省最多92% tokens。"
tags:
  - "clippings"
status: processed
---
> Headroom 在 agent 送進 LLM 前先做本地壓縮，官方 benchmark 顯示 tool-heavy 任務可省 47% 到 92% tokens。但本地實測也可能是 0%——而 0% 不一定是壞掉。

![zoomable](https://images.vocus.cc/7269cf7b-17ce-4f05-8e97-8798ff2a7700.png)

上週六晚上，我打開 OpenAI 的 usage dashboard，看到本月 API 費已經跑到上個月的 1.7 倍。

我沒有加新的專案，沒有換模型，沒有突然暴寫程式。我只是讓 Claude Code 多跑了幾個 issue：讀 codebase、搜關鍵字、跑測試、修 bug、再跑測試。每一步看起來都很合理，加起來就很可怕。

如果你也用 AI coding agent，你大概知道我在說什麼。不是訂閱費的問題，是那些你沒意識到被塞進 context 的 tokens。

我開始追問題出在哪裡。結論很反直覺：不是模型太貴，也不是我太依賴 AI。是 agent 在每一輪對話裡，把太多「曾經有用、但下一輪不必完整保留」的東西原封不動送進模型。

****AI agent 時代的真正分水嶺，不是模型多強，而是你讓它背著整個倉庫跑步，還是先幫它整理成剛剛好的任務包。****

## 你的 agent 不是話多，是行李太多

回想一下你用 agent 修 bug 的過程。你問一句「幫我修這個後端錯誤」，它可能接著做：

1. 讀 10 個相關檔案，每個幾百行
2. 搜尋函式呼叫，回傳 100 筆結果
3. 跑一次測試，噴出幾百行 stack trace
4. 把以上內容塞回對話，再跑下一輪

你看起來只是在對話。對模型來說，每一輪都被餵進更多東西：完整 grep 結果、完整 git diff、完整測試輸出、完整 log。

問題不是這些資訊沒用。問題是它們不需要每一行都原封不動地跟著你走到下一輪。

Agent 每多做一件事，context 就胖一圈。context 每胖一圈，API call 就更貴。API 更貴，你就更不敢讓 agent 多做幾步。最後你以為自己在控制成本，其實是把 agent 的工作半徑縮小。

這就是 Headroom 想解的問題。

## Headroom 不是 prompt cache，它是 context 壓縮層

先把最容易誤會的事情講清楚：Headroom 不是 prompt cache。

平台原生快取，例如 Anthropic prompt caching、OpenAI prefix caching，處理的是「前綴重複」。如果你每次 request 前面一大段都一樣，provider 可以重用算過的部分。這很好，但它幫不了你的是：agent 每次工作都製造大量「不重複但很肥」的新內容。

Headroom 管的是後者。它的位置長這樣：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F6e23a55c-8507-4358-a352-a3023141b39e.png&width=740&sign=LrSMIVim0JbyIV4lSp6hxaXkSJKiY3LhGeF5iPzAzI8) ![zoomable](https://images.vocus.cc/6e23a55c-8507-4358-a352-a3023141b39e.png)

它不是叫你少用 agent，而是在 agent 把東西丟進模型前，先問一句：這些內容哪些需要完整保留？哪些可以摘要？哪些其實是格式重複？

Headroom 內部有幾種策略：

- ****SmartCrusher**** ：處理 JSON、API 回應、資料列、結構化 log 這類內容，保留 schema、異常值與代表性樣本。
- ****CodeCompressor**** ：用 tree-sitter AST 理解程式碼結構，但實務上有很強的保護閘門。
- ****Kompress-base / LLMLingua 類文字壓縮**** ：處理一般文字或 log，但需要額外依賴或模型。
- ****CCR（Compress-Cache-Retrieve）**** ：原文留在本地，模型需要時可以回頭取。

CCR 是我最在意的地方。傳統摘要最大的風險是：摘要錯了就沒救。你把 stack trace 壓成三句話，結果關鍵行剛好被刪掉，agent 就會一本正經地往錯的方向修。

Headroom 的設計是：先給模型壓縮版；壓縮版不夠，再用本地取回工具拿原文。這不是完美保證，但它把壓縮從「一次性賭博」變成「先摘要、必要時回查」。

## 真實數據：官方 benchmark + 本地踩坑

Headroom README 給的核心 benchmark 數字很漂亮：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F9146ef14-d1b5-48f5-b216-3248e6919acb.png&width=740&sign=6T8z6Xwa34j3SgS4siRvSEWvDPlD4ZxCDPkkUlSst_g) ![zoomable](https://images.vocus.cc/9146ef14-d1b5-48f5-b216-3248e6919acb.png)

| 場景 | 壓縮前 | 壓縮後 | 省下 |
| --- | --- | --- | --- |
| Code search（100 筆結果） | 17,765 tokens | 1,408 tokens | 92% |
| SRE 事故除錯 | 65,694 tokens | 5,118 tokens | 92% |
| GitHub issue 分類 | 54,174 tokens | 14,761 tokens | 73% |
| Codebase 探索 | 78,502 tokens | 41,254 tokens | 47% |

這張表的重點不是「所有人都能省 92%」。真正的重點是：不同內容型態的浪費程度差很多。

搜尋結果、JSON、log、測試輸出通常有大量格式重複，壓縮空間很大；程式碼本身資訊密度高，能壓的空間小，而且亂壓會出事。這就是為什麼 SRE 除錯可以省到 92%，但 codebase exploration 只有 47%。

為了寫這篇，我自己在本地裝了一次 Headroom v0.22.3：

```javascript
pip install headroom-ai
```

裝的過程蠻順的，依賴沒打架，這在 Python 工具裡其實已經值得放心一下。然後我用純 Python library 模式測

```
compress()
```
，挑了 code search、SRE 除錯、issue triage、codebase exploration 四個小樣本——結果四個全部都是 ****0% 壓縮****

說實話那當下我有點愣住。

第一個反應當然是覺得自己哪裡裝錯，或測試腳本有 bug。我又重跑了幾次，換了內容、換了模型，還是 0%。後來慢慢往文件深處翻，才意識到——這可能不是壞掉，反而比較像 Headroom 的安全閘門在工作。

官方 limitations 明確寫到幾件事：

- 短訊息少於 300 tokens 時不壓，因為 overhead 大於收益。
- source code 預設 passthrough，避免破壞正確性。
- grep/search result 在某些版本文件中會被視為已經夠精簡，可能不再壓。
- optional dependency 缺失、解析失敗、壓完更大，都會 fallback 成原文。

所以這次 0% 結果不是可以拿來反駁官方 benchmark 的數字。它比較像一個提醒： ****Headroom 的效果不能只用****

```
pip install
```
****加一個******
```
compress()
```
******小樣本判斷。你要測的是完整使用模式，最好走 proxy、wrap 或明確開啟對應 extras。****

這也讓我對這篇的結論更保守：我不會說「裝上去就省 90%」。我會說：如果你的 workflow 真的有大量 tool output，Headroom 值得測；但你一定要用自己的任務量測，而不是只看官方表格。

## 壓縮後 AI 會不會變笨？

省錢很好，但如果省到 agent 開始改錯檔案、漏看錯誤、把測試結果看反，那叫自己挖坑。

Headroom 文件裡有兩類品質訊號。

第一類是 accuracy benchmark：

| 基準測試 | 原始表現 | Headroom 後 | 變化 |
| --- | --- | --- | --- |
| GSM8K（數學推理） | 87.0% | 87.0% | ±0 |
| TruthfulQA（事實問答） | 53.0% | 56.0% | +3% |

另外兩個常被引用的數字是「壓縮下的保留率」，不是 before/after 對照，分開敘述比較不會誤讀：SQuAD v2 在 19% 壓縮下保留 97% 表現；BFCL（工具呼叫）在 32% 壓縮下保留 97% 表現。

我會保守解讀：GSM8K 沒掉分是好訊號，TruthfulQA +3% 不該包裝成「壓縮讓模型更聰明」。比較合理的說法是：在這些測試組合裡，壓縮沒有造成可見的準確度損傷。

第二類其實更重要：安全閘門。

- 最近 4 則訊息中的程式碼不壓縮。
- 使用者訊息包含 analyze、review、explain、fix、debug 時，程式碼被高度保護。
- 短訊息不壓。
- system prompt 不壓，避免破壞工具規格與 prefix cache。
- 壓縮失敗時回傳原文，不把錯誤往上拋給 agent。

這幾條看起來樸素，但比「什麼都硬壓」要細心很多。寫 AI coding 工具的人應該都知道——最怕的不是少省 10% tokens，而是把 API contract、測試錯誤、函式簽名壓壞。會去寫這些守則的團隊，至少是有想過讀者會在哪裡跌倒的。

## 壓縮失敗時，兩件事要先知道

Headroom 的限制文件其實寫得很誠實。這是我覺得讀者最需要先知道的兩件事。

### 情境一：壓縮版不夠用，需要 retrieve

CCR 的「取回」機制聽起來很理想，但現實裡有兩個成本。

第一，agent 必須意識到自己需要原文。假設 stack trace 第 7 行有關鍵錯誤碼，壓縮版剛好沒呈現。Agent 從壓縮版判斷錯誤在 module A，但原文其實指向 module B。這時候它必須知道要回頭查。

第二，retrieve 本身也要花 tokens。若 agent 一直取回原文，淨省率會被吃掉一部分。不是不划算，而是你不能把「壓縮率」直接等同於「帳單省率」。

底線：CCR 是好設計，但你的 agent 是否真的會在不確定時回頭查，取決於 prompt、框架與使用習慣。

### 情境二：短對話幾乎無感

官方 production telemetry 顯示，很多短 conversational turns 的壓縮率只有個位數。limitations 也明確列出：短對話、單輪 request、沒有累積 context 的情境，Headroom 幫助很小。

如果你是「問一句、回一句、開新對話」的使用者，Headroom 不是你的優先項。甚至可能因為多一層 proxy，latency 微幅增加。這種情境硬裝壓縮層，省不到錢，還多一個可能出 bug 的環節。

## 哪些人會很有感，哪些人可以先跳過

總結成一句話：你的 agent 做越多 tool call，Headroom 越值得。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F0edc8bb7-6bbf-4370-b656-dca80ace1d3d.png&width=740&sign=a03qyIlOile2JK0E7HYbwu4E9R0ecgE_wtM30LG72ck) ![zoomable](https://images.vocus.cc/0edc8bb7-6bbf-4370-b656-dca80ace1d3d.png)

| 你的使用型態 | 值不值得 |
| --- | --- |
| 每天用 Claude Code / Codex / Cursor，tool call 很多 | 值得 |
| 常處理長 log、JSON、測試輸出、API 回應 | 很值得 |
| 大型 repo exploration，讓 agent 先讀結構 | 值得，但別期待 90% |
| 只用 Web ChatGPT 或手機 App | 不適合 |
| 每次都是短問短答 | 幾乎無感 |
| 公司環境不能跑本地 Python 或 proxy | 先確認合規 |

我自己的評比會給它這樣的定位：

- ****概念價值：高。**** Agent context 成本是真問題，而且會越來越常見。
- ****數據可信度：中高。**** 官方 benchmark 有可重現指令，但不同文件版本與情境差異要分開看。
- ****落地成熟度：中。**** v0.x 快速迭代，適合工程使用者試，不適合期待無痛產品體驗的人。
- ****短期採用建議：小範圍試。**** 不要全域啟用，先挑最肥的 workflow。

## 第一週怎麼試？一份 7 天驗證節奏

如果你決定試，不要第一天就全開。這是我的建議節奏：

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2Ffa450ade-eaf6-4b8d-b06f-e1dde462334b.png&width=740&sign=iF7A3yZ-SMgbT0iXPoZxGLs8zQAPFopAsM1PeaD4Ct0) ![zoomable](https://images.vocus.cc/fa450ade-eaf6-4b8d-b06f-e1dde462334b.png)

| 天 | 做什麼 | 看什麼 |
| --- | --- | --- |
| Day 1 | 裝好 Headroom，先跑非關鍵專案 | proxy / wrap 是否啟動，stats 是否有資料 |
| Day 2 | 在 JSON 或 API 回應密集任務測試 | 壓縮前後是否保留 schema、error、異常值 |
| Day 3 | 找一個需要大量 log / stack trace 的除錯任務 | 壓縮版有沒有漏掉關鍵行 |
| Day 4 | 做一次完整 issue triage（3-5 個 issue） | 摘要是否足夠支撐分類與下一步 |
| Day 5 | 比對開 / 關 Headroom 的 token 使用量 | 看淨省率，不只看壓縮率 |
| Day 6 | 跑一次日常最常做的任務 | 有沒有需要 retrieve 卻沒觸發 |
| Day 7 | 決定放進日常，或只在特定場景開 | 不必二選一，可以除錯時開、短任務關 |

****判斷標準**** ：如果 Day 5 的實際省率不到 15%，而且你沒有感覺 agent 更專注，那 Headroom 可能不適合你的 workflow。這完全 OK，不是每個人的使用型態都一樣。

## 安裝方式：三條路線

最簡單的路是 Agent Wrap：

```javascript
pip install "headroom-ai[all]"
headroom wrap claude
```

README 也列出

```
headroom wrap codex
```
、
```
headroom wrap cursor
```
、
```
headroom wrap aider
```
等模式。這條路線最接近「把 Headroom 掛在 agent 前面」。

如果你用自建 client 或想保持工具不變，用 proxy：

```javascript
headroom proxy --port 9090
```

再把 LLM client 的 base URL 指到本地 proxy。

```
--port
```
可以指定任何閒置號碼；Headroom 文件列出的預設值是
```
8787
```
，但實務上挑一個你 dev 環境慣用的 port 就好。

如果你在寫自己的 agent，才考慮直接用 library：

```javascript
from headroom import compress
result = compress(messages, model="gpt-4o")
```

但這次本地實測剛好提醒：library 模式最容易讓人誤判。它不是不能用，而是你要確認內容型態、extras、模型與安全閘門都有符合你的測試目的。

## Headroom、agentmemory、CodeGraph：三種不同層次

這三個名字很容易混在一起，但它們各管各的。

- ****CodeGraph**** 像專案地圖。幫 agent 先理解 codebase 結構：哪些檔案、模組、依賴關係比較重要。
- ****agentmemory**** 像長期筆記本。處理跨 session 記憶：上次踩過什麼坑、這個專案的慣例、哪些測試容易爆。研究材料中記錄它有 LongMemEval #1 96.2% 的定位數字。
- ****Headroom**** 像當下任務的行李整理員。它管的是這一輪 agent 工作時，哪些 tool output 不該完整拖進下一輪 context。

三者串起來的關係：

```javascript
CodeGraph：先給 agent 專案地圖
        ↓
Agent 開始工作：讀檔、搜尋、跑測試、修 bug
        ↓
Headroom：壓縮本輪 tool output，避免 context 爆掉
        ↓
agentmemory：把這次學到的教訓留下，下次不用重來
```

Headroom 不是要取代 memory，也不是要取代 codebase understanding。它補的是中間那段最容易失控、但大家最晚才注意到的成本層。

## 結語：你該不該現在就裝？

老實說，這題沒有標準答案，要看你現在的使用型態。

如果你已經有穩定的本地 AI coding workflow，每個月看到 API 帳單會皺一下眉，那我會建議你拿一個非關鍵專案，找一週時間試試看。不用第一天就期待 90% 的省率，先觀察自己的 workflow 是不是真的有大量 tool output——如果是，能省下 40% 到 70%，我覺得就已經很對得起這一週的試錯時間。

如果你主要用 Web 版 ChatGPT，或者根本沒有 API 成本壓力，那 Headroom 大概不是現在最該看的工具。它解的是本地 agent 的 context 成本，不是所有 AI 使用者都會遇到的問題。

如果你在公司環境用，那還要多想兩件事：資料能不能經過本地 proxy、原始 tool output 會快取在哪裡。Headroom 是主打本地執行沒錯，但合規這件事不是看 slogan，是看你的實際部署方式——這個只有你自己最清楚。

最後想說一句：AI agent 時代真正的分水嶺，可能不是模型多強，而是你讓它背著整個倉庫跑步，還是先幫它整理成剛剛好的任務包。