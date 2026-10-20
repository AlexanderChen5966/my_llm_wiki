---
title: AI 驅動規格驅動開發（SDD）四框架比較：ZeroSpec / OpenSpec / SpecKit / BMAD
date: 2026-06-15
tags: [ai/agent, dev/architecture, type/comparison]
source: 自製分析文件（2026-05-08）
type: reference
status: processed
asset_value: 88
---

四套 AI SDD 框架各自解決不同問題，**ZeroSpec 是 Layer 0 環境基礎設施**（告訴所有 AI 工具「這個專案是什麼」），其他三套是 **Layer 1 工作流框架**（告訴 AI「這個功能要怎麼做」）。ZeroSpec 必須先裝好，其他三套才能在上面正確運作。

## T（核心論點）

四框架的根本差異在於**回答的問題不同**：

| 框架 | 回答的問題 |
|------|----------|
| **ZeroSpec** | 「這個專案的 AI 工具需要知道什麼？」（環境基礎）|
| **OpenSpec** | 「這個功能要做什麼，做完後怎麼歸檔？」（輕量迭代）|
| **SpecKit** | 「這個產品從構想到上線要怎麼治理？」（企業治理）|
| **BMAD-METHOD** | 「這個系統要怎麼讓多個 AI 角色協作完成？」（多 Agent 協作）|

## B（適用場景）

**適用：**
- 為新專案選擇 AI SDD 框架組合
- 評估是否需要在 ZeroSpec 之上疊加更完整的工作流框架
- 了解 OpenSpec / SpecKit / BMAD 的使用場景差異

**不適用 / 注意：**
- 此分析文件建立於 2026-05-08，各框架可能持續更新
- BMAD-METHOD 學習曲線高（19+ 角色 Agent），不適合個人或小團隊快速迭代場景

## R（完整比較）

### 四框架對照表

| 維度 | ZeroSpec | OpenSpec | SpecKit | BMAD |
|------|---------|---------|---------|------|
| **定位** | 專案 AI 導覽基礎 | 輕量迭代規格 | 企業治理規格 | 多 Agent 開發方法論 |
| **時間軸** | 持續同步（程式碼後）| 功能前 + 歸檔 | 功能前 + 治理 | 全生命週期 |
| **文件對象** | AI coding agent | 開發者 + AI | 開發者 + PM | 多角色 Agent 團隊 |
| **核心產出** | AGENTS.md / SPEC / ADR / SA | proposal / specs / design / tasks | Constitution / PRD / Plan / Tasks | PRD / 架構 / User Story / 代碼 |
| **內建 Agent** | 無 | 無 | 無 | 19+ 專業角色 |
| **企業就緒性** | 中（文件治理）| 低～中 | 高 | 高 |
| **學習曲線** | 低 | 低 | 中 | 高 |
| **適用規模** | 任何 | 個人到中型 | 中型到大型 | 中型到企業 |

### 時間軸層次關係

```
Layer 0: ZeroSpec（AGENTS.md）────────────────────── 持續維護
  └─ 先裝好，其他框架才能在上面正確運作

Layer 1（三選一或組合）：
  OpenSpec:   [propose] ──→ [apply] ──→ [archive]
  SpecKit:    Constitution → Specify → Plan → Tasks → Implement
  BMAD:       分析 → 規劃 → 解決方案 → 實施（四階段）
```

### 重要區別：SPEC 概念

OpenSpec 的 `specs/`（實作前，意圖）和 ZeroSpec 的 SPEC（實作後，現實）名稱相近但性質完全不同：

| | OpenSpec `specs/` | ZeroSpec SPEC |
|---|---|---|
| **時機** | 實作前（意圖）| 實作後（現實）|
| **內容** | 需求場景、用戶故事 | API 合約、DTO、業務規則 |
| **生命週期** | 功能完成後歸檔 | 長期維護、隨程式碼更新 |

## 推薦組合方式

| 情境 | 建議 | 說明 |
|------|------|------|
| **現有專案日常維護** | ZeroSpec + OpenSpec | 輕量、迭代快速，適合 App 維護型專案 |
| **新專案 + 有 PM 參與** | ZeroSpec + SpecKit | 五階段治理，需要合規文件的企業專案 |
| **複雜系統 + 多 Agent 協作** | ZeroSpec + BMAD | 19+ 角色 Agent 協作審查，大型系統 |
| **最輕量** | ZeroSpec 單獨使用 | 只需 AI 文件導覽 |
| **企業級全配** | ZeroSpec + SpecKit + BMAD | 治理 + 多 Agent + 完整文件 |

**注意**：BMAD 的 `project-context.md` 與 ZeroSpec 的 `AGENTS.md` 職責重疊，建議以 AGENTS.md 為主，project-context.md 改為 `@AGENTS.md` 引用。

## C（關鍵行動）

- [ ] 個人知識庫場景（維護型）→ ZeroSpec + OpenSpec 組合最適合
- [ ] 評估現有 wiki CLAUDE.md 的「任務路由表」是否可被 ZeroSpec AGENTS.md 涵蓋

## 相關連結

- [[zerospec]] — ZeroSpec 完整說明（Layer 0 上下文整備框架）
- [[spec-driven-ai-workflow]] — spec-kit / OpenSpec 規格驅動工作流詳細說明
- [[synthesis-spec-driven-workflow]] — 規格驅動工作流的 CAVE 精粹（四層鏈路分析）
- [[agents-md-guide]] — AGENTS.md 設計原則（ZeroSpec 的核心產出）
- [[context-engineering-fundamentals]] — 多 Agent 拓撲與 context 管理理論基礎
- [[concepts/meta-ai-engineering-methodology]] — 三層職責分離框架（此四套框架的上層概念）
