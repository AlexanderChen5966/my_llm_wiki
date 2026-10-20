---
title: Vibe Coding vs Agentic Coding：產品成熟度決定用哪種方法
date: 2026-05-28
tags: [ai/agent, dev/architecture]
source: https://yu-wenhao.com/zh-TW/blog/agentic-coding-guide/
type: article
status: processed
asset_value: 48
---

## T（核心論點）

Vibe Coding 和 Agentic Coding 不是二選一，而是對應不同產品成熟度的工具：前者如 3D 列印做 mockup，後者如射出成型做量產——問題不是哪個好，而是你的產品在哪個階段。

## B（適用場景）

**適用：**
- Demo / POC → Vibe Coding（快、夠用、探索方向）
- MVP 過渡期 → 逐漸引入 Agentic 工程紀律
- Production / Scale → Agentic Coding（測試、安全、可維護、CI/CD）

**不適用 / 注意：** 用 Vibe Coding 做要上線收費的產品就像用 3D 列印量產——品質不可控、出問題無法系統性改善、累積毀滅性技術債。

## R（操作步驟）

1. 判斷產品目前的成熟度階段（Demo/MVP/Production/Scale）
2. Demo：直接 Vibe Coding，不需要過度設計
3. MVP 轉 Production：開始引入 AGENTS.md / CLAUDE.md、自動化測試、任務拆分文件
4. Production 起：採用完整 Agentic Coding 工程紀律（spec-first、CI/CD、品質閘門）
5. Scale：多 Agent 協作 + 監控 + 自動化品質管理

## C（關鍵行動）

- [ ] 在用 Vibe Coding 之前先問：「這個東西要上線嗎？需要維護嗎？」
- [ ] Production 環境不能只靠語感，確認有 spec 文件和驗證機制
- [ ] 不要在 Demo 階段用 Agentic Engineering 的重量級工具，浪費時間

## 原始摘要

WenHao Yu（2026-02-14）：做了 10 年 PM 的作者用硬體製造類比解釋兩種方法。軟體等級對照：Demo = Mockup（3D 列印）；MVP = A/B Sample；Production = PVT；Scale = MP（量產線）。5 大工具比較、產品成熟度框架、可複製模板。

## 相關連結

- [[agentic-engineering-sop]]
- [[agentic-engineering-mindset]]
- [[wenhao-yu-ai-workflow]]
- [[synthesis-agentic-engineering]]
- [[ai-architecture-survival-guide-non-architect]] — 同主題基層操作視角：本篇談成熟度分級，該篇談不懂架構時的六個生存習慣與五個警訊
