---
title: "Claude Code快捷鍵+指令大全！13大類速查不用背，從Ctrl+C到多Agent協作一次整理"
source: "https://www.bnext.com.tw/article/90925/claude-code-slash-commands-shortcuts-complete-guide"
author:
  - "[[數位時代 BusinessNext]]"
published: 2026-08-19
created: 2026-08-19
description: "多數人只把 Claude Code 當成「輸入需求、等待回覆」的聊天助手，善用快捷鍵與確定性指令，能比自然語言互動更快、更精準。"
tags:
  - "clippings"
status: "processed"
---
[![](https://bnextmedia.s3.hicloud.net.tw/pumpkin/image/photo/2026-08/img-1785684158-19300.gif)](https://meetgreatersouth.tw/?utm_source=web_bn&utm_medium=logo_banner&utm_campaign=2026mgs&utm_content=167926&utm_term=channel_all)

2026.08.19 | [AI與大數據](https://www.bnext.com.tw/categories/ai)

多數人只把 Claude Code 當成「輸入需求、等待回覆」的聊天助手，善用快捷鍵與確定性指令，能比自然語言互動更快、更精準。

[＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude) [＃Claude Code](https://www.bnext.com.tw/tags/Claude%20Code)

---

> 最後更新：2026/8/19，新增/design快捷鍵

多數人使用 Claude Code 的方式很單純：輸入需求、等待回覆、不滿意就重來。但事實上，從壓縮對話、倒帶操作、切換模型，到安全審查、排程工作、管理記憶體，許多你以為要手動處理的事，Claude Code 都有對應的斜線指令可以直接觸發。

以下內容整理自 Claude Code 官方文件，將官方整理的內建指令以及快捷鍵依應用情境分類，方便大家查找與上手，快收藏起來吧！

## 在 Claude Code 會幫你處理事情的前提下，還有必要了解指令嗎？

答案是：有，但理由變了。以前學指令是因為「不知道指令就做不到」，現在的理由是 **效率與精準度** 。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

**指令比對話快。** `/compact` 一秒執行完，用對話說「幫我壓縮目前的上下文，重點保留認證邏輯」，Claude 要先理解意圖、確認、再執行，而直接使用指令可以省下更多的溝通成本。

**指令觸發的是確定性行為。** `/rewind` 就是倒帶，不會因為措辭模糊而執行到一半。用對話請 Claude「回到剛才那個版本」，有可能它理解的「剛才」跟你的不一定一樣。

**部分指令無法用對話替代。** `/tui fullscreen` 、 `/terminal-setup` 、 `/setup-bedrock` 這類改變 CLI 本身設定的指令，系統必須由使用者明確輸入指令來觸發，無法靠存在模糊空間的口語要求來執行。

快捷鍵處理的則是「對話做不到的事」：你沒辦法對 Claude 說「幫我把選取範圍向右延伸」或「把這段提示送到外部編輯器」，這些操作只存在於介面層，Claude 根本無法透過回覆執行。另外，快捷鍵裡有幾個高頻操作（ `Ctrl+C` 中斷、 `Ctrl+R` 搜尋歷史、 `Shift+Tab` 切換權限模式），一旦熟悉這些快捷鍵，在操作上就能更流暢。

## Claude Code 指令大全

下面先提供2張可以直接存下來參考的圖：

![#0 Claude Code指令大全](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/a6ch-1778660938.png?w=1200&output=webp)

圖／ ChatGPT生成

![#2 Claude Code指令大全](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/zjtm-1778660940.png?w=1200&output=webp)

圖／ ChatGPT生成

### 一、對話與上下文管理

| 指令 | 說明 |
| --- | --- |
| `/btw <問題>` | 提出側邊問題， **不寫入對話歷史** ，不污染主線 |
| `/compact [焦點說明]` | 壓縮對話歷史釋放上下文，可指定保留重點（例： `/compact 保留認證邏輯討論` ） |
| `/context [all]` | 視覺化上下文使用量，標出記憶體膨脹點與最佳化建議；傳入 `all` 展開每項細節 |
| `/clear [名稱]` | 清空上下文開啟新對話，舊對話仍可用 `/resume` 取回；可傳名稱供日後辨識。別名： `/reset` 、 `/new` |
| `/recap` | 按需生成目前工作階段的單行摘要 |
| `/export [檔名]` | 匯出對話為純文字；不帶檔名則開啟複製／儲存對話框 |
| `/copy [N]` | 複製最後一個回覆； `/copy 2` 複製倒數第二個；有程式碼區塊時出現互動選擇器，按 `w` 可寫入檔案 |

### 二、程式碼變更與版本追蹤

| 指令 | 說明 |
| --- | --- |
| `/diff` | 互動式差異檢視器：左右鍵切換 git diff／每回合變更，上下鍵瀏覽檔案 |
| `/rewind` | 將對話和程式碼倒帶到上一個 checkpoint，或從選定訊息重新摘要。別名： `/checkpoint` 、 `/undo` |
| `/branch [名稱]` | 在此時刻建立對話分支，用於實驗不同方向。別名： `/fork` |
| `/add-dir <路徑>` | 為本次工作階段新增額外工作目錄（僅授予檔案存取，不套用 `.claude/` 配置） |
| `/security-review` | 分析目前分支待處理變更，偵測注入、驗證、資料洩露等安全風險 |
| `/review [PR]` | 在目前工作階段本地審閱 pull request；深度雲端審閱請用 `/ultrareview` |

### 三、任務規劃與自動化

| 指令 | 說明 |
| --- | --- |
| `/plan [描述]` | 進入 Plan Mode，Claude 先擬計畫再動手；可帶描述立即開始，例： `/plan fix the auth bug` |
| `/ultraplan <提示>` | 在 ultraplan 工作階段草擬計畫，可在瀏覽器中檢視，再遠端執行或發回終端機 |
| `/design [描述]` | 生成多組可編輯 UI 畫板，挑選、微調後再交給 Claude 實作（例： `/design a few options for 登入頁` ）；目前為早期預覽，畫板需手動存檔才會保留 |
| \`/goal \[條件 | clear\]\` |
| `/schedule [描述]` | 建立、更新、列出或執行雲端例行工作，對話式引導設定。別名： `/routines` |
| `/autofix-pr [提示]` | 產生網頁工作階段監視目前分支 PR，在 CI 失敗或審閱評論時自動推送修復；需 `gh` CLI |

> 延伸閱讀： [Claude Code 開始「畫」介面！新增 /design 指令，寫程式前可先挑一張設計稿](https://www.bnext.com.tw/article/91917/claude-code-design-command-early-preview)

### 四、並行執行與 Agent 管理

| 指令 | 說明 |
| --- | --- |
| `/agents` | 管理 agent 配置，也是 Agent 管理員入口 |
| `/tasks` | 列出並管理目前工作階段背景執行的工作。別名： `/bashes` |
| `/background [提示]` | 將目前工作階段分離為背景 agent 執行，釋放終端機；可傳入額外指示。別名： `/bg` |
| `/stop` | 停止目前附加的背景工作階段；記錄與 worktree 保留。若只分離不停止請用 `/exit` 或 `←` |

### 五、工作階段管理

| 指令 | 說明 |
| --- | --- |
| `/rename [名稱]` | 重新命名目前工作階段；不帶名稱則從對話歷史自動生成 |
| `/resume [工作階段]` | 按 ID 或名稱繼續對話，或開啟工作階段選擇器。別名： `/continue` |
| `/desktop` | 將目前工作階段移至 Claude Code 桌面應用繼續。僅限 macOS 和 Windows。別名： `/app` |
| `/teleport` | 將網頁版 Claude Code 工作階段拉入此終端機；需 claude.ai 訂閱。別名： `/tp` |
| `/remote-control` | 讓此工作階段可從 claude.ai 遠端控制。別名： `/rc` |
| `/remote-env` | 配置以 `--remote` 啟動的網頁工作階段的預設遠端環境 |
| `/sandbox` | 切換 sandbox 模式（僅限支援平台） |

### 六、模型與效能設定

| 指令 | 說明 |
| --- | --- |
| `/model [模型]` | 切換 AI 模型；支援的模型可用左右鍵調整努力程度，變更立即生效 |
| \`/effort \[low | medium |
| \`/fast \[on | off\]\` |

### 七、介面與個人化

| 指令 | 說明 |
| --- | --- |
| `/config` | 開啟設定介面，調整主題、模型、輸出樣式等。別名： `/settings` |
| `/theme` | 更換色彩主題，含自動跟隨終端機深淺色、色盲無障礙（daltonized）、ANSI 及自訂主題 |
| \`/color \[顏色 | default\]\` |
| \`/tui \[default | fullscreen\]\` |
| `/focus` | 切換焦點檢視，僅顯示最後提示、單行工具摘要和最終回應。僅限全螢幕渲染 |
| `/scroll-speed` | 互動調整滑鼠滾輪捲動速度；僅限全螢幕渲染，JetBrains IDE 終端機不支援 |
| `/keybindings` | 開啟或建立快捷鍵配置檔案 |
| `/statusline` | 配置狀態列顯示內容；不帶引數從 shell 提示自動配置 |
| `/terminal-setup` | 配置 Shift+Enter 等快捷鍵（僅在需要的終端機中顯示，如 VS Code、Cursor、Warp、Zed） |

### 八、記憶體與權限管理

| 指令 | 說明 |
| --- | --- |
| `/memory` | 編輯 `CLAUDE.md` 記憶體檔案、啟用／停用自動記憶體、檢視自動記憶體項目 |
| `/permissions` | 管理工具的允許、詢問、拒絕規則；可檢視最近 Auto Mode 拒絕紀錄。別名： `/allowed-tools` |
| `/init` | 用 `CLAUDE.md` 初始化專案；設定 `CLAUDE_CODE_NEW_INIT=1` 可進入互動式引導流程 |

### 九、外部整合

| 指令 | 說明 |
| --- | --- |
| `/mcp` | 管理 MCP 伺服器連線與 OAuth 驗證 |
| `/ide` | 管理 IDE 整合並顯示連線狀態 |
| `/chrome` | 配置 Claude in Chrome 設定 |
| `/install-github-app` | 為存放庫設定 Claude GitHub Actions 整合 |
| `/install-slack-app` | 安裝 Claude Slack 應用程式（開啟瀏覽器完成 OAuth） |
| \`/voice \[hold | tap |
| `/web-setup` | 用本地 `gh` CLI 認證將 GitHub 帳號連接至網頁版 Claude Code |
| `/setup-bedrock` | 互動式精靈配置 Amazon Bedrock 驗證、區域與模型（需設定 `CLAUDE_CODE_USE_BEDROCK=1` ） |
| `/setup-vertex` | 互動式精靈配置 Google Vertex AI 驗證、專案與區域（需設定 `CLAUDE_CODE_USE_VERTEX=1` ） |

### 十、Skills 與 Plugins

| 指令 | 說明 |
| --- | --- |
| `/skills` | 列出可用 skills；按 `t` 依 token 計數排序；按 `Space` 隱藏 skill |
| `/plugin` | 管理 Claude Code plugins |
| `/reload-plugins` | 重新載入所有作用中的 plugins 套用變更，無需重啟 |

### 十一、診斷、費用與帳戶

| 指令 | 說明 |
| --- | --- |
| `/doctor` | 診斷並驗證 Claude Code 安裝與設定；按 `f` 可讓 Claude 自動修復問題 |
| `/heapdump` | 將 JS 堆快照與記憶體分解寫入 `~/Desktop` ，用於診斷高記憶體使用 |
| `/usage` | 顯示工作階段成本、方案使用限制與活動統計。 `/cost` 、 `/stats` 為別名 |
| `/extra-usage` | 配置額外使用量，在達到速率限制時繼續工作 |
| `/status` | 開啟設定介面的狀態標籤（版本、模型、帳戶、連線），Claude 回覆中也可使用 |
| `/insights` | 產生報告，分析工作階段的專案領域、互動模式與摩擦點 |
| `/privacy-settings` | 檢視與更新隱私設定（僅限 Pro／Max） |
| `/upgrade` | 開啟升級頁面 |
| `/login` / `/logout` | 登入／登出 Anthropic 帳戶 |
| `/passes` | 分享一週免費 Claude Code 通行證（帳戶符合資格時顯示） |

### 十二、團隊協作

| 指令 | 說明 |
| --- | --- |
| `/team-onboarding` | 從過去 30 天使用歷史產生團隊入職 Markdown 指南；Pro／Max／Team／Enterprise 可取得分享連結 |

### 十三、其他

| 指令 | 說明 |
| --- | --- |
| `/help` | 顯示說明與可用命令 |
| `/powerup` | 以互動式課程（附動畫演示）探索 Claude Code 功能 |
| `/release-notes` | 在互動式版本選擇器中檢視變更日誌 |
| `/feedback` | 提交意見反應或回報問題。別名： `/bug` |
| `/mobile` | 顯示 QR Code 下載 Claude 行動應用。別名： `/ios` 、 `/android` |
| `/radio` | 在瀏覽器開啟 Claude FM lo-fi 廣播；無瀏覽器時列印串流 URL（Bedrock／Vertex／Foundry 不支援） |
| `/stickers` | 訂購 Claude Code 貼紙 |
| `/exit` | 結束 CLI。別名： `/quit` |

## Claude Code 常用快捷鍵

![#1 Claude Code指令大全](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-05/tsd8-1778660939.png?w=1200&output=webp)

圖／ ChatGPT生成

| 快捷鍵 | 動作 | 說明 |
| --- | --- | --- |
| `Ctrl+C` | `app:interrupt` | 取消目前操作（ **無法重新綁定** ） |
| `Ctrl+D` | `app:exit` | 結束 Claude Code（ **無法重新綁定** ） |
| `Ctrl+T` | `app:toggleTodos` | 顯示／隱藏工作清單 |
| `Ctrl+O` | `app:toggleTranscript` | 切換詳細逐字稿檢視 |
| `Ctrl+J` | `chat:newline` | 插入換行，不送出 |
| `Escape` | `chat:cancel` | 取消目前輸入 |
| `Ctrl+L` | `chat:clearInput` | 強制全螢幕重繪並保留輸入；全螢幕模式下兩秒內按兩次執行 `/clear` |
| `Cmd+K` | `chat:clearScreen` | 全螢幕模式下兩秒內按兩次執行 `/clear` |
| `Ctrl+G` / `Ctrl+X Ctrl+E` | `chat:externalEditor` | 在外部編輯器開啟輸入內容 |
| `Ctrl+S` | `chat:stash` | 暫存目前提示 |
| `Ctrl+_` / `Ctrl+Shift+-` | `chat:undo` | 復原上一個動作 |
| `Shift+Tab` | `chat:cycleMode` | 循環切換權限模式（Windows 無 VT 模式時預設為 `Meta+M` ） |
| `Meta+P` | `chat:modelPicker` | 開啟模型選擇器 |
| `Meta+O` | `chat:fastMode` | 切換快速模式 |
| `Meta+T` | `chat:thinkingToggle` | 切換延伸思考模式 |
| `Ctrl+X Ctrl+K` | `chat:killAgents` | 終止本工作階段所有背景子 agent |
| `Ctrl+R` | `history:search` | 開啟歷史搜尋 |
| `↑` | `history:previous` | 上一筆歷史記錄 |
| `↓` | `history:next` | 下一筆歷史記錄 |

> 延伸閱讀：  
> [Claude、ChatGPT語音輸入這樣用！5步驟說清任務＋評估標準，讓AI幫你生出最強提示詞](https://www.bnext.com.tw/article/90920/ai-prompt-tips)  
> [ChatGPT Image 2.0指令大全！40組提示詞一次整理，LINE貼圖、簡報封面、插畫菜單全搞定](https://www.bnext.com.tw/article/90908/chatgpt-image-2-prompt-guide-complete)

資料來源： [Anthropic 1](https://code.claude.com/docs/zh-TW/commands) 、 [Anthropic 2](https://code.claude.com/docs/en/keybindings)

本文初稿為AI編撰，整理．編輯/ 黃若彤

關鍵字： [＃AI工具](https://www.bnext.com.tw/tags/AI%E5%B7%A5%E5%85%B7) [＃Claude](https://www.bnext.com.tw/tags/Claude) [＃Claude Code](https://www.bnext.com.tw/tags/Claude%20Code)

往下滑看下一篇文章

tw\_bnext \[Dynamic Article\]-20260814-08:04

arrow\_forward\_ios

閱讀文章

<video title="Advertisement" src="https://rr3---sn-ipoxu-un56.googlevideo.com/videoplayback?expire=1787158837&amp;ei=tXCFasWYG5C9vcAP44z28Qs&amp;ip=60.250.84.232&amp;id=1406db4b826ead6e&amp;itag=22&amp;source=youtube&amp;requiressl=yes&amp;xpc=Eghovf3BOnoBAQ==&amp;cps=344&amp;met=1787130037,&amp;mh=-8&amp;mm=31&amp;mn=sn-ipoxu-un56&amp;ms=au&amp;mv=m&amp;mvi=3&amp;pl=21&amp;rms=au,au&amp;susc=gvp&amp;acao=yes&amp;ctier=L&amp;mime=video/mp4&amp;vprv=1&amp;rqh=1&amp;dur=10.170&amp;lmt=1779702448298922&amp;mt=1787129642&amp;cpn=dJukK_JHXfT2shZW&amp;txp=6318224&amp;sparams=expire,ei,ip,id,itag,source,requiressl,xpc,susc,acao,ctier,mime,vprv,rqh,dur,lmt&amp;sig=AE0s2JYwRQIhAK27MqSQYaUUOLFCxXpZzvfezcDTnwm0s0Te-smvCjM-AiAdNtQm-KY1_NIkb02NP0z_XZtfifBB4dmiI2i7WxrgJQ==&amp;lsparams=cps,met,mh,mm,mn,ms,mv,mvi,pl,rms&amp;lsig=APaTxxMwRQIgQdwaNEG5t_m6toHpnaCSUrDbGlzCWCdv0Na9ZEANDGUCIQC0nVDWi1VACetyyiRqF__eL1XRapclEC1NjYccA64Eow==" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.785.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_939351216%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B962325777%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C956056724%2Cnull%2C%5B%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2Cnull%2C45792627%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323831%5D%2C%5B83323832%2C%5B%5B962325777%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398187%5D%2C%5B95398188%2C%5B%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95338773%2C95322027%2C95331589%2C95397688%2C95332046%2C95378629%2C95340253%2C95340255%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=2046440087248357&amp;pvsid=8002505467759861&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.bnext.com.tw%2Farticle%2F90925%2Fclaude-code-slash-commands-shortcuts-complete-guide%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

pause

volume\_mute

AD

00:00

00:11

00:44

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/bnext.com.tw/bnext.com.tw-1786694685.517094.mp4?verify=1787129800-t%2Bh2qeVkH1gyoeiPw7Pt%2BEXWvN5ba86ZUq%2B8oTfLOI8%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/bnext.com.tw/bnext.com.tw-1786694685.517094.mp4?token=GIfBD_2rn8aN-A-vnbmRmrKXJbG4GQrvIGfudVe8mow&amp;expires=1787216200"></video>

即時熱門文章

[1 普發一萬要來了！賴清德宣布2027上路，怎麼申請、最新進度一次看](https://www.bnext.com.tw/article/91705/2026-cash-handout-10000-progress) [2 Uber Eats疊單爭議是什麼？外送專法計酬怎麼算？追溯補發、服務費調漲一次看](https://www.bnext.com.tw/article/91867/food-delivery-batch-orders-pay-dispute-ubereats-law) [3 AI封裝關鍵零件！矽電容概念股有哪些？力積電打進英特爾，這檔是台股「最純」](https://www.bnext.com.tw/article/91904/what-is-silicon-capacitor-stocks) [4 AI文章無所遁形？Anthropic推出「隱形浮水印」機制，一篇搞懂背後原理、限制和影響](https://www.bnext.com.tw/article/91882/claude-text-watermark-explained) [5 009828開募！首檔PCB ETF為何不配息？台光電、欣興兩檔權重就占近三成](https://www.bnext.com.tw/article/91889/009828-pcb-etf) [6 ChatGPT Work怎麼用？盤點14種上班族好用功能，如何打造超高效AI職場工作流？](https://www.bnext.com.tw/article/91878/chatgpt-work-14-features-cloud-vs-local)

![不只投廣告！在 AI 時代，無限創意行銷如何從企業營運出發，放大行銷成效？](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/yey4-1785133327.jpg?w=900&output=webp)

2026.08.05 | [行銷與Martech](https://www.bnext.com.tw/categories/martech)

不只投廣告！在 AI 時代，無限創意行銷如何從企業營運出發，放大行銷成效？

生成式 AI 讓文案、圖片與廣告素材都能更快完成，當執行成本下降，企業判斷問題出在哪裡的能力，才是拉開差距、真正獲得成長的關鍵。

---

分享

一家連續十年營收 700 萬元的健身業者，如何在一年內將業績推升至 3,500 萬元，成長整整 5 倍？答案可能和多數人想的不一樣。

這家健身品牌不是一開始就加碼投放廣告，而是先從調整組織架構與薪酬制度著手，透過明確分工改善團隊運作，進而建立「全員銷售」文化，這並不是要求每位員工都成為業務員，而是讓團隊理解與認同品牌理念，確保從服務、回覆、教學到售後過程中的每一個顧客接觸點，都能傳遞一致的訊息與體驗，從而建立顧客對品牌的信任感，甚至願意主動購買與持續回購。而規劃這套成長策略的幕後推手，正是無限創意行銷。

## AI 讓行銷變簡單，為何行銷公司反而更難做出成效？

雖然公司名稱裡有「行銷」兩個字，但無限創意行銷並非只從行銷工具出發，而是以顧問式行銷的角色，先理解企業經營問題，再決定應採取哪些行銷與營運解方。「我們不會一開始就告訴企業該採用哪些行銷策略，而是根據企業真的需求來提供適合解方」，無限創意行銷創辦人翁小五一語道出公司的獨特定位。

通常，企業在面對營收成長瓶頸時，第一個念頭就是做行銷，然而，翁小五從過往的工作與創業經驗中發現，限制企業營收成長的原因有很多，不一定與行銷有關，背後可能還有其他問題，若沒有釐清真正的問題所在，就急著投入廣告或其他行銷資源，很可能只是治標不治本。

基於這樣的觀察，無限創意行銷決定採取反向思考邏輯：先回到企業經營本身，找出造成「成長趨緩」的真正原因，再據此制定策略、優化營運流程，最後透過行銷放大企業優勢。當每一筆行銷投入都建立在更健康的企業體質與更精準的策略判斷之上，行銷才不只是「有做」，而是真正能為企業帶來成長。

這套「先找問題、再選工具」的服務模式，在生成式 AI 工具普及後，也成為拉開行銷成效差距的關鍵。

過去，行銷公司的價值建立在製作能力、投放技術與媒體資源；但當 AI 逐步降低文案、素材與分析的執行成本，單純提供「做內容、下廣告、做報表」的服務，差異化將快速縮小。此時真正考驗的，反而是行銷人的理解力和判斷力：能否看懂消費者真正需要什麼？能否找出限制企業營收成長的關鍵環節？而這正是無限創意行銷長期深耕企業經營現場、不斷透過數據與市場驗證所累積的核心能力。

![無限創意行銷-2.JPG](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/gjjj-1785133327.jpg?w=1200&output=webp)

無限創意行銷創辦人 翁小五

圖／ 數位時代

## AI 拆解營收黑洞，讓數據為企業成長導航

在無限創意行銷的顧問式服務流程中，數據是相當重要的依據。翁小五進一步說明，無限創意行銷的數據應用可以分為三個層次：從檢視行銷活動成效、拆解顧客旅程中的問題節點，再進一步延伸至年度營運規劃，讓企業不只知道「過去發生了什麼」，更能判斷「下一步該怎麼走」。

首先，是檢視行銷活動成效，其中最重要的原則在於，根據行銷目標去解讀數據。舉例來說，如果目標是增加首購顧客，除了檢視素材吸引力與點擊率，還要持續追蹤到站、詢問、成交、新客占比與首購成本；如果目標是擴大營收規模，則應進一步檢視回購率、客單價與顧客終身價值。

數據應用的第二層價值，是拆解從「廣告投入」到「營收產出」之間的過程，理解每一個節點的數據分別反映什麼問題，再針對問題逐一修正。

至於第三個層次，則是讓數據成為企業經營與資源配置的依據。無限創意行銷會從年度營收目標出發，結合產業特性、淡旺季變化與過往行銷成果，拆解每一季、每個月的業績任務，再將過去有效的策略放大、成效不佳的做法修正，據此安排下一年度的行銷資源與營運節奏。

## 不急著加碼廣告，從企業體質出發找出 5 倍成長解方

對無限創意行銷來說，數據是找出問題、配置資源與規劃成長路徑的關鍵，文章開頭提到的健身業者，正是此套方法論的最佳印證。當初，翁小五深入診斷後發現，這家企業真正的問題在於組織結構過度扁平，老闆長期忙於處理日常事務，難以專注在營運規劃；此外，組織缺乏真正負責營運管理的中階主管、教練流動過高導致學員缺乏安全感等，皆是影響回購率的因素。

因此，無限創意行銷先從組織架構著手，設立專責店長一職，並重新設計薪酬制度，藉此讓教練、店長與老闆皆可專注在自身工作上。之後，再透過前期廣告數據與顧客洞察，找出消費者選擇品牌的真正原因，並運用行銷活動與口碑經營持續放大。同時，翁小五也在過程中導入「全員銷售」概念，確保顧客在各個接觸點都享有一致的體驗，品牌才能逐步累積顧客信任。

最終，這家企業的營收一年就成長至 3,500 萬元，達到原先的 5 倍。這個案例也說明了無限創意行銷的方法論：行銷不是企業成長的起點，而是放大企業優勢的槓桿；企業體質愈健全，這支槓桿所能放大的成長效益也愈大。

## 從「會做」到「會判斷」，AI 時代，無限創意行銷正在重新定義行銷公司的價值

隨著 AI 能力越來越強大，下一代行銷公司的價值，不只是比誰更懂得運用 AI，而是行銷人的市場理解力與專業判斷力，知道 AI 產出的內容哪些值得留下、哪些應該捨棄，或是該如何持續修正，才能打造出能夠被消費者感受到的品牌體驗。

因此，無限創意行銷未來將積極建立團隊與 AI 協作的方法論，將過往的專案判斷、提問框架與數據檢核邏輯，轉化為 AI 協作流程。團隊在面對新專案時，不是直接要求 AI 給答案，而是先定義目標、限制條件與判斷標準，再去檢查 AI 產出是否符合商業情境。

對翁小五而言，AI 時代真正稀缺的，不是工具，而是定義問題、理解消費者與判斷品質的能力。未來無限創意行銷的競爭力，也不在於使用多少 AI，而是持續將人的品味、市場理解與產業 Know-how，透過 AI 放大，協助企業打造更精準的品牌體驗與更高品質的決策。

即時熱門文章

[1 普發一萬要來了！賴清德宣布2027上路，怎麼申請、最新進度一次看](https://www.bnext.com.tw/article/91705/2026-cash-handout-10000-progress) [2 Uber Eats疊單爭議是什麼？外送專法計酬怎麼算？追溯補發、服務費調漲一次看](https://www.bnext.com.tw/article/91867/food-delivery-batch-orders-pay-dispute-ubereats-law) [3 AI封裝關鍵零件！矽電容概念股有哪些？力積電打進英特爾，這檔是台股「最純」](https://www.bnext.com.tw/article/91904/what-is-silicon-capacitor-stocks) [4 AI文章無所遁形？Anthropic推出「隱形浮水印」機制，一篇搞懂背後原理、限制和影響](https://www.bnext.com.tw/article/91882/claude-text-watermark-explained) [5 009828開募！首檔PCB ETF為何不配息？台光電、欣興兩檔權重就占近三成](https://www.bnext.com.tw/article/91889/009828-pcb-etf) [6 ChatGPT Work怎麼用？盤點14種上班族好用功能，如何打造超高效AI職場工作流？](https://www.bnext.com.tw/article/91878/chatgpt-work-14-features-cloud-vs-local)

[![台達電全解讀](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-07/e5i9-1785123864.jpg?w=600&output=webp)](https://www.bnext.com.tw/magazine/view/130221)