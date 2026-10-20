---
title: Agent Skills vs MCP：選擇判斷與 Token 效率
date: 2026-05-13
updated: 2026-07-31
tags: [ai/claude, ai/mcp, ai/agent]
source: https://israynotarray.com/ai/20260120/4189755271/
type: article
status: processed
asset_value: 75
---

## T（核心論點）

> 🔴 **T 層已修正（2026-09-03，兩次健檢指認）**：原句「Agent Skills 比 MCP 更省 Token——MCP 需要持續連線且每次都消耗 Context」**兩個前提都已失效**。

**修正後的說法：**

> **在工具定義全量載入的前提下**，Skills 較省 Token（靜態指令包、只在觸發時載入）；**啟用 Tool Search／Deferred Tool Loading 之後，兩者的載入成本差距大幅縮小**——只有工具名稱進上下文，完整 schema 按需取回。

兩個前提各自為何失效：

| 原前提 | 現況 |
|---|---|
| 「即使沒用到也得載入數百行工具定義」 | 🔴 **Claude Code 預設已改為 Tool Search 延遲載入**，只有工具名進上下文（見 [[claude-code-cost-optimization-7-tips]]、[[claude-code-context-management]]）|
| 「MCP 需要**持續連線**」 | 🔴 與 [[mcp-stateless-architecture]] 對不上——新版協定**每請求自帶內容、Server 不保存 Session** |

⚠️ **不能用日期判誰對**：原句是 2026-07-31 才併入本條目的補充，**比對方來源更新**，卻反映了更舊的機制。這正是「修正未傳播」的典型——本次已在 T 層直接改寫而非僅加註。

**仍然成立的部分**：Skills 的 Progressive Disclosure 在「判斷邏輯」這一側依然有結構優勢（見下方補充資料的四層與 Context Hoarding），與工具定義的載入成本是兩件事。

## B（適用場景）
**Skills 適合：** 重複流程、靜態判斷邏輯、不需要即時資料的場景
**MCP 適合：** 需要動態工具、即時外部資料、資料庫查詢的場景
**不適用：** 兩者都不是萬能，複雜場景考慮組合使用

## R（操作步驟）
選擇 Skills vs MCP 的決策流程：
1. 問：「這個任務需要即時外部資料嗎？」→ 是 → MCP；否 → Skills
2. 問：「這個流程會重複執行嗎？」→ 是 → Skills（封裝）；否 → 直接 Prompt
3. 問：「需要跨工具協作嗎？」→ 是 → 軟編排型 Skills + MCP 組合

## C（關鍵行動）
- [ ] 裝了太多 MCP Server 會讓 Context 在開始工作前就被吃光
- [ ] Skills 的 trigger 設計要精確，避免不必要的載入
- [ ] 先用 Skills，真正需要動態工具時再加 MCP

## 原始摘要
文章以 Claude Code 為例示範 Agent Skills 的使用方式，並與 MCP 做詳細比較。核心論點：MCP 是「工具連線」，Skills 是「知識封裝」——兩者解決不同問題，Token 成本差距顯著。

## 補充資料：兩者的成本結構差異（2026-07-31 併入）

來源：dev.to/googleai〈Skills vs MCP: How AI Tools Have Evolved〉經 codelove.tw 翻譯（與 @greggyb 的訪談影片摘要）。
補上原條目未涵蓋的**成本面與載入機制**：

| | MCP | Skills |
|---|---|---|
| **本質** | 透過自然語言讓模型以程式化方式存取 API 與檔案系統 | **就是一個包含可重用提示詞的 Markdown 檔案** |
| **載入成本** | 🔴 **即使你根本沒用到那些工具，也得載入數百行工具定義**——造成上下文膨脹 | **漸進式揭露**：代理可以「知道有」20 個 skills，卻不需要一次載入所有完整指令 |
| **誰能寫** | 需要開發能力 | **非開發者也能撰寫、反覆調整並分享**，完全不需寫程式 |

**兩者不是競爭關係，而且可以疊加**——**Skills 甚至可以教你的代理「如何使用 MCP 工具」**（用 Skill 包裝 MCP 工具的使用時機與流程）。

> 訪談中的一句話：**「現在最熱門的程式語言就是英文。」**

## 相關連結

- [[mcp-stateless-architecture]] — 🔴 **推翻「MCP 需持續連線」**：新版協定每請求自帶內容、Server 不保存 Session
- [[claude-code-cost-optimization-7-tips]] / [[claude-code-context-management]] — 🔴 **推翻「工具定義全量載入」**：預設 Tool Search 延遲載入，只有工具名進上下文
- [[claude-skills-guide]] — Skills 兩種類型的完整指南
- [[claude-code-context-management]] — Context 管控策略
- [[claude-5-context-engineering-rules]] — 官方漸進式揭露原則（本篇「知道有但不全載入」的機制依據）
- [[api-cli-skills-agent-architecture]] — 「Skill 不取代 CLI 而是包裝 CLI」與本篇「Skills 可教代理用 MCP 工具」是同一模式

## 補充資料：Progressive Disclosure 的四層與「Context Hoarding」（2026-08-07 併入）

來源：iThome《30 天從零拆解 AI Agent》【AI Agent 08】。本頁原本從 token 效率角度比較 Skills 與 MCP，此處補上**按需載入的四層機制**與其失效點。

### 問題命名：Context Hoarding

當 Agent 要會寫 SQL、部署服務、分析 Log、操作 Git、產生報表、回覆 Email、處理 Kubernetes、執行 Security Review⋯⋯全部塞進 System Prompt 的後果：

- System Prompt 越來越長／Token 成本持續增加
- **重要規則被大量內容稀釋**
- **不相關指令互相干擾**
- Prompt 更新變得困難
- **模型每一輪都重新讀取根本用不到的內容**

> 解法原則：**不要預先載入所有能力細節，而是讓 Agent 先知道「有哪些能力」，需要時再載入對應內容。**
> Progressive Disclosure ＝ **先暴露最少資訊，只有在需要時才逐步展開更多細節。**

### Skill 不是一段 Prompt——一份完整 Skill 的九個欄位

名稱／描述／**什麼情況應該使用**／執行步驟／需要的工具／限制／範例／**驗證方式**／相關資源

（對照 [[skill-md-writing-techniques]]：description 只寫「什麼時候用」正是第一層 Discovery 的關鍵）

### 四層機制

| 層 | 作用 | 關鍵設計 |
|---|---|---|
| **1. Discovery** | 讓模型知道**有哪些能力存在** | 只暴露名稱＋描述，不含正文 |
| **2. Selection** | 決定用哪個 | 三種策略，見下 |
| **3. Loading** | 實際載入內容 | 🔑 **Loading 應該是可追蹤的事件** |
| **4. Execution** | 執行 | 高風險步驟要**真的擋得住**（見下） |

**第二層的三種選擇策略**：
| 策略 | 特性 |
|---|---|
| **模型自己選** | 彈性高、能處理模糊任務、不需太多固定規則；但**可能選錯／選太多／忽略真正需要的 Skill** |
| **Rule-based Routing** | 可預測但僵化 |
| **Hybrid** | 兩者並用 |

**🔑 第三層 Loading 必須可追蹤——要能回答七件事**：
哪個 Skill 被載入／為什麼被選中／在哪一輪載入／載入多少內容／是否包含額外檔案／**是否影響 Permission**／**Skill 什麼時候失效**

> 沒有這些紀錄，就無法判斷一次失敗是**模型本身、System Prompt、Skill Selection、Skill 內容，還是 Tool Runtime** 的問題——這五個層次的責任必須能分開。

**第四層 Execution 的關鍵**：Skill 寫「在部署前要取得批准」是不夠的，必須做到**沒有批准就真的不能部署**。
→ 這正是 [[agent-control-layer-permission-hooks-planning]] 的「Prompt 不是安全邊界」在 Skill 層的對應：**Skill 內文只是 guide，強制力要放在 Tool Runtime 的 Permission Gate。**

## 同系列後續（Day 09–10）

- [[agent-context-management]] — **Day 09**：把本篇的 Progressive Disclosure／Eviction 思想從 Skill 擴展到整個 Working Context——Selection / Compaction / Eviction / Rehydration 四層、Context Tier（Pinned/Active/Compressed/External）、Protected Fields、Soft Budget、Stub。本篇命名的 **Context Hoarding**，在該篇成為「只 Load 不 Evict」的第 4 號錯誤設計
- [[agent-memory-lifecycle]] — **Day 10**：跨 Session 的 Memory 四層生命週期。其中明確劃出 **Memory ≠ Skill** 的界線——Skill 是「遇到某類任務該怎麼做」（可重用 Procedure），Memory 是「過去發生過什麼」（從 Experience 保留的 State）
