---
title: "Claude Skills 教學：從零開始打造你的 AI 自動化工作流"
source: "https://yu-wenhao.com/zh-TW/blog/claude-skills-guide/"
author:
  - "[[WenHao Yu]]"
published: 2026-03-07
created: 2026-05-15
description: "這篇 Claude Skills 教學從實際案例出發，帶你理解 Skill 的運作原理、三層載入架構、觸發機制、設計模式，以及 Anthropic 官方的 Skill 寫作哲學。附實戰經驗和上手指南。"
tags:
  - "clippings"
status: processed
---
🌐

這篇文章也有英文版本 [Read in English →](https://yu-wenhao.com/en/blog/claude-skills-guide/)

每個用 AI 工作的人，幾乎都曾撞上同一面牆：你教會了它一件事，隔天開新對話，一切歸零。

你花了三天調好一套工作流程，跟 Claude 來回對話搞定了。結果隔天又要從頭解釋。你把 prompt 存在筆記裡每次貼，但一段 500 字的指令，每天貼一次，貼了一個月——你開始懷疑，這真的是 AI 該有的用法嗎？

不是。有一個東西專門解決這件事——Agent Skills。

我用 Claude Code，所以這篇以 Claude Skills 為例。但同樣的概念在 Cursor 叫 Rules、在 Codex 叫 Instructions——本質一樣，格式也正在統一。

我現在每天早上輸入兩個字「開工」，Claude 自動跑完 8 個步驟——回顧昨天、查週目標、同步四個專案、建好今天的工作清單——然後把每個任務標上顏色：綠色 AI 直接做、黃色 AI 備好選項我來挑、紅色我親自處理、灰色今天不用動。3 分鐘後問我：「今天先做哪個？」

因為我把這套流程寫成了一個 skill，所以兩個字，同樣的步驟，同樣的格式，每天都是。

這篇從頭講：Skill 是什麼、怎麼運作、怎麼寫、以及 Anthropic 自己怎麼想這件事。

---

## 為什麼需要 Skill

每次貼 prompt 的問題不只是煩——是浪費。

每貼一次，就佔掉 context window 的一塊空間。而 context window 是有限的。目前最強的 Claude Opus 也只有 200K tokens（大約 15 萬字），聽起來很多，但系統指令、MCP 工具描述、對話歷史全部共用這個空間。你掛的工具越多、對話越長，能塞進去的指令就越少。我自己實測過，MCP 工具掛多一點，對話還沒開始就已經吃掉一半的 context——剩下 50% 才是你真正能工作的空間。

還有一個更隱蔽的問題： **做法不一致。** 同樣一件事，今天你這樣解釋，明天換個說法，Claude 的做法就不一樣。有時候它會多做一步，有時候漏掉一步，有時候格式不對。沒有標準化的 SOP，品質靠運氣。

Skill 解決的就是這兩件事： **省 context、穩品質。** 在 Anthropic 的開放標準裡，這套機制叫 Agent Skills——不只 Claude Code 能用，Cursor、Gemini CLI、Codex 都支援同一套格式。但不管叫什麼名字，核心都一樣：讓 AI 寫程式、做任務時，按照你定義的 AI 工作流來執行，把重複性工作變成真正的 AI 自動化。

---

## Claude Skills 長什麼樣

技術上，一個 Claude Code Skill 就是一個資料夾。核心是一份叫 `SKILL.md` 的 Markdown 檔案——你的 SOP 寫在這裡。開頭是一段 YAML 設定（告訴 Claude 這個 skill 做什麼、什麼時候該用），後面是具體的步驟和指令。

一個簡化的例子：

```markdown
---
name: daily-planning
description: 每日工作規劃流程。當用戶說「規劃今天」「daily planning」
  「早安」「開工」時使用。
---

# 每日規劃

## 步驟

1. 確認今天日期和星期幾
2. 讀取最近兩份 daily note，盤點 carry-over
3. 讀取週目標，確認進度和剩餘天數
4. 掃描各專案的進行中任務
5. 建立今天的 daily note
6. 根據優先級建議今天的工作項目
```

`---` 之間的 YAML 就是封面——Claude 靠這段判斷要不要啟動這個 skill（後面會細講這件事有多重要）。下面的 Markdown 就是 SOP 本體。就這麼簡單。

而完整的 skill 資料夾長這樣：

```plaintext
my-daily-planning/
├── SKILL.md          ← 主要指令（必要）
├── scripts/          ← 可執行的腳本（選用）
├── references/       ← 參考文件（選用）
└── assets/           ← 模板（選用）
```

`SKILL.md` 是食譜本體——你的工作流程、步驟、判斷邏輯都寫在這裡。Claude 讀了就知道該怎麼做。

其他三個資料夾是配料：

- **`scripts/`** ：需要跑程式的部分。比如一支 shell script 用來解析 CSV 資料、算統計數字，或是一支 Python 腳本用來呼叫外部 API。這種事用程式跑比叫 Claude 自己算更準。而且 scripts 可以直接執行，不需要載入 context window——前面說的 context 稀缺問題，這裡又省了一塊。
- **`references/`** ：太長不適合塞在 SKILL.md 裡的參考資料。比如一份 API 文件、一份風格指南、或一份決策規則表。Claude 需要時才去翻，不會每次都佔用 context。
- **`assets/`** ：模板和素材。比如 daily note 的 Markdown 模板，每天建新檔案時直接套用，格式不會跑掉。

看出來了嗎？這四個東西各自解決的問題不同，但背後都在服務同樣兩件事： **省 context、穩品質。** Scripts 不佔 context 又算得準，references 按需載入不浪費空間，assets 讓輸出格式標準化。整個 skill 資料夾的設計，就是圍繞這兩個核心在轉。

寫一次。之後每次對話，Claude 自動套用。不用貼 prompt，不用重複解釋。

---

## MCP 是廚房，Skills 是食譜

如果你用過 [Claude Code](https://yu-wenhao.com/zh-TW/blog/claude-code-tutorial/) 的 MCP（Model Context Protocol），你已經給了 Claude 一整間廚房——各種工具、食材、設備。

但有廚房不代表會做菜。

| MCP（廚房） | Skills（食譜） |
| --- | --- |
| 連接工具 | 教怎麼用這些工具 |
| 提供即時資料存取 | 封裝工作流和最佳實踐 |
| 解決「能不能做」 | 解決「怎麼做最好」 |

我的 [Panopticon（內容監控系統）](https://yu-wenhao.com/zh-TW/blog/personal-panopticon/) 是一個好例子。MCP 讓 Claude 能連到 API 抓取 Reddit、Hacker News、Product Hunt 的熱門內容。但「抓到之後怎麼處理」——篩選互動分數 4 以上的、分成 5 種內容類型、推薦 2-3 個題材——這些判斷邏輯寫在 skill 裡。

MCP 提供能力。Skills 提供智慧。

而且這兩層都有了開放標準。MCP 統一了「工具怎麼連」，Agent Skills 統一了「工具怎麼用」。Agent Skills 是 Anthropic 在 2025 年底推出的開放標準，目前已被 30+ 工具採用——不只是 Claude Code，Cursor、VS Code Copilot、Gemini CLI、OpenAI Codex、JetBrains Junie 都支援。你寫的 skill 可以跨平台使用。

---

## 三層載入：Claude 怎麼知道該用哪個 Skill

我現在同時掛了 17 個 skills。如果每個都全部載入，Claude 會被資訊淹沒——想像一個廚師同時翻開 17 本食譜的所有頁面。

所以 skill 不是一次全部載入的。它的解法是一個三層架構，叫 **Progressive Disclosure** （漸進式揭露）：

**第一層：封面** （永遠載入，~100 tokens/skill）

就像食譜封面，只有兩樣東西：

- `name` ——這個 skill 叫什麼
- `description` ——做什麼、什麼時候該用（「番茄義大利麵，適合週末午餐」）

Claude 讀這個就知道什麼時候該啟動。

**第二層：完整指令** （覺得相關才載入，建議 < 500 行）

封面和完整指令都在同一份 SKILL.md 裡。差別是：封面永遠載入，但 Claude 只有判斷「這個任務需要這份食譜」時，才會翻開來讀後面的完整內容。

**第三層：附加文件** （按需查閱，無限制）

`references/` 、 `scripts/` 、 `assets/` 裡的東西。Claude 需要時才去翻。Scripts 可以直接執行，不需要載入 context。

這個設計讓 17 個 skills 可以和平共存。我說「早安」，只有 `/daily` 被完整載入，其他 16 個安靜待命。說「寫 FB 貼文」， `/content` 被打開。說「SEO 研究」， `/seo` 被打開。每個 skill 只在被需要的時候才佔用 context window。

---

## Description 決定生死

三層架構裡，第一層封面的 `description` 是最關鍵的——它決定你的 skill 會不會被觸發。

Claude 靠這段文字判斷：「使用者現在說的話，跟這個 skill 有關嗎？」判斷方式是 LLM 自己的語義理解——它不是在做關鍵字比對，而是真的在「讀懂」你的意圖和 description 的描述，然後決定要不要啟動。

聽起來很聰明，但有一個問題：Claude 傾向保守。它寧可不觸發，也不亂觸發。有人做過實測，description 寫得模糊的時候，自動觸發準確率只有 55%。不是 Claude 讀不懂你的意圖，而是 description 寫得不夠明確時，它會選擇「不確定就不啟動」。

怎麼寫才對？一個好的 description 要回答兩件事：

1. **這個 skill 做什麼** ——讓 Claude 知道它的用途
2. **使用者會怎麼說** ——具體的觸發詞，中英文都列

寫得好的 description：

```yaml
description: 每日工作規劃流程。當用戶說「規劃今天」「daily planning」
  「今日規劃」「開工」時使用。
```

做什麼：每日工作規劃。怎麼觸發：四組中英文關鍵字。Claude 讀到「開工」就知道該啟動。

寫得爛的 description：

```yaml
description: 幫助處理日常工作。
```

只有一個模糊的用途，沒有觸發詞。Claude 不知道使用者會說什麼話來啟動它，結果就是永遠不觸發。

幾個要注意的：

- **雙語觸發** ：如果你像我一樣中英文混著用，description 裡要同時寫中英文觸發詞。「規劃今天」和「daily planning」都要能觸發同一個 skill。
- **寧可多寫不要少寫** ：因為 Claude 傾向保守不觸發，Anthropic 自己建議 description 寫得越明確越好——觸發詞多列幾組，比少列安全。

Description 的角色其實跟產品的 landing page 一樣。你怎麼描述它，決定了別人（在這裡是 Claude）會不會「點進來」。

---

## Skill 能做到什麼

Skill 不只是「把步驟寫下來」。Anthropic 從早期使用者中歸納了 5 種設計模式：

| 模式 | 做什麼 | 例子 |
| --- | --- | --- |
| 按步驟執行 | 有順序的流程，步驟間有依賴 | 每日規劃：回顧昨天 → 查週目標 → 建 daily note |
| 跨工具協作 | 串接多個工具，資料在系統間流動 | 專案初始化：建 GitHub repo → 生成資料夾 → 設定 CI/CD → 通知 Slack |
| 迭代校稿 | 做一版、檢查、改、再檢查，直到達標 | 內容校稿：初稿 → 簡繁轉換 → 品質檢查 → 不過關就回去改 |
| 條件分支 | 同一個目標，根據情況走不同路徑 | SEO 研究：根據輸入決定查關鍵字、分析競爭對手、還是畫產業版圖 |
| 嵌入專業知識 | 把領域知識寫進去，不只是工具操作 | Code review 標準：命名規則、錯誤處理原則、測試覆蓋率，寫一次就好 |

大部分好用的 skill 混用多種模式。你不用刻意套，但知道有哪些可能性，寫的時候會更有結構。

---

## 怎麼寫得好

Anthropic 的 `skill-creator` （官方用來寫 skill 的 meta-skill）裡有一段話，我覺得點出了寫 skill 最重要的一件事：

> Try to explain to the model why things are important in lieu of heavy-handed musty MUSTs. （與其用一堆死板的 MUST 來壓模型，不如跟它解釋為什麼這件事重要。）

用原因代替命令。

如果你發現自己在 SKILL.md 裡寫了很多「ALWAYS」和「NEVER」（全大寫），那是一個訊號——試著改成解釋為什麼。

舉個例子。你在 skill 裡可以這樣寫：

```plaintext
ALWAYS 在執行命令前顯示命令內容。NEVER 直接執行。
```

也可以這樣寫：

```plaintext
執行命令前先顯示內容，因為使用者需要確認安全性。
未經確認就執行可能造成不可逆的損害。
```

第一種寫法，Claude 只會照做這兩條規則。遇到規則沒覆蓋的情境（比如一個看起來安全但其實有風險的命令），它不知道該怎麼判斷。

第二種寫法，Claude 理解了「為什麼」——保護使用者安全。所以即使遇到規則沒寫到的灰色地帶，它也會傾向謹慎處理。原因讓模型舉一反三，規則只能覆蓋你想到的情境。

唯一的例外是格式規範——「輸出一定要用這個模板」這種機械性要求，沒有「為什麼」可以解釋，直接寫死就對了。

---

## Skill 不會一次到位

### 一個 Skill 要改很多次

我的 `/daily` 現在是 v6。第一版很爛——步驟不清楚、路徑寫錯、有些情況沒處理到。

第二版加了內容發現系統整合。第三版發現週進度計算常出錯，加了明確的計算規則。第四版加了自動觸發——週二提醒跑週會、月初提醒歸檔。第五版加了 iPhone 輕量模式（偵測環境，手機上跳過需要 Python 的步驟）。

每一版都是在使用中發現「這裡不夠好」，然後改。

Skill 不是寫完就丟著的設定檔——它是你工作流的活文件。你的工作方式會變，需求會變，skill 跟著變。每改一次，你對自己的工作流就多理解一層。這個過程本身就在逼你思考：「我到底是怎麼工作的？哪些步驟真的重要？」

Anthropic 的指南裡也說了同樣的話：

> 先在一個具體任務上反覆嘗試，直到 Claude 做對。然後再把成功的方法抽取成 skill。

不要追求一步到位。今天做一個 v1，用幾天發現問題改成 v2。好的工作流是長出來的，不是設計出來的。

### 17 個 Skill 也不是一次建的

我是從一個 `/daily` 開始的。用了一陣子之後，發現每週一人週會的流程也在重複，做成了 `/wam` 。後來關鍵字研究每次都要重新解釋策略，做成了 `/seo` 。再後來 YouTube 影片分析的 API 串接也固定了，做成了 `/youtube` 。

每一個 skill 都是因為「這件事我重複做了太多次」才誕生的。不是坐下來規劃「我需要哪些 skill」，而是日常工作中自然長出來的。

半年後我回頭看，已經有 17 個了——不是因為我一開始就計畫好，而是因為每天都在用、每次遇到重複的事就多封裝一個。

舉幾個我日常在用的 skill，讓你有個感覺：

| Skill | 做什麼 | 為什麼值得做成 Skill |
| --- | --- | --- |
| `/daily` | 每日工作規劃（8 步驟） | 每天都要跑，步驟多且有依賴關係 |
| `/seo` | SEO 關鍵字研究（8 個子模式） | 查詢流程固定，但每次參數不同 |
| `/content` | 內容創作 + 風格校稿 | 寫作風格要一致，靠記憶不可靠 |
| `/wam` | 每週一人週會 | 固定流程：算分 → 回顧 → 規劃下週 |
| `/youtube` | YouTube 影片轉譯分析 | API 串接 + 後處理邏輯不想每次重寫 |

不用一開始就做很多。從一個你每天都在做的重複性工作開始。

---

## 怎麼做你的第一個 Skill

你不需要自己手寫 SKILL.md。不管是讓 AI 寫程式還是寫 Skill，Claude 都能幫你搞定。實際上我自己的 skills 沒有一個是手寫的。

有兩條路：

**路線 A：讓 Claude 幫你寫。**

最自然的方式。你先在對話中反覆嘗試某個工作流，直到 Claude 做對。然後跟它說：「把剛才的流程做成一個 skill。」它會自動生成 SKILL.md、建好資料夾結構。你試用幾天，發現不對的地方再叫它改。

Anthropic 官方甚至有一個專門做這件事的 skill，叫 `skill-creator` （2026 年 3 月初剛做了重大升級）。你在 Claude Code 裡啟動它，Claude 會像訪談一樣問你問題——你的工作流長什麼樣、什麼時候該觸發、有哪些邊界條件——然後根據你的回答自動產出 SKILL.md。

不只是產出。它還能幫你測試和優化：

- **Eval** ：自動生成測試案例（「使用者說 X，skill 應該做 Y」），驗證 skill 能不能正確觸發
- **Improve** ：根據測試結果自動優化 description 和指令，用 60/40 train/test split 防止 overfitting
- **Benchmark** ：追蹤成功率、token 用量，甚至能跑 A/B test——兩個版本盲測對決

**路線 B：從社群找起點再客製。**

現成的 skills 到處都有。最大的目錄是 [SkillsMP](https://skillsmp.com/) ，收錄了 96,000+ 個從 GitHub 聚合的 skills。 [Anthropic 官方 repo](https://github.com/anthropics/skills) 數量少但品質有保證。社群也有 [Awesome Agent Skills](https://github.com/VoltAgent/awesome-agent-skills) 策展清單，收錄了 Anthropic、Vercel、Stripe、Cloudflare 等官方團隊發布的 skills。

但要注意：社群 skill 是通用的起點，不是你的終點。一個通用的「每日規劃」skill 不知道你用什麼專案管理工具、你的週目標格式長什麼樣、你習慣怎麼排優先順序。真正好用的 skill 一定是根據你自己的工作流客製過的——這也是為什麼我的 `/daily` 改了六版才到現在的樣子。

不管哪條路，關鍵都一樣： **先做對一件事，再封裝。** 不要一開始就想設計一個完美的 skill。v1 能用就好，用了幾天發現漏洞再改。我的 `/daily` 改了五次才變成現在的樣子。

---

## 這意味著什麼

每次開新 session，Claude 都是一個什麼都不知道的新人。Skill 就是你給它的 onboarding 手冊——讀完就知道你的流程、你的標準、你的偏好。不用每次重新帶，開口就能上手。

`/daily` 是行政助理， `/wam` 是週會主持人， `/content` 是寫作教練。每一個都是最懂你的資深員工——知道你的偏好、熟悉你的流程、記得你上次踩過的坑——不是第一天報到的實習生。

Agent Skills 本身不難——就是 Markdown 加上一些結構。但它代表的趨勢很重要： **AI 從「你每次都要教它」變成「你教一次就好」。**

而且這個標準是開放的。你今天在 Claude Code 寫的 skill，明天可以搬到 Cursor、Gemini CLI、Codex 上用。你投入的時間不會被綁定在一個平台上。

對我來說，Agent Skills 是 [Agentic Coding](https://yu-wenhao.com/zh-TW/blog/agentic-coding-guide/) 從「能用」變成「好用」的那一步。工具接好了（MCP），知識封裝好了（Skills），剩下的就是讓 AI 按照你的方式做事——不管你用它來寫程式、做研究、還是把日常管理變成 AI 自動化流程。

> **想在一天內把這些串起來？** 5/16（六）我和 Vista 在台北開一場 6 小時實作工作坊，帶你用 Claude Code + Obsidian 建好目標追蹤、知識庫、自動化腳本——不用自己摸三個月。 [看工作坊詳情 →](https://www.solo.tw/courses/ai-command-center)

---

### 延伸閱讀

這套 AI 工作流方法要導入企業環境？ [這篇記錄了我在現場的觀察](https://yu-wenhao.com/zh-TW/blog/enterprise-ai-adoption-bottleneck/) ——最大的瓶頸往往不在工具，在更前面。

*如果這篇讓你有了想法， [訂閱每週一封信](https://yu-wenhao.com/zh-TW/) ——我固定寫 AI 工作流、和一路上想通的事。*

*想聊聊怎麼把 AI 融入你的工作流？ [看看我的服務](https://yu-wenhao.com/zh-TW/services/) 。*

#AI #Claude Code #agentic coding #Claude Skills #developer-tools

## 常見問題

Claude Skills 是什麼？

解決「每次開新對話都要重新教 AI」的問題。一個 skill 是一個資料夾：SKILL.md（主要指令，YAML 封面 + Markdown 步驟）、scripts/（可執行腳本）、references/（參考文件）、assets/（模板）。寫一次，之後每次對話自動套用。

Skills 和 MCP 有什麼不同？

MCP 讓 Claude 連到外部工具和 API（解決「能不能做」），Skills 教它拿到資料後怎麼處理（解決「怎麼做最好」）。以我的 Panopticon 為例：MCP 負責從 Reddit、Hacker News 抓內容，Skill 負責篩選互動分數 4 以上的、分成 5 種內容類型、推薦 2-3 個題材。

不會寫程式也能用 Skills 嗎？

可以。SKILL.md 就是 Markdown，不需要寫程式碼。你甚至不用自己手寫——Anthropic 官方有一個叫 skill-creator 的 skill，啟動後 Claude 會像訪談一樣問你問題（工作流長什麼樣、什麼時候觸發、有哪些邊界條件），然後自動產出 SKILL.md 和資料夾結構。

Skills 只能在 Claude Code 上用嗎？

不是。Agent Skills 是 Anthropic 2025 年底推出的開放標準，目前已被 Cursor、VS Code Copilot、Gemini CLI、OpenAI Codex、JetBrains Junie 等 30+ 工具採用。你寫的 skill 可以跨平台使用，不會被綁定在單一工具上。

一個 Skill 能寫多長？

SKILL.md 建議控制在 500 行以內。超過的話用三層架構處理：封面（name + description，~100 tokens）永遠載入，完整指令被觸發時才載入，太長的參考資料拆到 references/ 資料夾按需查閱。這樣 17 個 skills 同時掛著也不會塞爆 context window。