---
title: "Day 30｜完整回顧：從 NLP 到 Agentic RAG 的成果與下一步 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10407473"
author:
  - "[[iThome]]"
published:
created: 2026-09-04
description: "三十天前，我們從一個看似單純的問題出發：如何讓 LLM 根據指定資料回答，並在資料不足時知道自己該停下來？一路做到今天，答案已經很清楚——關鍵從來不是挑一套向量資料庫，再寫一段「很強的 Prompt」..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 30

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 30 篇

## Day 30｜完整回顧：從 NLP 到 Agentic RAG 的成果與下一步

- 分享至

三十天前，我們從一個看似單純的問題出發：如何讓 LLM 根據指定資料回答，並在資料不足時知道自己該停下來？一路做到今天，答案已經很清楚——關鍵從來不是挑一套向量資料庫，再寫一段「很強的 Prompt」，而是建立一條能被拆解、測試、拒絕與追溯的資料流。

最後一天不再加入新框架。這一篇會把成果、數據、失敗案例與尚未完成的邊界放在一起，看看這 30 天究竟完成了什麼，也確認每一個「做得到」後面都有證據。

## 完整系統

```markdown
flowchart LR
    A[繁體中文 Markdown] --> B[清理與結構化 Chunk]
    B --> C[BM25]
    B --> D[Embedding + Qdrant]
    C --> E[RRF Hybrid Recall]
    D --> E
    E --> F[Cross-encoder Reranker]
    Q[使用者問題] --> R[問題改寫或 Follow-up 解析]
    R --> C
    R --> D
    F --> G{證據足夠嗎}
    G -- 否 --> X[拒答]
    G -- 是 --> H[Context Builder]
    H --> I[Grounded Prompt]
    I --> J[LLM]
    J --> K{格式與引用有效嗎}
    K -- 否 --> X
    K -- 是 --> L[回答與可追溯來源]
    L --> M[回答評測與 Failure Trace]
```

Day 1 到 Day 4 定義問題、模型限制與 Markdown 資料契約；Day 5 到 Day 10 完成清理、Chunk、斷詞、TF-IDF、BM25 與第一份評測；Day 11 到 Day 17 建立 Embedding、Qdrant、Semantic Search、Hybrid 與 Reranker；Day 18 到 Day 24 才正式進入 LLM、引用、拒答、改寫與多輪；最後六天用檢索評測、回答評測、失敗分層、安全與 API 將作品收束。

每一層都能回答「為什麼存在」。BM25 保留識別字與錯誤碼的精準訊號，Embedding 處理同義與跨語言，RRF 擴大候選覆蓋，Cross-encoder 讀取查詢與 Chunk 的配對，Context Builder 控制來源與預算，Validator 則阻止沒有合法引用的回答流出。

## 檢索成果

最初的詞集合基準在 v2 評測集上 Hit@1 只有 9/16（0.56）；Day 17 的完整精排變成 15/16（0.94），而唯一失敗的 q18 同時是標註爭議案例。Day 25 的正式指標如下：

| 方法 | Hit@1 | Hit@3 | Recall@3 | MRR |
| --- | --- | --- | --- | --- |
| BM25 | 0.750 | 0.812 | 0.812 | 0.809 |
| Semantic | 0.875 | 0.938 | 0.938 | 0.908 |
| Hybrid | 0.875 | 0.938 | 0.938 | 0.922 |
| Reranked | 0.938 | 1.000 | 1.000 | 0.969 |

這份表不證明 Reranker 在所有資料集都最好，只證明目前九份文件、17 個 Chunk、16 題有答案評測下的結果。評測集小到一題就會改變 Hit@1 0.0625，且由同一位作者出題與標註；它適合做回歸基準，不是公開排行榜。

## 回答、引用與拒答成果

本機 `qwen3.5:9b` 的八題端到端回答實測：

```markdown
Status Accuracy       7/8
Citation Valid        7/8
Citation Precision    0.812
Reference Coverage    0.812
```

唯一狀態失敗不是知識錯誤，而是回答正文引用 `S1` 、 `S2` ，宣告欄位只列 `S1` 。系統選擇 Fail Closed，不交付來源鏈不一致的答案。另一方面，Token 題的「不等同」被字串評分器漏判，也證明自動評分本身需要人工審查。

拒答前置閘門則保留 16/16 有答案題，擋下 9/10 無答案題；FastAPI `Depends` 問題以 0.144 穿過 0.10 門檻。這個反例讓我們沒有把門檻硬調到 0.15，也沒有宣稱 Cross-encoder 分數是可回答機率。最終拒答依賴三層：檢索證據、模型 `insufficient` 、引用與格式驗證。

## Agentic 到底在哪裡？

本系列沒有加入一個自由規劃、任意呼叫工具的通用 Agent。這裡的 Agentic 是一組有邊界、可記錄、可回退的決策：

- 問題需要改寫嗎？移除識別字時回退原句。
- Follow-up 需要補回哪個主題？歷史只協助解析，不當證據。
- 候選證據足夠嗎？不足就在生成前拒答。
- 模型輸出符合引用契約嗎？不符合就 Fail Closed。
- 來源含明確注入語句嗎？命中就隔離，全部隔離則拒答。
- 新文件可以立即回答嗎？先進 Staging，審核與重建索引後才上線。

每個決策都有輸入、輸出、理由與失敗路徑。這種受限 Orchestration 比「讓模型自己想辦法」更符合目前知識助理的風險與評測能力，也讓 Agentic 不只是標題中的裝飾詞。

## 安全與工程成果

Day 28 的離線安全測試確認：惡意來源命中兩條注入規則並被隔離，偽造 `S999` 被引用驗證器拒絕，合法 `S1` 通過。這只是基本防線，不等同通過紅隊測試；正規表示式會漏掉變形攻擊，也可能誤傷討論安全議題的正常文件。

Day 29 將 Pipeline 包成 `/health` 、 `/v1/search` 、 `/v1/answers` 與 `/v1/documents` 。文件匯入回 HTTP 202 與 `indexed=false` ，不讓未審核文字直接進入線上 Context。API Schema 限制問題長度、Top-K 與來源數，Lifespan 則確保模型只載入一次並正確關閉 Qdrant。

最終離線驗證包括：

```markdown
Python compileall：Day 18–30 全部通過
核心與 API 單元測試：6/6 通過
Security checks：通過
pip check：No broken requirements found
```

API Test Client 仍發出相依套件棄用警告；它沒有造成測試失敗，但應列入升級工作，而不是從報告刪掉。

## 成果邊界：哪些事情還沒驗證？

這個作品已經可以搜尋、回答、引用、拒答、評測與提供 API，但仍不能稱為生產系統：

- 知識庫只有九份自寫技術文件、17 個 Chunk，遠少於真實規模。
- 評測集只有 18 題檢索題、10 題拒答題與 8 題回答題，且缺少雙人標註。
- 回答實測只有一個本機生成模型與單次執行，沒有多模型、重複生成與延遲分布。
- Context 預算仍以字元近似，尚未使用生成模型 Tokenizer。
- 問題改寫會漂移，多輪只有兩輪示範，尚未覆蓋主題切換與矛盾修正。
- Prompt Injection 防禦只有基礎規則、邊界與驗證，尚未進行完整紅隊測試。
- 文件 API 只進 Staging，還沒有背景審核、增量索引、版本切換與回滾。
- API 尚無認證、授權、Rate Limit、TLS、Audit Log 與文件級 ACL，不能公開暴露。

把限制寫清楚不是削弱成果，而是讓下一次改善有正確起點。RAG 最危險的不是「還有問題」，而是不知道問題在哪裡，卻用一個流暢回答掩蓋它。

## 如果再走下一個 30 天

如果繼續發展，優先順序應該是擴充資料與評測，而不是立刻換更大模型。先增加真實文件、近似文件、版本衝突與多文件題，請第二位標註者處理 q18 這類爭議；再建立 Claim-level Groundedness、更多近域無答案題與問題改寫評測。只有秤變可靠，模型與參數比較才有意義。

工程上則要把 Ingestion 做成背景工作：審核 Staging 文件、增量切分與向量 Upsert、重建稀疏索引、以版本化 Collection 原子切換；服務端加入 API 身分、文件 ACL、限流、Trace、P50/P95 延遲、成本與模型版本。安全面要持續進行多語與混淆攻擊測試，並維持最小工具權限。

## 三十天留下的可重現交付物

文章之外，這個系列保留了原始知識文件、清理與 Chunk 程式、BM25 與向量檢索、RRF、Reranker、Context Builder、LLM Client、輸出 Schema、引用驗證、Evidence Gate、問題改寫、多輪狀態、安全 Guard、FastAPI，以及對應的 YAML 評測資料與測試。它們不是三十份互不相干的 Demo，而是沿同一份 Metadata 與 `document_id` 串起來的版本。

重現結果時，仍要同時固定 Python 相依、模型 ID、知識庫版本與執行模式。離線測試能驗證契約，端到端生成則需要本機 Ollama 與指定模型；兩者應分開報告，不能因 Fake Client 測試通過就宣稱真實模型品質也通過。README 與各 Day 指令提供入口，實測數字則保留資料規模與限制，讓後續版本有可比較基線。

## 這個系列真正學到的五件事

第一，資料工程不是 RAG 前置雜務。孤兒標題、錯誤 Chunk 與遺失 Metadata 會一路放大到搜尋、回答和引用。第二，混合檢索與精排各有角色：稀疏和語意訊號負責召回，Cross-encoder 重新判斷配對，但任何分數都不自動等於可回答機率。

第三，引用必須是程式可驗證的契約。模型只能選擇 Context 中存在的 Marker，來源名稱與連結由程式映射；即使內容看似正確，只要引用鏈斷裂就不能悄悄交付。第四，拒答是一條多層流程，也需要有答案保留率和近域反例，不能用一句「不知道就說不知道」完成。

第五，評測不是最後一天才做的排行榜。從 Day 10 的第一份題目開始，評測一路幫我們發現檢索改善、標註爭議、引用格式失敗、字串評分誤判與門檻過度擬合。沒有逐題 Trace，再高的平均分都很難轉成下一個可靠修改。

## 如果把它交給下一位開發者

接手者第一步不該先換模型，而是跑 `final_check.py` 、單元測試與現有評測，確認環境能重現基線；接著閱讀失敗題與限制清單，再決定要擴資料、改檢索、調生成或補服務安全。任何修改都應保留前後逐題差異，以及索引、Prompt、模型和資料版本。

若要做第一個真實 POC，建議選一個範圍清楚、已有文件擁有者的內部主題，先建立 50 到 100 個真實問題與近域無答案題，再導入文件 ACL 和審核流程。使用者驗收要看能否找到證據、是否在資料不足時停下，以及錯誤能不能被追溯，而不只是聊天語氣是否自然。

## 最後再驗收一次

`code/day30/scripts/final_check.py` 會檢查 Day 1–30 檔名連續、評測 ID 不重複、回答狀態合法，以及 README 是否保留完整路線：

```bash
python scripts/final_check.py
python -m unittest discover -s tests -v
```

因此，最後得到的不是一個只能展示聊天畫面的 Demo，而是一個有資料契約、有檢索基準、有生成邊界、有來源驗證、有拒答反例、有安全測試，也知道自己尚未完成什麼的 Agentic RAG 知識助理。

## 完成不等於沒有下一步

競賽的三十天需要一個句點，系統本身則需要持續治理。知識會更新、模型會更換、攻擊方式會變，評測與權限也必須跟著版本演進。這套作品最重要的能力，不是宣稱永遠正確，而是每次回答都留下可檢查的來源與決策，並公開承認哪些條件尚未驗證。

三十天的主線到這裡結束：從文字清理走到 LLM 回答，從第一個關鍵字命中走到 Recall、MRR、引用與 Failure Trace。讓 LLM 開口並不難；讓它的每一句話都能回到證據，在證據不足時願意停下來，才是這個系列真正完成的事。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10407057)

[

Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務

](https://ithelp.ithome.com.tw/articles/10407057)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 30 篇

目錄

1. 26
	[Day 26｜評估回答品質：正確性、引用與幻覺分析](https://ithelp.ithome.com.tw/articles/10406229)
2. 27
	[Day 27｜RAG 常見失敗案例：Chunk、Top-K 與 Prompt 的影響](https://ithelp.ithome.com.tw/articles/10406516)
3. 28
	[Day 28｜RAG 安全性：Prompt Injection 與惡意文件](https://ithelp.ithome.com.tw/articles/10406994)
4. 29
	[Day 29｜把 RAG 包裝成 API：建立可使用的知識助理服務](https://ithelp.ithome.com.tw/articles/10407057)
5. 30
	[Day 30｜完整回顧：從 NLP 到 Agentic RAG 的成果與下一步](https://ithelp.ithome.com.tw/articles/10407473)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/9/19/e81292fc-aa02-4bfb-9f0c-643ab1cf557d_small.png)

[Elastic Security: 監測 x 告警，揪出潛在威脅！【Webinar：Elastic 系列講座】｜歐立威科技](https://itplus.ithome.com.tw/vod-page/670?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

歐立威科技 |

59 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/0ea22c57-7343-42b8-a25c-4eb8b3169710_small.png)

[以開發者資料驅動的 CI/CD 優化策略](https://itplus.ithome.com.tw/vod-page/1046?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/26/61f55fc8-2f99-4351-a252-ad4316e38759_small.png)

[Governance in MLOps - AI應用的守護者](https://itplus.ithome.com.tw/vod-page/1174?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/4/9/26d93c5a-9096-4d57-a7c2-86791e02f859_small.png)

[數位永續新格局：迎戰 CSRD 時代的 ESG 淨零轉型致勝關鍵 —Fortinet 顛覆性碳足跡解決方案與數位產品護照全面整合首發](https://itplus.ithome.com.tw/vod-page/1134?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位永續高峰會 |

30 分