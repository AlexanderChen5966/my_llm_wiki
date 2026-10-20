---
title: "如何建立自己的 LLM 知識庫（LLM Knowledge Bases） | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3861"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-04-05
created: 2026-05-18
description: "Andrej Karpathy 分享以 LLM 建構知識庫的新工作流：將多來源資料轉為 Markdown wiki、由 AI 自動摘要、連結與優化，結合 Obsidian 實現高效搜尋、問答與知識累積，打造可持續進化的 AI 知識系統。"
tags:
  - "clippings"
status: processed
---
前 Andrej Karpathy（前 OpenAI 聯合創始人、前特斯拉 AI 總監），作為 AI 圈公認的頂級專家，近期分享了一個相當值得關注的觀點。他的一則 [推文](https://x.com/karpathy/status/2039805659525644595) 獲得超過 960 萬瀏覽、3.7 萬按讚、6.8 萬收藏，引發廣泛討論。

他提到，自己近期的工作重心已經從「寫程式」轉向「編知識庫（building knowledge bases）」——這不僅是一種工作方式的轉變，也代表了 LLM 應用的一個重要方向。

以下即是他所分享的實作方式與思考：

**資料匯入（Data ingest）：**  
我會先把來源文件（例如文章、論文、程式庫、資料集、圖片等）整理並索引到一個 `raw/` 目錄中，接著使用 LLM 逐步「編譯」成一個 wiki。這個 wiki 本質上就是一組有結構的 `.md` 檔案集合。

這個 wiki 會包含：

- 所有 `raw/` 資料的摘要
- 文件之間的反向連結（backlinks）
- 將資料分類成不同概念
- 為這些概念撰寫文章
- 並建立完整的交互連結

在將網頁文章轉換成 `.md` 檔時，我會使用 Obsidian Web Clipper 擴充功能；同時我也會透過快捷鍵下載所有相關圖片到本地，讓 LLM 可以更方便地引用。

**IDE（開發環境）：**  
我使用 Obsidian 作為「前端 IDE」，用來瀏覽原始資料、已編譯的 wiki，以及衍生出的視覺化內容。

值得注意的是：  
整個 wiki 的內容幾乎都是由 LLM 負責撰寫與維護，我很少直接手動修改。

我也嘗試過一些 Obsidian 插件來用不同方式呈現資料（例如使用 Marp 製作投影片）。

**問答（Q&A）：**  
當你的 wiki 規模夠大時（例如我目前某些研究約有 100 篇文章、約 40 萬字），事情就開始變得很有趣。

你可以直接向 LLM agent 提出各種複雜問題，它會自行在 wiki 中查找資料、分析並回答。

我原本以為需要用到比較複雜的 RAG（檢索增強生成），但實際上 LLM 在這種「小規模」下，已經能很好地：

- 自動維護索引檔
- 為文件生成摘要
- 並快速讀取相關資料

**輸出（Output）：**  
我不太喜歡只在終端機或純文字中看答案，而是會讓 LLM 直接產出：

- Markdown 文件
- 投影片（Marp 格式）
- matplotlib 圖表

然後再回到 Obsidian 中查看。

根據不同問題，其實還可以延伸出更多視覺化輸出格式。  
而且我通常會把這些輸出再「歸檔」回 wiki，讓知識庫持續增強。

也就是說，我每一次的探索與提問，都會累積成知識資產。

**校驗（Linting）：**  
我也會讓 LLM 對整個 wiki 做「健康檢查」，例如：

- 找出不一致的資料
- 補齊缺失資訊（透過網路搜尋）
- 發現有趣的關聯（作為新文章的候選）

逐步優化 wiki 的結構與資料品質。

LLM 在「提出下一步應該研究什麼問題」這件事上也表現得相當好。

**額外工具（Extra tools）：**  
我也會開發一些輔助工具來處理資料，例如：

我用 vibe coding（直覺式開發）做了一個簡單的搜尋引擎，可以：

- 直接透過 Web UI 使用
- 或透過 CLI 提供給 LLM 當工具使用（用於更大型查詢）

**進一步探索（Further explorations）：**  
隨著資料庫成長，自然會開始思考：

- 使用合成資料（synthetic data generation）
- 搭配微調（fine-tuning）

讓 LLM 不只是依賴 context window，而是能直接「把知識學進權重中」。

**總結（TL;DR）：**  
從多個來源收集原始資料 →  
由 LLM 編譯成 Markdown wiki →  
再透過各種 CLI 工具讓 LLM 進行問答與持續優化 →  
最後在 Obsidian 中統一瀏覽。

整個過程中，你幾乎不需要手動編輯 wiki，因為那是 LLM 負責的領域。