---
title: "Epic 的 Unreal Engine Claude Code 外掛：6 月底已上架，用法與風險"
source: "https://www.aiposthub.com/epic-unreal-engine-claude-code-plugin-official/"
author:
  - "[[Philo]]"
published: 2026-10-21
created: 2026-11-04
description: "Epic Games 的 Unreal Engine Claude Code 外掛讓 Claude 透過 MCP 操作執行中的 Unreal Editor。本文用官方倉庫、Claude 外掛市集與 UE 5.8 文件，整理上架時間、三項技能、安裝步驟、權限風險，以及哪些團隊現在適合試用。"
tags:
  - "clippings"
status: processed
---
10 月 20 日，一則中文貼文在 X 上熱傳，說 Epic Games 官方「剛剛」在 GitHub「悄悄開源」Unreal Engine Claude Code 外掛，並且已經正式上架 Anthropic 官方外掛市集。

貼文描述的情境很吸引人：Claude Code 可以「接管你正在運行的 Unreal Editor」，一句話就能建立 Actor、連藍圖節點、調材質。

但把官方倉庫與市集紀錄攤開來看，時間線對不上。GitHub 倉庫 6 月 8 日就已建立，外掛 6 月 26 日（美國時間）加進 Anthropic 的官方外掛清單，目前版本是 3.1.1。

所以這是一個已經上線幾個月、持續更新的外掛，只是被社群重新翻出來。

貼文提到的能力大多屬實。外掛確實能讓 Claude Code 透過 MCP 呼叫 Unreal Editor 裡數百個工具，包括生成物件、編輯藍圖與材質。

但它同時給 Claude 很大的操作權限，Epic 自己的 README 也直接警告，要把它當成執行助理寫的任意程式碼來看待。

我的判斷是，已經在用 UE 5.8 的團隊，可以在專案副本裡試用。想在正式專案上直接放手讓代理改資產的人，要先把版本控制與權限設好。

要看懂這個外掛，得先弄清楚它在 Claude Code 裡到底放了什麼。

## Unreal Engine Claude Code 外掛是什麼？Epic 放進去的三項技能與一個掛鉤

這個外掛本身很小，主要由三項技能和一支啟動掛鉤組成，真正的工具來自 Unreal Editor 內建的 MCP 伺服器。

MCP 是一種讓 AI 程式呼叫外部工具的通訊規格。Epic 在 UE 5.8 文件裡說明，Unreal MCP 會在編輯器程序內開一個伺服器，Claude Code 這類客戶端用本機 HTTP 連進去。

外掛就是教 Claude 如何正確使用這條連線。倉庫裡有三項技能：unreal-mcp 負責教 Claude 操作執行中的編輯器，create-toolset 教人替引擎新增工具，unreal-skill 教人替專案寫專屬的代理技能。

README 的目錄只列了 unreal-mcp，但倉庫實際有三個技能資料夾，所以貼文說的「三大技能」並沒有錯，只是 README 的目錄沒有列全。

另一個元件是 SessionStart 掛鉤，也就是 Claude Code 每次開啟或恢復工作階段時自動執行的小腳本。它會往上找.uproject 檔，確認這是 Unreal 專案，再提示 Claude 優先採用 UObject、Slate 與 UHT 這些 Unreal 慣例。

外掛沒有附現成的.mcp.json 設定檔，連線設定要由編輯器的控制台指令產生。授權是 MIT，Epic 的版權聲明寫在 LICENSE 裡。

在 Claude 的外掛目錄頁，它顯示為「Unreal Engine MCP」，標示「Made by Epic Games」，我在 10 月 21 日查詢時顯示約 871 次安裝。

官方倉庫的安裝說明是：在 Claude Code 裡執行 /plugin install unreal-engine-skills-for-claude-code@claude-plugins-official。

團隊共用的話，可以把外掛寫進專案的.claude/settings.json，成員信任資料夾後會被提示安裝。

說到「上架多久」，這正是貼文最需要更正的地方。

![GitHub 預覽卡：EpicGames 的 unreal-engine-skills-for-claude-code-plugin 倉庫，顯示 Epic Games 標誌](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/epic-unreal-claude-code-plugin-cover.png)

GitHub 為 Epic Games 官方倉庫 unreal-engine-skills-for-claude-code-plugin 自動產生的預覽卡，含倉庫名稱與 Epic 標誌，非本站拍攝／實測。 圖片來源： 官方原文 。

## 貼文說「剛開源」？倉庫 6 月 8 日就建立，6 月底上架

貼文的時間感錯了，但外掛的重點資訊大多正確，下表把可以查證的日期放在一起。

| 日期（2026） | 事件 | 來源 |
| --- | --- | --- |
| 4 月 7 日 | 提交紀錄中最早一筆的日期，GitHub 倉庫則是 6 月 8 日才建立 | GitHub 提交紀錄與倉庫資訊 |
| 6 月 26 日 | 外掛加入 claude-plugins-official 清單，提交時間為美國中部時區 | Anthropic 市集倉庫提交 |
| 7 月 21 日 | 安裝說明改為指向官方市集，版本 3.0.4 | GitHub 提交紀錄 |
| 8 月 20 日 | Epic 論壇公告，Unreal MCP 隨 UE 5.8 出貨，並開放到 UEFN | Epic 開發者論壇 |
| 9 月 10 日 | 補上代理程式的連線恢復說明，版本 3.1.0 | GitHub 提交紀錄 |
| 9 月 17 日 | 調整為允許安全的並行工具呼叫 | GitHub 提交紀錄 |

表中日期以各筆紀錄的時區為準，換成台灣時間可能差一天。因為提交日期可以由作者端設定，判斷公開時間時，我以倉庫建立日與市集提交這兩個平台紀錄為準。這張表的意思是，10 月 20 日的貼文只是讓外掛在社群被重新看見，外掛早就上架了。

那則貼文我在 10 月 21 日早上查詢時約有 1,300 個讚，這只能當作熱度線索。

站內 10 月 9 日的舊文寫的是，Unreal Engine 官方 X 帳號在 10 月 8 日介紹 Unreal MCP，那是對外宣傳的日子。

Epic 論壇 8 月 20 日的公告已寫明 Unreal MCP 隨 UE 5.8 出貨，本篇外掛的倉庫與市集紀錄更早到 6 月，所以兩種說法並不矛盾，只是起算點不同。

貼文裡其他說法，我逐項對了官方內容。「30 多個工具集、數百個工具」與 README 一致。「預設只露出三個入口工具」也與 UE 5.8 文件一致，這個設計叫工具搜尋，預設開啟。

貼文另外寫，三個入口工具的上下文開銷極低，約 250 個詞元以上，這個數字我在 Epic 的倉庫與 UE 文件裡都找不到出處，只能當作貼文作者的估算。

詞元是模型計算文字量與費用的單位，所以入口工具愈少，每次對話預先塞進去的說明就愈短。Epic 的說法比較保守：UE 文件只說這能讓工具清單保持精簡，外掛 README 另外說明能讓提示快取維持有效，兩邊都沒有給詞元數字。

貼文用「接管」來形容 Claude 的操作範圍。這句話要連同工具怎麼被找到、被執行一起看，也就是下一節的運作方式。

## 外掛怎麼讓 Claude 操作編輯器？先找工具，再用同一個入口執行

Claude 不會一開始就看到數百個工具，而是先問有哪些工具集，讀完說明再呼叫。

UE 5.8 文件寫得很清楚，工具搜尋模式下只有 `list_toolsets` 、 `describe_toolset` 與 `call_tool` 三個入口。前兩個回傳工具集名稱與參數說明， `call_tool` 則把指令送進編輯器，並在同一回合回傳結果。

這樣設計的好處是，工具定義不會一次塞滿對話。倉庫的技能也說，這能讓提示快取保持溫熱。

工具涵蓋的範圍很廣。README 列出場景物件、藍圖、材質、Niagara 特效、Control Rig、Sequencer、UMG 介面、遊戲能力系統與自動化測試，還有一個能把多個動作打包成單一 Python 腳本的工具。

技能 unreal-mcp 還寫了幾條安全守則。大批量修改前後都要先存檔，因為這些操作不一定能復原。程式碼編譯或著色器編譯進行時，工具呼叫可能卡住或出現難以判讀的失敗，所以要等編譯完成。

另外要檢查每次工具回傳的結果，不能只看有沒有丟例外，並且留意 Play-in-Editor 開著時，資產建立的行為會不同。

create-toolset 則是給想擴充引擎能力的團隊。技能要求新工具以靜態函式實作，文件要寫得精簡，測試是強制項目。語言由使用者決定，但技能建議優先用 Python，因為比較容易迭代。

unreal-skill 教你把專案專屬的做法寫成代理技能，例如命名規則或資料夾結構。技能可以寫成 Python 類別，或直接在內容瀏覽器建立成 UAsset，後者不需要寫程式。

了解運作方式後，就能看實際要做哪些設定。

延伸閱讀： [Unreal MCP 官方登場：AI 能操作遊戲編輯器，專案完成還差哪些步驟？](https://www.aiposthub.com/unreal-mcp-official-editor-tools-experimental/)

## 怎麼安裝與連線？從啟用引擎外掛到第一個唯讀指令

安裝的重點在 Unreal 這一端，因為外掛只是說明書，工具要靠引擎的兩個外掛提供。

README 的前提是啟用 ModelContextProtocol 與 AllToolsets 兩個引擎外掛。AllToolsets 才提供工具，只開前者的話，伺服器會啟動，但沒有任何工具可用。

接著要讓伺服器跑起來。在編輯器控制台輸入 ModelContextProtocol.StartServer，或在偏好設定裡開啟自動啟動，預設位址是 127.0.0.1 的 8000 埠，路徑為 /mcp。

然後在控制台執行 ModelContextProtocol.GenerateClientConfig ClaudeCode，編輯器會寫出.mcp.json。已安裝版引擎的檔案放在.uproject 旁邊，原始碼版引擎則放在工作區根目錄。

最後從專案根目錄啟動 Claude Code，輸入 /plugin 確認外掛已啟用，再輸入 /mcp 確認 unreal-mcp 已連線。

第一個指令建議選唯讀的，例如請 Claude 列出目前關卡的所有 Actor。能順利回傳，才表示整條線路通了，不用拿會改資產的指令當測試。

如果編輯器常需要重新編譯或重開，README 還提供選用的連線代理 `unreal_mcp_proxy` 。它能在編輯器關閉時維持 Claude 端的連線，編輯器回來後自動重連。

不過代理保存的是上次的工具清單，官方特別提醒，看得到工具不代表連得到編輯器，要用唯讀呼叫確認。

Windows 還有一個細節。掛鉤需要 bash，所以要裝 Git for Windows 或使用 WSL，沒有的話外掛的工具仍可用，只是少了啟動提示。

## 適合誰、不適合誰與導入前檢查

適合已經使用 UE 5.8、並且願意用專案副本做小任務的團隊，不適合想直接在正式專案上讓代理大量改資產的人。

這個外掛省下的是反覆開面板、點選與命名的時間。像是批次改材質參數、替一批 Actor 重新命名、跑自動化測試，這類規則明確的工作最容易驗收。

需要美術判斷或關卡手感的工作，就算能操作也不代表做得好。每次結果仍要由人看過，這點和 Epic 在 UE 5.8 文件說的實驗性質相符。

試用前，我會先做幾件事。把專案存檔並提交到版本控制，Epic 的 README 也要求長時間操作前先存檔或擱置變更，這樣萬一出現意外結果才救得回來。

再來確認 Claude Code 的權限模式。README 建議，載入外掛時盡量不要使用 --dangerously-skip-permissions，如果一定要用，就把提示寫窄、以讀取為主，並在可拋棄的專案裡進行。

另外，確認連線埠沒被占用，並決定要用 AllToolsets 還是只啟用需要的工具集。文件說明，只想開放部分能力時，可以單獨啟用各個工具集外掛。

實際交給 Claude 的第一批任務，我會挑能立刻驗收的。例如替關卡裡的 Actor 依類型重新命名，驗收方式是打開 Outliner 看名稱。

再來是替一批材質實例調整同一個參數，驗收方式是在視窗裡比對前後畫面。README 說工具能擷取編輯器畫面並檢查輸出日誌，所以也可以要求 Claude 自己截圖確認，不必只靠它的文字回報。

自動化測試也適合，因為測試有明確的通過與失敗結果。Claude 能探索並執行 C++ 自動化測試，並讀回詳細結果，你只要看失敗的項目就好。

延伸閱讀： [Epic AI 開發轉向解析：Tim Sweeney 說要做更多作品，Unreal 團隊該怎麼準備](https://www.aiposthub.com/epic-tim-sweeney-ai-development-unreal-workflow/)

## 限制與風險：無驗證的本機連線與可執行 Python 的工具

最大的風險來自權限，其次是仍在實驗階段的功能。

Epic 在 README 寫道，本機網址並不是信任邊界。伺服器綁定 localhost，會檢查來源以防瀏覽器分頁連入，但同一台電腦上以同一使用者執行的任何程式都能連。

UE 5.8 文件也說明，這個外掛沒有驗證層，不適合暴露到本機以外。所以共用電腦與不受信任的機器，都不該開啟這個伺服器。

`ProgrammaticToolset.execute_tool_script` 這個工具會在編輯器程序裡執行任意 Python，能存取專案檔案與資產資料庫。README 的說法是，每次呼叫都要當成高權限操作。

還有一個容易被忽略的差異。UE 5.8 文件說，工具呼叫在遊戲執行緒上依序執行，客戶端不該同時送出重疊的呼叫。

但外掛 9 月 17 日的提交與目前技能卻寫著，伺服器接受並行請求並自行處理同步，只是有相依關係的呼叫仍要依序送出。兩份官方文件的更新時間不同，所以我建議以你實際使用的引擎版本為準。

UE 文件另列了遙測設定：ModelContextProtocol.EnableAnalytics 預設為 true，說明只有一句，控制遙測是否送出。這一頁沒有列出送出的內容，團隊若有資料政策，要自己再查。

傳輸方式也有限。UE 文件說只支援 HTTP 與 Server-Sent Events，不支援 stdio 與 WebSocket，選客戶端時要先確認它能連 HTTP。

另外，即時編譯不會帶入新增的 UFUNCTION，新增工具需要重開編輯器。文件還明說，功能多半未完成，介面與資料格式隨時可能改變。

## Unreal Engine Claude Code 外掛常見問題

### 這個 Unreal Engine Claude Code 外掛要付費嗎？

外掛本身是 MIT 授權的開源專案，倉庫裡沒有附加費用條款。

使用 Claude Code 需要的帳號與用量，則依 Anthropic 自己的方案計算，這部分不在 Epic 外掛的範圍。

### 只有 Claude Code 能連 Unreal MCP 嗎？

不是。UE 5.8 文件說，任何相容 MCP 的客戶端都能連，控制台的 GenerateClientConfig 也支援 Cursor、VS Code、Gemini 與 Codex 的設定檔。

這個外掛專為 Claude Code 準備技能與掛鉤，其他客戶端需要自己補類似的說明。

### 一定要 UE 5.8 嗎？

README 沒有列出支援的引擎版本，但它的前提是引擎提供 ModelContextProtocol 與 AllToolsets，而 Epic 文件把它們放在 UE 5.8 的說明裡。

所以我會以 5.8 為準，其他版本要先確認有沒有這兩個外掛。

### 連上了卻看不到 Unreal 的工具，該怎麼查？

外掛的操作說明列了幾個常見原因。先確認設定檔裡是 unreal-mcp 還是 unreal-mcp-proxy，再看編輯器的 MCP 啟動紀錄，是否出現連接埠被占用的訊息。

如果缺的是某個工具集，例如 Niagara，就在控制台執行 ModelContextProtocol.RefreshTools，仍然沒有的話，再檢查該工具集的外掛有沒有在.uproject 啟用。

### 可以隨遊戲一起出貨嗎？

文件說明，封裝版遊戲技術上可以呼叫模組啟動 MCP 伺服器，但註冊工具的方式不同，而且整體仍屬實驗功能，Epic 提醒出貨時要小心。

## 先在專案副本試一個唯讀指令，再決定放權到哪裡

這個外掛的價值，在於把 Epic 對 Claude 使用 Unreal Editor 的建議寫成技能，讓代理少走冤枉路。

它是已經運作幾個月的工具，離放手交付還有一段距離。如果你想試，先複製一份專案，啟用兩個引擎外掛，執行唯讀的 Actor 列表，確認能連線之後，再慢慢交給 Claude 批次命名或改材質這類小任務。

想持續追蹤遊戲引擎與代理工具的實用更新，可以免費訂閱 AI 郵報，並回頭核對 Epic 與 Anthropic 官方頁面的版本號。

## 資料來源

- EpicGames/unreal-engine-skills-for-claude-code-plugin（MIT）： [https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin](https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin?ref=aiposthub.com)
- 外掛 README： [https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin/blob/main/README.md](https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin/blob/main/README.md?ref=aiposthub.com)
- unreal-mcp 技能： [https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin/blob/main/skills/unreal-mcp/SKILL.md](https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin/blob/main/skills/unreal-mcp/SKILL.md?ref=aiposthub.com)
- Claude 外掛目錄頁，Unreal Engine MCP： [https://claude.com/plugins/unreal-engine-skills-for-claude-code](https://claude.com/plugins/unreal-engine-skills-for-claude-code?ref=aiposthub.com)
- Anthropic 官方外掛清單 claude-plugins-official： [https://github.com/anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official?ref=aiposthub.com)
- Epic 開發者文件，Unreal MCP in Unreal Editor（UE 5.8）： [https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-mcp-in-unreal-editor](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-mcp-in-unreal-editor?ref=aiposthub.com)
- Epic 開發者論壇，Unreal MCP is now available in UEFN： [https://forums.unrealengine.com/t/unreal-mcp-is-now-available-in-uefn/2745278](https://forums.unrealengine.com/t/unreal-mcp-is-now-available-in-uefn/2745278?ref=aiposthub.com)
- 中文貼文（熱度線索，社群轉述）： [https://x.com/mylifcc/status/2112408469903114580](https://x.com/mylifcc/status/2112408469903114580?ref=aiposthub.com)