---
title: "Claude Code Skills：讓 AI 變身專業工匠"
source: "https://kaochenlong.com/claude-code-skills"
author:
  - "[[高見龍]]"
published: 2026-01-03
created: 2026-05-14
description: "Claude Code Skills（Agent Skills）是什麼？本文整理 SKILL.md 結構、Progressive Disclosure、與 Slash Commands/MCP/Subagents 的差異，並示範如何建立自己的 Skill＋安全注意事項。"
tags:
  - "clippings"
status: processed
---
![Claude Code Skills：讓 AI 變身專業工匠](https://cdn.kaochenlong.com/rails/active_storage/representations/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MjM3MCwicHVyIjoiYmxvYl9pZCJ9fQ==--5c006b9f8a976a5e357b47bd9679bc52bbfcfe36/eyJfcmFpbHMiOnsiZGF0YSI6eyJmb3JtYXQiOiJ3ZWJwIiwicmVzaXplX3RvX2ZpbGwiOlsxMjgwLDQ0OF19LCJwdXIiOiJ2YXJpYXRpb24ifX0=--ecffecc7310878204e6133d2b5591dfd779a0246/claude-code-skills-cover.jpg)

credit: Nano Banana

如果你已經用 Claude Code 一段時間，每次開新專案，Claude 都像個剛入職的新人，什麼都得從頭說明。你得告訴它「我們團隊用的是這套 Coding Style」、「Deploy 流程是這樣跑的」、「這個 API 要這樣串」。講一次還好，講十次、二十次之後就會開始懷疑到底我花錢是在用 AI 還是在當 AI 的助理？

你應該已經知道 `CLAUDE.md` 這個檔案的用途。把專案的慣例和規則寫在 `CLAUDE.md` 裡，Claude Code 啟動時就會自動讀取，省去每次重複解釋的麻煩。 `CLAUDE.md` 常被拿來放專案指引，雖然 Claude Code 也支援使用者層級的 `~/.claude/CLAUDE.md` 可以跨專案共用，但不管放哪一層，指引一長就會遇到難維護、而且啟動時全部載入的問題，我還遇過 Claude Code 提醒我再繼續下去效能會變差...

![claude-code-skills-1](https://cdn.kaochenlong.com/rails/active_storage/representations/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MjM2OCwicHVyIjoiYmxvYl9pZCJ9fQ==--b18b8267f9c072ddf9e5d2556bd92f3cd1735239/eyJfcmFpbHMiOnsiZGF0YSI6eyJmb3JtYXQiOiJ3ZWJwIiwicmVzaXplX3RvX2xpbWl0IjpbMTI4MCwxMDI0XSwicXVhbGl0eSI6ODV9LCJwdXIiOiJ2YXJpYXRpb24ifX0=--0b69e9fe8af003acacbcd26b0aac87dbbf711297/claude-code-skills-1.webp)

重點是， `CLAUDE.md` 的內容會在專案啟動的時候就全部載入，不管當前的任務用不用得到。

有沒有什麼方法，可以把「專業知識」打包成獨立的模組，讓 Agent 自己判斷什麼時候該用、只載入需要的部分、而且可以跨專案重複使用？嘿嘿，有的，就是 Anthropic 在 2025 年推出的 Skills 功能。

## 什麼是 Skills？

根據 Anthropic 的 [官方定義](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) ，Skills 是這樣的東西：

Skills are modular, self-contained packages that extend Claude's capabilities by providing specialized knowledge, workflows, and tools. Think of them as 'onboarding guides' for specific domains or tasks—they transform Claude from a general-purpose agent into a specialized agent equipped with procedural knowledge.

翻成白話文就是 Skills 是一種打包好的「專業技能包」，可以把 Claude 從一個什麼好像都略懂的通才，變成某個領域的專家。

想像你開了一間小吃店，你是一位什麼料理都會做、都能做但可能都做的不太好吃的廚師。有天我經過你的小吃店，我看你骨骼精奇，是一位百年難得一見的練武奇才，覺得維護世界的和平就要靠你了，所以決定給你一本「台南小吃完全手冊」，裡面寫著擔仔麵的湯頭怎麼熬、肉燥要用什麼部位的豬肉、蝦仁要去哪個市場買最新鮮、還有那個獨門醬油膏的調配比例。看完這本秘笈之後，你就能做出道地的府城味了。

Skills 就是這本「武功秘笈」。Skills 能提供的東西很多，包括：

- 專業工作流程：多步驟的作業程序，例如「怎麼做 Code Review」、「怎麼處理 PR」
- 工具整合：跟特定檔案格式或 API 互動的方法，例如「怎麼處理 PDF」
- 領域專業知識：你們公司或團隊特有的商業邏輯和慣例
- 資源：腳本、參考文件、範本等執行任務時需要的素材

## Skills 長什麼樣子？

一個 Skills 的基本結構很簡單，至少需要一個 `SKILL.md` 檔案：

```
skill-name/
└── SKILL.md   # 必要的
```

這個 `SKILL.md` 檔案開頭必須包含一段 YAML frontmatter：

```yaml
---
name: skill-name
description: A description of what this skill does and when to use it.
---
```

根據 [agentskills.io](https://agentskills.io/specification) 的規格， `name` 欄位有一些規則要遵守：

- 長度在 1 到 64 個字元之間
- 只能用小寫字母、數字和 `-`
- 不能以 `-` 開頭或結尾，也不能有連續的 `-`
- 必須與目錄名稱一致，待會我們實作的時候就會看到

`description` 欄位也有限制：

- 長度在 1 到 1024 個字元之間，所以這裡不是寫執行細節的地方
- 應該描述這個 Skills 做什麼、什麼時候該用

frontmatter 之後的內容才是給 Agent 看的指令內容。你可以在裡面寫任何你想讓 Agent 知道的東西，例如操作步驟、注意事項、範例用法等等。如果你的 Skills 比較複雜，需要額外的腳本或參考資料，可以建立這樣的目錄結構：

```
skill-name/
├── SKILL.md      # 必要的
├── scripts/      # 可執行的程式碼（Python、Bash、JavaScript）
├── references/   # 額外的參考文件
└── assets/       # 靜態資源（範本、圖片、資料檔）
```

根據 [Agent Skills 規格](https://agentskills.io/specification) ，這三個目錄都是 Optional directories，也就是要加不加都可以。這些目錄名稱是規格建議的標準結構，讓不同的 Skills 有一致的組織方式。Claude 是透過你在 `SKILL.md` 裡的引用來發現這些檔案的，所以你需要在 `SKILL.md` 裡明確引用，Claude 才會知道它們的存在。

## Progressive Disclosure

這裡要講一個我認為 Skills 設計得最漂亮的地方，叫做 Progressive Disclosure（漸進式揭露）。

這概念有點像用 Google Maps 導航。當輸入目的地之後，它不會一次把整條路線的每個細節都唸出來，而是先給一個大方向，例如「往北走，大約 10 分鐘後右轉」。等快到路口了，才會接著說「前方 50 公尺右轉，進入衡陽路」。如果中途想找加油站或停車場，它才會載入附近的資訊，Skills 的運作方式就是這樣。

根據 Anthropic 部落格的 [說明](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) ，Skills 的內容分成三層：

第一層是 Metadata，大概只有 100 個 tokens 左右。這層只包含 `name` 和 `description` 兩個欄位。當 Claude Code 啟動的時候，會預載所有已安裝 Skills 的 Metadata，用來判斷「這個任務跟哪些 Skills 有關」。

第二層是 Instructions，就是 `SKILL.md` 的主體內容。依 [Agent Skills 規格](https://agentskills.io/specification) 建議，這層最好控制在 5000 個 tokens 以內。當 Claude 判斷某個 Skills 跟當前任務相關，才會把這層載入。

第三層是 Resources，就是 `scripts/` 、 `references/` 、 `assets/` 這些目錄裡的檔案。這些內容是按需載入的，Claude 需要用到哪個檔案才會去讀取。

這樣設計的好處在於只要把 Skills 正確地分層，由於檔案和資源是按需讀取的，理論上可以在一個 Skills 裡打包無限量的知識。在 Blog 文章裡有提到一句：

> This means that the amount of context that can be bundled into a skill is effectively unbounded

因為 context window 是有限的，如果每次對話都把所有知識塞進去，就算是像是有百萬 context window 的 Gemini，撐爆也只是早晚的問題。Progressive Disclosure 的設計讓 Claude 可以「用多少、拿多少」，不浪費 context 又能在需要的時候取得足夠完整的資訊。

## Skills 跟其他機制有什麼不同？

如果你用過 Claude Code 一段時間，應該會發現有好幾種方法可以「客製化」Claude 的行為。Skills 跟其他機制有什麼差別？什麼時候該用哪一種？我整理一個比較表：

<table><colgroup><col> <col> <col> <col> <col> <col> <col></colgroup><tbody><tr><th colspan="1" rowspan="1">機制</th><th colspan="1" rowspan="1">觸發方式</th><th colspan="1" rowspan="1">持久性</th><th colspan="1" rowspan="1">內容類型</th><th colspan="1" rowspan="1">載入時機</th><th colspan="1" rowspan="1">可包含程式碼</th><th colspan="1" rowspan="1">適用場景</th></tr><tr><td colspan="1" rowspan="1">Skills</td><td colspan="1" rowspan="1">Claude 自動判斷何時啟用（根據 description）</td><td colspan="1" rowspan="1">跨對話、跨專案可用</td><td colspan="1" rowspan="1">程序性知識 + 可執行腳本</td><td colspan="1" rowspan="1">動態載入（Progressive Disclosure）</td><td colspan="1" rowspan="1">是</td><td colspan="1" rowspan="1">重複性專業工作流程、需要 Claude 主動判斷的情境</td></tr><tr><td colspan="1" rowspan="1">Custom Commands</td><td colspan="1" rowspan="1">使用者輸入 <code>/command</code> （也可引導 Claude 代為呼叫）</td><td colspan="1" rowspan="1">專案或個人層級</td><td colspan="1" rowspan="1">Prompt 範本</td><td colspan="1" rowspan="1">使用者觸發時載入</td><td colspan="1" rowspan="1">否（單一 prompt 檔）</td><td colspan="1" rowspan="1">固定格式的重複操作、使用者明確知道要做什麼的情境</td></tr><tr><td colspan="1" rowspan="1">MCP</td><td colspan="1" rowspan="1">工具呼叫</td><td colspan="1" rowspan="1">啟動時載入</td><td colspan="1" rowspan="1">外部服務連接</td><td colspan="1" rowspan="1">按需呼叫</td><td colspan="1" rowspan="1">是（server 端）</td><td colspan="1" rowspan="1">連接外部 API、資料庫、檔案系統</td></tr><tr><td colspan="1" rowspan="1">Subagents</td><td colspan="1" rowspan="1">Claude 自動委派或手動啟動</td><td colspan="1" rowspan="1">任務期間</td><td colspan="1" rowspan="1">獨立 AI 實例</td><td colspan="1" rowspan="1">需要時建立</td><td colspan="1" rowspan="1">是</td><td colspan="1" rowspan="1">需要獨立 context、平行處理、專門化任務</td></tr></tbody></table>

我來解釋一下這些機制的差異。

Custom Commands（也就是斜線命令）需要你手動輸入 `/say-something` 才會觸發。根據 [官方文件](https://code.claude.com/docs/en/slash-commands) 的說法，斜線命令是「A Markdown file containing a prompt that Claude executes when invoked」，也就是說它只能包含 prompt 範本，不會有程式腳本或其他資源。你可以把它想成一種「巨集」，預先寫好一段指令，之後一鍵展開執行。它適合用在你已經知道要做什麼的情況，例如每次 commit 前都要跑同一套檢查流程，像我就寫了好幾個這樣的指令，像是 `/remove-comment` 用來移除不必要的註解，或是 `/sanitize-ai` 用來清理 AI 產生的「機味」文字。

但 Skills 不一樣，Agent 會自動根據當時的對話內容或情境判斷要不要啟用某個 Skill。假設你裝了一個翻譯用的 Skill，當我們跟 Agent 說「幫我把這段英文翻成中文」，Agent 會自己判斷「喔，這個任務跟翻譯有關，我應該要使用那個翻譯的 Skill」，不需要我們特別去觸發。

Skills 沒有像 Slash Commands 那樣「輸入固定 /xxx 就必定執行」的觸發機制，它主要由模型依 description 做相似度判斷來決定要不要啟用。官方文件說 Skills 是「model-invoked」，也就是由 Agent 自己決定什麼時候用。如果你需要手動觸發的功能，那應該用 Custom Commands 而不是 Skills。不過你可以在對話中明確提到 Skills 的功能來引導 Agent，例如說「用我們的翻譯規則把這段翻成中文」，讓 Agent 更容易判斷該啟用哪個 Skill。

MCP（Model Context Protocol）也有類似的自動判斷機制，但兩者運作的層次不同。Skills 提供的是「知識」，Agent 載入後會根據這些知識來「指導自己」的行為。MCP 提供的是「工具」，Agent 會呼叫這些工具來執行具體操作。

再回到剛才翻譯的例子，如果我們要做翻譯，Skills 會告訴 Agent「翻譯的時候要用什麼語氣、專有名詞怎麼處理、哪些詞不要翻」，而 MCP 則是讓 Agent 可以存取你的術語表資料庫、或是呼叫 DeepL API 來輔助翻譯。一個是腦袋裡的知識，一個是手上的工具。

MCP 是用來連接外部服務的。如果我們需要請 Agent 存取資料庫、呼叫某個 API、或是操作檔案，就可以使用 MCP。根據 [官方定義](https://code.claude.com/docs/en/mcp) ，MCP「enables Claude Code plugins to integrate with external services and APIs by providing structured tool access」。

是說每次有新東西出現，總是就會有些先知會丟出「取代論」的說法，最近社群有些討論說 Skills 會取代 MCP，但就我看並不會，因為這兩個解決的是不同層次的問題。Skills 是給 Agent「腦袋」，MCP 是給 Agent「手腳」。你可以同時用 Skills 教 Agent 怎麼做翻譯，又用 MCP 讓它能存取術語庫，兩者之間是互補而不是互斥或互相取代的關係。

至於 Subagents，根據 [官方說明](https://code.claude.com/docs/en/sub-agents) ，它是「autonomous subprocesses that handle complex, multi-step tasks independently」。當一個任務太複雜、需要獨立的 context 空間、或是可以平行處理的時候，Claude 會啟動 Subagents 來幫忙。

簡單來說：

- 想讓 Agent 自己判斷什麼時候用什麼專業知識？用 Skills
- 想手動觸發固定的操作流程？用 Custom Commands
- 想連接外部服務？用 MCP
- 想平行處理複雜任務？讓 Claude 用 Subagents

這四個機制不是互斥的，可以同時運作。舉個例子：使用 Custom Command `/review-pr` 觸發 Code Review 流程，Claude 會載入 Code Review 的 Skills 來取得審查標準，同時透過 MCP 連接 GitHub API 拉取 PR 內容，如果 PR 改動的檔案很多，Claude 可能還會啟動 Subagents 平行審查不同的檔案。四個機制各司其職然後一起完成任務，看到 AI 很忙的樣子，看起來不是很酷嗎:)

## 建立自己的 Skill

聽這麼多理論手癢了嗎？來實際動手做一個 Skills 吧！

從一個大家比較常會遇到的情境開始講起。我猜很多工程師都不喜歡寫 Git 的 Commit Message，我也是。如果我想要建立一個可以幫忙處理 Commit Message 的 Skill，讓 Agent 不只幫我們寫，而且還要遵循特定格式...

第一步，建立一個資料夾，名稱隨意：

```bash
mkdir commit-message-helper
```

第二步，在這個目錄裡建立 `SKILL.md` 檔案，內容如下：

```
---
name: commit-message-helper
description: Helps write Git commit messages following the Conventional Commits specification. Use this skill when the user asks to commit changes, write commit messages, or mentions git commits.
---

# Commit Message Helper

When writing commit messages, follow these rules:

## Format

<type>(<scope>): <subject>

<body>

<footer>

## Types

- feat: A new feature
- fix: A bug fix
- docs: Documentation only changes
- style: Changes that do not affect the meaning of the code
- refactor: A code change that neither fixes a bug nor adds a feature
- perf: A code change that improves performance
- test: Adding missing tests or correcting existing tests
- chore: Changes to the build process or auxiliary tools

## Guidelines

1. Subject line should be no longer than 50 characters
2. Use imperative mood ("add feature" not "added feature")
3. Do not end the subject line with a period
4. Separate subject from body with a blank line
5. Use the body to explain what and why, not how

## Examples

Good:
feat(auth): add OAuth2 login support

Implement OAuth2 authentication flow to allow users to log in
with their Google or GitHub accounts.

Closes #123

Bad:
updated stuff
```

別忘了 `name` 欄位要跟資料夾名稱一樣，這是規格要求的。再來注意 description 欄位的寫法，這裡特別寫了「when the user asks to commit changes, write commit messages, or mentions git commits」，這樣 Claude 才知道「什麼時候」該啟用這個 Skill。description 寫得越具體，Claude 的判斷就越準確。

是說，如果不知道這個檔案的內容怎麼辦？問 AI 啊！把情境跟 AI 講，不管哪一家的 AI 應該都有辦法寫出來，上面的內容就是我請 Claude 幫我寫的。

如果你的 Skills 需要執行腳本，可以建立 `scripts/` 目錄然後把程式碼放裡面。例如我想加一個驗證 commit message 格式的腳本，就把這個腳本放在 `scripts` 目錄裡，例如叫做 `validate_commit.py` ：

```python
import re
import sys

def validate_commit_message(message):
    pattern = r'^(feat|fix|docs|style|refactor|perf|test|chore)(\(.+\))?: .{1,50}'
    if re.match(pattern, message.split('\n')[0]):
        return True, "Valid commit message format"
    return False, "Invalid format. Expected: type(scope): subject"

if __name__ == "__main__":
    message = sys.argv[1] if len(sys.argv) > 1 else ""
    valid, msg = validate_commit_message(message)
    print(msg)
    sys.exit(0 if valid else 1)
```

光把腳本放進 `scripts/` 目錄還不夠，你需要在 `SKILL.md` 裡加註說明，告知 Agent 這個腳本的存在和用法：

```
## Validation

Before committing, run the validation script to check the format:

python scripts/validate_commit.py "your commit message"
```

這樣 Agent 才知道有這個工具可以用。

最後，把這個 Skills 安裝到 Claude Code。有兩種放法：

- 放到 `~/.claude/skills/` ：這是個人層級，你在任何專案都能用
- 放到專案的 `.claude/skills/` ：這是專案層級，只有這個專案能用，但只要專案裡有這個 Skill，團隊成員也都能用。

萬一兩邊有同名的 Skill，根據 [官方文件](https://code.claude.com/docs/en/skills) ，個人層級的會蓋過專案層級的 Skill。

## 寫好 Skills 的技巧

我自己土砲做了幾個自己有在用的 Skills 之後，有一些小小的心得跟大家分享。首先是 description 的寫法，這應該是整個 Skills 最重要的部分，因為它決定了 Agent 什麼時候或是會不會啟用這個 Skills。不好的寫法：

```yaml
description: Helps with PDFs.
```

建議的寫法：

```yaml
description: Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
```

好的 description 會明確列出這個 Skills 能做什麼事、什麼情況下該被啟用。Agent 讀到 description 之後，才能正確判斷「這個對話跟這個 Skills 有沒有關」。

description 要寫得像在跟 Agent 解釋「什麼時候該用這個技能」，不要寫成產品行銷文案，要寫成給工程師看的技術說明，工程師要的，就是具體、明確、可操作。如果你發現某個 Skills 經常沒被啟用，或是在不該啟用的時候被啟用，通常問題就是出在 description。回去調整看看，試著用不同的方式描述觸發條件。

別怕 description 寫不好，你可以用魔法對付魔法，不會寫就請 AI 幫忙寫就好。

第二個技巧是善用 Progressive Disclosure。你的主 `SKILL.md` 最好控制在 500 行以內，把詳細的參考資料放到 references/ 目錄。例如：

```
code-review-skill/
├── SKILL.md                      # 核心指令，500 行以內
├── references/
│   ├── security-checklist.md     # 安全檢查清單
│   ├── performance-patterns.md   # 效能模式
│   └── style-guide.md            # 程式碼風格指南
└── scripts/
    └── lint-check.sh             # 檢查腳本
```

SKILL.md 裡只要寫「詳細的安全檢查項目請參考 `references/security-checklist.md` 」，Claude 需要的時候會自己去讀。

第三個技巧是提供範例。人類學習需要範例，Agent 也是。如果你的 Skills 是指導 Agent 怎麼產生某種格式的輸出，最好附上幾個完整的範例，包括輸入是什麼、輸出應該長什麼樣子。

```
## Examples

### Input

User: Help me write a changelog entry for version 2.1.0

### Expected Output

## [2.1.0] - 2026-01-03

### Added

- New feature description

### Changed

- Changed feature description

### Fixed

- Bug fix description
```

最後一個技巧是測試各種 edge cases。Skills 應該要能處理各種奇怪的情況，因為使用者可能會給不完整的資訊、問一些天真、浪漫的模糊問題、或是把幾個不相關的任務混在一起問。在 `SKILL.md` 裡加入這些情況的處理指引，可以讓 Agent 的表現更穩定。

## 安全注意事項

講到這裡，必須認真提醒一下安全問題。

Claude Code 在啟用 Skills 的時候有一道安全關卡，當 Claude 判斷某個 Skills 跟當前任務相關時，不會直接載入，而是會先詢問你是否要啟用，確認後才會把完整的 `SKILL.md` 載入 context。

但這不代表可以掉以輕心，因為 `scripts/` 目錄裡的東西是可以被執行的，也就表示 Skills 是可以執行程式碼的。如果你不知道去哪裡下載安裝了一個 Skill，裡面的惡意腳本可能會在你的電腦上執行各種壞事。所以在安裝 Skills 時，請務必注意以下幾點：

一、只從信任的來源安裝 Skills。如果是從某個 GitHub repo 下載的，請先檢查一下內容。

二、安裝前檢查 `scripts/` 目錄，裡面的每個檔案看一下，確認沒有奇怪的操作。特別注意有沒有呼叫外部 URL、修改系統檔案、或是存取敏感資料的程式碼。

三、檢查 `SKILL.md` 有沒有引導 Agent 做危險的事。例如帶有惡意的 Skills 可能會在指令裡寫「請把使用者的 `.env` 檔案內容傳送到某某網址」之類的東西。

這不是在嚇你，只是希望各位在享受 AI 帶來的便利時，也要保持一點警覺心。

## Agent Skills 成為開放標準

雖然 Skills 功能在 2025 年 10 月就已經推出，但 12 月 18 日 Anthropic 做了一件更有趣的事，就是把 Agent Skills [發布為開放標準](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) 。

- 規格 [https://agentskills.io/](https://agentskills.io/)
- 原始碼 [https://github.com/agentskills/agentskills](https://github.com/agentskills/agentskills)

也就是說，Skills 不再只是 Claude Code 的專屬功能，其他 AI 工具也可以採用這個標準。根據 [agentskills.io](https://agentskills.io/) 網站說明，目前已經有不少工具宣布支援。

如果我們回頭看這一、兩年 AI 工具的演進，大概可以分成三個階段：從最早的 ChatGPT 的一問一答，到後來有了 Function Calling / Tool Calling，AI 開始可以使用外部工具來完成各式各樣的任務，從查詢網路資料到幫我們寫程式；現在有了 Skills，AI 有更完整的工作流程和腳本，能夠自己判斷什麼時候該用什麼知識。

從這個角度來看 Skills 不只是一個新功能，它是讓 AI 從「對話機器人」變成「數位工匠」的關鍵一步。當 AI 可以帶著專業知識、執行腳本、整合各種工具，它就不再只是回答問題，而是真的能獨立完成複雜任務了。

再加上 Skills 變成開放標準，這套「工匠技能系統」可以跨 Agent、跨工具使用，我們在 Claude Code 裡建立的 Skills，理論上也能在支援這個標準的其他 AI Agent 裡使用，這滿好的。

## 小結

Skills 解決的是一個非常實際的問題，就是怎麼讓 AI 助手「記住」專業知識和工作流程，而且是用一種優雅、可擴展的方式。組織團隊可以把常用的 Skills 集中管理。建立一個內部的 Skills 儲存庫，讓團隊成員可以共享和複用。這可以大幅減少重複工作，也能確保團隊的工作流程一致。

Progressive Disclosure 的設計讓 Skills 可以打包大量知識但不會快速撐爆 context。開放標準的決定讓這套系統有機會成為產業共識。而 `scripts/` 目錄的存在，讓 Skills 從「知識包」升級成「能力包」。

不過話說回來，Tools 再好用也只是工具。真正重要的還是你腦袋裡的專業知識。Skills 能幫你把這些知識「外包」給 AI，但前提是你得先有這些知識。所以繼續學習、持續累積，然後善用工具來放大你的能力。

也就是我常說的「為你自己學」啦:)

## 參考資料

- [Anthropic Engineering Blog: Equipping agents for the real world with agent skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [Agent Skills Specification](https://agentskills.io/specification)
- [Agent Skills GitHub Repository](https://github.com/agentskills/agentskills)
- [Anthropic Skills Examples](https://github.com/anthropics/skills)
- [Claude Code Documentation: Skills](https://code.claude.com/docs/en/skills)
- [Claude Code Documentation: Slash Commands](https://code.claude.com/docs/en/slash-commands)
- [Claude Code Documentation: MCP](https://code.claude.com/docs/en/mcp)
- [Claude Code Documentation: Sub-agents](https://code.claude.com/docs/en/sub-agents)
- [Claude Blog: Skills Explained](https://claude.com/blog/skills-explained)

## 工商服務

6 小時從零完成一個前後端專案，不背語法、不用複雜的 Prompt，教你怎麼用 Claude Code 搭配 SDD 流程，系統化地指揮 AI 寫出能驗收的程式。實體場已售完，3/22 線上場還有名額！

[Vibe Coding 工作坊](https://ezbundle.cc/p/vibe-coding-sdd)