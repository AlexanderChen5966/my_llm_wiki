---
title: "AI coding agent 省 token 實戰：固定成本、prompt caching 回本門檻與 context rot - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10402744"
author:
  - "[[iThome]]"
published:
created: 2026-08-20
description: "資訊基準日：2026-08-13。文中的定價倍率、模型世代與功能行為，都以這一天抓取的官方文件為準。Claude Code 的行為隨版本變動，對照時先跑 claude --version。 如果你每天..."
tags:
  - "clippings"
status: "processed"
---
3

## AI coding agent 省 token 實戰：固定成本、prompt caching 回本門檻與 context rot

- 分享至

> 資訊基準日：2026-08-13。文中的定價倍率、模型世代與功能行為，都以這一天抓取的官方文件為準。Claude Code 的行為隨版本變動，對照時先跑 `claude --version` 。

如果你每天用 coding agent 寫程式，卻說不出自己的 token 花在哪裡，這篇給你一組能自己驗算的判準。內容以 Claude Code 的官方文件為主，因為它把成本機制寫得最完整；用其他工具的人，機制觀念通用，數字請回自家定價頁對照。

先給一個量級感。Anthropic 官方文件寫的企業部署平均是每位開發者每個活躍日約 13 美元，其中 90% 的使用者低於每活躍日 30 美元（ [Manage costs effectively](https://code.claude.com/docs/en/costs) ）。你的數字如果離這個範圍很遠，先量，再省。

量的方法不必自己寫腳本。 `/usage` 會把用量依 skills、subagents、plugins 和個別 MCP server 拆開，並在單一行為佔到 10% 以上時直接標記，例如 long context 或 cache misses。

## 你的 token 分成兩種，策略完全不同

把 agent 的 token 消耗拆成兩類，後面所有判斷都會變簡單。

**固定成本** 是對話還沒開始就付掉的部分。工具定義是最大一塊。Anthropic 在 [Advanced tool use](https://www.anthropic.com/engineering/advanced-tool-use) （2025-11-24）舉了一個五個 MCP server 的實例：GitHub 的 35 個工具約 26K tokens、Slack 11 個約 21K、Sentry 5 個約 3K、Grafana 5 個約 3K、Splunk 2 個約 2K。官方的結論句是「That's 58 tools consuming approximately 55K tokens before the conversation even starts.」再掛一個 Jira 就再加約 17K。同一篇提到 Anthropic 自己見過工具定義吃掉 134K tokens 的案例。

這些是 **改善前的量測** 。同一類解法後來成了 Claude Code 的預設行為，所以 55K 與 134K 是「當年為什麼非改不可」的證據，不是你今天的現況。

CLAUDE.md 也算固定成本。它在 session 開始就被載入，所以裡面寫的每一條規則，在你做完全無關的工作時同樣佔著位置。

**變動成本** 隨對話長大。官方文件把機制講得很直白：Claude Code 每次請求都送出完整對話，而每次 Claude 用工具，又會再送一次帶著那批工具結果的請求。結果是一句話的簡短提問，在開了一整天的 session 裡照樣拉動整段歷史的用量。

這個量級有人實測過。連續 20 年 Microsoft MVP、黑暗執行緒站主 Jeffrey 在〈 [AI Agent 上個網看個圖，要花多少錢？](https://blog.darkthread.net/blog/chat-token-usage/) 〉（2026-03-15）量到：對 agent 說一句「Hi」耗掉 252 tokens；把整篇文章重送一次超過 6K tokens、約新台幣 0.7 元。Django 共同創作者 Simon Willison 在〈 [How coding agents work](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/) 〉（2026-03-16）補上機制面：對話越長，每一輪的 input token 越貴，caching 只把這條曲線壓低，沒有拉平。

![https://ithelp.ithome.com.tw/upload/images/20260813/20178110KGwiEC9eHu.png](https://ithelp.ithome.com.tw/upload/images/20260813/20178110KGwiEC9eHu.png)

*深藍色是固定成本，對話開始前就載入，之後每一輪都重送一次；淺藍色是變動成本，隨對話累加。柱高為示意比例，圖中唯一的絕對數字 55K tokens 出自官方文件。*

兩側的處理方式不同。固定成本先動，因為浪費最明確，用 `/context` 看組成；變動成本需要策略，壓過頭會變笨，用 `/usage` 的 long context 標記追蹤。

### 閒置的 token 和做事用掉的 token

前面按「什麼時候產生」分類，這裡再按「有沒有在做事」分一次，因為後面的數字要靠它才讀得對。

工具定義佔的位置是 **閒置成本** ：你這次任務用不到的工具，留在 context 裡既花錢又製造干擾。搜尋、讀檔、推理消耗的是 **工作成本** ：那是 agent 真的在做事。

兩套分類是交叉的，不是嵌套。固定成本裡既有閒置的部分，也有必要的部分；變動成本大多是工作成本，但硬塞進來的原始資料同樣純屬閒置。

![https://ithelp.ithome.com.tw/upload/images/20260813/20178110ISja2qdoOa.png](https://ithelp.ithome.com.tw/upload/images/20260813/20178110ISja2qdoOa.png)

*判斷該加還是該減，看的是欄不是列：左欄要壓到接近零，右欄在對的任務上該放大。*

兩者的最佳解方向相反。閒置成本要壓到接近零，工作成本在對的任務上值得放大。省 token 的目標不是把總數壓低，而是把 context 花在對的地方。

### 你什麼都沒改，也會變貴

還有一種成本跟你的寫法無關。Anthropic 的 [Models overview](https://platform.claude.com/docs/en/about-claude/models/overview) 在 context window（模型單次能看到的內容總量）欄位的註記寫著：「Claude Fable 5 uses the tokenizer introduced with Claude Opus 4.7; compared to models before Claude Opus 4.7, the same text produces roughly 30% more tokens. The exact increase depends on the content.」

跨過 Claude Opus 4.7 這條分界線，同一份程式碼與同一段對話會被算出多約三成的 token。同一個 1M 的 context window，能裝的字數也從 Opus 4.6 的約 750k words 掉到 Opus 5 與 Sonnet 5 的約 555k words。你的 prompt 沒改、專案沒變大，帳單和 context 壓力卻同時上升。

## 砍固定成本：省下來的同時準確率會上升

這一節的重點是一個反直覺的官方數據。

Anthropic 的 Tool Search Tool 不再把全部工具定義預先載入，改成讓 Claude 隨需發現、只看到當前任務真的需要的工具。效果官方寫「This represents an 85% reduction in token usage while maintaining access to your full tool library.」

同一篇另一組數字更值得記：開啟之後「Opus 4 improved from 49% to 74%」、「Opus 4.5 improved from 79.5% to 88.1%」，可用的 context 也從 122,800 tokens 提升到 191,300 tokens。不把整份工具清單攤在模型面前，它反而更常選對工具。

這組準確率有兩個範圍限制要一起讀：它量的是大型工具庫下「選對工具」的能力，不是一般程式任務的正確率；受測的 Opus 4 與 Opus 4.5 也都已列入 Legacy（見文末）。

這些數字全部出自 Anthropic 自己的量測，沒有第三方復現，所以請當成官方自評：方向可信，倍率不必照抄。塞滿的工具清單同時是成本問題和干擾問題，砍它沒有取捨。

實務上有幾個現成的槓桿，都寫在官方的 [成本文件](https://code.claude.com/docs/en/costs) 裡：

- **`/context` 是唯一可信的基準線** ，它會列出工具定義、CLAUDE.md、MCP 各佔多少。官方明說 MCP 工具定義在 2026-08-13 這天的文件版本已是預設 deferred，只有工具名會進 context，等 Claude 真的要用某個工具才載入定義。你如果還在憑 2026 年以前的文章判斷 MCP 開銷，那個前提已經變了； `/context` 顯示的結果與文章不符時，先跑 `claude --version` 對照。
- **CLI 工具的固定成本比 MCP 低** 。官方給的理由是 `gh` 、 `aws` 、 `gcloud` 、 `sentry-cli` 這類工具完全不佔 per-tool listing。
- **閒置的 MCP server 有價** 。Flask 與 Jinja2 作者、Sentry 共同創辦人 Armin Ronacher 在〈 [Skills vs Dynamic MCP Loadouts](https://lucumr.pocoo.org/2025/12/13/skills-vs-mcp/) 〉（2025-12-13）估過 MCP 掛進 context 約要 8k tokens；那是他自述的估計值，也同樣早於 deferred 成為預設。
- **CLAUDE.md 與 Skills 的載入時機不同** 。CLAUDE.md 在 session 開始就載入，SKILL.md 的本文只有在被呼叫時才進 context，所以只有特定工作流才用到的規範放在 Skills 比較划算。官方建議 CLAUDE.md 控制在 200 行以內。

搬家有實測支撐。dailydoseofds.com 的作者 Avi Chawla 在〈 [How we cut our Claude Code token usage](https://blog.dailydoseofds.com/p/how-we-cut-our-claude-code-token) 〉（2026-04-20）記錄的降幅是從 1,040 萬 tokens 降到 370 萬，主要手段是 Skills 的漸進載入加上結構化的 CLI。

（需作者補充：你自己的 CLAUDE.md 精簡前後、以及停用不必要的 MCP server 前後， `/context` 的實際數字。這一段放你的真實數字會比官方數據更有說服力。）

## prompt caching：唯一需要你自己算一次的地方

prompt caching（提示詞快取）把重複的請求前綴存起來重用。它是省 token 裡唯一有「算錯方向反而更貴」風險的機制，所以值得動筆算一次。

[Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 文件給了三個倍率：5 分鐘 TTL 的快取寫入是基礎價的 1.25 倍、1 小時 TTL 的寫入是 2 倍、快取讀取是 0.1 倍。有了這三個數字，回本門檻就不必問別人。

假設同一段前綴要送 N 次，且 **這 N 次都落在同一個快取生命週期內** ，用基礎價當單位：

- 完全不用快取：成本 `N`
- 用 5 分鐘快取：第一次寫入付 1.25，之後每次讀取付 0.1，總計 `1.25 + 0.1 × (N − 1)`
- 用 1 小時快取： `2.0 + 0.1 × (N − 1)`

解 `1.25 + 0.1(N − 1) < N` 得 `N > 1.28` ，解 `2.0 + 0.1(N − 1) < N` 得 `N > 2.11` 。所以 5 分鐘快取在第二次呼叫（第一次命中）就回本，1 小時快取需要兩次命中。

**那個前提是整段推導的關鍵。** 如果兩次呼叫的間隔超過 TTL，快取已經過期，每一次都是重新寫入，成本變成 `1.25N` 或 `2.0N` ，比完全不用快取更貴。真正的決策變數是「你在一個 TTL 窗口內會命中幾次」，不是「你總共呼叫幾次」。偶爾用一下的低頻流量，開長 TTL 快取只會加價。

![https://ithelp.ithome.com.tw/upload/images/20260813/20178110EiXU8sHQi1.png](https://ithelp.ithome.com.tw/upload/images/20260813/20178110EiXU8sHQi1.png)

*四條線都由官方那三個倍率算出。空心圓是快取線與「完全不用快取」灰線的交點，也就是回本點；紅色虛線是每次呼叫都超過 TTL 的情況，它跑在灰線上方。*

這個推導只依賴三個官方倍率，你可以自己驗算。網路上流傳的門檻數字經常更保守，若沒附推導或量測條件，當參考就好。

### 快取要不要自己開，看你的身分

這件事最容易寫錯，因為各家預設不同。

| 你的身分 | 要不要自己開 | 生命週期 |
| --- | --- | --- |
| Anthropic API 自建應用 | 要自己加 `cache_control` | 預設 5 分鐘 |
| Claude Code | 自動套用 | 訂閱制 1 小時；動用 usage credits 或改用 API key、cloud provider 則為 5 分鐘（設 `ENABLE_PROMPT_CACHING_1H=1` 可保住 1 小時） |
| OpenAI API | 自動，輸入達 1024 tokens 以上生效 | 見 [官方頁](https://developers.openai.com/api/docs/guides/prompt-caching) |

查證時看到中文文章寫「Pro 是 5 分鐘、Max 是 1 小時」，與現行官方文件不符。這類細節改動頻繁，引用二手文章前值得回官方頁確認。

### 讓快取失效的兩類原因

命中的條件是前綴從第一個 token 起完全一致，所以失效方式比想像中多。

**前綴一致性被破壞** ，常見來源有三個：

- system prompt 裡放了會變的值，例如目前時間、隨機 UUID、request ID
- 工具註冊順序變動，定義內容沒改也算前綴改變
- 切換模型，因為各模型的快取彼此獨立，換過去等於作廢舊快取，還要付新模型的寫入費

**機制本身的限制** ，也有三個：

- 快取查找有 20 個 block 的 lookback 上限，單輪 agentic loop 的工具呼叫一多，就可能在你沒察覺的情況下失效
- 平行 fan-out 時，多個相同前綴的請求彼此吃不到對方的快取
- 超過 TTL 就過期，下一次呼叫要重新寫入，付的是 1.25 倍或 2 倍的寫入價，比不用快取還貴

切換模型這一項特別值得放在心上，因為「試試看換個模型」是很自然的動作，代價卻不只是那一次呼叫。

（需作者補充：你踩過哪一種快取失效？哪個動作炸掉了前綴、你是怎麼發現的？）

## context 變長不只變貴，還會變笨

到這裡都在談錢。變動成本還有一個非金錢的代價，而它是很多人做壓縮（compaction）的真正理由。

`context rot` 指的是 context 變長之後，模型表現隨之下滑的現象，而且不需要撞到 context window 上限就會發生。中文技術圈目前幾乎沒有這個詞的討論，所以這裡直接用英文原詞。

最紮實的證據來自 Chroma 研究團隊的〈 [Context Rot](https://www.trychroma.com/research/context-rot) 〉（2025-07-14）。他們用 18 個模型（涵蓋 Claude、GPT、Gemini、Qwen）跑 needle-in-a-haystack、LongMemEval（306 題、約 113k tokens）以及重複詞任務，量到的是表現隨長度非線性下滑，不是平緩衰減。更早的〈 [Lost in the Middle](https://arxiv.org/abs/2307.03172) 〉（2023-07）記錄了另一面：長 context 中段的資訊最容易被忽略。Anthropic 在〈 [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) 〉（2025-09-29）也採同樣立場。

實務上有兩個成本不對稱的動作，都寫在官方成本文件裡：

- `/clear` 不花錢。切換到不相關的任務時，開新的比留著舊的划算。
- `/compact` 本身是一個大請求，因為它要讀完它要摘要的那段對話。壓縮不是免費的。

## 什麼時候該反過來多燒 token

省過頭也是一種錯，而最清楚的反例來自官方。

Anthropic 在〈 [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) 〉（2025-06-13）寫：「agents typically use about 4× more tokens than chat interactions, and multi-agent systems use about 15× more tokens than chats.」他們沒有因此放棄 multi-agent，因為同一篇的結果是以 Opus 4 當主導 agent、Sonnet 4 當 subagent 的架構，在他們的內部研究評測上勝過單一 agent 的 Opus 4 達 90.2%。

同一篇還有一句值得單獨記下：「We found that token usage by itself explains 80% of the variance」，三個因素合計解釋 BrowseComp 評測 95% 的表現變異。

這句話和前面「砍掉 85% 反而更準」並不衝突，兩者講的是不同那一半。這裡增加的是工作成本：agent 多搜尋、多讀、多驗證。前面砍掉的是閒置成本：從來沒被用到的工具定義。同樣是 token，一邊該加，一邊該減。

這些都是官方內部評測，方法學未完全公開，也沒有第三方復現。Claude Code 的 agent teams 另有一個官方自評數字：teammate 跑 plan mode 時約為一般 session 的 7 倍 token。

方向相反的意見同樣來自一手作者。Cognition 的 Walden Yan 在〈 [Don't Build Multi-Agents](https://cognition.com/blog/dont-build-multi-agents) 〉（2025-06-12）主張不要把 context 拆給平行 agent。Anthropic 的 Claude Code 負責人 Boris Cherny 在一場約 2026-05-20 的公開訪談裡，主張先把事情做對、別過度優化省 token（逐字稿為第三方轉錄，非官方版本）。

要落到可操作，這裡給三個問題：

1. 這個任務的產出值不值 15 倍 token？研究型、探索型、跑錯了就重來的任務通常值得；例行小改動不值得。
2. 子任務彼此獨立嗎？輸入輸出單純、沒有強上下文依賴、可平行分發的任務才適合拆出去。
3. 拆出去之後回到主線的是精煉結果還是原始資料？如果是原始資料，你只是把污染延後發生。

## 省法自己會出事

每個省 token 的手段都有它的失敗模式，這一節是為了讓你少繳學費。

**壓縮會弄掉硬性約束。** `anthropics/claude-code` 的 issue #9796 記錄了長對話壓縮後摘要只留敘事、丟掉強制約束，導致 agent 違反指令。這個 issue 在 2026-08-13 查證時仍然開放、沒有官方回應。規避方式是壓縮前自己寫一份 checkpoint，把不可違反的約束再講一次，而不要信任摘要會保留它們。

**自動化的迴圈會失控。** 同一個 issue tracker 上有兩個量級驚人的案例：#4095 報告 5 小時內燒掉 16.7 億 tokens、估計一萬六到五萬美元，根因有爭議、修復狀態未確認；#9579 報告單日 1.08 億 tokens，Anthropic 工程師回覆已修（約 2025-10-18）。教訓是任何會自己跑的迴圈都需要硬性上限與終止條件。

**用小模型省下的錢，可能被重試吃回去。** 工程師 Tian Pan 在〈 [Retry budget and LLM agent cost amplification](https://tianpan.co/blog/2026-04-16-retry-budget-llm-agent-cost-amplification) 〉（2026-04-16）算過鏈式 agent 的重試會重送整段歷史：3 個步驟多花 1.7 到 1.9 倍、5 個步驟多花 2.2 到 2.5 倍，隨步驟數非線性上升。這是具名工程師的個人量測，非官方數據。Armin Ronacher 在〈 [Agent Design Is Still Hard](https://lucumr.pocoo.org/2025/11/21/agents-are-hard/) 〉（2025-11-21）從另一個角度得到同樣結論：便宜模型在迴圈裡不一定更省，因為它會多繞幾輪。

**別把工具自報的壓縮率當成帳單降幅。** 查證中文社群文章時遇到一個實例：某篇引用的降幅其實是工具自己回報的「終端輸出壓縮率」，而那個工具的官方 README 自己澄清過，輸出壓縮率不等於帳單節省率。看到漂亮的百分比，先問它量的是輸出長度還是計費 token。

（需作者補充：你踩過的坑。compaction 掉了約束害 agent 重做已完成的工作？MCP 塞爆 context？用量在什麼情況下突然噴掉？）

## 〔How-to〕15 分鐘自我盤點

以下是全文唯一的步驟區塊，原理在前面各節，這裡只給動作。

1. 跑 `/context` ，記下工具定義、CLAUDE.md、MCP 各佔多少，當成你的固定成本基準線。  
	![https://ithelp.ithome.com.tw/upload/images/20260813/201781106DmXc3vUs0.png](https://ithelp.ithome.com.tw/upload/images/20260813/201781106DmXc3vUs0.png)
2. 跑 `/usage` ，按 `d` 或 `w` 切換 24 小時與 7 天。看 attribution 與 behavior flags 兩塊。
3. 用 `/mcp` 關掉這週沒用到的 server。
4. 把 CLAUDE.md 的專用流程搬進 Skills，目標 200 行以內。
5. 簡單任務用 `/effort` 降一級。 `effort` 在 Claude Opus 5 與 Claude Sonnet 5 上，於 Claude API 與 Claude Code 預設就是 `high` ，而 thinking token 依 output 費率計價。
6. 用 `PreToolUse` hook 把冗長輸出擋在 context 外，官方範例是先 grep 測試輸出再交給 Claude；同樣邏輯適用於把跑測試、抓文件、處理 log 交給 subagent。
7. 需要事前估算時，用 [Token counting API](https://platform.claude.com/docs/en/build-with-claude/token-counting) 在送出前先算。

## 哪些內容會過期

這個主題的半衰期很短，以下是最容易失效的部分。

- **tokenizer 世代分界在 Claude Opus 4.7** 。跨越這條線的 token 數比較不能直接沿用。
- **模型的 Legacy 狀態變動快** 。在 2026-08-13 這天，Opus 4.8、4.7、4.6、Sonnet 4.6、4.5 與 Opus 4.5 都已列入 Legacy，現役是 Fable 5、Opus 5、Sonnet 5 與 Haiku 4.5。任何以 Opus 4.6 當主角的成本結論，都已經是舊版討論。
- **快取折扣的數字會被改** 。Gemini 的 context caching 折扣在 2025-05 的首發 [公告](https://developers.googleblog.com/gemini-2-5-models-now-support-implicit-caching/) 寫 75%，現行 [定價頁](https://ai.google.dev/gemini-api/docs/pricing) 是 90%。
- **「省 90%」不是全系列適用** 。OpenAI 的快取讀取折扣逐模型不同，GPT-5 系列是 90%， `o3` 是 75%， `o1` 只有 50%。
- **beta 功能的行為會變** 。伺服器端的 context editing 與 compaction 目前都是 beta，官方在 100-turn eval 上自評 context editing 省 84% token，同樣是官方自評、沒有第三方復現。

所以看到任何省 token 的數字，先問三個問題：

1. **量的是哪個世代？** 跨過 tokenizer 分界的數字不能直接比。
2. **量的是輸出長度還是計費 token？** 兩者差距可以很大。
3. **有沒有交代量測條件？** 沒有方法學的百分比，只是一個好記的數字。

這三個問題比任何一份技巧清單活得久。工具與倍率都會被改掉，「這個數字怎麼來的」永遠問得下去。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![](https://itadstatic.ithome.com.tw/B9/1787130677_6a8573357d6ff.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2608B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/274460e7-47bc-4cf4-9e5c-4f52e469c796_small.jpg)

[How your API be my API](https://itplus.ithome.com.tw/vod-page/166?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

33 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/11/9892e6e2-7b5e-4c0a-bd33-b94bb32bd372_small.jpg)

[OPC-UA 汽車製造的數位轉型新曙光？攻擊者正潛伏在下一波新攻擊;)](https://itplus.ithome.com.tw/vod-page/518?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

43 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/10/5a8abfe5-156d-4f08-a876-62c23d668038_small.png)

[Flowmon - 結合MITRE 與 Netflow 達成CP值最高的資安事件應變手段](https://itplus.ithome.com.tw/vod-page/489?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/5eae41d8-fde6-4290-a0cc-0f6039e7bb7a_small.png)

[從品牌重造到供應鏈整合的 AI 演化：Cympack 的實戰經驗](https://itplus.ithome.com.tw/vod-page/1345?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

31 分