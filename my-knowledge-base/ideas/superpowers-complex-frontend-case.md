---
title: Superpowers 複雜前端改造實戰（交叉表 Canvas 化案例）
date: 2026-06-17
tags: [ai/agent, ai/claude]
source: https://codelove.tw/@tony/post/qEkP73
type: idea
status: processed
asset_value: 38
---

袋鼠雲數棧 easyIndex 交叉表改造的 superpowers 實戰案例，核心貢獻是一套**五步可遷移協作模板**，適用於所有跨層聯動的複雜前端需求。

**五步可遷移協作模板：**
1. **需求歸一化**：明確目標、範圍、**非目標**、驗收標準（先統一問題定義再實作）
2. **上下文收斂**：只暴露高相關模組（控制**修改半徑**，不是省 token）
3. **關鍵契約先立**：欄位、列舉、模式邊界先統一（避免兩套欄位並行演進）
4. **分段實作**：每段必須可獨立驗證
5. **失敗結構化回灌**：基於「錯誤類型/位置/重現路徑/期望差異」修復，而非讓 AI 自由猜測

**交叉表案例三關鍵決策：**
- `dimensions` + `dimType: 'row' | 'column'` 統一語義（不拆兩套欄位）
- `resultDisplayMode` 決定組參分流（先鎖模式邊界，再讓 AI 補實作細節）
- Canvas 渲染先拆職責（corner/header/frozen/body 四層畫布）再寫程式碼

**AI 最適合承擔：** 結構化樣板程式碼生成、已確定契約下的局部補全、明確邊界內的拼接
**人工聚焦：** 架構決策、模式邊界判斷、整合驗證與收口

完整 superpowers 框架見 [[superpowers-framework]]。
