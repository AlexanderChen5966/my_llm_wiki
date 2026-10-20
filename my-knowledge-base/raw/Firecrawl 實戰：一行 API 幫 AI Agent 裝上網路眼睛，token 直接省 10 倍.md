---
title: "Firecrawl 實戰：一行 API 幫 AI Agent 裝上網路眼睛，token 直接省 10 倍"
source: "https://vocus.cc/article/6a194068fd8978000190b7b8"
author:
  - "[[Slow Signals]]"
published: 2026-05-30
created: 2026-06-05
description: "AI Agent 最大弱點，不是寫不出答案，而是看不到最新網頁。這篇用硬數字和實戰劇本，拆解它為什麼是 Agent 最該先裝的「網路眼睛」。"
tags:
  - "clippings"
status: processed
---
> 傳統爬蟲寫三小時、改五遍、還是抓不到 JS 渲染的內容？firecrawl 把整條 pipeline 壓成一行指令。不是又一個爬蟲工具——是讓 AI 能「看」網頁的基礎設施。

![zoomable](https://images.vocus.cc/c5e156d0-3212-4c9f-a706-48782b3bb1ff.png)

**圖1：firecrawl 解的不是「AI 寫不出來」，而是「AI 根本看不到最新的網頁」**

---

你有沒有過這種經驗？

想讓 AI 幫你分析五個競品官網的定價變化。你打開 Claude，描述需求。Claude 說：「我無法瀏覽網頁。」

你嘆口氣，自己打開五個網站，複製內容，貼進筆記，整理格式——半小時過去了。爬蟲也不是沒試過：裝 Scrapy、寫 XPath、處理 JavaScript 渲染、被反爬擋下來、換 User-Agent、還是抓不到動態載入的內容。最後發現，花在「讓爬蟲正常運作」的時間，比實際分析資料的時間還多。

這篇要介紹的 firecrawl，就是為了解決這個問題而生的。它把上面那整段折騰，壓縮成一行指令。

---

## 傳統爬蟲為什麼總是壞掉？

先說一個有點反直覺的事實：寫爬蟲的技術門檻，不在「抓資料」本身，在「讓它不要壞」。

你用 requests.get() 抓回來的 HTML，是瀏覽器渲染 ****之前**** 的原始碼。現代網頁大量依賴 JavaScript 動態載入內容——React、Vue、Next.js 渲染的頁面，requests 抓到的只是一個空殼

```
<div id="root"></div>
```
。所以你裝 Puppeteer 或 Playwright 來跑 headless browser。但 headless browser 有自己的問題：記憶體吃很兇、容易被 Cloudflare 擋下來、需要自己管理 proxy pool。

就算你克服了渲染問題，下一步更煩：HTML 裡塞滿了導航欄、廣告、footer、追蹤碼。你要自己寫清理邏輯，把

```
<nav>
```
、
```
<script>
```
、
```
<style>
```
過濾掉，再把剩下的內容轉成純文字。最後產出的東西——就算成功了——也不是 LLM 能直接消化的格式。你還要再把文字塞進 prompt，祈禱 token 不要爆掉。

整個流程花三小時，其中兩個半小時在跟爬蟲搏鬥。真正的分析，可能只花了半小時。

firecrawl 做的事情很簡單：把這三小時變成一秒。

---

## firecrawl 的三招

### 第一招：渲染——JS 頁面零配置處理

firecrawl 內建 headless browser，你不需要自己裝 Puppeteer、不需要管 Chrome 路徑、不需要處理記憶體洩漏。給它一個 URL，它自動執行 JavaScript、等待動態內容載入、截取完整頁面。

更關鍵的是：它內建 rotating proxy 和速率限制管理。你不需要自己買 proxy 服務、不需要手寫 retry logic。96% 的網站它都能正常抓取——包括那些用 React 渲染、Cloudflare 防護的頁面。

### 第二招：結構化——用自然語言描述你要的欄位

傳統爬蟲抓資料後，你要寫正則表達式或 CSS selector 來擷取特定欄位。firecrawl 的

```
/extract
```
端點讓你用自然語言描述你要的結構：

```javascript
{
  "prompt": "擷取這個頁面的產品名稱、價格和庫存狀態"
}
```

firecrawl 自動回傳結構化 JSON。你不需要學 XPath、不用管 HTML 結構變化——它用 LLM 幫你判斷哪個

```
<div>
```
是價格、哪個
```
<span>
```
是庫存狀態。

### 第三招：LLM-ready——輸出自帶清洗

這是最被低估的一招。firecrawl 的輸出不是 HTML、不是 JSON、不是截圖——是 ****乾淨的 Markdown**** 。

為什麼這很重要？因為 LLM 讀 Markdown 最省 token。一頁 HTML 可能 50KB，轉成 Markdown 剩 5KB——token 用量差 10 倍。而且 Markdown 的結構（標題、列表、程式碼區塊）LLM 天生就理解，不需要多花 token 解釋「這是一段導航欄，請忽略」。

把這三招串起來：給 URL → firecrawl 自動渲染 JS 頁面 → 自動清洗內容 → 回傳乾淨 Markdown。一行指令，從「打開瀏覽器」到「可以餵給 LLM」全部自動。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F4dfed01f-20d3-466d-8bfd-c2d70e14235a.png&width=740&sign=ATiLGJMDD3fLise_REjrjjTwl8yIXffea5xlE6PUrSk) ![zoomable](https://images.vocus.cc/4dfed01f-20d3-466d-8bfd-c2d70e14235a.png)

**圖2：firecrawl 的三招不是把搜尋做快，而是把「能不能餵給 LLM」這件事一次解決。**

---

## 一眼看懂 firecrawl 的硬實力

口號誰都會喊，我自己看工具只認數字。firecrawl 官方文件裡，我覺得最值得記住的是這幾個：

| 指標 | 數字 | 為什麼重要 |
| --- | --- | --- |
| 網站覆蓋率 | 96% | 含 React 渲染、Cloudflare 防護的頁面也抓得到 |
| P95 延遲 | 3.4 秒 | 不是慢吞吞的批次工具，能掛進即時 Agent 流程 |
| token 用量 | 省 ~10× | 50KB HTML → 5KB Markdown，直接反映在帳單上 |
| 核心端點 | 7 個 | Search / Scrape / Interact / Agent / Crawl / Map / Batch Scrape |
| GitHub | ⭐125,843 | 2026-05 實測，2024 年 4 月上線至今的成長曲線 |

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F99334fd1-7301-4f1f-bf7e-434f4367f71f.png&width=740&sign=gdQpXvXUlAG5QYgvuWojPsNsRrOzgWZtcuYJq-wYoMs) ![zoomable](https://images.vocus.cc/99334fd1-7301-4f1f-bf7e-434f4367f71f.png)

**圖3：這張表我刻意只留五個數字——它們各自對應一個你真的會在意的問題：抓得到嗎、夠快嗎、省不省錢、夠不夠用、有沒有人背書。**

那個「省 10×」我多解釋一句，因為它最容易被當成行銷話術。它指的不是 firecrawl 比較便宜，而是 ****同一頁內容餵給 LLM 時的 token 差距**** ：HTML 帶著一堆導航欄、追蹤碼、樣式標籤，Markdown 把這些全洗掉。token 省下來，LLM 的閱讀成本和出錯機率都跟著降。

---

## 比三招更進一步：它已經會「操作」網頁了

如果故事停在「讀網頁」，firecrawl 也只是個更好用的爬蟲。真正讓它從工具升級成基礎設施的，是 v2.9 之後多出來的一個能力： ****操作**** 。

```
/interact
```
端點讓 firecrawl 不只是抓取頁面，而是能用一段自然語言或程式碼操控瀏覽器——點按鈕、填表單、捲動頁面、等待動態載入。換句話說，過去爬蟲卡死的那些場景：要登入才看得到的內容、要按「載入更多」才出現的列表、要填查詢條件才有結果的頁面——現在 AI Agent 可以自己完成。

再往上一層是

```
/agent
```
端點：你描述一個目標（「找出這家公司過去三個月的所有新聞稿並整理成表」），它自動規劃多步驟、自己決定要爬哪些頁面、要點哪些連結，然後把結果收回來。

這就是為什麼說 firecrawl 不是「又一個爬蟲」。爬蟲是「我給你網址，你把內容抓回來」；firecrawl 走到的是「我給你目標，你自己去網路上把事情做完」。從 ****被動讀取**** 到 ****主動操控**** ，這一步才是 AI Agent 真正需要的那雙「手」——眼睛之外，還有手。

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F5615569c-7ac3-439c-936f-719687f2b82f.png&width=740&sign=ZufyM-vcYK2WWXpMKw3rMW208aDwoCXNt0ehPEhIZRg) ![zoomable](https://images.vocus.cc/5615569c-7ac3-439c-936f-719687f2b82f.png)

**圖4：v2.9 的 /interact 讓它會「操作」、v2.10 的 /parse 讓它連本地 PDF/DOCX 都能轉 Markdown——爬蟲的邊界一路被往外推。**

---

## 三個明天就能用的實戰劇本

### 劇本一：幫 Claude Code 裝眼睛（5 分鐘、免費 tier）

Claude Code 很強，但它不能上網。遇到新的 API、新的 library、新的錯誤訊息——它只能靠訓練資料回答，知識截止在訓練日期。

firecrawl 原生支援 MCP（Model Context Protocol）。你只需要一行指令：

```javascript
firecrawl mcp
```

從此 Claude Code 就能即時搜尋網路。效果是這樣的：

- 你：「幫我用 Next.js 15 的新 caching API 改這個元件」
- Claude Code 透過 firecrawl MCP → 搜尋 Next.js 15 官方文檔 → 爬取 API reference 頁面 → 回傳 Markdown → 讀完後生成正確程式碼
- 整個過程你只下一句話；過去你得自己開瀏覽器查文檔、複製貼上、再回來寫 code

這不是「方便一點點」，這是把 Claude Code 從「只能靠記憶的助手」升級為「能即時查證的同事」。

### 劇本二：競品監控自動化（10 行程式碼 + crontab）

每天早上花 30 分鐘手動瀏覽五個競品官網？用這個 10 行的 Python 腳本取代：

```javascript
from firecrawl import Firecrawl
app = Firecrawl(api_key="fc-YOUR_KEY")

urls = ["https://competitor-a.com/pricing",
        "https://competitor-b.com/pricing",
        "https://competitor-c.com/pricing"]

for url in urls:
    result = app.scrape(url)
    with open(f"/data/{url.split('/')[-2]}_{result['metadata']['date']}.md", "w") as f:
        f.write(result['markdown'])
```

```
0 8 * * * python3 /scripts/competitor_watch.py
```

每天早上八點自動爬取、存成 Markdown。你可以再串一個 LLM 自動比較前後差異、產生 Slack 通知。從「每天手動 30 分鐘」變成「全自動 2 分鐘」。

### 劇本三：PDF 報告批次轉 Markdown（/parse 端點實測）

公司有 200 份 PDF 研究報告，想讓 LLM 幫你摘要分析？過去你得一份一份打開、複製貼上。firecrawl v2.10 的

```
/parse
```
端點直接解決：

```javascript
from firecrawl import Firecrawl
app = Firecrawl(api_key="fc-YOUR_KEY")

for pdf in ["report_001.pdf", "report_002.pdf", "..."]:
    result = app.parse(pdf)  # 支援 PDF/DOCX/XLSX，最大 50MB
    with open(f"/summaries/{pdf}_parsed.md", "w") as f:
        f.write(result['markdown'])
```

200 份 PDF → 200 份 Markdown → 餵給 LLM 批次摘要 → 一小時搞定。過去這可能要一個實習生做一週。

---

## 成本和競品：挑哪個？

| 工具 | 價格 | 核心能力 | 適合誰 |
| --- | --- | --- | --- |
| firecrawl | 免費 tier + 付費按頁計 | 渲染 + 結構化 + Markdown + MCP | AI Agent 開發、自動化管線 |
| Jina Reader | 免費 | 純讀取 URL → Markdown | 偶爾需要讀網頁內容 |
| Browserless | 付費（按 browser hour） | Headless browser API | 重度 JS 互動（登入、填表） |
| ScrapingBee | 付費（按 request） | 代理管理 + 渲染 | 傳統爬蟲升級（不需要 Markdown） |
| ScrapeGraph | 免費 tier + 付費 | API 化爬蟲 + LLM 萃取 | 較新、社群較小，想試 firecrawl 替代品 |

![vocus｜新世代的創作平台](https://resize-image.vocus.cc/resize?compression=6&norotation=true&url=https%3A%2F%2Fimages.vocus.cc%2F72104b53-249c-4698-9ffb-b487960a6a81.png&width=740&sign=zXFcHwPk3iz6g6TsYQVC7NFPyiOUPNcQ1v_A5kYgrC4) ![zoomable](https://images.vocus.cc/72104b53-249c-4698-9ffb-b487960a6a81.png)

**圖5：不是 firecrawl 全贏——而是看你要的是「讀一頁」「爬很多頁」「操作表單」還是「餵 AI Agent」。**

簡單判斷：

- 你只是偶爾想看某個網頁的純文字內容 → Jina Reader
- 你需要爬大量頁面、但不需要 Markdown 格式 → ScrapingBee
- 你需要登入網站、操作複雜表單 → Browserless
- 你要做 AI Agent 開發、RAG 系統、自動化監控 → firecrawl

---

## 也要老實講：firecrawl 不是萬能

我盡量不把任何工具只講亮點。firecrawl 有幾個邊界，先說清楚，你判斷起來會準一些：

****1\. 重度 JS SPA 仍有邊界情況。****

96% 覆蓋率很漂亮，但反過來說，還有那 4%——尤其是大量依賴 WebSocket、複雜前端狀態的頁面，偶爾還是會抓不乾淨。真要用在關鍵流程，建議先拿你自己的目標網站測一輪。

****2\. 雲端方案是按頁計費。****

免費 tier 試水溫很夠，但如果你要 batch scrape 上萬頁、或天天 crawl 整站，成本要先算過。它省的是你的「開發時間」，不是無上限的「執行費用」。

****3\. 自託管要自己養 proxy 和瀏覽器實例。****

firecrawl 開源（AGPL-3.0），你可以自架省雲端費。但 proxy pool 輪換、headless browser 實例管理這些「髒活」會回到你身上——這正是雲端版幫你扛掉的部分。省錢和省心，這裡要二選一。

****4\. 反爬機制不是百分百繞得過。****

有些網站的反爬很硬，firecrawl 內建 proxy 也未必每次都過。它降低的是「對付反爬」的門檻，不是給你一張無視 robots.txt 的通行證——這點在合規上也請自己拿捏。

這四點放在判斷邊上，不會被 ⭐125K 沖昏頭。值不值得，你自己的 workflow 說了算。

---

## 最後想說

firecrawl 不是「又一個爬蟲工具」。它是一個訊號。

十二萬個開發者已經在用，不是因為它「比 Scrapy 好用一點點」，而是因為 ****AI Agent 正在從「只能靠訓練資料回答」進化到「能即時看網路、搜資料、讀文件，甚至操作網頁」**** 。這個進化需要基礎設施——不是 MCP 協議本身，而是 MCP 生態中那些真正有用的工具。

firecrawl 是這個生態中最成熟的一個。

如果你已經在用 Claude Code 或 Cursor，今天花 5 分鐘裝 firecrawl MCP。明天開始，你的 AI 就會有眼睛了。

---

> ****你最想用 firecrawl 做什麼？****
> 
> A. 幫 Claude Code 裝眼睛，讓它能即時搜文檔
> 
> B. 自動監控競品官網變化 C. 把 PDF 報告轉成 LLM 能讀的格式
> 
> 留言告訴我你的答案——最多人選的主題，我會拍成實戰影片。也別忘了追蹤沙龍，後續還有 n8n 自動化等 AI Agent 基礎設施系列教學。