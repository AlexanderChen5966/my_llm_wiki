---
title: Wiki 健康檢查報告
date: 2026-05-13
tags: [pkm/workflow]
type: note
status: processed
generated_by: wiki-health-check v1.0.0
---

# Wiki 健康檢查報告 — 2026-05-13

## 整體評分

| 項目 | 數值 |
|------|------|
| 總條目數 | 11 |
| 嚴重問題 | 0 |
| 警告 | 3 |
| 建議 | 2 |
| **健康指數** | **⚠️ 待改善** |

---

## 🔴 嚴重問題（需立即處理）

無。

---

## 🟡 警告（建議本週處理）

### W1：懸空 wikilink
`[[obsidian-knowledge-base-planning]]` 被 `wiki/karpathy-knowledge-base-method.md` 引用，但此頁面不存在。

**建議：** 建立 `wiki/obsidian-knowledge-base-planning.md` 指向規劃文件，或將連結改為正確路徑。

---

### W2：wikilink 格式不一致
wiki 條目中同時存在兩種 b2b-manager 連結格式：

| 格式 | 範例 | 正確性 |
|------|------|--------|
| 舊格式（flat）| `[[b2b-manager-api]]` | ❌ 找不到檔案 |
| 新格式（subfolder）| `[[b2b-manager/api]]` | ✅ 正確 |

**影響範圍：** `wiki/INDEX.md` 中的所有 b2b-manager 連結使用舊格式。

**建議：** 將 `wiki/INDEX.md` 中的 `[[b2b-manager-xxx]]` 全部改為 `[[b2b-manager/xxx]]`。

---

### W3：raw/ 狀態未更新
已有對應 wiki 條目的 2 篇 raw 檔案，`status` 仍為 `inbox`：
- `raw/AI 大神 Karpathy 的 AI 筆記流...md`
- `raw/Obsidian Web Clipper...md`

**建議：** 補標記 `status: processed`，避免重複處理。

---

## 🟢 建議（下次處理）

### S1：INDEX.md 孤立
`wiki/INDEX.md` 沒有任何筆記指向它（0 入連結）。作為索引頁應該是被查詢的入口，而非孤立存在。

**建議：** 在 `CLAUDE.md` 或根目錄 `歡迎.md` 中加入 `[[wiki/INDEX]]` 連結。

### S2：b2b-manager/INDEX.md 未納入全域索引
`wiki/INDEX.md` 目前只列出 `[[b2b-manager/INDEX]]`，但沒有直接連結到子條目。
考慮是否需要在全域 INDEX 中展開 b2b-manager 子條目。

---

## 優先處理清單

1. 修正 `wiki/INDEX.md` wikilink 格式（W2）— 5 分鐘可完成
2. 補標記 2 篇 raw 檔案狀態（W3）— 2 分鐘
3. 建立 `obsidian-knowledge-base-planning.md` 缺口（W1）— 或修正連結
