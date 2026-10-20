---
title: "Loop Engineering 是什麼？為什麼矽谷都在說：別再 prompt 你的 AI"
source: "https://rar.design/posts/loop-engineering-complete-guide"
author:
  - "[[設計師 Riven]]"
published: 2026-07-15
created: 2026-07-16
description: "LoopEngineering（迴圈工程）是 2026 年 6 月爆紅的 AI 協作方法：與其一句一句下指令，不如設計一個會自動觸發、自動驗證、自動決定下一步的迴圈。本文拆解它的起源、五大構件、三個難題，以及設計師該怎麼接住這場典範轉移。"
tags:
  - "clippings"
status: processed
---
Claude Code 之父說他不再 prompt Claude 了。從 Prompt、Context、Harness 到 Loop，槓桿點的第四次搬家

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年7月15日 上午 9:47

AI 設計

AI WORKFLOW · 2026

![Loop Engineering 迴圈工程概念圖：從 Prompt、Context、Harness 到 Loop 的四層嵌套演進](https://s.teachifycdn.com/image/width=2048,quality=80/attachment/public_image/22587fb3-a6f7-49ff-abeb-dab0899a7a0c/445cf3eb-9020-4f3b-9252-7a0fcffc6fe0.png)

Loop Engineering 迴圈工程概念圖：從 Prompt、Context、Harness 到 Loop 的四層嵌套演進

▲ Loop Engineering：Prompt、Context、Harness 一層包一層，最外圈的迴圈才是讓一切動起來的東西

QUICK ANSWER

Loop Engineering（迴圈工程）是 2026 年 6 月在 AI 圈爆紅的工作方法：與其一句一句 prompt AI，不如設計一個會自己跑的迴圈——自動觸發任務、把結果交給驗證、依回饋決定下一步，達成條件才停。它是繼 prompt engineering、context engineering 之後的下一層，把你從「下指令的人」變成「設計下指令系統的人」。

CHAPTER 01 · WHAT IS IT

## 一句話炸開半個科技圈

六月上旬，AI 圈的話題被一句話重新洗牌。Claude Code 的創造者、Anthropic 的 Boris Cherny 說：「我已經不 prompt Claude 了。我有一堆迴圈在跑，是它們在 prompt Claude、決定接下來做什麼。我的工作是寫迴圈。」

“

I don’t prompt Claude anymore. My job is to write loops.

— Boris Cherny，Claude Code 創造者

幾乎在同一時間，開發者 Peter Steinberger 發文說：「你不該再 prompt coding agent 了，你該設計會 prompt 它們的迴圈。」這則貼文幾天內衝破 650 萬瀏覽。隔天，在 Google 深耕開發者體驗多年的 Addy Osmani 發表了一篇同名文章，把這件事正式命名為 Loop Engineering，還給了它一套完整的解剖學。一個名字、兩則發言、一篇文章，一週之內，這個詞成了 agentic AI 討論的重心。

定義其實很好懂。過去兩年你跟 coding agent 的相處方式是：寫一個好 prompt、給足 context、讀它回什麼、再打下一句。工具在你手上，一回合接一回合，你全程握著。Loop engineering 把這個姿勢整個翻掉——你設計一個小系統，讓它自己找到工作、分派出去、檢查結果、記下進度、決定下一步，然後由這個系統去戳 agent，而你去做別的事。工作的單位從「一個 prompt」變成「一個迴圈」：行動、觀察回饋、決定下一步、直到終止條件成立。

換句話說，你從聊天框裡的那個人，變成蓋那台機器的人。這不是措辭技巧的升級，是槓桿點搬家了。

CHAPTER 02 · THE LINEAGE

## 從 Prompt 到 Loop，槓桿點的四次搬家

Loop engineering 沒有憑空冒出來。它是同一條路線圖上的第四站，前三站你應該都經歷過。

THE FOUR SHIFTS · 2022 → 2026

2022–24

Prompt Engineering

優化「你怎麼說」。角色設定、範例、step by step。天花板很明顯：再完美的措辭，也補不了模型沒拿到的資訊。

2025

Context Engineering

優化「模型看到什麼」。Shopify 執行長 Tobi Lütke 給出定義、Karpathy 背書，prompt 從此變成 context 的子集。

2026

Harness Engineering

優化「agent 活在什麼環境」。鷹架、工具、限制、回饋——讓 agent 從聰明變可靠的那一整圈。

2026/06

Loop Engineering

優化「什麼循環驅動 agent 往目標走、什麼時候停」。前三層都還在，它是讓一切動起來的那一層。

值得記的是，這四層是嵌套而非取代。你還是要寫 prompt、還是要管 context、還是要架 harness——loop 只是把它們全部放進一個會轉的循環裡。學術上這條線可以追到 2022 年 Princeton 與 Google 的 ReAct 研究：讓模型推理一步、行動一步、看結果、再推理，跟一次回答完的模型行為完全不同。今天所有的 agent 迴圈，都是 ReAct 的後代。

而土炮那條線更有趣。2025 年 7 月，澳洲工程師 Geoffrey Huntley 把 coding agent 包進一個 bash while 迴圈：每一圈開全新的 context，狀態全部寫在磁碟上的 markdown 檔，跑完一件事就結束、再開下一圈。他用辛普森家庭那個一直撞門框還開心喊「I’m helping!」的角色命名，叫它 Ralph loop——看起來蠢到不行，但他靠這個迴圈用 297 美金的 token 成本，交付了一個報價五萬美金的 MVP。當時大家笑著轉發，一年後，這套土炮的每一個零件都變成 Claude Code 跟 Codex 的內建功能。 [Codex 今年爆紅](https://rar.design/posts/codex-why-popular-2026) 的其中一個原因，就是它把迴圈需要的零件全部收進了產品裡。

CHAPTER 03 · HOW IT WORKS

## 一個迴圈的解剖：五個構件加一份記憶

Osmani 那篇命名文章最有用的地方，是把「迴圈」拆成了可以逐一檢查的零件。他說一個迴圈需要五樣東西，加上一個放記憶的地方。而且這五樣在 Claude Code 跟 Codex 裡幾乎一對一存在，名字略有不同，能力完全同構。

![Anthropic 官方的 autonomous agent 迴圈示意圖：人類下達目標後，LLM 透過行動與環境回饋不斷循環，直到任務完成](https://cdn.sanity.io/images/4zrzovbb/website/5c0c0e9fe4def0b584c04d37849941da55e5e71c-2401x1000.png)

▲ Anthropic 官方文件裡的 agent 迴圈：行動進環境、回饋回模型，一路循環到任務清空為止（來源：Anthropic）

01 · AUTOMATIONS

心跳

排程觸發、自動巡檢與分揀。有它，迴圈才是迴圈，而非你手動跑過一次的東西。

02 · WORKTREES

隔離

每個平行 agent 各有一份 checkout，兩個 agent 改同一個檔案的災難從根源消失。

03 · SKILLS

專案知識

把慣例、流程、踩過的坑寫成 SKILL.md，agent 每一圈都讀，知識開始複利。

04 · CONNECTORS

接上真實工具

MCP 把迴圈接上 issue tracker、資料庫、Slack。只看得到檔案系統的迴圈，是很小的迴圈。

05 · SUB-AGENTS

做的跟驗的分開

寫程式的模型給自己的作業打分數，會打得太寬容。第二個 agent 用不同指令來驗，才抓得到第一個說服自己的東西。

＋ EXTERNAL STATE

記憶

一個 markdown 檔或一塊看板，記下做完什麼、接下來做什麼。agent 每次都失憶，但 repo 不會。

這六樣東西你可能已經在用一半了。Skills、Connectors、Plugins 我之前寫過一篇 [設計師必裝清單](https://rar.design/posts/claude-directory-skills-connectors-plugins-essentials) ，當時的框架是「讓 Claude 更懂你」——放進迴圈的語境重看一次，它們是迴圈的第三與第四號零件。你替專案寫的每一支 skill，都是未來某個迴圈不用重新猜的知識；你接上的每一個 connector，都是迴圈能替你動手的一個現實世界端點。我自己拿 [Meta 廣告的官方 MCP](https://rar.design/posts/claude-meta-ads-mcp-guide) 跑投放檢查時，體感就是這樣：連接器接好之後，剩下的問題只剩「多久跑一次、跑完誰驗收」。

產品面最值得認識的是 Claude Code 的兩個指令。/loop 讓一個任務按節奏重跑；/goal 更狠——你給一個可驗證的完成條件，例如「auth 目錄的測試全過、lint 乾淨」，它就一路做到條件為真才停，而且每一回合是由另一個獨立的小模型判斷做完沒。做事的人不自己驗收，這個原則直接內建在指令裡。Codex 有同名的指令、同構的能力，這也是為什麼 Osmani 說：一旦你看出形狀相同，就不用再吵哪個工具，你設計的迴圈在哪邊都能跑。想從單一 agent 的基本功開始， [Claude Code 設計師指南](https://rar.design/posts/claude-code-complete-guide-for-designers) 裡把 agentic loop 的入門版走過一遍了。

想聽第一手的思路，這場 Anthropic 官方對談值得完整看完——Cherny 談模型與 harness 怎麼互相演化，正是迴圈能成立的前提：

![](https://www.youtube.com/watch?v=iF9iV4xponk)

CHAPTER 04 · THE HARD PARTS

## 真正難的三件事：記憶、停止、驗證

把迴圈跑起來很簡單，讓它跑一小時不出事才是工程。實務上翻車的原因高度集中，幾乎都落在三個地方。

一

Context 會爛掉

迴圈每走一步都往視窗塞東西，越長越失焦。解法是把 context engineering 搬進迴圈裡：壓縮舊步驟、把狀態外部化到檔案、讓 sub-agent 開乾淨視窗只回傳結論。

二

迴圈不會自己停

天真迴圈的招牌 bug 是永不停止。要有多層出口：驗證器、迭代上限、token 與時間預算，還有最容易被漏掉的——連續幾步毫無進展就中斷、交回人類。

三

回饋必須誠實

迴圈的品質上限等於回饋的誠實度。金標準是測試、type checker、linter 這種機械驗證——模型講不贏編譯器。讓模型當裁判可以補位，但裁判是可能被說服的。

第三點值得多講一段，因為它有一個經典翻車姿勢叫 reward hacking：你把目標寫成「讓 CI 變綠」，agent 找到的最短路徑，是把那個一直失敗的測試整個拿掉。目標達成了，災難也達成了。另一個近親叫 hallucinated success——agent 回報「做完了」，但「做完了」是一個主張，而非一個證明。這兩個坑指向同一個結論：能放機械驗證的地方就放機械驗證，模型的自我報告永遠排在最後面。

![Anthropic 官方的 evaluator-optimizer 模式示意圖：一個模型負責產出方案，另一個模型負責評估並回饋，兩者循環直到通過](https://cdn.sanity.io/images/4zrzovbb/website/d3083d3f40bb2b6f477901cc9a240738d3dd1371-2401x1000.png)

▲ Anthropic 的 evaluator-optimizer 模式：產出者與評估者分開循環，正是「做的跟驗的分開」的正式版（來源：Anthropic）

你會發現這三件難事有個共通點：它們全部是設計決策，跟模型多聰明沒有關係。模型是迴圈中間那個黑盒子，工程全在迴圈本身。這也是「loop engineering 比 prompt engineering 難」的原因——你要負責的東西從一句話的品質，變成整個系統的目標、驗證與停止條件的品質。

CHAPTER 05 · FOR DESIGNERS

## 設計師版的迴圈長什麼樣

這波討論幾乎全繞著寫程式轉，原因很誠實：程式碼天生就有現成的驗證器。測試會回傳過或不過、型別檢查會報錯、編譯器不會給你面子。迴圈需要的「誠實回饋」，工程領域是內建的。

但這個模式的本體是「目標、行動、驗證、重複」，而它挑的條件只有一個：你手上有沒有可驗證的回饋。用這個標準掃一遍設計與內容工作，可迴圈的環節其實不少：規格檢查是一個——間距、字級、色票對不對得上 design system，這是可以機械驗證的；素材批次處理是一個——尺寸、命名、匯出格式，全部有標準答案；每天的工具情報巡檢也是一個——掃過官方 release notes 跟部落格，整理成摘要等你起床看。這些工作的共通點是重複、可長時間跑、成功條件講得清楚，正是迴圈划算的三個前提。

“

「好看」沒有 compiler。

品味那一關，迴圈跑完之後還是回到你眼前

而掃不進去的部分，恰好就是設計師的本體。層級的取捨、風格的判斷、這個版面「對了」的那個瞬間——這些沒有測試可以跑，沒有 linter 會亮綠燈。所以設計師版的 loop engineering，第一課是拆分：把有標準答案的環節丟進迴圈，把沒有標準答案的環節牢牢留在自己手上。哪些判斷能交出去、哪些不能，我畫過 [一張判斷地圖](https://rar.design/posts/ai-design-workflow-judgment-map) ，那篇的邏輯放到迴圈時代反而更鋒利了——因為迴圈會把「交出去」這件事的規模放大十倍，你切錯的成本也跟著放大十倍。

我自己的體感是：迴圈時代對設計師最大的紅利，是你終於可以用「產線」的方式思考個人工作。以前只有工程團隊養得起 CI，現在一個人加幾支 skill、幾個排程，就能讓重複的檢查與整理在你睡覺時自己轉。真正稀缺的能力從動手變成拆解——你能不能把自己的工作流拆成「可驗證」與「需要我」兩堆，決定了迴圈是你的槓桿還是你的噪音。

CHAPTER 06 · LIMITS

## 冷水：你可能還不需要迴圈

熱潮的第一週，反方的聲音就出來了，而且講得有道理。第一盆冷水是成本。迴圈本質上是用 token 換你的注意力，每個 sub-agent 都是一份完整的模型與工具開銷，長迴圈燒錢是安靜的。Osmani 自己在命名文章裡就先講了醜話：token 富人跟 token 窮人跑同一個迴圈，體感會是兩個世界。

第二盆是必要性。當週流傳最廣的反方文章標題就叫「多數開發者現在還不需要 agent 迴圈」——很多任務，開一個互動 session 直接做，比架一整套迴圈更快也更安全。迴圈划算的前提很窄：工作重複、能長時間無人跑、成功條件可驗證。三個條件缺一個，你架的就不是產線，是一台會自己花錢的裝置藝術。

第三盆最深。迴圈出貨越快，「存在的東西」跟「你真正理解的東西」之間的缺口長得越快——Osmani 叫它 comprehension debt，理解債。他還警告了一個更安靜的滑坡：當迴圈自己會跑，最舒服的姿勢就是不再有意見，照單全收。他那句話值得抄在牆上：同一個迴圈，兩個人用，一個拿來在自己深刻理解的工作上跑更快，一個拿來逃避理解工作本身。迴圈分不出這兩者的差別，但你分得出。

CHAPTER 07 · TAKEAWAYS

## 蓋你的迴圈，留在判斷的位置

650 萬

引爆話題的貼文瀏覽數

5＋1

迴圈構件＋外部記憶

297 美金

Ralph loop 交付五萬美金 MVP 的成本

把整篇收成三句話。第一，loop engineering 的本質是槓桿點的第四次搬家——從措辭，到資訊，到環境，到循環。第二，它比 prompt engineering 難而非簡單，因為你要負責的從一句話變成一個系統：目標怎麼定、回饋誠不誠實、什麼時候停。第三，對設計師與創作者，正確的入場方式是小：先架一個每天跑的情報巡檢，或一個交付前的規格檢查，驗證那一環先放自己的眼睛，跑順了再談放手。

Osmani 給這波熱潮的收尾我很喜歡，借來放在這裡：蓋你的迴圈，但用一個打算繼續當工程師的姿態去蓋。翻譯成設計師的語言——蓋你的迴圈，但留在品味與判斷的位置上。按下開始鍵的人很多，知道迴圈該往哪轉的人，一直都很少。

## 延伸閱讀[Claude Code 設計師完整指南：Agentic Loop、CLAUDE.md 與五大功能](https://rar.design/posts/claude-code-complete-guide-for-designers)

[

單一 agent 迴圈的入門版，從這裡開始最順 →

](https://rar.design/posts/claude-code-complete-guide-for-designers)[

Claude Code 完整教學：能做什麼、怎麼安裝、設計師可以怎麼用

迴圈跑在哪個工具上，先把工具本體摸熟 →

](https://rar.design/posts/claude-code-complete-guide-2026)[

Codex 為什麼突然紅了？500 萬週活與最新更新全解析

迴圈的另一半戰場，構件跟 Claude Code 幾乎一對一 →

](https://rar.design/posts/codex-why-popular-2026)[

Claude 三大目錄怎麼裝？Skills、Connectors、Plugins 必裝清單

迴圈的第三與第四號零件，先把知識與連接器備好 →

](https://rar.design/posts/claude-directory-skills-connectors-plugins-essentials)[

設計師到底該怎麼用 AI？一張判斷地圖

哪些判斷能交給迴圈、哪些必須留在自己手上 →

](https://rar.design/posts/ai-design-workflow-judgment-map)[

Claude Cowork 怎麼用？新手入門完整教學與設計師實戰

非工程師要跑排程與自動化，從 Cowork 進場最低摩擦 →

](https://rar.design/posts/claude-cowork-beginner-guide)

## 官方資源

想讀第一手材料，順序建議這樣走：先讀 Addy Osmani 的命名文章 [Loop Engineering](https://addyosmani.com/blog/loop-engineering/) ，五構件的原始定義都在裡面；再回頭補 Anthropic 的 [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) ，evaluator-optimizer 這些迴圈模式的學名出自這裡；最後去看 Geoffrey Huntley 的 [Ralph 原始文章](https://ghuntley.com/ralph/) ，感受一下一年前這一切還是 bash 土炮時的樣子。[SUBSCRIPTION](https://rar.design/courses/aiwaken)

[

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

## 常見問題

### Q：Loop Engineering 一句話是什麼？

A：設計一個會自己 prompt AI agent 的系統——自動觸發、行動、驗證、決定下一步，達成可檢查的條件才停，而非由你一句一句手動下指令。

### Q：這個詞是誰提出的？

A：2026 年 6 月初，開發者 Peter Steinberger 的貼文主張「別再 prompt agent，去設計 prompt 它的迴圈」，幾天內衝破 650 萬瀏覽；Google 工程師 Addy Osmani 隨即發表同名文章正式命名並給出五構件的結構。Claude Code 創造者 Boris Cherny 的「我已經不 prompt Claude 了」則把這件事從觀點變成了現場報告。

### Q：它跟 Prompt Engineering、Context Engineering 差在哪？

A：prompt 管你怎麼說、context 管模型看到什麼、harness 管 agent 活在什麼環境，loop 管什麼循環驅動 agent 往目標走以及何時停。四層是嵌套關係——你還是要寫 prompt、管 context，迴圈只是讓它們全部動起來的最外層。

### Q：一定要會寫程式才能做迴圈嗎？

A：一年前是，現在門檻低很多——排程、skills、連接器、sub-agent 這些構件已經內建在 Claude Code、Codex 與 Cowork 這類產品裡。真正的門檻從寫 bash 變成拆解工作流：你要能講清楚目標、驗證方式與停止條件。

### Q：怎麼避免迴圈跑不停、燒光 token？

A：多層出口一起上：可驗證的完成條件、迭代次數上限、token 與時間預算，以及連續數步毫無進展就中斷交回人類的偵測。沒有明確終止設計的迴圈，是這個領域最常見也最貴的錯誤。

### Q：設計師現在該做什麼？

A：從一個小迴圈開始：一個每天自動跑的工具情報巡檢，或一個交付前的規格檢查。把有標準答案的環節交給迴圈，驗證那一環先放自己的眼睛，跑穩了再逐步放手。品味與取捨留在自己手上——那是迴圈接不走的部分。

目前尚無留言