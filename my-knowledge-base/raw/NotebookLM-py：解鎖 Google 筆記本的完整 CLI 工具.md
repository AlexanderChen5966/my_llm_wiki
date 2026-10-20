---
title: "NotebookLM-py：解鎖 Google 筆記本的完整 CLI 工具"
source: "https://tenten.co/learning/notebooklm-py-skill/"
author:
  - "[[Skye]]"
published: 2026-01-16
created: 2026-05-15
description: "Google NotebookLM 沒有 API？別擔心！NotebookLM-py 讓你能用 Python 直接控制它。自動上傳、查詢、生成 Podcast，這篇教學讓你成為 AI 筆記大師。"
tags:
  - "clippings"
status: processed
---
Google NotebookLM 自 2023 年推出以來，憑藉其 RAG（檢索增強生成）架構，成為研究者與內容創作者的重要工具。然而，這款產品始終存在一個痛點：核心功能如閃卡、腦圖、報告等內容無法直接匯出，而 NotebookLM 解析後的原始文本（YouTube 字幕、PDF 內容、網頁搜尋結果）更是完全鎖在介面內。

開發者 teng-lin 於 2025 年 1 月發布的 [notebooklm-py](https://github.com/teng-lin/notebooklm-py) 專案，正是為解決這些限制而生。這款 Python CLI 工具透過逆向工程 NotebookLM 的未公開 API，讓使用者能夠完整存取所有功能，並將其轉換為可程式化的工作流程。

---

#### 核心功能：補足 NotebookLM 網頁版的缺口

notebooklm-py 的設計理念相當務實。網頁版 NotebookLM 雖然提供豐富的生成功能，但輸出選項極為受限。以閃卡為例，網頁版僅支援透過連結分享，無法下載為 Markdown 或 Anki 格式。腦圖只能匯出為 PNG 圖片，喪失了後續編輯的彈性。

這款 CLI 工具打破了這些限制。使用者可以透過簡單的指令，將閃卡下載為結構化文字檔，腦圖匯出為可編輯的 Markdown 格式，報告則能完整保留其原始結構。這對於需要將 NotebookLM 整合進現有工作流程的專業人士而言，解決了長期存在的痛點。

更值得注意的是「解析文本擷取」功能。NotebookLM 在處理 YouTube 影片時會自動擷取字幕，解析 PDF 時會進行智能文字識別，處理網頁時會過濾無關元素。這些經過處理的乾淨文本，原本只存在於 NotebookLM 的內部系統中。notebooklm-py 讓使用者能夠直接取得這些已處理的內容，省去自行使用 yt-dlp 或 PDF 解析工具的麻煩。

---

#### 技術架構：為何這款工具值得信賴

notebooklm-py 建立在兩個重要的技術基礎上。

首先是 RAG 架構帶來的低幻覺特性。與一般 [AI Agent](https://tenten.co/learning/ai-agent-year/) 不同，NotebookLM 的回答完全基於使用者上傳的文件，不會混入外部知識。這意味著從 notebooklm-py 取得的答案，每一句都能追溯到具體的來源段落。對於學術研究或企業文件分析而言，這種可驗證性至關重要。

其次是瀏覽器自動化技術的成熟。notebooklm-py 使用 Playwright 進行瀏覽器自動化，模擬真實使用者的操作模式，包括打字速度、滑鼠移動軌跡等細節。這種做法雖然比直接呼叫 API 複雜，但能夠繞過 Google 對自動化工具的偵測機制。

從系統需求來看，安裝過程相對簡單：

```bash
pip install notebooklm-py
pip install "notebooklm-py[browser]"
playwright install chromium
```

完成安裝後，透過 `notebooklm login` 進行一次性的 Google 帳號認證，後續操作就無需重複登入。

---

#### 三種使用情境：從個人到企業

notebooklm-py 支援三種主要的使用模式，適應不同場景的需求。

**CLI 指令模式** 適合快速任務與 Shell 腳本整合。例如，研究者可以在終端機中直接執行：

```bash
notebooklm create "Q1 市場分析"
notebooklm source add "https://example.com/report.pdf"
notebooklm generate audio --wait
notebooklm download audio ./podcast.mp3
```

這種工作流程能夠在數分鐘內，將一份 PDF 報告轉換為 AI 生成的 Podcast，完全無需開啟瀏覽器。

**Python API** 則提供更細緻的控制能力。開發者可以將 NotebookLM 整合進現有的 Python 應用程式，建立自動化的研究管線。異步架構（asyncio）確保了高效能的批次處理能力。

**Claude Code Skills** 是最具前瞻性的整合方式。透過安裝 notebooklm skill， [Claude Code](https://tenten.co/learning/claude-code-master-tips/) 能夠直接與 NotebookLM 對話，實現自然語言驅動的文件查詢。使用者只需要說「幫我查一下上週會議紀錄中提到的預算數字」，Claude 就會自動連接 NotebookLM、執行查詢、回傳答案，整個過程無需任何複製貼上。

這種 [MCP 協議](https://tenten.co/learning/chatgpt-claude-mcp/) 整合模式，代表了 AI 工具互聯的新趨勢。NotebookLM 作為知識庫，Claude Code 作為執行引擎，兩者結合形成了強大的研究助手。

---

#### 功能矩陣：完整能力一覽

| 功能類別 | 支援內容 |
| --- | --- |
| 筆記本管理 | 建立、列表、重命名、刪除、分享 |
| 來源匯入 | URL、YouTube、PDF/TXT/MD/DOCX、Google Drive、貼上文字 |
| 互動對話 | 問答、對話歷史、自訂人設 |
| 內容生成 | 音訊 Podcast、影片、簡報、測驗、閃卡、報告、資訊圖表、腦圖 |
| 研究工具 | 網頁研究、Drive 研究、自動匯入來源 |
| 下載匯出 | 音訊、影片、簡報、資訊圖表、報告、腦圖、數據表、測驗、閃卡 |

這份功能清單幾乎涵蓋了 NotebookLM Plus 的所有能力。免費版使用者同樣可以使用大部分功能，差異主要在於生成次數與來源數量限制。

---

#### 實務應用案例

**案例一：學術文獻回顧**

研究生可以將數十篇 PDF 論文上傳至 NotebookLM，透過 notebooklm-py 批次處理：自動生成每篇論文的摘要報告，建立跨論文的主題腦圖，最後匯出為閃卡進行複習。這個原本需要數週的工作，可以壓縮到一天完成。

**案例二：企業知識庫建構**

企業可以將內部文件（合約範本、技術文檔、會議紀錄）整理進 NotebookLM，再透過 [Claude Code](https://tenten.co/learning/claude-code-2-guide/) 的 Skills 整合，讓員工能夠用自然語言查詢公司知識庫。這種做法比傳統的文件搜尋更直覺，且答案會附帶來源引用。

**案例三：內容創作者工作流**

[YouTube 創作者](https://tenten.co/learning/11-youtube-thumbnail-guide/) 可以將競爭對手的影片 URL 批次匯入，透過 notebooklm-py 擷取所有影片的字幕文本，再用 Claude 分析內容策略。這種跨平台的內容情報收集，過去需要手動處理，現在可以完全自動化。

---

#### 風險與限制

notebooklm-py 並非沒有風險。作為一款使用未公開 API 的第三方工具，它存在幾個需要注意的面向。

**API 穩定性** ：Google 可能在任何時候修改 NotebookLM 的內部介面，導致工具暫時失效。開發者已經在 GitHub 上建立了快速修復機制，但使用者仍需接受這種不確定性。

**帳號安全** ：雖然 notebooklm-py 不會儲存使用者的 Google 密碼（採用瀏覽器 Cookie 認證），但任何自動化工具都存在帳號被標記的風險。建議使用專用帳號進行測試。

**使用條款** ：Google 的服務條款通常禁止自動化存取。雖然目前沒有使用者因使用此類工具而被停權的案例，但這仍是潛在風險。

這款工具最適合用於個人研究、原型開發與內部工具整合。對於需要穩定 API 的生產環境，建議等待 Google 推出官方 API。

---

#### 延伸閱讀

對於想深入了解 AI 工具整合的讀者，以下資源可能有所幫助：

- [RAG 在電商的 AI 應用](https://tenten.co/learning/ai-rag-in-ecommerce/) ：了解 RAG 架構如何降低 AI 幻覺
- [AI Agent 與 MCP 協議在工業自動化的前景](https://tenten.co/learning/ai-agent-for-industrial-automation/) ：探索工具互聯的技術標準
- [Anthropic 經濟指數報告](https://tenten.co/learning/anthropic-economic-index/) ：了解 Claude 在企業中的應用現況
- [AI 程式碼工具終極指南](https://tenten.co/learning/ai-coding-tool/) ：比較各種 AI 開發工具的優劣

---

### 作者觀點

notebooklm-py 的出現，反映了當前 AI 工具生態的一個有趣現象：官方產品往往聚焦於消費者友好的介面設計，而忽略了專業使用者對程式化存取的需求。這種供需缺口，催生了大量的第三方整合工具。

從更宏觀的角度看，這款工具預示了 AI 工具互聯的未來。當 NotebookLM 能夠作為知識庫被其他 AI 代理（如 Claude Code）呼叫時，我們距離真正的「AI 工作流程自動化」又近了一步。這不僅是工具層面的進步，更是工作方式的根本性改變。

---

*作者：Tenten Research Team*

*如果您正在評估如何將 AI 工具整合進企業工作流程，或需要客製化的 AI 自動化解決方案，歡迎與* [*Tenten 團隊預約諮詢*](https://tenten.co/contact) *，我們擁有豐富的 Claude Code、MCP 協議與企業 AI 部署經驗。*