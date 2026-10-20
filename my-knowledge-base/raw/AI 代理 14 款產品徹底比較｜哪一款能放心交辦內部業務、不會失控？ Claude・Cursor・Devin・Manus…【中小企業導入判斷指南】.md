---
title: "AI 代理 14 款產品徹底比較｜哪一款能放心交辦內部業務、不會失控？ Claude・Cursor・Devin・Manus…【中小企業導入判斷指南】"
source: "https://codelove.tw/@tony/post/a66Ona"
author:
published:
created: 2026-09-07
description: "> **「讓 AI 去碰公司內部系統，不會出事嗎？」**> **「只有工程師能用？ 一般業務也能用嗎？」**> **「操作失誤能復原嗎？ 費用會不會無上限？」**[](#1-%E6%A5%AD%E5%8B%99%E8%A6%96%E8%A7%92%E7%9A%84%E7%B5%90%E8%A..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月07日](https://codelove.tw/@tony/post/a66Ona "2026-09-07 08:07:20")

## AI 代理 14 款產品徹底比較｜哪一款能放心交辦內部業務、不會失控？ Claude・Cursor・Devin・Manus…【中小企業導入判斷指南】

> **「讓 AI 去碰公司內部系統，不會出事嗎？」**  
> **「只有工程師能用？ 一般業務也能用嗎？」**  
> **「操作失誤能復原嗎？ 費用會不會無上限？」**

## 1\. 業務視角的結論：AI 代理 14 個產品的綜合比較矩陣

[![將 14 個 AI 代理產品以 8 個指標排列的綜合比較矩陣。縱向為產品、供應商與總部國旗，橫向為導入容易度到費用明確度等指標，儲存格以圓形與三角形符號表示](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-summary-matrix.dd711817.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bdb7dd59e8b3e261bf1c399069989adf)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-summary-matrix.dd711817.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bdb7dd59e8b3e261bf1c399069989adf)

「如果導入 AI 代理，就能把每天的輸入與調查工作整包交出去嗎？」——對此抱持興趣，卻又害怕現場暴走、資訊外洩、以及意料之外的成本，而裹足不前的中小企業經營層與主管們。

本文針對主要的 **AI 代理 14 個產品** ，徹底調查其官方文件、使用條款、價格體系、資安方針等 **第一手資料（2026 年 8 月～9 月調查時點）** ，並以「8 個獨創指標」比較是否能在業務上安全且有效地委任自律作業。首先請先看結論的「14 個產品 業務視角綜合比較矩陣」。

> **【閱讀評價矩陣時的重要前提】**
> 
> - 儲存格中的符號（◎・◯・△・ー），是根據各公司公開的官方條款、規格與價格體系，由編輯部以中小企業業務使用的觀點所做的綜合整理與評價之客觀判斷。
> - 各公司的免責條款、損害賠償責任，以及著作權補償是否存在，未作為指標化項目，而是於第 3 章以契約事實另行獨立說明。
> - 各評價的詳細客觀依據、條款內容與來源 URL，皆完整記載於後續的第 2 章與第 6 章。

---

### 1.1 背景與本文目的

※ 這裡所稱的 **AI 代理** ，並非只會透過對話回覆文字或點子的「ChatAI」，而是指當被給予目標（goal）後，能自主推理， **「呼叫外部工具、改寫檔案、執行命令與瀏覽器，實際動手做事」** 的軟體群。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788743512&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa66Ona&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788743512440&amp;bpp=1&amp;bdt=1086&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788743511%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788743511%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788743511%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250%2C952x280&amp;nras=3&amp;correlator=4203459691429&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2797&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400775%2C95401045%2C122880837%2C122880828%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsnDVDddRBW1FNNLpS2KaHHHdg0fJgAWflEhS2vtr1aU6fH2CJ2ngW_EWWVrDhkQabQXr9T9FFNgUB1fcsOzBUAUiAwnqWXeF8aP9_wpQaQc_04&amp;pvsid=8597108220344386&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=42" title="Advertisement" aria-label="Advertisement"></iframe>

本文是業務自動化 66 個產品分成 6 種角色的系列總論「 [【圖解】AI・ChatGPT・RPA・iPaaS・n8n…業務自動化的名詞太多了！若把 66 個產品比喻成 6 種角色，就變成一張工廠插圖](https://qiita.com/songchong/items/dc75c6f3789348408f9b) 」之中，針對② AI 代理（坐在操作台前的人形機器人＝會判斷並動手的工具）的 14 個產品，從中小企業導入決策的角度進一步深入剖析。系列文章列表請參閱 [作者 Qiita 頁面（@songchong）](https://qiita.com/songchong) 。

從各家宣傳來看，充滿了「完全自律」、「代替你完成整個軟體開發」、「自動操作瀏覽器進行研究」等驚人成果。然而，當經營層或資訊部門真的要導入時，會遇到比 ChatAI 更嚴重得多的門檻。

因為 AI 代理不只是「回覆語言」，而是會 **直接修改公司內部的檔案與系統** ，一旦判斷失誤，就可能立刻導致資料毀損、誤發送、或業務中斷。正如總論所指出的，工作流程或 RPA 這類產線側失敗，通常會以「異常停止」的方式在設計範圍內被攔下；但「操作台機器人（AI 代理）的失敗，有時會超出設計者的認知之外，持續以錯誤方式運作」——這是其本質性的風險。

因此，本文為了回答開頭的 3 個問題，定義了「 **8 個獨創評價指標** 」，對 14 個產品進行橫向比較。

本文的目的不是對特定工具做排名推薦。 **而是提供客觀判斷材料，幫助辨別「依自家業務內容（開發還是一般實務）、需要守護的資料主權、可接受的自律程度（可委任的權限範圍）、以及預算體系來看，哪個產品才是最安全且最有效的選擇」** 。

---

## 2\. 導入時應檢討的「8 個獨創指標」與評價理由

如果只靠「AI 模型的 IQ」來決定 AI 代理，導入到現場後往往會直接引發「只有工程師能用的命令列（CLI）」、「擅自覆寫檔案導致無法還原」、「月底收到數十萬元帳單」等問題。

這裡將說明經營層與主管必須確認的 **8 個評價指標** ，以及為何這些視角不可或缺、14 個產品的實際差異為何。

---

### 2.1 導入容易度（UI 型態・使用環境・前提條件）

[![將 14 個 AI 代理產品依主要入口分成 4 類的圖。瀏覽器／雲端完成型（Agentforce・Devin・Manus）、桌面應用程式型（Cursor・Kiro・Antigravity・Claude Cowork）、編輯器／IDE 擴充型（GitHub Copilot・Cline）、終端機／CLI／自家常駐型（Claude Code・Codex・CrewAI・Hermes Agent・OpenClaw）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-access-modes.68637ebc.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3882cd1bb3062b5bc276db05f93f844a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-access-modes.68637ebc.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3882cd1bb3062b5bc276db05f93f844a)

第一道關卡，就是「公司裡的誰、在什麼環境下能立刻開始使用」這個導入門檻的高低。

#### ① 依提供形態分類的 4 種類型

AI 代理的運作場所，並不全都是像 ChatAI 一樣「打開瀏覽器就能用」。

> 很多產品有多種使用方式。這裡將 **供應商在導入流程中排在最前面的形式** 視為「主要入口」並只計算 1 種（內部分類見 EVIDENCE §2.3；其他可用形式也一併列在同一表中）。

1. **瀏覽器／雲端完成型（3 個產品）**:  
	[Agentforce](https://renkeimap.jp/tool/agentforce) 、 [Devin](https://renkeimap.jp/tool/devin) 、 [Manus](https://renkeimap.jp/tool/manus) 。只要透過瀏覽器或專用應用程式登入即可使用，不受使用者電腦規格限制。
2. **編輯器／IDE 擴充型（2 個產品）**:  
	[GitHub Copilot](https://renkeimap.jp/tool/github-copilot) 、 [Cline](https://renkeimap.jp/tool/cline) 。以擴充功能的形式整合到 Visual Studio Code 或 JetBrains 等 **既有正在使用的編輯器** 中。
3. **桌面應用程式型（4 個產品）**:  
	[Cursor](https://renkeimap.jp/tool/cursor) 、 [Kiro](https://renkeimap.jp/tool/kiro) 、 [Antigravity](https://renkeimap.jp/tool/antigravity) 、 [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) 。需 **安裝應用程式本體** 並啟動，直接讀寫本機檔案。Cursor・Kiro・Antigravity 是「具備編輯器功能的獨立應用程式」，安裝方式與上面的擴充型不同。
4. **終端機・CLI／自家常駐型（5 個產品）**:  
	[Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Codex](https://renkeimap.jp/tool/codex) 、 [CrewAI](https://renkeimap.jp/tool/crewai) 、 [Hermes Agent](https://renkeimap.jp/tool/hermes-agent) 、 [OpenClaw](https://renkeimap.jp/tool/openclaw) 。需要命令列操作，或建置 Node.js / Python / Docker 環境，甚至需要常駐為 daemon，對非工程師而言門檻較高。

#### ② 前提條件的陷阱

調查使用型態後，可以明確分出「 **在使用前就必須具備特定契約或高階環境的產品** 」。

- **[Agentforce](https://renkeimap.jp/tool/agentforce)** ：以 Salesforce 合約（Enterprise Edition 以上等）為前提，不能單獨購買後拿去用於其他系統。
- **[CrewAI](https://renkeimap.jp/tool/crewai)** ：官方明確要求自家運行時必須建置「Python 3.10～3.13」環境。
- **[OpenClaw](https://renkeimap.jp/tool/openclaw)** ：以從命令列安裝與運作為前提（ `npm install -g openclaw` ），且需要自行準備模型 API 金鑰。
- **[Hermes Agent](https://renkeimap.jp/tool/hermes-agent)** ：官方安裝方式是 **一行安裝指令** （macOS / Linux / WSL2 用 `curl` ，Windows 用 PowerShell）或桌面應用程式發佈。安裝程式會一併安裝 uv、Python 3.11、Node.js、ripgrep、ffmpeg。Windows 支援不需 WSL 的原生運作（官方表述：「Native Windows runs Hermes without WSL — CLI, gateway, TUI, and tools all work natively.」）。不過 **使用時仍需另外準備模型提供商的 API 金鑰，或自行運行的模型伺服器** 。

> 判定基準：◎＝可直接透過瀏覽器或標準 IDE 擴充使用 ／ ◯＝需要特定 SaaS 合約或環境前提 ／ △＝需要 CLI/Python/Node/本機常駐等高階環境建置 ／ —＝開放範圍內未記載

---

### 2.2 業務適合（擅長領域與自律／Remote Control 的實態）

[![顯示自律執行與遠端操作 4 種型態的圖。雲端虛擬 PC 自律與人工介入、雲端工作空間遠端指示、自家常駐 daemon 遠端控制、手邊桌面直接執行](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-remote-control.e4b18540.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fc537115fd9121df661219b2c88fc98b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-remote-control.e4b18540.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fc537115fd9121df661219b2c88fc98b)

第二個指標是「能把公司內哪種業務交給它」的業務適合性，以及自律執行與 **遠端操作** （Remote Control）的真實程度。

#### ① 依擅長領域分類的 3 種類型

AI 代理並非萬能魔法。從各產品官方資料可看出的主戰場分成 3 類。

1. **軟體開發・程式撰寫特化（8 個產品）**:  
	[Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Cursor](https://renkeimap.jp/tool/cursor) 、 [Devin](https://renkeimap.jp/tool/devin) 、 [Codex](https://renkeimap.jp/tool/codex) 、 [Cline](https://renkeimap.jp/tool/cline) 、 [GitHub Copilot](https://renkeimap.jp/tool/github-copilot) 、 [Kiro](https://renkeimap.jp/tool/kiro) 、 [Antigravity](https://renkeimap.jp/tool/antigravity) 。主目的是理解原始碼、修正 bug、建立 PR（Pull Request）、自動執行測試。
2. **一般業務・研究調查・文件製作（3 個產品）**:  
	[Manus](https://renkeimap.jp/tool/manus) （市場研究、投影片製作、Web 製作）、 [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) （整理本機文件與產出成果物）、 [Agentforce](https://renkeimap.jp/tool/agentforce) （CRM 內的客戶應對、人事流程、商機更新）。
3. **多代理協作・自律執行基礎平台（3 個產品）**:  
	[CrewAI](https://renkeimap.jp/tool/crewai) （分工為研究、撰寫、檢查等角色）、 [OpenClaw](https://renkeimap.jp/tool/openclaw) （從訊息應用程式自律呼叫）、 [Hermes Agent](https://renkeimap.jp/tool/hermes-agent) （自己從工作產生步驟並儲存，再拆分給下游代理並平行執行）。

> **今天這 14 個產品的主戰場，仍然是「開發」。** 14 個產品中有 8 個是處理原始碼的工具，而以經理、總務、業務等一般業務為主要訴求的只有 3 個產品（Manus・Claude Cowork・Agentforce）。如果要在全非技術人員的部門導入，請先閱讀這 3 個產品，以及後續章節的「委任統制」「隔離復原」。

#### ② 自律執行與遠端操作（Remote Control）的到達度

在現場最受矚目的「AI 是否能遠端操作 PC 與瀏覽器」這一點上，從第一手資料確認到的功能差異相當鮮明。

產品遠端操作・自律執行形式人類介入（Take-over） **[Manus](https://renkeimap.jp/tool/manus)** 在雲端虛擬 PC 上自律操作（瀏覽器・終端機）。支援排程執行可即時查看虛擬 PC 畫面，並可 **中途由人類接手操作（Take-over）** 。本機操作代行有瀏覽器擴充功能 **[Devin](https://renkeimap.jp/tool/devin)** 在雲端隔離開發環境（shell・editor・browser）中自律實作可透過 Slack / Teams 聊天遠端指示 **[Agentforce](https://renkeimap.jp/tool/agentforce)** 從 Salesforce 背景或渠道（Slack/Web/SMS）遠端自律操作 CRM手頭棘手的案件會自動移交給人類操作員 **[OpenClaw](https://renkeimap.jp/tool/openclaw)** 常駐於終端設備，從平常使用的聊天工具（WhatsApp、Telegram、Discord、Slack 等）接收指示透過訊息應用程式遠端觸發家中／公司端設備的操作如此可見，存在著明確的架構差異： **「自律操作雲端虛擬機器且人可中途接手畫面的型態（Manus）」** 、 **「從聊天遠端下指令，在雲端內完成的型態（Devin、Agentforce）」** 、 **「從訊息應用程式遠端操控手邊裝置的型態（OpenClaw）」** 。

另外，像 [Cursor](https://renkeimap.jp/tool/cursor) ・ [Claude Code](https://renkeimap.jp/tool/claude-code) ・ [Cline](https://renkeimap.jp/tool/cline) 這種在本機編輯器或終端機中讓人邊看差異邊一步步進行的型態（圖中的④「本機對話・直接陪伴型」），屬於本機高速輔助而非遠端自律，因此未列入上表。

> 判定基準：◎＝支援高階自律執行＋遠端操作（雲端虛擬 PC・瀏覽器操作代行・聊天遠端指示） ／ ◯＝支援程式碼或本機檔案的高階自律實作・操作 ／ △＝僅限特定功能或補強 ／ —＝開放範圍內未記載

---

### 2.3 委任統制（人類介在 HITL・自律模式・承認關卡）

[![顯示防止暴走的委任統制與 4 個安全閥的圖。人類介在、自律模式預設關閉、權限範圍限制、稽核紀錄永久保存](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-governance-hitl.17b2c394.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bfcf9dae30f1279cbc2f86691600d05a)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-governance-hitl.17b2c394.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bfcf9dae30f1279cbc2f86691600d05a)

業務上最危險的情況，就是「AI 擅自判斷並寄信給外部」、「修改了正式資料庫」。

#### ① 人類介在（Human-in-the-loop）與自律模式的預設值

仔細檢視第一手資料後可以發現，優質的代理越多越會把「 **完全自律執行預設關閉** 」作為預設。

- **[Kiro（AWS）](https://renkeimap.jp/tool/kiro)** ：官方文件明確寫著「 **Autonomous mode is off by default** （自動駕駛預設關閉）」，當判斷有疑慮時會停止工作並要求人類確認。
- **[Codex（OpenAI）](https://renkeimap.jp/tool/codex)** ：當執行會跨越工作範圍邊界的危險操作時，預設會要求人類確認並暫停。
- **[Antigravity（Google）](https://renkeimap.jp/tool/antigravity)** ：在執行變更前停下來要求批准，是預設建議設定。
- **[Agentforce](https://renkeimap.jp/tool/agentforce)** ：採取嚴格的護欄設計，例如「120 秒即中止、同時執行僅 1 件」、「無法處理的案件轉交人類」。

#### ② 危險的自律運轉風險

另一方面，如果批准設定放得太鬆，會產生重大風險。

- **[Cline](https://renkeimap.jp/tool/cline)** 的官方文件直言：「 **Without checkpoints, auto-approve feels risky because Cline can make many changes before you notice a problem.**（沒有檢查點時，自動核准很危險，因為在你發現問題前，Cline 可能已經做了很多變更。）」
- **[GitHub Copilot](https://renkeimap.jp/tool/github-copilot)** 也明確表示可能出錯，會在執行危險指令前先要求許可。

> 判定基準：◎＝自律模式預設關閉、跨越工作範圍的操作需事前核准、標準配備稽核紀錄 ／ ◯＝有批准設定或復原功能 ／ △＝承認關卡視設定而定，或有自律暴走風險 ／ —＝開放範圍內未記載

---

### 2.4 安全主權（重新訓練 OFF・瀏覽限制・機密保護・認證資訊管理）

[![顯示資料主權防護牆的圖。共有 4 項：阻擋重新訓練、限制人類瀏覽、隔離認證資訊、將模型推論留在本機](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-security-sovereignty.7dabd9ec.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=016444c852cecb7a7e9409d1e2ed2445)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-security-sovereignty.7dabd9ec.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=016444c852cecb7a7e9409d1e2ed2445)

此指標是檢查你輸入的程式或客戶資料是否會被拿去訓練 AI，以及 API Key 等認證資訊是否被安全管理。

#### ① 重新訓練（Training Use）的條款實態

與 [過去對使用條款的調查](https://qiita.com/songchong/items/3abbc43f0d7c471e6688) 相同，個人用途與法人用途的處理差異很大。

- **明確寫明不會用於訓練**:  
	[Agentforce](https://renkeimap.jp/tool/agentforce) 以 Einstein Trust Layer 的功能列出「零資料保留」（官方表述：「這個 Einstein Trust Layer 透過零資料保留、毒性偵測、安全資料擷取、動態 grounding 等強健的資安功能與護欄來保護客戶資料。」），並在其他頁面明記「零資料保留是嚴格政策，prompt 與生成回應不會被保存，也不會用於底層第三方大型語言模型的訓練」。  
	[Claude Code](https://renkeimap.jp/tool/claude-code) ・ [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) 在法人商用合約下預設不會用於訓練。 [Cursor](https://renkeimap.jp/tool/cursor) 則在啟用「Privacy Mode」後可變成不學習。
- **個人使用或部分方案會成為訓練對象的例子**:  
	[Antigravity](https://renkeimap.jp/tool/antigravity) 在個人免費使用時，預設會用於品質改善（可在設定中變更）。 [CrewAI](https://renkeimap.jp/tool/crewai) 的條款中存在將輸入與輸出都用於模型訓練的條文。 [GitHub Copilot](https://renkeimap.jp/tool/github-copilot) 則是個人免費版與法人契約的處理方式相反。
- **找不到條文明示的例子**:  
	[Manus](https://renkeimap.jp/tool/manus) 在開放範圍內未確認到有說明是否會用於訓練的條文。

#### ② 資格資訊（API Key・認證權杖）的保存位置

要讓 AI 代理操作公司內部系統，就必須給它 API Key 或 OAuth 權杖。

- 像 **[OpenClaw](https://renkeimap.jp/tool/openclaw)** 這樣儲存在本機區域（ `~/.openclaw/credentials` ）的產品，可以降低外洩到雲端的風險。
- 若把公司系統的管理員金鑰交給雲端型代理，為防止一旦外洩時造成嚴重後果，原則上應發行權限（Scope）盡可能縮小的專用金鑰。

> 判定基準：◎＝法人／商用方案明確寫明不重新訓練，且認證資訊安全管理 ／ ◯＝可透過設定關閉學習 ／ △＝條款中即為訓練對象，或未記載 ／ —＝開放範圍內未記載

---

### 2.5 隔離復原（Sandbox・Rollback・失控防止）

[![顯示操作失誤與暴走時的隔離與復原機制的圖。雲端 VM 隔離（Devin・Manus・Kiro）、本機隔離・資料夾限制（Codex・Claude Code）、檢查點復原（Cline・Cursor）、隔離依設定而定且預設關閉（OpenClaw・Antigravity）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-sandbox-recovery.cf5d25f2.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=13557ac1adb5b6c3d345ea5eb8e6cebd)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-sandbox-recovery.cf5d25f2.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=13557ac1adb5b6c3d345ea5eb8e6cebd)

當 AI 代理執行錯誤程式碼或刪除檔案時，是否具備「 **能安全隔離環境** 」與「 **能回復到前一狀態** 」的防護機制。

#### ① Sandbox（隔離執行環境）有無

- **完整的雲端虛擬環境**:  
	[Devin](https://renkeimap.jp/tool/devin) 不是在使用者的裝置上運作，而是在雲端上的獨立工作空間（shell・editor・browser）中執行。 [Manus](https://renkeimap.jp/tool/manus) 也是在雲端隔離虛擬 PC 上運作，而 [Kiro](https://renkeimap.jp/tool/kiro) 則會為每個工作建立隔離環境，並在結束後銷毀。這是對正式端點影響最小、最安全的架構。
- **本機 Sandbox・資料夾限制**:  
	[Codex](https://renkeimap.jp/tool/codex) 預設以隔離方式執行，且權限以 3 階段管理。 [Claude Code](https://renkeimap.jp/tool/claude-code) 只能寫入啟動資料夾底下的範圍，具有存取限制。
- **無隔離・直接執行**:  
	[OpenClaw](https://renkeimap.jp/tool/openclaw) 與 [Antigravity](https://renkeimap.jp/tool/antigravity) 雖然具備隔離功能，但預設關閉；若設定錯誤，命令會直接在本機電腦上執行。

#### ② Rollback（回滾）機制

- **[Cline](https://renkeimap.jp/tool/cline)** 備有「Checkpoints（檢查點）」功能，可將檔案與對話一鍵回滾到過去某個時間點。
- **[Cursor](https://renkeimap.jp/tool/cursor)** 則透過「自動還原點」，只將代理變更過的檔案復原到前一狀態。

> 判定基準：◎＝雲端／OS 層級的隔離 Sandbox＋回滾機制標準配備 ／ ◯＝有啟動資料夾限制或編輯器還原點 ／ △＝需手動隔離或復原，或是本機直接執行 ／ —＝開放範圍內未記載

---

### 2.6 業務持續性（成果物帶出・前付失效・鎖定）

[![顯示工具結束時風險的圖。成果物可帶出（Claude Code・Cline・Cursor・OpenClaw 的結果都能留在本機）、前付點數失效條款（Manus）、雲端獨佔綁定（Agentforce 的資料累積在 CRM 內、Manus 有浮水印）、開源的永續性（OpenClaw・Hermes・Cline）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-continuity-exit.b9a0d831.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ec92eea6e87ca8010e13180a46896cf9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-continuity-exit.b9a0d831.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ec92eea6e87ca8010e13180a46896cf9)

導入之後如果想更換工具時，所關心的出口策略是「 **成果物能否完整帶走** 」與「 **已付款項是否會白白浪費** 」。

#### ① 成果物的形式與鎖定

- **[Claude Code](https://renkeimap.jp/tool/claude-code)** 、 **[Cline](https://renkeimap.jp/tool/cline)** 、 **[Cursor](https://renkeimap.jp/tool/cursor)** 、 **[OpenClaw](https://renkeimap.jp/tool/openclaw)**:  
	成果物全部都會以標準原始碼、Markdown、本機檔案、Git 歷史的形式留在手邊。即使更換工具，成果物也 100% 仍在自己手上，因此鎖定程度極低。
- **[Agentforce](https://renkeimap.jp/tool/agentforce)**:  
	代理更新的商機紀錄與詢問紀錄會累積在 Salesforce CRM 內部。
- **[OpenClaw](https://renkeimap.jp/tool/openclaw)** 、 **[Hermes Agent](https://renkeimap.jp/tool/hermes-agent)** 、 **[Cline](https://renkeimap.jp/tool/cline)** （開源）:  
	軟體本體可作為開源放在自己手上，即使供應商終止服務，手邊的程式碼與設定仍可繼續使用。

#### ② 前付點數的失效條款

要特別注意的是點數前付型的條款。

- **[Manus](https://renkeimap.jp/tool/manus)**:  
	除了成果物會帶有「Made with Manus」浮水印且目前無法移除外，使用條款也明記「為點數前付型，退會時未使用的部分將失效」。若在仍有未用餘額時解約，可能會造成費用浪費。

> 判定基準：◎＝成果物以標準 Git／檔案形式留在手邊，且無失效風險 ／ ◯＝支援標準匯出 ／ △＝帶浮水印、前付點數退會失效條款存在 ／ —＝開放範圍內未記載

---

### 2.7 連攜操作（操作入口 MCP・API 連線・對象 SaaS）

[![顯示操作入口 MCP 與外部連線形式的圖。MCP client 9 個產品，以及本身也能成為 MCP server 的 both 5 個產品（Claude Code・Codex・CrewAI・OpenClaw・Hermes Agent）、與 Salesforce 直連的 Agentforce、以及瀏覽器代行的 Manus](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-mcp-connectivity.5e259b80.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=eb43b526d4ff00e9460e6d5bdc03cd07)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-mcp-connectivity.5e259b80.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=eb43b526d4ff00e9460e6d5bdc03cd07)

這是 AI 代理連接外部業務系統（kintone、freee、Slack、Google 雲端硬碟等）時，作為「操作入口」的擴充性。

#### ① MCP（Model Context Protocol）的客戶端與伺服器

調查共通規格 **MCP** （Model Context Protocol）的支援情況後，可知以下事實：

- **MCP Client（操作的一方）**:  
	14 個產品全部都能作為「客戶端」，呼叫外部 MCP 伺服器來讀寫資料。
- **支援 MCP Server（自身也可被操作）— both**:  
	[Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Codex](https://renkeimap.jp/tool/codex) 、 [CrewAI](https://renkeimap.jp/tool/crewai) 、 [OpenClaw](https://renkeimap.jp/tool/openclaw) 、 [Hermes Agent](https://renkeimap.jp/tool/hermes-agent) 這 5 個產品也能作為 MCP 伺服器運作。比如「從 Claude Code 呼叫 OpenClaw 的功能作為工具」這類代理與代理之間的高階串接與鏈式整合是可行的。

> 判定基準：◎＝MCP both（客戶端・伺服器雙向支援）可進行雙向連攜 ／ ◯＝標準支援 MCP client ／ △＝以自有規格或個別設定為主 ／ —＝開放範圍內未記載

---

### 2.8 費用明確（課金單位・點數・無上限風險）

[![將 14 個 AI 代理產品的課金型態分為 4 類的圖。席次月費・定額制（Copilot・Cursor・Claude Code・Cowork・Codex）、點數計量（Manus・Cline・Kiro・Antigravity）、對話數・ACU・執行次數計量（Agentforce・Devin・CrewAI）、BYOK＝無償 OSS 加上模型實費（Hermes Agent・OpenClaw）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-pricing-structure.a2324afb.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=beeb5d0ad3ed80966d9756467e667ee3)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-pricing-structure.a2324afb.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=beeb5d0ad3ed80966d9756467e667ee3)

這是判斷能否在簽呈上寫下確定預算，以及是否有因試誤或迴圈導致帳單暴增風險的指標。

#### ① 依課費單位區分的 4 種型態

AI 代理的費用體系，比 ChatAI 更複雜。

1. **席次月費・定額制（5 個產品）**:  
	[GitHub Copilot](https://renkeimap.jp/tool/github-copilot) （Business $19/月）、 [Cursor](https://renkeimap.jp/tool/cursor) （Pro $20/月）、 [Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) 、 [Codex](https://renkeimap.jp/tool/codex) 。每個席位就能決定預算，因此可以直接在簽呈上填寫金額。Codex 包含在 ChatGPT 的 Go／Plus／Pro 方案中，超出部分則以額外購買點數的方式處理。
2. **點數計量（4 個產品）**:  
	[Manus](https://renkeimap.jp/tool/manus) （購買 $20～$200 點數）、 [Cline](https://renkeimap.jp/tool/cline) （依使用量計算點數）、 [Kiro](https://renkeimap.jp/tool/kiro) 、 [Antigravity](https://renkeimap.jp/tool/antigravity) 。 [Antigravity](https://renkeimap.jp/tool/antigravity) 的免費額度有每週上限，付費則是 AI 點數。 **用多少加多少，務必先設定上限** 。
3. **對話數・ACU・執行次數計量（3 個產品）**:  
	[Agentforce](https://renkeimap.jp/tool/agentforce) （可從對話數／點數／使用者數中選擇）、 [Devin](https://renkeimap.jp/tool/devin) （以 ACU：Agent Compute Units 為單位）、 [CrewAI](https://renkeimap.jp/tool/crewai) （以工作流程執行次數計算）。 **不同產品對「1 次」的定義不同** ，所以在估價前務必確認單位。
4. **BYOK（Bring Your Own Key＝自備 API 金鑰）— 軟體免費，僅需支付模型實費（2 個產品）**:  
	[OpenClaw](https://renkeimap.jp/tool/openclaw) 、 [Hermes Agent](https://renkeimap.jp/tool/hermes-agent) 。軟體本體是免費開源，自行簽約的模型提供商（如 OpenAI、Anthropic 等）API 成本另計。 **軟體費用雖為 0，但模型的計量費仍然存在** 。

#### ② 因迴圈而致的無上限風險

自律代理特有的風險之一，是錯誤修復反覆進行，陷入「無限迴圈」，一夜之間消耗數萬元的點數或 token。能設定預算上限（Spend Limit）的產品，幾乎是必要的自保措施。

> 判定基準：◎＝以固定席次金額為主，月費預算較容易確定 ／ ◯＝有免費 BYOK 或定額額度 ／ △＝以計量點數／ACU 為主，費用較容易波動 ／ —＝開放範圍內未記載

---

## 3\. AI 代理的法律責任與智慧財產／免責現實

比 ChatAI 更嚴重的是，「 **如果 AI 代理做錯操作，導致客戶資料消失或造成損害，法律責任由誰承擔？** 」這個問題。

在調查各公司的使用條款（Terms of Service）後，得出了經營層必須正視的 **嚴峻事實** 。

區分代表性產品操作事故・資料毀損的責任歸屬著作權侵害補償（智慧財產補償）契約上的必要防衛措施 **大型雲端・IDE 內建型（4 個產品）** [GitHub Copilot](https://renkeimap.jp/tool/github-copilot), [Codex](https://renkeimap.jp/tool/codex), [Antigravity](https://renkeimap.jp/tool/antigravity), [Kiro](https://renkeimap.jp/tool/kiro) 無保證（AS-IS）與責任限制皆於條款中明記有補償條款（GitHub Copilot・Kiro・Codex；補償的方向與範圍請參考原文（EVIDENCE §3.9））／Antigravity 未在開放範圍內記載本番直連禁止・提交前人工目視確認 **自律專精型雲端（4 個產品）** [Devin](https://renkeimap.jp/tool/devin), [Manus](https://renkeimap.jp/tool/manus), [Agentforce](https://renkeimap.jp/tool/agentforce), [CrewAI](https://renkeimap.jp/tool/crewai) 無保證與責任限制於條款中明記（Agentforce 否認默示保證並設有責任上限；有償服務並未寫成「現狀有態」）有補償條款（Devin・Manus・Agentforce・CrewAI；補償的方向與範圍請參考原文（EVIDENCE §3.9））沙箱隔離・前付點數最小購買 **本機桌面・編輯器（3 個產品）** [Cursor](https://renkeimap.jp/tool/cursor), [Claude Code](https://renkeimap.jp/tool/claude-code), [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) 無保證（AS-IS）與責任限制於條款中明記有補償條款（3 個產品皆有法人合約條款；Anthropic 設有為第三方請求提供防禦的規定）確保自動還原點・使用 Git 分支管理 **開源・自家常駐（3 個產品）** [OpenClaw](https://renkeimap.jp/tool/openclaw), Hermes Agent, [Cline](https://renkeimap.jp/tool/cline) 授權本文中的無保證・責任限制（MIT／Apache-2.0） **沒有保護使用者的補償** 。Apache-2.0（Cline）的補償條款，是「再散布者需補償貢獻者」的方向，並不是保護使用者。MIT（OpenClaw・Hermes Agent）則沒有補償條款Docker 等隔離執行・嚴格限制存取權限#### ① 14 個產品全部都明定「不保證」（其中 13 個產品寫明「現狀有貢（AS-IS）」）

本文是逐字對照各公司的條款與授權書 **保存副本及原文** 後確認的（清單見 EVIDENCE §3.9）。  
**14 個產品全部** 都明定無論明示或默示皆不提供保證，其中 **13 個產品** 明記以「 **現狀有貢（AS-IS）** 」提供服務。 [Agentforce](https://renkeimap.jp/tool/agentforce) （Salesforce 的 Main Services Agreement）則是唯一例外：它對有償服務有明示保證，但否認默示保證，且只將「現狀有貢」適用於免費服務與 Beta 版本。

- **[Devin（Cognition）](https://renkeimap.jp/tool/devin)** ： `COGNITION MAKES NO WARRANTY OF ANY KIND THAT THE COGNITION IP, SERVICES, DOCUMENTATION, OR ANY PRODUCTS OR RESULTS OF THE USE THEREOF, WILL MEET YOUR OR ANY OTHER PERSON'S REQUIREMENTS, OPERATE WITHOUT INTERRUPTION, ACHIEVE ANY INTENDED RESULT` （譯：Cognition 不以任何形式保證其智慧財產、服務、文件，或使用這些內容所產生的任何成果或結果，能符合你或任何他人的需求、不中斷運作、或達成任何預期結果）
- **[Cursor（Anysphere）](https://renkeimap.jp/tool/cursor)** ： `本服務及 Suggestions，係以現狀有貢（"AS IS"）且「視可提供情況」（"AS AVAILABLE"）提供。ANYSPHERE 不就本服務及 Suggestions 提供任何形式之明示或默示保證。` （日本語版條款原文）
- **[OpenClaw](https://renkeimap.jp/tool/openclaw)** ： `THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED` （譯：本軟體係以現狀有貢方式提供，不附帶任何明示或默示保證。MIT 授權本文）
- **[Codex（OpenAI）](https://renkeimap.jp/tool/codex)** ： `本服務係以「現狀有貢」方式提供` （法人向「OpenAI 服務協議」免責條款之條文，日本語版原文）
- **[Agentforce（Salesforce）](https://renkeimap.jp/tool/agentforce)** ： `EXCEPT AS EXPRESSLY PROVIDED HEREIN, NEITHER PARTY MAKES ANY WARRANTY OF ANY KIND, WHETHER EXPRESS, IMPLIED, STATUTORY OR OTHERWISE` （譯：除本協議明示者外，任一方均不提供任何形式之保證，不論明示、默示、法定或其他）。不過 `SERVICES PROVIDED FREE OF CHARGE, AND BETA SERVICES ARE PROVIDED “AS IS,”` （譯：免費服務與 Beta 服務係以現狀有貢提供）將現狀有貢的範圍限定住了。

也就是說， **對於 AI 代理所造成的誤操作或事故損害，14 個產品不是不負責任，就是把責任限定在使用費等上限以內** 。這是契約上的出發點。

#### ② 著作權補償（Copyright Commitment）的限定性

生成的程式碼或成果物若侵害他人著作權時的「著作權補償」，也與 ChatAI 相同，僅限於有償法人合約。

- 條款副本中 **有補償條款** 的產品包括 [GitHub Copilot](https://renkeimap.jp/tool/github-copilot) ・ [Kiro](https://renkeimap.jp/tool/kiro) ・ [Devin](https://renkeimap.jp/tool/devin) ・ [Manus](https://renkeimap.jp/tool/manus) ・ [CrewAI](https://renkeimap.jp/tool/crewai) ・ [Cursor](https://renkeimap.jp/tool/cursor) ・ [Claude Code](https://renkeimap.jp/tool/claude-code) ・ [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) ・ [Codex](https://renkeimap.jp/tool/codex) （法人服務協議中的「OpenAI 提供之補償」）・ [Agentforce](https://renkeimap.jp/tool/agentforce) （Main Services Agreement 的「Indemnification by SFDC」）。但多數都附帶嚴格的免責條件，例如需啟用安全過濾器等（補償的方向與範圍請參考原文（EVIDENCE §3.9））。
- **開源類別沒有保護使用者的補償。** [Cline](https://renkeimap.jp/tool/cline) 的 Apache-2.0 雖然也有補償條款，但那是 **由再散布者補償貢獻者** 的方向，並非保護使用者。 [OpenClaw](https://renkeimap.jp/tool/openclaw) ・ [Hermes Agent](https://renkeimap.jp/tool/hermes-agent) 的 MIT 授權則完全沒有補償條款。

#### 結論：權限設計的技術關卡是唯一防線

把工作交給 AI 代理，意味著「 **在法律上，公司要承擔和員工手動操作完全相同的法律責任** 」。因此，後面會提到的「縮小權限範圍」、「避免直連正式環境」、「一定要設承認關卡」這些運作設計，對中小企業而言就是攸關存亡的問題。

---

## 4\. 適合中小企業的「不失敗 AI 代理導入步驟」（實務 7 步驟）

[![中小企業用 AI 代理導入 7 步驟圖。依序排列：確認要交辦的業務、在隔離環境中事前 PoC、最小化權限設計、強制設置承認關卡、法人契約與預算上限、告知回滾流程、日誌稽核與成效驗證](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-rollout-steps.cefb155e.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=835a352532f3796b1e384a7c45d1fe96)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-rollout-steps.cefb155e.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=835a352532f3796b1e384a7c45d1fe96)

這是為了在避免重大事故與預算超支的同時，最大化 AI 代理效益的 **實務 7 步驟** 。

- **STEP 1：確認要交辦的業務（從即使壞掉也能恢復的工作開始）**:  
	不要一開始就交辦「正式資料庫更新」或「自動寄信給客戶」。應選擇「競品網站研究」、「新畫面原型實作」、「測試程式碼撰寫」等容易確認成果物、即使失敗也能回復的工作。
- **STEP 2：在隔離環境中進行事前 PoC（少量人員驗證）**:  
	準備與正式環境切離的沙箱（例如 Git 實驗分支或測試用雲端 VM），由 1～2 人進行運作驗證。
- **STEP 3：權限（Permission）最小化設計**:  
	先從「唯讀（Read Only）」開始授予代理 API Key 權限。在 PoC 尚未確認安全前，絕對不要給寫入（Write/Delete）權限，這是鐵則。
- **STEP 4：強制設置承認關卡（自律模式 OFF）**:  
	像 Kiro 那樣設定為「Autonomous mode is off by default（自動駕駛預設關閉）」，並在危險命令執行或檔案覆寫前，務必讓人類在畫面上按下「Approve」的運作規則。
- **STEP 5：簽訂法人合約與設定預算上限**:  
	簽約不會將輸入資料重新訓練的法人方案，並務必設定計量課費上限（Monthly Spending Limit），防止無上限扣費。
- **STEP 6：在公司內周知回滾流程**:  
	將代理做錯操作時如何回到前一狀態的流程（Git revert、Cline Checkpoints、Cursor 還原點）製成手冊。
- **STEP 7：定期做日誌稽核與效益驗證**:  
	每週盤點代理的執行日誌，並量測「節省了多少工作時間」、「是否出現非預期錯誤或不必要的迴圈」。

---

## 5\. 經營層・資訊部門用的「導入前自我檢查表」

在核准導入稟議之前，或是決定要發放給現場使用之前，請先自我檢查以下 13 項。

#確認項目判定檢查重點 **1\*\*** 業務領域是否相符 **\[ \]目的是否明確，是「開發程式實作」、還是「一般研究調查・文件製作」、或是「CRM 內部流程」？** 2 **\*\*操作環境的安全性** \[ \]執行位置是否限定在雲端隔離虛擬機器或特定資料夾？（不是直接連到正式環境吧） **3\*\*** 自律模式的限制 **\[ \]在有疑慮時或危險操作前，是否設定為向人類要求批准（Human-in-the-loop）？** 4 **\*\*是否具備回滾功能** \[ \]若發生錯誤變更，是否能一鍵將檔案或 DB 回到前一狀態？ **5\*\*** 是否完全阻絕學習用途 **\[ \]所簽方案是否設定為公司內部程式碼或業務資料不會被拿去重新訓練 AI 模型？** 6 **\*\*資格資訊（API Key）的保護** \[ \]提供的 API Key 權限是否已縮到最小（例如唯讀）？會不會從裝置外洩？ **7\*\*** 預算上限（Limit）設定 **\[ \]當點數或計量課費陷入迴圈時，是否能設定自動停止的使用額度？** 8 **\*\*前付餘額失效條款** \[ \]如果是點數前付型，退會時餘額是否會被沒收？ **9\*\*** 成果物的可攜性 **\[ \]生成的程式碼或文件是否會被加浮水印，或被綁死在特定平台？** 10 **\*\*MCP 支援與擴充性** \[ \]是否支援未來能與自家公司系統（kintone/freee 等）串接的共通規格（MCP）？ **11\*\*** 執行日誌的稽核追蹤 **\[ \]是否留下可追蹤代理「何時、用哪個工具、改了什麼」的日誌？** 12 **\*\*免責條款的公司內認知** \[ \]是否已在公司內達成共識：萬一因操作失誤造成損害，100% 由自家公司自行負責？ **13\*\*** 負責人 IT 能力是否相符\*\*\[ \]是否打算把必須使用 CLI 與環境建置的產品硬塞給非工程師部門？---

## 6\. 14 個產品的個別檔案與官方第一手資料列表

本文所有敘述、判定與資料，皆以各公司公開的官方第一手資料為根據。各代理的官方手冊、使用條款、資安文件與價格表 URL 如下。14 個產品橫向列表亦可於 **[IT 連攜地圖（renkeimap.jp）](https://renkeimap.jp/)** 查閱。

產品名供應商企業總部國・準據法官方產品頁・條款來源調查日 **[Agentforce](https://renkeimap.jp/tool/agentforce)** Salesforce Japan 株式會社（美國 Salesforce）美國 [產品頁](https://www.salesforce.com/jp/agentforce/) ／ [運作方式・FAQ](https://www.salesforce.com/jp/agentforce/how-it-works/) 2026-08-26 **[Antigravity](https://renkeimap.jp/tool/antigravity)** Google LLC美國 [官方網站](https://antigravity.google/) ／ [開始使用指南](https://antigravity.google/docs/getting-started) 2026-08-26 **[Claude Code](https://renkeimap.jp/tool/claude-code)** Anthropic, PBC美國（加州法） [官方文件](https://code.claude.com/docs) ／ [法人條款](https://www.anthropic.com/legal/commercial-terms) 2026-08-26 **[Claude Cowork](https://renkeimap.jp/tool/claude-cowork)** Anthropic, PBC美國（加州法） [Claude 價格表](https://claude.com/ja/pricing) ／ [隱私中心](https://privacy.claude.com/en/) 2026-08-26 **[Cline](https://renkeimap.jp/tool/cline)** Cline Bot Inc.美國 [官方儲存庫](https://github.com/cline/cline) ／ [工作流程文件](https://docs.cline.bot/core-workflows/checkpoints) 2026-08-26 **[Codex](https://renkeimap.jp/tool/codex)** OpenAI OpCo, LLC美國（加州法） [官方文件](https://learn.chatgpt.com/docs) ／ [使用條款](https://openai.com/ja-JP/policies/row-terms-of-use/) 2026-08-26 **[CrewAI](https://renkeimap.jp/tool/crewai)** CrewAI, Inc.美國（紐約州法） [官方網站](https://crewai.com/) ／ [使用條款](https://crewai.com/terms-of-use) 2026-08-26 **[Cursor](https://renkeimap.jp/tool/cursor)** Anysphere, Inc.美國 [官方網站](https://cursor.com/) ／ [資安・隱私](https://cursor.com/security) 2026-08-26 **[Devin](https://renkeimap.jp/tool/devin)** Cognition AI, Inc.美國 [官方網站](https://devin.ai/) ／ [官方文件](https://docs.devin.ai/get-started/devin-intro) 2026-08-26 **[GitHub Copilot](https://renkeimap.jp/tool/github-copilot)** GitHub, Inc.（Microsoft）美國 [產品頁](https://github.com/features/copilot) ／ [隱私聲明](https://docs.github.com/ja/site-policy/privacy-policies/github-general-privacy-statement) 2026-08-26 **[Hermes Agent](https://renkeimap.jp/tool/hermes-agent)** Nous Research美國 [官方文件](https://hermes-agent.nousresearch.com/) ／ [官方儲存庫](https://github.com/NousResearch/hermes-agent) 2026-09-01 **[Kiro](https://renkeimap.jp/tool/kiro)** Amazon Web Services (AWS)美國 [官方網站](https://kiro.dev/) ／ [自律模式文件](https://kiro.dev/docs/web/autonomous-mode/) 2026-08-26 **[Manus](https://renkeimap.jp/tool/manus)** Butterfly Effect Pte. Ltd.新加坡 [官方網站](https://manus.im/) ／ [使用條款](https://manus.im/terms) 2026-08-26 **[OpenClaw](https://renkeimap.jp/tool/openclaw)** OpenClaw Foundation—（非營利組織・未記載所在地） [官方網站](https://openclaw.ai/) ／ [資安指引](https://docs.openclaw.ai/gateway/security) 2026-08-26---

## 7\. 結尾 — 為了選擇與自家體制相符的方案

[![依業務目的與體制選擇 AI 代理的反向查詢地圖。開發（Cursor・Claude Code・Devin・Codex）、研究調查（Manus）、Salesforce（Agentforce）、常駐與遠端操作（OpenClaw・Claude Cowork）、多個 AI 協作（CrewAI・Kiro）及各自的決定因素與注意事項](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-usecase-fit.0a40498c.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=416280b075d40d96975afd87eb23cc0e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fagent-usecase-fit.0a40498c.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=416280b075d40d96975afd87eb23cc0e)

### 7-1. 開頭三個問題的答案

1. **不會出事嗎** → 會出事。所以要先選擇 **預設關閉自動執行、且可插入承認關卡的產品** （§2.3）。而且 **事故責任由自家公司承擔** —— 14 個產品都在條款中明定，不負損害責任或僅負上限內責任（§3）。
2. **只有工程師能用嗎** → 今天的主戰場 **仍然是開發** 。14 個產品中有 8 個是程式工具，而以一般業務為主的只有 3 個（§2.2 ①）。若是以非技術人員為主的部門，請先看這 3 個。
3. **能復原嗎・費用如何** → 能否回復的機制在 **各產品之間差異明顯** （§2.5 ②）。費用則取決於 **課費型態** ，若是計量點數型，不先設定上限就很難預估（§2.8）。

「因為很流行」「因為未來就是代理時代」——僅憑這類話就選工具，並毫無防備地串接到公司內部系統，是最危險的行為之一。

最後，整理出依公司業務目的反向選擇該用哪個代理的 **逆向查找標準** 。

自家公司想做的業務適合的產品群最應重視的指標運作重點 **想自動化公司內系統的委外開發・改版** [Cursor](https://renkeimap.jp/tool/cursor) 、 [Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Devin](https://renkeimap.jp/tool/devin) 、 [Codex](https://renkeimap.jp/tool/codex) 隔離復原・安全主權使用隔離沙箱，讓它在 Git 分支上建立 PR（禁止直連正式環境） **想交辦市場研究・競品調查・投影片製作** [Manus](https://renkeimap.jp/tool/manus) 業務適合・費用明確善用可由人接手操作的環境，並注意不要過度消耗點數 **想自動化 Salesforce 的客服與 CRM 更新** [Agentforce](https://renkeimap.jp/tool/agentforce) 委任統制・安全主權設計 Salesforce 內的護欄與權限範圍，並設定移交給人類操作員的流程 **想讓它常駐在公司電腦，並從聊天中遠端操作工具** [OpenClaw](https://renkeimap.jp/tool/openclaw) 、 [Claude Cowork](https://renkeimap.jp/tool/claude-cowork) 連攜操作・安全主權用 MCP 連線限定可碰觸的工具，機密密碼由本機環境保護 **想將多個 AI 組合起來跑複雜的業務流程** [CrewAI](https://renkeimap.jp/tool/crewai) 、 [Kiro](https://renkeimap.jp/tool/kiro) 委任統制・業務適合配置 0～10 分的檢查員代理，或讓自律模式預設關閉、由人類批准AI 代理的價值，不是「把人類的工作全部搶走」。而是「 **在由人類明確決定的權限範圍內，忠實代行繁瑣的試誤與例行作業** 」。

請先確認自家公司「想交辦哪種工作」、「萬一出事能不能回復」，再拿本文的矩陣與檢查表作為武器，穩健且安全地推進導入判斷。

---

※ 本文所載的產品規格、條款、價格與 URL，皆以 2026 年 8 月～9 月調查時點的各公司官方公開資訊為基礎。最新規格與各系統間的串接可否，請於 **[RenkeiMap（IT 連攜地圖）](https://renkeimap.jp/)** 隨時查詢更新。本系列的全部文章列表與舊文，也請一併參閱 **[作者 Qiita 頁面（@songchong）](https://qiita.com/songchong)** 。

**【可轉載】關於本文轉載**

本文的文字與圖表全部都可轉載。圖片請 **不要加工** 直接使用。轉載時請在出處標註 [renkeimap.jp](https://renkeimap.jp/) 或本文連結。事前不需要聯絡。

※ 作者曾任職於日立系 IT 供應商、長照軟體供應商、以及大學醫院 IT 部門，現獨立從事中小企業 IT・DX 支援，同時以第一手資料調查業務系統之間的「連結」。文中的「編輯部」是指作者所屬的 [IT 連攜地圖編輯部](https://renkeimap.jp/about) 。若發現錯誤，請透過 [更正窗口](https://renkeimap.jp/request) （免費・免帳號）回報。更正紀錄也會公開。

---

原文出處： [https://qiita.com/songchong/items/94df4fbdeabaaf8f8ef5](https://qiita.com/songchong/items/94df4fbdeabaaf8f8ef5)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/a66Ona) [2瀏覽](https://codelove.tw/@tony/post/a66Ona)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/a66Ona)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝21

324

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788743511&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fa66Ona&amp;pra=5&amp;aiof=11&amp;asro=0&amp;itsi=19&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788743511495&amp;bpp=11&amp;bdt=140&amp;idt=40&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788742928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788742928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4203459691429&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400775%2C95401045%2C122880837%2C122880828%2C95340252%2C95340254&amp;oid=2&amp;pvsid=8597108220344386&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=45" title="Advertisement" aria-label="Advertisement"></iframe>