---
title: "CLAUDE.md — 讓 Claude 跨對話記得你的專案，不用每次重講 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10400239"
author:
  - "[[iThome]]"
published:
created: 2026-06-22
description: "這是「Claude Code 實戰手冊」系列的第三篇(G3)。上一篇講了用量與計費，結尾提到「用 CLAUDE.md 避免重複代」這招，這篇就完整拆解它:CLAUDE.md 是什麼、怎麼寫、為什麼它能..."
tags:
  - "clippings"
status: processed
---
0

## CLAUDE.md — 讓 Claude 跨對話記得你的專案，不用每次重講

- 分享至

這是「Claude Code 實戰手冊」系列的第三篇(G3)。上一篇講了用量與計費，結尾提到「用 CLAUDE.md 避免重複代」這招，這篇就完整拆解它:CLAUDE.md 是什麼、怎麼寫、為什麼它能同時讓 Claude 更懂你的專案又更省 token。

1. 換一個對話，它又得把整個專案重新摸一遍  
	你大概遇過這個情況。

昨天你跟 Claude Code 把一個專案弄得好好的，它摸熟了你用的是 Vue、知道你的命名習慣、知道哪幾個檔案不能亂動。結果今天你開一個新的對話，問它一個問題，你會發現，它得從頭再把這個專案認識一次。

倒不是說它會亂猜、會出錯(現在的 Claude Code 沒這資訊也不會瞎掰)。而是:它為了搞懂「這到底是什麼專案」，會自己去翻整個資料夾、一個一個讀檔案、搜尋它需要的東西 —— 把昨天已經摸熟的事，今天再摸索一遍。

問題就在這個「重新摸索」:

每翻一個檔、每讀一段程式碼，都在燒 token(也花時間)。 G2 講過，白板上字越多越貴 —— 它翻找專案讀進來的那些內容，全都堆上白板。所以同一個專案，你開十個新對話，它就把資料夾重新翻十遍，那個成本是重複付的。

於是你常常會忍不住，乾脆自己先打一段:「這是一個 Vue 專案、用 yarn 不是 npm、components 命名要小駝峰、那個 legacy 資料夾不要動……」想幫它省點摸索。

但這段話 —— 每。次。新。對。話。你。都。要。重。打。

會這樣是因為 G2 講過的「白板」 —— 每一輪對話都是一塊乾淨的白板，/clear 或開新對話，白板就擦掉了。它沒有真正的長期記憶，昨天摸熟的、你昨天交代的，今天這塊白板上通通沒有。

那有沒有辦法，讓那些「摸過一次就該一直記得」的專案資訊，不用每次重新摸、也不用你每次重打?

有。這就是這篇的主角:一個叫 CLAUDE.md 的檔案。

2. CLAUDE.md 是什麼:專案的「常駐說明書」  
	一句話:

CLAUDE.md 是一個放在你專案資料夾裡的純文字檔，Claude Code 每次開始對話，都會自動先讀它一遍。

你可以把它想成你貼在專案門口的一張說明書 + 地圖:「進來工作前先看這個:這是什麼專案、用什麼技術、有什麼規矩、東西大概放在哪。」每個進來的 Claude(每一塊新白板)都會先讀過，才開始幹活。

回到第 1 節那兩個成本，CLAUDE.md 正好一次省掉兩個:

省掉「它重新摸索」的成本:你把專案資訊先記在這份檔案裡，等於先給了它一張地圖。它不用再盲目翻整個資料夾去搞懂這是什麼專案，一開場就帶著地圖，少翻很多不相關的東西。  
省掉「你重打」的成本:寫一次，之後每個對話自動載入，你不用再每次開口重講那段說明。  
所以它一上工就帶著專案的全貌，知道去哪找、什麼別碰，不用瞎摸;你也省下每次重打那段說明的力氣。它少翻很多檔、你少打很多字，省下的全是 token —— 這就是 G2 結尾說的「更懂你的專案，又更省用量」。

![https://ithelp.ithome.com.tw/upload/images/20260620/201827963rUYueZZol.png](https://ithelp.ithome.com.tw/upload/images/20260620/201827963rUYueZZol.png)

3. 怎麼生出第一版:/init(老實說，這是「懶得自己寫」的起手式)  
	聽到「要寫一個檔案」，新手通常會卡住:「我哪知道裡面要寫什麼?」

好消息是，你不用從零寫。在你的專案資料夾裡打 /init，Claude 會自己掃一遍你的專案 —— 看你用什麼語言、什麼框架、怎麼跑起來、有什麼慣例 —— 然後幫你生出第一版 CLAUDE.md。你打開來會看到它已經幫你填好「這是什麼專案、怎麼安裝、怎麼測試」這些基本資訊。

![https://ithelp.ithome.com.tw/upload/images/20260620/20182796oZKtSqBRAs.png](https://ithelp.ithome.com.tw/upload/images/20260620/20182796oZKtSqBRAs.png)

![https://ithelp.ithome.com.tw/upload/images/20260620/20182796LRLfGhu6mV.png](https://ithelp.ithome.com.tw/upload/images/20260620/20182796LRLfGhu6mV.png)

老實講一件事:/init 不是每次新專案的必做儀式。 我自己用久了，很多時候根本沒打 /init，直接開對話開始做，Claude 也做得好好的 —— 因為我會邊做邊補。

但對剛開始的你，/init 是最快的起手式:它幫你生一個「不知道怎麼下手」的初稿，你再改。比起對著空白檔案發呆，讓它先掃一版出來、你再修快太多了。

(小提醒:如果你的專案已經有 CLAUDE.md，/init 不會直接蓋掉它，而是看過後建議你可以補哪些。所以重跑也不怕。)

4. 裡面該寫什麼、不該寫什麼  
	/init 生出來的只是初稿，真正讓它好用的，是你接下來怎麼修。

判斷標準只有一條:

「每次開新對話都得跟它講一遍的東西」，才值得寫進 CLAUDE.md。

該寫(這些每次都該講,所以常駐最划算)  
這是什麼專案 —— 一兩句話，讓它有個全貌。  
技術棧 —— 用什麼語言、框架、套件管理器(yarn 還是 npm?你不寫明，它只能自己去推，推一遍也是花時間)。  
怎麼跑起來 —— 安裝、啟動、測試的指令。  
團隊規矩 / 慣例 —— 命名風格、commit 訊息格式、資料夾結構的約定。  
不要碰的地方 —— 哪些檔案/資料夾是 legacy、不要動、不要重構。  
不該寫(寫了反而拖累)  
會一直變的細節 —— 今天的進度、這次要修的 bug。這種「當下狀態」不該放常駐檔(這牽涉到另一個工作流，結尾會講)。  
落落長的小說 —— CLAUDE.md 每次對話都會被讀進白板，它本身也佔 token。寫越長，每次對話的固定開銷越大。精簡講重點就好，不要寫成一篇作文。  
一次性的東西 —— 只用一次的指示，直接在對話裡講就好不用進檔案。  
CLAUDE.md 是「規矩」，不是「日記」。放那些穩定、會重複用到的事；會變的、一次性的，留在對話裡。

5. 四種 CLAUDE.md:放哪裡，決定誰會讀到它  
	這是這篇唯一稍微進階一點的地方，但值得花一分鐘搞懂，因為同一份規矩，放錯位置就是不會生效。

CLAUDE.md 其實有四個層級，Claude 會由上而下全部讀進來、疊在一起用:

層級 放哪裡 給誰用 會進 git 嗎  
組織政策 系統層級路徑(依作業系統 / 企業部署而定) 整間公司統一規範 —  
個人全域 ~/.claude/CLAUDE.md 你自己，跨所有專案 不會(在你家目錄)  
專案./CLAUDE.md(專案根目錄) 這個專案的所有人(團隊共用) ✅ 會，該進版控  
個人 + 專案./CLAUDE.local.md 只有你，且只在這個專案 ❌ 不進，要加進.gitignore  
新手只要先抓兩個最常用的對比:

./CLAUDE.md(專案層) —— 「這個專案的規矩」，團隊共用。你把它 commit 進 git，同事 clone 下來他的 Claude 也讀得到同一份規矩，整組人的 Claude 共享同一套專案知識。  
~/.claude/CLAUDE.md(個人全域) —— 「我這個人不管做哪個專案都想要的習慣」，例如「回我中文」「commit 用某種格式」。它跟著你，不跟著專案。  
至於 CLAUDE.local.md —— 當你有「只想自己用、又不想讓同事看到」的偏好(例如你個人的測試帳號、你習慣的暫存路徑)，放這裡並記得加進.gitignore 免得不小心 commit 出去。

進階註記(現在不用管):當你的專案規矩越長越多，可以把它們按主題拆成./.claude/rules/ 資料夾裡的多個小檔，甚至設定「只有在處理某類檔案時才載入某條規則」。這是團隊規模大了之後的整理術。現在你一個 CLAUDE.md 就夠了，知道有這條路就行。

![https://ithelp.ithome.com.tw/upload/images/20260620/20182796jlrTOy1lYX.png](https://ithelp.ithome.com.tw/upload/images/20260620/20182796jlrTOy1lYX.png)

6. 怎麼讓它越來越準:/memory 與「直接叫它記住」  
	CLAUDE.md 不是寫完一次就結束的。最好的用法是邊用邊長。

隨時打開來編輯:/memory  
在對話中打 /memory，它會列出目前這段對話載入了哪些記憶檔(專案的、個人的、local 的全列出來)，點任何一個就能打開編輯。你也可以直接用編輯器打開 CLAUDE.md 改 —— 兩種都行，/memory 只是讓你不用自己找檔案在哪。

官方推薦的起步方式就是這樣:用 /init 生初稿，自己編輯補上慣例，之後用 /memory 隨時調。新手照這個做就夠了。

對話中發現「它早該知道」:直接用講的  
還有一個更輕鬆的方式。當你在對話中發現「這件事 Claude 早該知道，害我又解釋一遍」，你可以直接用自然語言叫它記住，例如:

「這個專案一律用 yarn，不要用 npm。」 「跑測試前要先開本機的 Redis。」

Claude 會把這件事存進它的記憶。

⚠️ 這裡有個分界，免得你搞混:你「用講的」叫它記住，存進去的其實是另一套「自動記憶系統」，跟你手寫的 CLAUDE.md 是兩個不同的機制。 簡單說:CLAUDE.md 是你親手寫的規矩；自動記憶是Claude 自己幫你記的筆記。這兩個怎麼分工、各自適合放什麼 —— 就是下一篇 G4 的主題。這篇你先知道有「用講的」這個捷徑就好。

7. 本課單字:讓專案記得你的指令  
	延續系列慣例，這篇收三個跟「專案記憶」有關的指令:

/init  
在專案裡掃一遍、自動生出第一版 CLAUDE.md。不知道怎麼開始寫時的起手式 —— 讓它先生草稿你再改。

/memory  
列出目前載入了哪些記憶檔(專案 / 個人 / local)，點開即可編輯。想調整 CLAUDE.md 又懶得找檔案在哪時用它。

/clear(複習 G1)  
清空白板、開一個乾淨的新對話。為什麼這裡複習它？因為 /clear 之後對話歷史沒了，但 CLAUDE.md 會自動重新載入 —— 這正是 CLAUDE.md 的價值:對話可以隨時清，專案的規矩不會跟著消失。

8. 給新手的三個提醒  
	a. 先有，再求好  
	別追求一次寫出完美的 CLAUDE.md。先 /init 生一版、跑跑看，發現它哪裡沒摸到、又要你重新解釋一遍，就把那件事補進去。它是長出來的，不是設計出來的。

b. 精簡比完整重要  
記住它每次對話都會被讀進白板、佔 token。寧可短而準，不要長而全。 真正每次都用到的才寫，其他別塞。

c. 分清楚「規矩」和「進度」  
CLAUDE.md 放穩定的規矩(技術棧、慣例、別碰的地方)。「這次做到哪、剛剛決定了什麼」這種當下進度，不要放這裡 —— 它會過期，而且每次對話都重讀一份過期資訊很浪費。那這種進度要怎麼傳？下面結語講。

結語  
CLAUDE.md 解決的，是新手一個很實際的疲勞:每換一個對話，就要把專案重講一遍。

把那些「講過一次就該一直記得」的事 —— 技術棧、慣例、別碰的地方 —— 寫進這個檔案，放對層級之後每個對話它都自動帶著上工。你少打了很多重複的字，它也少翻很多沒必要的東西。

不過 CLAUDE.md 有它的極限:它適合放穩定的規矩，不適合放\*\*「這次做到哪」的當下進度\*\*。

那如果我做到一半，白板快滿了不得不換一個對話 —— 我要怎麼把白板上「剛剛做到哪、決定了什麼」這些東西，整包傳給下一個對話、甚至下一個 agent?

這是我自己工作流裡很常用的一招:讓 Claude 產生一份交接檔,傳給下一棒。 那是 G5 的主題，到時候細講。

下一篇 G4 — Memory 系統:讓 Claude 跨「專案」記得你。這篇第 6 節提到「用講的叫它記住，存進的是另一套自動記憶」 —— G4 就會把 CLAUDE.md(你手寫的專案規矩)和自動記憶(Claude 自己幫你記的筆記)這兩套機制徹底分清楚，告訴你什麼該寫進哪裡。

系列文章  
G1 Claude Code 從零開始 — Portfolio v2 重做  
G2 用量與計費 — 搞懂怎麼收費，再決定要不要省  
G3(本篇) CLAUDE.md — 讓 Claude 跨對話記得你的專案  
G4 Memory 系統 — 讓 Claude 跨 session 記得你(即將發布)  
G5 換 section 不斷線 — 把當下進度交接給下一個對話 / agent(規劃中)  
完整 20 篇地圖會放在系列首頁。本系列同步發佈於 Medium。

關於我

Tim Wei / AI/ML Engineer @ EY Taiwan，專注於 Responsible AI、金融業 ML 落地。也在台大擔任 AI 應用的企業導師。

歡迎在留言區跟我討論，或在 LinkedIn 上找我。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)

[![.](https://itadstatic.ithome.com.tw/B9/1781687778_6a3265e272d9c.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9004)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/15f864ef-9fa8-478c-92b7-d808d8b79b9e_small.jpg)

[數位銀行詐欺與銀行面臨的漏洞所帶來的挑戰以及 AI 如何幫助](https://itplus.ithome.com.tw/vod-page/368?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/5539ef0e-d758-437f-94d2-10c18df8cf1a_small.png)

[自動化監控網站運行服務 – Gatus](https://itplus.ithome.com.tw/vod-page/561?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/5/bc80c3a2-2af3-4056-9418-ce81ef6a8ff9_small.png)

[混合式多雲中暢行無阻](https://itplus.ithome.com.tw/vod-page/880?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/7/24/cdc09a6a-9b57-47e5-b4e9-7659f171ba07_small.png)

[跨雲安全威脅解析，AI 驅動雲端安全策略與資安防護全攻略！](https://itplus.ithome.com.tw/vod-page/1212?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

31 分