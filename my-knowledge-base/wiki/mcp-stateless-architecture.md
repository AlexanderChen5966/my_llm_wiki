---
title: MCP 無狀態（Stateless）架構：邁向企業級部署
date: 2026-07-22
updated: 2026-07-30
tags: [ai/mcp, ai/agent]
source: https://tools.wingzero.tw/article/sn/3991
type: article
status: processed
asset_value: 55
---

被譽為 AI 世界「USB-C」的 MCP（Model Context Protocol）最新協定版本正式導入 **Stateless（無狀態）架構**。這次更新沒有炫目的新功能，而是調整底層 Session 管理方式，讓 AI Agent 與 MCP Server 更容易水平擴充、更適合企業大規模部署。

## T（核心論點）

MCP 演進的重點從「模型更聰明」轉向「基礎建設更可擴充」。**舊版每個 MCP Server 必須保存每位 Client 的 Session**（記住連線狀態），同時服務大量 Agent 時記憶體負擔與負載平衡都變難；**新版改由每一次請求自帶所需內容完成操作**，Server 不再需要記住使用者連線資訊——這與現代 Web API 的無狀態設計一致。AI 的下一場競賽，除了模型能力，也在 MCP 這類開放標準與底層協定。

## B（適用場景）

**適用：**
- 開發需要水平擴充、高可用的 MCP Server / 企業 AI 平台
- 部署在 Kubernetes / Serverless 環境的 Agent 服務
- 同時服務數百員工、數千 Agent、每日數萬次 Tool Calls 的規模

**不適用 / 注意：**
- 一般聊天使用者幾乎感受不到差異
- 需要跨請求保持狀態的場景，狀態要外置（DB / 快取），而非放在 Server 記憶體

## R（Stateless 帶來的優勢）

Server 不再需要同步 Session，因此：

- 更容易**水平擴充（Horizontal Scaling）**——任一實例都能處理任一請求
- 更適合 **Kubernetes 與 Serverless** 架構
- 降低 Server **記憶體負擔**
- 提高系統**容錯能力**
- 更容易建立**高可用性**的企業 AI 平台

意義：AI Agent 不只是變聰明，而是真正開始具備企業級部署能力；未來開發 AI 應用不需為每個模型開發不同版本工具，透過同一套協定就能讓多模型共用相同工具與服務，企業建置成本持續降低。

## C（關鍵行動）

- [ ] 設計 MCP Server 時預設走無狀態，把需要保留的狀態外置到 DB / 快取
- [ ] 評估既有 MCP Server 是否還依賴 in-memory Session，規劃遷移
- [ ] 企業規模部署時，把 MCP Server 當成一般無狀態 Web 服務做水平擴充與負載平衡

## 補充資料：完整改版內容（2026-07-28 Release Candidate，07-30 併入）

來源：TechOrange（2026-07-29）整理 VentureBeat／MCP 官方部落格／The New Stack／AAIF。
**本節補上原條目未涵蓋的部分**——stateless 只是這次「史上最大更新」的其中一項。

### 為什麼舊架構卡住企業

過去 MCP 客戶端得和**特定一台伺服器維持連線**，伺服器把後續每個請求「釘」在發出連線的那台機器上。

- 本機桌面應用可行，但**搬上雲端要靠多台可增減的伺服器橫向擴展就成麻煩**——那台機器當掉，Agent 正在做的工作跟著中斷
- 企業被迫額外架設把請求導回同一台的機制，或讓流量閘道逐一拆解請求內容才知道送哪
- 主要維護者 Den Delimarsky：舊版本下「**只要一個運算節點掛掉，請求就會開始失敗**」

→ 企業得額外替協定處理分散式部署，**而不是直接利用既有雲端架構**。改版後 MCP 伺服器可像一般網路服務直接部署在負載平衡器後面。
**狀態沒有憑空消失，是從協定層交回應用層自行管理。**

> AAIF 執行董事 Mazin Gilbert 以網際網路作比：**當年 HTTP 能贏得全球信任，靠的正是開放標準、無狀態擴展與中立治理三件事同時成立**——這也是協定進企業正式環境的三個條件。

### 🆕 兩項新規格：Tasks 與 MCP Apps

| 規格 | 解決什麼 | 改變 |
|---|---|---|
| **Tasks** | 長時間任務（轉錄一小時錄音、分析大型程式碼庫、等 ERP 批次作業）過去只能**一直維持連線等結果**，或自行設計背景任務與通知機制；Client 中途關閉還得自己處理恢復、重試、狀態同步 | Server 先回傳 **Task Handle**，Client 日後再查詢結果，**不需維持連線**——背景工作、重試機制、任務狀態管理不再需要每個團隊各自設計 |
| **MCP Apps** | 過去 Agent 知道該請使用者確認資料、填表單、看報表，也只能先輸出文字，再由開發者自建 Web 頁面串接 | Server **直接回傳互動式介面**，把表單、儀表板等 UI 納入協定，不同 AI Client 能以一致方式呈現 |

> 重點不是「Agent 可以顯示介面」（本來就做得到），而是**把每家公司各自實作的能力變成生態系共用的標準**。

### 穩定性與安全強化

- **首度訂出正式的功能棄用政策**：任何功能從宣告淘汰到真正移除**至少間隔 12 個月**；只有出現公開安全通報才可縮短，**硬底線 90 天**。對要把 MCP 寫進長期系統的大型企業，這是不會被無預警推翻的承諾
- **封堵混淆攻擊（mix-up attack）**：新版**強制客戶端驗證授權回應的來源**，防止客戶端被誘導把授權回應誤認為來自另一個身分伺服器。Delimarsky 強調這是**預防性工程強化，並非因為 MCP 實際遭攻擊**
- **企業受管授權**（與 Okta 合作的擴充）：企業能以既有公司身分系統統一控管誰能存取哪些 MCP 伺服器

### 治理現況（從一家公司的開源專案 → 多廠商共治）

- Anthropic 於 **2024-11** 創建，**2025-12** 捐給 Linux Foundation 旗下 **AAIF**
- 核心維護者橫跨 **Anthropic、微軟、OpenAI、Google、Amazon** 五大科技公司
- 共同創建者 David Soria Parra 坦言技術上**仍握有否決權但從未動用**，**Anthropic 貢獻占比已降到半數以下**
- **AAIF 成員從成立時約 40 家成長到 240 家**，且擴散到零售、金融、電信——「使用者」進場代表要影響協定走向
- **SDK 下載量半年翻倍，目前每週約 2.5 億次**（對照：2025-12 捐贈時 Python + TypeScript 合計每月約 9,700 萬次）

## 補充資料：規格已定案，SDK 高階 API 還沒跟上（2026-09-04 實測）

來源：〈自己做一個 MCP server:237 行，第一次跑就抓到我三個月前洩漏的東西〉（MikeWang，Medium，2026-09-04）
https://medium.com/@n913239/自己做一個-mcp-server-237-行-第一次跑就抓到我三個月前洩漏的東西-921040a17e7b

本條目描述的是 **spec 層面**的演進。實際落地有一段時間差，作者用 Python SDK 2.0.0 建了一台本機 stdio
server，直接發原始 JSON-RPC 驗證，得到兩個與規格不符的結果：

- **實際協商出的協定版本是 `2025-11-25`，不是 `2026-07-28`**。SDK 2.0.0 的常數雖寫著
  `LATEST_PROTOCOL_VERSION = 2026-07-28`，但高階 `MCPServer` 跑的仍是**有 `initialize` 握手的舊協定**。
- **`server/discover` 沒有實作**。規格寫明 server **MUST** 實作，實測回 `-32601 Method not found`。
  規格定案五週，高階 API 尚未跟進。

🔑 **對本條目的意義**：「協定變 stateless」目前是**規格事實而非執行時事實**。
評估既有 MCP Server 是否要遷移（C 層第二點）時，先實測自己那台實際協商出什麼版本，
不要以規格發布日推定。**文件會過期，連程式碼裡的常數都可能誤導你；唯一確定的方法是對它發一個 request。**

📌 **影響範圍的界線**：本機 stdio server 一開一關就是一個 process，本來就沒有 session 要維護，
因此幾乎不受影響；stateless 的實質收益（拿掉 session 後不必黏在同一台機器、可直接放到一般負載平衡後面）
集中在**部署於 HTTP 後面的 remote server**——與本條目 B 層的適用場景一致。

## 相關連結
- [[self-built-mcp-server-237-lines]] — 本補充的來源：237 行自建 MCP server 的實測，含 SDK 2.0.0 破壞性變更
- [[microsoft-mcp-for-beginners-course]] — 🔑 **同一條版本線的教材側**：Microsoft MIT 課程對齊 spec 2025-11-25，而 **2026-07-28 RC 把傳輸層改 stateless 並廢棄 Roots／Sampling／Logging**——學它的人要知道課綱比協定舊
- [[agent-skills-vs-mcp]] — ⚠️ 該篇原主張「MCP 需持續連線」與本篇無狀態架構衝突，已於 2026-09-03 在該篇 T 層改寫並指回本篇
- [[mcp-stateless-migration-state-ownership]] — **遷移實作指南**：state 沒消失只是變成你的責任（handle 契約／idempotency／protocol adapter／關掉 sticky routing 測試）
- [[mcp-fastmcp-server]] — FastMCP Server 實作（把工具做成獨立可攜服務），本篇是該協定的架構演進背景
- [[ai-skills-mcp-plugins-agents-comparison]] — MCP 在 Skills/Plugins/Agents 四概念中的定位（通訊協議層）
- [[agent-skills-vs-mcp]] — Skills vs MCP 選型：即時外部資料走 MCP

## 原始摘要

DeTools 工具翼零文章。MCP 新版導入無狀態架構，Server 不再保存 Session、改由每次請求自帶內容，換來易擴充、適合 K8s/Serverless、低記憶體負擔與高可用，標誌 AI Agent 具備企業級部署能力，AI 競爭延伸到底層基礎建設與開放標準。
