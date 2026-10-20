---
title: "雙工具策略：Gemini CLI 探索、Claude Code 執行"
source: "https://www.termdock.com/zh/blog/dual-tool-strategy-gemini-cli-claude-code"
author:
  - "[[Termdock Team]]"
published: 2024-01-01
created: 2026-05-13
description: "用雙工具策略節省 60-70% 的 AI 寫程式工具費用。Gemini CLI 免費方案負責探索，Claude Code 負責複雜執行。附完整工作流、費用計算和設定指南。"
tags:
  - "clippings"
status: processed
---
## 問題：用手術刀塗奶油

想像一個外科醫生用手術刀塗奶油。技術上可以。但成本荒謬，而且刀刃會為了真正重要的手術而鈍掉。

Claude Code 就是那把手術刀。2026 年最強的終端 AI 寫程式工具。多檔案重構、架構推理、複雜除錯，精準度遠超市場上任何對手。Pro 方案每月 $20 美元但用量有限，重度使用者得訂 Max 方案，每月 $100-200 美元。

Gemini CLI 是廚房菜刀。Google 的開源終端 agent，每個 Google 帳號免費。每天 1,000 次模型請求，不需要信用卡。內建智慧模型路由，簡單任務走 Gemini Flash（快、省），複雜任務走 Gemini Pro（慢、強）。程式碼解釋、小修改、寫測試、產文件這類直接的工作，表現不錯。

兩者的差距是真實的。Claude Code 搭載 Opus 4.6，在需要深度架構理解的複雜多步驟任務上，產出品質明顯更好。Gemini CLI 處理簡單任務更快，但遇到大規模重構和隱微的 bug 就到了天花板。

雙工具策略就是利用這個落差。每天 60-70% 不需要頂級推理的任務交給 Gemini CLI。剩下 30-40% 真正複雜的交給 Claude Code。所有 AI CLI 工具的完整能力和定價比較，可以參考 [2026 AI CLI 工具完全指南](https://www.termdock.com/zh/blog/ai-cli-tools-guide) 。

## 判斷框架：什麼任務用什麼工具

核心問題只有一個。這個任務需要深度多步驟推理，還是明確的單步操作？

| 任務類型 | 推薦工具 | 原因 |
| --- | --- | --- |
| 程式碼庫探索（「這個模組做什麼？」） | Gemini CLI | 唯讀、零風險、免費 |
| Code review 和回饋 | Gemini CLI | 模式比對為主，非深度推理 |
| 為既有程式碼撰寫單元測試 | Gemini CLI | 範圍明確、模版驅動 |
| 小 bug 修復（3 個檔案以內） | Gemini CLI | 低複雜度、快速迭代 |
| 產生文件 | Gemini CLI | 直接的資訊擷取工作 |
| 跨多檔案的架構重構 | Claude Code | 需要全系統理解 |
| 複雜除錯（並行、狀態管理） | Claude Code | 需要深度因果推理 |
| 新功能實作（10+ 檔案） | Claude Code | 多步驟規劃和執行 |
| 安全相關程式碼變更 | Claude Code | 準確度比成本重要 |
| 效能優化 | Claude Code | 需要理解執行期行為 |

規律很清楚。Gemini CLI 處理範圍明確、錯誤風險低的任務。Claude Code 處理第一次就要做對、否則要花好幾小時除錯的任務。

灰色地帶的任務（3-5 個檔案的重構、中等複雜度的 bug）先用 Gemini CLI 試。第一次沒抓到重點，再切到 Claude Code。你損失 2-3 分鐘的 Gemini CLI 時間，但在簡單任務上省下了付費額度。

## 設定雙工具 workflow

### 安裝 Gemini CLI

Gemini CLI 需要 Node.js 18+ 和 Google 帳號。

```bash
npm install -g @google/gemini-cli
```

在終端輸入 `gemini` 。瀏覽器開啟進行 Google 帳號驗證。不用 API key，不用信用卡。免費額度立即啟用，每天 1,000 次模型請求，每分鐘 60 次。

### 安裝 Claude Code

Claude Code 需要 Claude 訂閱（Pro 方案 $20/月，或 Max 方案 $100-200/月）或 Anthropic API key。

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

在終端輸入 `claude` ，依照提示完成驗證。連結到你的 Anthropic 帳號後即可使用。

### 雙終端配置

想像一間控制室。兩個螢幕、兩組畫面、兩個測量不同東西的儀器。

雙工具 workflow 在兩個工具同時可見時效果最好。一個終端面板開 Gemini CLI 做探索和快速迭代，另一個面板開 Claude Code 做專注執行。你可以一邊看 Gemini CLI 對某個模組的分析，一邊指揮 Claude Code 進行重構。不切視窗，不斷脈絡。

這種並排配置正是專業終端開發環境的價值所在。 [Termdock](https://www.termdock.com/zh) 可以自由拖拉調整終端面板大小，左邊放 Gemini CLI、右邊放 Claude Code、底部放測試跑者。檔案直接拖進任一終端就能操作，workspace 層級的 Git 狀態自動在兩邊同步。

Try Termdock — Multi Terminal works out of the box. [Free download →](https://github.com/termdock/termdock-issues/releases)

## 實戰範例：完整開發一個功能

具體案例。在 Next.js API 加上 rate limiting middleware。總耗時約 25 分鐘。

### 第一步：用 Gemini CLI 探索（3 分鐘）

```bash
# 終端 1 — Gemini CLI
gemini

> What rate limiting libraries work well with Next.js App Router?
> Show me the current API route structure in this project.
> Are there any existing middleware patterns I should follow?
```

Gemini CLI 讀取專案，找到既有的 middleware 串接方式，建議三種做法：自製 token bucket、搭配 Redis 的 `@upstash/ratelimit` 、或較簡單的 `next-rate-limit` 。花費：3-5 次免費請求。

### 第二步：確定方案（2 分鐘）

```bash
# 繼續在終端 1 — Gemini CLI
> Compare @upstash/ratelimit vs a custom token bucket for this project.
> We're already using Vercel KV. Does @upstash/ratelimit work with that?
```

Gemini CLI 確認與 Vercel KV 相容，根據既有架構推薦 `@upstash/ratelimit` 。花費：再 2 次免費請求。

### 第三步：用 Claude Code 實作（15 分鐘）

```bash
# 終端 2 — Claude Code
claude

> Implement rate limiting for all API routes using @upstash/ratelimit
> with our existing Vercel KV connection. Use the sliding window
> algorithm, 60 requests per minute per IP. Add the middleware to
> the existing chain in middleware.ts. Include proper 429 responses
> with Retry-After headers.
```

現在看手術刀怎麼工作。Claude Code 讀取完整專案 context，理解既有 middleware 模式，安裝依賴套件，建立 rate limiting middleware，整合到既有串接鏈，加上正確的錯誤回應，建立測試。這是一個跨多檔案的變更，涉及 `middleware.ts` 、新建的 `lib/rate-limit.ts` 、 `package.json` 更新，以及新的測試檔。

### 第四步：用 Gemini CLI 驗證（5 分鐘）

```bash
# 回到終端 1 — Gemini CLI
> Review the changes Claude Code just made to middleware.ts and
> lib/rate-limit.ts. Any issues?
```

Gemini CLI 審查 diff，標記疑慮，確認實作符合專案的既有模式。花費：再 2-3 次免費請求。

**費用拆解：** 8-10 次 Gemini CLI 請求（免費）加上 1 次 Claude Code session（訂閱涵蓋）。如果不用雙工具策略，探索、比較、實作、審查全部消耗 Claude Code 的額度。

## 費用計算

省下多少取決於你的使用模式。三個實際情境。

### 情境一：獨立開發者，中度使用

|  | 只用 Claude Code | 雙工具策略 |
| --- | --- | --- |
| 方案 | Max 5x（$100/月） | Pro（$20/月） |
| 複雜任務/天 | 5-8 | 5-8（Claude Code） |
| 簡單任務/天 | 15-20（也用 Claude Code） | 15-20（Gemini CLI，免費） |
| 月費 | **$100** | **$20** |
| 節省 | — | **$80/月（$960/年）** |

把每天 15-20 個簡單任務轉移到 Gemini CLI 免費方案，獨立開發者可以從 Max 降到 Pro，不犧牲任何真正重要的能力。

### 情境二：專業開發者，重度使用

|  | 只用 Claude Code | 雙工具策略 |
| --- | --- | --- |
| 方案 | Max 20x（$200/月） | Max 5x（$100/月） |
| 複雜任務/天 | 10-15 | 10-15（Claude Code） |
| 簡單任務/天 | 25-40（也用 Claude Code） | 25-40（Gemini CLI，免費） |
| 月費 | **$200** | **$100** |
| 節省 | — | **$100/月（$1,200/年）** |

重度使用者把日常任務轉到 Gemini CLI 後，通常能從 Max 20x 降到 Max 5x。

### 情境三：預算型開發者

|  | 只用 Claude Code | 雙工具策略 |
| --- | --- | --- |
| 方案 | Pro（$20/月） | 全部免費工具 |
| 複雜任務/天 | 3-5（碰到限制） | 3-5（Gemini CLI Pro 模型） |
| 簡單任務/天 | 5-10（更快碰限制） | 15-20（Gemini CLI，免費） |
| 月費 | **$20** | **$0** |
| 節省 | — | **$20/月（$240/年）** |

能接受複雜任務品質稍低的開發者，Gemini CLI 的 Pro 模型路由可以處理大部分工作。等到持續碰到 Gemini CLI 的品質天花板時，再加入 Claude Code Pro 方案。

每個 AI CLI 工具的詳細定價拆解，在 [2026 AI CLI 工具完全指南](https://www.termdock.com/zh/blog/ai-cli-tools-guide) 中有完整說明。

## 最大化免費額度的技巧

### 搞懂什麼算一次「請求」

Gemini CLI 每天 1,000 次的限制算的是模型 API 呼叫，不是你輸入的 prompt 數量。一個像「重構這個檔案」的 prompt，可能觸發 5-15 次 API 呼叫，Gemini CLI 會讀取檔案、規劃修改、撰寫程式碼、驗證結果。實際上，1,000 次日請求大約等於 80-150 個 prompt，取決於複雜度。

### 用精準的 prompt

把 prompt 想成地圖上的座標。模糊的 prompt 像「改善這個專案」，agent 在整片地景上漫遊，每一步都燒請求。精準的 prompt 像「在 `app/api/auth/signup/route.ts` 的 signup handler 加上輸入驗證」，地圖上的一個點。Agent 直線走到那裡。

### 讓模型路由自動工作

Gemini CLI 的自動路由把簡單 prompt 送到 Gemini Flash，複雜 prompt 送到 Gemini Pro。除非有特定理由，不要手動覆蓋這個設定。Flash 處理簡單任務更快，把每日額度留給真正需要 Pro 級推理的任務。

### 批次你的探索

不要對同一個模組問五個獨立問題，改問一個複合問題：「解釋認證模組，它的進入點、session 管理策略、token 更新機制、以及錯誤處理模式。」一個 prompt、一組 API 呼叫，而不是五輪獨立的往返。

### 注意用量分配

Gemini CLI 目前在介面上不顯示請求計數器。如果要做大量探索，把工作分散到一整天，不要在早上的幾個小時內把請求燒完。每日額度在太平洋時間午夜重置。

## 這個策略什麼時候不適用

雙工具策略有真實的限制。以下情境應該直接用 Claude Code。

**不熟悉的程式碼庫加上緊迫的時程。** 想像在時間壓力下導航一座陌生城市。你需要最好的地圖，不是最便宜的。Claude Code 更強的 context 理解省下的時間，遠超過 Gemini CLI 免費方案省下的金錢。Gemini CLI 一次錯誤方向浪費的 30 分鐘，就超過一個月的 Claude Code Pro 訂閱費。

**安全相關的程式碼。** 認證流程、加密、存取控制、支付處理。Claude Code 和 Gemini CLI 之間的準確度差距在這裡很關鍵。一個安全漏洞的代價比訂閱費高出好幾個數量級。

**生產環境事故排查。** 服務掛了需要跨 log、程式碼和基礎設施做根因分析時，Claude Code 更深的推理鏈能更快、更準確地定位問題。這不是省成本的時候。

**長鏈式重構。** 如果一個重構涉及 20 個以上檔案且修改會層層傳遞，Gemini CLI 的 context 管理會力不從心。Claude Code 的 1M context window 和更強的架構推理能乾淨地處理這類任務。關於多檔案 workflow 和多 agent 開發模式的詳細說明， [2026 AI CLI 工具完全指南](https://www.termdock.com/zh/blog/ai-cli-tools-guide) 涵蓋了 git worktree 平行化和 agent 協調的完整策略。

**學習新技術的時候。** 如果你正在用 AI CLI 工具學一個新框架或語言，Claude Code 的解釋持續地更細緻、更準確。訂閱費的投資會以學習品質的形式回報。

## 開始行動清單

1. **安裝 Gemini CLI** — 執行 `npm install -g @google/gemini-cli` ，再輸入 `gemini` 用 Google 帳號驗證。確認看到 Gemini CLI 的提示介面。約 2 分鐘。
2. **安裝 Claude Code** — 執行 `curl -fsSL https://claude.ai/install.sh | bash` ，再輸入 `claude` 驗證。至少需要 Pro 訂閱（$20/月）。約 5 分鐘。
3. **設定你的終端配置** — 開兩個並排的終端面板。左邊：Gemini CLI。右邊：Claude Code。標記清楚，隨時知道你在跟哪個工具對話。
4. **建立 CLAUDE.md 檔案** — 在專案根目錄加一個 context 檔案，寫入你的架構、慣例和限制條件（200-500 字）。兩個工具都受惠於明確的專案 context。
5. **開始第一次雙工具 session** — 選一個真實任務。用 Gemini CLI 探索相關程式碼、腦力激盪方案。用 Claude Code 實作選定的方案。計時。一次 session 就能看出模式。
6. **追蹤一週的使用模式** — 記錄哪些任務送到哪個工具、為什麼。一週後你會有清楚的個人判斷框架，比查任何指南都快。
7. **調整 Claude Code 方案** — 使用雙工具策略兩週後，檢查能否降級 Claude Code 訂閱等級。多數開發者發現可以降一級而不影響生產力。

DH

Danny Huang· [Follow on Threads →](https://www.threads.com/@cyh.289)

Free Download

### Ready to streamline your terminal workflow?

Multi-terminal drag-and-drop layout, workspace Git sync, built-in AI integration, AST code analysis — all in one app.

[Download Termdock →](https://github.com/termdock/termdock-issues/releases)

#gemini-cli#claude-code#cost-optimization#ai-cli#workflow

## 相關文章

2026年3月16日 ·ai-cli-tools

### [2026 AI CLI 工具完全指南：從安裝到多 Agent 工作流](https://www.termdock.com/zh/blog/ai-cli-tools-guide)

完整涵蓋 2026 年所有主流 AI 終端寫程式工具的指南。包含 Claude Code、Gemini CLI、Copilot CLI、Codex CLI、aider、Crush、OpenCode、Goose、Amp 的安裝教學、定價分析、context engineering、多 Agent 工作流、MCP 整合、終端模擬器搭配，以及安全最佳實踐。

ai-cliclaude-codegemini-clicopilot-cliterminaldeveloper-tools

2026年3月16日 ·ai-cli-tools

### [Claude Code + Gemini CLI：免費搭付費，AI 寫程式帳單省 60-70%（2026 實戰指南）](https://www.termdock.com/zh/blog/claude-code-vs-gemini-cli)

Claude Code 的進階推理搭配 Gemini CLI 的免費額度，AI 寫程式費用直接砍 60-70%。任務路由、設定教學、費用計算一次搞定。

claude-codegemini-clicomparisonai-clideveloper-tools

2026年3月20日 ·ai-cli-tools

### [平行開發實戰：3 個 Claude Code Agent、3 個 Worktree、1 個 Termdock](https://www.termdock.com/zh/blog/parallel-dev-3-agents-3-worktrees-termdock)

用 3 個 Claude Code agent 搭配 3 個 git worktree，在一個 Termdock 視窗中同時開發 3 個功能。完整實戰流程，從設定到合併。

claude-codegit-worktreeparallel-developmentmulti-agenttermdockai-cli

2026年3月20日 ·ai-cli-tools

### [用 3 個 AI CLI 工具同時建構全端應用程式](https://www.termdock.com/zh/blog/scaffold-fullstack-app-3-ai-cli-tools)

同時使用 Claude Code、Gemini CLI、Codex CLI 搭配 git worktree，30 分鐘內從零建構全端應用程式。完整步驟與指令。

claude-codegemini-clicodex-cliscaffoldfullstackparallel-developmentai-cli