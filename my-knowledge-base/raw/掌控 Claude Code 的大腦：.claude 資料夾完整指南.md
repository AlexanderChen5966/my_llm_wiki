---
title: "掌控 Claude Code 的大腦：.claude 資料夾完整指南"
source: "https://realnewbie.com/posts/master-claude-code-brain-complete-guide-to-claude-folder"
author:
  - "[[徐培鈞]]"
published: 2026-05-05
created: 2026-08-19
description: "用過 Claude Code 的人應該都看過 .claude 這個資料夾。很多人以為這只是暫存檔，不去理它，然後繼續跟 AI 對話。但它其實是 Claude Code 的「設定檔根目錄」——你希望 Claude 遵守哪些規範、哪些操作可以做哪些不行、遇到特定任務該怎麼處理，全部都在這裡設定。如果不去設定它，Claude 還是能用，但就只是通用的 AI 助手。設定好了，它就會變成熟悉你專案的團隊成員。這篇文章會帶你從頭認識它的完整結構，讓你能真正掌控 AI 的行為。"
tags:
  - "clippings"
status: "processed"
---
用過 Claude Code 的人應該都看過 [`.claude` 這個資料夾](https://code.claude.com/docs/en/claude-directory) 。

很多人以為這只是暫存檔，不去理它，然後繼續跟 AI 對話。

但它其實是 Claude Code 的「設定檔根目錄」——你希望 Claude 遵守哪些規範、哪些操作可以做哪些不行、遇到特定任務該怎麼處理，全部都在這裡設定。

如果不去設定它，Claude 還是能用，但就只是通用的 AI 助手。

設定好了，它就會變成熟悉你專案的團隊成員。

這篇文章會帶你從頭認識它的完整結構，讓你能真正掌控 AI 的行為。

## .claude 資料夾在哪裡？裡面有什麼？

它其實存在於兩個地方，各自負責不同的事。

一個放在專案裡跟團隊共享，另一個放在你的家目錄只給你自己用。

### 專案級（\<project>/.claude/）

專案級是給整個團隊用的。

當你在專案中第一次啟動 Claude Code，它會自動在專案根目錄產生這個資料夾。

裡面放的是這個專案專屬的規則、指令和工作流。

因為是團隊共用，所以建議提交到 Git 版本控制，讓每個人 clone 下來就能拿到一致的設定。

![](https://wp.realnewbie.com/wp-content/uploads/2026/05/claude_project_folder_tree.svg)

### 全域級（~/.claude/）

全域級是給你個人用的。

裡面放的是你的個人偏好、跨專案的全域指令，以及 Claude 在各個專案累積的會話歷史和記憶。

這些設定不需要也不應該跟別人共享。

![](https://wp.realnewbie.com/wp-content/uploads/2026/05/claude_global_folder_tree.svg)

### 衝突時誰贏？

當兩邊的設定有衝突時，專案級會覆蓋全域級。

舉個例子。

你在全域的 `~/.claude/CLAUDE.md` 寫了「commit message 一律用英文」。

但某個專案的 `.claude/CLAUDE.md` 寫了「commit message 用中文」。

這時候 Claude 會聽誰的？

答案是聽專案級的——commit message 會用中文。

權限也是同樣的邏輯。

假設你在全域 `settings.json` 允許了 `Bash(curl *)` 。

但專案的 `settings.json` 把它放進了 deny 清單。

結果也一樣——專案的 deny 會蓋過全域的 allow，Claude 無法執行 `curl` 。

這個設計很合理——團隊的規範應該優先於個人的偏好。

不是每個專案都需要上面列出的所有檔案。

但先知道完整的結構，接下來逐一介紹時你會更有方向感。

## CLAUDE.md：Claude 的系統提示詞

`CLAUDE.md` 是整個 `.claude` 資料夾裡最重要的檔案。

每次你啟動 Claude Code 的會話，它做的第一件事就是讀取 `CLAUDE.md` ，把裡面的內容載入系統提示詞。

你寫在裡面的所有指令，Claude 都會在整個會話期間遵守。

比方說，你寫「所有 commit message 都要用 `feat:`、 `fix:` 或 `chore:` 開頭」，Claude 就會每次都照做。

你寫「永遠不要用 `console.log` ，改用專案的 logger 模組」，它也會記住。

### 放什麼進去？

好的 `CLAUDE.md` 通常包含這幾類資訊：

- 建置與測試指令（ `npm run test` 、 `make build` 等）
- 專案架構簡述（用什麼框架、目錄結構怎麼安排）
- 編碼規範（命名慣例、錯誤處理方式、禁止的做法）
- 需要特別注意的地雷（像是「測試用的是真實本地資料庫，不是 mock」）

### 一個實際的範例

```
# Project: Acme API

## Commands
npm run dev     # 啟動開發伺服器
npm run test    # 跑測試（Jest）
npm run lint    # ESLint + Prettier 檢查

## Architecture
- Express REST API, Node 20
- PostgreSQL via Prisma ORM
- 所有 handler 放在 src/handlers/
- 共用型別放在 src/types/

## Conventions
- 每個 handler 都用 zod 做 request validation
- 回傳格式統一為 { data, error }
- 永遠不要在回應中暴露 stack trace
- 使用 logger 模組，不要用 console.log
```
```
# Project: Acme API

## Commands
npm run dev     # 啟動開發伺服器
npm run test    # 跑測試（Jest）
npm run lint    # ESLint + Prettier 檢查

## Architecture
- Express REST API, Node 20
- PostgreSQL via Prisma ORM
- 所有 handler 放在 src/handlers/
- 共用型別放在 src/types/

## Conventions
- 每個 handler 都用 zod 做 request validation
- 回傳格式統一為 { data, error }
- 永遠不要在回應中暴露 stack trace
- 使用 logger 模組，不要用 console.log
```

大約 20 行，就能讓 Claude 在這個專案中有效率地工作，不需要你每次都重複交代。

### 重要原則：控制在 200 行以內

`CLAUDE.md` 的內容會直接佔用上下文視窗（context window）。

寫太多，不但浪費 token，Claude 對指令的遵守率也會下降。

經驗法則是控制在 200 行以內。

如果你有個人偏好想加進去但不想影響團隊，可以另外建一個 `CLAUDE.local.md` ，這個檔案只在你本機生效，不需要提交到版本控制。

![](https://wp.realnewbie.com/wp-content/uploads/2026/05/claude_md_vs_local_md.svg)

### 不只放在.claude/ 裡：根目錄也能用

前面提到 `.claude` 資料夾有兩個位置——專案級和全域級。

這兩個位置裡的 `CLAUDE.md` ，Claude 每次會話都會讀取並合併。

但其實 `CLAUDE.md` 還可以放在 `.claude/` 資料夾以外的地方。

Claude Code 在啟動時，會自動搜尋多個位置的 `CLAUDE.md` 並合併載入。

所以你可以直接把它放在專案根目錄（ `<project>/CLAUDE.md` ），Claude 一樣會讀到。

有些團隊喜歡這樣做，因為檔案放在根目錄更醒目，新成員比較不會漏掉。

不過要注意：這個彈性只有 `CLAUDE.md` 才有。

`settings.json` 、 `rules/` 、 `skills/` 、 `agents/` 這些設定必須放在 `.claude/` 資料夾裡面才會生效。

所以 `.claude/` 資料夾仍然是不可取代的——它是所有設定的集中管理處， `CLAUDE.md` 只是多了一個「也能放在根目錄」的便利選項。

### 子目錄的 CLAUDE.md

除了根目錄和 `.claude/` 以外，你也可以在專案的子目錄裡放 `CLAUDE.md` 。

舉個例子，假設你的專案有前端和後端兩個目錄：

```
my-project/
├── CLAUDE.md                  # 整個專案的通用指令
├── frontend/
│   ├── CLAUDE.md              # 前端專屬的指令
│   └── src/
└── backend/
    ├── CLAUDE.md              # 後端專屬的指令
    └── src/
```
```
my-project/
├── CLAUDE.md                  # 整個專案的通用指令
├── frontend/
│   ├── CLAUDE.md              # 前端專屬的指令
│   └── src/
└── backend/
    ├── CLAUDE.md              # 後端專屬的指令
    └── src/
```

`frontend/CLAUDE.md` 裡可能寫著「元件一律用 functional component」。

`backend/CLAUDE.md` 裡可能寫著「所有 API 回傳格式統一用 `{ data, error }` 」。

當 Claude 在編輯 `frontend/src/` 底下的檔案時，它會載入 `frontend/CLAUDE.md` 的指令。

但它不會同時載入 `backend/CLAUDE.md` ——因為那不是它目前在處理的目錄。

這樣做的好處是：不同模組可以有自己的規範，而且互不干擾，也不會浪費上下文空間。

### 用 @ 語法拆分內容

當你的 `CLAUDE.md` 越寫越長，可以用 `@` 語法把部分內容拆到其他檔案。

`@` 的作用就像「把那個檔案的內容貼到這裡」。

Claude 在讀取 `CLAUDE.md` 時，碰到 `@` 開頭的一行，就會去讀取後面指定的檔案，把內容合併進來。

`@` 後面接的是檔案路徑，寫法跟終端機裡的路徑一樣：

- `@./code-standards.md` ——`./` 代表「跟這個 `CLAUDE.md` 同一個目錄」，所以它會去找同目錄下的 `code-standards.md`
- `@./docs/api-spec.md` ——找同目錄底下 `docs/` 資料夾裡的 `api-spec.md`
- `@~/.claude/my-rules.md` —— `~/` 代表你的家目錄，所以它會去找全域 `.claude` 資料夾裡的 `my-rules.md`

來看一個完整的例子。

假設你的檔案結構長這樣：

```
my-project/.claude/
├── CLAUDE.md
├── code-standards.md
└── security-requirements.md
```
```
my-project/.claude/
├── CLAUDE.md
├── code-standards.md
└── security-requirements.md
```

`CLAUDE.md` 裡面這樣寫：

```
# 專案指令

## 建置指令
npm run dev
npm run test

## 詳細規範
@./code-standards.md
@./security-requirements.md
```
```
# 專案指令

## 建置指令
npm run dev
npm run test

## 詳細規範
@./code-standards.md
@./security-requirements.md
```

Claude 讀取時，會把 `code-standards.md` 和 `security-requirements.md` 的內容合併進來。

最終效果跟你把三個檔案的內容全部寫在同一個 `CLAUDE.md` 裡一模一樣。

差別只在於維護起來更方便——改編碼規範的人不用碰安全需求的內容。

## settings.json：安全護欄與權限管理

如果 `CLAUDE.md` 管的是「Claude 應該知道什麼」，那 `settings.json` 管的就是「Claude 能做什麼」。

它控制的是 Claude 的行為權限——哪些指令可以執行、哪些操作必須封鎖。

這是整個 `.claude` 系統中最重要的安全機制。

### 權限的三種模式

`settings.json` 用三種清單來管理權限：

- **Allow（白名單）** ——列在裡面的操作直接放行，不需要每次確認
- **Deny（黑名單）** ——列在裡面的操作直接封鎖，Claude 完全無法執行
- **Ask（預設）** ——沒有被明確允許或拒絕的操作，每次都會先問你

一個典型的設定長這樣：

```
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)",
      "Bash(git push --force *)",
      "Read(./.env)",
      "Read(./.env.*)"
    ]
  }
}
```
```
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)",
      "Bash(git push --force *)",
      "Read(./.env)",
      "Read(./.env.*)"
    ]
  }
}
```

你會發現設定檔裡沒有 `ask` 這個欄位。

因為 Ask 是預設行為——任何沒有被列進 `allow` 或 `deny` 的操作，Claude 都會先問你才執行。

你只需要把「確定安全的」放進 allow，「絕對不能碰的」放進 deny，剩下的 Claude 會自己來問你。

### 防禦誤刪的安全實踐

跟 AI 合作寫程式時，最怕的就是它執行了不可逆的操作。

以下是兩個值得採用的防禦措施：

#### 把危險指令加進黑名單

`rm -rf` 、 `sudo` 、 `git push --force` 這類不可逆的指令，應該直接放進 `deny` 清單。

Claude 一旦被封鎖，就完全無法執行這些操作，連問都不會問。

#### 善用垃圾桶機制

`rm` 指令會永久刪除檔案，一旦刪了就沒辦法救回來。

但你可以讓 Claude 改用「移至垃圾桶」的方式來刪除，這樣檔案還是會消失在目錄裡，但實際上被移到了垃圾桶，隨時可以救回來。

做法分兩步。

第一步，安裝一個垃圾桶工具。

macOS 可以用 `trash` （透過 Homebrew 安裝： `brew install trash` ），Linux 可以用 `trash-cli` （透過 npm 安裝： `npm install -g trash-cli` ）。

這些工具的行為跟 `rm` 很像，差別在於檔案會被移到系統的垃圾桶，而不是直接消失。

第二步，在 `CLAUDE.md` 裡告訴 Claude 用這個工具取代 `rm` 。

```
## 刪除檔案的規範
永遠不要用 rm 刪除檔案。
改用 trash 指令，例如：trash my-file.txt
```
```
## 刪除檔案的規範
永遠不要用 rm 刪除檔案。
改用 trash 指令，例如：trash my-file.txt
```

這樣 Claude 在需要刪除檔案時，就會自動使用 `trash` 而不是 `rm` 。

如果你想更保險，可以在 `settings.json` 裡直接封鎖 `rm` ：

```
{
  "permissions": {
    "deny": ["Bash(rm *)"]
  }
}
```
```
{
  "permissions": {
    "deny": ["Bash(rm *)"]
  }
}
```

這樣即使 Claude 忘了用 `trash` ， `rm` 也會被擋下來。

### settings.json 的層級

跟 `CLAUDE.md` 一樣， `settings.json` 也有層級，而且層級之間有明確的優先順序。

從低到高依序是：

#### 使用者級（~/.claude/settings.json）

這是優先順序最低的層級，也是你個人的全域設定。

放在家目錄的 `.claude/` 裡，對你電腦上的所有專案生效。

適合放你個人的通用偏好，比如「所有專案都封鎖 `sudo` 」。

#### 專案級（.claude/settings.json）

這是優先順序較高的層級，也是團隊共用的專案設定。

放在專案的 `.claude/` 資料夾裡，只對這個專案生效。

適合放專案特有的權限規則，比如「這個專案允許執行 `docker compose` 」。

當兩者衝突時，專案級會覆蓋使用者級——跟前面 `CLAUDE.md` 的規則一樣。

另外，如果你想在某個專案裡加入個人的權限調整，但不想影響團隊的 `settings.json` ，可以另外建一個 `settings.local.json` 。

跟 `CLAUDE.local.md` 一樣，這個檔案只在你本機生效，應該加進 `.gitignore` ，不提交到版本控制。

補充一點：如果你是在公司的企業方案中使用 Claude Code，還會有一個更高層級的 `managed-settings.json` ，由 IT 部門統一部署，所有人都無法覆蓋。

但那屬於企業部署的範疇，這篇文章不展開介紹。

## rules/：模組化的規則系統

`rules/` 是 `.claude/` 資料夾裡的一個子目錄，用來存放獨立的規則檔案。

每個檔案是一個 Markdown 文件，裡面寫的是你希望 Claude 遵守的規範。

比方說，一個叫 `api-conventions.md` 的規則檔案可能長這樣：

```
# API 設計規範

- 所有 API endpoint 都用 RESTful 命名
- 回傳格式統一為 { data, error }
- 錯誤回應一律使用標準 HTTP status code
- 永遠不要在回應中暴露 stack trace
```
```
# API 設計規範

- 所有 API endpoint 都用 RESTful 命名
- 回傳格式統一為 { data, error }
- 錯誤回應一律使用標準 HTTP status code
- 永遠不要在回應中暴露 stack trace
```

Claude 會自動讀取 `rules/` 資料夾裡的所有檔案，不需要你手動引入。

你可能會想：這些規範直接寫在 `CLAUDE.md` 裡，或用 `@` 語法拆出去不就好了？

關鍵差別在於：不管是 `CLAUDE.md` 還是 `@` 引入的檔案，每次會話都會 **全部載入** 。

而 `rules/` 裡的規則可以設定成 **只在 Claude 編輯特定類型的檔案時才載入** 。

這是 `CLAUDE.md` 和 `@` 語法都做不到的。

### 路徑範圍限制（Path-scoped Rules）

假設你的專案同時有前端和後端的程式碼。

你可以這樣組織規則：

```
.claude/rules/
├── api-conventions.md       # 後端 API 規範
├── testing-standards.md     # 測試規範
├── database-patterns.md     # 資料庫規範
└── react-components.md      # 前端元件規範
```
```
.claude/rules/
├── api-conventions.md       # 後端 API 規範
├── testing-standards.md     # 測試規範
├── database-patterns.md     # 資料庫規範
└── react-components.md      # 前端元件規範
```

如果沒有做任何設定，Claude 會在每次會話中載入所有規則檔案，跟放在 `CLAUDE.md` 裡沒有差別。

但你可以在規則檔案的開頭加上 YAML frontmatter，指定它只在特定檔案類型被編輯時才載入：

```
paths:
  - "**/*.tsx"
  - "**/*.jsx"
# React Component Rules

- 所有元件都使用 functional component
- 禁止使用 class component
- 狀態管理一律用 hooks
```
```
paths:
  - "**/*.tsx"
  - "**/*.jsx"
# React Component Rules

- 所有元件都使用 functional component
- 禁止使用 class component
- 狀態管理一律用 hooks
```

加了 `paths` 之後，這條規則只有在 Claude 編輯 `.tsx` 或 `.jsx` 檔案時才會被讀取。

當 Claude 在處理後端的 `.py` 或 `.sql` 檔案時，這條前端規則完全不會出現，也不會佔用上下文空間。

這就是 `rules/` 最大的價值——你可以為不同的程式語言或模組各自定義專屬的規範，它們只在需要的時候才出現。

### 什麼時候該從 CLAUDE.md 搬到 rules/？

一個簡單的判斷標準：如果某段規則超過 15 到 20 行，而且只跟特定領域有關，就應該搬到 `rules/` 裡。

如果它還只適用於特定檔案類型，那就再加上路徑範圍限制。

## skills/：可重用的自動化工作流

到目前為止，我們介紹了兩種讓 Claude 遵守規範的方式：

`CLAUDE.md` 是每次會話都會載入的通用指令，適合放建置指令、架構說明這類每次都用得到的資訊。

`rules/` 是根據你正在編輯的檔案類型條件式載入的規範，適合放特定領域的編碼原則。

這兩者都是在告訴 Claude「你應該遵守什麼」。

`skills/` 不一樣——它告訴 Claude 的是「碰到某個任務時，你應該按照什麼步驟去執行」。

舉個例子：「部署到正式環境」不是一條規範，而是一連串的操作步驟（跑測試 → 建置 → 檢查環境變數 → 部署 → 驗證）。這種東西就適合做成 skill。

載入方式也不同。

`CLAUDE.md` 每次都載入， `rules/` 根據檔案類型自動載入。

`skills/` 則是在你主動觸發（輸入斜線指令）或 Claude 判斷對話內容符合時才載入。

平時完全不佔上下文空間，所以你可以寫很長、很詳細的操作指南，不用擔心拖慢日常的對話品質。

### Skill 的基本結構

每個 skill 是 `.claude/skills/` 底下的一個資料夾。

資料夾裡面必須有一個 `SKILL.md` 檔案，這是 skill 的核心——Claude 會讀取它來知道該怎麼執行這個任務。

以一個「部署到正式環境」的 skill 為例， `SKILL.md` 長這樣：

```
name: deploy
description: 部署到正式環境的標準流程。當使用者提到「部署」或「deploy」時使用。
## 部署步驟

1. 先跑 \`npm run test\` 確認所有測試通過
2. 跑 \`npm run build\` 產生正式版本
3. 檢查環境變數是否正確設定
4. 執行 \`npm run deploy:production\`
5. 驗證部署結果
```
```
name: deploy
description: 部署到正式環境的標準流程。當使用者提到「部署」或「deploy」時使用。
## 部署步驟

1. 先跑 \`npm run test\` 確認所有測試通過
2. 跑 \`npm run build\` 產生正式版本
3. 檢查環境變數是否正確設定
4. 執行 \`npm run deploy:production\`
5. 驗證部署結果
```

上半部用 `---` 包起來的區塊叫做 YAML frontmatter。

`name` 是這個 skill 的名稱，也是斜線指令的名字——設定成 `deploy` 之後，你在對話中輸入 `/deploy` 就能觸發它。

`description` 是給 Claude 看的描述——Claude 會根據這段文字判斷什麼時候該自動載入這個 skill。

下半部就是你希望 Claude 執行的具體步驟，用一般的 Markdown 來寫。

### 手動觸發 vs. 自動觸發

Skill 有兩種觸發方式：

**手動觸發** ——你在對話中輸入 `/deploy` ，Claude 就會載入這個 skill 的指令來執行。

**自動觸發** ——Claude 會根據 `description` 欄位的描述，自己判斷對話內容是否符合，然後主動載入。

如果你不想讓 Claude 自動觸發某個 skill，可以在 frontmatter 加上 `disable-model-invocation: true` ，這樣就只能手動呼叫。

### Skill vs. CLAUDE.md 的使用時機

判斷規則很簡單：

如果一條指令幾乎每次對話都用得到（像是編碼規範、建置指令），放在 `CLAUDE.md` 。

如果它只在特定任務時才需要（像是部署流程、PR 審查清單），做成 skill。

放太多東西在 `CLAUDE.md` 裡等於每次對話都在浪費上下文。

Skill 讓你保持日常對話的精簡，同時又能在需要時呼叫出複雜的工作流。

### 關於 commands/

如果你在其他文章或舊專案裡看到 `.claude/commands/` 資料夾，那是 skill 的前身。

它的功能比較單純——把常用的提示詞模板化，讓你用斜線指令快速呼叫。

目前 `commands/` 和 `skills/` 已經合併，兩者建立的斜線指令效果一樣。

但 `skills/` 多了資料夾結構、frontmatter 設定、自動觸發等功能，所以新的工作流建議直接用 `skills/` 來建。

舊的 `commands/` 檔案仍然可以正常運作，不需要急著遷移。

### 進階：在 Skill 資料夾裡放輔助檔案

前面說過，每個 skill 的基本結構就是「一個資料夾 + 一個 `SKILL.md` 」。

但如果你的工作流比較複雜，skill 資料夾裡也可以放其他輔助檔案。

比方說，部署流程可能需要搭配一份檢查清單模板：

```
.claude/skills/
└── deploy/
    ├── SKILL.md                   # skill 的核心指令
    └── templates/
        └── deploy-checklist.md    # 部署時使用的檢查清單模板
```
```
.claude/skills/
└── deploy/
    ├── SKILL.md                   # skill 的核心指令
    └── templates/
        └── deploy-checklist.md    # 部署時使用的檢查清單模板
```

你可以在 `SKILL.md` 的步驟裡告訴 Claude 去讀取 `templates/deploy-checklist.md` ，它就會把那份檔案的內容拿來用。

比方說， `SKILL.md` 裡可以這樣寫：

```
name: deploy
description: 部署到正式環境的標準流程。
## 部署步驟

1. 先跑 \`npm run test\` 確認所有測試通過
2. 跑 \`npm run build\` 產生正式版本
3. 讀取 \`templates/deploy-checklist.md\`，逐項檢查所有項目是否通過
4. 執行 \`npm run deploy:production\`
5. 驗證部署結果
```
```
name: deploy
description: 部署到正式環境的標準流程。
## 部署步驟

1. 先跑 \`npm run test\` 確認所有測試通過
2. 跑 \`npm run build\` 產生正式版本
3. 讀取 \`templates/deploy-checklist.md\`，逐項檢查所有項目是否通過
4. 執行 \`npm run deploy:production\`
5. 驗證部署結果
```

而 `templates/deploy-checklist.md` 裡面放的是檢查清單的內容：

```
# 部署前檢查清單

- [ ] 所有測試通過
- [ ] 環境變數已設定（DATABASE_URL、API_KEY）
- [ ] 版本號已更新
- [ ] CHANGELOG 已填寫
- [ ] 已通知團隊即將部署
```
```
# 部署前檢查清單

- [ ] 所有測試通過
- [ ] 環境變數已設定（DATABASE_URL、API_KEY）
- [ ] 版本號已更新
- [ ] CHANGELOG 已填寫
- [ ] 已通知團隊即將部署
```

這樣 Claude 在執行到第三步時，就會去讀取那份檢查清單，逐項確認。

這樣做的好處是把「流程邏輯」和「模板內容」分開管理。

改模板的人不需要動 `SKILL.md` ，改流程的人也不需要碰模板。

剛開始使用時不需要這麼做——一個 `SKILL.md` 就夠了。

等到 skill 的內容越來越長，再考慮把模板拆出來。

## agents/：專家子代理

`agents/` 讓你定義獨立的 AI 人格，每個 agent 在自己的上下文視窗中運行，不會污染你主要的對話。

這在需要處理複雜、多步驟的任務時特別有用。

### Agent 的核心概念

每個 agent 是一個 Markdown 檔案，用 YAML frontmatter 定義它的行為：

```
name: code-reviewer
description: 專門負責程式碼審查的子代理
model: claude-sonnet-4-6
allowed-tools:
  - Read
  - Grep
  - Glob
你是一位嚴格的程式碼審查者。
檢查重點：安全漏洞、效能問題、錯誤處理、命名規範。
只提出具體可行動的建議，不要泛泛而談。
```
```
name: code-reviewer
description: 專門負責程式碼審查的子代理
model: claude-sonnet-4-6
allowed-tools:
  - Read
  - Grep
  - Glob
你是一位嚴格的程式碼審查者。
檢查重點：安全漏洞、效能問題、錯誤處理、命名規範。
只提出具體可行動的建議，不要泛泛而談。
```

### 為什麼需要 Agent？

假設你請 Claude 幫你研究一個大型專案的三個模組：使用者認證、訂單處理、通知系統。

如果全部在主對話裡做，Claude 需要依序讀取每個模組的原始碼。

讀完認證模組的十幾個檔案後，上下文已經被塞了一大半。

等到要研究訂單模組時，Claude 可能已經因為上下文不夠，開始遺忘前面認證模組的細節。

三個模組全部看完，上下文早就爆了。

用 agent 的做法完全不同。

你可以同時派出三個子代理，每個負責一個模組。

每個 agent 都有自己獨立的上下文視窗，不會互相干擾。

認證模組的 agent 只讀認證相關的程式碼，訂單模組的 agent 只讀訂單相關的程式碼。

三個 agent 各自完成分析後，把結論回傳給你的主對話。

你的主對話從頭到尾只收到三份摘要，不需要塞進任何原始碼，始終保持乾淨。

### Agent 的實用設定

你可以為 agent 指定幾個重要的參數：

- **`model`** ——使用比較便宜的模型（像是 Haiku）來處理簡單的探索任務，節省成本
- **`allowed-tools`** ——限制 agent 能用的工具，比如只給它「讀取」權限，不讓它修改任何檔案
- **`skills`** ——預載特定的 skill，讓 agent 帶著專業知識啟動

在對話中，你可以用 `@code-reviewer` 來指定呼叫某個 agent，或者讓 Claude 根據任務自動選擇合適的 agent。

## 記憶機制：Claude 怎麼「記住」你的專案

前面介紹的 `CLAUDE.md` 、 `rules/` 、 `skills/` 都有一個共同點：它們的內容是 **你自己手動寫的** 。

你知道專案用 pnpm，所以你寫在 `CLAUDE.md` 裡。你知道前端要用 functional component，所以你寫在 `rules/` 裡。

但有些知識不是你事先知道的——它是 Claude 在工作過程中自己發現的。

比方說，Claude 在幫你除錯時發現測試資料庫跑在 port 5433 而不是預設的 5432，或者發現某個舊模組有一個沒寫在文件裡的特殊初始化順序。

這些知識如果不記下來，下次開新的會話，Claude 又會從零開始摸索，可能踩到同樣的坑。

記憶機制就是讓 Claude 自己把這些發現寫下來。

它會把工作過程中學到的重要資訊存進一個記憶檔案。

下次你開新的會話時，Claude 會先讀取這個檔案，這樣它就不會重複踩坑。

跟 `CLAUDE.md` 的差別在於： `CLAUDE.md` 是你寫給 Claude 的，記憶檔案是 Claude 寫給自己的。

### 記憶存在哪裡？

記憶檔案不是存在專案的 `.claude/` 資料夾裡，而是存在你家目錄的 `~/.claude/projects/` 底下。

每個你用 Claude Code 工作過的專案，都會在這裡有一個對應的子目錄。

目錄名稱是一串 hash 值（像是 `a3f8b2c1` ），代表專案的路徑。

裡面主要有兩種東西：

**會話歷史（`.jsonl` 檔案）** ——每次對話的完整記錄，用來讓你回顧過去的操作。

**記憶檔案（ `MEMORY.md` ）** ——Claude 自動學到的專案知識。

![](https://wp.realnewbie.com/wp-content/uploads/2026/05/claude_memory_folder_tree.svg)

`MEMORY.md` 的內容可能長這樣：

```
# 專案記憶

- 測試資料庫跑在 port 5433，不是預設的 5432
- \`PaymentService\` 初始化時必須先呼叫 \`loadConfig()\`，否則會 silent fail
- \`src/legacy/\` 底下的模組不支援 ES module import，要用 require()
- 跑 e2e 測試前需要先啟動 \`docker compose up -d\`
```
```
# 專案記憶

- 測試資料庫跑在 port 5433，不是預設的 5432
- \`PaymentService\` 初始化時必須先呼叫 \`loadConfig()\`，否則會 silent fail
- \`src/legacy/\` 底下的模組不支援 ES module import，要用 require()
- 跑 e2e 測試前需要先啟動 \`docker compose up -d\`
```

這些都是 Claude 在之前的會話中，透過閱讀程式碼或除錯過程自己發現的。

你也可以用 `/memory` 指令來手動查看和編輯這些記憶內容。

### 清理與維護

長時間使用後， `~/.claude/` 目錄可能會因為會話日誌（`.jsonl` 檔案）而膨脹到好幾 GB。

定期清理舊的會話快取是合理的做法。

`statsig/` 資料夾（分析快取）也可以安全刪除，Claude Code 會自動重建。

但有一個絕對不能碰的東西： **`memory/` 相關的檔案** 。

如果你把記憶檔案刪掉，Claude 會遺失它在這個專案學到的所有知識。

更極端的情況——如果你把整個 `~/.claude/` 資料夾刪除，Claude 會在所有專案中失去記憶和設定，等於從零開始。

## 從零開始的導入建議

不需要一次把所有東西都設定好。

建議按照這個順序循序漸進：

**第一階段：最小可用設定。**

先建立 `CLAUDE.md` ，寫上你的建置指令和最重要的編碼規範。

再加一個 `settings.json` ，把明確的危險指令放進 deny 清單。

光是這兩個檔案，就能讓 Claude Code 的表現有明顯提升。

**第二階段：拆分規則。**

當 `CLAUDE.md` 開始超過 100 行，把特定領域的規則搬到 `rules/` 資料夾。

如果某些規則只適用於特定檔案類型，加上路徑範圍限制。

**第三階段：建立工作流。**

找出你最常重複下達的指令（像是「審查 PR」「跑部署」「寫 commit message」），把它們做成 skill。

**第四階段：進階設定。**

如果你需要平行處理或上下文隔離，開始使用 agent。

如果你需要跟外部工具整合（像是 Jira、GitHub），設定 MCP Server。

### 版本控制提醒

最後一個容易忘記的事：把 `settings.local.json` 和 `CLAUDE.local.md` 加進 `.gitignore` 。

這些檔案可能包含個人路徑、本機環境設定，甚至敏感資訊。

它們只該存在於你自己的電腦上，不該出現在版本控制裡。

```
# .gitignore
.claude/settings.local.json
.claude/CLAUDE.local.md
```
```
# .gitignore
.claude/settings.local.json
.claude/CLAUDE.local.md
```

## 重點整理

`.claude` 資料夾控制了 Claude Code 的五個核心子系統：

- **指令** （ `CLAUDE.md` 和 `rules/` ）——告訴 Claude「應該知道什麼」
- **權限** （ `settings.json` ）——限制 Claude「能做什麼」
- **工作流** （ `skills/` ）——定義 Claude「按需執行的任務」
- **專家代理** （ `agents/` ）——在隔離的環境中處理專門任務
- **記憶** （ `~/.claude/projects/` ）——讓 Claude「記得什麼」

理解這些部分怎麼搭配運作，設定 Claude Code 就不再是猜測，而是有系統的工程實踐。

從簡單的 `CLAUDE.md` 開始，隨著需求增長再逐步擴充。

不用一步到位，但值得儘早開始。