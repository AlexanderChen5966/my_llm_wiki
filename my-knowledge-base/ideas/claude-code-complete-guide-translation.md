---
title: Claude Code 完全攻略指南（完整入門譯文備查）
date: 2026-07-20
tags: [ai/claude, type/how-to]
source: https://codelove.tw/@tony/post/qv2Ewq
type: idea
status: processed
asset_value: 42
---

codelove（Tony 翻譯自 Qiita s-furuya-nri）的 Claude Code 完全攻略指南，15 章從安裝到 CLAUDE.md、自訂代理、Skills、MCP、Hooks、CI/CD、並行代理的體系化綜覽譯文。體系雖完整但與知識庫既有條目高度重疊——該主題已飽和，不另立 wiki，放 ideas 作為完整入門譯文備查。

**15 章結構**：①Claude Code 是什麼 ②安裝設定 ③基本用法 ④上下文管理（/init、@檔案、/compact、/clear）⑤對話控制（快捷鍵/斜線指令/檢查點回溯）⑥Plan Mode 與 Thinking Mode（think/think hard/ultrathink）⑦.claude 目錄全貌（全域→專案→本機三層優先序）⑧CLAUDE.md 撰寫技巧（100-300 行/具體到 AI 可判斷/ASCII 依賴圖/反模式）⑨自訂代理與指令（Agent=誰、Skill=知道什麼、Instruction=怎麼做）⑩Skills（Progressive Disclosure 三層/description 寫法）⑪MCP 串接 ⑫Hooks 自動化（PreToolUse/PostToolUse/Stop）⑬GitHub 與 CI/CD（claude -p 非對話模式）⑭並行代理 4 種方法（子代理/代理視圖/代理團隊/動態工作流程）⑮總結。

**與既有條目重疊對照**（該主題已飽和的佐證）：
- 最完整實踐 → [[claude-code-community-workflow-2026]]（asset_value 100）
- 新手入門 → [[claude-2026-beginner-guide]] / [[claude-code-newbie-guide]]
- CLAUDE.md 技巧 → [[claude-md-guide]] / [[claude-md-optimization-techniques]]
- Skills → [[claude-skills-beginner-guide]]
- 並行代理/動態工作流 → [[claude-code-dynamic-workflows]]
