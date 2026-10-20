---
title: AI Native SDLC：程式碼不再是瓶頸，每階段交付一個版本控制的產物
date: 2026-09-01
tags: [ai/claude, ai/workflow, dev/architecture]
source: https://juejin.cn/post/7678278576718299179（中譯轉述）／原文 https://claude.com/blog/the-ai-native-sdlc-playbook（Louis Claxton，Anthropic 應用 AI 團隊，2026-08-21）
type: article
status: processed
asset_value: 82
---

Anthropic 應用 AI 團隊的官方 SDLC 指南《The AI-Native SDLC Playbook》中譯轉述。

⚠️ **素材不完整**：中譯版只詳述了 Plan / Design / Build 三個階段，Test / Deploy / Maintain 三階段僅在總表中出現，內文被截斷。引用後三階段細節前應回原文 `claude.com/blog/the-ai-native-sdlc-playbook` 補齊。

## T（核心論點）

> **程式碼已經不再是瓶頸了。**

傳統 SDLC 六階段的設計初衷，是為了在「編寫與實現程式碼最耗時、最昂貴」的時代最大化效率。而現在建置階段已被壓縮到幾小時，規劃、評審、部署卻還維持原本時長——**整個流程變成沙漏形：中間快、兩頭慢**。

🔑 **真正占時間的不是建置，是它周圍那些以人類速度運行的環節。**

貫穿全指南的一條主線：

> **每個階段結束時寫入一個版本控制的產物，下一個階段讀取它。**

`intent.md` → `spec.md` → `plan.md` → 程式碼 → 評審 → 部署 → 事故記錄寫回 `intent.md`，形成閉環。而 🔑 **提交鏈本身就是審計記錄**：誰提了什麼需求、Agent 產出了什麼、誰批准了它。

## B（適用場景）

**適用：** 說服組織「加速寫程式無法縮短交付」時的官方論據；設計 AI 輔助開發的階段產物與治理門禁；為每階段設定可量測的先導／滯後指標。

**不適用 / 注意：**
- ⚠️ 這是**企業／團隊尺度**的流程設計。單人專案照搬六階段產物鏈會過重——本庫的個人開發流程判準見 `rules/decision-framework.md`。
- ⚠️ 六個實踐**是模組化的**，官方明說企業可依自身需要優先改造不同階段，不必全套上。
- 🔴 指南把治理寄託在 **Hooks 作為審批門禁**，這點與本庫既有結論一致（Prompt／CLAUDE.md 不是安全邊界），但指南本身沒有討論 Hook 被繞過的情形。

## R（操作步驟）

### 傳統 vs AI Native 六階段對照

| 階段 | 傳統 SDLC | AI Native SDLC |
|---|---|---|
| 規劃 | 需求由委員會收集，研討會與層層簽批，手工撰寫 | Claude 從源頭綜合痛點，寫入 `intent.md`（人類可讀、機器可執行）|
| 設計 | 分析師寫規格，設計師再解讀（兩階段、有資訊損耗）| 需求與設計壓縮在與 Agent 的**一次工作會話**中完成；組織標準以 Skills 形式嵌入並版本化 |
| 建置 | 測試與程式碼手寫，文件事後補 | 測試與程式碼由 AI 生成；組織知識維護在版本化、機器可讀的 `CLAUDE.md` 與 Skills |
| 測試 | QA 門禁設在階段邊界 | **持續 Evals 貫穿整個實現過程** |
| 部署 | 人類審查每一行程式，治理依賴評審週期 | 多層 Agent 評審，**人類審查保留給受監管與關鍵程式碼**；治理由 Hooks 作為審批門禁強制執行 |
| 維護 | 人類盯生產環境找 bug | Agent 監控線上部署，任何突破控制帶的異常都被診斷並**寫回循環** |

### 階段一：規劃——用 `intent.md` 捕獲意圖

發起人直接和 Claude 腦力激盪，產出**一份用發起人自己語言寫成的原始規格**。

1. 發起人用自然語言描述問題
2. 反覆討論直到具體化
3. Claude 按組織模板寫成 `intent.md`
4. 發起人糾正理解偏差
5. 提交到共享的版本控制倉庫

`intent.md` 的實際欄位（官方範例）：作者與狀態／**問題**／**預期成果**／受影響的使用者與系統／**約束**／**待確認問題**。

> 範例的約束欄位寫的是「門戶會話中不引入新的 PII。僅使用現有認證」——**約束寫的是邊界，不是願望**。

**治理**：產品負責人審批，決策以 git 歷史中的合併或關閉評審記錄。
**指標**：先導＝從第一次對話到提交 `intent.md` 的時間；滯後＝被接受進入階段二的 `intent.md` 存活率。

### 階段二：設計——需求與設計合併為一次會話

Claude 讀取 `intent.md`，**在組織 Skills 的約束下**產出規格。官方 Prompt 模板的關鍵一句：

> 「明確描述任何關注點，**尤其是策略之間存在矛盾的地方**。」

🔑 這是把「規格衝突」變成明確交付物的一步——由 Agent 標記出來，再由產品負責人與策略負責人共同解決，而不是讓矛盾隱含在規格裡流到下游。

**指標**：先導＝`intent.md` 與 `spec.md` 提交之間的耗時；滯後＝**建置開始後的需求返工次數**。

### 階段三：建置——沒有被接受的計畫，就不開始實現

1. 工程師在 **Plan 模式**下啟動會話（可讀程式庫但不能修改）
2. 提供 `intent.md` 與 `spec.md`
3. Claude 產出計畫：需要修改的檔案、工作順序、驗證測試
4. 🔑 **工程師質詢計畫**——什麼可能出問題？風險最高的步驟？有其他方案嗎？
5. 反覆迭代，**直到一個新人僅憑計畫就能實現**
6. 將已批准的計畫作為 `plan.md` 提交
7. 接受計畫，讓 Claude 開始實現
8. 🔑 **實現過程中偏離計畫時，在同一個 commit 中更新 `plan.md`**

`plan.md` 的實際欄位：需要修改的檔案（逐一列出）／工作順序／**風險**（範例：「理賠核心 API 限速 50 rps，面板需要做快取」）／**驗證**（範例：「test_status.py 覆蓋四種理賠狀態；截圖與批准的設計稿一致」）。

### `CLAUDE.md` 官方範例的段落結構

指南給的範例分四段：**命令**（建置／測試／Lint，並註明「CI 中運行，推送前必須修復」）／**約定**（「金額永遠用 BigDecimal，不用 double」「不引入新的 Lombok」）／**架構**（哪個目錄放什麼、「不要編輯生成的類」）／🔑 **「Claude 容易犯的錯」**（「不要升級依賴版本，這由平台團隊管」「遺留的 v1/ 包已凍結，改動放到 v2/」）。

> 最後那一段是官方範例裡最值得抄的結構——它把「這個專案的地雷」變成 `CLAUDE.md` 的一個正式欄位，而不是散落在各段裡。與 [[apple-claude-md-teardown]] 的結論完全一致。

## C（關鍵行動）

- [ ] 說「AI 讓我們快 3 倍」之前，先確認量的是沙漏的哪一段——**建置變快不等於交付變快**
- [ ] 導入時挑**一個**階段先改造（六實踐是模組化的），優先挑目前最慢的那一段
- [ ] 每個階段產物都進 git，並確認「提交鏈能不能當審計記錄」——答不出誰批准了什麼，這條產物鏈就沒有治理價值
- [ ] `plan.md` 的驗收標準用「新人僅憑計畫能否實現」來測，而不是自己覺得夠清楚
- [ ] 要「一定擋得住」的治理寫成 Hook，不要寫進規格或 `CLAUDE.md`
- [ ] 引用 Test／Deploy／Maintain 三階段細節前，回原文補齊（本素材該段被截斷）

## 相關連結
- [[synthesis/synthesis-ai-dev-workflow-frameworks]] / [[synthesis/synthesis-spec-driven-workflow]] — **本篇為兩份精粹的來源**（2026-09-03 重跑）：唯一的原廠參考實作（產物鏈與提交鏈即審計記錄）
- [[apple-claude-md-teardown]] — 同期處理：Apple 官方 `CLAUDE.md` 的實際拆解，與本篇的 `CLAUDE.md` 範例互為理論與實例
- [[spec-driven-ai-workflow]] / [[spec-first-ai-coding-workflow]] — SDD 的既有整理；本篇是 Anthropic 官方版本的產物鏈
- [[ai-sdd-framework-comparison]] — ZeroSpec / OpenSpec / SpecKit / BMAD 四框架比較，本篇可當「原廠參考實作」對照
- [[planning-with-files]] / [[persistent-markdown-planning-skill]] — 用檔案承載計畫的既有做法
- [[task-list-single-source-of-truth]] — 🔑 **強互補**：本篇「偏離計畫時在同一個 commit 更新 `plan.md`」＝該條目「task-list.md 唯一正本」的同一原則
- [[enterprise-ai-four-layers-prompt-policy-tool-workflow]] — 為什麼治理要落在 Workflow／Hook 而非 Prompt
- [[claude-code-hooks-memory]] — Hooks 作為強制門禁的機制細節
- [[delivery-speed-vs-maintenance-cost]] — ⚠️ **張力對照**：本篇主張把流程改快以配合建置速度；該篇主張真正沒變便宜的是「軟體的一生」，加速交付會把維護成本推給未來
- [[claude-code-three-modes-distinction]] — Plan 模式的機制細節
