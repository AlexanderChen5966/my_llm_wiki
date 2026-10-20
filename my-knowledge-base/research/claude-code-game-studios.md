---
title: Claude Code Game Studios：49 Agent 遊戲工作室架構
date: 2026-05-28
tags: [ai/agent, dev/architecture]
source: https://github.com/Donchitos/Claude-Code-Game-Studios
type: reference
status: processed
asset_value: 27
---

## 核心論點

Claude Code Game Studios 用 49 個專業化 Agent + 72 個 Skills，把單一 Claude Code session 打造成有層級結構的遊戲開發工作室，核心洞察是：**AI session 需要組織結構，而非通才助手**——Director 守護願景、Department Lead 擁有職能域、Specialist 執行落地工作。

## 架構設計原則

| 層級 | 角色 | 職責 |
|------|------|------|
| Directors | 創意總監、技術總監 | 守護整體願景，擋掉偏離決策 |
| Department Leads | 程式/美術/音效/敘事/QA Lead | 擁有各自職能域，設定標準 |
| Specialists | 具體執行者 | 實際開發、設計、測試工作 |

**49 個 Agent 覆蓋範圍**：設計、程式、美術、音效、敘事、QA、製作管理——完整對映真實遊戲工作室組織圖。

## 對一般 Agent 架構的啟示

即使不做遊戲，這個架構示範了幾個可移植的原則：

1. **職能分離 > 通才**：一個 session 裡有清晰職能的 Agent 比「什麼都做的 Claude」更穩定
2. **向上升級路徑**：每個 Agent 知道何時該把問題升到 Lead 層
3. **品質閘門**：QA Agent 在每個交付物前強制檢查，防止累積技術債
4. **72 Skills 組織**：Skill 按職能域分組，不是散亂的 prompt 集合

## 相關連結

- [[agentic-engineering-sop]]
- [[agent-skill-design-patterns]]
- [[synthesis-agent-framework-philosophy]]
