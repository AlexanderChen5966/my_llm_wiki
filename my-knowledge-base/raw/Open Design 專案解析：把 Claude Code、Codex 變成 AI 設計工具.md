---
title: "Open Design 專案解析：把 Claude Code、Codex 變成 AI 設計工具"
source: "https://www.knightli.com/zh-tw/2026/05/18/open-design-open-source-claude-design-alternative/"
author:
  - "[[KnightLi]]"
published: 2026-05-18
created: 2026-05-22
description: "介紹 nexu-io/open-design 這個開源 AI 設計專案：它如何把 Claude Code、Codex、Cursor、Gemini 等本地 Agent CLI 接入設計工作流，並透過 Skills、Design Systems、本地 daemon 和沙盒預覽生成網頁、投影片、圖片與影片。"
tags:
  - "clippings"
status: processed
---
Open Design 是 nexu-io 推出的開源 AI 設計專案，定位是 local-first、open-source 的 Claude Design / Figma 替代方案。

它想解決的問題很明確：Claude Design 證明了大模型可以直接生成設計製品，但這類能力如果只存在於閉源、雲端、單一模型的產品裡，使用者就很難自託管、接入自己的 Agent、替換模型、沉澱私有設計系統，或者把產物放進本地工作流。

Open Design 的思路不是重新造一個大模型，而是把你電腦上已有的 coding-agent CLI 接進一個設計工作台。Claude Code、Codex、Cursor Agent、Gemini CLI、OpenCode、Qwen、Copilot CLI、Kimi、DeepSeek TUI 等工具，都可以成為它的「設計引擎」。

## Open Design 是什麼

Open Design 可以理解為三個部分的組合：

- 一個 Web UI，用來對話、預覽、管理專案和匯出產物。
- 一個本地 daemon，負責調度 Agent、管理文件、保存專案和提供 API。
- 一組 Skills、Design Systems 和模板，用來約束 Agent 生成更像設計作品，而不是隨手拼出來的 AI 頁面。

使用者輸入需求後，Open Design 不只是把一句話丟給模型。它會先讓使用者補充設計簡報，選擇場景和方向，再把專案元資料、目前設計系統、Skill 文件、模板、檢查清單等上下文注入給 Agent。Agent 在真實文件夾裡讀寫文件，最後生成可在沙盒 iframe 中預覽的 artifact。

<iframe width="954" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1099643069&amp;adf=325096956&amp;w=954&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779327954&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=954x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F18%2Fopen-design-open-source-claude-design-alternative%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=954&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779412293568&amp;bpp=1&amp;bdt=1747&amp;idt=1&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779240418%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0&amp;nras=2&amp;correlator=1378463800982&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=987&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95340253%2C95340255&amp;oid=2&amp;pvsid=2978397797938546&amp;tmod=1184876705&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這讓它更接近「AI 設計工作流」，而不是單次網頁生成器。

## 為什麼它和普通 AI 生成網頁不一樣

很多 AI 工具都能生成一個 HTML 頁面，但 Open Design 的重點不是「讓模型寫頁面」，而是「讓模型按設計流程交付可預覽、可匯出、可迭代的製品」。

它強調幾個設計：

- 先問問題，再生成。新設計 brief 會先出現互動式 question form，用來鎖定受眾、語氣、品牌上下文、約束和視覺方向。
- Skills 是文件，不是黑盒外掛。每個 Skill 由 `SKILL.md` 、 `assets/` 、 `references/` 組成，可以被閱讀、替換和擴展。
- Design Systems 是 Markdown，不是固定主題 JSON。顏色、字體、間距、元件、動效、品牌語氣和反模式都可以寫進 `DESIGN.md` 。
- Agent 在真實專案目錄裡工作。它能讀模板、寫文件、生成圖片、輸出 `.pptx` 、`.pdf` 、`.zip` 等文件。
- 產物在沙盒 iframe 中預覽，減少直接執行不受控程式碼的風險。

這套結構的目標，是讓 AI 更像一個有規範、有素材、有檢查清單的設計協作者。

## 它支援哪些 Agent

<iframe width="954" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1099643069&amp;adf=2370273430&amp;w=954&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779327954&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=954x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F18%2Fopen-design-open-source-claude-design-alternative%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=954&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779412293572&amp;bpp=1&amp;bdt=1751&amp;idt=0&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779240418%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C954x280&amp;nras=3&amp;correlator=1378463800982&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=1910&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95340253%2C95340255&amp;oid=2&amp;pvsid=2978397797938546&amp;tmod=1184876705&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Open Design 的一個亮點是「把 Agent 當運行時」，而不是綁定某一家模型。

README 中列出的支援對象包括 Claude Code、Codex CLI、Devin for Terminal、Cursor Agent、Gemini CLI、OpenCode、Qwen Code、Qoder CLI、GitHub Copilot CLI、Hermes、Kimi、Pi、Kiro、Kilo、Mistral Vibe、DeepSeek TUI 等。它會從 `PATH` 中自動檢測這些 CLI，並允許使用者切換。

如果本地沒有合適的 CLI，也可以走 OpenAI-compatible 的 BYOK proxy，把自己的 `baseUrl` 、 `apiKey` 和模型填進去，讓 daemon 把串流輸出規範化成同一套聊天流。

這種設計的好處是：

- 不鎖定單一模型。
- 可以複用使用者已經安裝和配置好的 Agent。
- 本地文件讀寫由 daemon 管理，權限邊界更清楚。
- 對企業或重度使用者來說，更容易接入自己的模型和 API 供應商。

## Skills 和 Design Systems 是它的核心資產

Open Design 內建了大量 Skills 和 Design Systems。README 中提到，專案內建的 Skills 覆蓋網頁原型、SaaS landing page、dashboard、mobile app、gamified app、社群輪播圖、雜誌海報、PPT、週報、財務報告、HR onboarding、invoice、kanban、OKR 等場景。

<iframe width="954" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1099643069&amp;adf=954940372&amp;w=954&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779327954&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=954x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F18%2Fopen-design-open-source-claude-design-alternative%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=954&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779412293574&amp;bpp=1&amp;bdt=1753&amp;idt=1&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779240418%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C954x280%2C954x280&amp;nras=4&amp;correlator=1378463800982&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=2879&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95340253%2C95340255&amp;oid=2&amp;pvsid=2978397797938546&amp;tmod=1184876705&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Design Systems 則用於給 Agent 提供品牌級視覺約束。倉庫介紹中列出了 Linear、Stripe、Vercel、Airbnb、Tesla、Notion、Apple、Anthropic、Cursor、Supabase、Figma、小紅書等設計系統來源。

這兩者的關係可以這樣理解：

- Skill 決定「這次要交付什麼類型的作品」。
- Design System 決定「作品應該長成什麼品牌風格」。

如果沒有這兩層約束，AI 很容易生成那種看起來熟悉但缺少判斷的通用頁面。有了 Skill 和 Design System，模型至少有了清楚的任務邊界、視覺參考和檢查規則。

## 它能生成什麼

Open Design 不是只做網頁原型。

按 README 的描述，它覆蓋 web、desktop、mobile prototypes、slides、images、videos、HyperFrames 等方向，並支援 HTML、PDF、PPTX、ZIP、Markdown 等匯出形式。媒體生成方面，它把圖片、影片、音訊也放在同一個設計循環裡，例如海報、頭像、資訊圖、地圖插畫、短影片和 HTML 轉 MP4 動效。

<iframe width="954" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1099643069&amp;adf=3097720424&amp;w=954&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779327954&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=954x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F18%2Fopen-design-open-source-claude-design-alternative%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=954&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779412293578&amp;bpp=1&amp;bdt=1757&amp;idt=0&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779412292%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779412292%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779412292%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C954x280%2C954x280%2C954x280%2C970x90&amp;nras=6&amp;correlator=1378463800982&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=3756&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=6&amp;eid=95340253%2C95340255&amp;oid=2&amp;psts=AOrYGsmVYVNi04ywt03iylrcuLeT68YW38y8QElrxpYfnUWAmw0IES6qi4uu4MY4SFeCrtPOV96CIzazc9eHhA-HGeAksCwQZw4eg_fAfGu9fEcmjXFZElisLjQRAw%2CAOrYGsmiHvtwww0GKyww1mjgJlKhOLW1kFQi3h7P9LUFzUKnerP7BkQZo1Pps2ZyHqqXq38DcZGf6A8zHVstkEyhbDpoSMS8vYPX35r0gA%2CAOrYGskFiai8U7jZJGNOvK8Is4Xow1Dp6C526Y7RiDjmB1CDp4WLHC6RqgyfwLtHvczvSv_4mrUjxNzIsHhifc0v9_QJmWCwvUn9-9StNoYQ34qCWV4%2CAOrYGsmCQWwpTJBqFupnll6az5piCi-q1oFTLfjDtR4nr5QyIr9XLTpuMf2gVzsJBM7u42A536KvHeQqaZ3q6G1ONPznevOX1xL_frt8&amp;pvsid=2978397797938546&amp;tmod=1184876705&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=6&amp;uci=a!6&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這意味著它的使用場景可以很寬：

- 創業團隊快速做 pitch deck。
- 產品團隊生成 landing page 或功能原型。
- 營運團隊做活動頁、社群圖、週報。
- 設計師用它做 moodboard、視覺方向和第一版 layout。
- 開發者把需求轉成可運行的前端 artifact。

它的價值不只是「生成一個頁面」，而是把多個內容形態放進同一套 Agent 工作流。

## 本地優先是什麼意思

Open Design 強調 local-first。它不是把所有東西都交給一個遠端 SaaS 後端，而是在本地跑 daemon 和專案工作區。

倉庫 README 裡描述的架構大致是：

- 前端是 Next.js / React / TypeScript。
- 本地 daemon 使用 Node、Express、SQLite 和 SSE。
- 專案、會話、訊息、tab、模板等資料保存在本地 SQLite 和 `.od/projects/<id>/` 目錄。
- Agent 透過 `child_process.spawn` 啟動，並在專案 artifact 文件夾中讀寫。
- 預覽透過沙盒 iframe 渲染。
- 匯出包括 HTML、PDF、PPTX、ZIP、Markdown。

這種結構更適合希望把設計產物留在本機、接入本地 Agent、控制 API key、維護私有工作區的使用者。

<iframe width="954" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=1099643069&amp;adf=1239212730&amp;w=954&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779327954&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=954x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F18%2Fopen-design-open-source-claude-design-alternative%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=954&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779412293580&amp;bpp=1&amp;bdt=1759&amp;idt=1&amp;shv=r20260521&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779412292%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779412292%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779412292%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C954x280%2C954x280%2C954x280%2C970x90%2C954x280&amp;nras=7&amp;correlator=1378463800982&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=4817&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=1086&amp;eid=95340253%2C95340255&amp;oid=2&amp;psts=AOrYGsmVYVNi04ywt03iylrcuLeT68YW38y8QElrxpYfnUWAmw0IES6qi4uu4MY4SFeCrtPOV96CIzazc9eHhA-HGeAksCwQZw4eg_fAfGu9fEcmjXFZElisLjQRAw%2CAOrYGsmiHvtwww0GKyww1mjgJlKhOLW1kFQi3h7P9LUFzUKnerP7BkQZo1Pps2ZyHqqXq38DcZGf6A8zHVstkEyhbDpoSMS8vYPX35r0gA%2CAOrYGskFiai8U7jZJGNOvK8Is4Xow1Dp6C526Y7RiDjmB1CDp4WLHC6RqgyfwLtHvczvSv_4mrUjxNzIsHhifc0v9_QJmWCwvUn9-9StNoYQ34qCWV4%2CAOrYGsmCQWwpTJBqFupnll6az5piCi-q1oFTLfjDtR4nr5QyIr9XLTpuMf2gVzsJBM7u42A536KvHeQqaZ3q6G1ONPznevOX1xL_frt8%2CAOrYGskJ8tUfd8Xsz9nmF-nIgTbMLxpN3uB9fzN7qzUyOXUBVsWiQC5IsSeHe2BsIJw8hA0sHUVloRhPdhs5RTQCq864MkAzy_E0VMmxSQ&amp;pvsid=2978397797938546&amp;tmod=1184876705&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=7&amp;uci=a!7&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

不過 local-first 不等於完全離線。實際生成仍然取決於你使用的 Agent 和模型。如果你用的是雲端模型 API，內容仍然會發往對應供應商。Open Design 更準確的定位，是把工作區、調度、文件和預覽放回本地，把模型層交給使用者自己選擇。

## 和 Claude Design / Figma 的關係

Open Design 在 README 中明確把自己稱為 Claude Design / Figma 的開源替代方向，但它並不是傳統意義上的 Figma 複刻。

Figma 是設計師手動編輯、協作和交付設計稿的專業工具；Open Design 更偏 Agent-native：使用者透過自然語言、表單、Skills 和設計系統驅動 Agent 產出可運行 artifact。

它更像是把幾個東西合在一起：

- Claude Design 的 artifact-first 體驗。
- Figma 的設計系統意識。
- Claude Code / Codex 這類 Agent 的文件讀寫和執行能力。
- 本地 daemon 的專案管理和沙盒預覽。

所以它未必會替代專業設計師的全流程工具，但很適合做「從想法到可預覽原型」的快速通道。

## 適合誰使用

Open Design 更適合這些人：

- 已經在使用 Claude Code、Codex、Cursor、Gemini CLI 等 Agent 的開發者。
- 想把 AI 設計產物放到本地專案目錄裡管理的人。
- 需要快速生成網頁原型、PPT、海報、營運素材的創業團隊。
- 希望自定義 Skills、Design Systems、提示詞棧的進階使用者。
- 不想被單一模型或單一雲端產品鎖住的團隊。

不太適合這些人：

- 只想打開網頁、輸入一句話、立刻下載圖片的輕量使用者。
- 完全不想碰 Node、pnpm、daemon、CLI 和本地配置的人。
- 需要成熟多人協作、設計評審和向量編輯能力的專業 Figma 流程。

換句話說，Open Design 更像給 Agent 使用者和技術型設計團隊準備的工具，而不是面向所有人的輕量設計 SaaS。

## 需要注意的地方

Open Design 的 README 標註了 `0.8.0-preview` ，並說明專案仍在快速迭代。這類專案的活力很強，但也意味著 API、資料目錄、桌面版遷移、Skills 結構和匯出流程可能還會變化。

使用前最好注意幾點：

- 不要把它當成穩定企業級設計平台。
- 先用測試專案體驗工作流，再匯入重要資料。
- 如果要遷移 `.od/` 資料，先備份，確保 daemon 和桌面應用都已停止。
- BYOK 時注意 API key、代理地址和本地私有網路存取風險。
- 生成的設計仍需要人工審查，尤其是品牌、版權、文案和視覺一致性。

開源專案最大的好處是可檢查、可改、可貢獻；代價是你需要接受一定的工程摩擦。

## 小結

Open Design 的看點，不只是「開源版 Claude Design」。它真正有意思的地方，是把 Agent CLI、Skills、Design Systems、本地 daemon 和沙盒預覽組織成一個設計工作流。

它把設計生成從單次 prompt 推向了更結構化的流程：先問問題，選方向，載入設計系統，讀取 Skill，寫入真實文件，預覽 artifact，再匯出結果。

如果你已經習慣用 Claude Code、Codex 或 Cursor 處理程式碼，那麼 Open Design 值得關注。它代表了一種新的產品形態：不是 AI 幫你畫一張圖，而是 AI 在本地專案空間裡，按設計系統和任務技能，生成一組可以繼續迭代的設計製品。