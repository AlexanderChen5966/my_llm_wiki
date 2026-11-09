---
title: 決策框架
date: 2026-06-24
updated: 2026-11-09
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

**開發流程（五情境）**，依改動複雜度與是否有 UI 需求選（2026-11-09 更新為**流程 v3**：五情境的骨架不變，新增的都是情境內的條件步驟，見下一節）：

1. **簡單改動** → 直接手動修改（照「外科式修改」：每一行變更都要對應需求）
2. **需求明確（無 UI）** → 〔讀〕**現況層** → `/requirements` → agent 開發 → `/stack-review` → `/doc-update` → `pr`
3. **需求明確（有 UI）** → `figma-to-requirements` → 〔讀〕**現況層** → `/requirements` → agent 開發 → `/stack-review` → `/doc-update` → `pr`
4. **複雜 / 高風險（無 UI）** → 〔讀〕**現況層** → `/requirements` → `/ai-pair dev`（多模型協作審查）→ `/doc-update` → `pr`
5. **複雜 / 高風險（有 UI）** → `figma-to-requirements` → 〔讀〕**現況層** → `/requirements` → `/ai-pair dev`（多模型協作審查）→ `/doc-update` → `pr`

> **〔讀〕= 讀取既有產出，不是執行步驟**（不產生交付物、不需下指令）。
> 其餘節點都是要實際執行的 skill。`pr` 只寫 MR 說明；commit、push、開 MR 一律人工。

### 情境內的條件步驟（流程 v2／v3，2026-11-09 加入）

符合條件才做，**不是新的情境**——它們是情境 2～5 內部的方法選擇（同 2026-07-29 對 TDD 的判斷：開發方法不另立情境，否則「情境」會混入兩種分類基準）。

| 時機 | 條件 | 做什麼 | 來源 |
|---|---|---|---|
| `/requirements` 中 | 需求模糊：基本資訊答不出 2 項以上、多種做法會改變影響範圍、只講結果沒講觸發情境 | `grilling` 追問；能查的先查，只問只有人能決定的事 | Matt Pocock（v2） |
| `/requirements` 中 | 用到框架／套件的寫法 | 依 lock 檔版本查官方文件並附網址，查不到標「未驗證」 | Addy source-driven（v3） |
| `/requirements` 中 | 每個任務 | 寫可檢查的「驗證：」 | Karpathy step → verify（v3） |
| agent 開發 | 新程式（如 `next/`） | `tdd` | Matt Pocock（v2） |
| agent 開發 | bug 卡住 | `diagnosing-bugs`：先做出能重現的回饋迴路 | Matt Pocock（v2） |
| agent 開發 | 碰到風險清單上的單一決定（認證、路由權限、跨服務契約、狀態結構、不可逆操作） | `doubt-check`：新 context reviewer 只拿產物＋契約找錯，最多 3 輪；跨模型每次先問 | Addy doubt-driven（v3） |
| `/stack-review` | 有 `docs/shared/` 任務文件 | 自動加派 spec subagent，與規範審查平行、結果分開列 | matt-code-review（v2） |
| `/doc-update` | 每次收尾 | Step 7 沉澱判斷：不明顯、長期有用、忘了會出事才留，依序選測試 → 專案規則 → 三層記憶 → 專案文件 → knowledge-capture；**寫入前先給使用者看** | Compound Engineering 觀念（v3） |

### 情境 2/3 與 4/5 的分界（2026-11-09 決定）

- **一般任務走情境 2/3**；開發中遇到風險清單上的**單一**決定，用 `doubt-check`，不必整個任務升級
- **一開始就知道會動到多個風險區塊**，才走情境 4/5（`/ai-pair dev`）
- 前提不變：只有產生需求文件的任務才走 ai-pair，小改動直接做

> 流程版本頁：https://claude.ai/artifact/TVWdDupHzBGXhF8nusPHPz （v1 基準 → v2 Matt 截長補短 → v3 借來的做法）。
> 版本比對工具：`~/.agents/experiments/workflow-bench/`（回放 SSGS-13687，v1／v2／v3 都 14/14；費用 $4.18／$4.94／$4.79）。

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
- [[ai-dev-workflow-taxonomy]] — 六個失敗點地圖，對照流程 v3 每一步補的是哪個失敗點
- [[dev-workflow-optimization-tools]] — 五情境的工具層優化評估（ZeroSpec / planning-with-files / CCB / 省 token 四件套 / project-documentation 現況層等，本檔為其模式定義的權威來源）
