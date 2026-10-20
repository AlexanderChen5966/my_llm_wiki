---
title: "Graph Engineering是什麼？Loop、Node、Edge、State與三個AI工作流模板"
source: "https://yololab.net/archives/graph-engineering-ai-agent-workflow-guide"
author:
  - "[[YOLOlab]]"
published: 2026-07-27
created: 2026-07-29
description: "Graph Engineering把Agent、工具、程式與人工審核組成一張可追蹤的任務圖。本文釐清它與Loop Engineering的關係，說明Node、Edge、State、路由與驗證，並提供文章、程式開發和研究三種入門模板。"
tags:
  - "clippings"
status: processed
---
Graph Engineering是一種設計AI工作流程的方法：把模型、工具、程式、人工審核與驗證器拆成不同節點，再用明確路徑、共享狀態和停止條件控制它們如何協作。

機器學習與人工智慧

它沒有宣告Loop失效。Loop是Graph中的循環路徑；Graph Engineering真正擴大的，是工程師從管理單一Agent的反覆執行，進一步處理多個工作單元之間的分工、交接、並行與恢復。

## 重點快讀

- Graph由節點、邊與狀態構成，節點不一定是AI。
- Loop是Graph中的循環結構，仍是修正、測試和探索任務的重要模式。
- 固定規則應優先交給程式，模型負責需要理解、分類或判斷的部分。
- 多Agent只有在角色、權限、工具或上下文確實需要分離時才有價值。
- 第一張Graph應保持精簡，並為每個節點設定輸入、輸出、驗收與最大重試次數。

## Graph Engineering為什麼突然受到討論？

2026年7月18日，OpenClaw創作者Peter Steinberger在X提出：「我們還在談Loop，還是已經轉向Graph？」約四個半小時後，Hamel Husain發布〈Loop Engineering Is Dead. Enter Graph Engineering〉，讓Graph Engineering迅速成為AI工程社群的新話題。

視覺藝術與設計

這場討論距離Addy Osmani於2026年6月7日系統化說明Loop Engineering約41天。Osmani所描述的Loop Engineering，是設計一套能發現工作、交給Agent、檢查結果、保存紀錄並決定下一步的系統，而非持續由人手動輸入下一條提示詞。

「Loop已死」比較接近吸引注意的標題。狀態機、DAG、條件路由與工作流編排早已存在；2026年的新變化，是節點開始大量使用能自行理解任務、呼叫工具和產生不確定輸出的語言模型。

因此，Graph Engineering目前適合被視為一個實用的工程框架，而不是已經具有單一定義、證照或標準規格的新學科。

## Graph Engineering到底是什麼？

一張最小可用的Graph包含三個元素。

### Node：誰負責執行工作

Node是工作單元，可以是一次LLM呼叫、一個搜尋Agent、一段Python程式、資料庫查詢、API工具、測試套件、人工確認，或文件與程式碼審查器。

節點是否值得獨立存在，要看它是否具有不同的責任、工具、權限、上下文或驗收方式。將「讀取資料」、「稍微整理」和「換成列表」拆成三個Agent，通常只會增加Token和交接成本。

### Edge：完成後要往哪裡走

- **固定順序：** 研究完成後進入寫作。
- **條件分支：** 測試成功就結束，失敗就進入修復。
- **並行執行：** 同時搜尋市場、競品與技術資料。
- **循環路徑：** 審閱失敗後退回修改。
- **人工閘門：** 發布、付款或部署前等待人確認。

LangGraph與Microsoft AutoGen GraphFlow的官方文件，都以節點、邊、狀態、條件分支、並行與循環描述這類工作流。

### State：節點之間傳遞什麼

State是整張圖的工作記憶。它不應只是越來越長的聊天紀錄，而應是有結構的任務資料。

```
topic: "Graph Engineering"
research_notes: []
verified_claims: []
draft: ""
review_status: "pending"
revision_count: 0
source_conflicts: []
final_approved: false
```

每個節點只讀取必要欄位，也只修改自己負責的欄位。研究節點不應直接覆蓋文章正文，審閱節點也不應悄悄修改原始來源。

## Graph不等於一群AI

Graph Engineering經常和Multi-agent一起出現，但兩者不能畫上等號。

```
讀取表單
   ↓
程式檢查欄位
   ↓
LLM判斷問題類型
   ↓
查詢資料庫
   ↓
人工核准
   ↓
寄出回覆
```

整個流程只有一個LLM節點，其餘節點都是確定性程式、工具與人工操作。

Google在ADK 2.0中強調圖式工作流可以混合確定性程式與AI推理。固定商業規則由Graph控制，需要解讀自然語言或處理模糊資訊時才交給模型，可以降低模型隨意跳過步驟、走錯路徑與重複呼叫工具的風險。

機器學習與人工智慧

Graph Engineering的關鍵不是Agent數量，而是責任是否清楚。

## Loop和Graph是什麼關係？

Loop是Graph中的循環路徑。

```
寫程式 → 測試 → 分析錯誤 → 修復
   ↑                         ↓
   └──────────失敗──────────┘
```

Graph則可以同時包含順序、分支、並行、人工核准和多個Loop。因此，Loop Engineering解決的是如何讓一個執行單元根據回饋持續修正；Graph Engineering進一步處理多個執行單元如何被安排在同一套系統內。

需要深入理解控制迴路、Verifier、Retry與停止條件，可延伸閱讀 [Closed-loop AI Agent怎麼設計？狀態機、控制迴路、驗證器與停止條件](https://yololab.net/archives/closed-loop-ai-agent-engineering-blueprint) 。

## 什麼時候只需要一個Loop？

- 翻譯一段文字。
- 產生十個標題。
- 修改單一函式。
- 摘要一份短文件。
- 只有一種工具與一種輸出。
- 人類可以立即判斷結果是否正確。

Anthropic與OpenAI都建議先使用能完成任務的最簡單架構。多Agent和複雜工作流會增加延遲、Token、除錯與整合成本，只有在單一Agent確實無法維持指令、工具選擇或品質時才值得擴張。

機器學習與人工智慧

## 哪些訊號代表需要Graph？

### 需要不同權限或工具

研究Agent只能讀取網頁，寫作Agent只能建立草稿，發布節點則必須等待人工核准。這些責任不應放在同一個具有全部權限的Agent裡。

### 存在清楚的條件分支

程式編譯成功與失敗需要走不同路徑；客服問題依帳務、技術或安全類別轉交不同流程。

### 任務可以自然並行

市場規模、競品、使用者需求與法規可以分別搜尋，再等待所有結果完成後匯總。

### 需要保存Checkpoint

長任務可能因網路、額度或人工核准而暫停。流程必須知道已完成什麼、使用哪個資料版本，以及恢復時從哪個節點繼續。

### 單一Agent的角色開始互相衝突

同一套提示詞同時要求模型研究、寫作、審查、批准與發布，容易使標準混在一起。將審查與寫入權限分開，通常比繼續增加提示詞更可靠。

## Graph Engineering的五個核心模組

### 1\. Node Contract

- 任務目標。
- 可讀取的狀態。
- 可修改的欄位。
- 可使用的工具。
- 輸出格式。
- 驗收標準。
- 時間與Token上限。
- 失敗時的處理方式。

兩個節點若能合併，而且不會失去權限隔離、專業能力或獨立驗收，就不需要拆開。

### 2\. Deterministic Routing

```
if tests_passed:
    route = "finish"
elif retry_count < 3:
    route = "repair"
else:
    route = "human_review"
```

能用程式判斷的條件，優先使用程式。模型路由適合處理語意分類、風險判斷與資料不足等難以完整列舉的情況。即使由模型路由，也應限制可選路徑，並保存判斷結果與依據。

### 3\. Structured State

State需要Schema、版本與欄位Owner。錯誤資料一旦寫入共享狀態，後續節點可能把它當成事實繼續處理。重要主張應保留來源、查核狀態與有效時間，不能只保存整理後的文字。

### 4\. Independent Verification

審閱節點應取得乾淨上下文、任務規格與可驗證證據，而非直接繼承執行Agent的全部推理歷史。

更換模型可以增加觀點差異，但不能保證正確。即使使用相同模型，獨立上下文的Reviewer仍可能找出問題；對共享程式碼或文件，較穩定的做法是讓多個Agent提供唯讀意見，但將最終寫入維持為單一路徑。

程式碼應由實際Build和Test驗證；資料應由Schema與來源驗證；正式發布則應由回讀結果和人工核准驗證。

### 5\. Budget與停止條件

- 最大節點執行次數。
- 最大修正輪數。
- 最大Token與工具費用。
- 最長執行時間。
- 最大平行數。
- 需要人工接手的條件。
- 不可逆操作前的核准節點。

Graph可以把失敗分散到更多節點，也可以讓成本在並行中快速增加。沒有預算與停止條件的Graph，只是規模更大的失控Loop。

多代理角色、Manager、Handoff與共享狀態的完整設計，可延伸閱讀 [多代理工作流怎麼編排？Manager、Handoff、共享狀態與衝突控制](https://yololab.net/archives/claude-dynamic-workflows-automation-guide) 。

## 三個可以直接使用的Graph模板

### 模板一：深度文章Graph

```
站內查重
   ↓
研究與來源查核
   ↓
建立文章大綱
   ↓
撰寫初稿
   ↓
獨立審閱
   ├─ 通過 → 人工確認 → 建立草稿
   └─ 不通過 → 修改正文 → 再次審閱
                         最多2次
```
```
search_intent: ""
existing_articles: []
verified_sources: []
outline: []
draft: ""
review_issues: []
revision_count: 0
approval: false
```

研究節點負責事實，寫作節點負責表達，審閱節點只輸出問題清單，WordPress寫入則必須位於人工確認之後。

### 模板二：程式開發Graph

```
讀取需求與Repository
   ↓
建立修改方案
   ↓
建立隔離工作區
   ↓
修改程式碼
   ↓
執行Lint、Type Check與Test
   ├─ 通過 → 產生Diff與測試報告 → 人工Review
   └─ 失敗 → 分析錯誤 → 修復 → 重新測試
                              最多3次
```

測試節點不應只接受模型宣稱「已完成」。它需要保存實際命令、退出碼、失敗案例與未執行項目。

### 模板三：並行調研Graph

```
┌→ 市場規模搜尋 ─┐
                 ├→ 競品搜尋 ─────┤
定義研究問題 ────┼→ 使用者痛點 ───┼→ 等待全部完成 → 去重整合 → 事實查核
                 ├→ 技術趨勢 ─────┤
                 └→ 法規與風險 ───┘
```
```
claim: ""
evidence: ""
source_type: "primary | secondary"
published_date: ""
confidence: "high | medium | low"
```

並行搜尋可以縮短等待時間，但匯總節點仍要處理重複、矛盾、時間差與來源品質。

## 新手如何畫出第一張Graph？

### 第一步：寫出完成條件

例如：產出一篇具備五個可靠來源、通過事實查核，而且尚未寫入WordPress的繁體中文文章草稿。

### 第二步：找出真正不同的責任

1. 研究。
2. 寫作。
3. 審閱。

### 第三步：定義每個節點的輸入和輸出

研究讀取主題，輸出來源與主張；寫作讀取已查核資料，輸出初稿；審閱讀取規格與初稿，輸出問題清單。

### 第四步：畫出固定路徑與條件路徑

先畫正常路徑，再補充失敗、重試和人工介入。

### 第五步：設定停止條件

- 什麼狀態算成功？
- 最多修正幾次？
- 哪些問題必須交給人？
- 達到多少成本或時間就停止？

能在一張紙上說明這五件事，就已經完成第一張可實作的Graph。

## 新手常見的五個錯誤

### 為了看起來複雜而增加節點

節點數量不是成熟度。單一模型呼叫能穩定完成的工作，不需要拆成五個Agent。

### 把所有對話當成State

長對話缺乏欄位責任、版本與來源。State應保存結構化事實與Artifact，而非無限制累積聊天內容。

### 所有路由都交給模型

確定性條件交給模型只會增加成本與不穩定。模型應集中在需要語意理解和模糊判斷的位置。

### 讓所有Agent都能修改共同產物

並行寫入容易產生覆蓋、衝突與無法追蹤的決策。若沒有Branch、Lock、Merge和Owner機制，應維持單一寫入者。

### 只有重試，沒有失敗狀態

達到上限後，Graph應保存最後可靠狀態、錯誤證據與未完成項目，再交給人工處理。

## 常見問題

### Graph Engineering是正式的新技術標準嗎？

目前不是。它是2026年7月開始受到廣泛討論的工程用語，用來描述以Graph管理Agent、工具、狀態、路由與驗證的方法。其底層概念來自既有的Workflow、DAG、狀態機與Agent Orchestration。

### Graph Engineering一定要使用LangGraph嗎？

不一定。LangGraph是具代表性的Graph-based Agent框架，但Google ADK、Microsoft AutoGen、一般工作流引擎，甚至自行撰寫的狀態機都能實作相同概念。

### 一張Graph可以只有一個Agent嗎？

可以。模型、程式、資料庫、工具與人工核准都可以成為節點。許多可靠系統只有少量LLM節點，大部分路徑由確定性程式控制。

### 多Agent一定比單一Agent更快嗎？

不一定。只有相互獨立的子任務才能有效並行。若Agent需要頻繁交換上下文或修改共同檔案，協調成本可能高於節省的時間。

### 如何判斷Graph是否設計得太複雜？

移除一個節點後，如果權限、品質、恢復能力與驗收方式都沒有損失，該節點通常沒有獨立存在的必要。第一版Graph應盡量在一張紙上完整呈現。

## 從Loop走向Graph，真正增加的是管理責任

Graph Engineering提供了一套更清楚的語言，讓工程師描述誰負責什麼、資料如何移動、哪條路徑可以自動執行，以及何時必須停止。

名稱可能持續改變，底層問題不會消失。當AI開始參與更長、更複雜且具有副作用的工作，系統就必須管理狀態、權限、驗證、預算與責任。

機器學習與人工智慧

先建立一個可以驗收的Loop，再把真正需要分工的部分連成Graph。這條路比一開始部署大量Agent更慢一點，卻更容易除錯、控制與長期維護。

## 資料來源與延伸閱讀

- [Peter Steinberger：Loop與Graph原始貼文](https://x.com/steipete/status/2078277297791189132)
- [Hamel Husain：Loop Engineering Is Dead. Enter Graph Engineering](https://x.com/HamelHusain/article/2078346425621237935)
- [Addy Osmani：Loop Engineering](https://addyosmani.com/blog/loop-engineering/)
- [Anthropic：Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)
- [Google：ADK 2.0](https://adk.dev/2.0/)
- [LangGraph：Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)
- [LangGraph：Persistence](https://docs.langchain.com/oss/python/langgraph/persistence)
- [Cognition：Multi-agent實務](https://cognition.com/blog/multi-agents-working)
- [AI Agent工作流怎麼設計？從任務契約到驗證的七層架構](https://yololab.net/archives/ai-agent-workflow-guide-mcp-memory-skills)

*資料查核日期：2026年7月27日。*