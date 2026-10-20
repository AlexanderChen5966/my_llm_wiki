---
title: "如何打造你的第二大腦 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3865"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-04-07
created: 2026-05-13
description: "Karpathy AI 第二大腦方法教學：用三個資料夾＋Schema 檔打造個人知識庫，結合 agent-browser 自動收集資料，讓 AI 將 raw 筆記整理成 wiki，持續優化並產出高品質洞察。"
tags:
  - "clippings"
status: processed
---
這是 [@NickSpisak\_](https://x.com/NickSpisak_/status/2040448463540830705) 整理的如何透過 AI 有系統地打造自己的第二大腦的方法，以下為完整的翻譯內容，給大家參考：

\--

@karpathy 發表了一篇文章，說明他如何使用 AI 建立個人知識庫。

這個概念很簡單：與其把筆記分散在各種應用程式中，不如把所有東西丟進同一個資料夾，然後讓 AI 幫你整理成一個個人維基（wiki）——包含摘要、關聯、文章，而且會隨著使用次數越來越聰明。

不需要特殊軟體，不需要資料庫，只需要資料夾與文字檔。

在 7 分鐘內你將學到：

- 建立完整資料夾結構（2 分鐘）
- 用一個 CLI 工具自動抓取網頁資料
- 一個讓整個系統運作的單一「schema 檔」
- 如何讓 AI 把原始筆記整理成 wiki
- 讓系統越用越聰明的複利技巧
- 一個能在錯誤累積前發現問題的健康檢查

### 1\. 建立三個資料夾（2 分鐘）

打開終端機或檔案總管，建立一個專案資料夾，並在裡面建立三個子資料夾：

my-knowledge-base/  
raw/ （原始資料：文章、筆記、截圖）  
wiki/ （AI 整理後的內容）  
outputs/ （AI 產生的回答與報告）

就這樣。這就是 @karpathy 使用的結構。

- raw/：雜亂的資料來源
- wiki/：AI 整理後的內容
- outputs/：問題的答案

不用安裝任何 App，不用註冊帳號，就三個資料夾。

### 2\. 填滿 raw 資料夾（10 分鐘）

大多數人會卡在這裡。

答案很簡單：全部都丟進去。

- 複製貼上文章（.md 或.txt）
- 儲存截圖或圖表
- 匯出筆記
- 放入會議紀錄、研究文件、專案資料
- 加入你收藏的書籤

**不要整理、不要改名、不要清理。**  
那是 AI 的工作。

我在 X 的內容流程中保留了 17 個原始來源檔案，但完全沒有手動整理。

但 Karpathy 沒提到一個關鍵：自動化收集。

### 3\. 用 Agent Browser 自動收集資料（可選但強大）

agent-browser 是 Vercel Labs 推出的 CLI 工具，可讓 AI 操控真實瀏覽器。

GitHub 超過 26K 星，安裝只要兩行：

npm install -g agent-browser  
agent-browser install

這會下載專用 Chrome，讓 AI 能：

- 打開網頁
- 抓取內容
- 直接存入 raw/

例如：

agent-browser open https://example.com  
agent-browser get text "article"

不需要手動複製貼上。

它甚至可以處理：

- JavaScript 動態頁面
- 需要登入的網站
- 需要點擊或滾動的內容

而且比 Playwright MCP 少用 82% tokens，可以抓更多資料。

### 4\. 撰寫 Schema 檔（5 分鐘）

建立一個檔案（如 CLAUDE.md）：

\# Knowledge Base Schema  
  
\## What This Is  
A personal knowledge base about \[YOUR TOPIC\].  
  
\## How It's Organized  
\- raw/ contains unprocessed source material. Never modify these files.  
\- wiki/ contains the organized wiki. AI maintains this entirely.  
\- outputs/ contains generated reports, answers, and analyses.  
  
\## Wiki Rules  
\- 每個主題一個.md  
\- 每篇開頭一段摘要  
\- 用 \[\[topic\]\] 做連結  
\- 維護 INDEX.md  
\- 新資料加入時更新 wiki  
  
\## My Interests  
\[列出 3–5 個重點主題\]

Karpathy 強調：  
**保持簡單、扁平化。**

這就是 AI 的「說明書」。

### 5\. 讓 AI 編譯你的 Wiki（15 分鐘）

對 AI 下指令：

> 讀取 raw/，依 CLAUDE.md 規則建立 wiki/

AI 會自動：

完成後，你會得到一個完整知識庫。

重點：  
**不要手動修改 wiki。讓 AI 維護。**

### 6\. 問問題並保存答案（持續進行）

例如：

- 我對某主題有哪些盲點？
- 比較兩個來源的差異
- 用 500 字整理某主題

AI 會基於你的知識庫回答。

再把答案存回 outputs/ 或 wiki/。

每次提問都會讓系統變更強

### 7\. 每月健康檢查

請 AI：

- 找出矛盾
- 找出未解釋的主題
- 找出沒有來源的內容
- 建議補充文章

因為錯誤會累積。

定期檢查可以避免偏差。

### 8\. 不一定要用 Obsidian

很多人推薦 Obsidian，像 Lex Fridman 也在用。

但 Karpathy 的做法是：

就是資料夾 +.md 檔

不需要複雜工具。

過多插件反而降低效率。

### 結論

整個系統只需要：

Karpathy 的文章有 4.1 萬人收藏。

但真正差別在於：

你是否真的動手做

選一個主題  
建立資料夾  
丟入資料  
讓 AI 開始運作

剩下的，交給系統。