---
title: Antithesis 時間旅行除錯的通則：可被推翻的條件、固定現場、一次一個假設
date: 2026-11-05
tags: [ai/agent, dev/testing]
source: https://www.aiposthub.com/antithesis-debug-practical-prompt-workflow-guide/（aiposthub／Philo，2026-10-23）；官方公告 https://antithesis.com/blog/2026/time-traveling-agents/
type: idea
status: processed
asset_value: 9
---

> 🔴 **來源與適用**：Antithesis 是付費平台，功能為 **2026-10-23 快照**（站方核對日；Release 64）。官方功能主張屬**廠商自稱，未獨立驗證**；站方明說**未實測**、未補造完整命令。一般專案無法直接套用，本條只抽出不依賴平台的通則。
> 為何放 ideas：asset_value 9，產品細節不收；留下四條除錯通則。

## T（核心論點）

Antithesis 在 2026-10-15 宣布，可由 API／CLI（Snouty）讓代理回到**確定性重播的失敗現場**調查問題。前提是**系統已接入 Antithesis 且為 Release 64**。拿掉平台細節後，值得帶走的是一套「讓 AI 除錯有證據可查」的做法。

## R（四條不依賴平台的通則）

1. **把正確行為寫成可被推翻的條件**：例如「寫入已被確認後，不得再讀到更早的版本」。能被一次失敗推翻的條件，才可驗證；「系統要穩定」無法驗證。
2. **固定現場**：先記下失敗那次執行的編號與時間點，所有調查都針對同一個現場，避免每次重跑都換了情況。
3. **一次一個假設**：每個假設都記錄「怎麼驗證」與「為什麼排除」；不要一次改多個地方。
4. **觀察、解釋、下一步分開寫**：報告分三欄，看到什麼（事實）、推論原因（解釋）、接下來做什麼；事實與推論不混在一起。

## B（注意）

- **除錯分支裡的修改不等於專案修正**：修正要回到版本控制的程式碼，並**另開新測試**驗證，不能拿除錯現場的結果當作修好
- 與 [[agent-bug-fix-eval-green-is-not-correct]] 同一個提醒：測試轉綠不代表真的修對

## C（連結與應用）

- [ ] 下次請 AI 查 bug，要求報告分「觀察／解釋／下一步」，並附失敗現場的編號或重現步驟
- [ ] 修完後另開一個新測試確認，不沿用除錯時的結果

## 相關連結

- [[ai-collaboration-five-step-acceptance-criteria]] — 同屬「用證據取代『做完了』」的驗收思路：可核對的條件加上可回查的證據
- [[agent-bug-fix-eval-green-is-not-correct]] — 修復後的驗證不能只看測試綠燈
