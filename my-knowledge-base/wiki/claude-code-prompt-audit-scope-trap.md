---
title: Claude Code /doctor prompt-audit 的範圍陷阱：預設看不到使用者層技能，實際有效的是「設定化石」偵測
date: 2026-09-29
tags: [ai/claude, ai/prompt-engineering, type/how-to]
source: https://codelove.tw/@tony/post/xN6DGx（譯自 https://qiita.com/suwa_nobu/items/ae5a8c1609c794a74a25）
type: article
status: processed
asset_value: 64
---

Claude Code 2.1.283（changelog 2026-09-25，官方）新增 `/doctor prompt-audit`（別名 `/checkup prompt-audit`），宣稱會稽核 CLAUDE.md／skills／agents／commands 裡「為舊模型寫的提示寫法」。作者 suwa_nobu（49 個技能、286 個子代理的重度環境）實測後發現兩件事：範圍會靜默縮水，而且真正抓到的東西跟名字不一樣。

## T（核心論點）

> **在 `claude -p` 非互動模式下，`/doctor prompt-audit` 讀不到 `~/.claude`（權限無法核准），使用者層 skills／agents 會靜默不在範圍內——必須加 `--add-dir ~/.claude`。而且它名叫 prompt-audit，實際有效的是「設定與現況脫節」的化石偵測，不是舊模型寫法偵測。**

- **範圍陷阱**：第一次在專案內跑，報告開頭自承「未監査」`~/.claude/CLAUDE.md`、`~/.claude/{rules,skills,commands,agents,output-styles}`、`~/.claude/plugins`；22 個錯誤中 15 個是權限被拒——49 個技能與 286 個子代理一個都沒看，只審了專案內 18 個檔。**不報錯，只是少審**
- **加了 `--add-dir` 也只是掃過**：對 `~/.claude` 的 49 次操作中 Grep 19／Glob 16／Bash 6／Read 8，完整讀過的只有 2 個子代理與 2 個技能；報告自承「中訊號區間以 grep 為主」
- **抓到的是環境化石**：他人的 macOS 絕對路徑、引用不存在的外掛、引用已刪除的文件（能對照 git 歷史指出刪除時點）、專案代理被解析到使用者側同名代理；`budget_tokens`、prefill、「think step by step」這類 API 化石是**零**
- **官方 changelog 佐證**：同版（2.1.283）另一條寫著 "Improved `prompt-audit` on Claude Code configuration: stale paths, stale commands and contradicting instruction files now lead the report"——報告本來就把死路徑、死指令、互相矛盾的指令檔排在前面，這正好解釋作者的觀察（依本機 changelog，官方文件頁未核）

### 兩個同名 prompt-audit（依本機 changelog，待官方文件核對）

| | `/claude-api prompt-audit` | `/doctor prompt-audit` |
|---|---|---|
| 性質 | `claude-api` skill 的子指令 | Claude Code 內建指令（別名 `/checkup prompt-audit`） |
| 加入版本 | 2.1.221 | 2.1.283 |
| 稽核對象 | 提示詞與 tool description | CLAUDE.md／skills／agents／commands |
| 本庫記載 | [[llm-cost-optimization-strategies]]「Prompt Audit」戰術 | 本條 |

依本機 `~/.claude/cache/changelog.md` 所見，兩者**並存**，不是同一指令的舊稱與新稱；但官方文件頁尚未核對，暫不下絕對結論。

## B（適用場景）

**適用：**
- 累積了使用者層 skills／agents（`~/.claude/skills`、`~/.claude/agents`）、想定期或換模型時稽核的 Claude Code 使用者
- 要在 CI 或腳本裡用 `claude -p` 跑稽核的人——這是最容易「默默漏掉一半」的情境
- 設定用得越久、搬過機器、刪過文件的環境（化石越多越有收穫）

**不適用 / 注意：**
- ⚠️ 所有數據都是**作者實測**（suwa_nobu，Windows 10／隔離安裝 2.1.283，2026-09-28 單日、各情境單次），**未逐條驗證指摘正確性**、評分標準未公開；結果可能每次不同
- ⚠️ **只測了 `-p` 非互動模式**；互動式 session 中核准權限後範圍是否同樣縮水，**未驗證**
- ⚠️ 技能數對不起來（目錄 49、`SKILL.md` 48、報告 45），差異原因作者未確認
- ⚠️ 成本是 `--output-format` 回傳的 `total_cost_usd`，實際請款依方案不同
- 「引用不存在外掛」是**作者環境的觀察**：作者環境引用 `document-skills:*` 被判為不存在外掛，但本機 `~/.claude/plugins` 確實裝有 `document-skills`——**某個引用是不是化石，依各自環境而定**，不能直接套用作者的清單

## R（操作步驟）

1. **非互動模式一定帶 `--add-dir`**：要納入使用者層設定時，在 `claude -p` 呼叫上加 `--add-dir ~/.claude`（`/add-dir` 的說明見 [[claude-code-commands-reference]]）
2. **先讀報告開頭的「未監査」段**：確認這次到底審了哪些路徑，再看指摘——範圍縮水不會以錯誤呈現
3. **預期抓到的是設定化石**：優先處理他人或舊機器的絕對路徑、已刪文件引用、未安裝外掛引用、同名代理解析錯位；舊模型語句類反而很少
4. **逐條判斷是真化石還是承重內容**：報告中訊號區以 grep 為主、完整讀檔極少，指摘要自己打開檔案確認；外掛引用要對照自己的 `~/.claude/plugins` 再決定
5. **預留成本**：作者兩次合計約 34 分鐘／$20.31，不適合高頻率跑；搭配低成本的手工稽核法（[[claude-skill-audit-four-step-minimal-method]]）輪替

**作者實測數據（【作者實測】，單日單次）：**

| | 第一次（專案內，`-p`） | 第二次（`--add-dir ~/.claude`，幾乎空白的專案） |
|---|---|---|
| 所需時間（API 時間） | 21.7 分 | 12.7 分 |
| 成本（total_cost_usd） | $10.69 | $9.62 |
| 輸出 token | 105,589 | 53,868 |
| 工具呼叫 | 75 次 | 56 次 |
| 審查對象 | 專案 18 個檔 | 45 個技能／286 個子代理 |
| 指摘 | 66 筆 | 12 筆 |

分類件數（第一次→第二次）：舊模型語句 14→2、設定陳舊／矛盾 49→8、工具說明 0→0、結構 3→2。

**本庫適用：**
- 本庫 skills 在專案層（`notes/.claude/skills -> ../skills`），在預設範圍內會被掃到
- 使用者層 `~/.claude/skills`（doc-update、requirements、stack-review 等）需 `--add-dir ~/.claude` 才會納入

## C（關鍵行動）

- [ ] 在 CI／腳本跑 `/doctor prompt-audit` 時一律加 `--add-dir ~/.claude`，並檢查報告開頭的未審範圍
- [ ] 把它當「設定化石偵測器」用：死路徑、已刪文件、未安裝外掛、同名代理解析錯位
- [ ] 外掛引用類指摘先對照本機 `~/.claude/plugins` 再判斷，不直接照刪
- [ ] 區分 `/claude-api prompt-audit`（提示詞／tool description）與 `/doctor prompt-audit`（Claude Code 設定），待官方文件頁核對後再更新本條

## 原始摘要

作者以 49 個技能、286 個子代理的環境，分兩次執行 `/doctor prompt-audit`：第一次範圍被權限卡住只審了專案內 18 個檔；第二次加 `--add-dir ~/.claude` 才辨識到 45 個技能與 286 個子代理，但以 grep 掃描為主。指摘品質高（能對上 git 歷史指出刪除時點），但出現的幾乎都是環境化石而非舊模型寫法。結論：「雖然名字叫 prompt-audit，但實際上有效的是『設定有沒有和現況脫節』的檢查。」

## 相關連結

- [[claude-code-skill-scaling-hidden-costs]] — 同作者（suwa_nobu）系列、同屬「Claude Code 靜默失效」：該篇是 skill 說明被靜默清空，本篇是稽核範圍靜默縮水
- [[claude-md-optimization-techniques]] — 「技巧 9：用 `/doctor` 自動體檢」；本篇是其 prompt-audit 子指令的範圍陷阱
- [[claude-skill-audit-four-step-minimal-method]] — 手工稽核法（抓當前激活層），可與本指令對照、輪替
- [[llm-cost-optimization-strategies]] — 另一個 `/claude-api prompt-audit`（提示詞層），依本機 changelog 為不同指令
- [[claude-code-commands-reference]] — `/add-dir` 等指令速查
- [[superpowers-skill-bloat-risk-tiering]] — 未安裝的 `superpowers:*` 引用也被抓出；Skill 膨脹與風險分級
