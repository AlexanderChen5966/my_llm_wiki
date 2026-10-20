---
title: "看這篇就懂：Claude Code 完全攻略指南"
source: "https://codelove.tw/@tony/post/qv2Ewq"
author:
published:
created: 2026-07-20
description: "[](#%E3%81%93%E3%82%8C%E3%82%92%E8%AA%AD%E3%82%81%E3%81%B0%E5%88%86%E3%81%8B%E3%82%8Bclaude-code-%E5%AE%8C%E5%85%A8%E6%94%BB%E7%95%A5%E3%82%AC%E3%82%A..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月31日](https://codelove.tw/@tony/post/qv2Ewq "2026-05-31 08:04:05")

## 讀了這篇就懂 Claude Code 完全攻略指南

> **適合讀者** ：想要開始使用 Claude Code 的人，以及已經在用、但想更深入活用的人  
> **前置知識** ：終端機操作基礎、Git 基本操作  
> **本文可學到的內容** ：從安裝到 [CLAUDE.md](http://claude.md/) 、自訂代理、MCP 活用，整理成一冊可在實務現場派上用場的使用技巧

---

## 目錄

1. [Claude Code 是什麼](#1-claude-code-%E6%98%AF%E4%BB%80%E9%BA%BC)
2. [安裝與設定](#2-%E5%AE%89%E8%A3%9D%E8%88%87%E8%A8%AD%E5%AE%9A)
3. [基本用法](#3-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
4. [上下文管理](#4-%E4%B8%8A%E4%B8%8B%E6%96%87%E7%AE%A1%E7%90%86)
5. [對話控制（快捷鍵・指令）](#5-%E5%B0%8D%E8%A9%B1%E6%8E%A7%E5%88%B6%E5%BF%AB%E6%8D%B7%E9%8D%B5%E6%8C%87%E4%BB%A4)
6. [Plan Mode 與 Thinking Mode](#6-plan-mode-%E8%88%87-thinking-mode)
7. [`.claude` 目錄的全貌](#7-claude-%E7%9B%AE%E9%8C%84%E7%9A%84%E5%85%A8%E8%B2%8C)
8. [CLAUDE.md 的撰寫技巧](#8-claudemd-%E7%9A%84%E6%92%B0%E5%AF%AB%E6%8A%80%E5%B7%A7)
9. [自訂代理・指令的活用](#9-%E8%87%AA%E8%A8%82%E4%BB%A3%E7%90%86%E6%8C%87%E4%BB%A4%E7%9A%84%E6%B4%BB%E7%94%A8)
10. [用 Skills 擴充功能](#10-%E7%94%A8-skills-%E6%93%B4%E5%85%85%E5%8A%9F%E8%83%BD)
11. [透過 MCP 串接外部工具](#11-%E9%80%8F%E9%81%8E-mcp-%E4%B8%B2%E6%8E%A5%E5%A4%96%E9%83%A8%E5%B7%A5%E5%85%B7)
12. [用 Hooks 自動化工作流程](#12-%E7%94%A8-hooks-%E8%87%AA%E5%8B%95%E5%8C%96%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B)
13. [GitHub 連動與 CI/CD 自動化](#13-github-%E9%80%A3%E5%8B%95%E8%88%87-cicd-%E8%87%AA%E5%8B%95%E5%8C%96)
14. [代理功能（並行處理）](#14-%E4%BB%A3%E7%90%86%E5%8A%9F%E8%83%BD%E4%B8%A6%E8%A1%8C%E8%99%95%E7%90%86)
15. [總結・下一步](#15-%E7%B8%BD%E7%B5%90%E4%B8%8B%E4%B8%80%E6%AD%A5)

---

## 1\. Claude Code 是什麼

Claude Code 是 Anthropic 提供的 **代理型程式開發助理** 。它不只是單純的程式碼補完工具，而是具備以下特點：

科技新聞

- **理解整個程式碼庫** — 掌握專案的檔案結構、相依關係、規範後再生成與修改程式碼
- **檔案編輯與指令執行** — 只要自然語言指示，就能自動進行多檔案編輯、執行測試、Git 操作
- **可客製化** — 透過教它專案特有的規範與工作流程，可以提升整個團隊的開發品質

---

## 2\. 安裝與設定

### 安裝

```
# macOS / Linux / WSL（建議）
curl -fsSL https://claude.ai/install.sh | bash

# Homebrew
brew install --cask claude-code

# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

### 啟動

```
cd your-project    # 移動到專案目錄
claude             # 啟動 Claude Code
```

首次啟動時，系統會要求你使用 Claude 訂閱方案或 Anthropic Console 帳號登入。

### 使用環境

環境特點 **終端機 CLI** 功能最完整。支援檔案編輯・指令執行・MCP 全功能 **VS Code 擴充功能** 可直接在編輯器內使用，可與程式碼補完並行 **JetBrains 外掛** 適合 IntelliJ / WebStorm / PyCharm 使用者 **桌面應用程式** 方便進行工作階段管理與排程執行---

## 3\. 基本用法

Claude Code 可在終端機中直接以 **自然語言下指令** 來操作。

科技新聞

### 常見用法範例

```
# 產生程式碼
> 用 Repository 模式實作 UserRepository

# 修正 bug
> 修正這個例外。堆疊追蹤如下...

# 重構
> 把這個 Service 重構到認知複雜度 15 以下

# Git 操作
> 把變更內容提交，順便幫我想 commit 訊息

# 撰寫測試
> 幫這個 Service 寫單元測試

# 建立 PR
> 用目前的變更建立 PR，也幫我寫說明文
```

### 重點

- **指示要具體** — 不要只說「整理一下」，而要說「把認知複雜度重構到 15 以下」
- **提供上下文** — 像「繼承 Repository 介面」這類專案特有資訊要一起給
- **分階段進行** — 大型任務拆開來下指令，可降低出錯機率

---

## 4\. 上下文管理

要有效使用 Claude Code，最重要的是 **管理上下文視窗** 。上下文視窗會存放整段對話、讀入的檔案、指令輸出。當內容太多時，Claude 可能會「忘記」之前的指示。

### /init 初始化專案

```
claude
> /init
```

在專案根目錄執行 `/init` ，會自動偵測建置系統、測試框架、程式碼模式，並產生 **起始版 [CLAUDE.md](http://claude.md/)** 。如果你不想從零開始寫，先用這個產生雛形再慢慢調整，是最快的路徑。

專案管理

### @ 提名直接參照檔案

```
> @src/auth/login.py 說明 session 管理
> @package.json 列出裡面的 scripts
```

寫成 `@檔案路徑` 時，Claude 會立即載入該檔案。這比「看看 src/auth 裡的檔案」更精準，也能減少不必要的檔案讀取，節省上下文。

### /compact 壓縮上下文

```
> /compact                          # 自動摘要
> /compact 聚焦在 API 的變更       # 指定摘要內容
```

當長時間對話累積太多上下文時，執行 `/compact` 可以在保留重要資訊（程式碼模式、檔案狀態、已決定事項）的同時壓縮掉不必要的歷史。若在 [CLAUDE.md](http://claude.md/) 寫上 `"壓縮時務必保留變更檔案清單與測試指令"` ，壓縮時就不容易遺失重要資訊。

### /clear 清除

```
> /clear
```

當要切換到無關的任務時，用 `/clear` 來完整重置上下文。如果在同一個工作階段混進太多不相干的任務，舊上下文會干擾判斷並降低效能。

### 其他上下文提供方式

方法用途 `@檔案路徑` 直接參照特定檔案圖片貼上分享截圖・UI 設計 `cat error.log | claude` 透過 pipe 餵入 log・資料URL 指定查閱文件・API 參考資料---

## 5\. 對話控制（快捷鍵・指令）

Claude Code 提供了許多快捷鍵與斜線指令，方便有效率地操作工作階段。

### 主要快捷鍵

鍵動作 `Esc` 中斷 Claude 執行（保留上下文） `Esc` × 2打開回溯選單（還原到檢查點） `Ctrl+G` 切換 Plan Mode ↔ 一般模式### 主要斜線指令

科技新聞

指令說明 `/init` 自動生成 [CLAUDE.md](http://claude.md/) `/clear` 完全重置上下文 `/compact [指示]` 壓縮上下文（可用指示客製摘要） `/rewind` 回到檢查點 `/permissions` 設定工具・指令的允許權限 `/mcp` 確認 MCP 伺服器狀態 `/goal [條件]` 設定整個工作階段的驗證條件 `/btw [問題]` 不消耗上下文的順手提問 `/agents` 管理子代理 `/hooks` 確認 hook 設定 `/sandbox` OS 層級的沙箱隔離### 工作階段管理

```
claude --continue         # 繼續最新的工作階段
claude --resume           # 從工作階段列表中選擇並恢復
```

在工作階段內，也可以像 `/rename oauth-migration` 這樣幫它命名，之後更容易找出來。

### 檢查點與回溯

送給 Claude 的每個 prompt 都會自動成為檢查點。按兩次 `Esc` 或執行 `/rewind` 可打開回溯選單，將對話、程式碼或兩者一起還原到之前的狀態。

> **實務用法** ：在要求 Claude 進行高風險實驗前，先意識到檢查點的存在。若失敗了，就回溯並嘗試不同方法。

---

## 6\. Plan Mode 與 Thinking Mode

### Plan Mode

用 `Ctrl+G` 切換到 Plan Mode 後，Claude 只會 **讀取檔案** ，不會做任何修改。複雜任務建議用「探索 → 規劃 → 實作 → 提交」四階段進行，準確度會更高。

```
# 1. 探索（Plan Mode）
> 先用 /init 產生 CLAUDE.md，再切到 Plan Mode
claude (plan mode)
> 讀取 src/auth，理解 session 管理與登入機制

# 2. 規劃（維持 Plan Mode）
claude (plan mode)
> 我想加入 Google OAuth。需要修改哪些檔案？請幫我做計畫

# 3. 實作（切回一般模式：Ctrl+G）
claude (default mode)
> 依照計畫實作 OAuth 流程，也把 callback handler 的測試寫好

# 4. 提交
claude (default mode)
> 用具描述性的訊息提交，並建立 PR
```

**適合使用 Plan Mode 的時機：**

- 還不確定要怎麼做的時候
- 變更會跨多個檔案時
- 在不熟悉的程式碼庫中工作時

**可以略過的時機：**

- 修正拼字錯誤、變更變數名稱等小修改
- 可以用一句話說清楚差異的任務

### Thinking Mode（延伸思考）

啟用 Thinking Mode 後，Claude 會在回答前先進行 **內部推理步驟** 。雖然比一般回答更花時間，但在複雜除錯、設計決策、演算法實作時，準確度會提升。

科技新聞

```
> think hard about this: 找出這個 race condition 的原因並提出修正方案
```

你可以用 `think` （輕度思考）、 `think hard` （深度思考）、 `ultrathink` （最高強度思考）來調整思考力度。

---

## 7..claude 目錄的全貌

Claude Code 的客製化，是根據特定的目錄結構來進行。先了解整體樣貌吧。

### 目錄結構

```
~/.claude/                          # ← 全域設定（所有專案共通）
├── CLAUDE.md                       # 全域指示檔
├── settings.json                   # 權限設定・MCP 伺服器定義
├── agents/                         # 自訂代理（所有專案共通）
│   ├── architecture-validator.md
│   ├── code-reviewer.md
│   └── ...
├── commands/                       # 自訂指令（Skills 的舊格式）
│   └── review.md
└── skills/                         # 個人 Skills（所有專案共通）
    └── my-skill/
        └── SKILL.md

<專案根目錄>/
├── CLAUDE.md                       # ← 專案專屬指示檔
├── .claude/
│   ├── settings.json               # 專案專屬權限設定
│   ├── settings.local.json         # 本機設定（建議加入 .gitignore）
│   └── skills/                     # 專案 Skills（團隊共享）
│       └── deploy/
│           └── SKILL.md
└── .mcp.json                       # MCP 伺服器設定（團隊共享）
```

### 設定的階層（優先順序）

層級位置用途共享範圍 **全域** `~/.claude/` 個人偏好・所有專案通用規則只有自己 **專案** `<project>/.claude/` 專案特有規範・Skills整個團隊（Git 管理） **本機** `settings.local.json` 個人專案特有設定只有自己 **記法** ：全域 → 專案 → 本機，越往下優先度越高。若有相同設定，較狹窄的範圍會勝出。

科技新聞

---

## 8\. CLAUDE.md 的撰寫技巧

### CLAUDE.md 是什麼

`CLAUDE.md` 是給 Claude Code 的 **永久指示檔** 。它會在不同工作階段中持續被讀取，因此把專案規範、架構、程式碼規則寫進去之後，就不用每次都重複下達同樣的指令。

### 建議的長度與結構

項目建議 **行數** 100～300 行 **太長時** 會壓縮上下文視窗，減少可供其他工作使用的空間 **太短時** AI 的判斷會變得模糊，容易產生違反規範的程式碼### 結構範本

```
# 專案指示

## 基本設定
- 以繁體中文回答
- 程式碼註解請符合既有風格

## 技術棧
- 語言: Python 3.12
- 框架: FastAPI
- 架構: Clean Architecture（四層）
- ORM: SQLAlchemy
- DI: dependency-injector
- ...（列出使用的技術）

## 架構規範
- 各層相依方向規則
- 各層職責定義
- Repository 模式的實作規範

## 命名規範
- 各類型規則（變數／常數／函式／類別）
- 具體前綴、後綴

## 程式碼規範
- formatter / linter 設定
- 禁止事項（例如禁止萬用字元 import）
- 複雜度上限

## 安全性
- 認證／授權方針
- 日誌輸出限制（禁止 PII）
- DB 存取規則

## 測試
- 測試函式命名規則
- Mock / Fake 方針
```

### 撰寫的口訣（實戰經驗）

#### ① 具體化到 AI 能做判斷分歧的程度

```
# ❌ 含糊不清（AI 無法判斷）
- 要有適當的命名

# ✅ 具體（AI 可立即套用）  ← 從 naming.md 擷取
- 變數: snake_case（例: user_id, item_count）
- 常數: UPPER_SNAKE_CASE（例: API_TIMEOUT_SECONDS, MAX_RETRY_COUNT）
- 布林值: 使用 is_ / has_ / can_ 前綴（例: is_active, has_permission）
- 函式: snake_case 且以動詞開頭（例: get_user_by_id, save_order）
- 類別: PascalCase + 層級後綴（Service / Repository / Router）
- 非同步函式禁止加 _async 後綴（async def 本身就代表非同步）
```

#### ② 用 ASCII 圖表現層級依賴

```
### 層級構成與依賴方向  ← 從 architecture.md 擷取
API層 (Router / Controller)
    ↓
Application層 (Service)
    ↓
Domain層 (Entity / Repository Interface)
    ↓
Infrastructure層 (Repository 實作 / DB / 外部 API)

- 依賴方向只能由上往下單向流動
- Domain層只能使用不依賴框架的純 Python
- Infrastructure層透過 Repository 模式抽象化資料來源
```

這樣寫之後，Claude Code 就會自動套用像是「Service 只能透過 Repository 介面存取資料」、「Domain 層不能 import 外部函式庫」這些規則。

#### ③ 寫出具體的程式碼規範

```
## 程式碼規範  ← 從 coding-rules.md 擷取
- 縮排：4 個空白（禁止 Tab）
- 單行最大字元數：120 字元
- 禁止萬用字元 import（測試檔除外）
- 巢狀層級不超過 3 層（善用 Early Return）
- 認知複雜度：目標 5 以下，最大 15
- \`except\` 只有最上層錯誤處理器可捕捉裸 Exception
- \`except\` 區塊不可為空（一定要寫 log 或重新拋出錯誤）
```

#### ④ 禁止事項要明確寫出

```
## 禁止事項
- 不可在 Domain 層引入框架相依性
- 禁止萬用字元 import（測試時除外）
- 禁止在非同步函式後加 _async 後綴
- 禁止硬編碼認證資訊
- 日誌不得包含 PII（個人資訊）
- 不可在 API 層撰寫商業邏輯
```

### 反模式

反模式問題對策 [CLAUDE.md](http://claude.md/) 超過 500 行壓縮上下文過多把細節拆到 Skills「寫得漂亮一點」之類的模糊指示AI 無法判斷拆成可執行的明確規則把任務步驟寫進 [CLAUDE.md](http://claude.md/) 每次都會載入，浪費上下文移到 Skills把所有技術資訊都塞進去資訊過量只保留「應該永遠套用」的規則

---

## 9\. 自訂代理・指令的活用

### 代理與指令的差異

種類位置用途 **代理** `~/.claude/agents/` 或 `.claude/agents/` 具有特定角色的 AI 人格 **指令** `~/.claude/commands/` 或 `.claude/commands/` 特定任務流程（Skills 的舊格式）### 自訂代理的做法

只要把 Markdown 檔放進 `~/.claude/agents/` 即可。

```
# 架構驗證代理

你是這個專案的**架構師**。
請依據 Clean Architecture 原則，驗證架構是否健全。

## 角色
- 驗證整個專案的架構一致性
- 找出相依方向違規與層級職責違反
- 提出具體的重構建議

## 驗證觀點

### 1. 層級依賴方向
- 是否遵守 API → Application → Domain → Infrastructure 的單向依賴
- Domain 層是否沒有框架相依

### 2. 層級職責
- Router：只處理 HTTP request/response，不包含商業邏輯
- Service：是否單一職責，是否透過 Repository 介面存取
- Domain：是否為純語言程式碼，沒有外部函式庫 import
- Repository：是否以介面為基礎，能在測試時替換

### 3. DI（依賴注入）
- 是否使用建構子注入
- 是否沒有直接參照具體類別

## 輸出格式

### 檢查結果
- 🔴 Critical: ...
- 🟡 Warning: ...
- 🟢 Good: ...
```

### 實踐例：專門代理 + 規則 + Instruction 的體制

在專案裡，可以用 **和真人團隊運作相同的結構** 來重現代理系統。

```
┌───────────────────────┐
            │  開發者的指示          │
            └───────────┬───────────┘
                        │
   ┌────────┬────────┬──┴───┬────────┐
   ▼          ▼          ▼        ▼          ▼
Reviewer   Architect  Analyzer  Tester   （必要時擴充）
```

構成要素職責 **Agent** 角色定義（審查、設計驗證、影響分析、測試等） **Skill** 領域知識（命名規範、架構、安全性等） **Instruction** 專案規則、從過去失敗中學到的教訓推薦的代理配置：

代理名稱職責主要驗證項目 `architecture-validator` 架構驗證層級依賴方向・職責分離・DI 範圍 `code-reviewer` 程式碼審查規範違反・安全性・認知複雜度 `impact-analyzer` 影響範圍分析建置影響・執行期影響・測試影響 `test-writer` 自動產生測試正常情況・異常情況・邊界值涵蓋這些只要把各自的 `.md` 檔放到 `~/.claude/agents/` 就能運作。建議隨著專案成長逐步增加代理。

> **每次失敗就補上規則，避免重複犯同樣的錯。**  
> 把過去失敗案例萃取出的規則累積到 Instruction 檔，並讓代理參考，就能建立出整個團隊一起學習的機制。

### 代理 + 規則 + Instruction 的關係

```
Agent（誰）   → 定義角色與行為
Skill（知道什麼）→ 分層提供領域知識
Instruction（怎麼做）→ 持續套用共通規則・過去教訓
```

例如 `code-reviewer` 代理啟動時：

1. 先從 Instruction 檔載入共通規則
2. 依變更內容自動觸發相關 Skill（命名規範 Skill、安全性 Skill 等）
3. 根據代理定義執行審查

### 撰寫的口訣

1. **把檢查重點用條列寫清楚** — 避免 AI 模糊判斷，直接列出檢查項目
2. **指定輸出格式** — 統一報告格式（Critical/Warning/Good 等）
3. **把規範參考來源寫進去** — 像 `參照: /path/to/guideline.md` 這樣寫，規範更新時可自動跟進
4. **從失敗中補規則** — 審查時漏掉的問題加入 Instruction，以後不要再犯
```
## 檢查流程
1. 結構理解：先概覽模組構成與相依關係
2. 規範參照：確認以下指南
   - architecture.md（層級依賴・職責）
   - coding-rules.md（程式碼規範）
   - naming.md（命名規範）
3. 執行檢查：依上述檢查重點逐項確認
4. 輸出結果：整理違規與改善建議並回報
```

---

## 10\. 用 Skills 擴充功能

### Skills 是什麼

Skills 是一種教 Claude Code「特定工作的做法」的機制。和 [CLAUDE.md](http://claude.md/) 不同，Skills 是 **只在需要時才載入** ，因此不會壓迫上下文視窗。

### Progressive Disclosure（段階式揭露）

```
1. 先只確認 Skill 名稱和 description（判斷是否觸發）
     ↓ 只有相關時才
2. 載入 SKILL.md 內文（執行指示）
     ↓ 只有必要時才
3. 存取 references/ 裡的檔案（補充資訊）
```

即使安裝了 20 個 Skills，實際載入的也只有 1～2 個。這就是它和 [CLAUDE.md](http://claude.md/) 最大的差異。

### SKILL.md 的結構

```
my-skill/
├── SKILL.md           # 必要：metadata + 指示
├── references/        # 選用：詳細參考資料
├── templates/         # 選用：範本
└── scripts/           # 選用：自動化腳本
```
```
---
description: 產生文章草稿。「寫文章」「產生草稿」時觸發
disable-model-invocation: false
---

## 步驟

1. 確認主題
2. 思考架構
3. 產生草稿
```
- `---` 包住的部分是 **YAML front matter** （metadata）
- 之後的內容是 **Markdown 內文** （執行指示）
- `description` 會用來判斷是否自動觸發

### CLAUDE.md 與 Skills 的分工

基準CLAUDE.mdSkills **何時載入** 永遠載入需要時才載入 **適合內容** 應該永遠套用的規則特定任務的流程 **範例** 命名規範、層級依賴規則部署流程、PR 審查流程 **建議大小** 100～300 行不限（段階式揭露）### 配置位置

位置使用範圍 `~/.claude/skills/<name>/SKILL.md` 所有專案（個人）`.claude/skills/<name>/SKILL.md` 只有此專案（團隊共享）### 設計模式

#### 模式1：工作流程自動化型

```
---
description: 執行 PR 審查時。程式碼變更後觸發
disable-model-invocation: true
---

## PR 審查流程

1. 確認變更檔案
2. 參考架構規範
3. 確認命名規範
4. 輸出審查結果
```

#### 模式2：子代理分離型

```
---
description: 需要深入調查時
context: fork
agent: Explore
---

徹底調查 $ARGUMENTS，並將結果整理摘要。
```

`context: fork` 表示會在與主對話不同的上下文中執行。這很適合需要讀取大量檔案的調查任務。

### description 的寫法很重要

```
# ❌ 含糊不清（不會自動觸發）
description: 好用的實用工具

# ✅ 具體（會正確觸發）
description: 當需要建立 TypeScript 單元測試時觸發。「寫測試」「新增測試」時發火
```

---

## 11\. 透過 MCP 串接外部工具

### MCP（Model Context Protocol）是什麼

MCP 是用於 AI 工具整合的開源標準協定。連接 MCP 伺服器後，Claude Code 就能直接存取外部工具、資料庫、API。

### 能做的事

- **GitHub** — PR 審查、Issue 管理、程式碼搜尋
- **Sentry** — 錯誤監控、除錯
- **資料庫** — 用自然語言查詢
- **Slack / Notion** — 傳送訊息、查閱文件
- **檔案系統** — 存取特定目錄

### 新增伺服器的方法

#### HTTP 伺服器（建議：雲端服務用）

```
# 連接 GitHub
claude mcp add --transport http github https://api.githubcopilot.com/mcp/ \
  --header "Authorization: Bearer YOUR_GITHUB_PAT"

# 連接 Sentry
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
```

#### stdio 伺服器（本地執行用）

```
# 檔案系統存取
claude mcp add --transport stdio filesystem \
  -- npx -y @modelcontextprotocol/server-filesystem /path/to/dir

# 記憶體伺服器
claude mcp add --transport stdio memory \
  -- npx -y @modelcontextprotocol/server-memory
```

### 設定範圍的使用方式

範圍指令用途 **本機** `--scope local` （預設）個人的開發伺服器 **專案** `--scope project` 團隊共享（儲存在 `.mcp.json` ） **使用者** `--scope user` 所有專案都可用的個人工具### `.mcp.json` 共享給團隊

把 `.mcp.json` 放在專案根目錄並納入 Git 管理，團隊每個人都能使用相同的 MCP 伺服器。

```
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer ${GITHUB_TOKEN}"
      }
    }
  }
}
```

像 `${GITHUB_TOKEN}` 這樣使用環境變數，就可以避免把認證資訊直接提交到 repository。

### 管理指令

```
claude mcp list              # 顯示清單
claude mcp get github        # 查看詳細資訊
claude mcp remove github     # 刪除
```

在工作階段內，也可以用 `/mcp` 指令來確認狀態與驗證。

---

## 12\. 用 Hooks 自動化工作流程

### Hooks 是什麼

Hooks 是一種會在 Claude Code 工作流程中特定位置 **自動執行腳本** 的機制。和 [CLAUDE.md](http://claude.md/) 的文字指示不同，Hooks 是 **決定性的** ，指定的動作一定會執行。

### CLAUDE.md 與 Hooks 的差異

特性CLAUDE.mdHooks **執行保證** 由 AI 判斷是否套用（可能忘記）一定會執行（決定性） **用途** 指引・規範必須每次都執行的動作 **範例** 「測試後執行 linter」在編輯檔案後自動執行 ESLint### 主要 hook 點

hook 點時機使用範例 `PreToolUse` 工具執行前阻擋寫入特定目錄 `PostToolUse` 工具執行後在編輯檔案後自動格式化與 lint `Stop` Claude 在回合結束時執行測試，若未通過就強制持續作業 `Notification` 送出通知時傳送自訂通知### 設定方法

只要自然語言請 Claude 幫你建立 Hooks 即可：

```
> 寫一個在所有檔案編輯後執行 ruff format 的 hook
> 寫一個阻擋寫入 migrations 資料夾的 hook
```

這些設定會存到 `.claude/settings.json` ，也可以用 `/hooks` 指令查看。

### 設定範例

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "command": "ruff format $CLAUDE_FILE_PATH"
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "command": "if echo $CLAUDE_FILE_PATH | grep -q 'migrations/'; then echo 'BLOCK: 禁止直接編輯 migrations 目錄' && exit 1; fi"
      }
    ],
    "Stop": [
      {
        "command": "python -m pytest tests/ -x -q 2>&1 | tail -5"
      }
    ]
  }
}
```

### Hooks 的使用場景

- **程式碼格式化** ：編輯後自動執行 `black` / `ruff` / `prettier`
- **Lint** ：對變更檔案自動執行 lint 檢查
- **保護** ：阻擋寫入特定檔案（migration、設定檔等）
- **驗證關卡** ：在 Stop hook 執行測試，直到通過才繼續作業

> Stop hook 最多連續阻擋 8 次，之後會結束該回合。

---

## 13\. GitHub 連動與 CI/CD 自動化

### CLI 工具（gh）的基本連動

如果安裝了 GitHub CLI（ `gh` ），Claude Code 就能直接建立 Issue、開 PR、讀取留言等。

```
# 安裝 gh CLI
brew install gh
gh auth login
```
```
# 在 Claude Code 工作階段內
> 檢查 gh issue #42 的內容，並實作修正
> 測試通過後建立 PR
```

### GitHub Actions 自動 PR 審查

把 Claude Code 接到 GitHub Actions，就能在每次建立 PR 時自動執行程式碼審查。

```
# .github/workflows/claude-review.yml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: curl -fsSL https://claude.ai/install.sh | bash
      - name: Run review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "請審查這個 PR 的變更。
            請從架構規範、命名規範、安全性三個面向檢查，
            若有問題請留言。" \
            --output-format json
```

### GitHub Code Review 連動

使用官方的 GitHub Code Review 功能時，每個 PR 都會自動附上審查留言。設定方式請參考 [官方文件](https://code.claude.com/docs/en/code-review) 。

### Issue 自動分流

```
# 當 Issue 建立時，自動分類與標記優先度
on:
  issues:
    types: [opened]

steps:
  - run: |
      claude -p "讀取 GitHub Issue #${{ github.event.issue.number }}，
        判斷標籤（bug/feature/docs）與優先度（high/medium/low），
        並用 gh 指令加上標籤。" \
        --allowedTools "Bash(gh *)"
```

### 非對話式模式（claude -p）

若要從 CI/CD 或腳本執行 Claude Code，可使用 `-p` 旗標：

```
# 一次性提問
claude -p "說明這個專案的結構"

# JSON 輸出（供腳本串接）
claude -p "列出 API 端點" --output-format json

# 串流輸出（即時處理）
claude -p "分析 log 檔" --output-format stream-json --verbose

# 使用 auto mode 自主執行
claude --permission-mode auto -p "修正所有 lint 錯誤"
```

---

## 14\. 代理功能（並行處理）

Claude Code 內建可並行執行多個任務的機制。

### 4 種方法

方法調整者通信用途 **子代理** Claude（同一工作階段內）只回傳結果委派旁支任務（調查・分析） **代理視圖** 使用者各自獨立平行執行獨立任務，之後再確認 **代理團隊** Claude（領導者）共享任務清單大型任務的拆分與協作（實驗性） **動態工作流程** 腳本由腳本控制大型自動化（例如 500 檔移轉）### 選擇方式

- 「幫我調查並整理」→ **子代理** （使用 `context: fork` 的 Skill）
- 「並行修 3 個 bug」→ **代理視圖** （ `claude agents` ）
- 「規劃並執行這次重構」→ **代理團隊**

### 確認指令

```
claude agents    # 開啟代理視圖（管理背景工作階段）
```

在工作階段內：

```
/agents          # 管理子代理
/tasks           # 查看背景任務
```

---

## 15\. 總結・下一步

### 推薦的開始方式（3 步驟）

1. **先安裝並實際操作** — `curl -fsSL https://claude.ai/install.sh | bash` → `claude`
2. **先寫 [CLAUDE.md](http://claude.md/)** — 先從技術棧 + 命名規範 + 架構規範開始（約 50 行）
3. **先做一個自訂代理** — 最容易感受到效果的是程式碼審查用代理

---

## 參考文獻

#標題URL1Claude Code 官方文件（首頁） [https://code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview) 2Anthropic 官方網站 [https://www.anthropic.com/](https://www.anthropic.com/)

---

原文出處： [https://qiita.com/s-furuya-nri/items/4385ac59ebefb923cf0d](https://qiita.com/s-furuya-nri/items/4385ac59ebefb923cf0d)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qv2Ewq) [1,316瀏覽](https://codelove.tw/@tony/post/qv2Ewq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qv2Ewq)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784510841&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fqv2Ewq&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI1Il1dLDBd&amp;dt=1784510841724&amp;bpp=3&amp;bdt=165&amp;idt=58&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784277631%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784277631%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784277631%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5230528083853&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=5&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396104%2C95396568%2C42533293%2C95396252&amp;oid=2&amp;pvsid=4117756922266236&amp;tmod=426775654&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=2067%2C30%2C2067%2C30%2C1920%2C30%2C1773%2C1050%2C1533%2C1003&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.16&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=67" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260716/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>

開放原始碼