---
title: "Emil gestures 與 break-ui 教學：AI 做完介面，怎麼測拖曳手感與最壞資料？"
source: "https://www.aiposthub.com/emil-gestures-break-ui/"
author:
  - "[[Philo]]"
published: 2026-11-11
created: 2026-11-13
description: "Emil gestures 與 break-ui 教學。底部抽屜拖起來很順，關到一半卻抓不回來；客戶卡片放示範名字很漂亮，換成長信箱就破版。這兩種問題都要靠操作和資料找出來，單看首頁截圖很難發現。"
tags:
  - "clippings"
status: "processed"
---
底部抽屜拖起來很順，關到一半卻抓不回來，客戶卡片放示範名字很漂亮，換成長信箱就破版。這兩種問題都要靠操作和資料找出來，單看首頁截圖很難發現。

Emil Kowalski 的 gestures 與 break-ui 是兩個分工不同的技能文件。前者整理拖曳的互動規則，後者用極端但合理的資料檢查介面。

對已經有網站的人，我會先用它們補驗收條件。技能可以提醒 AI 注意細節，但能不能通過，仍要回到成品測試。

## gestures 把拖曳分成哪兩個階段？

[gestures 原始文件](https://github.com/emilkowalski/skills/blob/main/skills/gestures/SKILL.md?ref=aiposthub.com) 先分手指按住與放開。按住時，元件應直接跟隨手指，放開後，才處理慣性、回彈與停靠位置。

如果按住時還套緩動，畫面會追在手指後面，使用者容易覺得卡。放開時則要同時考慮移動距離與速度，因為短促快甩和慢慢移動同樣距離，可能代表不同意圖。

文件也要求關閉中的抽屜能被再次抓住，並建議一般底部抽屜優先考慮現成的 Vaul。這能把常見邊界情況交給已有設計的元件處理。

![Emil Kowalski skills 專案預覽](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/emil-gestures-break-ui.png)

Emil Kowalski skills 專案預覽。來源： 官方／作者發布頁 。

## 本站的小樣本測試看到了什麼？

本站 2026 年 11 月 10 日的紀錄，使用 Claude Code 的 Opus 5.5 子代理，讓一般提示與附上技能全文各產生五份底部抽屜，再由程式模擬按住、拖曳與放開來測量。子代理的模型身分依呼叫設定記錄，沒有另行驗證。

兩組都做到跟隨、頂端阻力與速度關閉，「關到一半再抓回來」一般組是 0／5，附文件組是 3／5。這表示技能在這項條件有幫助，但沒有讓所有輸出通過。

測試限制也很具體：它要求不使用外部函式庫，與文件優先用 Vaul 的建議不同，量的是位置與操作結果，沒有量真人手感，也沒有真手機測試。所以這組數字適合提醒驗收項目，不能當成全面的技能效能排名。

## break-ui 怎麼測最壞資料？

[break-ui](https://github.com/emilkowalski/skills/blob/main/skills/break-ui/SKILL.md?ref=aiposthub.com) 要求在同一個元件切換正常與最壞資料，讓問題能前後對照。它關心長名字、缺欄位、零筆資料、大數字、不同語系及 emoji 等情境。

可以先請 AI 完成這份工作單：

> 保留目前客戶卡片，新增正常資料／最壞資料切換。測長信箱、空清單、缺職稱、大金額與 emoji 姓名，列出哪個欄位破版、如何重現及修改方式。修完要用同一組資料重測。

這樣留下的是可重現的問題，工程師也能檢查修正後是否又傷到正常狀態。

## 常見問題

### 有 spring 字樣就代表手感正確嗎？

不代表。spring 是模擬彈簧運動的方法，但狀態若太早設成已關閉，動畫仍可能抓不回來。

### 貼技能全文等於安裝技能嗎？

兩者的載入方式不同。上述測試用的是貼文件，不宜宣稱已驗證完整安裝流程。

### 只測 390px 就夠嗎？

390px 可以先找到手機版問題，發布前仍要補真手機、鍵盤與其他必要尺寸。

## 把技能變成具體的通過條件

先挑一個抽屜、一張卡片，把重現步驟與通過條件寫下來。能抓回關閉中的面板、也能承受真實資料的介面，才有值得保留的改善。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

延伸閱讀： [2026 Claude 教學：官方 90 個 Use Cases、12 個工作流程與 Prompt 範例](https://www.aiposthub.com/claude-use-cases-90-workflows-2026/)