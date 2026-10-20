---
title: 截圖 → 需求 → 實作：用 DESIGN.md 驅動的 UI 開發工作流
date: 2026-06-23
tags: [dev/flutter, ai/agent, pkm/workflow]
source: 對話設計（app01-double / b2b-manager 實際專案）
type: idea
status: archived
---

> **歸檔（2026-06-23）**：此工作流已從構想落地為實際實作，移入 archive/ 保存。下方「實作狀態」記錄最終成果。

把 Figma 截圖轉成精確 UI 任務需求的工作流設計。核心：**截圖辨識的精確度不來自工具，來自「專案特定的 DESIGN.md token 詞彙」**。把設計系統寫進環境（DESIGN.md），讓通用 workflow 對著它把截圖元素對應到真實 token，而非 hardcode。呼應 [[harness-engineering]] 的環境優於模型。

## 實作狀態（2026-06-23 已落地）

| 項目 | 狀態 |
|------|------|
| `figma-to-requirements` skill | ✅ 建立 v1.0.1，安裝於**全域** `~/.claude/skills/`（讀「截圖所屬 repo 的 DESIGN.md」，跨專案通用）|
| `figma-to-flutter` skill | ✅ 退役歸檔；元件對應表併入各專案 DESIGN.md（元件名對齊現行 codebase）|
| b2b-manager DESIGN.md | ✅ 既有 + 併入 Figma→Flutter 元件對應表 |
| app01-double DESIGN.md | ✅ 新建（反映真實 token：SLColor extension、SLText 無 screenutil）|
| CLAUDE.md DESIGN.md 載入 | ✅ 兩專案皆改「引用」（Claude on-demand）；GEMINI.md 維持 `@import`（Gemini 無 rules 機制）|
| 既有開發流程 | ✅ 不變，僅在 `/requirements` 前插入 figma-to-requirements |

**screenutil 釐清**：b2b 設計慣例為「新 UI 固定 px」，但 `flutter_screenutil` 套件與 `ScreenUtilInit` 仍保留供既有部分使用（**未移除依賴**）。app01 本就不用 screenutil。

## 完整工作流

```
docs/assets/<功能>/*.png（一組功能流程截圖）
        ↓ figma-to-requirements（通用 skill，讀「本專案」DESIGN.md）
          產出：UI 規格 + token 對應表 + 狀態矩陣 + 待新增 token 清單
        ↓ 文字補功能邏輯
   requirements（影響範圍 + 風險 + 任務拆解）
        ↓
   docs/shared/<功能>_task.md（完整任務文件）
        ↓ 實作模式二選一（既有流程不變）
   A: Claude 開發 → /stack-review → /doc-update
   B: /ai-pair dev（含 review）→ /doc-update
        ↓（建議新增）對照截圖驗收 → /doc-update
```

## 三個關鍵設計決策

### ① figma-to-requirements 與 requirements 不合併
「最終產出同一份文件」≠「必須同一個 skill」。一份文件可由兩 skill 各填一段（**共用模板**）。
- requirements 仍需獨立處理**純文字需求**（多數任務不是從 Figma 來）→ 合併會讓它背著視覺指令的包袱
- 分工：figma-to-requirements 做**視覺→token 對應**；requirements 做**影響範圍/風險/任務拆解**
- 用共用 task-doc 模板達成「同一份文件」，不靠合併 skill

### ② 不通用化 figma-to-flutter，改通用化 workflow
figma-to-flutter 之所以精確，正因綁死 b2b 慣例。通用化它＝退回「通用 Flutter 建議」＝回到不精確。
- 該通用的是 **workflow**（figma-to-requirements 讀各專案 DESIGN.md）
- 專案差異放進**各自的 DESIGN.md**（b2b 有 screenutil 規則、app01 沒有）
- 新增第三個 Flutter 專案 → 只寫一份 DESIGN.md，不寫新 skill

### ③ figma-to-flutter 退役（但先搬知識）
新管線下「截圖→code」這個動作多餘了——完整任務文件 + DESIGN.md + CLAUDE.md 已足夠讓 agent 直接實作。
- ⚠️ 但 figma-to-flutter 藏著踩坑換來的知識（**Figma→Flutter 元件對應表**、**響應式尺寸判斷規則**），要先搬進 DESIGN.md，**搬完再退役**，不是先刪
- app01 本來就不適用 figma-to-flutter（它不用 screenutil），DESIGN.md 建好後直接走三段管線

## 既有開發流程是否要改

**不改結構，只在最前面插一段。** 下游（stack-review / doc-update / ai-pair）零修改：
- 改的只有 `/requirements` 的**輸入變豐富**（多了截圖前處理）
- 隱性依賴：實作步驟更依賴 DESIGN.md 完整度（所以 DESIGN.md 品質現在更重要）
- 兩種實作模式（agent+review / ai-pair）與「加不加 Figma 前處理」正交，照用

## 提升精確度的五個機制（與工具無關）

1. **DESIGN.md token 詞彙** — 模型把截圖藍色對應到 `SLColor.primary` 而非猜 HEX
2. **狀態矩陣強制產出** — 「需求不精確」最大破口是漏狀態；截圖已列出成功/失敗/有無配件等狀態，要全抓
3. **「待新增 token」當關卡** — 截圖出現 DESIGN.md 沒有的色 → 標「先在 color.dart 新增」，防止中途 hardcode
4. **對照截圖驗收** — review→doc-update 之間補視覺驗收（build 截圖 vs Figma 並排），code review 看不出「按鈕位置跑掉」
5. **維持少 skill** — figma-to-requirements + requirements + DESIGN.md 已足夠，驗收用手動，別再長新 skill

## 實作要點

- 多畫面當**一組 flow** 處理（畫面順序 + 跨畫面共用元件 + 每畫面狀態），非 N 份獨立規格
- 善用截圖**檔名語意**（如「未選擇站點，洗車方案不能選擇」帶 business rule 線索）
- 需求文件落點：兩專案都用 `docs/shared/`（已確認 app01 也有此目錄）

## 相關連結
- [[open-design-taste-skill-integration]] — DESIGN.md 當唯一真相的同源思路
- [[harness-engineering]] — 環境優於模型：把慣例寫進 DESIGN.md/CLAUDE.md 而非專用 skill
- [[get-shit-done-gsd-framework]] — 按功能流程拆 phase、需求覆蓋率追蹤的同構概念
- [[concepts/agent-workflow-taxonomy]] — figma-to-requirements 屬 ⑥ Skills 工作流、DESIGN.md 屬環境層
