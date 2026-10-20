---
title: "Agentic Engineering 的 8 個等級 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3832"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-03-20
created: 2026-05-18
description: "本文整理 AI Coding Agent 落地實踐的 8 個等級，從 Tab Complete、Context Engineering、MCP 與 Skills，到 Background Agents 與 Autonomous Agent Teams，帶你理解團隊生產力差距真正來自哪裡，以及如何一步步縮小 AI 能力與工程實踐之間的落差。"
tags:
  - "clippings"
status: processed
---
[翻譯自 The 8 Levels of Agentic Engineering，可以檢視目前自己的開發方式](https://www.bassimeledath.com/blog/levels-of-agentic-engineering)

\--

AI 的程式撰寫能力正超越我們有效運用它的能力。這也是為什麼在 SWE-bench 分數上拼命追求極致，卻無法與工程管理層真正關心的生產力指標對齊。當 Anthropic 的團隊能在 10 天內推出像 Cowork 這樣的產品，而另一個團隊使用相同模型卻無法突破一個壞掉的 POC，其差異在於：一個團隊已經縮小了能力與實踐之間的落差，而另一個團隊尚未做到。

這個落差不會在一夜之間消失。它是透過不同的等級逐步縮小的。總共有 8 個等級。你們大多數人可能已經超過前幾個等級，而且應該渴望達到下一個等級，因為每往上一個等級，產出都會有巨大的飛躍，而模型能力的進步會進一步放大這些提升。

另一個你應該關心的原因是多人效應。你的產出比你想像中更依賴你的隊友的等級。假設你是等級 7 的高手，在你睡覺時透過背景代理提交了多個優質 PR。如果你的 repo 在合併前需要同事審核，而那位同事是等級 2，仍然在手動審查 PR，那會壓制你的產出。因此，把你的團隊整體往上拉，對你來說是符合自身利益的。

從與多個使用 AI 輔助程式開發的團隊與個人交流中，我觀察到以下這樣的等級進展，雖然並非完全線性：

## Levels 1 & 2：Tab Complete 與 Agent IDE

我會快速帶過這兩個等級，主要是為了完整性。可以自由略讀。

一切始於 Copilot 與 tab 補全。按下 tab，自動補齊程式碼。對許多人來說這可能早已被遺忘，或對新進入 agentic engineering 的人而言甚至直接跳過。它偏向對經驗豐富的開發者有利，因為他們能先建立程式骨架，再由 AI 填補細節。

以 AI 為核心的 IDE，例如 Cursor，透過將聊天與你的程式碼庫連接，改變了遊戲規則，使跨多檔案的修改變得容易許多。但其上限始終受到 context 限制。模型只能幫助它看得到的內容，而令人困擾的是，它常常不是沒有看到正確的 context，就是看到了太多錯誤的 context。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=828129217&amp;pi=t.aa~a.2909957178~i.23~rp.4&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779082369&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3832&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=13&amp;dt=1779082369490&amp;bpp=1&amp;bdt=1066&amp;idt=-M&amp;shv=r20260512&amp;mjsv=m202605120101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1779082252%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1779082252%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Dc9ca8bae2a3b1250%3AT%3D1765424491%3ART%3D1779082252%3AS%3DAA-AfjYduexFOiKl2S-uBYfnhlcY&amp;prev_fmts=825x280%2C255x600%2C255x600%2C0x0&amp;nras=2&amp;correlator=6561042982208&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=7&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=271&amp;ady=1874&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;oid=2&amp;pvsid=8426934295606093&amp;tmod=1064401766&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fpage%2F2&amp;fc=384&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=243x742_l&amp;bz=1.15&amp;pgls=CAs.&amp;ifi=16&amp;uci=a!g&amp;btvi=3&amp;fsb=1&amp;dtd=8" title="Advertisement" aria-label="Advertisement"></iframe>

在這個等級，大多數人也會在其選擇的 coding agent 中嘗試 plan mode：將一個模糊的想法轉換為結構化的逐步計畫，讓 LLM 依此執行，並對該計畫進行迭代，然後再觸發實作。在這個階段這樣的方式運作良好，也是維持控制的一種合理方式。不過我們會在後面的等級看到，對 plan mode 的依賴會逐漸減少。

## Level 3：Context Engineering

2025 年的年度關鍵詞，context engineering 成為一件重要的事情，因為模型已經足夠穩定，能在適當數量的指令與剛好的 context 下可靠地執行任務。過多雜訊的 context 與描述不足的 context 一樣糟糕，因此重點在於提升每個 token 的資訊密度。「每一個 token 都必須為自己的存在而戰」成為口號。

![](https://farm66.static.flickr.com/65535/55157603228_a39a121c2e_b.jpg)  
同樣的訊息，用更少的 token 表達——資訊密度是關鍵（來源：humanlayer/12-factor-agents）

在實務上，context engineering 涉及的範圍比多數人想像的更廣。它包含你的 system prompt 與規則檔（例如.cursorrules、CLAUDE.md）。它也包含你如何描述工具，因為模型會閱讀這些描述來決定要呼叫哪些工具。它包含對對話歷史的管理，確保長時間運行的 agent 不會在第十輪後失去主軸。它還包含每一輪要暴露哪些工具，因為過多的選項會讓模型像人類一樣不知所措。

現在你已經比較少聽到 context engineering 這個詞。天平已經傾向於更能容忍雜訊並能在混亂情境中推理的模型（更大的 context window 也有所幫助）。然而，對於 context 使用的敏感度仍然重要。以下是一些仍然會受到影響的情境：

較小的模型對 context 更敏感。語音應用通常使用較小的模型，而 context 大小也會影響第一個 token 的產生時間，進而影響延遲。  
高 token 消耗的工具與模態。例如 Playwright 類型的 MCP 與影像輸入會快速消耗 token，使你比預期更早進入 Claude Code 的「compact session」狀態。  
擁有數十個工具的 agent，模型花費更多 token 在解析工具 schema，而非做實際工作。

更廣泛的重點是，context engineering 並沒有消失，它只是進化了。焦點從過濾不良 context，轉為確保正確的 context 在正確的時間出現。這個轉變為第 4 等級鋪路。

## Level 4：Compounding Engineering

Context engineering 改善的是當前 session。Compounding engineering 改善的是之後每一次 session。由 Kieran Klaassen 推廣，compounding engineering 成為一個轉折點，不只對我，對許多人來說都讓「vibe coding」不再只是原型開發。

它是一個 plan、delegate、assess、codify 的循環。你用足夠的 context 規劃任務，讓 LLM 能成功完成。你將任務委派出去。你評估輸出結果。然後關鍵在於，你將學到的東西寫入：什麼有效、什麼失敗、下次應該遵循什麼模式。

![](https://farm66.static.flickr.com/65535/55157828450_37fbf200c1_b.jpg)  
這個循環：plan、delegate、assess、codify——每一輪都讓下一輪更好

codify 這一步使其產生複利效果。LLM 是無狀態的。如果它重新引入你昨天剛移除的依賴，那麼除非你明確告訴它不要這樣做，否則明天它還會再做一次。最常見的做法是更新 CLAUDE.md（或等效的規則檔），讓這些經驗內建於未來的每一次 session 中。一個提醒：將所有東西都寫入規則檔的本能可能會適得其反（太多指令等於沒有指令）。更好的做法是建立一個環境，使 LLM 能夠自行輕易找到有用的 context，例如維護一個最新的 docs/ 資料夾（這會在第 7 等級再提到）。

實踐 compounding engineering 的人通常對輸入給 LLM 的 context 非常敏感。當 LLM 出錯時，他們會本能地先思考是否缺少 context，而不是質疑模型能力。這種直覺讓第 5 到第 8 等級成為可能。

## Level 5：MCP and Skills

Level 3 與 Level 4 解決的是 context。Level 5 解決的是能力。MCP 與自訂 skills 讓你的 LLM 能夠存取你的資料庫、你的 API、你的 CI pipeline、你的設計系統、用於瀏覽器測試的 Playwright，以及用於通知的 Slack。模型不再只是思考你的 codebase，而是可以對它採取行動。

關於 MCP 與 skills 已經有很多優質資料，因此我不會重複解釋它們是什麼。但以下是一些我使用它們的例子：我的團隊共享一個 PR review skill，我們持續對它進行迭代（現在仍然如此），它會根據 PR 的性質，有條件地啟動子代理。一個負責資料庫整合安全性。另一個執行複雜度分析，以標記冗餘或過度設計。另一個檢查 prompt 的健康狀態，以確保 prompt 符合團隊的標準格式。它同時也會執行 linters 與 Ruff。

![](https://farm66.static.flickr.com/65535/55156551402_27c9af9900_b.jpg)  
一個 PR 會觸發一個 review skill，進而分派給多個專門的子代理——每個代理檢查不同的品質面向

為什麼要對 review skill 投入這麼多？因為當 agent 開始大量產出 PR 時，人類的審查會成為瓶頸，而不是品質把關。Latent Space 提出了一個有說服力的觀點：我們熟知的程式碼審查已經死亡。取而代之的是自動化、一致且由 skill 驅動的審查。

在 MCP 方面，我使用 Braintrust MCP，讓我的 LLM 能夠查詢評估日誌並直接進行修改。我使用 DeepWiki MCP，讓我的 agent 能夠存取任何開源 repo 的文件，而不需要手動將其拉進 context。

當團隊中有多個人各自撰寫相同 skill 的不同版本時，就值得將它們整合成共享 registry。Block（節哀）有一篇很好的說明：他們建立了一個內部 skills 市集，包含超過 100 個 skills，並為特定角色與團隊提供精選套件。skills 會像程式碼一樣被管理：PR、審查、版本歷史。

還有一個值得注意的趨勢：LLM 越來越常使用 CLI 工具，而不是 MCP（幾乎每家公司都在推出：Google Workspace CLI、Braintrust 即將推出）。原因是 token 效率。MCP 伺服器會在每一輪將完整工具 schema 注入 context，不論 agent 是否使用。CLI 則相反：agent 執行特定指令，只有相關輸出會進入 context。我大量使用 agent-browser，就是基於這個原因，而不是使用 Playwright MCP。

在繼續之前，有一點很重要。Level 3 到 Level 5 是後續一切的基礎。LLM 在某些事情上表現出奇地好，在其他方面則表現不佳，你需要建立對這些邊界的直覺，才能在其上疊加更多自動化。如果你的 context 混亂、prompt 不完整或描述錯誤、工具描述不清楚，那麼 Level 6 到 Level 8 只會放大混亂。

## Level 6：Harness Engineering & Automated Feedback Loops

這就是火箭真正開始發射的地方。

Context engineering 是在整理模型看到的內容。Harness engineering 則是建立整個環境、工具與回饋迴圈，使 agent 能在沒有你介入的情況下穩定運作。給 agent 的不只是編輯器，而是回饋機制。

![](https://farm66.static.flickr.com/65535/55157685804_20445957f3_b.jpg)  
OpenAI 的 Codex harness——一整套觀測系統整合到 agent 中，使其能查詢、關聯並推理自己的輸出（來源：OpenAI）

OpenAI 的 Codex 團隊將 Chrome DevTools、觀測工具與瀏覽器操作整合到 agent runtime，使其可以截圖、操作 UI、查詢日誌並驗證自己的修復。給定一個 prompt，agent 可以重現 bug、錄製影片並實作修復。接著它會透過操作應用進行驗證、開 PR、回應 review，並合併，只有在需要判斷時才升級給人類。agent 不只是寫程式，它能看到程式的結果並進行迭代，就像人類一樣。

我的團隊開發用於技術故障排除的語音與聊天 agent，因此我建立了一個 CLI 工具 converse，讓任何 LLM 都能與我們的後端端點進行逐輪對話。LLM 會修改程式碼，使用 converse 測試對話流程，並持續迭代。有時這些自我改善的迴圈會持續數小時。當結果可驗證時特別強大：對話必須遵循特定流程，或在特定情境下呼叫工具（例如轉接真人客服）。

支撐這一切的概念是 backpressure：自動化回饋機制（型別系統、測試、lint、pre-commit hooks），讓 agent 能在沒有人工介入下偵測並修正錯誤。如果你想要自主性，你就需要 backpressure。否則你只會得到一台產出垃圾的機器。這也延伸到安全性。Vercel 的 CTO 指出，agent、生成的程式碼與你的機密應該位於不同的信任區域，因為埋在日誌中的 prompt injection 可能誘使 agent 洩漏憑證。安全邊界就是 backpressure：它限制 agent 在失控時能做什麼，而不只是它應該做什麼。

這裡有兩件事有幫助：

為吞吐量設計，而不是完美。當每次 commit 都要求完美時，agent 會反覆處理同一個 bug，甚至覆蓋彼此的修復。更好的方式是允許小型非阻塞錯誤，並在發布前做最終品質檢查。我們對人類同事也是如此。

限制大於指令。逐步提示（「先做 A，再做 B，再做 C」）正在過時。根據我的經驗，定義邊界比提供檢查清單更有效，因為 agent 會執著於清單而忽略其他內容。更好的 prompt 是：「這是我想要的結果，直到通過所有測試為止。」

Harness engineering 的另一半，是確保 agent 能在沒有你的情況下導航你的 repo。OpenAI 的做法是：將 AGENTS.md 控制在約 100 行，作為目錄，指向其他結構化文件，並將文件更新納入 CI，而不是依賴容易過時的手動更新。

當你建立了這一切後，自然會出現一個問題：如果 agent 能驗證自己的工作、導航 repo 並自行修正錯誤，那你為什麼還需要坐在電腦前？

提醒一下，對於處於早期等級的人，接下來的內容可能會顯得陌生（但可以先收藏之後再回來看）。

## Level 7：Background Agents

熱門觀點：plan mode 正在消失。

Claude Code 的創作者 Boris Cherny，目前仍然在 80% 的任務中使用 plan mode。但隨著每一代模型的進步，在規劃後的一次成功率持續上升。我認為我們正接近一個臨界點：plan mode 作為獨立的人類介入步驟會逐漸消失。不是因為規劃不重要，而是因為模型已經足夠擅長自行規劃。前提是你已經完成 Level 3 到 Level 6 的工作。如果你的 context 乾淨、限制明確、工具描述清楚、回饋迴圈完善，模型可以可靠地自行規劃。如果沒有，你仍然需要監督。

需要澄清的是，規劃本身不會消失，只是形式改變。對新手而言，plan mode 仍然是正確的起點（如 Level 1 與 2 所述）。但在 Level 7 的複雜功能中，「規劃」更像探索：探索 codebase、在 worktree 中建立原型、映射解決方案空間。而且越來越多是由背景代理替你完成這些探索。

這很重要，因為這正是解鎖背景代理的關鍵。如果 agent 能生成可靠計畫並執行，而不需要你批准，它就可以在你做其他事情時非同步運作。這是從「我在切換多個分頁」到「工作在我不在時持續進行」的關鍵轉變。

Ralph loop 是常見的入門方式：一個自動化 agent 迴圈，重複執行 coding CLI，直到所有 PRD 項目完成，每次迭代都使用新的乾淨 context。根據我的經驗，Ralph loop 很難設計，PRD 的任何不完整都會帶來問題，過於接近「丟了就不管」。

你可以並行執行多個 Ralph loop，但當 agent 數量增加，你會發現你的時間花在協調、排序、檢查與推進，而不是寫程式。你已經變成中階管理者。你需要一個 orchestrator agent 來負責調度，讓你專注於意圖，而不是物流。

Dispatch 同時在 3 個模型上啟動 5 個 worker——你的主 session 保持精簡，而 agent 在背後工作  
Dispatch 同時在 3 個模型上啟動 5 個 worker——你的主 session 保持精簡，而 agent 在背後工作

我大量使用的工具是 Dispatch，一個我為 Claude Code 建立的 skill，它將你的 session 轉為指揮中心。你維持單一乾淨 session，而 worker 在隔離 context 中執行工作。dispatcher 負責規劃、委派與追蹤，讓主 context 保持可控。當 worker 卡住時，它會提出問題，而不是靜默失敗。

Dispatch 在本地運行，適合快速開發：回饋快、容易除錯、無需基礎設施。Ramp 的 Inspect 則適合長時間自主任務：每個 agent 在雲端 sandbox VM 中運行。PM 在 Slack 發現 UI bug，Inspect 會接手處理，即使你的電腦關閉。代價是運營複雜度，但能換取規模與可重現性。我的建議是兩者並用。

在這個等級，一個非常有效的模式是：用不同模型做不同工作。優秀的工程團隊不是由複製人組成，而是由不同背景與思維的人組成。LLM 也是如此。我會用 Opus 實作、Gemini 探索、Codex 做 review，整體產出比單一模型更強。

同時，你必須將實作者與審查者分離。如果同一模型同時實作與評估，它會有偏誤，忽略問題並錯誤地認為任務已完成。這不是惡意，而是人類也不會自己批改自己的考卷。用不同模型或不同 prompt 做 review，品質會大幅提升。

![](https://farm66.static.flickr.com/65535/55157603248_6f5674dbe9_b.jpg)  
不要讓同一模型批改自己的考卷——將實作與審查分離

背景代理也讓你能將 CI 與 AI 結合。例如：每次 merge 自動更新文件、修復安全問題、升級依賴並執行測試。良好的 context、複利規則、強大的工具與自動回饋迴圈，現在都能自動運行。

## Level 8：Autonomous Agent Teams

目前還沒有人完全掌握這個等級，但已有一些團隊在嘗試。

在 Level 7 中，是一個 orchestrator LLM 將工作分派給 worker LLM（hub-and-spoke）。Level 8 則移除這個瓶頸。agent 彼此直接協作，自行認領任務、分享資訊、標記依賴、解決衝突，而不需要透過中央協調。

Claude Code 的 Agent Teams 功能是早期實作：多個 instance 在同一 codebase 並行工作，各自擁有 context 並直接溝通。Anthropic 使用 16 個 agent 建立一個可編譯 Linux 的 C 編譯器。Cursor 使用數百個 agent 連續數週建立瀏覽器並遷移 codebase。

但仔細看會發現問題。Cursor 發現沒有階層時，agent 變得保守且停滯。Anthropic 的 agent 在沒有 CI 時會破壞既有功能。所有實驗者都同意：多 agent 協作仍然很困難，尚未達最佳狀態。

我認為模型目前還不足以支撐這個等級的自主性，而且速度慢、token 成本高，對多數日常工作來說不划算。Level 7 才是目前最有槓桿的地方。未來 Level 8 可能成為主流，但現在應該專注於 Level 7。

## Level?

不可避免的下一個問題：接下來是什麼？

當你能順暢地協調 agent 團隊後，介面不需要再只是文字。語音對語音（甚至思維對思維）的互動將成為自然發展。看著你的應用，用語音描述修改，並即時看到變化。

很多人追求完美的一次生成（one-shot）。但這假設人類知道自己想要什麼，而事實並非如此。軟體開發一直都是迭代的，未來也會如此。只是會變得更容易、更快，並超越純文字互動。

所以：你現在在哪個等級？  
你正在做什麼，讓自己邁向下一個等級？