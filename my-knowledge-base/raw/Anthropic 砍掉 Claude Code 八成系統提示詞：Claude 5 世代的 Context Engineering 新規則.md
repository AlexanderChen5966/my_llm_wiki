---
title: "Anthropic 砍掉 Claude Code 八成系統提示詞：Claude 5 世代的 Context Engineering 新規則"
source: "https://cyberq.tw/2026/07/28/anthropic-removes-80-of-system-prompt/"
author:
  - "[[Chen Glenn]]"
published: 2026-07-28 12:35:00Asia/Taipei
created: 2026-07-28
description: "Claude Opus 5 正式發布後，Anthropic 技術團隊成員 Thariq Shihipar 日前..."
tags:
  - "clippings"
status: processed
---
Claude Opus 5 正式發布後，Anthropic 技術團隊成員 Thariq Shihipar 日前發表了一篇標題為〈 [The new rules of context engineering for Claude 5 generation models](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) 〉的文章。內容的重點只有一句話：他們針對 Claude Opus 5、Claude Fable 5 等新世代模型，把 Claude Code 的系統提示詞刪掉了超過八成，而內部程式開發評測（coding evals）沒有出現可測量的退步。需要注意的是，這項縮減僅套用於新一代模型，是模型能力跨過門檻後的重新校準。

對於過去兩年不斷把規則寫進 CLAUDE.md、把範例塞滿 system prompt 的開發團隊而言，這是一個相當難堪的結論，那些辛苦累積的規則，有很大一部分不但沒有幫助，反而正在拖累新一代模型。

## 一、核心論點：Anthropic 承認自己「過度約束」了模型

Anthropic 用了「unhobbling」（解除束縛）這個字來描述這次調整。官方文章指出，他們在系統提示詞、CLAUDE.md 與 Skills 三個層次上同時過度限制了 Claude Code。

最具說服力的證據來自內部使用記錄的逐字稿，在同一個請求中，模型會同時收到「視情況留下文件說明」與「不要加註解」這類互相衝突的指示，因為系統提示詞、Skill 與使用者請求彼此打架。模型當然還是能推測出使用者的真正意圖，但它必須先花費推理資源去仲裁這些矛盾，才能決定要動哪一行程式碼。

換句話說，問題不在於「規則太多」這種數量問題，而在於規則之間的衝突消耗了模型本來可以拿來解題的認知預算。

值得注意的是，Anthropic 並未否認這些護欄過去的價值。文章明確承認，在舊模型上若沒有這些約束，Claude 寫出的註解在許多情況下會是錯的，當時只能接受這個取捨。改變的是模型判斷力，不是原則。

## 二、六組「過去 vs 現在」對照

官方文章列出六項已經過時的最佳實踐。以下整理為對照表：

| 過去的做法 | 現在的做法 | 說明 |
| --- | --- | --- |
| 給 Claude 規則 | 讓 Claude 自行判斷 | 硬性禁令改為情境式引導 |
| 給 Claude 範例 | 設計工具介面 | 範例會把模型限縮在特定探索空間 |
| 全部資訊前置塞入 | 漸進式揭露（progressive disclosure） | 需要時才載入對應 Skill 或工具定義 |
| 重複強調同一件事 | 精簡的工具描述 | 指示寫在工具描述裡，不在系統提示詞重複 |
| 用 CLAUDE.md 當記憶體 | 自動記憶（auto-memory） | 模型自行儲存與工作相關的記憶 |
| 簡單的 Markdown 規格 | 豐富的參考資料 | 程式碼、測試套件、HTML artifact、評分準則 |

其中最常被引用的是註解規則的改寫。舊版系統提示詞以近乎命令的方式禁止模型撰寫多段式 docstring 與多行註解區塊，甚至規定「除非使用者要求，不得建立規劃、決策或分析文件，從對話脈絡工作，而非產生中間檔案」，新版則替換為一句判斷式引導：

> Write code that reads like the surrounding code: match its comment density, naming, and idiom.

這個改寫的價值在於它把「規範」轉譯成「品味」。前者在遇到例外情境時必然出錯，後者則能讓模型自行從程式庫的既有風格中推導出正確答案。

「範例改為介面設計」這一項同樣值得開發者注意。官方以 Todo 工具為例，把狀態欄位定義成 pending、in\_progress、completed 的列舉值，本身就已經向模型暗示了使用方式；再加上「同時只保留一項 in\_progress」這樣的約束，行為就被定義完成，不需要再附上範例。Schema 設計取代 few-shot，是這波轉向裡最具工程意涵的一條。

漸進式揭露則不只用於 Skills。Claude Code 已有部分工具（如 Task 類工具）採用「延遲載入」（deferred loading），代理必須先透過 ToolSearch 搜尋才能取得完整定義，讓工具數量可以擴張而不佔用上下文。

至於「豐富的參考資料」，官方給出的例子比表格更具體：規格可以是一份詳盡的測試套件，或另一個程式庫中待移植的函式，而評分準則（rubrics）的用法是搭配 dynamic workflows 啟動帶有該準則的驗證代理（verifier agents），讓 Claude 得以核對特定領域的品味判斷，例如「什麼是好的 API 設計」這類難以用規則窮舉的問題。

## 三、官方建議的四層 Context 架構

文章最後給出實作層面的分工建議，這部分對自建 agent harness 的團隊價值最高：

**System Prompt** ：與產品情境高度綁定，說明 Claude 正在哪個產品裡、正在做什麼。使用 Claude Code 的人幾乎不會動到它，但若你在打造自己的 agent，這裡才是應該投入大量時間的地方。

**CLAUDE.md** ：保持輕量。簡短描述 repo 的用途即可，把大部分 token 花在「陷阱」（gotchas）上，例如型別全部集中在單一檔案這類反直覺的組織方式。凡是 Claude 讀一下檔案系統就能知道的事，不要寫。

**Skills** ：定位為「讓 Claude 需要時能找到資訊的輕量指南」，除非是高度關鍵的領域，否則避免寫成強約束。長 Skill 應拆成多檔案，同樣採漸進式揭露。Skill 最適合承載的是屬於你、你的團隊或你的產品的特定觀點與知識。

**References** ：以 @ 提及檔案作為參考資料。官方明確表示應優先使用程式碼形式的參考，因為那是模型最熟悉的語言。一份 HTML mockup 通常會比一段文字描述或一張截圖產生更好的結果。

Anthropic 同時推出了 /doctor 指令（claude doctor），可自動盤點並精簡使用者的 Skills 與 CLAUDE.md。第三方測試指出該指令會先報告它想刪除的內容再實際修改，屬於低風險的稽核方式，不過需要較新版本的 Claude Code 才能使用，建議實際執行前先確認版本。

## 四、市場趨勢：Context Engineering 走到分岔路

這篇文章之所以在社群引發大量討論，是因為它剛好撞上了 2026 年 AI 工程領域的兩個大趨勢。

**第一個趨勢是標準化。** AGENTS.md 由 OpenAI 於 2025 年 8 月發布、多家工具商共同推動，並於 2025 年 12 月被納入 Linux Foundation 旗下的 Agentic AI Foundation（AAIF），與 Anthropic 的 MCP 同屬一個基金會。截至 2026 年初，採用該格式的開源專案已超過 60,000 個，並被 30 種以上工具原生讀取，包含 Claude Code、Codex CLI、Cursor、Gemini CLI、Copilot、Windsurf、Devin、Amazon Q 等。指令檔的格式戰事實上已經結束。

**第二 **個趨勢** 是上下文的成本與污染意識。** 業界對「上下文越大越好」的信仰在 2026 年明顯反轉。工具定義的膨脹是典型案例，單一複雜的 JSON schema 就可能吃掉 500 個以上的 token，接上數個 MCP server 後，光是工具定義就可能突破 50,000 token，OpenAI 則建議單一 agent 的工具數控制在 20 個以內，超過 10 個之後準確率就開始下降。Anthropic 的延遲載入工具，正是對同一個問題的另一種解法。

在這個脈絡下，Anthropic 這篇文章其實不是孤立事件，而是「上下文精簡化」這趨勢中最具份量的一次背書，由模型供應商親自出面，用自家旗艦產品的評測資料，宣告舊的堆疊式做法已經失效。

## 五、反面評論：三個必須保留的懷疑

CyberQ 認為，除了樂觀敘事之外，我們也提出了幾個值得台灣團隊納入考量的保留意見。

**其一，樣本數問題。** 可以發現該公司 X 上的貼文與官方部落格文章出自同一人、同一篇內容，那是「一位位置絕佳的工程師對單一產品演進的自述」，不是兩個獨立的驗證來源，更不足以構成普遍法則。這個提醒相當中肯，「80% 可刪」是 Claude Code 在 Anthropic 自家評測集上的結果，未必能外推到你的法遵審核 agent 或客服 agent。

**其二，研究結論並不一致。** 普林斯頓研究團隊曾針對 10 個 repo、124 個已合併 PR 進行對照實驗，結果顯示 AGENTS.md 確實有幫助，但另一組研究者發現，由 LLM 自動生成的 AGENTS.md 反而略微降低任務成功率，同時使成本增加約 23%，人工撰寫的版本才有約 4% 的改善。ETH 的研究則指出，架構總覽式的內容會增加推論成本並誘使模型進行更廣泛的檔案巡覽，卻未提升任務成功率。這些結果的共同概念其實並不是怪寫多寫少這種問題，而是寫的是不是模型自己推不出來的東西，這一點恰好與 Anthropic 對 CLAUDE.md 的建議完全一致。

**其三，治理與資安的張力。** 從企業視角看，讓模型自行判斷與可稽核、可解釋之間存在結構性衝突。RSA 2026 的一個明顯共識是，上下文視窗本身就是 AI agent 的主要控制點與新的安全邊界，因為模型無法區分系統提示詞、使用者輸入與檢索內容，每一個 token 都可能是指令或攻擊面。

有趣的是，這個結論與 Anthropic 的建議在方向上是一致的，最小權限原則同樣支持精簡上下文。但真正不能刪的是另一類東西，也就是法遵邊界、資料存取範圍、稽核紀錄要求。這些不屬於模型判斷力可以取代的護欄，而且在 EU AI Act 於 2026 年 8 月進入執法階段後，只會變得更硬噢。

另外，CyberQ 發現某些媒體或社群會流傳「系統提示詞由 800 token 縮減至 164 token」以及不同的發布日期等具體數字，這些細節並沒有經過官方確認，觀察官方唯一給出的量化說法是移除超過 80% ，以及評測無可測量的損失。

## 六、給開發團隊與 MSP 的實務建議

綜合官方指引與上述保留意見，我們建議以下的稽核順序：

**先盤點衝突，而非先刪除長度。** 把 system prompt、CLAUDE.md、Skills 三層攤開比對，優先移除彼此矛盾的指示。這是收益最高、風險最低的一步。

**建立評測後再動刀。** 官方之所以敢刪，是因為有 evals 撐腰。沒有評測集就大幅刪減，等於用生產環境做實驗。可採「刪一條、跑一次」的漸進方式。

**用「模型推不出來嗎？」當作保留標準。** 建置指令、目錄結構、慣用框架這類從 repo 就能讀出的資訊，刪。反直覺的組織方式、歷史包袱、不能碰的模組，留。

**把驗證流程改寫成 Skill。** 與其在 CLAUDE.md 裡反覆叮嚀請再次確認，不如做成可被選擇性呼叫的驗證 Skill，這與官方以 rubrics 搭配驗證代理的做法方向一致。

**法遵與資安規則不適用本次建議。** 資料邊界、存取範圍、稽核要求應維持顯式宣告，並盡量在上下文層（retrieval 時）強制執行，而非僅依賴提示詞。

**跨工具團隊優先維護 AGENTS.md。** 若團隊同時使用 Claude Code、Codex、Cursor，以 AGENTS.md 為單一事實來源，僅在需要工具專屬功能時才追加 CLAUDE.md。

## 為某一世代模型精心調校的提示詞，會在下一世代成為負債

這次事件真正的意義，並不在「80%」這個數字，而在於它揭示了 context engineering 的半衰期， **為某一世代模型精心調校的提示詞，會在下一世代成為負債。** Anthropic 願意公開承認自己手工調校的成果反而妨礙了新模型，這種誠實在產業裡並不常見，也讓這篇文章比一般的最佳實踐清單更有參考價值。

但對企業使用者而言，結論不該是「照著刪」。比較務實的讀法是，把提示詞當作程式碼來管理，有版本、有測試、有定期重構的排程。當模型每半年就換一代，任何靜態的規則文件都不可能永遠正確。

## 推薦延伸閱讀:

- Anthropic（Thariq Shihipar），〈The new rules of context engineering for Claude 5 generation models〉，2026-07-24： [https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models)
- Anthropic，〈Effective context engineering for AI agents〉： [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Linux Foundation，〈Linux Foundation Announces the Formation of the Agentic AI Foundation (AAIF)〉，2025-12： [https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)
- Bosio Digital，〈Context Engineering: What Anthropic’s 80% Cut Means for Business〉： [https://bosio.digital/articles/context-engineering-rules](https://bosio.digital/articles/context-engineering-rules)
- Morph，〈AGENTS.md Spec (2026)〉： [https://www.morphllm.com/agents-md-guide](https://www.morphllm.com/agents-md-guide)
- Addy Osmani，〈AGENTS.md – giving agents project context〉： [https://addyosmani.com/agents/15-agents-md/](https://addyosmani.com/agents/15-agents-md/)
- SwirlAI，〈State of Context Engineering in 2026〉： [https://www.newsletter.swirlai.com/p/state-of-context-engineering-in-2026](https://www.newsletter.swirlai.com/p/state-of-context-engineering-in-2026)
- Zenity，〈Context Engineering Is AI Security’s New Perimeter〉： [https://zenity.io/blog/events/context-engineering-security-engineering](https://zenity.io/blog/events/context-engineering-security-engineering)
- Augment Code，〈How to Build Your AGENTS.md (2026)〉： [https://www.augmentcode.com/guides/how-to-build-agents-md](https://www.augmentcode.com/guides/how-to-build-agents-md)
<iframe sandbox="allow-scripts" title="告別隨意寫程式！Matt Pocock 推出 grill-me 技能，讓 AI 成為最嚴苛的架構審查官 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/25/matt-pocock-grill-me-skill/embed/#?secret=oIYrkhpBXC#?secret=TdA4RSuLro" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="Claude Opus 5 登場：半價逼近尖端模型，基準測試與 API 新機制解析 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/25/claude-opus-5-debuts-13483/embed/#?secret=zE4wEKaOpK#?secret=hw6L4a1LFH" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="OpenAI 模型突破資安測試環境自主入侵 Hugging Face — CyberQ 賽博客" src="https://cyberq.tw/2026/07/23/openai-model-breaks-13445/embed/#?secret=fIUe6mVd7Z#?secret=1QL72y5dCa" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="效益與速度的平衡：Google 正式推出 Gemini 3.6 Flash 與實測剖析 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/22/the-ultimate-balance-13436/embed/#?secret=XyTqAoORlO#?secret=yG8CGFN3GZ" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="Claude Fable 5 訂閱新制正式上路：Max 內含 50% 額度，Pro 改點數制並獲 100 美元補貼 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/18/claude-fable-539s-new-subscription/embed/#?secret=xK25WECan7#?secret=w7JdiG6Rpu" width="500" height="335" frameborder="0"></iframe><iframe sandbox="allow-scripts" title="GitHub 趨勢周報 Vol.25：去中心化通訊與AI代理工具 — CyberQ 賽博客" src="https://cyberq.tw/2026/07/27/github-trends-weekly-report/embed/#?secret=oIbTKZTck9#?secret=4CVnjnRXAq" width="500" height="335" frameborder="0"></iframe>