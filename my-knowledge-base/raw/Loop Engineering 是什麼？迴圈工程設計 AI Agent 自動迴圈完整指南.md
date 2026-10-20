---
title: "Loop Engineering 是什麼？迴圈工程設計 AI Agent 自動迴圈完整指南"
source: "https://seo.whoops.com.tw/loop-engineering/"
author:
  - "[[Sliven 褚崇名]]"
  - "[[作者]]"
published: 2026-06-22
created: 2026-06-22
description: "Loop Engineering 迴圈工程是什麼？跟 prompt engineering 差在哪？本文用台灣讀者視角拆解 AI Agent 迴圈設計：三個適合檢查、七個零件、開放 vs 封閉迴圈，以及避免失控燒錢的三條護欄。"
tags:
  - "clippings"
status: processed
---
Loop Engineering（迴圈工程）是一種新的工作方式： **你不再自己一個一個下 prompt 給 AI，而是寫一個小程式** ，讓它代替你去派工、讀結果、打分數、再決定要不要繼續跑。根據 2026 年 6 月這波討論裡 Claude Code 作者 [Boris Cherny](https://x.com/bcherny) 的說法，AI 已經從你對面的聊天對象，變成你程式裡的一個函式呼叫。這不是未來式，是現在進行式。

Loop Engineering，中文可以叫「迴圈工程」或「迴圈設計」。loop 就是迴圈，在程式裡指的是 **「重複執行、直到某個條件成立才停」** 的結構。把它套到 AI 代理（AI agent）身上，意思就是你設計一套會自己循環的系統，讓 [Claude](https://seo.whoops.com.tw/claude-ai-tutorial) 、Codex 這類 coding agent 在裡頭自動跑，而不是你盯著螢幕一句一句餵它。

這個觀念在 2026 年中突然爆紅，但它的底層邏輯其實不新。只要你做過任何「重複、可以驗收、值得自動化」的工作，你遲早會碰到同一個問題： **到底是我來開 AI，還是我寫個東西去開 AI？** loop engineering 就是後者的正式名字。

> **TL;DR：重點先看**
> 
> • **核心觀念** ：loop engineering 是抽象層的升級，從「下 prompt」到「設計會自己跑 prompt 的系統」。AI 從聊天夥伴變成程式裡的函式。  
> • **最難的不是跑，是停** ：一個不會停的迴圈不是自動化，而是會自己燒錢的無底洞。Uber 甚至把每位工程師每月的 AI 帳單上限設在約 1,500 美元（根據公開報導）；三條護欄（次數上限、diff 檢查、成本上限）缺一不可。  
> • **誰該看** ：不只是工程師。內容產製、SEO 監測、競品追蹤，只要符合「重複、可驗收、值得」三條件，都會被迴圈化。

## Loop Engineering 是什麼？一句話講清楚

![Loop Engineering 將 AI agent 放進行動、觀察、推理、重來與停止條件的自動迴圈。](https://seo.whoops.com.tw/wp-content/uploads/2026/06/loop-engineering-ai-agent-loop-cycle-whoops-seo.webp)

Loop engineering 的核心不是多寫一句指令，而是讓 agent 在可驗收的循環裡自己推進。

用最白話的說法：過去這兩年，用 AI 寫東西、寫程式，感覺像在打一場很累的乒乓球。你發一球（下指令），等它回，瞄一眼回球，調整一下，再發下一球。 **你的注意力就是那台發球機** 。你一離開鍵盤，AI 就卡在那一拍，什麼都不會自己做。

loop engineering 是你決定不再當那台發球機的那一刻。你寫一段小程式，有時候是幾行 shell 迴圈，有時候是一個排程任務，有時候是幾百行 TypeScript。這段程式才是去跟模型說話的角色。 **它挑下一個任務、派出去、讀回來打分數、記下結果，然後判斷要不要再跑一輪** 。模型不再是聊天的對手，而是你程式在 `while` 迴圈裡呼叫的一個函式。

不管迴圈長成什麼樣子，正中央都是同一個四步循環： **行動、觀察、推理、重來** （act、observe、reason、repeat）。AI 做一件事，讀回發生什麼，對照目標判斷這代表什麼，再決定要不要再來一次。其他所有零件，排程、工作樹、子代理、狀態檔，都只是包在這個循環外面的鷹架。

## 為什麼半個科技圈突然在吵這個詞？

2026 年 6 月，一句短短的話把科技圈的討論區整個點燃。PSPDFKit 創辦人 Peter Steinberger 在 6 月 7 日貼出：「你不該再自己下 prompt 給 coding agent 了。你該設計會去下 prompt 的迴圈。」這則推文衝破約 220 萬次觀看，留言區隨即變成戰場，大家吵的完全是同一個問題：這句話到底在說什麼。

![](https://x.com/i/status/2063697162748260627)

圈子外的反應一樣兩極。在一個問「loop engineering 是不是下一個 AI 開發流行語」的 [Reddit 討論串](https://www.reddit.com/r/myclaw/comments/1u047p8/so_is_loop_engineering_the_next_ai_dev_buzzword/) 裡，開發者兩邊倒：有人認定這真的是下一層抽象，也有人酸這只是「一頂戴在排程工作頭上的帽子」，本質跟傳統的 cron job 沒兩樣。你會發現，大家不是在吵技術，是在吵「這到底算不算新東西」。

把觀念講得最乾淨的，反而是早就默默在做的人。打造 Claude Code 的核心成員 Boris Cherny，在 Steinberger 貼文前兩天就先在台上說了同一件事：「我現在不下 prompt 給 Claude 了。我有迴圈在跑。是它們在替我下 prompt、替我想下一步。我的工作，是寫迴圈。」這段話出自他的一場完整訪談，整段脈絡比單句更有料：

![](https://www.youtube.com/watch?v=RkQQ7WEor7w)

把這幾段話擺在一起，你會看到同一個訊號從不同方向冒出來：能夠自己跑、自己檢查、自己決定要不要繼續的 AI 工作流，正在從「少數人的酷把戲」變成「正規的工作方式」。Anthropic 自己的人、獨立的開發者、還有各家寫程式的 AI（從 OpenAI Codex、Claude Code 到 Cursor）都在往同一個方向移動。這也是為什麼它值得你花十分鐘搞懂，而不是當成又一個會過期的流行語。

## 三層樓比喻：prompt → harness → loop

![Prompt、harness 與 loop 三層抽象，說明迴圈工程建立在代理環境之上。](https://seo.whoops.com.tw/wp-content/uploads/2026/06/loop-engineering-prompt-harness-loop-layers-whoops-seo.webp)

prompt、harness、loop 是往上疊的三層抽象；loop 不是取代前兩層，而是把它們包進會重複運轉的系統。

Google 工程師 [Addy Osmani](https://addyosmani.com/blog/loop-engineering/) 給了一個很好用的框架，他把這幾年的演進想成一棟三層樓。理解這個比喻，你就不會把幾個名詞混在一起。

- **一樓，prompt engineering（提示詞工程）** ：你把一個 prompt 寫好，交給模型，拿一個答案。焦點是「這一次」。
- **二樓，agent harness engineering（代理環境工程）** ：你設計單一個 agent 跑在裡面的那個環境，它有哪些工具、能看到什麼、怎麼被限制。harness 還在，這層依然重要。
- **三樓，loop engineering（迴圈工程）** ：迴圈跑在 harness 上面，依排程啟動，會生出 helper、會餵養自己。你設計的不再是一個 agent，是一整個會反覆運轉的系統。

關鍵在於：樓層是往上疊的，前一層並不會被取代。你還是需要寫好的 prompt，也還是需要好的 harness；loop engineering 只是把整套東西包進一個會自己重來的程序裡。很多人之所以覺得它「沒什麼新鮮」，是因為只盯著一樓看， **沒看到三樓那一層循環邏輯才是主角** 。

## prompt engineering、agentic workflow、loop engineering 到底差在哪？

這三個詞常被混著用，但不該混。如果你正在查 **prompt engineering vs loop engineering** 到底差在哪，差別其實就一張表的事。搞清楚之後，你才知道自己手上的任務到底屬於哪一層，該用哪種方式對待。

| 面向 | prompt engineering | agentic workflow（代理工作流） | loop engineering |
| --- | --- | --- | --- |
| 你做的事 | 為單一回合寫好指令 | 把多步驟呼叫串起來、帶分支 | 設計「會替你下指令的系統」 |
| 自主程度 | 零，你全程在場 | 中等，步驟先定好，模型填空 | 高，依排程跑到停止條件成立 |
| 最適合 | 一次性任務、內容、簡單查詢 | 形狀已知的多步驟流程 | 路徑未明的開放式、反覆型工作 |
| 你優化的是 | 那一段 prompt | 那條鏈 | 迴圈本身，以及裡頭的評分標準 |

最後一列是重點。agentic workflow 你優化的是「流程怎麼串」，loop engineering 你優化的是 **「循環怎麼轉、裡面的尺規怎麼定」** 。說穿了， **loop 的成敗，取決於你寫進去的那把尺，跟模型有多強關係不大** 。這也是後面會談到的最大風險來源。

## 你的任務真的適合做成迴圈嗎？三個檢查

![判斷任務是否適合迴圈工程，需要通過重複性、可驗收與有價值三個檢查。](https://seo.whoops.com.tw/wp-content/uploads/2026/06/loop-engineering-task-fit-checks-whoops-seo.webp)

不是所有工作都值得做成迴圈；重複性、可驗收與有價值三關缺一不可。

知道分類是一回事，知道自己手上的任務屬不屬於這一類是另一回事。在動手寫迴圈之前，先把候選任務丟過三個檢查， **全部通過才值得投入，缺一個就老實用手動 prompt 或寫個普通腳本** 。

- **重複性** ：這件事你做得很頻繁，頻繁到設計系統的成本會被攤平。一年做三次的事，不值得寫迴圈。
- **可驗收** ：「做完了」可以被寫成一個 agent 或驗證子代理實際能跑的檢查。如果你講不清楚「過關長什麼樣」，迴圈就不會知道什麼時候該停。這條是最多人忽略、也最致命的一條。
- **有價值** ：產出值得那些 token。迴圈有時間和金錢的地板成本，瑣碎的工作根本跨不過去。

反過來說，哪些工作「不該」做成迴圈？一次性、無法客觀驗收、或產出價值低於成本的任務， **硬做成迴圈只會多一個要維護的東西** 。判斷方法和做 [搜尋意圖](https://seo.whoops.com.tw/search-intent) 分類很像：先問「這件事的本質是什麼」，再決定工具，而不是先有錘子再把所有東西都看成釘子。

## 一個能上線的迴圈，需要哪些零件？

![能上線的迴圈工程系統由觸發器、隔離、技能、品檢、連接器與記憶等零件組成。](https://seo.whoops.com.tw/wp-content/uploads/2026/06/loop-engineering-production-loop-components-whoops-seo.webp)

正式環境裡的迴圈不只是 while loop，還要有觸發、隔離、技能、品檢、連接器與記憶。

任何一個能在正式環境活下來的迴圈， **解的都是同一組問題** ：什麼時候跑、多個 agent 怎麼不互相踩腳、它們帶了哪些知識、工作怎麼切分與驗證、迴圈怎麼接上你其他的系統、怎麼交給同事、每次跑之間記住了什麼。Codex 和 Claude Code 對這些零件叫法不同，但責任對得起來。這裡用一個餐廳廚房來比喻，比較好記。

- **觸發器** ：總要有個「不是你打字」的東西啟動它。在 [Codex](https://seo.whoops.com.tw/openai-codex) 是排程分頁，在 Claude Code 是排程任務、hook 或 GitHub Action 的組合。它必須綁著一個停止條件一起出廠。
- **並行的隔離** ：兩個 agent 同時碰同一份程式碼，只會換來兩種下場，要嘛靜靜互相覆蓋，要嘛合併衝突打到崩潰。git worktree（工作樹）給每個 agent 自己的分支與工作副本，解掉這個問題。不隔離就並行，等於你親手寫了一個競態狀態。
- **寫下來的脈絡** ：一份 skill（技能資料夾，裡頭是 SKILL.md 加需要的腳本）是專案知識在每次執行之間活下去的方式。模型在描述對上任務時自動載入它。訣竅是把那份描述寫得像標語，不是像宣言。
- **切分工** ：最快回本的迴圈模式，是把「寫的人」和「審的人」拆開。一個 agent（或一串子代理）負責改，另一個、通常用更精簡模型的，負責對照規則和測試打分數。品檢員不必比廚師聰明，它只要「不一樣」，有自己的指令和自己的尺規就夠了。
- **連接器** ：鎖在工作目錄裡的迴圈，只是個比較華麗的腳本。連接器讓它夠得到外面的世界：issue 系統、staging API、資料庫、Slack、監控、即時網頁。這些多半跑在 MCP（Model Context Protocol）上，像 [Claude Desktop](https://seo.whoops.com.tw/claude-desktop) 就是用同一套協定來掛工具，你在某個工具設好的連接器，常常換到另一個工具就能直接用。
- **外掛** ：比連接器再上一層，把一組連接器和「會用它們的技能」綁在一起，讓同事一行指令就能裝好你整套迴圈，而不是去你的 repo 裡逆向工程。連接器是線路，外掛是線路加上說明書。
- **活過程式的記憶** ：以上全部，在迴圈記不住昨天做過什麼時都白搭。形式刻意選得無聊：repo 裡一份 markdown 清單、旁邊一個 JSON 檔、一個看板、一個資料庫。挑一個，每次開跑讀它、跑完追加它。這是迴圈撐過當機、context window 重置、凌晨三點被砍程序的辦法。

## 開放迴圈 vs 封閉迴圈：你該選哪一種？

![開放迴圈適合探索未知路線，封閉迴圈適合正式環境中的檢查點與停止條件。](https://seo.whoops.com.tw/wp-content/uploads/2026/06/loop-engineering-open-closed-loop-comparison-whoops-seo.webp)

開放迴圈適合探索，封閉迴圈適合交付；一旦進正式環境，邊界與停止條件會比自由度更重要。

[Shann Holmberg](https://x.com/shannholmberg) 畫出了一條我認為最實用的分界線： **開放對上封閉** 。這條軸直接決定你該把迴圈塑成什麼形狀。

![](https://x.com/i/status/2063924108535197842)

| 面向 | 開放迴圈（open loop） | 封閉迴圈（closed loop） |
| --- | --- | --- |
| 契約 | 給目標和護欄，路線自己挑 | 先畫好路線、每步驟驗收、明確停止條件 |
| 適合 | 原型、未知地形、摸索階段 | 正式環境、可預測成本、穩定品質 |
| 代價 | 標準一模糊，產出大多是噪音 | 前期設計成本高 |
| 成本 | 不可預測，容易爆 | 可控，且會因固定尺規而越跑越好 |

用腳踏車來比喻。開放迴圈像不裝輔助輪就直接騎下山，速度快、刺激，但你對路線完全沒掌握，標準一模糊就摔。封閉迴圈像先畫好路線、每個彎設檢查點，你照著騎，速度普通但穩。 **在正式環境裡，封閉迴圈預設就是贏家** ：讓一個迴圈負責目標、步驟交給專科、粗活下推給子代理、用自動檢查決定什麼過關。開放迴圈只在你連「這工作是什麼」都還沒搞清楚、或純粹想燒 token 探索的時候才值得開。

## 為什麼「讓迴圈停下來」才是最難的事

![Loop Engineering 需要次數上限、diff 檢查與成本上限，避免 AI 迴圈無限運轉。](https://seo.whoops.com.tw/wp-content/uploads/2026/06/loop-engineering-stop-guardrails-cost-control-whoops-seo.webp)

一個不會停的迴圈不是自動化，而是風險來源；次數、diff 與成本上限要從第一天就裝上。

浪漫版的 loop engineering 是這樣：你寫好迴圈，一千個 agent 一夜之間幫你把公司蓋起來。真實版是這樣：你寫好迴圈， **然後你大半的工作，是確保它們會停** 。

走進任何一個相關討論串，多數開發者第一個擔心的不是架構，是錢。同一個 Reddit 討論串裡最大聲的回覆，全在講失控的花費：有人貼出迴圈 overnight 默默啃掉幾百塊美金的收據，問這種東西除了公司帳戶誰養得起。根據 [TechCrunch 報導](https://techcrunch.com/2026/06/02/uber-caps-employee-ai-spending-after-blowing-through-budget-in-four-months/) ，Uber 在四個月內燒完整年度 AI 預算之後，把每位工程師、每個工具、每月的上限設在約 1,500 美元。費用從「寫程式」搬到了「跑那個會寫程式的東西」上。

修法毫不浪漫，而且沒得商量： **每一個上線的迴圈，都要帶著硬護欄，不然就別上線** 。今年的實務文章反覆落在同一組三條護欄。

- **次數上限** ：一個硬性天花板，卡住的迴圈才不會一直空轉。
- **diff 檢查** ：一旦最近幾輪的產出不再有任何變化，就砍掉這次執行。
- **成本上限** ：以 token 或金額計，在帳單結算前先結束執行。

三條缺一，你跑的就不是迴圈，是一張開著的發票。

第二個風險更安靜，也更大。迴圈用機器速度送出你沒寫的程式碼，你 repo 裡的東西和你真正理解的東西，中間的落差就越拉越開。Addy Osmani 稱之為 [comprehension debt（理解債）](https://addyosmani.com/blog/comprehension-debt/) ： **迴圈跑得越好，這筆債累積得越快** ，除非真的有人去讀那些進來的 diff。驗收是你的事。「做完了」是一個宣稱，不是一個證明。

第三個風險，是沒有人會事先提的那個：一個背後沒有品味的迴圈。Greg Brockman 講過更廣的版本：隨模型越來越強，產出的瓶頸不再是模型，而是指揮它那個人的品味。這句話幾乎可以一字不改套到迴圈上。迴圈會放大你寫進評分標準、技能、驗收步驟裡的任何判斷。這三者得反映真正的品味：在你的程式庫裡「好」是什麼意思、哪些邊界情況才算數、什麼時候東西真的可以上線、什麼時候只是技術上過關。

當這三者到位，迴圈會朝對的方向複利。當它們不到位，你只是用更快的速度，送出根本不值得做的工作：更多沒人讀的文件、更多沒人要的 PR、更多對著錯誤根因自動關掉的工單。 **迴圈讓你可以用機器速度犯錯。** loop engineering 厚待本來就有品味的人； **harness 是容易抄走的部分，裡頭那把尺，才是真正屬於你的部分** 。

## 不只是工程師的事：哪些工作正在被迴圈化

到這裡，你可能覺得 loop engineering 是寫程式的人的事。這誤會大了。說白了，它的本質就是 **讓 AI 自己跑、自己檢查、自己決定何時收手** ，跟你的職稱無關。凡是會反覆出現、講得出過關標準、值得花成本的工作，都在被迴圈化，而且很多就在你每天都碰的領域裡。

以內容產製為例。一篇符合規格的文章要經過選題、查資料、寫初稿、自我審查、改寫、過機械檢查，這本身就很接近一個單一 agent 的「研究、草擬、自審、修改」小迴圈。你平常追求的那種 [10 倍內容](https://seo.whoops.com.tw/how-to-create-10-times-worth-of-content) ，從來就不是一次寫到位，而是反覆打磨出來的，這正是迴圈形狀的工作。再把範圍放大，一個調度器把目標拆成區塊、分派給不同專科，每個專科再用自己的 helper，這就是大尺度的迴圈樣貌。機制一模一樣，差別只在「人數」。

這裡有個迷思要破。很多人以為把內容交給迴圈自動跑，就是 [拼命堆字數](https://seo.whoops.com.tw/word-count-for-seo) 量產。剛好相反。 **迴圈最大的價值，是每次產出都被同一把尺檢查，不在於產出更多** 。沒有尺的迴圈，只會用機器速度製造沒人要看的東西，這也是前面「品味」那段反覆強調的重點。換句話說，能把內容 [涵蓋整個購買週期](https://seo.whoops.com.tw/successful-seo-planning-purchase-cycle) 的，從來是那把尺，不是產出速度。

SEO 與 AI 搜尋的監測也是同一回事。追蹤關鍵字排名變化、定期跑 [AEO](https://seo.whoops.com.tw/what-is-aeo) 與 [GEO](https://seo.whoops.com.tw/what-is-geo) 體檢、把處理組與對照組拿來比對成效，這套流程本來就是週期性的，天然適合排成迴圈。差別只在於：過去是人工每週開試算表手動跑，現在可以是排程自動跑、自動產生摘要、再由人下判斷。這也是為什麼連 [AI SEO](https://seo.whoops.com.tw/ai-seo) 的工作本身，都在往「設計迴圈」移動，而不是「多按幾次 AI」。把 [SEO 的基本功](https://seo.whoops.com.tw/what-is-seo) 顧好、把 [技術 SEO](https://seo.whoops.com.tw/technical-seo) 的地基打穩，這些事一旦穩定，就是第一批最該被迴圈化的候選。

再往 AI 搜尋那頭看，迴圈化的腳步更快。Google I/O 2026 之後， [搜尋本身改用 AI Agent](https://seo.whoops.com.tw/google-io-2026-search-agents-seo) 在跑， [Google AI Mode](https://seo.whoops.com.tw/google-ai-mode) 和 [AI Overviews](https://seo.whoops.com.tw/what-is-google-ai-overviews) 成了新的曝光戰場，連帶讓 [AISO](https://seo.whoops.com.tw/ai-search-optimization-aiso-strategies) 這類新觀念冒出來。當連搜尋引擎自己都在用 agent 反覆運作，你這邊還停在「手動按一次 AI」， **落差只會越拉越大** 。想 [靠 AI 帶流量](https://seo.whoops.com.tw/increase-website-traffic-with-ai-seo) ，遲早要學會設計迴圈，而不是更用力地手動下指令。

再往上一層，是把「即時網頁」當成迴圈的心跳。你讓一個監控端點盯著某個網址，內容一變就觸發：競品定價頁改了，啟動一輪競爭回應；API changelog 更新了，啟動一條文件改寫流程；狀態頁出事，喚醒值班 agent。迴圈不再照固定排程輪詢，而是「真的有新東西」才跑。這條路跟 [llms.txt](https://seo.whoops.com.tw/llms-txt-guide) 、 [結構化資料](https://seo.whoops.com.tw/what-is-structured-data) 想解的問題是同一個方向：讓 AI 拿到的是 [搜尋引擎實際抓到](https://seo.whoops.com.tw/google-search) 的、網路上「真的在那裡」的資料，而不是模型記憶裡的猜測。

說到底，loop engineering 的精神可以套回你既有的判斷框架。你做 [關鍵字最佳化](https://seo.whoops.com.tw/keyword-optimization-guide) 、排 [內部連結策略](https://seo.whoops.com.tw/internal-link-optimization-strategy) 、追 [SEO 新趨勢](https://seo.whoops.com.tw/seo-trends) ，這些工作哪一個不是反覆發生、有明確驗收、而且值得投入？只要符合條件，它們遲早都會被裝進迴圈裡。這也是為什麼 [資深 SEO 工作者](https://seo.whoops.com.tw/expert-explain-seo) 會把重心從「做更多」搬到「設計會自己跑、自己停的系統」。差別只是你什麼時候動手。

## 起步建議：三步試水

**穩當的進場順序是先小後大、先封閉後開放，分三步** 。先別急著把整套生產線搬進迴圈，風險最低、回饋最快的試水方式是這三步。

- **第一步，挑一件你每週都在重複的小事** 。例如每週整理一次排名變化、定期把一批文章過同一組品質檢查。確認它同時通過重複、可驗收、值得這三關。
- **第二步，先做封閉迴圈** 。把步驟、驗收條件、停止條件全部先寫死，跑在排程上。 **從第一天就把護欄裝上去，哪怕你覺得用不到** 。
- **第三步，每天讀 diff** 。迴圈跑得越順，你越要親自看它產出了什麼， **別讓理解債默默長大** 。等這件小事穩定運作一兩週，再考慮放大範圍。

工具不是重點，原則才是。不管你最後用 Claude、Codex 或 [ChatGPT](https://seo.whoops.com.tw/chatgpt-tutorial) 系列的排程功能，能讓你真的用得起來、而且看得懂產出的，就是對的工具。也別忘了，這套觀念和你已經在做的 [SEO 改善](https://seo.whoops.com.tw/how-to-improve-seo) 、 [站內優化](https://seo.whoops.com.tw/what-is-on-page-seo) 是相容的，迴圈只是把你反覆在做的事，交給一個會自己跑、自己停、自己留紀錄的系統。如果你想找一條更完整的進場路線，本站的 [SEO 新手入門](https://seo.whoops.com.tw/seo-getting-started) 和 [年度 SEO 權威指南](https://seo.whoops.com.tw/seo-for-this-year) 都把基礎打底的工作拆成了可照做的順序，正好適合當作第一批被迴圈化的候選任務。等你摸熟了，回頭看那句引爆討論的話，你會發現它沒那麼玄：它只是把你早就該做的事，講出了一個名字。

## 常見問題（FAQ）

### Q1：Loop engineering 到底是什麼？

它是一種工作方式：你設計一套會自己循環的系統，讓 coding agent 在裡面自動跑，而不是你自己一句一句下 prompt。你寫的小程式負責挑任務、派工、讀結果、打分數、決定要不要再來一輪，模型變成你程式裡的一個函式呼叫。中央那個四步循環是行動、觀察、推理、重來。最常見的起點，是把一件你每週都得手動重做一次的事，排成一個會自己跑的定時任務。

### Q2：Loop engineering 和 prompt engineering 差在哪？

prompt engineering 優化的是單一回合：一個輸入、一個輸出、一個人坐在椅子上。loop engineering 優化的是一個能在你不在場時跑很多回合的系統。迴圈自己決定下一步問什麼、讀結果、對照目標，然後繼續或停止。換個數字感：你手動一個晚上頂多來回五、六輪，一個排好的迴圈可以跑幾百輪，你早就下班了。

### Q3：開放迴圈和封閉迴圈有什麼不同？

開放迴圈給 agent 一大片探索空間，有目標但限制很鬆，可以走你沒指定的路。它對探索很有力，對你的 token 預算很殘酷。封閉迴圈是有邊界的：路線先定好、每步驟有驗收、停止條件明確。封閉迴圈更便宜、更可預測，是正式工作的預設選擇。一個實用的切法是：還在摸索問題長什麼樣時開放，一旦要把成果交付出去，就切成封閉。

### Q4：做 loop engineering 需要新工具嗎？

不需要全新工具，你需要的是新觀念。Codex 的排程分頁、Claude Code 的排程任務與 hook、git worktree、MCP 連接器，這些基本零件都已在主流工具裡，其中 git worktree 你現在的開發環境多半就內建了。真正要你花心力準備的，是寫進迴圈的那把尺：評分標準、技能檔、驗收步驟，這些才是決定成敗的部分。

### Q5：Loop engineering 會不會很燒錢？怎麼避免失控？

沒裝護欄的迴圈確實會默默燒錢，這也是實務上被問最多的問題，Reddit 上不乏 overnight 默默燒掉幾百美金帳單的真實案例。標準做法是三道上限全上：限定最多跑幾次、跑不動（產出不再變化）就停、花到一定金額就收手。三道缺一，它就會變成失控的成本來源，而不是省力的自動化。

### Q6：我不是工程師，loop engineering 跟我有關嗎？

有關。凡是「會反覆發生、可以客觀打分、值得花那些 token」的工作，都會被迴圈化。內容產製的選題、查資料、寫稿、自審、改寫是迴圈；SEO 與 AI 搜尋的排名追蹤、AEO 體檢、 [AISO](https://seo.whoops.com.tw/what-is-aiso) 監測、成效對照也是迴圈。差別只在過去人工跑，現在可以排程自動跑、再由人下判斷。觀念是通用的，工具會越來越親民。