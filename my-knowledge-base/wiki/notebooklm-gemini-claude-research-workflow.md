---
title: NotebookLM / Gemini / Claude 三 AI 分工術：券商分析師用 Markdown 接力的產業研究工作流
date: 2026-07-24
tags: [ai/workflow, pkm/workflow, type/how-to]
source: https://www.bnext.com.tw/article/91590/notebooklm-gemini-claude-ai-research-workflow
type: article
status: processed
asset_value: 64
---

前券商分析師 Freddy 把 NotebookLM（現名 Gemini Notebook）、Gemini、Claude 串成一條研究流水線，各司其職。**串連關鍵是統一輸出標準化 Markdown 文件並存進 Google Drive**，讓三個 AI 接力讀檔，把「靠人腦記憶 + Windows 資料夾 + Word 檔」的舊習升級為靈活的「專屬產業大腦」。

## T（核心論點）

多 AI 協作的價值不在單一模型多強，而在**用統一的中介格式（Markdown）+ 共用儲存層（Google Drive）讓工具接力**。每個 AI 負責它最擅長的一段（歸納 / 橫向比較 / 產出），前一段的輸出成為後一段的輸入，資料因此不再被個別檔案與人腦記憶困住。

## B（適用場景）

**適用：** 需要長期追蹤、跨來源比對的研究型任務（產業分析、競品追蹤、學術文獻）；資料量大、需可驗證引用。
**不適用 / 注意：** 依賴三個工具都能連 Google Drive；Markdown 標準化需事先規劃好格式，否則接力會斷。

## R（四段流水線）

1. **NotebookLM（Gemini Notebook）＝原始記憶庫**：一家公司一個筆記本。把該公司多年的法說會逐字稿、財報、訪談筆記丟進去讓 AI 先歸納（基於真實來源、不怕失憶），要求輸出成 **Markdown 格式**的財務概況、策略動向，存進 Google Drive。
2. **Gemini＝產業橫向比較庫**：建立產業層級的「筆記本」研究專案，連結 Google Drive 取得上一步的公司 Markdown 檔。結合連網搜尋與 YouTube 影音解讀，即時比對「公司承諾 vs 市場動態」，並與雲端裡已研究過的其他公司文件做橫向對比。
3. **Claude＝互動式產出層**：Gemini 分析成果同樣存成 Markdown，換 Claude 連 Google Drive 讀檔。它能提議新分析框架，並把估值模型寫成**互動式網頁**——客戶不用看 Excel，拉動滑桿改營運指標就能看到估值變化。
4. **串連關鍵＝標準化 + 雲端**：三個 AI 能協作的主因是都能連 Google Drive，且從第一步就統一輸出格式一致的 Markdown，後續工具才能接力讀檔。

## C（關鍵行動）

- [ ] 檢視自己的研究任務能否套用「一主題一筆記本 → 橫向比較 → 互動式產出」三段式
- [ ] 把 Markdown 當成跨 AI 工具的通用中介格式（呼應本知識庫 wiki 全用 .md 累積知識的理由）
- [ ] 儲存層選能被多個 AI 連接的雲端（Google Drive / MCP），讓工具接力而非各自為政
- [ ] 對照本知識庫的 Markdown-first 做法：這是「人腦記憶 → 外部大腦」的實際案例

## 補充資料｜Expert Intelligence：正版電子書當資料來源（2026-09-01 併入）

來源：https://www.bnext.com.tw/article/92042（官方公告 2026-08-27）。Gemini Notebook 新增 **Expert Intelligence**，首波支援**逾 10 萬本** Google Play 圖書電子書直接匯入當資料來源。

- 🔴 **版權門檻是硬的**：必須在 Google Play 圖書**真正擁有該書**才能互動；筆記本分享給協作者時，未購書者該書內容呈現**不可用狀態**
- 匯入後可提問，也能把整本書轉成資訊圖表、語音摘要或測驗題
- 🔑 **最大亮點是「資料混用」**：把管理學著作與自己的情境一起發想（官方例：主管苦惱如何給員工回饋）；或拍下冰箱食材搭配飲食書規劃一週菜單——**書從「讀完束之高閣」變成可反覆調用、與個人資料交互參照的知識庫**
- 合作出版社：企鵝蘭登、麥克米倫、Bloomsbury、O'Reilly Media、De Gruyter Brill、約翰霍普金斯大學出版社；另邀 15 位以上作家打造「精選筆記本」（含原書外的延伸素材）
- 確認手中書籍是否支援：Google Play 圖書詳細資料頁看 **「Tools」徽章**是否含 Gemini Notebook
- ⚠️ 官方定位為**跨產品線長期計畫**，未來延伸至 Gemini App 與搜尋 AI Mode，來源將擴展至第三方訂閱內容、商業研究報告與專業教科書
- ⚠️ 美國限時贈書活動（促銷碼 `EXPERTINTELLIGENCE`）**台灣尚未開放**

## 原始摘要

數位時代《一天一AI》專欄，受訪者 Freddy（前券商分析師）。NotebookLM 上週改名 Gemini Notebook。核心系統「MD 檔 + 雲端 + AI 專案」解放被檔案困住的資料，升級為「專屬產業大腦」。（原文含亞馬遜等業配段，已忽略。）

## 相關連結
- [[chatgpt-deep-research-guide]] — ChatGPT Deep Research 的 5 步驟與 5 要素提示詞範本，可當本流水線的另一個研究入口
- [[notebooklm-skill-research-pipeline]] — 把此類研究流程自動化的開源工具（notebooklm-skill），本篇為「人工方法論」對照
- [[google-workspace-notebooklm-gemini-admin-workflow]] — NotebookLM/Gemini 在 Google Workspace 的行政工作流
- [[synthesis-llm-cost-routing]] — 多工具分工的本質是任務路由（弱規劃者+強執行者）
- [[claude-personal-data-dashboard-case-study]] — Claude 把資料寫成互動式產出的另一案例
- [[karpathy-knowledge-base-method]] — 人策展 / AI 整理的分工哲學
