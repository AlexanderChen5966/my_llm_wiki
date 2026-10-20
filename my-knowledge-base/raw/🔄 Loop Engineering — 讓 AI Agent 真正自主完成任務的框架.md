---
title: "🔄 Loop Engineering — 讓 AI Agent 真正自主完成任務的框架"
source: "https://vocus.cc/article/6a299ddafd89780001f30503"
author:
  - "[[Cheng Jung Chen]]"
published: 2026-06-11
created: 2026-06-22
description: "2026-06-11如果你用過 Claude Code、Codex CLI、Cursor 或任何 coding agent，你一定遇過這種情況：你給了一個詳細的 prompt → agent 開始跑 → 跑了一半卡住 → 你手動修正 → 再給一次 prompt → 又卡在不一樣的地方。最後"
tags:
  - "clippings"
status: processed
---
如果你用過 Claude Code、Codex CLI、Cursor 或任何 coding agent，你一定遇過這種情況：

為什麼會看到廣告

你給了一個詳細的 prompt → agent 開始跑 → 跑了一半卡住 → 你手動修正 → 再給一次 prompt → 又卡在不一樣的地方。最後你花的時間比自己寫還多。

問題不在模型不夠強，在於你的工作流程還是「prompt → output → 人工 review → 修正 → 再 prompt」的線性模式。這不是 agent 的問題，是你的 loop design 有問題。

****Loop Engineering**** 是一套設計 feedback cycle 的框架，核心概念很簡單：把「你手動介入每一個步驟」變成「你設定目標，讓 agent 自己迭代直到 verified outcome」。

---

## 五個 Agent 死因（99% 的 loop 失敗都可以歸到這五類）

****1\. Context Collapse****

跑到第 12 步忘了第 1 步想做什麼。長任務吃掉 context window，模型繼續跑但已經偏離主線。 → 解法：把任務拆成 sub-loop，每個 sub-loop 有自己的 context + verifier。

****2\. No Self-Correction****

遇到錯誤就重試同一招，無限昂貴打轉。 → 解法：capture error → 獨立 call 分析 root cause → 換策略再試。禁止盲目 retry。

****3\. No Verifier****

做完不等於做對。而且 self-critique 不算數 — 產出 output 的那個 context 沒有能力客觀評判自己的 output。 → 解法：Worker 和 Verifier 必須是獨立的 API call。共用歷史的 verifier 會繼承 worker 的盲點。

****4\. No Guardrails****

Agent 可以亂刪檔案、亂 call API、亂花錢。 → 解法：定義 RULES.md，限制 terminal 權限、設定 budget cap、能用 read-only 就 read-only。

****5\. No Memory****

每次從零開始，同樣的錯誤重複犯。 → 解法：失敗時不存 raw log，而是萃取一條通用規則（RULE: concise principle），存檔，下次執行先載入。

---

## 核心架構：Worker + Verifier 分離

```javascript
LOOP CONTROLLER → GOAL + CONTEXT
                      │
               DISCOVER + PLAN
                      │
               WORKER (context A)
                      │  output
               VERIFIER (context B)
                      │  clean judgment
                     GATE
                    /    \
                 PASS    FAIL
                  │        │
                DONE    DIAGNOSE
                        提取規則
                        新策略 → WORKER
```

****關鍵規則**** ：Worker 用 context A，Verifier 用 context B。兩個獨立的 API call，不共享歷史。

實作起來就幾行：

```javascript
worker = client.messages.create(model="...", messages=[{"role": "user", "content": prompt}])

verifier = client.messages.create(
    model="...",
    messages=[{"role": "user", "content": f"Grade this output against this rubric:\n\nOUTPUT: {worker.text}\n\nRUBRIC: {rubric}"}]
)
```

---

## 分級路由

不要把每個任務都用最強模型。

| 任務類型 | 模型層級 |
| --- | --- |
| 資料萃取、格式化、樣板生成 | 便宜（Haiku / MiniMax） |
| 中等 code gen、review | 中階（Sonnet / DeepSeek V4 Flash） |
| 架構決策、困難 bug 診斷、最終驗證 | 最強（Fable 5 / Opus） |

大部分 iteration 很便宜，真正該花錢的是 ****驗證環節**** 。

---

## 兩種 Loop Scale

- ****Single-agent loop**** ：一個 agent 跑完整個 D→P→E→V→I 循環。適合任務明確的小目標。
- ****Fleet loop**** ：orchestrator 派 specialist sub-agent 平行工作。每個 sub-agent 跑自己的 loop。適合複雜專案。

Fleet loop 的關鍵：每個 sub-agent 有自己的 context、自己的 verifier、自己的記憶。不是把一個大 prompt 塞給一個 agent。

---

## Memory as Rules, Not Logs

這是被低估的一點。大多數人把失敗 log 堆起來，但 log 不會幫助 agent 進步。

正確做法：失敗時讓模型萃取出 ****一條通用規則**** ，存成可讀的文字檔案。下次 loop 啟動時先載入這些規則，agent 就不會重複犯同樣的錯。

```javascript
RULE: 修改 SQL schema 時永遠先備份 production 資料
RULE: 不要假設 API response 包含所有欄位，先 validate
RULE: 第三方套件升級後跑完整 test suite，不只 smoke test
```

三個月後你會有幾十條這種規則。這比任何 vector memory 機制都實用。

---

## 資源

****GitHub**** ：github.com/lunkerchen/loop-engineering-skill（MIT License，雙語文件）

****Hermes 直接安裝**** ：

```javascript
hermes skills tap add lunkerchen/loop-engineering-skill
hermes skills install loop-engineering
```

Tags: [#LoopEngineering](#LoopEngineering) [#AIAgent](#AIAgent) [#HermesAgent](#HermesAgent) [#OpenSource](#OpenSource)