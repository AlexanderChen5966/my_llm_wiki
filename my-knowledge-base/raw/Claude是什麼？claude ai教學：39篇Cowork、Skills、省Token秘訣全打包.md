---
title: "Claude是什麼？claude ai教學：39篇Cowork、Skills、省Token秘訣全打包"
source: "https://www.bnext.com.tw/article/91080/claude-cowork-skills"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-08-19
created: 2026-08-19
description: "Claude在近期大幅更新後，舊的長指令用法效果已大不如前。本文整理38篇教學，涵蓋模型選用、提示詞技巧到進階代理人設定，幫助使用者少走冤枉路。"
tags:
  - "clippings"
status: "processed"
---
[![](https://bnextmedia.s3.hicloud.net.tw/pumpkin/image/photo/2026-08/img-1785684158-19300.gif)](https://meetgreatersouth.tw/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026mgs&utm_content=167926&utm_term=channel_all)

2026.08.19 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

## Claude入門學習地圖！39篇教學、8大主題一次收，Cowork、Skills、省Token秘訣全打包

Claude在近期大幅更新後，舊的長指令用法效果已大不如前。本文整理38篇教學，涵蓋模型選用、提示詞技巧到進階代理人設定，幫助使用者少走冤枉路。

[＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude) [＃提示詞（prompt）](https://www.bnext.com.tw/tags/%E6%8F%90%E7%A4%BA%E8%A9%9E%EF%BC%88prompt%EF%BC%89) [＃skill](https://www.bnext.com.tw/tags/skill)

---

2026 年，Anthropic 的更新速度明顯加快。

今年以來，Claude 幾乎每兩週就有一次重大更新，主要變動集中在模型能力提升、新增 Claude Code 編程工具、代理人功能擴充，以及更長的上下文處理能力。

這些改動對一般使用者的影響在於，過去那套用法，有些已經不適用了。以往流行的長指令、細節堆疊方式，在新版模型上效果反而不如預期。目前比較有效的做法是指令簡短但明確，讓模型自行判斷細節。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

為了幫使用者少走冤枉路，《數位時代》整理了 39 篇教學文章，推出這份「Claude 實戰地圖」，從要選哪個方案、怎麼下指令，到怎麼讓 Claude 住進 Excel、Word 裡幫你工作，全部一次整理清楚。

![Claude攻略](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/kv79-1780066485.png?w=1200&output=webp)

圖／ 數位時代

這份地圖不必 39 篇一次讀完，我們建議你：先看完第一章搞懂方案與模型怎麼選，再依工作場景跳到對應段落。常做簡報的人直接看 Cowork 與 Design，每天跟 Excel、Word 搏鬥的人則從第六章開始。

#### Claude入門學習地圖（點擊可直接前往該段落）：

[**一、入門：Claude 是什麼、方案怎麼選、跟其他 AI 怎麼比**](#一-入門：Claude-是什麼、方案怎麼選、跟其他-AI-怎麼比)  
Chat、Cowork、Code三大模式、訂閱方案、Opus/Sonnet/Haiku 選用、工具比較、記憶轉移

[**二、官方提示詞指南＋「少即是多」高階提示工程**](#二-官方提示詞指南＋進階提示詞)  
Anthropic 官方建議、少即是多、Opus 4.7 升級攻略、情境工程、進階提示詞  
[**三、Cowork實戰：打造串聯雲端硬碟的AI代理人**](#三-Cowork：把-Claude-變成同事)  
Cowork 入門、6 步驟教學、Markdown 知識庫、自動排毒、PPT 簡報、Excel 公式  
[**四、Skills技能養成：教AI自動記住你的格式偏好**](#四-Skills：教-Claude-記住你的習慣)  
Skill 是什麼、6 步驟打造、新手障礙排除、報帳助理、26 個現成 Skill

[**五、Projects與Claude Design：長期記憶專案與品牌風格簡報術**](#五-Projects-&-Claude-Design：簡報與長期記憶)  
Projects 5 步驟、Design 簡報入門、Design 進階用法、Refero 整合

[**六、用Claude處理工作場景：Excel、Word、研究、簡報**](#六-用-Claude-處理工作場景：Excel、Word、研究、簡報)  
Excel 公式、Word 改稿、麥肯錫風 PPT、論文整合、個人知識庫、互動視覺

[**七、省Token密技與配額管理：突破用量上限的策略**](#七-省-Token、配額管理) 縮減 token、PDF 處理省 22 倍、官方建議 4 條

[**八、Claude Code入門：給想再進一步的人**](#八-Claude-Code-入門：給想再進一步的人)  
新手安裝教學、CLAUDE.md 寫作規則、快捷鍵與斜線指令

## 一、入門：Claude 是什麼、方案怎麼選、跟其他 AI 怎麼比

當前的 Claude 早已蛻變為具備 Chat、Cowork 與 Code 三大核心模式的全能生態系。

隨著全新訂閱制的推行，互動與程式化額度已獨立分流，而在旗艦模型（Opus）、平衡模型（Sonnet）與輕量模型（Haiku）三款模型間的精準切換，更是攸關高達五倍的隱形成本，並透過最新的「記憶轉移」功能無痛搬遷偏好。

| 文章標題 | 簡介 |
| --- | --- |
| [Claude 桌面版三大功能模式：Chat、Cowork、Code 差在哪？一次搞懂最適合你的 AI 工作流](https://www.bnext.com.tw/article/90190/revealing-claude-three-modes-chat-cowork-code-philosophies) | 三個模式的本質差異：Chat 給你答案、Cowork 直接動你的檔案、Code 給工程師。看完知道哪個是你的場景。 |
| [Claude 2026 最新訂閱方案｜免費版功能有什麼？Agent SDK 費用怎麼算？](https://www.bnext.com.tw/article/90109/claude-ai) | 6 月起互動使用與程式化使用切割成兩個額度池，免費版、Pro、Max、Team 方案差別一次看懂。 |
| [Claude 模型怎麼選？Opus、Sonnet、Haiku 一表看懂，沒切換模型可能會多花 5 倍成本](https://www.bnext.com.tw/article/90667/claude-model-comparison-opus-sonnet-haiku) | 三個模型的能力、速度、價格差異對照，搭配實際場景建議，避免亂用旗艦模型燒掉額度。 |
| [Claude、Gemini、ChatGPT 三大工具差在哪？華頓商學院教授教你「付費版」選用指南](https://www.bnext.com.tw/article/90197/ethan-mollick-which-ai-to-use) | 華頓教授 Ethan Mollick 的工具選用建議：什麼場景用哪個，付費版差異一次看。 |
| [Claude 推出「記憶轉移」功能！一鍵把你在 ChatGPT、Gemini 的 AI 偏好全搬走，2 步驟搞定](https://www.bnext.com.tw/article/90203/claude-switch-to-claude-without-starting-over) | 不用從零重訓 Claude：用記憶轉移功能把 ChatGPT、Gemini 學過的偏好直接帶過來。 |
| [Claude 超實用地圖！Cowork、Claude Code 是什麼？初階者從哪開始學？進階技巧一次整理](https://www.bnext.com.tw/article/90460/claude-map-to-master) | 從「會聊天」到「會幫你動手做事」的全景圖，初階到進階的學習路徑與工具拆解。 |

## 二、官方提示詞指南＋「少即是多」高階提示工程

Anthropic 官方已經明確表示，過去兩年累積的提示詞技巧，在新一代模型上反而會降低輸出品質，「少即是多」是現在的官方建議。

本章收錄官方針對 Opus 4.7 的升級攻略、情境工程（Context Engineering）五大步驟，以及如何透過特定提示詞，將 Claude 從順從的答題機變成會主動質疑、反駁你的深度思考夥伴。

| 文章標題 | 簡介 |
| --- | --- |
| [Claude 最新提示詞指南！核心建議「少即是多」，5 個小技巧讓 AI 品質大提升](https://www.bnext.com.tw/article/90140/anthropic-claude-prompt-engineering) | Anthropic 官方建議：過去兩年的提示技巧可能反效果，現代模型偏好簡潔直接，5 個官方做法。 |
| [Claude Opus 4.7 提示詞攻略！Anthropic 官方曝 5 大調整，如何讓舊 prompt 一次升級？](https://www.bnext.com.tw/article/91048/claude-opus-4-7-prompting-guide-for-better-responses) | Anthropic 官方針對 Opus 4.7 提出的 5 大調整建議，舊 prompt 升級不需重寫。 |
| [Claude 進階技巧懶人包！7 個提示詞，讓 AI 從答題機變「會反駁你」的思考夥伴](https://www.bnext.com.tw/article/90564/claude-knowledge-workers-tips) | 7 個進階提示詞讓 Claude 主動質疑你的想法，而非順從輸出答案。 |
| [提示工程死透了！解密 Anthropic 情境工程 5 大步驟：如何聰明設定，讓 Claude 忘不了你？](https://www.bnext.com.tw/article/90407/anthropic-claude-code-how) | Anthropic 推的「情境工程」5 大步驟：從提示詞升級為情境設定，讓 Claude 記得你的偏好。 |
| [Anthropic 最新研究：愛幫 AI「改稿」的人，可多發揮近兩倍戰力！5 個關鍵用法一次學](https://www.bnext.com.tw/article/90176/unlock-ai-potential-how-to-achieve-best-results-through-iterative-questions) | Anthropic 研究：愛跟 AI 來回修改稿子的人，產出戰力多近兩倍，5 個關鍵用法。 |

---

## 三、Cowork實戰：打造串聯雲端硬碟的AI代理人

擺脫單純的對話框對答，非工程師工作者正式迎來 AI Agent（AI 代理）的即戰力時代。 Cowork 模式可允許 AI 直接介入雲端、硬碟與郵件進行檔案操作。

本章將帶領讀者掌握利用 Markdown (.md) 檔建立核心背景知識庫的關鍵技術，並透過定期排程提示維持模型穩定。內文更收錄 20 分鐘產出百頁簡報，以及利用一段提示詞從零生成 700 條 Excel 公式的跨工具實測。

| 文章標題 | 簡介 |
| --- | --- |
| [Claude Cowork 是什麼？Cowork 教學：簡報、報帳、整理雲端硬碟 5 個超實用場景](https://www.bnext.com.tw/article/89996/claude-cowork-transforming-generative-ai-into-a-digital-colleague) | Cowork 入門：5 個非工程師也能立刻用的場景示範，從簡報到報帳到雲端整理。 |
| [同事都在用 AI 做事？Claude Cowork 完整教學，教你一步步打造 AI Agent 超強工作流](https://www.bnext.com.tw/article/90369/how-to-claude-cowork-6-step) | Cowork 6 步驟完整教學，從設定到實際操作，打造可重複執行的工作流。 |
| [丟掉提示詞吧！Cowork 實戰技巧：建立核心資料夾、把背景知識寫成.md 檔，4 步驟操作一次學](https://www.bnext.com.tw/article/90284/claude-cowork-markdown) | 進階技巧：用 Markdown 檔案儲存背景知識，讓 Cowork 跨對話保留記憶。 |
| [Claude Cowork 自動排毒教學：用「1 段提示＋每週排程」，維持 AI 設定不走鐘！](https://www.bnext.com.tw/article/90421/claude-cowork-code-fix) | 用一段排程提示讓 Cowork 每週自我清理，避免設定走樣導致輸出品質下滑。 |
| [20 分鐘產出 100 頁 PPT！實測 Claude Cowork 模式，5 步驟打造高效 AI 簡報工作流](https://www.bnext.com.tw/article/90505/claude-cowork-ppt-workflow-guide) | 實測 Cowork 做 100 頁 PPT 的 5 步驟完整流程，含提示詞設計與注意事項。 |
| [他實測 11 款 AI 做 Excel，Claude Cowork 勝出！一段提示詞從零生成 700 條公式，5 步驟流程一次學](https://www.bnext.com.tw/article/91044/ai-excel-claude-cowork) | 跨工具實測：Cowork 在 Excel 任務勝出 10 款競品，5 步驟流程拆解。 |
| [Claude能直接寄Gmail了！新增寄信、轉寄功能，僅限付費方案開放](https://www.bnext.com.tw/article/91916/claude-can-send-gmail-now-2026) | Claude 付費版可直接寄出、回覆、轉寄 Gmail 郵件，用 4 步驟教你設定 |

---

## 四、Skills技能養成：教AI自動記住你的格式偏好

Skill 是 Claude 的可複用技能包，概念跟 ChatGPT 的 GPTs、Gemini 的 Gem 相近，差別在於觸發方式更彈性、能跨工具自動啟用。

如果你每天都在跟 AI 重貼同一段提示詞，或希望 Claude 記住你的工作格式偏好，Skill 就是解法。這一章從 Skill 是什麼、6 步驟打造第一個 Skill、新手常見障礙排除，到 X 上爆紅的 26 個現成 Skill 清單。

| 文章標題 | 簡介 |
| --- | --- |
| [Agent Skills 是什麼？有了 GPT 或 Gem，還需要 Skill 嗎？一次看懂 AI 工作流指南](https://www.bnext.com.tw/article/90186/what-is-agent-skill) | Skill 跟 GPTs、Gem 的核心差異與互補關係，看完知道什麼場景才該設 Skill。 |
| [Agent Skill 是什麼？Agent Skill 教學，6 步驟打造你的第一個 Skill](https://www.bnext.com.tw/article/90058/agent-skills-free-course-deeplearning-ai-anthropic-latest-partnership) | 從零打造第一個 Skill 的 6 步驟教學，零基礎入門必看。 |
| [Claude Skills 零基礎入門教學｜每次都要重貼提示詞？Skill 設好沒反應？一篇搞定新手常見困擾](https://www.bnext.com.tw/article/90870/claude-skills-guidebook) | 新手最常遇到的 Skill 設定問題一次解答，從觸發失敗到提示詞重複貼。 |
| [10 分鐘做完兩天的體力活？我用 Skill 把 AI 訓練成專屬助理，報帳、資料整理都超快！](https://www.bnext.com.tw/article/90328/claude-skill-efficiency-guide) | 報帳助理 Skill 實作分享，10 分鐘搞定平常要花兩天的重複作業。 |
| [X 上爆紅的 26 個 Skill！賈伯斯、卡帕西的思維都能「蒸餾」、還能算命，完整清單一次收](https://www.bnext.com.tw/article/90638/persona-skill-colleague) | X 平台爆紅的 26 個現成 Skill，含思維蒸餾、算命、決策輔助等清單。 |

## 五、Projects與Claude Design：長期記憶專案與品牌風格簡報術

不會 Claude Code 也想要長期記憶與專屬助理？使用 Projects 功能幫你養成，把背景資料、工作偏好、長期任務放進同一個 Project，Claude 能夠跨對話延續記憶。

Claude Design 則是 Anthropic 推出的簡報工具，可以直接匯出 PPTX 與 Canva 格式。以下涵蓋 Projects 功能 5 步驟建立工作流、Design 入門到進階、以及搭配 Refero 套用品牌設計語言的省 token 用法。

| 文章標題 | 簡介 |
| --- | --- |
| [不會 Claude Code 也能上手！用 Projects 打造 AI 工作流，5 步驟養成長期記憶助理](https://www.bnext.com.tw/article/90621/claude-projects-skills-mcp-guide) | Projects 5 步驟教學：不寫程式也能養出有長期記憶的個人助理。 |
| [Claude Design 簡報教學｜從建立專案到匯出 PPTX、Canva，4 步驟打造品牌風格簡報](https://www.bnext.com.tw/article/90713/claude-design-vs-figma-ai-product-design-prototyping) | Claude Design 入門 4 步驟，含匯出 PPTX 與 Canva 兩種格式。 |
| [Claude Design 進階用法｜6 步驟產出專業簡報，Slider 微調、Comment 修改一次學會](https://www.bnext.com.tw/article/90717/claude-design-vs-figma-adobe-canva-workflow-shift) | Design 進階用法 6 步驟：用 Slider 微調與 Comment 修改細節，做出更精緻的簡報。 |
| [全球品牌設計語言當簡報版型！Claude Design 搭 Refero 快速做出專屬簡報，還能大省 token](https://www.bnext.com.tw/article/90873/save-tokens-with-claude-design-and-refero-brand-templates) | Design 結合 Refero 快速套用品牌設計語言，做出專屬簡報還能省 token。 |

## 六、用Claude處理工作場景：Excel、Word、研究、簡報

Claude 目前可常駐 Excel 和 Word 的側邊欄，用來改善查帳、除錯公式、改稿、檢查文件等碎片化流程，不用再切視窗複製貼上。

這章拆解 6 個實用場景，包括 Excel 公式生成、Word 改稿、麥肯錫風 PPT 生成，並透過 9 個提示詞把 40 多篇論文提煉為結構化研究報告，以及對話框內玩轉最新的「互動視覺」圖表生成。

| 文章標題 | 簡介 |
| --- | --- |
| [Claude in Excel 官方教學！如何在 Excel 安裝 Claude？查帳、除錯公式、建立財務模型⋯好用提示詞一次看](https://www.bnext.com.tw/article/90029/claude-in-excel) | 官方 Excel 外掛教學：查帳、除錯公式、建立財務模型三大場景，附實用提示詞。 |
| [Anthropic 推 Claude for Word！AI 直接住進側邊欄，幫你改稿、檢查文件](https://www.bnext.com.tw/article/90633/what-is-claude-for-word) | Word 外掛功能介紹：AI 直接進駐側邊欄，協助改稿與檢查文件。 |
| [「麥肯錫風」簡報怎麼做？一段 Prompt 生成可編輯 PPT：皇家藍質感、關鍵圖表一次到位](https://www.bnext.com.tw/article/90111/moonshot-ai-kimi-market-analysis-trends) | 一段提示詞讓 AI 生出麥肯錫風 PPT，含皇家藍配色與關鍵圖表配置。 |
| [Claude 變身「史丹佛博士生」助理：研究生如何用 9 個提示，把 40 多篇論文變成研究報告？](https://www.bnext.com.tw/article/90291/ai-redefining-academic-research-literature-analysis) | 史丹佛研究生的 9 個提示詞：把 40 多篇論文整合成有結構的研究報告。 |
| [不用 Obsidian 也能建 AI 知識庫！Karpathy 同款「說明書」設定，4.1 萬人超人氣方法完整拆解](https://www.bnext.com.tw/article/90650/andrej-karpathy-ai-how) | Karpathy 推薦的個人說明書設定法，免用 Obsidian 也能建個人知識庫。 |
| [不只會回文了！Claude 推「互動視覺」新功能，可以直接在對話框裡畫圖表](https://www.bnext.com.tw/article/90301/anthropics-claude-launches-new-interactive-visualization-feature) | 互動視覺功能介紹：直接在對話框內生成可互動的圖表與視覺化內容。 |

---

## 七、省Token密技與配額管理：突破用量上限的策略

「Claude 用量上限」是 2026 年使用者常見的痛點。在升級付費方案之前，掌握精準的 Token 管理才是治本之道。

以下從 4 個爆量情境＋9 個對應技巧、資深工程師實測 5 招省 token 大法，到 Anthropic 官方給的 4 條建議，延長現有方案的配額時間。

| 文章標題 | 簡介 |
| --- | --- |
| [Claude 老是達到用量上限？4 個情境、9 個技巧，教你有效縮減 token](https://www.bnext.com.tw/article/90731/reduce-claude-token-usage-by-changing-habits-not-plan) | 4 個常見爆量情境＋9 個對應技巧，不升級方案就能延長額度。 |
| [Claude 用量爆掉先別急著買！工程師 5 招「省 token」大法，光 PDF 處理就差 22 倍](https://www.bnext.com.tw/article/90861/save-ai-tokens-usage-tips) | 工程師實測 5 招省 token 大法，PDF 處理方式差 22 倍 token 消耗。 |
| [Token 燒太快？Anthropic 官方給出 4 條 Claude Code 建議，這樣做降低額度消耗](https://www.bnext.com.tw/article/90531/claude-token-how-to-save) | Anthropic 官方給的 4 條建議，避免不必要的 token 浪費。 |

---

## 八、Claude Code入門：給想再進一步的人

官方定義「Cowork 就是給非工程師的 Claude Code」，如果你已經能用 Cowork 跑工作流，想再進一步控制細節、處理大量檔案、設計多 Agent 協作，以下 3 篇提供零基礎也能上手的安裝教學從安裝，完整拆解能讓 AI 錯誤率從 41% 暴跌至 3% 的 CLAUDE.md 12 條關鍵寫作規則，並附上 13 大類快捷鍵與斜線指令速查表。

| 文章標題 | 簡介 |
| --- | --- |
| [Claude Code 新手安裝教學｜Desktop App、終端機 2 種方式怎麼裝？不懂程式也能秒上手](https://www.bnext.com.tw/article/90451/claude-code) | 兩種安裝方式比較，非工程師也能上手的安裝指南。 |
| [CLAUDE.md 這樣寫才對！12 條規則一次整理，讓 Claude Code 錯誤率從 41% 降至 3%](https://www.bnext.com.tw/article/90965/claude.md-claude-code) | 12 條 CLAUDE.md 寫作規則，讓 Claude Code 的指令遵循率大幅提升。 |
| [Claude Code 快捷鍵+指令大全！13 大類速查不用背，從 Ctrl+C 到多 Agent 協作一次整理](https://www.bnext.com.tw/article/90925/claude-code-slash-commands-shortcuts-complete-guide) | Claude Code 完整快捷鍵與斜線指令清單，13 大類速查。 |

把 Claude 玩熟之後，如果你想再試試其他的工具，推薦你一定要試試 OpenAI Codex。Codex 現在已經能把其他 AI agent 的設定「搬家」匯入——想知道 Codex 怎麼用、又怎麼從 Claude 無痛搬過去，可以參考 [這篇完整教學＋遷移技巧](https://www.bnext.com.tw/article/91113/openai-codex-complete-guide-and-migration-tips) 一次說清楚。

![AI工作](https://image-cdn.learnin.tw/bnextmedia/image/album/2025-09/kprd-1758698474.png?w=1200&output=webp)

圖／ AI生圖

Anthropic 的更新不會停，這份地圖收錄的功能與技巧，半年後很可能又有新版本。《數位時代》編輯團隊也會持續追蹤，把值得一試的用法補進清單。

另外，如果你想一次掌握跨模型的指令邏輯，從基礎寫法、各家官方指南，到職場商務、視覺生圖與翻譯校正，可以搭配我們收錄超過 100 組實戰指令的 [〈AI 提示詞完整地圖：NotebookLM 簡報、Claude 專屬助理、萬用工作流一次全收〉](https://www.bnext.com.tw/article/91010/ai-prompt-map-100-guide-productivity-tools) ，先建好可複用的提示詞庫，再把這些指令套進 Claude 的 Cowork 與 Skill。

> 延伸閱讀：  
> [17組萬用提示詞快收藏！從寫長文到管時間，丟給Claude、ChatGPT、Gemini都能用](https://www.bnext.com.tw/article/91079/17-essential-ai-prompts)  
> [NotebookLM簡報怎麼去AI味？3步驟學會YAML指令，附3款商務模板「複製就能用」](https://www.bnext.com.tw/article/91085/ai-presentation-yaml-style-guide)

關鍵字： [＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude) [＃提示詞（prompt）](https://www.bnext.com.tw/tags/%E6%8F%90%E7%A4%BA%E8%A9%9E%EF%BC%88prompt%EF%BC%89) [＃skill](https://www.bnext.com.tw/tags/skill)

往下滑看下一篇文章

<video title="Advertisement" src="https://rr4---sn-ipoxu-un5ez.googlevideo.com/videoplayback?expire=1787151647&amp;ei=n1SFaqjpNtuI1d8PibWuoAo&amp;ip=60.250.84.232&amp;id=6eb88a193e12486a&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=388&amp;met=1787122847,&amp;mh=cw&amp;mm=31&amp;mn=sn-ipoxu-un5ez&amp;ms=au&amp;mv=m&amp;mvi=4&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=46.764&amp;lmt=1785290979206108&amp;mt=1787122437&amp;cpn=lh2-BW2hhe21AUc3&amp;txp=6308224&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRQIhALwJmu9NEDGUKsio_hlad0t94RgSqdMbpjYcrXcWFmd_AiBMk1HuPjkDgzE7CaXR39xbmdPhvqQhd7IJD5k-WcBetQ==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRAIgaHiN8cyf0chce7n48R3qp-TWrZm14kcMznzOPCht9GACIBOnMT796TKIYAEg34HOAHeq5s9eUGIzpIMCbUN5p-MF" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.785.0_en.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1631513141%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B962325777%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C956056724%2Cnull%2C%5B%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323831%5D%2C%5B83323832%2C%5B%5B962325777%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398187%5D%2C%5B95398188%2C%5B%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%2C95338769%2C95393784%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=2062589489971309&amp;pvsid=1602487156255016&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F91080%2Fclaude-cowork-skills%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="169"></iframe><iframe src="about:blank" frameborder="0" width="1px" height="1px"></iframe><iframe src="about:blank" frameborder="0" width="1px" height="1px"></iframe><iframe src="about:blank" frameborder="0" width="1px" height="1px"></iframe><iframe src="about:blank" frameborder="0" width="1px" height="1px"></iframe>

即時熱門文章

[1 普發一萬要來了！賴清德宣布2027上路，怎麼申請、最新進度一次看](https://www.bnext.com.tw/article/91705/2026-cash-handout-10000-progress) [2 Uber Eats疊單爭議是什麼？外送專法計酬怎麼算？追溯補發、服務費調漲一次看](https://www.bnext.com.tw/article/91867/food-delivery-batch-orders-pay-dispute-ubereats-law) [3 AI封裝關鍵零件！矽電容概念股有哪些？力積電打進英特爾，這檔是台股「最純」](https://www.bnext.com.tw/article/91904/what-is-silicon-capacitor-stocks) [4 AI文章無所遁形？Anthropic推出「隱形浮水印」機制，一篇搞懂背後原理、限制和影響](https://www.bnext.com.tw/article/91882/claude-text-watermark-explained) [5 009828開募！首檔PCB ETF為何不配息？台光電、欣興兩檔權重就占近三成](https://www.bnext.com.tw/article/91889/009828-pcb-etf) [6 ChatGPT Work怎麼用？盤點14種上班族好用功能，如何打造超高效AI職場工作流？](https://www.bnext.com.tw/article/91878/chatgpt-work-14-features-cloud-vs-local)

![不只投廣告！在 AI 時代，無限創意行銷如何從企業營運出發，放大行銷成效？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/yey4-1785133327.jpg?w=900&output=webp)

2026.08.05 | [行銷與Martech](https://www.bnext.com.tw/categories/martech)

不只投廣告！在 AI 時代，無限創意行銷如何從企業營運出發，放大行銷成效？

生成式 AI 讓文案、圖片與廣告素材都能更快完成，當執行成本下降，企業判斷問題出在哪裡的能力，才是拉開差距、真正獲得成長的關鍵。

---

分享

一家連續十年營收 700 萬元的健身業者，如何在一年內將業績推升至 3,500 萬元，成長整整 5 倍？答案可能和多數人想的不一樣。

這家健身品牌不是一開始就加碼投放廣告，而是先從調整組織架構與薪酬制度著手，透過明確分工改善團隊運作，進而建立「全員銷售」文化，這並不是要求每位員工都成為業務員，而是讓團隊理解與認同品牌理念，確保從服務、回覆、教學到售後過程中的每一個顧客接觸點，都能傳遞一致的訊息與體驗，從而建立顧客對品牌的信任感，甚至願意主動購買與持續回購。而規劃這套成長策略的幕後推手，正是無限創意行銷。

## AI 讓行銷變簡單，為何行銷公司反而更難做出成效？

雖然公司名稱裡有「行銷」兩個字，但無限創意行銷並非只從行銷工具出發，而是以顧問式行銷的角色，先理解企業經營問題，再決定應採取哪些行銷與營運解方。「我們不會一開始就告訴企業該採用哪些行銷策略，而是根據企業真的需求來提供適合解方」，無限創意行銷創辦人翁小五一語道出公司的獨特定位。

通常，企業在面對營收成長瓶頸時，第一個念頭就是做行銷，然而，翁小五從過往的工作與創業經驗中發現，限制企業營收成長的原因有很多，不一定與行銷有關，背後可能還有其他問題，若沒有釐清真正的問題所在，就急著投入廣告或其他行銷資源，很可能只是治標不治本。

基於這樣的觀察，無限創意行銷決定採取反向思考邏輯：先回到企業經營本身，找出造成「成長趨緩」的真正原因，再據此制定策略、優化營運流程，最後透過行銷放大企業優勢。當每一筆行銷投入都建立在更健康的企業體質與更精準的策略判斷之上，行銷才不只是「有做」，而是真正能為企業帶來成長。

這套「先找問題、再選工具」的服務模式，在生成式 AI 工具普及後，也成為拉開行銷成效差距的關鍵。

過去，行銷公司的價值建立在製作能力、投放技術與媒體資源；但當 AI 逐步降低文案、素材與分析的執行成本，單純提供「做內容、下廣告、做報表」的服務，差異化將快速縮小。此時真正考驗的，反而是行銷人的理解力和判斷力：能否看懂消費者真正需要什麼？能否找出限制企業營收成長的關鍵環節？而這正是無限創意行銷長期深耕企業經營現場、不斷透過數據與市場驗證所累積的核心能力。

![無限創意行銷-2.JPG](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/gjjj-1785133327.jpg?w=1200&output=webp)

無限創意行銷創辦人 翁小五

圖／ 數位時代

## AI 拆解營收黑洞，讓數據為企業成長導航

在無限創意行銷的顧問式服務流程中，數據是相當重要的依據。翁小五進一步說明，無限創意行銷的數據應用可以分為三個層次：從檢視行銷活動成效、拆解顧客旅程中的問題節點，再進一步延伸至年度營運規劃，讓企業不只知道「過去發生了什麼」，更能判斷「下一步該怎麼走」。

首先，是檢視行銷活動成效，其中最重要的原則在於，根據行銷目標去解讀數據。舉例來說，如果目標是增加首購顧客，除了檢視素材吸引力與點擊率，還要持續追蹤到站、詢問、成交、新客占比與首購成本；如果目標是擴大營收規模，則應進一步檢視回購率、客單價與顧客終身價值。

數據應用的第二層價值，是拆解從「廣告投入」到「營收產出」之間的過程，理解每一個節點的數據分別反映什麼問題，再針對問題逐一修正。

至於第三個層次，則是讓數據成為企業經營與資源配置的依據。無限創意行銷會從年度營收目標出發，結合產業特性、淡旺季變化與過往行銷成果，拆解每一季、每個月的業績任務，再將過去有效的策略放大、成效不佳的做法修正，據此安排下一年度的行銷資源與營運節奏。

## 不急著加碼廣告，從企業體質出發找出 5 倍成長解方

對無限創意行銷來說，數據是找出問題、配置資源與規劃成長路徑的關鍵，文章開頭提到的健身業者，正是此套方法論的最佳印證。當初，翁小五深入診斷後發現，這家企業真正的問題在於組織結構過度扁平，老闆長期忙於處理日常事務，難以專注在營運規劃；此外，組織缺乏真正負責營運管理的中階主管、教練流動過高導致學員缺乏安全感等，皆是影響回購率的因素。

因此，無限創意行銷先從組織架構著手，設立專責店長一職，並重新設計薪酬制度，藉此讓教練、店長與老闆皆可專注在自身工作上。之後，再透過前期廣告數據與顧客洞察，找出消費者選擇品牌的真正原因，並運用行銷活動與口碑經營持續放大。同時，翁小五也在過程中導入「全員銷售」概念，確保顧客在各個接觸點都享有一致的體驗，品牌才能逐步累積顧客信任。

最終，這家企業的營收一年就成長至 3,500 萬元，達到原先的 5 倍。這個案例也說明了無限創意行銷的方法論：行銷不是企業成長的起點，而是放大企業優勢的槓桿；企業體質愈健全，這支槓桿所能放大的成長效益也愈大。

## 從「會做」到「會判斷」，AI 時代，無限創意行銷正在重新定義行銷公司的價值

隨著 AI 能力越來越強大，下一代行銷公司的價值，不只是比誰更懂得運用 AI，而是行銷人的市場理解力與專業判斷力，知道 AI 產出的內容哪些值得留下、哪些應該捨棄，或是該如何持續修正，才能打造出能夠被消費者感受到的品牌體驗。

因此，無限創意行銷未來將積極建立團隊與 AI 協作的方法論，將過往的專案判斷、提問框架與數據檢核邏輯，轉化為 AI 協作流程。團隊在面對新專案時，不是直接要求 AI 給答案，而是先定義目標、限制條件與判斷標準，再去檢查 AI 產出是否符合商業情境。

對翁小五而言，AI 時代真正稀缺的，不是工具，而是定義問題、理解消費者與判斷品質的能力。未來無限創意行銷的競爭力，也不在於使用多少 AI，而是持續將人的品味、市場理解與產業 Know-how，透過 AI 放大，協助企業打造更精準的品牌體驗與更高品質的決策。

即時熱門文章

[1 普發一萬要來了！賴清德宣布2027上路，怎麼申請、最新進度一次看](https://www.bnext.com.tw/article/91705/2026-cash-handout-10000-progress) [2 Uber Eats疊單爭議是什麼？外送專法計酬怎麼算？追溯補發、服務費調漲一次看](https://www.bnext.com.tw/article/91867/food-delivery-batch-orders-pay-dispute-ubereats-law) [3 AI封裝關鍵零件！矽電容概念股有哪些？力積電打進英特爾，這檔是台股「最純」](https://www.bnext.com.tw/article/91904/what-is-silicon-capacitor-stocks) [4 AI文章無所遁形？Anthropic推出「隱形浮水印」機制，一篇搞懂背後原理、限制和影響](https://www.bnext.com.tw/article/91882/claude-text-watermark-explained) [5 009828開募！首檔PCB ETF為何不配息？台光電、欣興兩檔權重就占近三成](https://www.bnext.com.tw/article/91889/009828-pcb-etf) [6 ChatGPT Work怎麼用？盤點14種上班族好用功能，如何打造超高效AI職場工作流？](https://www.bnext.com.tw/article/91878/chatgpt-work-14-features-cloud-vs-local)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;us_privacy=1---&amp;gpp_sid=-1&amp;client=ca-pub-7689852534818818&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=10&amp;lmt=1787122779&amp;plat=9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F91080%2Fclaude-cowork-skills&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1787122779389&amp;bpp=1&amp;bdt=60&amp;idt=37&amp;shv=r20260818&amp;mjsv=m202608130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dfc12e3aa82034b13%3AT%3D1763599281%3ART%3D1787122755%3AS%3DALNI_MZqxZynIWJ1XywRGxIpwCjPcmkh5Q&amp;gpic=UID%3D0000130f31c4a4e1%3AT%3D1763599281%3ART%3D1787122755%3AS%3DALNI_MYDZbFuviRCgsGvdADPZ2ghPcyIfw&amp;eo_id_str=ID%3Dd3be4a8693f0ee9e%3AT%3D1779170320%3ART%3D1787122755%3AS%3DAA-AfjZHnmnnaS7HrNE2facNrowo&amp;nras=1&amp;correlator=3220068171158&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397678%2C122880367%2C122880369%2C95390277%2C95373848&amp;oid=2&amp;pvsid=1602487156255016&amp;tmod=601506302&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.bnext.com.tw%2Farticles&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;plas=167x714_l%7C167x714_r&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=371" title="Advertisement" aria-label="Advertisement"></iframe>

[![台達電全解讀](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/e5i9-1785123864.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130221)