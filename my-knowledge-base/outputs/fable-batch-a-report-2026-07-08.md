---
title: Fable 批次 A 報告 — notebooklm kit 五版本一致性
date: 2026-07-08
tags: [ai/claude, pkm/workflow, project/knowledge-base]
type: reference
status: processed
---

# 批次 A 報告：notebooklm kit 五版本一致性檢查

> 執行：2026-07-08（Fable 5）｜範圍：v1.3 / v2.0 / v2.0.1 / v2.1 / v2.1.1 五個 kit
> 檢查方法：五份 AGENTS.md + 四份 paper-discovery SKILL.md 全讀 + 全 kit 關鍵字交叉掃描
> 結論：**發現 2 個版本漂移 + 8 處檔名斷鏈 + 3 個低優先註記；無凍結版污染、無規則矛盾**
>
> ✅ **修正狀態（2026-07-08）**：A-1 / A-2 / A-3 共 11 處已全數修正並驗證（舊檔名引用歸零、版號三處同步為 1.1.1 / 2.2.1）。A-4～A-6 依建議不修，留註記。

---

## ❌ 需修正（✅ 已於 2026-07-08 修正完畢）

### A-1｜v2.0.1：AGENTS.md 的 paper-discovery 版號過時
- `AGENTS.md:277` Skills 清單寫 **1.1.0**
- `skills/paper-discovery/SKILL.md` 實際為 **1.1.1**（台灣中文文獻支援後升版，AGENTS 表未同步）
- **修法**：AGENTS 表改為 1.1.1

### A-2｜v2.1.1：AGENTS.md 的 paper-discovery 版號過時（兩處）
- `AGENTS.md:279` Skills 清單寫 **2.2.0**；實際 SKILL.md 為 **2.2.1**
- `AGENTS.md:6` 標頭 v2.1.1 說明行寫「詳見 paper-discovery v2.2.0」→ 同樣過時
- **修法**：兩處改為 2.2.1

### A-3｜v2.1 系列 docs 改名後的斷鏈（每 kit 4 處，共 8 處）
v2.1-kit 與 v2.1.1-kit 的 docs 實體檔已改名為 `codex-desktop-integration-v2.1.md` / `notebooklm-workflow-v2.1.md`，但以下引用仍指向**不存在的舊檔名**：

| kit | 檔案 | 斷鏈引用 |
|-----|------|---------|
| v2.1 / v2.1.1 | `wiki/INDEX.md:27` | `docs/codex-desktop-integration-v2.md` |
| v2.1 / v2.1.1 | `docs/codex-desktop-integration-v2.1.md`（§7 尾）| `docs/notebooklm-workflow-v2.md` |
| v2.1 / v2.1.1 | `skills/synthesis-to-ars-export/SKILL.md:25` | `docs/codex-desktop-integration-v2.md` |
| v2.1 / v2.1.1 | `skills/notebooklm-skill/SKILL.md:54` | `docs/notebooklm-workflow-v2.md` |

- v2.0 系列**無此問題**（檔名未改，引用一致）
- **修法**：8 處引用補上 `.1`（v2.1 是凍結版——但斷鏈屬於「改名造成的損壞」而非功能變更，建議連凍結版一起修；若想維持凍結版完全不動，也可只修 v2.1.1 並在 CHANGELOG 註記 v2.1 已知斷鏈）

---

## ⚠️ 低優先註記（可不修）

### A-4｜v2.0.1 的 workflow 文件無 v2.0.1 著墨
`docs/notebooklm-workflow-v2.md` 內文與尾註都停在「v2.0-kit 更新：2026-06-26」，搜尋品質補強只寫在整合模板與 AGENTS.md。v2.1.1 的 workflow 文件內文有補強行但尾註同樣停在 v2.1-kit。屬文件完整性問題，不影響功能。

### A-5｜v1.3-kit 的歷史命名混淆
- AGENTS.md 標頭寫「— v2」（當時指 AGENTS 文件自身版本，與後來的 kit 版號體系撞名）
- docs 檔名 `notebooklm-workflow-v2.md` 的「v2」指工作流架構版本（三角分工），非 kit 版本
- 屬歷史事實，v1.3 為最早凍結版，**建議不動**，僅此註記備查

### A-6｜凍結版無 citation_key 權威規則（符合設計，非問題）
v2.0 / v2.1 凍結版的 key 規則停在「作者+年份，衝突加 a/b」，無 BBT 權威條款——這是 .1 版才明定的，凍結版本來就該如此。使用凍結版時需自行留意 key 衝突風險。

---

## ✅ 通過項目

| 檢查項 | 結果 |
|--------|------|
| 凍結版污染（v2.0 / v2.1 是否混入 .1 版功能）| ✅ 全 kit 掃描「滾雪球/台灣中文/journal-rankings/search-log/三層徽章」零命中 |
| citation_key 權威規則三處一致性（.1 版 SKILL / AGENTS / 整合模板）| ✅ 措辭一致 |
| .1 版新功能完整性（Query Rewrite / 滾雪球 / 台灣中文 / 三件套）| ✅ 分佈正常 |
| 五 kit 間操作指引實質矛盾 | ✅ 無；凍結 vs 補強差異均符合成本雙軌設計 |
| wiki/INDEX.md 的 Kit 版本標記 vs AGENTS 標頭 | ✅ 四個 v2.x kit 一致（v1.3 無標記，屬早期格式）|

---

## 建議修正順序

1. **A-3 斷鏈**（8 處）——實際會讓使用者找不到文件，影響最大
2. **A-1 / A-2 版號漂移**（3 處）——誤導性低但一分鐘可修
3. A-4 可在下次功能更新時順手補；A-5 / A-6 不修

> 下一步：確認後由 Claude Code 執行修正（依批次計畫的「Fable 找問題、回頭確認再改」分工）。
