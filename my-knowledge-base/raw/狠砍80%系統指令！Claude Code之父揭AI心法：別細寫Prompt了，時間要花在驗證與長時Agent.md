---
title: "狠砍80%系統指令！Claude Code之父揭AI心法：別細寫Prompt了，時間要花在驗證與長時Agent"
source: "https://www.bnext.com.tw/article/91670/claude-code-prompt-reduction-agent-verification"
author:
  - "[[李先泰]]"
published: 2026-07-30
created: 2026-07-30
description: "Claude Code 負責人說模型已強到不需步步引導，該投資的是驗證。他舉的 11 天重寫案例裡，撐住結果的正是那套測試套件。"
tags:
  - "clippings"
status: processed
---
這兩年用好 AI 編碼工具的共識做法，是把指令寫得越細越好：先做什麼、再做什麼、哪些絕對不准碰，一條一條列進規範檔。

但 Anthropic 在 2026 年 7 月 24 日推出 Opus 5 之後，做了一件方向相反的事，把 Claude Code 的系統提示（system prompt，工具每次啟動時餵給模型的內建指令）砍掉超過 80%。

拍板的人是 Boris Cherny，Claude Code 的創造者與現任負責人。他進 Anthropic 前在 Meta 當了五年首席工程師，著有《Programming TypeScript》。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

在 7 月 28 日公開的 Y Combinator Startup School 2026 對談中，主持人 Diana Hu 問他為什麼要刪系統提示，他的回答是： **那些指令原本是在修正模型「本來就該會、但當時不會」的行為，Opus 5 已經會了。**

他還提到一個沒寫進官方文件的做法。Claude Code 有一個 simple mode，開啟後會改用極簡系統提示與精簡工具組，團隊拿它當對照組，測試那些提示到底有沒有用。

他在對談中的說法是，模型在沒有這些提示的狀態下「其實聰明一點」。但他同時劃了界線：Claude Code 作為一個給人使用的產品，還是需要留一部分提示，讓產品行為符合使用者的預期。

## 為何過去非得把指令寫細？

細指令這個習慣是被兩件事逼出來的：工具的形態，以及人的經驗。

外部限制是工具形態。Boris 提到他開始做 Claude Code 時，最強的編碼模型是 Sonnet 3.5，而市面上的編碼產品在做單行自動補完，多行補完算新鮮，聊天功能只能讀不能寫。模型有能力一次寫完整個檔案，卻沒有產品讓它這麼做，而他做 Claude Code 的起點就是把那些擋路的框架拿掉。

內部限制是人的習慣。Boris 說，他看到最常見的失敗模式來自寫了十幾二十年程式的資深工程師： **他們習慣先把系統設計想清楚、把單元測試鋪好，然後要求模型完全照自己會做的方式執行。** 他的判斷是，這套訓練在模型身上失效，因為模型不像系統，比較像一個活的東西，每一代個性都不一樣，得花時間認識它再調整。

兩邊的拉扯方向剛好相反：工具越成熟，越不需要人補位；但人的經驗越深，越傾向把流程規定死。Anthropic 的處理方式是用一種研究界叫 ablation 的方法，白話說就是刪除法評測： **把整段系統提示刪掉，再一行一行加回去，逐行看它的實際影響。**

工具也一樣，Boris 說他們經常下架工具、刪掉框架裡的程式碼；依他在對談中的說法，Claude Code 框架裡留下的程式，大部分是安全、權限、靜態分析與介面。

他對一般使用者的建議是同一套：每六個月刪掉自己的 `CLAUDE.md` 、skills 與 hooks，看看模型會怎麼做。這話說得輕巧，但對於花了半年維護一整套規範檔的人，刪下去要有心理準備。

## 他把賭注從指令移到驗證

那刪完之後要往哪裡改進？Boris 在對談中的答案很明確：驗證。

他的說法是，現在的技能比較不是提示詞工程，而是怎麼給 Claude 一個看起來有點太難的任務，再讓它有辦法在過程中自己檢查成果；他並形容驗證是「大多數人沒做對的那件最重要的事」。

重建提示的順序，他給的是三個動作： **先刪掉、然後照常使用、只有在模型重複踩同一個坑時才把那條指令加回去。** 至於 evals（評測集）該不該一路累積下去，他的答案沒那麼乾脆。

這裡有個代價他自己也講了。評測集並不比框架長壽多少，大概只能活一到三個模型世代，模型進步太快，分數很快就滿分飽和，飽和了就得整組丟掉重做。也就是說，「投資驗證」不是一次性建設，是一筆會持續折舊的資產。

他自己跑的實驗是這樣的：用 Claude Tag（在 Slack 裡運作的 Claude）把 Claude 的 Electron 桌面版改寫成 Swift 原生版，指令大意是「改寫、在 Mac 虛擬機裡跑起來、截圖、逐像素比對，沒做完不要停」。

對談當時這個任務已經跑了 14 到 15 天還沒停，他估計衍生出數千到數萬個代理。Claude 還自己開了一個 Slack 頻道，每幾分鐘貼一次進度截圖。

## 「編碼問題已經解決」這句話，他自己收窄過

Boris 講過「編碼問題已經解決」，但他在對談中補了前提： **這句話只對他做的那類程式成立，不適用所有人。** 深層的系統程式庫、分散式系統、要求到像素級的介面驗證，Claude 都還會卡住。他說 Opus 5 在視覺與電腦操作上進步幅度不小，但還不完美。

這也解釋了為什麼 Boris 對「怎麼成為頂尖使用者」的回答是「不要聽 LinkedIn 網紅，不要看 Twitter」。他認為那種一招見效的技巧並不存在，做法只能是經驗性的： **給一個太難的任務、給它能自我檢查的工具、看它卡在哪，再用更好的提示或一個 skill 補上。**

於是留下一個更難回答的問題。如果每一代模型都要重刪一次規範檔，每一到三代就要重做一次評測集，那真正會累積下來的資產是什麼？Boris 給的線索藏在對談最後：他建議 CS 學生別只學電腦科學，還要學怎麼把它應用出去，包括養出自己的設計判斷、商業判斷，以及跟使用者對話的能力。那是他說自己仍然會親手做的部分。

換句話說，會折舊的是規範檔與評測集，留得住的是判斷該往哪裡優化的那個人。

> 延伸閱讀： [Claude Cowork是什麼？Cowork教學：簡報、報帳、整理雲端硬碟5個超實用場景](https://www.bnext.com.tw/article/89996/claude-cowork-transforming-generative-ai-into-a-digital-colleague)

資料來源： [Y Combinator Startup School 2026 對談](https://www.youtube.com/watch?v=qyPCVqFUyDo) 、 [Bun 官方部落格「Rewriting Bun in Rust」](https://bun.com/blog/bun-in-rust) 、 [Andrew Kelley 部落格](https://andrewkelley.me/post/my-thoughts-bun-rust-rewrite.html) 、 [ARC Prize Opus 5 測試結果](https://arcprize.org/results/anthropic-claude-opus-5) 、 [Anthropic dynamic workflows 公告](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code) 、 [Anthropic Opus 5 發布公告](https://www.anthropic.com/news/claude-opus-5) 、 [Claude 5 世代情境工程新規則](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) 、 [Claude Code CLI 使用文件](https://code.claude.com/docs/en/cli-usage) 、 [Bun 加入 Anthropic 公告](https://bun.com/blog/bun-joins-anthropic) 、 [《Fortune》](https://fortune.com/2026/06/08/anthropics-boris-cherny-creator-of-claude-code-says-there-are-days-he-manages-tens-of-thousands-of-ai-agents-at-once/) 、 [中央銀行新臺幣美元收盤匯率](https://www.cbc.gov.tw/tw/lp-645-1.html)

本文初稿為AI編撰，整理．編輯/ 李先泰