---
title: 用 AI 打造每日閱讀助手：每天推 400 字，把待讀清單讀完
date: 2026-06-30
tags: [ai/workflow, pkm/workflow, type/how-to]
source: https://www.bnext.com.tw/article/91377/ai-daily-reading-assistant-400-words
type: article
status: processed
asset_value: 55
---

VoAI 產品總監 Jack 分享的三步循環，把 AI 變成私人知識策展助手：**每天只推 400 字**，一次讀一點，慢慢把讀不完的收藏清單清空。解決碎片化時代「收藏一堆卻永遠讀不完、看長文耐心變低」的痛點。

## T（核心論點）

收藏 ≠ 學會。與其讓好文積灰或幻想一次讀完，不如用 AI 把內容拆成每日小份量、記錄進度避免重複，靠日積月累前進——關鍵是「絕不貪心，一次只讀一小段」。

## B（適用場景）

- 收藏清單越來越長、閱讀量下降、看長文耐心變低
- 想穩定吸收新知，把想讀的東西真的讀完
- 已有待讀清單，或想讓 AI 推薦學習主題

## R（三步循環 + 落地）

**當 AI 夠了解你 → 讓它挑主題**：請 AI 依你的工作/家庭/理財/在意的事推薦「適合學習的議題與書籍」，每個附推薦理由，別太仰賴排行榜。

**請 AI 把主題「造成一本書」**：從推薦挑感興趣主題，請 AI 從公開 metadata、網路資料與模型知識規劃章節架構，整理成附來源標注的 Markdown 衍生作品——先看清楚，再決定是否找原書。

**每天 9 點推 400 字**：要求 AI 記錄推送 log（讀到哪、接著哪）避免重複。實測**提醒 2 次**（7 點 + 9 點）才真的看得完。

**落地方式**：
- 用 Hermes Agent 運行，推送片段到 Telegram
- 嫌複雜可用 Codex：文稿放進專案資料夾 → Codex 與手機 ChatGPT app 連線 → 排程每天推送
- 已有清單者提示詞：「以下是我收藏的閱讀清單，分成 3–5 個主題章節，每章附 2 句說明。之後每天推一段約 400 字，記下進度避免重複。清單：[url]」

## C（對自身的啟發）

- [ ] 「每日推送 + log 記錄進度」可作為本知識庫消化積壓 wiki/待讀素材的機制
- [ ] Codex 排程 + 手機推送，呼應 claude-code 的 /schedule 雲端任務
- [ ] Hermes Agent 推 Telegram 的做法，與本環境的 Telegram 通道設計相通

## 相關連結

- [[hermes-agent]] — Hermes Agent 架構
- [[wenhao-yu-ai-workflow]] — 每日 AI 工作流（daily brief）
- [[claude-code-advanced-workflow]] — /schedule 排程
- [[karpathy-ai-knowledge-base-workflow]] — 對知識庫提問的閱讀流
