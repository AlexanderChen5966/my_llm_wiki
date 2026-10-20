---
title: "Codex with ChatGPT：讓 ChatGPT 規劃、Codex 寫 Code，再交回 ChatGPT 自動 Review 的節省 Codex token 用量的 Skill | 網路資源"
source: "https://tools.wingzero.tw/articles/4041"
author:
  - "[[赤紅翼零]]"
published: 2026-08-29
created: 2026-09-01
description: "Codex with ChatGPT 是開源 AI Coding Workflow，將 ChatGPT 作為 Planner／Reviewer，Codex 作為實際程式修改與測試的 Executor，並透過唯讀 MCP Bridge 讓 ChatGPT 直接檢查 Workspace、Git Diff 與 Test Status。"
tags:
  - "clippings"
status: "processed"
---
網站連結 [https://github.com/XiaoDuoYa/codex-with-chatgpt](https://github.com/XiaoDuoYa/codex-with-chatgpt)

[Codex with ChatGPT](https://github.com/XiaoDuoYa/codex-with-chatgpt?utm_source=chatgpt.com) 是一套把 ChatGPT 網頁版與 Codex Coding Agent 串在一起的開源工具，核心概念可以濃縮成專案的一句話：「ChatGPT thinks. Codex works.」也就是讓 ChatGPT 負責需求分析、規劃與程式碼審查，而 Codex 繼續負責真正的程式修改、Shell 指令、測試與修正，形成一套「ChatGPT 當大腦、Codex 當執行者」的 AI Coding Workflow。

它特別之處在於，這套架構並不是另外呼叫 ChatGPT API，也不是把 OpenAI API Key 塞進 Codex。專案希望利用使用者既有的 ChatGPT 網頁版訂閱，將原本可能由 Coding Agent 消耗 Token 執行的 Planning 與 Review 工作交給 ChatGPT Web，再透過受到 OAuth 保護的唯讀 MCP Bridge，讓 ChatGPT 在需要時查看本機 Workspace 的特定程式碼。Codex 則繼續掌握所有實際執行權限。

這也是理解 Codex with ChatGPT 最重要的一點：它不是「另一個 Coding Agent」，而是一個讓兩個 AI 工作環境分工合作的橋接層。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788224965&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4041&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTc0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3NCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3NCJdXSwwXQ..&amp;dt=1788224746854&amp;bpp=2&amp;bdt=1354&amp;idt=2&amp;shv=r20260825&amp;mjsv=m202608270101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788224854%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788224854%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788224854%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1634x905%2C784x280&amp;nras=3&amp;correlator=2098544424175&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=3542&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399630%2C95400781%2C122880598%2C122880743%2C122880333&amp;oid=2&amp;pvsid=157430849057270&amp;tmod=684280329&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=0&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

整體架構大致可以理解為：

`需求 → ChatGPT 規劃 → Codex 執行 → ChatGPT Review → Codex 修正 → 完成`

其中 ChatGPT 主要處理 Reason、Plan 與 Review；Codex Harness 則負責 Edit、Git、Shell、Tests 與 Fix。

兩者中間透過 C2C Bridge 溝通。

專案把這套通訊進一步拆成 Control Plane 與 Data Plane。Control Plane 使用 Computer Use 傳遞非常小的結構化狀態訊息，工作狀態依序包含 `INIT → PLAN → EXECUTED → REVIEW → DONE` 。按照專案設計，Diff、Log 與完整 File Body 不會透過這個 Control Plane 直接貼來貼去。

真正需要查看 Workspace 時，則透過 MCP Data Plane 處理。ChatGPT 可以主動取得自己需要的資訊，而不是要求 Codex把大量程式碼複製到聊天視窗。

目前 C2C Bridge 提供 8 個唯讀工具：

- `workspace_info` ：取得目前 Workspace 基本資訊
- `list_directory` ：查看目錄結構
- `read_file` ：讀取需要的程式碼
- `search_workspace` ：搜尋 Workspace
- `git_status` ：檢查 Git 狀態
- `git_diff` ：查看實際程式修改
- `test_status` ：取得測試狀態
- `execution_summary` ：查看 Codex 執行摘要

「唯讀」是整套架構很關鍵的安全邊界。

ChatGPT 可以讀取必要的程式碼、搜尋 Workspace、查看 Git Diff 與 Test Status，但不能透過這個 MCP Bridge 修改檔案。真正具有 Edit、Git、Shell 與 Tests 執行能力的仍然是 Codex Harness。因此即使 ChatGPT 負責規劃，它也不會直接取得本機專案的寫入權限。

這種設計對 Code Review 尤其有意思。

一般雙 Agent Workflow 很容易出現一個問題：執行 Agent 修改完程式後，只回報「已經完成，所有測試通過」，Review Agent 實際上只是在審查執行 Agent 自己寫的摘要。

Codex with ChatGPT 則希望讓 ChatGPT 自己透過 MCP 查看真正的 `git_diff` 與 Test Records。也就是說，ChatGPT 不必完全相信 Codex 回報「Tests Passed」，而可以直接查看 Workspace 中實際發生的變化，再決定修改是否符合原本規劃。

因此它建立的並不只是：

`AI A → 告訴 AI B 結果`

而更接近：

`AI A 執行 → AI B 獨立檢查實際結果`

對複雜 Coding Task 而言，這種 Planning、Execution 與 Review 分離的架構，比讓同一個 Agent 從頭做到尾更有實驗價值。

另一個核心訴求是減少不必要的 API／Codex 額度消耗。專案作者提出的問題是：部分使用者已經付費訂閱 ChatGPT Plus 或 Pro，但 Coding Agent 在 Planning、Reasoning 與 Review 階段仍然持續消耗另外的 API 或 Codex 資源。Codex with ChatGPT 因此嘗試把比較偏「思考」的工作交給已經訂閱的 ChatGPT Web，而 Codex 專注在真正需要操作開發環境的工作。

需要特別注意的是，這並不等於「Codex 從此完全不消耗額度」。Codex 仍然是實際 Coding Harness，仍要執行修改、測試與修正。這套工具主要改變的是工作分配，而不是把 Codex 本身完全替換掉。

在資料處理方式上，專案也刻意避免直接上傳整個 Repository。官方說明是 Repository 不會被整包 Upload 給 ChatGPT，而是 ChatGPT 透過唯讀 MCP Connection，依需求讀取目前 Workspace 中真正需要的內容。

換句話說，工作模式比較像：

`ChatGPT：「我要看 src/auth/login.ts 的相關內容」 → MCP 讀取 → 回傳需要的資訊`

而不是：

`把整個 Repository 打包 → 上傳 ChatGPT`

這對大型專案尤其有意義，因為 Codebase 可能包含數千甚至數萬個檔案，真正處理某個 Bug 時通常只需要其中很小一部分。

連線層則使用 OAuth 2.1、一次性 Pairing Code 與 Cloudflare Quick Tunnel。Bridge 本身是 Loopback-only HTTP Server，再透過 Tunnel 建立 ChatGPT 能存取的安全連線。專案還實作 PKCE、Dynamic Client Registration、Refresh Rotation 與 Revocation 等驗證機制。

整套工具的主要特色可以整理為：

- ChatGPT Web 負責 Reasoning、Planning 與 Review
- Codex 負責修改程式、Shell、Git、Tests 與 Fix
- 不需要另外提供 ChatGPT API Key
- 不使用 Reverse Proxy 模擬 ChatGPT API
- 透過唯讀 MCP Bridge 讀取本機 Workspace
- ChatGPT 無法透過 Bridge 修改 Repository
- OAuth 2.1 與一次性 Pairing Code
- 使用 Cloudflare Quick Tunnel 建立連線
- ChatGPT 可以直接檢查 Git Diff
- ChatGPT 可以查看 Test Status 與 Execution Summary
- Planning、Execution、Review 被拆成明確工作階段
- 提供 `c2c` CLI 管理 Bridge、配對、診斷與 Log

實際使用方面，作者也刻意把安裝流程設計成可以交給 Coding Agent 處理。使用者可以直接把官方準備好的安裝指示貼給 Codex，讓 Agent 自行檢查 Git、Node.js 20 以上版本與 `cloudflared` ，接著 Clone Repository、使用 pnpm Build、安裝 Skill，再執行首次 Setup。

如果採手動方式，核心概念則簡單許多：先把 Repository 中的 `skill/` 安裝到 Codex Skills 目錄，接著要求 Codex 執行 Codex with ChatGPT 的首次設定，之後就可以直接要求 Codex 使用這套 Workflow 完成指定功能。

首次設定成功後，工具會檢查 Project、啟動 Workspace Bridge、建立安全連線、連接 ChatGPT，最後執行 File Read Test。一般使用者主要需要人工介入的環節，是 ChatGPT、Cloudflare 登入、驗證碼或 2FA 等必須由帳號本人完成的操作。

日常管理則提供 `c2c` CLI，包括 Setup、Status、Doctor、Pair、Unpair、Logs 與 Stop 等功能。對一般使用者而言，不一定需要理解底層 MCP、OAuth、Tunnel 與 Port 如何運作；但發生問題時，這些 CLI 可以用來診斷連線與 Bridge 狀態。

專案本身使用 TypeScript 開發，要求 Node.js 20 以上版本，Package Manager 採 pnpm。依目前 `package.json` ，主要依賴包括 Model Context Protocol SDK、Express、Commander、Zod 等，測試則使用 Vitest。專案採 MIT License。

從 Repository 結構也能看出它不是單純一份 Prompt。 `src/bridge` 負責本機 HTTP Bridge 與 Port Recovery； `src/mcp` 實作 8 個唯讀 MCP Tools； `src/auth` 處理 OAuth 2.1； `src/pairing` 負責一次性 Pairing Code； `src/workspace` 處理 Path Containment、Sensitive-file Policy、Search 與 Git； `src/tunnel` 管理 Cloudflare Tunnel； `src/execution` 保存 Review Loop 所需的 Execution Records，而 `skill/` 才是使用者實際與 Codex 互動的 UX 層。

從使用情境來看，Codex with ChatGPT 比較適合已經同時使用 ChatGPT 與 Codex 的開發者。如果只是偶爾要求 AI 修改一個小檔案，引入額外 Bridge、Tunnel 與雙 Agent Review Workflow 未必比較有效率；但面對需要先研究 Codebase、規劃多步驟修改、執行後再進行獨立 Review 的任務，這種角色分離就比較有意義。

例如開發一個新 Authentication Feature，可以先讓 ChatGPT 透過唯讀 MCP 搜尋相關程式碼，理解目前架構後提出 Implementation Plan；Codex 根據 Plan 修改實際檔案並執行 Tests；完成後 ChatGPT 再直接讀取 `git_diff` 與 `test_status` ，檢查實作是否偏離原本需求。若發現問題，再把 Review 結果交回 Codex 修正。

它和單純「開兩個 AI 視窗互相 Copy & Paste」的差異也很明顯。手動方式需要不斷複製 Code、Diff、錯誤訊息與 Review 意見；Codex with ChatGPT 則利用 MCP 讓 ChatGPT自己讀取 Workspace，需要傳遞的控制訊息保持很小，減少人類充當兩個 Agent 中間轉接器的工作。

不過，這套架構也帶來額外複雜度。使用者需要在本機執行 Bridge，並建立 ChatGPT 可以連線的 Tunnel；企業或處理敏感程式碼的團隊，也應自行審查 MCP 暴露範圍、OAuth 設計、Tunnel、安全政策與 ChatGPT 資料處理方式，而不能因為 Bridge 是 Read-only 就直接等同於「沒有資料外傳風險」。唯讀代表不能透過該介面修改檔案，不代表讀取出去的內容從未離開本機。

此外，這是一個社群開源專案，而不是 OpenAI 官方推出的 Codex／ChatGPT 整合功能。Repository 本身也明確標示為 Unofficial Community Project，並未受到 OpenAI 官方背書。若工作環境有企業資安、程式碼保密或合規要求，這一點尤其需要納入評估。

目前專案將狀態標示為 V1，並表示 Bridge、OAuth + Pairing、Public Tunnel、ChatGPT Connector Setup 與首次執行流程已完成 End-to-end 驗證。不過從專案成熟度來看，它仍屬於相當早期的工具，因此更適合喜歡嘗試 Agent Workflow、MCP 與 AI Coding Automation 的開發者，而不是直接假設它已經等同成熟企業級 Coding Infrastructure。

整體而言，Codex with ChatGPT 最有意思的並不是「把兩個 OpenAI 工具接在一起」，而是重新分配 AI Coding Agent 的角色：ChatGPT 負責閱讀、思考、規劃與獨立 Review，Codex 負責真正碰程式碼、跑指令與測試。再利用唯讀 MCP Bridge 讓 Reviewer 可以自行檢查 Codebase 與 Git Diff，而不是只相信 Executor 的摘要。對同時訂閱 ChatGPT、又經常使用 Codex 開發的使用者而言，它提供了一種值得研究的「Planner + Executor + Reviewer」AI Coding Workflow。

<iframe frameborder="0" src="https://d120dacd1abd9c060fae7559df46790c.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>