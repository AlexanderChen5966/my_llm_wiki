---
title: "Git Worktree 完整教學：3 個 AI Agent 平行開發不打架"
source: "https://moksaweb.com/git-worktree-ai-agents/"
author:
  - "[[Moksa]]"
published: 2026-08-17
created: 2026-08-19
description: "Git Worktree 完整教學：讓多個 AI Agent 平行改 Code 不互蓋。同一個倉庫多個工作資料夾的原理、add 與 remove 完整指令、Claude Code 內建 worktree 隔離與五個常見的坑。"
tags:
  - "clippings"
status: "processed"
---
你開了兩個 Claude Code 視窗想同時趕兩個功能，結果 A 視窗剛改好的檔案，下一秒被 B 視窗整個蓋掉——這就是你需要 Git Worktree 的原因。Git Worktree 能讓同一個專案同時擁有好幾個獨立的工作資料夾：每個 AI agent 關在自己的資料夾裡改自己的分支，歷史卻共用同一份，誰也蓋不掉誰。這是本系列第 9 篇，也是多 agent 平行開發的核心技術。讀完這篇，你就能讓兩三個 agent 同時開工，自己還悠哉地在主資料夾看穩定版。

> 我第一次被「多視窗互蓋」雷到，是兩個 agent 同時改同一支 app.js：一個在加功能、一個在修 bug，最後存下來的版本兩邊都不完整，等於兩個都白跑。後來我養成一個習慣：只要打算開第二個 agent，第一件事就是先用 Git Worktree 幫它開一個獨立資料夾。多這一個步驟，跟之後省下的擦屁股時間相比，完全不成比例地划算。
> 
> Moksa 觀點

## 1\. 什麼是 Git Worktree？

Git Worktree 是 Git 從 2.5 版就內建的多工作目錄功能：同一個.git 倉庫，可以同時掛上好幾個工作資料夾（術語叫工作樹，working tree）。你原本的專案資料夾是「主工作樹」，用 `git worktree add` 開出來的是「連結工作樹」——每個工作樹各自站在一條分支上，各自有獨立的檔案狀態，但所有快照歷史都寫回同一個.git。

```
git worktree add ../myapp-feature -b feature-login
# Preparing worktree (new branch 'feature-login')
# HEAD is now at 3f2a1c9 stable homepage layout

ls ..
# myapp/  myapp-feature/
# two folders on disk, ONE shared history behind them
```

用遊戲來比喻：.git 是存檔主機，工作樹是接在同一台主機上的多個螢幕。每個螢幕可以開不同的關卡（分支），但所有存檔（commit）都寫回同一台主機，哪個螢幕都查得到。對 vibe coder 來說意義只有一句話： **一個 agent 一個資料夾，物理隔離，誰都蓋不掉誰** 。

## 2\. Git Worktree 和多開 clone 差在哪？

最直覺的替代方案是再 `git clone` 一份專案。做得到，但 Git Worktree 在三件事上完勝：第一，共用物件庫——clone 是把整份歷史複製一遍，工作樹只多出工作檔案，歷史零重複；第二，commit 即時互通——clone 之間要靠 push 和 pull 中轉，工作樹之間任何一邊 commit 完，其他資料夾立刻看得到；第三，集中管理—— `git worktree list` 一眼列出全部，clone 散落各處，Git 根本不知道它們彼此有關。

```
# the clone way: history gets duplicated on disk
git clone myapp myapp-copy
du -sh myapp/.git myapp-copy/.git
# 180M  myapp/.git
# 180M  myapp-copy/.git        <- full duplicate

# the worktree way: history stays shared
git worktree add ../myapp-feature -b feature-login
du -sh ../myapp-feature/.git
# 4.0K  ../myapp-feature/.git  <- just a pointer file back home
```

|  | 再 clone 一份 | worktree |
| --- | --- | --- |
| 歷史儲存 | 完整複製一遍 | 共用同一份 |
| commit 互通 | 要 push/pull 中轉 | 立即互相看見 |
| remote 設定 | 各自維護 | 跟主倉庫共用 |
| 建立速度 | 重新複製整個專案 | 幾秒鐘 |
| 集中管理 | Git 不知道彼此存在 | list 一眼列出 |
| 刪除善後 | 手動刪資料夾 | remove 一條指令 |

如果你是從本系列第 1 篇一路讀上來的，這一篇正式進入「多線平行」的階段；還沒補完前面存檔、退回、暫存基本功的人，完整路線在 [Git 教學完整地圖](https://moksaweb.com/git-tutorial-map/) 。

## 3\. 為什麼多個 AI Agent 需要它？

先講清楚災難是怎麼發生的。兩個 agent 在同一個資料夾工作時，檔案系統是共享狀態：A 讀了 app.js 開始改，B 同一時間也在改同一支檔案，後寫入的直接蓋掉先寫入的。更慘的是 Git 連救的機會都沒有——因為互蓋發生在 commit 之前，被蓋掉的版本根本沒進過快照，第 5 篇教的後悔藥（ [reset 救援](https://moksaweb.com/git-reset-vibe-coding/) ）在這裡完全無用武之地。

```
# a real collision, minute by minute
# 14:02  agent A reads app.js, starts a big refactor
# 14:03  agent B reads the SAME app.js to fix a small bug
# 14:07  agent B saves first - its fix hits the disk
# 14:09  agent A saves last  - and silently WIPES OUT B's fix
# git status shows one modified file; nobody notices until prod
#
# the worktree fix: physical separation
# agent A works in ../myapp-agent1  (branch feature-refactor)
# agent B works in ../myapp-agent2  (branch fix-login-bug)
# two folders, two branches, zero contact
```

另一個常見情境甚至不需要第二個 agent：你想讓 agent 放手大改一個功能，同時自己要開著穩定版 demo 給客戶看。同一個資料夾做不到「一邊大改、一邊穩定」，切分支也沒用——一個資料夾同一時間只能站在一條分支上，一切過去，整個資料夾的檔案就全變了。工作樹把「一條分支一個資料夾」變成可能，這件事分支本身做不到。

## 4\. Git Worktree 4 步驟開出第二個工作樹

拿你手上任何一個有 Git 版本庫的專案，跟著做一次，五分鐘內可以完成：

### 用 add -b 開出新工作樹

在專案資料夾執行 git worktree add../myapp-agent1 -b feature-login，Git 會在上一層開出 myapp-agent1 資料夾，並建立新分支 feature-login。資料夾名稱建議帶上 agent 編號或功能名，之後開多個才不會亂。

### 進入資料夾安裝依賴

新工作樹只包含被 Git 追蹤的檔案，node\_modules 和.env 都不在裡面。cd 進去之後跑 npm install（Python 專案就重建 venv），再把.env 手動複製一份進來。

### 指派 AI agent 進駐

在這個資料夾開一個新的終端機視窗啟動 Claude Code，明確告訴它只負責這個功能。另一個 agent 留在原本的資料夾，兩邊的檔案從此互不干擾。

### 完工後合併並移除

agent 的成果驗收滿意後，回到主資料夾用 git merge feature-login 把分支收進來，再執行 git worktree remove../myapp-agent1 清掉工作樹，合併完的分支也可以順手刪除。

完整跑一次長這樣：

```
# step 1: create the worktree on a new branch
git worktree add ../myapp-agent1 -b feature-login
# Preparing worktree (new branch 'feature-login')

# step 2: set up the fresh checkout
cd ../myapp-agent1
npm install
cp ../myapp/.env .env      # env files do NOT follow automatically
                           # (gitignored files never do - see Day 3)

# step 3: start your agent HERE, in its own folder
claude

# step 4: when the work is approved, harvest and clean up
cd ../myapp
git merge feature-login
git worktree remove ../myapp-agent1
git branch -d feature-login
```

## 5\. 完整指令：add、list、remove、prune

Git Worktree 日常會用到的就四個子指令，全部列在這裡：

```
# add: create a worktree (with -b to also create a new branch)
git worktree add ../myapp-agent1 -b feature-login

# add from an EXISTING branch: just omit -b
git worktree add ../myapp-hotfix fix-issue-456

# list: see every worktree and which branch it holds
git worktree list
# /Users/you/myapp          3f2a1c9 [main]
# /Users/you/myapp-agent1   3f2a1c9 [feature-login]
# /Users/you/myapp-hotfix   9d04e2b [fix-issue-456]

# remove: delete a worktree you are done with
git worktree remove ../myapp-agent1
# refuses if there are uncommitted changes; add --force to override

# prune: clean up metadata after a folder was deleted BY HAND
git worktree prune
# tip: git worktree list --porcelain gives script-friendly output
```

還有兩個新手必撞的護欄，先知道就不用撞：

```
# guardrail 1: one branch = one worktree, enforced
git worktree add ../another feature-login
# fatal: 'feature-login' is already used by worktree
# at '/Users/you/myapp-agent1'
# (this is the anti-collision feature working as intended)

# guardrail 2: main repo cannot switch onto a branch
# that a worktree currently holds
git switch feature-login
# fatal: 'feature-login' is already used by worktree...
# finish or remove the worktree first, then switch

# naming convention that keeps three agents sane:
# ../myapp            main (you, the reviewer)
# ../myapp-agent1     one folder per agent, numbered
# ../myapp-agent2     folder name != branch name is fine
# ../myapp-hotfix     purpose-named for one-off fixes
```

remove 和 prune 的分工要搞清楚：remove 用在「工作樹還在，我要正常退場」，它會連資料夾帶管理資料一起清乾淨；prune 用在「資料夾已經被手動刪掉、Git 裡的登記資料還留著」的善後。另外有一條硬限制： **同一條分支不能同時被兩個工作樹 checkout** ，硬試會直接被拒絕：

```
git worktree add ../myapp-two main
# fatal: 'main' is already checked out at '/Users/you/myapp'

# the right workaround: give every worktree its OWN branch
git worktree add ../myapp-two -b main-experiment

# --force can override the safety check, but two folders
# writing to the same branch is asking for chaos - avoid it
```

## 6\. Vibe Coder 的 Git Worktree 多 Agent 佈局

指令會了，接著是佈局。命名慣例很簡單：主資料夾維持原名，工作樹用「原名加後綴」放在同一層，好找也好刪。Git Worktree 佈局的核心原則只有一條： **主資料夾永遠保持乾淨，不派 agent 進駐，專門用來 review 和 merge** 。

```
# the layout: one folder per agent, one branch per folder
projects/
├── myapp/            # main - clean, review and merge ONLY
├── myapp-agent1/     # branch feature-login  (Claude session 1)
└── myapp-agent2/     # branch fix-payment    (Claude session 2)

# set it up in two commands
git worktree add ../myapp-agent1 -b feature-login
git worktree add ../myapp-agent2 -b fix-payment
# then: one terminal per folder, one agent per terminal
```

「一個工作樹、一個 agent、一條分支」是鐵律，三者永遠一對一。收成果的時候人在主資料夾：先用 diff 看每個 agent 到底改了什麼（看不懂就照 [第 6 篇的驗收流程](https://moksaweb.com/git-diff-review-ai/) 丟給 AI 翻譯），滿意就 merge 整條分支收進來；只想要其中一兩個改動，用 cherry-pick 挑著撿即可。commit 訊息記得讓 agent 自己寫清楚， [第 7 篇](https://moksaweb.com/ai-commit-message/) 教過的規矩在多 agent 場景會加倍回報你——因為你要一次看懂三條分支的歷史。

```
# harvest time: you sit in the CLEAN main folder
cd ../myapp
git diff main..feature-login --stat
# src/login.js   | 120 ++++++++++++++
# src/routes.js  |   8 ++
# 2 files changed, 128 insertions(+)

git merge feature-login          # take the whole branch
git cherry-pick 9d04e2b          # ...or take just one commit

# then retire the finished worktree
git worktree remove ../myapp-agent1
git branch -d feature-login
```

最後把紀律寫進設定檔，讓 agent 自己遵守，你就不用每次口頭交代：

```
# CLAUDE.md - rules for a multi-agent worktree project

## Worktree discipline
- You are assigned to THIS folder and THIS branch only
- Never cd into ../myapp or any sibling worktree
- Commit early and often on your own branch
- When the task is done, stop - the human reviews and merges from main
```

## 7\. Claude Code 的內建 worktree 隔離

如果你的 agent 是 Claude Code，上面整套流程官方已經內建。啟動時加 `--worktree` （縮寫 `-w` ）帶一個名字，它會自動建工作樹、開分支、把整個 session 關進去：

```
claude --worktree feature-auth
# creates .claude/worktrees/feature-auth/ at the repo root
# on a new branch named worktree-feature-auth

claude --worktree
# no name given: Claude picks one like bright-running-fox

# open a second terminal, different name = fully isolated session
claude --worktree fix-payment

# on exit, Claude inspects the worktree for you:
# clean and unnamed -> removed automatically, branch included
# has work in it    -> you choose: keep it or remove it
```

幾個官方文件寫明的行為：工作樹預設放在專案根目錄的.claude/worktrees/ 底下（記得把這個路徑加進.gitignore）；對話中直接說「開一個 worktree 來做」也可以，Claude 會自己建；離開 session 時它會檢查工作樹——乾淨的沒命名工作樹自動連分支一起清掉，有成果的會先問你要保留還是刪除。給 gitignored 檔案的解法也有：在專案根目錄放一個.worktreeinclude，列出的檔案會自動複製進每個新工作樹：

```
# .worktreeinclude - gitignored files to copy into every new worktree
.env
.env.local
config/secrets.json
```

更進一步，subagent 也能隔離：在自訂 subagent 的設定檔加上 `isolation: worktree` ，那個子代理每次執行都會拿到自己的暫時工作樹，跑完沒有改動就自動清除。這跟第 4 篇講的 [checkpoint](https://moksaweb.com/claude-code-checkpoint-vs-git/) 是不同層的保護：checkpoint 管「時間軸上的反悔」，工作樹管「空間上的隔離」，兩個可以同時用。不是用 Claude Code 的人也不用羨慕，照第 4 節的手動流程做，效果完全一樣，只是自動化程度差一點。

```
# .claude/agents/refactorer.md
# this subagent ALWAYS runs inside its own temporary worktree
---
name: refactorer
description: Applies mechanical refactors across many files
isolation: worktree
---

Apply the requested refactor across every affected file,
then run the tests and report the results.
```

## 8\. 五個常見的坑與解法

- **坑一：依賴不會跟過去。** 新工作樹只有被 Git 追蹤的檔案，node\_modules、venv 這類 gitignored 的東西每個工作樹要各自重裝。開完工作樹的第一件事永遠是裝依賴，不然 agent 一跑測試就滿江紅。
- **坑二：.env 也不會跟過去。** 金鑰設定檔同樣是 gitignored（第 3 篇教過為什麼），要手動複製一份進去，Claude Code 使用者用.worktreeinclude 自動化。
- **坑三：IDE 開錯資料夾。** 兩個資料夾長得一模一樣，最常見的悲劇是你以為在改 agent1 的分支，其實 IDE 開的是主資料夾。把終端機和編輯器的視窗標題確認清楚再動手。
- **坑四：直接刪資料夾不等於 remove。** 手滑用檔案總管把工作樹資料夾刪了，Git 內部的登記資料還在，之後會出現各種怪錯誤。補救方法是跑一次 prune 清掉孤兒登記。
- **坑五：同一條分支不能兩處 checkout。** 第 5 節講過的硬限制，正確解法永遠是幫每個工作樹開自己的分支，不是 –force。
```
# pitfall 4 demo: you deleted a worktree folder by hand
git worktree list
# /Users/you/myapp          3f2a1c9 [main]
# /Users/you/myapp-agent1   3f2a1c9 [feature-login] prunable

git worktree prune
git worktree list
# /Users/you/myapp          3f2a1c9 [main]
# clean again - and branch feature-login still exists
```
```
# pitfalls 1 and 2: the "new worktree ritual" - run it every time
cd ../myapp-agent1
npm install                  # deps do not follow you here
cp ../myapp/.env .env        # secrets are gitignored, copy by hand
ls node_modules | head -n 3  # sanity check before the agent starts
# .bin/
# accepts/
# acorn/
```

## 9\. 哪裡可以找到更多官方資源？

影片學習派可以搭配這三支：David Ondrej 的「Git Worktrees Clearly Explained (and how to use them)」直接從 AI 協作的角度切入、Redhwan Nacef 的「Git Worktree Tutorial | The Most Underrated Git Command?」把每個子指令逐一演示、The Productive Dev 的「Learn git worktree in under 15 minutes」十五分鐘完整走一遍日常流程。都是英文影片，開 YouTube 自動翻譯字幕即可，或把看不懂的段落截圖問 Claude。

![Youtube video](https://i.ytimg.com/vi/JVM40liIElk/hqdefault.jpg)

Youtube video

![Youtube video](https://i.ytimg.com/vi/4_p1OdLeDLE/hqdefault.jpg)

Youtube video

![Youtube video](https://i.ytimg.com/vi/9mTcpRmkTA4/hqdefault.jpg)

Youtube video

## 10\. 使用 Git Worktree 有哪些限制和注意事項？

誠實說清楚代價。第一，磁碟空間：雖然歷史共用，但工作檔案是實體複製，三個工作樹就是三份原始碼加三份 node\_modules，前端專案很容易一個資料夾就一兩 GB。第二，心智負擔：三個資料夾、三個終端機、三條分支，你要隨時記得自己人在哪裡——管理不好的話，搞混資料夾的成本可能比當初互蓋檔案還高。第三，含 submodule 的專案支援不完整，官方文件明講不建議多工作樹並用。

```
# the honest disk bill for a typical frontend project
du -sh myapp myapp-agent1 myapp-agent2
# 1.2G  myapp          <- mostly node_modules
# 1.1G  myapp-agent1   <- same deps, installed again
# 1.1G  myapp-agent2   <- and again
# the .git history is shared once - dependencies are not
```

更重要的是知道什麼時候根本不需要開工作樹。一次只跑一個 agent 的人，維持第 1 篇教的小步 commit 節奏就足夠了；寫到一半臨時被插單，用 [第 8 篇的 stash](https://moksaweb.com/git-stash/) 遠比開一個工作樹快。判斷式就一句話： **有兩件以上的改動需要「同時」進行才開工作樹** ，否則它只是幫你多養幾個要餵依賴的資料夾。工具是為了解決痛點，不是為了展示技巧。

## 11\. 參考依據

- [git worktree 官方文件（git-scm.com）](https://git-scm.com/docs/git-worktree) ——add、list、remove、prune 所有行為的最終依據
- [Claude Code worktree 官方文件](https://code.claude.com/docs/en/worktrees) ——本文 –worktree、.worktreeinclude、自動清理行為的出處
- [Claude Code subagent 官方文件](https://code.claude.com/docs/en/sub-agents) ——isolation 設定的完整說明
- [Pro Git 繁體中文版](https://git-scm.com/book/zh-tw/v2) ——分支模型的原理，看懂它工作樹就通了
- [Git 官方文件總覽](https://git-scm.com/doc) ——其餘指令的查詢入口

延伸閱讀：本站的 [Claude Code 系列](https://moksaweb.com/category/claude-code/) 有 140 篇以上的實戰教學，多 agent 工作流的細節都在裡面；整條 30 天學習路線見 [Git 教學完整地圖](https://moksaweb.com/git-tutorial-map/) 。

## 12\. 常見問題 FAQ

### Git Worktree 會佔用很多硬碟空間嗎？

比多開 clone 省很多。所有工作樹共用同一個物件資料庫，歷史完全不重複儲存；真正佔空間的是工作檔案本身，以及每個資料夾各自安裝的 node\_modules 這類依賴。開兩三個工作樹對現代硬碟來說負擔不大。

### Git Worktree 需要多新的 Git 版本？

這個功能在 2015 年的 Git 2.5 就加入了，remove 和 move 子指令則要 2.17 之後才有。只要你的 Git 是近幾年安裝的就能直接用，不確定的話跑一次 git –version 確認。

### 兩個工作樹可以同時 checkout 同一條分支嗎？

不行，Git 會回報 already checked out 直接拒絕。這是保護機制：兩個資料夾同時改同一條分支，狀態必亂。正確做法是用 -b 幫每個工作樹開一條自己的分支；用 –force 硬闖技術上可行，但強烈不建議。

### 不太會下指令的人能用 Git Worktree 嗎？

可以。你可以直接叫 Claude Code 幫你開好工作樹再開工，也可以啟動時加 –worktree 讓它全自動處理：建資料夾、開分支、結束時檢查要不要清掉，一行指令包辦整個流程。

### 在工作樹裡 commit，主資料夾看得到嗎？

看得到，而且是即時的。所有工作樹共用同一份歷史，任何一邊拍下快照，其他資料夾用 git log 馬上查得到，不需要 push 或 pull 中轉。這正是它比多開 clone 方便的關鍵。

### 幾個 agent 同時開工才值得用 Git Worktree？

兩個以上同時動工就值得開。如果一次只跑一個 agent，照舊在原資料夾用小步 commit 的節奏就夠，多開工作樹只是徒增管理成本。平行需求真正出現的那一刻再開，是最省心的原則。

### 移除工作樹之後，分支還在嗎？

還在。git worktree remove 只刪掉工作資料夾和對應的管理資料，分支和上面的 commit 都完好無缺。確認成果都合併回主線之後，再用 git branch -d 把不需要的分支刪掉即可。

### 有了工作樹還需要 stash 嗎？

需要，兩者解決的問題不同。工作樹處理「多條線同時進行」的平行問題，stash 處理「同一條線臨時被打斷」的暫存問題。日常單線開發被插單，用 stash 還是比開一個新工作樹快得多。