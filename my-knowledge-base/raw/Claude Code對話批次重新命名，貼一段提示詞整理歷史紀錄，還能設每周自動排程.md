---
title: "Claude Code對話批次重新命名，貼一段提示詞整理歷史紀錄，還能設每周自動排程"
source: "https://www.bnext.com.tw/article/91678/claude-code-rename-chat-titles"
author:
  - "[[韋惟珊]]"
published: 2026-07-31
created: 2026-08-03
description: "你的 Claude 對話清單，現在長什麼樣子？我的改名前：一半英文、一半「AI workflow」。改名後：20260724｜文章上稿｜Salesforce AI 業務報告電子報上傳。"
tags:
  - "clippings"
status: processed
---
貓貓表示：歡迎大家 [贊助打賞](https://portaly.cc/1day.1ai/support) ，支持我和貓貓繼續整理實用又好看的內容。

我現在最常使用的 AI 工具其實是 Claude 桌機版 App 裡的 Claude Code，但隨著使用紀錄變多，AI 自動命名的對話我越來越看不懂，很難翻找歷史紀錄。

[吳相勳老師在 Facebook 分享](https://www.facebook.com/share/p/1D39sShL6X/) ，他靠一個提示詞，讓 Codex 批次重新命名對話的名稱。我就想：Claude Code 有沒有辦法做一樣的事？

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

Claude Code 和 Codex 一樣，有一組 session 管理工具，可以列出你所有的對話、讀取內容，幫你直接改名，超方便的！只需要貼一次提示詞，以後定時批次改名就好。但要注意，目前只適用 Claude Code，Claude.ai 網頁版就不能用。

我把步驟整理成圖卡給你！

如果這對你有幫助， [點此連結](https://bnext.surveycake.biz/s/kyG4L) 分享心得給我，也歡迎轉傳給同樣在用 Claude Code 的朋友。

![AI 的對話紀錄，你好查找嗎？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-76186de391450e94.jpg?w=900&output=webp)

## AI 的對話紀錄，你好查找嗎？

在 Claude Code 每個新對話，它都會自動命名，但使用紀錄一多，就開始難查找。參考吳相勳老師 Codex 對話標題批次重新命名指令，請 AI 檢查目前管理的對話，根據每則對話的開始日期、主要討論對象與實際內容，重新命名對話標題。

![指定任務目標和命名規則](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-4fe31b13b3688163.jpg?w=900&output=webp)

## 指定任務目標和命名規則

以下指令輸入到 Claude Code：

> 檢查你目前可以管理的對話，根據每則對話的開始日期、主要討論對象與實際內容，重新命名對話標題。標題必須方便日後搜尋、辨識與分類。有明確對象：YYYYMMDD｜相關對象｜主題名稱。無明確對象：YYYYMMDD｜主題名稱。日期必須使用該對話「建立」的日期。若對話內容不足以判斷主題、日期，在標題加上「？」，方便識別。

![提供判斷對話主題和對象的方式](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-44cba05d19d75428.jpg?w=900&output=webp)

## 提供判斷對話主題和對象的方式

對象判定優先順序：品牌、系統、公司、組織、課程或專案。主題名稱請以具體的動作為主（新增、設計、規劃、研究等），控制在 8–12 個中文字之間。若對話包含多項任務，選擇決策最重要或最終交付的主題。

![規劃執行的流程](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-359c135c6d6fe382.jpg?w=900&output=webp)

## 規劃執行的流程

請 Claude 先取得對話清單（session ID、現有標題、建立時間、內容），並讀取對話內容來判定主題與對象，據此產出新標題清單，注意避免和其他對話標題過度重複，最後執行改名，並回報成功與失敗的清單。我試的是三天、10 個對話，從「Salesforce AI sales to Pluto」改成「20260724｜文章上稿｜Salesforce AI 業務報告電子報上傳」。

![懶得每次手動跑？設一個定期排程](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-abfa0f7cfcef1e8d.jpg?w=900&output=webp)

## 懶得每次手動跑？設一個定期排程

在 Claude Code 裡說「幫我設每週一自動整理對話標題」，用 /schedule 建立排程。新開的對話會在下次排程時一起重新命名，你不需要記得這件事。

[訂閱《一天一AI》日報](https://account.bnextmedia.com.tw/guest/subscribe?new_ids=44&utm_source=web_bn&utm_medium=post&tr_medium=website&tr_site_name=1day1ai&tr_space=post) ，每天跟你分享超實用的AI小技巧！

> 延伸閱讀：  
> [懶得打字寫提示詞？Mac開聽寫、Windows按Win＋H，對AI碎念10分鐘它就能抓重點](https://www.bnext.com.tw/article/91664/voice-dictation-ai-prompt)
> 
> [你傳給同事的Claude對話，Google搜得到！用一行語法自己查，公司機密有沒有外流](https://www.bnext.com.tw/article/91663/claude-share-link-leak-self-check)