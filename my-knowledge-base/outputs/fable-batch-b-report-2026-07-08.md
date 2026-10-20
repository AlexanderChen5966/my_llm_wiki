---
title: Fable 批次 B 報告 — ai-pair-main 版本溯源與歸檔判斷
date: 2026-07-08
tags: [ai/claude, pkm/workflow, project/knowledge-base]
type: reference
status: processed
---

# 批次 B 報告：ai-pair-main 版本溯源與歸檔判斷

> 執行：2026-07-08（Fable 5）｜範圍：實驗用文件（ai-pair 8 版本 + Skill存檔/pkm + cross-tool 三稿）
> 檢查方法：全版本 SKILL.md 版號萃取 + 與使用中版本（`~/.claude/skills/ai-pair`）逐檔 diff + pkm 9 skill 與正本 diff
> 結論：**版本演進鏈完整重建；發現 1 個同名衝突、1 個「存檔比正本新」的倒置、4 個歸檔整理項**
>
> ✅ **修正狀態（2026-07-08，使用者確認 v4.3.6 有調整模型只是未同步）**：
> - B-1 ✅ v4-test 移入 `Skill存檔/ai-pair-v4-test-v4.1.0測試副本`（同名衝突解除，`~/.claude/skills` 只剩一個 ai-pair）
> - B-2 ✅ 存檔 reference 同步回使用中（新增 v4.4-opencode遷移/v4.5/v4.5.1/phase2/重構建議-v3 共 5 項；覆寫 v4.2.5/v4.3-agy 共 2 項；舊編號 v4.4 移入廢棄區並註記已重編為 v4.5；agy --model 事實修正確認生效）。**此後唯一真相源：`~/.claude/skills/ai-pair/`**
> - B-4 ✅ v2.3.1 copilot 版歸位 `Skill存檔/ai-pair-main-v2.3.1`，版本鏈完整
> - B-6 ✅ cross-tool v1/v2 移入廢棄區，v3 留原地（=GitLab 正式版複本）
> - B-3 / B-5 ✅ 各建 `README-快照說明.md` 註記（資料夾版號不符說明、pkm 快照日期與正本位置）

---

## 版本演進鏈（重建結果）

```
v1.3.0 ──→ v2.3.1 ──→ v3.1.0 ──→ v4.1.0 ──→ v4.2.0 ──→ v4.2.5 ──→ v4.3.0 ──→ v4.3.5 ★使用中
(存檔)    (根目錄     (存檔)     (存檔＝      (存檔)     (存檔)     (存檔)    (= 存檔 v4.3.6 資料夾)
          copilot版)             v4-test)                                      │
                                                                               └→ 前瞻設計 v4.4 / v4.5 / v4.5.1
                                                                                  （僅存於存檔 reference/，live 沒有）
```

- **使用中版本**：`~/.claude/skills/ai-pair` = **4.3.5**
- 存檔完整保存了演進各階（v2.3.1 除外，它散落在實驗用文件根目錄）

---

## ❌ 需修正（確認後執行）

### B-1｜使用中目錄的同名衝突（影響最大）
`~/.claude/skills/` 同時存在 **ai-pair（4.3.5）** 與 **ai-pair-v4-test（4.1.0）**，兩者 SKILL.md 的 `name:` 欄位**都是 `ai-pair`**，description 也相同——這會造成 skill 觸發歧義（本 session 的可用 skill 清單中兩者並列，說明兩份都被載入）。
- v4-test 的 SKILL.md 經 diff 確認**與存檔 v4.1 逐字相同**，是過時的測試副本
- **修法**：把 `~/.claude/skills/ai-pair-v4-test` 移入 `Skill存檔/`（或直接刪除，存檔 v4.1 已有相同內容）

### B-2｜「存檔比正本新」的倒置（事實矛盾級）
`Skill存檔/ai-pair-main-v4.3.6/reference/` 比使用中的 `~/.claude/skills/ai-pair/reference/` **更新**：

| 項目 | 存檔 v4.3.6 | 使用中 ai-pair |
|------|------------|---------------|
| 重構建議-v4.4-opencode遷移.md / v4.5.md / v4.5.1.md | ✅ 有 | ❌ 無（只有舊的 v4.4.md）|
| phase2-mai-vs-gpt54mini/ 實驗紀錄 | ✅ 有 | ❌ 無 |
| 重構建議-v4.2.5.md 成本表 | 「更新至 2026-06-25」+ Credits 單位 | 舊版「v4.2 成本表」|
| 重構建議-v4.3-agy遷移.md 對 `agy --model` 的描述 | ✅「支援（格式不同）」| ❌「無此旗標」——**兩地對同一事實描述相反**（差異 223 行）|

- **修法**：以存檔 v4.3.6/reference 為準，同步回使用中的 ai-pair/reference（前瞻設計 v4.5 Mesh 正是 wiki 條目 [[ai-pair-skill]] 曾引用的那份）；或至少明定「reference 文件的唯一真相源在哪」

### B-3｜資料夾名與內容版號不符
`Skill存檔/ai-pair-main-v4.3.6/` 的 SKILL.md 實際 version 是 **4.3.5**（與使用中版本逐字相同），資料夾名超前了一號。
- **修法**：資料夾改名 `ai-pair-main-v4.3.5-snapshot`，或在資料夾內放一行 README 說明

### B-4｜根目錄散件應歸位
`實驗用文件/ai-pair-main claude + copilot/`（v2.3.1）是版本鏈中 v1.3→v3.1 的缺口補齊者，但放在根目錄而非 Skill存檔。
- **修法**：搬入 `Skill存檔/ai-pair-main-v2.3.1/`（符合 CLAUDE.md「停用內容移入 archive」規則）

---

## ⚠️ 註記（低優先）

### B-5｜Skill存檔/pkm/ 落後正本（4/9）
| skill | 存檔 | 正本（notes/skills）|
|-------|------|------|
| daily-brief-generator | 1.2.0 | 1.3.0 |
| knowledge-organizer | 1.1.0 | 1.4.0 |
| raw-pipeline | 1.3.0 | 1.6.0 |
| wiki-health-check | 1.3.0 | 1.4.0 |

其餘 5 個相同。存檔本質是快照，落後半預期；建議在 `Skill存檔/pkm/` 放一行說明「快照日期約 2026-05，正本在 /Users/alexander/notes/skills/」，避免誤當來源使用。

### B-6｜cross-tool-ai-setup 三稿 → 正式版已收斂 ✅
- **v3 與 GitLab 正式版逐字相同**（diff 確認）
- v1（362 行）→ v2（494 行）→ v3（518 行）：章節結構完全一致，演進為內容增補，v3 已涵蓋前兩版要點
- **建議**：v1 / v2 移入 `Skill存檔/廢棄區/`，實驗用文件根目錄留 v3 或直接指向 GitLab 正式版

---

## ✅ 通過項目

- `Skill存檔/廢棄區/` 現有 2 個 .skill 檔（flutter-b2b-documentation-v1 / code-review-v1）標記得當
- 存檔版本鏈無缺漏斷代（v2.3.1 歸位後即完整）
- 使用中 ai-pair 4.3.5 的 SKILL.md 本體與最新存檔一致（分歧只在 reference/）

## 建議修正順序

1. **B-1 同名衝突**——正在影響 skill 觸發，一分鐘可修
2. **B-2 reference 倒置**——事實矛盾（agy --model），會誤導未來的重構決策
3. B-4 / B-3 歸檔整理
4. B-5 / B-6 順手做

> 修正狀態：⬜ 待確認
