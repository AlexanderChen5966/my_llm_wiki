---
title: "Git Worktree 是什麼？一個專案同時跑多個 AI Agent 的秘訣"
source: "https://israynotarray.com/git/20260121/2025687513/"
author:
  - "[[Ray]]"
published: 2026-01-21
created: 2026-08-19
description: "Git Worktree 完整教學：Git 2.5 開始的功能，讓你在同一個 repo 開出多個工作目錄、各自停在不同分支，不用 stash、不用切分支就能平行開發。這篇示範怎麼用 worktree 一次跑多支 Claude Code Agent 處理不同功能，附 add、remove、merge 指令對照表與「拒絕合併無關歷史」解法。"
tags:
  - "clippings"
status: "processed"
---
目錄

![Git Worktree 是什麼？一個專案同時跑多個 AI Agent 的秘訣](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/Gemini_Generated_Image_dbbxwrdbbxwrdbbx.png)

Git Worktree 是什麼？一個專案同時跑多個 AI Agent 的秘訣

> [!note] Note
> Note
> 
> Git Worktree 是 Git 2.5 開始提供的功能，可以讓你在同一個 Repository 之下同時擁有多個工作目錄，每個目錄 checkout 不同分支。不用 stash 也不用切分支，搭配 AI Agent 同時跑不同任務超方便。

## 前言

相信許多人都已經在使用 AI Agent 來協助完成各種任務，但在實際應用上，我們總是手上會有很多需求，一個專案可能就要同時負責 N 個功能開發，這時候就會有一個疑問：「一個專案只能跑一個 AI Agent 嗎？」，所以這一篇就來聊聊一下該怎麼做到在一個專案中同時運行多個 AI Agent。

## 多個任務 AI Agent

廢話就不多說，我們直接切入正題，假設我們今天要製作一個 TodoList，這個 TodoList 會有四個主要功能：

在普通狀況下我們可能是先完成新增待辦事項的功能，接著再來完成查詢、更新、刪除等功能，所以 AI Agent 就會一個接著一個的跑，這樣雖然可以達成目標，但如果新增的功能很複雜的話，那是不是要跑很久才能接著往下一個功能去？

所以這邊就要介紹一個東西，也就是 `Git WorkTree` 。

這個是 Git 的一個功能，是在 `2.5` 時釋出的功能，簡單來講，他可以讓我們在同一個 Git 倉庫中，同時存在多個工作目錄，這樣我們就可以在同一個專案中，同時跑多個 AI Agent 來處理不同的任務。

那麼通常我們會輸入以下指令初始化專案 Git：

```bash
git init
git switch -c feature
```

![Git](https://images.israynotarray.com/blog/ai/ai-agent-worktree/20260121112418.png)

這時候就會停在 `feature` 分支，接著我們就可以使用 `git worktree` 來建立多個工作目錄。

`git worktree` 有兩種常見用法：

### 情境一：使用現有分支建立工作目錄

如果你已經有一個分支（例如 `feature` ），想為它建立一個獨立的工作目錄：

```bash
git worktree add <path> <existing-branch>
```

- `<path>` ：要建立的工作目錄路徑
- `<existing-branch>` ：現有的分支名稱

```bash
# 範例：為現有的 feature 分支建立工作目錄
git worktree add ../feature-worktree feature
```

### 情境二：建立新分支並同時建立工作目錄

如果你想建立一個新分支，並同時建立工作目錄：

```bash
git worktree add <path> -b <new-branch-name>
```

- `<path>` ：要建立的工作目錄路徑
- `-b` ：表示要建立新分支
- `<new-branch-name>` ：要建立的新分支名稱

```bash
# 範例：建立新的 feature-new 分支和工作目錄
git worktree add ../feature-new-worktree -b feature-new
```

> **關鍵差異** ：有 `-b` 是「建立新分支」，沒有 `-b` 是「使用現有分支」。

### 何謂工作目錄

看到這邊應該會對於工作目錄感到困惑，簡單來說，工作目錄就是我們平常在電腦上看到的專案資料夾。

假設你的專案結構長這樣：

```
/Users/ray/
└── my-project/       ← 你的主專案（main 分支）
```

當你執行 `git worktree add ../my-project-feature feature` 後，資料夾結構會變成：

```
/Users/ray/
├── my-project/           ← 主專案（main 分支）
└── my-project-feature/   ← 新的工作目錄（feature 分支）
```

這兩個資料夾 **共享同一個 Git 歷史** ，但可以 **同時切換到不同分支** 工作。你可以把它想像成：同一份專案開了兩個視窗，各自編輯不同版本，互不干擾。

> [!note] Note
> Note
> 
> 更白話一點其實就是 Copy 一份專案出來，不過這份專案是跟原本的專案共用同一個 Git 倉庫。

## 實戰示範

接著我們就來實際示範如何使用 `Git WorkTree` 來同時跑多個 AI Agent。

這邊就使用了剛剛建立的 TodoList 專案，並且針對每個功能建立一個工作目錄。

```bash
# 建立新增待辦事項的工作目錄，並建立新分支 feature-add
git worktree add ../todo-add -b feature-add
# 建立查詢待辦事項的工作目錄，並建立新分支 feature-query
git worktree add ../todo-query -b feature-query
# 建立更新待辦事項的工作目錄，並建立新分支 feature-update
git worktree add ../todo-update -b feature-update
# 建立刪除待辦事項的工作目錄，並建立新分支 feature-delete
git worktree add ../todo-delete -b feature-delete
```

接下來都輸入完畢後，我們就會在專案目錄的上層看到四個新的資料夾，分別對應到我們剛剛建立的工作目錄，以我的路徑來講是 `/Users/rayxu/GitHub/todolist-worktree/`

```
/Users/rayxu/GitHub/
├── todolist-worktree/        ← 主專案（main 分支）
├── todo-add/                 ← 新增待辦事項工作目錄（feature-add 分支）
├── todo-query/               ← 查詢待辦事項工作目錄（feature-query 分支）
├── todo-update/              ← 更新待辦事項工作目錄（feature-update 分支）
└── todo-delete/              ← 刪除待辦事項工作目錄（feature-delete 分支）
```

![工作目錄](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/20260121153351.png)

然後接著你就可以打開四個終端機視窗，分別切換到四個工作目錄

```bash
# 切換到新增待辦事項工作目錄
cd ../todo-add
# 切換到查詢待辦事項工作目錄
cd ../todo-query
# 切換到更新待辦事項工作目錄
cd ../todo-update
# 切換到刪除待辦事項工作目錄
cd ../todo-delete
```

接著就可以在各自的終端機輸入 `claude` 指令來啟動 AI Agent，並且針對各自的功能進行開發。

![claude 啟動](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/20260121153708.png)

接著底下我簡單四個都同時叫他們做「幫我建立一個 xxx.js 的檔案」

![各自運作](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/20260121153820.png)

接下來你可以回到各自的工作目錄中查看結果，你應該會看到各自的檔案都已經被建立完成，唯獨原本的 `todolist-worktree` 主專案資料夾中是不會有這些檔案的，因為這些檔案都是在各自的工作目錄中被建立的。

![結果展示](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/20260121154116.gif)

那接下來我們該怎麼把這些功能合併回主專案呢？你要先針對這些檔案進行 commit，接著再回到主專案中使用 `git merge` 來合併這些分支。

```bash
# 1. 在 worktree 中提交變更
cd ../todo-add
git add .
git commit -m "feat: 新增待辦事項功能"

# 2. 移除 worktree（釋放分支）
cd ../todolist-worktree
git worktree remove ../todo-add

# 3. 合併分支
git merge feature-add
```

> [!note] Note
> Note
> 
> 如果出現「致命錯誤: 拒絕合併無關的歷史」，代表你需要在合併時改成 `git merge feature-add --allow-unrelated-histories` 參數。因為正常來講，你必須要在建立 worktree 之前（todolist-worktree），主專案應該要先有一個初始 commit。

![合併過程](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/20260121155102.png)

如果你打開 commit 線圖，你會發現就跟你發 PR 一樣，會多出一個分支，然後 Merge 回主分支。

![線圖展示](https://images.israynotarray.com/blog/ai/ai-agent-git-worktree/20260121155213.png)

最後這邊也提供一下相關的 Git 指令給大家參考：

| 指令 | 說明 |
| --- | --- |
| `git init` | 初始化專案 Git |
| `git switch -c <branch>` | 建立並切換到新分支 |
| `git worktree add <path> <existing-branch>` | 使用現有分支建立工作目錄 |
| `git worktree add <path> -b <new-branch>` | 建立新分支並同時建立工作目錄 |
| `git worktree remove <path>` | 移除 worktree（釋放分支） |
| `git add .` | 將所有變更加入暫存區 |
| `git commit -m "message"` | 提交變更 |
| `git merge <branch>` | 合併分支 |
| `git merge <branch> --allow-unrelated-histories` | 合併無關歷史的分支（解決拒絕合併錯誤） |

## 延伸閱讀

Worktree 跟 AI Agent 真的是絕配，如果你想把這個組合玩得更深，可以接著看：

- [想用 Claude Code 開發？這篇帶你從入門到進階](https://israynotarray.com/ai/20260128/374686091/) ：Claude Code 完整入門與進階技巧，搭配 Worktree 效益最大化。
- [Claude Cowork Dispatch 與 Remote Control - 用手機遠端控制你的 AI Agent](https://israynotarray.com/ai/20260319/738324368/) ：多個 Worktree 加上手機遠端派發任務，並行開發體驗會差很多。