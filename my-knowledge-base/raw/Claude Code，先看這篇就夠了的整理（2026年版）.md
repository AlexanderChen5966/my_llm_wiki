---
title: "Claude Code，先看這篇就夠了的整理（2026年版）"
source: "https://codelove.tw/@tony/post/qOZzea"
author:
published:
created: 2026-07-14
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------大家好，我是剛入職的新鮮人工程師 [fuyunoki](https://qiita.com/fuyunoki)。這..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月13日](https://codelove.tw/@tony/post/qOZzea "2026-07-13 17:15:11")

## Claude Code，先看這篇就夠了的整理（2026年版）

## 前言

大家好，我是剛入職的新鮮人工程師 [fuyunoki](https://qiita.com/fuyunoki) 。

這次因為在新鮮人入職的公司也能使用 Claude 了，所以我把自己調查整理的內容彙整成文。既然都整理好了，就順手公開成文章。希望也能幫助同樣遇到「公司可以用 Claude Code 了，但到底該從哪裡開始？」的新鮮人與年輕工程師。

老實說，一開始翻官方文件時，功能實在太多，看得我只想問「到底該先記住什麼啊……」。所以這篇不只整理了官方文件的內容，也一起蒐集了現役工程師的部落格、Anthropic 內部案例等資料，把實務上到底是怎麼用的也一併整理進來。

讀者對象正如標題所示，是「接下來要開始碰 Claude Code 的人」。反過來說，已經用得很熟的人，可能不會看到太多新鮮資訊。

---

## 1\. 先安裝

到了 2026 年，從官方文件來看，已經明確改成 **推薦使用原生安裝程式** 。透過 npm 安裝則被列為不建議（Deprecated）。原因大概是團隊內因 Node.js 版本差異導致行為略有不同，以及常踩到 `sudo npm install -g` 這類權限問題。我自己一開始也想用 npm 裝，結果稍微卡了一下，所以之後要裝的人，老老實實用原生安裝程式會比較好。

### Mac / Linux

```
curl -fsSL https://claude.ai/install.sh|bash
```

### Windows

打開 PowerShell 後直接執行以下指令即可（不需要系統管理員權限）。

```
irm https://claude.ai/install.ps1|iex
```

`irm` （ `Invoke-RestMethod` ）會從 Anthropic 的伺服器取得安裝腳本，接著用 `iex` （ `Invoke-Expression` ）直接執行，是一行搞定的安裝方式。不需要安裝 Node.js，會安裝到 `~\.local\bin\claude.exe` 。如果想讓它能使用 Bash 工具，建議另外安裝 Git for Windows；如果沒裝，則會把 PowerShell 當作 shell 工具使用。

如果你是用 CMD，可以使用以下方式（因為 `irm` 是 PowerShell 專用指令，不能在 CMD 裡直接用）。

```
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

如果想在 WSL 上使用，請在 WSL 的終端機內執行與 Mac/Linux 相同的 `curl` 指令。雖然原生 Windows 支援也持續在進步，但一開始不熟時，透過 WSL 使用通常資訊更多，也更穩定。

### 如果非得使用 npm

若是為了相容性，下面的方式也能用，但 **絕對不要加 `sudo`** 。如果出現權限錯誤，正確作法是用 nvm 把 Node.js 重新安裝到家目錄底下。

```
npminstall -g @anthropic-ai/claude-code
```

### 確認動作與首次啟動

```
claude --versionclaude
```

首次啟動時，瀏覽器會自動打開，並要求使用 Anthropic 帳號（Pro / Max / Teams / Enterprise / API Console）進行 OAuth 驗證。驗證 token 會本機儲存在 `~/.claude/` ，所以只要通過一次，之後就不需要再登入。

**需求規格參考** ：macOS 13 以上 / Ubuntu 20.04+、Debian 10+ / 安裝 WSL 的 Windows 10（1809+）、RAM 4GB 以上（大型程式碼庫建議 8GB）。實際運算會由 Anthropic 端伺服器處理，因此本機不需要 GPU。

---

## 2\. 初期設定：想用日文、想選模型

- Claude Code 對日文指示也能正常應對。最一開始加一句「請用日文回答」，或者寫進後面會提到的 [CLAUDE.md](http://claude.md/) ，之後就能穩定用日文回覆。
- 可以用 `/model` 指令切換模型。像是較重的設計判斷、複雜的除錯調查就用 Opus 系列；平常的實作工作用 Sonnet 系列；單純的大量重複工作則用 Haiku 系列。依照任務重量來切換，能在成本與速度兩方面都比較划算。這點其實很容易被忽略。

---

## 3\. CLAUDE.md ― 專案的「使用說明書」

[CLAUDE.md](http://claude.md/) 是 Claude Code 在工作階段開始時會自動讀取的設定檔。裡面寫的內容，之後整個對話都會當作前提知識。可以把它想成「第一次被分配到專案時，前輩交給你的資料」會比較好理解。

### 要放哪裡

| 位置 | 適用範圍 |
| --- | --- |
| `~/.claude/CLAUDE.md` | 全專案共用 |
| 專案根目錄的 `CLAUDE.md` | 該專案整體 |
| 子目錄（例如： `src/CLAUDE.md` ） | 該目錄底下 |

### 如果不知道該寫什麼，就用 /init

在專案根目錄執行 `/init` ，Claude Code 會分析程式碼庫，並自動產出適合該專案的 [CLAUDE.md](http://claude.md/) 雛形。如果你卡在不知道該寫什麼，那就先 `/init` ，再慢慢調整內容，通常會更快。

### 該寫什麼

- Claude 只看程式碼不一定能推測出的建置／測試指令
- 專案特有的架構與設計方針
- 團隊的程式碼規範（預設無法知道的部分）
- 測試執行步驟、推薦工具
- 常見陷阱與不直觀的行為（例如「只有這個目錄的建置步驟不同」）

### 相反地，不用寫的內容

- 看程式碼就知道的事
- 一般性的語言／框架規範
- 經常變動的 API 規格細節
- 像「寫出漂亮的程式碼」這種過於理所當然的方針

[CLAUDE.md](http://claude.md/) 每次都會整份進到 context，所以寫太多反而會變成雜訊。建議只保留重點，細部作業流程則拆到後面提到的 Skills 或自訂指令，等專案逐漸變大時會更有價值。

---

## 4\. 一直被問很煩的問題 → 用權限設定解決

Claude Code 預設在每次修改檔案或執行指令時，都會跳出 `[y/n]` 確認。雖然這是為了安全，但像 `git commit` 、 `npm install` 這種固定操作每次都被打斷，老實說節奏會很差。

可以用 `/permissions` 指令，針對工具或指令設定「永遠允許」「永遠確認」「永遠拒絕」。

```
# 例：永遠允許 git commit
claude allow"git commit"
# 例：永遠允許檔案編輯
claude allow edit
```

一開始先從最小權限開始，逐步把值得信任的固定操作（lint 執行、測試執行、git 系列指令等）加到允許清單裡，是比較安全的做法。反過來說， **破壞性操作（force push、資料庫操作、正式環境部署等）最好一路都不要自動核准** 。這裡一旦放太鬆，出事時的損害會很大，要特別小心。

---

## 5\. 基本流程：Explore → Plan → Implement → Commit

Anthropic 推薦的開發流程分成這 4 個階段：

1. **Explore（探索）** ：用 Plan Mode，先讀相關程式碼，掌握現況
2. **Plan（規劃）** ：維持 Plan Mode，把實作方針與變更範圍說清楚
3. **Implement（實作）** ：切換到 Normal Mode，依照計畫實作
4. **Commit（提交）** ：檢視差異，提交與建立 PR

直接丟一句「幫我實作」的所謂「vibe coding」，如果只是一次性原型還可以，但用在正式程式碼上很容易出事。尤其是跨多個檔案的變更，或是會大幅影響既有架構的修改， **先讓它寫計畫，再由人類審查後才開始實作** ，只多這一道流程，回頭修改的成本就會明顯下降。

當然，像是只改 1 個檔案、幾行字就結束的單純工作，也不一定要硬走 Plan Mode；依任務大小靈活切換會比較實際。

---

## 6\. Context Window 管理，這可能是最重要的

Claude Code 的效能會隨著 context window（對話記憶容量）被塞滿而下降。無論官方或非官方文章，大家幾乎都一致提到這點；我也覺得這大概是用好 Claude Code 最重要的原則。

### 常用指令

| 指令 | 功能 |
| --- | --- |
| `/clear` | 重設對話紀錄，把 context 清成 0 |
| `/compact` | 將對話摘要化，節省 context |
| `/rewind` | 回到先前的檢查點 |
| `/help` | 顯示可用指令列表 |

### 很常見的失敗模式

- **把「順便」的工作帶進來** ：某個任務進行到一半，插入不相關的問題，然後又回到原本任務。context 會被無關資訊塞滿，準確度下降。→ 在切換到不相關任務之前，務必先 `/clear` 。
- **一樣的錯誤反覆修** ：Claude 出錯後，你下修正指示，但它還是沒修好，接著又修正……這時候就是失敗嘗試的歷史污染了 context。→ **如果訂正兩次還是沒修好，就先 `/clear` ，把學到的內容加進新的 prompt 從頭再來** ，通常反而比較快。我自己也卡過幾次，這點很有感。

切換到不相關任務時，養成定期 `/clear` 的習慣會比較好。

---

## 7\. 用自訂斜線指令消除「每次都在打一樣的內容」

「每次都在輸入同一段指令」這件事，可以存成斜線指令。只要放一個 `.md` 檔，就能用 `/指令名` 呼叫。

### 放置位置

- 專案單位（團隊共用）：`.claude/commands/`
- 使用者單位（個人用、全專案共用）： `~/.claude/commands/`

檔名（不含副檔名）會直接變成指令名稱。像是建立 `review-pr.md` ，就可以用 `/review-pr` 呼叫，機制非常直觀。

### 記載範例

```
### Git 提交工具 (Git Commit Tool)

當 Claude 要在本機儲存庫中提交變更時，請依照以下定義與規則執行。

#### 允許的指令 (Allowed Tools)
\`git add <檔名>\`
\`git status\`
\`git commit -m "<訊息>"\`

#### 引數提示 (Argument Hint)
\`[message]\`：請指定提交訊息。

#### 指令說明 (Description)
將變更後的檔案加入暫存區，並使用指定訊息建立 Git 提交。

#### 執行指示 (Instruction)
建立提交時，請使用以下格式指定訊息並執行：
\`Create a git commit with message: $ARGUMENTS\`
```

`$ARGUMENTS` 可以接收執行指令時傳入的參數（也可以用 `$1` 、 `$2` 這種位置參數）。 `allowed-tools` 可以限制這個指令可使用的工具或命令，而 `description` 會顯示在輸入 `/` 時的候選清單中。

實務上，像是把「建立分支 → 提交 → 建立 PR」整個流程做成指令，或是先檢查程式碼規範再讓它提交，都是很常見的用法。

### 養成的技巧

與其一開始就做很複雜的指令，不如先把 **最近一週最常輸入的固定句子，做成第一個指令** 。據說照著「個人指令 → 專案指令（團隊共用）→ 外掛（跨多專案／跨團隊發佈）」這個順序發展，才不會做出一堆沒人用的指令。當指令數超過 20～30 個時，選擇時會開始有點負擔，所以建議用分類前綴整理好。

---

## 8\. Skills（技能）

把重複執行的工作整理成「預先定義好的流程」的機制。放在 `.claude/skills/<技能名稱>/SKILL.md` 。

```
---
name: {技能名稱}
description: {本技能的說明}
disable-model-invocation: true (避免 claude 自動執行)
allowed-tools: Bash, Read, Edit---
（技能內容與步驟）
---
```
- 指定 `disable-model-invocation: true` 後，Claude 不會自己自動觸發，只會在你手動明確呼叫時使用。
- 可以用 `allowed-tools` 限制可使用的工具。

和斜線指令的區別可以這樣理解：「被叫到就執行＝斜線指令」；「希望依照上下文自動觸發、或是需要附帶額外檔案／腳本的複雜工作＝Skill」。另外，根據目前官方文件，`.claude/commands/` 已被視為相容性用途的舊格式，`.claude/skills/` 才是逐漸被推薦的新方向。新建立的內容，使用 Skills 格式會比較穩妥。

---

## 9\. Hooks（鉤子）

這是一種會在特定事件（例如編輯檔案後、執行指令前後等）自動觸發 shell 指令的機制。設定檔放在 `.claude/settings.json` 。

常見用途包括：每次修改檔案後自動執行 Linter（例如 ESLint），或是在提交前固定跑一次規範檢查，讓 Claude 每次都被強制做那些可以機械化完成的檢查。這可以減輕人類的審查負擔。

---

## 10\. 子代理人

這是一種讓它在與主對話獨立的 context 中，專門執行特定任務的機制。優點是不會壓迫主 context window。

```
---
name: security-reviewer
description: 審查程式碼中的資安漏洞
tools: Read, Grep, Glob
model: opus
---
你是一位資深資安工程師。請從以下觀點審查程式碼：
- 注入類漏洞（SQL Injection、XSS、命令注入等）
- 驗證與授權不足
- 程式碼中嵌入的機密資訊／認證資訊
```

像「請用子代理人幫我做這段程式碼的資安審查」這種情境，把 **使用子代理人的意圖與指示內容明確寫進 prompt** ，通常比較容易照預期運作。延伸來看，也可以把「寫程式的 Claude」和「審查程式的 Claude」分成不同 session，也就是 Writer / Reviewer 模式。畢竟自己剛寫完的程式，總是容易看得比較寬鬆；用專門的、較新鮮的 review context，準確度通常會更高，這點也很符合人類的 code review 經驗。

---

## 11\. MCP（模型上下文協定）

這是用來連接外部應用與服務的機制。可以用 `/plugin` 打開外掛市集，裡面會把 Skills、Hooks、子代理人、MCP 伺服器等整合成一個發佈單位。

常見的整合對象如下：

- **GitHub** ：建立 PR、回覆 review comment、Issue 分流
- **Slack** ：自動發布工作完成通知或錯誤警報
- **PostgreSQL / MySQL** ：用自然語言執行查詢、確認 schema
- **Figma** ：讀取設計資料並反映到 UI

如果你使用的是型別語言，文件也提到加入偏向程式碼理解的外掛（像是符號導覽、編輯後自動檢查錯誤）會比較能提升 Claude 的準確度。

---

## 12\. 現役工程師的實踐經驗（自己加資料查詢分）

接下來整理一些網路上比較少提到，但我在調查時覺得「這個應該記起來」的小知識。

### 不要讓 review 變成「挑毛病」大會

如果只對 review 角色的 Claude 下「幫我找問題」這種指示，有時候它會回傳大量其實不影響實作的小地方。若把這些全部都修掉，反而容易導致不必要的抽象化或過度防禦性的程式碼，也就是所謂的 over-engineering。 **最好直接告訴它「只回報會影響正確性或需求的問題，其他一律視為可選項」** 。

### 分段提交不只是給人看的，也是給 Claude 的

如果按照任務步驟讓它分段提交，當 Claude 走偏時，人類和 Claude 都能很快回到前一個狀態。比起一次做出很大的變更再一起檢查，出事時的恢復速度會快非常多。

### UI 實作用「先看再修」的反覆流程很有效

有不少回饋都提到：先給它 mockup 圖，讓它實作；再把實作結果截圖，拿去跟 mock 比對並修正，這個流程重複 2～3 次，完成度會明顯提升。與其想一次到位，不如從一開始就以反覆調整為前提來做。

### 自動運轉（auto-accept）用在核心邏輯以外

那種跳過每次確認提示、讓 Claude 自主作業的「auto-accept mode」，在原型開發或周邊功能上很有用。Anthropic 內部也有案例是從乾淨的 Git 狀態開始，讓它自主工作，到八成左右再由人類做最後調整。不過涉及核心商業邏輯的修改，還是同步地給明確指示、一步一步來，會比較不容易出事。這可以當作「到底能放手到什麼程度」的一個參考。

### 用來新程式碼庫的上手導覽

像「這個 API 是怎麼設計的？」「這個功能最後一次是誰改的？」這種像在問 pair programmer 的問題，直接丟給 Claude Code 的用法越來越常見。它不只可以沿著 Git 歷史找出設計意圖與變更原因，而且幾乎不需要特別的 prompt 技巧，只要提問，Claude 就會自己去探索程式碼並回答。對於一個剛被丟進大型程式碼庫的新鮮人來說，這真的蠻實用的。

### 和 monorepo 的相容性

如果 schema、API 定義、實作都放在同一個儲存庫裡，Claude 會比較容易一次掌握整體 context，因此和 monorepo 架構相當合拍，這也是有人特別提到的優點。

---

## 13\. 遇到卡住的點

| 症狀 | 原因／對策 |
| --- | --- |
| `claude: command not found` | PATH 沒有設好。重新載入 shell 設定（例如 `source ~/.zshrc` ），或手動把 PATH 加上去 |
| `npm install -g` 出現權限錯誤 | 不要用 `sudo` 。改用 nvm 把 Node.js 安裝到家目錄底下，或直接切換成原生安裝程式 |
| 指令太多，開始不知道要選哪個 | 用分類前綴整理，沒人在用的指令就定期清理 |
| 同一個錯誤一直修不好 | 先 `/clear` ，再把從失敗中學到的內容加進新 prompt，重新開始 |
| review 結果太多，看不完 | 指示它只回報「會影響正確性與需求」的問題，縮小範圍 |

---

## 14\. 總結：最初 1 週要做的事清單

1. 用原生安裝程式完成安裝，啟動 `claude` 一次並完成認證
2. 在專案根目錄執行 `/init` ，建立 [CLAUDE.md](http://claude.md/) 雛形（不需要的內容刪掉）
3. 用 `/permissions` 把 lint、測試執行等安全操作加入允許清單
4. 先挑一個小任務，實際跑一次 Explore → Plan → Implement → Commit 的流程
5. 把這一週內「一直在重複輸入同一句話」的作業，做成一個自訂斜線指令
6. 切換到不相關任務時，養成先 `/clear` 的習慣

Claude Code 的功能很多，一開始難免會被大量資訊淹沒，但實際上常用的功能，往往會因專案性質而縮得很精簡。我自己的建議是：先抓住 [CLAUDE.md](http://claude.md/) 、權限設定、 `/clear` 這三個重點開始用，等真的有需要時，再把 Skills、Hooks、子代理人、MCP 一個個補上，這樣比較不容易半途放棄。

希望這篇對大家有幫助。如果有任何修正，歡迎留言告訴我。

## 參考文章

**官方文件・官方發信**

1. [Claude Code Best Practices - Claude Code Docs（官方文件）](https://code.claude.com/docs/en/best-practices)
2. [進階設定 - Claude Code Docs（安裝與更新方式）](https://code.claude.com/docs/ja/setup)
3. [Claude Code: Best practices for agentic coding（Boris Cherny 於 Anthropic 官方部落格的鏡像）](https://gist.github.com/jussker/e825980ed46af2b99318e19ef01083be)
4. [How Anthropic engineering teams use Claude Code every day - Codingscape](https://codingscape.com/blog/how-anthropic-engineering-teams-use-claude-code-every-day)

**個人・企業部落格（實務經驗・技巧）**

5. [用 Claude Code 把所有日常工作都加速到爆！ - Qiita（@minorun365）](https://qiita.com/minorun365/items/114f53def8cb0db60f47)
6. [給第一次接觸 Claude Code 的工程師的簡明入門 - Zenn（スタフェステックブログ）](https://zenn.dev/stafes_blog/articles/nano72mkn-claude-code-intro)
7. [【Claude Code 的活用案例】常用的 5 個自訂斜線指令！ - Findy Tech Blog](https://tech.findy.co.jp/entry/2025/07/23/070000)
8. [Claude Code 自訂斜線指令集 Part1 - ENECHANGE Developer Blog](https://tech.enechange.co.jp/entry/2025/07/11/120000)
9. [Claude Code：理解自訂斜線指令的作法 - BioErrorLog Tech Blog](https://www.bioerrorlog.work/entry/claude-code-custom-slash-command)
10. [Claude Code 安裝完整指南【2026】｜全 OS 對應・npm 不推薦對策 - 株式会社Uravation](https://uravation.com/media/claude-code-install-guide-2026/)

---

原文出處： [https://qiita.com/fuyunoki/items/5818688d20225aa8088a](https://qiita.com/fuyunoki/items/5818688d20225aa8088a)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qOZzea) [16瀏覽](https://codelove.tw/@tony/post/qOZzea)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qOZzea)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Anthropic 為何限制中國大陸使用 Claude？](https://codelove.tw/@tony/post/qO2P8a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️5

741

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

170

🥉

[流引色群@jptd847utpp](https://codelove.tw/@sp9863)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783990937&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqOZzea&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783990937202&amp;bpp=1&amp;bdt=94&amp;idt=56&amp;shv=r20260713&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783990683%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783990683%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783990683%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5738546163608&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396252&amp;oid=2&amp;pvsid=4225956933282167&amp;tmod=761720134&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=71" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260713/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>