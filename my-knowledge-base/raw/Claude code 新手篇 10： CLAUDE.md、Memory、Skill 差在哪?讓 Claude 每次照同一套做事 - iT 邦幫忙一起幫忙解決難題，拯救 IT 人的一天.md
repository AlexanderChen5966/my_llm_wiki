---
title: "Claude code 新手篇 10： CLAUDE.md、Memory、Skill 差在哪?讓 Claude 每次照同一套做事 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10424843"
author:
  - "[[iThome]]"
published:
created: 2026-11-04
description: "這是「Claude Code 實戰手冊」系列的 G10。G5 教你換對話前寫交接檔、G6 和 G9 教你讓 Claude 做網頁 —— 這些一句話用久了，你會發現自己一直在重打同一段指令。這篇講 sk..."
tags:
  - "clippings"
status: processed
---
2

## Claude code 新手篇 10： CLAUDE.md、Memory、Skill 差在哪?讓 Claude 每次照同一套做事

- 分享至

> 這是「Claude Code 實戰手冊」系列的 G10。G5 教你換對話前寫交接檔、G6 和 G9 教你讓 Claude 做網頁 —— 這些一句話用久了，你會發現自己一直在重打同一段指令。這篇講 skill：把它存成一個檔案，之後一個指令就叫得出來，也講怎麼讓它跟得上你越改越多的做法。

## 1\. 一套做法，散在三個地方

G9 的結尾留了一句：同一句話打到第三次，就該把它變成一個指令。

先講我自己的例子。我在 Threads、粉專發的說明小動畫，是叫 Claude 用程式一格一格畫出來的：頭像的表情、字卡、人站在哪裡，全部由程式算，最後串成影片。10 月 11 日到 18 日，做了 9 支。

每一支背後都是同一套做法。但這套做法不是一開始就有的，是一支一支改出來的：第一支，我挑了眼睛的反光和汗滴的位置；10 月 13 日定了人怎麼走位；10 月 14 日加了開頭和結尾，還分成發在自己的頻道、發在社團兩種。

其實第一支做完那天，我就把做法存成了一個 skill。一週後回頭看，才發現問題：

- skill 裡寫的，還是第一天的規格：直式 3:4、10 秒。後來早就改成 4:5、每頁多停 1.5 秒了。
- 後來加的規矩，全寫在另一份做法筆記裡，已經 147 行，一條都沒放回 skill。
- 還有幾條，記在 Claude 的記憶裡（G4）。

做法散在三個地方，每個地方都只有一部分是新的。每次我說「做一支影片」，Claude 得自己去翻記憶、找筆記，再對照一份過時的 skill。

這篇講 skill 是什麼、怎麼做，也講怎麼讓它跟得上你。第 7 節，我會把這三個地方整理成一個新的 skill。

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796mrZMyHhKt1.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796mrZMyHhKt1.png)

---

## 2\. Skill 是什麼

一句話： **一個資料夾，裡面放一份 `SKILL.md` ，寫的是「這件事每次怎麼做」。**

> **Skills extend what Claude can do. Create a `SKILL.md` file with instructions, and Claude adds it to its toolkit. Claude uses skills when relevant, or you can invoke one directly with `/skill-name`.**
> 
> skill 擴充 Claude 能做的事。建立一份 `SKILL.md` ，寫上指示，Claude 就會把它收進自己的工具箱。相關的時候它會自己用，你也可以打 `/skill 名稱` 直接叫它。  
> —— [Claude Code 官方文件〈Extend Claude with skills〉](https://code.claude.com/docs/en/skills)

所以叫它的方法有兩種：

- **你叫：** 打 `/名字` ，它就照著做。
- **Claude 自己叫：** 它看得到每個 skill 的說明，覺得你現在要做的事對得上，就自己拿出來用。

**其實你已經用過了。** `/code-review` 、 `/loop` ，還有 G9 的 `/design` 、 `/slides` ，官方都叫「內建 skill」—— Claude Code 本來就帶著的。這篇要講的，是怎麼做你自己的。

我原本以為自己很少用 skill。寫這篇之前，翻了這台電腦 8 月底以來的 Claude Code 對話記錄：Claude 一共載入了 21 次、9 種 skill —— 畫圖表的、做 Excel 和 Word 的、做 artifact 的，還有我自己做的動畫 skill。我自己打 `/` 叫的，0 次；開口指定「用某個 skill」的，只有 1 次。 **其他時候，我只說要做什麼，是它自己挑的。**

---

## 3\. 它能幫你做什麼

先講好處。我自己的用法，大概是這三種：

| 用法 | 例子 | 做成 skill 之後 |
| --- | --- | --- |
| 常打的一段指令 | 換對話前的交接檔、「做成 artifact」 | 打 `/handoff` ，不用重打 |
| 好幾個步驟的做法 | 我做社群說明動畫的流程：去背、畫表情、逐格截圖、串成影片 | 每次照同一套，不會漏步驟 |
| 參考資料，用到才讀 | 我的網頁配色和字型 | 做頁面時它自己套，不做頁面時不佔位置 |

什麼時候該做？官方給的時機很具體：

> **You paste the same playbook or multi-step procedure into chat for the third time → Capture it as a skill**
> 
> 同樣的做法或多步驟流程，你貼到第三次了 —— 就把它做成 skill。  
> —— [Claude Code 官方文件〈Extend Claude Code〉](https://code.claude.com/docs/en/features-overview)

反過來，這幾種不用做：

- **只做一次的事。** 直接打就好。
- **每次對話都要知道的規矩。** 例如「寫中文一律用全形標點」—— 那是 G3 `CLAUDE.md` 的工作，第 7 節會講差在哪。

---

## 4\. 動手做第一個：/handoff

動畫那個要裝 Playwright 和 ffmpeg，當第一個 skill 太重了。先做一個五分鐘做得完的：把 G5 那段交接指令做成 skill。

G5 教你換對話之前，叫 Claude 寫一份交接檔：目標、做完什麼、為什麼這樣決定、下一步、哪裡有雷。那段指令每次都要重打一次，正適合做成 skill。只要三步：

1. 建一個資料夾： `~/.claude/skills/handoff/`
2. 在裡面放一份 `SKILL.md`
3. 打 `/handoff`

`~` 是你的使用者資料夾（Mac 是 `/Users/你的名字` ，Windows 是 `C:\Users\你的名字` ）。放在這裡，你所有專案都用得到。

`SKILL.md` 的內容，可以直接抄：

```markdown
---
description: 換對話前，把目前的進度寫成一份交接檔，讓下一個對話接手。
disable-model-invocation: true
argument-hint: "[這次任務的主題]"
---

我們等一下要換一個新對話繼續。幫我寫一份交接檔。

- 主題：$ARGUMENTS（空白的話，你自己取一個簡短的英文主題）
- 存在專案的 handoff/ 資料夾，檔名是「今天日期-主題.md」，例如 handoff/2026-10-18-g10-draft.md
- 用繁體中文，照下面的標題和順序寫，不要自己加段落：

## 1. 這個任務的目標
## 2. 已經完成什麼、改了哪些檔
## 3. 關鍵決定，以及為什麼這樣決定
## 4. 還沒做的下一步（照順序）
## 5. 不要碰的地方、踩過的雷

寫完告訴我檔案路徑，還有新對話開場要打的那一句。
```

拆開來看：

- **最上面 `---` 包起來的那段** ，是給 Claude Code 看的設定：
	- `description` ：這個 skill 做什麼。打指令時選單上看到的就是這句。
		- `disable-model-invocation: true` ：只有你能叫，Claude 不會自己用。什麼時候換對話，該由你決定。
		- `argument-hint` ：打 `/handoff` 的時候，旁邊會提示你可以接一個主題。
- **下面那段** ，就是你本來每次要重打的指令。
- **`$ARGUMENTS`** ：你在 `/handoff` 後面打的字，會填進這個位置。打 `/handoff g10-draft` ，主題就是 `g10-draft` 。

存好之後，在輸入框打 `/hand` ，選單就會出現它：

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796rIqM6tYUQw.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796rIqM6tYUQw.png)

選好之後，後面會出現灰色的提示，告訴你可以接一個主題 —— 這就是 `argument-hint` ：

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796Py9QF9yRCH.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796Py9QF9yRCH.png)

打 `/handoff g10-draft` ，它就照那五段寫好一份交接檔：

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796Uk3CWxocRR.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796Uk3CWxocRR.png)

> ⚠️ **要在做事的那個對話裡打。** 仔細看截圖最下面，它說「第 3、5 節是我從程式碼推斷的」—— 因為我是開了新對話、第一句就打 `/handoff` ，前面的討論它根本看不到，「為什麼這樣決定」「踩過的雷」只能從程式碼猜。 `/handoff` 要在做完事、準備換對話的 **那個** 對話裡打。

> 💡 **不想自己建資料夾？叫 Claude 做。** 我把 G5 那段指令整段貼給它，前面加一句：「我每次換對話前都會打下面這段話。幫我把它做成一個個人 skill，名字叫 handoff，放在 ~/.claude/skills/。」不到一分鐘就建好了，還自己多加了幾條規則：寫之前先看專案的實際狀態、考慮過但放棄的做法也要寫、從程式碼猜的要標明是猜的。
> 
> 但建好之後，一定要打開檢查。它寫的版本 **沒有** 加 `disable-model-invocation: true` ，說明裡還寫了「使用者說要換對話……時使用」—— 也就是你一講「要換對話了」，它就可能自己開始寫交接檔。想自己決定什麼時候交接，就自己補上那一行。

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796IWun2jdIyZ.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796IWun2jdIyZ.png)

> 💡 **選單裡找不到？** 如果 `~/.claude/skills/` 這個資料夾是你剛剛才建的，Claude Code 還沒開始看它。打 `/reload-skills` ，或重開一次 Claude Code。重開最省事：之後再改檔案，就會自動生效。

---

## 5\. 放哪裡：給自己，還是給整個專案

| 放在 | 路徑 | 誰用得到 |
| --- | --- | --- |
| 個人 | `~/.claude/skills/名字/SKILL.md` | 你自己，這台電腦上的所有專案 |
| 專案 | 專案裡的 `.claude/skills/名字/SKILL.md` | 這個專案；commit 進 git，隊友也有 |

同一個名字兩邊都有的話，個人的優先。

剛開始，放個人就好。等你發現某個 skill 只跟某個專案有關，再搬進那個專案。我的動畫 skill 就放在專案裡：它要用的範本、頭像、做好的影片，都在那個專案的資料夾。

想看自己現在有哪些 skill，打 `/skills` ：

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796o5jsJCZeLk.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796o5jsJCZeLk.png) ）

> 💡 **網路上的舊教學寫 `.claude/commands/` ？** 那是舊格式，現在已經併進 skill 了。官方寫得很清楚：
> 
> **Custom commands have been merged into skills.**
> 
> 自訂指令已經併進 skill。
> 
> 舊的檔案照樣能用，`.claude/commands/handoff.md` 和 `.claude/skills/handoff/SKILL.md` 都會做出 `/handoff` ；兩個同名時，skill 優先。新做的就用 skill，因為它多了資料夾可以放參考檔，也能讓 Claude 自己判斷要不要用。

> ⚠️ **取名別跟內建指令撞名。** 你的 skill 會蓋掉同名的內建指令 —— 例如你做了一個叫 `usage` 的 skill，在終端機打 `/usage` 就不再顯示用量，而是跑你寫的東西。取名前先打 `/` 加上名字，看看有沒有同名的。

> 💡 **雲端跑的讀不到個人 skill。** 排程在雲端跑的工作（routine）不會讀你電腦裡的 `~/.claude/skills/` 。要在雲端用，得把 skill commit 進專案的 `.claude/skills/` 。

---

## 6\. 誰來叫：你，還是 Claude

第 2 節說過，叫它的方法有兩種。差別在 `description` 那一行。

Claude 每次都看得到所有 skill 的名字和說明。你請它做事時，它會拿你的話去比對那些說明，對得上就自己載入。所以 **`description` 要寫「做什麼、什麼時候用」** ，用你平常會講的字。

預設是你跟 Claude 都能叫。要改，加一行設定：

| 設定 | 你能叫 | Claude 能自己叫 | 適合 |
| --- | --- | --- | --- |
| 不寫（預設） | ✅ | ✅ | 大部分的 skill |
| `disable-model-invocation: true` | ✅ | ❌ | 有後果、要你決定時機的事：交接、commit、發文 |
| `user-invocable: false` | ❌ | ✅ | 背景知識，你不會想打指令叫它 |

官方舉的例子很好懂：

> **You don't want Claude deciding to deploy because your code looks ready.**
> 
> 你不會想讓 Claude 因為「程式碼看起來好了」，就自己決定上線。

### 第二個例子：讓它自己套用我的配色

G9 的結尾那句「做成 artifact，用我的配色」，也可以做成 skill。這次反過來，讓 Claude 自己判斷要不要用。

`~/.claude/skills/page-style/SKILL.md` ：

```markdown
---
description: 網頁樣式（顏色、字型、版面寬度）。做任何 HTML 頁面、artifact、圖卡時套用。
---

做頁面時照這套樣式：

- 背景 #0E1A1F，內文 #F5F2EA，次要文字 #94A3A8
- 強調色只用 #FF6B35（標題、重點數字）
- 標題字型 Fraunces，內文 Inter + Noto Sans TC，程式碼 JetBrains Mono
- 內文最寬 760px，行高 1.7
```

存好之後，我只說「把這篇的重點做成一頁 HTML」，沒有提到配色，它就自己把 `page-style` 拿出來用了：

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796Jbrx4Ua9cD.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796Jbrx4Ua9cD.png)

> 💡 **自己載入，不是每次都會發生。** 我用同一句話、換成比較小的 Haiku 模型再試一次，它就沒把 `page-style` 拿出來用，做出來的頁面是它自己的配色。一定要套用的時候，直接打 `/page-style` ，或在句子裡講「用 page-style」。

> 💡 **G9 不是說配色寫進 `CLAUDE.md` 就好？** 也對。如果這個專案天天在做頁面，寫進 `CLAUDE.md` 就夠了。我的情況是：每個專案都想用同一套配色，但不是每次對話都在做頁面 —— 做成個人 skill，每個專案都有，平常只佔一行說明。

---

## 7\. 這不就是 CLAUDE.md 嗎？

兩個都是「寫給 Claude 看的指示」，差在 **什麼時候載入** 。

`CLAUDE.md` 每次對話一開始就整份載入，從頭到尾都在。skill 分三層，用到哪層才載入哪層：

- **第一層：名字和說明。** 每次都在，讓 Claude 知道有這個 skill。Anthropic 的 Agent Skills 文件估計，一個大約 100 tokens。
- **第二層：本文。** 你叫它、或 Claude 決定用它的時候，才整份載入。載入之後，會一直留在這個對話裡。
- **第三層：參考檔、腳本。** 放在同一個資料夾裡，本文有提到、真的用到時才讀。

第一層到底佔多少？第 5 節那張 `/skills` 的截圖，右邊就有數字：我的 `page-style` 不到 20 tokens，清單裡最多的 `docs` 大約 340。說明寫多長，就佔多少。

![https://ithelp.ithome.com.tw/upload/images/20261021/20182796j9HicVqYe6.png](https://ithelp.ithome.com.tw/upload/images/20261021/20182796j9HicVqYe6.png)

所以一份很長的參考資料，放進 `CLAUDE.md` ，每次對話都整份佔著 context；放進 skill，平常只佔一行說明。官方的說法：

> **Unlike CLAUDE.md content, a skill's body loads only when it's used, so long reference material costs almost nothing until you need it.**
> 
> 跟 `CLAUDE.md` 不一樣，skill 的本文只在用到時才載入，所以很長的參考資料，在你需要之前幾乎不花成本。

至於 skill 本身要寫多長，官方有個建議：

> **Keep `SKILL.md` under 500 lines. Move detailed reference material to separate files.**
> 
> `SKILL.md` 保持在 500 行以內，詳細的參考資料移到另外的檔案。

本文放「怎麼做」，細節（長長的規格、範例）拆成另一個檔，本文寫一句「需要時讀 reference.md」就好。

### 實際拆一次：我的動畫 skill

回到第 1 節那套動畫做法。我請 Claude 把三個地方（舊的 skill、做法筆記、它的記憶）讀一遍，整理成一個新的 skill，再一條一條對照實際的動畫檔，確認寫進去的東西真的存在。

整理完是這樣：

```markdown
.claude/skills/avatar-code-animation/
├── SKILL.md            ← 規則和步驟（49 行）
├── reference/          ← 細節，用到才讀
│   ├── spec.md         規格、開頭結尾、輸出指令
│   ├── anim-html.md    動畫檔要改哪幾塊
│   ├── expressions.md  14 種表情
│   ├── staging.md      走位
│   ├── checklist.md    交付前檢查、踩過的坑
│   └── rig-v2.md       還在試的新做法
├── template/           ← 新的一支從這裡複製
└── scripts/            ← 換頭像才用的去背程式
```

跟第一版比：

|  | 第一版（10/11） | 第二版（10/18） |
| --- | --- | --- |
| 檔案 | 一份 `SKILL.md` | `SKILL.md` + 6 份參考檔 + 範本 + 程式 |
| `SKILL.md` | 170 行，什麼都寫在裡面，連程式碼都貼進去 | 49 行：一定要守的規則、步驟、參考檔清單 |
| 規格 | 直式 3:4、10 秒 | 4:5、每頁停 1.5 秒、開頭結尾看發在哪裡 |
| 規矩在哪 | skill、做法筆記、Claude 的記憶，各一部分 | 整理進同一個 skill |

`SKILL.md` 最上面，放的是「一定要守的規則」：眼睛不要反光、人不要翻轉、跟股票有關的影片一定要有免責聲明。放最上面是有原因的，第 8 節會講。

官方對這種情況有一句話：

> **A workflow you keep tweaking by hand is a skill that needs another revision.**
> 
> 一個你還在手動調整的流程，代表這個 skill 該改版了。  
> —— [Claude Code 官方文件〈Extend Claude Code〉](https://code.claude.com/docs/en/features-overview)

> 💡 **舊的那份要關掉。** 我的第一版是在 claude.ai 上做的，會同步到電腦上。新版放在專案裡、用同一個名字，打 `/avatar-code-animation` 叫到的是新的。但我開了一個新對話問它，它的清單裡兩份都在 —— 我只說「做一支影片」的時候，它有可能挑到舊的。所以新版確認沒問題後，舊的要到 claude.ai 的設定裡關掉。

### 什麼時候用哪個

走到 G10，「把資訊交給 Claude」的方法已經有好幾種了。放在一起比：

| 你想要的 | 用什麼 | 在哪篇 |
| --- | --- | --- |
| 這次就好 | 直接打一句話 | — |
| 每次對話都要知道的規矩（例：寫中文用全形標點） | `CLAUDE.md` | G3 |
| 關於你這個人（偏好、習慣） | Memory | G4 |
| 這次做到哪，交給下一個對話 | 交接檔 | G5 |
| 有時候才用、一用就是一整套做法或資料 | skill | 本篇 |
| 一定要每次做到，不能靠它記得 | hooks | G13 |

G6 說過：「每次都要 HTML，就寫進 `CLAUDE.md` 。」這句還是對的。這篇補上另一半： **不是每次都要，但一要就是一整套的，做成 skill。**

> 💡 **`/handoff` 跟 G5 的交接檔是什麼關係？** 交接檔是 **產出** ， `/handoff` 是 **怎麼產** 。skill 讓每一份交接檔都照同一套寫。

---

## 8\. 限制和常卡的地方

- **它是建議，不是保證。** 官方說得很直接：寫在 `CLAUDE.md` 或 skill 裡的指示，"is a request, not a guarantee"。一定要每次都做到的事，要用 G13 的 hooks。
- **改了 `SKILL.md` ，要再叫一次。** skill 載入之後，同一個對話裡它不會回頭重讀檔案。改完重新打一次 `/名字` 。
- **對話被壓縮後，可能只剩前段。** 對話太長被自動壓縮時，每個 skill 只保留最前面的一段（前 5,000 tokens）。最重要的指示寫在最上面；它開始不照做了，就再叫一次。
- **說明寫得太模糊，它不會自己用；寫得太廣，它會亂用。** 自動觸發靠的就是 `description` 那一行。
- **格式寫錯，不會報錯。** 最上面的 `---` 一定要在第一行； `disable-model-invocation` 少一個連字號，它不會提醒你，只會當作沒寫。
- **每個 skill 的說明，每一輪都佔一點位置。** 一個不多，一百個就有感了。打到第三次再做，不要一口氣做一堆。清理留到 G20 的 `/doctor` 。

---

## 9\. 本課單字：把重複的工作流打包

### /你取的名字

叫出你做的 skill，例如 `/handoff` 。後面接的字會填進 `$ARGUMENTS` 。

### /skills

列出你所有的 skill。可以打字篩選，按 `t` 依佔用的 token 數排序。

### /reload-skills

剛建了新的 skills 資料夾、選單裡卻找不到的時候用。

### 「幫我把這段做成 skill」

一句話，讓 Claude 把你重打的指令存成 `SKILL.md` 。存好記得打開檢查，特別是它有沒有加「只讓你叫」的設定。

---

## 10\. 給新手的三個提醒

### a. 打到第三次再做

只做過一次的事，不值得做成 skill。每個 skill 的說明都佔位置，留給真的常用的。

### b. 有後果的，只讓你自己叫

交接、commit、發文這種做了就有後果的，加上 `disable-model-invocation: true` 。什麼時候做，你決定。

### c. 它是建議，不是保證

skill 讓它每次照同一套做，但它還是可能跳步驟。一定要做到的，等 G13。

---

## 結語

回到開頭那套動畫做法。現在它只在一個地方：我說「做一支影片」，Claude 拿到的就是最新的那一套；之後再改規矩，改的也是同一個地方。

G9 處理的是「 **別人** 要花多少力氣，才拿得到你最新的那份」。這篇處理的是「 **你** 要花多少力氣，才能讓它每次照同一套做」。答案是一份 `SKILL.md` 。

但 skill 教的是「怎麼做」，碰不到你電腦以外的東西。交接檔想直接存進 Notion、動畫素材想從雲端硬碟抓，就要換另一個工具。官方是這樣分的：

> **MCP connects Claude to external services. Skills extend what Claude knows, including how to use those services effectively.**
> 
> MCP 讓 Claude 連上外部的服務；skill 擴充 Claude 知道的事，包括怎麼把那些服務用好。

下一篇就講 MCP。

> 📌 **關於本篇的細節：** 設定欄位、載入方式、指令名稱，都是依 **2026 年 10 月的官方文件** 和 v2.1.302 整理的。skill 的功能還在持續增加，如果你是很久以後讀到這篇，請以官方文件為準。

---

下一篇 **G11 — MCP：連接外部世界** 。skill 教它怎麼做，MCP 讓它碰得到你的工具。

---

## 系列文章

- G1 Claude Code 從零開始 — Portfolio v2 重做
- G2 用量與計費 — 搞懂怎麼收費，再決定要不要省
- G3 CLAUDE.md — 讓 Claude 跨對話記得你的專案
- G4 Memory 系統 — 讓 Claude 不只記得專案，還記得你
- G5 換 section 不斷線 — 把當下進度交接給下一個對話
- G6 用 HTML 跟 Claude 溝通 — 讓你一眼看懂它給你的東西
- G7 控制 Claude 怎麼想 — 選模型與思考深度
- G8 先想清楚再動手 — Plan Mode 與權限模式
- G8.5 auto mode — 你的預設模式到底在幫你擋什麼
- G9 Artifacts — 把產出變成一個會更新的網址
- **G10（本篇）** Skills — 把重複的工作流打包
- G11 MCP — 連接外部世界（規劃中）

> 完整 20 篇地圖會放在系列首頁。本系列同步發佈於 Medium。

---

**關於我**

Tim Wei / EY 安永 Technology Consulting 顧問（AI/ML），做 RAG 知識系統、機器學習模型與 AI 治理，也擔任數位發展部 AI 人才培育計畫的講師。

歡迎在留言區跟我討論，或在 [LinkedIn](https://www.linkedin.com/in/timwei0801) 上找我。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1793367981_6ae49fad90cca.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2610B9008)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/2c9f5583-e80c-42d0-bfa2-593c55be1c26_small.png)

[Mock framework 原始碼剖析](https://itplus.ithome.com.tw/vod-page/747?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

37 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/4bfeb802-4f92-4d9a-bbff-e1659bb7c2f3_small.png)

[【DevOps入門班】超越直覺：產品交付階段的數據驅動科學](https://itplus.ithome.com.tw/vod-page/971?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

42 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/0dd7cb73-1a3c-4f23-9261-965ce5bae171_small.png)

[如何透過Azure快速打造MLOPS](https://itplus.ithome.com.tw/vod-page/976?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

26 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/00fac5bb-f3c6-4f25-8fad-e64c952ed68a_small.png)

[from generativeai import SecureCode](https://itplus.ithome.com.tw/vod-page/1055?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

36 分