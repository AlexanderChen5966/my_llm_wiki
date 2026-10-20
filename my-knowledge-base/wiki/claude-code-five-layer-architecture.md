---
title: Claude Code 五層架構：AI 智慧體開發套件完整解析
date: 2026-05-19
tags: [ai/claude, ai/agent, dev/architecture]
source: https://vocus.cc/article/69facfcffd8978000168ef78
type: article
status: processed
asset_value: 82
---

Claude Code 的 Agent Development Kit 採用五層架構設計，從記憶層到分發層，系統性解決 AI 智慧體開發中的上下文汙染、權限控管與任務委派痛點。核心設計哲學：讓 AI 發揮非確定性創造力的同時，用工程化手段加上邊界與護欄。

## T（核心論點）

五層架構讓 AI 智慧體從「對話機器人」變成可落地於企業開發流程的 AI 智慧體平台。每層解決不同問題：Layer 3（Hooks）和 Layer 4（Subagents）是所有生產級應用的必備機制。

## B（適用場景）
**適用：** 設計 AI Agent 系統；需要解決 context 汙染、權限控管、任務委派問題的開發場景
**不適用 / 注意：** Subagent 不能再產生子 Subagent（無限遞迴限制），確保遞迴深度可控

## R（五層詳解）

### Layer 1 — CLAUDE.md：記憶層（The Memory Layer）

整個架構的基礎層，所有規則被載入的起點。分三個層次：

| 層次 | 位置 | 作用 |
|------|------|------|
| 全域層級 | `~/.claude/CLAUDE.md` | 永遠被載入，定義全專案通用架構規則 |
| 專案層級 | `.claude/CLAUDE.md` | 專案「憲法」與 repository 地圖 |
| 代理層級 | `CLAUDE.md`（特定 Agent） | 給特定智慧體使用的規則檔 |

> 類比：系統的 BIOS + 作業系統核心，一開機就存在，定義所有行為的基本邊界。

### Layer 2 — SKILLS：知識層（The Knowledge Layer）

每個 SKILL 是獨立知識包，包含 `SKILL.md`（說明文件 + 觸發條件）、腳本、範本、參考文件。

**自動比對機制**：系統根據任務描述觸發對應 SKILL，在隔離的 subagent 中執行，用完即釋放，不長期占用主對話 context。

> 類比：隨身碟或外接知識庫——需要時插上，用完就拔掉，主記憶體永遠保持乾淨。

### Layer 3 — HOOKS：護欄層（The Guardrail Layer）

最具工程感的設計。類似 Git hooks，針對 AI 智慧體生命週期事件設置關卡。

**支援事件類型：**
- `PreToolUse` / `PostToolUse`：工具執行前後
- `SessionStart` / `SessionStop`：會話開始與結束
- `SubagentStop`：子智慧體結束時

事件觸發後經過**匹配器（Matcher）**檢查，再執行對應命令。

> 類比：CI/CD 的 pipeline + 資安防線，用確定性邏輯補足 AI 的不確定性。

### Layer 4 — SUBAGENTS：委派層（The Delegation Layer）

解決 context 無限膨脹的關鍵設計。主智慧體把子任務委派給專門的子智慧體：

```
主智慧體
  ├── code-reviewer   — 程式碼審查
  ├── test-runner     — 執行測試
  └── explorer        — 探索未知程式碼庫
```

每個子智慧體有**自己的 context window**，不汙染主智慧體。重要限制：子智慧體**不能再產生子智慧體**（no infinite recursion）。

> 類比：將工作拆分給不同部門的同事，每個人在自己的會議室作業，最後只把結果回報給專案經理。

### Layer 5 — PLUGINS：分發層（The Distribution Layer）

負責封裝與分享。把自訂的 SKILL、HOOK、SUBAGENT、CLAUDE.md 打包成 Plugin，透過 Marketplace 發布，讓團隊成員用一行指令安裝。

```javascript
my-agent-plugin/
├── skills/
├── agents/
├── commands/
├── hooks/
└── CLAUDE.md
```

> 類比：讓 AI 智慧體的能力可以像 npm 或 GitHub Actions 一樣共享、版本控制、團隊協作。

### 五層協同運作流程

1. **啟動時**：Layer 1（CLAUDE.md）永久載入，定義全局規則
2. **遇到任務**：Layer 2（SKILLS）自動匹配需要的知識，在隔離環境執行
3. **執行動作前後**：Layer 3（HOOKS）攔截事件，進行檢查、通知、防護
4. **需要分工**：Layer 4（SUBAGENTS）產生子智慧體，各自處理子任務，保持主 context 乾淨
5. **成果發布**：Layer 5（PLUGINS）將可重用能力封裝，分享給整個團隊

## C（關鍵行動）
- [ ] Layer 3 Hooks 優先設定：PreToolUse 做安全檢查，PostToolUse 做通知或格式化
- [ ] Layer 4 Subagents 策略：複雜任務先判斷能否拆成獨立子任務再委派
- [ ] Layer 5 Plugin 打包：把穩定的 Skill + Hook 組合打包為 Plugin 供團隊使用

## 相關連結
- [[claude-code-plugins-guide]] — Plugin（第五層）的六元件詳解與安裝指南
- [[claude-skills-guide]] — SKILLS（第二層）設計原理
- [[harness-engineering]] — Harness 工程：從環境角度看護欄設計
- [[claude-md-guide]] — CLAUDE.md（第一層）撰寫最佳實踐
- [[claude-code-context-management]] — Subagent 委派與 context 管控策略
- [[synthesis-claude-code-internals]] — 本篇為其來源之一；⚠️ 該精粹指出本篇是六篇中最舊的一篇（2026-05-19），**Layer 5 Plugin 描述與 `.claude/CLAUDE.md` 路徑寫法建議先核實**，並釐清本篇「五層」是 build-time 切面，與 harness 的 runtime 三層不同層級
