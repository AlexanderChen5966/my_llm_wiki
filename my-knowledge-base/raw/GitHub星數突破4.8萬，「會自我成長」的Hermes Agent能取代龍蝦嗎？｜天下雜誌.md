---
title: "GitHub星數突破4.8萬，「會自我成長」的Hermes Agent能取代龍蝦嗎？｜天下雜誌"
source: "https://www.cw.com.tw/article/5140619"
author:
  - "[[樂羽嘉]]"
published: 2026-04-13
created: 2026-05-18
description: "在GitHub霸榜多天、累計拿到4.8萬顆星的新AI代理Hermes Agent有多好用？能取代「龍蝦」OpenClaw嗎？"
tags:
  - "clippings"
status: processed
---
## GitHub星數突破4.8萬，「會自我成長」的Hermes Agent能取代龍蝦嗎？

在GitHub霸榜多天、累計拿到4.8萬顆星的新AI代理Hermes Agent有多好用？能取代「龍蝦」OpenClaw嗎？


調整朗讀語速

![龍蝦-Hermes-Hermes Agent-AI Agent-AI代理-AI](https://cw-image-resizer.cwg.tw/resize/uri/https%3A%2F%2Fcdn-www.cw.com.tw%2Farticle%2F202604%2Farticle-69dc935857b623.03150293.png/?w=1600)

龍蝦-Hermes-Hermes Agent-AI Agent-AI代理-AI

圖片來源：Nano Banana

瀏覽數：3882

- 編譯
- 天下Web only

發布時間：2026-04-13

瀏覽數：3882

限時優惠$11解鎖暢讀天下

這幾天紅到出圈的個人化AI助理Hermes Agent，官網簡介寫著「the agent that grows with you」──一個會跟你一起成長的代理。這正是Hermes Agent的獨特之處。

Hermes Agent內建學習迴圈（Learning Loop）框架，能從任務中自動淬煉出技能，也能夠長期記憶用戶偏好，跨對話精準回憶，目標是「愈用愈懂你」。部署方面也有很高的彈性，無論是5美元的小資VPS、Docker容器化環境，還是Serverless架構，都支援6種不同的部署模式。它也能一鍵切換超過200種大模型，無縫串接 Telegram、Discord與Slack等熱門通訊平台。

OpenClaw比較依賴靜態設定，如果你要處理複雜任務，開發者必須修改設定檔，編寫Skills、定義怎麼調用工具，組合好幾個代理。OpenClaw的記憶通常是無狀態的或是簡單的向量檢索（RAG），除非你手動修改代碼或設定檔，它失敗後也不會自動學會下次怎麼做。

Hermes Agent則是單一代理框架，這也是它的核心賣點。它不鼓勵鼓勵堆疊大量的子代理，主張讓一個具備強大推理能力的大腦（如Claude 3.5或Hermes 4模型）透過自我更新來解決問題。由於內建學習迴圈，它發現一個好的解法時，會自動生成Skill文件，下次執行時，它就能自動載入上次寫好的SOP，也就是能直接記住最有效的解法。

社群媒體上這幾天也出現各種聳動標題，不乏各種大吹Hermes Agent、勸大家趕快從OpenClaw切換到Hermes Agent的論調。

還有人分享「Hermes橘皮書」（Hermes Orange Book），聲稱Hermes Agent被嚴重低估、可謂是目前最強大的開源AI代理框架之一。

Hermes Agent是由Nous Research團隊開發，最初於2月25日發布。觀察GitHub上的發布紀錄，從3月中旬更新V0.2.0版本到這幾天天發布V0.8.0每次更新都有實用的功能改進，是非常活躍的開源專案。

先前OpenAI共同創辦人卡帕西（Andrej Karpathy）分享的「LLM Wiki筆記法」，也已經立刻被加到Hermes內建技能中。

Hermes Agent不只支援安裝在電腦上，還可以透過Termux終端模擬器安裝在 Android手機，模型與網關（Gateway）的設定則類似OpenClaw 。

## Hermes Agent跟龍蝦還有哪裡不一樣？

OpenClaw的核心是把我們的AI完全從對話框中拉出來，連接到實際的工作、學習與生活中去執行任務，例如連接Telegram、執行終端命令、控制瀏覽器，幫我們寄電郵、管理行程等。

OpenClaw的缺點則是無法從我們的日常使用中自動學習、演化。

OpenClaw的記憶是靜態的。我們把資訊寫入設定檔，它讀取資訊，會話結束，下次它再讀取一次。它不會主動從執行過程中提取任何內容，也不會因為我們糾正過它一次，下次就自動做對。

在使用完所有工作流後，我們仍需要提醒它、封裝成Skills或特定的提示詞等。

雖然目前有一些特定Skills旨在賦予OpenClaw自我學習的能力，Hermes Agent從底層架構的學習迴圈到記憶系統、再到代理執行內部，都聚焦在「愈用愈懂你」。

Hermes Agent的特殊之處在於一個封閉的學習迴圈。

每次完成任務後，Hermes會檢查：這次執行值得記下來嗎？觸發條件非常具體，例如工具被調用超過5次、犯錯後自我修復、被用戶糾正，或是採取了一條不明顯但有效的路徑，只要滿足任一條件，它就會在~/.hermes/skills目錄中生成一個Skill文件。

就像技能市場中廣泛使用的Skills一樣，這個自動生成的文件是下次可以直接照做的操作流程。從名稱、描述、步驟以及涉及的工具調用都寫得清清楚楚。格式遵循agentskills.io的開放標準，理論上可以和其他Agent 跨平台相容，並用於 OpenClaw、Claude Code與Cursor等工具。

Skills文件寫入後也不是一成不變，如果Hermes Agent在後續執行中發現了更好的路徑，會直接修改。它偏好使用「補丁」（Patch）方式，只傳入舊字串、替換內容，不重寫整個文件。

這種選擇背後的考量是，重寫整個文件容易破壞原本有效的部分，補丁只會動到有問題的部分，比較安全，也節省Token。

另一個不同之處是記憶系統。

Hermes的記憶系統經過專門設計，總共分為四層，每層負責不同的任務，在不同時間進行檢索。

第一層稱為常駐提示詞記憶。有兩個文件：MEMORY.md和USER.md，儲存每次會話開始時需要自動讀取的上下文。總字數限制只有3575字。這個數字是 Hermes Agent特意縮減到這麼小，強迫進行篩選。

第二層是會話存檔。每段對話都會寫入SQLite資料庫並使用全文索引進行搜尋檢索。當Hermes Agent需要歷史背景時，它會主動發起查詢，透過LLM總結搜尋結果，然後只會注入與眼前任務相關的部分。

第三層是技能文件，也就是這個學習迴圈產出的成果。預設情況下，系統提示詞中只會讀取技能名稱和簡短描述，全文則按需求讀取。這種設計是為了讓技能庫從40個增加到200個，上下文成本則幾乎不變。

第四層稱為Honcho，這是一個可選擇的用戶建模層，會跨會話被動累積你的偏好、溝通風格和領域知識，適用於把Hermes Agent當成長期個人助理的用戶。

這四層的分工原則非常明確，需要出現在每次對話中的東西就放進第一層，只有特定主題出現時才有用的東西留在第二層等待檢索。如果是可重複使用的操作流程，讓第三層處理。如果是用戶的長期個人偏好，則交給第四層。

## Hermes Agent會取代龍蝦嗎？

目前Reddit、Youtube和X上的社群共識不是Hermes取代龍蝦，比較傾向認為兩者可以互補。

OpenClaw 負責「做事」，處理多管道互動、團隊工作流程和複雜的生態系整合，Hermes負責「動腦」，主攻持久化記憶、自動生成技能和高層次的模型推理。

常見的設定是把Hermes當作高級規劃器，在OpenClaw 工具上運作。你只要執行hermes claw migrate指令，就可以把現有的OpenClaw 技能、記憶和設定一鍵搬移到Hermes 中。

Hermes Agent嘗試把「AI 能力」從一次性調用變成可以持續累積的資產，也就是不把代理當成臨時調用的介面，而是當成長期存在的系統，可以在使用過程中不斷積累能力，最後反過來影響模型本身。

Hermes的自我持續進化之路，還要走很久。目前記憶噪音、技能品質、訓練迴圈穩定性都需要反覆改進，部署門檻也還在，一般用戶不太能無痛使用。不過若是Hermes發展起來，未來評估AI代理的方式，可能會從它現在能做什麼，變成「它可以成長成什麼樣子」。

（資料來源：36kr.com、PA News、Github）