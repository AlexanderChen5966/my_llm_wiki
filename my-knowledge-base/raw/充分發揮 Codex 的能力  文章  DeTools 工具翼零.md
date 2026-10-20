---
title: "充分發揮 Codex 的能力 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3921"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-05-21
created: 2026-05-22
description: "Codex 正從程式助手進化為完整工作系統。本文解析 durable threads、voice input、automations、Goals 與 side panel 等新能力，說明 AI 如何跨越 repository，持續執行真實工作流程。"
tags:
  - "clippings"
status: processed
---
這篇文章整理自 Jason Liu（X：@jxnlco）對 Codex 工作流的觀察與實務建議。根據 Jason Liu 個人網站的介紹，他目前是 OpenAI Codex 團隊的 Developer Experience Engineer，也就是負責協助開發者理解、使用並拓展 Codex 應用方式的角色。

文章的重點不只是介紹 Codex 如何協助寫程式，而是進一步說明：當 Codex 能結合長期 thread、語音輸入、工具操作、瀏覽器、自動化、Goals、side panel 與 shared memory 時，它其實已經從單純的「程式碼助手」，逐漸變成一套能承接完整電腦工作流程的 agent 系統。

對開發者來說，這篇文章值得注意的地方在於，它不是單純列功能，而是從實際工作模式出發，說明如何把 Codex 用在程式碼之外的任務，例如文件審查、訊息追蹤、工作排程、網頁檢查、artifact 修正與長期專案記憶管理。也因此，這篇內容可以視為理解 Codex 下一階段使用方式的一份實務導讀。

需要注意的是，這篇文章雖然來自 OpenAI Codex 團隊成員的公開分享，但仍應視為作者個人的使用經驗與觀點整理，而不是 OpenAI 官方文件或正式產品規格。

\--

大多數開發者第一次使用 coding agents 時，都是拿來處理程式碼：檢查 repository、產生 diff、執行測試，以及開啟 pull request。

這仍然是 Codex 的核心重心。但現在，電腦上的許多工作其實早已透過程式碼來中介：執行 shell 指令、瀏覽網頁、呼叫 API、匯出文件、回應事件，以及觸發自動化流程。隨著這些介面逐漸開放給 Codex，它開始不再只是狹義上的程式助手，而更像是一套「完成電腦工作」的系統。

Codex App 將這種轉變具體化。一個 thread 可以保留上下文、使用工具、呈現產出物，並在多次提示之間持續延伸，而不是每次交流後都重新開始。

想更有效地使用 Codex，代表要把以下能力整合運用：

- 可以保留上下文的持久化 threads
- 在使用者仍參與流程時，使用語音輸入、steering 與 queuing
- browser、computer-use、MCP servers 與 connectors，讓 Codex 能在 repository 之外行動
- thread automations 與 Goals，讓工作在使用者離開後仍持續進行
- side panel，讓使用者能檢視程式碼、文件、簡報與其他產出物

## Durable threads

Durable threads：指的是能在多次工作階段之間保留工作上下文的長時間 Codex threads。

Pinned threads 是讓 durable threads 保持在手邊的一種方式。它們很適合用於重複性的工作流程，例如：

- Chief of Staff thread
- 發版 thread
- 文件審查 thread
- 專門用於外部監控的 thread

這些是持續存在的工作空間，而不是短暫聊天。Codex 可以隨時間回到這些 threads，保留先前的決策、偏好與工作上下文，而不需要每次重新建立。

Pinned-thread 快捷鍵讓這件事更實用。Command-1 到 Command-9 可以直接跳轉到已儲存的 threads。

## Voice input

語音輸入很有價值，因為它能捕捉一個想法最粗略、尚未被壓縮成精緻文字前的樣子。

Codex 內建語音輸入。它特別適合處理那些說出來很自然、打字卻很麻煩的模糊起點，例如：

「我記得有個叫 Ben 的人曾在 Slack 提過這件事。」

「我不記得細節了。」

「幫我去查一下。」

對於能搜尋、收集上下文並回報結果的 agent 來說，這些資訊通常已經足夠。

它也很適合用來進行兩到三分鐘的想法傾倒，也就是在任務還沒完全成形前，先把腦中的想法講出來。

逐字稿也有類似效果。比起簡短摘要，原始會議逐字稿或口述規劃筆記往往能提供更好的素材，因為它保留了不確定性、重點與尚未完成的思路。

## Steering and queuing

當語音功能與對執行中任務的明確控制結合時，它會更有價值。

Steering：在目前步驟完成前，直接中斷正在執行的 Codex 任務，並給予新的方向。

Steering 很適合用在 agent 走錯方向、需要在完成前修正時。例如在網站審查時，使用者可以一邊在 side panel 上標註，一邊中斷目前工作：

「把這個再小一點。」

「這兩個元素之間的間距怪怪的。」

「這段文案不對。」

Queuing：在目前步驟完成後，替 Codex 加入接下來要做的工作。

Queuing 不一樣。它不會中斷目前正在進行的任務，而是把下一個任務排到後面。使用者可能會說：

「等工作完成後，把預覽連結傳到 Slack 給 reviewer。」

Steering 改變的是 Codex「現在正在做什麼」。Queuing 改變的是「接下來要做什麼」。兩者都能讓使用者在工作展開的過程中保持接近現場。

## Tools and reach

當 thread 擁有連續性之後，下一個問題就是它能操作哪些東西。Codex 可以逐步向外延伸：

`$browser` ：side panel 內建瀏覽器，Codex 可以用它檢查與標註網頁介面。

`@chrome` ：用於需要登入狀態與 Chrome 工作流程的情境。

`@computer` ：用於只能透過桌面 GUI 完成的工作。

`$browser` 適合 side-panel 的瀏覽器審查。 `@chrome` 適合依賴使用者 Chrome 環境的登入狀態工作。 `@computer` 則適合那些只能透過桌面 GUI 存在的任務。

MCP servers 與 connectors 將同樣的概念延伸到整體工作流程中。Slack、Gmail 與 Calendar 很重要，因為很多重要工作最初其實是以訊息、信件或排程問題的形式出現，而不是程式碼。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=2301762618&amp;pi=t.aa~a.2909957178~i.87~rp.4&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779416433&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3921&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=13&amp;dt=1779416364283&amp;bpp=2&amp;bdt=1740&amp;idt=3&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1779416350%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1779416350%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Dc9ca8bae2a3b1250%3AT%3D1765424491%3ART%3D1779416350%3AS%3DAA-AfjYduexFOiKl2S-uBYfnhlcY&amp;prev_fmts=825x280%2C0x0%2C255x600%2C255x600&amp;nras=2&amp;correlator=6524292963483&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=285&amp;ady=3449&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95391807%2C95386193&amp;oid=2&amp;pvsid=3099861037430390&amp;tmod=959842395&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fpage%2F1&amp;fc=384&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=247x742_l&amp;bz=0&amp;pgls=CAs.&amp;ifi=8&amp;uci=a!8&amp;btvi=3&amp;fsb=1&amp;dtd=69114" title="Advertisement" aria-label="Advertisement"></iframe>

Skills 可以讓重複工作流程變成可重複使用的模組。一旦某個流程被證明有效，就可以把它封裝成 skill，讓 Codex 下次直接執行，而不用重新學習整套流程。

## Work from anywhere

Codex mobile app 改變了使用者「必須待在桌前」這件事。任務可以先在 Mac 上開始，因為檔案、權限與本地設定都已經存在，之後使用者再透過手機繼續追蹤。

這在許多小情境中特別有意義。使用者可以離開桌面，讓 Codex 執行較長的任務；人在外面時回答問題、批准下一步，或在回到座位前重新導向 thread。本地環境會保持在原處，使用者不必一直待在電腦前。

## Automations

Automations 能讓 Codex 按照排程執行工作。當重複性工作應該從一個工作空間重新開始時，例如每日報告或定期 repository 檢查，可以使用 scheduled automation。當排程應該回到同一個持續中的 conversation 與其執行中的上下文時，則可以使用 thread automation。

Thread automations：像心跳一樣，按照排程定期喚醒同一個 Codex thread 的機制。

Pinned threads 很有用，但它們仍然要等使用者回來。Thread automation 則可以每幾分鐘或每幾小時檢查某件事，持續執行直到條件達成，並且隨時間調整頻率。

一個 Chief of Staff thread 可能每 30 分鐘執行一次：

「每 30 分鐘檢查 Slack 與 Gmail 中有哪些尚未回覆、且需要我注意的訊息。」

「幫我判斷哪些事情最重要。」

「如果有人問我問題，請盡可能深入研究答案，並替我草擬回覆，但不要直接送出。」

當使用者回來時，最耗時的上下文收集工作通常已經完成。人類仍然決定哪些內容要送出。

Thread automations 也適合處理 feedback loops。Thread automation 可以監看 pull request comments、Google Docs 留言或 Slack 回覆，並在使用者離開時持續推進相關工作。

以動畫工作流程為例，當 reviewer 在 Slack 分享一支影片時，thread automation 可以按照排程檢查該 thread；當有評論出現時，自動重新 render 更新版本，並在同一個 thread 中標記 reviewer 回覆。如果某個整合無法完成最後上傳，desktop automation 也可以透過 GUI 完成最後一步。

這個循環橫跨 Slack 的回饋、codebase 中的 render，以及 desktop automation 完成最後上傳。

## Goals

Goals 在任務有真正終點、且 agent 可以持續朝這個終點推進時最有威力。

Goals：指的是具有完成終點、且 agent 能隨時間持續推進的長時間 Codex 任務。

一個較弱的 goal 可能是：

「根據這份 Markdown 文件實作計畫。」

更強的 goal 則會有可衡量的成功條件。

例如，一位工程師可能要把內部工具從 Python 遷移到 Rust。他可以先建立新的目錄、定義 goal，並明確指定完成條件：只有當新實作的單元測試通過時，這項工作才算完成。

一個 goal 結合了持續執行與 verifier。使用者會定義結果、停止條件，以及判斷 Codex 是否正在接近目標的訊號。

有用的 verifier 包括：

- 測試套件
- benchmark
- bug 重現流程
- 驗證矩陣
- 必須持續通過的 end-to-end workflow

有野心很重要，但如果沒有驗證機制，那就只是願望。

## The side panel

Side panel 會把工作成果保留在產生它的 conversation 旁邊。使用者不需要匯出 artifact 再切換脈絡，而是可以直接在原處檢視。輸出物可能是程式碼，也可能是簡報、PDF、瀏覽器頁面、資料表，或其他在過程中建立出來的 artifact。

它特別適合支援四種工作：

- 檢查 artifacts
- 標註需要修改的地方
- 操作網頁介面
- 審查變更

Side panel 讓使用者可以在原處檢視 Markdown、試算表、資料表、文件與投影片。他們可以檢查、標記並修正 artifacts，而不會打斷整個工作循環。

![](https://farm66.static.flickr.com/65535/55284327656_b904795f02_b.jpg)

Annotations

簡報或 PDF 可以保持開啟在產生它的 thread 旁邊，方便直接檢視與修正。 ![](https://farm66.static.flickr.com/65535/55284561154_c45f0d2189_b.jpg)

Sheets in Codex

Codex 的 in-app browser 可以檢查已渲染的頁面、控制頁面，並直接回應使用者在受檢視畫面上的標註。頁面或 artifact 上的評論會留在同一個工作循環中，而不是變成另一個額外交接。

Web 不只是輸出，也是控制介面。Codex 可以建立 artifact，在 side panel 中打開它、檢查它、除錯它，並在同一個物件上持續修正。

![](https://farm66.static.flickr.com/65535/55284734750_dc16b994bd_b.jpg)

這些介面特別適合這種流程：

- `index.html` ：輕量靜態 artifacts
- Storybook：UI 審查
- Remotion Studio：程式化動畫
- 瀏覽器型投影片簡報
- 用於分析工作流程的資料應用程式

單一 `index.html` 檔案就能成為持久化的互動式 artifact，而且不需要伺服器。Thread automations 也可以隨時間更新靜態 artifacts，讓使用者回來時，thread 裡已經有新的內容等著他。

## Shared memory

當長時間 threads 能在單一 conversation 之外共享記憶時，它們會變得更有用。

Shared memory：指的是儲存在單一 thread 之外的持久化上下文，讓未來工作可以從明確、可檢視的內容接續。

一種持久化的實用模式，是把 persistent threads 錨定在 Obsidian vault 中。實際上，這代表一個由純文字檔組成的資料夾，方便檢查、編輯、搬移與長期保存。團隊可以把這個資料夾放在雲端儲存、Git、Dropbox、Google Drive，或任何適合他們工作流程的同步系統中。

一個 vault 可能長這樣：

vault/  
├── TODO.md  
├── people/  
├── projects/  
├── agent/  
└── notes/

在最上層，AGENTS.md 可以定義 Codex 應該如何更新這個工作空間，隨著它逐漸理解人物、專案、決策與尚未關閉的事項。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=720102159&amp;pi=t.aa~a.2909957178~i.191~rp.4&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779416443&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3921&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=13&amp;dt=1779416364297&amp;bpp=1&amp;bdt=1755&amp;idt=1&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1779416350%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1779416350%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Dc9ca8bae2a3b1250%3AT%3D1765424491%3ART%3D1779416350%3AS%3DAA-AfjYduexFOiKl2S-uBYfnhlcY&amp;prev_fmts=825x280%2C0x0%2C255x600%2C255x600%2C825x280&amp;nras=3&amp;correlator=6524292963483&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=285&amp;ady=7909&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=4167&amp;eid=95391807%2C95386193&amp;oid=2&amp;psts=AOrYGskp1gTogW5p5t1d_FSPBMdLCjHFpZMLg_RLsG4-HiWpIf9Vdqd3QIMWIqhru_SXZnmYw_X3JmVdB-fscmDfrYHki-ZLcuZns60plEUxFiaaLYU&amp;pvsid=3099861037430390&amp;tmod=959842395&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fpage%2F1&amp;fc=384&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=247x742_l&amp;bz=1.14&amp;pgls=CAs.&amp;ifi=9&amp;uci=a!9&amp;btvi=4&amp;fsb=1&amp;dtd=78924" title="Advertisement" aria-label="Advertisement"></iframe>

不要直接複製某一種固定的 vault 結構。重點是教會 agent：持久化上下文應該存放在哪裡、哪些上下文應該保存，以及什麼時候不要造成不必要的頻繁更新。

一份實用的 AGENTS.md 可能會這樣寫：

- 將 `~/vault` 視為持久化工作記憶。
- 優先使用正式主筆記，而不是製造大量零散筆記。
- 明確分類 TODO、人物、專案、每日摘要與臨時筆記。
- 保留決策、阻塞點、負責人、日期與有用連結。
- 如果沒有實質變化，就不要頻繁改動 vault。

Repositories 保存程式碼；vault 保存持續演進的上下文：參與的人、發生了什麼變化、目前卡在哪裡、哪些事情需要追蹤，以及那些原本會在不同工作階段之間消失的資訊。

重要的上下文不應該只存在於 conversation transcript 裡。應該把它寫到某個地方，讓下一個 thread 能夠接著使用。

Codex 也在 Settings > Personalization > Memories 中提供第一方記憶功能。它們提供一層本地回憶能力，用來保存偏好、重複工作流程與已知陷阱。這些功能是補充明確寫下的上下文，而不是取代它。Chronicle 也朝著同樣方向前進，透過最近的螢幕上下文，幫助 Codex 建立記憶。

## From code outward

Codex 仍然從程式碼開始。但現在，更多圍繞程式碼的工作，也已經能透過同一套系統觸及：MCP servers、browser surfaces、desktop controls、thread automations，以及可審查的 artifacts。

這改變了整個控制模式。Steering 會中斷正在進行的工作。Queuing 會安排下一個任務。Thread automations 會在使用者離開時讓 thread 保持活躍。Goals 則替 Codex 加入明確的完成終點，讓它可以持續朝目標推進。

現在，即使工作離開了 repository，Codex 也能把一個工作流程從指令、執行，一路帶到 artifact 審查。

[原文連結](https://x.com/jxnlco/status/2057153744630890620)