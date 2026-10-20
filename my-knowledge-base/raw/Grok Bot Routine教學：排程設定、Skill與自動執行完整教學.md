---
title: "Grok Bot Routine教學：排程設定、Skill與自動執行完整教學"
source: "https://www.bnext.com.tw/article/92295/grok-bot-routine-schedule-ai-news-briefing"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-09-18
created: 2026-09-18
description: "Grok Bot排程怎麼設定？本文以AI新聞晨報實測Routine功能，3步驟設定每天早上8點自動執行，並整理Bot、Skill與Routine差異及"
tags:
  - "clippings"
status: processed
---
每天早上進辦公室，先開五、六個網站，看過去一天有哪些產業新聞，再挑出值得追的幾則。這件事你做過幾百次，流程幾乎一樣，只有內容每天不同。

如果流程都一樣，能不能讓AI在你進辦公室前就先做完？Grok Bot的Routine（排程）功能就是為這種事設計的： **設定好任務和時間，之後它會準時自己開跑。**

本文用「每周一至周五早上8點自動開跑AI新聞晨報」當範例，以桌面版操作為主；手機App只能暫停或恢復排程。還沒建過Bot的讀者，先看《數位時代》8月的 [Grok Bot新手教學](https://www.bnext.com.tw/article/91991/grok-bot-getting-started-60) ，本文從「已經有一個Bot」開始。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## Grok Bot Routine是什麼？排程功能跟一般任務差在哪？

白話版：一般任務是你叫一次、它做一次；Routine是你設定一次、它之後照表跑。官方文件的定義是，Routine把一個工作流程指派給一個Bot，並告訴它何時執行，可以是固定時間，也可以是收到指定訊息後自動開始，例如某個Slack頻道出現特定關鍵字。

**Routine解決的是「這件事我不想每天重新交代」。** 它不會讓Bot變聰明，只是把你已經確認做得對的事，固定下來重複做。

## Grok Bot的Bot、Skill、Routine有什麼差別？

官方文件給的順序也值得記住。先用一次性任務做到結果可靠，把方法存成Skill，最後才排程自動化。跟帶新人一樣，先看它做對一次，再放它自己跑。 下面三步就照這個順序走。

## Grok Bot排程怎麼設定？3步驟建立Routine

### 第1步：先讓Grok Bot手動執行一次

打開負責這件事的Bot對話，直接貼上任務：

```
搜尋過去24小時內OpenAI、Anthropic、Google、Meta、xAI這五家公司的重要消息，挑出最值得關注的5則。每則依序寫出：發生什麼事、為什麼重要、發布日期、原始來源連結。沒有可靠來源的消息不要收錄；不足5則就照實說，不要湊數。
```

**接下來觀察三件事** ：來源是不是官方或主流媒體、有沒有混進過期新聞、格式好不好讀。不滿意就在同一段對話補一句修正，例如「不要收錄產品降價的消息」，再跑一次。排程只是把同一件事定時重做，第1步沒磨好，錯誤會每天準時出現。

![圖一：Grok運作畫面](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/pc4v-1789640775.jpg?w=2000&output=webp)

Grok Bot接收到指令後，大概在 30 秒以內就找到了對應的新聞內容。

圖／ Grok Bot

### 第2步：把固定工作流程存成Skill

結果穩定之後，對Bot說：

```
把我們剛才用的流程存成一個skill，名稱叫「AI新聞晨報」。內容要包含：搜尋範圍、篩選規則、輸出格式，以及找不到可靠來源時的處理方式。
```

官方文件建議Skill要寫到能獨立執行：需要哪些權限、怎麼驗證結果、哪些事要先問你。存好之後，在桌面版輸入框打「/」就能叫出來。

### 第3步：建立Routine，設定執行時間與頻率

可以直接在對話裡告訴Bot，也可以在Bot對話的Routines面板加排程，選項有每小時、每天、周一至周五、每周、每月等。用對話最省事：

```
建立一個routine：每周一至周五台灣時間早上8點開始執行，用「AI新聞晨報」這個skill搜尋過去24小時的AI新聞，整理成5則晨報，直接貼在這個對話裡。不要寄信、不要發到Slack。如果搜尋失敗或來源無法讀取，請說明原因，不要回覆沒有新聞。如果搜尋成功但沒有合適的消息，回覆「本次未找到符合條件的新聞」，不要湊數。
```

![圖二：建立 Routine](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/ecp2-1789640842.jpg?w=2000&output=webp)

建立 routine 時注意，要跟 Grok Bot 說清楚執行時間，還有必要的規則以及限制。

圖／ Grok Bot

這段話裡有五件事，官方文件說每個Routine都要講清楚： **什麼時候跑（含時區）、資料從哪來、要交出什麼、哪些事要先問你、搜不到或讀不到時怎麼辦。**

另外， **時區與假日要自己顧。** 排程用的是設定裡「Timezone」那一欄的時區，預設跟著你的電腦，不是它去讀的網站或Slack的時區。建好後到設定確認時區，也對一下下一次執行時間。選「周一至周五」不代表會避開台灣國定假日；假日若不想執行，可以手動暫停。

![圖三：調整 Routine](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-09/kjh3-1789640912.jpg?w=2000&output=webp)

Routine 的內容隨時可以在 Grok Bot App 的右側邊欄中調整。

圖／ Grok Bot

還有兩件事要提醒。8點是開始時間，Bot要搜尋、整理再貼回來，因此完成晨報還需要一些時間，不要預期8點整會整點完成。此外，就算筆電關著它也會照跑，因為它在Bot自己的雲端電腦上執行。

還有，一個Bot最多可存 **50個Routine，執行紀錄只保留最近20次** 。因此重要的晨報若想留檔，可在指令加一句「同時把晨報存進Notion的某某頁面」，或其他你指定的地方。

日後，這個Routine要停就直接按暫停。但要注意刪除後立即生效、不能復原，刪掉Bot也會連帶刪掉它的所有Routine。

## Grok Bot Routine沒有執行怎麼辦？先檢查6件事

如果到了設定時間，Routine卻沒有自動執行，可以依序檢查：

1. Routine是否仍為啟用狀態
2. 排程時間與Timezone是否正確
3. 負責執行的Bot是否仍存在
4. 任務需要的Plugin是否仍保持登入
5. Grok Bot的雲端電腦能否正常存取資料來源
6. 帳號用量或存取權限是否遭暫停

也可以打開Routine的執行紀錄，查看最近一次失敗的原因。修改設定後，建議先用Test run測試一次，再等待下一次正式排程。

> 延伸閱讀 [：「我用AI，是為了離開電腦！」SpaceXAI設計師公開Grok Bot工作流：2張截圖、一段語音就能改Figma](https://www.bnext.com.tw/article/92273/grok-bot-figma-screenshot-workflow)