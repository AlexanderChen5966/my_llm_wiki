---
title: Claude Code 輸出樣式 Concise：縮短的是說明文字，不是資訊
date: 2026-08-24
tags: [ai/claude, ai/prompt-engineering, type/how-to]
source: https://codelove.tw/@tony/post/al5W1q
type: article
status: processed
asset_value: 60
---

## T（核心論點）

Claude Code 的內建輸出樣式 Concise 縮掉的是重複說明文字，指令與程式碼一個都沒少、選項反而更多——因此「怎麼回答」的指示該寫進輸出樣式（進系統提示詞本身），而不是塞在 CLAUDE.md（掛在系統提示詞之後的使用者訊息）。

## B（適用場景）

**適用：**
- 覺得 Claude Code 每次回答前言太長、敘述太多，想一句設定就改善
- 目前把文風／語氣指示寫在 CLAUDE.md，想搬到更正確的機制層
- 想在不汙染既有設定的前提下，做 A/B 比較（`--settings` 單次試用）

**不適用 / 注意：**
- **不作用於子代理**——subagent 跑自己的系統提示詞；唯一例外是 fork，它繼承父層系統提示詞
- 五種內建樣式裡**只有 Concise 是縮短方向**，Explanatory 與 Learning 官方明寫「設計上會產生比 Default 更長的回應」
- 換樣式要 `/clear` 或開新工作階段才生效（輸出樣式屬系統提示詞，只在啟動時讀一次）
- `/output-style` 指令已於 **v2.1.91 刪除**，請改用 `/config`
- 實測樣本小（3 題、每種樣式各跑 1 次、Haiku 4.5、Windows 10、Claude Code 2.1.237），實際差異因題目而異
- 專案規範、程式碼庫知識這類「需要知道什麼」仍該留在 CLAUDE.md，不要搬進輸出樣式

## R（操作步驟）

1. 確認版本：Concise 於 **Claude Code 2.1.237（2026-08-20 發布）** 新增。changelog 原文：「Claude leads with results and skips preamble and narration, while doing the work just as thoroughly.」
2. 設定法一（推薦）：`/config` → **Output style** → 選 Concise。選項會存進 `.claude/settings.local.json`。
3. 設定法二：直接寫設定檔
   ```json
   { "outputStyle": "Concise" }
   ```
4. 設定法三（單次試用、不汙染現有設定）：
   ```bash
   claude -p "問題" --settings ./st-concise.json
   ```
   本文的實測就是用這個方法比對 Default 與 Concise。
5. 改完後執行 `/clear` 或開新工作階段，設定才會反映。
6. 把 CLAUDE.md 裡的語氣／文風指示搬到輸出樣式；專案規範與程式碼庫指示留在 CLAUDE.md。
7.（選用）自製樣式：把 Markdown 放進 `~/.claude/output-styles/`。**務必寫 `keep-coding-instructions: true`**，否則預設的程式碼撰寫指示會一起消失。

## C（關鍵行動）

- [ ] 改完設定沒感覺？先確認有 `/clear` 或開了新工作階段——輸出樣式只在啟動時讀一次
- [ ] 別期待樣式套用到子代理；只有 fork 會繼承
- [ ] 自製樣式漏掉 `keep-coding-instructions: true` 會連預設的 coding 指示一起清掉

## 原始摘要

**驗證環境**：Windows 10 / Claude Code 2.1.237 / 模型固定 Haiku 4.5 / 以 `claude -p --output-format json` 對同一問題每種樣式各執行 1 次，取 3 題平均。

**實測數據**：平均字數（不含空白）697 → **456（−35%）**；平均輸出 token 891 → **711（−20%）**。三題個別字數：Python 去重 864 → 585；git 修正前一個 commit 訊息 662 → 421；調查 npm test 失敗原因 566 → 362。

**為何字數 −35% 但 token 只 −20%**：因為程式碼與指令完全沒被刪掉，省下的文字被拿去補內容。git commit --amend 那題中，`--no-edit` 與 `--author="..."` **只有 Concise 版本列出**（Default 沒有）；`--force-with-lease` 的注意事項兩版都有。被刪掉的只有重複說明——同一段解釋從 2 句變成 1 句，指令本身完全沒變。

**五種內建樣式**：Default（基準）／**Concise（新，較短）**／Proactive（想減少確認、直接執行）／Explanatory、Learning（設計上較長，官方原文 `produce longer responses than Default by design`）。

**機制差異**：輸出樣式放進**系統提示詞本身**；CLAUDE.md 放在**系統提示詞後面**的使用者訊息。官方區分——專案規範、程式碼庫等指示用 CLAUDE.md。一句話：「怎麼回答」屬輸出樣式；「需要知道什麼」屬 CLAUDE.md。

**三個坑**：(1) 要 `/clear` 或新工作階段才生效；(2) 不作用於子代理，fork 例外；(3) `/output-style` 指令已於 v2.1.91 刪除，改用 `/config`。

原文出處：Qiita jqit_suwa（https://qiita.com/jqit_suwa/items/ccd228bb1c33b2a918f5），由 codelove.tw 翻譯轉載。參考連結為 Claude Code changelog 2.1.237 項目與 Output styles 官方文件。

## 相關連結

- [[claude-code-cost-optimization-7-tips]]
- [[claude-md-optimization-techniques]]
- [[claude-code-context-management]]
- [[claude-conversation-style-value-axes]]
