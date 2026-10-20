---
title: Claude Cowork 自動化：用 memory + connector 找出值得自動化的工作
date: 2026-06-22
tags: [ai/claude]
source: https://www.bnext.com.tw/article/91264/claude-memory-connector
type: article
status: processed
asset_value: 50
---

問 AI「我有哪些工作可以自動化」，第一輪答案多半是猜的。打開記憶、連上 connector，AI 才能根據你**真的做過的事**給出有用建議。

**四步驟：**
1. 開啟 Claude 記憶（Settings → Capabilities：搜尋過去對話 + 記下脈絡）
2. 用幾次對話讓 AI 認識你的工作，記憶形成「工作背景/個人背景/最近在做的事」三類
3. 用 memory + connector 聯合提問：`「根據我的 memory、已安裝的 connector，建議我可以做的自動化工具、自訂工作流（skill）或排程任務。」`
4. 三個篩選問題：每月做 ≥3 次嗎？目的說得清楚嗎？步驟說得出來嗎？

**關鍵 prompt：**
```
根據我的 memory，加上我可以裝的 connector，建議我哪些工作適合做成
自訂工作流、排程任務或寫進 memory。每項給：分類理由、設定時間、踩坑提醒。
```

核心邏輯：先讓 AI 認識你，再問它對你的建議。
