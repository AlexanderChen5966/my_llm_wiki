---
title: "Claude Code推出新版「專案」功能：只要設定目標，AI就能自動完成多線開發"
source: "https://www.bnext.com.tw/article/92300/claude-code-projects-redesign-parallel-threads"
author:
  - "[[李先泰]]"
published: 2026-09-18
created: 2026-09-18
description: "Claude Code推出新版專案功能。只需給定目標，AI即會自動拆解任務、多線平行開發並審查彙整，離開電腦照樣運作，手機也能隨時盯進度。"
tags:
  - "clippings"
status: processed
---
Anthropic在9月17日（台灣時間9月18日）為旗下AI程式開發工具Claude Code推出新版「專案」（projects）功能，以beta測試版形式開放部分付費用戶。

官方形容這次改版是專案從「放檔案的資料夾」變成「一場持續進行的對話」： **使用者說清楚想完成什麼，Claude就自己把工作拆開，同時開好幾條「工作線」（thread）平行處理，再審查、彙整成果。人離開電腦，工作照樣往下跑，用手機也能盯進度、下指令。**

## 工作線做事、Claude指揮：一句話交辦，它自己拆工

過去用AI寫程式，若一個案子要同時改好幾個地方，得由人自己切分工作、分別開多個對話，最後再把結果拼回來。新版專案把這些協調工作交給Claude，官方的說法是「 **工作線做事，Claude指揮** 」：每個專案有一個負責統籌的「協調者」，底下是多條實際動手的工作線。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

開專案時先選定目標與要處理的程式庫（repo，存放程式碼的地方），Claude隨即建議幾項可以立刻接手的工作。之後就像對幕僚長交辦事情，把需求丟給協調者，由它判斷該開一條新的工作線，還是併入正在進行的那一條。

官方舉了兩個例子。一是「讓App的結帳流程變快」：Claude會開多條工作線，同時檢查結帳流程各環節哪裡慢、測試不同的改善方法，並各自提出合併請求（PR，把改好的程式碼送交團隊審查後併入正式版本）。

二是淘汰舊版的v1 API（API是讓不同軟體互相溝通的介面）：把API、網頁、手機App三個程式庫接進同一個專案，Claude替每個程式庫各開一條線，把還在呼叫舊版的程式改接新版、跑測試、送出PR，最後告訴你哪一個要先合併。

實際運作時，每條工作線都是一個獨立、在雲端執行的Claude Code工作階段（session），各自在一份程式碼副本上作業，互不干擾；若兩條線改到同一段程式碼，就依軟體開發常見的「合併衝突」流程處理，跟平常審查PR沒有兩樣。使用者可以在專案主對話看整體進度，也能點進任何一條線檢查細節、修正方向。

## 記得團隊的決定，也記得你的溝通習慣

另一個重點是「共享記憶」。舊版Projects已能共用專案知識與指令；這次改版進一步讓各工作線共同累積、讀取專案記憶。官方舉例，Claude能記得發布日期改到周五、當初為什麼砍掉匯出功能、動到計費系統前要先問誰，後續工作自動套用這些決定，不必每次重寫一長串提示詞（prompt）。

Claude也會學使用者的溝通習慣，可以要求它多久回報一次、多常開新工作線、每次更新寫多細。專案裡還新增「資源庫」（library），集中收納使用者上傳的檔案與Claude產出的成果。

值得注意的是，這裡的「舊版」指Anthropic在2024年6月推出的「Claude Projects」，那是把多個對話、檔案與指令收在同一處的一般工作區。科技媒體《VentureBeat》點出差別： **舊版幫Claude記住「你給它的脈絡」，新版則記住並協調「它自己正在做的工作」。**

## 誰能用？對手在哪？有什麼限制？

開放範圍分階段。9月17日起，beta先開放部分Claude Pro與Max方案用戶，條件是在Claude Code使用雲端工作階段，且網頁版或桌面版沒有既有專案；Anthropic表示公告後一周內會擴大到這兩個方案的更多Claude Code用戶，未取得資格者可登記候補名單。

之後才推到Claude全線產品（含一般聊天介面與Cowork）與Team、Enterprise方案，官方未給日期。Pro與Max用戶原有的專案維持現狀，等新版擴及上述產品時再一併升級。

多個AI代理（agent）同時開工並非Anthropic獨有。事實上，OpenAI的Codex app、GitHub的代理工具、Google的Jules都支援平行、非同步的長時間開發工作；Anthropic的差別是把「一條持續進行的對話」放在最上層當協調者，開發者只跟這一個對話溝通。

新功能有兩個代價。第一是 **用量** ：每條工作線都是完整的Claude Code工作階段，官方提醒，多條工作線同時執行，可能更快觸及用量上限；使用者可查看專案用量，並分別設定協調者與工作線的模型與推理深度（effort）。《VentureBeat》指出，此功能不另外收費，直接扣既有訂閱用量。

第二是 **執行位置** 。截至9月17日，工作線只能在雲端執行，好處是關掉筆電也會繼續跑；但依據  
Anthropic在X上的說明，雲端工作線碰不到使用者電腦裡的檔案與工具，也連不進公司內部網路。

其實對倚賴內部網路與本機工具的企業開發團隊，本機執行才是關鍵，而這部分Anthropic只說「很快」推出，截至9月17日尚未給出時間表。

資料來源： [Anthropic官方公告](https://claude.com/blog/projects-redesigned) 、 [Anthropic Projects功能說明](https://www.anthropic.com/news/projects) 、 [《VentureBeat》](https://venturebeat.com/orchestration/anthropic-launches-claude-code-projects-an-always-on-conversation-that-remembers-and-delegates-your-long-running-dev-work)

本文初稿為AI編撰，整理．編輯/ 李先泰