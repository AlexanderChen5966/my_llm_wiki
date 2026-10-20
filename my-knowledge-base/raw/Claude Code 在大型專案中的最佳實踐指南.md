---
title: "Claude Code 在大型專案中的最佳實踐指南"
source: "https://codelove.tw/@tony/post/aAOnPx"
author:
published:
created: 2026-05-25
description: "Claude Code 在大型專案中的最佳實踐指南  ============================    Claude Code 在大型專案中的最佳實踐：從配置到落地的完整指南-----------------------------------> 本文基於 Anthropi..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月23日](https://codelove.tw/@tony/post/aAOnPx "2026-05-23 18:03:03")

```
Claude Code 在大型專案中的最佳實踐：從配置到落地的完整指南
```

---

> 本文基於 Anthropic 官方部落格 [How Claude Code works in large codebases](https://link.juejin.cn/?target=https%3A%2F%2Fclaude.com%2Fblog%2Fhow-claude-code-works-in-large-codebases-best-practices-and-where-to-start) 整理，結合實際工程場景深度解讀，並補充大量可直接複用的配置案例。

---

### 先說結論

很多團隊導入 Claude Code 後，效果參差不齊。有的團隊在百萬行 monorepo 裡用得飛起，有的團隊在幾千行的小專案裡反而覺得「不如直接寫」。

差距不在模型，在 **配置** 。

Anthropic 的 Applied AI 團隊觀察了大量企業級部署後，總結出一套可複用的模式。這篇文章把這套模式拆開講清楚，每一層都附上可以直接抄走的配置片段，讓你少走彎路。

---

### Claude Code 怎麼理解大型程式碼庫

先搞清楚一個基礎問題：Claude Code 是怎麼在程式碼庫裡找東西的？

**它不用 RAG。**

大多數 AI 編碼工具的路子是：把整個程式碼庫向量化，查詢時檢索相關片段。這套方案在小專案上沒問題，但在活躍的大型團隊裡會出問題——embedding 管道跟不上提交速度。你查的時候，索引可能還停留在兩週前的狀態，回傳的是已經被重新命名的函式、已經刪掉的模組，而且沒有任何提示說這些資訊已經過期。

Claude Code 的做法更接近一個真實的工程師： **遍歷檔案系統、讀檔案、用 grep 精確定位、跨檔案追蹤引用** 。它直接在開發者本機上執行，操作的是即時程式碼庫，沒有需要維護的中央索引。

你可以在一次對話裡看到它真實的「思考路徑」——大致是這樣：

```
text 體驗AI代碼助手 代碼解讀複製代碼> 幫我找一下訂單逾時退款的邏輯在哪

● 我先看一下專案結構
  └─ Bash(ls -la services/)

● 看起來訂單相關在 services/order/，先 grep 一下關鍵字
  └─ Grep("refund|timeout", path: "services/order", output_mode: "files_with_matches")

● 命中 3 個檔案，重點看 refund_handler.go
  └─ Read("services/order/internal/refund/refund_handler.go", offset: 1, limit: 80)

● 這裡呼叫了 TimeoutPolicy.shouldRefund，跳過去看定義
  └─ Grep("func.*shouldRefund", path: "services/order")
```

這個方案的代價是： **它需要足夠的起始上下文才能知道去哪里找** 。如果你讓它在十億行程式碼庫裡搜尋一個模糊的模式，還沒開始幹活就會撞上上下文視窗限制。

所以，程式碼庫的可導航性直接決定了 Claude Code 的上限。

---

### 核心認知：模型只是一半，配置才是另一半

這是很多團隊忽視的點： **Claude Code 的實際表現，由圍繞模型建構的「配套系統」決定，而不只是模型本身。**

這套配套系統由五個擴充點組成，加上兩個額外能力：

組件是什麼何時載入最適合做什麼CLAUDE.mdClaude 自動讀取的上下文檔案每次對話專案約定、程式碼庫知識Hooks在關鍵時刻觸發的腳本事件觸發自動化一致性行為、捕捉會話學習Skills特定任務類型的打包指令按需載入跨會話和專案的可重用專業知識Plugins打包好的 skills + hooks + MCP 配置安裝後始終可用在組織內分發一套完整配置LSP 集成透過語言伺服器提供即時程式碼智能配置後始終可用符號級導航、自動錯誤檢測MCP 伺服器連接外部工具和資料來源配置後始終可用讓 Claude 存取內部工具Subagents獨立的 Claude 實例按需呼叫探索與編輯分離、並行工作這五個擴充點有 **建構順序** ，每一層都建立在前一層之上。下面逐層拆。

---

### 第一層：CLAUDE.md——讓 Claude 理解你的專案

[CLAUDE.md](http://claude.md/) 是 Claude 在每次對話開始時自動讀取的上下文檔案。根目錄放全域資訊，子目錄放局部約定。

**關鍵原則：精簡、分層。**

根目錄的 [CLAUDE.md](http://claude.md/) 只放指標和關鍵注意事項，其他的放到子目錄裡。內容越多，雜訊越大，效能越差。

#### 一個真實的根目錄 CLAUDE.md

```
markdown 体验AI代码助手 代码解读复制代码# 專案概覽

這是一個電商平台的 monorepo，約 80 萬行程式碼，模組如下：

- /services/payment   - 支付服務（Java 17 + Spring Boot 3.x）
- /services/inventory - 庫存服務（Go 1.22）
- /services/order     - 訂單服務（Go 1.22）
- /frontend           - React 18 + Vite + TypeScript
- /infra              - Terraform + Helm
- /docs               - 架構與 API 文件

詳細模組約定見各目錄下的 CLAUDE.md。

# 全域硬性約定

- 所有對外 API 的變更必須同步更新 \`/docs/api-changelog.md\`，未更新視為不完整
- 資料庫遷移檔案放在 \`/migrations\`，命名格式：\`YYYYMMDD_HHmm_描述.sql\`
- 禁止直接修改 \`/generated\` 目錄下的檔案（由 buf/protoc 生成）
- 提交前必須本地通過 \`make precommit\`
- 任何新增依賴需要在 PR 描述裡寫明引入理由

# 常用命令

| 場景 | 命令 | 說明 |
|------|------|------|
| 日常單元測試 | \`make test-unit\` | 約 1 分鐘 |
| 完整測試 | \`make test\` | 約 20 分鐘，CI 才跑 |
| 啟動本地環境 | \`make dev\` | 依賴 Docker Desktop |
| 型別 + lint 檢查 | \`make check\` | 提交前必跑 |

# 不要做的事

- 不要在 \`/services/payment\` 裡寫新功能時跑全量測試，單跑該模組即可
- 不要在 PR 裡同時混入格式化變更與邏輯變更
- 不要把環境變數寫進程式碼，統一在 \`infra/env/\` 配置
```

#### 子目錄的 CLAUDE.md 只補充該目錄特有的資訊

```
markdown 体验AI代码助手 代码解读复制代码# /services/payment

## 技術棧
- Java 17，Spring Boot 3.2.x，Gradle 8
- 資料庫：PostgreSQL 15（主）+ Redis 7（限流/冪等）

## 測試與建置
- 本模組測試：\`./gradlew :payment:test\`
- 整合測試需要 docker-compose：\`make payment-it\`
- 建置產物：\`./gradlew :payment:bootJar\`

## 關鍵檔案
- \`PaymentProcessor.java\` —— 入口，**執行緒安全要求**，修改前必讀類頭註解
- \`gateway/\` —— 各支付渠道適配，新增渠道走 \`GatewayAdapter\` 介面
- \`idempotency/\` —— 冪等鍵儲存，禁止繞過

## 常見任務索引
- 新增支付渠道：參考 \`docs/add-gateway.md\`
- 修改對帳邏輯：必須同時更新 \`tests/reconciliation/\`
```

#### 一個容易犯的錯誤

把可重用的專業知識塞進 [CLAUDE.md](http://claude.md/) 。比如把「如何做安全審查的 30 條清單」全塞進根目錄——這些內容應該放進 Skills，按需載入，而不是每次對話都占用上下文。

判斷標準很簡單： **這條資訊每次對話都需要嗎？** 是 → [CLAUDE.md](http://claude.md/) ；否 → Skill。

---

### 第二層：Hooks——讓配置自我進化

大多數團隊把 Hooks 理解成「防止 Claude 做錯事的腳本」。這只用到了 Hooks 一半的價值。

**Hooks 更大的價值在於持續改進。**

#### 基礎用法：寫完檔案自動格式化

```
json 体验AI代码助手 代码解读复制代码// .claude/settings.json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "if [[ \"$CLAUDE_FILE_PATH\" =~ \\.(ts|tsx|js|jsx)$ ]]; then npx prettier --write \"$CLAUDE_FILE_PATH\" && npx eslint --fix \"$CLAUDE_FILE_PATH\"; fi"
          },
          {
            "type": "command",
            "command": "if [[ \"$CLAUDE_FILE_PATH\" =~ \\.go$ ]]; then gofmt -w \"$CLAUDE_FILE_PATH\" && goimports -w \"$CLAUDE_FILE_PATH\"; fi"
          }
        ]
      }
    ]
  }
}
```

#### 進階用法：阻止危險操作

```
json 体验AI代码助手 代码解读复制代码{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/guard-bash.sh"
          }
        ]
      }
    ]
  }
}
```

`.claude/hooks/guard-bash.sh` ：

```
bash 体验AI代码助手 代码解读复制代码#!/usr/bin/env bash
# 攔截高風險命令：rm -rf /、生產資料庫連線、force push 到 main
INPUT=$(cat)
CMD=$(echo "$INPUT" | jq -r '.tool_input.command // ""')

if echo "$CMD" | grep -qE '(rm\s+-rf\s+/|drop\s+database|--force.*origin/main)'; then
  echo '{"decision":"block","reason":"危險命令已攔截，請人工確認後再執行"}'
  exit 0
fi

if echo "$CMD" | grep -qE 'psql.*prod|kubectl.*-n\s+production'; then
  echo '{"decision":"ask","reason":"該命令會接觸生產環境，請確認意圖"}'
  exit 0
fi
```

#### 最被低估的用法：會話學習 Hook

```
json 体验AI代码助手 代码解读复制代码{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/session-reflect.sh"
          }
        ]
      }
    ]
  }
}
```

`.claude/hooks/session-reflect.sh` ：

```
bash 体验AI代码助手 代码解读复制代码#!/usr/bin/env bash
# 會話結束時，讓 Claude 自己反思有沒有值得沉澱到 CLAUDE.md 的東西
SESSION_DIR="${CLAUDE_PROJECT_DIR}/.claude/sessions"
mkdir -p "$SESSION_DIR"

cat <<'PROMPT' > "$SESSION_DIR/reflect-prompt.md"
回顧這次對話：
1. 我有沒有走過彎路？是因為缺少哪條上下文？
2. 哪些使用者的糾正應該寫進 CLAUDE.md 讓以後不再犯？
3. 是否發現了應該寫成 Skill 的可重用模式？

請生成 patch 建議，不要直接修改檔案。
PROMPT

# 觸發一次獨立 Claude 進程做反思，寫到 inbox 等人 review
claude -p "$(cat $SESSION_DIR/reflect-prompt.md)" \
  --output-format json \
  > "$SESSION_DIR/$(date +%Y%m%d-%H%M%S).reflection.json"
```

第二天上班的時候，DRI 把 `sessions/` 裡的反思掃一遍，把真正有價值的沉澱到根 [CLAUDE.md](http://claude.md/) 。這是配置隨時間自我進化的核心機制。

---

### 第三層：Skills——按需載入專業知識

大型程式碼庫有幾十種任務類型。如果把所有專業知識都塞進每次對話，上下文會被撐爆，效能會下降。

Skills 解決這個問題的方式叫 **漸進式揭露** ：把專業工作流和領域知識打包成獨立的 Skill，只在任務需要時載入。

#### 一個 Skill 的真實結構

```
bash 体验AI代码助手 代码解读复制代码.claude/skills/payment-gateway-onboarding/
├── SKILL.md              # 入口檔案，含 frontmatter
├── checklist.md          # 接入新支付渠道的 30 項檢查清單
├── templates/
│   ├── adapter.java.tmpl # 適配器模板
│   └── test.java.tmpl    # 測試模板
└── references/
    └── compliance.md     # PCI-DSS 合規要點（按需讀取）
```

`SKILL.md` ：

```
markdown 体验AI代码助手 代码解读复制代码---
name: payment-gateway-onboarding
description: 接入新的第三方支付渠道（Stripe / Adyen / 支付寶 / 微信支付等）。當使用者提到「接入支付」、「新增 gateway」、「對接 XX 支付」時使用。
allowed-paths:
  - services/payment/**
---

# 接入新支付渠道

## 第一步：確認資訊齊全
在動手前，確認拿到了以下材料（缺一不可）：
- [ ] 渠道方的 API 文件與沙箱環境憑證
- [ ] 商戶號 / appid（生產 + 沙箱）
- [ ] 回調簽名演算法與公私鑰
- [ ] 合規備案文件（見 \`references/compliance.md\`）

## 第二步：生成骨架
基於 \`templates/adapter.java.tmpl\` 建立新檔案：
\`services/payment/src/main/java/.../gateway/<channel>/<Channel>Adapter.java\`

## 第三步：實作 GatewayAdapter 介面
必須實作：\`charge\`, \`refund\`, \`query\`, \`verifyCallback\`。
冪等鍵統一走 \`IdempotencyService\`，不要自己造輪子。

## 第四步：測試矩陣
按 \`checklist.md\` 跑完 30 項檢查，缺一項不允許合併。
```

#### Skills 的路徑作用域

注意上面 frontmatter 裡的 `allowed-paths` ——這個 Skill 只在 `services/payment/**` 下工作時啟用。前端工程師改 React 元件時，不會被這個 Skill 打擾。這對 monorepo 特別重要。

#### 實際的 Skill 庫示例

成熟團隊的 `.claude/skills/` 目錄大概長這樣：

```
bash 体验AI代码助手 代码解读复制代码.claude/skills/
├── security-review/             # 安全審查清單
├── payment-gateway-onboarding/  # 新增支付渠道
├── db-migration/                # 資料庫遷移規範
├── api-changelog/               # API 變更自動更新 changelog
├── incident-postmortem/         # 事故復盤模板
└── frontend-component-new/      # 新增前端元件
```

每個 Skill 只在被相關任務啟用時載入，平時不占上下文。

---

### 第四層：Plugins——把好的配置分發出去

一個團隊摸索出了好用的 Skills + Hooks + MCP 配置組合，怎麼讓整個組織都用上？

答案是 Plugins：把這些東西打包成一個可安裝的包。新工程師入職第一天安裝這個 Plugin，立刻擁有和老員工一樣的上下文和能力。

#### 一個內部 Plugin 的目錄結構

```
csharp 体验AI代码助手 代码解读复制代码acme-platform-plugin/
├── plugin.json                  # 清單
├── skills/
│   ├── deploy-to-staging/
│   └── internal-rpc-client/
├── hooks/
│   ├── settings.json
│   └── scripts/
│       └── jira-link.sh
└── mcp/
    └── servers.json             # 內部 analytics / Jira / Confluence MCP
```

`plugin.json` ：

```
json 体验AI代码助手 代码解读复制代码{
  "name": "acme-platform",
  "version": "1.4.2",
  "description": "Acme 內部 Claude Code 配置：部署、Jira、內部 RPC 工具集",
  "skills": ["./skills/deploy-to-staging", "./skills/internal-rpc-client"],
  "hooks": "./hooks/settings.json",
  "mcp": "./mcp/servers.json",
  "minimum-claude-code": "1.0.0"
}
```

工程師只需要：

```
bash 体验AI代码助手 代码解读复制代码claude plugin install [email protected]:devplatform/claude-code-plugin.git
```

#### Anthropic 觀察到的真實案例

一家大型零售商在全面推廣 Claude Code 之前，先建構了一個 Skill，把 Claude 連接到他們的內部分析平台，讓業務分析師不用離開工作流就能拉取效能資料。這個 Skill 被打包成 Plugin，在大規模推廣前就分發到位了—— **先做基礎設施，再做推廣** ，這是順序問題。

---

### 第五層：LSP 集成——從文字匹配升級到符號導航

沒有 LSP 的情況下，Claude 用文字匹配來找程式碼。在大型程式碼庫裡，grep 一個常見函式名比如 `process` 可能回傳幾千個匹配，Claude 要燒掉大量上下文逐個打開檔案判斷哪個才是目標。

有了 LSP，Claude 獲得了和 IDE 一樣的導航能力：追蹤函式呼叫到定義、跨檔案追蹤引用、區分不同語言中同名函式。過濾在 Claude 讀任何檔案之前就完成了。

#### 配置 LSP 集成

```
json 体验AI代码助手 代码解读复制代码// .claude/settings.json
{
  "lsp": {
    "servers": {
      "typescript": {
        "command": "typescript-language-server",
        "args": ["--stdio"],
        "rootMarkers": ["package.json", "tsconfig.json"]
      },
      "go": {
        "command": "gopls",
        "args": ["serve"],
        "rootMarkers": ["go.mod"]
      },
      "java": {
        "command": "jdtls",
        "rootMarkers": ["pom.xml", "build.gradle"]
      },
      "cpp": {
        "command": "clangd",
        "args": ["--background-index", "--clang-tidy"],
        "rootMarkers": ["compile_commands.json"]
      }
    }
  }
}
```

#### 沒 LSP 和有 LSP 的對比

```
text 体验AI代码助手 代码解读复制代码# 沒 LSP：grep 找 ProcessOrder 的定義
$ grep -rn "ProcessOrder" .
找到 437 處匹配
→ Claude 需要打開 N 個檔案來確定真正的定義

# 有 LSP：goToDefinition
→ 一次呼叫直接回傳 services/order/internal/processor.go:42
→ 上下文消耗：1 個檔案
```

一家企業軟體公司在推廣 Claude Code 之前，專門在全組織部署了 LSP 集成，目的是讓 C 和 C++ 的導航在大規模場景下可靠運行。對於多語言程式碼庫，這是投入產出比最高的基礎設施投資之一。

---

### 第六層：MCP 伺服器——連接內部工具

MCP 伺服器是 Claude 連接內部工具、資料來源和 API 的方式。

#### 一份典型的 MCP 配置

```
json 体验AI代码助手 代码解读复制代码// .claude/mcp.json
{
  "mcpServers": {
    "jira": {
      "command": "npx",
      "args": ["-y", "@acme/mcp-jira"],
      "env": {
        "JIRA_BASE_URL": "https://acme.atlassian.net",
        "JIRA_TOKEN": "${JIRA_TOKEN}"
      }
    },
    "internal-docs": {
      "command": "node",
      "args": ["./tools/mcp/confluence-server.js"],
      "env": {
        "CONFLUENCE_SPACE": "ENG"
      }
    },
    "code-search": {
      "command": "./tools/mcp/sourcegraph-server",
      "env": {
        "SG_ENDPOINT": "https://sg.internal.acme.com"
      }
    }
  }
}
```

配上之後，Claude 可以直接呼叫 `mcp__jira__create_issue` 、 `mcp__internal-docs__search` 這類工具，不需要 copy-paste。

#### 注意建構順序

MCP 伺服器應該在基礎配置（ [CLAUDE.md](http://claude.md/) 、Hooks、Skills）都到位之後再建構。常見的反模式是：

> 「我們先接一個 GitHub MCP 上去看看效果。」

——結果是 Claude 在還沒搞清楚專案結構的情況下亂調外部 API，搞出一堆雜訊。 **基礎不打好，外部工具只會放大混亂。**

---

### 三個讓大型程式碼庫可用的配置模式

#### 模式一：讓程式碼庫對 Claude 可導航

##### 保持 CLAUDE.md 精簡分層

根目錄只放指標和關鍵注意事項。Claude 在遍歷程式碼庫時會逐層載入 [CLAUDE.md](http://claude.md/) ，根目錄的上下文永遠不會丟失，所以不需要在根目錄塞所有資訊。

##### 從子目錄初始化，不要從倉庫根目錄

在 monorepo 裡，這可能反直覺——工具通常假設從根目錄執行。但 Claude 會自動向上遍歷目錄樹載入所有 [CLAUDE.md](http://claude.md/) ，所以從子目錄開始工作，既能獲得局部精確上下文，也不會丟失根目錄的全域資訊。

```
bash 体验AI代码助手 代码解读复制代码# 反例：在倉庫根目錄，讓它修支付服務的 bug
cd ~/work/acme-monorepo
claude
# → 載入根 CLAUDE.md，但本地上下文還是倉庫根，開搜會掃到很多無關目錄

# 正解：直接進子目錄
cd ~/work/acme-monorepo/services/payment
claude
# → 既載入了根 CLAUDE.md，又載入了 services/payment/CLAUDE.md
# → 命令、測試範圍都自動收斂到本服務
```

##### 按子目錄作用域設定測試和 lint 命令

當 Claude 只改了一個服務，卻要跑整個測試套件，會超時，還會用無關輸出浪費上下文。子目錄的 [CLAUDE.md](http://claude.md/) 應該指定該目錄適用的命令：

```
markdown 体验AI代码助手 代码解读复制代码# /services/inventory/CLAUDE.md

## 命令

- 單元測試：\`go test ./...\`（只跑本服務，約 30 秒）
- 整合測試：\`make inventory-it\`（依賴 docker-compose，約 3 分鐘）
- 建置：\`go build ./cmd/server\`
- Lint：\`golangci-lint run ./...\`

⚠️ 不要在本目錄下跑 \`make test\`（那是全量測試，20 分鐘，會超時）。
```

注意：這個方案對服務化架構效果好。對於有深層跨目錄依賴的編譯型語言 monorepo（比如大型 C++ 專案），按子目錄作用域更難實現，可能需要專案特定的建置配置。

##### 用.ignore 檔案和權限規則排除雜訊

```
json 体验AI代码助手 代码解读复制代码// .claude/settings.json
{
  "permissions": {
    "deny": [
      "Read(/generated/**)",
      "Read(/node_modules/**)",
      "Read(/build/**)",
      "Read(/.next/**)",
      "Read(/dist/**)",
      "Read(/vendor/**)",
      "Read(/**/*.min.js)",
      "Read(/**/*.lock)"
    ],
    "ask": [
      "Bash(git push:*)",
      "Bash(rm -rf:*)",
      "Bash(kubectl:*)"
    ]
  }
}
```

把這個檔案 **提交到版本控制** ，整個團隊都能獲得同樣的降噪效果，不需要每個人單獨配置。

##### 為目錄結構不規範的程式碼庫建構程式碼地圖

如果程式碼沒有按常規目錄結構組織，在倉庫根目錄放一個 `CODEMAP.md` ，給 Claude 一個可以掃描的目錄：

```
markdown 体验AI代码助手 代码解读复制代码# 程式碼庫結構地圖

## 一級目錄
- \`/core\`           核心業務邏輯（Java，約 30 萬行）
- \`/adapters\`       外部系統適配器（按渠道分子目錄）
- \`/legacy-billing\` 舊版計費系統（PHP，**只讀**，2026Q4 下線）
- \`/tools\`          內部開發工具，CI 也依賴
- \`/docs\`           技術文件（ADR + API 規範）

## 重要入口
- HTTP 入口：\`core/src/main/java/.../HttpServer.java\`
- 定時任務：\`core/src/main/java/.../jobs/JobScheduler.java\`
- 配置載入：\`core/config/\` + 環境變數見 \`infra/env/\`

## 歷史包袱
- \`core/util/Helpers.java\` —— 4000 行的「什麼都往裡塞」工具類，要拆但還沒拆
- \`adapters/legacy-mq/\` —— 已停用但還沒刪，觸發別影響
```

然後在根 `CLAUDE.md` 加一句： `不熟悉本倉庫的話先讀 CODEMAP.md` 。

#### 模式二：隨模型演進主動維護 CLAUDE.md

[CLAUDE.md](http://claude.md/) 裡的指令是為當時的模型寫的。模型升級後，這些指令可能變成負擔。

一個告訴 Claude 把每次重構拆成單檔案變更的規則，可能是為了幫助舊模型保持專注——但新模型完全能處理協調的跨檔案編輯，這條規則反而會阻止它做正確的事。

**建議每三到六個月做一次配置審查** ，在重大模型發布後也值得做一次。問自己每條規則：

問題處理這條規則是在補償模型的某個局限嗎？是 → 測試新模型是否還需要這條規則是在表達真實的專案約定嗎？是 → 保留半年內有人違反過這條規則嗎？沒有 → 可能已是常識，可以刪這條規則的反例容易在 PR 裡抓到嗎？是 → 移到 hook/CI，不要靠 prompt一份很務實的審查清單可以做成 Skill：

```
markdown 体验AI代码助手 代码解读复制代码# .claude/skills/config-audit/SKILL.md
---
name: config-audit
description: 季度性審查 CLAUDE.md，識別可以刪除或遷移到 hook 的規則
---

按以下步驟逐條審查：
1. 讀取所有 CLAUDE.md（根 + 各子目錄）
2. 對每條規則打三個標籤：[必要 | 可移除 | 可移到 hook]
3. 生成 diff 提案到 \`.claude/audit-YYYY-MM.md\`，不直接改檔案
4. 郵件 / Slack 通知 DRI review
```

#### 模式三：給 Claude Code 管理分配明確的負責人

技術配置到位了，但沒有人負責維護，配置會腐化，好的實踐會停留在少數人手裡。

Anthropic 觀察到推廣最順利的公司，都在大規模推廣之前做了專項基礎設施投入。有的是兩個工程師提前建構了一套 Plugins 和 MCP，讓第一批使用者一上手就有生產力。有的是整個團隊專門負責 AI 編碼工具的基礎設施，在推廣前就把一切準備好。

**最低可行版本** ：一個 DRI（直接負責人）。這個人負責：

- Claude Code 配置的所有權
- Settings、權限策略、Plugin 市集、 [CLAUDE.md](http://claude.md/) 約定的決策權
- 保持這些內容的時效性
- 每月發一份「配置變更 + 資料指標」週報

沒有這個角色，自下而上的採用會產生熱情，但也會產生碎片化——每個團隊各自摸索，好的實踐停留在部落知識裡，無法擴散。

**對於大型組織** ，尤其是受監管行業，治理問題會早早浮現：誰控制哪些 Skills 和 Plugins 可用？如何防止幾千個工程師各自重複建構同樣的東西？AI 生成的程式碼如何走和人工程式碼一樣的審查流程？

建議從一開始就建立跨職能工作組，把工程、資訊安全、合規代表拉在一起定義要求，共同制定推廣路線圖。

---

### 一個容易忽視的細節：Subagents

Subagents 是獨立的 Claude 實例，有自己的上下文視窗，接受任務、完成工作、只把最終結果回傳給父實例。

在大型程式碼庫裡，一個常見的高效模式是： **探索和編輯分離** 。

#### 實戰示例

假設你要在一個 50 萬行的服務裡重構日誌模組。直接做的問題是：父 Agent 一邊搜一邊讀，光是搞清楚「現在的日誌怎麼用的」就吃掉了 60% 的上下文，等到要編輯時已經沒空間了。

正確的做法：

```
text 体验AI代码助手 代码解读复制代码> 我要把專案裡的 log4j 全部替換為 slog，先幫我摸清現況

● 我先派一個只讀 subagent 去做完整調查，結果寫到檔案，然後我基於結果做改動。

● Task(subagent_type: "explore", description: "調查 log4j 使用")
  Prompt:
    遍歷整個倉庫，找出所有 log4j 的使用方式。生成報告到 .claude/scratch/log4j-usage.md，
    包含：
    1. 所有 import log4j 的檔案路徑
    2. 按使用模式分類（直接 logger.info / 包裝類 / 自訂 Appender）
    3. 特殊用法清單（非同步 logger、MDC、自訂格式）
    不要做任何修改。

● [subagent 幹完，返回報告路徑]

● 現在我讀這份報告，制定遷移計畫
  └─ Read(".claude/scratch/log4j-usage.md")
```

父 Agent 的上下文視窗幾乎沒被探索消耗，全留給真正的重構編輯。一次任務跑下來，能多撐 3-5 倍的工作量。

#### 用 Subagent 做並行驗證

```
text 体验AI代码助手 代码解读复制代码> 改完後幫我跑三組驗證

● 並行啟動三個 subagent：
  Task(subagent_type: "test-engineer", "跑所有 log 相關單元測試")
  Task(subagent_type: "security-reviewer", "確認 slog 配置沒洩漏 PII")
  Task(subagent_type: "verifier", "對照遷移清單逐項核對")
```

主 Agent 等三個結果彙總，再決定是否合併。這個模式在大型 PR 收尾階段特別好用。

---

### 總結：從哪裡開始

如果你的團隊剛開始在大型程式碼庫裡用 Claude Code，按這個順序來：

1. **先寫 [CLAUDE.md](http://claude.md/)** ：根目錄放全域資訊（用上面的模板抄一份），關鍵子目錄放局部約定。精簡，只放真正有用的。
2. **配置.ignore 和權限規則** ：排除生成檔案和建置產物，提交到版本控制。模板見上面 `.claude/settings.json` 。
3. **設定 LSP 集成** ：特別是 C/C++/Java 這類強型別語言，這是最高投入產出比的基礎設施。
4. **建構第一批 Hooks** ：從 lint/format 自動化開始，再加一個 stop 反思 hook，最後做命令攔截。
5. **把可重用的專業知識打包成 Skills** ：不要全塞進 [CLAUDE.md](http://claude.md/) 。先做 3-5 個高頻任務的 Skill。
6. **打包成內部 Plugin 分發** ：讓新人 `claude plugin install` 一行命令就能上車。
7. **指定一個 DRI** ：沒有負責人，配置會腐化。
8. **三到六個月做一次配置審查** ：隨模型演進調整，刪掉已經不需要的規則。

Claude Code 在百萬行 monorepo、幾十年歷史的遺留系統、跨幾十個倉庫的分散式架構裡都有成功案例。這些環境的共同點不是程式碼庫有多整潔，而是團隊在配置上做了投入。

**模型只是一半，配置才是另一半。** 把這另一半做到位，差距就出來了。

---

*參考資料： [How Claude Code works in large codebases: Best practices and where to start](https://link.juejin.cn/?target=https%3A%2F%2Fclaude.com%2Fblog%2Fhow-claude-code-works-in-large-codebases-best-practices-and-where-to-start) — Anthropic Blog, May 14, 2026*

---

原文出處： [https://juejin.cn/post/7641417840354099240](https://juejin.cn/post/7641417840354099240)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aAOnPx) [63瀏覽](https://codelove.tw/@tony/post/aAOnPx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aAOnPx)

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

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝18 💬11 ❤️1

550

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

82

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779686912&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaAOnPx&amp;pra=5&amp;asro=0&amp;itsi=11&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779686912001&amp;bpp=1&amp;bdt=79&amp;idt=78&amp;shv=r20260520&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779686630%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779686630%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779686630%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5382915471704&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95391807&amp;oid=2&amp;pvsid=2607983516690917&amp;tmod=1302172221&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=98" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260520/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>