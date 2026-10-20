---
title: "Grok Bot 是什麼？實測有自己雲端電腦的 AI Agent"
source: "https://www.darrelltw.com/grok-bot-review/"
author:
  - "[[Darrell]]"
published: 2026-08-23
created: 2026-09-17
description: "xAI 推出的 Grok Bot 實測心得，每個 Bot 有自己的雲端電腦，可以開瀏覽器操作沒有 API 的網站，實測比價流程、把操作存成技能、Bot 之間互相溝通交辦，以及每天自動跑的例行任務，附三種取得方式的費用比較"
tags:
  - "clippings"
status: processed
---
重點摘要

**Grok Bot = 每個 Bot 配一台自己的電腦**  
\- 它是真的開瀏覽器點頁面，所以沒有 API 的網站（PChome、momo）也做得動  
\- 示範一次就能存成技能，之後丟關鍵字重跑  
\- 技能是所有 Bot 共用的，Bot 也能互相傳訊息交辦  
\- 目前 Early Beta，只有 macOS

## Grok Bot 是什麼？

Grok Bot 是 xAI 在 2026 年 8 月推出的 AI Agent 工具，每個 Bot 都配一台自己的雲端電腦，可以開瀏覽器登入你平常在用的網站、讀寫檔案，把一整串工作做完再回報。

跟一般 AI 聊天機器人最大的差別是：聊天機器人給你答案，Grok Bot 是直接幫你把事情做掉。而且那台電腦在雲端，你自己的筆電關機、休眠，它照樣繼續跑。

以前曾經有一段時間，社群很瘋狂提到要買 mac mini 來養龍蝦，或是現在比較流行的 hermes agent 重點都是讓 AI 可以直接 **接管一台電腦來做到更全方位的助理**

而 Grok Bot 是內建提供的雲端電腦 Bot 就可以存取檔案、使用瀏覽器等等

可能你會覺得 Claude, OpenAI 等等也能做到一樣的事情？！ 但他們用的 Computer Use 會直接影響使用者當下的電腦，例如 AI 在做事，你就不能操作 或是電腦關機休眠，原本安排的事情可能就沒做

![Grok Bot 官網首頁，標示 Early Beta 與 Download for macOS 按鈕](https://www.darrelltw.com/grok-bot-review/grokbot_landing_page-1600.avif)

Grok Bot 官網首頁，標示 Early Beta 與 Download for macOS 按鈕

## 方案和費用

有三種使用的方式：

| 方案 | 費用 | 重點 |
| --- | --- | --- |
| Start a Trial | 免費（要綁信用卡） | 全功能，但用量非常吵，超過用量你再決定要不要付錢 |
| Get Pro+ | $60 / 月 | 全功能，用量充足 |
| Get Access with Grok | 已包含在訂閱裡 | 需要 SuperGrok Heavy 或 SuperGrok Plus 方案以上 |

![Grok Bot 三種取得方式：免費 Trial 需綁卡、Pro+ 每月 60 美元、SuperGrok 訂閱內含](https://www.darrelltw.com/grok-bot-review/grokbot_pricing_plans-1600.avif)

Grok Bot 三種取得方式：免費 Trial 需綁卡、Pro+ 每月 60 美元、SuperGrok 訂閱內含

$60 一個月換算台幣接近兩千元，只有訂閱這個方案不算便宜 也可以考慮直接訂閱 SuperGrok Heavy ($300)或 Plus($100) 美金的方案 好處是你還有 Grok 的網頁跟 Grok Build 可以使用，適合重度操作 AI 的用戶才能看到並提交表單

![Confirm your account link 畫面，將 SuperGrok 帳號與另一個帳號連結，說明會採用兩者較高的方案](https://www.darrelltw.com/grok-bot-review/grokbot_account_link_confirm-1600.avif)

Confirm your account link 畫面，將 SuperGrok 帳號與另一個帳號連結，說明會採用兩者較高的方案

## 安裝與設定

安裝就是標準的 macOS 流程，下載 dmg 之後把 App 拖曳安裝

![Grok Bot 安裝畫面，把 Grok Bot.app 拖進 Applications 資料夾](https://www.darrelltw.com/grok-bot-review/grokbot_installer_drag_to_applications-1500.avif)

Grok Bot 安裝畫面，把 Grok Bot.app 拖進 Applications 資料夾

第一次打開會先問 哪些工具是你想要串接的 可以先選一選他先幫你把外掛裝好 (但連線授權後續還是要做一次)

![Grok Bot 初次設定畫面，詢問你每天都用哪些工具，可選 Google 工作區、Slack、Notion、Salesforce 等](https://www.darrelltw.com/grok-bot-review/grokbot_onboarding_tools_select-1600.avif)

Grok Bot 初次設定畫面，詢問你每天都用哪些工具，可選 Google 工作區、Slack、Notion、Salesforce 等

接著建第一個 Bot 取名字、選顏色、選形狀，就這樣，沒有 system prompt 之類的欄位要填

![建立第一個 Bot 的畫面，可選顏色與形狀並輸入名稱，下方有三個建議的 Bot 範本](https://www.darrelltw.com/grok-bot-review/grokbot_create_first_bot-1600.avif)

建立第一個 Bot 的畫面，可選顏色與形狀並輸入名稱，下方有三個建議的 Bot 範本

例如我先選了幾個未來想測試的工具 Slack、Trello 等等

### 外掛程式 Plugins

外掛程式就是目前現有的連接器們 數量算是非常多，國外常用的服務應該都有在裡面

![Grok Bot 外掛程式市集，Gmail 與 Google Calendar 已新增，另有 Google Drive、Granola 與團隊外掛程式](https://www.darrelltw.com/grok-bot-review/grokbot_plugins_marketplace-1600.avif)

Grok Bot 外掛程式市集，Gmail 與 Google Calendar 已新增，另有 Google Drive、Granola 與團隊外掛程式

分類列表裡除了常見的 Productivity、Sales、Design，還有 Agent Orchestration 跟 MCP

補充說明

外掛不是必要的沒裝任何外掛，Bot 一樣能靠瀏覽器把事情做完，但外掛們走 API 或 MCP 相對節省 Tokens

## 最關鍵的差異：Bot 有自己的一台電腦

每個 Bot 都配一台自己的電腦，是一套 Linux 桌面環境，桌面上有 Chrome 跟終端機和檔案管理 你在聊天室右上角點一下就能看到它的螢幕 還能看到他正在執行的動作！

![Bot 的桌面畫面，Dock 上有 Chrome、終端機等應用程式，右上角有教它一項任務按鈕](https://www.darrelltw.com/grok-bot-review/grokbot_bot_own_desktop-1600.avif)

Bot 的桌面畫面，Dock 上有 Chrome、終端機等應用程式，右上角有教它一項任務按鈕

把三個介面都打開來：

![Bot 桌面實際運作畫面，同時開著 Thunar 檔案管理器、終端機與 Chrome 瀏覽器，檔案管理器停在 workspace 目錄](https://www.darrelltw.com/grok-bot-review/grokbot_bot_desktop_working-1600.avif)

Bot 桌面實際運作畫面，同時開著 Thunar 檔案管理器、終端機與 Chrome 瀏覽器，檔案管理器停在 workspace 目錄

## Grok Bot vs Hermes vs 龍蝦：自建透天 vs 飯店公寓

之前大家熱烈討論要買 Mac mini 放家裡養龍蝦（OpenClaw），或是租一台 VPS 跑 Hermes Agent，但大部分非工程師的朋友基本上都在第一關卡死

這三者最大的差別，其實就像在比\*\*「買地自己蓋透天」 **跟** 「住拎包入住的飯店式公寓」\*\*：

![AI Agent 核心對比圖解：拎包入住（Grok Bot）vs 自建透天（Hermes / OpenClaw 龍蝦）](https://www.darrelltw.com/grok-bot-review/grokbot_vs_hermes_openclaw_comparison-1600.avif)

AI Agent 核心對比圖解：拎包入住（Grok Bot）vs 自建透天（Hermes / OpenClaw 龍蝦）

- **心智模型（自建透天 vs 拎包套房）** ：OpenClaw / Hermes 就像自己買地拉水電，自由度 100% 但門檻極高；Grok Bot 則是拎包入住的精裝修公寓，點開 App 雲端電腦就已就緒
- **電腦放哪裡（自備主機 vs 24/7 雲端 VM）** ：自建派需要自備常開的 Mac mini 或 VPS 伺服器；Grok Bot 跑在純雲端虛擬機上，完全不需要佔用你本機的硬體資源
- **怎麼教它做事（寫 Code 配置 vs 示範一次就學會）** ：自建派需要寫 Python 程式或 YAML 設定檔；Grok Bot 只要在桌面錄影點擊一次，它就能自動萃取成技能
- **適合誰（改 Code 掌控派 vs 日常省事外包派）** ：要 100% 資料私密與自由換模型選 Hermes / 龍蝦；不想碰 Linux 只想把日常瑣事外包選 Grok Bot

| 比較維度 | Grok Bot | Hermes Agent | OpenClaw (龍蝦) |
| --- | --- | --- | --- |
| 架設難度 | 極低（下載 App 登入就能用） | 高（要自己租 VPS、SSH、裝環境） | 高（要管 Daemon、寫設定檔） |
| 電腦放哪裡 | xAI 雲端電腦（關機筆電它照樣跑） | 自己的 VPS 或家裡主機 | 自己的 VPS 或家裡主機 |
| 模型自由度 | 鎖定 Grok（不能換 Claude / GPT） | 完全自由（想接哪家模型都可以） | 完全自由（支援 15+ 種平台） |
| 殺手級特色 | 螢幕錄製直接學會、操作無 API 網頁 | 會自己從任務中學新技能的自主大腦 | 生態成熟、技能市集資源最多 |
| 費用結構 | 月費制（$60 Pro+ / 內含算力） | 軟體免費，自付 VPS 租金與 API 費 | 軟體免費，自付 VPS 租金與 API 費 |
| 適合對象 | 不想碰 Linux / 只想把日常瑣事外包的人 | 想自己改 Code 的進階開發者 | 需要跨多平台自動化的重度玩家 |

## 教它一次，存成技能

右上角有一個「教它一項任務」的按鈕 點下去之後你就在 Bot 桌面上示範一次 後續他會讀取這些操作並且把 **整套流程整理成技能**

我示範的是：在 PChome 跟 momo 搜同一個型號比價

![Grok Bot 對話畫面，示範完成後存成比價 PChome 與 momo 技能，並列出步驟與 query 參數](https://www.darrelltw.com/grok-bot-review/grokbot_teach_task_save_skill-1600.avif)

Grok Bot 對話畫面，示範完成後存成比價 PChome 與 momo 技能，並列出步驟與 query 參數

存完它自己複述了一次流程，重點是這幾件事：

- 步驟固定：開 PChome 搜 `{query}` 、新分頁開 momo 搜同一個詞、關掉翻譯或促銷彈窗、回報兩邊價格
- 輸入只有搜尋詞，示範時用的是 `rd280u`
- **不會下單**

`{query}` 是參數，之後想比什麼 丟型號給它就好，不用再走一次示範

## 技能是共用的，Bot 之間會互相交辦

比價這件事我不想放在主要的 Bot 身上 就問它能不能交給另一個 Bot

答案是可以，而且技能本來就是全部 Bot 共用的 它接著自己傳了一則訊息給比價小幫手，交代之後這套流程由對方負責

對話框裡會直接顯示「已傳訊息給 比價小幫手」跟「訊息來自 比價小幫手」 所以 Bot 之間講了什麼，你在自己的聊天室就看得到，不用切過去確認

代表 **Bot 之間是可以互相溝通的** 未來你可以交代給一個主要的 bot 他再幫你把任務拆分給其他專精的 bot 去執行

例如一人公司裡面的 CEO 你交代給 CEO 後就把任務轉派給對應的部門去執行！

## 例行任務：讓它每天自己跑

Grok Bot 當然也內建的排程功能 排程的設定跟其他工具一樣簡單

![例行任務設定畫面，包含名稱、指示、執行時機每天上午 8:00，以及執行歷史紀錄](https://www.darrelltw.com/grok-bot-review/grokbot_scheduled_task_settings-624.avif)

例行任務設定畫面，包含名稱、指示、執行時機每天上午 8:00，以及執行歷史紀錄

上面有啟用中的開關跟測試執行，下面有執行歷史紀錄可以看 排程頻率選單預設是每天上午 8:00

如果想要每天跑一個比價，看看有沒有變便宜 就可以設定一個每天早上 x 點執行的排程

## 實測：拿 BenQ RD280U 跑一次比價

技能存好之後，我直接比價小幫手 Bot 跑一次，順便要它整理成表格

它是邊跑邊回報的，momo 先有結果，PChome 那邊還在的截圖

![比價小幫手回報 momo 搜尋結果，同時右側顯示 Bot 正在操作 momo 網頁的螢幕縮圖](https://www.darrelltw.com/grok-bot-review/grokbot_price_compare_running-1600.avif)

比價小幫手回報 momo 搜尋結果，同時右側顯示 Bot 正在操作 momo 網頁的螢幕縮圖

右邊的螢幕縮圖可以看到它人在 momo 的搜尋頁，還順手關掉了 Google 翻譯的彈窗

全部跑完的結果：

![比價小幫手完整回報 BenQ RD280U 在 PChome、momo、Yahoo 購物、良興的價格與庫存狀況](https://www.darrelltw.com/grok-bot-review/grokbot_price_compare_result-1600.avif)

比價小幫手完整回報 BenQ RD280U 在 PChome、momo、Yahoo 購物、良興的價格與庫存狀況

要它整理成表格，它就真的輸出一張表，欄位還自己分了網站跟出貨的倉庫！

![比價小幫手輸出的比價表格，欄位為網站、頻道、品名、售價、原價，列出 PChome 官方倉、PChome 南紡購物中心與 momo 的四筆商品](https://www.darrelltw.com/grok-bot-review/grokbot_price_compare_table-1600.avif)

比價小幫手輸出的比價表格，欄位為網站、頻道、品名、售價、原價，列出 PChome 官方倉、PChome 南紡購物中心與 momo 的四筆商品

分「網站」跟「頻道(倉庫)」這件事很實用 PChome 底下的官方倉跟南紡購物中心是兩種不同的出貨方式 通常官方倉庫比較會有 24h 速速到貨 南紡有 14,888 跟 16,888 兩種價格

原價 16,888，主流通路現在幾乎都落在 14,888

搭配一開始提到我們可以串接 Slack, Gmail 等等 可以多做一個指令

```plaintext
如果價格低於 14,888
請用 Slack or Gmail 通知我
```

這樣你就不會錯過真的特價的時候 偶爾回來關心一下排程是不是真的有在執行就好

## 還有哪些應用場景？

前面 BenQ 螢幕比價只是最好示範的一個案例 下面三個是我後來看到不少社群的貼文後 覺得很有趣也可能很實用的 Grok Bot 場景！

### 場景一：沒有 API 的網頁後台，每天自動登入查看

做電商的賣家或接案團隊，每天早上都要檢查好幾個網站和後台 例如登入某個老舊 CRM 或是 ERP，或是去某些網站查看資料 每次都要開瀏覽器、輸入帳號密碼、點進列表，再手動複製到 Excel 或是來回貼上

這時候直接給 Bot 一組帳號，讓它在自己的雲端電腦跑 在桌面示範一次怎麼登入、點到哪一頁、看哪幾欄，存成技能 Skill 接著設定每天早上固定幾點跑排程 Bot 就會自己開 Chrome 登入，看完把摘要整理好 提醒一句：帳號權限給唯讀或是盡量限縮，不然可能有風險

```plaintext
每天早上 9:00 登入後台
查看是否有待處理的新訂單
如果有，把訂單編號和金額整理成摘要用 Line 發送
```

### 場景二：先幫你寫草稿，你允許它才送出

一人公司、接案者或是要顧社群的小編 每天回覆客戶詢價信、處理 Slack 上重複的問題，大半天時間就沒了 大家其實想找 AI 幫忙回覆 但就怕 AI 不小心回答錯誤，造成不可逆的損失

我們可以先把 Bot 接上 Gmail 有新的信件進來時，Bot 會先讀上下文並先寫成草稿 寫完草稿後會先傳送到 Slack 或是其他通訊軟體 等你看過跟核准後它才會真的送出

```plaintext
收到新的客戶諮詢信件時
先參考過往資料起草回覆建議
推送到我的 Slack 等我核准後再寄出
```

這場景其實我目前有用 n8n 在做 表單 → 信箱 → n8n → 寫草稿 → 我手動調整再回覆

### 場景三：幫你盯搶票、候補、缺貨回補(PS5)

想搶演唱會門票、連假的高鐵早鳥票或是盯著熱門商品什麼時候補貨 例如 2026 下半年的 PS5 Pro 我們不可能整天守在網頁前重新整理一直刷 但通常等你忙完點開看 不知道何時釋出的名額或商品早就被買走了

這跟前面測的 BenQ 螢幕比價是類似的邏輯 你在雲端電腦示範一次打開網頁，教它看按鈕狀態，存成技能 然後把排程頻率拉高，讓它定時開 Chrome 巡邏 一旦按鈕狀態變了，立刻發通知到手機或 Slack！

售票系統或商品頁通常沒有公開 API 而且大多是動態載入，讀不到真正狀態 Grok Bot 是真的在雲端電腦載入完整畫面 我們人類看得到什麼按鈕，它也都可以

不過這裡有個限制：它是幫你盯著，不是幫你瞬間搶票 會秒殺的商品或是票卷，它開瀏覽器點按鈕絕對搶不贏黃牛程式 它適合的是候補、釋票、商品補貨這種等得起的情境

```plaintext
每 15 分鐘檢查一次這個頁面
如果購買按鈕從「已售完」變成可以點擊加入購物車
立刻發 Slack 通知我
```

## 適合的對象和場景

### 場景參考

| 適合 | 先不用急 |
| --- | --- |
| 常要跨好幾個網站查同一件事（比價、找貨、追蹤庫存） | 只需要問答、寫東西、整理資料 |
| 要處理的網站沒有 API | 目標服務有現成 API 或 MCP 可以接 |
| 有固定重複的網頁操作流程 | 每次任務內容都不一樣 |

### 適合對象

我覺得一直對龍蝦和 Hermes 有興趣 卻不知道從哪邊下手的人都蠻適合的

但目前使用的門檻還是有點高，60, 100, 300 美金都還是不小一筆花費 如果你的日常生活或是工作場景，真的覺得需要把一些工作外包給 AI 幫忙執行 例如他 **每天能幫你省下 2-3 個小時** 你又換算自己的時薪假設抓 500 元

那這個 AI 助理 **每個月就能幫你省下 20000 以上！** 這樣就很適合嘗試看看

或是你想知道找人幫你梳理自己有哪些工作流程適合外包給 AI 也可以找我的付費諮詢聊聊，甚至是安排一對一的陪跑或是教學！

## 常見問題

有免費 Trial，但要先綁信用卡官方寫明沒訂閱不會扣款，用量有限制(非常小)

還不行，官網目前只有 macOS 下載

我實測的比價技能，它在存檔時主動註明「不會下單」不過它確實有能力操作瀏覽器，所以得在 prompt 特別寫死，或是不要登錄

選 Get Access with Grok，把 Grok 帳號跟 Grok Bot 帳號做連結，權限和用量會取兩邊較高的方案需要 SuperGrok Heavy 或 SuperGrok Plus

[Cowork](https://www.darrelltw.com/claude-cowork-intro/) 是在你自己的電腦上動你的檔案，算是跟你共用一台你的電腦(Computer Use)，Grok Bot 是給 Bot 一台它自己的電腦去外面跑前者適合整理本機資料，後者適合要跨網站操作的事