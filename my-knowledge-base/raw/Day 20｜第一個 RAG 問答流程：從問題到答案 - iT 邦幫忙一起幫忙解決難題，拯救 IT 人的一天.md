---
title: "Day 20｜第一個 RAG 問答流程：從問題到答案 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405040"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "前兩天把精排結果整理成 Context，又把問題、來源與回答規則組成 Messages。今天終於可以按下 Enter，讓一個問題真的走完檢索、精排、上下文建構與 LLM 生成，產生本系列第一個 RAG..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 20

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 20 篇

## Day 20｜第一個 RAG 問答流程：從問題到答案

- 分享至

前兩天把精排結果整理成 Context，又把問題、來源與回答規則組成 Messages。今天終於可以按下 Enter，讓一個問題真的走完檢索、精排、上下文建構與 LLM 生成，產生本系列第一個 RAG 回答。

不過，在把函式依序接起來之前，還有兩個會影響後續維護的決定：整條 Pipeline 不能綁死單一 LLM 供應商；離線測試也不能每次都啟動 9B 模型。先處理這兩件事，第一次回答才不會同時變成第一個技術債。

## 先讓 Pipeline 不綁死單一模型

```python
class LlmClient(Protocol):
    def generate(self, messages: list[dict[str, str]]) -> str: ...
```

只要能接收 Messages 並回傳文字，就符合 Pipeline 的需求。實作提供三種 Client：本機 `OllamaClient` 、一般 Chat Completions 形式的 `OpenAICompatibleClient` ，以及離線測試用的 `StaticLlmClient` 。後者只回傳預先準備的字串，不代表模型品質，但能測試解析、引用與拒答流程，而不需要網路或 GPU。

本機版本使用 Ollama 的 `/api/chat` ：

```python
@dataclass(frozen=True)
class OllamaClient:
    model: str = "qwen3.5:9b"
    base_url: str = "http://127.0.0.1:11434"

    def generate(self, messages):
        response = _post_json(
            f"{self.base_url}/api/chat",
            {
                "model": self.model,
                "messages": messages,
                "stream": False,
                "think": False,
                "format": "json",
                "options": {"temperature": 0},
            },
            {"Content-Type": "application/json"},
            timeout=120,
        )
        return response["message"]["content"]
```

URL、模型與 API Key 都由環境變數提供，不寫死在文章或版本庫。HTTP 錯誤、逾時與非 JSON 回應則統一轉成 `LlmClientError` ，讓 API 層以後能區分「知識庫沒答案」與「模型服務故障」。

## 串起 Pipeline

`RagPipeline.from_project()` 只在初始化時載入文件、建立 Chunk、開啟 Qdrant、載入 Embedding 與 Reranker。查詢時不能每次重載模型，否則大部分延遲都浪費在啟動。

```python
def ask(self, question: str, max_sources: int = 3) -> RawRagAnswer:
    results = search_reranked.search(
        question,
        self.chunks,
        self.retriever_index,
        top_k=max(5, max_sources),
    )
    context = build_context(
        question,
        results,
        self.documents_by_id,
        max_sources=max_sources,
    )
    model_output = self.llm_client.generate(
        build_messages(question, context)
    )
    return RawRagAnswer(question, model_output, context)
```

為什麼檢索取至少 5 個，Context 卻預設只放 3 個？前者是精排候選池，後者是生成預算，兩種 `K` 服務不同目的。候選池要讓 Reranker 有選擇空間，真正交給模型的來源則要節制。

## 第一次實際呼叫

本機已有 `qwen3.5:9b` ，執行方式如下：

```bash
RAG_LLM_MODE=ollama \
RAG_MODEL=qwen3.5:9b \
HF_HUB_OFFLINE=1 \
python scripts/ask.py "HTTP 401 和 403 有什麼差別？"
```

模型回傳的原始 JSON 內容合理：它說 401 代表身分尚未確認，403 代表已確認身分但權限不足。它在回答文字中使用了 `S1` 與 `S2` ：

```json
{
  "status": "answered",
  "answer": "HTTP 401 表示身分尚未被確認 [S1]；未通過安全過濾器的驗證也會回傳 401 [S2]。HTTP 403 則表示身分已確認但權限不足 [S1]。",
  "citations": ["S1"]
}
```

如果只用肉眼閱讀，這是一個不錯的答案；但請仔細看最後一行：回答正文用了 `S2` ， `citations` 欄位卻只列 `S1` 。Day 19 的契約沒有被完整遵守。這正好證明 Prompt 不是驗證器，也替明天留下最真實的案例。

## 先保留原始結果，不急著美化

第一版 Pipeline 先回傳 `RawRagAnswer` ，保留原始模型輸出與實際 Context，不急著把錯誤吞掉。這使 Debug 時能回答三個問題：檢索取回了什麼？模型實際看到了什麼？模型原始輸出是什麼？若只回傳最後一段文字，這三層會被壓成一個黑盒子。

Pipeline 也明確關閉 Qdrant Client。Day 17 的評測程式在程序結束時曾出現 Qdrant 解構警告，新的 `close()` 把資源生命週期放回應用程式控制；Day 29 進入 FastAPI 後，會由 Application Lifespan 統一建立與釋放。

然而，保留原始結果也提醒我們：今天完成的是「可以跑通的 RAG」，還不是「可以直接交付的 RAG」。它仍缺三道關卡：JSON 能否解析、引用是否合法、資料是否足以回答。這三件事不能交給模型自己宣布成功。

同樣地，換成更大的模型不會自動解決資料流問題。即使某個模型每次都能輸出漂亮 JSON，來源映射、拒答、評測與服務錯誤仍需要應用程式負責。模型是 Pipeline 的一個元件，不是整個系統。

## 把黑盒子拆成七個可觀察階段

第一個可執行版本很容易只用 `print()` 顯示答案，但可維護的 Pipeline 應替每一層保存可追蹤資料。一次請求至少可拆成：

```markdown
question
→ retrieval_query
→ candidate_results
→ reranked_results
→ context_bundle
→ messages
→ raw_model_output
→ parsed / validated answer
```

Day 20 還沒有最後兩層，但 `RawRagAnswer` 已保存 Context 與原始輸出。這使我們能在明天重播同一個模型結果測 Citation Validator，不必每調一行解析器就重新跑一次 9B 模型。對昂貴或外部 API 而言，這個分離也能節省成本。

保存資料時要注意可序列化。 `QdrantClient` 、模型物件與索引不能直接寫入 JSON；Trace 應保存 ID、分數、文字摘要、Prompt 版本與時間，而不是把整個 Python 物件 Dump 下來。若要重播，還需要知識庫與索引的版本識別，否則同一個 Chunk ID 內容已經變更，重播結果會失真。

## 模型服務故障，不等於知識庫沒答案

模型呼叫可能遇到連線拒絕、逾時、服務回 500、JSON 損壞或回應欄位變更。這些都屬於技術故障，不能轉成「目前知識庫沒有答案」。若把所有 Exception 都吞掉後回覆 `insufficient` ，使用者會誤以為資料不存在，維運人員也看不到模型服務正在故障。

因此 Client 統一丟出 `LlmClientError` ，上層 API 日後可以回 502 或 503，並保存 Request ID；只有可回答性規則或模型合法輸出的 `insufficient` 才是正常拒答。錯誤分類是服務契約的一部分：同一句友善文字不能同時代表知識不足、格式錯誤與網路逾時。

Timeout 也要分層。連線 Timeout、讀取 Timeout 與整體請求 Deadline 的意義不同；如果 Reranker 已花 800 毫秒，LLM 仍拿到完整 120 秒，API 總延遲可能超過使用者容忍。正式服務應由最外層建立 Deadline，再把剩餘時間分配給檢索與生成。

## 如何在沒有模型時測完整 Pipeline？

`StaticLlmClient` 接收一段固定回應：

```python
client = StaticLlmClient(
    '{"status":"answered",'
    '"answer":"401 代表身分未確認。[S1]",'
    '"citations":["S1"]}'
)
```

使用它可以驗證 Retrieval 之後的 Messages、JSON 解析、引用映射與 API 回應，不需要啟動 Ollama。另一組替身則可以刻意回傳壞 JSON、 `S999` 或逾時 Exception，確認系統採 Fail Closed。這不是用假的模型評估回答品質，而是把確定性的應用邏輯從非確定性的生成模型中隔離出來。

真實模型測試仍然必要，因為只有它能揭露「正文用了 S2、欄位漏列 S2」這種遵循問題。兩種測試不是互相取代：替身保證程式邏輯，真實模型衡量行為分布。

## 延遲與成本從今天開始累積

檢索階段已有 Embedding 與 Reranker，生成又加入 9B 模型。一次查詢的總時間不是只有 LLM 推論，還包含斷詞、BM25、Query Embedding、Qdrant、Cross-encoder、Prompt 序列化與 HTTP。後面做 API 時，應分別記錄每層耗時，而不是只看端到端的一個數字。

來源數也會影響生成成本。Context 從 1,000 字增加到 3,000 字，不一定讓回答變三倍好，卻一定增加 Prompt Token 與 Prefill 時間。Day 27 的 Top-K Ablation 會回到這筆帳；今天先確立原則：每增加一份來源，都要能說明它補了什麼證據。

## 結語

今天第一次讓問題完整走到模型輸出，並用本機 `qwen3.5:9b` 實際跑通。抽象的 `LlmClient` 讓 Ollama、相容 API 與離線替身共用同一條 Pipeline； `RawRagAnswer` 則保留問題、Context 與模型原始輸出，讓每一層都可追查。

第一個答案沒有帶來完美收場，反而送來一個更有價值的錯誤：正文引用了 `S1` 、 `S2` ，宣告欄位卻只有 `S1` 。下一篇就從這道裂縫出發，把來源從模型自由書寫的文字，變成程式可以驗證與映射的資料。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10404701)

[

Day 19｜Prompt 設計：要求 LLM 只根據資料回答

](https://ithelp.ithome.com.tw/articles/10404701)[下一篇](https://ithelp.ithome.com.tw/articles/10405135)

[

Day 21｜讓答案附上來源：RAG 引用機制設計

](https://ithelp.ithome.com.tw/articles/10405135)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/26/a061265c-33c4-4874-b48c-2155e6213840_small.jpg)

[邊開火邊修正 - 最小產品可以如何嘗試？](https://itplus.ithome.com.tw/vod-page/433?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Agile Summit 敏捷高峰會 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/ad0edf99-f7e1-45a5-8e67-6096057b7b54_small.png)

[用早稻田MBA的企業經營學經營軟體工程團隊、創造商業價值](https://itplus.ithome.com.tw/vod-page/974?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/5/26/0776548a-ce34-46c2-9b92-c9e37f2c0d34.jpg)

[探索 Agentic AI 的前沿趨勢與潛在挑戰](https://itplus.ithome.com.tw/webinar-page/251?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/b3e60988-d724-4187-93fc-679e6eb3869b_small.png)

[GSI 協議：提升 AI 驅動 SDD 的效率與敏捷性](https://itplus.ithome.com.tw/vod-page/1482?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

26 分