---
title: "Home 目錄一堆 .claude.json.tmp 檔案？可以刪嗎？（Claude Code 設定檔寫入解析）"
source: "https://israynotarray.com/ai/20260730/5308472196/"
author:
  - "[[Ray]]"
published: 2026-07-30
created: 2026-08-05
description: "打開 Home  目錄突然看到一整排 .claude.json.tmp.3188.1774732246449 這種檔案，其實這些是 Claude Code 在存 ~/.claude.json 設定檔時，寫入沒收尾留下的暫存檔殘骸，檔名由『設定檔路徑 + .tmp + PID + 毫秒時間戳』組成。這篇將會拆解 Claude Code 先寫暫存檔再 rename 的寫入流程、為什麼會留下 0 bytes 跟上百 KB 兩種殘骸、為什麼 cleanupPeriodDays 的自動清理永遠掃不到它們（因為它們住在 ~/ 而不是 ~/.claude/）、暫存檔沿用檔案權限帶來的隱私問題，還有 .claude.json.backup 與 ~/.claude/backups/ 這幾個檔案的差別跟清理指令。"
tags:
  - "clippings"
status: processed
---
目錄

![Home 目錄一堆 .claude.json.tmp 檔案是什麼？可以刪嗎？（Claude Code 設定檔原子寫入解析）](https://images.israynotarray.com/blog/ai/ithome-2026-vibe-coding/ChatGPT%20Image%202026%E5%B9%B48%E6%9C%883%E6%97%A5%20%E4%B8%8B%E5%8D%8804_24_48.png)

Home 目錄一堆.claude.json.tmp 檔案是什麼？可以刪嗎？（Claude Code 設定檔原子寫入解析）

## 前言

如果你用 Claude Code 已經有一陣子，某天打開 Home 目錄整理東西的時候，應該有機會看到一整排長得很像亂碼的檔案：

```
.claude.json.tmp.3188.1774732246449
.claude.json.tmp.7114.1774839016660
.claude.json.tmp.44948.1774775486853
.claude.json.tmp.89591.1778114908901
```

以我自己來講，前陣子清硬碟的時候一次看到 35 個這種檔案，雖然佔用的空間不多，但看了卻很煩，而且當下也不太敢刪，畢竟名字裡面帶著 `.claude.json` ，萬一它其實是設定檔的一部分呢？

但...其實這些檔案是可以刪除的，只是要刪除之前，我覺得搞懂它會比較好，搞懂它是誰生的、為什麼會留下來、為什麼 Claude Code 自己不會清，還有裡面到底裝了什麼，這樣下次再看到才不會猶豫 or 擔憂。

除此之外，之前我有寫過一篇 [Claude Code Auto Memory 一直佔用記憶體？清理與檢查完整教學](https://israynotarray.com/ai/20260602/4582937160/) ，那一篇聊的是 `~/.claude/` 資料夾裡面的對話紀錄怎麼吃掉你的硬碟，這一篇則是它樓上的鄰居，剛好可以一起看。

## 那一排檔案到底是什麼？

其實這些檔案正常來講不應該存在，因為這是 Claude Code 在儲存 `~/.claude.json` 時寫到一半留下來的暫存檔。

![Claude Code 亂數檔案](https://images.israynotarray.com/blog/ai/ithome-2026-vibe-coding/20260803160601.png)

Claude Code 亂數檔案

這些檔案名稱看起來像亂碼，但其實是有規則的，讓我把它拆成四段來看你會更清楚：

```
.claude.json.tmp.3188.1774732246449
```

拆開後，其實就對應底下表格：

| 這一段 | 它在講什麼 |
| --- | --- |
| `.claude.json` | 它正要覆蓋的目標，也就是 Claude Code 的全域設定檔 |
| `tmp` | 暫存檔標記 |
| `3188` | 當時產生這個檔案的 Claude Code 的 PID |
| `1774732246449` | 毫秒等級的 Unix 時間戳，記錄它是什麼時候被建立的 |

那 `1774732246449` 換算起來到底是什麼時候呢？把後面三位的毫秒去掉，再丟給 `date` 就知道了，macOS 的寫法是這樣：

```bash
date -r 1774732246 '+%Y-%m-%d %H:%M:%S'
# 2026-03-29 05:10:46
```

Linux 的話則是改用 `date -d @1774732246` 。

這邊有一個小地方可以順手驗證一下，你會發現換算出來的時間，跟 `ls -l` 看到的檔案修改時間是對得起來的。

也就是說光看檔名，你就可以知道它是哪一天、哪一個 Claude Code 留下來的東西。

## 為什麼存個設定檔還要多一個暫存檔？

其實這個問題的答案是為了避免把設定檔案寫成半殘，而這個做法有一個專業術語，就叫做「原子寫入」（atomic write）。

原子寫入？這什麼東西？

這邊我用比較生活面的例子舉例...假設你要把一份寫好的文件直接覆蓋掉舊版，最直覺的做法就是打開舊檔、清空、把新內容打進去。可是萬一打到一半電腦當機，那份檔案就變成半殘的了，新的沒寫完，舊的也回不來。

所以為了避免這種做法 Claude Code 採用「先寫暫存檔，再改名蓋掉舊檔」的做法，流程大概是這樣：

1. 先把完整的新內容寫進一個全新的暫存檔
2. 把原本設定檔的權限套用到暫存檔上
3. 用 `rename` 把暫存檔改名成 `~/.claude.json` ，一次蓋過去

它跑起來的樣子就像底下這樣：

```
[DEBUG] Writing to temp file: ~/.claude.json.tmp.73914.1766019293909
[DEBUG] Preserving file permissions: 100644
[DEBUG] Temp file written successfully, size: 103774 bytes
[DEBUG] Applied original permissions to temp file
[DEBUG] Renaming ~/.claude.json.tmp.73914.1766019293909 to ~/.claude.json
[DEBUG] File written atomically
```

這樣好處在於，假設在寫入的過程中被中斷了，舊的設定檔還在，新的暫存檔也還在，你可以再重新啟動 Claude Code，它就會再跑一次寫入流程，確保設定檔不會被破壞。

## 那為什麼還是會有殘骸留下來？

很簡單，因為跑到 `rename` 那一步之前，Claude Code 就被中斷了。

所以它就...變成了孤兒遺失在你的 Home 目錄下，大多常見狀況是這幾個：

- 直接把終端機視窗關掉
- 按 `Ctrl + C` 強制中止
- 電腦當機、強制重開機 or 強制登出

準備刪除之前，你也可以透過這些殘骸的大小了解它是死在哪一步：

| 殘骸大小 | 它死在哪一步 |
| --- | --- |
| 0 bytes | 檔案剛開好，內容還沒寫進去就被中斷，or 整台機器在資料真正落地之前就斷電了 |
| 80 KB 到 120 KB 不等 | 內容整份都寫完了，只差最後那一步 `rename` 沒跑到 |

以我自己那 35 個來講，20 個是 0 bytes、15 個是有內容的，比例大概六四分。

## 為什麼 Claude Code 自己不清掉？

這個答案是因為 Claude Code 只會清理 `~/.claude/` 底下的東西，而這些暫存檔殘骸住在上一層的 Home 目錄，所以才會清除不到，儘管內建有 `cleanupPeriodDays` （預設 30 天）這個自動清理機制。

> Files in the paths below are deleted on startup once they're older than `cleanupPeriodDays`. The default is 30 days.

官方也有處理過這件事情，只是版本是 v2.1.47 左右：

> Moved config backup files from home directory root to `~/.claude/backups/` to reduce home directory clutter

設定檔的備份從 Home 目錄根搬到 `~/.claude/backups/` ，但原有的 `.tmp` 殘骸就沒有搬走，所以才會一直留在那邊，這部份你就要自己動手清理了

## 怎麼檢查跟清掉

我們一步一步來，先確認你到底有幾個、佔了多少空間：

```bash
ls -lh ~/.claude.json.tmp.* | wc -l
du -ch ~/.claude.json.tmp.* | tail -1
```

接著把每一個的時間戳批次換算成看得懂的日期，順便排序，這樣你就知道最新的一個是什麼時候留下來的：

```bash
··
··

for f in ~/.claude.json.tmp.*; do
  ts=${f##*.}
  printf '%s  %s\n' "$(date -r $((ts/1000)) '+%Y-%m-%d %H:%M')" "$(basename "$f")"
done | sort
```

跑出來會像這樣：

```
2026-03-29 05:10  .claude.json.tmp.3188.1774732246449
2026-03-29 17:11  .claude.json.tmp.44948.1774775486853
2026-03-30 10:50  .claude.json.tmp.7114.1774839016660
2026-04-01 02:56  .claude.json.tmp.6106.1774983410009
```

確認過都是舊東西之後，如果你會擔心手滑，可以先用 `echo` 空跑一次，看它到底會動到哪些檔案：

```bash
for f in ~/.claude.json.tmp.*; do echo "rm $f"; done
```

確定沒問題就可以真的刪了：

```bash
rm -f ~/.claude.json.tmp.*
```

> [!note] Note
> Note
> 
> 這個萬用字元只會吃到 `.claude.json.tmp.` 開頭的檔案， `~/.claude.json` 本身跟 `~/.claude.json.backup` 都不在範圍內。 不過官方文件也有特別提醒不要刪掉 `~/.claude.json` ，因為你的登入狀態跟偏好設定都在裡面，所以自己手動下指令的時候還是看清楚再按 Enter 比較保險。

刪完之後 Claude Code 基本上是不會受影響的。

## 順便認識旁邊那幾個親戚

Home 目錄跟 `~/.claude/` 底下還有幾個名字長得很像的檔案，一起講清楚比較不會搞混：

| 名字 | 它在管什麼 | 可以刪嗎 |
| --- | --- | --- |
| `~/.claude.json` | 全域設定檔本體，登入狀態、偏好、個人 MCP 伺服器都在這 | 不要刪 |
| `~/.claude.json.tmp.<PID>.<時間戳>` | 這篇的主角，原子寫入沒收尾留下的殘骸 | 可以 |
| `~/.claude.json.lock` | 同一套寫入流程用的鎖檔，避免多個 session 同時寫壞設定 | 正常會自己消失，看到殘留的可以刪 |
| `~/.claude.json.backup` | 舊版本留在 Home 目錄根的設定備份 | 可以 |
| `~/.claude/backups/.claude.json.backup.<時間戳>` | 現在的備份機制，設定檔遷移前的快照 | 可以，官方文件把它歸在刪掉不會損失任何使用者看得到的東西那一類 |

希望這一篇有幫助你認識與了解 claude 設定檔的原子寫入流程，知道它為什麼會留下暫存檔殘骸、為什麼自動清理掃不到它們、以及怎麼安全地清理掉～

## 相關文章[閱讀時間約 2 分鐘](https://israynotarray.com/ai/20260727/3871780834/)

[Claude Code 中輸入 /model 找不到你要的模型？試試這小技巧吧！](https://israynotarray.com/ai/20260727/3871780834/)[

閱讀時間約 6 分鐘

AI 明明在雲端，為什麼開越多 Agent 電腦越 Lag？](https://israynotarray.com/ai/20260714/5005075303/)[

閱讀時間約 2 分鐘

新版 Claude Code 終端機攔截滑鼠點擊超煩！用 CLAUDE\_CODE\_DISABLE\_MOUSE\_CLICKS 環境變數把它關掉](https://israynotarray.com/ai/20260629/2995176290/)