---
title: "介紹適用各種任務的 Claude Code 動態工作流程 (dynamic workflows)"
source: "https://muki.tw/claude-code-dynamic-workflows/"
author:
  - "[[MUKI space*]]"
published: 2026-06-05
created: 2026-08-14
description: "Anthropic 在 Claude Code 推出的 dynamic workflows，讓 Claude 可以為長任務即時生成專屬工作流程，引入 subagents、隔離上下文、對抗驗證與彙總機制。"
tags:
  - "clippings"
status: "processed"
---
介紹適用各種任務的 Claude Code 動態工作流程 (dynamic workflows).md

上週 Anthropic 在 Claude Code 推出了動態工作流程 (dynamic workflows)，可以讓 Claude 即時幫自己寫一個專屬的工作框架 (harness)，而不是永遠只用那個預設的 coding 模式

> 以下文章整理自 Thariq 在 X 上的動態工作流程介紹  
> 原文出處： [https://x.com/trq212/status/2061907337154367865](https://x.com/trq212/status/2061907337154367865)

預設的 Claude Code harness 是為寫程式設計的，但我們使用久了會發現，很多任務在結構上都很像寫程式，例如他們有規格、有子任務、有驗證、也有重構。而 Anthropic 團隊過去為了做到最佳表現，會另外在 Claude Code 上疊自訂的 harness，用在像是以下領域：

- Research 深度研究
- Security analysis 安全分析
- Agent teams 多代理協作
- Code Review 程式碼審查

而現在，這些東西都可以用 **動態工作流程** 在 Claude Code 裡原生打造、重用與分享

![動態工作流程把預設模式組成專屬框架的手繪配圖](https://muki.tw/wordpress/wp-content/uploads/2026/06/dynamic-workflows-figure-1.png)

動態工作流程把預設模式組成專屬框架的手繪配圖

## 如何啟用動態工作流程

首先確定你的 Claude Code 版本是 v2.1.154 以上，再來是有付費 (Pro / Max / Teams..) 方案皆可

我自己最常用的就是輸入指令 `/effort` ，選擇 `ultracode` ，再跟他說我要做什麼事情，他會自動判斷啟動 workflows

![](https://muki.tw/wordpress/wp-content/uploads/2026/06/CleanShot-2026-06-05-at-13.01.35@2x.png)

AD

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="722" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-6816776861246904&amp;output=html&amp;h=280&amp;slotname=2619535264&amp;adk=536108718&amp;adf=3680071312&amp;pi=t.ma~as.2619535264&amp;abgtt=7&amp;w=722&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1786687820&amp;rafmt=1&amp;armr=3&amp;format=722x280&amp;url=https%3A%2F%2Fmuki.tw%2Fclaude-code-dynamic-workflows%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;aiof=12&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786686254087&amp;bpp=2&amp;bdt=1161&amp;idt=132&amp;shv=r20260812&amp;mjsv=m202608100101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dabed5c4f6c137a51%3AT%3D1786685192%3ART%3D1786687573%3AS%3DALNI_MYfJJNV1SVrdt4l6IPLqwFH3KHkcg&amp;gpic=UID%3D000014df72091a95%3AT%3D1786685192%3ART%3D1786687573%3AS%3DALNI_MYFojP7AHvvhSEstSBqIy9HMPHDYQ&amp;eo_id_str=ID%3D61820914debd3376%3AT%3D1786685192%3ART%3D1786687573%3AS%3DAA-AfjbBDTvN3lpfNfXahxBDXTZo&amp;prev_fmts=0x0%2C220x600%2C1634x908&amp;nras=2&amp;correlator=4867460128278&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=322&amp;ady=2342&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399119%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsmWs_eHqpfPb63-D5MNTnrIvrAQCFLy7_MutNkKb4Q8425svu78dZ_sIURAkfdgfwmHEKVDh0BXs_s3mdhcvBGaLUy31AbbRumvtDlkSCqV&amp;pvsid=8111741327692228&amp;tmod=1284125063&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fmuki.tw%2Fjournal%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=M%7C%7CeEbr%7Cp&amp;abl=XS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C241x714_r&amp;bz=0&amp;pgls=CAEaBTcuMC4x~CAEQBBoHMS4xODIuMA..&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 動態工作流程在做什麼

動態工作流程的核心，是執行一支含有特別 API 的 JavaScript，它可以在程式裡開出和協調不同的 subagents，去做各自的工作、互相驗證、再彙總結果

以下整理幾個關鍵點分享給大家：

- workflow 本身就是一個 JS 腳本，旁邊有內建的工具 (JSON、Math、Array...等)方便處理資料
- 它可以決定這個 subagent 要用哪一個模型？以及要不要在獨立的 worktree 中跑 (也就是所謂的上下文隔離)
- 如果 workflow 執行中被中斷 (例如關掉 terminal)，下次 resume session 後，一樣可以從中斷的地方接著跑

所以我們可以把它想成是用 JS 寫一個任務流程引擎，裡面由很多個 Claude 分身在不同上下文裡各司其職

![JS 工作流程連接代理、模型、隔離與驗證的手繪配圖](https://muki.tw/wordpress/wp-content/uploads/2026/06/dynamic-workflows-figure-2.png)

JS 工作流程連接代理、模型、隔離與驗證的手繪配圖

## 為什麼需要動態的工作流程？

如果我們只用預設的 Claude Code harness，通常是一個 context window 內同時負責規劃 + 執行，這對一般 coding 任務很好用，但一旦遇到以下情況：

- 任務超長、需要反覆嘗試
- 高度平行 (上百、上千小項目)
- 結構非常複雜、還有 adversarial 驗證、比對之類

就會慢慢撞到幾個典型的失敗模式：

1. 懶人上身 (Agentic laziness)：任務太大、太多步，Claude 會在部分進展後提早說完成了。例如安全檢查該看 50 項，只看了 20 項就收工
2. 自我偏好偏差 (Self-preferential bias)：要它檢查自己產出的東西時，很容易偏袒自己原本的結論。尤其是請它照 rubric 評分、自我驗證時，容易高估自己的方案
3. 偏離目標 (Goal drift)：如果碰到很長的對話，Claude Code 會在每次壓縮、整理、總結時，都會掉一些細節。極端情況下，可能只記得大方向，所以像很多邊界條件、或千萬不要做 X 的設定都會消失

動態工作流程透過 **多個分離的 Claude + 多個 context window + 專注的小目標** 來對付這些問題

我們可以把它想成，我們不再是呼叫一個 Claude 做完所有事情，而是讓很多 subagents 各自扮演不同角色來產生、驗證、對抗、比稿、整理... 等

![切開單一視窗以避免偷懶偏袒與飄移的手繪配圖](https://muki.tw/wordpress/wp-content/uploads/2026/06/dynamic-workflows-figure-3.png)

切開單一視窗以避免偷懶偏袒與飄移的手繪配圖

## 常見的 workflow pattern

文章裡整理了幾個非常實用的工作模式，理解這些模式後，我們就能任意組合，也能透過提示詞精準地告訴 Claude 要做怎樣的 workflow

1. Classify-and-act (分類後再行動)：  
	先請一個分類用的 agent 判斷 **這是哪一種任務** ？再依照不同任務種類，把工作分派給對應的 agent 或策略  
	但我們也可以跑完整個流程，最後才讓這個分類的 agent 來判斷輸出的格式  
	意思就是可以分類後再行動，也可以行動後再分類，取決於你
2. Fan-out-and-synthesize (先分開再合併)：  
	把一個大任務拆成很多小步驟，每一步各交給一個 agent，在它自己的乾淨 context 裡單獨跑完，最後再做一個彙總用的 agent 把所有結構化結果整理合併  
	這應該是熟悉 sub agent 的朋友很常做的事情了吧，這特別適合在 **有上百個小項目** 或 **每一步都該獨立思考** 的情境
3. Adversarial verification (對抗式驗證)：  
	每開一個負責產出的 agent，就再開一個專門挑錯的 agent，根據既定的 rubric 或標準，嚴格檢查甚至刻意攻擊前者的輸出
4. Generate-and-filter (產生＋篩選)：  
	先大量生成各種想法，再用 rubric 或 verifier 進行篩選與去重，最後只留下品質最好、經過檢驗的那一批
5. Tournament (錦標賽)：  
	讓多個 agent 同時解同一個問題，再用一個評審 agent 做比較與進行淘汰，到最後產生的冠軍就是最佳解
6. Loop until done (做到真的沒東西為止)：  
	這是我現在很愛用的一個工作方式，以我為例，我會告訴 Claude Code 我的目標是什麼，但不預先設定迭代次數，讓 workflow 自己去規劃、持續產生新的 agents 去處理，直到滿足我的目標為止
![](https://muki.tw/wordpress/wp-content/uploads/2026/06/CleanShot-2026-06-05-at-11.55.52@2x.png)

## 實際用法分享

原文列了很多例子，整理幾個我覺得比較有代表性的與大家分享

### 1\. 大型 Migration / Refactor

例如 Bun 把核心從 Zig 改寫成 Rust，就是用 workflows 幫忙完成的案例

做法大概是：

- 先把整個任務拆成 callsite、失敗測試、module…… 等多個維度
- 每個 fix 開一個 subagent，在自己的 worktree 裡做修改
- 另一個 agent 負責 adversarial review (找問題、比對規格)
- 通過後再 merge
- 同時你可以限制它不要亂用很吃資源的指令，以便在你的機器資源裡最大化平行度

### 2\. Deep Research

Claude Code 內建的 `/deep-research` 技能本身就是一個動態 workflow，以下是他的流程：

- 同時展開多個網路搜尋
- 做一個驗證 agent 檢查來源品質，並進行對抗式查核
- 最後彙整成一份附有引用來源的報告

同樣的架構可以用在以下案例：

- 從 Slack 或各個平台抽出一段期間的狀態報告
- 深入探索某個 feature 在 codebase 裡到底怎麼實作

### 3\. Deep Verification (全量事實查核)

如果你有一份長報告，希望每一句技術或事實敘述都有來源可參考，就可以使用這樣的流程：

- 先整理出報告裡所有需要查證的事實陳述
- 針對每一條陳述，分別派出獨立代理去找來源佐證
- 再由另一組驗證代理檢查來源品質，確認證據是否可靠

### 4\. 大量項目排序

當你有上千筆項目，想依某個定性指標排序 (例如 bug 嚴重程度)，如果直接打開 Claude Code 請他處理，可能會塞爆你的上下文，導致品質下降

如果改用 workflow，可能的做法與流程如下：

- 使用錦標賽 (tournament) 或成對比較 (pairwise comparison) 的流程來排序
- 也可以先做平行分組排序 (parallel bucket ranking)，再把各組結果合併
- 每一次比較都交給一個 agent 處理，迴圈只保存目前的對戰表，而上下文只放當前這一組比較資訊

如此一來，兩兩比較就會比一次性的打分可靠很多

### 5\. 遵守 Memory 和 Rule

使用 AI 最大的一個問題，就是即使我們把一堆規則寫進 `CLAUDE.md` ，Claude 還是會時不時會抽風忘記

Thariq 提出一個很酷的解法，我們可以建立一個 workflow，把這些規則變成 **必須檢查的清單** ，一條規則就交給一個 verifier agent 專門負責，然後再配一個懷疑論者的 persona 來審查這些規則本身，避免可能的誤判

所以往反方向思考，我們也可以去深挖最近的 session、code review 中，我們常常糾正 Claude 的地方，然後用平行代理把這些問題分類，找出反覆出現的錯誤模式

接著再用對抗式驗證逐條檢查，如果當時有這條規則是不是真的能防止那個錯誤？最後，只把通過驗證、確實有幫助的規則整理回 CLAUDE.md，讓它變成下一次工作時會被持續遵守的操作規範

### 6\. 根因調查

除錯時，本來就應該同時保留多個獨立假說，再逐步驗證。但如果所有判斷都放在同一個上下文 (context) 裡，Claude 很容易黏著第一個看起來合理的假說，所以我們可以透過 workflow 進行如下拆解：

- 針對不同證據來源分派獨立的 agent，例如日誌、檔案與資料
- 讓每個代理各自提出可能的根因假說
- 再交給驗證代理與反駁小組 (verifier / refuter panel)，逐一檢驗每個假說

同樣的結構也可以套用在這些問題上：

- 為什麼三月銷售下滑？
- 為什麼這條資料管線故障？
- 任何需要事後檢討的分析。

### 7\. 大規模的分類診斷與排程

沒有 AI 前，團隊每天幾乎的有處理不完的支援、錯誤回報與待辦，可以試著使用以下分診流程協助節省時間：

- 先替每一筆項目分類
- 移除重複的項目
- 判斷下一步動作：嘗試修復，或升級給真人處理

Thariq 還特別提到了隔離模式，參考如下：

- 負責閱讀不可信外部內容的 agent 的權限要受控管，不能執行高風險操作
- 真正需要高權限的行動，交給另一組專門負責執行的代理

最後再搭配 `/loop` 指令，就能讓這個流程長期運轉

### 8\. 任務級評測

如果我們給某一類的任務寫了一個技能或提示詞，想針對這部分做簡單的評測時，也可以用動態工作流程來執行：

- 在獨立的 worktree 啟動多個 agent，分別產生輸出
- 再啟動比較 agent，依照評審的標準來給分與比較

如此一來，就可以反覆調整與優化技能

## 什麼時候不要用動態工作流程

乍看之下，動態工作流程很強。但大家都知道，事情總是有正反兩面，動態工作流程他不是萬靈丹，而且最大的缺點就是很耗 token

如果你是要做以下的任務，那你可能就不適合使用動態的工作流程：

- 一般、日常的 coding 任務，不需要一次開 5 個、甚至多個 reviewer 來幫你改一個小函式
- 問題本身很小、上下文單純
- 對需求還模糊，只是想隨便試試看，這時使用 workflow 反而會把模糊的東西放大，沒有什麼意義

比較好的使用方式是把 workflows 當成 **推進 Claude Code 到以前做不到的新用法** 的工具，而不是每一件小事都直上 workflow

![判斷長任務才啟動動態工作流程的手繪配圖](https://muki.tw/wordpress/wp-content/uploads/2026/06/dynamic-workflows-figure-6.png)

判斷長任務才啟動動態工作流程的手繪配圖

## 小結與實務建議

最後整理原文內給的一些實務建議：

- 動態工作流程很吃 **具體加結構化的提示詞** ，建議大家明確表達出想要使用的 pattern (例如： **錦標賽式比較 + 對抗式驗證 + 持續迭代直到完成**)，會讓 workflow 少走一些彎路
- 它不是只能做超大任務，你也可以叫 Claude 開一個快速的 workflow，例如只做一輪的對某個假設的對抗式檢查
- 搭配 `/goal` 和 `/loop` 效果會更好
- 可以明確表達 token 的預算，例如這個 workflow 請控制在 10k tokens 內，讓 Claude 幫你做計劃與限制
- 在 workflow menu 按 `s` 可以存檔，會放到 `~/.claude/workflows` 或當成 skill 發布。放進 skill 時，可以提示 Claude 把它們視為模板，而不是一行不動地照抄腳本

我覺得動態工作流程並不是一個什麼新的指令、或新的技術，早在去年我就有用自己做的排程軟體 [Orbit Agents](https://mukiwu.github.io/orbit-agents/) 在做文章提到的「大規模的分類診斷與排程」這樣的 workflow 了。

只是現在官方有了更好的處理方式，讓 Claude 自己給每個任務寫 workflow，所以能真正解決 Claude 本身的問題，例如長任務的工作倦怠、懶惰、偏見、上下文視窗的限制... 等等

如果你本身就有在用 Claude Code 寫程式、做研究、做分診或是迴圈... 的人，可以試用看看 workflow，也許能解決你的各種困擾

muki@journal ~/AI 應用 $ 0