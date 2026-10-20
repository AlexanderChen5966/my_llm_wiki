---
title: "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報"
source: "https://www.kocpc.com.tw/archives/665567"
author:
  - "[[Rocky]]"
published: 2026-08-18
created: 2026-08-18
description: "大多數人製作簡報，應該都是透過 PowerPoint 或 Google 簡報，雖然不難用，但每次要修改簡報檔時，還是需要打開這些工具才能編輯，雖然有線上版，但遇到一些情況可能就會變得有點棘手，像是你在的環境剛好沒有網路，電腦也沒有安裝離線版。最近就發現到這個蠻厲害的 Bento Slides，它是一個 HTML 格式的簡報檔，不僅能播放簡報內容，打開後還能直接編輯，意味著你不再需要任何簡報工具就能編輯、製作簡報。"
tags:
  - "clippings"
status: "processed"
---
大多數人製作簡報，應該都是透過 PowerPoint 或 Google 簡報，雖然不難用，但每次要修改簡報檔時，還是需要打開這些工具才能編輯，雖然有線上版，但遇到一些情況可能就會變得有點棘手，像是你在的環境剛好沒有網路，電腦也沒有安裝離線版。最近就發現到這個蠻厲害的 Bento Slides，它是一個 HTML 格式的簡報檔，不僅能播放簡報內容，打開後還能直接編輯，意味著你不再需要任何簡報工具就能編輯、製作簡報。

編輯功能基本上你需要的都有，操作介面也跟大多數簡報工具差不多，因此非常容易上手。它還有一個非常大的特色：你可以讓 AI 幫你製作簡報檔。下面就示範給大家看。

## Bento Slides 介紹和操作教學

Bento Slides 是一款免費、開源，而且相當特別的簡報製作工具。和 PowerPoint、Google Slides 這類傳統簡報軟體最大的不同在，Bento Slides 將整份簡報、編輯器、播放器、圖片、動畫與相關資源全部封裝在單一 HTML 檔案中。使用者不需要安裝額外軟體，也不需要註冊帳號，只要使用瀏覽器開啟.bento.html 檔案，就能直接查看、編輯與播放簡報，甚至在沒有網路連線的環境下也能正常使用。

另一個很大的特色是，從設計上就非常適合搭配 AI 代理使用。簡報內容會以結構化資料保存在 HTML 內，因此 Codex、Claude、Cursor、Gemini 等 AI 工具可以直接讀取並修改簡報內容。使用者只需要用自然語言描述需求，例如「新增一頁產品比較」、「把這些數據製作成長條圖」或「重新設計這張投影片」，AI 就能直接修改原本的 Bento Slides 檔案。

在簡報功能方面，Bento Slides 也不是只有基本文字與圖片排版，它支援圖表、動畫、Morph 轉場、影片、音訊、Speaker Notes、互動效果等功能，可製作出比一般靜態簡報更豐富的呈現方式。圖片等素材還能直接嵌入 HTML，因此完成後只需要分享一個檔案，就能把整份簡報交給其他人。

此外，Bento Slides 採用 Local-first 設計，簡報檔主要保存在使用者自己的裝置上。

**主要特色**

- 免費開源：採用開源方式提供，可自由使用。
- 單一 HTML 檔案：簡報、播放器與編輯器都能封裝在同一個檔案。
- 免安裝軟體：透過瀏覽器即可開啟、編輯與播放。
- 支援離線使用：檔案保存在本機，沒有網路也能開啟。
- 適合 AI 代理操作：Codex、Claude、Cursor 等工具可直接修改簡報。
- 支援 Morph 動畫：可製作元素位置、尺寸等變化的轉場效果。
- 內建圖表功能：支援長條圖、折線圖、圓餅圖、散佈圖等資料視覺化。
- 支援多媒體：可加入圖片、影片、音訊及動畫效果。
- 支援 Speaker Notes：適合正式簡報與演講使用。
- 素材可直接嵌入檔案：圖片等資源可放進 HTML，分享時不用另外打包資料夾。
- Local-first 設計：使用者能自行掌握簡報檔案與資料。

[前往 Bento Slides 官網](https://bento.page/)

按上方連結進到 Bento Slides 後，你可以先線上體驗，再決定要不要下載該 HTML 檔案（按 Download 即可下載）。點 Try it in your browser 來線上體驗：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160407_0_55b729.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

下載的話你會獲得這個.html 檔，就跟線上打開一樣。旁邊的 agents.md 是等等示範使用 Codex 自動製作簡報時，需要提供給 AI 閱讀的操作指引檔。這個檔案也能在剛剛的網頁中找到：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160358_0_51a9f3.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

<iframe frameborder="0" src="https://25afbfe63d01f9c558d525b72de7295d.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>

按 Get AI Skill 就會自動滑到這個區塊，看你是要搭配 Claude 還是 Codex、Gemini：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160406_0_59355d.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

下圖就是 Bento Slides 的 HTML 打開模樣，就是一個簡報編輯器，上方有各種工具，右邊是設定，左邊則是投影片：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160359_0_cb9240.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

如果你打開不是繁體中文，點右上角的地球圖示即可切換語言：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160359_0_3e96b0.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

簡報中的文字、圖檔等各種物件都能夠編輯：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160400_0_588b97.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

每一張投影片也能刪除或新增更多投影片：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160400_0_c32dad.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

也支援插入媒體功能，影片或音訊都行，也能從連結新增：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160401_0_7a193a.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

還提供共用功能，你可以邀請同事或朋友來一起編輯這個簡報檔案：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160401_0_5be61a.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

如果你要播放簡報，就按下方的「投影片放映」：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160402_0_dbdbb1.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

儲存功能位於右上角。如果有需要，還能為這個簡報檔加上密碼，功能可說非常全面：

![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160402_0_b00604.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

編輯功能就不多做介紹了，大家再自行玩玩看。

接下來看怎麼叫 AI 代理自動製作或編輯簡報，我使用的是 Codex。首先要注意的是，簡報檔和剛剛下載的 AGENTS.md 操作指引檔必須放在同一個資料夾內，這樣製作前 AI 就會先閱讀這個技能檔，了解到整個架構。

我在 Codex 裡新增一個 Bento 簡報，來源資料夾就設定簡報檔和技能檔的那個資料夾：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160403_0_5abb02.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

我要求 AI 幫我把 [TP-Link Deco BE65-PoE 開箱實測](https://www.kocpc.com.tw/archives/646084) 這篇文章製作成簡報，製作之前先閱讀 AGENTS.md 技能檔，然後後面還有一些內容要求：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160404_0_299384.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

後面也建議補上這個提示詞，因為 Bento 官方規範要求，如果要維持真正的「單一 HTML 檔」，圖片應該轉成 Data URI 放進 doc.assets，再用 asset: 引用：

請不要只在 Bento HTML 中引用 kocpc.com.tw 的遠端圖片網址。

將實際使用到的文章圖片下載到暫存位置，  
轉成適當大小與格式後嵌入 Bento 的 doc.assets，  
確保即使電腦完全離線，  
Deco\_BE65\_PoE.bento.html 仍然能完整顯示所有圖片。

![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160405_0_ad36e1.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

AI 就會開始處理，等待它跑完就好：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160406_0_c5fa63.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

完成後打開 HTML 檔案，就看到它幫我把 TP-Link 那邊文章製作成簡報了，而且內容可說很完美，我的要求都有做到，每張簡報都要搭配圖和重點文字：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160408_0_989f47.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")

每一張投影片內容當然也都能修改：  
![Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260818160409_0_490cb0.jpg "Bento Slides 簡報就是單一 HTML 檔，可直接打開編輯、播放，甚至讓 AI 製作簡報 - 電腦王阿達")