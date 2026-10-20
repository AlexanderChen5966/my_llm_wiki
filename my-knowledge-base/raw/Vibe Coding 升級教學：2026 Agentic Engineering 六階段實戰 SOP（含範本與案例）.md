---
title: "Vibe Coding 升級教學：2026 Agentic Engineering 六階段實戰 SOP（含範本與案例）"
source: "https://israynotarray.com/ai/20260517/3851629074/"
author:
  - "[[Ray]]"
published: 2026-05-17
created: 2026-05-19
description: "之前寫過 Vibe Coding 是什麼跟新手該補的基礎，這一篇是 2026 升級版完整教學。Karpathy 自己在 2026 年 4 月 Sequoia Ascent 演講把 vibe coding 升級成 agentic engineering 的概念，這篇就是用 agentic engineering 紀律重做 AI 開發實戰流程，涵蓋 Spec / Plan / Tasks 三層落檔（CLAUDE.md + AGENTS.md、ROADMAP、tasks.md 範本）、員工分工（Skill vs Sub-agent vs Agent Teams 差別）、開發紀律（同一輪補測試、UI 分工、Conventional Commits）、雙層收尾驗證（typecheck + test + build 三段驗證 + Writer/Reviewer 模式 + AI 引用 cross-check）、失誤回滾決策表、Memory + CLAUDE.md 雙層架構，最後用一個個人電商 MVP（Nuxt 3 + Supabase + Stripe）案例走一遍完整流程，包含 Stripe 結帳前端竄改價格安全雷怎麼救。"
tags:
  - "clippings"
status: processed
---
![Vibe Coding 升級教學：2026 Agentic Engineering 六階段實戰 SOP（含範本與案例）](https://images.israynotarray.com/blog/ai/vide-agentic-engineering/ChatGPT%20Image%202026%E5%B9%B45%E6%9C%8817%E6%97%A5%20%E4%B8%8B%E5%8D%8802_43_12.png)

Vibe Coding 升級教學：2026 Agentic Engineering 六階段實戰 SOP（含範本與案例）

## 前言

如果你有用過 Claude Code、Cursor、Codex 這類 AI Coding 工具，應該都遇過類似的瓶頸：剛上手覺得超神奇，可是真的拿來開發一個有規模的專案，各種雷就跑出來 — context 爆掉之後 AI 忘光、改 A 壞 B 改了又改、AI 改著改著把好的 code 也蓋掉、好幾個檔案一起被改沒辦法回滾等等。

這些都不是工具的問題，是跟 AI 協作的流程沒建立起來。

說到這邊有件事滿值得提一下， [Andrej Karpathy](https://en.wikipedia.org/wiki/Andrej_Karpathy) 是 2025 年 2 月（ [原始 tweet](https://x.com/karpathy/status/1886192184808149383) ）提出 Vibe Coding 這個詞的人，到了 2026 年 4 月他自己在 [Sequoia Ascent 2026](https://karpathy.bearblog.dev/sequoia-ascent-2026/) 的演講（標題「Software 3.0, Agentic Engineering, and Jagged Intelligence」）上直接升級了這個概念，提出 **Agentic Engineering（代理工程）** 。演講裡關鍵的一句話是：

> 「Vibe coding raises the floor. Agentic engineering is about extrapolating the ceiling.」

翻成白話就是：Vibe coding 是抬升底線，讓沒寫過 code 的人也能做出東西；Agentic engineering 是抬升天花板，是專業工程師該有的工作方式。

那 agentic engineer 跟以前的 vibe coder 差在哪？根據演講的核心訊息，重點不再是描述需求讓 AI 寫然後直接 ship，而是 AI 扮演實作者、你扮演監督者，要做的幾件事大致包含：設計規格、監督計畫、檢查 diff、寫測試、建立評估迴圈、管理權限、隔離 worktree、維持品質。換句話說，agentic engineer 不會盲目接受 AI 生成的 code，而是用工程紀律包住 AI 速度。

之前我寫過 [淺談 AI Vibe Coding 當今主流的 BDD 與 SDD](https://israynotarray.com/ai/20251202/797714546/) 跟 [想開始 Vibe Coding？新手推薦先搞懂的這些基礎知識](https://israynotarray.com/ai/20260427/1657384902/) 這兩篇，前者偏方法論、後者偏新手該補的基礎技能，可是都還停在 vibe coding 框架。這一篇就直接升級，用 agentic engineering 的紀律把整套實戰流程攤開來講，每個環節都附可以直接抄走的範本，最後用一個個人電商 MVP（Nuxt 3 + Supabase + Stripe）的案例走一遍完整流程。

> **Note**  
> 為什麼一定要有紀律？根據 [CodeRabbit 2025 年 12 月發布的「State of AI vs Human Code Generation」報告](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report) （分析了 470 個 GitHub open-source PR，320 個 AI co-authored + 150 個 human-only），AI 寫的 code 比人寫的多 **1.7 倍** major bug、整體資安問題多 **1.57 倍** ，其中 **XSS 漏洞高達 2.74 倍** 、不安全的密碼處理多 1.88 倍。AI 生成的速度很快可是品質沒保證，靠的是你流程設計接住它的失誤。

## Vibe Coding vs Agentic Engineering：2026 你該選哪個？

在進實戰流程之前，先用一張表把兩者差別講清楚：

| 面向 | Vibe Coding | Agentic Engineering |
| --- | --- | --- |
| 提出時間 | 2025 年 2 月 Karpathy 提出 | 2026 年 4 月 Karpathy 升級 |
| 核心精神 | 直接憑直覺描述、AI 寫、不太 review | 設計 spec、監督 plan、檢查 diff、寫測試、做評估 |
| 適用情境 | demo、PoC、拋棄式專案 | 生產級服務、需要長期維護的專案 |
| 流程紀律 | 鬆散、即興 | 結構化、有 SOP |
| 人類角色 | 描述想法的 user | 監督 AI 員工的工程師 |
| 失敗模式 | 跑得起來但隨時會爆 | 慢一點但較可控 |

從這張表可以看出來，兩者不是替代關係，而是適用情境不一樣。週末做個小工具玩玩，vibe coding 完全沒問題；可是要做給別人用的東西、要長期維護的專案，就得切換到 agentic engineering 模式。

接下來這篇的 SOP，全部都是站在 agentic engineering 的紀律寫的。

## 完整流程一張圖：一個功能從需求到 ship

在進細節之前，先用一張流程圖把整套 SOP 壓縮成一個功能的完整協作流程。這張圖有三個角色 — 你（使用者）、Leader（主 Claude）、員工（Sub-agent + Skill），整體 7 個 step：

```
[使用者]                    [Leader / 主 Claude]            [員工 / Sub-agent + Skill]
   │                              │                                  │
   │ ① 提需求                     │                                  │
   ├─────────────────────────────▶│                                  │
   │                              │ ② plan mode 拆解需求             │
   │                              │   （必要時呼叫 sub-agent 探索）   │
   │                              ├─────────────────────────────────▶│
   │ ⚠ 需求有疑問時，Leader       │   產出 docs/tasks.md             │
   │   主動 AskUserQuestion       │◀─────────────────────────────────│
   │◀─────────⇄───────────────────│                                  │
   │                              │ ③ 開發（UI 切給 ui-engineer）    │
   │                              ├─────────────────────────────────▶│
   │                              │   同一輪補單元測試                │
   │                              │◀─────────────────────────────────│
   │                              │ ④ Code Review                    │
   │                              │   （另開 session or sub-agent）   │
   │                              ├─────────────────────────────────▶│
   │                              │   fresh context 抓 bias           │
   │                              │◀─────────────────────────────────│
   │                              │ ⑤ 驗收 + 三段驗證                 │
   │                              │   typecheck + test + build        │
   │                              ├─────────────────────────────────▶│
   │                              │   基於 spec（CLAUDE.md / AGENTS）│
   │                              │◀─────────────────────────────────│
   │ ⑥ 回報 + 更新 ROADMAP/tasks  │                                  │
   │◀─────────────────────────────│                                  │
   │ ⑦ 你 review diff + commit / push                                │
   ├──────────⇄───────────────────│                                  │
```

幾個關鍵看點：

- **你只在 ①、⑥、⑦ 三個 step 親自參與** （提需求、看回報、review diff + commit），中間 ② → ⑤ 都是 Leader 在 orchestrate
- **理想狀態下 Leader 偏向 orchestrator 角色** — 拆解、分派、收回報，把實作切給員工。實務上主 agent 還是常常會自己接手寫 code，除非你明確設計 delegation 流程（例如 prompt 內直接寫請呼叫 X sub-agent 處理），不要假設它會自動 route 出去
- **⚠ step 是 Anthropic 官方推薦的 [interview pattern](https://code.claude.com/docs/en/best-practices#let-claude-interview-you)** — 需求模糊時 Leader 主動用 `AskUserQuestion` 反問你，避免下游做錯方向
- **④ Code Review 一定要 fresh context** — 由另一個 session 的 Claude or sub-agent 來做，避免 self-confirmation bias
- **⑤ 驗收測試的依據是 spec** （CLAUDE.md / AGENTS.md），不是 Leader 自己腦補的標準

每個 step 對應到後面章節的細節：① ② 在 **一、規劃階段** 、③ 在 **二、員工分工** + **三、開發紀律** 、④ ⑤ 在 **四、收尾驗證** 、⑦ 在 **三、開發紀律** 的 commit 切點。

如果中間 Leader 走偏了，回到 **五、失誤救援** 的回滾決策表；雷被抓到之後寫進 **六、Memory 系統** 變長期 rule。

## 一、規劃階段：Spec → Plan → Tasks 三層落檔

Agentic engineering 的第一個原則就是 **規劃要文件化** 。業界主流框架例如 GitHub 在 2025 年釋出的 [Spec Kit](https://github.com/github/spec-kit) （Specify → Plan → Tasks → Implement 四階段，每階段都會產出 `spec.md` / `plan.md` / `tasks.md` 這類檔案）、Anthropic 官方的 [Claude Code best practices](https://code.claude.com/docs/en/best-practices) （Explore → Plan → Implement → Commit 四階段，plan mode 可以按 `Ctrl+G` 把 plan 打開到編輯器直接編輯落檔），核心精神都一致 — plan 跟 spec 應該寫成可讀可改的檔案，而不是只活在對話裡。

具體要分三層：

1. **Spec（規格）** ：這個專案在做什麼、給誰用、邊界在哪
2. **Plan（計畫）** ：要做哪些 feature、什麼時間做完
3. **Tasks（任務）** ：當前任務拆成哪些步驟、做到哪了

三層各自落檔，每層解決一個 context 問題。

### 1.1 Spec：寫一份 CLAUDE.md（搭配 AGENTS.md）當專案憲法

開工前要先搞清楚一件事： **不同 AI Coding 工具讀的設定檔不一樣** 。

- **Claude Code 只讀 `CLAUDE.md` ，不讀 `AGENTS.md`** （ [Anthropic 官方文件](https://code.claude.com/docs/en/memory) 明確說「Claude Code reads `CLAUDE.md`, not `AGENTS.md` 」）
- **其他工具（Cursor、Codex、Windsurf、Aider 等）讀 `AGENTS.md`**

#### AGENTS.md 是什麼？

[AGENTS.md](https://agents.md/) 是 OpenAI 在 2025 年 8 月主導推出的開放標準，跟 Amp、Google Jules、Cursor、Factory 等多家 AI 工具廠商合作發展，2025 年 12 月由 OpenAI 捐給 [Linux Foundation 旗下的 Agentic AI Foundation（AAIF）](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) ，跟 Anthropic 捐的 MCP（Model Context Protocol）、Block 捐的 Goose 並列為 AAIF 三大創始項目。截至 2026 年 5 月，全球已經有超過 6 萬個開源專案在用它，支援 Amp、Codex、Cursor、Devin、Factory、Gemini CLI、GitHub Copilot、Jules、VS Code 等 20+ 個 AI Coding 工具。

#### 該怎麼配置？

最實用的做法是 **兩個檔案都寫，但內容不重複** ：

- `AGENTS.md` 放專案規範本體（給跨工具團隊共用）
- `CLAUDE.md` 第一行 `@AGENTS.md` 把 AGENTS 內容 import 進來，下面再加 Claude Code 專屬規則 + `@` import ROADMAP / tasks

或者更省事的做法是 `ln -s AGENTS.md CLAUDE.md` 建立 symlink，內容一份兩個檔名共用（Windows 需要 Administrator 權限 or Developer Mode，沒開的話用 `@AGENTS.md` import 比較保險）。

如果你只用 Claude Code 不在意跨工具，直接寫 `CLAUDE.md` 即可，AGENTS.md 不用寫。

#### CLAUDE.md 範本

直接 copy 這個結構到你的專案根目錄 `CLAUDE.md` ：

```markdown
# 專案名 - Claude Code Instructions

@AGENTS.md

## 開工前必讀
@docs/ROADMAP.md
@docs/tasks.md

## Project Overview
（一兩句話：這個專案是什麼、給誰用）

## Build & Test
- 安裝：pnpm install
- 開發：pnpm dev
- 測試：pnpm test（全綠才能 commit）
- 打包：pnpm build
- 型別檢查：pnpm typecheck

## Code Style
- 使用 ES modules（import/export），不用 CommonJS
- 命名：composable 用 useXxx、service 用 xxxService、純函式用 camelCase
- 縮排：2 空格
- 引號：單引號

## Testing Rules
- 每次改 code 同一輪就把測試補上，不可拖到下一個 task
- 測試檔命名：xxx.test.ts，跟實作放同一個資料夾
- 純函式優先抽出來測（比 mock 容易）

## Git Workflow
- commit message 用 Conventional Commits（feat / fix / test / refactor / docs / chore）
- 一個邏輯改動一個 commit，不要堆
- 主分支：main；功能分支：feature/xxx

## Security
- 不要把 API key、token、密碼寫進 code
- .env 檔案不可 commit
- 敏感檔案：.env*、*.key、credentials.json

## 明確排除的事
- 不引入 [某類功能]，原因：[原因]
- 不使用 [某個技術]，原因：[原因]
```

> **Note**  
> 上面範本第 3 行 `@AGENTS.md` 是雙檔架構用的。 **如果你只寫 CLAUDE.md 不寫 AGENTS.md，把這行刪掉即可** （否則 Claude Code 會抱怨找不到 AGENTS.md）。  
> 反過來如果走雙檔架構：AGENTS.md 的內容跟上面 CLAUDE.md 範本一樣， **只是不需要 `@AGENTS.md` 那行** （因為它本身就是被 import 的目標）；而且 `## 開工前必讀` 區塊建議只放在 CLAUDE.md 裡，因為 `@import` 是 Anthropic 對 CLAUDE.md 的擴充、其他工具實作 AGENTS.md 不一定支援。

#### @import 機制（這是 Claude Code 的關鍵）

注意上面範本第一個區塊 `## 開工前必讀` 裡的 `@<path>` 語法 — 這是 **Anthropic 對 CLAUDE.md 的擴充語法** （ [官方文件確認](https://code.claude.com/docs/en/memory) ），AI 啟動時讀到 `@<path>` 會自動把那個檔案的內容也載進 context，遞迴 **最多 5 hops 深** ，relative path 跟 absolute path（例如 `@~/.claude/personal-rules.md` ）都支援。

這個機制超關鍵，因為 Claude Code 啟動時 **預設只會自動載入 CLAUDE.md 本身** （以及 ancestor 路徑的 CLAUDE.md）， `docs/` 底下的 ROADMAP.md、tasks.md 不會被自動讀到。如果你只寫 ROADMAP 沒在 CLAUDE.md 裡 `@` 引用，AI 開新 session 進來根本看不到你的計畫，等於白寫。

> **Note**  
> `@<path>` 是 Anthropic 對 CLAUDE.md 的擴充， **AGENTS.md 規格本身沒有規定 import 語法** ，其他工具實作 AGENTS.md 不一定支援 `@import` 。所以跨工具情境，重要規範建議直接 inline 寫進 AGENTS.md 比較保險，不要假設其他工具會跟著解析 `@import` 。Claude Code 這邊照常透過 `@AGENTS.md` 把整份內容拉進來、再透過 `@docs/ROADMAP.md` 把計畫接上。

### 1.2 Plan：寫一份 ROADMAP.md 列功能跟階段

CLAUDE.md（搭配 AGENTS.md）是專案憲法、不太會變；ROADMAP.md 是這個 sprint or 這個月要做什麼 feature 的計畫，會隨開發推進更新。

#### ROADMAP.md 範本

放在 `docs/ROADMAP.md` ：

```markdown
# 專案名 ROADMAP

## 願景
（一兩句話：這個專案要解決什麼問題、給誰用）

## Week 1：MVP
- [ ] 功能 A：[簡述行為跟驗收條件]
- [ ] 功能 B：[同上]

## Week 2：補強
- [ ] 功能 C：[同上]
- [ ] 功能 D：[同上]

## 待評估
- 功能 X：可能做、可能不做，看 Week 1 完成度
- 功能 Y：同上

## 明確排除的事
- 功能 Z：因為 [原因]
```

`## 明確排除的事` 這一塊很重要，AI 容易自己腦補功能、越界做事，把不做的事明文列出來可以幫你卡住範圍。

### 1.3 Tasks：寫一份 tasks.md 同步當前進度

第三層是當前任務的進度追蹤。AI 內建有待辦清單工具（例如 Claude Code 的 todo 系統），可是這個本質上活在 context 裡，跑 `/compact` 、關掉 session 重開、context 一爆掉，這份清單就跟著消失了。所以更穩的做法是同時請 AI 把進度寫到實體檔案。

#### tasks.md 範本

放在 `docs/tasks.md` ：

```markdown
# 本次任務：[ 任務簡述 ]

## 待辦清單
- [x] 1. 讀懂需求、plan 已落到 docs/ROADMAP.md
- [x] 2. 拆解子任務
- [ ] 3. 實作 A
  - [ ] 3-1. 寫 A.composable
  - [ ] 3-2. 同步寫 A.composable.test
- [ ] 4. 實作 B
  - [ ] 4-1. 寫 B.service
  - [ ] 4-2. 同步寫 B.service.test
- [ ] 5. 跑 typecheck + test + build 三段驗證
- [ ] 6. commit + push

## 進行中
3-1：寫 A.composable，目前卡在 [描述]

## 完成紀錄
- 2026-XX-XX：plan 確認、tasks 拆完
```

開工前直接跟 AI 說：先列待辦清單、同步寫到 `docs/tasks.md` ，每完成一項就更新檔案。這樣即使隔天重開新 session，AI 一進專案讀檔案就接得回去。

## 二、員工分工：你的三種 AI 員工配置

Karpathy 在演講裡有強調 agentic engineering 的 agentic 二字 — 在 2026 年的現在，你不再是親手寫 code 的工程師，而是 **管理一群 AI 員工的監督者** 。Claude Code 在 2025-2026 演進出三種員工配置，每一種解決不同的問題。

### 2.1 Skill：注入主 agent 的標準流程包

Skill 是放在 `.claude/skills/<name>/SKILL.md` 的 markdown 指令包，當 Claude 偵測到任務匹配 skill 的描述就會自動 load 進主 context。Skill 跟主 agent **共用 context、共用模型** ，適合重複會做的標準動作。

#### Skill 範本（API 慣例）

`.claude/skills/api-conventions/SKILL.md` ：

```markdown
---
name: api-conventions
description: REST API design conventions for this project
---

# API Conventions

- URL 路徑用 kebab-case：/api/user-profiles
- JSON property 用 camelCase：userId, createdAt
- list endpoint 一律支援分頁：?page=1&pageSize=20
- API 版本放在 URL 路徑：/v1/、/v2/
- 錯誤回應格式：{ code, message, details? }
```

> **Note**  
> 想了解 Skill 完整機制，可以看我之前寫的 [比 MCP 更省 Token 的 Agent Skills](https://israynotarray.com/ai/20260120/4189755271/) 。

### 2.2 Sub-agent：獨立 context、獨立權限的專職員工

Sub-agent 跟 Skill 最大的差別是：sub-agent 有 **獨立的 context window、獨立的 system prompt、獨立的 tool 權限，甚至可以跑不同的模型** 。Claude 主 agent 把任務派給 sub-agent 之後，sub-agent 在自己的 context 裡做事，做完只回傳摘要給主 agent，主 agent 的 context 完全不會被汙染。

放在 `.claude/agents/<name>.md` ：

#### Sub-agent 範本（security-reviewer）

`.claude/agents/security-reviewer.md` ：

```markdown
---
name: security-reviewer
description: 替新寫的 code 做資安 review
tools: Read, Grep, Glob, Bash
model: opus
---

你是資深資安工程師。Review code 時關注：
- Injection 漏洞（SQL、XSS、command injection）
- 認證跟授權瑕疵
- code 裡的 secrets / credentials
- 敏感資料處理

請給出具體的行號跟修法建議。
```

#### Sub-agent 範本（test-writer）

`.claude/agents/test-writer.md` ：

```markdown
---
name: test-writer
description: 替剛寫好的 composable / service 補對應的單元測試
tools: Read, Edit, Bash
model: sonnet
---

你是專門寫單元測試的 AI engineer。

收到任務時：
1. 先讀目標檔案、理解函式 / composable 的行為
2. 列出 happy path + edge case + error case 三類測試
3. 用 Vitest 寫成 .test.ts 檔案
4. 跑 pnpm test 確認全綠
5. 若有 case 沒過，回報失敗 case 給主 agent 決定怎麼處理
```

#### Sub-agent 範本（ui-engineer）

`.claude/agents/ui-engineer.md` ：

```markdown
---
name: ui-engineer
description: 處理 Tailwind class、元件樣式、design system 相關修改
tools: Read, Edit, Glob
model: sonnet
---

你是專門處理 UI/UX 的 frontend engineer。修改畫面樣式時遵守：

- filled input 不要再加 border
- focus ring 走 design system token，不要 hardcode 顏色
- touch target 至少 44px（包含按鈕、icon button、可點區塊）
- 間距用 design system 級距（mt-2 / mt-4 / mt-6 / mt-8），不要亂塞 mt-5 mb-3 沒邏輯的數字
- 顏色用 design system color token，不要 hardcode hex 色碼
- 不要用 !important 覆蓋既有樣式 — 找到根本原因改正確的層級
- RWD 優先寫 mobile，再用 md:、lg: 加大版斷點

收到任務時：
1. 先讀目標元件、確認既有 pattern 跟 token 用法
2. 修改 Tailwind class（不寫自訂 CSS 除非真的必要）
3. 改完後對照範例畫面 / Figma（如果有的話）確認視覺一致
```

#### 什麼時候該選 Skill、什麼時候該選 Sub-agent？

| 情境 | 選 Skill | 選 Sub-agent |
| --- | --- | --- |
| 重複會做的標準動作（API 慣例、commit 規範） | ✓ |  |
| 需要獨立 context 不汙染主對話（深度搜尋、code review） |  | ✓ |
| 要跑不同模型省錢（主 Sonnet + sub Haiku） |  | ✓ |
| 要限制工具權限（reviewer 只能讀不能寫） |  | ✓ |
| 想要 AI 自動偵測就用、不要手動觸發 | ✓ |  |
| 多步驟、有決策點的 workflow |  | ✓ |

### 2.3 Agent Teams：跨 session 平行的進階模式

Agent Teams 是 2026 年新出的進階模式 — 多個 Claude 跑在獨立 worktree、透過 git 協調工作，每個 agent 認領自己的任務、自己 merge、自己解衝突。實務經驗大致是 3-5 個 parallel agents 算是 sweet spot，超過 10 個之後成本提高但效益不一定增加。

這個模式對新手過於複雜，可以先掌握 Skill + Sub-agent 兩種就好，等到專案大到一個人 + 一個主 Claude 跑不動的時候再考慮。

## 三、開發紀律：三條每天都要堅持的鐵則

進入實作階段之後，三條紀律是每天都要堅持的，每一條都是直接接住 AI 失誤的網。

### 3.1 改 code 同一輪對話就把測試補上

這條規則最常拿出來提醒 AI：每次改 code 都要同一輪對話就把測試補上，不要拖到下次再說「再開一個 task 統一補」。

為什麼這麼堅持？因為 AI 開發有個很容易踩的雷，就是改 A 壞 B。你今天叫它調整某個 composable 的邏輯，它跑出來的 code 表面上看起來都對，可是其實已經 side effect 改到別的地方了，你眼睛看不出來。而測試就是接住這種 side effect 的網。

那為什麼要強調同一輪呢？因為事後補的測試很容易變成為了讓 code pass 的測試，而不是真的在驗證行為的測試。同一輪補的時候，AI 寫實作跟寫測試的上下文是連在一起的，需求、邏輯、case 三件事都還活著；如果你拖到下一個 task 才補，那時候上下文已經漂移，AI 補出來的 case 通常只能蓋到表面，蓋不到真正的 edge case。

#### 補測試提示範本

開工前 / 每次新功能前直接跟 AI 說：

```
這個任務每改一個檔案，同一輪就把對應的測試補上來。
補測試要列為跟實作並列的子任務（不是放在實作下面的 sub-task），
這樣才不會做完實作就漏掉。
最後跑 pnpm test 確認全綠才算這個 task 結束。
```

### 3.2 動 UI 一律切給專門的 Skill 或 Sub-agent

只要這次修改牽涉到 UI/UX（畫面排版、元件樣式、Tailwind class 調整等等），都直接呼叫專門處理 UI 的員工去做，而不是讓主 agent 自己亂改。

為什麼這樣分？

簡單來講就是術業有專攻。主 agent 在做架構決策、寫邏輯的時候很強，可是一旦動到 UI/UX，它常常會犯一些通病：例如用一堆 `!important` 覆蓋既有樣式、配色完全沒有 design system 概念、間距亂塞 `mt-5 mb-3` 沒邏輯。

UI 相關的員工通常會內建 design system 知識、配色規範、可用性原則，它知道 filled input 不要再加 border、focus ring 要走 token、touch target 至少 44px 這些常識。

#### UI 切員工提示範本

```
這次要動 [元件名 / 畫面區塊]，目標是 [描述要達成的效果]。
請呼叫 UI 專門的員工處理（前面 2.2 的 ui-engineer sub-agent，
or 如果有裝 plugin 例如 ui-ux-pro-max 也可以呼叫那個）。
主 agent 只負責決定動哪個元件、想要什麼方向，
實際改 Tailwind class 那一刻交給專門員工接手。
```

### 3.3 一個邏輯改動就一個 commit，不要堆成大塊

第三條是 commit 紀律。AI 如果沒有明確規則，常常會一次改十幾個檔案塞進一個 commit，等到後面某一段需要 revert 的時候，發現整包綁在一起，根本沒辦法乾淨地拆出來。

所以規則一定要明文化： **一個邏輯改動 = 一個 commit** 。例如：

- 補 A 元件的測試 → 一個 commit
- 修 B 路由的 bug → 另一個 commit
- 重構 C 共用邏輯 → 另一個 commit

這樣未來要 revert 任何一段都不會牽動其他工作，搭配第五段會講的回滾策略，補救成本可以壓到最低。

#### commit message 範本（Conventional Commits）

```
feat: 新增 A 功能
fix: 修 B 元件的 router bug
test: 補 C composable 的測試
refactor: 抽出 D 共用邏輯
docs: 更新 ROADMAP.md
chore: 升級套件版本
```

#### AI commit 切點提示範本

開工前跟 AI 說：

```
commit 切點規則：一個邏輯改動就一個 commit。
- 不要把實作 + 測試 + 修 bug 堆在同一個 commit
- 補測試跟實作要拆開 commit（test: 跟 feat: 各一個）
- 重構獨立 commit、不混進其他改動
commit message 用 Conventional Commits 格式
（feat / fix / test / refactor / docs / chore）。
```

## 四、收尾驗證：三層安全網接住 AI 失誤

寫完 code 不等於做完，收尾階段有三層安全網要過，每一層接的失誤類型都不一樣。

### 4.1 第一層：typecheck + test + build 三段驗證

每次任務告一段落要交付的時候，固定請 AI 跑三段驗證：

1. `pnpm typecheck` （or `tsc --noEmit` ）— 確認型別都過得了
2. `pnpm test` — 確認既有測試全綠
3. `pnpm build` — 確認可以打包出乾淨的產物

這三段抓的是不同層的問題：

- **typecheck** 抓型別錯誤，例如 AI 把 `User` 物件傳到一個吃 `string` 的 function
- **test** 抓行為錯誤，例如 AI 改了一個 function 結果該回傳 true 的 case 變成 false
- **build** 抓打包之後才會爆的錯誤，例如某個 import 路徑 dev mode 跑得起來、production build 卻爆掉

只跑其中一段是不夠的，這三段加在一起才是相對完整的安全網。

#### 交付前 checklist 範本

```
- [ ] pnpm typecheck（or tsc --noEmit）通過
- [ ] pnpm test 全綠
- [ ] pnpm build 成功
- [ ] 自己跑一次實際功能、用瀏覽器戳一下畫面
- [ ] git status 沒有意外的檔案改動
- [ ] commit message 符合 Conventional Commits 格式
```

### 4.2 第二層：Writer / Reviewer 雙 Claude 模式

這是 Anthropic 官方在 best practices 推薦的進階工法。簡單講就是 **開兩個 Claude session，一個負責寫、一個負責 review** ，因為 Claude review 自己剛寫的 code 會有 confirmation bias、不容易抓到問題；換一個 fresh context 的 Claude 來 review，能抓到更多 edge case。

實作方式：

| Session A（Writer） | Session B（Reviewer） |
| --- | --- |
| `實作 A 元件的 rate limiter middleware` |  |
|  | `Review @src/middleware/rateLimiter.ts。找出 edge case、race condition、跟既有 middleware pattern 的不一致處` |
| `根據以上 review 意見修正：[Session B 輸出]` |  |

更進階的做法是把 Reviewer 設成一個 sub-agent（像前面 2.2 的 security-reviewer），這樣每次寫完都可以一鍵呼叫 review，不用手動開新 session。

### 4.3 第三層：AI 引用的版本號、套件名、API 自己去官方對一次

第三層安全網是抗 AI 過期記憶用的：每次 AI 在寫 code 時引用了套件版本、API 用法、雲端服務後台這類更新很快的東西，都自己打開官方文件對照一遍，不要直接讓 AI 的記憶 ship 進專案。

為什麼這麼龜毛？因為 AI 的 training data 是有截止日期的，特別是套件版本、API 規格、雲端後台 UI 這些東西更新非常快，AI 用記憶答出來的常常已經是好幾個月前的版本了。例如它可能寫某個套件的 API，可是其實在新版裡早就改名了；或者它告訴你某個雲端服務在後台點 X 設定，可是後台介面早改版了。

#### AI 引用必查清單

```
看到 AI 寫的 code 出現以下引用，自己打開官方文件對一次：

[必查]
- 套件 API 用法（特別是 major 版本可能改過）
- 雲端服務後台 UI / 設定步驟（AWS、GCP、Cloudflare、Firebase console 等）
- 環境變數 / 設定檔欄位名
- 框架 / SDK 版本對應的語法（Vue 3 vs Vue 2、Nuxt 3 vs 2 等）
- 第三方服務 API 規格

[不用查]
- 純語法（JS 內建陣列方法、字串處理等）
- 純邏輯（演算法、純函式）
- 你自己專案內的 module 跟 type
```

## 五、失誤救援：回滾決策表

實戰中一定會遇到 AI 改壞 code 的時候，這時候最浪費時間的反應是在錯的方向上繼續加東西想 fix — 越救越大坑。所以一定要有明確的回滾判斷尺。

Claude Code 在 2026 年加了一個叫 `/rewind` 的功能（or `Esc + Esc` ），可以回到任一個 checkpoint，連 Claude 改過的 code state 都會還原。這個是第一層救援工具。

> **Note**  
> 一個重要 caveat — Anthropic 官方明確說 checkpoint **只追蹤 Claude 改過的檔案，不追蹤 Bash command 造成的檔案變更** （例如 `npm install` 改的 node\_modules、 `rm` 刪掉的東西、build 產出的檔案）。這部分要靠 git 才能 recover，所以 git commit 紀律不能省。

判斷尺一句話： **壞掉的是執行細節？還是規劃方向？**

### 回滾決策表

| 情境 | 動作 |
| --- | --- |
| AI 剛剛改的東西 review 完不對 | `/rewind` （or `Esc + Esc` ）回上個 checkpoint |
| 單一檔案實作細節錯，整體方向 OK | `git restore <檔案>` 重做 |
| 多個相關檔案壞掉，方向 OK | `git restore` 多個檔案、重做 |
| AI 套錯架構 / 誤解需求 | 砍 plan 重寫，從規劃階段重來 |
| 補的測試蓋不到實作意圖 | 砍 test、重看 plan、重補測試 |
| AI 一次改太多看不出哪邊是好的 | 先 `git status` 確認 + `git stash` 保險，再 `git restore .` 全部回滾（這就是為什麼前面強調小 commit） |
| 跑 typecheck / test 大規模爆掉 | 高機率是方向錯，先停手檢查 plan |

從這張表可以看出來，回滾不是失敗、是流程的一部分，把它當成抗 AI 失誤的工具來用，整個開發節奏會穩很多。

## 六、Memory 系統：兩層架構 + 正向 pattern 範本

最後一個環節是 memory 系統的設計，這個環節做好之後，前面所有的紀律都會變成 AI 主動會做的事，不用每次都手動提醒。

memory 要分兩層設計：

### 6.1 全域層：用 ~/.claude/CLAUDE.md 或 ~/.claude/rules/ 放跨專案通用規則

跨專案會通用、你明確想讓 Claude 每次都遵守的規則（套件管理慣例、補測試紀律、不過度設計、不擅自擴大範圍等），按 [Anthropic 官方文件](https://code.claude.com/docs/en/memory) 有兩個位置可以放：

- **`~/.claude/CLAUDE.md`** — 整份單一檔案、每次 session 自動載入，適合放 5-10 條核心原則
- **`~/.claude/rules/<topic>.md`** — 按主題拆檔案（一條 rule 一個檔），也是每次 session 自動載入，好處是可以用 YAML frontmatter `paths:` 欄位做 path-scoped rules（例如某條 rule 只在處理 `src/api/**` 時才載入）

新專案直接共用不用重教。

> **Note**  
> 順帶釐清一個常見誤會 — Claude Code 還有一個叫 **Auto memory** 的功能，是 Claude 自己累積的（不是你手寫的），存在 `~/.claude/projects/<project>/memory/` ， **按專案分而不是全域** 。本章談的「全域層規則」指的是你手動寫給 AI 看的規範，跟 Auto memory 是不同的東西。Auto memory 完整機制可以看我之前寫的 [Claude Code Auto Memory 是什麼？](https://israynotarray.com/ai/20260330/2896397295/) 。

#### 全域規則範本（pnpm 慣例）

`~/.claude/rules/pnpm.md` ：

```markdown
---
name: 套件管理用 pnpm
description: JS/TS 專案一律用 pnpm 不要用 npm
type: feedback
---

JS/TS 專案預設用 pnpm，不要用 npm。

**Why:** lock file 慣例統一、硬碟空間節省、monorepo 友善。

**How to apply:** 看到 package.json 預設下 pnpm install / pnpm run xxx。
如果專案有 package-lock.json 但沒有 pnpm-lock.yaml，
先問使用者要不要遷移到 pnpm。
```

#### 全域規則範本（補測試紀律）

`~/.claude/rules/testing.md` ：

```markdown
---
name: 改 code 同一輪就補測試
description: 每次程式異動都要在同一輪對話補上對應的測試
type: feedback
---

每一次新增或修改程式碼，都必須在同一次工作中補上對應的測試。

**Why:** 事後補的測試容易變成「為了 pass 而 pass」，蓋不到 edge case。

**How to apply:**
- 寫新 composable / service / 純函式時，同一次任務內加 *.test.ts
- 改既有檔案造成行為變化時，擴充既有測試 or 新增測試
- 純函式優先抽出來測（比 mock 容易得多）
- 跑 pnpm test 全綠才算完成
- 補測試列為跟實作並列的子任務
```

#### 全域規則範本（嚴守交付範圍）

`~/.claude/rules/minimal-scope.md` ：

```markdown
---
name: 嚴守指定交付範圍
description: 使用者說只要 X 時不要順手附帶其他檔案
type: feedback
---

使用者交付任務時明確說只改 X 就只改 X，不要順手改其他檔案。

**Why:** 擴大範圍會引入未授權的變更，使用者花更多時間 review，
甚至引入 regression。

**How to apply:**
- 每次任務開頭明確列出範圍
- 超出範圍的發現先記下來再問，不要先動
```

### 6.2 專案 AGENTS.md / CLAUDE.md 放專案特定 rule

專案層放這個專案才用得到的設計決策、產品邊界、踩過的雷。判斷尺： **這條 rule 換到別的專案還會用嗎？** 會用就放全域、只在這專案用就放專案層。

如果你想了解 Claude Code memory 完整機制，可以參考我之前寫的 [Claude Code Auto Memory 是什麼？自動記憶功能完整教學](https://israynotarray.com/ai/20260330/2896397295/) 。

### 6.3 寫 rule 的時候，正向 pattern 比禁令式更有用

最後一個關鍵：寫 rule 的時候要刻意混搭禁令式跟正向 pattern，光寫禁令是不夠的。

| 類型 | 例子 | 效果 |
| --- | --- | --- |
| 禁令式 | 不要列大綱開場、不要用 emoji、不要過度設計 | AI 會避開，可是不知道對的長怎樣 |
| 正向 pattern | 成功的 plan 文件應該包含目標、任務拆解、不做的事、驗收條件四塊 | AI 知道要主動產出什麼 |

從這張表可以看出來，禁令式只能擋住雷、無法主動引導正確的產出，所以要搭配正向 pattern 一起用。

#### 正向 pattern 範本

`~/.claude/rules/plan-template.md` ：

```markdown
---
name: 成功 plan 文件的範本
description: AI 寫 plan 文件時應該參考的正向結構
type: feedback
---

寫 plan 文件時，應該包含以下四塊：

1. **目標**：一兩句話寫清楚要解決什麼問題、給誰用
2. **任務拆解**：每一個子任務獨立列出來，含驗收條件
3. **不做的事**：明確排除（很重要，AI 容易越界）
4. **開發紀律備忘**：套件管理、測試、commit 切點等

範例參考專案的 docs/ROADMAP.md。

**Why:** 禁令式 rule 只能擋住雷，正向 pattern 才能引導出對的東西。

**How to apply:** 收到寫 plan、規劃功能、整理 ROADMAP 這類任務時，
先讀過這份結構再動筆。
```

兩種一起用最有效：禁令防雷、正向引導。

## 實戰案例：個人電商 MVP 從 Spec 到 Ship

講完六大環節，最後用一個比較貼近真實工作的案例走一遍完整流程： **做一個個人小型電商 MVP** ，賣自製商品、要能收得到錢。

技術棧用 Nuxt 3 + Vue 3 + Tailwind + Pinia + Supabase（DB + Auth）+ Stripe（付款）+ Vercel（hosting）。這個 stack 是 2026 年小型電商 MVP 滿主流的組合 — 後端不用自己架、付款交給 Stripe、hosting 一鍵部署。

### Step 1 — 寫 CLAUDE.md（搭配 AGENTS.md）當專案憲法

開新專案第一件事是寫 CLAUDE.md（如果跨工具團隊也同步寫一份 AGENTS.md，CLAUDE.md 第一行 `@AGENTS.md` import 進來）。電商比一般工具多了 **金錢 / 安全 / RLS** 這幾個重點區塊，所以憲法裡要明文寫清楚：

```markdown
# RaySelf Shop - Claude Code Instructions

@AGENTS.md

## 開工前必讀
@docs/ROADMAP.md
@docs/tasks.md

## Project Overview
個人賣自製商品的小型電商 MVP，目標 1 個月內上線收得到錢。

## Tech Stack
- Frontend: Nuxt 3, Vue 3, Tailwind, Pinia
- Backend: Supabase（PostgreSQL + Auth + Storage）
- Payment: Stripe Checkout（Redirect 模式）
- Hosting: Vercel
- Package Manager: pnpm

## Build & Test
- 安裝：pnpm install
- 開發：pnpm dev
- 測試：pnpm test
- 型別檢查：pnpm typecheck
- 打包：pnpm build

## Code Style
- Vue 3 Composition API + script setup
- composable 用 useXxx 命名（useCart、useProducts、useCheckout）
- API 呼叫抽到 composables/api/*.ts
- 純函式（價格計算、運費計算）抽到 utils/
- server route 放 server/api/

## Testing Rules
- 純函式（價格計算、運費、稅金、折扣）100% 覆蓋
- API composable 至少測 happy path + 1 個 error case
- 同一輪補測試，不可延後

## Git Workflow
- commit message 用 Conventional Commits
- 一個邏輯改動一個 commit

## Security（電商核心、不可妥協）
- **金額一律以 server-side Supabase 查為準，絕對不信任前端傳來的價格**
- Stripe secret key 只在 server route 用，前端不可碰
- Supabase RLS 一律啟用，所有 table 設定權限規則
- Webhook signature 一定要驗證（防止偽造訂單）
- .env 不可 commit；.env.example 只放空白範本

## 明確排除的事
- 不做使用者註冊登入（Week 1-3 範圍外，用 Stripe Checkout 的 email 收單）
- 不做商品上傳後台（先用 Supabase Studio 手動上架）
- 不做多商品變體（size / color）
- 不做退款流程
- 不做多語言
```

### Step 2 — 寫 ROADMAP.md 切三個 Week

`docs/ROADMAP.md` ：

```markdown
# RaySelf Shop ROADMAP

## 願景
讓個人創作者能用最低成本上線一個收得到錢的小型電商。

## Week 1：商品展示（公開頁面）
- [ ] Supabase schema：products / orders / order_items 三張表 + RLS policy
- [ ] 商品列表頁 \`/\`：抓 products、顯示卡片
- [ ] 商品詳情頁 \`/products/[slug]\`：完整圖文 + 加入購物車
- [ ] 購物車 store（Pinia + localStorage 同步）

## Week 2：結帳 + 訂單寫入
- [ ] server route \`/api/checkout\` 建立 Stripe Checkout Session
- [ ] 結帳流程串接：購物車 → Stripe Redirect
- [ ] server route \`/api/stripe-webhook\` 收 webhook、寫單到 Supabase
- [ ] 訂單成功頁 \`/success\`、取消頁 \`/cancel\`

## Week 3：簡易後台
- [ ] 後台訂單列表頁（用環境變數鎖訪問 or basic auth）
- [ ] 訂單狀態手動更新（pending / shipped / completed）

## 明確排除的事
- 使用者註冊登入：之後再做
- 商品上傳後台：Week 1-3 用 Supabase Studio 手動上架
- 退款流程：之後再做
```

### Step 3 — 開工前進 plan mode 跟 AI 討論

進 Claude Code，先進 plan mode：

```
讀 CLAUDE.md（裡面已經 @import AGENTS.md 跟 docs/ROADMAP.md），了解專案脈絡。
我要開始做 Week 1 商品展示，請列出檔案結構、主要 composable / 純函式、
Supabase schema 跟 RLS policy 草稿。
```

AI 大概會回類似這樣的計畫：

- Supabase schema：
	- `products` ：id, slug, name, description, price\_cents, image\_url, stock
		- `orders` ：id, stripe\_session\_id, customer\_email, total\_cents, status, created\_at
		- `order_items` ：id, order\_id, product\_id, quantity, price\_cents\_at\_purchase
- 程式碼結構：
	- `composables/api/useProducts.ts` — products fetch
		- `composables/useCart.ts` — Pinia store + localStorage 同步
		- `utils/cart.ts` — 純函式（小計、運費、總額計算）
		- `pages/index.vue` 、 `pages/products/[slug].vue`
		- 對應 test 檔
- RLS policy：products 公開 read、orders 只有 server-side service\_role 能 write

確認 plan 之後請 AI 寫進 `docs/tasks.md` ，然後離開 plan mode 開始實作。

### Step 4 — 實作 + 同一輪補測試

開工前先 prompt：

```
每改一個檔案，同一輪就把對應的測試補上。
補測試列為跟實作並列的子任務。
完成一項就同步更新 docs/tasks.md。
```

AI 會：

1. 寫 `utils/cart.ts` （純函式：計算小計、總額）→ 同 turn 寫 `utils/cart.test.ts` → 跑 pnpm test
2. 寫 `composables/api/useProducts.ts` → 同 turn 寫對應 test（mock Supabase client）
3. 寫 `composables/useCart.ts` → 同 turn 寫 test
4. 寫 `pages/index.vue` + `pages/products/[slug].vue` UI → 呼叫 ui-engineer sub-agent（前面 2.2 的範本）處理 Tailwind class

### Step 5 — 三段驗證 + Writer/Reviewer 抓安全議題

實作完跑：

```bash
pnpm typecheck  # 型別過
pnpm test       # 全綠
pnpm build      # 打包成功
```

然後 **這裡是電商案例最關鍵的一步** — 開另一個 Claude session 當 Reviewer， **特別 review 安全層** ：

```
你是 sub-agent security-reviewer。
請 review 這個 Week 2 的結帳流程：
- @server/api/checkout.post.ts
- @composables/useCheckout.ts
- @server/api/stripe-webhook.post.ts

重點關注：
1. 金額是否從 server-side Supabase 查（不可信任前端 price）
2. Stripe webhook signature 是否有驗證
3. Supabase RLS 是否擋住未授權的 orders write
4. 客戶 email 等敏感資料是否有正確處理
```

### Step 6 — 切 commit

照 Conventional Commits 一個邏輯一個 commit：

```
feat: Supabase schema + RLS policy（products / orders / order_items）
feat: 商品列表 API composable + 純函式 cart utils
test: 補 cart utils 跟 useProducts 的單元測試
feat: 商品列表頁 + 商品詳情頁 UI
feat: 購物車 Pinia store + localStorage 同步
test: 補 useCart 的單元測試
docs: 更新 ROADMAP，Week 1 商品展示完成
```

未來如果 Week 1 的某段需要 revert，每個 commit 都是獨立邏輯改動，可以乾淨地拆出來。

### Step 7 — 踩到雷：前端竄改價格的安全雷

Week 2 串完 Stripe Checkout 之後跑 Writer/Reviewer，Reviewer Claude 抓出一個關鍵問題：

> `server/api/checkout.post.ts` 直接把前端購物車裡的 `price_cents` 拿來建立 Stripe Session，這個是經典的 client-side trust 漏洞 — 使用者可以打開 DevTools 改 localStorage，把 10000 元商品改成 1 元再付款。

照回滾決策表判斷，這不是 fix 一行 code 就能解決的事：

| 情境 | 動作 |
| --- | --- |
| 不是個別欄位寫錯 | ✗ 不適用 git restore 單檔 |
| 是整個架構的信任邊界劃錯 | ✓ 砍 plan 重寫 Stripe 部分 |

所以正確動作是 **砍 plan 重寫 Stripe 結帳流程** ，新版的 plan：

1. `/api/checkout` 只收 `product_id` 跟 `quantity` ， **不收 price**
2. server-side 從 Supabase 查每個 product 的 `price_cents`
3. 用 server-side 算出來的金額建立 Stripe Checkout Session

修完之後， **把這條雷寫進 AGENTS.md 變成長期 rule** ：

```markdown
## Security
- 金額一律以 server-side Supabase 查為準，絕對不信任前端傳來的價格
- 任何涉及金錢計算的 endpoint，必須以資料庫查出來的 source-of-truth 為基準
- 開新的 server route 涉及金額時，先檢查這條 rule
```

同時把這條昇級到全域層的 `~/.claude/rules/` ，因為這條 rule 換到任何電商 / 訂閱 / 任何涉及金錢的專案都適用：

```markdown
# ~/.claude/rules/money-server-side.md

---
name: 金錢一律 server-side
description: 任何涉及金錢的計算必須以 server-side 資料為準，不可信任 client
type: feedback
---

涉及金錢的 endpoint（結帳、儲值、訂閱、退款等）：
client 只送識別 ID（product_id / plan_id），server-side 自己查價格、自己算總額。

**Why:** client-side 資料可以被 DevTools / localStorage 隨意竄改，
任何 client 傳來的金額都是不可信的。

**How to apply:**
- 收 client 的 endpoint signature 不可包含 price / amount / total 欄位
- server-side 一定要從 DB 查 source-of-truth 金額
- Stripe / 金流 API 的金額參數只能用 server-side 算出的結果
```

這樣同樣的雷大幅降低在 Week 3 後台 / 之後的退款流程 / 之後的訂閱機制再踩第二次的機率。這就是踩坑封 SOP 這個元 pattern 的完整示範： **從 review 抓到 → 砍 plan 重寫 → 寫進 AGENTS.md + 全域層 `~/.claude/rules/` → 大幅降低再次踩雷的機率** （不是百分百免疫，AI 偶爾還是會漏 rule，可是頻率會大幅降低）。

## 這套 SOP 的局限：什麼時候不要用、什麼時候會失靈

寫到這邊也要誠實講局限 — 這套 SOP 不是萬靈丹，跑過幾輪你會碰到下面這幾個天敵：

1. **小專案 overkill** ：週末做個 demo、寫個一次性腳本、PoC 試水溫，整套 CLAUDE.md + AGENTS.md + ROADMAP + tasks.md + sub-agent 配置維護起來太重，這時候直接 vibe coding 就好，前面那張對照表的 `適用情境` 欄已經有講
2. **Token 成本不便宜** ：Writer/Reviewer 等於每個 review 都跑兩次 Claude、sub-agent 每次叫起來都是 fresh context，跑 Opus + 重度使用很容易月費三位數美金跳起來。比較省的配置是重的任務（plan、實作）用 Opus、輕的任務（Reviewer、test-writer、ui-engineer 這類 sub-agent）切 Sonnet or Haiku
3. **Context 衰減是這套 SOP 最大的天敵** ：對話超過 100K+ token 之後 AI 開始漏 rule、忘記 CLAUDE.md 寫的紀律、tasks.md 也常常忘記同步。要養成定期 `/compact` + 該開新 session 就開的習慣，不要讓單一對話跑太久
4. **Sub-agent 不會自動穩定 routing** ：Claude 自動偵測這個任務該不該叫 ui-engineer 這個 routing 不是 100% 準，跑久了主 agent 會自己接手亂改 Tailwind。穩一點的做法是 prompt 內明確指定要呼叫的 sub-agent 名稱，不要全靠自動觸發
5. **memory 不是越多越好** ：累積到 20-30 條之後 AI 不一定每條都遵守，重要的 rule 跟次要的 rule 混在一起會稀釋。定期 prune、把過時或低優先的條目刪掉，比一直加新的有用

這幾個局限不是叫你別用，而是讓你照做之前有合理期待。整套 SOP 提高的是穩定性、不是保證穩定 — 真正穩的還是底層的 Git、測試、typecheck、build、人工 review，AI 只是讓你跑得更快。

## 結語：把這套變成你自己的 SOP

整理一下，這篇給你的是一套從 spec 到 ship 的完整 agentic engineering 實戰流程：

1. **規劃階段** ：CLAUDE.md（搭配 AGENTS.md）的專案憲法 + ROADMAP.md（功能計畫）+ tasks.md（進度同步）三層落檔，靠 `@<path>` import 串起來
2. **員工分工** ：Skill 注入主 context、Sub-agent 獨立 context、Agent Teams 跨 session 平行
3. **開發紀律** ：同一輪補測試 + UI 切員工 + 一個邏輯一個 commit
4. **收尾驗證** ：typecheck + test + build 三段 + Writer/Reviewer 雙 Claude + AI 引用 cross-check
5. **失誤救援** ：/rewind → git restore → 砍 plan 重寫 三層救援
6. **Memory 系統** ：全域 + 專案兩層架構 + 正向 pattern 範本

這六大環節背後其實有一個共同精神： **每一條都是抗 AI 失憶、抗方向漂移、抗 side effect 的紀律** 。Karpathy 講 agentic engineering 的精髓就是這個 — 你不再是親手寫 code 的工程師，而是監督一群 AI 員工的工程師，你的工作是設計 spec、監督 plan、檢查 diff、寫測試、做評估、管權限。

更重要的是，你開始跑這套流程之後，會逐漸踩到自己專案特有的雷。當你踩到的時候不要只是當下解決， **要把它寫進 memory or CLAUDE.md / AGENTS.md 變成下次的 rule** 。這樣同樣的雷不會踩第二次，每多踩一個就多固化一條流程，慢慢就會累積出一套真正屬於你自己的 agentic engineering SOP。

希望這篇對你建立自己的 agentic engineering 流程有幫助哩～如果你跑過幾輪之後發現自己的流程跟我不太一樣，那也很正常，畢竟每個團隊習慣本來就不同，可以挑你覺得有用的部分自己試試看，慢慢調出自己的版本就好。

你的支持會直接轉換成更多技術筆記

如果我的筆記讓你少踩一個坑、節省 Debug 的時間，  
也許你可以請我喝杯咖啡，讓我繼續當個不是 Array 的 Ray ☕

 [![buymeacoffee](https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)](https://www.buymeacoffee.com/israynotarray)| [![line](https://israynotarray.com/images/line-images.png) ](https://store.line.me/search/zh-Hant?q=israynotarray)| [![portaly](https://israynotarray.com/images/logo.b91a6dbf.webp)](https://portaly.cc/israynotarray/support)

Terminal

ray — -zsh — 80x24

Welcome to Ray Terminal v1.0.0

Type "help" to see available commands.

\------------------------------------

➜ ~