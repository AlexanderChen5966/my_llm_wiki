---
title: "Codex 新手入門：從安裝到外掛、MCP、Skills，一篇把設定講清楚"
source: "https://codelove.tw/@tony/post/3ojwXx#google_vignette"
author:
published:
created: 2026-07-20
description: "Codex 小白入門：從安裝到外掛、MCP、Skills，一篇把設定講明白  ========================================    Codex 小白入門：從安裝到外掛、MCP、Skills，一篇把設定講明白--------------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月17日](https://codelove.tw/@tony/post/3ojwXx "2026-07-17 12:04:45")

## Codex 新手入門：從安裝到外掛、MCP、Skills，一篇把設定講清楚

## Codex 小白入門：從安裝到外掛、MCP、Skills，一篇把設定講明白

```
Codex 小白入門：從安裝到外掛、MCP、Skills，一篇把設定講明白
```

---

最近老粉絲群裡有不少同學一直催我：能不能出一篇 Codex 的基礎教學，不要只講「很強」「很好用」，而是把安裝、設定、外掛、Skills、MCP、記憶、連接 App 這些東西一次講清楚。我理解這個需求。

軟體

我這邊也有一些 AI Coding（SDD Agent AI 工具等）和 Node 技術交流交流群，感興趣的可以加微信 **ikoala520** 進群，一起學習，共同進步。

因為新手用 Codex 最大的問題，往往不是不會提問，而是不知道 Codex 的「幾層設定」分別管什麼。你可能已經裝好了 Codex，也能讓它改程式碼、寫文章、解釋報錯，但一看到 `config.toml` 、 `AGENTS.md` 、 `MCP` 、 `Plugins` 、 `Skills` 、 `Memory` ，腦子裡馬上變成一團線。這篇文章就解決這件事。

我不會把它寫成官方文件翻譯，也不會一上來堆一堆參數。我們按一個新手真正上手的順序來：先裝起來，再讓 Codex 懂你的專案，然後一點點接上外掛、MCP、Skills、記憶和 App。

[![image.png](https://i.imgur.com/qHndS5T.jpeg)](https://i.imgur.com/qHndS5T.jpeg) 先說明一下：Codex 更新很快。本文寫作時，我本機驗證的 CLI 版本是 `codex-cli 0.140.0-alpha.2` ，時間是 2026 年 6 月 14 日。具體命令以你本機的 `codex --help` 和官方文件為準。本文重點講方法和設定思路，命令只是幫你建立第一套可執行的骨架。

### 你先記住一句話

Codex 不是只有一個聊天框。

開發工具

更準確地說，它像一個可以被你設定的工作台：

你要解決的問題應該放在哪裡這一次任務的要求目前對話 / prompt這個專案長期遵守的規則 `AGENTS.md` 你的全域預設設定 `~/.codex/config.toml` 某類任務的標準流程 `Skills` 一組打包好的能力 `Plugins` 外部系統和工具介面 `MCP servers` 你的長期偏好 `Memory` Gmail、GitHub、Chrome 這類真實應用 `Apps / Connectors` [![image.png](https://i.imgur.com/E3bzZwi.jpeg)](https://i.imgur.com/E3bzZwi.jpeg) 很多新手一開始容易犯的錯，是把所有東西都塞進 prompt。

比如你每次都對 Codex 說：「請先閱讀專案結構，遵守我們的程式碼規範，執行測試，不要亂改檔案，寫完給我總結。」

這當然能用，但很累。更好的方式是把長期規則沉澱到 `AGENTS.md` ，把預設行為放到 `config.toml` ，把重複工作流寫成 `Skill` ，把外部能力交給 `MCP` 和外掛。

這樣 Codex 才會從「一個會回答問題的視窗」，變成「一個適合你工作流的助手」。

開發工具

### 第一步：先把 Codex 裝起來

Codex 現在有幾種常見使用方式。

入口更適合誰你可以怎麼理解Codex CLI程式設計師、經常在終端機工作的人在專案目錄裡直接讓 Codex 讀程式碼、改程式碼、跑命令Codex Desktop App想用圖形介面、外掛、連接 App 的人更像一個完整工作台IDE 擴充套件長時間待在編輯器裡的人讓 Codex 跟你的程式碼編輯環境貼近Cloud / Web 任務想把任務交給雲端跑的人適合平行處理或長任務如果你是新手，我建議從 **Codex Desktop App + CLI** 這套組合開始。桌面 App 適合看外掛、連接 App、管理執行緒；CLI 適合進入具體專案幹活。

#### 1\. Codex Desktop App 怎麼安裝

桌面 App 的官方入口是：

```
text 代碼解讀複製代碼https://developers.openai.com/codex/app
```

如果你想直接下載安裝檔，也可以按系統選擇：

- macOS Apple Silicon：下載 `Codex.dmg`
- macOS Intel：下載 `Codex-latest-x64.dmg`
- Windows：透過 Microsoft App Installer 安裝

對應直達地址如下：

```
text 代碼解讀複製代碼macOS Apple Silicon:
https://persistent.oaistatic.com/codex-app-prod/Codex.dmg

macOS Intel:
https://persistent.oaistatic.com/codex-app-prod/Codex-latest-x64.dmg

Windows:
https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi
```

macOS 安裝步驟很簡單：

1. 先確認自己的晶片類型。點左上角蘋果圖示，選擇「關於這台 Mac」，看晶片是 Apple Silicon 還是 Intel。
2. 下載對應的 `.dmg` 檔案。
3. 雙擊打開安裝包，把 Codex 拖到 Applications / 應用程式。
4. 打開 Codex App，按提示登入 OpenAI 帳號。
5. 如果 macOS 提示安全確認，在系統設定裡允許打開。

Windows 安裝步驟也類似：

1. 打開 Windows 下載位址。
2. 按 Microsoft Store / App Installer 的提示安裝。
3. 安裝完成後啟動 Codex。
4. 登入 OpenAI 帳號。
5. 在 App 裡檢查外掛、連接器和本地工作區權限。

如果你已經裝了 CLI，也可以在終端機裡執行：

```
bash 代碼解讀複製代碼codex app
```

這個命令會啟動 Codex Desktop App。如果本機還沒裝 App，它會引導你去安裝。

#### 2\. Codex CLI 怎麼安裝

CLI 的官方入口是：

```
text 代碼解讀複製代碼https://developers.openai.com/codex/cli
```

現在更推薦用官方獨立安裝程式。macOS / Linux 可以這樣安裝：

```
bash 代碼解讀複製代碼curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Windows PowerShell 可以這樣安裝：

```
powershell 代碼解讀複製代碼irm https://chatgpt.com/codex/install.ps1 | iex
```

如果你習慣用套件管理器，也可以選下面幾種方式：

```
bash 代碼解讀複製代碼# npm
npm install -g @openai/codex

# Homebrew
brew install --cask codex
```

如果你想手動下載二進位檔，可以去 GitHub Releases：

```
text 代碼解讀複製代碼https://github.com/openai/codex/releases
```

裝好 CLI 以後，先跑這幾個命令：

```
bash 代碼解讀複製代碼codex --version
codex login
codex doctor
```

`codex login` 用來登入帳號。

`codex doctor` 用來檢查本機安裝、設定、認證和執行階段健康狀態。新手遇到「為什麼不能用」「為什麼工具不生效」「為什麼 MCP 連不上」時，先跑它，比直接猜原因可靠。

你還可以看完整命令：

```
bash 代碼解讀複製代碼codex --help
```

我本機看到的常用命令包括：

命令用途 `codex` 啟動互動式 CLI `codex exec` 非互動執行一次任務 `codex review` 做程式碼審查 `codex login` / `logout` 管理登入 `codex doctor` 診斷環境 `codex mcp` 管理 MCP server `codex plugin` 管理外掛 `codex app` 啟動桌面 App `codex update` 更新 Codex你不用一開始全記住。新手階段只要記住：

```
bash 代碼解讀複製代碼codex
codex login
codex doctor
codex --help
```

夠用了。

### 第二步：理解 AGENTS.md

如果只能先學一個設定檔，我建議先學 `AGENTS.md` 。

它的作用很簡單：告訴 Codex，這個專案該怎麼工作。

比如專案怎麼啟動、怎麼測試、程式碼風格是什麼、哪些目錄不要亂動、提交前要跑什麼命令、回答時要注意什麼。你可以把它理解成「寫給 AI 同事看的專案說明書」。

一個最小版本可以這樣寫：

```
markdown 代碼解讀複製代碼# AGENTS.md

## Project overview

This is a Next.js project. The main app is in \`apps/web\`.

## Common commands

- Install dependencies: \`pnpm install\`
- Start dev server: \`pnpm dev\`
- Run typecheck: \`pnpm typecheck\`
- Run tests: \`pnpm test\`

## Coding rules

-  existing file structure and naming style.
- Prefer small, focused changes.
- Do not rewrite unrelated files.
- Do not add new dependencies unless necessary.

## Verification

Before saying the task is complete, run the relevant check:

- UI changes: run \`pnpm typecheck\`
- Logic changes: run related tests
- Large changes: run \`pnpm test\`

If a command was not run, say it clearly.
```

這裡最關鍵的不是寫得多，而是寫得準。

我建議你在 `AGENTS.md` 裡優先放四類內容：

內容示例專案結構前端在哪裡、後端在哪裡、設定在哪裡常用命令安裝、啟動、測試、建置編碼約束不要改哪些檔案、用什麼風格、怎麼命名驗收方式什麼情況下跑型別檢查，什麼情況下跑測試有的小夥伴可能會問：這些東西為什麼不直接寫在 prompt 裡？

因為 prompt 是臨時的， `AGENTS.md` 是長期的。你今天讓 Codex 改登入，明天讓它改列表，後天讓它寫測試，這些任務都應該遵守同一套專案規矩。把規矩寫進 `AGENTS.md` ，後面就不用每次重複念一遍。

### 第三步：理解 config.toml

`AGENTS.md` 管「專案規矩」， `config.toml` 管「Codex 自己怎麼執行」。

最常見的位置是：

```
text 代碼解讀複製代碼~/.codex/config.toml
```

它通常用來設定模型、沙箱、批准策略、MCP、profile、環境變數策略等內容。

你也可以臨時覆蓋設定。比如 CLI 幫助裡可以看到這種寫法：

```
bash 代碼解讀複製代碼codex -c model=\"o3\"
codex -c 'sandbox_permissions=[\"disk-full-read-access\"]'
codex -c shell_environment_policy.inherit=all
```

這說明一個重要思路： `config.toml` 是長期預設值， `-c` 是本次臨時覆蓋。

新手階段，不建議一上來改很多。你可以先理解這幾個設定方向：

設定方向你應該怎麼理解model預設使用哪個模型sandboxCodex 跑命令時能存取哪些檔案approval哪些操作需要你確認MCP外部工具怎麼接進來profiles給不同場景準備不同設定features開啟或關閉某些實驗能力舉個偏保守的理解方式：

```
toml 代碼解讀複製代碼# ~/.codex/config.toml

# 示例：按你帳號和目前 Codex 版本可用模型調整
model = "o3"

# 新手建議不要急著開到最高權限。
# 真正需要全權限時，再在可信專案裡單獨開啟。
sandbox_mode = "workspace-write"

# 是否需要人工確認，取決於你的使用場景。
# 日常互動可以保守一點，自動化腳本可以更明確地限制。
approval_policy = "on-request"
```

不同版本欄位可能會有變化。如果你打開後發現欄位名不一樣，不要慌，先看：

```
bash 代碼解讀複製代碼codex --help
codex doctor
```

以及你目前 Codex 版本對應的官方文件。

這裡有一個很實用的經驗： **別把新手設定成 `danger-full-access` 起步。**

`danger-full-access` 很強，但也意味著 Codex 執行命令時擁有非常大的檔案系統權限。它適合你非常信任目前工作區、並且知道自己在做什麼的場景。剛開始用的時候，先用較保守的沙箱和批准策略，熟悉之後再逐步放開。

### 第四步：Plugins 是什麼

很多人聽到外掛，以為就是「多一個按鈕」。

在 Codex 裡，外掛更像一個能力包。一個外掛裡可能包含：

- 一個或多個 `Skills`
- MCP server 設定
- App / Connector 能力
- 腳本、資源、範本
- 外掛自己的說明和中繼資料

你可以用命令查看外掛相關能力：

```
bash 代碼解讀複製代碼codex plugin --help
```

常見子命令包括：

```
bash 代碼解讀複製代碼codex plugin list
codex plugin add <plugin>
codex plugin remove <plugin>
codex plugin marketplace list
```

安裝外掛時，命令形式大概是：

```
bash 代碼解讀複製代碼codex plugin add sample@debug
codex plugin add sample --marketplace debug
```

如果你使用桌面 App，也可以在外掛入口裡搜尋、安裝、啟用。

[![image.png](https://i.imgur.com/d0OKzdY.jpeg)](https://i.imgur.com/d0OKzdY.jpeg) 新手階段，我不建議你一口氣裝一堆外掛。你可以按工作流選：

你的場景優先考慮寫程式碼、看 PRGitHub、Browser、Chrome寫文章、報告Documents、Browser、Data Analytics做 PPTPresentations處理表格Spreadsheets做產品和介面Product Design、Figma、Browser操作網頁或桌面Chrome、Computer Use外掛的價值，不是「看起來很高級」，而是讓 Codex 能接觸真實材料、呼叫真實工具、交付真實檔案。

比如你裝了 GitHub 相關外掛後，它不只是告訴你「應該怎麼 review」，而是可以讀取 PR、看評論、查 CI、給出修改建議。你裝了 Presentations，它不只是寫 PPT 文案，而是能生成真正的簡報檔案。

這就是外掛和普通 prompt 的差別。

### 第五步：Skills 是什麼

`Skills` 很適合新手理解成：把一套固定做事方法寫成檔案。

比如你經常寫公眾號文章，每次都要經歷：

1. 定選題
2. 查資料
3. 搭大綱
4. 寫初稿
5. 做配圖
6. 預覽
7. 改節奏

如果每次都在 prompt 裡說一遍，很麻煩。你可以把這套流程寫成一個 `SKILL.md` ，以後只要任務匹配，Codex 就能按這個方法走。

Codex 的Skills 的位置在 ，注意是在這裡 `~/.codex/skills`

[![image.png](https://i.imgur.com/5bZ7yHg.jpeg)](https://i.imgur.com/5bZ7yHg.jpeg) 一個 Skill 通常長這樣：

```
markdown 代碼解讀複製代碼---
name: ai-article-writer
description: Use when writing long-form AI technical articles.
---

# AI Article Writer

## Workflow

1. Clarify the target reader and article angle.
2. Collect reliable references.
3. Propose title options and outline.
4. Draft the article in Markdown.
5. Add diagrams when useful.
6. Render a local preview.
7. Review readability before final delivery.
```

注意， `Skill` 不是單純的 prompt 範本。

它更像一套「作業規程」。好的 Skill 會告訴 Codex：

要素說明什麼時候使用 `description` 寫清楚觸發場景怎麼做明確步驟用哪些資料引用相關參考檔案用哪些腳本複用已有腳本，而不是每次重寫怎麼驗收最後檢查什麼那 `Skill` 和 `AGENTS.md` 有什麼區別？

很簡單：

檔案適合放什麼 `AGENTS.md` 這個專案長期遵守的規則 `SKILL.md` 某一類任務的標準做法比如「這個專案用 `pnpm` ，不要用 `npm` 」，應該放 `AGENTS.md` 。

比如「寫公眾號文章前必須先做選題定位，再寫大綱」，應該放 `Skill` 。

### 第六步：MCP 是什麼

`MCP` 全稱是 Model Context Protocol。你可以先不用記英文，只要記住它解決的問題：

**讓 Codex 透過標準協定連接外部工具和資料來源。**

比如：

MCP 能連接什麼典型用途GitHub讀取 PR、Issue、檔案、CI資料庫查詢業務資料文件系統讀取團隊知識庫瀏覽器打開頁面、檢查 UI搜尋服務查找外部資料內部系統呼叫你們自己的工具在 CLI 裡可以用：

```
bash 代碼解讀複製代碼codex mcp --help
```

我本機看到的常用子命令包括：

```
bash 代碼解讀複製代碼codex mcp list
codex mcp get <name>
codex mcp add <name> --url <url>
codex mcp add <name> -- <command>
codex mcp remove <name>
codex mcp login <name>
codex mcp logout <name>
```

新增 MCP server 有兩種常見方式。

第一種是 HTTP 方式：

```
bash 代碼解讀複製代碼codex mcp add openaiDeveloperDocs --url https://developers.openai.com/mcp
```

第二種是本地命令方式：

```
bash 代碼解讀複製代碼codex mcp add my-tool -- node server.js
```

如果需要環境變數，也可以看到類似這種形式：

```
bash 代碼解讀複製代碼codex mcp add my-tool --env API_KEY=$MY_TOOL_API_KEY -- node server.js
```

實際使用時，不建議把真實密鑰直接寫在命令裡。更好的方式是放在環境變數中，或者使用 Codex / MCP 支援的安全設定方式。

如果以上 cli 你都不喜歡，也可以透過 codex 桌面端安裝和使用 mcp

[![image.png](https://i.imgur.com/e4iNgtJ.jpeg)](https://i.imgur.com/e4iNgtJ.jpeg)

新手理解 MCP，可以抓住一個判斷標準：

**凡是 Codex 需要讀取外部系統、呼叫外部工具、存取即時資料，就優先想 MCP 或 App Connector。**

不要讓模型憑記憶回答你的私有資料。

比如你問：「我們專案最近哪個 PR 沒過 CI？」

這類問題不應該讓 Codex 猜，也不應該讓你複製貼上一堆網頁內容。更合理的方式是接 GitHub，然後讓 Codex 直接讀取真實 PR 和檢查結果。

### 第七步：Memory 記憶功能怎麼用

`Memory` 很容易被誤解。

[![image.png](https://i.imgur.com/YOqJAFx.jpeg)](https://i.imgur.com/YOqJAFx.jpeg) 它不是讓 Codex 記住所有事實，也不是你的私人資料庫。它更適合記住長期偏好和穩定習慣。

適合放進 Memory 的內容：

適合記示例寫作偏好我喜歡中文技術文章，風格實戰、少空話常用工作方式修改程式碼後要跑 typecheck長期稱呼偏好回答我時用中文穩定背景我經常寫公眾號 AI 工具文章不適合放進 Memory 的內容：

不適合記原因密碼、token、密鑰安全風險太高今天的新聞很快過期某個 PR 當前狀態應該從 GitHub 即時讀取臨時任務要求放目前對話就夠了一句話： **Memory 記偏好，不記事實；記習慣，不記機密。**

如果你希望 Codex 長期記住「我寫文章喜歡實戰風格」，放 Memory 很合適。

如果你希望 Codex 知道「這個專案怎麼啟動」，放 `AGENTS.md` 更合適。

如果你希望 Codex 知道「GitHub 上這個 PR 目前是否通過 CI」，那應該接 GitHub App 或 MCP。

### 第八步：怎麼連接 App，讓 Codex 真正操作起來

Codex 的強大，不只是能寫答案，而是能連接真實工具。

常見 App / Connector 包括：

App / Connector能做什麼GitHub讀倉庫、看 PR、查 Issue、處理 reviewGmail搜尋郵件、總結執行緒、提取待辦、起草回覆Google Drive / Docs讀取文件、整理資料Figma讀取設計稿、輔助設計到程式碼Browser打開網頁、測試本地頁面、截圖驗收Chrome使用你的 Chrome 登入狀態操作網頁Computer Use操作本地桌面應用這裡要分清楚兩個概念：

類型更像什麼MCP標準協定介面，適合工具和資料來源App / Connector已授權的真實應用連接舉個例子，你要讓 Codex 看 GitHub PR，有幾種可能：

- 透過 GitHub 外掛
- 透過 GitHub App / Connector
- 透過 MCP server
- 透過 `gh` CLI

它們不是互斥的。實際工作中，Codex 往往會優先使用目前環境裡已經可用、權限合適、最穩定的入口。

新手階段，你只要記住一個安全原則： **讀取可以更自動，寫入必須謹慎。** 比如讀取 PR、總結郵件、打開網頁，一般風險較低。

但發送郵件、提交表單、刪除檔案、發布文章、改權限、付款、發訊息，這些動作一定要讓 Codex 先停下來給你確認。把 AI 當成有人監督的助手，而不是完全放飛的機器人。

### 新手第一次設定，按這個順序來

別一開始就追求「全配滿」。

更好的順序是：

[![image.png](https://i.imgur.com/34eDhqI.jpeg)](https://i.imgur.com/34eDhqI.jpeg)

你可以按下面這套路線走：

1. 安裝 Codex，完成登入。
2. 執行 `codex doctor` ，確認環境正常。
3. 在專案根目錄寫一個簡單的 `AGENTS.md` 。
4. 只改必要的 `config.toml` ，先別開太多高權限。
5. 先接一個最常用的外掛，比如 GitHub 或 Browser。
6. 如果你經常重複某類任務，再寫 Skill。
7. 需要外部資料時，再設定 MCP。
8. 最後再考慮把長期偏好放進 Memory。

這套順序的好處是，你每一步都能驗證。

很多人一上來裝十幾個外掛、配五六個 MCP、寫一大堆全域規則，最後出問題時根本不知道是哪一層壞了。新手階段最重要的不是炫技，而是能定位問題。

### 給你一套基礎設定範本

下面這套範本可以作為起點，不建議無腦複製到所有專案裡，但你可以按自己的專案改。

#### 1\. 專案根目錄 AGENTS.md

```
markdown 代碼解讀複製代碼# AGENTS.md

## Project overview

Briefly describe what this project does and where the main code lives.

## Common commands

- Install: \`pnpm install\`
- Dev: \`pnpm dev\`
- Typecheck: \`pnpm typecheck\`
- Test: \`pnpm test\`
- Build: \`pnpm build\`

## Working rules

- Read existing code before editing.
- Keep changes focused on the requested task.
- Do not rewrite unrelated files.
-  existing naming and style.
- Ask before adding new dependencies.

## Verification

Before marking work complete:

- Run the most relevant check.
- If checks cannot run, explain why.
- Do not claim tests passed unless they were actually run.
```

#### 2\. 全域 config.toml

```
toml 代碼解讀複製代碼# ~/.codex/config.toml

# Pick the model you normally want Codex to use.
# 示例：按你帳號和目前 Codex 版本可用模型調整
model = "o3"

# Conservative default for local coding work.
sandbox_mode = "workspace-write"

# Let Codex ask when it needs permission for sensitive operations.
approval_policy = "on-request"

# You can add MCP servers below when needed.
# [mcp_servers.example]
# command = "node"
# args = ["server.js"]
```

如果你的 Codex 版本提示欄位不識別，先不要硬改，跑：

```
bash 代碼解讀複製代碼codex doctor
codex --help
```

再根據目前版本調整。

#### 3\. 一個最小 Skill 範本

```
markdown 代碼解讀複製代碼---
name: project-review
description: Use when reviewing a feature implementation before merge.
---

# Project Review

## Workflow

1. Read the requirement or issue.
2. Inspect changed files.
3. Look for bugs, regressions, missing tests, and risky assumptions.
4. Run or recommend relevant verification commands.
5. Report findings first, then summarize.

## Output

- Findings with file references.
- Test gaps.
- Suggested next steps.
```

#### 4\. MCP 新增命令範本

```
bash 代碼解讀複製代碼# HTTP MCP server
codex mcp add <name> --url <server-url>

# Local stdio MCP server
codex mcp add <name> -- <command> <args>

# Check configured MCP servers
codex mcp list
```

這裡最重要的是 `<name>` 要起得清楚，比如 `github` 、 `docs` 、 `postgres-readonly` ，不要叫 `test1` 、 `abc` 。後面排查問題時，清楚的名字會救命。

### 常見坑

#### 1\. 把 AGENTS.md 寫成散文

`AGENTS.md` 不是給人看的專案宣傳稿，而是給 Codex 執行任務時用的作業說明。少寫願景，多寫命令、約束和驗收方式。

#### 2\. 一開始就開最高權限

新手剛開始不要追求「無確認自動跑完」。AI 工具越能操作真實系統，越要有邊界。先用保守權限跑通工作流，再根據需要放開。

#### 3\. MCP 配好了但忘了驗證

配完 MCP 後，先跑：

```
bash 代碼解讀複製代碼codex mcp list
codex doctor
```

能列出來只是第一步，真正任務裡能不能呼叫，還要看認證、網路、權限和 server 本身是否正常。

#### 4\. 外掛裝太多

外掛不是越多越好。你真正需要的是工作流。

如果你每天都寫程式碼，先把 GitHub、Browser 這類接好。如果你每天寫文章，先把 Documents、Browser、圖片生成或預覽流程跑通。裝一堆不用的外掛，只會讓你更困惑。

#### 5\. 把 Memory 當事實庫

Memory 適合記偏好，不適合記即時事實。專案狀態、PR 狀態、資料庫資料、新聞變化，都應該從真實來源讀取。

#### 6\. 不區分「讀」和「寫」

讓 Codex 讀取資料、總結內容、檢查頁面，風險通常較低。

讓 Codex 發送郵件、發布文章、刪除檔案、提交表單、改權限，風險就高很多。涉及外部副作用的動作，最好都讓它先停下來確認。

### 我自己的建議

如果你剛開始用 Codex，我建議先別急著研究所有高階設定。

第一週只做三件事：

1. 每個專案補一個 `AGENTS.md` 。
2. 學會用 `codex doctor` 排查問題。
3. 只接一個你最需要的外掛或 App。

等你發現某個任務每週都在重複，比如寫文章、review PR、整理週報、做 PPT、查資料，再把它沉澱成 Skill 或外掛工作流。

Codex 真正厲害的地方，不是你一次 prompt 寫得多華麗，而是你能把自己的工作方式一點點沉澱下來。今天寫進 `AGENTS.md` 的專案規則，明天接上的 GitHub，後天固化成 Skill 的寫作流程，都會讓它越來越像你的工作台。

對新手來說，最好的入門路線不是「學完所有概念再開始用」，而是先跑起來，再把反覆出現的問題放到正確的位置。

這篇文章你如果只記住一張圖，就記住前面的七層設定地圖：

臨時需求放對話，專案規則放 `AGENTS.md` ，執行預設值放 `config.toml` ，重複流程放 `Skills` ，外部能力交給 `Plugins` 、 `MCP` 和 `Apps` ，個人偏好再放進 `Memory` 。

把這幾層分清楚，Codex 就不再是一堆陌生名詞，而是一套你能慢慢搭起來的 AI 工作系統。

我這邊也有一些 AI Coding（SDD Agent AI 工具等）和 Node 技術交流交流群，感興趣的可以加我微 **ikoala520** 進群，一起學習，共同進步。

### 參考資料

- OpenAI Codex 官方文件： [developers.openai.com/codex](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.openai.com%2Fcodex)
- Codex Desktop App： [developers.openai.com/codex/app](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.openai.com%2Fcodex%2Fapp)
- Codex CLI： [developers.openai.com/codex/cli](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.openai.com%2Fcodex%2Fcli)
- Codex GitHub Releases： [github.com/openai/code…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fopenai%2Fcodex%2Freleases)
- npm `@openai/codex` ： [www.npmjs.com/package/@op…](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2F%40openai%2Fcodex)
- Codex CLI 本機幫助： `codex --help`
- Codex MCP 命令幫助： `codex mcp --help`
- Codex 外掛命令幫助： `codex plugin --help`
- Model Context Protocol： [modelcontextprotocol.io](https://link.juejin.cn/?target=https%3A%2F%2Fmodelcontextprotocol.io)

---

原文出處： [https://juejin.cn/post/7661983059005816858](https://juejin.cn/post/7661983059005816858)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3ojwXx) [79瀏覽](https://codelove.tw/@tony/post/3ojwXx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3ojwXx)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

🔥 [首次亮相 NVIDIA Jetson Orin Nano Super - 最經濟實惠的生成式 AI 超級計算機](https://codelove.tw/@tony/post/qvpzV3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝7 💬2 ❤️4

223

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

109

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784516516&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3ojwXx&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI1Il1dLDBd&amp;dt=1784516516689&amp;bpp=2&amp;bdt=135&amp;idt=36&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784510841%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784510841%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784510841%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4541354098936&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=31099679%2C95396101%2C95396568%2C95391233%2C95396252&amp;oid=2&amp;pvsid=4467547014927234&amp;tmod=426775654&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=2067%2C30%2C2067%2C30%2C1920%2C30%2C1773%2C1050%2C1533%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.16&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=44" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260716/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>

軟體