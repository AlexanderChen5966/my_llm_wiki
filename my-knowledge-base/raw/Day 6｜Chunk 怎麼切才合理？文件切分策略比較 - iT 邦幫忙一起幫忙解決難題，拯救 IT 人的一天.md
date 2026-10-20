---
title: "Day 6｜Chunk 怎麼切才合理？文件切分策略比較 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402481"
author:
  - "[[iThome]]"
published:
created: 2026-08-13
description: "上一篇文章，我們完成了知識庫的文件載入器：讀取 Markdown、解析 Front Matter、清理格式雜訊，最後產生帶有 Metadata 與來源路徑的 Document。今天要處理接在它後面的問..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 6

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 6 篇

## Day 6｜Chunk 怎麼切才合理？文件切分策略比較

- 分享至

上一篇文章，我們完成了知識庫的文件載入器：讀取 Markdown、解析 Front Matter、清理格式雜訊，最後產生帶有 Metadata 與來源路徑的 `Document` 。今天要處理接在它後面的問題：一份清理完成的文件，要怎麼切成適合搜尋的片段？

Day 3 討論 Context 時已經埋下伏筆：RAG 不是把整個知識庫塞進 Prompt，而是先找出少量相關內容再交給模型，而這個「少量內容」的基本單位就是 Chunk。Chunk 切得太大，一次搜尋會帶進大量不相關的文字，稀釋真正有用的訊號；切得太碎，每個片段又會失去讓人看懂它的前後文。今天會先把測試語料準備起來，再實作四種常見的切分策略，用實際輸出比較它們的差異。

## 先把測試語料準備起來

Day 5 用來測試載入器的三份文件只是佔位的雛形，今天把它們補寫成有實際內容的文件，再新增六份，湊成一批可以反覆使用的測試語料。九份文件依照 Day 4 的資料夾規劃分成三個主題：spring-security 底下是 SecurityFilterChain、認證流程與 CSRF 保護；natural-language-processing 底下是 Token、文字清理與中文斷詞；rag 底下是 RAG 概念、文件切分與 Embedding。每份文件都遵守 Day 4 的資料契約，帶有識別碼、標題、主題與版本等 Metadata。

準備語料時，刻意保留了幾種會讓切分策略露出弱點的結構：有單獨成行的標題、有超過四百字的長段落，也有將近五百字的 Java 程式碼區塊。文件總量看起來很小，但這是刻意的選擇——九份文件的所有切分結果都能人工逐一檢查，哪個策略把句子切成兩半，看一眼就知道。等 Day 10 建立評測集時，這批文件也會是設計問題與標準答案的基礎。先求可核對，再求規模。

## 為什麼不能把整份文件當成搜尋單位？

最直覺的做法其實是不切分：搜尋時比對整份文件，命中後把全文交給模型。文件只有九份時，這樣做不會有明顯問題，但它有三個結構性缺點。第一，Day 3 談過 Context 有容量與品質限制，一次帶入多份完整文件很快就會碰到上限，也迫使模型在大量不相關的文字中尋找答案。第二，搜尋粒度會影響排序品質：一份長文件可能只有一段和問題相關，以整份文件計分時，這段訊號會被其餘內容稀釋。第三，引用也需要粒度——告訴使用者「答案在這份文件裡」，和「答案在這一段」，是完全不同的體驗。

反過來，切得越碎，每個片段越精準，卻也越難被獨立理解。「它會影響搜尋結果」這種句子單獨成為一個 Chunk 時，沒有人知道「它」指的是什麼。切分策略要做的，就是在這兩端之間找一個可以被驗證的位置。

## 四種常見的切分策略

最簡單的是固定長度切分：每 N 個字切一刀。優點是實作簡單、片段大小完全可控；缺點也很直接——它完全不理會語意邊界，句子、段落甚至程式碼區塊都可能被攔腰切斷。

重疊切分是固定長度的常見補救：讓相鄰片段共享一段文字，例如每段三百字、前後重疊五十字。就算某句話正好落在切分邊界上，它仍有機會完整出現在下一個片段裡。代價是片段數量變多、內容重複，之後建立索引與比對的成本都會增加。

段落切分改用作者自己標注的結構。Markdown 的空行本來就是語意邊界，照著段落切，片段的完整性由作者保證。問題在於長度失控：標題單獨一行也是一個段落，而一個寫得很長的段落可能遠超過想要的大小。

句子切分的粒度介於兩者之間。繁體中文的句子邊界主要由「。」「！」「？」「；」這些全形標點決定，與英文以半形句點加空白斷句的習慣不同。單一句子通常太短，不適合直接當作 Chunk，實務上會把連續的句子打包，直到接近大小上限。

還有一條 Day 5 就確立的原則，在切分階段同樣適用：程式碼區塊必須保持完整。一段被切成兩半的設定範例，對搜尋與引用都沒有價值，甚至會誤導。因此接下來的實作中，程式碼區塊永遠是不可分割的單位，即使它超過大小上限。

## 實作：讓策略可以互相比較

接著建立 `scripts/chunk_documents.py` 。它從 Day 5 的載入器接手 `Document` ，提供三個切分函式： `fixed_size_chunks()` 同時支援固定長度與重疊， `paragraph_chunks()` 依段落切分， `structured_chunks()` 則是組合策略——段落優先合併，超過上限的長段落再按句子打包。

```python
from dataclasses import dataclass
from pathlib import Path
import re

from load_documents import Document, load_documents

# 繁體中文的句子邊界：全形句號、驚嘆號、問號與分號
_SENTENCE_END = re.compile(r"(?<=[。！？；])")

@dataclass(frozen=True)
class Chunk:
    id: str
    document_id: str
    index: int
    text: str

def make_chunks(document: Document, pieces: list[str]) -> list[Chunk]:
    texts = [piece.strip() for piece in pieces]
    texts = [text for text in texts if text]
    return [
        Chunk(
            id=f"{document.id}#{index:03d}",
            document_id=document.id,
            index=index,
            text=text,
        )
        for index, text in enumerate(texts)
    ]

def fixed_size_chunks(document: Document, size: int = 300, overlap: int = 0) -> list[Chunk]:
    """固定長度切分；overlap 大於 0 時，相鄰片段共享結尾文字。"""
    step = size - overlap
    text = document.content
    pieces = [text[start:start + size] for start in range(0, len(text), step)]
    return make_chunks(document, pieces)

def split_blocks(content: str) -> list[str]:
    """切成段落與程式碼區塊；程式碼區塊視為不可分割的整體。"""
    blocks: list[str] = []
    buffer: list[str] = []
    in_code_block = False

    for line in content.split("\n"):
        if line.strip().startswith("\`\`\`"):
            buffer.append(line)
            if in_code_block:
                blocks.append("\n".join(buffer))
                buffer = []
            in_code_block = not in_code_block
            continue

        if in_code_block:
            buffer.append(line)
            continue

        if not line.strip():
            if buffer:
                blocks.append("\n".join(buffer))
                buffer = []
            continue

        buffer.append(line)

    if buffer:
        blocks.append("\n".join(buffer))
    return blocks

def paragraph_chunks(document: Document) -> list[Chunk]:
    """一個段落（或程式碼區塊）就是一個 Chunk。"""
    return make_chunks(document, split_blocks(document.content))

def structured_chunks(document: Document, max_size: int = 400) -> list[Chunk]:
    """段落優先合併，過長段落再按句子打包；程式碼區塊永遠保持完整。"""
    pieces: list[str] = []
    current: list[str] = []
    current_size = 0

    def flush() -> None:
        nonlocal current, current_size
        if current:
            pieces.append("\n\n".join(current))
            current = []
            current_size = 0

    for block in split_blocks(document.content):
        if len(block) > max_size and not block.startswith("\`\`\`"):
            flush()
            piece = ""
            for sentence in _SENTENCE_END.split(block):
                if piece and len(piece) + len(sentence) > max_size:
                    pieces.append(piece)
                    piece = ""
                piece += sentence
            if piece:
                pieces.append(piece)
            continue

        if current_size + len(block) > max_size:
            flush()
        current.append(block)
        current_size += len(block)

    flush()
    return make_chunks(document, pieces)

def describe(name: str, chunks: list[Chunk]) -> None:
    lengths = [len(chunk.text) for chunk in chunks]
    print(
        f"{name:<24} chunks={len(lengths):>3}  "
        f"avg={sum(lengths) // len(lengths):>4}  "
        f"min={min(lengths):>3}  max={max(lengths):>4}"
    )

if __name__ == "__main__":
    documents = load_documents(Path("knowledge-base"))
    print(f"Loaded {len(documents)} documents\n")

    strategies = {
        "fixed(300)": lambda d: fixed_size_chunks(d, size=300),
        "fixed(300, overlap=50)": lambda d: fixed_size_chunks(d, size=300, overlap=50),
        "paragraph": paragraph_chunks,
        "structured(400)": structured_chunks,
    }

    for name, strategy in strategies.items():
        all_chunks: list[Chunk] = []
        for document in documents:
            all_chunks.extend(strategy(document))
        describe(name, all_chunks)
```

有兩個設計值得說明。第一，每個 `Chunk` 都帶著 `document_id` 與流水序號組成的識別碼，例如 `nlp-token#002` 。這延續 Day 4 的可追溯性要求：未來回答引用來源時，系統要能從 Chunk 一路連回原始文件與 Metadata。第二， `split_blocks()` 沿用 Day 5 的圍欄判斷方式，把程式碼區塊視為原子單位； `structured_chunks()` 遇到超長的程式碼區塊時，寧可讓 Chunk 超過上限，也不切開它。

另外要誠實說明一個簡化：這裡的大小單位是字元數，不是 Day 3 說的 Token。字元數在同一種語言內是合理的近似，實作與觀察也比較直覺；等後面接上 Embedding 模型與 LLM 時，會再回來把上限換算成 Token。

## 實際執行與觀察

在專案根目錄執行：

```bash
python scripts/chunk_documents.py
```

九份文件在四種策略下的結果如下：

```markdown
Loaded 9 documents

fixed(300)               chunks= 15  avg= 245  min=  1  max= 300
fixed(300, overlap=50)   chunks= 20  avg= 207  min= 10  max= 300
paragraph                chunks= 38  avg=  95  min= 11  max= 461
structured(400)          chunks= 17  avg= 217  min= 13  max= 461
```

統計數字已經透露了各策略的性格，但真正的問題要看切分邊界。固定長度在認證流程那份文件上的表現是這樣的：

```markdown
第 1 個 Chunk 的結尾：…一個或多個 AuthenticationProvider。以帳號密碼登
第 2 個 Chunk 的開頭：入為例，DaoAuthenticationProvider 會透過…
```

「登入」這個詞被從中間切成兩半：前一個片段以「以帳號密碼登」收尾，後一個以「入為例」開頭。之後無論是關鍵字搜尋還是斷詞，都無法在任何一個片段裡找到完整的「登入」。同一份文件的 Java 程式碼區塊也被切成兩段，兩個片段各自帶著未閉合的程式碼圍欄。統計表裡 fixed(300) 的 min=1 同樣值得一看：有一份文件的長度剛好比 300 的倍數多一個字，於是最後一個 Chunk 只剩一個句號。固定長度的問題不是「偶爾切壞」，而是它對內容結構完全沒有概念。

重疊版本讓片段數從 15 增加到 20，換來的保險是落在邊界上的句子有機會在下一個片段中完整出現。但它仍然會切壞程式碼區塊，只是壞的位置不同。

段落切分產生的 38 個 Chunk 完整性最好，但平均長度只有 95 字；min=11 的那個 Chunk 是「# Token 是什麼」——一行單獨成塊的標題，對搜尋幾乎沒有價值。structured(400) 把相鄰的短段落合併、把超過 400 字的長段落按句子打包，17 個 Chunk 的平均長度回到 217；max=461 是認證文件裡那個 Java 程式碼區塊，超過上限但完整保留，這是設計上的取捨而不是缺陷。

structured 也不是沒有問題。中文斷詞那份文件的長段落超過上限、觸發句子打包時，前面的標題被單獨排擠成一個只有 17 字的 Chunk。標題應該和它的內文黏在一起、Chunk 是否應該記住自己所屬的標題路徑，這些問題先記下來，等 Day 21 設計引用機制時一併處理。

## 今天的選擇

比較之後，本系列的預設策略是 structured(400)：以段落為基礎、長段落按句子打包、程式碼區塊永遠完整。它在片段完整性與大小可控之間的平衡最好，Chunk 識別碼也讓每個片段都能追溯回原始文件。

但要強調的是，今天的比較只涵蓋「結構完整性」——我們還沒有任何證據說 structured(400) 的搜尋品質比較好，400 這個數字也只是合理的起點而不是結論。固定長度與重疊切分並沒有被淘汰，它們實作簡單、行為可預期；等 Day 10 建立評測集之後，所有策略都要用同一組問題重新接受檢驗。到時候比的不是誰切得漂亮，而是誰讓正確的內容更容易被找到。

## 結語

今天知識庫從三份雛形文件擴充成九份測試語料，並完成了 `chunk_documents.py` ：四種切分策略、統一的 `Chunk` 資料結構，以及一組可以重複執行的比較輸出。實際數據讓每種策略的性格變得具體——固定長度會把「登入」切成「登」與「入」，段落切分會讓標題單獨成塊，structured 則用合併與句子打包換到比較穩定的片段大小。

下一篇，我們會面對中文搜尋繞不開的前置步驟：斷詞。同樣一批 Chunk，「向量資料庫」被切成「向量／資料庫」還是「向量資料／庫」，會直接決定關鍵字搜尋找不找得到它。我們會處理中文斷詞，並建立本系列第一個搜尋基準。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402199)

[

Day 5｜繁體中文文件清理：RAG 前不可忽略的 NLP 基礎

](https://ithelp.ithome.com.tw/articles/10402199)[下一篇](https://ithelp.ithome.com.tw/articles/10402521)

[

Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋

](https://ithelp.ithome.com.tw/articles/10402521)

系列文

[讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理](https://ithelp.ithome.com.tw/users/20163705/ironman/9159) 共 7 篇

目錄

1. 3
	[Day 3｜LLM 到底知道什麼？Token、Context 與知識限制](https://ithelp.ithome.com.tw/articles/10402075)
2. 4
	[Day 4｜建立知識庫：從 Markdown 文件到可處理資料](https://ithelp.ithome.com.tw/articles/10402198)
3. 5
	[Day 5｜繁體中文文件清理：RAG 前不可忽略的 NLP 基礎](https://ithelp.ithome.com.tw/articles/10402199)
4. 6
	[Day 6｜Chunk 怎麼切才合理？文件切分策略比較](https://ithelp.ithome.com.tw/articles/10402481)
5. 7
	[Day 7｜先不用 AI：中文斷詞與最基本的關鍵字搜尋](https://ithelp.ithome.com.tw/articles/10402521)
[完整目錄](https://ithelp.ithome.com.tw/users/20163705/ironman/9159)

[![.](https://itadstatic.ithome.com.tw/B9/1784292772_6a5a25a43c9b5.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/6/30/952c1084-8afa-4097-ba23-cf59109fef24_small.png)

[訓練與認證管理系統打造全方位人才培訓](https://itplus.ithome.com.tw/vod-page/133?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

20 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/14/f565b41a-0f14-4861-b56f-0e5b0d044bb5_small.jpg)

[小孩子才做選擇：又廣、又深、又快的網路分析，誰說不能全都要？](https://itplus.ithome.com.tw/vod-page/374?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/46f5257d-797c-4fdb-82a2-4b8c236682d8_small.png)

[如何用JS建構POS系統 - 發票機跟刷卡機](https://itplus.ithome.com.tw/vod-page/1071?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

21 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/e03f039c-9b4d-49e3-9d66-fbf9c3330d22_small.png)

[FinOps with PostgreSQL](https://itplus.ithome.com.tw/vod-page/1224?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分