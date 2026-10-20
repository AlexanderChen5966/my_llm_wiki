---
title: "Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407057"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "前 28 天的程式都由命令列啟動。這對實驗很方便，卻要求每個呼叫者都懂 Python 環境、模型參數與專案路徑；其他系統也無法只送一個問題，就穩定拿回答案。 今天用 FastAPI 把搜尋、問答與文件..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 29

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 29 篇

## Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務

- 分享至

前 28 天的程式都由命令列啟動。這對實驗很方便，卻要求每個呼叫者都懂 Python 環境、模型參數與專案路徑；其他系統也無法只送一個問題，就穩定拿回答案。

今天用 FastAPI 把搜尋、問答與文件匯入包成 HTTP API。畫面暫時不重要，真正要固定的是輸入與輸出契約、模型生命週期、錯誤語意，以及未審核文件不能立即參與回答的安全邊界。

## 四個端點，對應三類能力

第一版提供健康檢查、查詢與文件匯入三類能力，共四個端點：

```markdown
GET  /health        服務健康檢查
POST /v1/search     回傳精排後 Chunk
POST /v1/answers    回傳驗證後回答與來源
POST /v1/documents  將新文件放入 Staging，尚不索引
```

搜尋與回答分開很重要。Debug 時可以先確認 `/v1/search` 找到什麼，再檢查生成；只提供聊天端點會讓所有問題重新混成黑盒子。文件匯入則刻意不立即進索引，延續 Day 28 的審核邊界。

## 先用 Schema 把輸入邊界寫清楚

```python
class SearchRequest(BaseModel):
    question: str = Field(min_length=2, max_length=500)
    top_k: int = Field(default=3, ge=1, le=10)

class AnswerRequest(BaseModel):
    question: str = Field(min_length=2, max_length=500)
    max_sources: int = Field(default=3, ge=1, le=5)
```

問題長度、Top-K 與來源數都有上限，避免單次請求無限制消耗模型與 Context。回應不直接暴露本機絕對路徑，只回傳 Marker、標題與 Chunk ID。

```python
class AnswerResponse(BaseModel):
    status: str
    answer: str
    sources: list[SourceResponse]
    decision_reason: str | None = None
```

`status=insufficient` 是正常業務結果，不應回 HTTP 500；模型服務故障、索引損壞才是系統錯誤。API 設計必須區分「不知道答案」和「服務壞了」。

## 模型只在服務啟動時載入

Embedding 與 Reranker 載入成本高，不能每個 Request 重建。FastAPI Lifespan 在啟動時建立 `RagPipeline` ，關閉時釋放 Qdrant Client：

```python
@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.service = KnowledgeService(
        RagPipeline.from_project(
            client_from_env(),
            enable_rewrite=True,
        )
    )
    yield
    app.state.service.close()
```

這也替測試留下替換點。API 契約測試使用 `FakeService` ，不載入 9B LLM 與 Reranker，就能驗證路由、Schema、狀態碼與 Staging 行為。模型品質由 Day 25、26 的評測負責，HTTP 契約不應每次都付完整推論成本。

## 文件匯入先進 Staging

文件 ID 只允許小寫英數與連字號，正文限制 20 到 50,000 字。接受後以暫存檔加 `os.replace()` 原子寫入，Metadata 標記 `status: draft` ：

```python
class DocumentRequest(BaseModel):
    id: str = Field(pattern=r"^[a-z0-9][a-z0-9-]{2,63}$")
    title: str = Field(min_length=2, max_length=120)
    topic: str = Field(min_length=2, max_length=60)
    content: str = Field(min_length=20, max_length=50_000)
```

API 回覆 HTTP 202：

```json
{
  "id": "new-document",
  "status": "accepted",
  "indexed": false,
  "message": "文件已進入 staging；完成內容審核並重建索引後才會參與回答。"
}
```

`indexed=false` 不是未完成的藉口，而是誠實的非同步契約。將新文件安全地寫入原始知識庫、審核、切分、Embedding、Qdrant Upsert、BM25 重建與版本切換，是一條獨立的 Ingestion Pipeline；把這些工作全塞進一個同步 Request，會造成逾時與半更新狀態。

## 啟動與呼叫

安裝並啟動：

```bash
python -m pip install -r requirements.txt
RAG_LLM_MODE=ollama RAG_MODEL=qwen3.5:9b \
HF_HUB_OFFLINE=1 \
uvicorn scripts.api:app --host 127.0.0.1 --port 8000
```

詢問：

```bash
curl -X POST http://127.0.0.1:8000/v1/answers \
  -H 'Content-Type: application/json' \
  -d '{"question":"HTTP 401 和 403 有什麼差別？","max_sources":3}'
```

本機開發預設只綁 `127.0.0.1` 。目前沒有認證、授權、Rate Limit、TLS 與 Audit Log，不應直接改成 `0.0.0.0` 暴露到公開網路。API 能啟動不等於已符合生產安全。

## 契約測試

`tests/test_api.py` 使用 FastAPI Test Client 與暫存 Staging 目錄，檢查：健康端點、搜尋 200、回答來源 Marker，以及文件匯入回 202 且 `indexed=false` 。

```bash
python -m unittest discover -s tests -v
```

Day 29 的 API 契約測試實測通過 1 項；Day 30 加入核心規則後會一起跑 6 項。測試過程出現 Test Client 相依套件的棄用警告，但沒有測試失敗；這類警告要列入相依升級待辦，不能當成無事發生。

## 從「能呼叫」到「能部署」，還有四道工程題

到這裡，Pipeline 已經可以被其他程式呼叫，但離正式部署還有四道工程題：錯誤能否被呼叫者正確理解、推論資源能否承受併發、文件匯入能否安全完成，以及服務健康是否真的代表可用。若知識庫含敏感資料，還要延續 Day 28 的原則，在 Retrieval 前執行文件 ACL 過濾。

下面逐一展開這四題。今天的範圍是把可測 Pipeline 變成邊界清楚的服務，不用一個漂亮的 `/docs` 畫面假裝平台工程已經完成。

### 狀態碼要反映失敗層級

API 呼叫者需要知道是否值得重試。輸入不符合 Schema 回 422；文件已接受但仍待審核回 202；知識庫證據不足仍可回 200，並以 `status=insufficient` 表示正常完成。若上游模型逾時可回 503 或 504，索引尚未就緒則由 Readiness Check 阻止流量，不應偽裝成「沒有答案」。

內部例外不能原樣回傳給使用者，否則可能曝光路徑、套件版本或 Prompt。服務端以 Trace ID 對應詳細 Log，外部只收到穩定的錯誤碼與可理解訊息。是否重試也要有上限與退避；生成請求若其實已在後端執行，客戶端無限重送會放大成本與負載。

### 同步問答也需要併發控制

FastAPI 的路由可以是非同步，不代表 CPU/GPU 推論會自動平行。Embedding、Reranker 與本機 LLM 可能占用大量計算，過多同時請求會讓所有請求一起逾時。第一版可用 Semaphore 限制生成併發，設定佇列長度與 Request Timeout；超過容量時明確拒絕，比讓程序耗盡記憶體更安全。

若某些模型函式是同步阻塞，也不能直接卡住 Event Loop。可以放入受控的 Worker Thread、Process 或獨立推論服務，但要量測實際資源行為。服務的容量規劃應記錄 P50、P95、P99 延遲、佇列等待、模型推論時間與拒絕數，而不只是在單人本機測試中「看起來很快」。

### 文件匯入應該是可追蹤工作

`POST /v1/documents` 回 202 後，正式系統應另外回傳 `job_id` ，讓呼叫者查詢 `pending` 、 `reviewing` 、 `indexed` 或 `failed` 。背景工作依序做惡意檔案掃描、格式解析、人工或規則審核、Chunk、Embedding、索引建置與驗證；任一步失敗都保留原因，不留下半套可搜尋狀態。

索引更新可使用版本化 Collection：先在新版本完整建置並跑 Smoke Test，通過後原子切換讀取別名；失敗則繼續使用舊版。這比直接修改線上索引安全，也能在品質退步時快速回滾。BM25 與向量索引必須屬於同一知識庫版本，避免 Hybrid Search 把兩個時間點的資料混在一起。

### 存活、就緒與回答品質是三件事

`/health` 回 200 只能代表 Web Process 還活著。就緒檢查（Readiness）還要確認索引可讀、必要模型已載入、Staging 目錄可用，以及服務是否有足夠容量接新流量；任何一項未完成，就不應讓負載平衡器送入請求。

但 Readiness 通過仍不代表回答品質合格。Day 25、26 的離線評測應在新模型、Prompt 或索引版本部署前執行，通過門檻才發布。線上再監看拒答率、引用驗證失敗、延遲與錯誤分布。工程健康和模型品質是兩條監控軸，缺一不可。

## 版本化與相容性

路徑中的 `/v1` 代表 API 契約版本，不等於模型版本。Prompt、Embedding、Reranker、知識庫與程式都應另外記錄內部版本，並可透過受控的 Response Metadata 或 Trace 查詢。只要外部欄位語意不變，就能在 v1 內更新模型；若刪除欄位或改變狀態定義，才需要新 API 版本與遷移期。

來源物件未來可能新增 URL、片段摘要與文件版本，新增可選欄位通常較容易相容；把 `sources` 從陣列改成字串則會破壞客戶端。契約測試不只測今天的服務，也可保存一份 OpenAPI Snapshot，避免重構時無意改壞介面。

## 從本機到部署前的檢查清單

部署前至少確認：

- 服務以非 Root 身分執行，秘密由安全設定注入，不寫進 Repository。
- 外部連線使用 TLS，CORS 只允許必要來源。
- Request Body 與上傳大小都有上限。
- Log 不保存不必要的敏感原文。
- API 已加入認證、授權與限流。
- 備份、索引切換與回滾都實際演練過。

同時要做 Graceful Shutdown：停止接收新請求、等待有上限的在途推論、關閉 Qdrant 與背景 Worker。若部署更新時直接殺掉程序，使用者可能得到半個回應，Ingestion 也可能停在不明狀態。服務化的完成標準，是失敗與重啟同樣被設計，而不只是成功路徑能被 `curl` 呼叫。

## 結語

今天完成 FastAPI 服務層：搜尋與回答分開、輸入範圍固定、Pipeline 由 Lifespan 管理、來源不曝光內部路徑，新文件以 HTTP 202 進入 Staging，不會立即污染索引。契約測試確認四個端點的基本行為，模型與搜尋品質則繼續由獨立評測負責。

下一篇是第 30 天。最後一篇不再加入新框架，而是把 30 天的資料流、評測結果、Agentic 決策、未完成限制與下一步放在同一張圖上。真正的結束不是宣稱系統完美，而是能清楚說明它現在做得到什麼、證據在哪裡，以及哪一些風險仍然存在。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10406994)

[

Day 28｜RAG 安全性：Prompt Injection 與惡意文件

](https://ithelp.ithome.com.tw/articles/10406994)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 29 篇

目錄

1. 25
	[Day 25｜評估搜尋品質：Recall@K、MRR 與命中率](https://ithelp.ithome.com.tw/articles/10406131)
2. 26
	[Day 26｜評估回答品質：正確性、引用與幻覺分析](https://ithelp.ithome.com.tw/articles/10406229)
3. 27
	[Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響](https://ithelp.ithome.com.tw/articles/10406516)
4. 28
	[Day 28｜RAG 安全性：Prompt Injection 與惡意文件](https://ithelp.ithome.com.tw/articles/10406994)
5. 29
	[Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務](https://ithelp.ithome.com.tw/articles/10407057)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/4/30/f49229f9-d26f-4f63-935f-130e4de630c6_small.jpg)

[AI 時代資料處理殺手鐧：Microsoft Fabric + Power BI Copilot 讓你用說的畫圖表](https://itplus.ithome.com.tw/vod-page/824?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/37b66aa1-ff69-4b7c-9ea1-9a23aeddaf8d_small.jpg)

[國泰 2024 SRE Shift-Right 的發展策略](https://itplus.ithome.com.tw/vod-page/1029?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

SRE CONFERENCE |

35 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/10/31/0c286c67-9d3e-4338-89cf-1637ba816e60_small.png)

[健保數位轉型新篇章：雲端架構、資訊治理與醫療服務革新](https://itplus.ithome.com.tw/vod-page/1265?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣醫院資訊主管會議 |

39 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/5/4/b7bc8d02-d14e-4cd7-8c9e-5c8c23df32e0_small.png)

[Agentic SRE 的崛起：以 kagent 與 A2A 協議建構下一代觀察性平台](https://itplus.ithome.com.tw/vod-page/1428?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Observability Day 可觀察性技術日 |

38 分