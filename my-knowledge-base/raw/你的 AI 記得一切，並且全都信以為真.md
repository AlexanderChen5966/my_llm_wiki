---
title: "你的 AI 記得一切，並且全都信以為真"
source: "https://codelove.tw/@tony/post/xdjBYa"
author:
published:
created: 2026-09-01
description: "我認為我們至今對 AI 記憶的理解方式還是錯的。大多數實作都只是同一種模式的變體：先把先前資訊存起來，之後再取回，注入到提示詞中，然後把結果稱作記憶。這當然有用，但從架構上看，它其實和在公寓裡到處貼便利貼，然後決定「公寓現在會記得事情」沒有太大差別。當我越深入接觸 AI 系統，我越覺得記憶根本不該屬..."
tags:
  - "clippings"
status: "processed"
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月30日](https://codelove.tw/@tony/post/xdjBYa "2026-08-30 12:02:11")

## 你的 AI 記得一切，並且全都信以為真

我認為我們至今對 AI 記憶的理解方式還是錯的。大多數實作都只是同一種模式的變體：先把先前資訊存起來，之後再取回，注入到提示詞中，然後把結果稱作記憶。這當然有用，但從架構上看，它其實和在公寓裡到處貼便利貼，然後決定「公寓現在會記得事情」沒有太大差別。當我越深入接觸 AI 系統，我越覺得記憶根本不該屬於模型本身，而應該屬於模型周圍的系統。

模型應該可以在明天消失，但歷史仍然保留。Claude 今天寫下的內容，GPT 明天應該能讀；本地模型下週應該能質疑它；而半年後我們使用的任何模型，仍然應該能理解為什麼會存在一個看起來很蠢的權宜之計。這就是我一直在做的實驗：不是把「長期記憶」當成另一個助理功能，而是為 AI 代理人建立一層共享的外部記憶。做得越久，我越懷疑真正有趣的問題不是記憶本身，而是「遺忘的經濟學」。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228762&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626799&amp;bpp=3&amp;bdt=1252&amp;idt=4&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=915&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 模型知道很多事，但對星期二一無所知。

現代模型懂的程式語言比我這輩子都可能學不完。它知道分散式系統、資料庫、React、Python、Rust、Kubernetes、冷門 RFC，還可能知道十五種不同的說法，來解釋為什麼我的架構複雜得沒有必要。它不知道的是，我上週二在專案裡到底發生了什麼。它不知道我們已經試過看似明顯的解法但失敗了，不知道一個難看的介面之所以存在，是因為還有三個儲存庫依賴它，也不知道某個看似隨意的慣例，其實是一場兩小時討論後的結果，而沒有人想再重來一次。

這個差別很重要，因為這些資訊不可能合理地存在於模型權重裡。它不是通用知識，而是歷史，更精確地說，是工作過程中產生的狀態。這也是我認為 RAG 和記憶之間的差異開始變得更清楚的地方。RAG 通常是取回那些已經存在某處的資訊：文件、程式碼、工單、政策、文章、資料庫紀錄。記憶則應該保存由流程本身產生的資訊：我們為什麼選 A 而不是 B，為什麼 C 失敗，為什麼要容忍 D，E 之後改變了什麼，哪個假設只是暫時的，以及團隊在犯錯後學到了什麼。

這些東西不一定總是文件。很多時候，它們正是本該變成文件、卻從來沒有變成文件的資訊。於是新的 AI 會話必須靠從頭重新發現這些缺失的歷史來支付代價。這就是我想攻擊的部分。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228762&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626807&amp;bpp=1&amp;bdt=1260&amp;idt=0&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280&amp;nras=4&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1747&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 所以我把記憶移到代理人外面了

這個原型刻意做得很無聊，我是把這件事當成稱讚。它是一個團隊共用的小型 HTTP 記憶中心。記憶是帶有結構化中繼資料的純 Markdown 檔，分成一般知識與專案特定知識。每個 AI 會話都透過自己的 MCP 伺服器存取它，工具介面非常小：列出有哪些內容、抓取相關內容、再寫入新的內容。

重點不在 MCP 的管線，而在邊界。模型不是記憶庫，客戶端不是記憶庫，MCP 伺服器也不是記憶庫。記憶獨立存在於它們之外。會話開始時，系統會先給它一個小型索引，讓它知道有哪些記憶可用；但完整內容只有在代理人明確要求時才會進入上下文。

這個差異很重要，因為糟糕的記憶架構很容易變成一種超昂貴的做法：把整個公司的歷史直接吼進每一次提示詞裡。那不是記憶，那是包裝精美的上下文污染。我希望代理人知道過去存在，但不必把整個過去強行塞進每一次互動裡。它也許會看到某個關於失敗遷移、客戶特定限制，或某個 API 為什麼長得很怪的記憶，然後再決定這些事實是否與眼前任務有關。

這就形成了我所說的記憶檢索經濟。索引很便宜，細節會吃上下文，但這句話背後藏著一個醜陋的假設：代理人必須抓到正確的記憶。就目前而言，原型大多讓模型依據索引中的描述來決定要取什麼。這不是成熟的檢索系統，而是刻意做得很原始的基準線。一個存在但從未被取用的記憶，在功能上等同於被遺忘；而把不相關的記憶抓進來，只是多了幾步的上下文污染。因此，檢索的精確率與召回率必須成為評估的一部分，而不是我偷偷塞進「讓代理人自己決定」這句話裡的東西。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1226305621&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228762&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626812&amp;bpp=1&amp;bdt=1265&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2704&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

以上這些都不代表外部記憶是新想法。MemGPT 早就以階層式記憶與虛擬上下文管理來描述長時間運作的代理人，Letta 也把這條路線推成有狀態的代理人平台，而官方 MCP 範例裡也包含了持久化的知識圖譜記憶伺服器。對我來說更有趣的問題比較窄：當記憶是團隊範圍而不是助理範圍、是純文字且可跨客戶端攜帶、而且每一條記憶都帶有明確的信任狀態而不是自動被視為權威時，會發生什麼改變？我不是在發明記憶，而是在尋找一個組織邊界，讓它成為有用的基礎設施，而不只是另一個助理功能。

## 沒有來源的記憶，只是拿退休金的幻覺

一旦代理人可以寫入持久化記憶，另一個問題就立刻出現了：為什麼下一個代理人要相信前一個代理人寫的內容？想像一個代理人把「這個元件一定要用 Redis」存成記憶。那是團隊明確的決策、根據當前程式碼推論出的結果、暫時的權宜之計，還是某個自信但錯誤的結論，只是剛好撐過了那一輪會話？沒有來源的持久化記憶非常危險，因為錯誤資訊不會隨著對話消失。它會在你的基礎設施裡退休，並在無限未來中持續誤導後續代理人。

所以在這個系統裡，記憶是一份報告，而不是一條指令。未經審核的記憶，實際意思大致是：「Agent X 在時間 Y 說這件事是真的。」它可以有用，但證據力很弱，若要影響重要決策，應該先拿程式碼、規劃或使用者資訊來檢查。經過審核的記憶則具有更強的權威；如果 AI 修改了它，除非人類重新確認，否則審核狀態就會消失。模型不能在不發出任何聲響的情況下改寫已核准的歷史，還繼續保有核准徽章。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228762&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626816&amp;bpp=1&amp;bdt=1269&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3592&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這裡有個明顯陷阱：如果每條記憶都需要人類核准，那我只是把文件瓶頸換到另一層而已。這會完全破壞我正在主張的寫入端經濟學。所以審核不能是寫入路徑，而必須是一種升級機制。代理人應該能以低信任成本廉價地建立記憶；只有其中較少、且真正成為穩定專案指引的那一部分，才應該消耗人類審核。這條信任階梯是否能擴展仍然未證明，但至少在經濟上是自洽的：人類負責整理權威，而不是手動撰寫歷史痕跡。

聽起來很行政化，直到你開始思考持久化 AI 記憶到底意味著什麼。一旦資訊能跨會話存活，信任也必須跟著存活。一條有用的記憶需要作者、年齡、上下文、存在理由，以及和真實來源之間的關聯。它也需要能被挑戰。否則我們不是在建立組織知識，而是在建立一個充滿自信句子的資料庫，而網路已經證明，這兩者並不會自動是一回事。

## 記憶最麻煩的地方，就是它會變舊

大多數 AI 記憶示範看起來都很棒，因為它們只持續十五分鐘。你存入一些內容、取回它、模型記住了，大家皆大歡喜。把同一套系統放著跑六個月，實驗就不那麼上相了。專案會變、API 會搬家、大家會推翻原本決定，而一條記憶可以完美地被取回，卻同時已經完全是錯的。

因此，這個中心會追蹤記憶年齡，並把老舊記憶標成過時。如果某條記憶在描述程式碼，系統會明確提醒代理人把這些說法和目前實作比對。我刻意不自動刪除舊記憶，因為年齡不等於真相。四年前的一個架構決策，仍可能解釋為什麼系統有一半長成現在這樣；而今天早上才建立的記憶，可能已經完全是胡說八道。年齡應該影響的是信心，而不是是否存在。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788230356&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626824&amp;bpp=1&amp;bdt=-M&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788228766%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=8&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4563&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1038&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGsn3xRg5CVp9_vOApXYhHPGVA5nogXh3vc3EUaxtlzCZ3-003xIQygDf2ogm1qh0RQmx6B8EF7ayUQPYAKlHefQtpUIRzMJQZsoqZGrYNPKDXKVdwy4Yi48dXNQtUrx5Kiw%2CAOrYGsnExCJY-XnEWLIZBSLhCL6tNstV0ury8edWseeIWA0-UT1P_IDWeXz2ekCqguiWKT_j03JzQ9sNOrbWsWV2wFCQ_MB0QjE4WNotr1fEGgk%2CAOrYGskhyUOdc7bzqEkV6E1I1IPZg6hWcMJWJOGTNORHFeNNIVtiTrieoqdflaPyIeNBNdxY9ddTbUBygePUl5FqRNwxhe0csoe4edKZSY5KIYdkWQ%2CAOrYGsmjEkmjx-ADaXKMEQ-PV4iZyZ1opYI42Xls7OHKrvrVhL1QlZKhsA7k81-JocK6AdEygnp0KI0q6tTDDEAlhIfXP_e3yUMOpX_SeyjoSCseHw%2CAOrYGsmpdXZImj_y8n6iEqSMRwg6r9nO1Z67qI2hj3DUhyP1bWEr1BSXNkrGw7igpL0EZz6leOva4TIA4fXY-3jfv54iiXQQJo4PSxh2eWCkBR_TLA%2CAOrYGskhij4SZuZgXl2Y5-5CRRDFCsubE7qk4DYOIevfFQRHyoEkmATr1wvZidgZqOf63Uq8TkY5tpHDYYjy_NdNwzmyre0NE1H8WspbbKM&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這也讓設計方向脫離了常見的快取思維。快取在問的是某個值還能不能重複使用；記憶問的是一個更難的問題：我現在應該相信它多少？當儲存內容包含由不同代理人在不同假設下做出的數月決策時，這個差異就會變得很重要。

## 組織價值很明顯，但經濟學並不明顯

最有吸引力的賣點很容易理解。想像你剛加入一個專案，問 AI 代理人：「這個系統為什麼長這樣？」現在它可以檢查儲存庫，並解釋目前存在的是什麼；有了記憶之後，它甚至可能解釋「為什麼會存在」：為什麼某個 provider 被拒絕、為什麼一次遷移失敗，或為什麼一條本來只是臨時的相容層，現在已經進入第三年。

一個系統的真實架構，只有一部分能在程式碼裡看見。其餘部分分散在 Slack 討論、會議、被放棄的分支，以及某個工程師說的那句「不要碰那個，當初是有原因的」。不幸的是，這二十年來我們一直都在賣這個問題的解方：Wiki、Confluence、ADR、內部入口網站。每一代工具都保證這次一定能救我們。

它們都遇到同樣的經濟問題：寫文件的人要先付出成本，而未來某個人才能享受好處。AI 代理人也許能改變這點，因為代理人在工作發生時就已經在場。它看過檔案、失敗的方法、修正動作，以及決策改變的原因，所以產出一條精簡記憶的邊際成本幾乎是零。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=4060917935&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788230457&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626828&amp;bpp=1&amp;bdt=-M&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788228766%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280&amp;nras=9&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5395&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1775&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGsn3xRg5CVp9_vOApXYhHPGVA5nogXh3vc3EUaxtlzCZ3-003xIQygDf2ogm1qh0RQmx6B8EF7ayUQPYAKlHefQtpUIRzMJQZsoqZGrYNPKDXKVdwy4Yi48dXNQtUrx5Kiw%2CAOrYGsnExCJY-XnEWLIZBSLhCL6tNstV0ury8edWseeIWA0-UT1P_IDWeXz2ekCqguiWKT_j03JzQ9sNOrbWsWV2wFCQ_MB0QjE4WNotr1fEGgk%2CAOrYGskhyUOdc7bzqEkV6E1I1IPZg6hWcMJWJOGTNORHFeNNIVtiTrieoqdflaPyIeNBNdxY9ddTbUBygePUl5FqRNwxhe0csoe4edKZSY5KIYdkWQ%2CAOrYGsmjEkmjx-ADaXKMEQ-PV4iZyZ1opYI42Xls7OHKrvrVhL1QlZKhsA7k81-JocK6AdEygnp0KI0q6tTDDEAlhIfXP_e3yUMOpX_SeyjoSCseHw%2CAOrYGsmpdXZImj_y8n6iEqSMRwg6r9nO1Z67qI2hj3DUhyP1bWEr1BSXNkrGw7igpL0EZz6leOva4TIA4fXY-3jfv54iiXQQJo4PSxh2eWCkBR_TLA%2CAOrYGskhij4SZuZgXl2Y5-5CRRDFCsubE7qk4DYOIevfFQRHyoEkmATr1wvZidgZqOf63Uq8TkY5tpHDYYjy_NdNwzmyre0NE1H8WspbbKM%2CAOrYGslDc_o1T7lcER5qhLntnHbeCZN8mz8Rv66Dn-Wxw42vtcifGzQlnXXgrNSA8Mf9Fiy1zjfwrc01FhcNz7Jfr52OgQqi1dbNf8hlHxn74gGK9LI&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這比單純說 AI 會讀文件有趣得多。真正的可能性在於：AI 在做事的副產品中，就能產生歷史痕跡，而不是要求人類事後把一切補成文件。如果這件事成立，代理人就改變了許多過去知識管理系統失敗的寫入端經濟學。不過這句話裡的「如果」很重。我還沒有證明它。

## 這個原型證明了什麼，以及絕對沒證明什麼

這個原型端到端是可以運作的。外部文字記憶可以獨立於模型存在。不同客戶端可以共用同一個儲存。記憶可以有選擇地注入新的會話。寫入可以做結構驗證，來源可以由基礎設施強制，過時資訊也能被揭露，而不是默默被信任。這證明了底層基礎是可行的。

但它沒有證明這個底層真的有用。這是兩件非常不同的事，而 AI 工程界已經受夠了：週二做出一個 demo，週三就宣布自己找到了新型態的智慧。一個可運作的記憶 API，只能證明代理人可以取回先前狀態。真正重要的主張是：因為有了那個狀態，代理人是否能做出更好的工作。

記憶是有成本的。它消耗上下文、增加檢索延遲、弱記憶可能讓推理偏掉，而過時記憶可能把代理人推向陳舊的假設。因此，一個運作良好的記憶層，也可能變成一台高效率的機器，把昨天的錯誤灌進今天的會話裡。這個架構只有在它所避免的探索與修正成本，大於它本身帶來的記憶開銷時，才會創造價值。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1106437894&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788230467&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626833&amp;bpp=1&amp;bdt=-M&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788228766%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280&amp;nras=10&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6227&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2615&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGsn3xRg5CVp9_vOApXYhHPGVA5nogXh3vc3EUaxtlzCZ3-003xIQygDf2ogm1qh0RQmx6B8EF7ayUQPYAKlHefQtpUIRzMJQZsoqZGrYNPKDXKVdwy4Yi48dXNQtUrx5Kiw%2CAOrYGsnExCJY-XnEWLIZBSLhCL6tNstV0ury8edWseeIWA0-UT1P_IDWeXz2ekCqguiWKT_j03JzQ9sNOrbWsWV2wFCQ_MB0QjE4WNotr1fEGgk%2CAOrYGskhyUOdc7bzqEkV6E1I1IPZg6hWcMJWJOGTNORHFeNNIVtiTrieoqdflaPyIeNBNdxY9ddTbUBygePUl5FqRNwxhe0csoe4edKZSY5KIYdkWQ%2CAOrYGsmjEkmjx-ADaXKMEQ-PV4iZyZ1opYI42Xls7OHKrvrVhL1QlZKhsA7k81-JocK6AdEygnp0KI0q6tTDDEAlhIfXP_e3yUMOpX_SeyjoSCseHw%2CAOrYGsmpdXZImj_y8n6iEqSMRwg6r9nO1Z67qI2hj3DUhyP1bWEr1BSXNkrGw7igpL0EZz6leOva4TIA4fXY-3jfv54iiXQQJo4PSxh2eWCkBR_TLA%2CAOrYGskhij4SZuZgXl2Y5-5CRRDFCsubE7qk4DYOIevfFQRHyoEkmATr1wvZidgZqOf63Uq8TkY5tpHDYYjy_NdNwzmyre0NE1H8WspbbKM%2CAOrYGslDc_o1T7lcER5qhLntnHbeCZN8mz8Rv66Dn-Wxw42vtcifGzQlnXXgrNSA8Mf9Fiy1zjfwrc01FhcNz7Jfr52OgQqi1dbNf8hlHxn74gGK9LI%2CAOrYGsnVuW6OIyDxmWtPS_7N4p49Vm4o-VO2gujDruHQqOV9FwBIkdUqDx3QfF9RiV1gawpUlYi19V37QHWoyIQimXURm9VUSndhdahGkue1ovpm5A&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這也是我自己的假設後來改變的地方。我原本以為最明顯的收益會是更便宜的會話。有了記憶的程式碼代理人，應該能少用一些 token，因為它每次都不必重新摸索整個儲存庫。理論很漂亮。然後我超過五分鐘地想了一下，結果很不幸地把這個理論想壞了。

## 真正的成本，可能是探索稅

一個具備記憶的會話，實際上可能會用更多 token。它會先拿索引、再抓記憶、解讀來源、驗證過時說法，之後可能還是得檢查相同的原始碼。如果只看 API 花費，我很容易想像結果會是中性，甚至略微更糟。但 API 花費也許不是該看經濟效益的地方。

每個新的程式碼代理人進入儲存庫時，某種程度上都帶著專業失憶。它搜尋程式碼、重建架構、發現慣例、嘗試某件事，接著發現為什麼行不通；然後隔天又有另一個會話來，重演一個更小版本的同樣考古行動。真正昂貴的不一定是探索過程中消耗的 token。真正昂貴的是，人類又要再解釋一次：這個回應格式不能改、那個遷移我們已經試過、這個服務因為客戶端限制所以行為不同、對，我知道這看起來不對、不，拜託不要再重構它了。

到了某個時候你會發現，組織一直在為了重新發現自己早就花錢發現過的資訊而持續付費。這就是探索稅。記憶不需要完全消除它才有價值；它只要能減少足夠多的重複調查、走錯路，以及人類修正，就足以抵銷自身的開銷。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3034205192&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788230495&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227626838&amp;bpp=1&amp;bdt=-M&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788228766%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788228766%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=1809313742010&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7132&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3531&amp;eid=95399630&amp;oid=2&amp;psts=AOrYGsn3xRg5CVp9_vOApXYhHPGVA5nogXh3vc3EUaxtlzCZ3-003xIQygDf2ogm1qh0RQmx6B8EF7ayUQPYAKlHefQtpUIRzMJQZsoqZGrYNPKDXKVdwy4Yi48dXNQtUrx5Kiw%2CAOrYGsnExCJY-XnEWLIZBSLhCL6tNstV0ury8edWseeIWA0-UT1P_IDWeXz2ekCqguiWKT_j03JzQ9sNOrbWsWV2wFCQ_MB0QjE4WNotr1fEGgk%2CAOrYGskhyUOdc7bzqEkV6E1I1IPZg6hWcMJWJOGTNORHFeNNIVtiTrieoqdflaPyIeNBNdxY9ddTbUBygePUl5FqRNwxhe0csoe4edKZSY5KIYdkWQ%2CAOrYGsmjEkmjx-ADaXKMEQ-PV4iZyZ1opYI42Xls7OHKrvrVhL1QlZKhsA7k81-JocK6AdEygnp0KI0q6tTDDEAlhIfXP_e3yUMOpX_SeyjoSCseHw%2CAOrYGsmpdXZImj_y8n6iEqSMRwg6r9nO1Z67qI2hj3DUhyP1bWEr1BSXNkrGw7igpL0EZz6leOva4TIA4fXY-3jfv54iiXQQJo4PSxh2eWCkBR_TLA%2CAOrYGskhij4SZuZgXl2Y5-5CRRDFCsubE7qk4DYOIevfFQRHyoEkmATr1wvZidgZqOf63Uq8TkY5tpHDYYjy_NdNwzmyre0NE1H8WspbbKM%2CAOrYGslDc_o1T7lcER5qhLntnHbeCZN8mz8Rv66Dn-Wxw42vtcifGzQlnXXgrNSA8Mf9Fiy1zjfwrc01FhcNz7Jfr52OgQqi1dbNf8hlHxn74gGK9LI%2CAOrYGsnVuW6OIyDxmWtPS_7N4p49Vm4o-VO2gujDruHQqOV9FwBIkdUqDx3QfF9RiV1gawpUlYi19V37QHWoyIQimXURm9VUSndhdahGkue1ovpm5A%2CAOrYGskRPs0hO38aHDGu4ww5i9WZP4zBppnTIQnz0zrQkVGCXNs2MaKZwt72Qb3VlGg6gt_uyrZQfn5cTEKqocfkzZfeRvko34mRI44RFdXH1TTqqg&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這也改變了實驗應該怎麼衡量。我現在真正關心的比較，是在同一個儲存庫、同一個模型、相同任務下，開啟記憶中心與關閉記憶中心兩種條件。然後我想衡量 token 使用量，沒錯，但也要看達成可接受結果所需的實際時間、人類修正的次數、重複走錯路的次數，以及代理人違反已知團隊決策的頻率。

我目前的預測有點讓人不太舒服，因為它和我最初的論點不完全一致。token 使用量可能大致持平，甚至稍微更糟，但人類修正和重複的架構錯誤會下降。如果是這樣，那麼即使 API 帳單幾乎沒變，記憶系統在經濟上仍然是有價值的。工程師一小時的成本，仍然遠高於讓模型多讀一千個 token。

所以最強的賣點也許不是「AI 會話變便宜了」，而是更不科幻、也更實用的說法：AI 會話不再反覆重審已經定案的決策。

## 我無法加速的實驗

有一項評估我沒辦法很像樣地造假：時間。現在這個儲存庫還很新，也相對乾淨。真正有意思的失敗模式，會出現在矛盾開始累積、兩個代理人對同一事件的描述不同、專案演變速度超過儲存層、而某個錯誤假設撐得夠久，最後讓五條後續記憶都依賴它的時候。

到了那個階段，系統就不再只是檢索層，而會變成知識維護問題。來源、過時程度、整併、矛盾處理，以及最終的遺忘，都會變得重要。可惜的是，沒有一個可信的 benchmark 開關叫做 `--simulate-six-months-of-organizational-chaos` 。系統必須先老到開始變惱人，我才能學到它到底能不能在真實使用中撐住。

## 為什麼純文字可能是最重要的無聊決策

我到現在還覺得最有趣的部分，是可攜性。這些記憶是純文字。它們不是模型專屬的隱藏狀態，不是只有在某個嵌入空間裡才有意義的向量，也不是某種專有的「持久化認知表示」。文字非常無聊，而這正是我喜歡它的原因。

Claude 可以建立一條記憶，GPT 可以讀它。本地模型可以不同意它。尚未存在的模型，也可以繼承同一段專案歷史。這不代表模型可以互換。不同模型會用不同方式解讀同一條記憶，看見不同事物、採用不同推理方式，也會犯不同錯誤。權重仍然非常重要。但歷史不會再因為模型更換而消失。

這形成了一種我認為會越來越重要的分離。模型可以變成可替換的運算資源，而記憶則成為持久的組織狀態。公司已經在不同供應商之間移轉，混用本地與雲端模型，並讓多個代理人一起處理同一套系統。我們通常談互通性時，講的是工具和協定。共享記憶也許會成為這一層的另一部分。

## 也許這根本不是 AI 記憶問題

我一開始做這個實驗，是在思考怎麼讓 AI 有更好的記憶。但我現在越來越不相信 AI 才是重點。軟體團隊會忘記決策當初為何做出、方法為什麼失敗、以及哪個看起來很蠢的實作之所以存在，是因為更乾淨的版本之前就炸過一次。六個月後，某個人會「修正」它，然後在 production 裡重新發現同一個問題。

人類靠文件、經驗，以及那個記得屍體都埋在哪裡的工程師來補救這件事。每個新的 AI 會話都帶著缺少這些累積歷史的狀態而來，但代理人也確實在工作發生時就 присутствать，並能替下一個接手的人留下結構化痕跡。如果這些痕跡能保持可取回、可稽核、可攜帶，而且能抵抗過時的胡說八道，那麼記憶就不再只是助理功能，而開始像是基礎設施。

我已經證明這套基礎設施可以存在。但我還沒證明檢索能持續可靠、信任模型能擴展，或組織節省的成本會大於額外開銷。那部分還需要資料、模型替換、真實使用，以及大概再過六個月，讓我自己的系統用越來越有創意的方式證明我是錯的。

---

原文出處： [https://dev.to/marcosomma/your-ai-remembers-everything-and-trusts-all-of-it-4gg](https://dev.to/marcosomma/your-ai-remembers-everything-and-trusts-all-of-it-4gg)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xdjBYa) [49瀏覽](https://codelove.tw/@tony/post/xdjBYa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xdjBYa)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝15 💬4

206

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788227625&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxdjBYa&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788227625722&amp;bpp=3&amp;bdt=144&amp;idt=125&amp;shv=r20260825&amp;mjsv=m202608260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788227622%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788227622%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1809313742010&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630&amp;oid=2&amp;pvsid=5784764305491295&amp;tmod=496159329&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=174" title="Advertisement" aria-label="Advertisement"></iframe>