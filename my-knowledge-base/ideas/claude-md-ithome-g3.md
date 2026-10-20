---
title: CLAUDE.md 跨對話記憶指南（iThome G3 系列）
date: 2026-06-22
tags: [ai/claude]
source: https://ithelp.ithome.com.tw/articles/10400239
type: article
status: processed
asset_value: 42
---

iThome 系列 G3：從新手角度解說 CLAUDE.md 四層結構和使用方法。主要內容已被 [[claude-md-guide]] 涵蓋，此處記錄新手視角的補充說明。

**四層級（從上到下全部載入）：**

| 層級 | 位置 | 用途 | Git |
|------|------|------|-----|
| 組織政策 | 系統層級 | 全公司統一規範 | 無 |
| 個人全域 | `~/.claude/CLAUDE.md` | 跨所有專案的個人習慣 | 不會 |
| 專案 | `./CLAUDE.md` | 專案規矩（團隊共用）| 會 |
| 個人+專案 | `./CLAUDE.local.md` | 只有我在這個專案的偏好 | 不進 |

**新手判斷準則：** 「每次開新對話都得跟它講一遍的東西，才值得寫進 CLAUDE.md」

不該寫：會一直變的進度、落落長說明（每次對話都要讀，佔 token）、一次性的指示。

**兩種更新方式：**
- `/memory` 列出所有載入的記憶檔，點開即可編輯
- 在對話中用自然語言說「這個專案一律用 yarn」，Claude 自動記住

## 同系列後續

- [[claude-code-session-handoff]] — **G5**：本篇「不該寫會一直變的進度」那條界線的正面解——當下進度要用一次性的**交接檔**傳給下一個對話（五段骨架：目標／已完成／關鍵決定與原因／下一步／別碰的地雷）。**記憶管長期穩定，交接管當下這一攤**
