---
title: "Day 21｜讓答案附上來源：RAG 引用機制設計 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10405135"
author:
  - "[[iThome]]"
published:
created: 2026-09-03
description: "昨天的第一個 RAG 回答讀起來沒有問題，結構卻露出一道裂縫：answer 用了 [S1] 與 [S2]，citations 只宣告 S1。介面若相信欄位，會漏掉 S2；程式若改信正文，又可能接受模型..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 21

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 21 篇

## Day 21｜讓答案附上來源：RAG 引用機制設計

- 分享至

昨天的第一個 RAG 回答讀起來沒有問題，結構卻露出一道裂縫： `answer` 用了 `[S1]` 與 `[S2]` ， `citations` 只宣告 `S1` 。介面若相信欄位，會漏掉 `S2` ；程式若改信正文，又可能接受模型憑空寫出的 `[S999]` 。兩邊都相信一半，正是最危險的狀態。

今天就沿著這個真實失敗往下追，把「請附上來源」從 Prompt 裡的一句要求，變成程式可以解析、檢查與映射的引用機制。

## 來源不是讓模型自由填寫的文字

本系列從 Day 4 就替文件保留 ID、標題、路徑與來源 Metadata，Day 6 的 Chunk 再保存 `document_id` ，Day 18 則替本次 Context 分配 `S1` 、 `S2` 。這條鏈就是引用的真實來源：

```markdown
[S1] → chunk_id → document_id → title / source_path / source_url
```

模型只能選擇 `S1` ，不能自行撰寫網址或文件標題。最後顯示的來源名稱由程式從 `ContextBundle` 取出。這樣即使標題日後修正，引用仍會指向同一份文件；模型也沒有機會把一個看似合理、實際不存在的網址送給使用者。

## 先解析回答契約

`answer_schema.py` 將模型輸出解析成固定資料：

```python
@dataclass(frozen=True)
class ParsedAnswer:
    status: str
    answer: str
    citations: tuple[str, ...]

def parse_model_output(raw: str) -> ParsedAnswer:
    payload = json.loads(raw.strip())
    status = payload.get("status")
    answer = payload.get("answer")
    citations = payload.get("citations")

    if status not in {"answered", "insufficient"}:
        raise ValueError("invalid status")
    if not isinstance(answer, str) or not answer.strip():
        raise ValueError("answer must be non-empty")
    if not isinstance(citations, list):
        raise ValueError("citations must be a list")
```

解析器容許模型偶爾加上的 Markdown JSON 圍欄，但不會猜測缺失欄位。格式不合法時，系統回覆「輸出格式無法驗證」，不把半套結果當成答案。

## 四條引用規則

驗證器同時取得 `ParsedAnswer` 與實際的 `ContextBundle` ，執行四個檢查：

1. 正文與 `citations` 中的代號都必須存在於本次 Context。
2. `answered` 回答至少要有一個正文引用。
3. 正文出現的引用集合必須和 `citations` 完全一致。
4. `insufficient` 回答不得附上來源。
```python
_INLINE_CITATION = re.compile(r"\[(S\d+)\]")

def validate_citations(answer, context):
    available = {source.marker for source in context.sources}
    declared = set(answer.citations)
    inline = set(_INLINE_CITATION.findall(answer.answer))
    errors = []

    unknown = (declared | inline) - available
    if unknown:
        errors.append(f"unknown citations: {sorted(unknown)}")
    if answer.status == "answered" and not inline:
        errors.append("answered response has no inline citation")
    if answer.status == "answered" and inline != declared:
        errors.append("inline citations and citations field do not match")
    if answer.status == "insufficient" and (declared or inline):
        errors.append("insufficient response must not cite sources")
    return tuple(errors)
```

這裡刻意採嚴格集合相等，而不是「有交集就好」。模型如果正文用了 `S2` ，就必須在欄位中承認；欄位宣告了正文沒出現的 `S3` ，也不應偷偷列進來源區。

## 把昨天的答案送進驗證器

把 Day 20 的原始 JSON 交給驗證器，結果是：

```markdown
inline citations = {'S1', 'S2'}
declared citations = {'S1'}
validation error = inline citations and citations field do not match
```

Pipeline 採 Fail Closed：不替模型猜「它大概只是忘了列 S2」，也不悄悄刪除正文的 `[S2]` ，而是把狀態改成 `insufficient` ：

```markdown
模型輸出未通過來源驗證，因此暫不提供答案。
```

這看起來很嚴格，卻是可信系統該有的行為。引用不是裝飾；只要來源鏈不一致，這個回答就還沒有準備好交付。正式系統可以在驗證失敗後做一次有上限的格式修復或重試，但不能無限重跑，也不能在沒有紀錄的情況下修改模型輸出。第一版先保留最清楚的拒絕行為。

## 來源列表由程式產生

驗證成功後，只把真正引用的 `ContextSource` 交給介面：

```python
cited = set(parsed.citations)
sources = tuple(
    source for source in context.sources
    if source.marker in cited
)
```

介面可以顯示：

```markdown
來源：
- [S1] Spring Security 的認證流程
  chunk: spring-security-authentication#005
```

絕對檔案路徑只適合內部除錯，不應直接曝光給外部使用者；若 Metadata 有 `source_url` ，正式 UI 可以改顯示經過允許的連結。這是「內部可追蹤」和「外部可公開」的不同資料層。

## 合法引用還不夠：要回到每個主張

驗證器只能證明三件事：引用代號存在、格式一致、來源能被映射。它不能證明來源文字真的支持回答中的每個主張，更不能證明知識庫本身正確。模型仍可能引用 `S1` ，卻在句子中多加一個 `S1` 沒寫的版本號。

因此，引用驗證是必要條件，還不是充分條件。Day 26 的回答評測還要檢查正確性、參考要點與人工 Groundedness；Day 28 則要防止惡意文件要求模型偽造引用。

第一步，是讓引用靠近它所支持的主張。只在答案最後列出「參考資料」仍不夠。假設一段回答同時說明 401、403 與 CSRF，最後只放一個 `[S1]` ，讀者無法判斷這份來源支持三個主張，還是只支持其中一個。因此 Prompt 要求引用緊跟在相關句子後面，而不是把所有代號集中到段落尾端。

```markdown
較難查證：401 代表未完成認證，403 代表權限不足。參考：[S1][S2]
較易查證：401 表示尚未通過認證 [S1]；403 表示身分已知但權限不足 [S2]。
```

這仍不是完整的 Claim-level Grounding，但至少保留了「哪一句話由哪份資料支持」的結構。前端也能讓使用者點擊句尾代號，直接展開對應 Chunk，而不必在長篇答案和來源列表之間來回猜測。

## 多個 Chunk 可能來自同一份文件

`S1` 與 `S2` 是本次 Context 的片段代號，不等於兩份獨立文件。當長文件切成數個 Chunk 後，模型可能同時引用同一文件相鄰的兩段。正文仍應保留兩個代號，因為它們指向不同證據位置；來源列表則可以在 UI 依 `document_id` 分組，顯示成一份文件下的兩個片段。

這個區分也會影響評估。若五個引用都來自同一份文件，不能宣稱「有五份來源交叉佐證」；反過來，也不能因 UI 去重成一列，就丟掉 Chunk 級的追蹤能力。文件層適合閱讀，Chunk 層適合驗證，兩者需要同時保留。

## 驗證器要測哪些邊界？

引用功能最適合用小型、確定性的單元測試保護。至少要涵蓋：合法單一引用、合法多引用、正文引用不存在的 `S999` 、欄位多宣告一個代號、 `answered` 沒有引用，以及 `insufficient` 卻附帶來源。這些測試不需要啟動 Embedding 或 LLM，只需建立假的 `ContextBundle` 與 `ParsedAnswer` 。

還要注意目前正規表示式只接受 `[S1]` ，不接受 `[s1]` 、 `[S 1]` 或全形括號。這是刻意收緊輸出契約，不是解析能力不足。若未來想容許更多格式，應先在解析層正規化，再讓驗證器只處理一種內部格式；否則同一個來源會有多種寫法，前端與評測都會變得難以一致。

## 三種引用品質不能混成一個分數

今天的檢查回答的是引用有效性（Citation Validity）：代號是否真的存在。之後還要分別看引用覆蓋（Citation Coverage）與來源支持度（Citation Support）。Coverage 檢查重要主張是否都有引用；Support 則確認來源內容是否真的能推出那個主張。一個回答可能三個代號都合法，卻只有第一句有引用；也可能每句都有引用，但引用的段落只談相似主題。

把三者拆開後，錯誤才有可修復方向：Validity 失敗多半是輸出契約問題，Coverage 不足可能要調 Prompt，Support 不足則可能來自檢索、Chunk 或模型推論。若全部合成一個「引用正確率」，系統就只得到一個分數，卻不知道該改哪一層。

## 結語

今天把來源引用做成一條封閉、可驗證的資料鏈：模型只使用 `S1` 等短代號，程式檢查代號是否存在、正文與宣告是否一致，再從 `ContextBundle` 映射回真正文件。Day 20 那份看似正確的回答因漏列 `S2` 被拒絕；系統不是突然變笨，而是第一次真的遵守自己的回答契約。

下一篇處理更前面的問題：如果檢索結果本來就不足，是否還要呼叫模型？我們會建立拒答的多層決策，並擴充無答案題，驗證「不知道就說不知道」究竟能做到什麼程度。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10405040)

[

Day 20｜第一個 RAG 問答流程：從問題到答案

](https://ithelp.ithome.com.tw/articles/10405040)[下一篇](https://ithelp.ithome.com.tw/articles/10405359)

[

Day 22｜不知道就說不知道：拒答與資料不足判斷

](https://ithelp.ithome.com.tw/articles/10405359)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/12de7ccf-38eb-426a-93d5-a2527e910e38_small.png)

[Monolithic application 於 AWS OpsWorks+Chef 的 Multi-Region 自動化維運管理實例](https://itplus.ithome.com.tw/vod-page/563?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/10/2/63204d50-6b41-4549-842c-9ff88bf6edc0_small.png)

[企業雲端Email及網頁服務守護者！以AI x API 為基礎阻絕APT攻擊【宏碁資訊網路學堂】](https://itplus.ithome.com.tw/vod-page/709?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

ＡＥＢ大補帖 |

40 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/abd417c7-bacf-4f11-9b67-853c0dfb91e1_small.png)

[從系統思考角度談 DevOps 三步工作法](https://itplus.ithome.com.tw/vod-page/973?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

61 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/ad0edf99-f7e1-45a5-8e67-6096057b7b54_small.png)

[用早稻田MBA的企業經營學經營軟體工程團隊、創造商業價值](https://itplus.ithome.com.tw/vod-page/974?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

40 分