---
title: "Karpathy 馴服 Claude 的秘密武器「claude.md」全公開！"
source: "https://vocus.cc/article/69f9b50ffd897800011399da"
author:
  - "[[HKT實驗室]]"
published: 2026-05-05
created: 2026-05-18
description: "本篇文章介紹 Andrej Karpathy 使用的 claude.md 檔案"
tags:
  - "clippings"
status: processed
---
![zoomable](https://images.vocus.cc/c81eb2c6-e933-4d74-b2bf-3ef8c3f81b02.png)

andrej-karpathy-skills /CLAUDE.md

你還在忍受 AI 幻覺寫程式嗎？😫

為什麼會看到廣告

<iframe frameborder="0" src="https://30d9344d9310bda5e43c70520aafbc14.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>Andrej Karpathy（前特斯拉 AI 總監、OpenAI 創始成員）早就給出解法了！

他只用一份 ****claude.md**** 檔案，就把 Claude 變成超聽話的私人工程師＋寫作助理，寫扣、改 bug、整理筆記全部乖乖照辦，效率直接起飛 🚀

今天直接把大神的 claude.md 公開給大家看，複製貼上你也能玩出同款神 AI ✨

---

### 🧠 claude.md 到底是什麼？

其實就是一頁「AI 使用說明書」，每次對話開頭丟給 Claude 的系統提示（system prompt），用 Markdown 寫好寫滿。

Karpathy 把它放在專案資料夾裡，Claude 一讀完…直接變成他的形狀（笑），再也沒有 AI 在那邊給肖 🤣

---

### 🔥 大神寫了什麼？

## CLAUDE.md 行為準則說明

本準則旨在引導 LLM 在編寫程式碼時，避免常見的錯誤與過度設計。這些規則傾向於「謹慎重於速度」，對於簡單任務可自行判斷。

## 1\. 先思考，再寫程式

****不要預設，不要隱藏困惑，要把取捨攤開來。****

動手實作前應做到：

- 明確列出你的假設。若不確定，主動提問。
- 若有多種解讀方式，一併提出，不要私自選擇。
- 若有更簡單的做法，直接說出來，必要時可反對原本的要求。
- 遇到不清楚的地方，停下來，指出哪裡困惑，並提出問題。

## 2\. 簡單優先

****用最少且足夠的程式碼解決問題，不要寫未來的可能需求。****

- 不要實作未被要求的額外功能。
- 不要為只會使用一次的程式碼建立抽象層。
- 不要加入未被要求的「彈性」或「可設定性」。
- 不要處理不可能發生的錯誤情況。
- 若寫了 200 行程式碼而可以用 50 行達成，請重寫。

問自己：「資深工程師會說這段程式太複雜嗎？」如果會，就簡化。

## 3\. 精準修改

****只動必須動的地方，只清理自己造成的混亂。****

編輯既有程式碼時：

- 不要「順便改進」旁邊的程式碼、註解或格式。
- 不要重構沒有壞掉的部分。
- 遵循現有的寫作風格，即使你個人偏愛不同風格。
- 若發現不相關的無用程式碼，可以「提出來」，但不要擅自刪除。

當你的修改造成無效的引用時：

- 請移除 ****因你的修改而變得未使用**** 的 import、變數或函式。
- 除非被要求，否則不要移除既有的無用程式碼。

檢驗標準：每一行變更都應能直接對應到使用者提出的需求。

## 4\. 目標驅動執行

****定義成功標準，並反覆驗證直到達成。****

將任務轉換成可驗證的目標：

- 「加入驗證」 → 寫出針對無效輸入的測試，然後讓測試通過。
- 「修正錯誤」 → 寫出能重現錯誤的測試，然後讓測試通過。
- 「重構 X」 → 確保重構前後測試皆通過。

對多步驟任務，先簡述計畫：

text

```javascript
1. [步驟] → 驗證：[檢查項目]
2. [步驟] → 驗證：[檢查項目]
3. [步驟] → 驗證：[檢查項目]
```

明確的成功標準可以讓你獨立完成迴圈。模糊的目標（如「讓它動就好」）則需要不斷釐清。

---

****本準則有效時的跡象：**** diff 中的不必要變更減少、因過度設計而重寫的次數減少、問題在實作前提出而非犯錯之後。

原文參考: [andrej-karpathy-skills/CLAUDE.md](https://github.com/forrestchang/andrej-karpathy-skills/blob/main/CLAUDE.md)