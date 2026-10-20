---
title: "Codex快捷鍵教學！10組必學操作一次看，Quick Chat一鍵叫ChatGPT當軍師"
source: "https://www.bnext.com.tw/article/92234/codex-keyboard-shortcuts-quick-chat-guide"
author:
  - "[[李先泰]]"
published: 2026-09-14
created: 2026-09-15
description: "Codex併入ChatGPT桌面App後10組必學快捷鍵：⌘⌥N開一般對話再帶回Codex、⌘⌥S開支線討論，Review與多任務切換一次學。"
tags:
  - "clippings"
status: processed
---
Codex在2026年7月9日併入ChatGPT桌面App，Chat、Work、Codex三種模式擠進同一個視窗。很多人會在Codex下指令，卻還是靠滑鼠切模式、翻檔案，想找ChatGPT討論就在兩個視窗間複製貼上。

本文整理10組最值得記的macOS預設快捷鍵，先給速查表，再說明每組適合什麼情境。

| # | macOS | Windows／Linux | 官方功能名稱 | 用途 |
| --- | --- | --- | --- | --- |
| 1 | ⌘⌥N | Ctrl+Alt+N | Quick chat（ChatGPT only） | 開一般ChatGPT對話，討論完再加進Codex |
| 2 | ⌘⌥S | Ctrl+Alt+S | Open side chat（Codex only） | 不離開主任務，開一條支線討論 |
| 3 | ⌃⇧M | Ctrl+Shift+M | Open model picker | 切換模型 |
| 4 | ⌃1／⌃2／⌃3 | Alt+1／2／3 | Switch to Chat, Work, or Codex | 三種模式互切 |
| 5 | ⌘P | Ctrl+P | Search files（Codex only） | 搜尋專案檔案 |
| 6 | ⌃⇧G | Ctrl+Shift+G | Open review tab（Codex only） | 開Review分頁看修改 |
| 7 | ⌘⌥B | Ctrl+Alt+B | Toggle review panel（Codex only） | 開關側邊Review面板 |
| 8 | ⌃\` | Ctrl+\` | Toggle terminal（Codex only） | 開關內建終端機 |
| 9 | ⌘⌥A | Ctrl+Alt+A | Next chat needing attention（Codex only） | 跳到下一個待處理對話 |
| 10 | ⌘K | Ctrl+K | Open command menu | 搜尋功能與快捷鍵 |

> 編按：以上為預設快捷鍵，部分功能須在對應模式或畫面中使用；macOS 的 ⌃ 代表 Control、⌥ 代表 Option、⌘ 代表 Command。此外，標「Codex only」的鍵在Chat或Work模式按了沒反應是正常的。

## ⌘⌥N Quick Chat：怎麼把ChatGPT的討論帶回Codex？

要先建立一個觀念： **併入ChatGPT之後，Codex畫面裡有三種對話，用的鍵不同，額度規則也不能一概而論。** Codex主對話是你平常派任務的那一條。Quick Chat（⌘⌥N）依官方文件「開的是一般的ChatGPT對話」，不會出現在Codex側欄，而是進到ChatGPT的對話紀錄。Side Chat（⌘⌥S）則是官方標為Codex專用的支線對話。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

Quick Chat適合任務還沒想清楚、想先跟ChatGPT聊一輪的時候。按⌘⌥N拆需求、比方案、整理規格，這段討論不會塞進Codex的任務脈絡。

有結論後回到Codex的「New chat」，官方說明可以從這裡「開啟一個既有的ChatGPT對話，把它加進Codex對話」。白話說，你不必再複製貼上，直接要求Codex依討論結論執行。

## ⌘⌥S Side Chat：什麼時候該開支線而不是新對話？

Side Chat官方只標了一句「Codex only」，用途可以理解成「主任務跑著，我旁邊問一下」。依7月30日的桌面版更新紀錄，Chrome擴充功能還可以把網頁上畫起來的文字直接送進Side Chat。

兩者怎麼選？ **Quick Chat是離開Codex去找ChatGPT當軍師，Side Chat是留在Codex裡開會。** 想釐清方向、又不想污染任務脈絡，用⌘⌥N；任務已經在跑，只是想問這段程式碼或這份草稿為什麼這樣改，用⌘⌥S。

## Quick Chat用的是哪一池額度？

Threads上流傳「Quick Chat是省Codex額度的官方側門」， **官方文件能確認兩件事，但沒有「省額度」這個結論。**

OpenAI說明中心寫明Codex、ChatGPT Work、ChatGPT for Excel與Workspace Agents共用同一個額度與點數池，一般ChatGPT對話不在這份清單裡；而Quick Chat開的正是一般ChatGPT對話。換言之，⌘⌥N開出來的對話不走Codex那一池，但一般對話本身也有依方案訂的上限，能省多少沒有官方數字。

要留意的是另一個同名功能。Option+Space原本是ChatGPT的輕量Quick Chat快捷鍵，7月合併後彈出的視窗一度會接到Codex或Work任務，openai/codex的GitHub上有使用者回報因此耗掉大量額度。

9月11日更新後，這組鍵改為叫出「Pets」桌面小幫手的懸浮控制列與它的Quick Chat，從那裡開的對話不屬於任何專案，官方也沒有說明它走哪一池額度。本文說的Quick Chat一律指⌘⌥N。

## Review兩組鍵怎麼分工

⌃⇧G開一個獨立的Review分頁，適合一次檢查整批修改；⌘⌥B在目前工作畫面旁展開或收合Review面板，適合邊下指令邊對照差異。

官方文件提醒，Review面板要求專案位於Git儲存庫內，還不是的話App會提示你建立。它反映的是整個儲存庫的狀態，不只Codex改過的部分：預設顯示未暫存變更，切到「最後一輪」就只看Codex剛剛的改動。

在差異行上按「+」留言後，記得回到對話送出「請依留言修改」之類的指令，Codex才會把留言當成修改指引動手。

### 不碰滑鼠找東西？三組搜尋鍵各管一件事

⌘P是Search files，Codex專用，打檔名就能在專案裡定位並開啟檔案。⌃⇧M是模型選擇器，官方頁沒有把它限定在哪個模式。⌘K是命令選單，功能名稱、指令與對應快捷鍵都能搜，忘記某組鍵時先按它，⌘⇧P是同一個入口；但游標停在Terminal裡時，⌘K會變成「清除終端機」，這是官方頁明列的重疊。

### 同時跑好幾個任務怎麼不迷路？

⌃1、⌃2、⌃3依畫面順序切到Chat、Work、Codex。Windows版是Alt+1到3，因為Windows的Ctrl+1到9另有「跳到第幾個對話」的用途，不能直接把⌘換成Ctrl。⌘⌥A是Codex專用的「跳到下一個需要處理的對話」，多個任務同時在跑、有的等你核准時，連按它就能逐一巡過去。⌃\`開關內建終端機，想跑測試或看檔案清單不必切到外部視窗。

10組不用一次背完，可以先記⌘⌥N、⌘⌥S、⌘K這三組；如果忘了任何一組，按⌘/打開Keyboard Shortcuts設定頁，以你當下App顯示的為準。

資料來源： [OpenAI官方快捷鍵頁](https://learn.chatgpt.com/docs/reference/commands) 、 [OpenAI官方Projects and chats說明](https://learn.chatgpt.com/docs/projects) 、 [ChatGPT與Codex更新紀錄](https://learn.chatgpt.com/docs/changelog) 、 [OpenAI官方Code review說明](https://learn.chatgpt.com/docs/code-review) 、 [OpenAI官方Pets說明](https://learn.chatgpt.com/docs/pets) 、 [OpenAI說明中心Using Codex with your ChatGPT plan](https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan) 、 [openai/codex GitHub issue #31925](https://github.com/openai/codex/issues/31925)

本文初稿為AI編撰，整理．編輯/ 李先泰