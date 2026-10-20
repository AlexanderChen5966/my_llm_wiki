---
title: "Claude是什麼？claude ai教學：38篇Cowork、Skills、省Token秘訣全打包"
source: "https://www.bnext.com.tw/article/91080/claude-cowork-skills"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-06-01
created: 2026-06-01
description: "Claude在近期大幅更新後，舊的長指令用法效果已大不如前。本文整理38篇教學，涵蓋模型選用、提示詞技巧到進階代理人設定，幫助使用者少走冤枉路。"
tags:
  - "clippings"
status: processed
---
2026 年，Anthropic 的更新速度明顯加快。

今年以來，Claude 幾乎每兩週就有一次重大更新，主要變動集中在模型能力提升、新增 Claude Code 編程工具、代理人功能擴充，以及更長的上下文處理能力。

這些改動對一般使用者的影響在於，過去那套用法，有些已經不適用了。以往流行的長指令、細節堆疊方式，在新版模型上效果反而不如預期。目前比較有效的做法是指令簡短但明確，讓模型自行判斷細節。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

為了幫使用者少走冤枉路，《數位時代》整理了 38 篇教學文章，推出這份「Claude 實戰地圖」，從要選哪個方案、怎麼下指令，到怎麼讓 Claude 住進 Excel、Word 裡幫你工作，全部一次整理清楚。

圖／ 數位時代

這份地圖不必 38 篇一次讀完，我們建議你：先看完第一章搞懂方案與模型怎麼選，再依工作場景跳到對應段落。常做簡報的人直接看 Cowork 與 Design，每天跟 Excel、Word 搏鬥的人則從第六章開始。

#### 點擊可直接前往該段落：

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

Anthropic 的更新不會停，這份地圖收錄的功能與技巧，半年後很可能又有新版本。《數位時代》編輯團隊也會持續追蹤，把值得一試的用法補進清單。

另外，如果你想一次掌握跨模型的指令邏輯，從基礎寫法、各家官方指南，到職場商務、視覺生圖與翻譯校正，可以搭配我們收錄超過 100 組實戰指令的 [〈AI 提示詞完整地圖：NotebookLM 簡報、Claude 專屬助理、萬用工作流一次全收〉](https://www.bnext.com.tw/article/91010/ai-prompt-map-100-guide-productivity-tools) ，先建好可複用的提示詞庫，再把這些指令套進 Claude 的 Cowork 與 Skill。

> 延伸閱讀：  
> [17組萬用提示詞快收藏！從寫長文到管時間，丟給Claude、ChatGPT、Gemini都能用](https://www.bnext.com.tw/article/91079/17-essential-ai-prompts)  
> [NotebookLM簡報怎麼去AI味？3步驟學會YAML指令，附3款商務模板「複製就能用」](https://www.bnext.com.tw/article/91085/ai-presentation-yaml-style-guide)