---
title: 決策框架
date: 2026-06-24
tags: [pkm/workflow]
type: reference
status: processed
---

## 一般原則

**最小必要原則**：增加機制前先問「這個功能的受益者是誰」。多人協作才有用的功能（子層編號、可信度標記、SCQA 變體），在單人使用的情境下砍掉。

**可逆性優先**：能改回去的決定先做，不能改回去的暫停確認。刪除改成 archive，修改保留原始檔。

**遇到不確定時，讀比寫便宜**：先讀現有條目，確認沒有重複，再決定要不要建新的。

---

## 知識庫內容路由

遇到一篇新素材，按以下順序判斷：

1. **已有高度重疊的條目嗎？**
   - 是 → 評估要不要更新現有條目，或直接標記為已處理（不建新條目）
   - 否 → 繼續往下判斷

2. **這個知識有多大的可重用性？**
   - 高（可反覆查閱、可操作、跨專案適用）→ `wiki/`（TBRC 格式）
   - 中（觀念性、背景補充、單次參考、實驗性）→ `ideas/`
   - 深度多來源分析 → `research/`

3. **幾篇文章在講同一件事？**
   - 3 篇以上 → 合併成一個 wiki 條目，不要各自建立
   - 有新角度 → 補充進現有條目，或建立「角度補充型」條目（如 hermes-agent-architecture-deep-dive 補充 hermes-agent）

---

## 功能/工具是否採用

| 條件 | 說明 |
|------|------|
| 解決目前有感的痛點 | 純「有趣」不夠，要有具體的使用場景 |
| 單人使用可以跑通 | 不依賴團隊配合才能發揮效果 |
| 成本可接受 | token 消耗、維護負擔、學習成本都算 |
| 可以漸進採用 | 能先用最小版本驗證，再決定是否深入 |
| 實驗後再導入全域 | 先在專案或功能層級實驗，確認有效再加進全域環境 |

---

## 優先順序判斷

**知識庫整理優先順序**：積壓 inbox 先清（raw-pipeline）→ synthesis 整合同主題條目 → elevation 升級跨主題概念框架

**開發流程（五情境）**，依改動複雜度與是否有 UI 需求選：

1. **簡單改動** → 直接手動修改
2. **需求明確（無 UI）** → 〔讀〕**現況層** → `/requirements` → agent 開發 → `/stack-review` → `/doc-update`
3. **需求明確（有 UI）** → `figma-to-requirements` → 〔讀〕**現況層** → `/requirements` → agent 開發 → `/stack-review` → `/doc-update`
4. **複雜 / 高風險（無 UI）** → 〔讀〕**現況層** → `/requirements` → `/ai-pair dev`（多模型協作審查）→ `/doc-update`
5. **複雜 / 高風險（有 UI）** → `figma-to-requirements` → 〔讀〕**現況層** → `/requirements` → `/ai-pair dev`（多模型協作審查）→ `/doc-update`

> **〔讀〕= 讀取既有產出，不是執行步驟**（不產生交付物、不需下指令）。
> 其餘節點都是要實際執行的 skill。

### 情境 2-5 的共同前置：現況層（2026-07-30 加入）

**現況層** = `project-documentation` 產出的**架構／API／變更歷程文件**
（本 repo 的 `docs/`，以及**上游 repo** 的——如 b2b-manager 的上游是 b2b-backend）。

情境 1 不需要（沒有 `/requirements`）；情境 4/5 最需要（高風險改動最禁不起契約猜錯）。
`figma-to-requirements` 與現況層**互不依賴**，兩者都是餵給 `/requirements`
（前者管 UI，後者管 API／架構），順序不影響結果。

⚠️ **這類文件是描述性的，會與程式碼漂移**——與 AGENTS.md／DESIGN.md（規範性、人工維護、不會失真）
性質不同。所以規則不是「讀了就信」：

| 判斷 | 動作 |
|---|---|
| 標 `⚠️ 推導未驗證`／`status: superseded`／**無 `source-commit`** | 翻原始碼 |
| JSON 鍵命名、包裝層數、分頁欄位名 | **預設不採信**，除非該處註明已從序列化配置確認 |
| 有 `source-commit` 且版本相近、未標未驗證 | 可採用，在任務文件註明來源章節 |

**「遇到不確定時，讀比寫便宜」成立的前提是現況層可信**——所以可信度標記不是加分項，
是這條原則能成立的必要條件。詳見 [[dev-workflow-optimization-tools]]。

---

## 升級條件（需要暫停確認的情況）

以下情況必須先問再動：

- **破壞性操作**：刪除、大量重寫、改動 raw/ 目錄
- **不確定歸屬的素材**：文章跨好幾個主題，不確定放哪
- **影響多個條目的變更**：修改 INDEX.md 結構、改標籤體系

---

## 相關連結

- [[cognitive-hub]] — 這份框架的設計參考來源（Rules & Protocols 層的定位）
- [[inbox-detection-protocol]] — 具體的 raw/ inbox 判斷協議（SOP 層）
- [[dev-workflow-optimization-tools]] — 五情境的工具層優化評估（ZeroSpec / planning-with-files / CCB / 省 token 四件套 / project-documentation 現況層等，本檔為其模式定義的權威來源）
