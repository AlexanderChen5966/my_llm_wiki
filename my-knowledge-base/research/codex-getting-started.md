---
title: Codex 入門：AI 工作代理人完整教學
date: 2026-05-20
tags: [ai/agent, ai/workflow, ai/prompt-engineering]
source: https://www.bnext.com.tw/article/90840/getting-started-with-codex-as-your-ai-work-agent
type: article
status: processed
asset_value: 45
---

Codex 是 OpenAI 的 AI 工作代理人，能在指定資料夾中建立、編輯、整理檔案，直接交付可用的工作成果。2026 年 4 月大更新後支援瀏覽器預覽、macOS 應用程式操作、PDF/Excel/簡報直接側欄預覽。

## 與 Claude Code 的關係

Codex 和 Claude Code 定位相似：都可以在指定的專案資料夾工作，讀寫檔案，直接交付可用成果（不只是聊天回覆）。主要差異在底層模型（GPT-5.5 vs Claude）和 UI 設計。

## 3 大提示詞原則

來源：Riley Brown「Codex Full Course 2026」歸納：

1. **要求交付成果，不要描述過程**：說「建立一個 Excel 表格列出 X，存到這個資料夾」，而非「幫我整理一下 X 的資訊」
2. **給出明確的成功標準**：「直到表格有 20 列、可以在 Excel 打開、欄位包含 A/B/C 才算完成」
3. **分解複雜任務**：大任務拆成多個小任務，每個小任務有明確的驗收條件

## 專案資料夾設定（關鍵）

新手最常跳過但最重要的步驟：先建立專案資料夾，再讓 Codex 在裡面工作：

```
Codex projects/
├── my-research/        ← 研究類任務
└── my-new-business/    ← 創業/產品任務
```

在 Codex 介面指定資料夾後，所有產出的檔案都會存在本機，可以直接用。

## 相關連結
- [[codex-vs-claude-code]] — Every CEO 實測 6 種工作流：為何從 Claude Code 投奔 Codex
- [[codex-plugin-workflow]] — Codex Plugin 可重用工作流模組系統
