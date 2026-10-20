---
title: Gemini Notebook 8組讀書指令：從摘要工具變成陪練家教
date: 2026-09-03
tags: [ai/workflow, pkm/workflow]
source: https://www.bnext.com.tw/article/92111/notebooklm-study-prompts-exam-prep
type: note
status: processed
asset_value: 36
---

Gemini Notebook（NotebookLM）預設用法多半停在「幫我整理重點」，本文整理 8 組換一種問法的讀書指令，把它從單向摘要工具變成會出題、會追問、會考你的陪讀助教。核心可轉用的做法：①先請它依主題產出讀書指南（含核心概念、常見誤解、附解答考題）畫出教材地圖；②要求「一題一題測驗＋每題給回饋」而非一次丟一整份題庫，逼近真人家教互動；③最後一步角色互換——換使用者口頭解釋概念，AI 依教材指出說對/漏掉/待補強之處，貼近費曼技巧。文中也提醒限制：預測考題只能反映教材本身重點排序，讀不到教師的口頭補充或偏好題型，仍需回頭核對原始教材。若要深入查原文完整 8 組指令逐字稿與使用限制，見 raw/ 原文。

## 補充資料：官方版「陪練家教」——互動式學習總覽（2026-09-23 併入）

來源：aiposthub〈Gemini Notebook 互動式學習總覽怎麼用？〉（2026-09-22）、《數位時代》〈Gemini Notebook新功能：可即時語音對話、錄音…〉（2026-09-18）。本篇的 8 組指令是使用者自己把摘要工具變成陪練家教；Interactive Learning Overviews（ILO）則是 Google 把同一件事做成官方功能。
- **時間線**：2026-09-15 Google 宣布四項學習功能，ILO 於 09-21 開放所有使用者。另外三項：即時語音對話（行動版，首週僅限 18 歲以上的 AI Ultra 訂戶）、行動版錄音直接成為 Source、約 60 秒的 Short Video Overviews
- **路徑與限制**：Studio → Reports → 格式選 Interactive → Template 選 Learning Overview；僅網頁版可建立；部分內容要求年滿 18 歲；會消耗額度
- 🔑 **可重用論點**：AI 生成的完整報告會製造「掌握感」，要先作答再回查來源。作者建議的 25 分鐘流程：5 分看主線 → 8 分關掉答案自己解釋 → 8 分做測驗（錯了回查來源）→ 4 分把錯題分成「名詞不熟／因果不懂／公式不會用／來源有衝突」，再請 Chat 針對其中一類出新題
- 🔴 **分享報告連結＝分享整本 Notebook**：收件者會拿到完整來源，分享前要檢查個資與付費教材
- 作者引用 PubMed 33683913（測驗式學習統合分析）支持「主動作答」這個方法，但作者也明說這**不能推論成這個功能本身能提升成績**

## 相關連結
- [[gemini-study-notebooks]] — 該條目介紹 Google Study Notebooks 產品功能（診斷→拆單元→追進度的官方學習平台），本篇是使用者在 Gemini Notebook（NotebookLM）上自行設計的讀書指令操作技巧，兩者互補但非同一功能。
