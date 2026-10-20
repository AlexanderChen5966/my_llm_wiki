---
title: Claude Code 官方 Prompt Library：52 組提示詞按開發五階段分類
date: 2026-07-09
tags: [ai/claude, ai/prompt-engineering, type/how-to]
source: https://www.bnext.com.tw/article/91421/claude-code-prompt-library-52-prompts
type: article
status: processed
asset_value: 62
---

Anthropic 官方 Claude Code 文件中的 Prompt Library，收錄 52 組可直接複製貼上的提示詞，來源含 Common workflows、Best practices、Anthropic 團隊實務、法務/行銷/資安團隊報告，按 Discover / Design / Build / Ship / Operate 五階段分類，部分標記適合 PM、設計師、行銷、資安、維運等非工程角色。

## T（核心論點）

52 組提示詞背後共享 6 個寫法規律，理解規律比背誦提示詞更重要：**描述結果不描述步驟**（讓 Claude 自己找檔案）、**給自我驗證方式**（同一提示詞內要求執行並驗證，才會自己迭代不會做一步就停）、**給參照物**（指定現有檔案/模式當參考，Claude 才會照專案風格而非套用一般最佳實踐）、**給可量測目標**（效能/覆蓋率給具體數字）、**直接給原始素材**（log/截圖/plan output 貼入或用 @ 引用，效果遠優於描述）、**指定回答格式**。

## B（適用場景）

工程師寫程式/測試/重構/審查/除錯；PM 評估改動範圍、寫規格書；設計師估算 UI 影響、原型比對；行銷/文件撰寫批次改文案；資安/維運跑安全審查與事件調查。

## R（五階段重點）

- **Discover 探索**：描述「想知道什麼」而非指定讀哪些檔案，搜尋用「行為」非「檔名」；後兩組適合 PM/設計師評估改動範圍（如 `which files would I need to touch to {add a dark mode toggle}?`）
- **Design 設計**：刻意把「探索」和「動手」分開（先列檔案但不動）；訪談式寫規格書讓 Claude 問問題而非你描述需求
- **Build 實作**：實作/測試/重構/審查/修正五個子類。測試類把「寫、跑、修」放同一提示詞才會迭代到通過；重構類需給可量測完成標準；審查類讓 Claude 讀整個代碼庫上下文而非只看 diff；修正類把糾正寫進 CLAUDE.md，一次修正全隊受益
- **Ship 發布**：從 diff/commit 歷史推導說明而非你描述做了什麼，自動對齊既有風格
- **Operate 維運**：除錯只需描述症狀不用指出檔案；事件調查列出要關聯的證據來源（log/部署/設定變更）讓 Claude 交叉比對；自動化把重複動作存成 skill、hook 把「每次要記得問」變自動觸發

## Prompt Library 效果打折的三種情況

1. **跨 session 用同一提示詞但沒持久化背景**——每次新 session 從新視窗開始，需寫入 CLAUDE.md 或用自動記憶功能
2. **需要外部工具但沒接**——查 log 需先接 MCP server、開 PR 需 gh CLI/GitHub connector
3. **對 vibe coding 有期待但代碼庫缺乏約束**——沒測試、沒 CLAUDE.md、沒規範，輸出品質與一致性會不穩定

## C（關鍵行動）

- [ ] 寫 Prompt 前檢查是否符合 6 個規律（結果非步驟/自我驗證/參照物/可量測/原始素材/回答格式）
- [ ] 工作階段結束前讓 Claude 整理當次學到的內容寫入 CLAUDE.md
- [ ] 高風險提示詞（審查/安全）先確認前置工具（MCP/gh CLI）已接好

## 相關連結

- [[claude-code-commands-reference]] — Claude Code 斜線指令速查（52 組提示詞是自然語言而非指令）
- [[claude-code-commands-2026]] — 2026 新命令解析
- [[agent-era-prompt-structure]] — Agent 時代 Prompt 五要素（Context/Request/Output/Constraints/Checkpoint，互補框架）
- [[synthesis-prompt-engineering-evolution]] — Prompt Engineering 演進精粹
