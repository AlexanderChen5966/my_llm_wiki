---
title: "LoopX：讓 Codex、Claude Code 長時間持續工作，打造可管理、可交接的 AI Agent 控制層 | 網路資源"
source: "https://tools.wingzero.tw/articles/4022"
author:
  - "[[赤紅翼零]]"
published: 2026-08-13
created: 2026-08-13
description: "LoopX 是為長時間 AI Agent 任務打造的開源 Stateful Control Plane，透過 Objective、Todo、Gate、Evidence、Quota 與 Handoff 保存持久狀態，讓 Codex、Claude Code、Cursor 等 Agent 能跨工作階段持續協作。"
tags:
  - "clippings"
status: "processed"
---
網站連結 [https://github.com/huangruiteng/loopx](https://github.com/huangruiteng/loopx)

![](https://github.com/huangruiteng/loopx)

LoopX 是一套針對「長時間運作 AI Agent」設計的開源控制層（Control Plane）。它本身不負責取代 Codex、Claude Code、Cursor 等 Coding Agent，而是把任務目標、待辦事項、人工審核關卡、證據、Quota 與交接狀態保存下來，讓 Agent 即使跨越多次執行、不同工作階段，甚至更換 Agent，仍能知道目前做到哪裡、接下來該做什麼。官方將它定位為 provider-neutral、stateful control plane，以及一個輕量的 loop engineering state kernel。

這個定位和一般 AI Agent Framework 有明顯差異。Agent Framework 通常處理模型怎麼呼叫工具、怎麼規劃任務以及如何執行；LoopX 關心的則是「工作跑了幾十個小時甚至幾天之後，狀態還能不能被可靠地延續」。官方直接指出，它不是另一套 Agent Framework，也不是綁定特定模型供應商的 Orchestration Runtime。真正執行工作的仍然是 Codex、Claude Code、Cursor 或 Shell Agent，LoopX 管理的是讓這些工作能跨執行階段延續的持久狀態。

這也是 LoopX 想解決的核心問題。AI Agent 完成一個幾分鐘的任務並不難，但當工作延伸成多日工程、研究、Benchmark 或實驗時，情況就不同了。需求可能改變、測試證據會過期、PR 可能收到 Reviewer 意見，某些操作又必須等待專案 Owner 批准。只依靠聊天 Context 加上一個定時器，很難可靠管理這種工作。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786610615&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4022&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786610610269&amp;bpp=2&amp;bdt=2516&amp;idt=2&amp;shv=r20260811&amp;mjsv=m202608120101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1786610598%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1786610598%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1786610598%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x852%2C784x280&amp;nras=3&amp;correlator=2419760090665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=3164&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=0&amp;eid=95397133%2C95399117%2C122880418&amp;oid=2&amp;pvsid=8440440656907246&amp;tmod=292580776&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C852&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x672_l%7C216x672_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=5728" title="Advertisement" aria-label="Advertisement"></iframe>

LoopX 因此把長期任務的重要資訊集中到一個持久狀態層，包括 Objective、Gates、Todos、Scope、Evidence 與 Quota。Agent 每次不是「一直跑到不知道什麼時候停」，而是執行一個有邊界的工作片段（bounded agent slice），完成後寫回證據、交接資訊與下一個 Todo，再由 Quota 與目前狀態判斷下一次是否應該繼續。

如果整理 LoopX 的主要設計，可以看到幾個相當明確的方向：

- 保存 Objective、Gate、Todo、Scope、Evidence 與 Quota 等長期狀態
- 支援 Codex、Claude Code、Cursor 與自訂 Agent Runtime
- Agent 每次執行 bounded turn，而非無限制持續運作
- 保留 Evidence Log，讓後續 Agent 能確認前一步到底完成了什麼
- 支援 Handoff，讓不同 Agent 或工作階段可以接手
- 透過 Claim、Lease 與 Task Boundary 管理多 Agent 工作權限
- 遇到需要人類判斷的 Gate 時停止並詢問，而不是自行越權決策
- 適合 Engineering、Research、Benchmark、Experiment 與 Operations 等長期 Loop

其中 Evidence 是理解 LoopX 很重要的一環。一般 Agent 很容易留下「我已經修好了」、「測試通過」這類自然語言描述，但長期任務真正需要的是可以回頭檢查的證據。LoopX 把 Evidence 納入持久狀態，讓後續工作可以根據目前證據、Todo 與 Gate 決定下一步，而不是完全相信上一輪 Agent 留下的一段摘要。

官方也用「Agent-native Kanban」來解釋這套設計。可以把每項工作想成 Kanban 上的一張卡片，但卡片除了 Todo，還帶有 Identity、Authority、Evidence 與 Continuation 等資訊；Claim、Gate、Monitor、Writeback 等操作則負責改變工作狀態。看得到的 Board 只是狀態投影，真正的 Source of Truth 仍然是 LoopX 保存的資料。

這種設計到了多 Agent 情境會更有意義。LoopX 將已註冊的 Agent 視為 Peers，而不是要求永遠存在一個固定的 Leader Agent。誰可以處理工作、目前由誰持有任務，以及下一步交給誰，會透過 Claims、Leases、Capabilities、Task Boundaries 與 Typed Continuation 決定。換句話說，它比較像是在建立 Agent 團隊的工作制度，而不是單純讓幾個模型互相聊天。

Quota-aware 的設計同樣值得注意。長時間 Agent 最大的風險之一，是排程器還在正常運作，但 Agent 已經沒有真正有價值的工作可以做，結果只是持續消耗 Token 與運算資源。LoopX 將 Quota 與下一次 Wake 納入控制狀態，目的是讓系統根據工作進展決定是否繼續，而不是設定一個 Cron Job 後無限制地喚醒 Agent。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=1429272977&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786610622&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4022&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786610610283&amp;bpp=3&amp;bdt=2530&amp;idt=3&amp;shv=r20260811&amp;mjsv=m202608120101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1786610598%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1786610598%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1786610598%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x852%2C784x280%2C832x280&amp;nras=4&amp;correlator=2419760090665&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=4821&amp;biw=1634&amp;bih=852&amp;scr_x=0&amp;scr_y=1437&amp;eid=95397133%2C95399117%2C122880418&amp;oid=2&amp;psts=AOrYGsltCHpSHDmLcBrzsMmhrx8dfkUyhiJVrcZ0HO3veKy2yFQ1CNClg_Xdmz7eDE8yDuApuU39AwTGWDq1NmchL5UTO1pK_-8IH_Q%2CAOrYGsmjLrBMwrHRptRnS7vSm9RsOVXhTluRLGeCtOeFyngeC0wgXBTWRdzZHrSIhWwr2_38ky-KuU9X675WSzi0t7B53504kkRrx5-TKA&amp;pvsid=8440440656907246&amp;tmod=292580776&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C852&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x672_l%7C216x672_r&amp;bz=1.15&amp;ifi=13&amp;uci=a!d&amp;btvi=3&amp;fsb=1&amp;dtd=12007" title="Advertisement" aria-label="Advertisement"></iframe>

從實際應用來看，LoopX 特別適合多日軟體工程。例如讓 Coding Agent 處理 GitHub Issue 時，第一輪可能先分析問題，第二輪修改程式碼，接著跑測試、建立 PR，再等待 Reviewer 回覆。隔天 Reviewer 要求修改後，Agent 還需要理解前一天的 Scope、測試結果與決策，而不是重新閱讀整個 Repository 後猜測當時為什麼這樣改。LoopX 正是為這類跨 Turn、跨時間的工作保存狀態。官方也把 Issue 與 PR Loop 列為主要使用場景。

研究與實驗也是另一個適合的方向。官方提供 Auto Research 的 KNN Demo，讓 proposer、executor 與 evaluator/promoter 等 Agent 平行迭代，同時保存 Todo、Quota、Evidence 與 Targeted Wake。這類流程若只靠聊天紀錄，很快就會累積大量難以判讀的 Context；改成明確記錄假設、結果與下一步，會比較接近真正的研究工作流。

LoopX 也公開了一些長時間案例，但這些數字需要正確解讀。官方展示的 OpenViking Contribution Sequence 與 Auto ML Showcase 都跨越 200 小時以上的 elapsed loop lifetime，不過官方特別說明，這指的是專案從開始到後續 Review／更新所跨越的真實時間，並不是模型連續執行 200 小時，更不是宣稱系統可以完全無人監督地控制正式生產環境。

這個限制其實也說明 LoopX 的設計哲學。它並不把「完全自主」當成唯一目標。官方明確表示 Dangerous Permissions、Publishing、Production Writes 與 Final Ownership 仍應保留在人類手上；當流程碰到需要 Owner、Safety、Publication 或 Private-data Decision 的 Gate 時，Agent 應提出具體問題並等待決策。

因此，如果拿 LoopX 和 AutoGen、LangGraph 等 Agent 編排概念相比，不宜直接把它視為替代品。後者較常處理 Agent 如何組合、節點如何流動以及工具如何被呼叫；LoopX 更專注在長時間任務的 durable state、governance、evidence 與 handoff。現有 Agent 照樣負責工作，只是在外面多了一層能記住「這個專案現在究竟處於什麼狀態」的控制機制。

它也不同於單純使用 `while true` 、Cron 或定時 Prompt 讓 Claude Code、Codex 不斷工作。定時器可以回答「什麼時候再叫醒 Agent」，卻無法完整回答「現在還有沒有值得做的事」、「證據是否足夠」、「是否必須等待人類批准」以及「下一個 Agent 是否有權限接手」。LoopX 嘗試把這些問題轉換成明確的狀態與操作規則。

從適用族群來看，LoopX 並不是一般聊天機器人使用者需要安裝的工具。它更適合已經大量使用 Coding Agent、正在測試長時間 Autonomous Workflow，或需要管理多 Agent 團隊的開發者與研究團隊。尤其當任務開始跨越數小時、數天、多個 PR 或多輪實驗後，單純依賴模型 Context 的限制會越來越明顯，此時 LoopX 這種獨立狀態層才比較能發揮作用。

整體而言，LoopX 解決的不是「怎麼讓 AI Agent 更聰明」，而是另一個逐漸浮現的問題：當 Agent 已經能工作之後，怎麼讓它持續工作而不失去目標、證據與控制。它將 Objective、Todo、Gate、Evidence、Quota 與 Handoff 從聊天內容抽離，保存成可延續的工作狀態，再讓 Codex、Claude Code、Cursor 等不同 Runtime 負責實際執行。對正在探索長時間 Coding Agent、AI Research Agent 或多 Agent 協作的人來說，LoopX 的價值比較接近 Agent Ops 與控制平面，而不是另一個單純的 AI Agent Framework。

<iframe frameborder="0" src="https://0830e02297e33911c2bc3fbf64239d0c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe><iframe frameborder="0" src="https://0830e02297e33911c2bc3fbf64239d0c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>