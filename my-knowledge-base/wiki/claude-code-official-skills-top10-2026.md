---
title: 2026 程式圈熱門 10 個 Skills：官方安裝命令 + 安裝位置速查
date: 2026-07-13
tags: [ai/claude, type/reference]
source: https://codelove.tw/@tony/post/a9yLpa（原文：https://juejin.cn/post/7660712622825144374）
type: article
status: processed
asset_value: 50
---

2026-07 時點程式圈熱門 10 個 Claude Code Skills 排行與安裝命令速查：GitHub 上已有 1400+ 個 Skill、官方 marketplace 收錄 658 個，但社群測試 100 個裡 70% 不合格。Superpowers/Karpathy/GStack 三個框架已有專篇（見相關連結），本文補充官方 Anthropic Skills 儲存庫裡另外 6 個實用單品：Frontend Design、Webapp Testing、Security Review、MCP Builder、Skill Creator、Composio。

## T（核心論點）

Skill 生態數量爆炸但品質參差（社群測試 70% 不合格），真正能留在日常工作流的不到 10 個。與其追求數量，不如鎖定官方儲存庫（`anthropics/skills`）裡經過驗證的單一用途 Skill，搭配一個流程框架（如 Superpowers）打底。

## B（適用場景）

決定要裝哪些 Claude Code Skill 時的快速選單；需要具體安裝指令而非只是概念介紹。

## R（本文補充的 6 個官方單品 Skill，未見於既有 Superpowers/Karpathy/GStack 專篇）

| Skill | 一句話 | 安裝命令 |
|-------|--------|---------|
| **Frontend Design** | 讓 AI 生成的 UI 脫離「一眼看出來的 AI 風格」，安裝量 277,000+ | `npx skills add anthropics/skills --skill frontend-design` |
| **Webapp Testing** | 基於 Playwright 的 UI 自動化測試，說「測試這個登入頁面」自動跑點擊/輸入/驗證/報告 | `npx skills add anthropics/skills --skill webapp-testing` |
| **Security Review** | Claude Code 內建，推 PR 前自動語義掃描 SQL 注入/XSS/敏感資訊外洩/授權缺陷 | 內建，指令 `/security-review` |
| **MCP Builder** | 自然語言描述→自動生成完整 MCP 伺服器（骨架/工具定義/設定檔/部署腳本） | `npx skills add anthropics/skills --skill mcp-builder` |
| **Skill Creator** | 「元 Skill」，描述需求自動生成標準化 `SKILL.md` | 指令 `/skill-creator` |
| **Composio** | 連接器 Skill，讓 Claude Code 直接操作 Jira/Linear/Slack/GitHub 等 1000+ 外部應用 | `git clone https://github.com/ComposioHQ/awesome-claude-skills.git` 後 `claude --plugin-dir "$PWD/connect-apps-plugin"` |

**三種通用安裝方式**：`npx skills add <儲存庫名>`（最通用）／`/plugin install <skill名>@<市集名>`（Claude Code 插件市集）／`git clone <倉庫> ~/.claude/skills/<skill名>`（手動）。

**存放位置**：`~/.claude/skills/`（個人全域，不納入版控）vs 專案內 `.claude/skills/`（**最佳實務**：提交到 Git，團隊 clone 後自動獲得相同 Skill 能力）。

**四點使用建議**：不要貪多（裝 5 個精的優於 50 個雜的）／從 Superpowers 開始打底／按需新增（重複性工作先想能否做成 Skill，用 Skill Creator 快速固化）／注意 Skill 品質（好的 description 讀起來像路由規則而非宣傳語，核心 SKILL.md 精簡、細節推到 references/ 按需載入）。

## C（關鍵行動）

- [ ] 新裝 Skill 前，先確認官方 `anthropics/skills` 儲存庫是否已有現成單品，優先於社群自製版本
- [ ] 專案級 Skill 放進 `.claude/skills/` 並提交 Git，而非只裝在個人全域目錄
- [ ] PR 前安全檢查優先用內建 `/security-review`，不必額外裝第三方掃描 Skill

## 相關連結

- [[ai-agent-skills-popular-2026]] — Superpowers/Karpathy/Matt Pocock/GStack/GSD 五大框架深度比較（本文的 Superpowers/Karpathy/GStack 條目請見此篇，不重複展開）
- [[claude-code-skill-survivors-20-picks]] — 213k star Superpowers 精選 20 個倖存 Skill 的四大分類
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧，呼應本文「description 像路由規則」的品質標準
- [[ai-code-review-safeguards]] — Security Review 之外的 AI 生成程式碼防護層補充
- [[chatgpt-skills-usage-guide]] — ChatGPT Skills 使用教學（跨平台對照：OpenAI 版的 Skills 與 Claude Code 官方 Skill 生態）
