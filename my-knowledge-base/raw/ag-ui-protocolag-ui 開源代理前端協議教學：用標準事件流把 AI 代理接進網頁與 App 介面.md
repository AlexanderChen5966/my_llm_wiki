---
title: "ag-ui-protocol/ag-ui 開源代理前端協議教學：用標準事件流把 AI 代理接進網頁與 App 介面"
source: "https://www.aiposthub.com/ag-ui-protocol-agent-frontend-event-stream-guide/"
author:
  - "[[Philo]]"
published: 2026-11-17
created: 2026-11-19
description: "ag-ui-protocol/ag-ui 定義了 AI 代理與前端 UI 之間串流文字、工具呼叫與狀態同步的標準協議，讓開發者不再為每個代理框架重寫前端串接程式。"
tags:
  - "clippings"
status: "inbox"
---
做 AI 應用最痛苦的時刻，往往不是模型調教，而是每次後端想從 LangChain 換成 CrewAI 或 Pydantic AI，前端的串流渲染與工具狀態程式碼就得全部重寫一次。

傳統的網頁開發習慣一問一答的 HTTP 請求模式，但 AI 代理會在生成過程中途呼叫工具、同步更新共享畫面狀態、暫停等待人類核准，甚至派出子代理平行處理任務。

在 X 與 GitHub 備受矚目的開源專案 `ag-ui-protocol/ag-ui`，正是為了解決代理與前端介面之間沒有共通語言的困境而誕生。

這套由 CopilotKit 團隊發起並以 MIT 授權開源的協議，用一條標準化的雙向事件流，讓任何框架打造的代理都能無縫對接網頁、手機 App 或終端機介面。

如果你正在開發需要串流對話、動態介面渲染或人機協作的 AI 產品，這篇 ag-ui 協議教學能幫你快速掌握架構與前後端串接實作。

相對地，如果你開發的只是純後端批次排程腳本，完全不需要與終端使用者介面即時互動，那麼直接呼叫模型 API 即可，並不需要引入前端協議層。

接下來我們會拆解這套標準在現代代理協議堆疊中的定位、1.0 版的八大事件家族、五分鐘前後端串接實戰，以及上線前必須注意的傳輸與安全邊界。

## AG-UI 是什麼？看懂代理協議三角架構與事件流設計

要真正理解這套協議的價值，必須先看清楚當前開源社群已經成形的「代理協議三角分工」。

在這個分工架構裡，MCP 協定負責讓代理連接外部工具與資料來源，A2A 協定負責讓多個代理彼此溝通協作，而 AG-UI 則專門負責將代理的能力送到使用者眼前。

換句話說，同一個後端代理在執行任務時，往往會同時使用這三種標準：透過 MCP 查詢資料庫、透過 A2A 委派子任務，最後再透過 AG-UI 把即時進度與互動畫面推送給前端網頁。

在過去沒有統一標準的時期，每個後端框架都自己發明一套伺服器推送格式。

前端工程師必須針對 LangGraph 寫一套資料解析器，換成 Mastra 或 Google ADK 又要再寫一套完全不同的狀態機，維護成本極高。

很多開發者也容易把它與 A2UI 或 MCP Apps 等生成式介面規格搞混，其實兩者屬於上下游互補關係。

A2UI 與 MCP Apps 定義的是「代理想在畫面上顯示什麼樣的卡片或元件結構」，而 AG-UI 則是負責把這些元件資料、文字串流與狀態變更即時搬運到前端的底層執行期協議。

在運作機制上，前端應用程式只需要送出一份標準化的 `RunAgentInput` 物件給後端，裡面包含對話歷史、前端提供的本地工具清單、共享狀態以及額外上下文。

接著後端代理就會回傳一條具備嚴格型別定義的事件串流，目前正式發布的 1.0 穩定版一共定義了 31 種事件，歸納在八個核心事件家族中。

一個最精簡的相容代理甚至不需要一次實作全部 31 種事件，只要能正確發出執行生命週期的起訖事件與文字訊息事件，就能立刻接上任何標準前端用戶端。

### AG-UI 1.0 八大事件家族速查表

| 事件家族 | 代表事件類型 | 主要負責的介面互動 |
| --- | --- | --- |
| 執行生命週期 | `RUN_STARTED`、`RUN_FINISHED`、`STEP_STARTED` | 標記單次代理任務與子步驟的起訖與錯誤狀態 |
| 文字訊息串流 | `TEXT_MESSAGE_START`、`TEXT_MESSAGE_CONTENT` | 逐字推送助理回覆，支援斷線重連與中斷恢復 |
| 工具呼叫過程 | `TOOL_CALL_START`、`TOOL_CALL_ARGS`、`TOOL_CALL_RESULT` | 讓前端即時顯示代理正在呼叫哪個工具與傳入參數 |
| 共享狀態同步 | `STATE_SNAPSHOT`、`STATE_DELTA` | 透過快照與 JSON Patch 增量同步代理與畫面的共享資料 |
| 推理過程呈現 | `REASONING_START`、`REASONING_ENCRYPTED_VALUE` | 展示模型思考脈絡，並支援加密推理內容來回傳遞 |
| 活動進度追蹤 | `ACTIVITY_SNAPSHOT`、`ACTIVITY_DELTA` | 在畫面上更新長任務的執行進度條與即時活動狀態 |
| 子代理委派 | `SUBAGENT_STARTED`、`SUBAGENT_FINISHED` | 將嵌套子代理的執行事件正確歸屬到對應任務來源 |
| 透傳與自訂事件 | `RAW`、`CUSTOM` | 保留底層框架原始資料或擴充團隊專屬業務事件 |

因為協議本身與傳輸通道解耦，官方規格同時支援網頁最常見的 HTTP 搭配 SSE 伺服器推送事件，以及效能更高的二進位 HTTP 搭配 Protobuf 格式。

如果你原本就已經有成熟的 WebSocket 連線架構，也可以透過官方提供的中介層將原本的訊息格式轉換為標準事件流。

目前包括 Google、Microsoft、Amazon 與 Oracle 都已採納相關標準，主流框架如 LangChain、CrewAI、Mastra、Google ADK、Pydantic AI 與 LlamaIndex 也都內建了原生整合支援。

延伸閱讀： [LangChain 完成 3 億美元 C 輪融資！CapitalG 領投、估值衝上 30 億美元，企業級 AI 代理基礎設施拼什麼？](https://www.aiposthub.com/langchain-300m-series-c-capitalg-3b-valuation/)

![ag-ui官方 GitHub 專案封面圖：ag-ui 協議教學](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/ag-ui-protocol-agent-frontend-event-stream-guide-official.png)

ag-ui官方開源專案視覺圖。 圖片來源： 官方原文 。

## 快速上手實戰：鷹架建置與 30 行前後端串接

官方團隊提供了非常友善的入門路徑，你可以依照專案階段選擇「一鍵建立完整全端範例」或是「手寫最小化前後端串接」。

### 方法一：用官方 CLI 一鍵建立全端專案

如果你希望立刻看到整合好 Next.js 與 CopilotKit 的網頁聊天與生成式介面效果，只需要在終端機執行一條鷹架指令即可。

```bash
npx create-ag-ui-app@latest
```

執行後依照畫面提示挑選你慣用的後端代理框架，系統會自動產生對應的專案範本。

這個指令支援產生網頁版 React 應用與終端機測試用戶端，讓你在五分鐘內就能體會完整的雙向串流互動。

因為預設範本會呼叫 OpenAI 模型服務，啟動開發伺服器前記得先在終端機匯入你的 API 金鑰。

```bash
export OPENAI_API_KEY="sk-..."
```

### 方法二：手寫 30 行 Python 後端與 TypeScript 前端

如果想在既有系統裡無痛導入，完全不需要任何模型金鑰就能先在本地跑通整套事件流。

這套架構只需要兩邊安裝對應的官方套件，目前官方核心支援 Python 3.9 以上與 Node.js 20 以上環境，另外也提供 `.NET` 套件 `AGUI.Client` 以及多種社群語言 SDK。

首先在第一個終端機視窗安裝 Python 伺服器套件，包含協議核心、FastAPI 與 Uvicorn。

```bash
pip install ag-ui-protocol fastapi uvicorn
```

接著建立一個名為 `server.py` 的檔案，核心邏輯是接收 `RunAgentInput`，再透過 `EventEncoder` 將生命週期事件與逐字訊息編碼為串流回應。

這裡的 `EventEncoder` 會自動檢查請求標頭中的 `Accept` 欄位，幫你決定要輸出標準的文字 SSE 串流或是二進位格式，完全不用自己手刻字串拼接。

```python
import uuid
from fastapi import FastAPI, Request
from fastapi.responses import StreamingResponse
from ag_ui.core import (
    PROTOCOL_VERSION,
    RunAgentInput,
    RunStartedEvent,
    RunFinishedEvent,
    TextMessageStartEvent,
    TextMessageContentEvent,
    TextMessageEndEvent,
)
from ag_ui.encoder import EventEncoder

app = FastAPI()

@app.post("/")
async def run_agent(input_data: RunAgentInput, request: Request):
    encoder = EventEncoder(accept=request.headers.get("accept"))

    async def events():
        yield encoder.encode(RunStartedEvent(
            thread_id=input_data.thread_id,
            run_id=input_data.run_id,
            protocol_version=PROTOCOL_VERSION,
        ))
        message_id = str(uuid.uuid4())
        yield encoder.encode(TextMessageStartEvent(message_id=message_id, role="assistant"))
        for chunk in ["Hello ", "from ", "AG-UI!"]:
            yield encoder.encode(TextMessageContentEvent(message_id=message_id, delta=chunk))
        yield encoder.encode(TextMessageEndEvent(message_id=message_id))
        yield encoder.encode(RunFinishedEvent(thread_id=input_data.thread_id, run_id=input_data.run_id))

    return StreamingResponse(events(), media_type=encoder.get_content_type())
```

寫好之後用 Uvicorn 在本機 8000 埠號啟動這個端點。

```bash
uvicorn server:app --port 8000
```

接著開啟第二個終端機視窗，建立一個新的資料夾並安裝 TypeScript 用戶端套件與執行工具。

```bash
npm init -y && npm pkg set type=module
npm install @ag-ui/client tsx
```

建立一支名為 `client.ts` 的程式，透過 `HttpAgent` 連線到本地伺服器並註冊對應的事件回呼函式。

```ts
import { HttpAgent } from "@ag-ui/client";

const agent = new HttpAgent({ url: "http://localhost:8000/" });

agent.addMessage({ id: crypto.randomUUID(), role: "user", content: "Hi there!" });

await agent.runAgent({}, {
  onRunStartedEvent: ({ event }) => console.log(`run started (AG-UI ${event.protocolVersion})`),
  onTextMessageContentEvent: ({ event }) => { process.stdout.write(event.delta); },
  onRunFinishedEvent: () => console.log("\nrun finished"),
});
```

最後執行 `npx tsx client.ts`，你就會在終端機看到文字逐段串流印出。

這段範例最迷人的地方在於，未來當你把 `server.py` 抽換成 LangGraph、CrewAI 或 Google ADK 打造的複雜代理時，前端 `client.ts` 的串接程式碼完全不用修改。

## 實戰應用場景：不只是聊天室的進階互動模式

很多團隊一開始以為標準事件流只是用來做打字機文字串流，但實際上它真正強大的地方在於處理「非線性人機協作」。

在真實的企業軟體或 SaaS 產品裡，使用者很少只滿足於一個純文字對話框，他們更需要代理能直接操作畫面上的資料表、圖表與表單。

第一個高價值場景是「共享狀態即時同步」。例如你在寫一個 AI 試算表或旅遊行程規劃器，代理在背景修改某個欄位時，不需要每次都把整份龐大的 JSON 重新傳給前端。

後端只需要發出 `STATE_DELTA` 事件，攜帶符合 JSON Patch 標準的局部差異指令，前端就能精準更新畫面上對應的單一儲存格，大幅降低頻寬浪費與畫面閃爍。

第二個關鍵場景是「人機迴圈核准機制」。當代理準備呼叫刪除資料庫或發送外部信件等高風險工具時，後端可以回傳帶有 `interrupt` 中斷狀態的執行結果。

前端收到訊號後會暫停流程並跳出確認視窗，讓使用者檢視、修改甚至駁回代理填寫的參數，等到人類按下確認按鈕後，再將決策結果送回後端恢復執行。

第三個常見場景是「多代理巢狀監控」。當主代理把任務拆解給研究代理與寫作代理時，前端介面往往容易混成一團，不知道現在是誰在說話。

透過子代理事件家族的歸屬標記，前端可以輕鬆把不同子代理的推理過程與工具呼叫折疊成獨立的進度卡片，讓終端使用者清楚掌握系統正在進行的每一個環節。

第四個實用場景則是「多終端同步復用」。同一支部署在雲端的後端代理，可以同時對接網頁版工作台、Slack 機器人與開發者終端機，因為所有介面都只認同一份事件格式，大幅降低跨平台開發的人力成本。

延伸閱讀： [OpenAI 推出 Businesses in ChatGPT：週活 12 億用戶變商業入口，台灣商家看得到吃得到嗎？](https://www.aiposthub.com/businesses-in-chatgpt-business-hub-agents/)

## 安全邊界、架構限制與常見踩坑點

把標準事件流推進正式生產環境時，有幾個工程邊界與安全細節必須提前規劃，才不會上線後踩雷。

### 前端工具執行的權限風險

協議允許代理反過來呼叫註冊在瀏覽器端的前端工具，例如讀取使用者目前的表單狀態、跳轉頁面或觸發介面按鈕。

這裡的鐵則是絕對不要把敏感的商業邏輯或資料庫寫入憑證放在前端工具裡執行，所有涉及權限驗證與資料異動的操作，依然必須經過後端伺服器嚴格核對使用者身份與操作權限。

同時在傳遞共享狀態時，也要避免把後端內部的系統提示詞、模型計費資訊或機密欄位放進 `STATE_SNAPSHOT` 物件中，以免使用者只要打開瀏覽器開發者工具就看光所有敏感資料。

### 狀態不同步與斷線恢復陷阱

在網路不穩定的行動裝置環境下，若前端在串流途中斷線，只依賴 `STATE_DELTA` 增量補丁可能會因為漏接其中一個事件而導致畫面狀態錯亂。

建議在設計長對話或複雜表單應用時，務必在斷線重連或切換分頁回來時，主動向後端請求一次完整的 `STATE_SNAPSHOT` 與 `MESSAGES_SNAPSHOT` 重新校準狀態。

### 版本與中介層相容性

雖然 AG-UI 1.0 規範明確承諾與 0.x 版本保持雙向相容，但在混合使用不同語言的社群 SDK 時，部分進階事件如加密推理欄位或 Protobuf 二進位傳輸的支援進度可能略有差異。

在選用 Kotlin、Go 或 Rust 等社群維護的 SDK 之前，務必先到官方儲存庫的對應目錄確認目前已實作的事件家族覆蓋率，避免開發到一半才發現缺少特定事件編碼器。

## 常見問題

### 這套 ag-ui 協議教學裡提到的協議，跟 CopilotKit 是什麼關係？

AG-UI 最初就是由開源前端代理框架 CopilotKit 團隊發起並捐獻給社群的開放標準。CopilotKit 本身是建構在 React、Angular 與 Vue 上的完整前端元件庫，而這套協議則是底層負責連線通訊的通用標準，你完全可以不裝 CopilotKit，只使用輕量的 `@ag-ui/client` 自己刻介面。

### 我已經在用 MCP 了，還需要再導入這套前端協議嗎？

兩者解決的是完全不同層面的問題。MCP 是設計給「AI 代理連接後端工具、資料庫與檔案系統」使用的，而 AG-UI 則是設計給「AI 代理連接使用者瀏覽器與 App 畫面」使用的，在實務上兩者經常同時並存於同一個系統架構裡。

### 1.0 版本與舊版 0.x 之間會不會有不相容的升級陣痛？

根據官方 1.0 發布說明，新版本具備凍結的 JSON Schema 規格定義，並且在設計上保證雙向相容。這代表採用 1.0 版開發的用戶端可以順利連線到舊版 0.x 後端代理，反之亦然，團隊可以分階段漸進升級。

### 除了官方提供的 TypeScript 與 Python，其他後端語言也能用嗎？

可以。官方直接維護 TypeScript、Python 與.NET 三大主流 SDK，同時社群也已經建立 Kotlin、Go、Java、Dart、Rust、Ruby、C++ 與 Swift 等多語言套件。因為底層傳輸採用標準 HTTP SSE，即使沒有現成套件，任何語言都能依照 JSON Schema 規範自行實作事件編碼。

## 結語

`ag-ui-protocol/ag-ui` 填補了代理開發生態系最後一塊關鍵拼圖，讓前端工程師終於不用再針對每一套新出的 Python 代理框架重複撰寫脆弱的 SSE 解析器。

總結來說，這套協議最適合「正在打造產品級 AI 應用、需要整合多種後端代理框架、且重視共享狀態與人機互動體驗」的全端開發團隊。

相反地，它並不適合「只需要執行純後端自動化腳本、或單純呼叫一次性靜態問答 API」的簡單專案，因為額外引入事件狀態機只會增加不必要的開發複雜度。

如果你想持續掌握這類推動 AI 代理走向工程標準化的重要開源技術與實戰教學，歡迎立即訂閱免費 AI 郵報，每週為你拆解最值得追蹤的開發者工具。