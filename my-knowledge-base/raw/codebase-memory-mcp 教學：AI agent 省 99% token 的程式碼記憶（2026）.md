---
title: "codebase-memory-mcp 教學：AI agent 省 99% token 的程式碼記憶（2026）"
source: "https://slash-invest.com/codebase-memory-mcp-ai-agent-token-2026/"
author:
  - "[[Mr. Slash]]"
published: 2026-06-29
created: 2026-07-09
description: "codebase-memory-mcp 把整個 codebase 建成知識圖譜，Claude Code 等 AI agent 查程式碼不用逐檔 grep，官方實測省 99% token。安裝設定、實測效果、適用場景一次看。"
tags:
  - "clippings"
status: processed
---
先講一個這個月最多工程師在吵的數字：用 AI agent 寫程式，一個任務動輒燒掉四十萬到兩百萬個 token，重度用戶一個月的帳單可以衝到一兩千美金，極端個案幾天就破四千。Gartner 甚至預測到了 2028 年，AI 寫程式的成本會超過你請那個工程師的年薪。錢花到哪去了？很大一塊，是 agent 每次想「搞清楚這個 codebase 長怎樣」的時候，一個檔一個檔地 grep、read、再 grep——同樣的問題，它從零翻一次。codebase-memory-mcp 這個這幾天衝上 GitHub 日榜、單日新增兩千多顆星的開源工具，想砍掉的就是這筆冤枉錢：給你的 AI agent 一個「程式碼記憶」，把整個專案先建成一張圖，之後要查直接問圖，不用再翻檔。

**一句話結論：** codebase-memory-mcp 是一個開源的 MCP 工具，把整個 codebase 在毫秒級建成一張持久知識圖譜，讓 Claude Code 這類 AI agent 查呼叫鏈、找定義、看架構時直接問圖、不用逐檔 grep。官方實測五條結構查詢只花約 3,400 token（逐檔翻要約 41 萬，省 99%）；單一執行檔、零依賴、100% 本地，常用 AI 寫程式的人最該認識。

*（截至 2026 年 6 月。codebase-memory-mcp 仍在密集開發，版本、效能數字與支援的 agent 以官方 GitHub 為準。）*

## codebase-memory-mcp 是什麼？一句話搞懂

它是一個給 AI coding agent 用的「程式碼情報引擎」。你把它接到 Claude Code（或 Codex、Gemini CLI 這類支援 MCP 的工具）之後，講一句「幫我把這個專案建索引」，它就用 tree-sitter 把你整個 codebase 解析成一張知識圖譜——把每個函式、類別、呼叫關係、HTTP 路由、跨服務連結，都變成圖上的節點和邊。之後 agent 要問「誰呼叫了 ProcessOrder」「這個改動會影響哪些地方」「整個專案的架構長怎樣」，它不再傻傻地逐檔搜，而是直接對這張圖下一條查詢，毫秒級就拿到答案。

它叫 MCP，是因為它走的是 Model Context Protocol——一個讓外部工具接進 AI agent 的標準接口（你可以理解成「給 AI 裝外掛的 USB 孔」）。到 2026 年 6 月，這個 repo 已經累積約 1.9 萬顆 GitHub 星，這幾天單日新增兩千多顆，是當天全站新增星數最多的項目之一。授權是 MIT，能商用。

## 它到底解決了哪個痛？

痛在「重複勞動」。AI agent 寫程式之所以燒 token，關鍵是它沒有記憶——每開一個新對話、每跑一個新任務，它對你的 codebase 一無所知，只能靠 grep 關鍵字、打開檔案、再順著 import 一路翻下去。一個稍微大一點的專案，光是「搞懂這段程式碼接到哪」就要來回幾十次工具呼叫，而 agentic coding 的計費邏輯是：每一次工具呼叫，幾乎都把整個 context 重送一遍。研究實測過，agentic 任務燒的 token 是普通聊天的十倍到一百倍，帳單主要就堆在這些 input token 上。

codebase-memory-mcp 的做法是把「搞懂結構」這件事一次做完、存起來。第一次建索引時，它把整個 codebase 變成圖；之後 agent 的問題都變成對圖的一條查詢。官方給的對照很直白：五條結構查詢，走它約消耗 3,400 個 token，走傳統逐檔搜索約 41.2 萬個——少了 99.2%。它附了一份 arXiv 預印本（編號 2603.27277），在 31 個真實 repo 上評測，答案質量 83%、token 比逐檔探索少約 10 倍、工具呼叫少約 2.1 倍。數字怎麼解讀等下再說，但方向很清楚：把記憶補上，省下的就是重複翻檔的那一大筆。

## 它怎麼在「不塞整個 codebase」的情況下幫 agent 找答案？

關鍵在它換了一種「給 AI 看程式碼」的方式。一般做法是把相關檔案整段塞進 context 讓模型自己讀，又貴又容易漏。codebase-memory-mcp 反過來，先用 tree-sitter（一個語法解析器）把程式碼拆成抽象語法樹，再抽出函式、類別、呼叫、import 這些結構，建成一張可以查詢的圖。模型要的不是「整段原始碼」，而是「這個函式被誰呼叫、它又呼叫了誰」這種結構答案——而結構，正好可以用一條圖查詢精準撈出來。

把整個 codebase 硬塞給模型讀，等於用一大堆雜訊換一個結構問題的答案。codebase-memory-mcp 只回傳你真正要的那幾個節點和邊。官方點破：別的程式碼圖工具還自帶一個 LLM 來做「自然語言轉查詢」，它偏不——你正在對話的那個 agent，本來就是最好的翻譯器，何必再多養一個、多付一份 API 錢。

為了讓圖夠準，它在 tree-sitter 之上還疊了一層叫 Hybrid LSP 的型別推導，對 Python、TypeScript、Go、Java、Rust、C#、PHP 等十幾種主流語言，能像 IDE 的「跳到定義」那樣，解析出 user.profile.display\_name() 真正指向三個模組外的哪個定義。整條流水線是：tree-sitter 快速解析 → Hybrid LSP 補型別 → 存進本地 SQLite 圖庫 → agent 下查詢。會省、會準，是因為它把「讀程式碼」這件事，從「看一大段文字」偷換成了「查一張結構圖」。

## 那些誇張的數字，可信嗎？

先把官方數字攤出來：把 Linux 核心（2,800 萬行、7.5 萬個檔）全量建索引約 3 分鐘；一般專案是毫秒到幾秒級（Django 約 6 秒）；建好之後，Cypher 圖查詢小於 1 毫秒、追一條呼叫鏈小於 10 毫秒。支援 158 種語言的語法解析，其中十幾種有完整型別推導。這些是在 Apple M3 Pro 上跑出來的基準。

我的看法是：方向絕對成立，但「省 99%」是它挑了最理想的對照情境（五條純結構查詢）。真實寫程式不會只有結構查詢，agent 還是要讀程式碼原文、要改檔、要跑測試，這些一樣燒 token。所以實務上你省下的多半是「探索階段」那一截，不會整條帳單砍九成。把它想成「幫 agent 省掉重複偵查的開銷」，而不是「token 帳單直接打一折」，期待值會剛好。即便如此，對一個每天跟 agent 來回幾百次的人，光是探索這段省下來，也很有感。

## 自己用，一個月大概能省多少 token？

工具本身免費開源，真正的成本是你那個 AI agent 的 token 用量。下面這個小工具，幫你用「逐檔搜 vs 查圖」抓個概念，數字都能自己改：

AI agent 月度 token 帳單快算：逐檔搜 vs 程式碼記憶

## 不會寫程式也能用嗎？三步上手

能，而且這個比多數開發工具還簡單，因為它是單一執行檔、零依賴，不用裝 Docker、不用 runtime、不用 API 金鑰。流程大致三步：

1. **一行指令裝起來。** macOS 或 Linux 在終端機貼上官方那行 `curl ... | bash` ，它會自動下載對應你系統的執行檔、裝好；Windows 用 PowerShell 下載 install.ps1 跑一遍。最省事的做法甚至是直接跟 Claude Code 講「幫我裝這個 MCP server」，把 GitHub 連結丟給它。
2. **讓它自動接好你的 agent。** 安裝程式會自己偵測你電腦上裝了哪些 coding agent——Claude Code、Codex CLI、Gemini CLI、Zed、OpenCode、OpenClaw、Aider、VS Code 等十一種——再幫每一個寫好 MCP 設定。你只要重啟一下 agent 就行。
3. **講一句「幫我建索引」。** 在你的專案資料夾裡開 agent，說一句「Index this project」，它就把整個 codebase 建成圖。之後背景會盯著 git 變動自動更新，你問結構問題時，agent 自己會去查圖。

講白一點：純新手唯一要跨的門檻，是你得先在用一個支援 MCP 的 AI agent（最常見就是 Claude Code）。只要你已經在用，接上去幾乎零摩擦。它還有一個選用的 3D 圖形介面，裝 UI 版之後開 localhost:9749，可以把你 codebase 的關係圖用 3D 轉著看，挺直觀。

## 它跟「直接讓 agent 自己翻檔」差在哪？

差在「有沒有記憶」和「要不要重複勞動」。把三種做法擺一起比最清楚：

| 做法 | 怎麼找答案 | token 開銷 | 要不要多養工具 |
| --- | --- | --- | --- |
| agent 直接逐檔 grep／read | 每次從零翻檔、順 import 追 | 高，每次重來 | 不用，但最燒錢 |
| 自帶 LLM 的程式碼圖工具 | 另一個模型把問題轉成查詢 | 中，但多一份 API 費 | 要多配一組金鑰／模型 |
| codebase-memory-mcp | 建一次圖，之後 agent 直接查 | 低，查圖幾乎不花 | 不用，借用你現有的 agent |

它最聰明的取捨，就是「不自帶 LLM」。你已經在跟 Claude Code 對話了，那個模型本身就會把「誰呼叫了這個函式」翻譯成一條圖查詢，何必再塞一個模型進去、再付一份錢。少一個要配置的東西、少一組金鑰、少一份帳單，這種「想清楚再做減法」的工程品味，比功能表多長一截更值錢。

## 用之前要知道的限制與風險

先潑點冷水。第一，它的設計就是「讀你的 codebase、而且會寫進你 agent 的設定檔」——這是它要做事必然的權限，但你心裡要清楚它動了什麼。好的是官方強調全程 100% 本地，程式碼不會離開你的機器，不收任何遙測；每個發布的執行檔都過 VirusTotal 七十多個防毒引擎掃描、零偵測，並有 SLSA 等級三的建置簽章可驗。在意供應鏈安全的人，可以先 verify 再跑。

第二，它不是一個 LLM，本身不會「思考」——它只負責建圖和查圖，智慧那一層完全靠你的 agent。所以沒接 agent、單獨用它，意義不大。第三，158 種語言裡，型別推導只對十幾種主流語言完整，冷門語言會退化成純文字比對，結果沒那麼準。把它當成「給 agent 的一個很強的程式碼地圖」，而不是「裝了就自動會寫 code」，期待值會剛好。

## codebase-memory-mcp 適合誰？不適合誰？

適合的人很明確：每天用 Claude Code、Cursor、Codex 這類 AI agent 在中大型 codebase 上幹活、又心痛 token 帳單的人；要 agent 頻繁理解「這段接到哪、改了會炸到哪」的工程師；以及想讓 AI 在自己整個專案脈絡下工作、而不是每次都從零交代的人。對團隊也友善——它能把建好的圖壓成一個檔 commit 進 repo，同事 clone 下來不用重新建一次索引。

不適合的也很清楚：完全不寫程式、也不用任何 coding agent 的人，這工具跟你無關。只在小腳本、單檔專案上玩 AI 的人，agent 直接讀完整個檔就好，建圖反而是多餘的儀式。還有，期待「裝了就讓 AI 自動把整個專案寫好」的人——它給的是地圖，不是替你開車的司機。

## 對怕被 AI 取代、想靠技術變現的人，有什麼用？

這才是我覺得對一般人最值得留意的地方。AI 寫程式的成本正在變成一筆真實、而且會越滾越大的開銷——這也是為什麼「怎麼讓 agent 少燒 token」突然變成一門搶手的本事。能把 AI agent 用得又省又準的人，接案、做產品、幫公司省成本，都比別人多一張牌。codebase-memory-mcp 這種工具，本質上就是在教你一件事：AI 不是塞越多 context 越聰明，而是「給對的結構、問對的問題」才省又準。

更值錢的是順手摸到的底層能力。為了用它，你會接觸到 MCP、AI agent、知識圖譜、命令列這套東西，遷移性極高——今天拿來省 token，明天就能拿這套思路去自動化別的雜活。AI 不是只會把工作收走，它也在把「以前只有大公司玩得起的工程能力」開放給願意動手的個人。重點從來不是「我會不會被取代」，而是「這些開源彈藥，我有沒有撿起來用」。

## 我會怎麼看 codebase-memory-mcp？

我最欣賞的是它那個「不自帶 LLM、把記憶補上」的判斷。一堆工具拼命想讓 AI 一次吞下更多 context，結果又慢又貴；它反過來認清「agent 缺的不是更大的腦袋，是一份不會忘的程式碼地圖」，把問題從「塞更多」偷換成「建一次、查很多次」。這種抓本質的工程品味，加上單一執行檔、零依賴、本地優先這些務實到位的工程細節，是它能幾天衝上日榜的真正原因。

但要誠實講：它不是給所有人的工具，「省 99% token」也別照單全收——那是最理想情境的數字，真實省下的是探索那一截。如果你正好是「天天用 agent、codebase 不小、又在乎帳單」的那種人，它給的回報相當實在；如果你只是偶爾叫 AI 改個小腳本，那它對你太重了。工具沒有絕對好壞，是看你拿「建索引的那點麻煩」換回多少「之後的省與準」。

## 常見問題

### codebase-memory-mcp 要錢嗎？

工具本身免費、開源，授權是 MIT，能商用。它不收訂閱、不用 API 金鑰、不裝雲端服務。唯一的成本是驅動它的那個 AI agent 的 token 用量——但它的賣點正好就是幫你把這筆用量壓下來。

### 不會寫程式能用 codebase-memory-mcp 嗎？

能用，但前提是你得先在用一個支援 MCP 的 AI agent（最常見是 Claude Code）。安裝可以一行指令搞定，甚至直接叫 agent 幫你裝；之後講一句「幫我建索引」就行。它本身不需要你寫程式碼，但它是給「會用 AI agent 寫程式的人」省錢的工具，完全不碰 coding 的人用不上。

### 它真的能省 99% token 嗎？

「省 99%」是官方在「五條純結構查詢」這個理想對照下測出來的（3,400 token vs 41 萬）。方向是真的，但真實寫程式還要讀原文、改檔、跑測試，這些照樣燒 token，所以你實際省下的是「探索程式碼結構」那一段，不會整條帳單砍九成。當成「省掉重複偵查的開銷」來理解最準。

### 它支援哪些 AI agent 和語言？

安裝時會自動偵測並配置 Claude Code、Codex CLI、Gemini CLI、Zed、OpenCode、OpenClaw、Aider、VS Code 等十一種 agent。語言方面用 tree-sitter 支援 158 種的語法解析，其中 Python、TypeScript、Go、Java、Rust、C#、PHP 等十幾種有完整的型別推導，準確度最高。

## 不喜歡 codebase-memory-mcp，有什麼替代方案？

看你最在意哪一塊。只想要 agent 自己會找、不想多裝東西，那就靠 Claude Code、Cursor 內建的 codebase 索引和語意搜尋，夠用但每次探索照樣花 token。想要更傳統的「程式碼地圖」，可以看 Sourcegraph、Sourcetrail 這類程式碼導航工具，偏給人看而不是給 agent 查。要的就是「給 AI agent 用、本地、零依賴、查圖省 token」這個位置，那目前 codebase-memory-mcp 切得算乾淨，值得花那點建索引的成本。沒有哪個絕對最好，是看你願意拿多少「設定的麻煩」換「之後的省與準」。

## 延伸閱讀

- [video-use 是什麼？用 Claude Code 剪片，素材丟進去就吐 final.mp4](https://slash-invest.com/video-use-claude-code-edit-video-2026/)
- [YC 總裁 Garry Tan 的 gstack：23 個指令把 Claude Code 變一整隊工程團隊](https://slash-invest.com/gstack-garry-tan-claude-code-2026/)
- [Agent Reach：一句話讓你的 AI agent 免費讀遍整個互聯網](https://slash-invest.com/agent-reach-ai-agent-internet-access-2026/)
- [AI Berkshire：用 Claude Code 把四位投資大師變成你的投研團隊](https://slash-invest.com/ai-berkshire-claude-code-investment-2026/)
- [2026 加密貨幣交易所手續費比較與邀請碼整理](https://slash-invest.com/crypto-exchange-comparison-2026/)

*※ 本文為工具教學與資訊整理，截至 2026 年 6 月。codebase-memory-mcp 仍在密集開發，版本、效能數字與支援的 agent 更新極快，實際請以官方 GitHub（ [DeusData/codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) ）說明為準。文中提及之第三方服務費用以其官網為準，本文不構成任何投資或商業建議。*