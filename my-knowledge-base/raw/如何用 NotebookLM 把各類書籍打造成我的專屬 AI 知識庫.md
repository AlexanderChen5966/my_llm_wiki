---
title: "如何用 NotebookLM 把各類書籍打造成我的專屬 AI 知識庫"
source: "https://muki.tw/notebook-lm-knowledge/"
author:
  - "[[MUKI space*]]"
published: 2025-05-24
created: 2026-05-18
description: "我從小就喜歡看書、買書，但大家都知道房子的空間有限，不可能讓..."
tags:
  - "clippings"
status: processed
---
如何用 NotebookLM 把各類書籍打造成我的專屬 AI 知識庫.md

我從小就喜歡看書、買書，但大家都知道房子的空間有限，不可能讓我無限制的購買實體書籍，所以近年來我也轉向購買電子書籍，只是電子書雖然購買方便，可是不像實體書一樣可以將相同類型的書籍排列好，甚至在有需要的時候可以攤開來做比對。

![](https://muki.tw/wordpress/wp-content/uploads/2025/05/Gemini_Generated_Image_l7ctjgl7ctjgl7ct.jpg)

所以我碰到的問題是，如果想要找到特定資訊、比較不同觀點，或將相關知識串聯起來時，使用電子書真的很不方便，還不如實體書用得順手。

直到我遇見了 NotebookLM，這個由 Google 推出的 AI 筆記和研究助理，當我嘗試將同類型的書籍當作來源，組成一份特定主題的 NotebookLM 知識庫後，我覺得我好像找到正確打開電子書的方式了 XD，我的閱讀和知識管理也發生了很大的變化。今天，就想跟大家分享我的實戰流程與心得，以及這個方法能如何釋放你藏書的驚人潛力。

## 為什麼我想把書本裝進 NotebookLM？

每個人應該都有自己偏好的領域，像我可能是個人成長、投資、筆記知識管理... 等等，所以同一個領域下我會購買很多不同的書籍，希望能更全面的瞭解相關內容。但我卻常常發生以下問題：

- 資訊零散：我看了 A 書籍的觀點，卻忘了 B 書籍的案例，或是想找 C 書籍的數據卻很難找到。
- 串聯不易：我該如何快速比較不同作者對同一概念的闡述？我該怎麼整理同個理論在不同書籍的演變？

而 NotebookLM 可以讓我們上傳自己的書籍資料 (PDF)，並用這些資料為基礎進行對話和內容生成。所以我就在想，如果我把同一個主題的書籍都給 NotebookLM，是不是就能打造一個專屬於我的、針對特定領域的 AI 專家！

AD

<iframe width="722" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-6816776861246904&amp;output=html&amp;h=280&amp;slotname=2619535264&amp;adk=536108718&amp;adf=3680071312&amp;pi=t.ma~as.2619535264&amp;w=722&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1779071070&amp;rafmt=1&amp;armr=3&amp;format=722x280&amp;url=https%3A%2F%2Fmuki.tw%2Fnotebook-lm-knowledge%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjEuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguOTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNDguMC43Nzc4Ljk4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTQ4LjAuNzc3OC45OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=7&amp;dt=1779071070657&amp;bpp=1&amp;bdt=157&amp;idt=-M&amp;shv=r20260512&amp;mjsv=m202605120101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D0be31aad6c90d5a7%3AT%3D1764056540%3ART%3D1779070844%3AS%3DALNI_MbWZzM1RisnmD9S3kAF97Mzb0gETw&amp;gpic=UID%3D000011bc6848ed38%3AT%3D1764056540%3ART%3D1779070844%3AS%3DALNI_MZhzkBxrlRlT-2aBit455WSmoVVsQ&amp;eo_id_str=ID%3D83e326f1f7d5be28%3AT%3D1764056540%3ART%3D1779070844%3AS%3DAA-AfjYFEym4nHovZb8mmGQFppMH&amp;prev_fmts=0x0&amp;nras=1&amp;correlator=1551492754900&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=5&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1049&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=331&amp;ady=1999&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=42532524%2C95390787%2C95390680&amp;oid=2&amp;pvsid=8434063129449741&amp;tmod=1973811924&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fmuki.tw%2Fplanner%2Fdigital-note-taking%2F&amp;fc=1920&amp;brdim=1920%2C31%2C1920%2C31%2C1920%2C31%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=M%7C%7CeEbr%7Cp&amp;abl=XS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=243x742_l%7C243x742_r&amp;bz=1.15&amp;pgls=CAEaAzYuOQ..~CAEQBBoHMS4xNzUuMA..&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=2" title="Advertisement" aria-label="Advertisement"></iframe>

## 我如何打造自己的書籍知識庫

### 第一步、書籍的選擇與準備

我將主題相似或領域相同的書籍放在同一個 NotebookLM 專案中，才能讓 AI 在一個相對聚焦的知識範圍內進行深度學習和整合。這是我建立的其中一個 NotebookLM，針對「如何閱讀書籍」這樣的主題做處理。

可以把整本書籍上傳到 Notebook LM 嗎？

epub 都有受 DRM 保護，所以我通常是將書籍的大綱以及我快速閱覽有興趣的段落筆記成 PDF (但保留原文字句) 然後上傳作為來源。  
雖然每個人整理書籍的方式可能有所不同，但建議不要上傳整本書籍以免違反著作權，此外也建議不要公開分享給他人，只作為個人學習用途。

![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-11.37.20.png)

### 第二步、先看研讀指南

當我有多個來源時，我需要快速進入狀況，了解每個來源的重點。透過 NotebookLM 的「研讀指南」功能，可以讓我得到這些資訊：

![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-12.52.21.png)

![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-12.51.25.png)

### 第三步、從心智圖了解每本書的全貌

我不確定是我每本書的來源字數太多，還是關聯性不大，但總之我的心智圖功能，是沒辦法整合我所有的來源產生單一份心智圖。可以從圖片看到，即使我勾選了 5 個來源，心智圖還是只會從單一書籍產生，所以我的使用方式改成每一個來源產一份心智圖，有助於我在沒看過書的情況下，快速進入狀況。

![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-13.04.23.png)

### 第四步、提問與提問技巧分享

- **使用 NotebookLM 的推薦問題** ：NotebookLM 會根據來源推薦我們可以問的問題，如果一開始不知道該問什麼問題，可以從推薦問題中選擇，快速了解整個知識的脈絡與架構。
	- ![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-11.39.55.png)
- **精準提問** ：如果已經有想法，我們也可以精準的提問，NotebookLM 會從我們提供的書籍中找出相關的段落並整合答案，而且還會標明出處。
	- 在這些書中，對於閱讀的定義有哪些共同點和不同點？
		- ![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-11.44.40.png)
		- 每次問完一個問題，如果答案對你有所幫助，記得按下「儲存至記事」的按鈕，然後再開始新的一輪提問，這樣才不會讓所有的問題混在一起。
		- ![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-12.22.33-1024x367.png)
- **自動摘要** ：如果你有想了解的特定主題，或者覺得內容太多很難找出重點，就可以請 NotebookLM 幫我們做摘要，以快速了解。
	- 幫我摘要這五本書中關於「閱讀方法」的共通原則
		- ![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-12.27.01.png)
- **協助重點整理** ：我們也可以從來源書籍中，找出所有與特定關鍵字或主題相關的資料，NotebookLM 也會幫忙收集與列點，變成一個資訊清單。
	- 列出這些來源中所有提到將閱讀轉化成個人筆記的方法
		- ![](https://muki.tw/wordpress/wp-content/uploads/2025/05/CleanShot-2025-05-24-at-12.31.55.png)

### 補充：我常用的提問方法

除了上述這些提問方法外，我還有自己一些比較獨特的應用方式，可以幫助我更有效的從大量資料中萃取知識，並進行深度思考。以下分享我常用的技巧和心得：

- **觀點比較** ： 我特別喜歡問像：「關於領導力，請比較作者 A 在《從A到A+》和作者 B 在《第五項修練》中的核心觀點有何不同？」這能讓我更深入理解同一主題下的差別和作者詮釋的不同角度。
- **梳理脈絡** ： 如果書籍有順序性或代表不同的學派，我會問像：「關於行為經濟學的發展，請根據這幾本書的內容，簡述其主要里程碑。」
- **找到不足或需要補充的部分** ：有時候我會想知道我提供的這些來源是否夠全面？有沒有知識缺口？所以我會這樣提問：「綜合這些來源的內容，關於從閱讀轉成筆記的實際應用，還有哪些問題沒有被充分解答？或者哪方面是這些來源比較少提到的？」，由此來了解我是否有需要針對這部分，再去找相關的內容或書籍來補全。

### 小提示與注意事項

在使用 NotebookLM 的過程中，我也總結了一些經驗，希望能幫助各位更好地駕馭這個工具：

- **來源品質至關重要：** 上傳的書籍內容越清晰、數位化品質越高，AI 的理解和回答就會越精準。如果上傳的是自己的筆記，內容的結構性和完整性也很重要
- **提問的藝術：** 問題越清晰、越具體，得到的答案就越有價值。如果你覺得答案對你沒有幫助，可以嘗試用不同的方式提問，引導 AI 從不同角度思考
- **保持批判性思維：** AI 是強大的助手，但它給出的答案是基於你提供的來源進行的總結和推斷。我們仍需運用自己的專業知識和批判性思維來判斷和吸收資訊，不可全盤照收
- **版權考量：** 在數位化和分享書籍內容時，務必尊重和遵守版權法規。個人學習研究使用通常是被允許的，但公開傳播受版權保護的完整內容則需謹慎
- 免費版的限制 **：** 據官方所說，免費版的 NotebookLM 最多可以建立 100 個筆記本，每個筆記本的來源上限為 50 個，如果你跟我一樣是用免費版，就要特別注意該限制，來源與數量都要有所篩選

## 小結

如果你也和我一樣，面對成堆的電子書有點不知所措，不妨試試看這個方法。選擇一個你感興趣的主題，挑選幾本相關書籍作為來源，讓 NotebookLM 一起幫你學習研究。而知識庫的這些內容，將作為我們大腦知識的來源，再轉化成屬於我們自己的卡片筆記，如果你喜歡這篇文章，也有興趣了解我是如何將 NotebookLM 的內容做成卡片筆記，歡迎留言告訴我，我會再找時間整理跟大家分享。

muki@journal ~/AI 應用 $