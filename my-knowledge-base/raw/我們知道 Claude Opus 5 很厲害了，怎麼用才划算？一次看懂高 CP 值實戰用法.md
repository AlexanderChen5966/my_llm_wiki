---
title: "我們知道 Claude Opus 5 很厲害了，怎麼用才划算？一次看懂高 CP 值實戰用法"
source: "https://technews.tw/2026/07/29/claude-opus-5/"
author:
  - "[[徐 知誼]]"
published:
created: 2026-08-03
description: "Anthropic 近期推出 Claude Opus 5，主打複雜的代理型（agentic）程式設計與企業級工作尤其擅長費時的繁瑣任務。但對使用者來說，換成新模型只是第一步；真正決定產出品質的，是如何調校提示詞、用記憶機制留住跨工作階段的專案脈絡以及用 Skill 把重複流程一次設定、長期沿用。搞懂..."
tags:
  - "clippings"
status: processed
---
[![我們知道 Claude Opus 5 很厲害了，怎麼用才划算？一次看懂高 CP 值實戰用法](https://img.technews.tw/wp-content/uploads/2026/07/29162053/Claude-Opus-5-800x450.jpg)](https://img.technews.tw/wp-content/uploads/2026/07/29162053/Claude-Opus-5-800x450.jpg "我們知道 Claude Opus 5 很厲害了，怎麼用才划算？一次看懂高 CP 值實戰用法")

Anthropic 近期推出 Claude Opus 5，主打複雜的代理型（agentic）程式設計與企業級工作尤其擅長費時的繁瑣任務。但對使用者來說，換成新模型只是第一步；真正決定產出品質的，是 **如何調校提示詞** 、 **用記憶機制留住跨工作階段的專案脈絡** 以及 **用 Skill 把重複流程一次設定、長期沿用** 。搞懂這三者的分工，就能把每次都要重講的指示，變成一勞永逸的設定。

### Opus 5 的行為變了，舊提示詞要跟著調校

Anthropic 官方宣稱 Opus 5 逼近旗艦模型 Fable 5 的智慧水準，但只需要一半成本。然而 Opus 5 若沿用舊版 Claude Opus 4.8 的提示詞，多半能直接運作，但它有幾個明顯的行為變化，若不調整，反而會拖慢效率或增加成本。面對 Opus 5 時應把它當成「勤奮的代理人」而非聊天機器人，只要給予明確目標，它會自我檢查、反覆更新（但長時間自動執行時仍要有人在旁把關）。

官方工程師 Thariq 指出，過去使用逐步腳本的方式「過度約束」Claude，如今建議使用者應「給判斷力、別給規則」，並刪掉互相矛盾的舊指示（例如同時要求「詳盡」又「簡短」）。以下幾點是可以改善 Opus 5 表現的提示詞建議：

#### 1\. 當 Opus 5 回應變長：需明確要求精簡

Opus 5 的預設回應比舊版更長，且內建可調的「思考力道」（effort）旋鈕，遇到難題時調高、例行工作就調低──用運算量換取深度，是控制花費的關鍵。不過，effort 參數控制的是「模型思考多少」，而非「說多少」，因此調低 effort 不一定會縮短可見回應。要控制長度，請在提示詞中明講：

`Keep responses focused, brief, and concise. Keep disclaimers and caveats short, and spend most of the response on the main answer. When asked to explain something, give a high-level summary unless an in-depth explanation is specifically requested.`

###### （回答要重點突出、簡潔扼要。免責聲明和注意事項也要簡短，並將大部分篇幅用於回答主要問題。當被要求解釋某些內容時，除非明確要求詳細解釋，否則只需提供概要即可。）

若是較長的系統提示詞，可在結尾附上簡短提醒：

`<tone_preference>`  
`Keep outputs reasonably concise.`  
`</tone_preference>`

#### 2\. 進度說明：調整敘述的節奏

代理型工作中，Opus 5 傾向頻繁說明「接下來要做什麼」。要調降這類敘述，直接描述你想要的節奏與樣貌（正面示例比「不要做什麼」更有效）：

`Before your first tool call, say in one sentence what you're about to do.`  
` While working, give a brief update only when you find something important or change direction.`  
` When you finish, lead with the outcome.`

###### （第一次使用工具之前，先用一句話概括你即將要做什麼。工作過程中，只有當你發現重要資訊或改變方向時才進行簡短報告。工作完成後，先說明結果。）

#### 3\. 書面產出長度

寫入磁碟的檔案（報告、Markdown 文件、摘要）也常偏長。若你的產品包含 Claude 撰寫的文件，加上長度校準：

`Match the length of written documents to what the task needs: cover the substance, ``but do not pad with filler sections, redundant summaries, or boilerplate.`

###### （文檔長度應與任務需求相符：涵蓋實質內容，但不要加入無關內容、冗餘總結或模板式語句。）

#### 4\. 移除過時的驗證指示（重要）

由於 Opus 5 的強大能力，它會自行驗證工作成果，也會自行修正錯誤。所以若是舊提示詞含有「加入最終驗證步驟」「用子代理再檢查一次」「回應前再核對」這類指示，請移除──它們會與模型本身的行為疊加，造成過度驗證，白白浪費 token 且無助於品質。這是從舊模型遷移時最容易被忽略、卻最該清理的一環。

#### 5\. 約束任務範圍

同上，Opus 5 厲害歸麗害，但有時會像個搞職場老鳥一樣「假會」，自作主張擴大任務範圍、加入未被要求的步驟。針對範圍明確的窄任務，需在提示詞明白限定「只交付被要求的、按預期的範圍」：

`Deliver what was asked, at the scope intended. Make routine judgment calls yourself, ``and check in only when different readings of the request would lead to materially ` `different work. Finish the whole task, and stop short of actions that are clearly ` `beyond what was asked.`

###### （依照要求，在預定的範圍內完成任務。自行做出常規判斷，只有當對請求的不同解讀會導致實質的工作差異時才進行核查。完成整個任務，避免採取明顯超出要求的行動。）

#### 6\. 控制子代理數量

Opus 5 也比舊版更愛委派子代理。但委派只在「真正獨立、可平行、規模夠大」的工作使用才划算，用在小任務反而倍增成本與時間，因此若你的框架支援子代理，需要給明確規則或設上限：

`Delegate to a subagent only for large tasks that are genuinely independent and`  
`parallelizable. Do not delegate work you can finish yourself in a handful of tool`  
`calls, and do not use subagents to verify your own work. Keep spawn counts low.`

###### （只有當任務規模較大且真正獨立且可並行化時，才將其委託給子代理。不要委託那些你只需呼叫幾次工具即可完成的工作，也不要使用子代理來驗證你自己的工作。保持較低的生成數量。）

#### 7\. 思考力道（effort）的運用

Opus 5 把額外的 effort 更可靠地轉換成更好結果，所以層級選擇更關鍵。所有層級分由低至高分別為 ` low` 、 `medium` 、 `high` 、 `xhigh` 、 `max` 。建議從預設 ` high` 起步，依你自己的評測調整：品質不變時多用 ` low/medium` 省成本，吃重的程式與代理工作再升到 ` xhigh` 。跑 `xhigh` 或 `max` 時，記得把 `max_tokens` 設大，留下足夠思考與工具呼叫的空間。

#### 8\. 思考模式預設開啟，停用有前提

Opus 5 有個容易踩雷的設計： **思考（thinking）預設開啟** ，而且只有在 effort 為 `high` 或以下時才能停用；若在 `xhigh` 或 `max` 時設定停用思考，會直接回傳 400 錯誤。這是相較於 Opus 4.8 的一項重大變更。

更要注意的是，停用思考時，可見輸出偶爾會出現兩種異常：一是模型把工具呼叫寫成純文字，而非發出結構化的 `tool_use` 區塊，導致該呼叫不會執行，且在代理迴圈殘留對話歷史、影響後續回合，這在搜尋等工具密集的工作最常見；二是可見回應中夾帶 `<thinking>` 等內部 XML 標籤。

Anthropic 給的建議很明確： **最好的做法是保持思考開啟、改用較低的 effort 控制成本** 。多數任務下，「思考開啟＋ `low` effort」在相近成本下的表現，會優於「停用思考」。若整合情境非停用思考不可，可用一段合併提示同時緩解兩種異常，允許模型在工具呼叫前先說一句話、在沒有合適工具時直說、並禁止輸出內部標籤（避免特別點名 thinking 標籤，通用寫法更有效）：

`When you use a tool, you may say a brief sentence first. If no tool can express`  
`what the user asked for, say so instead of guessing. Do not include internal or`  
`system XML tags in your response.`

（使用工具時，您可以先說一句簡短的話。如果工具無法表達使用者的需求，請直接說明，不要猜測。請勿在回覆中包含內部或系統 XML 標籤。）

### 讓 Claude Code 記住你的專案：CLAUDE.md 與自動記憶

每次 Claude Code 工作階段都從空白的上下文視窗開始，靠兩套機制把知識帶到跨工作階段：一是你手寫的 CLAUDE.md，二是 Claude 自己累積的自動記憶。兩者都在每次對話開始時載入，但都被視為「上下文參考」而非「強制設定」，要在任何情況下硬性擋下某個動作，得改用 [PreToolUse hook](https://code.claude.com/docs/en/hooks-guide) 。

![](https://img.technews.tw/wp-content/uploads/2026/07/29170522/Claude-Opus-5-tips-0.jpg)

▲ CLAUDE.md 與自動記憶的比較。（Source： AI 製圖）

**▲ CLAUDE.md 與自動記憶的比較。（Source： AI 製圖）**

#### CLAUDE.md：放每次都該記得的事實

CLAUDE.md 放的是建置指令、慣例、專案結構、「一律執行 OOO」這類的規則。判斷準則很簡單：當 Claude 第二次犯同樣的錯、code review 抓到它早該知道的慣例，或你又被迫輸入一次上次講過的更正，就是該寫進去的時候。它可放在四種位置，範圍由廣至窄分別是組織政策層、使用者層（ `~/.claude/CLAUDE.md` ）、專案層（`./CLAUDE.md` ），以及會被 gitignore 的本機層（`./CLAUDE.local.md` ）。若你想快速起步，執行 `/init` 就會分析程式碼庫、自動產生含建置指令與慣例的起始檔案。

撰寫 CLAUDE.md 有幾個要領：每份控制在 200 行以內，過長會消耗上下文並降低遵循度；用 Markdown 標題與項目符號分組；指示要具體到可驗證，「使用 2 空格縮排」勝過「格式化程式碼」，「提交前執行 `npm test` 」勝過「測試你的變更」。大型專案則可用 `.claude/rules/` 把指示拆成多個主題檔，並透過 frontmatter 的 `paths` 欄位做路徑範圍規則，只有 Claude 處理符合樣式的檔案時才載入，省下上下文空間。

#### 自動記憶：讓 Claude 自己累積

自動記憶讓 Claude 在工作中自行存下對未來有用的筆記，包括建置指令、除錯心得、架構註記與風格偏好，完全不需你動手。它預設開啟，儲存在 `~/.claude/projects/<project>/memory/` ，同一 git 儲存庫的所有 worktree 共用，且為機器本機，不會跨機同步。目錄裡的 `MEMORY.md` 是索引，每次工作階段載入前 200 行或 25KB，細節則放在按需讀取的主題檔。這些全是純 Markdown，隨時可用 `/memory` 瀏覽、編輯或刪除；你也可以直接對 Claude 說「always use pnpm, not npm」，它就會存進自動記憶。

兩者的分工可以這樣記：CLAUDE.md 是你手寫、放事實與規則；自動記憶是 Claude 自己學、放心得與模式。若 Claude 沒遵循 CLAUDE.md，先跑 `/context` 確認檔案有載入，再檢查指示是否夠具體、有無跨檔衝突；必須在特定時機執行的規則，則該改寫成 hook。

### 用 Skill 把重複流程一次打包

當你反覆貼上同一份指示、檢查清單或多步驟流程，或 CLAUDE.md 中某段已長成「流程」而非「事實」時，就該做成 Skill。與 CLAUDE.md 最大的不同在於，Skill 的內容只在被使用時才載入，因此長篇參考資料在用到之前幾乎不花上下文成本。值得一提的是，自訂命令（custom commands）已併入 Skill，`.claude/commands/deploy.md` 與 `.claude/skills/deploy/SKILL.md` 都會建立 `/deploy` ，既有檔案仍可用，但 Skill 多了支援檔案、呼叫控制與自動載入等功能。

一個 Skill 就是一個目錄，以 `SKILL.md` 為入口，目錄名稱就是你輸入的命令。 `SKILL.md` 分兩部分： `---` 之間的 YAML frontmatter 告訴 Claude 何時使用，後續的 Markdown 則是實際指示：

`---`  
`description: Summarizes uncommitted changes and flags anything`  
`risky. Use when the user asks what changed or wants a commit message.`  
`---`

`## Current changes`  
`` !`git diff HEAD` ``

`## Instructions`  
`Summarize the changes above in two or three bullet points,`  
`then list any risks you notice.`

其中一個實用技巧是動態上下文注入，用 `` !`git diff HEAD` `` 這樣的語法，Claude Code 會先執行指令、把輸出替換進內容，讓 Claude 收到的是含實際資料的提示詞。觸發方式有兩種：問一個符合描述的問題讓 Claude 自動載入，或直接輸入 `/skill-name` 。

Skill 可放在企業、個人（ `~/.claude/skills/` ）、專案（`.claude/skills/` ）與外掛四種位置，同名時企業優先於個人、個人優先於專案，且任一層級都會覆蓋同名的內建 Skill。frontmatter 裡有幾個常用欄位值得記住：

![](https://img.technews.tw/wp-content/uploads/2026/07/29170809/Claude-Opus-5-tips-1.jpg)

▲ CLAUDE.md 與自動記憶的比較。（Source： AI 製圖）

要傳參數，用 `$ARGUMENTS` 、 `$ARGUMENTS[N]` 或簡寫 `$N` 取用。Claude Code 也內附一組隨時可用的 Skill，例如 `/doctor` 、 `/code-review` 、 `/debug` 、 `/loop` 。撰寫自己的 Skill 時要記得，內容一旦載入就會在整個工作階段中持續佔用上下文，本體要精簡， `SKILL.md` 建議控制在 500 行以內，細節移到支援檔案。

#### 一句話記住三者分工

這三套機制不是替代關係，而是各司其職。若是每次都該記得的事實，寫進 CLAUDE.md；是反覆使用的流程，做成 Skill；想讓 Claude 從你的更正中自己學，交給自動記憶；要在特定時機硬性強制執行，則改用 hook。再搭配針對 Opus 5 行為調校過的提示詞，並清掉為舊模型寫的過時 workaround，就能讓新模型發揮它該有的實力。

![](https://img.technews.tw/wp-content/uploads/2026/07/29171227/Claude-Opus-5-tips-2-1.jpg)

▲ CLAUDE.md 與自動記憶的比較。（Source： AI 製圖）

**▲ CLAUDE.md、Skill 與自動記憶的使用時機。（Source：AI 製圖）**

- [What’s new in Claude Opus 5](https://platform.claude.com/docs/en/about-claude/models/whats-new-opus-5)
- [Prompting Claude Opus 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5)
- [How Claude remembers your project](https://code.claude.com/docs/en/memory)
- [Extend Claude with skills](https://code.claude.com/docs/en/skills)

（首圖來源： [Anthropic](https://www.anthropic.com/news/claude-opus-5) ）

### 延伸閱讀：

- [AI 會「反省」嗎？Anthropic 試圖剖開 Claude 大腦，以防範惡意行為](https://technews.tw/2026/07/27/anthropic-dissects-claude-brain-understand-ai-self-reflection-prevent-malicious-behavior/)
- [企業 AI 成本救星來了！Anthropic 以 Fable 一半價格推出 Opus 5](https://technews.tw/2026/07/26/anthropic-debuts-opus-5-at-half-the-price-of-its-fable-sibling/)
- [OpenAI 為一般使用者公布GPT-5.6 提示詞指南：別想太多，先講結果](https://technews.tw/2026/07/14/openai-launches-prompting-guidance-for-gpt-5-6/)

## 還想知道什麼？TechNews Smart 告訴你

[1\. Opus 5 降價對企業 AI 的影響？](https://technews.tw/ai-agent/claude-opus-5/24661/) [2\. 代理型 AI 如何變革開發流程？](https://technews.tw/ai-agent/claude-opus-5/24662/) [3\. 記憶機制如何提升專案管理效率？](https://technews.tw/ai-agent/claude-opus-5/24663/) [4\. Anthropic 定價戰對市場衝擊？](https://technews.tw/ai-agent/claude-opus-5/24664/) [5\. AI 具判斷力後，人類如何轉型？](https://technews.tw/ai-agent/claude-opus-5/24665/)[從這裡可透過《Google 新聞》追蹤 TechNews](https://news.google.com/publications/CAAqBwgKMJfS3gow8arWAQ)

[![Google News](https://img.technews.tw/wp-content/themes/twentytwelve/images/GoogleNews_banner.png)](https://news.google.com/publications/CAAqBwgKMJfS3gow8arWAQ)

<video title="Advertisement" src="https://rr8---sn-ipoxu-un56.googlevideo.com/videoplayback?expire=1785750112&amp;ei=4PFvar6AA9Hss8IP6PC1sQY&amp;ip=60.250.84.232&amp;id=9ea9facb08b4b6f4&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=1021&amp;met=1785721312,&amp;mh=5U&amp;mm=31&amp;mn=sn-ipoxu-un56&amp;ms=au&amp;mv=m&amp;mvi=8&amp;pcm2cms=yes&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=33.204&amp;lmt=1784858626466307&amp;mt=1785720798&amp;cpn=OG_aZQyW6YPdqm_o&amp;txp=5532534&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRQIgV02OAB0b6eramBL5MWq5uFvpqozsPBHB_-MsvddRexwCIQCWylCASNimsjlQ5gAPjWT9wkMX8EHvktE28IRPeR6ZjA==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pcm2cms,pl,rms&amp;lsig=APaTxxMwRAIgWBr1PmOQRDcH0JVD_kTKattzyEBmTLy2ayZAoz2Ehj8CIAojf6XysEdwPMzLlWFQixNAXKzomsUM3SzpzDUPz-x-" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.779.0_en.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_1452585055%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C956056724%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5Bnull%2C745150931%2Cnull%2C%5Bnull%2C1%5D%5D%2C%5Bnull%2C749060184%2Cnull%2C%5Bnull%2C128%5D%5D%2C%5B841585769%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B846355750%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B905081831%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45774999%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45661356%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761216%2Cnull%2Cnull%2C%5B1%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B10%2C%5B%5B95391980%5D%2C%5B95391981%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fpagead%2Fmanaged%2Fjs%2Factiveview%2Fcurrent%2Frx_omid_video.js%5C%22%5D%5D%5D%5D%2C%5B95391982%2C%5B%5Bnull%2Cnull%2C45792627%2C%5Bnull%2Cnull%2C%5C%22https%3A%2F%2Fpagead2.googlesyndication.com%2Fomsdk%2Freleases%2Flive%2Fomid_monitoring_bin.js%5C%22%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5B100%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398187%5D%2C%5B95398188%2C%5B%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C95332046%2C95351426%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=4323685756748806&amp;pvsid=335157727233031&amp;top_accessible_page_url=%22https%3A%2F%2Ftechnews.tw%2F2026%2F07%2F29%2Fclaude-opus-5%2F%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="360" height="203" data-src=""></iframe><iframe src="about:blank" frameborder="0" width="1px" height="1px" data-src=""></iframe><iframe src="about:blank" frameborder="0" width="1px" height="1px" data-src=""></iframe>