---
title: "MarkItDown：Microsoft 用於將幾乎任何內容轉換為 Markdown 的工具"
source: "https://codelove.tw/@tony/post/ayodwx"
author:
published:
created: 2026-06-26
description: "如果你曾經在打造由 LLM 驅動的應用程式，你很可能也遇過同樣的問題：你的資料散落在 PDF、Word 文件、Excel 試算表和 PowerPoint 簡報裡——但你的 AI 流程期待的是乾淨的文字。複製貼上無法規模化，而大多數轉換工具不是會刪掉太多結構，就是輸出雜亂的內容。Microsoft..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月26日](https://codelove.tw/@tony/post/ayodwx "2026-06-26 13:15:07")

## MarkItDown：Microsoft 用於將幾乎任何內容轉換為 Markdown 的工具

如果你曾經在打造由 LLM 驅動的應用程式，你很可能也遇過同樣的問題：你的資料散落在 PDF、Word 文件、Excel 試算表和 PowerPoint 簡報裡——但你的 AI 流程期待的是乾淨的文字。複製貼上無法規模化，而大多數轉換工具不是會刪掉太多結構，就是輸出雜亂的內容。

Microsoft 的 **MarkItDown** 正是為了解決這個落差而設計。它是一個輕量級的 Python 工具，可將多種檔案格式轉換為 Markdown，並保留重要的結構：標題、表格、清單和連結。

---

## MarkItDown 是什麼？

MarkItDown 是一個 Python 函式庫（同時也是 CLI 工具），可將檔案與文件轉換成 Markdown。它不是為了產生像人類閱讀般逐字完美的輸出而設計。它的明確目標是把文字餵給 LLM 與文字分析流程——而 Markdown 正是適合這件事的格式，因為大多數大型語言模型都能原生理解 Markdown，而且它的 token 效率很高。

支援的格式包括：

- PDF
- Word（.docx）
- PowerPoint（.pptx）
- Excel（.xlsx 與舊版.xls）
- 圖片（EXIF 中繼資料 + 可選 OCR）
- 音訊檔（EXIF 中繼資料 + 可選語音轉錄）
- HTML
- CSV、JSON、XML
- ZIP 檔（會逐一遍歷並轉換內容）
- YouTube 網址（擷取逐字稿）
- EPUB

對於單一函式庫來說，這個涵蓋範圍相當廣。

---

## 安裝

你需要 Python 3.10 以上版本。最簡單的安裝方式如下：

```bash
pip install 'markitdown[all]'
```

`[all]` 旗標會安裝所有支援格式所需的選用相依套件。如果你想要更精簡的安裝，也可以只選特定格式：

```bash
pip install 'markitdown[pdf,docx,pptx]'
```

可用的選用擴充套件有： `pdf` 、 `docx` 、 `pptx` 、 `xlsx` 、 `xls` 、 `outlook` 、 `audio-transcription` 、 `youtube-transcription` 、 `az-doc-intel` 。

建議在虛擬環境中使用：

```bash
python -m venv .venv
source .venv/bin/activate
pip install 'markitdown[all]'
```

---

## 使用 CLI

命令列介面非常直接：

```bash
# 轉換檔案並輸出到標準輸出
markitdown report.pdf

# 將輸出儲存到檔案
markitdown report.pdf -o report.md

# 透過管道輸入
cat report.pdf | markitdown
```

就是這樣。基本使用不需要任何設定。

---

## 使用 Python API

如果你要在流程中以程式方式使用：

```python
from markitdown import MarkItDown

md = MarkItDown(enable_plugins=False)
result = md.convert("financials.xlsx")
print(result.text_content)
```

`result.text_content` 屬性會包含轉換後的 Markdown 字串。

### 轉換不同檔案類型

```python
from markitdown import MarkItDown

md = MarkItDown()

# Word 文件
result = md.convert("proposal.docx")

# PowerPoint 簡報
result = md.convert("slides.pptx")

# CSV 檔
result = md.convert("data.csv")

# HTML 檔
result = md.convert("page.html")

print(result.text_content)
```

不論檔案類型為何，API 都是一致的。你只要呼叫 `.convert()` ，就會得到一個結果物件。

---

## 使用 LLM 產生圖片描述

如果你傳入圖片檔（或含有圖片的 PowerPoint），MarkItDown 可以呼叫 LLM 為這些圖片產生描述，並將其納入 Markdown 輸出。你需要自行提供用戶端：

```python
from markitdown import MarkItDown
from openai import OpenAI

client = OpenAI()
md = MarkItDown(llm_client=client, llm_model="gpt-4o")

result = md.convert("diagram.jpg")
print(result.text_content)
```

當圖片的實際視覺內容對下游處理很重要，而不只是檔案中繼資料時，這個功能就很有用。

---

## 透過外掛支援 OCR

對於包含內嵌文字影像的 PDF 與 Office 文件（例如掃描文件、投影片中的截圖），MarkItDown 支援獨立的 OCR 外掛：

```bash
pip install markitdown-ocr
pip install openai
```
```python
from markitdown import MarkItDown
from openai import OpenAI

md = MarkItDown(
    enable_plugins=True,
    llm_client=OpenAI(),
    llm_model="gpt-4o",
)
result = md.convert("scanned_report.pdf")
print(result.text_content)
```

OCR 外掛使用與圖片描述相同的 LLM 視覺模式——不需要額外的機器學習函式庫或二進位檔。

---

## Azure 文件智慧分析

若要進行企業級文件解析（例如更好的表格擷取、表單辨識），MarkItDown 可整合 Azure 文件智慧分析（Azure Document Intelligence）：

```bash
# CLI
markitdown report.pdf -o report.md -d -e "<your_endpoint>"
```
```python
from markitdown import MarkItDown

md = MarkItDown(docintel_endpoint="<your_endpoint>")
result = md.convert("complex_form.pdf")
print(result.text_content)
```

如果你要處理複雜的財務文件、法律合約或表單，而且結構準確性很重要，這會是比較合適的路徑。

---

## 使用 Docker 執行

如果你偏好容器化工作流程：

```bash
docker build -t markitdown:latest .
docker run --rm -i markitdown:latest < your-file.pdf > output.md
```

---

## 外掛生態系

MarkItDown 支援第三方外掛，預設為停用。

```bash
# 列出已安裝的外掛
markitdown --list-plugins

# 在轉換時啟用外掛
markitdown --use-plugins path-to-file.pdf
```

若要尋找社群外掛，可以在 GitHub 上搜尋 `#markitdown-plugin` 。

---

## 安全性考量

在把它整合進伺服器端應用程式之前，有一件事值得注意：MarkItDown 會以目前程序的權限執行。它可以像 `open()` 或 `requests.get()` 一樣存取本機檔案與遠端 URI。

專案建議不要直接將不受信任的輸入傳給 `.convert()` 。如果你只需要轉換本機檔案，請使用 `convert_local()` 。如果你需要處理串流，請使用 `convert_stream()` 。針對你的使用情境，應盡量選擇最小範圍的 API。

這對任何檔案處理函式庫來說都是標準建議，但如果你正在打造面向網頁的功能，這一點尤其值得明確提醒。

---

## 值得使用嗎？

老實說，要看你的需求。

**如果符合以下情況，MarkItDown 很適合你：**

- 你正在打造需要匯入多種格式文件的 LLM 流程。
- 你希望在 PDF、Word、Excel、HTML 和其他格式之間維持一致的 Python API，而不必拼接多個函式庫。
- 你需要一個可快速批次轉換檔案、用於索引或 embedding 的 CLI 工具。
- 你希望能透過外掛擴充轉換行為。

**如果符合以下情況，MarkItDown 不是最合適的工具：**

- 你需要給人類觀看的像素級精準轉換。專案文件明確指出，輸出是為文字分析工具設計，不是為高保真文件呈現而設計。
- 你需要不依賴 LLM 的正式 OCR。OCR 外掛需要相容 OpenAI 的用戶端，這會增加延遲與成本。
- 你處理的是高度格式化的文件，而版面配置的重要性超過標題與表格（例如多欄學術論文、複雜的發票版面）。

---

## 快速參考

| 工作 | 指令 |
| --- | --- |
| 安裝所有格式 | `pip install 'markitdown[all]'` |
| 透過 CLI 轉換 | `markitdown file.pdf -o output.md` |
| 透過 Python 轉換 | `MarkItDown().convert("file.pdf").text_content` |
| 以 LLM 處理圖片 | 將 `llm_client` 與 `llm_model` 傳給 `MarkItDown()` |
| 啟用 OCR 外掛 | `pip install markitdown-ocr` ，然後設定 `enable_plugins=True` |
| 使用 Azure 文件智慧分析 | 將 `docintel_endpoint` 傳給 `MarkItDown()` |
| 透過 Docker 執行 | `docker run --rm -i markitdown:latest < file.pdf > output.md` |

---

**GitHub:** [https://github.com/microsoft/markitdown](https://github.com/microsoft/markitdown)

---

原文出處： [https://dev.to/arshtechpro/markitdown-microsofts-tool-for-converting-almost-anything-to-markdown-5hf5](https://dev.to/arshtechpro/markitdown-microsofts-tool-for-converting-almost-anything-to-markdown-5hf5)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/ayodwx) [5瀏覽](https://codelove.tw/@tony/post/ayodwx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/ayodwx)

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

📝13 💬2 ❤️1

641

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

69

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782462677&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fayodwx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782462677026&amp;bpp=2&amp;bdt=137&amp;idt=20&amp;shv=r20260625&amp;mjsv=m202606180101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782444306%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782444306%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782444306%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5811355423121&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1605&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099345%2C95394760&amp;oid=2&amp;pvsid=4031188236118782&amp;tmod=1647572126&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1605%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=50" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260625/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>