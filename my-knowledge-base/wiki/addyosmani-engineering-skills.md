---
title: Addy Osmani 20 個工程師 Skill 實戰
date: 2026-05-13
tags: [ai/agent, dev/architecture]
source: https://www.itnotetk.com/2026/04/21/addyosmani-agent-skills-engineering-discipline/
type: article
status: processed
asset_value: 72
---

## T（核心論點）
Addy Osmani（Google Chrome 前端性能傳奇）把「老工程師知道但新人沒踩過的事」封裝成 20 個不可跳過的 Skill checklist——把資深工程師的審慎判斷變成 AI 可執行的模組。

## B（適用場景）
**適用：** 想把工程師最佳實踐封裝進 AI 工作流；code review 和品質把關場景
**不適用：** 非工程類的 Skill 設計

## R（操作步驟）
參考 addyosmani/agent-skills 的 AGENTS.md 結構：
1. 每個 Skill 對應一個工程師最佳實踐（安全性、性能、可維護性等）
2. Skill 設計為可組合：多個 Skill 疊加等於深度 review
3. 搭配 AGENTS.md 放在 repo 根目錄，讓 AI 進入專案時自動載入

## C（關鍵行動）
- [ ] 參考 GitHub: addyosmani/agent-skills 的結構
- [ ] 優先把最容易出錯的工程判斷做成 Skill
- [ ] Skill 要有具體的檢查標準，不能只寫「注意安全性」

## 原始摘要
Addy Osmani 的 agent-skills 專案包含 production-grade 的工程 Skill 集合。這篇文章拆解其中 20 個核心 Skill 的設計邏輯，特別強調「把隱性的工程師直覺轉為明確的 checklist」。

## 相關連結
- [[agents-md-guide]] — AGENTS.md 的設置方式
- [[claude-skills-guide]] — Skills 的兩種核心用法
