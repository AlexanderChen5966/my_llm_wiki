---
title: "ChatGPT 推出 Computer History 功能，會記住你在 Mac 上做過什麼事，還能接著完成工作"
source: "https://www.kocpc.com.tw/archives/665255"
author:
  - "[[Rocky]]"
published: 2026-08-14
created: 2026-08-14
description: "很多人工作都會交給 AI 代理來協助，因此如果 AI 能夠隨時隨地的知道我們做了哪些事情，那對效率來說一定是大加分。現在，ChatGPT 就能做到這件事了，OpenAI 稍早宣布推出全新的 Computer History 功能，就如同它的名字，會記錄電腦使用紀錄，也就是說，開啟這功能後，你在允許的 App 和網站上做過哪些事情，ChatGPT 就能自動記錄相關操作活動，整理成可供日後查詢的工作歷程。隨後當你有需要時，可以隨時查詢歷史紀錄，甚至是讓 ChatGPT 幫你完成接下來的工作。"
tags:
  - "clippings"
status: "processed"
---
很多人工作都會交給 AI 代理來協助，因此如果 AI 能夠隨時隨地的知道我們做了哪些事情，那對效率來說一定是大加分。現在，ChatGPT 就能做到這件事了，OpenAI 稍早宣布推出全新的 Computer History 功能，就如同它的名字，會記錄電腦使用紀錄，也就是說，開啟這功能後，你在允許的 App 和網站上做過哪些事情，ChatGPT 就能自動記錄相關操作活動，整理成可供日後查詢的工作歷程。隨後當你有需要時，可以隨時查詢歷史紀錄，甚至是讓 ChatGPT 幫你完成接下來的工作。

比較可惜的是，目前個人方案只有 ChatGPT Pro 用戶能使用，ChatGPT Plus 還享受不到這項功能；另外 Business 與 Enterprise 方案也能用，但必須先由管理員開放權限。

## ChatGPT Mac 版推出「Computer History」：不透過截圖，改記錄操作事件，取代先前的 Chronicle

OpenAI 稍早在 X 官方帳號宣布推出 Computer History 新功能，ChatGPT 現在可以記住你在電腦上跨 App、跨網站的活動，往後的互動會更個人化，也不用每次都重新解釋一遍。此外，由於需要搭配系統內建的輔助使用，這是 Mac 桌面版 App 專屬的功能。

![](https://x.com/i/status/2087996496088297746)

對於 ChatGPT Pro 用戶來說，可能會覺得很熟悉，沒錯，Computer History 的前身是 Chronicle 研究預覽版。

Computer History 跟 Chronicle 的差別，OpenAI 強調不只是單純改名字，而是砍掉重練。Chronicle 做法是在本機擷取螢幕畫面，再定期叫 Codex 把最近的活動摘要成記憶。Computer History 完全放棄截圖，改抓所謂的 interaction events，包含點擊、打字、鍵盤快捷鍵、切換 App，以及 macOS 透過輔助使用系統曝露出來的上下文。

而很多人在意的哪些紀錄會被排除，OpenAI 也有明確列出，排除項目包括螢幕截圖、螢幕錄影、麥克風輸入、系統音訊，以及無痕模式的瀏覽活動。

![ChatGPT 推出 Computer History 功能，會記住你在 Mac 上做過什麼事，還能接著完成工作 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260814111343_0_556bcb.png "ChatGPT 推出 Computer History 功能，會記住你在 Mac 上做過什麼事，還能接著完成工作 - 電腦王阿達")

資料保留的部分會有兩種檔案，第一種是「暫存事件檔」，也就是最原始的操作過程，記錄你幾點按了哪些鍵、點了哪裡、切到哪個 App，逐筆累積，因此紀錄量很大。第二種是「記憶檔」，把操作過程濃縮之後的摘要，大概像「下午在 Figma 修改簡報第 3 頁，接著在瀏覽器查了某個規格」這種一兩句話的結論。

整個紀錄流程大致是這樣：事件先在你的 Mac 本機累積、最多留 48 小時，接著送到 OpenAI 伺服器處理成記憶，原始事件檔在伺服器端不會保留，也不會拿去訓練模型；生成好的記憶檔案則回存到你自己電腦的 ~/.codex/memories/extensions/skysight/ 底下，記憶檔案你不刪會一直保存。

你也能指定哪些 App 和網站要被自動記錄、哪些要排除，也可以逐項檢視時間軸上的內容後，再決定要不要刪除。

Computer History 預設是關閉狀態，因此不想要使用這功能的人就不用理會。開啟之後，Computer History 會把你指定 App 和網站上的活動，自動整理成「記憶」與一條時間軸，隨後 ChatGPT 和 Codex 兩邊都能引用。

也就是說，你可以直接問「我剛剛看的那份文件在哪」、「幫我總結一下今天做了哪些事」，或是接續一個做到一半被打斷的任務，不用自己回想。除了查詢之外，它還會主動觀察你的操作模式，如果發現某個流程你重複做了很多次，就會建議把它轉成可重複使用的 skill 或自動化腳本。要注意的是，這功能必須先開啟記憶才能運作：  
![ChatGPT 推出 Computer History 功能，會記住你在 Mac 上做過什麼事，還能接著完成工作 - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/08/20260814111547_0_db24f4.png "ChatGPT 推出 Computer History 功能，會記住你在 Mac 上做過什麼事，還能接著完成工作 - 電腦王阿達")

<iframe frameborder="0" src="https://7ddac2e32c0fc0a7e56c94400aa9ce10.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>

值得注意的是，這功能有風險在，OpenAI 在官方文件承認，Computer History 產生的檔案可能含有敏感資訊，而且不會加密，代表說其他以你的 macOS 使用者身分執行的程式有可能讀取到它們。另一個問題是提示詞注入的風險被放大 ，「Computer History 會提高來自 App 和網站內容的提示詞注入風險」。

OpenAI 給的建議是，跟其他人溝通的時候，除非對方事先明確同意，否則先把它關掉。含有健康、財務或個人資料的 App 都建議排除。

此外，Computer History 在整理活動、生成記憶時都會消耗 token，因此長時間開啟會占用一定的使用額度。

![](https://www.youtube.com/watch?v=W-HhMUe9hOg)