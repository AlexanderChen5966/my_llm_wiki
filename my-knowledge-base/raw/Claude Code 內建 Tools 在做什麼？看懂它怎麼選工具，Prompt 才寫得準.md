---
title: "Claude Code 內建 Tools 在做什麼？看懂它怎麼選工具，Prompt 才寫得準"
source: "https://israynotarray.com/ai/20260429/8473628195/"
author:
  - "[[Ray]]"
published: 2026-04-29
created: 2026-05-13
description: "這一篇稍微來聊一下 Claude Code 的內建 Tools。如果你有用過 Claude Code 應該會發現一件事，有時候你叫它讀一個檔案它直接 Read 打開，有時候你問它「這個專案有哪些 component」它跑去 Glob、Grep，可是偶爾它會繞遠路跑去用 Bash 的 cat、grep，讓人很困惑為什麼不直接用內建工具。其實 Claude 是有一套自己的工具選擇邏輯的，這篇就來聊一下 Claude Code 有哪些內建 Tools、它怎麼決定要用哪個、以及怎麼從工具使用模式反推回去調 Prompt。"
tags:
  - "clippings"
status: processed
---
![Claude Code 內建 Tools 在做什麼？看懂它怎麼選工具，Prompt 才寫得準](https://images.israynotarray.com/blog/ai/claude-tools-agens/Gemini_Generated_Image_kwkdeakwkdeakwkd.png)

Claude Code 內建 Tools 在做什麼？看懂它怎麼選工具，Prompt 才寫得準

## 前言

這一篇稍微來聊一下 Claude Code 的內建 Tools。如果你有用過 Claude Code 應該會發現一件事，有時候你叫它讀一個檔案它直接 Read 打開，有時候你問它「這個專案有哪些 component」它跑去 Glob、Grep，可是偶爾它會繞遠路跑去用 Bash 的 `cat` 、 `grep` ，讓人很困惑為什麼不直接用內建工具。其實 Claude 是有一套自己的工具選擇邏輯的，這篇就來聊一下 Claude Code 有哪些內建 Tools、它怎麼決定要用哪個、以及怎麼從工具使用模式反推回去調 Prompt。

> **Note**  
> 如果你還不熟悉 Claude Code 的基本操作，可以先去看 [想用 Claude Code 開發？這篇帶你從入門到進階](https://israynotarray.com/ai/20260128/374686091/) ，裡面有很完整的入門介紹。本篇所有工具名稱都對照官方的 [Tools reference](https://code.claude.com/docs/en/tools-reference) 。

## 為什麼要搞懂內建 Tools

你可能會想說：我又不是 Claude 本人，幹嘛搞懂它用什麼工具？

簡單來講，當你看得懂 Claude 選了什麼工具，你就反推得出來你的 Prompt 哪裡沒講清楚。

舉個實際的例子，假設你叫 Claude「幫我看一下登入邏輯」，結果它連續開了七八個 Glob 都沒找到，最後才用 Grep 搜到。如果你看不懂它在做什麼那只會覺得「怎麼這麼慢」，可是如果你知道 Glob 是用檔名 pattern、Grep 是用內容字串搜，你就會發現問題出在你的描述「登入邏輯」是個抽象概念，檔名根本不會直接寫 `login` ，下次你就會直接給它 `src/auth/` 這個路徑，事情瞬間變快。

另外一個原因是 Claude Code 的 Hook、Skill 的 `allowed-tools` 、permissions 的 deny 規則這些設定，全部都是建立在工具名稱上面的，你如果不知道哪些工具存在、各自做什麼，那這些進階設定根本沒辦法寫對。

## Claude Code 有哪些內建 Tools

先快速看一下整體有哪些，後面會挑常用的展開講。

| 分類 | 工具名稱 | 主要用途 |
| --- | --- | --- |
| 讀檔 | Read | 讀取檔案內容 |
| 讀檔 | Glob | 用檔名 pattern 找檔案 |
| 讀檔 | Grep | 用內容搜尋（底層是 ripgrep） |
| 改檔 | Edit | 對檔案做精確的字串替換 |
| 改檔 | Write | 整個檔案覆寫 |
| 改檔 | NotebookEdit | 修改 Jupyter Notebook 的 cell |
| 執行 | Bash | 跑 Shell 指令 |
| 執行 | PowerShell | 跑 PowerShell 指令（Windows 為主） |
| 執行 | Monitor | 背景執行指令並把每行輸出回傳給 Claude |
| 委派 | Agent | 派出 Subagent 處理任務 |
| 程式碼 | LSP | 透過 Language Server 做型別檢查、跳轉、找參考 |
| 任務 | TaskCreate / TaskUpdate / TaskList | 管理任務清單 |
| 互動 | AskUserQuestion | 主動問使用者多選題問題 |
| 計畫 | EnterPlanMode / ExitPlanMode | 進入或離開 Plan Mode |
| 排程 | CronCreate / CronDelete / CronList | session 內的排程任務 |
| 網路 | WebFetch | 抓網頁內容 |
| 網路 | WebSearch | 搜尋網路 |
| 其他 | Skill | 執行 Skill |
| 其他 | ToolSearch | 載入延遲載入的工具定義 |

看起來很多，可是日常你會大量看到的就是 Read、Glob、Grep、Edit、Write、Bash、Agent 這幾個，其他比較偶爾才會出現。接下來會挑這幾個重點工具來聊，最後再補充幾個值得認識的。

## 讀檔三劍客：Read、Glob、Grep

很多人剛開始用 Claude Code 都會搞不懂為什麼讀檔需要三個工具，明明就是讀檔不是嗎？其實這三個東西在做的事情完全不一樣，搞混了 Claude 就會選錯。

### Read

Read 是最直接的讀檔工具，給它一個絕對路徑就會把檔案內容回傳，而且回傳的時候會自動帶行號，長得像這樣：

```
1    function login(email, password) {
2      // ...
3    }
```

這個行號不是裝飾，後面 Edit 工具會用到，因為 Edit 需要精確的行內容才有辦法做替換。

Read 還有幾個特性要注意：

- 一次最多讀 2000 行，超過要用 `offset` 跟 `limit` 分段讀
- 可以讀圖片、PDF、Jupyter Notebook，圖片會用視覺方式呈現給 Claude
- 路徑必須是絕對路徑，不能寫相對路徑

那為什麼有些時候 Claude 不直接用 Bash 的 `cat` ？因為 `cat` 不會給行號，後續做 Edit 就會卡住，而且 `cat` 一次回所有內容沒辦法分段，大檔案直接灌爆 Context。所以官方的系統提示有特別寫「能用 Read 就不要用 cat」這條規則。

### Glob

Glob 是用檔名 pattern 找檔案，支援星號的通配符，像是這樣：

```
**/*.tsx          # 所有 tsx 檔
src/**/index.ts   # src 底下任何 index.ts
*.config.{js,ts}  # 根目錄的 config 檔
```

回傳結果會按照檔案的修改時間排序，最近改的會在前面。這個排序其實很實用，因為當你在 debug 某個功能的時候，最近改過的檔案通常就是你要排查的對象。

簡單來講 Glob 是你已經知道檔名長什麼樣子的時候用的。如果你要找的是包含某段內容的檔案，那就要用 Grep 了。

### Grep

Grep 底層是包裝 [ripgrep](https://github.com/BurntSushi/ripgrep) ，所以速度比 Bash 的 `grep` 快很多，而且支援完整的 Regex。

它有三種輸出模式：

- `files_with_matches` （預設）：只回傳有符合的檔案路徑
- `content` ：回傳實際匹配到的那幾行內容
- `count` ：回傳每個檔案匹配到幾次

以我自己來講最常用的是 `content` 模式搭配 `-n` （顯示行號）跟 `-C` （前後 context），這樣 Claude 一眼就能看到搜尋結果跟前後幾行的脈絡，不用再多開一次 Read 確認。

> **Note**  
> Grep 預設只搜單行，如果你要搜跨行的 pattern（像是「一個 function 開頭到第一個 return」這種），要設 `multiline: true` 。這個常被忽略，所以 Claude 在搜跨行結構時偶爾會找不到，記得提醒它一下。

### 三個工具的差異

聊完這三個工具後簡單總結一下時機：

- 你已經知道路徑：用 Read 直接打開
- 你知道檔名 pattern 但不知道在哪：用 Glob 找
- 你只知道內容裡有某些字串：用 Grep 搜

如果你下指令的時候直接點明這件事，例如「Grep 一下整個專案有沒有 `LEGACY_AUTH` 這個常數」，Claude 就不會浪費時間先 Glob 一輪，直接 Grep 出結果。

## 改檔的 Edit 跟 Write 不是同一件事

讀完接下來就是改了。Claude Code 提供兩個改檔的工具：Edit 跟 Write，很多人會以為這兩個差不多，其實差很多。

### Edit 是精確替換

Edit 的運作方式是傳入一段 `old_string` 跟一段 `new_string` ，Claude Code 會去檔案裡面找出 `old_string` 完全一致的位置然後換掉。重點在『完全一致』這四個字，連空白、縮排、換行符都要對。

也因為這樣 Edit 有一個重要的限制： **你必須先 Read 過那個檔案才能 Edit** 。沒讀過直接 Edit 會被擋掉，這是為了避免 Claude 用過時的記憶亂改檔案。

如果同樣的字串在檔案裡面出現多次，Edit 預設會直接報錯叫你提供更多上下文讓字串唯一。如果你就是想全部改掉，可以加 `replace_all: true` 。

### Write 是整檔覆寫

Write 就是直接把整個檔案重寫，路徑存在的話會把舊內容全部蓋掉。

聽起來很方便對吧？可是這個工具其實滿危險的，因為它會把你手動改過 Claude 沒讀到的內容全部清空。假設你開兩個視窗一邊在 Claude Code 一邊在 VSCode 改檔案，結果 Claude 用 Write 覆蓋，你剛剛在 VSCode 改的東西就直接消失了。

所以官方的系統提示會強烈建議「能用 Edit 就不要用 Write」，Write 大部分只用在「建立全新檔案」這個場景。

如果你發現 Claude 一直在用 Write 而不是 Edit，通常是因為它記憶中的檔案內容跟磁碟上的不一致（你中間自己改過），導致 Edit 怎麼配對都失敗，最後 fallback 到 Write。這時候叫它先重新 Read 一次再 Edit 通常就會好。

## Bash：什麼時候 Claude 會繞道用它

Bash 工具就是你想的那樣，跑 Shell 指令。可是 Claude Code 的系統提示會特別提醒它：能用內建工具就不要用 Bash。原因主要是 `cat` 、 `grep` 、 `find` 這類指令都有對應的內建版本，內建工具的 UX 會比較好，使用者要審查跟授權也比較直覺，回傳格式對 Claude 後續處理也更友善。

那 Claude 什麼時候還是會用 Bash 呢？大概有以下幾種情況：

- 你叫它跑測試 or 建置：例如 `pnpm test` 、 `pnpm build`
- 操作 Git： `git status` 、 `git diff` 、 `git log` 這類（Claude Code 把唯讀的 git 指令直接列為免授權）
- 啟動 dev server：例如 `pnpm dev`
- 跑套件管理指令： `pnpm install`
- 處理一些內建工具沒有的東西：例如複雜的 pipe（找出最大的五個檔案之類的）

Bash 還有一個 `run_in_background` 參數可以讓指令背景執行，這個對長時間任務很有用，像是跑 dev server or 長時間的 build，Claude 不需要呆等就可以繼續做別的事，背景指令完成之後它會被自動通知。

> **Note**  
> 這邊有一個有趣的細節： `Read` 跟 `Edit` 的 deny 規則只會擋內建工具，不會擋 Bash 子進程。也就是說你寫 `Read(./.env)` 進 deny，Claude 用 Read 讀 `.env` 會被擋掉，可是它跑 `cat .env` 還是會通過。如果要從 OS 層面真的擋住，需要去設 sandboxing。

## Agent：把任務丟給 Subagent

這是最多人不懂、用了威力卻最大的工具。

### Subagent 跟主對話差在哪

簡單來講，Agent 工具會開一個全新的 Subagent 去處理某個任務，這個 Subagent 有幾個特點：

- 有自己獨立的 Context Window，跟主對話完全不共享
- 完成任務後只回傳一個摘要結果給主對話
- 它看不到主對話的歷史，所以你要給它的指令必須完整自包含
- 每個 Subagent 都繼承父對話的權限再加上額外的工具限制
- 可以指定 Subagent 類型，後面會講

如果用比較生活面的例子來講的話，Subagent 就像你叫實習生去做事，你只給他任務描述跟所需的背景資料，他做完之後只回報結論給你，中間翻過幾本書、查過什麼資料你完全不會看到。

### 內建的三種 Subagent

Claude Code 預設提供三種內建 Subagent，每種的設計用途不太一樣：

| Subagent | 模型 | 工具權限 | 適合的場景 |
| --- | --- | --- | --- |
| Explore | Haiku（速度快） | 唯讀（不能 Write/Edit） | 大範圍搜檔、理解 codebase |
| Plan | 跟主對話相同 | 唯讀（不能 Write/Edit） | Plan Mode 用來研究專案的小幫手 |
| general-purpose | 跟主對話相同 | 全部工具 | 複雜任務需要邊讀邊改 |

從這張表可以看出來 Explore 是最常被自動觸發的，因為它便宜又快，而且只負責探索不會亂改檔案。Claude 在派 Explore 的時候還會指定一個 thoroughness 等級（quick、medium、very thorough），Prompt 越具體它就越能用低 thoroughness 完成任務、省更多 Token。

除了內建的之外你也可以在 `.claude/agents/` 底下定義自己的 Subagent，這就比較進階了，這篇就不展開講。

### 什麼任務適合丟 Agent

需要讀很多檔案才能得出結論的任務最適合丟 Agent：

- 「掃描整個專案，告訴我有沒有用到舊版的某個 API」
- 「分析這個功能的實作流程，從 router 一路追到 service」
- 「這個 codebase 哪裡有測試、哪裡沒測試」

如果這種任務你直接讓主對話做，那讀過的所有檔案都會塞到主 Context 裡面，後面的工作 Token 就所剩無幾。改用 Agent 派出 Subagent 之後，主對話只會收到一個精簡的摘要，Context 就保得住。

之前在 [你的 Claude Code Context 是不是都被吃光了？AGENTS.md 與 Context 管控全攻略](https://israynotarray.com/ai/20260409/1057131696/) 這篇我也有提到這個技巧，這邊再多補充一個觀念：你可以一次叫 Claude 派出多個 Agent 平行跑，例如「同時找出所有 API endpoint 跟所有 React component」，兩個 Subagent 會同時運作，比一個一個慢慢做快很多。

### 什麼任務不要丟 Agent

可是 Agent 也不是萬能的，有些情況不適合用：

- **需要來回討論的任務** ：Subagent 看不到主對話歷史，每次派任務都要把背景重打一次，需要反覆討論的事情用 Subagent 反而拖時間
- **目標已經很明確的單檔修改** ：直接 Read 跟 Edit 就好，丟給 Subagent 反而是繞遠路
- **任務本身需要主對話的脈絡** ：你又懶得把背景重打一次的話就直接讓主對話做，省事

> **Note**  
> 如果你不希望 Claude 自動派某種 Subagent，可以在 `permissions.deny` 加上 `Agent(Explore)` 這種規則來擋掉它，這個語法只會擋對應的 Subagent，其他 Subagent 還是能用。

## 其他偶爾會用到的

前面講的是日常會大量看到的，這邊再補幾個比較少出現可是值得知道的。

### WebFetch 跟 WebSearch

兩個工具名稱看起來很像可是用途不同。WebFetch 是給定一個 URL 把網頁抓下來轉成 Markdown，適合用在「我有一個官方文件 URL，請你照著做」這種場景。WebSearch 則是真的去搜尋關鍵字，適合用在「最新版本的某個 API 怎麼用」這種需要找資訊的場景。

如果你叫 Claude「查一下 Vue 3.5 的最新功能」，它通常會先用 WebSearch 找出官方文件的 URL，然後再用 WebFetch 把詳細內容抓下來，這是滿常見的組合技。

### AskUserQuestion

當 Claude 遇到不確定的選擇時，它有時候會主動跳出來問你問題，這時候用的就是 AskUserQuestion 工具。它是多選題形式的，比如你叫它建立一個專案可是沒講要用 React 還是 Vue，它就可能會先停下來給你幾個選項而不是自己猜。

如果你不希望它常常停下來問，可以在 CLAUDE.md 裡面寫清楚預設的選擇，這樣它就會自己決定不再彈出來問。

### LSP

LSP 是比較新的工具（透過 Language Server Protocol），適合做程式碼智慧的事情，像是跳轉到函式定義、找出某個 symbol 在哪裡被引用、列出某個 interface 的實作等等。比較有感的一點是 Claude 每次 Edit 完之後 LSP 會自動回報那個檔案的型別錯誤跟警告，所以 Claude 不需要再特別跑一次 build 就知道有沒有打錯字。

不過這個工具要先安裝對應語言的 Code Intelligence Plugin 才會啟用，沒裝的話 Claude 是看不到的。

### Monitor

Monitor 是讓 Claude 在背景跑一個指令，把每一行輸出即時回傳給它，適合用來：

- 盯著一個 log 檔，遇到 error 就跳出來提醒
- 監控 CI 任務的狀態變化
- 看某個資料夾有沒有新檔案產生

跟 `Bash` 的 `run_in_background` 差別在於 Monitor 是「持續餵資料給 Claude」，Bash background 則是「跑完才通知一次」。

### TaskCreate / TaskUpdate / TaskList

這幾個工具是 Claude 自己用來管理任務清單的，當任務比較複雜的時候它會先建立一份待辦，然後一項一項標記完成。你會在 Claude Code 介面看到 Claude 在拉清單，背後就是這幾個工具在運作。

這邊有個小細節：互動式 session（你日常用的 `claude` 指令）用的是 TaskCreate / TaskUpdate / TaskList 這組，至於 Headless 模式跟 Agent SDK 用的則是另一個叫做 TodoWrite 的舊工具，兩個概念上是同一件事不同實作。

## Claude 怎麼決定要用哪個工具

聊完工具本身，那 Claude 到底是怎麼決定要用哪個的呢？

簡單來講有兩個來源：

第一個是 **系統提示（System Prompt）** 。Claude Code 啟動時會載入一份很長的系統提示，裡面就明確寫了哪些工具優先、哪些工具有什麼限制，例如「能用內建工具就不要用 Bash」、「Edit 之前必須先 Read」這類規則。所以你可以理解成 Claude 的工具選擇邏輯有一部分是事先被教好的。

第二個是 **Claude 自己的判斷** 。模型會根據你的請求內容、目前對話的脈絡、Context 中的檔案資訊來推測哪個工具最適合。這部分就比較吃 Prompt 寫得清不清楚，Prompt 越模糊 Claude 越容易繞遠路。

舉個實際的例子比較好懂：

- 你說「打開 `src/auth/login.ts` 」 → Claude 直接 Read，因為路徑明確
- 你說「找一下登入邏輯」 → Claude 可能會先 Glob 找 `**/login*` or `**/auth/**` ，找不到再 Grep
- 你說「掃整個專案找用到 deprecated API 的地方」 → Claude 比較聰明的話會直接派 Explore Subagent

從這個角度看， **Prompt 給得越具體 Claude 用工具就越精準** ，這也是為什麼前面要建議大家學會看工具使用模式。

## 從工具使用模式反推 Prompt 哪裡寫差了

這部分是這篇最核心的東西，前面講的一堆工具其實都是為了讓你看懂這個。

### Claude 一直 Read 不同檔案找不到目標

通常代表你給的描述太抽象，它在猜檔案位置。例如你說「修一下首頁」，可是它根本不知道首頁是哪個檔案，就一直 Read 各種看起來像首頁的東西。

解法是 Prompt 直接給路徑 or 精確的描述，例如「修一下 `src/pages/Home.vue` 裡面的搜尋邏輯」。

### Claude 一直 Glob 都找不到

代表你描述的關鍵字跟實際檔名對不起來。例如你說「找登入相關的檔案」，可是專案實際是用 `Auth*` 命名的，Glob 一輪都找不到 `*login*` 。

解法是直接告訴它命名慣例，or 改用 Grep 搜 import 的字串會比較準。

### Claude Edit 一直失敗回去用 Write

如果你看到 Claude 試了幾次 Edit 都報找不到 `old_string` ，最後改用 Write，通常有兩個原因：

1. 你中間用 IDE 自己改過檔案，Claude 記憶中的內容跟磁碟不一致
2. 縮排 or trailing whitespace 不一致（特別是 tab vs space）

解法是叫它先重新 Read 一次再 Edit，or 你直接點明「請先 Read 一次最新內容再改」。

### Claude 不用 Subagent 自己硬讀很多檔

如果任務本來該用 Agent 派出去（例如全專案掃描），結果 Claude 自己一個一個 Read，那是因為它沒判斷出這是個大任務。

解法是你直接在 Prompt 裡面寫「請用 Explore subagent 幫我掃整個專案」，明確要求它派 Agent。

### Claude 一直繞道用 Bash

如果該用 Read 它跑去 `cat` 、該用 Grep 它跑去 `grep` ，多半是因為以下幾個原因：

- `permissions.deny` 把內建工具擋掉了，逼 Claude fallback 到 Bash（前面有提過 deny 規則只擋內建工具不擋 Bash）
- CLAUDE.md 裡面有不必要的引導指令
- 它要做的事情真的需要 pipe 組合，例如「找出最大的五個 log 檔」

前兩個情況用 `/permissions` 跟檢查 CLAUDE.md 就能修，第三個是合理的不用管。

## 總結

最後整理一下這篇的重點：

- Claude Code 內建 Tools 主要分成讀檔（Read、Glob、Grep）、改檔（Edit、Write）、執行（Bash）、委派（Agent）這四類，外加 LSP、Monitor、TaskCreate 等比較新的工具
- Read 必看行號，Edit 必先 Read，Write 盡量少用，免得蓋掉手動改的東西
- Glob 是找檔名 pattern，Grep 是找內容字串，搞清楚才不會選錯
- Agent 是最強大也最被忽略的工具，Claude Code 內建 Explore、Plan、general-purpose 三種 Subagent，需要讀很多檔案的任務丟給 Explore 最划算
- `permissions.deny` 的 Read/Edit 規則只擋內建工具，不擋 Bash 子進程，要從 OS 層擋要走 sandboxing
- 看懂 Claude 用什麼工具，就能反推你的 Prompt 哪裡沒講清楚

希望這一篇有讓你對 Claude Code 的內建 Tools 有更近一步的認識，下次它選錯工具的時候你就知道該怎麼調 Prompt 了哩～

你的支持會直接轉換成更多技術筆記

如果我的筆記讓你少踩一個坑、節省 Debug 的時間，  
也許你可以請我喝杯咖啡，讓我繼續當個不是 Array 的 Ray ☕

 [![buymeacoffee](https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)](https://www.buymeacoffee.com/israynotarray)| [![line](https://israynotarray.com/images/line-images.png) ](https://store.line.me/search/zh-Hant?q=israynotarray)| [![portaly](https://israynotarray.com/images/logo.b91a6dbf.webp)](https://portaly.cc/israynotarray/support)

Terminal

ray — -zsh — 80x24

Welcome to Ray Terminal v1.0.0

Type "help" to see available commands.

\------------------------------------

➜ ~