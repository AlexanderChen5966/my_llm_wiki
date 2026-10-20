---
title: PDF 轉 Markdown 三工具分流：先看有沒有文字層
date: 2026-08-12
tags: [pkm/workflow, ai/workflow]
source: https://www.bnext.com.tw/article/91732/pdf-markdown-tools
type: article
status: processed
asset_value: 60
---

## T（核心論點）

把 PDF 財報丟給 AI，表格數字全糊在一起——**這不是 AI 判讀失敗**：

> **PDF 格式本身只記錄文字印在哪個座標，不保存表格結構。**

正解是先把 PDF 轉成**帶結構標籤的 Markdown**，再交給 AI。而選工具的第一個、也是唯一關鍵的判準是：**這份 PDF 有沒有可選取的文字層。**

## B（適用場景）

**適用：**
- 本知識庫的 `raw/` 常有 PDF，`CLAUDE.md` 明訂「PDF >10 頁需指定 pages」——轉成 Markdown 後可一次讀完並保留結構
- 財報、規格書、掃描件、Office 檔要餵給 AI 之前的前處理

**不適用 / 注意：**
- 🔴 **台灣上市公司財報常是「混合型」**：前面文字說明有文字層，經會計師簽章的四大報表往往是掃描影像。**未逐頁確認，轉換後關鍵數據區塊會呈現空白**
- 🔴 **市面上沒有「免安裝又能處理掃描件」的免費工具**——能在 Web 端直接跑的都不支援 OCR，內建 OCR 的都要本機部署
- 三款都是 MIT 授權，免費、無用量限制、不需 API 金鑰

## R（操作步驟）

### 第 0 步：判斷 PDF 類型（用滑鼠反白測試）

用瀏覽器或 PDF 軟體開啟，**在表格區域**拖曳反白：
- 文字可順利反白 → **文字型**（有原生文字層）
- 只出現藍色選取方框、文字無法單獨標記 → **掃描型**（該頁是圖檔）
- ⚠️ **記得逐頁測試，不要只測封面**

### 三工具比較

| 比較面向 | pdf-inspector | MarkItDown | Docling |
|---|---|---|---|
| 開發機構 | Firecrawl | 微軟 | IBM Research |
| 免安裝網頁版 | ✅ 支援 | ❌ | ❌ |
| 內建 OCR | ❌ | ❌ | ✅ |
| 支援格式 | 僅 PDF | PDF、Office 生態系、電子郵件 | PDF、Office、影像圖檔 |
| 環境需求 | 無（瀏覽器；另有開源套件） | 基礎 Python | 需安裝機器學習模型套件 |
| 最佳情境 | **具備文字層的 PDF** | **Word / Excel / PPT 轉檔** | **掃描影像件、多欄複雜版面** |

**分流順序**：先丟 pdf-inspector 網頁版讓它自動判定 → 文字型直接轉完收工；判定掃描型或混合型 → 改用 Docling 做 OCR；原始檔是 Office 格式 → 交給 MarkItDown。

### 工具一：pdf-inspector（Firecrawl，Rust）

網址 `firecrawl.github.io/pdf-inspector/`，不用註冊，瀏覽器內運算（檢視原始碼可確認沒有夾帶檔案外流的網路呼叫）。
拖檔進去後會顯示 Markdown 原始碼，並回報屬性：**text-based**（可直接用）／**scanned**（換工具）／**mixed**（可用，但影像頁面輸出空白）／**image-based**（換工具）。
⚠️ 網頁版有 **25MB 上限**，且引擎版本略舊於套件版。

### 工具二：MarkItDown（微軟）

```bash
pip install 'markitdown[all]'     # 中括號不可省略，否則不裝 PDF 解析元件
                                  # Windows PowerShell 報錯請改雙引號 "[all]"
markitdown --help
markitdown /你的路徑/財報.pdf -o 財報.md
markitdown 簡報.pptx -o 簡報.md   # 真正的價值在 PDF 以外
```
不具備 OCR，遇掃描型文件會回傳空白。

### 工具三：Docling（IBM Research）

```bash
pip install 'docling[easyocr]'    # 必須明確指定 OCR 引擎群組，否則預設不裝
docling --version
docling convert /你的路徑/財報.pdf --to md --output ./
# 繁體中文掃描件（預設語言不含繁中，不指定會亂碼）
docling convert 掃描檔.pdf --ocr-engine easyocr --ocr-lang ch_tra --to md --output ./
```
⚠️ 版面分析 + OCR 兩組模型同時載入，**記憶體需求偏高**；記憶體少的電腦處理整頁掃描件可能**中途停止且不顯示錯誤訊息**。此時改用輕量的 `--ocr-engine tesseract --ocr-lang chi_tra`（需另裝 Tesseract 與繁中語言包）。
無 GPU 只靠 CPU 時，單頁掃描件可能耗時數十秒至數分鐘。

### 轉檔後的三個必做檢查

1. **跨頁的表格**——欄列是依座標推斷，從前一頁延續到下一頁時最容易錯位
2. **合併儲存格的表頭**——財報常見兩層表頭，確認數據對齊正確的年度欄位
3. **恆等式驗算**——例如「流動資產＋非流動資產＝資產總計」，抓出難以察覺的錯誤

### 🔑 交給 AI 時的提示詞範例

```
以下是一份台灣上市公司財報轉成的 Markdown。金額單位為新台幣仟元，年度為民國年。
回答時請標明每個數字來自哪一列、哪個年度欄位；
表格中找不到的數字，請直接說找不到，絕對不要推算。
```

> **「不要推算」這句是重點**——避免 AI 遇到轉檔缺漏時自行利用鄰近數字產生幻覺。

## C（關鍵行動）

- [ ] 收到 PDF 素材先做反白測試判斷類型（逐頁，別只測封面）
- [ ] 混合型財報：確認掃描頁的位置，那幾頁單獨走 Docling
- [ ] Office 檔（Word/Excel/PPT）不要先轉 PDF，直接用 MarkItDown
- [ ] 轉完跑三個檢查（跨頁表格／合併表頭／恆等式驗算）再餵給 AI
- [ ] 提示詞一律加「找不到就說找不到，絕對不要推算」
- [ ] 🔬 可試：把 `raw/` 裡的 PDF 素材改為「先轉 Markdown 再進 pipeline」，省去 `pages` 分段讀取的麻煩

## 原始摘要

三款工具皆為 MIT 授權。隱私面：pdf-inspector 網頁版在瀏覽器內運算；MarkItDown 與 Docling 皆本機離線執行（Docling 首次安裝需連網下載模型），套件內無回傳使用紀錄的遙測。實務限制：表格結構全靠推斷；OCR 可能出現中文字元誤判、千分位錯亂、數字位數遺失，且**錯誤外觀正常不易察覺**，掃描件轉出的財務數字務必逐格對照原始檔。開源工具改版頻繁，指令以 2026 年 8 月為準，執行前建議用 `--help` 確認參數。

## 相關連結

- [[markitdown-document-conversion]] — MarkItDown 的完整條目（明確目標是餵給 LLM 與文字分析流程，不是給人看的逐字完美輸出）
- [[olmocr-pdf-to-markdown]] — 另一條路線：7B VLM 理解整頁版面而非逐字辨識，olmOCR-Bench 82.4；與本篇 Docling 同屬「需本機部署的 OCR 方案」
- [[multimodal-document-processing-three-layers]] — 🔑 **上位視角**：本篇解決的是「文字抽取」，該篇指出 **OCR 抽取的是文字，不是語意關係**——流程圖 OCR 完箭頭方向與條件分支全部消失，需要視覺摘要結構化 JSON 才補得回來
- [[stirling-pdf-toolkit]] — PDF 前處理工具箱（拆頁、合併、去密碼）
- [[firecrawl-web-scraping-for-agents]] — pdf-inspector 的開發團隊 Firecrawl，其網頁轉 Markdown 產品線
- [[obsidian-web-clipper]] — 另一種素材入庫路徑（網頁 → Markdown）
- [[rag-retrieval-augmented-generation]] — Parse 階段的九種難處；本篇是其中「掃描型 PDF」與「表格」兩項的免費工具解
- [[notebooklm-gemini-claude-research-workflow]] — 轉出的 Markdown 接力給多個 AI 的下游工作流
