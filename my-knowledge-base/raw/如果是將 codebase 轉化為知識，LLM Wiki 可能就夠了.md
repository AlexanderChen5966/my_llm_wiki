---
title: "如果是將 codebase 轉化為知識，LLM Wiki 可能就夠了"
source: "https://codelove.tw/@tony/post/qZrGQq"
author:
published:
created: 2026-08-03
description: "[](#%E5%89%8D%E6%9B%B8%E3%81%8D)前言-----------------------------------你是否也有過這樣的經驗：每次請求程式代理人進行修改時，都得一再重做同樣的調查，心裡總會想「這個修正到底會影響到哪裡」呢？![:frowning2:](htt..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月31日](https://codelove.tw/@tony/post/qZrGQq "2026-07-31 17:30:11")

## 如果是將 codebase 轉化為知識，LLM Wiki 可能就夠了

## 前言

你是否也有過這樣的經驗：每次請求程式代理人進行修改時，都得一再重做同樣的調查，心裡總會想「這個修正到底會影響到哪裡」呢？ [![:frowning2:](https://cdn.qiita.com/emoji/twemoji/unicode/2639.png)](https://cdn.qiita.com/emoji/twemoji/unicode/2639.png) [![:point_up:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-fe0f.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-fe0f.png)

不少人應該都已經把公司內部的規格書或設計文件放進向量資料庫，讓代理人去搜尋了。

雖然文件內容本身通常都能正確查到，但每次提出相同問題時，都得從向量搜尋的結果重新組裝答案，所以每問一次，措辭就會微妙地變動，搜尋成本也會一點一滴累積起來——你是否也對這種感受很有共鳴呢？

[![Gemini_Generated_Image_6vz29r6vz29r6vz2.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2Fdaf1222c-5c37-482f-9e34-9fc79c15bffa.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e2f552d8fa8d854a9e0082c84f7bd328)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2Fdaf1222c-5c37-482f-9e34-9fc79c15bffa.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=e2f552d8fa8d854a9e0082c84f7bd328)

作為解決這個問題的一種思路，有一個叫做「 **LLM Wiki** 」的方法。

本文將先回顧傳統搜尋型工具的限制，再整理「LLM Wiki」的概念，最後介紹如何實際執行一個名為 OpenWiki、具有類似「LLM Wiki」機制的函式庫。

內容為截至 2026 年 7 月 28 日的資訊。OpenWiki 是公開不久、更新速度很快的工具，因此命令與版本請以官方儲存庫的最新內容為準。

## 代理人指示檔與圖譜 RAG 工具的限制

要讓程式碼庫被程式代理人理解，大致可分成兩種做法。

一種是在 `CLAUDE.md` 或 `AGENTS.md` 這類代理人指示檔中寫好規則。  
另一種則是使用 **Bedrock Knowledge Bases** 或 **GitNexus** 這類服務／函式庫，把程式碼與文件知識化，再以搜尋工具的形式交給代理人。

這兩種方式都很方便，但各自也都有問題。

1. 第一個問題，是每次提問都要重新呼叫工具的成本。 [![:chart:](https://cdn.qiita.com/emoji/twemoji/unicode/1f4b9.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f4b9.png)  
	經由圖譜取得資訊時，會在每個查詢都沿著圖譜重新走訪、重新組裝。第 10 次的答案也不會比第 1 次更好，卻得為同樣的探索成本付 10 次費用——很容易變成這樣的結構。
2. 第二個問題是，像 `CLAUDE.md` 這類文件如果不自行更新，就會逐漸過時。實作變了，但文件沒跟著變，結果不知不覺把錯誤資訊提供給代理人，這種情況就會發生。 [![:fallen_leaf:](https://cdn.qiita.com/emoji/twemoji/unicode/1f342.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f342.png)

Anthropic 的官方部落格中，列出了那些能讓程式碼庫被代理人有效理解的公司所共有的運作模式，例如：

- `CLAUDE.md` 依資料夾分層，並保持簡潔
- 導入 LSP，提高探索的正確性
- 不把責任全丟給個人，由團隊負責人制定標準

此外，隨著 AI 模型持續更新，過去為了補足限制而寫下的舊指示，反而可能拖新模型的後腿。因此，文章也提到需要每 3～6 個月定期檢視一次。

說到底，維持指示檔的新鮮度，或是維護搜尋工具本身，最後都會成為組織層級的運作成本。 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

## LLM Wiki 的概念

原本像 Wikipedia 這樣的 Wiki，就是有人先把資訊調查、整理好，再彙整成頁面，讓下一個想知道同樣內容的人只要讀那一頁就好。

「LLM Wiki」則是把撰寫與維護這些頁面的角色，從人類交給代理人，也就是一種 **由代理人維運的 Wiki** 。

[![AIエージェント開発フレームワークはどう選ぶ (1).png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F8a31332f-95ff-464c-b37e-c2adb4580af8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8938ec94e575437eee722ccadf5c1a61)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F8a31332f-95ff-464c-b37e-c2adb4580af8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8938ec94e575437eee722ccadf5c1a61)

概念本身很簡單，  
就是 **不是在被提問時才生成資訊，而是在資料匯入時就先把資訊準備好** 。 [![:star2:](https://cdn.qiita.com/emoji/twemoji/unicode/1f31f.png)](https://cdn.qiita.com/emoji/twemoji/unicode/1f31f.png)

RAG 是每次提問都從原始資料重新組裝答案；而 LLM Wiki 則是在一開始先讀一次來源，並把結果保留成 Markdown 頁面。

只要來源變了，更新那一頁就好。

整體架構有三層：

1. 來源文件（文章、程式碼等）
2. Wiki（由模型撰寫的 Markdown 檔，包含摘要與頁面間連結）
3. 結構檔（例如 `CLAUDE.md` 或 `AGENTS.md` ，用來指示模型 Wiki 的結構與應做的工作）

[![Gemini_Generated_Image_5g1sth5g1sth5g1s.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F2902c9a5-caca-487d-9a99-373fcce6da08.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=207454443c2000ea133fd122a41a5669)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F2902c9a5-caca-487d-9a99-373fcce6da08.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=207454443c2000ea133fd122a41a5669)

2026 年 4 月，Andrej Karpathy 在 GitHub Gist 上寫下了這個方法，並將其命名為「LLM Wiki」。

之後，Cognition 的「DeepWiki」、LangChain 的「OpenWiki」等具相似機制的工具也接連出現。

| 工具 | 開發商 | 特點 |
| --- | --- | --- |
| DeepWiki | Cognition | 面向公開 GitHub 儲存庫，只要把 URL 中的 `github.com` 換成 `deepwiki.com` 就能產生 |
| AutoWiki | Factory | 整合進 CI/CD，程式碼更新時自動重新生成 |
| OpenWiki | LangChain | OSS，不只程式碼，還能匯入 Gmail / Notion / X 等資料；具備「Personal Brain」功能 |
| GBrain | Garry Tan | 只在 Git 儲存庫內的 Markdown 中完成的簡易版本 |

文中也有清楚寫出限制，據說當來源數量超過 100 筆時，就需要與搜尋工具搭配使用。 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

## 試用 OpenWiki

接下來，我實際把 OpenWiki 對準本機儲存庫跑一次看看。

OpenWiki 是 LangChain 在 2026 年 7 月 1 日公開的 OSS CLI。  
GitHub 儲存庫建立於 2026 年 6 月 22 日，撰文當下的最新版本是 v0.2.4，星號數已超過 13,000，成長速度相當驚人。

它有兩種模式：針對程式碼庫的 **Code mode** ，以及匯總 Gmail、Notion、X 等個人資料的 **Personal mode** ；這次的目的在於理解專案程式碼，所以我使用 Code mode。 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

### 安裝與初始化

```
npm install -g openwiki
# or
pnpm add -g openwiki
```

安裝完成後，進入要初始化 openwiki 的專案，執行以下指令進行初始化：

```
openwiki --init
```

我的情況是，之前已經對另一個專案初始化過一次，所以當我在另一個專案再次執行時，系統記住了前一次的設定，並顯示出如下介面：

```
% openwiki --init
╭──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ OpenWiki first-run setup                                                                                                                                                 │
│ Configure the model, wiki scope, and sources.                                                                                                                            │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

  Detected from your command
    ✓ Run mode         Code
    ✓ Wiki scope       openwiki/

  Set up
    ❯ Provider         AWS Bedrock
    ✓ AWS credentials  legacy Bedrock keys (take precedence)
    ✓ Region           us-east-1
    ✓ Model            us.anthropic.claude-sonnet-5
    ✓ LangSmith        configured

┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ Prompt                                                                                                                                                                   │
│ Choose a model provider.                                                                                                                                                 │
│   OpenAI (openai) default                                                                                                                                                │
│   OpenAI (ChatGPT login) (openai-chatgpt)                                                                                                                                │
│   Anthropic (anthropic)                                                                                                                                                  │
│   GitHub Copilot (copilot)                                                                                                                                               │
│   Gemini (AI Studio) (gemini)                                                                                                                                            │
│   Gemini Enterprise (Vertex AI) (gemini-enterprise)                                                                                                                      │
│   OpenRouter (openrouter)                                                                                                                                                │
│   OpenAI-compatible (openai-compatible)                                                                                                                                  │
│ > AWS Bedrock (bedrock)                                                                                                                                                  │
│   Fireworks (fireworks)                                                                                                                                                  │
│   Baseten (baseten)                                                                                                                                                      │
│   Nebius Token Factory (nebius)                                                                                                                                          │
│   NVIDIA NIM (nvidia)                                                                                                                                                    │
│ Use up/down arrows, then press Enter.                                                                                                                                    │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

先說個注意事項：這次我是用 Bedrock 來初始化，但目前若使用 Bedrock，API 金鑰或 SSO 的存取金鑰無法使用。

請建立 IAM 使用者，並授予下列權限，或直接授予 AmazonBedrockFullAccess，之後使用發行出來的存取金鑰。

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "InvokeModel",
            "Effect": "Allow",
            "Action": [
                "bedrock:InvokeModel",
                "bedrock:InvokeModelWithResponseStream"
            ],
            "Resource": [
                "arn:aws:bedrock:*::foundation-model/*",
                "arn:aws:bedrock:{{<region>}}:{{<account-id>}}:inference-profile/*"
            ]
        }
    ]
}
```

使用 Bedrock 時，除了 IAM 使用者的存取金鑰之外，還必須輸入要使用哪個區域、哪個模型。

另外也會出現是否將追蹤資料送到 LangSmith 的選項，不過它可以免費使用，所以建議先開啟。

你需要的是 LangSmith 的 API 金鑰，因此請到 Settings 裡的「API Keys」建立後貼上。

[![名稱未設定.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F8ae03c02-c58d-4e8e-a644-a3db164a164b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6ca90b70a41c3f42b4d8b04ba1ae21b5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F8ae03c02-c58d-4e8e-a644-a3db164a164b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6ca90b70a41c3f42b4d8b04ba1ae21b5)

接著會詢問要用什麼提示詞來初始化 Wiki，不過一開始維持預設設定即可。

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ Prompt                                                                                                                                                                   │
│ Customize what this wiki should understand.                                                                                                                              │
│ Mode: Code                                                                                                                                                               │
│ Edit the brief below. Keep what is useful, delete what is not.                                                                                                           │
│                                                                                                                                                                          │
│ Edit wiki brief                                                                                                                                                          │
│ ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐                                                                     │
│ │ > A code wiki for this local repository. Prioritize a concise quickstart, architecture overview, │                                                                     │
│ │  source map, key workflows, domain concepts, operations/runbook notes, testing guidance, and     │                                                                     │
│ │ integration points. Inspect git history to understand reasoning behind code changes and the      │                                                                     │
│ │ progression of the repository. Keep pages grounded in the repository structure and recent code   │                                                                     │
│ │ changes. Prefer practical navigation for engineers over generic summaries.                       │                                                                     │
│ └──────────────────────────────────────────────────────────────────────────────────────────────────┘                                                                     │
│ Press Enter to continue.                                                                                                                                                 │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
  esc to go back
```

到這裡，初始化用的選項就都結束了。  
接下來會出現設定選項，選擇 `Run OpenWiki now` 進行初始化即可。

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ Prompt                                                                                                                                                                   │
│ Setup is complete.                                                                                                                                                       │
│ > Run OpenWiki now                                                                                                                                                       │
│   Open chat                                                                                                                                                              │
│ Run now writes the initial openwiki/ directory. Open chat skips the initial run.                                                                                         │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
  esc to go back
```

之後就會開始初始化。  
如果已經串接 LangSmith，也可以在儀表板上確認追蹤資訊。

這次使用的模型是 Sonnet 5，光是初始化就花了大約 1.5 美元。

[![名稱未設定.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F33fddc4c-a5f2-454f-82c4-d0edf0139155.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=68e104b2aef4b99728c38484ff69672e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F320164%2F33fddc4c-a5f2-454f-82c4-d0edf0139155.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=68e104b2aef4b99728c38484ff69672e)

初始化完成後，主要會有三個變化。

**1**. 代理人說明檔（ `CLAUDE.md` 或 `AGENTS.md` ）中，新增了使用 OpenWiki 時的注意事項。有趣的是，這個專案同時也有使用 GitNexus，因此 GitNexus 相關內容也被結構化了，讓代理人比較不容易迷路。

[CLAUDE.md](http://claude.md/)

```
<!-- OPENWIKI:START -->

## OpenWiki

This repository uses OpenWiki for recurring code documentation. Start with \`openwiki/quickstart.md\`, then follow its links to architecture, workflows, domain concepts, operations, integrations, testing guidance, and source maps.

The scheduled OpenWiki GitHub Actions workflow refreshes the repository wiki. Do not hand-edit generated OpenWiki pages unless explicitly asked; prefer updating source code/docs and letting OpenWiki regenerate.

<!-- OPENWIKI:END -->

<!-- GitNexus:START -->
  ...GitNexusの説明
<!-- GitNexus:END -->
```

**2**. 建立了 GitHub Actions 工作流程，會依照程式碼庫的變更更新 Wiki。

**3**. 建立了 `openwiki/` 目錄，Wiki 頁面本體會輸出到那裡。

實際在本機專案初始化後，內容大致會是這樣的結構：

- `index.md` — 整體入口點
- `INSTRUCTIONS.md` — 要 Wiki 撰寫哪些內容的簡述
- `quickstart.md` — 快速入門頁面
- `architecture/` — 架構總覽與主要工作流程
- `domain/` — 領域知識
- `operations/` — 維運與開發指南

像 `architecture/` 、 `domain/` 、 `operations/` 這類分類目錄內，也都會有自己的 `index.md` 。這可以視為該章節的目錄，整理了底下各頁面的連結與簡短說明。

各個頁面（例如 `quickstart.md` ）都會以 YAML frontmatter 附上 `type` 、 `title` 、 `description` 、 `tags` ，因此也很適合機器處理。

[quickstart.md](http://quickstart.md/)

```
---
type: Quickstart
title: {project_name}快速入門
description: 說明
tags: [quickstart, overview, amplify, bedrock]
---

# {project_name}快速入門

## 這是什麼
......
```

另外，`.last-update.json` 會記錄最後更新時間、執行的命令、當時的 git HEAD，以及所使用的模型，方便追蹤 Wiki 是根據哪個時間點的程式碼撰寫而成。

.last-update.json

```
{
  "updatedAt": "2026-07-28T01:30:33.244Z",
  "command": "update",
  "gitHead": "xxxx",
  "model": "us.anthropic.claude-sonnet-5"
}
```

若要讓 Wiki 保持在最新狀態，可使用下列指令更新，不過會產生成本，請多加注意。

```
openwiki --update
```

### Personal mode 的內容

前面介紹的是針對儲存庫程式碼的 **Code mode** ，而 OpenWiki 還有另一個 **Personal mode** ，可匯入個人資料並整理到 `~/.openwiki/wiki` 。

支援的連接器包括 Gmail、Notion、X、Web Search（透過 Tavily）、Hacker News，以及本機 Git 儲存庫；而且也可以註冊多個相同類型的連接器並並行運作。

```
openwiki personal --init
openwiki ingest all
```

初始化使用 `openwiki personal --init` ，更新則使用 `openwiki personal --update` 。需要 OAuth 的連接器（例如 Gmail、Notion、X 等）則必須事先用 `openwiki auth <provider>` 完成認證。

若要一次匯入所有連接器，就執行 `openwiki ingest all` ；若只要特定連接器，則像 `openwiki ingest web-search` 這樣指定名稱執行即可。

其內部處理流程是，先只完成帶有認證資訊的網路呼叫並保存原始資料，之後再由合成用代理人讀取這些資料、重寫 Wiki 頁面，採取兩段式處理。

把需要憑證的處理與 LLM 運行的處理分開，雖然不起眼，但這樣的設計讓人很安心。 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

### 用.openwikiignore 排除不想讓它讀取的路徑

在儲存庫根目錄放置 `.openwikiignore` ，就可以用與 `.gitignore` 相同的寫法（註解、空行、 `*` 與 `**` 的 glob、目錄指定、以及用 `!` 排除）來指定不希望 Wiki 讀取的路徑。

.openwikiignore

```
secrets/
*.log
!logs/keep.log
```

指定的路徑會在檔案系統探索階段就被排除，生成的頁面中也不會出現。

不過這不算是完整的機密保護機制；文件也明確寫到，模型仍可能從測試、README、提交訊息、既有 Wiki 等其他線索，推測出被排除區域的存在並寫進頁面。

### 關於遙測

OpenWiki 預設會傳送匿名使用遙測。可蒐集的內容包括指令（init/update）與成功／失敗結果、失敗時的大致錯誤類型，以及 init 時的模式、供應商、設定的連接器名稱等；不會包含檔案內容、儲存庫名稱、認證資訊、提示詞、模型輸出、錯誤訊息全文等。

如果在意的話，可以用環境變數 `OPENWIKI_TELEMETRY_DISABLED=1` （或業界標準的 `DO_NOT_TRACK=1` ）來停用。

執行時加上 `--telemetry-file=<path>` ，也可以把實際送出的內容直接輸出到檔案中確認。

## 最後

實際使用後，最讓我覺得合理的不是工具本身有多方便，而是「LLM Wiki」這個概念本身。它和每次提問都要從原始資料重新組裝答案的 RAG 不同，而是在匯入資料的當下先付一次成本。對於人類來說很快就會過時的 Wiki 維護工作，也可以交給不嫌麻煩的代理人來做，這才是核心。

先拿手邊的小型儲存庫試試看，是最快的方法。意外地，很快就能跑起來。 [![:point_up_tone1:](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)](https://cdn.qiita.com/emoji/twemoji/unicode/261d-1f3fb.png)

---

原文出處： [https://qiita.com/Syoitu/items/ff38655fed51a2920910](https://qiita.com/Syoitu/items/ff38655fed51a2920910)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qZrGQq) [52瀏覽](https://codelove.tw/@tony/post/qZrGQq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qZrGQq)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 ❤️4

295

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 💬4 ❤️1

58

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785721197&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqZrGQq&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuNzEiLG51bGwsMCxudWxsLCI2NCIsW1siTm90PUE_QnJhbmQiLCI5OS4wLjAuMCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MS4wLjc5MjIuNzEiXSxbIkNocm9taXVtIiwiMTUxLjAuNzkyMi43MSJdXSwwXQ..&amp;dt=1785721196588&amp;bpp=2&amp;bdt=125&amp;idt=415&amp;shv=r20260729&amp;mjsv=m202607280101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785721193%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785721193%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3913671042940&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95396138&amp;oid=2&amp;pvsid=8237693238270330&amp;tmod=1434000446&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=477" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260729/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=10&amp;uci=a!a&amp;btvi=4" title="Advertisement" aria-label="Advertisement"></iframe>