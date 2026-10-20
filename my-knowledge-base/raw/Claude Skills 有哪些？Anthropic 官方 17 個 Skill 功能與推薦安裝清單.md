---
title: "Claude Skills 有哪些？Anthropic 官方 17 個 Skill 功能與推薦安裝清單"
source: "https://www.bnext.com.tw/article/91307/anthropic-17-claude-skills-"
author:
  - "[[陳祈安]]"
published: 2026-06-23
created: 2026-06-24
description: "Anthropic公開17個官方Claude Skill，免費開源，涵蓋文件製作、品牌設計、工程開發、團隊溝通四大類。一文了解你適合安裝哪款Skill"
tags:
  - "clippings"
status: processed
---
你有沒有這樣的經驗：明明用了 Claude，但同樣的任務每次都要重新說明一遍，輸出品質也時好時壞？這正是 Skill 想解決的問題。Skill 是一個資料夾形式的指令包，Claude 載入之後，就能以更穩定、更符合你需求的方式完成特定類型的工作，而不需要每次都從頭給它脈絡。

Anthropic 官方在 GitHub 公開了 17 個可公開取用的 Skill，涵蓋文件製作、品牌設計、工程開發、溝通協作四大類，以及一個用來自製 Skill 的 Meta Skill。適用平台包括 [claude.ai](https://claude.ai/new) （免費與付費方案皆可）、Claude Code 以及 Claude API，不同平台的安裝與可用範圍略有差異。

> 前往 Anthropic [GitHub](https://github.com/anthropics/skills)

![anthropics github](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/i5uz-1782185272.png?w=1200&output=webp)

圖／ anthropics github

claude.ai 免費版也可使用 Skills，但需確認已啟用 Code execution and file creation；若要使用 GitHub 上的特定範例 Skill，可下載後以 ZIP 上傳。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

付費版 claude.ai 用戶則可於 Skill 資料庫使用並安裝，不需自行下載。Claude Code 使用者可透過指令安裝、API 使用者則透過 Skills API 啟用。

![anthropics github](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-06/g6rj-1782186734.png?w=1200&output=webp)

付費版 claude.ai 用戶則可於 Skill 資料庫中直接使用並安裝，不需自行下載。

圖／ claude

《未來商務》以下拆解這 17 個 Skill 在做什麼，快速找出最適合你安裝的類別及項目。可點擊以下目錄直接前往指定區段。

### 文章目錄

[類別一、文件製作類：報告、合約、簡報都能直接輸出](#1)  
[類別二、設計與品牌類：讓輸出有風格、有一致性](#2)  
[類別三、工程與開發類：讓 Claude 寫出更可靠的程式碼](#3)  
[類別四、溝通與內容類：對內對外都能維持一致語氣](#4)  
[類別五、Meta Skill：裝完這個，自己造 Skill](#5)  
[我應該從哪個 Skill 開始裝？](#6)

## 類別一、文件製作類：報告、合約、簡報都能直接輸出

| Skill | 功能 | 適合對象 |
| --- | --- | --- |
| pdf | 建立、填寫、編輯、擷取 PDF 內容 | 需要產出報告、發票、合約摘要的所有人 |
| docx | 產生與編輯.docx，支援樣式、表格、追蹤修訂 | 顧問、法務、專案管理，交付物格式是 Word 的情境 |
| xlsx | 建立與解析 Excel，保留公式與格式 | 財務分析師、營運團隊，需要 AI 處理公式與格式 |
| pptx | 產生可編輯的 PowerPoint，含文字、圖表、投影片母版 | 每週都在做簡報的業務、培訓、主管報告 |

這四個 Skill 構成一條完整的文件處理流程：從 PDF 擷取內容、在 Excel 分析、產出 Word 報告、最後轉成 PowerPoint 簡報，整條流程都有對應支援。

## 類別二、設計與品牌類：讓輸出有風格、有一致性

| Skill | 功能 | 適合對象 |
| --- | --- | --- |
| brand-guidelines | 將品牌色彩、字型、排版規則套用至所有 Claude 輸出 | 行銷團隊、品牌主、代理商 |
| canvas-design | 在 2D 畫布上排版，有圖層、字型、間距概念 | 個人操作者製作社群圖文、資訊圖表 |
| theme-factory | 產生完整配色、字型組合與元件風格 | 設計師快速迭代視覺識別 |
| algorithmic-art | 生成參數化與程序性藝術作品（基於 p5.js） | 創意開發者、生成藝術實驗 |

不論是前端介面、平面設計還是整體品牌規範，這組 Skill 解決的是「AI 輸出缺乏個性」的問題。四個 Skill 各有側重，但核心目標一致：讓 Claude 的輸出呈現出你要的風格，而不是千篇一律的預設樣式。

## 類別三、工程與開發類：讓 Claude 寫出更可靠的程式碼

| Skill | 功能 | 適合對象 |
| --- | --- | --- |
| claude-api | 教 Claude 正確使用 Anthropic API 的最新模式（串流輸出、工具呼叫、批次處理等） | 開發者串接 Anthropic API |
| mcp-builder | 協助生成 MCP Server，打通 AI 與內部系統 | 工程師建立 Agent 工作流 |
| frontend-design | 現代前端模式（React、Tailwind、無障礙設計） | 用 Claude Code 生成 UI |
| web-artifacts-builder | 產生複雜的單頁應用（含狀態管理、routing、shadcn/ui 元件） | 教育者、產品人製作 demo |
| webapp-testing | 用 Playwright 自動測試 Claude 生成的前端程式碼 | 工程師讓 AI 驗證自己的輸出 |

從 API 串接、MCP 伺服器建置、前端介面到自動化測試，這組 Skill 覆蓋開發工作中幾個最容易出錯的環節，幫 Claude 寫出能跑、能測、能上線的程式碼。

## 類別四、溝通與內容類：對內對外都能維持一致語氣

| Skill | 功能 | 適合對象 |
| --- | --- | --- |
| doc-coauthoring | 協作長篇文件，保留作者聲音，建議而非改寫 | 重視文風的寫作者 |
| internal-comms | 標準化全員公告、週報、狀態更新等格式 | 主管、創辦人、公關團隊 |
| slack-gif-creator | 生成適合 Slack 尺寸的動態 GIF | 想在 Slack 增添團隊文化感的人 |

這組 Skill 的共同邏輯是格式與語氣的一致性。不管是對外長篇文件、對內例行公告，還是 Slack 裡的一則動態貼圖，都在省去每次重新想格式和調語氣的時間。

## 類別五、Meta Skill：裝完這個，自己造 Skill

| Skill | 功能 | 適合對象 |
| --- | --- | --- |
| skill-creator | 引導你生成符合官方格式的自訂 Skill | 所有想標準化工作流程的團隊 |

這款是所有 Skill 中定位最特別的一個。載入後，Claude 會引導你逐步完成 Skill 的撰寫，包括 YAML frontmatter 的設計（特別是 description 欄位的措辭技巧，這是決定 Skill 是否會被正確觸發的關鍵）、指令結構、範例格式，以及用平行評估方式測量 Skill 效果的方法。

若只是想先體驗現成 Skill，可先從文件或工作場景相關 Skill 開始；若要自製 SOP，則可使用 skill-creator。

## 我應該從哪個 Skill 開始裝？

看完以上內容，如果你仍不確定該安裝哪個 Skill 的話，以下依「身份」推薦給你：

**・每週需要交付文件的商務用戶** → pdf＋docx＋xlsx＋pptx，這四個最實際，裝了就有感。

**・做內容與品牌的人** → brand-guidelines＋canvas-design＋doc-coauthoring，協助建立視覺與文字的一致性。

**・工程師** → claude-api＋mcp-builder＋frontend-design＋webapp-testing，從 API 串接到前端品質都有覆蓋。

**・管理團隊的人** → internal-comms＋brand-guidelines，標準化對內溝通是最快見效的起點。

**・用熟之後** → 安裝 skill-creator，開始把自己工作流程中反覆用到的步驟打包成自訂 Skill。

值得注意的是，Skill 並非裝越多越好。每個 Skill 啟動時，Claude 只讀取名稱與說明兩個欄位，完整指令要等到任務匹配後才載入。但安裝數量過多時，功能相近的 Skill 之間容易產生觸發競爭，Claude 可能在比對描述時啟動不該啟動的 Skill，反而影響準確度。我們建議，實務上可先 **找到適合自己情境的 5 到 8 個，比全部安裝更有實際效益；官方也建議避免同時啟用過多 Skill** 。

本文出自 [未來商務](https://fc.bnext.com.tw/articles/view/4707)