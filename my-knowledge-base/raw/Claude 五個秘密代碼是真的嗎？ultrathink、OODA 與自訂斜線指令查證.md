---
title: "Claude 五個秘密代碼是真的嗎？ultrathink、OODA 與自訂斜線指令查證"
source: "https://www.aiposthub.com/claude-five-secret-codes-fact-check/"
author:
  - "[[Philo]]"
published: 2026-11-12
created: 2026-11-13
description: "ultrathink、L99、OODA、/ghost、scaffold 是 Claude 官方秘密指令嗎？依 Claude Code 模型設定、技能與指令文件，區分內建關鍵字、提示詞方法及自訂技能，說明版本與產品範圍。"
tags:
  - "clippings"
status: "processed"
---
在提示詞前面加一個英文詞，就能讓 Claude 更聰明，這類說法很吸引人。但同一個詞可能是程式認得的開關，也可能只是模型會閱讀的一般文字。

Claude 五個秘密代碼這個題目，先要分清楚你用的是 Claude Code，還是聊天介面。兩者的指令入口與設定不能直接互換。

截至 2026 年 11 月 12 日，本文查核的官方頁面明確記載了 `ultrathink` 的特殊行為。L99、OODA、 `/ghost` 和 scaffold，則沒有在下列頁面被記載為同一類內建開關。

我會先看產品文件，再決定是否測試文字提示的效果。文件沒有列出的用法，仍可能是某個人的方法或自訂技能，只是不能直接稱作官方功能。

## 官方記載的 ultrathink 會做什麼？

[Claude Code 模型設定文件](https://code.claude.com/docs/en/model-config?ref=aiposthub.com) 說明，在提示中加入這個關鍵字，可以要求當回合進行更深入的推理，不會因此改掉整個對話的 effort 設定。effort 指的是模型花在推理上的努力程度。

同一段文件也區分了其他說法： `think hard` 等片語會當成一般提示文字處理，沒有被辨識為相同的關鍵字。它們可能影響文字指示的含義，但程式處理方式不同。

若在終端機輸入，ultrathink 還可能把當回合提高到 `xhigh` 努力程度。這一層需要 Claude Code v2.1.311 以上、支援的模型，以及允許該程度的個人和組織設定。官方也說明了無法提高時的處理方式。

因此，不能把「要求更深推理」換算成固定十倍，也不能從 Code 文件推論聊天介面會用同樣方式處理。要確認目前是否生效，應回到實際產品的設定與提示顯示。

![Claude Code 官方 Model configuration 文件分享圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/claude-five-secret-codes-fact-check.png)

Claude Code 官方模型設定文件分享圖；ultrathink 的行為以該頁說明為準。 來源： 官方／原作者發布頁 。

## 其餘四個詞，該如何分類？

下面的結果限於本次讀取的模型設定、內建指令、技能文件及平台提示詞教學，不是對所有版本、外掛與私人設定的全面盤點。

| 詞語 | 本次文件核對結果 | 使用時應知道的事 |
| --- | --- | --- |
| L99 | 沒有列為上述頁面的內建開關 | 名稱本身不能證明有特定權限或模式 |
| OODA | 沒有記載為內建模式 | 可把它當成觀察、定向、決定、行動的方法要求 |
| /ghost | 沒有列在內建指令頁 | 可能由使用者或外掛建立同名技能 |
| scaffold | 沒有記載為提示開關 | 英文原意常用於建立架構或骨架，要寫出要產生什麼 |

斜線開頭特別容易混淆。 [官方技能文件](https://code.claude.com/docs/en/skills?ref=aiposthub.com) 允許以技能名稱直接呼叫工作流程，因此別人電腦上的 `/ghost` ，可能來自他已安裝的技能。你的環境沒有那份設定，就不會自動得到同一個功能。

## 看到秘密指令，怎麼自己核對？

先記下說法的完整條件：哪個產品、哪個版本、要達到什麼效果。接著查 [內建指令清單](https://code.claude.com/docs/en/commands?ref=aiposthub.com) ，再檢查自己是否安裝了同名外掛或技能。

如果它只是提問方法，就把方法寫開。例如不要只丟 OODA，可以要求「先列觀察到的事實，再提出兩種解釋，選擇行動後寫出要看什麼結果」。這是本文建議的提示寫法，並非 Anthropic 宣布的特殊模式。

想比較效果時，用相同資料、相同模型與同一份驗收表，分別測試有沒有加那個詞。單次回答更長，不能直接當成推理更正確的證明，答案仍要有外部依據。

## 常見問題

### 官方文件沒有列，就代表完全沒用嗎？

沒有這種推論。文字指示可能改變回答，也可能指向自訂流程。本文查的是有沒有記載為產品內建機制。

### scaffold 可以拿來請 AI 做大綱嗎？

可以當作一般文字，但最好補上大綱用途、必要段落和完成標準，減少模型自己猜測。

### 要去哪裡學比較穩定的提示方法？

可以從 [Claude 平台的提示詞最佳實務](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices?ref=aiposthub.com) 開始，再對照使用產品的限制。不要把一套產品的指令直接搬到另一套。

## 把想得到的結果寫出來

下一次看到密語教學，先找官方頁面與環境條件，再把自己要的成果寫成一句能檢查的需求。即使最後沒有使用那個詞，這一步仍能幫你判斷交付是否合格。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

延伸閱讀： [AGENTS.md 怎麼減少介面廢話？一句英文指令、兩種頁面的測試與取捨](https://www.aiposthub.com/agents-md-interface-copy/)