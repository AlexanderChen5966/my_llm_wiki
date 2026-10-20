---
title: "Agent Team 實戰（十一）：Long-Run Persistence 四層框架——記憶／任務／身份／觀察"
date: 2026-05-01
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/34-agent-team-long-run-persistence/
type: article
status: processed
asset_value: 80
---

「Agent Team 實戰」系列第十一篇（作者 EMil Wu）。以「半夜排 remote agent 寫新聞草稿」的場景切入，把前一篇 A7sus4 個案與更早的記憶持久化討論收斂成通用的 long-run persistence 四層框架。本篇是系列中「框架化」層級的收束篇。

## T（核心論點）

> **持久化不是讓 Agent 一直跑，而是讓他停下來、換環境、醒來之後，仍然知道自己是誰、要去哪裡找狀態、怎麼交回成果。** 記憶能持久，不代表執行能持久——long-run Agent Team 需要記憶、任務、身份、觀察四層互相接上，才能讓 AI 停下來後仍能被叫回來、找到狀態，而非追求「無人化」。

## B（適用場景）

**適用：**
- 排程／遠端 Agent（如 `/schedule` 的 remote routine）在無共享 filesystem 的環境下執行任務，需要設計交接機制
- 需要長期承擔責任的 Agent（如稽核者、監控者）要決定該用 scheduled identity 還是 persistent identity
- 團隊要制定「哪些自我維護可以全自動、哪些架構演進需要人類 challenge」的邊界規則

**不適用 / 注意：**
- 本機 session 有共享 filesystem，可直接靠 repo artifact、hooks、inbox/outbox 交接，不需要套用 remote routine 那套「自我完備 prompt」設計
- 高風險架構演進（改 Agent 邊界、dispatch 模式、Memory Extraction Rules、稽核標準）不可全自動，須有人類 challenge 或獨立 cross-audit agent 介入

## R（四層持久化框架）

1. **記憶持久化**（State + Wiki + mem0 + QMD）：State 用 Git 記「現在做到哪裡」、Wiki 記「我們知道什麼」、mem0（Qdrant Cloud）記「為什麼這樣決定」、QMD 讓 wiki 可 search-first 而非每次塞進 context。解決「換 session 之後還找得到脈絡」的問題，但不保證任務能跨時間完成。
2. **任務持久化**（Prompt + artifact + branch）：Remote routine 沒有共享 filesystem，唯一穩定的交接面是 Git。Prompt 必須寫成一份自我完備的工作規格（HANDOFF.md 等級）：誰、repo 是什麼、讀哪些來源、輸出路徑、寫作風格、commit/push 流程、失敗時如何回報。核心原則：**跨環境執行時，不要把 Local 的一切當作理所當然，要共享 artifact**。Git 在此不只是版控，而是跨時間、跨環境的交接介面。
3. **身份持久化**（Directory + CLAUDE.md + lifecycle）：長期承擔責任的 Agent 需要自己的目錄、CLAUDE.md、state、work-log 與 lifecycle，不能只靠更長的 prompt（引用 A7sus4 為核心例證）。分兩種形態：scheduled identity（每次啟動靠 prompt 重建身份，適合一次性研究、定期掃描、固定格式報告）與 persistent identity（身份存在於本機 process 和 repo artifact，適合需累積趨勢、觀察現場、回寫 history 的工作）。兩者無高低之分，差別在持久化成本是否合理。
4. **觀察持久化**（Audit + finding + channel）：Long-run 最危險的不是單次任務失敗，而是錯誤發生了卻沒被發現。A7 的 observe-only scope 把低階事件整理成 finding，送到正確 channel：live bug 進 team-status、方法論洞察進 wiki、特定 dispatch 觀察留在 work-log、架構級問題進人類 review。目標不是「永遠不要錯」，而是「錯誤要可以被察覺，不要無聲無息地消失」。
5. **自動化邊界原則**：低風險自我維護（wiki 去重、skill drift 偵測、completion artifact 檢查、重複錯誤 pattern 收斂）可全自動；高風險架構演進需人類 challenge 或獨立 cross-audit agent 介入——「可以讓 Agent 在需要判斷的地方找你，但不要假裝判斷本身已經可以完全交給他」。

## C（關鍵行動）

- [ ] 設計 remote/排程任務的 prompt 時，比照 HANDOFF.md 標準寫全：身份、repo、來源、輸出路徑、風格、commit/push 流程、失敗回報方式
- [ ] 為長期責任型 Agent 判斷該用 scheduled identity 還是 persistent identity，依「任務是否需要跨次累積趨勢」決定
- [ ] 為系統列出「可全自動的低風險自我維護清單」與「需人類 challenge 的高風險架構變更清單」，作為自動化邊界的判準
- [ ] 稽核／觀察類 Agent 的輸出要能分流到正確 channel（team-status / wiki / work-log / human review），而非全部塞進同一份 log

## 原始摘要

以「半夜排 remote agent 寫新聞草稿」的場景切入，提出 long-run persistence 的四層框架：記憶持久化（State/Wiki/mem0/QMD）、任務持久化（remote routine 靠自我完備的 prompt + git branch 交接，因為沒有共享 filesystem，Git 是跨環境的交接介面而非單純版控）、身份持久化（承擔長期責任的 Agent 需要目錄、CLAUDE.md、state、lifecycle，引用 A7sus4 為例）、觀察持久化（把 silent failure 整理成 finding，送到正確 channel）。結論：持久化的目的不是無人化，而是讓人不用待在錯的 loop 裡，讓 AI 停下來後仍能被叫回來、找到狀態。

## 相關連結

- [[agent-team-practice-10-a7sus4-persistent-identity]] —— 個案→框架化的前後承接：第十篇記錄 A7sus4 這個具體個案（為何不直接建 A7、先建 production-precursor），本篇則把該個案抽象成通用的記憶／任務／身份／觀察四層持久化框架，A7sus4 的「persistent identity」被本篇當作身份持久化層的核心例證
