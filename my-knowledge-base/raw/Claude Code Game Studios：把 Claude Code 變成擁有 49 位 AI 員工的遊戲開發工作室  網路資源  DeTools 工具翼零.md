---
title: "Claude Code Game Studios：把 Claude Code 變成擁有 49 位 AI 員工的遊戲開發工作室 | 網路資源 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3942"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-13
created: 2026-06-15
description: "Claude Code Game Studios 是專為 Claude Code 打造的 AI 遊戲開發工作室框架，透過 49 個 Agents 與 72 種工作流程模擬真實團隊協作，涵蓋企劃、設計、開發、測試到發行流程，協助獨立開發者建立更有組織、更可控的 AI 遊戲開發模式。"
tags:
  - "clippings"
status: processed
---
Claude Code Game Studios 是一套專門為 Claude Code 打造的 AI 遊戲開發工作室框架，核心概念是把原本單一的 AI Coding Agent，轉變成擁有完整組織架構的虛擬遊戲開發團隊。它並不是一個遊戲引擎，也不是單純的 Prompt 集合，而是一套模擬真實遊戲工作室運作模式的 Agent 系統，透過多個專業角色分工，協助開發者從遊戲構想到正式發布，建立更有秩序的 AI 輔助開發流程。

在一般 AI 輔助開發過程中，開發者通常直接與單一 Agent 互動，因此很容易出現架構不一致、需求規劃不足、技術債累積或缺乏品質檢查等問題。Claude Code Game Studios 嘗試解決的正是這個痛點。它將遊戲製作流程拆解成導演、部門主管與專業人員等不同角色，讓 AI 在執行任務時遵循類似真實遊戲公司的協作方式，而非所有事情都由同一個 Agent 處理。

目前專案已發展成相當完整的規模。根據 GitHub 最新版本說明，系統包含 49 個 AI Agents、72 個工作流程技能（Workflow Skills），以及完整的協調機制，用來模擬真實遊戲工作室的階層式管理與開發流程。

從功能定位來看，Claude Code Game Studios 更接近一套「AI 遊戲開發作業系統」，而非單一工具。它涵蓋了遊戲開發的大部分環節，包括：

- 遊戲概念發想與設計規劃
- 遊戲設計文件（GDD）建立
- 系統架構設計
- UX 與 UI 設計規劃
- 程式開發與功能實作
- 資產規格管理
- 品質保證（QA）
- Sprint 規劃與專案管理
- 內容審查與一致性檢查
- 發行與 LiveOps 流程

這些流程被包裝成各種 Slash Commands 與專用 Agent，讓使用者能以接近工作室管理的方式推進專案。

專案最有特色的地方，在於其組織架構設計。官方將 Agent 分成三層架構：

- Directors（導演層）
- Department Leads（部門主管）
- Specialists（專業執行人員）

例如遊戲設計、技術架構、美術、敘事、音效、測試等領域都有專屬角色負責。當需要完成某項任務時，系統會根據工作內容將任務路由到最適合的 Agent，而不是讓單一模型同時處理所有事情。

除了 Agent 分工之外，Claude Code Game Studios 也建立了大量標準化工作流程。像是：

- `/start`
- `/create-epics`
- `/create-stories`
- `/quick-design`
- `/ux-design`
- `/create-architecture`
- `/architecture-review`
- `/content-audit`
- `/story-readiness`
- `/story-done`

這些指令實際上對應的是完整的遊戲製作流程，而非單純的 Prompt Shortcut。

從實際使用情境來看，Claude Code Game Studios 特別適合獨立遊戲開發者（Indie Developers）。

傳統獨立開發者往往需要同時兼任：

- 製作人
- 遊戲設計師
- 程式設計師
- QA 測試人員
- 專案經理

而 Claude Code Game Studios 的設計理念，就是讓 AI 承擔這些不同職能，讓開發者更像是在管理一個團隊，而不是直接撰寫每一份文件與每一行程式碼。

另一項值得注意的特色是其「協作優先（Collaboration First）」設計。

根據專案文件，系統並不鼓勵 Agent 自主修改專案，而是要求所有流程遵循：

**Question → Options → Decision → Draft → Approval**

也就是先提出問題、提供選項、等待決策、展示草稿，再經過使用者批准後才進行實際修改。這樣的設計可避免 AI 在大型專案中產生難以追蹤的變更。

在遊戲引擎支援方面，Claude Code Game Studios 目前主要支援：

- Godot Engine
- Unity
- Unreal Engine

並且針對不同引擎配置專門的 Agent 與工作流程，因此不需要所有專案都使用相同架構。

從市場定位來看，Claude Code Game Studios 屬於近年興起的「Agentic Software Development」趨勢代表作之一。它與一般 Coding Agent 的最大差異在於，重點不只是提升程式產出速度，而是嘗試把整個遊戲開發流程制度化。當 AI 開始參與設計、規劃、開發、測試與發行時，流程管理的重要性往往比單純程式生成更高。

不過也必須理解，Claude Code Game Studios 並不會自動產生一款成功的遊戲。它本質上仍是一套工作流框架，需要使用者提供創意方向、設計判斷與產品決策。此外，由於系統包含大量 Agent、指令與規範，新手可能需要花一些時間理解其架構，才能發揮最大效益。

<iframe width="825" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=3666733072&amp;adf=828129217&amp;pi=t.aa~a.2909957178~i.51~rp.4&amp;abgtt=13&amp;w=825&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1781503177&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=825x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticle%2Fsn%2F3942&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=825&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuNTQiLG51bGwsMCxudWxsLCI2NCIsW1siR29vZ2xlIENocm9tZSIsIjE0OS4wLjc4MjcuNTQiXSxbIkNocm9taXVtIiwiMTQ5LjAuNzgyNy41NCJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1781493484314&amp;bpp=1&amp;bdt=1226&amp;idt=1&amp;shv=r20260611&amp;mjsv=m202606090101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1781502949%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1781502949%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1781502949%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=825x280%2C0x0%2C255x600%2C255x600&amp;nras=2&amp;correlator=8150603758264&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=285&amp;ady=3374&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099009%2C95390668&amp;oid=2&amp;psts=AOrYGsnd23vrPUHYa1bCLKr0GPHwHKhTXynh1HDl12YxeqFia9b19A2wfePLinqJN1rOemO4zxeLhqqq7AfUbAPPEfj75Y3BlTXv_fbw&amp;pvsid=4519441312323837&amp;tmod=310799405&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Fweb%2Fpage%2F1&amp;fc=384&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=247x742_l&amp;bz=0&amp;pgls=CAs.&amp;ifi=16&amp;uci=a!g&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

整體而言，Claude Code Game Studios 是目前 AI 遊戲開發領域相當具代表性的開源專案之一。它不只是讓 Claude Code 幫忙寫程式，而是試圖將 AI 轉變成一整個虛擬遊戲工作室。對於獨立遊戲開發者、小型團隊，以及希望建立標準化 AI 開發流程的人來說，這套框架提供了一種全新的協作模式，也展現了未來 Agent 團隊化發展的可能性。