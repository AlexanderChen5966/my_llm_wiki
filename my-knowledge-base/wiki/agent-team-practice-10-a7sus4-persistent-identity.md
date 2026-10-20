---
title: "Agent Team 實戰（十）：A7sus4——用 Production-Precursor 解決稽核 Agent 的身份錨點問題"
date: 2026-05-01
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/33-agent-team-a7sus4/
type: article
status: processed
asset_value: 60
---

「Agent Team 實戰」系列第十篇（作者 EMil Wu）。A7（dispatched event monitor／稽核 Agent）規格已明確，作者卻沒有直接建 A7，而是先建 A7sus4 作為承擔 production duty 的過渡角色。本篇是系列中「個案」層級的具體記錄，第十一篇則把此個案抽象成通用的四層持久化框架。

## T（核心論點）

> Ephemeral subagent 監督者容易把自己理解成「等待者」（wait-mode），根因不是 prompt 寫得不夠好，而是**身份沒有錨點**——沒有工作目錄、CLAUDE.md、長期 state、持續 lifecycle。修正方式是把角色做成 persistent process（A7sus4），而非把 prompt 寫更長；且可以誠實承認這是會真的承擔 production 責任的 **production-precursor**，而非假裝自己只是 throwaway prototype。

## B（適用場景）

**適用：**
- 需要建立「稽核者／監督者」型 Agent，且該角色需長期累積趨勢資料、跨多次任務成長
- 目標 Agent 的完整規格（如本例 A7 的 Role 1 daemon）尚未成熟，但某個子角色（Role 3）的需求已明確、且現場已有問題發生，不能等全部想清楚才動手
- 需要判斷「這個任務值不值得 live monitor」的場景

**不適用 / 注意：**
- Signal-density 太低的任務（沒有 task archive、outbox、done JSON 等 filesystem signal）不該啟動 live monitor，應改用 post-hoc static audit 或 GM inspection，硬上只會讓 Agent 陷入等待者心態
- 稽核者必須 observe-only，不可直接修正被稽核對象，否則會失去獨立性、演變成疊床架屋的稽核鏈

## R（稽核 Agent 建構方法論五點）

1. **從失敗資料開始建，不從角色名稱開始建**：抽象 spec（「負責監控、驗證、報告」）離落地還有距離，A7 是靠累積 silent failure、self-validation、SAA wait-mode、completion report 不足、skill drift、external-service footgun 等實際案例才變清楚。
2. **把 capability、lifecycle、trigger/output 三軸分開設計**：Log Audit／Cross-Agent Validation／Health Monitoring 是 capability；daemon／periodic 是 lifecycle；Role 1／Role 2／Role 3 是 operational mode。三軸分開後，設計從「一個很大的 Agent」變成「可以分階段驗證上線的系統」。
3. **稽核者必須 observe-only**：只 observe、report、recommend，修正交由 GM 和 Dm 執行，避免需要再建 A9 稽核 A7、A13 稽核 A9 的疊疊樂。
4. **Live monitor 要有 signal-density preflight**：目標任務在 audit window 內需至少兩個以上預期 filesystem signal（task archive、outbox、done JSON、primary artifact）才值得啟動 live monitor，低於門檻應拒絕 spawn 或標記 degraded-confidence。
5. **可以先建 production-precursor，誠實承認它會承擔真實 production 責任**：A7sus4 不是 throwaway prototype，會真的執行 audit duty、累積 lessons，並設有 retirement clause（等 A7 1.x 跑過實際 audit cycle 後才退場）。

第一次 live audit（PASS，3 cycle 無 self-alert）驗證框架可行後，A7sus4 進一步把 deviation 分類：PASS-with-deviation（產出長度偏離 spec）、PASS-with-anomaly（spec 不完整但執行滿足列出要求）、FAIL-silent-completion（stop-hook 觸發但 primary artifact 未出現）——不只判斷 pass/fail，還區分「執行錯誤 vs 規格不一致 vs 系統性趨勢」。

## C（關鍵行動）

- [ ] 建稽核型 Agent 前，先盤點目前手上有哪些具體失敗案例，而非直接寫抽象 spec
- [ ] 為 live monitor 類任務設定 signal-density 門檻（至少 2 個預期 filesystem signal），低於門檻改用 static audit
- [ ] 稽核 Agent 的輸出至少分三級（PASS / PASS-with-deviation-or-anomaly / FAIL），並標註問題屬於執行錯誤、規格不一致還是系統性趨勢
- [ ] 若某角色規格未完全成熟但現場已有問題，考慮先建 production-precursor 並明訂 retirement clause，而非等全部想清楚

## 原始摘要

作者在 A7 規格明確後沒有直接建 A7，而是先建 A7sus4 承擔 Role 3 production duty，因為臨時 ephemeral subagent 監督者容易理解成「等待者」（出現 wait-mode、passive idiom），根因是身份沒有錨點，故改為常駐於 Overmind 的 persistent process，擁有自己的目錄、CLAUDE.md、audit-cycle skill、state、work-log。文中提出 signal-density preflight（訊號太少的任務不值得 live monitor）及稽核者方法論五點，並記錄第一次 live audit 抓到的 PASS-with-deviation、PASS-with-anomaly、FAIL-silent-completion 等具體異常分類。核心結論：「一個 Agent Team 的成熟，不是它不再犯錯，而是它開始有能力看見自己的錯」。

## 相關連結

- [[agent-team-practice-09-a7-audit-agent]] —— 系列前作，承接關係：本篇「A7 規格已明確」直接承接第九篇提出的 capability/lifecycle/operational role 三正交軸拆解法（同日 2026-05-01 發表，九是十的直接前置篇）
- [[agent-team-practice-11-persistence-four-layer-framework]] —— 個案→框架化的前後承接：本篇是 A7sus4 具體個案的完整記錄，第十一篇把「persistent identity」這個觀察抽象成通用的記憶／任務／身份／觀察四層持久化框架，A7sus4 是該框架「身份持久化」層的核心例證
