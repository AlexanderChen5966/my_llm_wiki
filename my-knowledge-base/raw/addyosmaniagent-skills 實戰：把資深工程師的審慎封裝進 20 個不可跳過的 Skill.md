---
title: "addyosmani/agent-skills 實戰：把資深工程師的審慎封裝進 20 個不可跳過的 Skill"
source: "https://www.itnotetk.com/2026/04/21/addyosmani-agent-skills-engineering-discipline/?doing_wp_cron=1777251734.0132501125335693359375#google_vignette"
author:
  - "[[tony]]"
published: 2026-04-21
created: 2026-05-14
description: "看過 Addy Osmani（Google Chrome 團隊的前端性能傳奇人物）過往作品的人都知道，他擅長把「老工程師知道但新人沒踩過的事」做成結構化 checklist。那本著名的《Learnin..."
tags:
  - "clippings"
status: processed
---
看過 Addy Osmani（Google Chrome 團隊的前端性能傳奇人物）過往作品的人都知道，他擅長把「 **老工程師知道但新人沒踩過的事** 」做成結構化 checklist。那本著名的《Learning JavaScript Design Patterns》、那份 Critical Rendering Path 論文、Squoosh 等工具——都是把 **隱性知識 → 顯性流程** 的典範。

所以當看到他開了 [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) 這個 repo（已經 19k stars），第一個念頭是：「這次他要對 AI agent 做同樣的事」。讀完 repo 的 `SKILL.md` 們，確認這不只是另一個 skill 集合，而是把 **senior engineer judgment 編譯成 non-skippable workflow** 。

這篇是完整評估 + 導入筆記。

---

## 一、為什麼需要這種 skill 包

AI coding agent 最大的問題不是「不會寫程式」，是 **太容易被說服跳過步驟** 。

典型對話：

```makefile
agent: 我先寫個測試...
user: 不用了，直接改功能就好
agent: 好的。
（於是 test coverage 再也回不來）
```

addyosmani 的 skill pack 裡有個關鍵設計叫 **Rationalizations table** ——列出常見「跳過藉口」跟對應反駁。agent 遇到被 push 跳步時有依據回應，不會馬上就範。這是整個專案的精神。

---

## 二、SKILL 分類：19 個 phase skill + 周邊

| Phase | Skills |
| --- | --- |
| **Define** | idea-refine, spec-driven-development |
| **Plan** | planning-and-task-breakdown |
| **Build** | incremental-implementation, test-driven-development, context-engineering, source-driven-development, frontend-ui-engineering, api-and-interface-design |
| **Verify** | browser-testing-with-devtools, debugging-and-error-recovery |
| **Review** | code-review-and-quality, code-simplification, security-and-hardening, performance-optimization |
| **Ship** | git-workflow-and-versioning, ci-cd-and-automation, deprecation-and-migration, documentation-and-adrs, shipping-and-launch |

加上：

- **3 個 specialist agent persona** ：code-reviewer、test-engineer、security-auditor
- **4 個 reference checklist** ：testing / security / performance / accessibility

總共 **19 skills + 3 agents + 4 checklists = 26 個構件** 。

---

## 三、SKILL.md 的格式規範

```markdown
---
name: lowercase-hyphen-name
description: One-line + "Use when…"
---
​
## Overview
What this skill does
​
## When to Use
Triggering conditions
​
## Process
Step-by-step workflow（不是 reference doc，是可執行步驟）
​
## Rationalizations
| 藉口 | 反駁 |
| "測試拖慢速度" | 「除錯時間是測試時間的 3-5 倍」 |
| "先能動就好" | 「prototype code 70% 會進 prod」 |
​
## Red Flags
什麼訊號出現時要警覺
​
## Verification
完成的判定條件（跑什麼命令、看什麼輸出）
```

三個設計原則：

1. **Process, not prose** ——skill 是 agent 要執行的劇本，不是人類讀的文章
2. **Anti-rationalization** ——列出被 push 跳步時怎麼回應
3. **Verification is mandatory** ——「看起來對」不算完成，要有具體證據

---

## 四、安裝到不同 agent

### 4.1 Claude Code（官方 marketplace，推薦）

```bash
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

### 4.2 從 source 裝

```bash
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

### 4.3 其他 agent

- **Cursor** ：把 `SKILL.md` 檔複製到 `.cursor/rules/`
- **Gemini CLI** ： `gemini skills install https://github.com/addyosmani/agent-skills.git --path skills`
- **GitHub Copilot** ：agent 定義放 `agents/` 目錄，規則追加到 `.github/copilot-instructions.md`
- **Windsurf** ：rules config 加 skill 內容
- **OpenCode** ： `AGENTS.md` + `skill` tool
- **Kiro IDE** ：放 `.kiro/skills/`

多 agent 相容性是這個 repo 跟 anthropic 官方 skills 很不同的地方—— **它把自己當 de facto 共通格式** 。

---

## 五、7 個 slash command

```bash
/spec           → spec-driven-development
/plan           → planning-and-task-breakdown
/build          → incremental-implementation
/test           → test-driven-development
/review         → code-review-and-quality
/code-simplify  → code-simplification
/ship           → shipping-and-launch
```

等於幫你把 dev flow 的七個 phase 做成 one-key 觸發。也支援 **自動觸發** ——Claude Code 會讀 `description` 欄位，用戶對話符合條件時自動啟用。

---

## 六、典型使用流程

用「加 OAuth 到現有 API」當範例：

**Step 1：澄清** — `/spec 幫我把 OAuth Google login 加到 Express API`  
**Step 2：規劃** — `/plan` 產出任務分解  
**Step 3：增量實作** — `/build 第一個 task`  
**Step 4：測試驅動** — `/test` 先寫測試再寫 implementation  
**Step 5：審查** — `/review` 按 checklist 過一遍  
**Step 6：上線** — `/ship` ：CI 綠燈？文件更新？ADR 記錄？Rollback plan？

---

## 七、幾個 SKILL 深入拆解

### 7.1 security-and-hardening

| 藉口 | 反駁 |
| --- | --- |
| 「我們是 internal tool 不用考慮 sec」 | internal tool 洩漏 prod credential 的案例每月都在發生 |
| 「加 CSRF 太麻煩」 | Express middleware 30 秒搞定，攻擊成本遠小於 |
| 「input validation 等 final 再加」 | 開發中期加最省事，上線前加漏洞最多 |

### 7.2 code-simplification

強調「less is more」：刪掉不必要的抽象、拒絕「hypothetical future need」為由的複雜化、用 Chesterton's Fence rule 判斷既有複雜度要不要留。對 AI agent 特別重要——agent 傾向 over-engineer，這 skill 會拉它回來。

### 7.3 context-engineering

Osmani 的專長。重點：寫到 skill 的 context 要 progressive disclosure，不要塞 full doc，用 link + 摘要，agent 需要時才展開。

---

## 八、對照：跟 Anthropic 官方 skills 的關係

| 面向 | Anthropic 官方 | addyosmani/agent-skills |
| --- | --- | --- |
| 定位 | 官方範例、平台 demo | 社群、實戰 workflow |
| 格式 | SKILL.md（官方規格） | SKILL.md（相容 + 延伸規範） |
| 內容側重 | 單 skill 功能示範 | 整條開發流程 |
| 相容性 | Claude 專屬 | Claude + 多 agent |
| 哲學 | 「這樣用 skill」 | 「這樣把工程紀律 codify 成 skill」 |

不衝突——Anthropic 的是 **平台教學** ，Osmani 的是 **生產實踐** 。

---

## 九、值得借鑑的設計哲學

### 9.1 Skill 是劇本，不是文件

好的 skill 長得像 runbook，不像 whitepaper。

### 9.2 Rationalizations 表是 skill 的盔甲

agent 要扛得住 user 的壓力才能把 skill 執行完。預先列出「常見被跳過的理由 + 反駁」。

### 9.3 Verification 是 skill 的鋼筋

沒有 verify 步驟的 skill 等於建議，不是規範。每個 skill 都有具體的 verify command（跑 test、看 coverage、跑 lint、跑 a11y audit）。

### 9.4 Progressive disclosure 省 token

skill 自己精簡，詳細 reference 放 `references/` 目錄，agent 要用時才讀。

---

## 十、導入建議

- **個人開發者** ：建議直接裝。7 個 slash command 立即提升日常 AI coding 品質
- **團隊導入** ：分階段，先用 `/review` 跟 `/test` 兩個 skill，建立共識後再擴大
- **Skill 作者** ：把 20 個 SKILL.md 全部讀一遍，會發現一套可複製的寫 skill 方法論

---

## 十一、小結

addyosmani/agent-skills 在 Claude Code 生態的位置很特殊：

- 比 **Awesome Claude Code** 精：不是清單，是可執行 skill
- 比 **Superpowers** 廣：covers 整條開發流程
- 比 **官方 skills** 實戰：來自多年 engineering 實踐

如果只看 Claude Code 生態一個 repo，我會推這個。Osmani 用 skill 這個新載體，把工程紀律 codify 給 AI 時代。