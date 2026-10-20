---
title: "Claude + Obsidian 打造 AI 第二大腦，Karpathy 的知識管理 LLM Wiki 教學｜科技翰林院"
source: "https://www.techhanlin.tw/claude-code-obsidian-second-brain/"
author:
  - "[[AI導演林思翰Hans]]"
published: 2026-04-14
created: 2026-05-18
description: "用 Claude Code 接上 Obsidian 本地資料庫，實作 Andrej Karpathy 提倡的 LLM Wiki 知識管理工作流，包含 Ingest 資料匯入、Query 智慧查詢、Lint 品質維護三大模組，打造一個能隨著你的學習自動進化成長的個人 AI 第二大腦。"
tags:
  - "clippings"
status: processed
---
2026 年 4 月，Andrej Karpathy 在 X 上丟出一個 gist，叫 LLM Wiki，5000 多顆星，1900 萬次曝光，在網路上引起熱烈討論！

Karpathy 是前 Tesla AI 總監、OpenAI 共同創辦成員，他的深度學習教學在 YouTube 累積數千萬觀看，是 AI 圈公認最有影響力的技術佈道者之一。

他的核心論點很簡單： ==LLM 最擅長的不是幫你思考，而是幫你做 bookkeeping，就是那些整理、歸檔、交叉引用的苦工。==

這個洞察讓一群開發者開始把 Claude Code 接上 Obsidian，打造真正能自我演化的 AI 第二大腦，變成一套可複製的工作流。

這篇文章會帶你從零開始，用 Claude Code Obsidian 組合建立你自己的 AI 知識管理系統，包含實際設定步驟、使用心得等。

## Claude Code 不只是寫程式的工具，它是你的終端機 AI 特助

很多人聽到 Claude Code 第一反應是：一個 AI 寫程式的工具？不是。

Claude Code 是 Anthropic 在 2025 年推出的 agentic CLI 工具，直接住在你的終端機裡。

你用自然語言告訴它要做什麼，它自己讀檔案、改檔案、跑指令、管 git，全程不需要你一行一行指揮。

跟 GitHub Copilot 那種自動完成下一行的模式完全不同，Claude Code 是 Agent 級別的自主操作，你說「幫我把這個資料夾裡所有 Markdown 檔案加上 YAML frontmatter」，它就真的會掃描整個目錄、判斷哪些檔案缺少 metadata、然後一個一個補上。

這個能力，才是它跟 Obsidian 搭配的關鍵。

### Claude Code 和 AI 第二大腦的關係

截至 2026 年 4 月，Claude Code 已經不是剛推出時的樣子了，GitHub 上每天有超過 13.5 萬筆公開 commit 來自 Claude Code，佔全平台 4%。它的能力清單很長，但跟知識管理最相關的是這幾個：

| 能力 | 說明 | 知識管理用途 |
| --- | --- | --- |
| 檔案讀寫 | 直接讀取、建立、編輯任何本機檔案 | 操作 Obsidian vault 裡的所有.md 筆記 |
| CLAUDE.md 持久記憶 | 每次啟動自動載入專案根目錄的 CLAUDE.md | 記住你的 vault 結構、筆記規範、偏好設定 |
| Slash Commands | 自訂指令放在.claude/commands/ 目錄 | 一鍵執行常用工作流（每日筆記、週回顧） |
| Subagent 平行處理 | 把複雜任務拆給多個子代理同時執行 | 同時掃描多個資料夾、批次加標籤 |
| MCP 協議 | 透過 Model Context Protocol 連接外部服務 | 橋接 Obsidian 插件、外部資料來源 |

**Claude Code 核心能力與 AI 第二大腦應用對照**

2026 年新增的排程代理（Scheduled Agents）功能特別值得注意。

你可以設定 Claude Code 每天早上自動整理昨天的筆記、每週五下午產出週回顧，不是手動觸發而是真正的自動化。

### CLAUDE.md 是 AI 第二大腦的持久記憶層

CLAUDE.md 這個檔案是整個工作流的基石，放在 Obsidian vault 根目錄，Claude Code 每次啟動都會自動讀取，你可以把它想成 AI 助理的工作手冊：你是誰、vault 的資料夾結構怎麼安排、筆記格式規範（frontmatter 必須包含哪些欄位），什麼可以做什麼不能做。

這個設計解決了 AI 工具最大的痛點：每次開新對話都要重新解釋一次背景。

有了 CLAUDE.md，Claude Code 永遠記得你的規則，你不需要每次都說「我的筆記用 PARA 架構」或「標籤只能用小寫英文」，寫一次就好。

![Obsidian 筆記軟體官方圖片，local-first 的 Markdown AI 第二大腦知識管理工具](https://www.techhanlin.tw/wp-content/uploads/2026/04/obsidian-knowledge-management-1.png)

Obsidian 筆記軟體官方圖片，local-first 的 Markdown AI 第二大腦知識管理工具

## Obsidian 為什麼是 AI 知識管理最佳載體

市場上筆記工具這麼多，為什麼偏偏是 Obsidian？答案很直接： ==它的所有筆記就是你電腦上的.md 純文字檔。==

沒有專有格式、沒有雲端資料庫、沒有 API 才能存取的限制。（延伸閱讀： [Obsidian 是什麼？完整介紹](https://www.techhanlin.tw/obsidian-what-is-note-app/) ）

Claude Code 是一個在檔案系統上工作的工具，Obsidian 的資料就在檔案系統上，天生相容，零整合成本。

### Claude Code Obsidian 天生相容的 3 個技術原因

第一，純 Markdown。Claude Code 讀寫.md 檔案跟呼吸一樣自然，不需要任何轉換層。Notion 的資料存在雲端資料庫裡，你要先透過 API 拉下來、轉格式、處理完再推回去，Obsidian 不用，直接改就好。

第二，雙向連結語法。Obsidian 用 \[\[wikilink\]\] 串連筆記，Claude Code 可以輕鬆解析這些連結、建立新連結、甚至掃描整個 vault 找出你漏掉的關聯。這個能力在處理大量筆記時特別有感。

第三，YAML frontmatter。YAML Frontmatter 是一種用於 Markdown 檔案開頭的特定格式區塊，主要用來存放該檔案的元數據，每篇筆記開頭的 metadata 區塊（tags、date、status、type），Claude Code 可以批次讀取、篩選、更新。你想找出所有 status 是 draft 的筆記？一句話的事。

### AI 第二大腦選擇 local-first 工具的長期好處

我們公司買 Notion 企業版很多年，一年要花 1800 美元在上面，Notion 在 2025 年調漲過一次價格，用戶的資料全鎖在他們的平台上。

要搬家？匯出的 Markdown 品質參差不齊，表格和資料庫幾乎無法完整帶走。

Obsidian 的筆記就是檔案，你今天用 Obsidian 打開，明天可以用 VS Code、Typora、甚至純文字編輯器打開，內容一模一樣。

就算 Obsidian 公司明天消失了，你的知識庫一個字都不會少，對於打算長期累積的第二大腦來說，這個保障比任何功能都重要。

![Obsidian 筆記軟體 local-first 架構，所有檔案存在本機的資料夾結構](https://www.techhanlin.tw/wp-content/uploads/2026/04/CleanShot-2026-04-14-at-15.59.58-2x.jpg)

Obsidian 筆記軟體 local-first 架構，所有檔案存在本機的資料夾結構

### Obsidian 定價方案：個人用完全免費

很多人最關心的問題：Obsidian 要錢嗎？個人使用完全免費，沒有功能限制、沒有筆記數量上限、沒有試用期。

你下載就能用，所有核心功能都包含在內。

| 方案 | 費用 | 說明 |
| --- | --- | --- |
| Obsidian 個人版 | 免費 | 所有核心功能，無限筆記，無限插件 |
| Obsidian 商業版 | $50 美元/年 | 公司內部使用需購買商業授權 |
| Obsidian Sync（選配） | $4 美元/月 | 跨裝置同步，端對端加密。也可用 iCloud/Git 免費替代 |
| Obsidian Publish（選配） | $8 美元/月 | 把筆記發布成網站，大多數人不需要 |
| Claude Pro 訂閱 | $20 美元/月 | 使用 Claude Code 的前提，包含 Opus 和 Sonnet 模型 |

**Obsidian 與 Claude Code 定價方案比較，個人用 AI 第二大腦的成本**

換句話說，打造 AI 第二大腦的最低成本就是 Claude Pro 的每月 20 美元。Obsidian 本身不花你一毛錢。

跨裝置同步如果不想付 Sync 的費用，用 iCloud（Mac/iOS）或 Git 就能解決，這個價格門檻，比任何同級方案都低。

## 用 2 種方式把 Claude Code 接上 Obsidian Vault

設定其實比想像中簡單，不需要寫程式，不需要裝一堆東西，以下三種方式從最簡單到最進階排列。

### 1.Claude Code 直接進 Vault 目錄，30 秒搞定

最簡單的方式，打開終端機，cd 到你的 Obsidian vault 目錄，然後輸入 claude 就好了。（延伸閱讀： [Obsidian 新手教學](https://www.techhanlin.tw/obsidian-tutorial-beginner-guide/) ）

Claude Code 會自動掃描目錄結構，讀取 CLAUDE.md（如果你有放的話），然後你就可以開始用自然語言操作你的筆記庫了。想試試看的話，第一個指令可以打：「幫我列出這個 vault 裡所有的資料夾結構和筆記數量」。

進階一點，你可以在 vault 根目錄建立.claude/commands/ 資料夾，放入自訂的 slash command。例如建一個 today.md 檔案，裡面寫「檢查今天的 daily note 是否存在，沒有就用 templates/daily.md 範本建立，然後從昨天的筆記搬入未完成的待辦事項」。之後每天打 /today 就自動執行。

### 2.用 MCP 橋接和 Obsidian 插件打造完整 AI 第二大腦

如果你不想離開 Obsidian 介面，有幾個插件可以直接在 Obsidian 裡面跑 Claude Code。

Agent Client 是目前最完整的選擇，支援 Claude Code、Codex、Gemini CLI 三種 AI 工具，可以用 @notename 語法直接引用筆記內容給 AI 參考。

另一個方向是 MCP 橋接，obsidian-claude-code-mcp 這個插件把 Obsidian vault 的操作註冊成 MCP 工具，讓 Claude Code 透過標準化的 Model Context Protocol 存取你的筆記。好處是可以跟 Claude Desktop 等其他 Anthropic 工具共用同一套 vault 存取邏輯。

不過說真的，如果你剛開始嘗試，直接用終端機進 vault 就夠了。插件和 MCP 是你用熟之後、想要更深度整合時再考慮的東西。不要一開始就搞太複雜，反而失去動力。

## Karpathy LLM Wiki 模式，最聰明的知識管理方法論

Karpathy 的 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 概念在 2026 年 4 月爆紅不是沒有原因的，他在原文裡開宗明義就點出大多數人使用 AI 的方式有根本性的問題：

“Most people’s experience with LLMs and documents looks like RAG: you upload a collection of files, the LLM retrieves relevant chunks at query time, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There’s no accumulation.”

翻成白話： ==大多數人用 AI 處理文件的方式就是 RAG，上傳一堆檔案，AI 在你提問時撈出相關片段拼答案。能用，但每次都從零開始。== 沒有累積，沒有沉澱。NotebookLM、ChatGPT 上傳檔案、大部分 RAG 系統都是這樣。

Karpathy 提出的替代方案完全不同：

“Instead of just retrieving from raw documents at query time, the LLM incrementally builds and maintains a persistent wiki — a structured, interlinked collection of markdown files that sits between you and the raw sources.”

不是每次查的時候才去原始文件撈，而是讓 LLM 持續建構和維護一個 wiki：一組結構化、互相連結的 Markdown 檔案，夾在你和原始資料之間。新資料進來不只是「存起來等查詢」，而是被消化、提取、整合到既有的知識網路裡。

他用一句話總結這個核心差異： ==“The wiki is a persistent, compounding artifact.”==

Wiki 是一個持久且不斷複利的產物，交叉引用已經建好了，矛盾已經被標記了，綜合分析已經反映了你讀過的所有東西。

每加一筆新資料、每問一個新問題，wiki 都會變得更豐富。

![Karpathy LLM Wiki 知識管理模式，用 Obsidian 建立 AI 第二大腦的三層架構](https://www.techhanlin.tw/wp-content/uploads/2026/04/CleanShot-2026-04-14-at-16.05.59-2x-scaled.jpg)

Karpathy LLM Wiki 知識管理模式，用 Obsidian 建立 AI 第二大腦的三層架構

### Obsidian 是 IDE，LLM 是工程師，Wiki 是程式碼

Karpathy 在原文裡用了一個很精準的比喻來描述他自己的實際使用方式：

“In practice, I have the LLM agent open on one side and Obsidian open on the other. The LLM makes edits based on our conversation, and I browse the results in real time — following links, checking the graph view, reading the updated pages. Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase.”

實際操作是這樣： ==一邊開著 LLM Agent（像 Claude Code），一邊開著 Obsidian。==

AI 根據對話內容編輯筆記，你即時瀏覽結果：追連結、看知識圖譜、讀更新後的頁面。

Obsidian 是 IDE，LLM 是工程師，wiki 是程式碼。

==這個比喻完美說明了為什麼 Obsidian 是最佳載體：它本來就是為了瀏覽和操作大量互連的 Markdown 檔案而設計的。==

### Claude Code Obsidian 實作 Ingest、Query、Lint 三步驟

Karpathy 把 LLM Wiki 的操作歸納成三個核心動作，用 Claude Code + Obsidian 都可以直接實作：

**Ingest（攝入）** ：把新資料餵給 Claude Code，它會自動提取重點、寫成結構化筆記、更新相關頁面的交叉引用。Karpathy 指出一次 ingest 可能會動到 10-15 個 wiki 頁面。例如你貼一篇論文的 PDF 進 vault，跟 Claude Code 說「ingest 這篇到 resources/ 目錄」，它會產出摘要頁，並在相關的專案筆記裡加上 \[\[連結\]\]。

**Query（查詢）** ：提問時 Claude Code 會搜索 vault 裡的筆記，從你自己累積的知識（不是它的訓練資料）合成答案。原文裡特別強調一個關鍵洞察：”Good answers can be filed back into the wiki as new pages… This way your explorations compound in the knowledge base just like ingested sources do.” ==好的回答應該被存回 wiki 變成新頁面，這樣你的探索也會像新資料一樣在知識庫裡產生複利。==

**Lint（健檢）** ：定期讓 Claude Code 掃描整個 vault，找出矛盾的資訊、過時的內容、孤立的頁面、缺失的交叉引用。

Karpathy 列出的健檢清單包括：頁面之間的矛盾、被新資料取代的舊結論、沒有任何連入連結的孤兒頁面、被提到但還沒有獨立頁面的重要概念。這個最省時間，也是人類最容易偷懶跳過的步驟。

### AI 第二大腦不是讓 AI 幫你思考，是讓它幫你整理

這邊要講一個很多人搞混的觀念， ==AI 第二大腦的重點不是讓 AI 替你想，而是讓 AI 幫你把想過的東西整理好。==

你讀了一篇文章、聽了一場演講、開了一個會，產出的筆記和想法是你的。

Claude Code 的角色是把這些散落各處的思考碎片串連起來，讓你下次需要的時候找得到。

Karpathy 在原文的結尾用最精簡的方式講清楚了這個分工：

“The tedious part of maintaining a knowledge base is not the reading or the thinking — it’s the bookkeeping. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don’t get bored, don’t forget to update a cross-reference, and can touch 15 files in one pass.”

==維護知識庫最煩的不是閱讀或思考，是 bookkeeping，就是那些苦工。人類會放棄維護 wiki 是因為維護成本的增長速度比價值還快。LLM 不會無聊，不會忘記更新交叉引用，而且一次能動 15 個檔案。==

他最後還拉了一個歷史脈絡：這個概念跟 1945 年 Vannevar Bush 提出的 Memex 異曲同工：一個私人的、主動策展的知識庫，文件之間的關聯跟文件本身一樣有價值。

Bush 當年解決不了的問題是誰來做維護，現在 LLM 解決了。

“The human’s job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM’s job is everything else.”

人的工作是策展資料來源、引導分析方向、問好問題、思考這一切代表什麼意義。其他所有事情，都交給 LLM。

強烈推薦大家去讀 Karpathy 的 [LLM Wiki 原文](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) ，整篇不長但每一段都是精華，從架構設計到實際操作技巧都有涵蓋。

我們科技翰林院上面的介紹只是重點摘錄，原文還有更多關於三層架構（Raw Sources / Wiki / Schema）、index 和 log 的設計、以及 CLI 工具選擇的細節。

## 實戰工作流：用 Claude Code Obsidian 可以做的 5 件事

講完理論來講實際操作，以下 5 個工作流都是社群裡驗證過的超好用方法。

### AI 第二大腦的每日筆記自動生成和研究整理

**1.每日筆記** ：設定一個 /today slash command，Claude Code 會檢查今天的 daily note 是否存在，沒有就用範本建立。範本可以包含今天的待辦事項區塊、昨天未完成的 tasks 自動搬入、本週重點專案的快速連結。整個過程不到 10 秒。

**2.研究筆記** ：跟 Claude Code 說「Research WebAssembly SIMD 的最新進展，存到 resources/ 目錄」。它會先搜尋 vault 裡已有的相關筆記，避免重複，然後綜合外部資訊產出一篇結構化的研究筆記：概述、關鍵概念、跟 vault 裡其他筆記的關聯、資料來源。自動加好 wikilink。

### Claude Code 自動加反向連結、批次標籤、每週回顧

**3.批次加反向連結** ：這個工作流最讓人驚艷，讓 Claude Code 掃描你最近一個月的 daily notes，找出所有提到的人名、專案名、書名，然後自動加上 \[\[wikilink\]\] 連結到對應的 entity note，沒有對應筆記的就自動建立，一個下午的手動工作，幾分鐘搞定。

**4.批次加標籤** ：vault 裡有 200 篇筆記沒有 tags？讓 Claude Code 根據內容自動分類，它會讀取每篇筆記的內容，參考你在 CLAUDE.md 定義的標籤規範，在 YAML frontmatter 裡補上適當的 tags。

**5.每週回顧** ：設定 /weekly-review 指令，Claude Code 統計本週完成和未完成的 tasks、辨識重複出現的主題、找出哪些專案停滯了、產出一份結構化的回顧筆記。這份回顧本身也是 Obsidian 筆記，可以被未來的 AI 查詢引用，這就是 AI 第二大腦的複利效應。

## 推薦的 Obsidian Vault 結構和 CLAUDE.md 範本

很多人卡在不知道怎麼開始，這邊給一個經過社群驗證的 vault 結構：

| 資料夾 | 用途 | Claude Code 怎麼用它 |
| --- | --- | --- |
| CLAUDE.md | AI 助理的工作手冊 | 每次啟動自動讀取 |
| .claude/commands/ | 自訂 slash commands | 存放 /today、/weekly-review 等指令 |
| daily/ | 每日筆記（YYYY-MM-DD.md） | 自動建立、搬入未完成待辦 |
| projects/ | 進行中的專案筆記 | 追蹤專案進度和待辦 |
| areas/ | 持續責任區域（健康、財務等） | 定期回顧和更新 |
| resources/ | 參考資料和研究筆記 | ingest 新資料的目標目錄 |
| \_inbox/ | 待處理收件匣 | 定期清空、歸檔到正確位置 |
| templates/ | 筆記範本 | 建立新筆記時引用 |

**Claude Code Obsidian 推薦的 Vault 資料夾結構**

CLAUDE.md 的內容建議控制在 100 行以內，太長會浪費 token。核心要寫清楚的是：你的身份和工作領域、資料夾結構的邏輯、筆記格式規範（frontmatter 必填欄位、wikilink 使用慣例）、以及 Claude Code 的行為邊界（例如不刪除任何檔案、不改檔名）。

### Claude Code 操作 Vault 的 3 個實用技巧

第一，善用.claude/commands/。每個常用工作流都寫成 slash command，而不是每次手打一長串指令。Claude Code 會讀取 commands 目錄裡的.md 檔案作為指令範本。檔名就是指令名稱。

第二，在 CLAUDE.md 裡定義不做清單。AI 工具最危險的地方不是它不會做什麼，而是它太積極了。明確寫出 NEVER 刪除任何檔案、NEVER 修改已有的 frontmatter tags 只能新增，這類規則，可以避免 Claude Code 好心辦壞事。

第三，定期跑 lint。每週讓 Claude Code 做一次 vault 健檢：找出沒有任何 backlink 的孤立筆記、frontmatter 缺少必填欄位的筆記、超過 90 天沒更新的 draft 筆記。這個習慣養成之後，你的知識庫品質會明顯提升。

![Claude Code 在 Obsidian Vault 中自動整理筆記的操作畫面](https://www.techhanlin.tw/wp-content/uploads/2026/04/CleanShot-2026-04-14-at-20.25.11-2x-2048x1232.jpg)

Claude Code 在 Obsidian Vault 中自動整理筆記的操作畫面

## 結論：Claude Code Obsidian 是知識工作者的新基礎設施

Claude Code + Obsidian 的組合之所以在 2026 年爆發，不是因為哪個工具特別厲害，而是它們加在一起剛好解決了知識管理最核心的問題：人類不擅長做 bookkeeping，AI 擅長。你負責閱讀、思考、產出洞察，Claude Code 負責整理、連結、維護。這個分工很自然，也很有效率。

Karpathy 的 LLM Wiki 概念把這套邏輯講得最清楚：Ingest 攝入新知、Query 查詢累積的知識、Lint 定期健檢。三個動作循環下去，你的 AI 第二大腦就會像滾雪球一樣越來越有價值。

如果你還沒試過，建議今天就開始。步驟很簡單：安裝 Claude Code（需要 Claude Pro 訂閱）、開一個 Obsidian vault、在根目錄放一個 CLAUDE.md、然後在終端機 cd 進去打 claude。花 10 分鐘設定，換來的是一個會自己進化的知識庫。科技翰林院會持續分享 AI 工具的實戰應用和深度分析，如果這篇對你有幫助，歡迎收藏並分享給需要的朋友。

---

## 推薦閱讀

[Obsidian 是什麼？一篇搞懂這款筆記軟體為什麼讓工程師和研究者都瘋狂](https://www.techhanlin.tw/obsidian-what-is-note-app/)

延伸閱讀： [2026 最新 ChatGPT 教學：老闆必學的企業 AI 代理人配置指南](https://www.techhanlin.tw/chatgpt-tutorial-ai-agent-guide-2026/)

[Obsidian 新手教學：從下載到上手，完整操作指南](https://www.techhanlin.tw/obsidian-tutorial-beginner-guide/)

[Happy Coder 是什麼？手機遙控 Claude Code 最簡單方法，實測 6 方案比較](https://www.techhanlin.tw/happy-coder-claude-code-mobile/)

---

## 參考資料

[Anthropic (2025). “Claude Code Overview”](https://docs.anthropic.com/en/docs/claude-code/overview)

[Andrej Karpathy (2026). “LLM Wiki”](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

[Obsidian (2026). “Obsidian – Sharpen your thinking”](https://obsidian.md/)

[XDA Developers (2025). “I put Claude Code inside Obsidian, and it was awesome”](https://www.xda-developers.com/claude-code-inside-obsidian-and-it-was-eye-opening/)

[DEV.to (2025). “Claude Code + Obsidian: Build a Second Brain That Actually Thinks”](https://dev.to/mibii/claude-code-obsidian-build-a-second-brain-that-actually-thinks-d61)

[Anthropic (2025). “Claude Code – GitHub Repository”](https://github.com/anthropics/claude-code)

[Builder.io (2026). “Claude Code vs Cursor 2026”](https://www.builder.io/blog/cursor-vs-claude-code)