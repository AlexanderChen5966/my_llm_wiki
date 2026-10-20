---
title: CLAUDE.md 設計模式集：依專案規模分別使用的 7 種範本
date: 2026-07-24
tags: [ai/claude, ai/prompt-engineering, type/how-to]
source: https://codelove.tw/@tony/post/3KKzQ3
type: article
status: processed
asset_value: 80
---

Claude Code 的輸出品質最左右的不是 prompt 也不是模型，而是 **CLAUDE.md 的寫法**。本文提供依專案規模區分的 7 種可直接複製的 CLAUDE.md 範本（3 種規模型 + 3 種用途特化型 + 反模式），核心原則是「寫得恰到好處，不多不少」。

## T（核心論點）

導入 Claude Code 的團隊中，「能產出符合預期程式碼」與「每次都要手動修正」的差異，關鍵在 CLAUDE.md 的設計品質。CLAUDE.md 不是寫越多越好——**依規模選對範本 + 一定要寫「禁止事項」+ 每月盤點**，才能讓輸出品質大幅提升。

## B（適用場景）

**適用：** 建立或重整任何 Claude Code 專案的 CLAUDE.md 時；判斷現有 CLAUDE.md 是否過度膨脹。
**不適用 / 注意：** 個人一次性腳本寫冗長 CLAUDE.md 是過度工程化；範本大小僅為參考值（非官方硬性數字，見 [[claude-md-optimization-techniques]] 的來源標註釐清）。

## R（7 種範本 + 反模式）

**載入階層**：使用者全域 `~/.claude/CLAUDE.md`（個人偏好）→ 專案根目錄 `./CLAUDE.md`（技術棧規範，Git 管理）→ 子目錄 `./packages/api/CLAUDE.md`（套件專屬，操作該目錄時額外讀取）。

> 🔴 **2026-07-30 更正**：原本寫「**越下層優先度越高**」是**錯的**。官方文件明訂：
> *All discovered files are **concatenated into context rather than overriding each other**. If two rules contradict each other, **Claude may pick one arbitrarily**.*
>
> **CLAUDE.md 不像 CSS 有嚴格的優先序覆蓋機制**——它更像你同時給了多份指示，矛盾時**不保證遵守哪一份**。
> → **正確做法是「避免衝突」，而不是「依賴優先序」**：使用者層寫通用偏好、專案層寫專案規範，兩者不要重疊。
> 唯一的例外保證：同一層目錄內 `CLAUDE.local.md` 會 append 在 `CLAUDE.md` 之後，**衝突時個人筆記是該層最後被讀到的**——但這個保證**跨層級無效**。

**依規模的 3 種範本**：
| 規模 | 對象 | 建議行數 | 重點 |
|------|------|---------|------|
| 極簡型 | 1 人 / 1–10 檔 / 一次性工具 | 10–20 行 | 只寫「在做什麼／可用哪些工具／怎麼執行」 |
| 分層型 | 2–5 人 / 50–300 檔 / Web 應用 | 40–80 行 | 明確「相依方向」與「禁止事項」兩區塊最有效 |
| 階層式分拆 | 5 人以上 / 3+ 套件 / monorepo | 根 20–40 + 各套件 15–30 行 | 根目錄只寫共通方針，各套件放專屬 CLAUDE.md |

**3 種用途特化型（與規模無關，可追加到根目錄或 `~/.claude/`）**：
- **程式碼審查型**：定義必查項目（安全性/效能/錯誤處理/測試）+ 輸出格式（Critical/Warning/Info + 行號 + 修正建議）
- **測試生成型**：AAA 模式、一測一 assertion、必要案例（正常/邊界/異常/冪等）、Mock 方針
- **重構型**：不改外部行為、重構前先確認測試通過、一 commit 一項重構、禁止把功能新增混進重構

**反模式（過大反效果）**：全部塞進型（>300 行 → 拆分）、只追加不整理型（矛盾指示共存 → 每月盤點）、程式碼大量內嵌型（範例 ≤5 行）、願望清單型（模糊指示 → 改具體規則如「函式 30 行內」）、混入機密型（只記環境變數名）。**總行數超過 500 行是警訊**。

## C（關鍵行動）

- [ ] 用「首日 15 分鐘檢查清單」建立新專案 CLAUDE.md：概要 / 技術棧 / 目錄責任 / 相依方向 / ≤5 項規範 / **≥3 項禁止事項** / 建置測試指令 / 是否需階層拆分
- [ ] Claude Code 更忠實遵守「不可以做什麼」而非「應該做什麼」——優先寫 NEVER
- [ ] 每月盤點一次 CLAUDE.md，反映技術棧變更、刪除矛盾與膨脹
- [ ] 本知識庫自身的 CLAUDE.md 可對照反模式檢查（目前偏長，屬多 Skill 路由需求）

## 原始摘要

譯自 Qiita hikariclaude01。7 種範本（極簡/分層/monorepo 階層 + 審查/測試/重構特化）+ 反模式表 + 首日 15 分鐘檢查清單。強調 CLAUDE.md 需像程式碼一樣持續維護。

## 相關連結

- [[synthesis-claude-md]] — CLAUDE.md 撰寫方法論知識精粹（本主題 4 篇整合）
- [[claude-md-guide]] — CLAUDE.md 撰寫技巧（Boris Cherny 六引擎 + Karpathy 12 條）
- [[claude-md-optimization-techniques]] — 精簡化技巧與「200 行/14 條」數字的來源釐清
- [[claude-md-behavioral-control-principles]] — 行為控制四原則（能力追加 vs 行為控制）
- [[claude-code-12-rules-error-reduction]] — 12 條規則把錯誤率從 41% 降到 3%
- [[claude-code-large-codebase]] — 大型專案三層 CLAUDE.md 配置
- [[claude-code-file-management]] — 設定檔四層架構（Managed/User/Project/Local）
