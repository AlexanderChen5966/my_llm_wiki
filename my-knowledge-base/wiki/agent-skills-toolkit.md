---
title: agent-skills：Addy Osmani 的 Production-grade AI 開發技能包
date: 2026-05-14
tags: [ai/agent, dev/architecture, pkm/workflow]
source: https://github.com/addyosmani/agent-skills
type: article
status: processed
asset_value: 75
---

Addy Osmani（Google Chrome DevRel）開源的生產級 AI 代理技能包，22 個 Skill 涵蓋從構思到交付的完整軟體開發生命週期（DEFINE → PLAN → BUILD → VERIFY → REVIEW → SHIP）。核心理念：技能是工作流程，不是參考文件——每個 Skill 有明確步驟、驗證門檻、反藉口表格，確保 AI 在每個開發階段都遵循資深工程師的判斷標準。

## T（核心論點）
AI coding agent 缺的不是聰明，而是跟資深工程師等量的**工程判斷與紀律**。agent-skills 把 Google 工程文化（Hyrum's Law、Beyoncé Rule、測試金字塔、Chesterton's Fence、Shift Left）編碼進 22 個可組合的 Skill 工作流——讓 AI 每次開發都像資深工程師一樣思考，而不只是按需求生成程式碼。

## B（適用場景）
**適用：** 需要讓 AI agent 具備完整工程紀律的所有專案；支援 Claude Code / Cursor / Gemini CLI / Windsurf / GitHub Copilot / Kiro IDE / OpenCode / Codex
**不適用：** 一次性原型快速驗證（流程過重）

## R（22 個 Skill + 7 個 Slash 命令）

### 六階段開發生命週期
```
DEFINE          PLAN           BUILD          VERIFY         REVIEW          SHIP
┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
│ Idea │ ───▶ │ Spec │ ───▶ │ Code │ ───▶ │ Test │ ───▶ │  QA  │ ───▶ │  Go  │
│Refine│      │  PRD │      │ Impl │      │Debug │      │ Gate │      │ Live │
└──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
 /spec          /plan          /build        /test         /review       /ship
```

### Slash 命令（7 個）
| 命令 | 用途 | 核心原則 |
|------|------|---------|
| `/spec` | 定義要建什麼 | Spec before code |
| `/plan` | 規劃如何建 | Small, atomic tasks |
| `/build` | 漸進式實作 | One slice at a time |
| `/test` | 驗證它能動 | Tests are proof |
| `/review` | 合併前審查 | Improve code health |
| `/code-simplify` | 簡化程式碼 | Clarity over cleverness |
| `/ship` | 交付到生產 | Faster is safer |

Skills 也會根據情境**自動觸發**——設計 API 觸發 `api-and-interface-design`，建 UI 觸發 `frontend-ui-engineering`。

### 22 個 Skill 分類

**Meta**
| Skill | 作用 |
|-------|------|
| `using-agent-skills` | 把工作映射到正確 Skill；定義共用操作規則 |

**Define（釐清要建什麼）**
| Skill | 作用 |
|-------|------|
| `idea-refine` | 發散/收斂思考，把模糊想法轉為具體提案 |
| `spec-driven-development` | 寫 PRD（目標/命令/結構/程式碼風格/測試/邊界）再動工 |

**Plan（拆解任務）**
| Skill | 作用 |
|-------|------|
| `planning-and-task-breakdown` | 把 Spec 拆成小的、可驗證的任務，含驗收條件與依賴排序 |

**Build（寫程式）**
| Skill | 作用 |
|-------|------|
| `incremental-implementation` | 薄垂直切片：實作→測試→驗證→commit；feature flags；回滾友善 |
| `test-driven-development` | Red-Green-Refactor；測試金字塔（80/15/5）；DAMP > DRY；Beyoncé Rule |
| `context-engineering` | 在正確時機餵入正確資訊（rules files / context packing / MCP）|
| `source-driven-development` | 每個框架決策引用官方文件，標記未驗證部分 |
| `doubt-driven-development` | 敵對式全新 context 審查：CLAIM→EXTRACT→DOUBT→RECONCILE→STOP |
| `frontend-ui-engineering` | 元件架構/設計系統/狀態管理/WCAG 2.1 AA |
| `api-and-interface-design` | Contract-first / Hyrum's Law / One-Version Rule / 錯誤語義 |

**Verify（證明能動）**
| Skill | 作用 |
|-------|------|
| `browser-testing-with-devtools` | Chrome DevTools MCP：DOM 檢查 / 網路追蹤 / 效能分析 |
| `debugging-and-error-recovery` | 五步驟分類：重現→定位→縮小→修復→防護 |

**Review（合併前品質關卡）**
| Skill | 作用 |
|-------|------|
| `code-review-and-quality` | 五軸審查；變更大小（~100 行）；嚴重度標籤（Nit/Optional/FYI）|
| `code-simplification` | Chesterton's Fence；Rule of 500；保持行為不變下降複雜度 |
| `security-and-hardening` | OWASP Top 10；auth patterns；secrets 管理；三層邊界系統 |
| `performance-optimization` | 先量測：Core Web Vitals 目標 / 分析工作流 / bundle 分析 |

**Ship（部署）**
| Skill | 作用 |
|-------|------|
| `git-workflow-and-versioning` | Trunk-based；atomic commits；commit-as-save-point 模式 |
| `ci-cd-and-automation` | Shift Left；Faster is Safer；feature flags；品質門檻 pipeline |
| `deprecation-and-migration` | Code-as-liability；強制 vs 建議棄用；zombie code 清除 |
| `documentation-and-adrs` | ADR；API 文件；記錄「為什麼」而非「做了什麼」|
| `shipping-and-launch` | 預上線清單；feature flag 生命週期；階段性 rollout；rollback |

### Skill 解剖學
每個 Skill 遵循一致結構：
```
SKILL.md
├── Frontmatter（name + description：什麼時候用，不是做什麼）
├── Overview    → 這個 Skill 做什麼
├── When to Use → 觸發條件
├── Process     → 逐步工作流
├── Rationalizations → 常見藉口 + 反駁
├── Red Flags   → 出問題的徵兆
└── Verification → 所需的證據（不接受「看起來對」）
```

**四大設計選擇：**
1. **流程而非說明文件**：Skill 是工作流，不是參考資料。每個有步驟、檢查點、退出標準
2. **反藉口機制**：預先堵死常見藉口（如「測試之後再寫」）
3. **驗證不可妥協**：每個 Skill 以證據要求結尾——測試通過、build 輸出、執行時資料
4. **漸進式揭露**：`SKILL.md` 是入口，支援 references 只在需要時載入（節省 token）

### Rationalizations 表實例（設計精神的核心）

Agent 遇到用戶要求跳步時，Skill 內建反駁依據——讓 AI 扛得住壓力，不馬上就範：

| 藉口 | 反駁 |
|------|------|
| 「測試拖慢速度」 | 除錯時間是測試時間的 3-5 倍 |
| 「先能動就好」 | prototype code 有 70% 最終會進 prod |
| 「我們是 internal tool 不用考慮安全」 | internal tool 洩漏 prod credential 的案例每月都在發生 |
| 「加 CSRF 太麻煩」 | Express middleware 30 秒搞定，攻擊成本遠小於損失 |
| 「input validation 等 final 再加」 | 開發中期加最省事，上線前加漏洞最多 |

### 典型使用流程（OAuth 加入 Express API）

```
Step 1 /spec   → 澄清需求與邊界（Out of Scope 寫清楚）
Step 2 /plan   → 任務分解，排依賴順序
Step 3 /build  → 增量實作（一次一個 slice）
Step 4 /test   → 先寫失敗測試，再寫讓它通過的 code
Step 5 /review → 五軸審查（安全/性能/可讀性/可維護/完整性）
Step 6 /ship   → CI 綠燈？文件更新？ADR 記錄？Rollback plan？
```

### 與 Anthropic 官方 Skills 的定位差異

| | Anthropic 官方 | addyosmani/agent-skills |
|--|--------------|------------------------|
| 定位 | 平台教學、功能示範 | 生產實踐、工程紀律 codify |
| 內容 | 單 Skill 功能展示 | 整條開發流程 |
| 相容性 | Claude 專屬 | Claude + 多 agent |
| 哲學 | 「這樣用 Skill」 | 「這樣把工程紀律封裝進 Skill」 |

### 三個 Agent Personas
| Agent | 角色 | 審查標準 |
|-------|------|---------|
| `code-reviewer` | Senior Staff Engineer | 五軸程式碼審查 |
| `test-engineer` | QA Specialist | 測試策略 + Prove-It 模式 |
| `security-auditor` | Security Engineer | 弱點偵測 + OWASP 評估 |

### 安裝
```bash
# Claude Code Marketplace
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills

# 或 local dev
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

**其他平台：**
- Cursor：複製任何 `SKILL.md` 到 `.cursor/rules/`
- Gemini CLI：`gemini skills install https://github.com/addyosmani/agent-skills.git --path skills`
- Kiro IDE：Skills 放在 `.kiro/skills/`

## C（關鍵行動）
- [ ] agent-skills 與 gstack / superpowers 互補——agent-skills 著重「工程品質判斷」，gstack 著重「戰略角色分工」，superpowers 著重「強制 TDD 紀律」
- [ ] 最有效的起點：`/spec` + `/plan` + `/build`（三個命令體驗完整差異）
- [ ] Skill 的 description 只寫「什麼時候用」，不寫「做什麼」——否則 AI 讀描述就跳過正文

## 補充資料

來源：`agent-skills/AGENTS.md at main`（GitHub addyosmani/agent-skills，2026-05-14）

### AGENTS.md 說明（供其他 AI Coding Agent 使用）

本套件的 AGENTS.md 提供給 Claude Code 以外的工具（如 Cursor、Gemini CLI、GitHub Copilot、Kiro IDE、Codex）使用的統一說明：

**快速安裝（各平台）**

```bash
# Claude Code（推薦）
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills

# Gemini CLI
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills

# Cursor
# 複製任何 SKILL.md 到 .cursor/rules/

# Kiro IDE
# Skills 放在 .kiro/skills/
```

**核心設計說明（摘自 AGENTS.md）**：
> Production-grade engineering skills for AI coding agents. Skills encode the workflows, quality gates, and best practices that senior engineers use when building software. These ones are packaged so AI agents follow them consistently across every phase of development.

**7 個 Slash 命令觸發對應的 Skill 自動啟動**：
- Skills 也會**自動觸發**——設計 API 時觸發 `api-and-interface-design`，建 UI 時觸發 `frontend-ui-engineering`

## 相關連結
- [[superpowers-framework]] — obra 的強制 TDD 工程紀律框架（Mandatory Workflow）
- [[gstack-framework]] — YC CEO Garry Tan 的完整 Sprint 框架（角色分工）
- [[spec-driven-ai-workflow]] — OpenSpec + Superpowers 規格驅動工作流
- [[claude-skills-ecosystem]] — 知識工作者導向的研究分析 Skill 套件（research-knowledge-super-skill）收錄於此
- [[harness-engineering]] — Guides × Sensors 系統設計理論
- [[agents-md-guide]] — AGENTS.md 設計原則（agent-skills 的重要 Guide）
- [[claude-skills-ecosystem]] — 更廣泛的 Skills 生態概覽
