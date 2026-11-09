---
title: "Loopcut 是什麼？在 Blender 用 ChatGPT 訂閱，和 Blender MCP 差在哪"
source: "https://www.aiposthub.com/loopcut-blender-chatgpt-plan-ai-agent/"
author:
  - "[[Philo]]"
published: 2026-10-21
created: 2026-11-04
description: "Loopcut 是開源的 Blender 內建 AI 代理，可登入 ChatGPT Plus 或 Pro 方案操作場景。本文依官方網站、GitHub 與 OpenAI 的 Sign in with ChatGPT 文件，整理運作方式、授權、隱私、費用、安裝提醒與導入前檢查。"
tags:
  - "clippings"
status: "processed"
---
Loopcut 來自 10 月 20 日 X 上的一則英文貼文，大意是「現在可以直接在 Blender 用你的 ChatGPT 訂閱，不需要 Blender MCP」，附 26 秒影片，我 10 月 21 日早上查詢時約有 1,350 個讚，10 月 25 日再查已有 1,839 個讚、17.8 萬次觀看。

這句話容易讓人以為是 OpenAI 或 Blender 推出的新外掛。但貼文作者的 X 簡介寫的是「building the AI agent for blender」，並連到 loopcut.org，所以這是 Loopcut 開發者自己的發表。

Loopcut 本身也不是外掛。它是把 Blender 5.2.2 整個複製出來修改的分支版本，加上一個內建的 AI 面板，所以安裝的是一個獨立的 Blender，不會動到你現有的那一份。

它能用 ChatGPT 訂閱，靠的是 OpenAI 公開的 Sign in with ChatGPT 流程，讓開源應用請使用者授權，用 Plus 或 Pro 方案的額度送出模型請求，不必另外申請 API 金鑰。

這對已經付費的 Blender 使用者很實際，因為省下一筆 API 帳單，也不用自己架 MCP 連線。

不過它的倉庫 9 月 19 日才建立，仍是測試版（我 10 月 21 日查時最新版是 0.2.5，10 月 25 日已更新到 0.2.17），macOS 版還沒簽章，Windows 版官方也說測試得比 Mac 少，所以更適合願意先在副本上試的人。

我的判斷是，已付 Plus 或 Pro 方案、想把命名、燈光與渲染設定這類技術雜務交給代理的 Blender 使用者，值得試一次。要在重要專案上放手讓代理改場景的人，先把檢查點與備份做好。

要看懂它和一般的 Blender MCP 差在哪，得先弄清楚 Loopcut 每一回合在做什麼。

## Loopcut 是什麼？內建 AI 面板的 Blender 分支，不是外掛

Loopcut 把「對話、寫程式、檢查、還原」都放進同一個 Blender 視窗，讓你用一句話交代技術工作，再逐步看它做了什麼。

官方 GitHub 說明寫的是：這是 Blender 5.2.2 的分支，加了一個面板、約 40 個修改過的檔案，並且用 GPL-3.0-or-later 授權，和 Blender 一樣。

面板停靠在 3D 視窗右側。第一次啟動時，可以選擇匯入你原本 Blender 5.2 的設定、附加元件、快捷鍵與佈景，也可以從頭開始。

每一回合的流程是這樣：你用一般語言交代工作，Loopcut 先讀目前場景，所以知道物件、材質與集合的名稱。接著它寫出 Blender 的 Python 程式碼，並顯示在畫面上。

程式碼等你核准才會執行，也可以選擇這場對話之後不再詢問。執行後它會用視窗截圖或渲染檢查結果，再列出「場景變更」，說明新增、移除與改名了哪些東西。

這個變更清單是從場景實際量出來的。官方說明強調，這樣模型讀到的是實際狀態，不會只看它自己印出來的值。

每一次改動場景的回合都會建立檢查點，按「全部復原」就能把場景與對話一起還原。官方說明你的.blend 檔不會被寫入，還原是在原地進行。但開發者文件也列出不在還原範圍內的東西：代理寫到磁碟別處的檔案、偏好設定、附加元件與連結的外部資源庫。

官網舉的範例都是明確的技術工作，例如依材質重新命名物件並把燈光放進獨立集合、架一組三點打光、設定 Cycles 取樣並輸出 EXR，或是把某個集合裡的每種材質套到椅子上批次渲染。

也有幾個比較特別的工具。 `check_contact` 會依實際網格量測物件有沒有接觸、有沒有穿模， `compare_with_reference` 讓你貼一張參考圖，與目前畫面並排比較。

10 月 20 日的 0.2.5 版另外加入雲端動作功能：丟一段影片，它會擷取人物全身動作並套到你的角色骨架上，或用一句描述生成 2 到 10 秒的動作。這類雲端服務要用 Loopcut 點數，影片每秒 3 點；文字每秒原本是 5 點，0.2.5 的發布說明寫明 10 月 21 日已降成 2 點，定價頁現在也是每秒 2 點。執行前都會先顯示報價。

下載版本目前提供 Apple 晶片的 Mac 與 64 位元 Windows，下載頁寫明 Intel Mac 尚未支援，也沒有列出 Linux 版。官方下載頁 10 月 25 日標示 0.2.17 的 macOS 安裝檔約 383 MB，Windows 安裝程式約 367 MB，免安裝的.zip 約 413 MB。我 10 月 21 日查 0.2.5 時是 374 MB 與 357 MB，新版的檔案變大了。

了解它的形式之後，就能看它怎麼用上 ChatGPT 訂閱。

![Loopcut 面板位於 Blender 視窗右側，列出三點打光的執行步驟與場景變更清單](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/size/w2400/2026/10/loopcut-blender-chatgpt-plan-cover.webp)

Loopcut 官方 GitHub 說明頁的主圖，顯示 Blender 視窗右側的 Loopcut 面板與場景變更清單，非本站拍攝／實測。 圖片來源： 官方原文 。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/loopcut-blender-chatgpt-plan-demo.mp4" width="1280" height="720" controls=""></video>

Loopcut 官方網站首頁的 26 秒 ChatGPT 方案示範影片，由開發者發布，非本站實測。畫面字幕標示「6 min 54 s of real agent work, sped up」，也就是真實作業約 6 分 54 秒、加速成 26 秒；面板標題顯示這支影片錄於 Loopcut 0.2.4 測試版，現在的版本是 0.2.17，介面可能已經不同。 來源： 官方發布 。

## Loopcut 怎麼用 ChatGPT 訂閱？OpenAI 的 Sign in with ChatGPT 流程

它走的是 OpenAI 為開源應用公開的授權流程，由你在瀏覽器裡明確同意，沒有借用你電腦裡的登入資料。

OpenAI 開發者文件說明，「ChatGPT 方案使用」是 Sign in with ChatGPT 的選用功能，開源應用可以請求使用者授權，用他們的 ChatGPT 方案處理符合資格的 Responses API 請求。

Responses API 是 OpenAI 讓程式送出對話請求的介面。文件也說明，符合資格的使用者是 ChatGPT Plus 與 Pro，而且這項授權不會讓應用存取你的 ChatGPT 對話或其他帳號內容。

Loopcut 的原始碼註解自述，它照這份文件實作，使用 PKCE 與本機回呼位址，並檢查回傳權杖與授權範圍。註解還寫明，它不會去讀 Codex 或 ChatGPT 自己存的登入資料，也不接受貼上的金鑰。

這點很重要，因為授權範圍由 OpenAI 的流程限定，你也能在 ChatGPT 的用量設定裡查看用量、調整限制。不過我只能從原始碼註解與文件確認設計，並沒有實際登入測試。

登入後，用量會算在你的 ChatGPT 方案內，不會消耗 Loopcut 的點數。

不過登入前要不要先有 Loopcut 帳號，官方的說法不一致。下載頁寫「你需要用免費的 Loopcut 帳號登入」，設定流程第一步也是請你建立帳號，再點「Sign in with ChatGPT」；隱私政策卻寫「可以不用 Loopcut 帳號使用」。我沒有實際登入測試，無法判斷哪一個是現況，建議先照下載頁的步驟走。

OpenAI 的 UI 指南寫明，應用要在介面上顯示「Using ChatGPT plan」，並連到 chatgpt.com/settings/usage 的用量設定，讓你查看與調整限制。

Loopcut 的原始碼註解提到，使用者可以在那裡為 Loopcut 設定每週用量上限。上限可能來自整個方案，也可能只針對這個應用，額度用完時會跳出提示。

這條授權路線也有功能限制。OpenAI 的預覽限制頁寫明，這種請求不支援圖像生成、檔案搜尋、程式碼直譯器、原生電腦操作與託管的 MCP 連接器，影像與檔案可以輸入，聲音與影片輸入則不行。

所以 Loopcut 需要的 Blender 工具，全部由它自己提供，用不到 ChatGPT 的內建工具。

另外要知道，OpenAI 的文件把這項能力放在預覽階段。官方說法是，ChatGPT 方案用量目前對開源夥伴開放，一般商業應用則只限選定夥伴的有限試辦。

Loopcut 的應用本身是開源的，但它另有付費的雲端服務。OpenAI 的條件日後怎麼調整，我沒有查到官方說明，這是使用前要留意的變數。

## 和 Blender MCP 比起來差在哪？官方比較表怎麼讀

兩者的架構不同：Blender MCP 是外部 AI 用戶端遠端控制 Blender，Loopcut 則是把代理放進 Blender 自己的程式裡。

Blender MCP 是一個開源附加元件，讓 Claude 或其他支援 MCP 的用戶端連到 Blender。站內已有教學，說明怎麼用它從分鏡走到可修改的專案。

貼文說「不需要 Blender MCP」，意思大概是少掉一條外部連線。你不必再開一個支援 MCP 的 AI 用戶端，也不必另外安裝附加元件。

Loopcut 定價頁有一張比較表，把自己、Blender MCP、Claude 的 Blender 連接器與單獨的 ChatGPT、Claude、Codex 並列。表格的結論多半對 Loopcut 有利，例如每一步都有檢查點，其他方案沒有。

這張表要看清楚來源。它是 Loopcut 自己做的，標註為「依各專案公開文件，2026 年 10 月確認」，頁面還提醒這些內容常常變動，若有過期要告訴他們。

表裡把 Blender MCP 與 Claude 的 Blender 連接器合併成一欄，在「用你已付費的 ChatGPT 方案」這一列寫的是「部分，只能透過 Codex」。我沒有獨立查證這一格，所以只能當作廠商對競品的描述。

我能查證的是，檢查點與場景變更清單確實寫在 Loopcut 的倉庫文件裡。Blender MCP 那幾欄是它對競品的描述，我沒有逐項查證，所以哪個工具比較準，沒有公開的對照測試可以回答，要用自己的專案試。

延伸閱讀： [Blender MCP 教學：從 X 熱傳 3D 短片，學會分鏡、場景與可修改專案的驗收](https://www.aiposthub.com/blender-mcp-ai-3d-film-scene-workflow/)

## 費用與隱私：哪些免費、哪些要付錢

用 ChatGPT 方案、自己的金鑰或本機模型，都不會向 Loopcut 付費，付費的是 Loopcut 帳號提供的雲端服務與它自己的託管模型。

官網對 ChatGPT 方案與自己的金鑰的說法是「永遠免費、不是二等公民」，本機模型同樣標示免費。Loopcut 帳號則提供素材搜尋、圖片、圖片轉 3D、聲音與雲端渲染，這些從同一個點數餘額扣款，每項服務在執行前都會顯示美元價格。

定價頁列出三種方案，首次查詢日為 2026 年 10 月 21 日，額度倍數已在 10 月 25 日重新核對。

| 方案 | 月費 | 每月包含 | 重點 |
| --- | --- | --- | --- |
| Free | 0 美元 | 100 點（1 美元），最多 20 分鐘雲端渲染 | Loopcut 的 Fast 模型有每週額度 |
| Pro | 20 美元 | 22 美元點數，最多 7 小時雲端渲染 | 可用 Pro 模型，額度為 Free 的 7 倍（10 月 21 日查時寫 13 倍） |
| Max | 60 美元 | 66 美元點數，最多 22 小時雲端渲染 | 額度為 Pro 的 3 倍 |

點數換算是 1 點 1 美分。雲端渲染每 GPU 分鐘 0.05 美元，圖片每張 0.06 美元，標準品質，圖片轉 3D 模型每個 0.80 美元，語音每分鐘 0.10 美元。官網也說這些服務分階段開放，實際可用項目要以定價頁標示為準。

隱私要分三條路徑看，而且要分「模型請求」與「對話記錄」兩件事。依隱私政策，用 ChatGPT 方案時，模型請求從你的電腦直接送到 OpenAI，不經過 Loopcut 的伺服器，登入權杖也只存在你的電腦上。

自己的金鑰與本機模型的模型請求也一樣，不經過它的伺服器。只有用 Loopcut 託管的 Fast 或 Pro 模型時，請求才會經過它的閘道，閘道記錄用量，不儲存內容，但模型供應商會收到內容。

我 10 月 21 日查時，定價頁寫 Fast 用 Z.ai 的 glm-5.3-flash、Pro 用小米的 mimo-v2.6-pro。10 月 25 日再查，表格已經改了：Fast 是 openai/gpt-6-luna、Pro 是 openai/gpt-6.1-sol，都經 OpenRouter 轉給 OpenAI，處理地點在美國，OpenRouter 不保存內容；表格另外列了 Claude Opus 5.5，由 Anthropic、Amazon Bedrock 或 Google Vertex AI 經 OpenRouter 提供，Anthropic 最多保留 30 天。

小米的 mimo-v2.6-pro 現在只在主要模型忙碌時當備援，處理地點是新加坡，資料保存期限欄寫「No fixed period stated」，也就是未載明。表格標示各家都不拿請求訓練模型，這是 Loopcut 依各供應商條款整理的，最後確認日是 10 月 9 日。

不過不論哪條路徑，應用仍會連到 loopcut.org 取服務清單，並送出少量使用事件，例如應用啟動、設定步驟與每回合的結果，政策說不包含提問內容與場景。

依 0.2.5 的發布說明，這些匿名使用統計是這一版才加入，預設開啟，帶有隨機的安裝編號，可以在偏好設定的附加元件頁關閉。隱私政策另外寫到，登入 Loopcut 帳號時，應用還會回報你用的是哪一種模型來源，並記在帳號上。

另一件我 10 月 21 日寫這篇時沒有寫到、10 月 25 日重新核對隱私政策才補上的事，是對話記錄。隱私政策（更新日 10 月 20 日）與定價頁都寫，從 0.2.6 版起，不論你用 ChatGPT 方案、自己的金鑰、本機模型或 Loopcut 託管模型，應用都會把每次對話的記錄傳給 Loopcut，而且預設開啟。記錄內容包括你的提問、代理的回覆、工具呼叫與結果、錯誤，以及應用的記錄檔；不包括圖片、.blend 檔、金鑰與登入權杖。記錄存放 90 天，Loopcut 說不拿去訓練 AI 模型，也不賣給廣告商。

要關閉的話，到偏好設定的附加元件頁（Preferences › Add-ons › Loopcut）把「Share conversations to improve Loopcut」關掉，這只會停止之後的上傳；已經傳出的記錄要寄信到 hello@loopcut.org 請對方刪除。所以「模型請求不經過它的伺服器」這句話仍然成立，但不能再讀成「對話完全不會到 Loopcut」。

## 適合誰、不適合誰與導入前檢查

適合已經付 ChatGPT Plus 或 Pro、日常有大量命名、整理與燈光設定工作的 Blender 使用者，不適合要在重要專案上直接放手，或對不明來源安裝檔很敏感的人。

如果你的工作是需要美術判斷的造型與構圖，代理能幫上的只有周邊的技術雜務。反過來，批次改名、整理集合、設定渲染預設這類規則明確的工作，最容易看出它的價值。

試用之前，我會先做幾件事。先用專案的副本，不要直接開正式的.blend 檔。雖然官方說原檔不會被寫入，但這是剛起步的測試版，副本比較安心。

再來確認 ChatGPT 的用量設定。登入後到 chatgpt.com/settings/usage，替這個應用設一個你能接受的上限，免得長時間的視窗截圖與重試把方案額度用光。

如果你的場景或提問含有客戶資料，也先決定要不要關掉對話記錄分享，再開始第一場對話。

然後逐步核准程式碼，不要一開始就選「這場對話不再詢問」。第一個任務挑小的，例如替場景裡的物件依類型改名，看清楚它寫的程式碼與場景變更清單，再決定要不要放寬。

最後檢查磁碟空間。依開發者文件，每場對話的檢查點預設上限是 2 GB，超過後會先清掉最舊的，但第一個與最近五個會保留。

延伸閱讀： [Scenario Blender 外掛開源：把 AI 圖片、影片與材質帶進 3D 現場](https://www.aiposthub.com/scenario-blender-open-plugin-ai-assets/)

## 限制與風險：測試版、未簽章的安裝檔與程式碼權限

最大的風險是它還很新，其次是 macOS 版未簽章，加上代理能執行任意 Python。

GitHub 顯示倉庫建立於 9 月 19 日，我 10 月 21 日查詢時有 40 顆星，最新版是 0.2.5 測試版；10 月 25 日再查已有 158 顆星、21 個 fork，最新版是當天發布的 0.2.17（10 月 10 日到 25 日共發了 17 個版本）。版本更新很快，代表它還在密集修改，而使用者與回報都還不多，遇到問題時能參考的案例也少。

簽章的狀態，README 與發布說明寫得不一致。README 寫兩個平台都還沒簽章，但 0.2.5 的發布說明說，Windows 安裝程式與其中的程式都已簽章，Mac 版則尚未簽章。

10 月 25 日的 0.2.17 下載頁與發布說明仍是同樣的狀況：Windows 安裝程式已簽章，Mac 版尚未簽章，README 也還寫著兩個平台都未簽章。所以我以較新的發布說明為準。Mac 使用者第一次開啟時，需要到系統設定的隱私權與安全性按「仍要打開」。Windows 即使已簽章，發布說明也提醒，新簽章累積信任前，仍可能出現「Windows 已保護您的電腦」的警告。

README 另外提供了用終端機指令移除 macOS 隔離標記的做法，我建議不要照貼。

官方下載頁列出每個安裝檔的 SHA-256 雜湊值，並附上 shasum 與 Get-FileHash 的比對方式，也是先要求核對，才提到那個指令。比較穩的做法是確認下載來源是官網或 GitHub 發布頁，雜湊值相符之後，再自己決定是否放行。

發布說明還寫著，Windows 比 Mac 測試得少，遇到安裝或更新的異常請回報。另外，發布說明提到這個應用會自己更新，面板出現「Restart to update」時按下去就會裝新版，所以簽章與更新來源也值得持續留意。

代理的權限也要想清楚。開發者文件說，檔案工具讀取專案以外的位置、寫入與搬移都要等核准，而且不提供刪除。但 `run_python` 是在 Blender 裡以你的權限執行模型寫的 Python，所以核准畫面上的程式碼才是真正的把關，這是我的判斷。

另外，Loopcut 聲明與 Blender 基金會沒有關係，也沒有獲得它的背書。它的託管服務與網站不是開源的，只有應用與 Blender 分支採用 GPL。

最後是用量。ChatGPT 方案路線有 Plus 或 Pro 的資格限制，用量受你方案本身的限制約束，長時間的場景修改很可能碰到上限。

## Loopcut 常見問題

### Loopcut 是 OpenAI 或 Blender 官方的產品嗎？

都不是。它由獨立開發者發表，README 聲明基於 Blender 但不隸屬於 Blender 基金會，也沒有獲得背書。

它使用的 Sign in with ChatGPT 是 OpenAI 公開給開源應用的授權流程。Loopcut 的原始碼註解說這條流程不需要事先核准，OpenAI 文件也寫明不需要用戶端密鑰或夥伴金鑰，所以登入畫面不能當成 OpenAI 對這個應用的推薦。

### 一定要有 ChatGPT Plus 才能用 Loopcut 嗎？

不用。官網列了四種模型來源：ChatGPT 方案、自己的金鑰、本機模型與 Loopcut 託管模型。

自己的金鑰支援 Anthropic 與相容 OpenAI 格式的服務，本機模型支援 Ollama、LM Studio、llama.cpp 與 vLLM 這類工具。

### 我原本的.blend 檔與外掛還能用嗎？

可以匯入。第一次啟動時能選擇匯入 Blender 5.2 的設定、附加元件、快捷鍵與佈景。

官方說明 Loopcut 使用獨立的設定資料夾，所以不會動到原本安裝的 Blender 設定。

### 能在現有的 Blender 5.3 或其他版本使用嗎？

目前不行。它是以 Blender 5.2.2 為基礎的獨立分支，無法裝進其他版本的 Blender。

## 先在副本上跑一個小任務，再決定放手到哪裡

Loopcut 的價值在於讓已有 ChatGPT 訂閱的 Blender 使用者，不必另外處理 API 與 MCP，就能試用會寫程式、會截圖檢查、能復原的場景代理。

如果你想試，先下載官方版本，在專案副本上做一個命名或打光任務，檢查場景變更清單與 ChatGPT 的用量頁，再決定是否擴大使用。

想持續追蹤 Blender 與 AI 代理的實用更新，可以免費訂閱 AI 郵報，並定期回頭看 Loopcut 的發布頁，確認簽章與版本狀態。

## 更新紀錄

- 2026 年 10 月 25 日：重新核對 Loopcut 官方網站、GitHub 與隱私政策，更新版本與星數、託管模型表、Pro 額度倍數、文字轉動作點數、安裝檔大小與貼文數字；補上 0.2.6 起預設傳送對話記錄的說明，以及登入是否需要 Loopcut 帳號的不一致處；標註示範影片錄於 0.2.4 版。我仍沒有實際安裝或登入測試。

## 資料來源

- Loopcut 官方網站： [https://loopcut.org/](https://loopcut.org/?ref=aiposthub.com)
- Loopcut 隱私政策： [https://loopcut.org/privacy](https://loopcut.org/privacy?ref=aiposthub.com)
- Loopcut 定價頁： [https://loopcut.org/pricing](https://loopcut.org/pricing?ref=aiposthub.com)
- Loopcut 下載頁： [https://loopcut.org/download](https://loopcut.org/download?ref=aiposthub.com)
- Loopcut GitHub 倉庫（GPL-3.0-or-later）： [https://github.com/Samffprice/loopcut](https://github.com/Samffprice/loopcut?ref=aiposthub.com)
- Loopcut 發布頁： [https://github.com/Samffprice/loopcut/releases](https://github.com/Samffprice/loopcut/releases?ref=aiposthub.com)
- Loopcut 開發者文件 DEVELOPING.md： [https://github.com/Samffprice/loopcut/blob/HEAD/loopcut/DEVELOPING.md](https://github.com/Samffprice/loopcut/blob/HEAD/loopcut/DEVELOPING.md?ref=aiposthub.com)
- OpenAI 開發者文件，Sign in with ChatGPT： [https://developers.openai.com/siwc](https://developers.openai.com/siwc?ref=aiposthub.com)
- OpenAI 開發者文件，Sign in with ChatGPT Quickstart： [https://developers.openai.com/siwc/quickstart](https://developers.openai.com/siwc/quickstart?ref=aiposthub.com)
- OpenAI 開發者文件，ChatGPT plan usage in your open-source app： [https://developers.openai.com/siwc/token-sharing-open-source](https://developers.openai.com/siwc/token-sharing-open-source?ref=aiposthub.com)
- OpenAI 開發者文件，Preview limitations： [https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations?ref=aiposthub.com)
- OpenAI 開發者文件，UI/UX guidelines： [https://developers.openai.com/siwc/ui-ux-guidelines](https://developers.openai.com/siwc/ui-ux-guidelines?ref=aiposthub.com)
- 開發者 X 貼文（熱度線索，原作者發布）： [https://x.com/spuicey/status/2112344791635681477](https://x.com/spuicey/status/2112344791635681477?ref=aiposthub.com)