---
title: "Claude 5 時代的情境工程六條新規則，與我砍掉的 1473 行 harness"
source: "https://www.agentcrew.cc/blog/posts/zh/claude-5-context-engineering-six-rules"
author:
  - "[[Dustin Yuchen Teng]]"
published: 2026-07-31
created: 2026-08-19
description: "讀完 Anthropic 對 Claude 5 的情境工程指南後，我把重點做成九張圖卡，然後照著把累積四個世代的 harness 削掉 1473 行。"
tags:
  - "clippings"
status: "processed"
---
目錄

讀完 Anthropic 官方對 Claude 5 的情境工程指南，我把裡面的重點整理成九張圖卡。整份指南的核心只有一句：更少規則，更好判斷，更強的上下文。

![圖卡一：Claude 5 時代的情境工程新規則，副標為更少規則、更好判斷、更強的上下文](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-01.jpg)

Anthropic 表示，他們已從 Claude Code 的系統提示中刪除超過 80% 的內容。原因不是少做事，而是新一代模型更能理解周邊脈絡、工具介面與使用者意圖。所以提示只是情境的一小部分，真正重要的是整體的 context engineering，重點從「塞更多規則」轉向「設計更好的上下文」。

## 總原則：從限制到判斷

![圖卡二：總原則從限制到判斷，分為減少限制、信任判斷、善用工具三欄](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-02.jpg)

新一代 Claude 更能理解脈絡、工具與使用者意圖，所以整體方向是三件事：減少限制，刪除許多不再必要的硬規則；信任判斷，讓模型依情境做合理決策；善用工具，靠 Skills、memory、artifacts 分工。

下面六條規則，都是這個總原則的展開。

## 規則一：少給規則，讓 Claude 用判斷

![圖卡三：規則一少給規則讓 Claude 用判斷，對照過去的硬性禁止與現在的貼近上下文寫法](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-03.jpg)

從「硬性禁止」轉向「貼近上下文」。過去的寫法是：「預設不寫註解。不要建立多段註解或分析文件。」現在的寫法是：「寫出像周邊程式碼的 code：匹配註解密度、命名與 idiom。」

舊模型需要更多護欄，新模型能權衡例外情況，所以把規則改成更高層的原則。

## 規則二：少給範例，改設計介面

![圖卡四：規則二少給範例改設計介面，以 Todo 工具的 status 參數為例](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-04.jpg)

與其示範，不如讓工具本身更有表達力。Anthropic 發現，給太多範例反而會限制 Claude 的探索空間。更好的做法，是把工具、腳本與檔案設計得更清楚。

過去會給三條使用範例；現在則是設計表達力強的工具，例如 Todo 工具用 `status: pending / in_progress / completed` ，並規定一次只允許一項任務為 `in_progress` 。參數設計本身就是提示，結構清楚，比範例更泛用。

## 規則三：不要一次塞滿，改用循序揭露

![圖卡五：規則三不要一次塞滿改用循序揭露，分為發現、載入、使用三步](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-05.jpg)

把正確的情境，在正確的時間載入。過去大家傾向把所有說明都放在最前面。現在更好的做法，是讓 Claude 需要時再載入 Skills、工具定義、CLAUDE.md 子檔與其他參考。

流程是三步：發現，先搜尋需要的知識或工具；載入，在需要時讀入 Skills、ToolSearch、相關檔案；使用，只讓當前任務看到必要 context。context 越精準越好，deferred loading 能節省上下文空間。

## 規則四：別重複自己，工具描述要簡潔

![圖卡六：規則四別重複自己，對照舊做法在 system prompt 與工具描述重複，與新做法把用法寫在工具描述](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-06.jpg)

同樣的事不要在 system prompt 與工具說明裡重複寫。早期模型常需要重複提醒，現在 Anthropic 發現，可以刪掉很多重複內容，把「如何使用工具」集中寫在工具描述裡。

舊做法是 system prompt 重複提工具、工具描述也再講一次；新做法是 system prompt 保持精簡，工具怎麼用直接寫在 tool description。這樣能減少衝突訊號，也讓指令位置更清楚。

## 規則五：從 CLAUDE.md 記憶到自動記憶

![圖卡七：規則五從 CLAUDE.md 記憶到自動記憶，對照手動寫入與 Claude 自動儲存記憶](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-07.jpg)

讓記憶機制更自然，而不是手動堆文件。過去常鼓勵使用者把重要資訊寫進 CLAUDE.md，用 `#` 快捷鍵或手動寫入；現在 Claude 會自動保存與工作、與個人偏好相關的 memory，跨 session 重用。

CLAUDE.md 因此回到輕量說明，記憶交給 memory 系統處理。

## 規則六：從簡單規格到豐富參照

![圖卡八：規則六從簡單規格到豐富參照，列出 HTML artifact、程式碼與測試、其他 codebase、rubrics 四種參考材料](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-08.jpg)

Claude 可以讀更高保真度的參考材料。除了 markdown 計畫檔，Claude 現在也能有效使用 HTML artifacts、程式碼、測試、其他 codebase，甚至 rubrics 來理解你要的結果。

HTML artifact 比文字描述更高保真；程式碼與測試可直接作為規格；其他 codebase 可參照與移植；rubrics 把你的審美與標準明文化。

## 落地做法：怎麼組裝你的 context

![圖卡九：落地做法怎麼組裝 context，分為 System Prompt、CLAUDE.md、Skills、References 四格](https://www.agentcrew.cc/blog/assets/posts/claude-5-context-engineering-six-rules/card-09.jpg)

把 system prompt、CLAUDE.md、Skills、References 各放在對的位置。

System Prompt 定義產品環境與代理角色，說明任務目標、輸入輸出格式、限制條件與代理角色，建立一致的行為基準。CLAUDE.md 簡述 repo，用 token 留給 gotchas，精簡描述專案結構、開發與測試指令、依賴與慣例，細節與陷阱留給 References。Skills 把團隊特有知識與流程模組化，封裝成模組，含觸發條件、步驟與產出格式。References 用 `@` 檔案帶入 spec、mockup、codebase，做為事實依據與上下文延伸。

最後一步是簡化：刪掉多餘與重複內容，優先使用循序揭露，可用 `/doctor` 檢查與優化。

## 照做之後

昨天按照 Anthropic 官方對他們五代模型的兩個建議，把整套四時代建構起來的 harness 削減了 1473 行，大約 2 萬個 token。

目前用 5 起來的確少了過度思考鑽牛角尖的感覺，也都能遵循得不錯。我故意切回 4.6，果然發現舊一代模型開始胡亂放飛自我。

如果從這個角度來看，是 Claude 最近少數的好事。