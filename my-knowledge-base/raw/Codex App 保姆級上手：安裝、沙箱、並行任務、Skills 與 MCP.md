---
title: "Codex App 保姆級上手：安裝、沙箱、並行任務、Skills 與 MCP"
source: "https://www.knightli.com/zh-tw/2026/05/06/codex-app-complete-guide-skills-mcp/"
author:
  - "[[KnightLi]]"
published: 2026-05-06
created: 2026-05-20
description: "Codex App 完整上手指南：安裝登入、專案與任務管理、沙箱權限、上下文、模型選擇、Git、worktree、雲端執行、AGENTS.md、外掛、Skills、MCP 和電腦自動化。"
tags:
  - "clippings"
status: processed
---
Codex App 可以理解成一個面向 AI 編程的任務工作台。它不是傳統 IDE，也不是單純聊天視窗，而是把多任務、專案管理、沙箱權限、Git、雲端執行、外掛、Skills、MCP 和自動化集中到一個介面裡。

如果你已經在用 Codex CLI、Claude Code、Cursor 或其他 coding agent，Codex App 最值得關注的地方，是它把「多個 agent 並行幹活」做成了更清楚的桌面工作流。

## Codex App 適合做什麼

Codex App 的核心價值不是讓 AI 回答問題，而是讓 AI 在專案目錄裡持續執行任務：

- 修改程式碼、執行命令、啟動開發服務。
- 管理多個專案和多個任務。
- 在本地或雲端執行長任務。
- 呼叫外掛、Skills 和 MCP 擴展能力。
- 透過 Git、worktree 和 PR 管理改動。

OpenAI 官方也把 Codex App 定位成一個用於管理多個 coding agent 的介面。它適合需要同時推進多個程式碼任務的人，尤其適合前端頁面、腳本工具、小型應用、文件整理和自動化工作流。

## 安裝前準備

<iframe width="933" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4070477712&amp;adf=325096956&amp;w=933&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779162132&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=933x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F06%2Fcodex-app-complete-guide-skills-mcp%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=933&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779240419616&amp;bpp=1&amp;bdt=1373&amp;idt=1&amp;shv=r20260519&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779093088%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779093088%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779093088%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0&amp;nras=2&amp;correlator=2057771736783&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=968&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390788%2C42533294%2C95390681%2C95340252%2C95340254&amp;oid=2&amp;pvsid=5023408509253606&amp;tmod=744773935&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=50552" title="Advertisement" aria-label="Advertisement"></iframe>

使用 Codex App 前，建議先準備好三個基礎工具：

1. `Git`
2. `Node.js`
3. `VS Code` 或你常用的 IDE

Codex App 支援 macOS 和 Windows。安裝後使用 ChatGPT 帳號登入，首次進入時可以選擇主要使用場景，例如編程或日常工作。Codex 會根據選擇預裝一部分外掛和 Skills，後續也可以在設定和外掛市場裡調整。

Windows 和 macOS 的主要功能大體一致，但部分電腦自動化能力可能依賴具體平台和外掛支援，實際以你目前版本顯示為準。

## 介面結構：專案、任務和對話

Codex App 是典型的三欄布局：

- 左側：專案、任務、歷史對話、外掛和自動化入口。
- 中間：目前對話視窗。
- 右側：檔案、瀏覽器、終端機、執行結果等多功能區域。

一個專案通常對應一個本地資料夾。你可以在同一個專案裡開多個對話，也可以同時打開多個專案，讓不同 agent 並行執行任務。

<iframe width="933" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4070477712&amp;adf=2370273430&amp;w=933&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779162132&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=933x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F06%2Fcodex-app-complete-guide-skills-mcp%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=933&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779240419619&amp;bpp=1&amp;bdt=1375&amp;idt=0&amp;shv=r20260519&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779093088%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779093088%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779093088%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C933x280&amp;nras=3&amp;correlator=2057771736783&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=1968&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390788%2C42533294%2C95390681%2C95340252%2C95340254&amp;oid=2&amp;pvsid=5023408509253606&amp;tmod=744773935&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=50554" title="Advertisement" aria-label="Advertisement"></iframe>

任務列表會顯示不同狀態：

- 正在執行：agent 仍在執行。
- 等待批准：需要你確認權限、連網、安裝依賴或執行高風險操作。
- 已完成：任務已經結束，可以查看結果或繼續追問。

這比在多個終端視窗裡來回切換更直觀，也更適合同時管理多個 AI 任務。

## 沙箱與權限控制

Codex App 的權限體系圍繞沙箱展開。預設情況下，目前專案資料夾會成為 agent 的主要工作範圍。

常見權限邊界包括：

- 可以讀取和修改專案目錄內的檔案。
- 預設不能隨意修改專案外檔案。
- 預設會限制連網或高風險命令。
- 需要越權時，會向使用者申請批准。

比較實用的模式是「自動審查」：低風險操作由系統自動放行，高風險操作再交給使用者確認。這樣既能減少頻繁彈窗，又能避免 agent 在不知情的情況下執行危險操作。

<iframe width="933" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4070477712&amp;adf=954940372&amp;w=933&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779162132&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=933x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F06%2Fcodex-app-complete-guide-skills-mcp%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=933&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779240419620&amp;bpp=1&amp;bdt=1377&amp;idt=1&amp;shv=r20260519&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779093088%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779093088%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779093088%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C933x280%2C933x280&amp;nras=4&amp;correlator=2057771736783&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=2934&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390788%2C42533294%2C95390681%2C95340252%2C95340254&amp;oid=2&amp;pvsid=5023408509253606&amp;tmod=744773935&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=50563" title="Advertisement" aria-label="Advertisement"></iframe>

「完全存取」要謹慎開啟。它適合你明確知道 agent 要做什麼，並且專案已經有 Git 備份、重要檔案也有額外備份的場景。日常使用不建議長期保持完全存取。

## 上下文、模型和額度

Codex App 會顯示目前對話的上下文使用情況。對話越長，歷史內容越多，模型需要處理的上下文也越大。

實用建議是：

- 一個任務完成後，優先開新對話。
- 長對話可以手動壓縮，但不要把壓縮當成萬能記憶。
- 複雜任務先寫清楚目標、邊界和驗收標準。
- 不要把大量無關日誌、報錯和檔案一次性塞進對話。

模型選擇上，可以根據任務複雜度調整思考強度。簡單修改、文案整理、重複任務不一定需要最高規格模型；架構遷移、疑難 bug、跨檔案重構則更適合使用更強模型。

如果介面裡有快速模式，也要注意它通常會帶來更高的額度消耗。趕時間時可以開，日常不必預設開啟。

<iframe width="933" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4070477712&amp;adf=3097720424&amp;w=933&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779162132&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=933x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F06%2Fcodex-app-complete-guide-skills-mcp%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=933&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779240419622&amp;bpp=1&amp;bdt=1379&amp;idt=0&amp;shv=r20260519&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779240418%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C933x280%2C933x280%2C933x280%2C728x90&amp;nras=6&amp;correlator=2057771736783&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=3811&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=72&amp;eid=95390788%2C42533294%2C95390681%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGslkyM4vRFopPtVNg27zxVz5PGNSsu4zLC9RmwRlSU6eD09kKYr83ILOzKHn5vJYEqglOKOtvU5NcP3AQKRZVlaO2XPpzrojCxXN%2CAOrYGsmYoVxP7zwVTdu3cDXIFTaDTtx3hbPWh33CUhkDa7SzpNihubZEdwvfPFATpbHW48yZYUQqFbRwqWeyEqHN3iiYAEz34f_mWYnnlQ%2CAOrYGsmfwUwE9qWfiNq7PhvGEVmdRouYLJxGgoqWteZE5DKos4lfWJUYNhLQJeB5X5nw4dr9vWLgPViE0az22kHUVyc1ksVyk1fnk3U3&amp;pvsid=5023408509253606&amp;tmod=744773935&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=5&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 圖片生成與多模態輸入

Codex App 可以接收圖片和檔案作為上下文，也可以在合適場景中呼叫圖像生成能力。

這對前端和內容類專案很有用。例如你可以讓 Codex：

- 根據截圖修正頁面樣式。
- 替換網頁中的不合適配圖。
- 生成產品圖、輪播圖或頁面素材。
- 根據 UI 截圖指出需要修改的位置。

更高效的用法是：不要只說「改好看一點」，而是結合截圖指出具體問題，例如「這個卡片間距太大」「這裡的圖和服務場景不匹配」「把地圖區域做得更清楚」。

## Steer：任務執行中及時糾偏

`Steer` 可以理解成「執行中接管方向」。當 agent 已經開始執行，但你發現它理解錯了方向，不應該等它全部跑完再改。

這時可以使用引導功能，把新的指令插入目前執行流程，讓 Codex 及時修正路線。

<iframe width="933" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4070477712&amp;adf=1239212730&amp;w=933&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779162132&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=933x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F06%2Fcodex-app-complete-guide-skills-mcp%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=933&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779240419624&amp;bpp=1&amp;bdt=1381&amp;idt=0&amp;shv=r20260519&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779240418%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C933x280%2C933x280%2C933x280%2C728x90%2C933x280&amp;nras=7&amp;correlator=2057771736783&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=4725&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=977&amp;eid=95390788%2C42533294%2C95390681%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGslkyM4vRFopPtVNg27zxVz5PGNSsu4zLC9RmwRlSU6eD09kKYr83ILOzKHn5vJYEqglOKOtvU5NcP3AQKRZVlaO2XPpzrojCxXN%2CAOrYGsmYoVxP7zwVTdu3cDXIFTaDTtx3hbPWh33CUhkDa7SzpNihubZEdwvfPFATpbHW48yZYUQqFbRwqWeyEqHN3iiYAEz34f_mWYnnlQ%2CAOrYGsmfwUwE9qWfiNq7PhvGEVmdRouYLJxGgoqWteZE5DKos4lfWJUYNhLQJeB5X5nw4dr9vWLgPViE0az22kHUVyc1ksVyk1fnk3U3%2CAOrYGsmT4tCJVh08kw9zR1yRvs-8oaXmzAv6vTT3g3Iv8x3tBg6kuX1dlOQb5C_jBWmP4USu_dgfdbWdg3STLDrnU_FiMkI7wumqClEx-g&amp;pvsid=5023408509253606&amp;tmod=744773935&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

適合使用 Steer 的場景包括：

- agent 誤解了需求。
- 生成的頁面風格明顯不對。
- 正在執行的方案成本太高。
- 你臨時補充了關鍵約束。

一般建議保留預設排隊行為，只在確實需要干預時手動使用 Steer。這樣既不會打亂正常任務，也能在關鍵時刻把方向拉回來。

## 計劃模式和內建瀏覽器

複雜任務建議先開計劃模式。計劃模式下，Codex 不會馬上改程式碼，而是先輸出方案，必要時還會用卡片形式追問關鍵選擇。

適合開計劃模式的任務：

- 框架遷移，例如從普通 React 專案遷到 Next.js。
- 大範圍重構。
- 涉及資料庫、認證、部署的功能。
- 你還沒想清楚技術路線的需求。

Codex App 的右側區域可以打開內建瀏覽器，用來預覽本地開發服務。你可以在頁面上做批註，讓 Codex 按具體 UI 位置修改問題。這種「看頁面、點位置、讓 AI 改」的流程，比純文字描述更適合前端除錯。

<iframe width="933" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-9230324840884748&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4070477712&amp;adf=2965993031&amp;w=933&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1779162132&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=7012339427&amp;ad_type=text_image&amp;format=933x280&amp;url=https%3A%2F%2Fwww.knightli.com%2Fzh-tw%2F2026%2F05%2F06%2Fcodex-app-complete-guide-skills-mcp%2F&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=933&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTY4IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNjgiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE2OCJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779240419626&amp;bpp=1&amp;bdt=1383&amp;idt=0&amp;shv=r20260519&amp;mjsv=m202605140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D2ad0dd00112c22b6%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MZUVL6EEx609_-OmiB6hBxvIwlXZw&amp;gpic=UID%3D000013644fab44ba%3AT%3D1776996875%3ART%3D1779240418%3AS%3DALNI_MYtMcifdGmdh7Zllq8N3zJOP6FF0Q&amp;eo_id_str=ID%3D0279f7b97ef3b3fe%3AT%3D1776996875%3ART%3D1779240418%3AS%3DAA-AfjZDRcmnpfMypQPVPPG8sx5f&amp;prev_fmts=0x0%2C933x280%2C933x280%2C933x280%2C728x90%2C933x280%2C933x280&amp;nras=8&amp;correlator=2057771736783&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=226&amp;ady=5785&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=2064&amp;eid=95390788%2C42533294%2C95390681%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGslkyM4vRFopPtVNg27zxVz5PGNSsu4zLC9RmwRlSU6eD09kKYr83ILOzKHn5vJYEqglOKOtvU5NcP3AQKRZVlaO2XPpzrojCxXN%2CAOrYGsmYoVxP7zwVTdu3cDXIFTaDTtx3hbPWh33CUhkDa7SzpNihubZEdwvfPFATpbHW48yZYUQqFbRwqWeyEqHN3iiYAEz34f_mWYnnlQ%2CAOrYGsmfwUwE9qWfiNq7PhvGEVmdRouYLJxGgoqWteZE5DKos4lfWJUYNhLQJeB5X5nw4dr9vWLgPViE0az22kHUVyc1ksVyk1fnk3U3%2CAOrYGsmT4tCJVh08kw9zR1yRvs-8oaXmzAv6vTT3g3Iv8x3tBg6kuX1dlOQb5C_jBWmP4USu_dgfdbWdg3STLDrnU_FiMkI7wumqClEx-g%2CAOrYGsnn8N8QVxuvjT8tAGWakFM-diSjeiOrVdIZk6apkm8L4N12jH-upULWgWiPlrYnuiLV-s8muEJUi74HkpbjOb3nOhPSfmxyLes&amp;pvsid=5023408509253606&amp;tmod=744773935&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## Git、IDE 和程式碼回滾

Codex App 不是完整 IDE。它可以查看程式碼、批註程式碼，但真正的手寫編輯仍然更適合交給 VS Code、Cursor、Windsurf 等 IDE。

建議每個 Codex 專案都盡早初始化 Git：

1. 讓 Codex 建立或檢查 `.gitignore` 。
2. 在完成一個可用狀態後提交一次。
3. 每次大改前確保有乾淨提交點。
4. 不滿意時用 Git 回滾程式碼。

如果只回滾對話歷史，程式碼不會自動跟著回滾。比較穩的做法是：對話層面回到合適節點，程式碼層面用 Git commit hash 回到對應狀態。

## Worktree：並行開發多個方向

`git worktree` 是 Codex App 裡非常適合並行 agent 的能力。

它的本質是：基於同一個倉庫建立多個獨立工作目錄，每個目錄對應不同分支。這樣你可以讓不同 agent 在不同資料夾裡同時開發，不互相覆蓋。

典型用法：

- 一個 worktree 優化客戶評價元件。
- 一個 worktree 調整門店資訊和地圖布局。
- 兩個任務完成後分別合併回主幹。
- 合併完成後刪除臨時 worktree。

這比在同一個目錄裡讓多個 agent 同時改程式碼穩得多。遇到衝突時，也可以按正常 Git 流程 review 和合併。

## 雲端執行環境

Codex 不只能在本機工作，也可以把任務委託到雲端環境執行。

雲端執行適合這些場景：

- 出門在外，手邊只有手機。
- 希望 agent 在背景跑長任務。
- 程式碼已經同步到 GitHub，需要讓 Codex 基於遠端倉庫修改。
- 希望透過 PR 形式審查和合併改動。

典型流程是：先把本地程式碼推到 GitHub，再讓 Codex 在雲端環境中拉取倉庫、執行任務、生成改動，最後以 PR 或 diff 的方式交給你審核。

本地繼續開發時，要記得把遠端最新改動同步回來。

## 記憶系統：寫好 AGENTS.md

新對話預設沒有完整歷史記憶。專案一複雜，如果每次都重新解釋背景，會很浪費。

最通用的做法，是在專案根目錄維護 `AGENTS.md` 。這個檔案可以記錄：

- 專案目標和主要技術棧。
- 常用命令。
- 目錄結構說明。
- 程式碼風格和命名約定。
- 禁止事項，例如不要批量刪除檔案。
- 測試、建置和部署規則。

你也可以讓 Codex 先通讀專案，再生成一版 `AGENTS.md` ，然後人工審一遍。複雜專案裡，這個檔案非常值得維護。

全域規則也要謹慎。適合寫入所有專案都通用的安全約束，例如「不要遞迴刪除目錄」「執行破壞性操作前必須確認」。不要把某個專案的細節寫進全域規則，否則會污染其他專案。

## 外掛和自動化

外掛用於把外部服務接入 Codex，例如 GitHub、Gmail、Google Drive、資料庫、部署平台等。

外掛的價值在於減少複製貼上。比如你可以讓 Codex：

- 查看某個 GitHub 倉庫的 star 趨勢。
- 整理郵件內容並發給自己。
- 定期執行某個檢查任務。
- 把結果寫成摘要。

自動化則適合重複任務。例如每週五下午檢查一次倉庫資料，並發送郵件報告。簡單自動化任務通常不需要最高規格模型，選擇較輕量的模型即可。

## Skills：把工作流變成可複用能力

Skills 是 Codex 的「專業說明書」。它不是單次提示詞，而是把某類任務的流程、規範、腳本和注意事項沉澱下來，讓 Codex 後續穩定複用。

常見來源有三類：

- 官方 Skills。
- 第三方 Skills。
- 自己編寫的 Skills。

適合做成 Skill 的工作：

- 把字幕整理成圖文筆記。
- 按公司規範寫週報。
- 批量處理圖片或文件。
- 固定格式的程式碼審查。
- 特定框架的專案初始化。

如果某個提示詞你已經複製貼上了很多次，就值得考慮把它做成 Skill。

## MCP：接入外部工具和資料庫

MCP 可以理解成給大模型使用的標準化工具協議。透過 MCP，Codex 可以呼叫外部服務完成更具體的任務。

例如接入 Supabase 後，可以讓 Codex：

- 建立資料表。
- 讀取資料庫結構。
- 修改後端接口。
- 把前端表單提交到資料庫。
- 根據資料庫狀態除錯問題。

這類能力很強，但也要注意權限邊界。資料庫、生產環境、部署平台、郵件帳號都屬於高風險資源。第一次接入時，最好使用測試專案和低權限帳號。

## 部署外掛

部署平台外掛可以讓 Codex 直接完成建置和發布，例如把前端專案部署到 Netlify 一類平台。

這類外掛適合小型網站、原型頁面、內部工具和展示專案。實際使用時建議注意：

- 部署前先跑本地建置。
- 環境變數不要直接寫進程式碼。
- 發布後檢查頁面是否能正常訪問。
- 生產專案要保留人工 review。

AI 可以幫你打通發布流程，但發布權限仍然要謹慎管理。

## 電腦自動化

在支援的平台和外掛環境裡，Codex 還可以操作瀏覽器或桌面應用，完成更接近 RPA 的任務。

例如：

- 打開聊天軟體並準備一條訊息。
- 瀏覽專案看板，彙總任務狀態。
- 生成英文簡報。
- 在你確認後發送給指定對象。
- 把這套流程做成定時自動化。

這類功能很有想像力，但也最需要安全邊界。凡是涉及發訊息、發郵件、提交表單、付款、刪除資料的操作，都應該保留人工確認。

## 使用建議

Codex App 的正確打開方式，不是讓它一次性全自動接管一切，而是把任務拆清楚，讓它在受控環境裡高效執行。

推薦習慣：

- 每個專案先初始化 Git。
- 複雜任務先開計劃模式。
- 並行任務優先用 worktree。
- 專案規則寫進 `AGENTS.md` 。
- 高風險操作保留人工確認。
- 重複流程沉澱成 Skill 或自動化。
- 外掛和 MCP 先在測試環境驗證。

## 參考資料

- [Introducing the Codex app - OpenAI](https://openai.com/index/introducing-the-codex-app/)
- [Using Codex with your ChatGPT plan - OpenAI Help Center](https://help.openai.com/en/articles/11369540-codex-in-chatgpt)
- [Plugins and skills - OpenAI Academy](https://openai.com/academy/codex-plugins-and-skills/)

## 小結

Codex App 的重點不是「多一個 AI 聊天視窗」，而是把 AI 編程變成一個可管理的工作台：本地專案、雲端任務、Git、worktree、外掛、Skills、MCP 和自動化都能串起來。

真正用好它的關鍵，是在「放權」和「控制」之間找到平衡。小任務可以大膽交給 Codex，複雜任務先讓它計劃，高風險動作必須確認。這樣它才會從一個會寫程式碼的助手，變成可以長期協作的工程工具。