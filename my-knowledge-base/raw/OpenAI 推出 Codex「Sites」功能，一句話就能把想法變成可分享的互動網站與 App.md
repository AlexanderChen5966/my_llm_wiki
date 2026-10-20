---
title: "OpenAI 推出 Codex「Sites」功能，一句話就能把想法變成可分享的互動網站與 App"
source: "https://www.koc.com.tw/archives/644871"
author:
  - "[[達小編]]"
published: 2026-06-04
created: 2026-06-10
description: "這次更新的重點不只是「AI 幫你做網頁」這麼簡單。OpenAI 同步把 Codex 的角色專用外掛、Sites、annotations 註解修改能力放在同一波更新中，代表 Codex 正在從程式開發助理，變成一個可以協助資料分析、行銷、銷售、產品設計、投資研究與內部工具製作的工作代理平台。"
tags:
  - "clippings"
status: processed
---
OpenAI 正在把 Codex 從「工程師寫程式工具」推向更廣的工作平台，官方最新宣布 Codex 新增 Sites 功能，可以把使用者的工作內容、想法、分析或專案計畫，轉成可互動的網站、Web App、儀表板、專案看板，並透過網址分享給團隊使用。這項功能目前會先在 ChatGPT Business 與 Enterprise 方案推出，之後再逐步擴大開放範圍。  

## OpenAI 推出 Codex「Sites」功能

這次更新的重點不只是「AI 幫你做網頁」這麼簡單。OpenAI 同步把 Codex 的角色專用外掛、Sites、annotations 註解修改能力放在同一波更新中，代表 Codex 正在從程式開發助理，變成一個可以協助資料分析、行銷、銷售、產品設計、投資研究與內部工具製作的工作代理平台。

![](https://x.com/i/status/2061845949170045346)

## Codex Sites 是什麼？可以建立、儲存、部署與檢查網站

根據 OpenAI [開發者文件](https://developers.openai.com/codex/sites) ，Sites 是 Codex 的一個外掛功能，能讓 Codex 建立、儲存、部署與檢查由 OpenAI 代管的網站、Web App 和遊戲。使用者可以從一段提示開始，也可以要求 Codex 把相容的既有專案整理成可部署版本，不必另外自己架設部署流程。

這代表 Sites 比一般「AI 產生靜態網頁」更接近真正的工作交付工具。舉例來說，使用者可以要求 Codex 建立一個專案需求儀表板，讓團隊成員送出需求、查看負責人、更新狀態並篩選列表；也可以要求 Codex 把財務模型變成情境規劃工具，讓主管直接比較不同假設，而不是在試算表裡切換分頁。OpenAI 官方文章也提到，Sites 可用於 dashboard、planner、review workspace、project board、gallery 與輕量工具等情境。

OpenAI 在官方 X 貼文中形容，透過 Sites，Codex 可以把工作、想法與計畫轉成一個互動式網站或 App，讓團隊透過 URL 探索、使用與分享。這也是這次更新最容易理解的地方，以前 Codex 的成果多半停留在程式碼、修正檔或 pull request，現在則可以直接產出一個讓非工程成員也能打開使用的介面。

### 不是每個人馬上能用，Business 先開，Enterprise 要管理員啟用

Sites 目前仍屬 preview 階段。OpenAI 文件 [寫明](https://developers.openai.com/codex/sites) ，現階段提供給 ChatGPT Business 與 Enterprise 工作區使用，更多方案會在之後推出。ChatGPT Business 工作區預設可用 Sites；ChatGPT Enterprise 則需要管理員透過 RBAC 權限控制開啟，成員才能使用。

這個限制並不意外。Sites 產出的不是單純草稿，而是正式部署的網站或 App。OpenAI 文件也提醒，每一個 Sites deployment URL 都是 production deployment，如果使用者只是想先審查建置結果，應該要求 Codex 儲存一個版本，而不是直接部署。換句話說，企業端使用 Sites 時，不能只把它當成玩具。只要牽涉內部資料、團隊協作、上傳檔案、權限控管或工作區登入，管理員就需要決定誰能部署、誰能存取、哪些資料能保存，以及哪些密鑰不能被寫進原始碼。

## Sites 可支援資料保存、檔案上傳與工作區權限

Sites 並非只能做一次性的展示頁。OpenAI 文件列出多種支援形態，如果網站需要保存紀錄、使用者進度或遊戲分數，可以要求使用 D1 這類關聯式資料庫；如果需要圖片、文件、音訊、影片或其他上傳檔案，可以使用 R2 物件儲存；如果是內部網站，也能要求使用目前工作區使用者身分。

在存取控制方面，Sites 可以設定為只有擁有者與 workspace admins 能看，也可以開放給整個 workspace，或指定特定使用者與群組。OpenAI 建議，新網站在完成審查前，應該先限制給擁有者與管理員，確認內容、資料處理方式與預期受眾後，再擴大存取範圍。

這部分是 Sites 和一般 no-code 網站生成器最大的差異之一。Sites 的重點不是只產生一個漂亮頁面，而是讓 Codex 在工作區、資料、存取權限、版本與部署之間建立可持續修改的流程。OpenAI 文件也把 Sites publishing 分成兩個階段：先儲存可審查版本，再部署正式版本。

### Codex 正在滲透到非開發人員族群

OpenAI 這次公布的數字也透露 Codex 的定位正在改變。官方表示，目前每週已有超過 500 萬人使用 Codex。Codex 原本從軟體開發工具起步，但現在非開發者，包含分析師、行銷、營運、設計師、研究員、投資人與銀行業使用者，約占整體 Codex 使用者 20%，成長速度則是開發者的 3 倍以上。

OpenAI 也提到，內部非技術團隊已經使用 Codex 建立內部 App、準備高階主管簡報、建立儀表板，或把創意簡報轉成符合品牌與設計限制的成果。Zapier 團隊則用 Codex 從 Slack、Google Docs、Coda 等工具擷取知識，再轉成事後檢討、事件回應計畫與功能票；NVIDIA 研究人員也使用 Codex 加快實驗流程。

這些案例點出一個很現實的方向：企業裡很多工作不是「正式軟體專案」，但也不是單純文件就能解決。像是客戶會議頁、活動進度板、產品上線 hub、素材審查區、內部資料看板，這些需求通常太小，不一定排得進工程團隊時程，但手動整理又很耗時間。Sites 瞄準的正是這個灰色地帶。

### 角色專用外掛同步登場，Codex 開始更像工作流程平台

除了 Sites，OpenAI 也同步推出角色專用外掛。官方表示，這些外掛會把相關 App、技能、指令與工作流程打包在一起，首波包含 62 個常用 App 與 110 個 skills。方向包含資料分析、創意製作、銷售、投資銀行、產品設計與公開股票投資等工作情境。

OpenAI 的 Codex Plugins 文件也說明，外掛可以把 skills、App integrations 與 MCP servers 包成可重複使用的 Codex 工作流程。例如 Gmail 外掛可讓 Codex 讀取與管理 Gmail，Google Drive 外掛可操作 Drive、Docs、Sheets、Slides，Sites 外掛則可建立與部署代管網站、Web App 和遊戲。

OpenAI 也把部分角色專用外掛範本放在 [GitHub](https://github.com/openai/role-specific-plugins) 上，包含 Sales、Data Analytics、Product Design 與 Financial Markets 等方向。該 repo 說明這些範本會把領域 skills、connector bindings 與 starter assets 包在一起，讓團隊依照自己的日常工作流程調整 Codex。

### Annotations 讓修改更精準，不必整份重做

這次更新另一個關鍵是 annotations。OpenAI 表示，開發者已經會使用 annotations 修改 Codex 建立的程式碼、Markdown 與網站，現在這種方式也延伸到文件、試算表與簡報。使用者可以指向一個網站導覽列，要求 Codex 更新字體；也可以標記投資論點中的某個 claim，要求 Codex 說明來源；或標註投影片上的圖表，要求它改善標籤。

這對實際工作流程很重要。AI 產出第一版通常不難，困難的是後續修改。如果每次都要整份重生，很容易把原本正確的部分也改壞。Annotations 的價值在於讓使用者直接指出需要調整的區塊，Codex 再針對該範圍處理，而不是整個專案從頭來過。

## 結語：Codex Sites 讓「做工具」變成新的工作輸出格式

從這次更新來看，OpenAI 對 Codex 的定位已經不只是 coding agent，而是要讓它成為可以把工作流程變成工具的代理平台。Sites 讓使用者把想法、分析、專案計畫與內部流程直接轉成可互動、可分享、可持續修改的網站或 App；角色外掛則讓 Codex 更容易接上不同職位的資料來源與常用工具；annotations 則補上後續修改與審查流程。

短期來看，Sites 最適合的不是取代大型正式產品開發，而是快速建立內部工具、專案頁、活動看板、客戶 review 頁、資料摘要儀表板與輕量工作 App。它真正降低的不是「寫網頁」門檻，而是把臨時工作需求軟體化的門檻。對企業來說，接下來要觀察的是 Sites 的權限、資料保存、部署治理與外掛生態是否足夠成熟；對一般使用者來說，則要等 OpenAI 把這項功能從 Business 與 Enterprise 方案逐步下放到更多方案。