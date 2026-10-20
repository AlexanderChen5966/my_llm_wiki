---
title: "【AI 工程進化史】從 Prompt Engineering 到 Graph Engineering：Context、Harness、Loop Engineering是演進還是造詞？ - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10397462"
author:
  - "[[iThome]]"
published:
created: 2026-07-27
description: "資訊基準日：2026-07-21。文中每個日期與主張都對應文末的可驗證來源；X 貼文與部分官方文章的取得限制已逐條標注。 你大概在時間線上看過這條鏈：Prompt Engineering → Cont..."
tags:
  - "clippings"
status: processed
---
0

## 【AI 工程進化史】從 Prompt Engineering 到 Graph Engineering：Context、Harness、Loop Engineering是演進還是造詞？

- 分享至

> 資訊基準日：2026-07-21。文中每個日期與主張都對應文末的可驗證來源；X 貼文與部分官方文章的取得限制已逐條標注。

你大概在時間線上看過這條鏈：Prompt Engineering → Context Engineering → Harness Engineering → Loop Engineering → Graph Engineering，配上一句「AI 工程的五個時代」。我把它逐節查了一遍，總共查核 50 個來源、正文引用其中 29 個。結論：這條鏈只有一半是真的。

五個詞逐一看：

- Prompt Engineering、Context Engineering：有多年實踐與官方定義撐著
- Harness Engineering：有官方文章，但「誰發明的」存在三個互相打架的版本
- Loop Engineering：2026 年 6 月起走紅，兩家官方加一位獨立作者寫出三種互不收斂的定義
- Graph Engineering：誕生於 2026-07-18，到本文資訊基準日（2026-07-21）只有三天，全部家當是一則推文

至於「五階段演進」這個敘事本身，兩條獨立查證路徑得出同一結論：查無任何原始文章一次提出這條鏈。它是內容站事後把五個獨立事件包裝成的故事。

現在讀這篇的理由很實際：中文圈第一篇 Graph Engineering 文章在 2026-07-20 上線，英文圈的模板式懶人包也在同步量產。等這些互相抄的版本被轉載幾輪、變成大家以為的定論，想追出處就難了。這篇文章趁現在給你一條每個節點都能點開驗證的時間軸，和一套下次可以自己套用的判準。

## 先看時間軸

| 詞彙 | 最早可驗證出現 | 命名者 |
| --- | --- | --- |
| Prompt Engineering | 2018–2021 間自然成形 | 無單一命名者；OpenAI／Anthropic 文件皆有專章 \[1\]\[2\] |
| Context Engineering | 2025-06-19 | Tobi Lütke（Shopify 創辦人）於 X 首發、AI 研究者 Andrej Karpathy 六天後放大 \[4\]\[5\] |
| Harness Engineering | 2026-02-11 | 有爭議：OpenAI 最早發文；Google Chrome 團隊的 Addy Osmani 稱 LangChain 的 Vivek Trivedy 為 coiner \[10\]\[11\]\[13\] |
| Loop Engineering | 2026-06-06／07 | Peter Steinberger（OpenClaw 創辦人）於 X 挑起、Addy Osmani 撰文命名（重建時間序）\[14\]\[16\] |
| Graph Engineering | 2026-07-18 | Steinberger 在 X 上的一句公開反問，尚無人給出正式定義 \[20\] |

各詞的官方採用狀態，見下方演進圖與文末總結表。

*圖示語言：實心節點＝已被官方或多方採用的實踐，顏色越深代表時間越晚；虛線空心節點與虛線箭頭＝尚未成立的候選。*

## Prompt Engineering：沒有生日的老詞

Prompt Engineering 指的是把給模型的指令寫好：要求寫清楚、附上範例、讓模型分步推理。OpenAI 與 Anthropic 的官方文件都有專章 \[1\]\[2\]。

它因應的問題最單純：LLM 的輸出品質高度依賴你怎麼問，在那個年代，寫好指令是唯一能動的品質槓桿。

五個詞裡只有它查不到「命名時刻」。它在 GPT-2、GPT-3 時期（約 2018 到 2021 年）由論文與實務社群自然長出來，找不到單一提出者；網路流傳的「Richard Socher 2018 年首創」說法，本次查證找不到一手佐證，不採用。

它也是最早被宣告死亡的。IEEE Spectrum 在 2024 年 3 月就發過〈AI Prompt Engineering Is Dead〉\[3\]，理由是自動調優的 prompt 已經贏過人工。同一篇文章自己就補了轉折：職缺還在，內容換了形狀。記住這個「宣告死亡、然後轉形」的模式，它後面還會重演。

## Context Engineering：教科書等級的一次命名

Context Engineering 指的是管理模型看到什麼：system prompt、工具清單、對話歷史、檢索結果，哪些該進 context window、哪些該壓縮或丟掉。Andrej Karpathy 的說法最常被引用：「把 context window 填進剛剛好資訊的精緻藝術與科學」（意譯）\[5\]。

它因應的是 prompt 解決不了的新瓶頸：進入 agent、RAG（Retrieval-Augmented Generation）與長對話的時代後，品質瓶頸從單句指令移到模型每一輪看到的整包資訊，Anthropic 因此把它定位成「提示工程的自然演進」\[8\]。

想看一個新詞怎麼站穩腳跟，看這個案例：

- **2025-06-19** 　Shopify 創辦人 Tobi Lütke 發推，說比起 prompt engineering，他更喜歡 context engineering 這個說法 \[4\]
- **2025-06-25** 　Karpathy 轉發放大，給出本節開頭那句定義 \[5\]
- **2025-06-27** 　資深開發者 Simon Willison 把來龍去脈記錄成文 \[6\]

從首發到被完整記錄，前後八天。

然後官方進場。LangChain 在 2025-07-02 提出 Write／Select／Compress／Isolate 四種策略 \[7\]；Anthropic 在 2025-09-29 的官方文章給出正式定義 \[8\]。從個人貼文到兩家官方收編，三個月。

![Anthropic 官方文章頁首，發布日期 Sep 29, 2025](https://ithelp.ithome.com.tw/upload/images/20260721/20178110oMh9KWeF0n.png)

*Anthropic 官方文章開頭便點名：幾年的 prompt engineering 之後，context engineering 已成新焦點（2026-07-21 截圖）\[8\]。*

名字是新的，工作是真的：當你開始取捨 system prompt、工具清單、對話歷史與檢索結果該不該進視窗，你做的早就超出「寫一句好 prompt」的範圍。

中文圈接得很快。ihower 的中文分析 2025-07-03 就上線 \[9\]，比多數簡體中文文章更早。

## Harness Engineering：有官方文章，沒有公認發明者

Harness 指模型外面那一圈：工具、權限、檢查點、回饋機制。讓這個詞流行起來的是一條公式：Agent = Model + Harness。

它因應的是 agent 真的開始動手做事之後的問題：模型決定「想做什麼」，但擋下危險操作、量測結果、修正偏差的是外圈系統。OpenAI 那篇文章給的分工說法（意譯）：context engineering 問「agent 該看到什麼」，harness engineering 問「系統該防止、量測、修正什麼」\[10\]。

![Agent = Model + Harness 結構圖](https://ithelp.ithome.com.tw/upload/images/20260721/20178110sup8chDRLV.png)

*Agent = Model + Harness：模型決定想做什麼，外圈系統決定它能安全做到什麼。*

問題出在「誰先說的」，三個版本各有依據：

- **OpenAI** （2026-02-11）　最早帶日期的官方文章〈Harness engineering〉\[10\]；此文對自動抓取回 403，內容經鏡像與多方引述交叉重建，逐字引用前應重新取得原文
- **LangChain 的 Vivek Trivedy** （2026-02-17）　六天後發表帶出公式的那篇 \[11\]；Addy Osmani 在自己的文章裡明白寫下「Trivedy coined the term」\[13\]
- **HashiCorp 創辦人 Mitchell Hashimoto** （約二月初）　中文圈部分文章指他更早，但四路查證都找不到一手網址

Thoughtworks 的 Birgitta Böckeler 在 martinfowler.com 的長文（2026-04-02）則把公式歸功給 LangChain \[12\]。

誠實的寫法只有一種：多人在 2026 年 2 月同期各自命名或推廣，沒有公認的單一發明者。另一個值得注意的訊號：Anthropic 至今只用 harness design 這種普通名詞寫法，沒有把它抬成學科 \[22\]。

## Loop Engineering：一週爆紅，三種定義

Loop Engineering 指的是不再逐次手動對 agent 下指令，改為設計讓它自己跑的 loop：執行、檢查結果、自我修正、再執行，人只在關鍵點介入 \[14\]\[16\]。

它因應的是效率瓶頸換了位置：模型夠強之後，人工逐步下指令反而成了整個流程最慢的一環，於是把人從每一步抽出來，改成設計流程本身。

![Loop 的基本形流程圖](https://ithelp.ithome.com.tw/upload/images/20260721/20178110PEkaGFWB9F.png)

*Loop 的基本形：人給目標、在關鍵點確認，中間的執行、檢查、修正由 agent 自己循環。*

這個詞的誕生週可以逐日排出來（此時間序為本次查證重建，並非任何單一來源自陳）：

- **2026-06-06** 　一句被歸給 Claude Code 負責人 Boris Cherny 的話開始流傳，大意是他已經改用寫 loop 的方式、不再逐次 prompt Claude；只找得到轉述版本，未找到本人原始貼文 \[15\]
- **2026-06-07** 　OpenClaw 的 Peter Steinberger 發推主張，你不該再手動 prompt coding agent，該設計會自己 prompt 的 loop；約 650 萬瀏覽 \[14\]
- **同日** 　Addy Osmani 發出第一篇把它命名並系統化的長文 \[16\]

官方在一到四週內跟進：LangChain 06-16 \[17\]、Anthropic 06-30 \[18\]。有趣的是三個版本互不收斂：Osmani 的五元件、LangChain 的四層架構、Anthropic 的四類 loop，其中只有後兩家是官方。詞很熱，共識還沒有。

潑冷水的也到了。《The Pragmatic Engineer》電子報作者 Gergely Orosz 訪了一圈工程師（2026-07-14）\[19\]：有人判斷 loop 只是模型不夠強時期的暫時性 hack，有人說這是舊自動化的重新包裝，也有團隊實測後回報 agent 會漂移、API 成本壓不住。

## Graph Engineering：一則推文，誕生於 2026-07-18

Graph Engineering 目前連公認的定義都沒有。最接近定義的嘗試來自 AlphaLab 的 Terry Chen：很多個 loop，加上明確設計「誰看著誰」\[24\]；要說清楚，那是他個人的框架，不是社群共識。

支持者給的理由（目前只是假說）：單一 loop 有上限，例如自我檢查失準、指標被系統自己玩壞，所以需要多個 loop 分工互相監督 \[24\]。學術端的 MAST 論文（Cemri 等人）早就指出多 agent 系統的失敗多源於設計不良 \[26\]，這件事同時是 graph 化的動機，也是對它的警告。

![Loop 與 Graph 假說的對比圖](https://ithelp.ithome.com.tw/upload/images/20260721/20178110YpgvKk3Sru.png)

*Loop 到 Graph 的假說：虛線部分是尚未被驗證的想像，不是既定實務。*

截至 2026-07-21，這個詞的全部可驗證家當如下。Steinberger 在 2026-07-18 於 X 上拋出一句反問（意譯）：「我們還在談 loop，還是已經轉向 graph 了？」，約 280 萬瀏覽（2026-07-21 截圖時點）\[20\]。他只拋出了問題，沒有給定義；同日跟風者喊出「Loop 已死，Graph 萬歲」（意譯）\[21\]。

![Steinberger 推文原文截圖](https://ithelp.ithome.com.tw/upload/images/20260721/20178110bcn0c8VoXH.png)

*Graph Engineering 目前的全部起點：一則十二個字的反問（2026-07-21 截圖）\[20\]。*

沒有定義性長文，也沒有任何官方採用。LangGraph 的官方文件描述了大量圖編排機制，但全文查無 graph engineering 這個詞 \[23\]。

中文圈的第一波內容甚至比英文定義文來得快：Terry Chen 那篇 2026-07-20 的入門長文 \[24\] 還提出「四種翻車、四種連線、錨」的整理，引用時同樣請當作一位具名作者的觀點看待。

概念本身不新。用圖結構編排多個 agent，LangGraph 的官方文件早把機制寫得清清楚楚 \[23\]；新的只有「把它抬成下一個 Engineering」這個說法，而這個說法目前是單源的。

## 三個判準，和一盆早了一年的冷水

回頭看整條鏈，殺傷力最大的一篇寫在炒作開始之前。Anthropic 的〈Building Effective Agents〉（2024-12-19）\[25\]：最成功的實作沒有用複雜框架，用的是簡單、可組合的模式；加複雜度之前，先證明它確實改善結果。這篇比 Loop 和 Graph Engineering 早了一年半，讀起來卻像專門為它們寫的。

實證研究站在同一邊。MAST 論文分析 7 個框架、超過 200 個任務的失敗軌跡，定性結論：多數失敗出在系統設計，與模型能力關係不大 \[26\]；網路流傳的具體失敗率百分比無法在論文正文對應，本文不引用。

產業面同樣冷：Gartner 預測超過四成 agentic AI 專案會在 2027 年底前取消 \[27\]，Uber 四個月燒完全年 AI 工具預算後開始設每人每月上限 \[28\]。

所以，下次再有新的「XX Engineering」出現時，套這三個判準：

1. **官方或多方採用了嗎？** Context Engineering 用三個月拿到兩家官方定義；Graph Engineering 目前是零。
2. **有新的工程動作嗎？** 如果描述它時只能重新排列既有工作（寫 prompt、管 context、接工具），它可能只是新標籤。
3. **有失敗案例沉澱嗎？** 一個實踐要成熟到有人公開踩坑（像 Orosz 收集的那些），才代表真的有人在用。

## 接下來看什麼

用觀察代替預言。接下來一到兩個月，有三個指標值得盯：

1. **Graph Engineering 有沒有被官方收編** ：LangChain、Anthropic、OpenAI 任何一家發出定義性文章，它就從一句反問升級成候選實踐；一直沒有，它多半會像大多數社群造詞一樣退場
2. **Loop Engineering 的定義有沒有收斂** ：三家版本合流成單一框架，或其中一版的引用量明顯壓過其他，這個詞才算站穩
3. **有沒有下一個詞** ：中文圈已有具名作者推測 Loop 之後是「Factory Model」\[29\]；不管下一個詞是什麼，本文的判準照用

而不變的底層值得放在最後：Anthropic 在這波炒作開始前一年就寫下的原則——先證明複雜度有用，才加複雜度 \[25\]。每個新的「XX Engineering」都該先過這一關。

## 總結

把查核結果壓成一張表帶走：

| 詞彙 | 因應的問題 | 判定 |
| --- | --- | --- |
| Prompt Engineering | 輸出品質高度依賴怎麼問 | 真實實踐；被宣告死亡後轉形，沒有消失 |
| Context Engineering | agent 時代「模型看到什麼」的管理量爆炸 | 真演進：八天內從首發到被完整記錄，三個月內獲兩家官方定義 |
| Harness Engineering | agent 動手做事後的防護、量測、修正 | 實務真實存在；「誰命名」三方各說各話，無共識 |
| Loop Engineering | 人工逐步下指令成為流程瓶頸 | 熱詞：官方兩家＋獨立作者共三種定義並存，踩坑報告已開始出現 |
| Graph Engineering | （假說）單一 loop 的能力上限 | 目前只是一句反問，還不構成一門學科 |

而「五階段演進」這條鏈本身，沒有任何人一次提出過，它是內容站把五個獨立事件事後串成的敘事。下次再有新的「XX Engineering」，拿同一套判準檢驗：官方或多方採用了嗎、有新的工程動作嗎、有失敗案例沉澱嗎。

本文的時間軸停在 2026-07-21，之後的部分你可以拿上面的指標和判準自己查，來源都在下面。

## 參考來源

依查證分級標注：L1＝官方一手、L2＝具名權威作者一手、L3＝優質二手（社群觀點）。標「未直接取得」者，內容經搜尋摘要／鏡像／時間戳解碼交叉核實，逐字引用前應重新取得原文。

1. L1｜OpenAI, Prompt engineering guide — [https://platform.openai.com/docs/guides/prompt-engineering](https://platform.openai.com/docs/guides/prompt-engineering)
2. L1｜Anthropic, Prompt engineering overview — [https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)
3. L2｜Dina Genkina, IEEE Spectrum, "AI Prompt Engineering Is Dead"（2024-03-06）— [https://spectrum.ieee.org/prompt-engineering-is-dead](https://spectrum.ieee.org/prompt-engineering-is-dead)
4. L2｜Tobi Lütke, X（2025-06-19，未直接取得）— [https://x.com/tobi/status/1935533422589399127](https://x.com/tobi/status/1935533422589399127)
5. L2｜Andrej Karpathy, X（2025-06-25，未直接取得）— [https://x.com/karpathy/status/1937902205765607626](https://x.com/karpathy/status/1937902205765607626)
6. L2｜Simon Willison, "Context engineering"（2025-06-27）— [https://simonwillison.net/2025/Jun/27/context-engineering/](https://simonwillison.net/2025/Jun/27/context-engineering/)
7. L1｜LangChain, "Context Engineering for Agents"（2025-07-02）— [https://blog.langchain.com/context-engineering-for-agents/](https://blog.langchain.com/context-engineering-for-agents/)
8. L1｜Anthropic, "Effective context engineering for AI agents"（2025-09-29）— [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
9. L2｜ihower,〈什麼是 Context Engineering 上下文工程?〉（2025-07-03）— [https://ihower.tw/blog/12817-context-engineering](https://ihower.tw/blog/12817-context-engineering)
10. L1｜OpenAI, "Harness engineering"（2026-02-11，未直接取得：403）— [https://openai.com/index/harness-engineering/](https://openai.com/index/harness-engineering/)
11. L1｜Vivek Trivedy, LangChain, "Improving Deep Agents with harness engineering"（2026-02-17）— [https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering)
12. L2｜Birgitta Böckeler, martinfowler.com, "Harness engineering"（2026-04-02）— [https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)
13. L2｜Addy Osmani, "Agent Harness Engineering"（2026-04-19）— [https://addyosmani.com/blog/agent-harness-engineering/](https://addyosmani.com/blog/agent-harness-engineering/)
14. L2｜Peter Steinberger, X（2026-06-07，未直接取得）— [https://x.com/steipete/status/2063697162748260627](https://x.com/steipete/status/2063697162748260627)
15. L2｜Boris Cherny 語錄，經 @rohanpaul\_ai 轉推（2026-06-06，二手轉述；2026-07-21 link check 回 404，該轉推已不可及，語錄僅存於二手報導，不可逐字引用）— [https://x.com/rohanpaul\_ai/status/2063289804708835412](https://x.com/rohanpaul_ai/status/2063289804708835412)
16. L2｜Addy Osmani, "Loop Engineering"（2026-06-07）— [https://addyosmani.com/blog/loop-engineering/](https://addyosmani.com/blog/loop-engineering/)
17. L1｜Sydney Runkle, LangChain, "The Art of Loop Engineering"（2026-06-16）— [https://www.langchain.com/blog/the-art-of-loop-engineering](https://www.langchain.com/blog/the-art-of-loop-engineering)
18. L1｜Anthropic, "Loop engineering: getting started with loops"（2026-06-30）— [https://claude.com/blog/getting-started-with-loops](https://claude.com/blog/getting-started-with-loops)
19. L2｜Gergely Orosz, The Pragmatic Engineer, "What is 'loop engineering'?"（2026-07-14）— [https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering](https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering)
20. L2｜Peter Steinberger, X（2026-07-18；2026-07-21 已以瀏覽器直接取得並截圖，原文與日期核實無誤）— [https://x.com/steipete/status/2078277297791189132](https://x.com/steipete/status/2078277297791189132)
21. L2｜Santiago Valdarrama, X（2026-07-18，未直接取得）— [https://x.com/svpino/status/2078516761318584774](https://x.com/svpino/status/2078516761318584774)
22. L1｜Anthropic, "Harness design for long-running application development"（2026-03-24）— [https://www.anthropic.com/engineering/harness-design-long-running-apps](https://www.anthropic.com/engineering/harness-design-long-running-apps)
23. L1｜LangGraph overview docs — [https://docs.langchain.com/oss/python/langgraph/overview](https://docs.langchain.com/oss/python/langgraph/overview)
24. L3｜Terry Chen, AlphaLab,〈Graph Engineering 是什麼？〉（2026-07-20，社群觀點）— [https://www.alphalab.site/graph-engineering](https://www.alphalab.site/graph-engineering)
25. L1｜Erik Schluntz & Barry Zhang, Anthropic, "Building Effective Agents"（2024-12-19）— [https://www.anthropic.com/engineering/building-effective-agents](https://www.anthropic.com/engineering/building-effective-agents)
26. L2｜Cemri et al., "Why Do Multi-Agent LLM Systems Fail?"（arXiv:2503.13657）— [https://arxiv.org/abs/2503.13657](https://arxiv.org/abs/2503.13657)
27. L1｜Gartner 新聞稿（2025-06-25；Gartner 網站對自動抓取回 403，2026-07-21 檢查）— [https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027)
28. L2｜TechCrunch, Uber AI 預算報導（2026-06-02）— [https://techcrunch.com/2026/06/02/uber-caps-employee-ai-spending-after-blowing-through-budget-in-four-months/](https://techcrunch.com/2026/06/02/uber-caps-employee-ai-spending-after-blowing-through-budget-in-four-months/)
29. L3｜刘道玉 AI 工作坊,〈Loop Engineering 深度综述〉（2026-06-14，社群觀點；Factory Model 為該作者推測）— [https://daoyuly.cn/2026/2026-06-14-loop-engineering-deep-review/](https://daoyuly.cn/2026/2026-06-14-loop-engineering-deep-review/)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1784885238_6a632ff62c5c0.jpg)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2607B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/cccef095-9dfa-42b9-866d-cee30889b77d_small.jpg)

[笑死，能關防毒幹嘛要做免殺呢？從令牌偽造到把防毒關進沙箱隔離](https://itplus.ithome.com.tw/vod-page/516?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

42 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/0fafcfc8-df88-4b70-a119-6d7d56d75679_small.png)

[如何以 Cloud Native 工具服務打造您的雲端安全](https://itplus.ithome.com.tw/vod-page/547?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

29 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/6dd5c87a-70ec-4618-85a3-ac8f77a3ad52.png)

[用 commit 生成編年史、構築世界再到多重宇宙](https://itplus.ithome.com.tw/vod-page/1081?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

34 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/72e25481-cd69-4044-a111-3fc45aad01d0_small.png)

[金融產品協作術：精準對齊多元需求與視角](https://itplus.ithome.com.tw/vod-page/1377?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

37 分