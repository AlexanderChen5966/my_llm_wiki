---
title: "AI Marketing Suite：讓 Claude Code 幫你做 SEO、競品分析、Email 與社群行銷 | 網路資源"
source: "https://tools.wingzero.tw/articles/4032"
author:
  - "[[赤紅翼零]]"
published: 2026-08-22
created: 2026-08-25
description: "AI Marketing Suite for Claude Code 是開源 AI 行銷技能系統，透過 /market 指令整合 SEO、網站健檢、競品分析、文案、Email、社群與廣告等工作。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/zubair-trabzada/ai-marketing-claude](https://github.com/zubair-trabzada/ai-marketing-claude)

AI Marketing Suite for Claude Code 是一套安裝在 Claude Code 裡使用的開源 AI 行銷技能系統，將網站行銷健檢、文案優化、SEO、競品研究、廣告文案、Email 行銷、社群內容規劃與客戶報告等工作，整理成一系列 `/market` 指令。它不是另一個獨立的行銷 SaaS，而是把 Claude Code 從 Coding Agent 延伸成可以直接在終端機工作的「AI 行銷助手」。

這個專案的定位也相當明確，作者主要鎖定創業者、個人工作者與想利用 AI 提供行銷服務的 Agency Builder。使用者不需要自己設計一長串 Prompt，只要輸入像 `/market audit <url>` 、 `/market competitors <url>` 或 `/market social <topic>` 等指令，Claude Code 就會載入對應 Skill、啟動專門的 Subagent 與 Python 工具，再把結果整理成可以繼續修改或交付客戶的文件。

它和一般「行銷 Prompt 大全」最大的差別，就在於這些能力已經被包裝成可重複使用的 Claude Code Skills。Repository 的架構包含一個主要 `/market` Orchestrator，以及 14 個子 Skill；安裝程式本身則將整套系統描述為 15 Skills、5 Agents 與 4 Scripts。換句話說，它不是要求使用者每次重新告訴 Claude「你現在是一名 SEO 專家」，而是預先把不同任務的工作方式、分析框架與輸出格式整理好。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="832" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=897698622&amp;adf=4116832479&amp;abgtt=6&amp;w=832&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787624123&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=832x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4032&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=832&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787624123311&amp;bpp=1&amp;bdt=207&amp;idt=0&amp;shv=r20260824&amp;mjsv=m202608180101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1787624075%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1787624075%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1787624075%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C784x280&amp;nras=2&amp;correlator=4905527357229&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=4&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=225&amp;ady=2388&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399757%2C122880398%2C122880333&amp;oid=2&amp;pvsid=1562369608460365&amp;tmod=862571202&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=1920%2C41%2C1920%2C41%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C216x714_r&amp;bz=1.15&amp;ifi=12&amp;uci=a!c&amp;btvi=2&amp;fsb=1&amp;dtd=4" title="Advertisement" aria-label="Advertisement"></iframe>

目前提供的 `/market` 指令涵蓋相當完整的數位行銷工作：

- `/market audit <url>` ：完整網站行銷健檢
- `/market quick <url>` ：快速行銷概況分析
- `/market copy <url>` ：分析並產生優化後的網站文案
- `/market emails <topic>` ：建立完整 Email Sequence
- `/market social <topic>` ：建立 30 天社群內容行事曆
- `/market ads <url>` ：產生不同平台使用的廣告創意與文案
- `/market funnel <url>` ：分析與改善 Sales Funnel
- `/market competitors <url>` ：競爭對手與市場定位分析
- `/market landing <url>` ：Landing Page CRO 分析
- `/market launch <product>` ：建立產品上市 Playbook
- `/market proposal <client>` ：產生客戶提案
- `/market report <url>` ：輸出完整 Markdown 行銷報告
- `/market report-pdf <url>` ：產生客戶可閱讀的 PDF 行銷報告
- `/market seo <url>` ：執行 SEO Content Audit
- `/market brand <url>` ：分析 Brand Voice 並整理品牌指南。

其中 `/market audit` 最能看出整套系統的設計思路。執行網站 Audit 時，Claude Code 不是只輸出一篇泛泛而談的網站評論，而會讓多個 Agent 分工分析不同面向，再整理成帶有分數與優先順序的報告。官方範例會分別評估 Content & Messaging、Conversion Optimization、SEO & Discoverability、Competitive Positioning、Brand & Trust，以及 Growth & Strategy。

這套評分也有固定權重。Content & Messaging 佔 25%，主要檢查文案品質、Value Proposition、Headline 與 CTA；Conversion Optimization 與 SEO & Discoverability 各佔 20%；Competitive Positioning 佔 15%；Brand & Trust、Growth & Strategy 則各佔 10%。最後再以加權平均計算 0～100 的 Overall Marketing Score。

這種設計的好處，是不同網站可以用相對一致的框架分析。假設行銷顧問一次研究十個潛在客戶網站，不必每次臨時決定要檢查哪些項目，而能用相同架構比較內容、轉換、SEO、競爭定位與成長策略。不過這些分數仍然是這套 Skill 所定義的分析框架，並不是 Google、Meta 或其他廣告／搜尋平台提供的官方評分，因此比較適合拿來找問題與建立優先順序，而不是視為絕對的行銷績效指標。

AI Marketing Suite 的 Agent 架構也是它與單一 Prompt 比較不同的地方。Repository 目前包含 5 個專門 Subagent，分別負責 Content & Messaging、CRO & Funnel Optimization、Competitive Positioning、Technical SEO & Tracking，以及 Brand、Pricing 與 Growth Strategy。完整 Audit 執行時，這些不同角色可以平行處理各自負責的分析。

除了 Claude 本身的推理，專案還搭配 4 支 Python Utility Scripts。 `analyze_page.py` 負責網頁行銷分析， `competitor_scanner.py` 處理競爭網站掃描， `social_calendar.py` 用於社群內容行事曆，而 `generate_pdf_report.py` 則負責 PDF 報告輸出。這表示整套工具並非所有工作都只靠 LLM 臨場生成，也將部分可以程式化的流程交給 Script 處理。

完整執行方式大致可以理解成：

`/market 指令 → Claude 讀取 Skill → Subagents 分工 → Python Scripts 執行分析 → 彙整結果 → Markdown / PDF`

官方描述的 Audit 流程也是如此：輸入指令後，Claude 先讀取 Skill 文件，再啟動 5 個 Subagent 分析不同維度，接著執行網頁分析與競爭者掃描 Script，最後把結果整理成有分數、優先順序與可執行建議的報告。

除了分析網站，它也提供不少直接「產出行銷素材」的功能。例如 Email 模板包含 Welcome Sequence、Lead Nurture 與 Product Launch，其中官方 Repository 分別準備了 5 封 Welcome Email、6 封 Lead Nurture Email 與 8 封 Product Launch Email 的 Template。其他模板還包括 Client Proposal、30-day Content Calendar 與 Launch Checklist。

這使它比較適合實際提供行銷服務，而不只是做一次網站分析。Agency 可以先用 `/market audit` 分析潛在客戶網站，從中找出 Conversion、SEO 或 Messaging 問題；接著用 `/market proposal` 把問題整理成提案，再利用 `/market copy` 、 `/market emails` 等指令執行改善。最後還能使用 `/market report-pdf` 輸出正式報告交付客戶。

對 Solopreneur 而言，使用方式則可以簡化許多。例如推出新產品前，用 `/market copy` 修改 Landing Page，再利用 `/market emails` 準備 Launch Email，接著用 `/market social` 建立 30 天內容計畫。內容創作者則可以利用 `/market competitors` 研究競爭者，再用 `/market funnel` 找出自己的轉換流程問題。這些也都是專案 README 明確列出的使用情境。

安裝方式相對簡單，官方提供 Bash 一鍵安裝 Script，也可以 Clone Repository 後執行 `./install.sh` 。如果需要 PDF 報告，則另外安裝 ReportLab。安裝後相關 Skill 與 Agent 會進入 Claude Code 的 Skills／Agents 環境，因此日後可以直接透過 `/market` 指令使用。

授權方面，AI Marketing Suite 採用 MIT License，因此程式可以自由使用、修改、合併、發布與再散布，只需要遵守授權條款。這對想修改 Skill、建立自己 Agency Workflow，甚至在既有架構上加入公司內部行銷方法的使用者比較友善。

如果與一般 Claude Code Skill 比較，這個專案的完整度主要體現在「把一個職能拆成整套工具」。它不是只有一個 SEO Prompt 或 Copywriting Skill，而是將 Audit、SEO、CRO、Competitor Research、Email、Social、Ads、Brand、Launch、Proposal 與 Reporting 串在同一個 `/market` 命名空間之下。對想把 Claude Code 從程式開發工具延伸到商業工作的人，這種職能型 Skill Suite 會比零散 Prompt 更容易重複使用。

整體而言，AI Marketing Suite for Claude Code 比較適合把它理解成「Claude Code 裡的一套 AI 行銷部門」。它利用 Skills 定義工作方法、Subagents 分工分析、Python Scripts 處理部分自動化，再透過 Template 與 Markdown／PDF 整理交付成果。對自由工作者、Solopreneur 或小型 Marketing Agency 而言，最大的價值不是取代專業行銷數據平台，而是把原本散落在不同 Prompt、文件與人工流程裡的行銷工作，整理成可以從終端機反覆執行的一套工作流。

<iframe frameborder="0" src="https://d0780161df4fe0546db901051bbaeea7.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="300" height="250" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>