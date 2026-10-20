---
title: "Day 5｜接上真實世界：RAG 讓 LLM 有憑有據 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10383804"
author:
  - "[[iThome]]"
published:
created: 2026-06-08
description: "前言：為什麼需要 RAG？ 昨天 (Day 4) 我們讓 LLM 學會了逐步推理（CoT），避免它「一閃而過」直接給出錯誤答案。但還有另一種常見風險：幻覺與過時。 舉例來說，問模型：「維也納美泉宮幾點..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 5

0

[生成式 AI](https://ithelp.ithome.com.tw/2025ironman/generative-ai)

### 踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友系列 第 5 篇

## Day 5｜接上真實世界：RAG 讓 LLM 有憑有據

- 分享至

## 前言：為什麼需要 RAG？

[**昨天 (Day 4)**](https://ithelp.ithome.com.tw/articles/10382734) 我們讓 LLM 學會了逐步推理（CoT），避免它「一閃而過」直接給出錯誤答案。但還有另一種常見風險： **幻覺與過時** 。

舉例來說，問模型：「維也納美泉宮幾點開門？」它可能：

1. 憑舊記憶回答「9 點」，卻沒注意官網其實改成 10 點。
2. 找不到答案，卻編造一個聽起來合理的時間。

不論哪一種，都會誤導使用者。  
**檢索增強生成（Retrieval-Augmented Generation, RAG）** 的目標，就是在回答前， **先去查找最新且可信的資料** ，讓模型「有憑有據」。

---

## RAG 基本流程

  
*圖：RAG 基本流程。使用者問題先經過查詢轉換，透過向量資料庫、關鍵字搜尋或網頁搜尋取得候選內容，經過整理後再交給 LLM 生成答案，並附上引用。*

這張圖展示了 **最常見的 RAG（Retrieval-Augmented Generation）流程** ：

1. **使用者問題** ：一開始我們丟給系統的問題，可能是「美泉宮幾點開門？」。
2. **查詢轉換（Embedding / 關鍵字）** ：把自然語言的問題轉換成電腦可搜尋的形式，例如語意向量（Embedding），或是抽取關鍵字。
3. **搜尋層（Retrieval）** ：透過三種常見方式尋找候選資料：
	- **向量資料庫** ：找出語意最接近的片段。
		- **關鍵字搜尋** ：依文字比對搜尋文件。
		- **網頁搜尋** ：直接連網抓取最新資訊。
4. **搜尋結果（Top-k Results）** ：從眾多候選中選出最相關的幾筆（通常是前 k 筆）。
5. **整理參考資料（Context Builder）** ：將搜尋到的內容與使用者問題組合，提供給模型一個「有根據的上下文」。
6. **LLM 生成答案** ：大型語言模型（如 GPT、Gemini、Claude）根據這些參考內容生成答案。
7. **輸出（答案 + 引用來源）** ：最終回答會包含來源，讓使用者能夠追溯並驗證資料。

整個流程就像「開書考」：LLM 不再靠記憶亂猜，而是先翻資料，再基於找到的依據作答。這正是 RAG 的核心價值—— **用真實資料降低幻覺，提升可信度** 。

#### 相關研究

Lewis, et al. [*Retrieval-augmented generation for knowledge-intensive NLP tasks.*](https://dl.acm.org/doi/abs/10.5555/3495724.3496517) (NeurIPS 2020)  
→ 這篇為 RAG 的源頭研究，提出「檢索 + 生成」的架構，為後來眾多應用奠定了基礎。

---

## RAG 什麼時候該用？

- **適合** ：需要引用來源、模型知識可能過時、或要基於特定文件回答
- **不適合** ：純數學運算、強即時資訊（天氣、股價）、純創意寫作

簡單的判斷：

- 「這題需要最新或有根據的答案嗎？」→ 用 RAG
- 「這題答案長期穩定不變嗎？」→ 直接問 LLM

---

## RAG 的常見實作方式

雖然大家常把 RAG 想成「丟進向量資料庫再查出來」，但實際上有不同的做法，各有適合的情境：

1. **向量搜尋（Vector Search）**
	- 把問題與文件段落轉換成向量，依語意相似度來搜尋。
		- 優點：能處理同義詞或語意接近的問題（例如「維也納皇宮」也能找到「美泉宮」）。
		- 缺點：需要額外的向量化步驟，效能與準確度取決於編碼模型。
2. **關鍵字搜尋（Keyword Search）**
	- 直接比對字詞，類似傳統搜尋引擎或全文索引。
		- 優點：快速、解釋性強，對精確詞彙（如「票價」、「9 點開門」）特別有效。
		- 缺點：對於語意模糊或換句話說的問題不一定能命中。
3. **Web 搜尋**
	- 透過搜尋 API 或即時網頁資料取得答案。
		- 優點：可用於最新資訊（例如「今天維也納下雨嗎？」）。
		- 缺點：結果品質依賴搜尋引擎，還需要額外處理雜訊與不可信來源。

這樣的比較讓我們理解：  
**RAG 不是只有向量庫** ，而是可以靈活結合不同搜尋方式，依需求挑選最合適的管道。

---

## Demo：最簡單的 RAG (Naive)

**目標** ：從零理解 RAG 的運作  
**流程** ：建立文件庫 → 向量化 → 搜尋 → 生成答案

**安裝相依套件**

```bash
pip install -U sentence-transformers faiss-cpu google-generativeai
```

**範例程式**

```python
from sentence_transformers import SentenceTransformer
import faiss
import google.generativeai as genai

# 1. 初始化編碼模型（BGE 系列對中文檢索效果不錯）
encoder = SentenceTransformer("BAAI/bge-base-zh-v1.5")

# 2. 文件庫（示範資料）
docs = [
    "美泉宮（Schönbrunn Palace）通常每天上午 9 點開門。",
    "美景宮（Belvedere Palace）一般營業時間是上午 10 點。",
    "聖史蒂芬大教堂（St. Stephen's Cathedral）通常上午 8 點開放。"
]

# 3. 建立向量索引
embeddings = encoder.encode(docs)
index = faiss.IndexFlatL2(embeddings.shape[1])
index.add(embeddings)

# 4. 查詢
query = "美泉宮幾點開門？"
q_emb = encoder.encode([query])
D, I = index.search(q_emb, k=1)
retrieved = docs[I[0][0]]

# 5. 呼叫 Gemini（免費額度可用）
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.0-flash")

prompt = f"根據以下資訊回答問題：\n\n資訊：{retrieved}\n\n問題：{query}\n請用中文簡潔回答。"
response = llm.generate_content(prompt)

print("搜尋結果:", retrieved)
print("LLM 回答:", response.text)
```

**輸出結果**

![Naive RAG 輸出結果](https://ithelp.ithome.com.tw/upload/images/20250920/20165544i72OYrdsid.png)  
*圖：Naive RAG 範例程式的實際輸出。模型先檢索到「美泉宮通常每天上午 9 點開門」的相關段落，再依據檢索結果生成答案。這樣的流程，就像讓 LLM 在「開書考」中作答：不是憑記憶或幻覺，而是依據明確依據來回應。*

**你剛剛完成了最簡單的 RAG** ：

- **搜尋** ：從文件庫找出最相關內容
- **增強** ：把命中文本塞進 Prompt
- **生成** ：LLM 在證據上作答

---

## Demo 2：用 Web 搜尋做 RAG

前一個範例使用的是「本地知識庫」——先準備一批文件，再建立索引。這種方式適合固定的 FAQ、產品文件或內部資料。

但如果我們想查的是 **最新的資訊** ，例如「美泉宮今天幾點開門？」或「這週維也納的天氣如何？」光靠本地文件就不夠了。這時候就需要 **Web 搜尋型的 RAG** 。

這裡我們示範透過 **[DuckDuckGo（DDG）搜尋引擎](https://github.com/deedy5/ddgs)** 來擴充 RAG：它支援中文查詢、不需要申請 API key，適合快速整合進入原型。

**安裝套件**

```bash
pip install ddgs google-generativeai
```

**範例程式**

```python
from ddgs import DDGS
import google.generativeai as genai

# 1. 問題
query = "美泉宮 Schönbrunn Palace 幾點開門？"

# 2. 執行 DuckDuckGo 搜尋（支援中文，免 API key）
with DDGS() as ddgs:
    results = [r["body"] for r in ddgs.text(query, region="tw-tzh", max_results=10)]

retrieved = "\n".join(results)

# 3. 呼叫 Gemini（免費額度可用）
genai.configure(api_key="你的_API_KEY")
llm = genai.GenerativeModel("gemini-2.5-flash")

prompt = f"根據以下網路搜尋資訊回答問題：\n\n資訊：{retrieved}\n\n問題：{query}\n請用中文簡潔回答，並在答案後標註來源。"
response = llm.generate_content(prompt)

# 4. 輸出
print("搜尋結果:", retrieved)
print("LLM 回答:", response.text)
```

**輸出結果**

![Web 搜尋型 RAG 實際輸出](https://ithelp.ithome.com.tw/upload/images/20250920/20165544NnwK66Nd96.png)  
*圖：Web 搜尋型 RAG 範例程式的實際輸出。這次不依賴本地文件庫，而是透過 DuckDuckGo 即時搜尋，取得美泉宮的開放時間，再交給 LLM 生成答案。這種方式適合需要「最新、公開資訊」的情境，例如查詢營業時間、新聞或官網公告。*

這樣的 **Web 搜尋型 RAG** 和前一個 **Naive RAG** 剛好形成對比：

- Naive RAG：適合固定知識（FAQ、產品手冊）
- Web RAG：適合需要最新資訊（官網、新聞、即時資訊）

---

## 與 CoT 的關係

昨天的 **CoT** 解決「推理透明」，今天的 **RAG** 解決「答案可靠」。 **最佳組合** 是：

1. 先用 RAG 找到相關資料
2. 再用 CoT 在資料基礎上逐步推理
3. 得到既有邏輯、又有根據的答案

例如：

> 問：「美泉宮值得花一整天參觀嗎？」
> 
> - RAG 找到：宮殿 1,441 個房間、花園、動物園、迷宮…
> - CoT 推理：面積大、內容多，確實值得一整天
> - 結論：建議排一天行程

---

## RAG 的局限性：遇到「即時問題」就卡住

雖然 RAG 很有用，但它也有天生的限制。  
舉個例子：

> 問題：「今天維也納下雨嗎？」

- **RAG 的做法** ：從知識庫或旅遊網站找到「維也納夏天常有午後雷陣雨」這類靜態資訊。
- **結果** ：答案正確性有限，因為這些資料無法告訴你「今天」的天氣。
- **關鍵差異** ：RAG 解決的是「過去或半靜態知識」的問題，但 **對於需要即時更新的資訊，它無能為力** 。

同樣的情況還有：

- 匯率（今天 1 歐元換多少新台幣？）
- 即時交通（現在到美泉宮要多久？）
- 訂票（能不能幫我訂明天下午的導覽票？）

這些問題都需要 **直接呼叫外部工具或 API** ，而不是只靠搜尋。

因此，RAG 不是萬靈丹，它解決了「讓答案有依據」的問題，但要處理 **即時、互動、行動** 的需求，就必須進一步學會—— **使用工具** 。

這也將是我們接下來會探討的重點。

---

## 小結

- **RAG 的價值** ：讓 LLM 接上真實世界，以最新可信的資料約束答案。
- **今天學到** ：從 Naive RAG 範例出發，理解 RAG 的基本流程與常見實作方式，並知道它適合解決哪些問題、哪些情境仍有限制。
- **RAG 與 CoT** ：兩者互補，一個讓答案有根據，一個讓推理更透明。
- **局限性** ：RAG 無法處理即時資訊或需要動作的需求，這正是我們接下來要進一步學會的—— **使用工具** 。

就像昨天讓模型「學會思考」，今天我們讓它「找到證據」。而明天，我們要讓模型 **真的能「動手做事」** 。

---

![維也納美泉宮](https://ithelp.ithome.com.tw/upload/images/20250919/20165544GIsKg27gqL.jpg)  
*圖：奧地利國家圖書館（Austrian National Library, Prunksaal）。穹頂壁畫與典籍交織成無盡的知識星河，提醒我們：LLM 若不倚靠真實資料，往往在幻覺中徘徊；透過 RAG，更能在浩瀚語言的殿堂裡，找到憑據與方向。（攝影：作者自攝）*

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10382734)

[

Day 4｜推理是第一步：Chain-of-Thought (CoT) 讓 LLM 從直覺走向可靠

](https://ithelp.ithome.com.tw/articles/10382734)[下一篇](https://ithelp.ithome.com.tw/articles/10384461)

[

Day 6｜行動是關鍵：Tool Use 讓 LLM 突破回答的侷限

](https://ithelp.ithome.com.tw/articles/10384461)

系列文

[踏上 Agentic AI 探索之旅：我不再獨自升級！覺醒你的 AI 替身，打造智慧協作隊友](https://ithelp.ithome.com.tw/users/20165544/ironman/8801) 共 30 篇

目錄

1. 26
	[Day 26｜AI 的守護靈獸：Guardrails 讓 Agent 劃出安全邊界](https://ithelp.ithome.com.tw/articles/10395588)
2. 27
	[Day 27｜讓 AI 暫停思考：Human-in-the-Loop 讓 Agent 與人類成為協作夥伴](https://ithelp.ithome.com.tw/articles/10396238)
3. 28
	[Day 28｜讓 AI 用流程思考：n8n 讓 Agent 以工作流驅動協作](https://ithelp.ithome.com.tw/articles/10396549)
4. 29
	[Day 29｜旅途最後一哩：人機協作讓 Agent 成為並肩共創的隊友，不再獨自升級！](https://ithelp.ithome.com.tw/articles/10396972)
5. 30
	[Day 30｜旅途的終點，也是下一個起點：回顧 30 天的 Agentic AI 探索之旅](https://ithelp.ithome.com.tw/articles/10397208)
[完整目錄](https://ithelp.ithome.com.tw/users/20165544/ironman/8801)

[![.](https://itadstatic.ithome.com.tw/B9/1779447171_6a10358375783.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2605B9004)