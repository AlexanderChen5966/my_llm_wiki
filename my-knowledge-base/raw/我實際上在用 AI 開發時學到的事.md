---
title: "我實際上在用 AI 開發時學到的事"
source: "https://codelove.tw/@tony/post/aYW9Xx"
author:
published:
created: 2026-10-21
description: "我曾經好幾個晚上都在用 AI 做東西，做出了不少功能，然後才突然發現，我其實沒辦法把自己剛完成的一半內容解釋清楚。也有些夜晚，我花了好幾個小時想搞懂某個超小的問題，結果不知怎麼地，從那裡學到的東西反而比前一整晚還多。仔細想想，這其實有點好笑。兩種情況下我都很有效率。兩種情況下我最後也都寫出了..."
tags:
  - "clippings"
status: "inbox"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月20日](https://codelove.tw/@tony/post/aYW9Xx "2026-10-20 08:03:03")

我曾經好幾個晚上都在用 AI 做東西，做出了不少功能，然後才突然發現，我其實沒辦法把自己剛完成的一半內容解釋清楚。

也有些夜晚，我花了好幾個小時想搞懂某個超小的問題，結果不知怎麼地，從那裡學到的東西反而比前一整晚還多。

仔細想想，這其實有點好笑。兩種情況下我都很有效率。兩種情況下我最後也都寫出了能運作的程式碼。但我從每次經驗中真正學到的東西，卻完全不同。

我最近寫了一篇文章，談到 AI 讓人太容易逃避思考，這件事也讓我開始反省自己的習慣。因為我一直都在用 AI 寫程式工具，而且用得很兇。我很喜歡它們。它們幫我做出了不少東西，不然那些東西我大概根本不會去嘗試。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792560511&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYW9Xx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560489582&amp;bpp=1&amp;bdt=805&amp;idt=1&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1654x873&amp;nras=3&amp;correlator=7970502359790&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=970&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95400480%2C95404669%2C95404823%2C95405781%2C31061691%2C31061693&amp;oid=2&amp;psts=AOrYGsnsG3YkP-eJz8KcEgDMy8WfRP4msbHNGKf8baEkhBgXECRDNndxEy1rEL7xsOeNg-ut5iTTUkfrzZ5Tmm2BjqmZUZweDG974ewMnc7l0fqVguI&amp;pvsid=7411412852523937&amp;tmod=1079284778&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=21572" title="Advertisement" aria-label="Advertisement"></iframe>

但我也還在學，而且學很多。

而且「把東西做出來」跟「真正理解它怎麼運作」之間，差很多。

## 你理解了解釋，不代表你理解了程式碼

這大概是所有人都會遇到的事，只是我們不太想承認而已。

我可能會請 AI 解釋某件事，它會給我一個非常合理的說明。每個部分都講得通。我可以跟得上，知道每一段在做什麼，然後心想，好，懂了。

結果三天後我需要改一點東西，突然盯著程式碼看，感覺自己像第一次看到它一樣。

顯然，在有人一路牽著我理解整段說明的時候，我是確實看懂了。

但那跟真的知道它是什麼，還是有一段距離。

而且我說的不是背語法，或記住每一個 API 細節。我也常常需要查資料，我不覺得這有什麼問題。

我指的是理解「為什麼要這樣做」。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792560511&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYW9Xx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560489589&amp;bpp=1&amp;bdt=811&amp;idt=1&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1654x873%2C952x280&amp;nras=4&amp;correlator=7970502359790&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=1811&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95400480%2C95404669%2C95404823%2C95405781%2C31061691%2C31061693&amp;oid=2&amp;psts=AOrYGsnsG3YkP-eJz8KcEgDMy8WfRP4msbHNGKf8baEkhBgXECRDNndxEy1rEL7xsOeNg-ut5iTTUkfrzZ5Tmm2BjqmZUZweDG974ewMnc7l0fqVguI&amp;pvsid=7411412852523937&amp;tmod=1079284778&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=21569" title="Advertisement" aria-label="Advertisement"></iframe>

如果我對某個功能唯一記得的事，就是 AI 寫了它，而且它能跑，那我其實沒有從中學到多少。

## 有時候，真正有意思的部分，是你原本根本不知道存在的問題

我最近一直在做一個文字編輯器，叫做 [Slate](https://github.com/pinkpixel-dev/slate) ，這對我來說是一段很有意思的學習經驗。

其中一件我之前沒太想過的事，是你在編輯檔案的同時按下儲存，實際上會發生什麼。

儲存檔案聽起來很簡單，對吧？把內容寫到磁碟、標記為已儲存、完成。

但如果使用者在儲存進行時還繼續打字呢？

被寫入的檔案內容是舊的，但編輯器裡已經有更新的變更。於是如果儲存完成後，編輯器把未儲存狀態清掉，基本上就是在告訴使用者「都存好了」，但其實並沒有。

Slate 用的是基於修訂版本的儲存追蹤來處理這件事。當儲存開始時，它會記住自己正在儲存的是文件的哪個修訂版本。這樣一來，儲存進行期間所做的變更，就不會被誤判成已儲存。

AI 當然可以直接幫我把這整套做出來。我可以測試儲存按鈕，看到它能運作，然後繼續過日子。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792560511&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYW9Xx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560489596&amp;bpp=2&amp;bdt=819&amp;idt=2&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1654x873%2C952x280%2C952x280&amp;nras=5&amp;correlator=7970502359790&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=2635&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95400480%2C95404669%2C95404823%2C95405781%2C31061691%2C31061693&amp;oid=2&amp;psts=AOrYGsnsG3YkP-eJz8KcEgDMy8WfRP4msbHNGKf8baEkhBgXECRDNndxEy1rEL7xsOeNg-ut5iTTUkfrzZ5Tmm2BjqmZUZweDG974ewMnc7l0fqVguI&amp;pvsid=7411412852523937&amp;tmod=1079284778&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=21565" title="Advertisement" aria-label="Advertisement"></iframe>

但真正理解為什麼需要這種修訂追蹤，才是這件事對我有價值的地方。

重點其實不在程式碼本身，而是在認知到一件事可能會在「操作開始」和「操作結束」之間改變。

而這不只是文字編輯器才會遇到的問題，任何應用程式都可能碰上。

這才是我想從專案中帶走的東西。

## 實際動手實驗，差很多

我在使用本機 AI 模型時也注意到同樣的事。

假設我執行一個模型，而它花了 30 秒才完成某件事。

好吧，所以它很慢，對吧？

也許吧。但那 30 秒到底花在哪裡？

大部分時間是花在把模型載入記憶體嗎？實際運算很慢嗎？第二次執行會快很多嗎？

這些都可以測試。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792560511&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYW9Xx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560489610&amp;bpp=1&amp;bdt=832&amp;idt=0&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1654x873%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=7970502359790&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=3491&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95400480%2C95404669%2C95404823%2C95405781%2C31061691%2C31061693&amp;oid=2&amp;psts=AOrYGsnsG3YkP-eJz8KcEgDMy8WfRP4msbHNGKf8baEkhBgXECRDNndxEy1rEL7xsOeNg-ut5iTTUkfrzZ5Tmm2BjqmZUZweDG974ewMnc7l0fqVguI&amp;pvsid=7411412852523937&amp;tmod=1079284778&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=5&amp;fsb=1&amp;dtd=21554" title="Advertisement" aria-label="Advertisement"></iframe>

而且我發現，當我真的開始實驗，而不是只叫 AI 解釋正在發生什麼時，我學到的東西會多很多。

再跑一次。分開量測。改一個地方。看看會發生什麼。

有時候結果完全符合我的預期；有時候卻差很遠，而通常這就是事情變得有趣的時候。

我很喜歡可以用 AI 幫我釐清問題。我可以問它還能試什麼，或為什麼結果不是我預期的那樣。

這讓學習陌生事物變得容易很多。

但 AI 告訴我「應該會怎樣」，跟我親眼看到「實際上會怎樣」，之間還是有很大的差別。

而且有時候 AI 的解釋也不對，這點我想大家都很清楚。

## 我不一定記得程式碼，但我會記得問題

我想這大概是我最有感的一件事。

當我真的花時間去理解某件事時，之後我也許不會記得具體的實作方式，但我會記得當時想解決的問題，以及我為什麼會那樣解。

而這會延伸到其他專案裡。

也許我會開始想：某個非同步操作，會不會在使用者已經改過東西之後才完成？

或是某個效能問題，其實是不是啟動時的問題？

又或者，我是不是存了一些狀態，其實完全可以從別的資料算出來？

我可能還是需要查資料。我可能還是會請 AI 幫我寫實作。

但至少我知道自己要找什麼，也知道為什麼這件事重要。

如果我只是沒多想就接受生成的程式碼，那我就會錯過這些。解法明明就在我的專案裡，但當我之後在別的地方遇到同樣的問題時，我甚至可能認不出來。

而考慮到我開始新專案的頻率，這種問題很有可能還是會再遇到。大概下週就會遇到。

## 當然，我不需要把每件事都變成學習機會

我也覺得這裡需要平衡。

有時候我只是想讓 AI 幫我把按鈕的內距修好。

我不需要花 45 分鐘去研究 CSS 間距的底層原理。我只想讓按鈕看起來對。

而且有很多事情，我也很樂意交給 AI 處理。樣板程式、重複性的修改、那些我已經做過一百次的事，或是單純我懶得做的瑣碎工作。

這也是我會用這些工具的原因之一。

但當我在做不熟悉的東西，或者在處理我知道之後還得維護的功能時，我會試著更投入一點。

有時候那代表我會多問一些問題。

有時候我會直接開始改東西，看看會發生什麼。

我覺得，就算只是先預測一下某個變更會帶來什麼影響，再去執行它，也可能有差。因為如果結果跟我預期的不一樣，那我現在就有一個值得調查的方向。

我每次都這樣做嗎？當然沒有。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1792560520&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYW9Xx&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=14&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560489626&amp;bpp=1&amp;bdt=849&amp;idt=1&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C334x250%2C1654x873%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C952x280%2C952x280&amp;nras=10&amp;correlator=7970502359790&amp;rume=1&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=176&amp;ady=5462&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=1996&amp;eid=95400480%2C95404669%2C95404823%2C95405781%2C31061691%2C31061693&amp;oid=2&amp;psts=AOrYGsnsG3YkP-eJz8KcEgDMy8WfRP4msbHNGKf8baEkhBgXECRDNndxEy1rEL7xsOeNg-ut5iTTUkfrzZ5Tmm2BjqmZUZweDG974ewMnc7l0fqVguI%2CAOrYGslw2y7IcAc7cLkOppXf8VDJdWDWGf_7Ib9TPxtABk617uR94TO4yy43SK5WmBU7Gl3MV6h5FBij7fUnasopvbiTCskp4xrEkjyCOqgQXFY_w7Iv4WX6c7A-sZpELwpWWvk%2CAOrYGslwL6ToS0XOnZJMasGseM8laMrGJ7s_7SZGPcIJ9CE2_pO8mqYm6Linf-OCwBJhyedqYvfuT1zDG0aBsRtirzlw5MdjqSuzY8_jyw_9elBN6g%2CAOrYGsmDPiCcUX82P54pDxhFT47vDdJU791gvo5U0fNBUu36c2JH5i9wnO_8xyaTin3QmA9mtCeIpTCSrXImrXEr3GIKFsJ03TV0B2usqCokUwPUv0s%2CAOrYGslIYK4HNzojFxeH55TqZ7WJy9ljHKDdSLsDmjTUkeI8uAO1Ch6yNNgnevK0UrMaORcYCileQlqb1T8xwOZe_Le8cez91ZNM0Fp3WYf_JA_H-NA%2CAOrYGsnc0G0Bsx7wbCjs7JWbXLRdE0boCoySRjvGfwapauZzcgwKsNbH_ImgcGXZ40dHFtIQ0BfeElaE-AmB5z1zMHb9tr1qEMLn9-LLXdbWT3IE&amp;pvsid=7411412852523937&amp;tmod=1079284778&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=9&amp;fsb=1&amp;dtd=31109" title="Advertisement" aria-label="Advertisement"></iframe>

有時候我已經對著某個東西看了兩個小時，AI 終於把它弄好了，而我腦中唯一的想法就是：「謝了，終於好了，繼續下一個。」

我不會假裝不是這樣。

但我確實在試著更有意識地分辨：什麼時候我是在學東西，什麼時候我只是在把事情做完。

因為這兩者不一定是同一件事。

## 把東西做出來，不等於學會它怎麼運作

我最喜歡 AI 寫程式工具的一點，就是我現在更願意去嘗試那些我幾乎不懂的東西了。

我試過各種語言、框架、本機模型、桌面應用程式，還有很多其他東西；如果沒有 AI，我以前大概都會先擱著不碰。

不一定是因為我學不會，而是因為要從一個完全陌生的東西開始，真的很花力氣。

AI 把最初那道門檻大幅降低了，我覺得這真的很棒。

但同時也很容易產生一種誘惑：就一直做下去就好。一個功能能動了，就接著做下一個。再下一個。等你回過神來，你已經有一整個應用程式了，但對其中一些很重要的部分，其實只有很模糊的理解。

而且很容易把這些進度，誤認成學習。

我自己確實也這麼做過。

我覺得，最讓我印象深刻的東西，通常都是那些我得稍微調查一下的部分。某些行為跟我預期的不一樣，或者我突然對它為什麼會那樣運作感到好奇，於是花了一些時間去搞懂。

有時候那種調查只花幾分鐘；有時候它會把我帶進一整晚的深坑。

不管怎樣，這些經驗通常都比那些第一次就完美運作的功能，更讓我記得住。

而這是我在繼續用 AI 開發時，想要一直記住的事。

我不想停止使用這些工具，也不想刻意把每件事都弄得更難。

我只是不想太沉迷於自己能做出多少東西，而忘了注意自己一路上到底學到了什麼。

所以……你呢？你有沒有在用 AI 開發的過程中學到一些有用的東西，現在已經能自己除錯或解釋了？或者，你有沒有回頭看過 AI 生成的程式碼，才發現自己其實完全不知道它怎麼運作？

---

原文出處： [https://dev.to/sizzlebop/what-i-actually-learn-when-i-build-with-ai-1074](https://dev.to/sizzlebop/what-i-actually-learn-when-i-build-with-ai-1074)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aYW9Xx) [12瀏覽](https://codelove.tw/@tony/post/aYW9Xx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aYW9Xx)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [REA：讓你的 AI 代理反向工程你希望擁有的應用程式功能](https://codelove.tw/@tony/post/qenz7x)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異](https://codelove.tw/@tony/post/3DRvQq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝11 💬1

341

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商事務所：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1792560488&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYW9Xx&amp;pra=5&amp;aiof=14&amp;asro=0&amp;itsi=3&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NS4wLjgwNTkuODUiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE1NS4wLjgwNTkuODUiXSxbIkNocm9taXVtIiwiMTU1LjAuODA1OS44NSJdLFsiTm90KEE6QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1792560488852&amp;bpp=2&amp;bdt=74&amp;idt=32&amp;shv=r20261015&amp;mjsv=m202610150101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1792560088%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1792560088%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1792560088%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=7970502359790&amp;rume=1&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1654&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95400480%2C95404669%2C95404823%2C95405781%2C31061691%2C31061693&amp;oid=2&amp;pvsid=7411412852523937&amp;tmod=1079284778&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1894%2C954%2C1654%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=40" title="Advertisement" aria-label="Advertisement"></iframe>