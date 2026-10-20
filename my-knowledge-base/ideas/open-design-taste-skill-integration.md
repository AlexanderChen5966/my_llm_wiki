---
title: Open Design × taste-skill 整合：探索→固化→量產的設計工作流
date: 2026-06-23
tags: [ai/agent, ai/claude]
source: 對話整合（open-design-claude-design-comparison + taste-skill-anti-slop-frontend）
type: idea
status: processed
---

把 Open Design（設計編排層）與 taste-skill（Skills 判斷層）放在一起評估後的結論：**兩者可結合，但不該並排當兩個權威同時用，而是分階段各司其職**。核心心法——taste-skill 負責「從無到有的品味注入」，Open Design 的 DESIGN.md 負責「品味固化後的一致量產」。

## 為什麼能結合：兩者在不同抽象層

對應 [[concepts/agent-workflow-taxonomy]] 的「多數系統是疊加非六選一」：

| | Open Design | taste-skill |
|---|---|---|
| 層級 | **編排層（③）**：驅動 coding agent 的設計工作台 | **Skills 工作流（⑥）**：裝進 agent 的判斷規則 |
| 接口 | 驅動 Claude Code / Codex / Cursor CLI | `npx skills add` 裝進同一個 CLI |
| 真相來源 | DESIGN.md 九段格式（color/typography/spacing/layout/components/motion/voice/brand/anti-patterns）| 通用反 slop 規則 + 三旋鈕（VARIANCE/MOTION/DENSITY）|

**接點在底層 coding agent**：Open Design 呼叫 agent 生成前端時，taste-skill 的規則就在 agent 的 context 裡，自動共存。

## 衝突點：兩個權威會打架

taste-skill 自己的「不適用」寫明：**「已有嚴格 Design System 規格的專案（可能衝突）」**。Open Design 恰恰就是 design system 框架。
- 若 DESIGN.md 指定「嚴格 editorial 網格」，而 taste-skill `DESIGN_VARIANCE=9` 要「非對稱實驗佈局」→ 兩個權威矛盾，產出反而不一致。
- 結論：**有了專屬 DESIGN.md，就不需要通用品味預設了。**

## 整合工作流：探索 → 固化 → 量產

```
1. 探索期（還沒有 design system）
   agent + taste-skill 變體（minimalist / brutalist / soft…）
   → taste-skill 甜蜜點正是「無 design system、輸出太通用」時注入品味
   → 產出 2-3 個有個性的方向

2. 鎖定方向
   把選中方向「固化」進 Open Design 的 DESIGN.md 九段格式
   → DESIGN.md = 唯一真相來源（single source of truth）

3. 量產期
   Open Design 驅動 agent 對著 DESIGN.md 生成
   → taste-skill 此時移除，或把三旋鈕「調到與 design system 一致」
   → 不要讓它繼續當獨立權威

4.（可選）品質檢查
   用 taste-skill 的 redesign-existing-projects 反向 audit 產出有沒有 slop
```

## 關鍵推論

- **採用 Open Design 全套** → taste-skill 多餘（甚至衝突），靠 DESIGN.md 就夠
- **不用 Open Design、只用裸 agent** → taste-skill 才有意義（`npx skills add` 一行裝好的輕量品味解）
- taste-skill 的真正定位：**給「不跑 Open Design」的人的冷啟動品味工具**

## 待驗證

- [ ] `od skill list` 看 Open Design 內建 259+ skills 的設計品味品質，確認是否已涵蓋（避免重複裝 taste-skill）
- [ ] 唯一例外：若某特定風格（如極端 brutalist）Open Design 做得不如 taste-skill 對應變體，可單獨 cherry-pick 該變體

## 相關連結
- [[open-design-claude-design-comparison]] — Open Design vs Claude Design 完整比較
- [[taste-skill-anti-slop-frontend]] — taste-skill 反 slop 前端設計套件
- [[concepts/agent-workflow-taxonomy]] — 為何兩者是不同抽象層、可疊加
- [[synthesis-practitioner-skills-design]] — 把「品味」這種隱性判斷顯性化成可重用機制
