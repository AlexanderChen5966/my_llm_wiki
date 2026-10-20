---
title: "Beautiful UI：專為 AI-native 產品整理的 UI 元件與互動設計參考 | 網路資源"
source: "https://tools.wingzero.tw/articles/4044"
author:
  - "[[赤紅翼零]]"
published: 2026-09-01
created: 2026-09-01
description: "Beautiful UI 是專為 AI SaaS、Agent 與 Copilot 打造的 AI-native UI Pattern Library，收錄 Thinking、Streaming Text、Tool Calls、Approval、Context Cards、Diff 等介面設計。"
tags:
  - "clippings"
status: "processed"
---
網站連結 [https://www.beautifului.dev/](https://www.beautifului.dev/)

Beautiful UI 是一個專門收集與展示「AI-native Interface」設計模式的 UI 資源網站，核心目標不是提供另一套 Button、Modal、Dropdown 等傳統元件，而是整理 AI 產品真正會遇到的介面需求，例如模型思考狀態、串流回答、工具呼叫、Agent 任務進度、人工確認、引用來源、修改 Diff，以及讓使用者調整 AI 產生內容的互動元件。對正在開發 AI SaaS、Agent、Copilot 或聊天式產品的團隊而言，它比較像一套 AI Product UI Pattern Library。

這也是 Beautiful UI 和一般 UI Component Library 最明顯的差異。傳統設計系統主要解決「網站需要哪些基礎元件」，Beautiful UI 關注的則是「當 AI 開始思考、使用工具、修改資料、提出建議甚至準備執行動作時，介面應該如何向人類呈現」。因此網站上的範例不只是視覺漂亮，而是大量圍繞 AI Agent 的狀態透明度、Human-in-the-loop 與結果可解釋性設計。

Beautiful UI 目前展示 20 種元件／互動模式，包含 Loading State、Thinking、Streaming Text、Approval Card、Tool Chips、Task Rows、Chat、Prompt Bar、Recommendation Card、Context Cards、Diff Table、Records Table、Filter Table、Sidebar Nav、Search、Flowchart、Insight Cards、Code Block、Fine-tune Card 與 Selection Actions。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228572&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4044&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224754420&amp;bpp=1&amp;bdt=1555&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788228448%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280&amp;nras=3&amp;correlator=50244112212&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=2784&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400778%2C42533293%2C122880743%2C122880333&amp;oid=2&amp;pvsid=5627734065478828&amp;tmod=684280329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

其中最能代表網站定位的，可能是 Thinking 元件。

一般 ChatGPT 類產品最簡單的處理方式，就是在模型工作時顯示 Loading Spinner；但當 AI Agent 開始進行搜尋、分析資料、寫程式與呼叫工具，單純顯示「Loading...」已經無法充分告訴使用者系統正在做什麼。

Beautiful UI 的 Thinking Pattern 因此設計成可展開的 Trace，可以呈現 Steps、Reasoning、Search、Coding 等不同工作狀態。重點並不是把所有內部資訊毫無整理地倒給使用者，而是提供一個清楚的視覺層級，讓使用者知道 Agent 現在處於什麼階段。

Streaming Text 則針對生成式 AI 最常見的串流回答設計。範例除了逐步出現的 Answer，還把 Sources、Actions 與 Follow-up Questions 整合在同一個回答介面。這代表 AI Response 不必只是聊天泡泡裡的一大段 Markdown，而可以進一步成為帶有資料來源與下一步操作的互動物件。

如果從 AI Agent 產品的角度整理，Beautiful UI 目前值得注意的設計模式包括：

- **Loading State** ：以 Pixel-grid Loader、Shimmer 與經過時間呈現 AI 工作中的狀態
- **Thinking** ：以可展開 Trace 顯示 Steps、Reasoning、Search、Coding
- **Streaming Text** ：將串流回答、來源、操作與 Follow-up 整合
- **Approval Card** ：Agent 執行前要求使用者確認，建立 Human-in-the-loop
- **Tool Chips** ：把 Tool Call、Code Edit 等動作壓縮成容易掃讀的 Chip
- **Task Rows** ：呈現 Running、Failed、Completed 等 Agent Task 狀態
- **Prompt Bar** ：整合 @ Sources、Slash Commands、Model Picker 與語音輸入
- **Recommendation Card** ：顯示 AI 建議、Confidence 與可選擇的後續 Action
- **Context Cards** ：呈現 AI Retrieval 取得的 Knowledge Chunk 及其來源
- **Diff Table** ：顯示 AI 準備對結構化資料進行的修改
- **Flowchart** ：以視覺化節點呈現 Trigger、Condition 等 Workflow
- **Insight Cards** ：將 Agent 分析結果與即時圖表結合
- **Code Block** ：同時考慮程式碼與 Unified Diff 的呈現
- **Fine-tune Card** ：讓使用者在 Inspector 中調整 AI 產生結果的屬性
- **Selection Actions** ：選取文字後直接要求 Agent Explain、Improve 或進一步修改。

Approval Card 特別能反映目前 Agentic AI 介面的設計趨勢。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=1429272977&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228578&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4044&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224754427&amp;bpp=1&amp;bdt=1562&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788228448%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280%2C235x600&amp;nras=5&amp;correlator=50244112212&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=4525&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=931&amp;eid=95399630%2C95400778%2C42533293%2C122880743%2C122880333&amp;oid=2&amp;psts=AOrYGskvsVLkTvz7zHPvkvWe6rbKQYLiXjFx3OusiCIidXkf_oR_vAUfyW1E8ts-8V7vvhSU1ytrsKmFozTPRTns5_iUJvbCB4AmsSIpd6s%2CAOrYGslWw79YUfgyu5XzozisCIo5jN0_qsgoDAGTaFMObIsMC36Prhzlr5kH74DzXwTL-C1s7XgWEAO8wwPncfrnDL4wykrSXCx63Xsb6Xoe0Fjiag%2CAOrYGskSBEmmHcOZdAZzFQzCBKzX3bt6UEJcjjnu4pzVEHrXUnFcY-B8_DxN2TYei4lLL-py1Kkx44fL_1OmDM8wSLPDXUNSKXiqLDJ2Rj035kqDplk&amp;pvsid=5627734065478828&amp;tmod=684280329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=13&amp;uci=a!d&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

當 AI 只是回答問題時，使用者通常不需要確認；但如果 Agent 可以寄 Email、修改資料、建立訂單或執行其他具有實際後果的動作，就需要在「AI 建議」與「AI 執行」之間建立一道清楚的控制點。

Beautiful UI 的 Approval Card 就是這類 Human-in-the-loop Pattern。網站範例讓 Agent 提出問題，提供數個預設答案，也允許輸入其他選項，再透過 Skip／Continue 控制流程。

Recommendation Card 則處理另一個問題：AI 有建議，但不代表建議一定正確。

範例不是只顯示「建議你補貨」，而是同時提供 Confidence、主要建議、Alternative Options，以及 Accept 等操作。這種設計把 AI 從「替使用者做決定」調整成「提供有依據、可以接受或拒絕的建議」。

Tool Chips 與 Task Rows 則很適合 Claude Code、Codex 類型的 Agent Interface。

當 Agent 一次進行十幾次搜尋、讀檔、Code Edit 或 Tool Call，如果每個動作都完整展開，Conversation 很快就會變得難以閱讀。Beautiful UI 示範的做法，是把工具操作濃縮成 Compact Chips，再利用 Task Rows 呈現任務的 Running、Failed、Completed 等狀態。

Context Cards 處理的則是 RAG 與 Knowledge Retrieval。

網站範例會顯示 Retrieved Knowledge Chunk、字元數以及 PDF、CSV 等來源。對企業 AI Search、Knowledge Base、Research Agent 來說，這類介面相當重要：使用者除了看到 AI 的結論，也能理解這個結論參考了哪些資料。

Diff Table 更進一步處理「AI 修改資料」的情境。

Coding Agent 已經讓 Git Diff 成為相當自然的 AI Interaction Pattern，但企業軟體不一定是在改 Code，也可能是在修改 CRM、商品資料、供應商資訊或試算表。Beautiful UI 因此把 Diff 概念套用到 Tabular Data，讓 AI Proposed Edits 可以先被人類檢查，再決定是否接受。

這個思路和 Approval Card 放在一起看會更清楚：Beautiful UI 設計的不只是「怎麼把 AI 做漂亮」，而是在處理 Agent 如何與人共同工作。

一套比較成熟的 Agent 介面可能形成：

`User Prompt → Thinking → Tool Calls → Context → Recommendation → Diff → Human Approval → Execution → Result`

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=3975518427&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788228580&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4044&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224754432&amp;bpp=1&amp;bdt=1567&amp;idt=1&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788228448%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788228448%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280%2C832x280%2C235x600%2C832x280&amp;nras=6&amp;correlator=50244112212&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=5827&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=2231&amp;eid=95399630%2C95400778%2C42533293%2C122880743%2C122880333&amp;oid=2&amp;psts=AOrYGskvsVLkTvz7zHPvkvWe6rbKQYLiXjFx3OusiCIidXkf_oR_vAUfyW1E8ts-8V7vvhSU1ytrsKmFozTPRTns5_iUJvbCB4AmsSIpd6s%2CAOrYGslWw79YUfgyu5XzozisCIo5jN0_qsgoDAGTaFMObIsMC36Prhzlr5kH74DzXwTL-C1s7XgWEAO8wwPncfrnDL4wykrSXCx63Xsb6Xoe0Fjiag%2CAOrYGskSBEmmHcOZdAZzFQzCBKzX3bt6UEJcjjnu4pzVEHrXUnFcY-B8_DxN2TYei4lLL-py1Kkx44fL_1OmDM8wSLPDXUNSKXiqLDJ2Rj035kqDplk%2CAOrYGsl2R5Ad1pvhoU-E69dAYw1aOg7K_R5YEnZAJsY86ixxPlj0KLP6Js1bLqYtQKIoeQ9JjSOkhQoaCtpsroqLliwBgFy_LlmkYhVG&amp;pvsid=5627734065478828&amp;tmod=684280329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C94%2C1920%2C94%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=14&amp;uci=a!e&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

而 Beautiful UI 網站上的不同 Primitives，基本上就是在替這條流程中的各個節點提供 UI 參考。

Prompt Bar 也比傳統 Chat Input 更接近現在 AI Workspace 的需求。網站範例把 @ Sources、Slash Commands、Model Picker 與 Dictation 放進 Composer。這表示輸入框不只是「打一段文字送出去」，而是 AI 工作環境的 Command Center：可以指定資料來源、呼叫功能、選擇模型，甚至透過語音下達需求。

Selection Actions 則代表另一種正在增加的 AI Interaction：不必每件事情都回到聊天框。

使用者直接選取一段內容，就能要求 AI Explain 或 Improve，旁邊再提供「Describe edits」輸入框。這種 Contextual AI UX 對文件編輯器、IDE、CMS 與設計工具尤其有參考價值，因為 AI Action 可以直接出現在使用者正在操作的內容附近。

Beautiful UI 也沒有把 AI 介面侷限在 Chatbot。網站還展示 CRM-style Records Table、Filter Table、Sidebar Navigation、Search、Flowchart、Insight Cards 等 Pattern。Records Table 的 Demo 包含 Tags、Sorting、Relationship Status；Filter Table 則利用 Status Chips 即時重新整理資料；Flowchart 用節點呈現 Trigger 與 If／Else 條件。

這點相當重要，因為「AI 產品 = 聊天介面」正在逐漸變成過度簡化的設計方式。

如果使用者是在管理供應商、修改資料庫、分析銷售數據或建立 Automation Workflow，真正適合的介面可能仍然是 Table、Card、Canvas 或 Inspector，只是在這些既有 UI 上加入 Agent 能力。Beautiful UI 的元件方向正好反映這種「AI 融入既有 Product UI」的思路，而不是所有功能都塞進聊天泡泡。

從使用族群來看，Beautiful UI 最適合 AI SaaS Founder、Product Designer、UI/UX Designer 與正在利用 AI Coding Tool 開發產品的前端工程師。尤其現在使用 Claude Code、Codex、Cursor 等工具時，功能本身可能很快就能寫出來，但如果 Prompt 只寫「幫我做一個漂亮的 AI Chat UI」，結果往往會回到非常相似的 Sidebar + Chat Bubble + Gradient Button。

Beautiful UI 提供的價值就在這裡：它讓開發者先看到更具體的 AI Interaction Pattern，再思考自己的產品究竟需要 Thinking Trace、Approval、Context、Diff、Task Status 還是 Recommendation，而不是把所有 AI 功能都設計成 ChatGPT Clone。

網站本身目前由 TurboProduct design studio 製作。首頁也直接提供其 Product Design 服務入口，因此 Beautiful UI 除了是一個免費公開的設計資源，同時具有 Design Studio 展示專業能力與取得潛在客戶的作用。

整體而言，Beautiful UI 最值得關注的地方不是「又多了一套漂亮元件」，而是它開始替 AI Software 整理一套更具體的介面語言。Thinking、Streaming、Tool Calls、Retrieved Context、Confidence、Approval、Diff、Task Status，這些過去不是傳統 SaaS Design System 核心元件的東西，正在 Agentic AI 時代變成非常常見的產品需求。

如果正在製作 AI Agent、企業 Copilot、AI Search、Coding Tool 或其他需要 Human-in-the-loop 的產品，Beautiful UI 可以當成一套相當集中的 UX Reference。它不會替產品決定完整的 Design System，但能幫助團隊回答一個更前面的問題：當 AI 不只是聊天，而是開始搜尋、思考、使用工具、提出修改甚至執行任務時，這些行為到底應該怎麼呈現在使用者面前？

<iframe frameborder="0" src="https://a4b95868b1d1e82f8ceee8d8b5632ecf.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe><iframe frameborder="0" src="https://a4b95868b1d1e82f8ceee8d8b5632ecf.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>