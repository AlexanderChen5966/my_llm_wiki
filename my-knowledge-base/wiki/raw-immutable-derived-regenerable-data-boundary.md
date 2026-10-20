---
title: 原始資料只進不出、衍生資料要能重建：用「誰產生、能否重來」決定每個資料夾的保護等級
date: 2026-09-23
tags: [ai/workflow, dev/architecture, type/concept]
source: https://ithelp.ithome.com.tw/articles/10408143（《研究生自救指南：30 天用 Claude Code 打造我的論文工具箱》Day 4，iT 邦幫忙使用者 20183988）
type: article
status: processed
asset_value: 64
---

> 「raw 不可變、LLM 只能讀」這件事本庫已有多條條目寫過（見相關連結）。本條目**不重講這個概念**，只收 Day 4 的四個新增量：衍生資料的判準、三類檔案分級、三層防護的失效理由、可重現清理與方法學的關係。系列總綱見 [[grad-thesis-claude-code-toolbox-series]]。

## T（核心論點）

**保護等級不該一刀切，要看每個資料夾「誰產生的、能不能重來」。**

1. **衍生資料的判準是「整個刪掉，能不能一鍵重生」。** `data/clean/` 裡的檔案不該是手改出來的，而是 `analysis/` 裡的程式碼跑出來的。能重生，規則錯了就改程式碼重跑，「三分鐘的事」；不能重生，就只能整個重來。
2. **三類檔案，三種緊張程度：**

   | 類別 | 誰產生 | 能否重來 | 對策 |
   |---|---|---|---|
   | `raw/` | 受試者 | ❌ 不能 | 「用命的守」 |
   | `clean/`、`output/` | 程式碼 | ✅ 可以 | 壞了不用慌，重跑即可 |
   | `draft/` | 本人 | ❌ 不能 | 要做版本控制 |

   分清楚之後，「該緊張的緊張，不該緊張的就放手讓工具去做」。
3. **防護要三層，因為每一層都會各自失效：** CLAUDE.md 明文禁止＋OS 唯讀＋異地備份。理由原文是「**規則會被忽略、唯讀會被解除、備份會忘記做**」。
4. **可重現的清理過程，本身就是方法學章節要交代的內容。** 多數人是事後憑回憶寫，而不是當初就留下紀錄。

## B（適用場景）

**適用：**
- 任何讓 AI 能動檔案的專案：論文、程式專案、資料分析、本知識庫
- AI 一次改很多檔案的情境。作者的理由：「它可能在三秒內改了十二個檔案——如果邊界沒有先畫好，你連要從哪裡開始檢查都不知道」，所以**邊界要在出事前畫**

**不適用 / 注意：**
- 作者自述的事故（直接在原始問卷檔上刪資料、算反向題、改欄位名，三個月後才發現清理規則訂錯）完整經過留到 Day 18，**不在本批**，本條目不預寫
- 🔴 **作者前後說法不一致，照原樣呈現、不自行統一**：
  - Day 4 的資料夾樹用 `refs/` 放文獻；Day 7、8 改用 `literature/`（底下有 `search-strategy.md`、`search-log.md`、`pdf/`、`cards/`、`results/`），且 Day 7 說 `literature/` 是「Day 4 資料夾結構裡」的
  - Day 5 說「Day 4 的資料夾樹已經留了 `.claude/skills` 的位子」，但 Day 4 原圖沒有畫出來
  - 📎 2026-09-29：Day 9–12 一律用 `literature/`、`draft/`，未再出現 `refs/`（上方原註記不動）

## R（操作步驟）

1. **資料夾樹（Day 4 原文）**：

   ```
   thesis/
   ├── CLAUDE.md      # 交班單
   ├── refs/          # 文獻 PDF 與書目檔
   ├── data/
   │   ├── raw/       # 原始資料，唯讀，只進不出
   │   └── clean/     # 清理後的資料，可以整個刪掉重生
   ├── analysis/      # 清理與統計的程式碼
   ├── output/        # 跑出來的表格與圖
   └── draft/         # 論文各章
   ```

   🔴 Day 7、8 改用 `literature/`；Day 5 說樹中已留 `.claude/skills` 的位置，但上圖沒有（見 B 層）。
2. **`raw/` 只進不出**：只能新增，不能修改、不能刪除，自己和 AI 都一樣。
3. **三層防護**：
   - 第一層：CLAUDE.md 明文寫「不要動 data/raw/」
   - 第二層：OS 唯讀，Windows 用 `attrib +R`，Mac／Linux 用 `chmod 444`
   - 第三層：另外備份一份到別的地方
4. **`clean/` 驗收**：把 `clean/` 整個刪掉，跑一次 `analysis/` 的程式碼，看能不能完整重生。
5. **每個新資料夾都先回答兩個問題**：「這裡的東西是誰產生的？」「能不能重來？」再決定它屬於哪一類。

## C（關鍵行動）

- [ ] 盤點自己專案的每個資料夾，逐一標上三類之一（原始／可重生衍生物／不可重生的人工產物）
- [ ] 對「可重生」那一類做一次刪除重建演練，確認真的能一鍵重生
- [ ] 對「原始」那一類檢查三層防護是否都在，缺哪一層補哪一層
- [ ] 待確認：本庫 `wiki/` 目前靠什麼機制做版本保護（見下方對照）

### 對照：本知識庫的 `raw/`

- **本庫目前只有規則層防護。** CLAUDE.md 寫明「NEVER modify or delete files in raw/」；`raw/` 的檔案權限是一般的 `rw-r--r--`，沒有設 OS 唯讀（2026-09-23 檢查）。原因是本庫刻意留了兩個例外：可新增 `.processed` 旁車檔，以及可改 `.md` frontmatter 的 `status:` 單一欄位。這兩個例外本質上是「流程元資料寫入」，和作者「只進不出」的嚴格版不同。
- **這個例外確實出過事，剛好印證「規則會被忽略」。** 2026-08-13 查出舊版 skill 誤判 frontmatter 邊界，把 `status: processed` 插進 description 字串裡。之後的對策是改用 YAML parser 解析，並用 diff 回驗正文零改動（見 [[inbox-detection-protocol]]）。
- **本庫的 `wiki/` 性質接近作者的 draft 類，不是 clean 類。** `wiki/` 由 AI 維護，無法從 `raw/` 一鍵重生，屬於「不能重來、要做版本控制」的那一類。專案根目錄目前不是 git repo（2026-09-23 檢查），但這**不代表沒有備份**，可能另有 Obsidian Sync 或其他機制，**列為待確認**，不在此下結論。

## 原始摘要

作者用「護理紀錄寫錯不能塗改，只能劃線、簽名、註記時間」比喻研究資料：原始資料是整篇論文結論的唯一證據，直接改原始檔等於銷毀證據，而且通常三個月後才會發現。文章提出三個原則：raw 只進不出（三層防護，因為單一層都會失效）；clean 必須能從程式碼重新生成（全篇最重要的一條）；每個資料夾都要能回答「誰產生、能否重來」。AI 動得快、一次改很多檔案，所以這條邊界比手動時代更重要，而且要在出事前畫好。

## 相關連結

- [[wikibrain-llm-wiki-implementation-patterns]] — 🔑 **同一原則的程式碼版**：「raw/ 唯讀要擋兩次；會弄壞資料的寫成程式碼」。WikiBrain 把守門寫進資料存取層，本篇則是非工程師用規則＋OS 權限＋備份三層達成
- [[karpathy-knowledge-base-method]] — 原始資料層不可變、LLM 只能讀（本概念的源頭）
- [[synthesis-karpathy-llm-wiki-method]] / [[llm-wiki-karpathy-vs-nashsu]] — 三層架構與 Raw 不可變的既有整理
- [[claude-md-optimization-techniques]] — 用本庫 `raw/` 唯讀規則當「描述型改成指令型」的範例
- [[agent-permission-and-os-sandbox]] — OS 層防護的完整版（本篇的第二層）
- [[harness-engineering]] — 用環境約束代替對模型的信任
- [[grad-thesis-claude-code-toolbox-series]] — 系列總綱
- [[ai-output-verifiable-id-and-status-field]]（Day 9 段）— 比較矩陣是 clean 類衍生物的實例：要改回卡片改再重跑，不手動改矩陣（2026-09-29 新增）
- [[ai-autonomy-by-rule-clarity-verifiability-reversibility]] — Day 11 校對「改副本、原檔不動」延續原則二（2026-09-29 新增）
