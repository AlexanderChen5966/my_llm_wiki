---
title: "Hiring Panel 履歷技能教學：七個評審角色怎麼用，為何不能當錄取機率"
source: "https://www.aiposthub.com/hiring-panel-resume-skill/"
author:
  - "[[Philo]]"
published: 2026-11-11
created: 2026-11-13
description: "Hiring Panel 履歷技能教學。履歷交給 AI 改，最常得到的是更流暢的句子，卻不一定知道招募者會在哪裡放棄閱讀。Hiring Panel 用不同審閱角色，把問題拆開再合併修改。"
tags:
  - "clippings"
status: "processed"
---
履歷交給 AI 改，最常得到的是更流暢的句子，卻不一定知道招募者會在哪裡放棄閱讀。Hiring Panel 用不同審閱角色，把問題拆開再合併修改。

這是 Alex Chen 發布的免費開源 Claude Code 技能。Skill 是交給 AI 遵循的工作方法，讓同一類任務能按固定流程進行。

它適合求職前找盲點，但評語仍是模型產生的第二意見。應徵條件、真實經歷與最後送出的文字，都要由本人確認。

## Hiring Panel 的七個角色怎麼組成？

[專案 README](https://github.com/alexyc9381/hiring-panel-skill?ref=aiposthub.com) 先安排模擬自動篩選，再由用人主管判斷，後面另有五個角色分別閱讀，包括主管、其他應徵者、執行長、招募者與未來同事。

所以「七個」指七個審閱角色，用人主管視角會在不同階段出現。它沒有因此變成七家公司的真實招募決定，也沒有保證使用七種不同模型。

這些視角各有用處：招募者關心快速閱讀，未來同事更在意合作方式。把各自理由攤開，才知道問題究竟是內容缺漏、證據不足，還是表達不清。

![Alex Chen 的 Hiring Panel 專案預覽](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/hiring-panel-resume-skill.png)

Alex Chen 的 Hiring Panel 專案預覽。來源： 官方／作者發布頁 。

## 如何準備第一份輸入？

先準備履歷和目標職缺全文，再使用 `/hiring-panel` 指定檔案或貼上文字。沒有職缺時，至少說清楚要應徵的職務，否則評語容易只剩通用建議。

[技能原文](https://github.com/alexyc9381/hiring-panel-skill/blob/main/skills/hiring-panel/SKILL.md?ref=aiposthub.com) 要求不得捏造雇主、職稱、日期、數字或技能。缺資料時會留下需要補充的位置，因此產出後應先處理這些空缺，再檢查語氣。

可以要求它把每項建議對回履歷原句，並分成可直接修的文字問題與需要本人補證據的內容。例如「負責專案」可以寫清楚工作範圍，但不能自己加一個不存在的成長率。

## 產出檔案怎麼驗收？

README 列出 `.hiring-panel/` 裡的 `PANEL.md` 與 `RESUME-IMPROVED.md` ，分別整理評語和修改稿。檔案存在之後，再確認內容是否對應同一份履歷與職缺。

一次執行包含八次子代理呼叫，會使用 Claude 方案額度。免費指的是技能授權，與模型用量分開，因此先用一個職缺測試就好。

驗收時優先查事實、時間線與職缺關鍵能力，再看措辭。若不同角色互相矛盾，回到實際職缺和自己的證據決定，不必把所有意見都塞進一頁。

## 常見問題

### 評審全說 HIRE，就代表能錄取嗎？

那是模擬角色的評語，沒有經過該公司的招募流程，不能換算錄取機率。

### 七個角色會互相看答案嗎？

技能安排五位讀者各自閱讀，再彙整意見，這能減少直接跟著前一位寫，但無法消除共同模型的盲點。

### 適合拿來篩選別人的求職資料嗎？

本文介紹的是本人修改履歷的用途。它的角色評語不應直接取代實際招募判斷。

## 先讓履歷每個主張都有證據

用一個目標職缺跑完，再逐條核對修改稿。把模型當成幫你指出問題的讀者，最後留下能在面試中清楚說明的真實內容。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

延伸閱讀： [2026 Claude 教學：官方 90 個 Use Cases、12 個工作流程與 Prompt 範例](https://www.aiposthub.com/claude-use-cases-90-workflows-2026/)