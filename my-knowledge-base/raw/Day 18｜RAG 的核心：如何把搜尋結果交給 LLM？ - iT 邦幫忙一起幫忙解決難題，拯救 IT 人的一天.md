---
title: "Day 18｜RAG 的核心：如何把搜尋結果交給 LLM？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10404634"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "Day 17 的最後一句是：「RAG 的 R 完工了，該寫 G 了。」檢索線現在很會找資料：Hybrid Search 先撈出候選，Reranker 再把真正相關的 Chunk 排到前面。但輪到生成時..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 18

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 18 篇

## Day 18｜RAG 的核心：如何把搜尋結果交給 LLM？

- 分享至

Day 17 的最後一句是：「RAG 的 R 完工了，該寫 G 了。」檢索線現在很會找資料：Hybrid Search 先撈出候選，Reranker 再把真正相關的 Chunk 排到前面。但輪到生成時，我們還不能把 Top-3 直接貼給模型，期待它自己處理一切。

原因很具體。三段文字若只用換行串起來，模型不知道它們各自來自哪份文件；內容一多，還可能擠滿上下文視窗；不同程式若各用一種拼接格式，後面的引用與評測也無法共用。因此，檢索結果與 Prompt 之間還需要一個「上下文建構器」（Context Builder），負責保留來源、控制預算與固定順序。今天先把這座橋搭起來，再讓 LLM 上場。

## 先別急著把 Top-3 串起來

一個可交給模型的來源至少要保留七項資料：本次回答使用的來源代號、Chunk ID、文件 ID、標題、來源路徑、精排分數與正文。來源代號使用 `S1` 、 `S2` 這種只在單次請求中有效的短名稱，讓模型引用時不必抄寫長路徑；真正的標題與路徑仍由程式保留，最後再映射回去。

```python
@dataclass(frozen=True)
class ContextSource:
    marker: str
    chunk_id: str
    document_id: str
    title: str
    source_path: str
    score: float
    text: str

@dataclass(frozen=True)
class ContextBundle:
    question: str
    sources: tuple[ContextSource, ...]
    text: str
    used_characters: int
```

`ContextBundle` 因此不是一條長字串。 `text` 是模型實際看到、也方便開發者閱讀的內容； `sources` 則保存後續引用驗證需要的結構化資料。顯示格式日後可以改，來源身分卻不能跟著消失。

## 加入來源數量與長度預算

第一版先使用 `max_sources=3` 與 `max_characters=2400` 。這兩個數字不是最佳參數，只是可解釋的起點。Context Builder 依 Reranker 排名取資料，每個 Chunk 必須完整放入；若剩餘空間放不下後面的 Chunk，就停止加入，避免在任意字元處把設定步驟或程式碼切成兩半。只有第一個 Chunk 自己就超過總預算時，才會截斷並加上省略符號，確保至少有一份資料可供檢查。

```python
def build_context(2400
):
    sources, blocks, used = [], [], 0

    for result in results[:max_sources]:
        document = documents_by_id[result.chunk.document_id]
        marker = f"S{len(sources) + 1}"
        header = (
            f"[{marker}] title={document.title}\n"
            f"chunk_id={result.chunk.id}\n"
            f"source_path={document.path}\n"
        )
        remaining = max_characters - used - len(header)
        if remaining <= 0:
            break
        # 完整版本還會處理第一個 Chunk 超過預算的情況
        block = header + result.chunk.text
        if len(block) > max_characters - used:
            break
        blocks.append(block)
        used += len(block)
```

這裡先以字元數近似，原因和 Day 6 一樣：目前的 Chunk 很小、語言一致，字元數容易重現。等系統綁定特定生成模型後，服務就應改用該模型的 Tokenizer，連同 Prompt、對話歷史與預留輸出一起計算；2,400 字元並不等於 2,400 Token。

## 實際組一次 Context

執行：

```bash
HF_HUB_OFFLINE=1 python scripts/context_demo.py
```

查詢「HTTP 401 和 403 有什麼差別？」時，實測取回 3 個來源、使用 1,085 字元。第一名是認證文件中直接解釋 401 與 403 的 Chunk，第二名是提到未通過驗證會回傳 401 的 `SecurityFilterChain` 文件，第三名則是 CSRF 文件。

```markdown
問題：HTTP 401 和 403 有什麼差別？
來源：3 個，使用 1085 字元

[S1] title=Spring Security 的認證流程
chunk_id=spring-security-authentication#005
驗證失敗時，預設會回傳 HTTP 401；權限不足則是 HTTP 403……

[S2] title=SecurityFilterChain 的角色與設定方式
chunk_id=spring-security-filter-chain#000
未通過驗證的請求會得到 HTTP 401 回應……

[S3] title=CSRF 保護的原理與設定
chunk_id=spring-security-csrf#000
CSRF 攻擊利用瀏覽器自動附帶 Cookie 的行為……
```

這個結果也提醒我們：Top-3 不代表三段都同樣必要。 `S1` 已能回答差異， `S2` 提供補充， `S3` 只因同屬 Spring Security 而進入上下文。Generation 不是把檢索錯誤變不見；資料給得越多，也可能把不相關資訊一起交給模型。 `max_sources` 因此是品質與成本參數，不是越大越好。

## 順序與去重的取捨

第一版保留 Reranker 排名，不再依標題或文件 ID 重排，因為精排就是目前對相關性的最佳判斷。同一文件可以出現多個 Chunk，只要它們各自提供不同內容；來源列表顯示時則能再合併成文件層級。若未來加入相鄰 Chunk 擴張，還要處理重疊文字，避免 Context 裡出現近乎相同的段落。

Context Builder 也不把精排分數交給模型判斷「可信度」。分數是程式的檢索訊號，不是事實正確率；模型只需要看到來源代號、標題與內容。分數留在內部 Trace 中，之後做拒答與診斷時使用。

## Context Builder 需要哪些測試？

Context Builder 沒有使用生成模型，看起來只是字串處理，卻正好站在檢索與生成的交界。來源代號只要錯位一次，後面的引用就可能指向另一份文件；預算只要算錯，程式碼也可能被攔腰切斷。因此，這個小元件反而適合用快速、確定性的單元測試守住。最基本的測試至少涵蓋四種情況。

第一，結果少於 `max_sources` 時，不能為了湊滿數量加入空來源。第二，同一組搜尋結果重跑時，Marker 與順序必須一致，否則同一份 Trace 無法比較。第三，第一個 Chunk 超過預算時可以安全截斷，但第二個之後放不下就應整段停止，不能產生沒有結尾的程式碼。第四， `document_id` 找不到對應 Metadata 時應直接失敗，而不是把 ID 當標題繼續生成；這代表索引與原始資料不同步，需要先修資料管線。

```python
def test_context_budget_keeps_source_identity():
    bundle = build_context(
        question="測試問題",
        results=ranked_results,
        documents_by_id=documents_by_id,
        max_sources=2,
        max_characters=800,
    )
    assert len(bundle.sources) <= 2
    assert bundle.used_characters <= 800
    assert [source.marker for source in bundle.sources] == ["S1", "S2"]
    assert bundle.sources[0].chunk_id == ranked_results[0].chunk.id
```

這種測試不需要 Ollama、Embedding 或網路，執行速度很快，適合放進每次 Commit 的基本回歸。真正昂貴的模型評測可以較低頻率執行，但資料契約一旦壞掉，應該在最前面就被攔下。

## 字元預算如何換成 Token 預算？

目前的 2,400 字元只是開發階段的可讀近似。正式改成 Token 時，不能只把 `len(text)` 換成一個 Tokenizer 呼叫，還要把完整請求拆成四筆帳：固定 System Prompt、使用者問題、所有來源，以及預留的模型輸出。假設模型 Context 上限是 8,192 Token，不能把來源也塞到 8,192；若預計回答最多 800 Token，還要替 Messages 的角色標記、JSON 結構與安全規則保留空間。

```markdown
可用來源預算
= 模型 Context 上限
- System Prompt
- 使用者問題與歷史
- 預留回答 Token
- 格式與安全緩衝
```

另一個常見問題是來源長度分配。完全按照排名逐段加入，第一個長 Chunk 可能吃掉全部預算；平均分配又可能截斷最重要的第一名。更進一步的策略可以替第一名保留較多預算、後續來源只取回答問題附近的句子，或先對相鄰 Chunk 做去重。這些策略都會改變模型看到的證據，因此也必須納入 Day 26 的回答評測，而不能只用 Token 節省比例決定。

## 最後別忘了留下上下文軌跡

每次問答應至少留下原問題、改寫後查詢、候選排名、被選入的來源代號、Chunk ID、使用長度與索引版本。遇到使用者回報錯誤時，才能重建當時模型究竟看到了什麼。如果只保存最後答案，知識庫一更新、模型一換版，錯誤就再也無法重現。

同時，Trace 不等於把完整內文永久寫進一般 Log。若文件含有內部資訊，Log 只記 ID、分數與版本，正文放在有權限與保存期限的稽核儲存中。可觀察性與資料最小化不是互斥選項，需要從一開始就分層設計。

## 結語

今天沒有讓 LLM 多回答一句話，卻完成了 Retrieval 與 Generation 之間最容易被忽略的一層：將排序結果轉成帶有來源身分、長度預算與穩定代號的 `ContextBundle` 。同一條查詢實測得到 3 個來源與 1,085 字元，也看見「相關」不等於「回答必需」——第三個 CSRF Chunk 就是具體的干擾證據。

下一篇要替這份 Context 寫使用契約。Prompt 不只要說「請根據資料回答」，還要定義來源是資料而不是指令、資料不足時如何回覆，以及模型必須輸出什麼結構。搜尋結果準備好了，接下來要決定模型可以怎麼使用它。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10404181)

[

Day 17｜Reranker：搜尋結果太多時，如何重新排序？

](https://ithelp.ithome.com.tw/articles/10404181)[下一篇](https://ithelp.ithome.com.tw/articles/10404701)

[

Day 19｜Prompt 設計：要求 LLM 只根據資料回答

](https://ithelp.ithome.com.tw/articles/10404701)

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

[![.](https://itadstatic.ithome.com.tw/B9/1787886940_6a90fd5c11c52.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/7/18/64587017-e3a7-4fc5-a342-546a00537119_small.jpg)

[資安弱點日新月異？讓曝險管理平台協助您一站瞭解公司資訊安全性！【宏碁資訊網路學堂】](https://itplus.ithome.com.tw/vod-page/90?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

ＡＥＢ大補帖 |

32 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/22/abb5eb18-d42b-452e-ae40-ff0e52b7d480_small.png)

[Zero Trust 讀書會：必讀 NIST 800-207標準解析 Part 1](https://itplus.ithome.com.tw/vod-page/619?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

零信任資安講堂 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/9/19/78e7646d-aaae-46e5-bad6-3d99c1ffbe3c_small.jpg)

[F5 NGINX Modernizes App 系列二：The NGINX 強化 API 架構高可用、安全性與可觀測性](https://itplus.ithome.com.tw/vod-page/964?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/03446d08-fbd2-4da2-8c04-90d39788a79c_small.png)

[純前端也能搞 AI？Gemini Nano 簡介](https://itplus.ithome.com.tw/vod-page/1375?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

39 分