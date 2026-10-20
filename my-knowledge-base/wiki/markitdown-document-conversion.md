---
title: MarkItDown：Microsoft 將幾乎任何檔案轉成 Markdown 的工具
date: 2026-06-30
tags: [ai/workflow, pkm/automation, type/how-to]
source: https://codelove.tw/@tony/post/ayodwx
type: article
status: processed
asset_value: 64
---

MarkItDown 是 Microsoft 開源的輕量 Python 工具（兼 CLI），把 PDF / Word / Excel / PPT / 圖片 / 音訊 / HTML / EPUB 等格式轉成 Markdown，並保留標題、表格、清單、連結等重要結構。明確目標不是給人看的逐字完美輸出，而是**把文字餵給 LLM 與文字分析流程**——Markdown 對 LLM 原生友善且 token 效率高。

## T（核心論點）

LLM 應用的資料常散落在 PDF/Word/Excel/PPT，但流程需要乾淨文字；複製貼上無法規模化，多數轉換工具不是刪太多結構就是輸出雜亂。MarkItDown 用一致的 API（`.convert()`）跨格式產出結構保留的 Markdown，專為 LLM 預處理而生。

## B（背景與適用場景）

- 打造需匯入多格式文件的 LLM 流程，想要一致 Python API 不必拼接多函式庫
- 需要 CLI 批次轉檔做索引 / embedding
- 對本知識庫而言：可作為 raw/ 多格式素材 → Markdown 的前處理橋
- **不適合**：給人看的像素級精準轉換、不依賴 LLM 的正式 OCR、版面重於結構的多欄學術論文/複雜發票

## R（主要內容）

**支援格式**：PDF、Word(.docx)、PPT(.pptx)、Excel(.xlsx/.xls)、圖片（EXIF + 可選 OCR）、音訊（EXIF + 可選轉錄）、HTML、CSV/JSON/XML、ZIP（遞迴）、YouTube URL（抓逐字稿）、EPUB。

**安裝/使用**（需 Python 3.10+）：
```bash
pip install 'markitdown[all]'     # 或只裝 [pdf,docx,pptx]
markitdown report.pdf -o report.md
```
```python
from markitdown import MarkItDown
md = MarkItDown()
result = md.convert("financials.xlsx")
print(result.text_content)   # 跨格式 API 一致
```

**進階能力**：
- LLM 圖片描述：傳 `llm_client` + `llm_model`，為圖片/含圖 PPT 生成描述納入輸出
- OCR 外掛（`markitdown-ocr`）：用 LLM 視覺模式處理掃描 PDF，免額外 ML 函式庫
- Azure Document Intelligence：企業級表格/表單擷取
- Docker：`docker run --rm -i markitdown:latest < file.pdf > output.md`

**安全性**：以當前程序權限執行，可存取本機檔案與遠端 URI。不要把不受信任輸入直接丟 `.convert()`；只轉本機檔用 `convert_local()`、串流用 `convert_stream()`。

## C（對自身的啟發）

- [ ] 可作為 raw/ 中 Office/EPUB 等 Claude 不直接吃的格式 → Markdown 的前處理工具
- [ ] YouTube 逐字稿擷取功能，可補本知識庫影音素材入庫管道
- [ ] 與 Stirling PDF 對照：MarkItDown 偏「轉文字餵 LLM」，Stirling 偏「PDF 文件操作」

## 相關連結

- [[stirling-pdf-toolkit]] — Stirling PDF：PDF 工具整合平台
- [[notebooklm-py-cli-tool]] — NotebookLM CLI 工具
- [[obsidian-web-clipper]] — Web Clipper 網頁入庫
- [[meetily-ai-meeting-assistant]] — 會議逐字稿→摘要（可接入庫的上游）
- [[pdf-to-markdown-tools]] — 三工具分流指南：MarkItDown 定位在「Office 生態系轉檔」，**不具 OCR，遇掃描型 PDF 回傳空白**；掃描件要改用 Docling
