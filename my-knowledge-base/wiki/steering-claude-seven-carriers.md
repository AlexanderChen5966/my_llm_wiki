---
title: 傳遞指令的七種載體：判斷「這條指令放錯地方了」
date: 2026-08-19
tags: [ai/claude, ai/prompt-engineering]
source: https://zhuanlan.zhihu.com/p/2059733450484199819（中譯，原文為 Anthropic 官方 blog：claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more）
type: article
status: processed
asset_value: 82
---

⚠️ 這是**官方 blog 的中譯**（非個人心得），所以下列判準屬官方立場。原文日期 2026-07-12。

## T（核心論點）

七種載體不是「哪個最好」的問題，而是**同一條指令放錯載體就會失效**。官方給的判斷方式是列出五個症狀：

> 🔑 **如果你發現自己正在做下面這些事，通常意味著這條指令放錯地方了。**

而貫穿五個症狀的一句話最該記住：

> 🔑 **模型主動決定要不要調用格式化器，和格式化器被系統自動執行，是兩件完全不同的事。**

## B（適用場景）

**適用：**
- CLAUDE.md 越寫越長卻越不管用時的重構依據
- 要建立「組織級確定性護欄」時（見下方 managed settings）
- 判斷某條規則該用 `paths:` 限定、還是該搬進 skill

**不適用 / 注意：**
- ⚠️ 七種載體的細節（各自語法）本庫已有專條，本條目聚焦**選用判斷**
- ⚠️ 中譯本，引用官方原文用語時建議回查原始 blog

## R（七種載體）

`CLAUDE.md` ／ `Rules` ／ `Skills` ／ `Subagents` ／ `Hooks` ／ `Output styles` ／ **`--append-system-prompt`**

其中 `--append-system-prompt` 是本庫先前完全沒收錄的一種，它的定位很特別：

- **比改 output style 溫和**——只在原始系統提示詞上**追加**，**不改變 Claude 的基礎角色**
- **呼叫時動態傳入，只對這一次生效**，不像檔案那樣跨 session 長期存在
- 🔴 **上下文成本通常更高**（會增加輸入 token；prompt caching 能在首輪之後降低一部分，但若同時要求更長更囉嗦的風格，輸出 token 也會被抬高）
- 最適合補充**具體的編碼標準、輸出格式要求或領域知識**

⚠️ 但官方對它有一條重要警告：

> 🔑 **這種方法對遵循度有「邊際遞減效應」：通常你附加的指令越多，Claude 對它們的執行反而會越鬆——尤其當這些指令彼此存在衝突時。**

## R（🔑 五個「放錯地方」的症狀與正解）

### ① 「每次 X 都必須做 Y」寫在 `CLAUDE.md` → 應該是 hook

如果這個行為需要**穩定、確定地發生**（例如每次編輯後自動跑 prettier、完成任務後發 Slack），
那就該寫成 `settings.json` 裡的 hook，**而不是寄希望於模型「記得去做」**。

### ② 「絕對不要做某事」寫在 `CLAUDE.md` → 純指令不是對的工具

> **只要是「絕對不能發生」的事情，純指令就不是對的工具。Claude 大多數時候會遵守，但在長會話、模糊情境、高壓任務，
> 或者檔案裡存在 prompt injection 時，它仍可能失手。**

🔑 **真正的護欄必須是確定性的**，要用 hooks 與 permissions：

- **`PreToolUse` hook 可以檢查呼叫並用「返回碼 2」阻止它**
- 🔑 **`managed settings` 更進一步：它由管理員下發、使用者本地配置無法覆蓋——是「組織級確定性護欄」的唯一可靠方式**

（這是官方自己把 [[claude-code-hooks-memory]] 的強制力光譜講到最硬的一段；`managed settings` 這個層級本庫先前未收錄）

### ③ 把一個 30 行的操作流程寫進 `CLAUDE.md` → 應該放 skill

> 🔑 **`CLAUDE.md` 用來存放那些 Claude 需要始終記住的「事實」**——建置命令、monorepo 佈局、團隊約定；
> **而部署 runbook 或安全審查 checklist 這類內容，應該放到 `.claude/skills/`，只在需要時載入正文。**

### ④ 只針對 API 的 rule 卻沒做路徑作用域 → 用 `paths:` 限定

如果一條 rule 只適用於 `src/api/**`，就該用 `paths:` 限定範圍。
⚠️ **否則它和把同樣內容直接塞進 `CLAUDE.md` 沒區別：永遠載入，永遠消耗 token。**

### ⑤ 把個人偏好寫進專案級 `CLAUDE.md` → 用使用者級版本

**所有基於檔案的方法都有對應的使用者級版本**，會在每次 session 載入而不限於某個 repo。
個人偏好（例如「總是使用語義化 commit message」）寫在本地使用者級檔案；
**專案級檔案只保留團隊共有、且只對當前程式庫生效的內容**。

## C（關鍵行動）

- [ ] 檢查 `CLAUDE.md`：有沒有「每次都要」或「絕對不要」的句子？那些該搬去 hook／permissions
- [ ] 超過 30 行的流程搬進 `.claude/skills/`；只適用特定路徑的規則加 `paths:`
- [ ] 組織級硬性規範用 **managed settings**（使用者無法覆蓋），不要只寫在文件裡

## 補充資料

### 這五條與本庫既有判例的交叉驗證

| 官方判準 | 本庫既有記錄 |
|---|---|
| 「絕對不要」要用 hooks + permissions | [[claude-code-hooks-memory]]：官方定性 CLAUDE.md 與自動記憶都是「上下文參考」非強制 |
| 30 行流程放 skill | [[skill-decision-what-to-automate]]：先整套寫在一起，複雜或需重用時再拆出去 |
| rule 要用 `paths:` 限定 | [[claude-md-optimization-techniques]] 記錄的 `.claude/rules/` 路徑範圍技巧；⚠️ 並注意 [[claude-code-harness-internals]] 提醒的「`paths:` rules 在 compaction 會先遺失」 |
| CLAUDE.md 放事實不放流程 | [[claude-5-context-engineering-rules]] 併入的「CLAUDE.md 簡述 repo，把 token 留給 gotchas」 |
| 個人偏好用使用者級 | [[claude-code-auto-memory]] 的 Auto Memory／全域／專案三分流 |

🔑 **五條全部指向同一件事：載體的差別不在語法，而在「強制力」與「載入時機」。**

### `--append-system-prompt` 與 output style 的取捨

| | `--append-system-prompt` | Output style |
|---|---|---|
| 影響範圍 | **只追加**，不改基礎角色 | **可能對行為造成很大、甚至意料之外的影響** |
| 生命週期 | **只這次呼叫**，不跨 session | 設定後持續 |
| 成本 | 🔴 **較高**（每次都算輸入 token）| 一次設定 |

⚠️ 所以它適合「這一次要特別交代的編碼標準／輸出格式」，**不適合長期規範**（長期的該進檔案）。

## 相關連結
- [[synthesis/synthesis-claude-code-internals]] — **本篇為其來源之一**（2026-09-03 重跑，6→8 篇）：官方載體選擇判準，與「每輪組裝」機制互為使用面與機制面
- [[claude-code-output-styles-concise]] — 七種載體之一（輸出樣式）的實測：🔑 **Concise 縮短的是說明文字，不是資訊**
- [[claude-code-hooks-memory]] — Hook 機制與強制力光譜（本篇是官方對這條光譜最硬的表述，含 managed settings）
- [[claude-md-design-patterns]] — 依規模的 CLAUDE.md 範本與反模式
- [[claude-md-optimization-techniques]] — `paths:` 路徑範圍與總量取捨
- [[claude-5-context-engineering-rules]] — 官方 Claude 5 世代守則（「別重複自己」與本篇 ④⑤ 同源）
- [[skill-decision-what-to-automate]] — 什麼該做成 Skill
- [[agent-control-layer-permission-hooks-planning]] — Capability ≠ Permission、Prompt 不是安全邊界
- [[agent-permission-and-os-sandbox]] — 手刻版的權限分級與 fail-closed
- [[claude-agents-skill-file-comparison]] — agents／skill 檔案格式對照
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills／MCP／Plugins／Agents 的定位比較
- [[claude-code-mods-overview]] — 2026-10 新增的 App 層擴充：不是跟模型講話，而是改 Claude Code 本身；判斷『指令放哪』時多了這一格
