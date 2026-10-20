---
title: Claude Code Skill 規模化的隱性成本：字數預算、靜默截斷與限制模式全滅
date: 2026-09-15
tags: [ai/claude, ai/agent, type/concept]
source: https://codelove.tw/@tony/post/3V2Pk3
type: article
status: processed
asset_value: 64
---

## T（核心論點）

> **Skill 數量增加幾乎不增加 token 成本，但代價是超出預算後既有 skill 的說明會被靜默清空——不報錯，只留名稱，且每次消失的對象不固定。**

作者在 Windows 10 / Claude Code 2.1.247〜2.1.270（2026-08-19〜09-14）環境下，用自製 47 個 skill 實測整合出 7 件事（含官方新指令構成第 7 項）：

1. **增加不會變重**：加 80 個 skill 只多 +185 token，因清單有字數預算（`skillListingBudgetFraction`）
2. **超出預算的部分說明會靜默消失**：呼叫次數少的先被清空（如 388 字→0、350 字→0），畫面照常顯示 47 個名稱、無警告無 log
3. **消失對象每次執行都不同**：同一環境重複測試 4 次，被清空的 skill 每次不一樣，不能認定某個 skill 絕對安全
4. **有效的 skill 確實有效，但資訊量也減少**：裝上 30k stars 的極簡 skill 後輸出減少 43%（9 次全中），但技術關鍵字減少 8～30%，內容是被替換而非單純刪減
5. **純指示書型 vs 含實作型 skill 成本差約 28 倍**：一個只有 `SKILL.md` 常駐約 3,225 token，另一個含 Python 實作啟動約 90,000 token
6. **`--restricted` 限制模式下 47 個自製 skill 全部消失**（工具也從 42 減到 23，含 Workflow / Monitor / LSP 及 8 個 MCP 工具）
7. **官方 `claude plugin eval`（2.1.269，2026-09-11 推出）能測「行為是否可重現」，但無法測「變化了多少量」**：6 種 grader（regex / tool_used / tool_order / file_exists / llm / baseline）皆為 pass/fail，官方文件明言 `There are no custom-code graders`

## B（適用場景）

**適用：**
- 已累積 10+ 個 skill 或正在評估要不要繼續加 skill 的 Claude Code 使用者
- 需要判斷「新裝的 skill 沒被叫到」是真的沒用還是說明被靜默清空
- 需要在 `--restricted` 模式下運作的自動化流程，且依賴自製 skill

**不適用 / 注意：**
- ⚠️ 所有數據僅來自作者單一環境（47 skill + MCP server + Windows 10），前提比乾淨環境更多，換情境比例可能改變
- 第 6 項限制模式測試是 2.1.248 時點，之後版本未再確認
- 第 8 項 `claude plugin eval` 僅測試到讀文件與 `--help` 階段，尚未實際套用到自製 skill 上驗證

## R（操作步驟）

**加入 skill 前：**
1. 用 `claude plugin details <外掛名稱>` 查看常駐與啟動時 token 成本，區分純指示書型與含實作型
2. 確認環境相容性看的是「這個工具看不看得到」而非「有沒有裝」（作者曾誤判別處安裝的 Python 版本符合需求）

**加入後：**
3. 比較加入前後的清單 token 數；若沒有明顯增加，代表某些既有 skill 的說明已被靜默清空
4. 真正想被叫到的 skill 要先累積呼叫實績——0 次呼叫的新 skill 最先被淘汰
5. `/skill-doctor` 的預告與實測有 1.7 倍落差（預告少 2,160 vs 實測少 3,710 token），刪除判斷仍需自行重新量測

**評估時：**
6. 用 `claude plugin eval` 測行為是否穩定可重現（可上 CI），但要看「變了多少」仍需手動數 `--output-format json` 的 token 數
7. 評分工具的 9 維 rubric 只看結構完整度（步驟清晰/失敗分支/檢查點），不判斷內容事實正確性，越具體的錯誤反而分數越高；建議額外直接問模型「有沒有哪裡怪怪的」

## C（關鍵行動）

- [ ] 加 skill 前用 `claude plugin details` 查成本，判斷是指示書型還是實作型
- [ ] 定期比對清單 token 數，及早發現既有 skill 說明被靜默清空
- [ ] 若流程需要在 `--restricted` 模式下運作，先假設所有自製 skill 會消失並規劃備援
- [ ] 用 `claude plugin eval` 做行為回歸測試，但變化量的驗證仍需手動量測

## 原始摘要

本文整合作者先前 6 篇獨立驗證文章 + 本週新推出的官方指令，成為第 7 項發現。核心觀察是「skill 是資產，但不是放著就會變成資產」：與其增加安裝數量，不如把真正有被呼叫的留下來。

## 相關連結

- [[claude-code-official-skills-top10-2026]] — 性質不同：該篇是推薦榜單，本篇是底層行為的技術實測方法論，互為補充而非合併
- [[claude-code-skill-survivors-20-picks]] — 性質不同：該篇是精選清單，本篇聚焦規模化後的隱性成本（字數預算/靜默截斷/限制模式全滅），互建連結供交叉參考
- [[claude-code-prompt-audit-scope-trap]] — 同作者（suwa_nobu）系列、同屬「Claude Code 靜默失效」：本篇是 skill 說明被靜默清空，該篇是 `/doctor prompt-audit` 在 `-p` 下稽核範圍靜默縮水（2026-09-29 新增）
