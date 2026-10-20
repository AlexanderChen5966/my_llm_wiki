---
title: "AI 工程很簡單，改變我們的工作方式才困難"
source: "https://codelove.tw/@tony/post/qzonBx"
author:
published:
created: 2026-09-07
description: "AI 工程聽起來很炫。現在到處都是新名詞：agentic development、AI-native engineering、spec-driven development，現在又多了 AI harness engineering。不過，撇開這些術語不談，真正有用的事情確實正在發生。AI 現在可以協..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月06日](https://codelove.tw/@tony/post/qzonBx "2026-09-06 08:45:09")

## AI 工程很簡單，改變我們的工作方式才困難

AI 工程聽起來很炫。現在到處都是新名詞：agentic development、AI-native engineering、spec-driven development，現在又多了 AI harness engineering。不過，撇開這些術語不談，真正有用的事情確實正在發生。AI 現在可以協助撰寫需求、挑戰 [PRD](https://en.wikipedia.org/wiki/Product_requirements_document) 、探索 UX 構想、推理架構、建立實作計畫、寫程式碼並驗證結果。

顯而易見的問題是 AI 能做什麼。更有意思的問題是，我們現在打造軟體的方式，是否已經準備好迎接它。

## 工作流程正在改變

我們一直在探索的一種工作流程，將開發拆成五個階段： **需求、細化、規劃、建置與驗證** 。這些階段本身並不新，但 AI 現在可以參與每一個階段。它可以接收現有的產品輸入，協助釐清問題、質疑假設、找出 PRD 中的缺口，然後把明確定義的需求轉換成計畫，最後變成實作任務。

這讓需求品質變得更重要。參與專案的人類可能知道「改善體驗」是什麼意思，因為他們已經為此討論過好幾次。但 agent 沒有這種共同背景。它需要明確的問題、範圍、限制、邊界情況與預期結果。這不代表要寫出超長的規格書；而是要在開始建置前，利用 AI 先把需求打磨得更精確。

AI 在這裡其實可以成為一個有點煩、但很有用的審查者，會問：當某件事失敗時會發生什麼？這個需求是否可測試？文件中的兩個部分是否彼此矛盾？我們還有哪些沒考慮到？它也可以幫忙比較不同版本的 PRD，或讓一個模型審查另一個模型的輸出，讓缺口更容易被發現。重點是，AI 在幫我們找出模糊之處，而不是替我們做決定。

## 也許瓶頸不在寫程式

當我們看團隊實際把時間花在哪裡時，這件事會更有意思。複雜的工作通常要在產品、UX、需求與工程之間來回好幾輪，開發才能真正開始。這通常是必要的，但也可能造成明顯的瓶頸。如果 AI agent 能很快做出可運作的實作，那麼為了把需求釐清而等上兩週，問題就比以前大得多。

這表示工程師需要更早參與，而不是等需求被認定為「完成」後才接手。UX 也需要更早加入討論。粗略的原型或線框圖，往往比再開一輪會議更快看出需求缺口，而 AI 讓這些輕量原型的製作成本低很多。需求、UX 與技術設計可以變成反覆迭代的循環，而不是一連串的交接。

## 文件不是越多越好

面對 AI 時，直覺往往是給它更多文件：更多 PRD、更多架構圖、更多 wiki 頁面、更多上下文。但如果好幾份文件對同一個功能的描述不一致，那我們其實不是在提供更好的上下文，而是在給 agent 更多混淆自己的方式。

agent 真正需要的是能夠理解系統的路徑。清楚的專案結構、聚焦的文件、實用的 agent 指令、能說明某個架構決策為何存在的設計記錄，以及整理良好的程式碼庫，往往都比一份超大型規格書更有幫助。目標不是把我們知道的一切都塞給 AI，而是讓 AI 在需要時能夠輕易找到它需要的東西。

## 問題可能出在 ticket 切法

AI 也讓我開始懷疑我們是怎麼切工作的。一張要花上數週甚至數月的 ticket，對 agent 來說很難推理，對人類而言，坦白說也不一定是很好的工作單位。「建置驗證機制」和把問題拆成密碼重設、權杖驗證、密碼更新以及相關測試，完全是兩回事。

這不代表要把每個功能都拆成幾十張小 ticket。我們只是需要有明確目的、可管理範圍，以及完成定義的工作。如果某件事要做幾個月，那它可能其實是一個專案，只是披著 Jira 的外衣。

同時，也不是所有事情都需要完整的 AI 生命週期。大型功能可能會受益於結構化需求、細化、規劃與驗證；但一個小型 BAU 變更，可能只需要一個 prompt 和一位開發者。如果把同一套流程套用到所有事情上，我們很可能只是用另一種官僚流程取代原本的官僚流程。工作流程應該要與工作的複雜度相符。

## agent 需要看到真實系統

還有一項相當根本的需求： **agent 必須理解真實系統** 。如果只給它 PRD，卻沒有存取相關程式碼庫，它仍然只能憑假設做事。只要有程式碼，它就能找到既有模式、理解限制、重用功能，並辨識提議的解法是否不適用。

當然，讓 AI 存取原始碼會帶來資安、授權、隱私與組織層面的考量，所以導入 AI 並不只是選對模型而已。有些最大的阻礙，根本和模型無關。

而且一旦多位開發者和多個 agent 同時並行工作，情況就會變得更有趣。每個 agent 都有自己的上下文，只能根據目前看到的內容做決策。一個 agent 可能改掉另一個 agent 不知道的東西，或者兩個 agent 都做出各自合理、但彼此搭配不起來的決定。這也讓良好的工程實務變得更重要：小幅變更、清楚邊界、良好測試、頻繁審查，以及一致的專案規則。

## 所以，我們準備好了嗎？

大概還沒完全準備好，不過這很正常。我們不需要立刻跳到自主式軟體開發。可以先挑一些真實的工作來試試這些流程，看看它們會在哪裡失效，然後在過程中持續改善。

因為 AI 改變的不只是我們寫程式的速度，它還把程式碼周邊那些拖慢我們的因素全都暴露出來：不清楚的需求、過大的 ticket、UX 介入太晚、文件漂移、交接、存取限制，以及需要花好幾天才能做出的決策。

這也是為什麼我認為 **AI harness** 的概念，比 prompt 與 agent 設定更大。harness 是我們為 AI 建立的周邊環境：我們如何定義工作、產品、UX 與工程如何協作、知識如何結構化、程式庫如何組織、如何驗證工作，以及 agent 擁有哪些存取權限。

AI 可能是新的部分，但真正決定它能不能讓我們變快的，是我們圍繞它所建立的工作方式。

**AI 工程很簡單，改變工作方式才難。**

---

原文出處： [https://dev.to/ujja/ai-engineering-is-easy-changing-how-we-work-is-hard-39j4](https://dev.to/ujja/ai-engineering-is-easy-changing-how-we-work-is-hard-39j4)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qzonBx) [13瀏覽](https://codelove.tw/@tony/post/qzonBx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qzonBx)

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

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝21

324

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788743539&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqzonBx&amp;pra=5&amp;aiof=11&amp;asro=0&amp;itsi=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788743539760&amp;bpp=4&amp;bdt=56&amp;idt=17&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788743511%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788743511%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788743511%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8606592216388&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2624&amp;eid=95399630%2C95400775%2C95401045%2C122880835%2C122880828&amp;oid=2&amp;pvsid=4943141697902967&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=21" title="Advertisement" aria-label="Advertisement"></iframe>