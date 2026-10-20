---
title: "從 0 到 1 構建企業級 RAG：一個中小企業可落地版本的完整架構"
source: "https://codelove.tw/@tony/post/x2vELx"
author:
published:
created: 2026-07-07
description: "從 0 到 1 構建企業級 RAG：一個中小企業可落地版本的完整架構=====================================    如果你翻過市面上關於 RAG 的技術文章，大概率會看到這樣一個公式：```ini 代码解读复制代码RAG = 向量資料庫 + 大模型 AP..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月04日](https://codelove.tw/@tony/post/x2vELx "2026-07-04 12:15:09")

## 從 0 到 1 構建企業級 RAG：一個中小企業可落地版本的完整架構

## 從 0 到 1 構建企業級 RAG：一個中小企業可落地版本的完整架構

```
如果你翻過市面上關於 RAG 的技術文章，大概率會看到這樣一個公式：
```
```
ini 代码解读复制代码RAG = 向量資料庫 + 大模型 API
```

這個公式本身沒有錯——但它描述的是 Demo，不是產品。

當你真的要把 RAG 落到一個企業的知識庫場景裡，你會發現 Demo 裡從來不出現的東西才是真正的工程量：文件怎麼入庫？長文件怎麼切？中文檢索關鍵字怎麼抽？向量召回了 20 條，哪 5 條最該送進上下文？rerank 掛了怎麼辦？使用者怎麼知道這次回答引用了哪些原文？這還只是「能用」層面。到「好用」層面，你還得回答：檢索參數應該是多少？怎麼知道自己調參調對了還是調廢了？

這篇文章是系列的第一篇，不講某個單一技術點，而是把整個專案的骨架攤開——講清楚一條能從 0 走到 1 的企業級 RAG 鏈路到底長什麼樣，每個模組解決什麼問題，以及我是怎麼把它們串起來的。

## 先定邊界：這個專案做什麼，不做什麼

做專案最怕的不是技術難，而是範圍漂移。所以在動第一行程式碼之前，我先劃了三條線：

**做什麼：**

- 完整 RAG 主鏈路：文件上傳 → 解析 → 分塊 → 向量化 → 混合檢索 → 可選重排 → 上下文組裝 → LLM 問答
- 配套管理控制台：知識庫管理、文件生命週期、Chunk 級可觀測、檢索參數調優
- 檢索評測閉環：能跑評測集、能看到每次檢索命中了什麼、能比較不同參數的實際效果

**不做什麼（至少這一版不做）：**

- 多租戶和權限體系——中小企業場景下，一個知識庫通常就一個團隊在用，過早抽象租戶只會增加無謂複雜度
- 複雜的文件前處理流水線——先用 Apache Tika 做通用解析，夠用。什麼表格擷取、圖片 OCR、層級結構保留，這些是第二階段的事
- 自研 Embedding 模型——直接對接 OpenAI 相容介面，你接 DeepSeek、接通義千問、接本地部署的 BGE 模型都行，我不綁你

這個邊界一劃，範圍就清楚了： **一條鏈路，一個控制台，一套評測閉環。** 下面逐層拆開。

## 整體架構：一條 RAG 鏈路穿過 8 個模組

先給一張專案首頁。左側是導覽選單，右側是工作台面板，頂上有個健康檢查的綠燈——證明服務確實在跑。

[![系統首頁](https://i.imgur.com/8OhQmz0.jpeg)](https://i.imgur.com/8OhQmz0.jpeg)

你看到的選單欄正好對應了 RAG 主鏈路上的每個階段。把這條鏈路拉直了看，大概是這樣的：

```
代码解读复制代码使用者上傳文件 → Tika 解析純文字 → 段落優先分塊 → Embedding 向量化
                                              ↓
使用者提問 → 向量召回 + 關鍵字召回 → 可選 Rerank → 鄰居 Chunk 補全
                                              ↓
              組裝上下文 → LLM 生成回答 → 帶引用的最終回應
```

這條鏈路上的每個方塊，在後端都對應一個獨立的 Java Package：

Package職責核心類 `document` 文件上傳、非同步索引、狀態機管理 `DocumentIndexService``parser` 文件格式解析（Tika） `TikaDocumentParserService``chunk` 段落優先切分，支援長段落滑窗 `ParagraphTextChunker``embedding` 呼叫 OpenAI 相容 Embedding API `OpenAiCompatibleEmbeddingService``vector` Qdrant 向量庫讀寫 `QdrantVectorStoreService``retrieval` 混合檢索編排：向量+關鍵字+融合 `RetrievalService``rerank` 精排服務呼叫，掛了自動降級 `HttpRerankService``chat` 問答編排：檢索→上下文→LLM→引用 `ChatService``evaluation` 檢索評測：用例管理+跑分 `RagEvaluationService``knowledgebase` 知識庫及其參數配置 `KnowledgeBaseService``audit` 問答日誌持久化 `RagQaLog` 每個 Package 都是按領域拆分，而不是按層拆—— `document` 包裡既有 Controller 也有 Service 也有 Entity，不是那種「所有 Controller 放一個包、所有 Service 放一個包」的水平分層。這樣做的好處是你改一個功能只需要在一個包裡跳，不會在六七個包之間橫跳。

前端路由跟後端 Package 是一一對應的——不是巧合，是刻意這麼設計的：

```
css 代码解读复制代码// frontend/src/router/index.ts
{ path: "/workspace", component: () => import("@/views/WorkspaceView.vue"), meta: { title: "工作台" } },
{ path: "/knowledge-bases", component: () => import("@/views/KnowledgeBaseView.vue"), meta: { title: "知識庫" } },
{ path: "/documents", component: () => import("@/views/DocumentView.vue"), meta: { title: "文件" } },
{ path: "/chunks", component: () => import("@/views/ChunkInspectorView.vue"), meta: { title: "Chunk" } },
{ path: "/settings", component: () => import("@/views/SettingsView.vue"), meta: { title: "參數配置" } },
{ path: "/chat", component: () => import("@/views/ChatView.vue"), meta: { title: "問答" } },
{ path: "/evaluation", component: () => import("@/views/EvaluationView.vue"), meta: { title: "評測" } },
```

這 7 個頁面對應了 RAG 工作流程中操作者真正關心的 7 件事：知識庫管理、文件入庫、Chunk 可觀測性、參數調優、問答互動、效果評測。不是「因為能做所以做」，而是「操作者在這個環節確實需要看一眼或調一下」。

## 基礎設施：6 個容器 + 1 個 Spring Boot

一個 RAG 系統跑起來，只靠 Spring Boot 是不夠的。這 6 個東西缺一不可：

服務用途為何不是可選的MySQL 8.4文件中繼資料、Chunk、問答日誌、評測用例結構化資料必須有家Qdrant向量儲存與檢索Dense Vector 的最近鄰搜尋，MySQL 做不了MinIO原始文件儲存文件不能塞資料庫，這是常識Embedding 模型文字轉向量外部 API 也行，但本地模型零延遲、零費用Reranker 模型檢索結果精排CPU 跑，速度夠用，精度提升明顯Nginx（Embedding/Reranker 代理）API 驗證模型容器本身沒有認證機制，外面套一層 Nginx 做 Bearer Token 驗證部署側我只貼一個啟動命令就夠了：

```
bash 代码解读复制代码docker compose --env-file .env up -d
```

這個 compose 檔裡做了幾件「產品化該做但 Demo 不會做的事」：

1. **埠號非標** ：MySQL 不用 3306，換成 23306；Qdrant 不用 6333，換成 26333。減少埠衝突和被掃描的機率。
2. **模型容器不直接暴露** ：reranker-model 和 embedding-model 容器本身沒有網路暴露，外面套了一層 Nginx 做 `Authorization: Bearer <token>` 驗證。
3. **資料持久化到 compose file 所在目錄** ：`./data/mysql` 、`./data/qdrant` 、`./data/minio` ——你刪容器、重建容器，資料不丟。
4. **所有敏感資訊走 env** ： `${MINIO_SECRET_KEY}` 、 `${QDRANT_API_KEY}` 、 `${RERANK_API_KEY}` ——env 檔進 `.gitignore` 。

## 主鏈路拆解：從文件上傳到問答返回

下面按順序走一遍主鏈路。不貼大段程式碼，只在關鍵決策點上展開。

### 1\. 文件入庫：5 個狀態的非同步流水線

文件上傳後不是同步處理——大文件解析可能幾十秒，讓 HTTP 請求等著不現實。所以上傳動作只做兩件事：文件落到 MinIO，資料庫裡建一條文件記錄（狀態 = `UPLOADED` ），然後丟給執行緒池非同步處理。

```
scss 代码解读复制代码// DocumentIndexService.java —— 核心入口
public DocumentResponse uploadAndIndex(DocumentUploadRequest request) {
    StoredFile storedFile = fileStorageService.upload(file);
    RagDocument document = createDocument(file, storedFile, ...);
    submitIndexingTask(document.getId());  // 非同步
    return DocumentResponse.from(document);
}
```

非同步流水線有 5 個狀態節點：

```
scss 代码解读复制代码UPLOADED → PARSING → CHUNKING → INDEXING → INDEXED
   ↓ 任意節點失敗
FAILED (記錄 failureReason)
```

這裡做了一個小但重要的設計：每個狀態切換都即時寫庫。好處是前端輪詢文件狀態時能看到「進行到哪一步了」，壞處是多了一次 UPDATE。在這個吞吐量級別下，多一次 UPDATE 完全不值得省。

Tika 解析階段用的是 `tika-parsers-standard-package` ，能處理 PDF、Word、PPT、HTML、純文字等常見格式。解析完成後得到一個純文字字串，進入分塊。

### 2\. 分塊：段落優先，不是無腦切

分塊策略決定了檢索品質的上限。這個專案用的是段落優先切分：

```
scss 代码解读复制代码// ParagraphTextChunker.java —— 核心邏輯
for (String paragraph : normalizedText.split("\n\s*\n")) {
    if (paragraph.length() > maxChars) {
        splitLongParagraph(chunks, paragraph, maxChars, overlapChars);  // 長段落滑窗切
    } else if (current.length() + paragraph.length() + 2 > maxChars) {
        flushCurrent(chunks, current);  // 當前塊滿了，先歸檔
        current.append(paragraph);      // 新段落開新塊
    } else {
        current.append(paragraph);      // 段落加到當前塊裡
    }
}
```

邏輯很直白：以段落為最小單位拼塊，拼不下就開新塊。只有單一段落超過 maxChars（預設 1000 字元）時才做滑窗切分，視窗重疊 150 字元。

為什麼這麼做？因為知識庫文件的段落天然是有語義邊界的。你把一個段落的最後兩句和下一個段落的前兩句硬拼到一起，生成的向量既不「像」上一段也不「像」下一段，檢索時兩頭都不討好。

### 3\. Embedding：對接 OpenAI 相容協議，模型隨便換

Embedding 層沒有任何自研邏輯，就是一個 HTTP 呼叫。介面相容 OpenAI 的 `/v1/embeddings` 格式。你填 `MODEL_EMBEDDING_BASE_URL` 接 DeepSeek 的 API 也行，填 `http://embedding:80` 接本地 BGE 模型也行。

```
ruby 代码解读复制代码rag:
  model:
    embedding:
      base-url: ${MODEL_EMBEDDING_BASE_URL:}       # 支援任意相容服務
      model: ${MODEL_EMBEDDING_MODEL:text-embedding-3-large}
```

向量維度跟模型走：用 BGE base 就是 768 維，用 text-embedding-3-large 就是 3072 維。Qdrant 的 collection 建立時維度必須匹配，所以換模型 = 換 collection = 重建索引。這個約束是向量檢索的本質決定的，跟架構無關。

### 4\. 混合檢索：向量 + 關鍵字 + 融合排序 + 可選 Rerank

這是整個專案最值得展開的部分。純向量檢索有一個致命問題：對專有名詞、縮寫、編號不敏感。比如你搜「HR-2024-003」，向量空間裡這條 chunk 跟你的 query 向量的餘弦相似度可能並不高，因為 Embedding 模型不認識你公司內部的文件編號規則。

所以檢索做了兩條腿：

**左腿——向量召回：** 用 question 轉 query vector → Qdrant 搜 topK（預設 20）。這是語意層面的召回，涵蓋面廣。

**右腿——關鍵字召回：** 從 question 裡抽關鍵字 → MySQL 全文索引搜尋 → 全文索引沒命中就降級到 LIKE。這是字面層面的補召回，專門抓專有名詞。

關鍵字抽取做了中文適配：

```
erlang 代码解读复制代码// 對包含中文的 token 做 4 字視窗切分
// "核心存儲有哪些" → "核心存儲"、"心存儲有"、"存儲有哪些" ...
if (containsCjk(token) && token.length() > CJK_NGRAM_LENGTH) {
    for (int i = 0; i <= token.length() - CJK_NGRAM_LENGTH; i++) {
        keywords.add(token.substring(i, i + CJK_NGRAM_LENGTH));
    }
}
```

這個做法的動機是：中文不像英文有天然的空格分詞邊界，使用者輸入「核心存儲有哪些功能」，如果不對它做 n-gram，全文索引可能一個字都匹配不上。

兩路結果合併後，每路有自己的分數：向量結果帶 cosine 分數，關鍵字結果給一個固定弱分數（0.2）。融合排序按總分降序取前 K。

如果開啟了 Rerank（ `RAG_RERANK_ENABLED=true` ），融合後的候選集會再送一遍給 Reranker 做精排。Reranker 是一個 Cross-Encoder，把 question 和每個候選 chunk 拼在一起打分，精度顯著高於向量相似度。但它有一個風險——網路掛了怎麼辦？

```
kotlin 代码解读复制代码if (Boolean.TRUE.equals(options.rerankEnabled()) && rerankService.available()) {
    try {
        return rerank(question, candidates, options);      // 精排
    } catch (RuntimeException ex) {
        log.warn("Rerank failed, fallback to fused retrieval score");  // 自動降級到融合分
    }
}
return sortByFusedScore(candidates, options);  // 降級路徑
```

關鍵設計： **rerank 是增強路徑，不是主鏈路。** 它掛了，系統回退到融合排序繼續工作，不影響問答可用性。

### 5\. 鄰居 Chunk 補全與上下文組裝

一段知識很可能跨了兩個 chunk。比如「API 金鑰取得方式如下：」在第 5 個 chunk，實際取得步驟在第 6 個 chunk。如果只送第 5 個進上下文，LLM 只能編一個答案。

所以檢索結果出來之後，對每個命中的 chunk 向前後各擴展 N 個鄰居（預設前後各 1 個）：

```
ini 代码解读复制代码int startIndex = Math.max(0, chunk.getChunkIndex() - options.neighborBefore());
int endIndex = chunk.getChunkIndex() + options.neighborAfter();
List<RagChunk> neighborChunks = chunkRepository
    .findByDocumentIdAndChunkIndexBetweenOrderByChunkIndexAsc(
        chunk.getDocument().getId(), startIndex, endIndex);
```

補全後的 chunk 列表再按 context-max-chars（預設 8000）截斷——先到先得，超出就丟棄。這是為了避免上下文超出 LLM 視窗，同時確保最相關的 chunk 一定在前面。

上下文組裝格式是這樣的：

```
代码解读复制代码引用 1，文件：《員工手冊 v3》，片段：5
（第 5 個 chunk 的完整內容）

引用 2，文件：《員工手冊 v3》，片段：6
（第 6 個 chunk 的完整內容）
```

LLM 拿到的 prompt 裡明確告訴它「引用來源怎麼標註」，回答裡就能帶引用編號。

### 6\. 問答編排：兩種模式 + 審計日誌

ChatService 提供三種呼叫方式：

- **同步問答** （ `POST /api/chat` ）：檢索 → LLM 生成 → 一次性返回 answer + citations
- **流式問答** （ `POST /api/chat/stream` ）：SSE 推送 token，Java 21 虛擬執行緒跑，交易用 TransactionTemplate 手動管理避免 SSE 長交易
- **僅檢索** （ `POST /api/chat/retrieve` ）：只跑檢索不調 LLM，返回命中了哪些 chunk

每次問答都會記一條審計日誌（ `RagQaLog` ）：question、answer、retrievedChunkIds、modelName、latencyMs。不記 IP，不記 user——中小企業的第一版不需要這些東西。

## 配置設計：全域預設 + 知識庫覆蓋 + 請求級覆蓋

檢索參數不是寫死的。每個知識庫可以有自己的預設參數，每次請求還可以臨時覆蓋。優先級是：

```
代码解读复制代码請求參數 > 知識庫配置 > application.yml 全域預設
```
```
yaml 代码解读复制代码# application.yml —— 全域預設
rag:
  retrieval:
    final-top-k: 5
    vector-top-k: 20
    keyword-top-k: 20
    rerank-enabled: false        # 預設關閉，配了 rerank 服務再開
    neighbor-enabled: true
    neighbor-before: 1
    neighbor-after: 1
    context-max-chars: 8000
```

這個設計讓「調參」這件事從「改程式碼→重新部署」變成了「在前端 Settings 頁面調滑桿→點保存→在 Chat 頁面直接試效果」。評測閉環也因此成為可能——你可以在不同參數組合下跑同一套評測集，用數據說話，而不是憑感覺。

## 前端：不是單純「套個殼」

前端用 Vue 3 + Naive UI + Pinia，Vite 建構完成後直接打到 `src/main/resources/static/` 底下，Spring Boot 一個 JAR 就能同時服務前端靜態資源和後端 API。不用額外部署 Nginx 託管前端。

幾個值得提的點：

1. **ConsoleLayout 是單頁面佈局，不是多頁面跳轉** 。左側選單切換只是 router-view 的內容在變，知識庫選擇和健康狀態始終可見。
2. **ChunkInspector 頁面能按文件查看所有 chunk 的內容和索引位置** ——索引階段出了什麼問題，不是靠猜，是直接看。
3. **Settings 頁面把所有檢索參數做成了視覺化控制項** （滑桿、開關、下拉框），改完參數立刻生效，不需要重啟。

## 當前狀態和下一步

主鏈路已經全部跑通：

- ✅ 文件上傳 → 解析 → 分塊 → 嵌入 → 入庫
- ✅ 混合檢索（向量 + 關鍵字）
- ✅ Rerank 精排（可選，降級可用）
- ✅ 鄰居 Chunk 上下文補全
- ✅ 同步 + 流式 LLM 問答
- ✅ 檢索評測框架
- ✅ 知識庫級參數配置

問答頁面和評測頁面還需要產品級重做——目前的功能可以跑，但在互動體驗和資料視覺化上還只是「能用」的水平。評測頁面需要加更多的視覺化呈現（比如 nDCG 曲線、參數比較視圖），問答頁面需要更好的引用展示和追問體驗。

系列後續 11 篇文章會逐個展開：部署、文件入庫、分塊策略、Embedding 技術選型、向量庫實戰、混合檢索細節、Rerank 部署與調優、上下文組裝技巧、問答產品設計、評測體系搭建，以及最終的工程復盤。

## 寫在最後

這篇文章想傳達的核心觀點其實就一個： **企業級 RAG 的工程複雜度不在 LLM，在檢索。** 大模型 API 是 RAG 鏈路的最後一公里，但你得先讓前九十九公里跑通了，這一公里才有意義。前九十九公里包括：文件怎麼進來、怎麼被切、怎麼被檢索、檢索結果怎麼被精排和補全、整個鏈路怎麼被觀測和優化。

這個專案是為中小企業在「第一天就用正確架構」而準備的——不是 Demo，而是一條從 0 到 1 的工程基線。你可以在這條基線上換模型、換向量庫、換前端框架，但鏈路骨架和模組拆分邏輯是可以複用的。

---

原文出處： [https://juejin.cn/post/7657791057036525614](https://juejin.cn/post/7657791057036525614)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x2vELx) [26瀏覽](https://codelove.tw/@tony/post/x2vELx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x2vELx)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385995&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx2vELx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385995158&amp;bpp=1&amp;bdt=91&amp;idt=92&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1940897230525&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099344%2C95395662%2C95393281%2C95393485&amp;oid=2&amp;pvsid=1869573343342123&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=160" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>