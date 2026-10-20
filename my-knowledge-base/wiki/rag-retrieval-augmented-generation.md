---
title: RAG：讓 LLM 有憑有據的檢索增強生成
date: 2026-06-17
updated: 2026-08-07
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10383804
type: article
status: processed
asset_value: 68
---

LLM 的幻覺根源有兩種：一是訓練資料已過時，二是模型憑直覺填補知識缺口。RAG 的解法不是換更強的模型，而是在回答前先查資料——把問題從「LLM 記得什麼？」改成「LLM 能用找到的資料說什麼？」

## T（核心論點）

Retrieval-Augmented Generation（RAG）在 LLM 生成答案前，先從外部資料來源檢索最相關的片段，把這些片段作為上下文送入模型，讓模型「有根據地作答」並附上來源。核心價值是把「靠記憶猜」換成「翻資料答」，降低幻覺率、提升可信度與可查核性。（Lewis et al., NeurIPS 2020）

## B（適用場景）

| 場景 | 用法 |
|------|------|
| 需要引用來源（FAQ、產品文件、法規查詢）| 向量 RAG 或關鍵字 RAG |
| 模型知識可能過時（官網公告、新聞）| Web 搜尋 RAG |
| 基於特定私有文件回答 | 本地向量資料庫 RAG |

**不適合：** 純數學運算（直接問 LLM）、強即時資訊如天氣股價（需工具呼叫 API）、純創意寫作。

## R（三種檢索方式與實作）

### 七步驟基本流程

```
問題 → 查詢轉換（Embedding/關鍵字）
     → 搜尋層（向量DB/關鍵字/Web搜尋）
     → Top-k 結果
     → Context Builder（組合問題+檢索片段）
     → LLM 生成答案
     → 輸出（答案+引用來源）
```

### 三種檢索方式比較

| 方式 | 優點 | 缺點 |
|------|------|------|
| 向量搜尋 | 處理同義詞/語意模糊 | 需要 embedding 步驟，準確度取決於編碼模型 |
| 關鍵字搜尋 | 快速、對精確詞彙效果好 | 換句話說的問題可能命中不了 |
| Web 搜尋 | 可取得即時資訊 | 結果品質依賴搜尋引擎，需過濾雜訊 |

### Demo 1：Naive RAG（本地向量資料庫）

```python
from sentence_transformers import SentenceTransformer
import faiss
import google.generativeai as genai

encoder = SentenceTransformer("BAAI/bge-base-zh-v1.5")  # 對中文效果佳
docs = [
    "美泉宮（Schönbrunn Palace）通常每天上午 9 點開門。",
    "美景宮（Belvedere Palace）一般營業時間是上午 10 點。",
]

# 建立向量索引
embeddings = encoder.encode(docs)
index = faiss.IndexFlatL2(embeddings.shape[1])
index.add(embeddings)

# 查詢 → 檢索 → 生成
query = "美泉宮幾點開門？"
q_emb = encoder.encode([query])
D, I = index.search(q_emb, k=1)
retrieved = docs[I[0][0]]

genai.configure(api_key="...")
llm = genai.GenerativeModel("gemini-2.0-flash")
prompt = f"根據以下資訊回答問題：\n\n資訊：{retrieved}\n\n問題：{query}\n請用中文簡潔回答。"
response = llm.generate_content(prompt)
```

### Demo 2：Web 搜尋 RAG（DuckDuckGo，免 API key）

```python
from ddgs import DDGS

query = "美泉宮 Schönbrunn Palace 幾點開門？"
with DDGS() as ddgs:
    results = [r["body"] for r in ddgs.text(query, region="tw-tzh", max_results=10)]
retrieved = "\n".join(results)
# → 送入 LLM 同上
```

**Naive RAG 適合：** 固定知識（FAQ、產品手冊）。**Web RAG 適合：** 需要最新資訊（官網公告、新聞）。

### CoT × RAG 黃金組合

1. RAG 先找到相關資料（讓答案有根據）
2. CoT 在資料基礎上逐步推理（讓推理透明）
3. 得到既有邏輯又有根據的答案

例：問「美泉宮值得花一整天？」→ RAG 找到 1,441 個房間、花園、動物園資料 → CoT 推導「面積大、內容豐，確實值得一整天」。

### RAG 的根本限制

RAG 解決的是「過去或半靜態知識」問題。對需要即時更新的資訊（天氣、即時交通、訂票行動），仍需直接呼叫外部工具或 API，不能靠搜尋解決。

## C（關鍵行動）

- [ ] 判斷任務前先問：「這題需要最新或有根據的答案嗎？」→ 用 RAG；「答案長期穩定不變嗎？」→ 直接問 LLM
- [ ] 中文語意搜尋優先考慮 BGE 系列 embedding 模型（`BAAI/bge-base-zh-v1.5`），對中文檢索效果較好
- [ ] RAG + CoT 組合使用：先檢索建立事實基礎，再讓模型逐步推理，比單用任一方式都更可靠
- [ ] 即時資訊需求（天氣/股價/訂票）改用工具呼叫，不要期待 RAG 能解決

## 相關連結

- [[chinese-rag-preprocessing-baseline]] — **繁中前處理的實測細節**：清理的三條界線（全形標點不轉、不用 NFKC、程式碼區塊原子化）、四種 Chunk 策略實測（固定長度把「登入」切成兩半）、jieba 預設詞典切壞繁體詞、以及「所有詞都值一分」的搜尋基準失敗模式
- [[converged-database-ai-data-foundation]] — 檢索的基礎架構面：RAG 同時需要新鮮度／權限／篩選條件，多儲存系統同步落差會讓 LLM「自信地答錯」
- [[chain-of-thought-reasoning]] — CoT 與 RAG 互補：推理透明度 vs 答案可信度
- [[langchain-rag-architectures]] — LangChain 三種 RAG 架構（2-Step/Agentic/Hybrid）進階版本
- [[langchain-tool-use]] — RAG 的下一步：當即時資訊需求出現時改用 Tool Use
- [[enterprise-rag-smb-architecture]] — 中小企業級 RAG 完整工程落地（8 模組骨架、混合檢索、Rerank 降級、鄰居 Chunk 補全），本篇基礎流程的實戰深化版
- [[lightrag-architecture-and-query-modes]] — 知識圖譜化的 RAG 解法：本篇向量 RAG 遇到跨文件關聯查詢受限時，LightRAG 用雙層架構（圖譜+向量）補足

---

## 補充資料：企業 RAG 的 ingestion pipeline 與檢索策略（2026-08-07 併入）

來源：iThome《Data Machi 30 天：從零開始打造企業 AI 知識工作流》Day 6–8 三篇合併。本頁原本偏「RAG 是什麼＋三種檢索方式」，此處補上**建知識庫那半場**（Parse→Chunk→Embed→Index）與**語意搜尋的失效模式**。

### 一、企業為什麼需要 RAG——三個具體問題

1. **私有資料不在模型訓練資料中**
2. **企業資料持續更新**
3. 🔑 **回答需要來源依據**——這一條的要求比想像中細，可追溯性要能回答五件事：

> 答案來自**哪一份文件**／文件是**哪一個版本**／相關內容位於**哪一頁**／**能否回到原始段落確認**／文件**是否仍在有效期間內**

（最後一項最容易被忽略——檢索到的可能是已失效的舊版辦法）

### 二、從 PDF 到知識庫的四步 pipeline

```
Parse（打開外殼取出可處理內容）→ Chunk（切成可搜尋片段）→ Embed（轉向量）→ Index（建可快速搜尋的索引）
```

**第一步 Parse 的實務難處**（不是「把 PDF 轉文字」這麼簡單）——掃描型 PDF、表格、多欄排版、圖表與圖片、頁首頁尾、頁碼、表單、合併儲存格、文字與圖片混合的操作手冊。

**第二步 Chunk 的權衡**：
| 問題 | 後果 |
|---|---|
| **Chunk 太大** | 模型難以聚焦真正答案／Prompt 變長／Token 成本增加／**重要內容被大量背景資訊稀釋** |
| **Chunk 太小** | 語意被切斷（例：「高優先級需求需在一個工作天內完成初步評估」與「若涉及資安事件則必須立即升級至主管處理」被拆開，就失去了條件關係）|

> 目標：**在檢索精準度與上下文完整性之間找到平衡。**

**Data Machi 的實作參數（可作起點）**：**Chunk Size 512 tokens ＋ Chunk Overlap 50 tokens**。重疊的作用是讓 Chunk A 的結尾也出現在 Chunk B 開頭，使跨 Chunk 的概念不易完全斷裂、命中相鄰 Chunk 時上下文較完整。

### 三、🔑 語意搜尋也會「理解過頭」——本節是最實用的判準

本頁原本列了三種檢索方式，但沒講**語意搜尋什麼時候會壞**。

**關鍵字搜尋的強項**（精確識別碼）：員工編號、產品 SKU、合約號碼、系統名稱、法條條號、專案代碼、文件名稱、特定縮寫。

**關鍵字搜尋的限制**：**使用者通常用自己的語言描述問題，但企業文件使用的是公司的正式術語**——
- 使用者問「人離職後帳號會怎麼處理？」，文件標題是「離職者資訊系統存取權限撤銷」
- 使用者問「正式環境出問題要找誰？」，文件標題是「Production Incident Escalation Procedure」
- 加上中英混用、正式名稱與口語簡稱並存、不同部門用不同術語

**但語意搜尋反過來會「理解過頭」**：
> `EMP-01837` **根本沒有什麼需要理解的語意**。這時要問的是「**哪一筆資料精確包含 EMP-01837？**」

🔑 **一句話判準**：
> **這個問題需要「精確找字」，還是「理解意思」？**

⚠️ 並且——**關鍵字搜尋並沒有因為 Embedding 出現就變得過時。**

### 四、Hybrid Search 的理由：一個查詢常同時含兩種需求

企業 RAG 常常兩種都要，因為單一查詢裡就混著兩類：

> 查詢「**Okta 離職帳號撤銷流程**」——`Okta` 是非常重要的**精確產品名稱**（要關鍵字），「離職帳號撤銷流程」則是**自然語言描述**（要語意）。

→ 這正是既有條目 [[lightrag-architecture-and-query-modes]] 的 `hybrid` / `mix` 模式存在的理由。

### ⚠️ 反面觀點：不是所有場景都該上 RAG

[[hand-coding-agent-file-tools]] 記錄了明確的反面論證（作者刻意不走 RAG）：

> RAG 要多出來的**不是只一個函式**——內容怎麼切塊、拿什麼算 embedding、向量存哪裡、**檔案改了索引怎麼跟著更新**都得考慮。
> 小型、持續變動的專案，**先用關鍵字搜尋已經夠用**；Anthropic 的 just-in-time 路線（正版 Claude Code 先拿檔名路徑等輕量線索，需要時才用 glob/grep 載入）就是另一條路。

→ **判準：RAG 的主要成本在 ingestion 與索引維護，不在檢索本身。** 資料相對穩定、量大、需要跨術語命中 → RAG；小型高頻變動、關鍵字夠用 → 直接搜尋。

### 同系列其餘兩篇（已獨立成條目）

- [[multimodal-document-processing-three-layers]] — **Day 9**：本節「Parse 的實務難處」清單（掃描型 PDF／表格／多欄排版／圖表）的**完整解法**——三層策略、視覺摘要的結構化 JSON 格式（含 `do_not_confuse` 適用邊界欄位）、Lazy OCR 成本控制
- [[enterprise-ai-five-stage-evolution]] — **Day 05**：Chat→RAG→Tool Use→Agent→Agentic Workflow→產品五段演進；本頁的 RAG 只是其中一段
- [[rag-as-agent-tool]] — **同系列 Day 10**：接續回答「RAG 找到資料之後呢」——🔑 **RAG 與 Agent 不是同層次的競爭方案，RAG 是 Agent 手上的一項 Tool**；Coordinator / Tool 兩層架構、為什麼不該把所有企業資料塞進同一個向量庫（更新頻率／查詢方式／權限／準確性四道差異）、Agentic RAG 與「工具越多不代表 Agent 越強」
