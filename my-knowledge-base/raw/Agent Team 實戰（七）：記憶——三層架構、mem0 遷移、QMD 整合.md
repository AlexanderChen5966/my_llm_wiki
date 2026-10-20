---
title: "Agent Team 實戰（七）：記憶——三層架構、mem0 遷移、QMD 整合"
source: "https://emilwu.tw/articles/29-agent-team-memory/"
author:
  - "[[EMil Wu]]"
published: 2026-04-18
created: 2026-09-02
description: "Agent Team 的記憶不是一個系統，是三個不同用途的系統疊在一起。State 記「正在做什麼」、Wiki 記「我們知道什麼」、mem0 記「為什麼這樣決定」——三層的更新頻率、查詢模式、同步方式完全不同。mem0 從本機 Chroma 遷移到 Qdrant Cloud 解決了跨機器同步，QMD 則為 wiki 加上了搜尋介面，讓 token 成本不再隨 wiki 大小線性增長。"
tags:
  - "clippings"
status: processed
---
![三層記憶架構：State、Wiki、mem0 分別處理不同的持久化需求](https://emilwu.tw/images/articles/29-agent-team-memory/hero-light.webp)

記憶不是一個系統，是三個不同用途的系統疊在一起——每一層有自己的更新頻率、同步方式和擁有者

> **一句話：** Agent Team 的記憶不是一個系統，是三個不同用途的系統疊在一起。

---

前一篇說到 Agent Em 的知識系統，我們接著說跟知識系統有關的記憶系統。

我在專門帶出門的 Macbook Air 上跟 GM 討論了一個設計決策，具體是關於 Em 和 C7 在做平行處理的時候，完成報告的格式應該包含哪些欄位、用什麼結構回報給 GM，討論了十幾分鐘，做了幾個判斷，結束 session。

隔天我打開我桌上的老 Macbook Pro，啟動 GM，問他「我們之前決定 completion report 的格式是什麼？」，他不知道，其實這是一個很常見的場景，因為他根本不記得有這段對話，那段對話發生在我帶出門的 Macbook Air 上，所以這段對話的 session 歷史儲存在那台機器的 `~/.claude/` 裡，換一台機器當然會「失憶」。

這揭露了一個我們需要接著要思考的問題： **Agent 的記憶問題不是「能不能記住？」，而是「在哪裡記住？」。** Context window 夠大、session 歷史可以 resume，這些都是「單台機器、單一 session」標準工作流程，但當你的工作跨越多台機器、多個 session、多個 Agent 的時候，「記住」這件事突然變成了一個分散式系統的問題，甚至你要創建一個給多人使用線上系統，怎麼管理每個人的 Repo, Agent, Memory，頓時成了一個 IT 級別的分散式記憶管理問題。

---

## 三層記憶：State、Wiki、mem0

萬事起頭難，經過了 Agent Em 的嘗試，我知道這次我要更小心，所以我設計了一個三層的記憶架構 \[2\] \[10\]，不是因為我喜歡把東西分成三層（前面好像一直用到 3 這個數字？），而是因為我在實際跟 Agent 協作中碰到的三種需要 Agent「記住」的需求場景，他們在性質上完全不同，如果塞在同一個系統裡會互相干擾（不過也有研究者認為，動態自組織記憶可能比靜態分層更有適應性 \[3\]，不過用 AI 就是這樣，今天的黃金定律或許明天就被推翻，先動手做反而可以學到更多），所以我把這三種需求場景定義如下：

**State（正在做什麼）：** 最不穩定的層，更新頻率是每次的 Prompt/Session 結束 ，內容是「目前誰在做什麼、做到哪裡了、下一步是什麼」。存在 `agent-GM/state/team-status.md` ，GM 每次開 session 都會讀，做完事就更新，走 Git 同步，所以跨機器可見。

**Wiki（我們知道什麼）：** 中等穩定，更新頻率以天計，我每次結束一天工作做 Wrap Up(之前提過的 /tom 的進化版)都會更新，內容是 Em 編譯過的結構化知識，concept 頁面、entity 頁面、交叉引用、分析報告（上一篇講了很多）。存在 `agent-Em/wiki/` ，走 Git 同步。任何 Agent 都可以讀，但只有 Em 可以寫。

**mem0（為什麼做這個決定）：** 最穩定的層 \[1\]，更新頻率是即時，內容是跨 session 的決策記憶，比如「我們決定 GM 管內部狀態、G7 管外部 fleet」「Em refactor 的原因是 CLAUDE.md 違反了 wiki 裡的方法論」這種判斷和理由，存在 Qdrant Cloud \[4\]，走雲端同步，跨機器即時可見。

| 層次 | 內容 | 更新頻率 | 同步方式 | 擁有者 |
| --- | --- | --- | --- | --- |
| State | 目前在做什麼 | Session | Git | GM |
| Wiki | 我們知道什麼 | Daily | Git | Em |
| mem0 | 為什麼這樣決定 | Real-Time | Qdrant Cloud | GM（Phase A） |

<svg id="mermaid-474zv09-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 540.66015625px;" viewBox="0 0 540.66015625 1000" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-474zv09-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-474zv09-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-474zv09-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-474zv09-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-474zv09-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-474zv09-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g></g><g><path d="M405.543,666L405.543,672.167C405.543,678.333,405.543,690.667,397.505,702.818C389.467,714.969,373.39,726.938,365.352,732.922L357.314,738.907" id="L_state_wiki_0" style=";" data-edge="true" data-et="edge" data-id="L_state_wiki_0" data-points="W3sieCI6NDA1LjU0Mjk2ODc1LCJ5Ijo2NjZ9LHsieCI6NDA1LjU0Mjk2ODc1LCJ5Ijo3MDN9LHsieCI6MzU0LjEwNTQ2ODc1LCJ5Ijo3NDEuMjk1NDYxMDMzNH1d" marker-end="url(#mermaid-474zv09-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M104.981,740L100.986,733.833C96.991,727.667,89.001,715.333,85.007,684C81.012,652.667,81.012,602.333,81.012,552C81.012,501.667,81.012,451.333,82.929,420.63C84.846,389.927,88.68,378.853,90.597,373.317L92.515,367.78" id="L_wiki_mem0_0" style=";" data-edge="true" data-et="edge" data-id="L_wiki_mem0_0" data-points="W3sieCI6MTA0Ljk4MDg1MjE4NTU4MjgyLCJ5Ijo3NDB9LHsieCI6ODEuMDExNzE4NzUsInkiOjcwM30seyJ4Ijo4MS4wMTE3MTg3NSwieSI6NTUyfSx7IngiOjgxLjAxMTcxODc1LCJ5Ijo0MDF9LHsieCI6OTMuODIzMzEzMzI3ODE0NTYsInkiOjM2NH1d" marker-end="url(#mermaid-474zv09-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M255.802,364L262.429,370.167C269.056,376.333,282.309,388.667,293.035,400.461C303.76,412.256,311.958,423.511,316.057,429.139L320.156,434.767" id="L_mem0_state_0" style=";" data-edge="true" data-et="edge" data-id="L_mem0_state_0" data-points="W3sieCI6MjU1LjgwMjA0ODg0MTA1OTYyLCJ5IjozNjR9LHsieCI6Mjk1LjU2MjUsInkiOjQwMX0seyJ4IjozMjIuNTExMzU2NTgxMTI1OCwieSI6NDM4fV0=" marker-end="url(#mermaid-474zv09-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M465.828,277L465.828,297.667C465.828,318.333,465.828,359.667,463.613,385.881C461.399,412.095,456.969,423.19,454.754,428.738L452.539,434.285" id="L_GM_state_0" style=";" data-edge="true" data-et="edge" data-id="L_GM_state_0" data-points="W3sieCI6NDY1LjgyODEyNSwieSI6Mjc3fSx7IngiOjQ2NS44MjgxMjUsInkiOjQwMX0seyJ4Ijo0NTEuMDU2MjY1NTIxNTIzMiwieSI6NDM4fV0=" marker-end="url(#mermaid-474zv09-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M186.605,579L186.605,599.667C186.605,620.333,186.605,661.667,186.605,687.833C186.605,714,186.605,725,186.605,730.5L186.605,736" id="L_EM_wiki_0" style=";" data-edge="true" data-et="edge" data-id="L_EM_wiki_0" data-points="W3sieCI6MTg2LjYwNTQ2ODc1LCJ5Ijo1Nzl9LHsieCI6MTg2LjYwNTQ2ODc1LCJ5Ijo3MDN9LHsieCI6MTg2LjYwNTQ2ODc1LCJ5Ijo3NDB9XQ==" marker-end="url(#mermaid-474zv09-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M133.297,62L133.297,68.167C133.297,74.333,133.297,86.667,133.297,98.333C133.297,110,133.297,121,133.297,126.5L133.297,132" id="L_GM2_mem0_0" style=";" data-edge="true" data-et="edge" data-id="L_GM2_mem0_0" data-points="W3sieCI6MTMzLjI5Njg3NSwieSI6NjJ9LHsieCI6MTMzLjI5Njg3NSwieSI6OTl9LHsieCI6MTMzLjI5Njg3NSwieSI6MTM2fV0=" marker-end="url(#mermaid-474zv09-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g transform="translate(405.54296875, 703)"><g data-id="L_state_wiki_0" transform="translate(-28.7734375, -12)"><foreignObject width="57.546875" height="24"><p>Git 同步</p></foreignObject></g></g><g transform="translate(81.01171875, 552)"><g data-id="L_wiki_mem0_0" transform="translate(-28.7734375, -12)"><foreignObject width="57.546875" height="24"><p>Git 同步</p></foreignObject></g></g><g transform="translate(292.43692, 398.09142)"><g data-id="L_mem0_state_0" transform="translate(-84.5703125, -12)"><foreignObject width="169.140625" height="24"><p>Qdrant Cloud 即時同步</p></foreignObject></g></g><g transform="translate(465.828125, 401)"><g data-id="L_GM_state_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>讀寫</p></foreignObject></g></g><g transform="translate(186.60546875, 703)"><g data-id="L_EM_wiki_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>讀寫</p></foreignObject></g></g><g transform="translate(133.296875, 99)"><g data-id="L_GM2_mem0_0" transform="translate(-16, -12)"><foreignObject width="32" height="24"><p>讀寫</p></foreignObject></g></g></g><g><g transform="translate(0, 128)"><g><g id="mem0" data-look="classic"><rect style="fill:#58a6ff !important" x="8" y="8" width="250.59375" height="228"></rect><g transform="translate(41.0703125, 8)"><foreignObject width="184.453125" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5;"><span style="color:#fff !important"><p>mem0 — 為什麼這樣決定</p></span></div></foreignObject></g></g></g><g></g><g></g><g><g id="flowchart-M1-4" transform="translate(133.296875, 70)"><rect style="" x="-87.796875" y="-27" width="175.59375" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-57.796875, -12)"><rect></rect><foreignObject width="115.59375" height="24"><p>決策記憶 + 理由</p></foreignObject></g></g><g id="flowchart-M2-5" transform="translate(133.296875, 174)"><rect style="" x="-62" y="-27" width="124" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-32, -12)"><rect></rect><foreignObject width="64" height="24"><p>週級更新</p></foreignObject></g></g></g></g><g transform="translate(11.10546875, 732)"><g><g id="wiki" data-look="classic"><rect style="fill:#6b8f71 !important" x="8" y="8" width="335" height="252"></rect><g transform="translate(98.8515625, 8)"><foreignObject width="153.296875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5;"><span style="color:#fff !important"><p>Wiki — 我們知道什麼</p></span></div></foreignObject></g></g></g><g></g><g></g><g><g id="flowchart-W1-2" transform="translate(175.5, 82)"><rect style="" x="-130" y="-39" width="260" height="78" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-100, -24)"><rect></rect><foreignObject width="200" height="48"><p>concepts / entities / analysis</p></foreignObject></g></g><g id="flowchart-W2-3" transform="translate(175.5, 198)"><rect style="" x="-62" y="-27" width="124" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-32, -12)"><rect></rect><foreignObject width="64" height="24"><p>天級更新</p></foreignObject></g></g></g></g><g transform="translate(270.42578125, 430)"><g><g id="state" data-look="classic"><rect style="fill:#c67a50 !important" x="8" y="8" width="254.234375" height="228"></rect><g transform="translate(63.1328125, 8)"><foreignObject width="143.96875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5;"><span style="color:#fff !important"><p>State — 正在做什麼</p></span></div></foreignObject></g></g></g><g></g><g></g><g><g id="flowchart-S1-0" transform="translate(135.1171875, 70)"><rect style="" x="-89.6171875" y="-27" width="179.234375" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-59.6171875, -12)"><rect></rect><foreignObject width="119.234375" height="24"><p>team-status.md</p></foreignObject></g></g><g id="flowchart-S2-1" transform="translate(135.1171875, 174)"><rect style="" x="-70" y="-27" width="140" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-40, -12)"><rect></rect><foreignObject width="80" height="24"><p>分鐘級更新</p></foreignObject></g></g></g></g><g id="flowchart-GM-12" transform="translate(465.828125, 250)"><rect style="" x="-43.1953125" y="-27" width="86.390625" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-13.1953125, -12)"><rect></rect><foreignObject width="26.390625" height="24"><p>GM</p></foreignObject></g></g><g id="flowchart-EM-14" transform="translate(186.60546875, 552)"><rect style="" x="-41.8203125" y="-27" width="83.640625" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-11.8203125, -12)"><rect></rect><foreignObject width="23.640625" height="24"><p>Em</p></foreignObject></g></g><g id="flowchart-GM2-16" transform="translate(133.296875, 35)"><rect style="" x="-43.1953125" y="-27" width="86.390625" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-13.1953125, -12)"><rect></rect><foreignObject width="26.390625" height="24"><p>GM</p></foreignObject></g></g></g></g></g></svg>

三層記憶各自獨立：不同的更新頻率、不同的同步方式、不同的擁有者——混在一起會互相干擾

---

## mem0 遷移：從 Chroma 到 Qdrant Cloud

mem0 最早是跑在本機的 Chroma（一個本地向量資料庫）上面 \[5\] \[6\]，Phase A 只給 GM 用，用法很簡單：session 開始的時候用 SessionStart hook 自動搜尋相關記憶，session 結束前用 `/m0-add` 手動存入新的決策記憶（後期為了「做夢」讓他自動化了）。

但「本機」就是問題，一台 Mac 上存的記憶，另一台 Mac 看不到，而我在多台機器甚至雲端、手機之間切換工作是常態，所以決定遷移到 Qdrant Cloud 是很正常的決定，而且遷移的過程本身並不複雜（建一個 free tier cluster，改一下 config 裡的 vector store 設定），但遷移照慣例還是會碰上問題。

**第一個狀況是 macOS 的 Python 安裝限制，** 新版 macOS 的 Python 是禁止直接安裝第三方套件的，理由是防止使用者破壞系統的依賴環境，這意味著 mem0 需要的那些 Python 套件不能直接裝，要先裝一個隔離的 Python 環境，再把套件裝在裡面。

還好我之前已經有 venv，所以這問題不大，但接下來碰到了一個連鎖問題：我用環境變數管理工具（direnv）在進入 repo 目錄時自動啟用這個隔離環境，但 Claude Code 的 SessionStart hook 在執行的時候，走的是另一條路徑，讀不到 direnv 的設定，所以 hook 啟動 mem0 的時候找不到正確的 Python，這問題的修正方式也不難，讓設定檔偵測自己被誰呼叫，分別用不同的方式載入環境就好，這兩個問題問問 AI 或 Google 都可以解決。

**第二個狀況是向量維度對不上，** 這個單純是 Default 值的問題，因為 Qdrant Cloud 在建資料庫時如果沒指定維度，會套用預設值，但我給 mem0 用的語意模型輸出維度跟預設值不一樣，第一次存記憶的時候就報錯了，384 維的向量（一開始不想搞太大）塞進 1536 維的容器。

解法也很簡單，刪掉 Qdrant 上的資料庫，然後在 mem0 設定裡讓它重建，重建之後存入和搜尋的全 Data Chain Pass。

**第三個狀況是 API key 衝突，** mem0 需要 Anthropic 的 API key 來做語言模型的呼叫（它是獨立的 Python 程式，不走 Claude 訂閱），所以環境設定裡需要放一個 Anthropic API key，但 Claude Code 也會讀這個環境變數，如果偵測到有 key，他會優先用 key 而不是用訂閱的登入，這個我在 [Claude Code 多帳號並行指南](https://emilwu.tw/resources/multi-account) 裡頭有說過，這個問題是我在隔天才發現，因為 Claude Code 剛啟動時會出現你使用的模型跟使用方式，但通常都沒在看，只有看帳單時會專心一點，這時我就發現有差異（又一個 silent failure），修正方式也很簡單，把環境變數改名加上 mem0 專屬前綴，讓 Claude Code 不認得它，只有 mem0 的腳本知道去讀這個名字。

三個問題的共同特性： **每一個都不是 mem0 本身的問題，而是 mem0 跟它的運行環境（macOS Python、Qdrant 預設值、Claude Code 環境變數）之間的狀況，** 這告訴我們，建立記憶系統的困難點不在於「存」和「取」，而是怎麼讓它在一個生態系裡跟每個元件都能相合而且穩定運作。

![mem0 遷移的三個坑：環境隔離、維度不符、API key 衝突](https://emilwu.tw/images/articles/29-agent-team-memory/migration-light.webp)

三個坑都不是 mem0 的問題，而是 mem0 跟運行環境之間的摩擦——整合的難點永遠在接縫處

---

## QMD 整合：不是第四層記憶，是搜尋介面

上一篇末尾提到，Em 的 wiki 越來越大之後，每次 /query 的 token 成本線性增長，因為 Em 缺少一個搜尋層，而我導入了 QMD 來解決這個問題，這邊說的更詳細一些。

QMD 是一個本地的搜尋引擎，專為 markdown 文件設計，有三種搜尋模式：關鍵字搜尋（最快）、語意搜尋（理解同義詞和自然語言）、以及混合模式（最慢但最準，會自動把你的問題展開成多個變體去搜）。

我導入 **QMD 不是幫系統裝上第四層記憶 \[9\]，** 而是為了效能、Token、應用考量，因為 QMD 引入後不存任何新的知識，只是作為 wiki 的搜尋介面，wiki 頁面還是 Em 寫的 markdown，QMD 只是把這些 markdown 做了 index，讓查詢的時候不需要遍歷整個 wiki。

整合分成三個階段，每個階段一個乾淨的 commit：

**Phase 1（Em 內部改造）：** 修改 Em 的查詢流程，從「讀索引 → 猜哪些頁面相關 → 全部讀取」改成「先搜尋 → 只讀搜尋結果回傳的頁面」，同時在 ingest 流程結尾加了一步自動更新搜尋索引，讓每次新增知識之後搜尋都能找到，升級完之後，查詢的 token 成本從「跟 wiki 大小成正比」降到「跟相關頁面數成正比」，所以查詢的成本跟圖書館的量體無關了，只跟他要查的知識有多少牽扯有關係。

Loading chart…

QMD 整合前後的 token 成本對比——遍歷式查詢隨 wiki 增長線性攀升，搜尋式查詢幾乎持平

測試的時候還碰到了一個有趣的現象：我用一個完全不存在的 topic 做測試，搜尋引擎還是回了一個評價高分的結果，因為他太”努力”找匹配了，把不存在的這個關鍵字展開成「未定義的變數」，自己推測這個變數的變體跟關聯，然後匹配到了一個講 Context 盲區的頁面，不過實務上 Agent 不會查亂碼，真正的 knowledge gap 案例會表現得更合理一些才對，不過這個就靜待之後實際運作的驗證。

而在確認 search-first 的模式可行之後，我才把搜尋能力開放給其他 Agent（在 C7 的 catalog 裡建了 QMD 的工具規格，讓 Dm 這類需要查知識的 Agent 可以不經過 Em 直接搜尋 wiki），最後才更新架構文件，因為你不會想在機制還沒穩定的時候就把它寫進 source of truth。

---

## 順序很重要：為什麼先 QMD 再 mem0 Phase B

這邊有一個不明顯但重要的先後順序問題：為什麼先做 QMD 整合，再做 mem0 的 Phase B（把 mem0 從 GM-only 擴展到所有 Layer 2 agent）。

原因是 **Em 的知識模式要先穩定，才能定義 mem0 的 extraction rules。**

mem0 Phase B 的核心挑戰不是技術（技術上就是在其他 Agent 的 session hook 裡加 mem0 呼叫），而是「每個 Agent 應該記住什麼」，比如 Em 記住的是知識結構的演變（wiki concept 的新增和修改），C7 記住的是資產管理的決策（為什麼某個 skill 被接受或拒絕），Dm 記住的是設計經驗（某個 Agent 的設計選擇成功或失敗的原因）。

但要定義這些 extraction rules，你需要先理解每個 Agent 做什麼，而理解他們在做什麼的前提是 Em 的知識庫穩定且可查詢，如果 Em 的 wiki 還在用 eager loading 的方式查詢（每次都讀所有頁面），那其他 Agent 在設計 extraction rules 的時候就沒辦法快速驗證「這條記憶有沒有跟 wiki 裡的既有知識重複」。

而在 QMD 整合完成之後，Em 有了 search-first 的 query 能力，其他 Agent 也可以直接用 QMD 搜尋 wiki，這讓 mem0 Phase B 的 extraction rules 設計有了可靠的「知識去重複」機制：在存新記憶之前，先搜尋 wiki 確認這個知識是不是已經被 Em 結構化地記錄過了。

---

## 跨機器同步的完整解法

回到開頭那個場景：我在出門用的 Macbook Air 上做了決策，隔天在當成桌機使用的另一台老 Macbook Pro 上問 GM，他不知道。

三層記憶架構解決了這個問題，但每一層的同步方式不一樣：

**Git（State + Wiki）：** 在一台機器上更新 team-status.md 或 Em wiki 之後，commit + push，另一台機器 pull 就能看到 \[7\]，這不是即時的（需要手動 git 操作），但 State 和 Wiki 的更新頻率夠低，手動同步可以接受。

**Qdrant Cloud（mem0）：** 在一台機器上用 /m0-add 存入記憶，另一台機器的 SessionStart hook 立刻就能搜尋到，因為 Qdrant Cloud 是雲端服務，不需要任何同步操作。

**direnv（環境變數）：** `.envrc` 檔案透過我建立的雲端機制交換（NAS 很適合放這種輕量服務，用 AI 開發也快），同步之後 `direnv allow` 就能載入，API key、venv 路徑、環境設定都在裡面。

三個同步機制各自負責一個層次，沒有重疊，也沒有遺漏 \[8\]，State 和 Wiki 用 Git 是因為它們是結構化的 markdown 檔案，Git 的 diff 和 merge 很適合，mem0 用 Qdrant Cloud 是因為決策記憶需要即時可見，不應該等 Git 操作，環境變數用 direnv 是因為它需要在進入目錄的時候自動載入，不應該手動 export。

（你可能會覺得「三個不同的同步機制」聽起來很複雜，但比起把所有東西塞進一個系統然後發現某些場景它處理不了，三個各自負責自己擅長的事情，反而更簡單，至少在 debug 的時候一出問題你就知道該看哪裡。）

<svg id="mermaid-urcihd5-svg" width="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 908.984375px;" viewBox="0 0 908.984375 348" role="graphics-document document" aria-roledescription="flowchart-v2"><g><marker id="mermaid-urcihd5-svg_flowchart-v2-pointEnd" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-urcihd5-svg_flowchart-v2-pointStart" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" style="stroke-width: 1; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-urcihd5-svg_flowchart-v2-circleEnd" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-urcihd5-svg_flowchart-v2-circleStart" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" style="stroke-width: 1; stroke-dasharray: 1, 0;"></circle></marker><marker id="mermaid-urcihd5-svg_flowchart-v2-crossEnd" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><marker id="mermaid-urcihd5-svg_flowchart-v2-crossStart" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" style="stroke-width: 2; stroke-dasharray: 1, 0;"></path></marker><g><g><g id="cloud" data-look="classic"><rect style="fill:#58a6ff !important" x="373.484375" y="19" width="210.640625" height="321"></rect><g transform="translate(462.8046875, 19)"><foreignObject width="32" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5;"><span style="color:#fff !important"><p>雲端</p></span></div></foreignObject></g></g><g id="MBP" data-look="classic"><rect style="fill:#c67a50 !important" x="698.125" y="8" width="202.859375" height="332"></rect><g transform="translate(749.34375, 8)"><foreignObject width="100.421875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5;"><span style="color:#fff !important"><p>Macbook Pro</p></span></div></foreignObject></g></g><g id="MBA" data-look="classic"><rect style="fill:#6b8f71 !important" x="8" y="8" width="202.859375" height="332"></rect><g transform="translate(61.4921875, 8)"><foreignObject width="95.875" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5;"><span style="color:#fff !important"><p>Macbook Air</p></span></div></foreignObject></g></g></g><g><path d="M158.914,70L167.572,70C176.229,70,193.544,70,215.754,70C237.964,70,265.068,70,292.172,70C319.276,70,346.38,70,367.774,73.872C389.167,77.743,404.85,85.486,412.691,89.358L420.532,93.229" id="L_A1_GIT_0" style=";" data-edge="true" data-et="edge" data-id="L_A1_GIT_0" data-points="W3sieCI6MTU4LjkxNDA2MjUsInkiOjcwfSx7IngiOjIxMC44NTkzNzUsInkiOjcwfSx7IngiOjI5Mi4xNzE4NzUsInkiOjcwfSx7IngiOjM3My40ODQzNzUsInkiOjcwfSx7IngiOjQyNC4xMTkxNDA2MjUsInkiOjk1fV0=" marker-end="url(#mermaid-urcihd5-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M155.578,174L164.792,174C174.005,174,192.432,174,215.198,174C237.964,174,265.068,174,292.172,174C319.276,174,346.38,174,367.774,170.128C389.167,166.257,404.85,158.514,412.691,154.642L420.532,150.771" id="L_A2_GIT_0" style=";" data-edge="true" data-et="edge" data-id="L_A2_GIT_0" data-points="W3sieCI6MTU1LjU3ODEyNSwieSI6MTc0fSx7IngiOjIxMC44NTkzNzUsInkiOjE3NH0seyJ4IjoyOTIuMTcxODc1LCJ5IjoxNzR9LHsieCI6MzczLjQ4NDM3NSwieSI6MTc0fSx7IngiOjQyNC4xMTkxNDA2MjUsInkiOjE0OX1d" marker-end="url(#mermaid-urcihd5-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M533.49,95L541.929,90.833C550.368,86.667,567.247,78.333,585.186,74.167C603.125,70,622.125,70,641.125,70C660.125,70,679.125,70,696.616,70C714.107,70,730.089,70,738.079,70L746.07,70" id="L_GIT_B1_0" style=";" data-edge="true" data-et="edge" data-id="L_GIT_B1_0" data-points="W3sieCI6NTMzLjQ5MDIzNDM3NSwieSI6OTV9LHsieCI6NTg0LjEyNSwieSI6NzB9LHsieCI6NjQxLjEyNSwieSI6NzB9LHsieCI6Njk4LjEyNSwieSI6NzB9LHsieCI6NzUwLjA3MDMxMjUsInkiOjcwfV0=" marker-end="url(#mermaid-urcihd5-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M533.49,149L541.929,153.167C550.368,157.333,567.247,165.667,585.186,169.833C603.125,174,622.125,174,641.125,174C660.125,174,679.125,174,697.172,174C715.219,174,732.313,174,740.859,174L749.406,174" id="L_GIT_B2_0" style=";" data-edge="true" data-et="edge" data-id="L_GIT_B2_0" data-points="W3sieCI6NTMzLjQ5MDIzNDM3NSwieSI6MTQ5fSx7IngiOjU4NC4xMjUsInkiOjE3NH0seyJ4Ijo2NDEuMTI1LCJ5IjoxNzR9LHsieCI6Njk4LjEyNSwieSI6MTc0fSx7IngiOjc1My40MDYyNSwieSI6MTc0fV0=" marker-end="url(#mermaid-urcihd5-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M185.859,278L190.026,278C194.193,278,202.526,278,220.245,278C237.964,278,265.068,278,292.172,278C319.276,278,346.38,278,363.432,278C380.484,278,387.484,278,390.984,278L394.484,278" id="L_A3_QD_0" style=";" data-edge="true" data-et="edge" data-id="L_A3_QD_0" data-points="W3sieCI6MTg1Ljg1OTM3NSwieSI6Mjc4fSx7IngiOjIxMC44NTkzNzUsInkiOjI3OH0seyJ4IjoyOTIuMTcxODc1LCJ5IjoyNzh9LHsieCI6MzczLjQ4NDM3NSwieSI6Mjc4fSx7IngiOjM5OC40ODQzNzUsInkiOjI3OH1d" marker-end="url(#mermaid-urcihd5-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path><path d="M559.125,278L563.292,278C567.458,278,575.792,278,589.458,278C603.125,278,622.125,278,641.125,278C660.125,278,679.125,278,692.125,278C705.125,278,712.125,278,715.625,278L719.125,278" id="L_QD_B3_0" style=";" data-edge="true" data-et="edge" data-id="L_QD_B3_0" data-points="W3sieCI6NTU5LjEyNSwieSI6Mjc4fSx7IngiOjU4NC4xMjUsInkiOjI3OH0seyJ4Ijo2NDEuMTI1LCJ5IjoyNzh9LHsieCI6Njk4LjEyNSwieSI6Mjc4fSx7IngiOjcyMy4xMjUsInkiOjI3OH1d" marker-end="url(#mermaid-urcihd5-svg_flowchart-v2-pointEnd)" fill="none" stroke="currentColor"></path></g><g><g transform="translate(292.171875, 70)"><g data-id="L_A1_GIT_0" transform="translate(-56.3125, -12)"><foreignObject width="112.625" height="24"><p>commit + push</p></foreignObject></g></g><g transform="translate(292.171875, 174)"><g data-id="L_A2_GIT_0" transform="translate(-56.3125, -12)"><foreignObject width="112.625" height="24"><p>commit + push</p></foreignObject></g></g><g transform="translate(641.125, 70)"><g data-id="L_GIT_B1_0" transform="translate(-13.5078125, -12)"><foreignObject width="27.015625" height="24"><p>pull</p></foreignObject></g></g><g transform="translate(641.125, 174)"><g data-id="L_GIT_B2_0" transform="translate(-13.5078125, -12)"><foreignObject width="27.015625" height="24"><p>pull</p></foreignObject></g></g><g transform="translate(292.171875, 278)"><g data-id="L_A3_QD_0" transform="translate(-32, -12)"><foreignObject width="64" height="24"><p>即時寫入</p></foreignObject></g></g><g transform="translate(641.125, 278)"><g data-id="L_QD_B3_0" transform="translate(-32, -12)"><foreignObject width="64" height="24"><p>即時讀取</p></foreignObject></g></g></g><g><g id="flowchart-A1-0" transform="translate(109.4296875, 70)"><rect style="" x="-49.484375" y="-27" width="98.96875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-19.484375, -12)"><rect></rect><foreignObject width="38.96875" height="24"><p>State</p></foreignObject></g></g><g id="flowchart-A2-1" transform="translate(109.4296875, 174)"><rect style="" x="-46.1484375" y="-27" width="92.296875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-16.1484375, -12)"><rect></rect><foreignObject width="32.296875" height="24"><p>Wiki</p></foreignObject></g></g><g id="flowchart-A3-2" transform="translate(109.4296875, 278)"><rect style="" x="-76.4296875" y="-27" width="152.859375" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-46.4296875, -12)"><rect></rect><foreignObject width="92.859375" height="24"><p>mem0 client</p></foreignObject></g></g><g id="flowchart-B1-3" transform="translate(799.5546875, 70)"><rect style="" x="-49.484375" y="-27" width="98.96875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-19.484375, -12)"><rect></rect><foreignObject width="38.96875" height="24"><p>State</p></foreignObject></g></g><g id="flowchart-B2-4" transform="translate(799.5546875, 174)"><rect style="" x="-46.1484375" y="-27" width="92.296875" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-16.1484375, -12)"><rect></rect><foreignObject width="32.296875" height="24"><p>Wiki</p></foreignObject></g></g><g id="flowchart-B3-5" transform="translate(799.5546875, 278)"><rect style="" x="-76.4296875" y="-27" width="152.859375" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-46.4296875, -12)"><rect></rect><foreignObject width="92.859375" height="24"><p>mem0 client</p></foreignObject></g></g><g id="flowchart-GIT-6" transform="translate(478.8046875, 122)"><rect style="" x="-71.4765625" y="-27" width="142.953125" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-41.4765625, -12)"><rect></rect><foreignObject width="82.953125" height="24"><p>Git Remote</p></foreignObject></g></g><g id="flowchart-QD-7" transform="translate(478.8046875, 278)"><rect style="" x="-80.3203125" y="-27" width="160.640625" height="54" fill="none" stroke="currentColor"></rect><g style="" transform="translate(-50.3203125, -12)"><rect></rect><foreignObject width="100.640625" height="24"><p>Qdrant Cloud</p></foreignObject></g></g></g></g></g></svg>

跨機器同步：State 和 Wiki 走 Git（手動但夠用），mem0 走 Qdrant Cloud（即時且零操作）

![兩台機器透過三種不同的同步機制共享記憶](https://emilwu.tw/images/articles/29-agent-team-memory/sync-light.webp)

三個同步機制各自負責一個層次，沒有重疊，也沒有遺漏

---

## 記憶不是一個問題

之前在寫 Context Engineering 那篇的時候，我提過 JIT loading 的原則，那個原則到了記憶系統這變得更具體了： **知識不只是「需要的時候才載入」，而是從「正確的層載入正確類型的知識」，** State 問的是「現在」，Wiki 問的是「什麼」，mem0 問的是「為什麼」，三個問題導向三個不同的資料源，如果你想用一個單一系統就解決所有問題，補東補西補到最後，其實你還是會建了三個不同方向的系統，只是沒有明確區分，與其這樣，不如一開始就把地基分好，避免將來整合時候的疊床架屋。

這樣看來，記憶系統裡最不直覺的設計其實是： **不要讓 Agent「記住一切」，而是設計一套機制，讓他在需要時「找到」對的東西，** 很妙吧，一個記憶系統做後的關鍵設計竟然是搜尋，那是不是應該改叫搜尋系統？不過記住和找到本來就是兩回事，記住是把東西塞進 Context、Rules、Memory，找到是知道該去哪裡問，在 Token 珍貴的時代，這顯德更重要，也是為什麼我的”記憶”系統裡最晚才穩定的不是儲存機制，而是搜尋機制（QMD），因為存東西問題不大，怎麼找到你存的東西、而且還找對，那才是真正的挑戰。

---

**參考資料**

\[1\] arXiv — Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory [https://arxiv.org/abs/2504.19413](https://arxiv.org/abs/2504.19413)

\[2\] arXiv — Multi-Layered Memory Architectures for LLM Agents: An Experimental Evaluation（多層記憶：working memory、episodic、semantic、procedural） [https://arxiv.org/html/2603.29194](https://arxiv.org/html/2603.29194)

\[3\] arXiv — A-MEM: Agentic Memory for LLM Agents（NeurIPS 2025，反面觀點：動態自組織記憶可能比靜態分層更有適應性） [https://arxiv.org/abs/2502.12110](https://arxiv.org/abs/2502.12110)

\[4\] Qdrant — Mem0 + Qdrant Integration Documentation [https://qdrant.tech/documentation/frameworks/mem0/](https://qdrant.tech/documentation/frameworks/mem0/)

\[5\] Airbyte — Chroma DB vs Qdrant: Key Differences [https://airbyte.com/data-engineering-resources/chroma-db-vs-qdrant](https://airbyte.com/data-engineering-resources/chroma-db-vs-qdrant)

\[6\] 4xxi — Vector Database Comparison 2026（注意：ChromaDB 2025 Rust 改寫後效能提升 4x，差距可能已縮小） [https://4xxi.com/articles/vector-database-comparison/](https://4xxi.com/articles/vector-database-comparison/)

\[7\] DEV Community — How I Gave My AI Agents a Permanent Memory That Syncs Across Machines（git-synced MCP memory server 實作） [https://dev.to/keshrath/how-i-gave-my-ai-agents-a-permanent-memory-that-syncs-across-machines-4755](https://dev.to/keshrath/how-i-gave-my-ai-agents-a-permanent-memory-that-syncs-across-machines-4755)

\[8\] DEV Community — Designing Memory for 20 AI Agents Across 9 Nodes（跨機器同步的 race condition 問題） [https://dev.to/linou518/designing-memory-for-20-ai-agents-across-9-nodes-multi-agent-memory-architecture-4nhe](https://dev.to/linou518/designing-memory-for-20-ai-agents-across-9-nodes-multi-agent-memory-architecture-4nhe)

\[9\] Martin Fowler — Strangler Fig Pattern（支持「先建搜尋介面再完成遷移」的漸進式架構策略） [https://martinfowler.com/bliki/StranglerFigApplication.html](https://martinfowler.com/bliki/StranglerFigApplication.html)

\[10\] Analytics Vidhya — Architecture and Orchestration of Memory Systems in AI Agents [https://www.analyticsvidhya.com/blog/2026/04/memory-systems-in-ai-agents/](https://www.analyticsvidhya.com/blog/2026/04/memory-systems-in-ai-agents/)