---
title: "Claude Design 完全指南:Anthropic 設計師公開一人支援 7 個產品的 8 個工作流"
source: "https://rar.design/posts/claude-design-complete-guide"
author:
  - "[[設計師 Riven]]"
published: 2026-04-19
created: 2026-05-25
description: "2026年 4 月 17 日 Anthropic 發布 Claude Design,用對話就能做出原型、簡報、UI mockup。這篇整理 Anthropic 內部設計師 Ryan Mather 的 8 個實戰工作流,對照 Brilliant、Datadog 早期使用案例,幫你決定要不要把 Clau..."
tags:
  - "clippings"
status: processed
---
從 Figma 股價應聲下跌、Brilliant「20 prompts 變 2 prompts」,到 Anthropic 垂直產品設計師 Ryan Mather 的真實工作流拆解

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年4月19日 下午 9:52

AI 設計

![Claude Design 官方產品介面示範](https://www-cdn.anthropic.com/images/4zrzovbb/website/499e91975d880b35eac6e48ad43161de7d10416c-2876x1614.jpg)

Claude Design 官方產品介面示範

Table of Contents · 目錄

[01Claude Design 到底是什麼？](#chapter-01)

[02為什麼這組技巧值得學？](#chapter-02)

[03Anthropic 設計師的 8 個工作流](#chapter-03)

[04怎麼開始用？四個步驟](#chapter-04)

[05什麼時候不要用？誠實評估](#chapter-05)

[06對一人創作者意味什麼？](#chapter-06)

[07一分鐘懶人包](#chapter-07)

2026 年 4 月 17 日，Anthropic 發布 Claude Design——用對話就能生 prototype、Landing Page、UI mockup 的設計工具。

同一天，Anthropic 內部設計師 Ryan Mather 在 X 上發了一則 thread，寫他怎麼用 Claude Design 一個人同時支援 Anthropic 旗下 7 個產品。這則推文兩天內累積 133 萬次觀看、1.7 萬書籤——書籤比讚更多，代表大家是拿來當工作手冊存的。

這篇要做的事:拆解 Claude Design 是什麼、用 Brilliant 和 Datadog 的數據說明這組技巧為什麼值得學、逐條整理 Ryan 的 8 個實戰工作流、說明什麼時候不該用它、以及作為一人經營多產品的創作者，這工具到底值不值得把你的工作流改掉。

Chapter 01

What is it

Claude Design 到底是什麼？

**Claude Design** 是 Anthropic Labs 推出的對話式設計工具——你用自然語言描述要做什麼，Claude 直接生成可互動的原型、簡報、Landing Page、UI mockup。

它由 Claude Opus 4.7 的視覺模型驅動，目前以研究預覽形式開放給 Pro、Max、Team、Enterprise 訂閱戶使用。進入點在 claude.ai/design。

介面很單純——左邊聊天、右邊畫布。你描述要什麼，Claude 在畫布上生出來，然後你可以用三種方式修:對話、點擊元件直接評論、或用 Claude 現場生出來的滑桿調整 spacing、顏色、圓角。

關鍵差異在這裡:**它不是在「畫」設計稿——它是在寫 code** 。Ryan Mather 在 thread 留言回覆時說得很直接:「它是用 code 生的。」這件事的含義之後會展開，但先記住這個本質差異。

Quick comparison

面向

Figma / Canva

Claude Design

操作方式

拖拉、點選

對話、評論

產出本質

像素/向量圖

可互動的 code

設計系統

手動維護

讀 codebase 自動建

交付

工程師重做

直接轉 Claude Code

Chapter 02

Why it matters

為什麼這組技巧值得學？

Ryan Mather 不是經營社群的網紅設計師。他是 Anthropic 垂直產品團隊的實務設計師，一個人支援 7 個產品——這個位置決定了他分享的內容不是理論，是每天被壓力測試過的工作流。

這則 thread 會在兩天內被 1.7 萬人存成書籤，有三個可驗證的理由。

**一、Thread 本身的轉化率**

133 萬觀看、8,362 讚、1.7 萬書籤—— **書籤數是讚的兩倍** 。書籤在社群媒體代表「這段之後會回來看」，不是即時反應的讚。這個比例只會出現在「拿來當工作方法參考」的內容上。

**二、Brilliant 的數據**

教育科技公司 Brilliant 的互動課程以動畫、邏輯複雜聞名。他們的資深產品設計師這樣說:

Brilliant · Senior Product Designer

原本需要 **20 個以上 prompt** 才能在其他工具重現的複雜互動頁面，在 Claude Design 只需要 **2 個** 。

— Olivia Xu, Brilliant

**三、Datadog 的數據**

Datadog 的產品團隊則將一週的設計循環壓縮到一次會議內完成。

Datadog · Product Manager

過去需要一週來回的「簡報、mockup、review」循環， **現在壓縮成一次會議** 。

— Aneesh Kethini, Datadog

這些不是宣傳稿，是兩間中大型科技團隊的實際使用回饋。結論很一致:這組工作流在真實環境中可以被複製。

那他到底說了什麼？下一章 8 條工作流，逐條拆。先看 1 分 17 秒的官方影片了解工具本身:

![](https://www.youtube.com/watch?v=t_LBECIQQqs)

Introducing Claude Design by Anthropic Labs — @claude 官方頻道

Chapter 03

Inside Anthropic

Anthropic 設計師的 8 個工作流

Ryan Mather(@Flomerboy)是 Anthropic 的垂直產品團隊設計師，RISD 出身，過去待過 Poetry Camera、Sudowrite、Google ATAP、littleBits。他目前一個人支援 Anthropic 旗下 7 個不同的產品——以傳統設計團隊人力比，這是不可能的。

以下是他在發布當晚公開的 8 個工作流，原文 thread 在 X 上累積 133 萬次觀看。我用台灣創作者的語言重新整理:

Workflow 01 · Foundation

**第一件事:花一小時設定設計系統和核心畫面**

Ryan 的第一條 tip 和所有 coding agent 的最佳實務撞上:「花一小時在設計系統和核心畫面的設定和精修，絕對值得。」

有留言問他「核心畫面是什麼意思？」他回答:如果你在 Instagram 工作，你會希望 Claude 有一份像素級精確的 home screen、capture screen、DMs、Reels 拷貝。這樣當你說「我們想重新設計首頁，把焦點放到照片」,Claude 已經有起跑優勢。對創作者來說，等同於——你先把自己品牌網站的關鍵頁面 匯入 Claude，之後任何改版都從這個基礎長出來。

Workflow 02 · Collaboration

**當著工程師的面即時改設計**

Ryan 說他現在常常能在「一次會議裡」跟工程師一起設計完一個新功能。因為 Claude 生 mockup 夠快，他們可以保持在高抽象層級的對話——討論概念、談限制，然後看著東西被做出來。這是從前「設計師回去畫、兩天後再 review」的根本翻轉。

Workflow 03 · Editing

**用 Comment 工具做外科手術式修改**

初稿出來後，通常有幾十個細節你想調整。用文字描述位置很痛苦，所以別用——直接點、直接評論。這也是 Muzli 編輯在實測中最強推的功能:「targeted, component-level changes 用 Comment，結構性或美學變更用 chat」。

Workflow 04 · Media

**叫 Claude 幫你的想法做影片 demo**

這是一個反直覺點:Ryan 說 Claude Design 能做的事「幾乎是任何你能想到的」。它比起 canvas-based 的設計工具，行為上更像 Claude Code——也就是說它可以生出動態 demo、影片、互動玩具，而不只是靜態稿。

Workflow 05 · Automation

**用 connector 直接讀 Slack 和文件**

接上 connector 後，你可以下這種 prompt:「請讀產品 review 會議的筆記，針對提到的所有問題，各生一組不同的設計解法做成 deck。」

然後 Ryan 原文是:「去散個步、摸摸草，回來用新鮮的眼睛看結果。」這是典型的 async 工作流——你不再坐著等，你讓 AI 做背景運算。

Workflow 06 · Mindset shift

**讓 Claude 現場做客製工具**

Ryan 強調:「別用你以前用 canvas 工具的方式來用 Claude Design。它是不同的動物，有不同的超能力。實驗、玩起來——你會發現自己的設計速度比以前快一個級距。」這個 mindset 比任何具體功能都重要。

Workflow 07 · Restraint

**知道什麼時候要放慢、自己動手**

這是整串 thread 最老成的一條。Ryan 點名三件事還是該自己動手:**新圖示、重點插畫、命名** 。「有些細節永遠會有不成比例的影響。很容易被 agentic design 的速度感吸進去——知道什麼時候要放慢，是一種獨立的藝術。」

Workflow 08 · Delight

**允許自己嘗試更發散的想法，且更寬鬆地抓著它們**

Ryan 說他最喜歡 Claude Design 的不是效率——是它讓設計這件事「重新變得 delightful」。因為生成成本低，你可以試更發散的方向，也可以更寬鬆地放掉不對的。這和傳統設計工具的關係是相反的——用拖拉式工具的時候，你每個小時都在成本線上，因此會傾向收斂。

“

*Claude Design 讓設計的過程重新變得令人愉悅——我可以嘗試更發散的想法，也可以更寬鬆地抓著它們。*

— Ryan Mather,Anthropic

Chapter 04

How to start

怎麼開始用？四個步驟

前提:你需要 Claude Pro(每月 $20)以上的方案。Claude Design 的用量獨立計算，不會吃掉你的 chat 或 Claude Code 額度。Enterprise 預設關閉，需要 admin 手動開。

01

**進入 claude.ai/design**

或從 Claude 左側導覽列的調色盤圖示進入。初次使用會問「你是做什麼的？」——選 Design / Engineering / Product / Marketing / Other。

02

**建立你的設計系統**

上傳既有的網站截圖、logo、色票、或者指向你的 GitHub repo。Claude 會自動建出配色、字型、元件庫。Ryan 的第一條 tip:這裡值得花一小時打磨。

03

**建立第一個專案，寫清楚 prompt**

好 prompt 包含四要素:目標(要做什麼)、版面(要怎麼排)、內容(要顯示什麼資訊)、受眾(誰會用)。官方範例:「為我們的新 API 產品做一個 Landing Page，包含 hero、程式碼範例、訂價區。」

04

**用對話 + 評論迭代，然後 Export**

結構性變更用 chat，元件級調整用 inline comment。滿意後從右上 Export 選:ZIP、PDF、PPTX、送到 Canva、Standalone HTML、或 Handoff to Claude Code。

Chapter 05

Caveats

什麼時候不要用？誠實評估

Claude Design 目前是研究預覽，不是完成品。從 thread 下的真實留言，挑出四個值得你注意的現況:

Issue 01

**用量吃得很兇**

Thread 底下一位 Max 20 方案(每月 $200)使用者回:「花一小時設定就把我整週額度用完了。」Ryan 回覆「團隊正在看這個問題」。現階段如果你想認真探索，預期要開 Extra Usage。

Issue 02

**Pixel perfect 複製還不強**

有人問怎麼讓 Claude Code 從 mockup 生出像素級精確的 code。Ryan 的建議:「如果用 MCP，盡量給它 code 而不是截圖。4.7 比 4.6 明顯進步，但模型在複製截圖這件事上還沒到神乎其技。」

Issue 03

**不是 Figma 替代品——至少還不是**

如果你的團隊跑正式 design review、管大規模 design tokens、多人協作改同一個畫面，Figma 仍然是主場。Claude Design 目前更適合快速探索、個人 / 小團隊原型、溝通用的 mockup。

Issue 04

**Inline comment 偶爾會消失**

官方 Help Center 公開承認這個 bug——已知的解法是把 comment 內容貼回 chat。這類小 bug 在研究預覽期正常，但值得先知道。

Chapter 06

For creators

對一人創作者意味什麼？

Ryan 的 thread 底下，有一則被廣泛引用的觀察來自 @Long La:

*和 coding agent 一樣的 pattern:花一小時在設計系統和核心畫面，之後省下幾天的隨機輸出。魔法很少在第一個 prompt——而是在你給模型的邊界有沒有被嚴肅對待。*

— @Long La

這句話對一人創作者特別準。Ryan Mather 一個人支援 7 個產品的能力，本質上和「一個人同時經營多個課程、一個訂閱制、一份電子報、一個設計品牌」的結構是一樣的—— **單人多通路經營** 。

如果你是這種型態的創作者，Claude Design 改變的不是「畫得更快」這件事，而是這三件事:

01

**不用找前端工程師就能做 Landing Page**

新產品、新課程的登陸頁，從想法到 HTML 檔案在一個對話裡完成，直接部署到你現有的網站。過去這個迴圈需要:設計師→工程師→來回改。

02

**簡報和 One-pager 可以當日產出**

廠商合作提案、訂閱制介紹、年度回顧——你的品牌風格建一次，之後所有衍生物保持一致。Export 成 PPTX 或送到 Canva 繼續編。

03

**多產品之間可以共用同一個 Design System**

這是最關鍵的一點。如果你經營多個品牌或多個方向，Team / Enterprise 方案允許同時維護多套 design system，你的每個線路各自視覺一致，但不會混到彼此的風格。

要注意的是——Claude Design 不會取代你的設計品味。Ryan 的第 7 條工作流講得很清楚:新圖示、重點插畫、命名——這些「不成比例影響」的小細節，仍然該你自己動手做。

它取代的是重複的、機械的、可以用結構規則描述的那部分工作——而這部分工作，剛好是一人創作者過去被綁死在電腦前的最大原因。

Chapter 07

Cheat sheet

一分鐘懶人包

Claude Design · TL;DR

**是什麼** · 用對話做設計稿、原型、Landing Page、簡報的 AI 工具。由 Claude Opus 4.7 驅動，產出是可互動的 code，不是像素圖。

**發布日** · 2026/4/17，目前研究預覽，Pro($20/月)起開放。

**適合** · Solo founder、一人創作者、PM、需要快速探索的設計師、不會 Figma 但需要做視覺的人。

**不適合** · 大型 Design Review 流程、Pixel Perfect 複製既有網站、用量吃緊的低階方案。

**最佳實踐** · 第一小時別急著做東西，先打磨好 design system 和核心畫面。這一小時省下後面幾天。

**三種輸入** · 文字 prompt、上傳 DOCX/PPTX/XLSX、指向 GitHub repo 讓 Claude 讀 codebase。

**六種輸出** · ZIP、PDF、PPTX、送 Canva、Standalone HTML、Handoff to Claude Code。

**一句話總結** · 這是 “design” 第一次像 code 一樣運作——你不再推像素，你在用對話迭代系統。

當 Ryan Mather 說「Claude Design makes it possible」時，他指的不是「做設計稿變快」——而是 **一個人能同時照顧好 7 個產品的結構可能性** 。這件事過去不成立。現在成立了。

如果你是單人經營多產品、多品牌、多通路的創作者，這個工具改變的不是你的設計產出速度——是你可以同時做幾件事的上限。

打開 claude.ai/design，先花一小時打磨你的設計系統。之後的事，會自己發生。

**訂閱制** ✨ AI × 設計趨勢

### AI 覺醒設計應用攻略

Claude Design 只是開始。AI × 設計的工具每週都在刷新，我幫你過濾雜訊， **只整理對設計師真正有用的應用** 。

✦ 每週 AI 工具實測與工作流

✦ 設計師角度的 Prompt 技巧

✦ 新產品第一手 Traditional Chinese 教學

目前尚無留言