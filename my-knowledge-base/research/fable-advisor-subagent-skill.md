---
title: fable-advisor：用 Fable 5 子代理充當決策諮詢顧問的 Claude Skill
date: 2026-07-08
tags: [ai/claude, ai/agent]
source: https://github.com/echo-of-machines/fable-advisor
type: reference
status: processed
asset_value: 42
---

echo-of-machines/fable-advisor 是一個 Claude Skill，用來在 Claude Code 中模擬官方 API 才有、Claude Code 沒有的「advisor tool」：在關鍵決策點動態呼叫一個更強的模型子代理提供第二意見，而不是全程用同一個模型硬撐。

## 核心論點

工作分層設計：**Opus 統籌（orchestrator）、Sonnet 做一般 worker、Fable 5 只在真正的決策點被諮詢**。這不是把 Fable 5 當主力模型用，而是把它當「稀缺、昂貴、只在關鍵時刻出手」的顧問角色，藉此在成本與品質之間取得平衡。

## 兩段式安裝（容易漏掉的地方）

Skill 分成兩個獨立部分，缺一不會自動生效：

1. **`SKILL.md`**（放在 skill 資料夾內）——諮詢的「程序」：何時該諮詢、怎麼包裝 context（聚焦簡報 vs 唯讀調查兩種模式）、如何權衡建議。只裝這部分只能用 `/fable-advisor` 手動觸發。
2. **`~/.claude/CLAUDE.md` 裡的一條規則**（不在 skill 資料夾內）——讓諮詢在決策點自動觸發的常駐指令。Skill 本身無法自我觸發這種 always-on 的 meta 行為，自動觸發那一半必須寫進全域 CLAUDE.md。

**換機器時的陷阱**：只搬 skill 資料夾，自動觸發不會跟著走，必須另外把 CLAUDE.md 規則也搬過去。

## 觸發條件與限制

- **只在 main model 是 Opus 時才觸發**：Fable 5 諮詢 Fable 5（若 main 已經是 Fable）是純浪費；main 是 Sonnet/Haiku 則此設定不適用。使用前需先確認目前 session 的實際 main model。
- **觸發時機**：決定採用一個非顯而易見的做法之前、卡住的時候、要改變做法之前、宣告任務完成之前（先讓交付物 durable）。
- **要克制**：Fable 是最貴的層級，會最快耗盡共用額度上限。只留給非瑣碎任務的高價值決策，不用於例行/反應性操作或簡單查詢——粗抓：一個任務大約「動手前諮詢一次、完工前諮詢一次」。
- 需要 Fable 存取權限與 Anthropic API（advisor/Fable 層級不支援 Bedrock / Google Cloud / Foundry）。

## 與本知識庫的關聯

- 與 [[claude-fable-5-usage-guide]] 的「fresh-context verifier subagent」原理相通，但角色不同：verifier 是驗證已完成的工作，advisor 是在動手前徵詢意見——兩者可以並存於同一個長任務流程。
- 呼應 [[fable-workflow-institutionalization-plan-2026-07-08]] 提出的「驗證迴圈原則化」缺口：這個 Skill 提供了一個具體、可直接參考安裝的實作範例，說明如何把「何時該用更貴的模型把關」寫成明文規則而非隱性判斷。
- 與 [[loop-engineering-6-components]] 的分級路由表（便宜模型做例行事、最強模型留給架構決策/困難診斷/最終驗證）同一設計哲學的具體實踐。

## 補充價值

作為機制型素材，最大用途是提供一個可直接安裝、驗證過的範本，用來設計「多模型分層協作」的自訂 CLAUDE.md 規則，而非本知識庫自建同類機制時要重新發明格式。
