---
title: "AI Agent 的介面為什麼像通訊軟體？拆解 Grok Bot 的 Bot 名單、頭像狀態與雲端電腦"
source: "https://rar.design/posts/grok-bot-agent-interface-design"
author:
  - "[[設計師 Riven]]"
published: 2026-09-24
created: 2026-09-24
description: "Grok Bot 是 SpaceXAI 在 2026 年 8 月推出的常駐 AI 代理：每個 Bot 有名字、記憶和一台雲端電腦，會登入你的工具把工作做完，只在需要核准時回來找你。本文整理功能、方案與風險，並拆解官方設計文裡 Bot 名單、頭像狀態與三層電腦視窗的設計決策。"
tags:
  - "clippings"
status: processed
---
從功能、方案到官方設計文，一次看懂 Grok Bot 能做什麼、要付多少、介面為什麼這樣設計

![設計師 Riven](https://s.teachifycdn.com/image/width=128,quality=80,format=webp/lecturer/avatar/fd0b78a9-03c9-4ad3-ae0a-a8965aa676ea/d822e5e9-519b-4f2e-b049-807272104edd.png)

[設計師 Riven](https://rar.design/@riven)

2026年9月24日 上午 2:32

UI/UX 設計

AI AGENT · INTERFACE DESIGN · 2026

![Grok Bot 官方發表主視覺：藍色漸層背景上的白色圓形 Bot 頭像與 Grok Bot 字樣](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/6b54d9ea-e23f-4c2b-b429-2ebfa2c19d53/372305ca-b2c3-492b-9165-4e54cdccc768.jpg)

Grok Bot 官方發表主視覺：藍色漸層背景上的白色圓形 Bot 頭像與 Grok Bot 字樣

▲ SpaceXAI 在 2026 年 8 月 11 日推出 Grok Bot，官方發表主視覺

QUICK ANSWER

Grok Bot 是 SpaceXAI 在 2026 年 8 月 11 日推出的常駐 AI 代理。每個 Bot 有名字、記憶和一台雲端電腦，會登入你的工具把工作做完，需要核准才回來找你。付費 Cursor 方案與 SuperGrok 都能用，用量每週重置。它的介面拿掉聊天紀錄，改成一排有名字的同事。

SpaceXAI 營運部有位員工，剛開始用 Grok Bot 時每十五分鐘就去問一次進度，問到最後，Bot 反過來問她為什麼一直問。

這段話出自官方的發表文，原本是要證明產品有多好上手。我讀到這裡停了很久，因為它剛好說中代理工具真正的設計題目：軟體開始自己把事情做完之後，介面要處理的就是人該看多少、什麼時候看、什麼時候放手。

Grok Bot 是 SpaceXAI（xAI 併入 SpaceX 之後的新名字）8 月 11 日推出的常駐代理。昨晚彭博社報導，它上線大約一個月，每週使用者就超過 41 萬人，單週成長 24%。這個月初，官方設計團隊發了一篇長文，把 Bot 名單、頭像、電腦視窗背後的取捨一條條寫出來，是我今年讀過最完整的代理介面設計紀錄。

我把官方公告、整套說明文件、方案頁和那篇設計文讀完，整理成這篇。前半回答實際的問題：它能做什麼、要付多少、哪裡要小心；後半拆解那篇設計文，裡面有幾個決定，做 AI 產品或任何「會自己動的介面」的設計師，都可以直接帶走。

CONTENTS

01

一台雲端電腦，一排有名字的同事

02

交辦一件工作的五個步驟

03

要付多少錢：跟著 Cursor 或 SuperGrok 走

04

放手之前：共用電腦與核准邊界

05

官方設計文拆解：三個決定、一個代價

06

設計師可以怎麼用、可以帶走什麼

07

重點整理

CHAPTER 01 · WHAT IT IS

## 一台雲端電腦，一排有名字的同事

先講它跟一般聊天機器人最大的差別：Grok Bot 的每個 Bot 都在一台常駐的雲端電腦上工作，上面有瀏覽器、檔案系統和終端機。你交代的事會在那台電腦上做完，成果直接放進你平常用的工具，像寄件匣裡的草稿、CRM 裡更新好的紀錄、雲端硬碟裡的檔案。你闔上筆電，它照樣繼續做。

官方產品團隊的說法是，大部分 AI 只幫你做到九成，差的那一成，是把成果放進真正的工具裡。Grok Bot 想補的就是這一成。

它登入工具的方式跟你一樣。有連接器或 [MCP](https://rar.design/posts/mcp-model-context-protocol-guide) 伺服器的服務，走結構化的串接；沒有的就直接開瀏覽器操作，連沒有像樣 API 的廠商後台、老舊的營運系統都能用。碰到密碼、雙重驗證或付款確認，Bot 會停下來把畫面交給你，由你自己輸入。

你可以開好幾個 Bot，每個都有自己的名字、頭像、職稱和記憶。SpaceXAI 內部常見的用法，是一個「幕僚長」Bot 帶幾個專員，收件匣、報帳、招募、修 bug 各一個；它們能互相傳訊、在群組聊天裡交接，只在需要判斷時拉你進來。

24/7

ALWAYS ON

全天候運作

50

BOTS / ACCOUNT

每個帳號約上限

6

BOTS / GROUP

每個群組聊天上限

1

SHARED COMPUTER

所有 Bot 共用一台

背景也交代一下。SpaceX 今年二月收購 xAI，公司之後改名 SpaceXAI，寫程式工具 Cursor 現在也是 SpaceXAI 旗下的公司。所以 Grok Bot 的下載、登入和計費都走 Cursor 帳號，下一章的方案規則也跟著 Cursor 走。

CHAPTER 02 · HOW IT WORKS

## 交辦一件工作的五個步驟

Grok Bot 上手的方式很像帶新同事。官方文件建議，第一件交辦的事最好橫跨兩三個工具、有明確的完成標準，而且先從讀資料、準備草稿開始，看過成果，再慢慢開放寄送或修改的權限。

1

交辦：像傳訊息給同事

不用先設定流程。講清楚要做什麼、去哪裡做、參考哪些資料、做完長什麼樣子，Bot 需要權限時會主動提出。

2

登入：在它的電腦上登入一次

瀏覽器的登入狀態會保留，下次不用重登。有連接器的服務從 Marketplace 安裝，對話裡打 @ 就能掛到這次的任務上。

3

分工：一個角色一個 Bot

每個 Bot 有自己的記憶和排程。兩到六個 Bot 可以放進同一個群組聊天，互相交接、分配由誰負責。

4

示範：做一次給它看

開啟 Teach a task 實際操作一遍，最長十分鐘、不錄麥克風，它會整理成一份技能草稿。這個功能正在逐步開放，流程穩定後再設成定時或事件觸發的 routine。

5

核准：該你拍板的留給你

寄信、發布、付款、刪除都可以設成一律先問你。核准卡會列出要做的動作和內容，你可以允許一次、拒絕，或存成規則。

第四步我會多看一眼。它跟 Claude Cowork 的 [Record a skill](https://rar.design/posts/claude-cowork-record-a-skill-guide) 是同一個方向：錄一次操作，AI 就能重跑。差別在 Grok Bot 把兩件事拆開放，錄下來的流程存成技能，所有 Bot 都能用；什麼時候跑、由誰負責，則掛在某一個 Bot 身上，變成它的固定職責。這個拆法在第五章還會再出現。

官方的發表影片不到兩分鐘，可以先看一次整體的樣子。

![](https://www.youtube.com/watch?v=F1_0Lkp16Rc)

CHAPTER 03 · PRICING

## 要付多少錢：跟著 Cursor 或 SuperGrok 走

Grok Bot 沒有單獨的訂閱。每一個付費的 Cursor 個人方案和 Cursor Teams 都內含，也可以連結個人的 SuperGrok 或 X Premium+ 帳號取得用量。8 月 11 日剛推出時只開給最高階的幾個方案，8 月 26 日才擴大到 SuperGrok、Cursor Pro 和所有 Teams 方案，網路上「要訂最貴的方案才能用」的說法已經過期了。

ACCESS · 取得方式

Grok Bot 用量與限制

Cursor Pro  
每月 20 美元

含每週用量，是 Cursor 最便宜的付費方案

Cursor Pro+  
每月 60 美元

每週用量高於 Pro

Cursor Ultra  
每月 200 美元

每週用量最高

Cursor Teams  
自助方案

每位成員都能用，依座位額度計算，不必另開 Premium 座位

連結 SuperGrok  
含 Plus、Heavy

依等級給用量，Heavy 最高；連結後不能解除，也不能轉到別的帳號

連結 X Premium+

用量低於 SuperGrok Plus

免費試用

一筆用量額度加 7 天期限，依代理步驟與 token 扣

不含

SuperGrok Lite 不含；SuperGrok Team 與 Enterprise 帳號不能連結

有三條規則最容易踩雷。第一，用量每週重置，用完之後如果開了隨用隨付（on-demand），會改由 Cursor 帳單計費，上限是你設定的每月額度；這個上限不會在任務跑到一半時硬切，正在跑的 Bot 可以超過上限把事情做完。

第二，Cursor 方案和 SuperGrok 連結不會疊加，兩邊都付錢，用量也不會變多。第三，SuperGrok 連結一建立就不能解除，也不能搬到別的 Cursor 帳號，連結前先確認登入的是對的帳號。

免費試用的算法也跟一般軟體不同。它是一筆用量額度外加七天期限，依 Bot 實際跑了多少步驟和 token 扣，跟你傳幾則訊息無關，一個大任務就可能一次用完，用掉的也不會補回。先交辦範圍小、可以驗收的任務，看清楚一件事大概花多少，再決定要不要付費。

平台方面，桌面版支援 macOS、Windows 和 Linux，行動版支援 iPhone、iPad 和 Android，同一個 Bot 在手機和電腦上是同一串對話。台灣的 App Store 已經上架，可以免費下載，登入後再依你的方案取得用量。

CHAPTER 04 · LIMITS

## 放手之前：共用電腦與核准邊界

Grok Bot 的安全文件寫得比我預期的坦白。最重要的一句是：你所有的 Bot 共用同一台雲端電腦，檔案、瀏覽器登入狀態和指令列憑證全部互通，官方明講不要把不同的 Bot 當成安全邊界。你替報帳 Bot 登入的記帳系統，行銷 Bot 一樣進得去。

KEY RISK · 共用電腦

Bot 之間沒有隔離。刪掉一個 Bot，也不會清掉它在共用電腦上留下的檔案和登入狀態；專案結束後要自己登出網站、刪掉工作檔，並到原服務撤銷連接器授權。

APPROVAL · 核准只管下一步

核准控制的是接下來要做的那個動作，已經完成的事不會因此復原。所以邊界要在交辦時寫清楚，例如「整理數據、提出預算調整建議，不要改廣告設定，也不要聯絡代理商」。

AUTO REVIEW · 規則越窄越好

自動審查分成「一律先問我」和「自動允許」兩種規則，衝突時以先問我為準。它本身是模型判斷，只能輔助最小權限原則，「瀏覽器裡一律允許」這種寬鬆規則不要設。

還有兩個設定要知道。Bot 要在你自己的電腦上執行指令時，預設每次都會先問你，官方建議直接設成「永不允許」，除非真的需要它碰本機檔案。另外，Grok Bot 需要開啟資料儲存，不支援 Cursor 的舊版隱私模式（Legacy Privacy Mode），處理客戶資料的團隊，導入前先讀過 Cursor 的隱私政策。

產品本身也還在 beta。第三方評測整理上線頭幾週的論壇回報，最常見的抱怨是重度使用者幾天就用完每週額度，以及瀏覽器層會當掉；8 月 20 日到 21 日之間，也有用戶回報手上所有 Bot 都無法運作。以剛上線六週的產品來說不算意外，但交辦重要工作前，先拿低風險的任務跑幾輪。

CHAPTER 05 · DESIGN

## 官方設計文拆解：三個決定、一個代價

9 月 3 日，SpaceXAI 發了一篇〈Designing Grok Bot for a world of persistent agents〉，如果你只打算讀一篇官方文章，我推薦這篇。它的出發點是：現在大部分的 AI 介面，都是圍著一段由使用者操作的對話設計的，開頭要設定、過程要盯著，對話停了工作就結束。他們想做的代理會跨越每一段對話持續存在、自己扛責任，所以介面裡最基本的物件都得重想一遍。

我把整篇讀完，把裡面的取捨整理成三個決定，外加一個我認為還沒解決的代價。

### 決定一：把主角從對話換成 Bot

他們先把 AI 產品這幾年累積的詞彙列出來：對話、session、模型、上下文長度、記憶、系統提示、專案、技能、連接器、代理、工具、沙盒、權限、自動化。每一個都對應系統裡真實存在的東西，但全部做成獨立的產品概念，等於要求使用者懂得比他需要的更多。最後他們只留下五個。

BEFORE · 14 個名詞

Chats · Sessions · Models · Context windows · Memories · System prompts · Projects · Skills · Connectors · Agents · Tools · Sandboxes · Permissions · Automations

AFTER · 5 個物件

Bots

有身分、記憶、執行環境與工具的常駐代理

Chats

跟 Bot 一起工作的對話介面

Prompts

給 Bot 的脈絡或指令，可以只用一次、存成技能，或設成自動觸發的 routine

Tools

透過軟體、API、連接器、終端機或電腦操作，取得資訊、採取行動

Artifacts

Bot 產出或修改的文件、設計、程式碼與資料

其他概念都先收在介面底下，等使用者有理由在意時才出現。照這個邏輯，技能和 routine 變成 prompt 的兩種存法，連接器歸到工具底下，使用者要記的名詞少了一大半。

接著是整個產品最關鍵的一步：側邊欄要放什麼。設計團隊的觀察很直白，對話是拋棄式的。我們為了一個問題開一段對話，它被推到側邊欄下面，一週後再開一段新的，很少回頭看最近五則以外的紀錄。

互動的單位是一個問題時，這完全合理；但如果對面那個東西應該認得你、記得之前的工作、長期負責某件事，一條不停往下沉的紀錄就很奇怪了。所以 Grok Bot 的主角是 Bot：有名字、頭像和職稱，記得跟你的對話，有自己的電腦和工具。你明天回來，面對的還是同一個 Bot。

![Grok Bot iPhone App 官方截圖三張：Bot 名單、Bot 在雲端電腦上登入 CRM、對話中的寄信卡片](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/ab4457b6-8431-4ec5-af0e-18f49417714d/48d08b9c-fd43-43b5-ac9f-8753d2670056.jpg)

Grok Bot iPhone App 官方截圖三張：Bot 名單、Bot 在雲端電腦上登入 CRM、對話中的寄信卡片

▲ Grok Bot 行動版的三個畫面：左邊是 Bot 名單，每一列是一個有職稱的 Bot 和它最新的進度；中間是 Bot 在自己的電腦上登入 CRM；右邊是對話裡的寄信卡片，可以一鍵寄出或捨棄（圖片來源：App Store 官方截圖）

標題那個問題的答案就在這裡。通訊軟體的列表按「人」排，你跟一個人的關係會延續下去；AI 聊天的列表按「問題」排，問完就沉到底。Grok Bot 要你跟 Bot 長期合作，所以它長得像通訊軟體：截圖左邊那排業務開發、人才招募、成長行銷、客服、報帳、發票收集、找房子，每一列都是職稱加上最新一句進度，跟你手機裡的聊天列表幾乎一模一樣。

### 決定二：讓人少看一點

名單成立之後，每個 Bot 在介面上的樣子要一次回答三個問題：這是誰？它在做什麼？我需要知道多少？三個答案的方向一致，都在想辦法讓人少花一點注意力。

名單要能快速掃過，頭像就得在眼角餘光裡也認得出來。設計團隊研究了插畫、動畫、遊戲和介面裡的角色系統，從姓名縮寫、emoji、像素風、水彩、黏土風、Noritake 風格的線稿、剪影，一路試到 identicon。

![Grok Bot 官方頭像風格探索：火箭、拿鍋鏟的貓、戴帽子的女生、扳手、戴海盜帽的恐龍、太空人六個角色，各自畫成五種畫風](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/82ed04af-d74b-49ae-9db9-82fcffcccd94/4cf0b5d5-ec8a-4da7-8c38-76793ebb975d.jpg)

Grok Bot 官方頭像風格探索：火箭、拿鍋鏟的貓、戴帽子的女生、扳手、戴海盜帽的恐龍、太空人六個角色，各自畫成五種畫風

▲ 官方設計文公開的頭像風格探索。我把其中五種畫風依角色重新排列：每一列是一種畫風，每一欄是同一個角色，可以看到同一組角色被畫成好幾種風格來比較（圖片來源：SpaceXAI 官方設計文）

多數方案只顧到一邊。水彩和黏土讓每個 Bot 很有個性，縮到側邊欄的尺寸卻細節太多；簡單的系統放進介面很自然，Bot 看起來又像可以互換。最後定案的系統固定基本結構，用簡單的形狀加上有表情的眼睛，再靠受控的變化和配件做出區別，每個 Bot 一眼就認得出來，又像來自同一個世界。

![Grok Bot 定案頭像：灰色水滴、藍色三角形、橘色雲朵、黑色圓形、綠色膠囊、棕色不規則形，每個都有一雙眼睛](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/43ef02fb-8504-4bb2-b454-3eaad7548451/290f4f68-73a7-4059-98d2-f9cb3c8eebf4.png)

Grok Bot 定案頭像：灰色水滴、藍色三角形、橘色雲朵、黑色圓形、綠色膠囊、棕色不規則形，每個都有一雙眼睛

▲ 官方設計文的主視覺：六個 Grok Bot 頭像，幾何形狀加一雙眼睛，靠輪廓、顏色和眼神區分彼此（圖片來源：SpaceXAI 官方設計文）

頭像成為身分之後，也就成了顯示狀態的地方。一個 Bot 可能在待命、思考、工作、等待、卡住或完成，他們大可以替每種狀態做一顆指示燈，但那就多一層要使用者解讀的 UI。最後的做法是讓頭像的動態來表現：待命時平靜、帶點好奇，收到工作會先表示接下，開工後動起來，等待或需要幫忙時換一種動法，做完就安定下來。

AVATAR STATES · 一顆頭像，六種狀態

IDLE

待命

THINKING

思考

WORKING

工作

WAITING

等待

BLOCKED

卡住

DONE

完成

狀態不另外做指示燈，全部交給頭像的動態表現；想知道細節，再把游標移上去看目前的動作。

第三個問題最有意思：Bot 在做事的時候，要讓你看到多少？常見的三個跳動圓點資訊太少，你分不出它是在做事還是卡住了。他們試過顯示一行目前動作的描述，結果使用者看到一步，就想看全部。

使用者研究發現，大家要這些細節，主要是為了安心，想確定它還在做、方向沒有跑掉。所以最後由頭像的動態提供第一層安心，真的想知道它在做什麼，再把游標移上去看。

同樣的邏輯也用在 Bot 的電腦上。那台電腦要多顯眼、使用者什麼時候能接手，他們試了四種擺法：浮動視窗好拿但會蓋住對話，左右並排讓工作一直看得到，彈出視窗把 Bot 的工作區當成暫時的打擾，全螢幕空間夠卻把對話整個擠掉。

試到後來他們發現，電腦放得越顯眼，產品就越在鼓勵使用者去監督它。這是整篇文章裡我最常想起的一個觀察。最後的設計分成三層，讓你走得進 Bot 的工作區，又不會被拉去操作它。

LEVEL 01 · STATUS

狀態

電腦運作時，標題列的圖示變成紫色，你只需要知道它在忙。

LEVEL 02 · PREVIEW

預覽

打開後在旁邊釘一個側欄，不離開對話也能跟著看。

LEVEL 03 · TAKEOVER

接管

Bot 需要幫忙時全螢幕打開，你接手操作，做完再交還給它。

還有一個很小的細節：他們替這台電腦做了會隨時間變化的桌布，早上偏亮、晚上偏暗，讓 Bot 的電腦有自己的時間感，跟你的桌面分得開。官方的比喻是，這比較像跟同事共事，你看得出他在忙，需要時瞄一眼他的螢幕，出狀況才坐到他旁邊。

### 決定三：讓每樣東西待在該待的地方

早期的 Grok Bot 幾乎什麼都用文字回答，五天的天氣預報寫成一段話，一串待辦也寫成段落，使用者還得自己重新整理一次。這讓團隊把回應的形狀當成答案的一部分，在對話裡做了卡片和小工具，適合文字就用文字，適合結構就直接給結構。前面截圖右邊那張能一鍵寄出的信件卡片就是一例；Bot 建立 routine、改設定、傳訊息給其他 Bot，也會以事件的形式出現在對話裡。

Bot 一多，就得決定每個角色該知道什麼。法務 Bot 需要某件糾紛的來龍去脈，財務 Bot 需要好幾年的帳，全部混成一個大記憶，反而很難給每個角色剛好需要的資訊。所以 Grok Bot 讓能力和脈絡走不同的邊界：工具和技能放在帳號層，因為很多 Bot 都要上網、處理文件、寄信；記憶和 routine 屬於個別的 Bot，因為那是這個角色長期知道什麼、負責做什麼。

跨角色的專案就用群組聊天共享脈絡，每個 Bot 仍然保有自己的專業記憶。他們也考慮過加儀表板、任務看板和明確的交接按鈕，最後都沒做，因為每一項都讓使用者多一份協調工作；日常的分派改由負責協調的 Bot 處理，只在需要判斷時找你。

專案尾聲，大部分的設計工作是在刪東西：視窗和面板的控制項、電腦檢視的選項、各種代理的細節資訊都拿掉了，也訂下每個帳號約 50 個 Bot、每個群組 6 個的上限。每個決定都回到同一個問題：這樣做是在幫人委派，還是又多給他一件要管的事？

“

代理扛的責任越多，介面該向人要的就越少。

SPACEXAI 設計團隊 · 官方設計文結語

### 我會多問的一件事

整篇讀完，我同意大部分的決定，只有一個地方想多問一句。把主角從對話換成 Bot 之後，「開新對話」這顆按鈕也跟著消失了。在一般的 AI 聊天裡，對話拉得太長、模型開始混亂時，開一段新對話是最簡單的逃生口。

第三方評測整理的論壇回報裡，剛好有一位創業者提出這件事：每個 Bot 都是一條沒有盡頭的對話串。至少到九月初，官方人員的回覆是沒有開新對話的選項、不能手動壓縮上下文，也看不到上下文用了多少。官方文件的建議則是，做重要決定前，請 Bot 回頭查一次資料來源，別只靠記憶。

我不覺得這是設計失誤。它提醒的是做介面的人都該記得的一件事：拿掉一個控制項，它原本替使用者處理的問題不會跟著消失，只會換個地方出現。Grok Bot 拿掉了對話清單，脈絡管理就從使用者手上移到 Bot 身上，而這件事 Bot 目前還做得不夠好。

CHAPTER 06 · IN PRACTICE

## 設計師可以怎麼用、可以帶走什麼

如果你是接案或小團隊的設計師，Grok Bot 最適合接手的，是那些跨好幾個後台、每週重複、又不需要設計判斷的雜事：把客戶散在信件裡的修改意見整理成一張表、標出對應的頁面；每個月到各家廠商後台下載發票、對好帳；追蹤幾個競品網站有沒有改版；把素材平台的授權文件歸檔。

哪些能交出去，可以用我之前畫的 [判斷地圖](https://rar.design/posts/ai-design-workflow-judgment-map) 來分：規則清楚、只需要收斂的工作交給它；跟客戶的溝通、要不要改稿、設計上的取捨，留給自己。寄信給客戶、發布作品、付款這類動作，一律設成先問你。

如果你已經在用 [Claude Cowork](https://rar.design/posts/claude-cowork-beginner-guide) ，兩者的分野大致在「以什麼為單位」。Cowork 是一件一件的任務，你指定資料夾、給目標、拿回成品；Grok Bot 是一個一個的角色，同一個 Bot 長期負責某類工作、記得你的偏好，在雲端電腦上持續運作。專案型的產出用前者順手，持續性的職責交給後者比較合適。

另一個角度對設計師更有用。如果你正在設計任何「會自己動的產品」，不管是 AI 功能、自動化工具，還是有背景任務的 App，這篇設計文裡有四件事可以直接帶走。

DESIGN TAKEAWAYS · 可以帶走的四件事

01 · IDENTITY + STATE

把狀態放進身分

別另外加狀態燈，讓代表這個物件的頭像、圖示或卡片直接表現狀態，掃一眼就同時知道是誰、在做什麼。

02 · LAYERED OVERSIGHT

監看分層

狀態、預覽、接管三層，預設只給最淺的一層，越深的層級越要使用者主動打開。

03 · RESPONSE SHAPE

回應要有形狀

能用卡片、表格或按鈕呈現的結果，就別寫成一段話，讓對話紀錄混合文字、事件和可以操作的物件。

04 · SHARED VS OWNED

能力共用，脈絡分開

每個角色都需要的能力放在最上層共用，只屬於某個角色的記憶和排程，留在那個角色身上。

這四件事跟我在 [2026 UI/UX 設計趨勢](https://rar.design/posts/ui-ux-design-trends-2026) 整理過的 Agentic UX 是同一條線。介面的工作正從讓人操作，慢慢變成讓人委派，設計師要處理的東西，也從按鈕和流程，換成人對系統的信任要怎麼一點一點建立。

CHAPTER 07 · TAKEAWAYS

## 重點整理

TAKEAWAYS · 一分鐘懶人包

→

Grok Bot 是 SpaceXAI 的常駐代理，每個 Bot 有名字、記憶和一台雲端電腦，會登入你的工具把工作做完，闔上筆電也照跑。

→

沒有單獨訂閱：付費 Cursor 方案內含，最低每月 20 美元；也能連結 SuperGrok 或 X Premium+，用量每週重置，方案之間不疊加。

→

所有 Bot 共用一台電腦，彼此沒有安全隔離；核准只管下一個動作，邊界要在交辦時寫清楚。

→

官方設計文把十四個名詞收成五個物件，側邊欄從對話紀錄換成 Bot 名單，所以它長得像通訊軟體。

→

頭像同時表示身分與狀態，電腦視窗分成狀態、預覽、接管三層，預設只給最淺的一層。

→

拿掉對話清單的代價是脈絡管理：每個 Bot 是一條長對話，重要決定前請它回頭查資料來源。

過去幾年，AI 介面的設計題目是怎麼把一段對話做好。Grok Bot 這篇設計文把題目換成另一個：當你手下有十個不下班的同事，你每天得花多少注意力在他們身上。它的答案是越少越好，前提是介面要設計到你真的敢少看。這個題目接下來每一個 AI 產品都得回答，包括你正在設計的那一個。

RELATED READS · 延伸閱讀[Claude 介面設計解析：Anthropic 如何用色彩、字體和摩擦力打造品牌定位](https://rar.design/posts/claude-interface-design-philosophy)

[

另一家 AI 公司怎麼用介面做品牌，可以對照 Grok Bot 的取捨

→](https://rar.design/posts/claude-interface-design-philosophy)[

Gemini Spark 是什麼？Google I/O 2026 發表的 24/7 AI 代理，能做什麼、和 Claude Cowork 差在哪

Google 的全天候個人代理，跟 Grok Bot 走同一個方向

→](https://rar.design/posts/gemini-spark-google-ai-agent-2026)[

ChatGPT Work 是什麼？GPT-5.6 上線、能做什麼、跟 Claude Cowork 差在哪

OpenAI 的做法：交出一個結果，讓它跨 App 拆步驟

→](https://rar.design/posts/chatgpt-work-gpt-5-6-guide)[

Manus 完整介紹：全球首款通用 AI Agent，幫你把事情做完而不是只給建議

通用代理的起點，看這一代產品從哪裡開始

→](https://rar.design/posts/manus-ai-agent-introduction)[

Hermes Agent 是什麼？Nous Research 開源的「會自我升級」AI agent 完整介紹

開源陣營的跨 session 記憶與自動建立技能

→](https://rar.design/posts/hermes-agent-nous-research-complete-guide)[

設計師為什麼該學 Claude Skills？把工作流封裝成 AI 永久能力

把工作流寫成技能的思路，跟 Grok Bot 的技能是同一套邏輯

→](https://rar.design/posts/claude-skills-for-designers)

EXTRA · LINKS 官方資源

[SpaceXAI 官方公告：Introducing Grok Bot](https://x.ai/news/introducing-grok-bot)

[官方設計文：Designing Grok Bot for a world of persistent agents](https://x.ai/news/designing-grok-bot)

[Grok Bot 官方文件總覽](https://docs.x.ai/grok-bot/overview)

[官方文件：核准、安全與隱私](https://docs.x.ai/grok-bot/approvals-security-and-privacy)

[方案與計費說明（Cursor 說明中心）](https://cursor.com/help/grok-bot/plans)

[Grok Bot 產品頁與下載](https://x.ai/bot)

[官方影片：Introducing Grok Bot（YouTube）](https://www.youtube.com/watch?v=F1_0Lkp16Rc)![AI 覺醒設計應用攻略](https://s.teachifycdn.com/image/width=2048,quality=80,format=webp/attachment/public_image/63e96ac9-97ce-4dfc-be50-154f731f1cc9/e5ca45f0-206a-478f-af1a-96960be47d6a.jpg)

AI 覺醒設計應用攻略

[

SUBSCRIPTION

AI 覺醒設計應用攻略

AI 工具每週都在更新，我幫你過濾雜訊，只留對設計師真正有用的應用。

了解訂閱 →](https://rar.design/courses/aiwaken)

FAQ

## 常見問題

### Q：Grok Bot 是什麼？

A：Grok Bot 是 SpaceXAI（原 xAI）在 2026 年 8 月 11 日推出的常駐 AI 代理，目前仍是 beta。每個 Bot 有名字、記憶和一台雲端電腦，電腦上有瀏覽器、檔案系統與終端機，會登入你平常用的工具完成多步驟的工作，只在需要你核准時回來。

### Q：Grok Bot 要多少錢？哪些方案可以用？

A：沒有單獨訂閱。每個付費的 Cursor 個人方案（Pro 每月 20 美元、Pro+ 60 美元、Ultra 200 美元）與 Cursor Teams 都內含，也可以連結個人的 SuperGrok、SuperGrok Plus、SuperGrok Heavy 或 X Premium+ 取得用量。用量每週重置，方案之間不疊加；免費試用是一筆用量額度，另有 7 天期限。

### Q：關掉電腦之後，Grok Bot 還會繼續工作嗎？

A：會。工作在雲端電腦上執行，關掉 App、闔上筆電或鎖上手機，都不會中斷進行中的任務或排好的 routine。

### Q：讓 Grok Bot 登入我的帳號安全嗎？

A：它有核准、自動審查與敏感步驟交還給你操作的機制，密碼、雙重驗證和付款確認都由你親自輸入。但所有 Bot 共用同一台雲端電腦，登入狀態與檔案互通，Bot 之間不是安全邊界。建議只連接工作需要的工具，寄信、發布、付款、刪除一律設成先問你。

### Q：一個帳號可以開幾個 Bot？

A：官方設計文提到的實際上限，是每個帳號約 50 個 Bot、每個群組聊天 6 個 Bot；每個 Bot 最多可以擁有 50 個 routine。

### Q：台灣可以用 Grok Bot 嗎？支援哪些平台？

A：可以。台灣 App Store 已經上架，免費下載後依你的 Cursor 或 SuperGrok 方案取得用量。桌面版支援 macOS、Windows 與 Linux，行動版支援 iPhone、iPad 與 Android。

### Q：Grok Bot 跟 Claude Cowork 差在哪？

A：兩者的單位不同。Cowork 以任務為單位，你指定資料夾與目標，它交回成品；Grok Bot 以角色為單位，每個 Bot 長期負責一類工作、保有自己的記憶與排程，並在雲端電腦上持續運作。專案型的產出適合前者，持續性的職責適合後者。