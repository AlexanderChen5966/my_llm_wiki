---
title: "Grok Bot 是什麼？不只會聊天，還能接手瀏覽器與日常工作的 AI 隊友"
source: "https://www.aiposthub.com/grok-bot-personal-agent-workflow-security/"
author:
  - "[[Philo]]"
published: 2026-08-12
created: 2026-09-17
description: "SpaceXAI 推出 Grok Bot early beta，讓 AI 使用雲端電腦、登入工具、保留記憶並定時執行工作。本文整理功能、案例與安全風險。"
tags:
  - "clippings"
status: processed
---
很多 AI Agent 看起來什麼都能做，實際使用時卻常把時間花在設定伺服器、建立帳號與處理失敗任務。使用者原本想省時間，最後反而多了一套需要維護的系統。

2026 年 8 月 11 日，SpaceXAI 以官方 Grok Bot 帳號宣布推出 [Grok Bot early beta](https://x.com/bot/status/2087224798078517251?ref=aiposthub.com) 。官方把它定位成可以登入工具、代替使用者操作，最後帶著完成成果回來的「AI 隊友」。Cursor Developer Experience 成員 Matt Palmer 隨後在 [〈Intro to Grok Bot〉](https://x.com/mattyp/status/2087252657589412119?ref=aiposthub.com) 長文中，進一步展示自己如何用它整理產品動態、建立技術 Demo，以及處理日常採購。

Grok Bot 最值得注意的地方，不是多了一個聊天視窗，而是 AI 終於有一台能持續運作的電腦。它可以使用瀏覽器、檔案、登入狀態與外部工具，碰到只有人能完成的驗證時，再把操作權交回使用者。

我對這個方向中性偏多。它確實解決了個人 AI Agent 一直存在的環境與交接問題，但目前仍是 early beta。若要試用，適合先從資料整理、內容草稿與通知等低風險工作開始，不建議一開始就交付付款、刪除資料或大量對外發送訊息。

## Grok Bot 是什麼？

Grok Bot 是一種能操作電腦與工具的 AI Agent。AI Agent 指的是不只回答問題，還能自行拆解步驟、呼叫工具並完成任務的系統。

依 Matt Palmer 的介紹，每個 Grok Bot 都在持續存在的 Linux 虛擬機器（VM）上運作。VM 可以理解成雲端上的一台獨立電腦，因此使用者關閉自己的電腦後，Bot 仍能繼續處理被交付的工作。

它可以開啟網頁與 App、存取檔案、擷取畫面，也能使用已建立的網站登入狀態。透過桌面端對話時，Bot 還能取得使用者授權的本機檔案。這讓它和一般聊天機器人出現明顯差別：聊天機器人主要提供文字答案，Grok Bot 則試圖直接完成答案背後的操作。

| 比較項目 | 一般 AI 聊天機器人 | Grok Bot |
| --- | --- | --- |
| 主要輸出 | 回答、建議、草稿 | 操作工具並交付成果 |
| 執行環境 | 單次對話為主 | 持續運作的雲端電腦 |
| 網站操作 | 提供步驟，或僅在特定模式執行 | 可使用瀏覽器與網站登入狀態 |
| 任務啟動 | 使用者主動提問 | 訊息、排程、Slack 或 GitHub 事件 |
| 記憶 | 以對話紀錄或單一帳號設定為主 | 分成使用者、Bot 與專案三層 |
| 人工介入 | 另開網頁自行處理 | 可在登入、2FA、CAPTCHA 或付款時交接電腦 |

這個產品方向的核心不是追求完全無人化，而是減少「AI 做到一半，人類只能重來」的斷點。只要交接流程足夠順，部分原本不值得自動化的小事，也可能變得值得交給 Agent。

![Grok Bot 官方影片展示 AI Bot 自動整理大量電子郵件](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/08/02-email-triage.jpg)

Grok Bot 官方影片展示 AI Bot 自動分類大量電子郵件的使用情境。圖片來源：Grok Bot 官方 X 帳號。

## 登入卡住時，Grok Bot 會把電腦交回來

跨網站自動化最常卡住的地方，不是 AI 不知道下一步，而是遇到登入、單一登入（SSO）、雙重驗證（2FA）、CAPTCHA 或付款確認。SSO 讓使用者以同一組公司帳號登入多項服務，2FA 則要求密碼以外的第二道驗證；CAPTCHA 是網站用來區分真人與程式的驗證機制。這些步驟通常需要本人身分、手機驗證或明確同意，無法單靠模型繼續完成。

Grok Bot 的做法是直接交接電腦。Bot 遇到阻礙時，使用者可以接手完成驗證，再把控制權還給 Bot。對 API key，也就是讓程式存取外部服務的驗證金鑰，Bot 能傳送安全表單讓使用者自行填入，而不是把敏感內容寫進一般對話。

這項設計很實際。過去許多個人 Agent 要求使用者事先建立服務帳號、準備遠端伺服器或設定每個網站的 API，導入成本很快超過省下的時間。Grok Bot 改為沿用人本來就會操作的介面，並在必要時請人接手，能降低第一段設定門檻。

代價是登入狀態本身就代表權限。Bot 一旦能操作已登入的購物、信箱或內部系統，影響就不只是答錯一句話，而可能變成寄錯訊息、修改資料或執行非預期交易。因此，順暢的交接只是可用性的解法，不等於安全問題已經消失。

## 記憶分成三層，讓 Bot 不必每次重新認識工作

Grok Bot 的長期記憶分為使用者、Agent 與專案三層。

- 使用者記憶：保存姓名、時區與偏好等跨 Bot 共用資訊。
- Agent 記憶：每個 Bot 有自己的角色設定與互動紀錄，知道自己負責什麼。
- 專案記憶：保存團隊決策、工作慣例與專案背景，不綁定單一 Bot 或單一成員。

這三層的價值在於把「你是誰」、「這個 Bot 做什麼」與「這項工作怎麼做」分開。若所有內容都塞進同一段 Prompt，規則會越來越難維護，也容易在不同任務之間互相干擾。

Grok Bot 也支援多個 Bot 協作。協調者可以把工作分給其他 Bot，而不同 Bot 也能互相觸發。對長期專案來說，這比單純增加聊天紀錄更重要，因為記憶開始成為可分工、可延續的工作資產。

不過，長期記憶同樣需要治理。錯誤資訊若被寫進記憶，可能持續影響後續任務。團隊仍要能查看、修正與刪除記憶，並區分個人偏好、專案事實與暫時性的推測。

## 從排程到 Slack 事件，讓 Bot 主動開始工作

Grok Bot 不必等使用者每次開啟聊天。依 Matt Palmer 的介紹，Bot 可以透過排程、新的 Slack 訊息或 Git 事件啟動，也能由另一個 Bot 觸發。使用者還可以先示範一段流程，將操作步驟保存成可重複執行的 routine，也就是固定工作流程。

這讓 Agent 從「問一次、做一次」變成背景工作者。例如，每天整理指定來源、每小時掃描產品頻道，或在程式碼事件發生後自動啟動檢查，都不需要人先記得打開聊天視窗。

Grok Bot 也能使用 Cursor 已有的 plugins、connectors 與 skills。Connector 是連接 Notion、Slack 或 GitHub 等外部服務的授權介面；skill 則是告訴 Agent 某類工作應遵循哪些步驟與規則。對已經在 Cursor 建立工作環境的使用者來說，這能減少重複設定。

![Grok Bot 官方影片展示把操作示範轉成可重複使用的 skill](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/08/03-workflow-skill.jpg)

Grok Bot 可觀察操作示範，再把流程保存成能重複執行的 skill。圖片來源：Grok Bot 官方 X 帳號。

[Cursor 官方資料](https://cursor.com/grok?ref=aiposthub.com) 顯示，Grok 4.5 已用於程式開發與一般知識工作，並可從桌面、網頁、手機、命令列工具（CLI）與軟體開發工具包（SDK）使用。CLI 讓使用者在終端機操作 Agent，SDK 則讓開發者把模型能力整合進自己的產品。Grok Bot 把這些能力再往前推一步，讓模型不只完成單次任務，也能在持續存在的電腦環境中等待事件、重複執行與跨工具工作。

## Matt Palmer 怎麼使用 Grok Bot？

Matt Palmer 分享的案例主要集中在「資訊很多，但不值得每天手動檢查」的工作。

第一個是 Demo Bot。它每天檢查他的 X 書籤，挑出值得嘗試的新技術，再依照既有寫作與專案規劃規則產生 Prompt。取得同意後，Bot 會啟動 Cursor Cloud Agent 建立技術 Demo。依作者個人描述，約 15 分鐘後就能在 Cursor App 中查看原型。

第二個是 Content Bot。它每小時掃描工程與產品 Slack 頻道，找出適合對外分享的小更新，再提供社群貼文草稿。Product Bot 則每天整理較大型的產品公告，讓他不用逐一翻找內部頻道。

他也把 Grok Bot 用在生活流程。採購 Bot 會比較 Instacart 與 Amazon 的商品、數量、價格與運費，再整理兩個購物車；DoorDash Bot 則從 Slack 找出團體訂餐連結，協助他查看菜單與進入正確的訂單頁面。

![Grok Bot 官方影片展示 AI Bot 依工作內容產生客戶素材](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/08/04-customer-materials-selected.jpg)

官方影片以客戶素材製作為例，呈現 Grok Bot 如何運用工作脈絡完成任務。圖片來源：Grok Bot 官方 X 帳號。

這些案例不是第三方效能測試，而是產品團隊成員的使用經驗，不能直接推論所有使用者都能得到相同結果。但它們清楚指出最適合 Grok Bot 的任務特徵：流程會重複、資訊散落在多個工具、需要登入，而且最後仍保留一個由人確認的節點。

## Grok Bot 安全嗎？最大風險是權限與 prompt injection

Matt Palmer 表示，Grok Bot 透過權限、允許清單（allow list）、封鎖清單（block list），以及負責檢查動作的 reviewer Agent 控制行為。使用者可以用自然語言設定規則，reviewer Agent 再判斷動作應該允許、阻擋，或升級給使用者確認。工作也在隔離環境中進行，降低它直接影響使用者裝置的範圍。

這些都是必要防線，但不能把「有審查 Agent」理解成「不會出錯」。Prompt injection 是目前瀏覽器 Agent 的核心風險之一。它指的是攻擊者把惡意指令藏在網頁、郵件或文件裡，試圖讓 Agent 誤以為那是使用者交代的任務。

[NIST 的 Agent hijacking 研究](https://www.nist.gov/news-events/news/2025/01/technical-blog-strengthening-ai-agent-hijacking-evaluations?ref=aiposthub.com) 指出，許多 AI Agent 仍可能被間接 prompt injection 影響，進而執行非預期行為。 [Anthropic 的瀏覽器 Agent 安全研究](https://www.anthropic.com/research/prompt-injection-defenses?ref=aiposthub.com) 也明確表示，沒有任何瀏覽器 Agent 能完全免疫這類攻擊。這不是 Grok Bot 獨有的缺陷，而是所有能讀取外部內容、同時又能操作工具的 Agent 都要面對的問題。

Grok Bot 的能力越完整，權限管理就越重要。若只是整理公開資料，失敗成本通常可控；若 Bot 同時能讀信箱、使用已登入網站與對外傳送資料，一次錯誤判斷就可能跨越多個系統。

實際使用時，建議先遵守以下原則：

1. 從只讀、可回復的任務開始，例如摘要、分類與建立草稿。
2. 每個 Bot 只開放完成工作真正需要的工具與資料。
3. 付款、刪除、大量發送與帳號設定變更一律保留人工確認。
4. 不用「幫我處理所有信件」這類範圍不明的指令，改成明確指定來源、動作與停止條件。
5. 定期檢查登入狀態、記憶內容、觸發規則與執行紀錄。

## Grok Bot 值得用嗎？

Grok Bot 值得關注，但現階段只適合特定工作。

如果每天都要在 Slack、GitHub、瀏覽器與內部工具之間搬運資訊，Grok Bot 的持續環境、事件觸發與人工交接可能真正減少操作成本。尤其是內容整理、產品情報、研究彙整、通知與原型製作，成果容易檢查，出錯後也相對容易修正。

如果工作涉及金流、客戶個資、合約、正式對外訊息或不可逆的資料變更，我會等產品離開 early beta，並看到更完整的權限紀錄、記憶管理與安全評估後再擴大使用。自然語言規則很方便，但便利不代表它能取代明確的存取控制與人工核准。

我對 Grok Bot 中性偏多的原因，是它沒有假裝人可以完全退出流程，而是把登入、驗證與付款等關鍵時刻設計成人機交接。若後續實際使用顯示 Bot 經常需要救援、記憶難以管理，或 reviewer Agent 無法穩定阻擋高風險動作，這個優勢就會消失。

## Grok Bot 常見問題

### Grok Bot 和 Grok 聊天機器人一樣嗎？

不一樣。Grok 聊天機器人以問答、搜尋與內容生成為主；Grok Bot 則有持續運作的電腦環境，可以登入工具、操作網頁、使用檔案並依事件或排程執行任務。

### Grok Bot 和 Cursor Cloud Agent 有什麼差別？

Cursor Cloud Agent 主要處理程式開發工作。Grok Bot 的範圍更廣，除了可以啟動 Cursor Cloud Agent，也能操作一般網站與商業工具，處理內容、研究、通知與生活流程。

### Grok Bot 可以在我的電腦關機後繼續執行嗎？

依 Matt Palmer 的產品介紹，Grok Bot 運作在持續存在的雲端 Linux VM，因此不必讓使用者電腦一直開機。若任務需要本機檔案或本人驗證，仍可能要求使用者透過桌面端提供檔案或接手操作。

### Grok Bot 能自動登入網站嗎？

它可以使用已建立的登入狀態。遇到 SSO、2FA、CAPTCHA 或付款等需要本人處理的步驟時，Bot 會把電腦控制權交給使用者，完成後再接手後續流程。

### Grok Bot 適合處理付款或重要帳號嗎？

early beta 階段不建議直接開放高風險權限。即使產品有 reviewer Agent、allow list 與 block list，瀏覽器 Agent 仍面臨 prompt injection 與誤操作風險。較穩妥的做法是讓 Bot 準備購物車或付款資料，最後一步由人確認。

## 結論：個人 Agent 的關鍵，不是更會說，而是能把工作安全地做完

Grok Bot 把 AI 模型、雲端電腦、登入狀態、長期記憶與事件觸發放進同一套產品。這讓它比一般聊天機器人更接近真正的工作夥伴，也讓原本散落在瀏覽器、自動化平台與遠端伺服器的設定變得集中。

真正決定它能否長期使用的，不只是模型能力，而是三件事：交接是否順暢、權限是否足夠細緻，以及使用者能否清楚看見 Bot 做過什麼。

現階段最實際的做法，是挑一個每週重複、成果容易檢查、失敗可回復的流程試用。當 Grok Bot 能穩定省下時間，再逐步增加工具與自動化範圍。這比一開始就追求「全自動個人助理」，更容易得到真正有用的結果。

## 資料來源

- [Grok Bot 官方 X 公告：Introducing Grok Bot, now in early beta](https://x.com/bot/status/2087224798078517251?ref=aiposthub.com)
- [Matt Palmer：Intro to Grok Bot](https://x.com/mattyp/status/2087252657589412119?ref=aiposthub.com)
- [NIST：Strengthening AI Agent Hijacking Evaluations](https://www.nist.gov/news-events/news/2025/01/technical-blog-strengthening-ai-agent-hijacking-evaluations?ref=aiposthub.com)
- [Anthropic：Mitigating the risk of prompt injections in browser use](https://www.anthropic.com/research/prompt-injection-defenses?ref=aiposthub.com)