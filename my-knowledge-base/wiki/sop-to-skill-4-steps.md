---
title: 把部門 SOP 變成 AI 技能：4 步做出 SKILL.md
date: 2026-06-30
tags: [ai/claude, pkm/workflow, type/how-to]
source: https://www.bnext.com.tw/article/91344/ai-skill-sop-claude-4-steps
type: article
status: processed
asset_value: 58
---

把部門工作 SOP 交給 Claude，4 步驟整理成可一鍵召喚的 skill。核心洞察：**AI 答不好往往不是它的錯，而是原始文件沒寫好**——這個做法除了產出 skill，更重要的是照出公司工作手冊裡藏著哪些缺口。

## T（核心論點）

大多數內部 SOP 只有「做什麼」，缺「怎麼判斷」和「出問題怎麼辦」，AI 只能猜。把 SOP 轉成 SKILL.md 的過程，會強迫你補齊範疇、判斷標準、計算邏輯、例外處理——文件補好了，技能產出水準才會提升。

## B（適用場景）

- **適合轉技能**：流程與判斷型 SOP（產出提案簡報、回應客訴、出報價單）
- **不一定適合**：需照截圖操作、牽涉解讀圖片或操作外部平台的教學手冊
- 想盤點並補強部門工作手冊缺口

## R（四步驟）

1. **請 AI 解讀文件，整理 5 個項目**：上傳文件下指令——「整理出：執行步驟（按順序）、重複出現的規則、常見錯誤、關鍵資訊（執行前需確認）、成功標準（怎樣叫做完/做對）」。這五項是建立技能的關鍵要素。
2. **把 5 項組成提示詞讓 Claude 寫技能包**：「根據剛梳理的步驟/規則/常見錯誤/關鍵資訊/成功標準，用 skill creator 做成 SKILL.md。當我說 [任務名稱] 時啟動，但不適用於：[2–3 個不相關任務]。有缺少的資訊先問我。」
3. **跑真實任務測試**：把原本按 SOP 能完成的任務指派給 Claude，觀察技能是否啟動、資訊不足時怎麼辦、是否誤答。
4. **回頭補原始文件缺口**：作者實測影音製作 SOP → 做 podcast 規劃答不好，因原文 podcast 內容少。常忽略的是範疇（分類）、判斷標準（怎麼選）、計算邏輯（成效怎麼看）、例外題（退件怎麼辦），補上大幅提升產出。

## C（對自身的啟發）

- [ ] 「五項要素（步驟/規則/常見錯誤/關鍵資訊/成功標準）」可作為本知識庫 skill 撰寫的檢查清單
- [ ] description 明確排除不相關任務，呼應 skill-md-writing-techniques 的第三人稱+排除情境模板
- [ ] 「AI 答不好先檢查文件」的診斷法，適用於檢視 CLAUDE.md 與 skill 缺口

## 相關連結
- [[ai-agent-reverse-workflow-decomposition]] — **非工程師視角的前一步**：先從終點反推工作流（每步 Output = 下步 Input）、算 ROI 再決定要不要 skill 化
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧
- [[sop-to-skill-4-steps]] 對照 [[claude-skills-beginner-guide]] — Skills 入門
- [[skill-md-9-types-and-gotchas]] — SKILL.md 九大類型與 Gotchas
- [[anthropic-official-17-skills]] — Anthropic 官方 17 個 Skills
- [[notion-3-7-agent-skills]] — Notion 3.7 把 SOP 頁面直接變成 Skill：本篇談撰寫方法，該篇談 Skill 放上協作平台後的權限與同步治理（2026-09-23 新增）
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 遷移到 Skills 的跨廠商對照（2026-10-08 快照）
- [[api-cli-skills-agent-architecture]] — ⚠️ 2026-10-19 觀點並列：該條補充段收錄數位時代「Skill 不需要 SOP，只要指引（guideline）」的說法，與本篇「先補齊 SOP 判斷標準再轉 Skill」並列不擇一
