---
title: The Founder's Playbook — 打造 AI 原生創業公司
date: 2026-05-25
tags: [ai/workflow, ai/agent, ai/claude]
source: Anthropic 官方 PDF（2026-05-06 v3，英文原版 35 頁 / 簡體中文翻譯版 42 頁）
type: reference
status: processed
asset_value: 75
source_format: pdf
---

## T（核心論點）

Anthropic 官方出版的 AI 原生創業指南，核心主張：**AI 消除了技術門檻，創辦人的角色從執行者轉變為 AI Agent 的指揮官**。手冊以四階段框架（Idea → MVP → Launch → Scale）逐一說明每個階段的目標、陷阱、離場條件，以及 Claude Chat / Claude Cowork / Claude Code 三個工具各自的適用時機。AI 時代創業的最大風險不是建不出來，而是「建錯了」——過早建構、過早擴張、技術債複利化。

## B（適用場景）

**適用：**
- 有意創業或正在 Idea/MVP 階段的創辦人，需要了解 AI 工具在各階段的角色分工
- 想了解 Anthropic 對 Claude Chat / Claude Cowork / Claude Code 定位差異的讀者
- 規劃 AI 原生產品架構，想了解 moat（護城河）建立策略的人

**不適用 / 注意：**
- 本書以 Anthropic 工具生態為主，Google / OpenAI 工具生態需另外參考
- 四個階段框架適合 B2C/B2B SaaS 類產品，硬體或高度監管產業需調整

## R（操作步驟）

### 三工具分工原則
| 工具 | 定位 | 適用場景 |
|------|------|---------|
| Claude Chat | 隨時可用的對話顧問 | 快速問答、腦力激盪、一次性任務 |
| Claude Cowork | 知識型自動化執行層 | 市調整合、文件起草、CRM 管理、定期報表 |
| Claude Code | Agentic 編程環境 | 生產程式碼、架構設計、MVP 建構 |

### 四階段框架

**Idea Stage**
- 目標：找到 problem-solution fit（不是建原型，是驗證）
- 離場條件：問題是否真實具體？方案是否解決真實問題？有足夠訊號可以開始建？
- 風險：把建原型當驗證、確認偏誤（AI 會找出你想要的答案）、過早擴張

**MVP Stage**
- 目標：把驗證過的問題轉化為真實用戶會回訪的產品
- 離場條件：出現真實的 PMF 訊號（Sean Ellis Test：>40% 用戶說「非常失望」）
- 風險：Agentic 技術債（跳過架構文件）、假 PMF（初期熱潮誤判）、零摩擦 scope creep

**Launch Stage**
- 目標：讓早期牽引力變成可重複的成長引擎，同時建立公司運作系統
- 離場條件：成長有重複性與可預測性；產品可承受生產負荷；營運不再需要創辦人親自執行
- 風險：創辦人成為瓶頸、技術債到期、安全與合規問題浮現

**Scale Stage**
- 目標：建立可防禦的護城河（domain expertise + workflow lock-in + user data 複利）
- 離場條件：可持續獲利、IPO 準備度，或被收購資格
- 風險：過早拓展新市場、GTM 函式尚未建立、創辦人角色未轉型為 CEO

## C（關鍵行動）

- [ ] 閱讀 Idea Stage 的「Defining and pressure-testing the problem hypothesis」練習，用於下一個新功能的需求驗證
- [ ] 參考 MVP Stage 的 CLAUDE.md 架構文件建立方式，確認 b2b-manager 的架構決策有文字記錄
- [ ] 研究 Launch Stage 的「Build the systems that replace founder attention」——是否有可以自動化的重複性流程？
- [ ] 記錄 Scale Stage 的 workflow lock-in 概念，作為 B2B 產品護城河設計的參考

## 原始摘要

Anthropic 官方出版的 35 頁創業指南（2026-05-06 v3），由 Claude 品牌設計。結構為 7 章：
1. The startup lifecycle, rebooted for 2026
2. What it means to be a founder is changing
3. Idea Stage（研究驗證、顧客探索、競品分析）
4. MVP Stage（架構文件、scope 定義、PMF 量測）
5. Launch Stage（技術債清償、安全合規、營運系統化）
6. Scale Stage（組織成熟化、GTM 建立、workflow lock-in）
7. Same job, new rules（AI 壓縮了每個階段的時程）

核心結語：「瓶頸不再是你能建什麼，而是你選擇建什麼。」

## 相關連結

- [[wiki/agentic-engineering-mindset]] — Agentic Engineering 思維框架
- [[wiki/claude-code-advanced-workflow]] — Claude Code 實戰工作流
- [[wiki/claude-md-guide]] — CLAUDE.md 架構文件最佳實踐
