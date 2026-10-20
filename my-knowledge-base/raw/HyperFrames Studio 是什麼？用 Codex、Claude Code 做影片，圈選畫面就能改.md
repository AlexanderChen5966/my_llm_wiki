---
title: "HyperFrames Studio 是什麼？用 Codex、Claude Code 做影片，圈選畫面就能改"
source: "https://www.aiposthub.com/hyperframes-studio-app-codex-claude-code-video-editor/"
author:
  - "[[Philo]]"
published: 2026-10-06
created: 2026-10-06
description: "HyperFrames Studio 推出桌面影片編輯器，讓 Codex、Claude Code 與人共用時間軸。本文整理 Mac、Linux 下載、帳號費用、圈選修改與 MP4 匯出條件。"
tags:
  - "clippings"
status: processed
---
HyperFrames Studio 是讓你用文字指揮 AI 製作影片，再用畫面與時間軸調整結果的桌面編輯器。它由開源 HyperFrames 的團隊推出，將 Claude Code、Codex 這類能讀取檔案、修改專案並執行工作的 AI 代理，接進同一個影片工作介面。

做過 AI 短影音的人，通常最花時間的部分發生在第一版完成之後。標題要往左移、某個畫面要短一點、產品截圖要換掉，光靠聊天描述位置，很容易來回好幾次。HyperFrames Studio 把修改要求接到實際畫面上，讓人可以圈出位置、釘上留言，也能直接調整時間軸。

台灣時間 2026 年 10 月 6 日，HyperFrames 在 [官方發布貼文](https://x.com/HyperFrames_/status/2107149824189698558?ref=aiposthub.com) 介紹這款新 App。這次發布的重點，是讓 AI 做出的專案留在可共同編輯的環境裡。對已經使用 Codex 或 Claude Code、需要反覆調整產品短片的人，這是一個值得嘗試的方向。

本文依官方下載頁與 Studio App 文件，整理它能怎麼製作、修改與匯出影片，以及開始前需要的帳號和使用條件。文中的操作情境為說明範例，功能判斷以官方資料為依據。

## HyperFrames Studio 與開源 HyperFrames 有什麼差別？

HyperFrames 提供的是可由程式描述的影片專案。HTML 負責畫面元素與結構，CSS 負責顏色、字體和排版，動畫設定則決定它們何時出現、如何移動。因為 AI 代理可以修改這些檔案，影片的標題、素材與節奏就能隨著指令調整，再輸出成一般播放器能播放的檔案。

依 [官方 GitHub 說明](https://github.com/heygen-com/hyperframes?ref=aiposthub.com) ，這套開源框架已提供命令列工具、預覽、渲染與編輯相關元件。渲染就是把專案中的畫面和聲音，依時間順序製成影片檔。熟悉這條路線的人，可以在自己選擇的 AI 開發工具裡處理專案。

新推出的 Studio App 把這些工作安排進桌面介面。你在左側描述需求，中間看影片，下面檢查時間軸，再把修改交給連接的 AI 代理。它保留手動編輯的入口，也讓畫面上的選取與註記成為 AI 理解要求的線索。

| 比較項目 | HyperFrames 開源工作流 | HyperFrames Studio App |
| --- | --- | --- |
| 開始方式 | 在 AI 工具或終端機建立影片專案 | 從桌面 App 的提示框或範例開始 |
| 修改方式 | 讓代理修改專案檔案，再看預覽 | 在同一介面打字、選取、畫圖、留言或調整片段 |
| 主要價值 | 可自行安排工具與自動化流程 | 集中需求、預覽與修改，減少切換介面的次數 |
| 底層專案 | 可編輯的 HyperFrames 專案 | 官方文件說明仍是一般 HyperFrames 專案資料夾 |

這個差別會影響你選擇哪個入口。如果原本的自動製片流程已經穩定，開源工具仍有用途。若常卡在「影片做出來了，但很難把修改講清楚」，Studio App 的視覺編輯介面就更切中問題。

延伸閱讀： [HyperFrames x Codex 快速上手：安裝、工作流、案例分享](https://www.aiposthub.com/hyperframes-codex-quickstart-guide/)

![HyperFrames Studio 首頁，包含 Make something 提示框、左側專案與 Get inspired 參考區](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/hyperframes-studio-20261006-index.jpg)

Studio App 首頁提供輸入需求、開啟專案與尋找參考的入口。 圖片來源： HyperFrames 官方文件 。

## 下載前先確認平台、HeyGen 登入與 AI 帳號

截至 2026 年 10 月 6 日， [官方下載頁](https://hyperframes.dev/studio?ref=aiposthub.com) 提供 macOS 和 Linux 版本，Windows 與 Windows arm64 標示為 Coming soon。Linux 文件列出的安裝檔為 x86\_64 AppImage，對應 64 位元 Intel 或 AMD 電腦。Windows 使用者需要等待桌面版本，不能直接套用 Mac 的安裝步驟。

App 本身的登入與 AI 代理的登入是兩件事。 [官方首次啟動說明](https://hyperframes.heygen.com/studio-app/index?ref=aiposthub.com) 要求透過 HeyGen 帳號登入 HyperFrames。開始請 AI 製作影片之前，還需要連接 Claude Code 或 Codex。前者使用 Claude 或 Anthropic Console 帳號，後者使用 ChatGPT 帳號。

依 [代理連接文件](https://hyperframes.heygen.com/studio-app/connect-your-agent?ref=aiposthub.com) ，Studio 可以協助安裝 Claude Code，Codex 則需要使用者依指示安裝與登入。兩個代理都準備好、又沒有另作選擇時，App 預設使用 Claude Code。之後可從聊天區的代理選單，或 Settings 裡的 Agent 項目切換，選擇會套用到下一次要求。

費用也要分開理解。官方明確說明，AI 執行用量計入代理登入帳號的方案，也提供改用 API key 的設定。API key 是讓工具連接模型服務並計費的存取金鑰。開源框架採 Apache-2.0 授權，並不代表影片製作過程的模型呼叫或外部素材服務都免費。

對已經訂閱相關 AI 服務的人，先做一支短片就能觀察自己的用量和修改次數。若尚未準備 AI 帳號，應先把代理連接與計費方式弄清楚，再安排固定產量。下載頁與本文引用的 App 文件沒有提供可套用到每支影片的統一總價。

![HyperFrames Studio 第一支影片引導，以 Hook、Proof、Turn、Call to action 四段建立短片](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/hyperframes-studio-20261006-first-video.jpg)

第一支影片引導將發布短片分成四段，並提供網站或想法的輸入欄位。 圖片來源： HyperFrames 官方文件 。

## 第一支影片可以從四段結構開始

Studio 的入門方式提供了明確的敘事骨架。 [官方第一支影片教學](https://hyperframes.heygen.com/studio-app/first-video?ref=aiposthub.com) 把產品發布短片分成 Hook、Proof、Turn 和 Call to action。你先挑選各段參考，再輸入網站或想法，按下 Use this cut，讓代理建立專案。

這四個名稱可以直接理解為「吸引注意、展示證據、帶出解法、提出行動」。它們讓 AI 在開工前就有內容順序，對不知道如何寫分鏡的新手尤其有幫助。

| 影片部分 | 要回答的問題 | 課程短片的說明範例 |
| --- | --- | --- |
| Hook：開頭吸引注意 | 觀眾為什麼要停下來看？ | 呈現做第一支動畫時常遇到的困難 |
| Proof：展示證據 | 產品能做出什麼具體成果？ | 展示一段實際完成的教學動畫 |
| Turn：帶出解法 | 如何從困難走到成果？ | 說明用指令建立，再逐段修改的方式 |
| Call to action：行動呼籲 | 看完後要做什麼？ | 顯示課程名稱與查看介紹的邀請 |

官方文件也說明，提供網站可以讓代理取得字體、配色與標誌作為品牌參考。你仍應補上影片目的和要說的重點，避免只丟一個網址，期待 AI 自行決定所有訊息。品牌外觀回答「看起來像誰」，內容需求則回答「這支影片要讓人記住什麼」。

例如，可以先提出一個範圍清楚的需求：「製作 20 秒直式課程介紹，前三秒呈現新手做動畫的困難，中段展示我提供的成果影片，最後顯示課程名稱。使用繁體中文，畫面上每次只放一個重點。」這是本文建議的提示詞範例，實際結果仍需要預覽後調整。

如果已有分鏡，也可以直接從首頁提示框開始。官方支援用 @ 加入檔案、其他專案或媒體。範例庫則讓人把看到的畫面當作參考帶進要求，降低從空白畫面開始的難度。

<video src="https://static.heygen.ai/hyperframes-oss/site/studio-launch-nib-sf-audio.mp4" width="1920" height="1080" controls=""></video>

HyperFrames Studio 官方發布影片，展示產品定位與編輯方式。影片來源： HyperFrames Studio 官方網站 。

## 圈選、畫圖與留言，讓修改對準同一個畫面

Studio 最有辨識度的功能，是讓「要改哪裡」變成具體的視覺位置。 [官方編輯文件](https://hyperframes.heygen.com/studio-app/edit-with-the-agent?ref=aiposthub.com) 列出三種工具：Select 選取元素、Draw 在畫面上圈畫、Comment 把留言釘在指定位置與時間。你可以指出一段文字，再描述要放大、換色或移動的方式。

畫圖也能用在時間軸上，指向一段需要縮短或調整的影片。這對「中間有點拖」這種模糊回饋很有用，因為代理能取得更明確的修改範圍。位置搭配理由，比只有「更有質感」容易形成可以檢查的結果。

不過，圈選或留言本身不會立即執行修改。它們會成為 Pending video edits，也就是等待送出的影片修改清單。官方文件列出的上限是 10 項，你可以先檢查要求，再加上一段共同說明，合併送給代理處理。

這種做法適合把同一輪回饋一起交代，例如標題位置、某段長度與最後一幕的文字。送出之前，先確認每項要求都指向你想改的畫面，能減少代理完成後才發現漏掉或改錯位置的情況。

本文建議先把影片停在問題出現的時間，圈出元素，再寫明修改與保留條件，例如「放大這個標題，維持置中，底下的說明文字不動」。修改完成後回到同一個時間點檢查，再播放前後幾秒，確認動畫和節奏也合理。

## 人可以直接改時間軸，不必把每個動作都交給 AI

共用時間軸的價值，在於你能依工作難度選擇修改方式。官方說明支援拖動片段位置、修剪長度、在播放頭位置分割片段，以及刪除片段。影片與音訊片段的 Quick edit 還能調整速度、音量和淡入淡出。

如果只是把結尾多留一點時間，直接拖動片段可能更快。涉及整段畫面重排、換素材或重新設計時，再交給 AI 處理比較合理。人與代理使用同一個專案，讓小修和大修可以接續進行。

App 也提供一般復原操作，每次代理執行在聊天區還有自己的 Undo 按鈕。這讓嘗試不同剪法時有退回上一版的入口。對內容工作者而言，能否方便修正與復原，會直接影響這個工具能否融入日常產出。

本文因此更看重它對反覆修改的支援。第一版製作速度固然有吸引力，但需要驗收的產品短片，常常會改到第三版、第四版。若每次修改都能清楚定位、查看結果並復原，工具的價值才會在完整製作流程裡累積。

延伸閱讀： [Claude Code 剪影片教學：搭配 HyperFrames 自動加字幕、動畫與輸出 MP4](https://www.aiposthub.com/claude-code-hyperframes-video-editing-guide/)

![HyperFrames Studio 匯出面板，包含品質、MP4、WebM、MOV 與影格率選項](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/hyperframes-studio-20261006-export.jpg)

官方匯出面板展示影片品質、格式與影格率設定。 圖片來源： HyperFrames 官方文件 。

## 匯出支援 MP4、WebM、MOV，4K 有專案條件

影片在編輯器裡看起來正常之後，還要做成可以交付的檔案。 [官方匯出文件](https://hyperframes.heygen.com/studio-app/export?ref=aiposthub.com) 提供 MP4、WebM 和 MOV 三種格式，影格率可選 24、30 或 60。影格率是每秒播放的畫面張數，會影響動作呈現的流暢程度。

匯出預設為 1080p、MP4、每秒 30 張畫面，適合先做一般分享版本。品質選項也包括專案原始尺寸，4K 則要在專案符合條件時才會提供。若目的是手機觀看，應先確認小螢幕上的字體大小和畫面順序，再決定是否需要更高解析度。

AI 仍在建立專案時，Export 無法使用。執行完成後，按下匯出按鈕，App 會依序準備、渲染並完成檔案。影片儲存在 Downloads 資料夾，完成後可以直接開啟播放，或在檔案總管中查看。

交付前應把匯出的成品從頭看完。預覽用來修改，成品檔則用來確認聲音、轉場與結尾有沒有完整輸出。若這一步出現問題，就回到原專案修正，避免把不完整的檔案交給客戶或排入社群發布。

## 適合先試產品短片，採用前看完整修改成本

從官方提供的四段入門結構、品牌網站參考與視覺回饋方式來看，產品介紹、功能發布、課程宣傳與社群短片，是合理的起步情境。這些內容通常目標清楚、篇幅短，也常需要調整文字和素材。

已經使用 Codex 或 Claude Code 的創作者，可以先拿一個熟悉的題目做小規模比較。記下第一版需要多久、修改幾次、花多少 AI 用量，以及最後有沒有成功匯出。這些紀錄比單看官方影片，更能回答工具是否節省了自己的工作時間。

若工作主要是大量長篇原始素材的剪輯、複雜的聲音製作，或已建立成熟的專業剪輯流程，這次發布資料還不足以證明 Studio 可以完整取代原有工具。可以先用它做片頭、圖文動畫或一支獨立短片，從較容易驗收的成果開始評估。

專案管理也需要理解。依 [官方故障排除文件](https://hyperframes.heygen.com/studio-app/troubleshooting?ref=aiposthub.com) ，專案存放在電腦的 HyperFrames Studio 資料夾，並可用既有 Studio 網頁編輯器或命令列工具開啟。這保留了繼續處理專案的彈性，但 AI 代理仍連接其服務，本機存放專案不能直接理解為完全離線製作。

本文對這款產品的判斷，是值得讓現有 AI 代理使用者試做一支短片。採用它的理由應該是修改更清楚、交付更順利。如果處理完一輪真實需求後，仍需要花大量時間重做畫面或整理錯誤，就應縮小使用範圍，保留原本效率更好的工具。

## HyperFrames Studio 常見問題

### 不會寫程式也能使用嗎？

官方把 Studio App 定位為不必自己寫程式的入口。你可以用文字、圈選和留言提出要求，讓代理處理專案。開始前仍需要完成 App 登入與代理設定，並自行檢查生成內容。

### Windows 可以下載嗎？

2026 年 10 月 6 日查詢時，下載頁的 Windows 和 Windows arm64 都標示 Coming soon。可下載的平台為 macOS 與 Linux，使用條件應以官方下載頁後續更新為準。

### 一定要使用 Claude Code 嗎？

官方 App 文件也支援 Codex，可以從代理選單切換。AI 用量會計入所連接帳號的方案，因此應選擇自己已設定好、且有可用額度的代理。

### 圈起來之後，AI 就會立刻修改嗎？

圈選、繪圖與留言會先進入等待送出的修改清單。你需要檢查內容，再按送出交給代理。這個流程讓同一輪回饋可以合併處理。

### 能輸出 4K 影片嗎？

官方文件說明，專案符合條件時會提供 4K 選項。預設匯出為 1080p MP4，不能把 4K 理解為所有專案都會有的預設能力。

## 從一支熟悉的短片，確認它是否改善修改流程

HyperFrames Studio 把影片製作接到 AI 代理，也讓人保留對畫面與時間軸的直接控制。先選一支原本就知道怎麼驗收的短片，完成建立、回饋、手動微調與匯出，就能看見它是否減少日常工作中的往返。

如果你的主要瓶頸是反覆描述修改位置，可以從 [HyperFrames Studio 官方下載頁](https://hyperframes.dev/studio?ref=aiposthub.com) 開始。想補足 AI 影片製作的基礎流程，也可以查看 [AI 郵報學院](https://academy.aiposthub.com/?ref=aiposthub.com) 的課程介紹，再依自己的工作需求選擇學習方向。