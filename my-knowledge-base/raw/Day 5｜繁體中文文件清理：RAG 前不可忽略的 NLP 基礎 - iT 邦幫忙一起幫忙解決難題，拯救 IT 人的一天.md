---
title: "Day 5｜繁體中文文件清理：RAG 前不可忽略的 NLP 基礎 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402199"
author:
  - "[[iThome]]"
published:
created: 2026-08-13
description: "上一篇文章，我們替知識庫建立了基本規則：使用 Markdown 保存原始文件，用 YAML Front Matter 保存識別碼、標題、來源、主題與版本，並讓原始資料和之後產生的搜尋索引分開管理。今天..."
tags:
  - "clippings"
status: "processed"
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 5

0

[AI Engineering](https://ithelp.ithome.com.tw/2026ironman/ai-engineering)

### 讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理系列 第 5 篇

## Day 5｜繁體中文文件清理：RAG 前不可忽略的 NLP 基礎

- 分享至

上一篇文章，我們替知識庫建立了基本規則：使用 Markdown 保存原始文件，用 YAML Front Matter 保存識別碼、標題、來源、主題與版本，並讓原始資料和之後產生的搜尋索引分開管理。今天會沿著這個設計往前走，第一次使用 Python 讀取知識庫，將人類方便閱讀的 Markdown 文件轉換成後續程式可以處理的資料。

這也是本系列第一篇真正進入可執行程式碼的文章。前幾天先定義問題、模型限制與資料格式，並不是刻意拖延實作，而是希望今天開始寫程式時，每一行程式都有清楚的目的。我們現在要解決的問題不是「如何把文字變乾淨」這麼簡單，而是：哪些格式雜訊應該移除，哪些內容即使看起來不規則，也必須被保留下來？

## 文件清理不是把文章改得漂亮

對人類來說，多一個空白、不同的換行或檔案使用不同的編碼，通常不會妨礙閱讀；對搜尋系統來說，這些差異可能造成不必要的結果。文件中可能同時存在 Windows 與 Unix 的換行符號、檔案開頭的 BOM、段落結尾多餘的空白，或連續出現許多空白行。如果不先處理，之後的文件切分可能把同一個段落拆得不自然，搜尋結果也可能包含大量空白與格式雜訊。

Markdown 還有另一個需要特別小心的地方：它的格式符號有時候本身就是內容的一部分。標題層級可以幫助我們理解段落結構，清單可能代表操作步驟，程式碼區塊則包含 API 名稱、設定格式與可直接使用的範例。清理的目的不是把所有符號刪掉，而是移除不影響語意的雜訊，同時保留未來搜尋與引用需要的資訊。

這個原則很重要。若為了讓文字看起來一致，直接刪除所有標點、英文、數字或程式碼，確實可能得到比較「乾淨」的字串，卻也會失去技術文件最重要的線索。像 `SecurityFilterChain` 、 `spring.security.user.name` 、 `HTTP 401` 與版本號，都可能是使用者查詢時最關鍵的內容。

## 這次先處理哪些問題？

Day 5 先處理不會改變原始語意的格式問題，包括換行符號統一、移除檔案開頭的 BOM、刪除一般段落結尾多餘的空白，以及將過多的空白行壓縮成合理的段落間距。除此之外，還有兩個繁體中文文件特有的雜訊。一是全形空白（U+3000），它經常混在行尾或段落之間，只針對半形空白的清理規則抓不到它。二是全形英數字：從 PDF 或舊網頁複製內容時，常會出現「ＨＴＴＰ　４０１」這種形式，若不先轉換成「HTTP 401」，之後的關鍵字搜尋就可能永遠比對不到。這些處理不需要理解文章的主題，也不應該改寫作者原本的句子。

不過，全形標點不在轉換範圍內。「，」「。」「？」是繁體中文的正規標點，不是需要修正的雜訊；把它們換成半形符號，等於改寫了作者的文字。同樣地，Python 內建的 NFKC 正規化雖然能一次轉換所有全形字元，但影響範圍過大，連上標、分數等字元都會一併被改寫，套用在程式碼上更可能直接改壞範例。因此這裡只轉換全形英數字與全形空白這個明確安全的子集，而且只在程式碼區塊之外進行。

至於重複文件、過期版本、HTML 標籤與內容本身的錯誤，今天不會用一個正規表示式全部刪除。重複與版本需要依賴 Metadata 判斷，HTML 是否應該保留要看資料來源，內容正確性則需要人工檢查或另外建立驗證流程。過度清理容易讓系統在沒有察覺的情況下改變原始資料，因此這裡採用「先做安全的格式整理，保留原始檔案」的策略。

## 先安裝讀取 Front Matter 的工具

Day 4 使用 YAML Front Matter 保存文件 Metadata，今天需要一個能夠同時解析 Front Matter 與 Markdown 正文的工具。這裡使用 Python 的 `python-frontmatter` 套件；它會把檔案上方的 Metadata 與下方的正文分開，讓我們不必自己處理 YAML 分隔線與欄位格式。

本系列以 Python 3.11 開發（新版 `python-frontmatter` 需要 Python 3.10 以上）。如果使用一般 Python 虛擬環境，可以先安裝套件：

```bash
python -m pip install python-frontmatter
```

這個套件只負責讀取文件，不會替我們決定哪些內容應該被刪除。文字清理規則仍然由自己的程式控制，這樣未來需要調整規則時，才不會把資料處理邏輯藏在工具裡。

## 建立第一個文件載入器

接著建立 `scripts/load_documents.py` 。這段程式會掃描 `knowledge-base/` 底下所有 Markdown 檔案，解析 Metadata，清理正文，最後將每份文件整理成一個 `Document` 物件。

```python
from dataclasses import dataclass
from pathlib import Path
import re

import frontmatter

# 全形英數字與全形空白轉半形；全形標點是繁體中文的正規寫法，不轉換
_FULLWIDTH_TO_HALFWIDTH = {code: code - 0xFEE0 for code in range(0xFF10, 0xFF1A)}  # ０-９
_FULLWIDTH_TO_HALFWIDTH.update({code: code - 0xFEE0 for code in range(0xFF21, 0xFF3B)})  # Ａ-Ｚ
_FULLWIDTH_TO_HALFWIDTH.update({code: code - 0xFEE0 for code in range(0xFF41, 0xFF5B)})  # ａ-ｚ
_FULLWIDTH_TO_HALFWIDTH[0x3000] = 0x20  # 全形空白

@dataclass(frozen=True)
class Document:
    id: str
    title: str
    metadata: dict
    content: str
    path: str

def clean_markdown(text: str) -> str:
    """清理不影響語意的格式雜訊，保留程式碼區塊內容。"""
    text = text.replace("\r\n", "\n").replace("\r", "\n")
    text = text.lstrip("\ufeff")

    cleaned_lines: list[str] = []
    in_code_block = False
    previous_line_was_blank = False

    for raw_line in text.split("\n"):
        if raw_line.strip().startswith("\`\`\`"):
            in_code_block = not in_code_block
            cleaned_lines.append(raw_line.rstrip())
            previous_line_was_blank = False
            continue

        if in_code_block:
            cleaned_lines.append(raw_line)
            continue

        line = raw_line.translate(_FULLWIDTH_TO_HALFWIDTH)
        line = re.sub(r"[ \t]+$", "", line)

        if not line.strip():
            if previous_line_was_blank:
                continue
            previous_line_was_blank = True
            cleaned_lines.append("")
            continue

        previous_line_was_blank = False
        cleaned_lines.append(line)

    return "\n".join(cleaned_lines).strip()

def load_documents(root: Path) -> list[Document]:
    documents: list[Document] = []

    for path in sorted(root.rglob("*.md")):
        post = frontmatter.load(path, encoding="utf-8-sig")
        metadata = dict(post.metadata)
        content = clean_markdown(post.content)

        document = Document(
            id=str(metadata.get("id") or path.stem),
            title=str(metadata.get("title") or path.stem),
            metadata=metadata,
            content=content,
            path=str(path),
        )
        documents.append(document)

    return documents

if __name__ == "__main__":
    documents = load_documents(Path("knowledge-base"))

    print(f"Loaded {len(documents)} documents")
    for document in documents:
        print(f"- {document.id}: {len(document.content)} characters")
```

這段程式有三個值得注意的設計。第一， `Document` 同時保存 Metadata、清理後正文與原始檔案路徑。後面搜尋結果需要回到來源時，不能只有一段文字，還必須知道它來自哪一份文件。第二， `clean_markdown()` 完整保留了程式碼區塊中的原始內容：不只空白與縮排維持原樣，全形轉換也不會進入程式碼區塊，因為字串裡的全形字元究竟是不是雜訊，不是清理程式可以代替作者判斷的。第三，這個載入器假設程式碼區塊一律使用 ` ``` ` 圍欄標記；Markdown 另外支援的 `~~~` 圍欄與四格縮排寫法，這裡刻意不支援，改由知識庫的撰寫規範統一格式。與其讓清理程式支援所有語法變形，不如先約束資料來源，讓規則保持簡單、行為可預期。

BOM 的處理位置也值得說明。實際測試會發現，如果檔案開頭帶有 BOM，而 `frontmatter.load()` 使用預設的 UTF-8 讀取，整份 Front Matter 會無法被辨識：Metadata 變成空的， `---` 區塊整段留在正文裡，而且過程中不會出現任何錯誤訊息。因此程式在讀檔時指定 `encoding="utf-8-sig"` ，讓 BOM 在解析之前就被移除；這個編碼讀取沒有 BOM 的檔案也完全正常。 `clean_markdown()` 裡的 `lstrip("\ufeff")` 則是第二層防禦，讓這個函式未來直接接收其他來源的字串時，行為仍然一致。

## 實際執行載入器

清理規則是否如預期運作，不能只靠閱讀程式碼確認。在專案根目錄執行載入器：

```bash
python scripts/load_documents.py
```

以 Day 4 規劃的資料夾結構放入三份測試文件後，會看到類似下面的輸出：

```markdown
Loaded 3 documents
- nlp-token: 33 characters
- rag-retrieval-augmented-generation: 81 characters
- spring-security-filter-chain: 229 characters
```

為了驗證清理行為，其中一份測試文件刻意用帶 BOM 的編碼儲存，另一份在正文放入「ＨＴＴＰ　４０１」這樣的全形雜訊。載入結果中，帶 BOM 的文件 Metadata 依然完整，全形英數字已經轉成「HTTP 401」，Java 程式碼區塊則一字未動。

## 為什麼要保留原始檔案路徑？

目前看起來， `path` 只是一個檔案位置；但在完整的 RAG 流程中，它會成為追蹤資料來源的重要線索。未來文件切成多個 Chunk 後，每個 Chunk 都可以保留文件的 `id` 、標題與路徑。當 LLM 產生回答時，系統就能把相關 Chunk 重新連回原始 Markdown，進一步顯示來源或產生引用連結。

如果在最初的載入階段就只留下清理後的文字，之後才發現需要顯示來源，就必須重新設計資料流。這也是為什麼本系列一直強調 Metadata 與可追溯性：搜尋品質固然重要，但回答能不能回到原始文件，同樣是知識助理的一部分。

## 清理前後應該如何驗證？

清理程式不能只看「有沒有成功執行」，還要確認它沒有意外破壞內容。最基本的檢查，是比較清理前後的文件數量、標題、Metadata、正文長度與程式碼區塊，並抽查全形英數字是否只在正文中被轉換、程式碼區塊是否維持原樣。若某份文件在清理後突然變成空白，或所有文件的標題都消失，就代表資料處理流程需要先停下來檢查。

今天的程式還沒有進行語意上的修改，因此不會判斷某句話是否正確，也不會刪除重複知識。這是刻意的保守設計：先讓輸入資料格式一致，再在後面的步驟逐一處理重複、版本與搜尋問題。每增加一個清理規則，都應該能回答「它要解決什麼問題」以及「它會不會改變原始語意」。

## 結語

今天是本系列第一次把知識庫交給程式處理。我們使用 `python-frontmatter` 解析 Markdown 文件，建立包含 Metadata、正文與來源路徑的 `Document` ，再透過清理函式統一換行、移除 BOM、整理多餘空白（包含全形空白）、把正文中的全形英數字轉成半形，同時完整保留程式碼區塊。

這個載入器還不是完整的 NLP Pipeline，但它已經建立了後續工作的共同輸入。下一篇，我們會準備一批測試語料，並討論文件要如何切成適合搜尋的 Chunk：一個 Chunk 應該保留多少上下文，才能讓之後的關鍵字搜尋找到真正有用的內容。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10402198)

[

Day 4｜建立知識庫：從 Markdown 文件到可處理資料

](https://ithelp.ithome.com.tw/articles/10402198)[下一篇](https://ithelp.ithome.com.tw/articles/10402481)

[

Day 6｜Chunk 怎麼切才合理？文件切分策略比較

](https://ithelp.ithome.com.tw/articles/10402481)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/26/6975ca4f-767f-46c4-a9e3-6c4198204dc5_small.png)

[從混亂到有效治理：Policy as Code 在跨團隊 CI/CD 的實踐與應用](https://itplus.ithome.com.tw/vod-page/1177?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/613c15de-b530-4ab1-96ea-bc0b4be94442_small.jpg)

[解鎖 AI 功能與生產力｜Dell 與 Windows 11 Pro 軟硬搭配](https://itplus.ithome.com.tw/vod-page/1252?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

IT EXPLAINED |

45 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/b5c33eaa-9086-4e86-9382-a01910a707db_small.png)

[我們與「敏捷」的距離](https://itplus.ithome.com.tw/vod-page/1360?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

48 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/786a51f4-a5d4-4cbe-af56-284e2e9802ad_small.png)

[走出混沌的DevOps時代: 智慧協作](https://itplus.ithome.com.tw/vod-page/1491?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

46 分