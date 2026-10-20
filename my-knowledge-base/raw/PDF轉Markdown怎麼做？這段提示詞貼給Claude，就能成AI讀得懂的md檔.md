---
title: "PDF轉Markdown怎麼做？這段提示詞貼給Claude，就能成AI讀得懂的md檔"
source: "https://www.bnext.com.tw/article/91640/pdf-to-markdown-claude-prompt"
author:
  - "[[韋惟珊]]"
published: 2026-07-28
created: 2026-07-28
description: "要怎麼把資料存成 Markdown 檔？怎麼把既有的 PDF 轉成 Markdown，讓 AI 好讀？我實測的經驗跟你分享。"
tags:
  - "clippings"
status: processed
---
早安呀！歡迎用 [贊助支持我和貓貓](https://portaly.cc/1day.1ai/support) ！你的每一份打賞，都是我們繼續整理資訊的動力。

讀者 Michael 來信問我兩個關於 Markdown 的問題：一個是「要怎麼把資料存成 Markdown 檔」，另一個是「怎麼把既有的 PDF 轉成 Markdown，讓 AI 好讀？」

> 延伸閱讀： [Markdown是什麼？一個部落客為了上稿催生的格式，變AI底層語言](https://www.bnext.com.tw/article/89761/markdown-story)

我分享一下自己的經驗。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

現在的 AI 通常讀得到 PDF 裡的資訊，有些模型也能看懂頁面上的圖片、圖表和版面。但問題是：看得到，不代表每一層意思都能正確理解。

比如公司習慣用紅色代表「禁止事項」、綠色代表「同一層級」。這些意思人可以懂，但 AI 不一定每次都能正確理解這個脈絡。

所以我覺得把 PDF 轉成 Markdown 真正有價值的地方，是在轉換過程逼你去檢查： **這份文件要讓 AI 幫我做什麼？為了完成這個任務，到底有沒有寫清楚？**

怎麼存 md 檔，原始 PDF 怎麼轉換？我整理成圖卡給你！

實作看看吧！ [點此連結](https://bnext.surveycake.biz/s/kyG4L) 跟我分享心得，我都會看。

![先問：你的這份文件，要讓 AI 做什麼？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-4212a593d5abd892.jpg?w=900&output=webp)

## 先問：你的這份文件，要讓 AI 做什麼？

想一個真實使用情境：你希望 AI 用這份文件「查資料」、「照 SOP 做事」，還是「判斷該怎麼處理」？目的不同，需要寫清楚的資訊也不同。例如只是查電話、規格，資訊找得到可能就夠了，但如果要 AI 判斷一筆申請能不能通過，它還需要知道適用條件、例外情況和判斷標準。

![Markdown 怎麼存、怎麼改？其實很簡單](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-95f8be07ee2d9515.jpg?w=900&output=webp)

## Markdown 怎麼存、怎麼改？其實很簡單

Markdown 本質上是純文字檔，副檔名是.md。和 AI 協作完，你可以請它用 Markdown 格式整理，存成像 SOP.md 這樣的 md 檔。想編輯時，可以直接用文字編輯器修改，或是把 md 檔 上傳 Google Drive，用 Google 文件開啟、修改，之後再下載成 Markdown。

![PDF 轉 Markdown：這個 prompt 直接貼給 Claude](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-b5e2e1fa88f93bcb.jpg?w=900&output=webp)

## PDF 轉 Markdown：這個 prompt 直接貼給 AI

上傳你的 PDF，跟 Claude/ChatGPT 等你習慣的 AI 說：

> 把這份文件整理成適合 AI 使用的 Markdown：保留標題層級與重要資訊；表格取消合併格，把隱藏條件寫成明確文字；顏色、符號、版面若有意義，也要轉成文字；截圖與圖表中的重要內容不要略過；流程改成編號步驟，適用條件寫在對應步驟旁；保留原始頁碼方便回查；不確定的地方標 \[待確認\]，不要自行猜測；完成後，列出可能遺失、矛盾或需要人工確認的地方。

![上百頁的 PDF 怎麼做？先分段、再批次處理](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-ba3dda2a4e4b8487.jpg?w=900&output=webp)

## 上百頁的 PDF 怎麼做？先分段、再批次處理

文件越大、表格和圖片越複雜，越不適合一次全部轉完。可以先依「完整章節」或「完整 SOP」把原始 PDF 拆成幾個小檔，再一份一份上傳轉換。每批都保留原始頁碼，最後再合併成一份 Markdown。

![最後檢查別只看文字，去測它能不能完成任務](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/img-mcp-fbd6be1200942a92.jpg?w=900&output=webp)

## 最後檢查別只看文字，去測它能不能完成任務

回到第一步，拿一個真實任務測試。例如：

> 請依這份文件判斷這筆申請該怎麼處理。

你可能會發現文件整理好了，AI 還是做不準，因為它還缺乏一些隱性知識，像是怎樣才算做對、做好？哪些情況是例外？這些補上，才能真的完成任務。

[訂閱《一天一AI》日報](https://account.bnextmedia.com.tw/guest/subscribe?new_ids=44&utm_source=web_bn&utm_medium=post&tr_medium=website&tr_site_name=1day1ai&tr_space=post) ，每天跟你分享超實用的AI小技巧！

> 延伸閱讀：  
> [Markdown過時了？Claude Code工程師：當文件只給AI看，用HTML更合理](https://www.bnext.com.tw/article/90893/when-humans-stop-editing-ai-outputs-why-anthropic-engineers-are-switching-from-markdown-to-html)
> 
> [券商分析師怎麼用AI做研究？他讓NotebookLM、Gemini、Claude三個AI接力，養出專屬產業大腦](https://www.bnext.com.tw/article/91590/notebooklm-gemini-claude-ai-research-workflow)