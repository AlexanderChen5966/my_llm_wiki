---
title: "Agent Team 實戰（七）：記憶——三層架構、mem0 遷移、QMD 整合"
date: 2026-04-18
tags: [ai/agent, type/concept]
source: https://emilwu.tw/articles/29-agent-team-memory/
type: article
status: processed
asset_value: 60
---

「Agent Team 實戰」系列第七篇（作者 EMil Wu）。承接第五篇的溝通架構，本篇處理跟記憶有關的下一個問題：跨機器切換時 GM「不記得」前一天的決策，因為 session 歷史存在單台機器的 `~/.claude/` 裡。作者由此設計出三層記憶架構，並記錄了 mem0 從本機 Chroma 遷移到 Qdrant Cloud 的三個技術坑，以及用 QMD 為 wiki 加裝搜尋介面、把查詢 token 成本從跟 wiki 大小成正比降到跟相關頁面數成正比的過程。

## T（核心論點）

> Agent Team 的記憶不是一個系統，而是三個用途完全不同的系統疊在一起：**State 記「現在」、Wiki 記「什麼」、mem0 記「為什麼」**。三者的更新頻率、同步方式、擁有者都不同，硬塞進同一個系統只會互相干擾；而記憶系統裡最後的關鍵設計不是「怎麼存」，而是「怎麼搜尋」（QMD）。

## B（適用場景）

**適用：**
- Agent Team 需要跨多台機器、多個 session 共享狀態時，判斷哪種資訊該放哪一層
- Wiki／知識庫隨時間增長後，查詢 token 成本開始隨規模線性上升，需要搜尋層而非遍歷式查詢
- 導入向量資料庫類記憶系統（如 mem0）時，預先識別環境整合的常見坑

**不適用 / 注意：**
- 團隊規模極小、單機單 session 使用時，三層架構可能過度設計，State 或可先用單一檔案代替
- QMD 這類 search-first 機制在面對不存在的 topic 時會「過度努力」匹配出高分但錯誤的結果，需留意假陽性

## R（三層記憶架構與整合方法）

1. **依用途拆分三層**：State（正在做什麼，team-status.md，Session 級更新，Git 同步，GM 擁有）／Wiki（我們知道什麼，結構化知識，Daily 更新，Git 同步，Em 擁有）／mem0（為什麼這樣決定，跨 session 決策記憶，Real-Time 更新，Qdrant Cloud 同步，GM 擁有）。
2. **mem0 遷移本機 Chroma → Qdrant Cloud 三個坑的排查順序**：先確認 Python 隔離環境是否被 hook 正確載入（macOS 新版禁裝第三方套件 + direnv 與 SessionStart hook 路徑不合）；再確認向量維度與資料庫預設值是否一致（384 維語意模型塞進預設 1536 維容器會直接報錯，需刪庫重建）；最後檢查 API key 命名是否與 Claude Code 自身讀取的環境變數衝突（優先用 key 而非訂閱登入，屬於不容易發現的 silent failure）。
3. **QMD 分三階段導入，每階段獨立 commit**：先讓 Em 內部從「讀索引→猜頁面→全部讀取」改為「先搜尋→只讀搜尋結果頁面」；確認 search-first 模式穩定後，才把搜尋能力開放給其他 Agent（於 C7 catalog 建 QMD 工具規格）；最後才更新架構文件，避免在機制未穩定時寫入 source of truth。
4. **順序原則：先讓知識模式穩定，才能定義下一階段的擷取規則**——本例是先完成 QMD 整合、確認 Em 的 search-first 查詢能力穩定，才能定義 mem0 Phase B（擴展到所有 Layer 2 agent）的 extraction rules，因為新記憶存入前需要用搜尋去重複已被結構化記錄的知識。
5. **跨機器同步依各層特性選擇對應機制**：State／Wiki 用 Git（結構化 markdown，diff/merge 友善，手動同步頻率夠低可接受）；mem0 用 Qdrant Cloud（決策記憶需即時可見，不應等待 Git 操作）；環境變數用 direnv（需要進目錄時自動載入）。三個同步機制各自負責一層，沒有重疊也沒有遺漏。

## C（關鍵行動）

- [ ] 設計 Agent 記憶系統前，先問「這個資訊回答的是現在／什麼／為什麼」三個問題中的哪一個，再決定放哪一層
- [ ] 導入向量資料庫類記憶元件前，先列出運行環境清單（Python 版本管理、雲端服務預設值、既有工具會讀取的環境變數命名），逐一排查潛在衝突
- [ ] Wiki／知識庫規模變大前，優先建立 search-first 查詢流程，而非等 token 成本線性增長後才補救
- [ ] 新增記憶擷取規則（extraction rules）前，先確保既有知識庫可被可靠搜尋去重

## 原始摘要

作者在跨機器工作時發現 GM「不記得」前一天在另一台機器上做的決策，因為 session 歷史存在單機 `~/.claude/`，由此設計三層記憶架構：State（team-status.md，Session 級，Git，GM）、Wiki（Em 編譯的結構化知識，Daily 級，Git，Em）、mem0（跨 session 決策記憶，Real-Time，Qdrant Cloud，GM）。mem0 從本機 Chroma 遷移 Qdrant Cloud 踩了三個坑：macOS Python 隔離環境與 direnv/hook 路徑不合、向量維度預設值衝突（384 vs 1536）、Anthropic API key 命名與 Claude Code 環境變數衝突。另導入 QMD 本地搜尋引擎（關鍵字／語意／混合三種模式）作為 wiki 的搜尋介面而非第四層記憶，分三階段整合，把查詢 token 成本從跟 wiki 大小成正比降到跟相關頁面數成正比。核心結論：記憶系統最後的關鍵設計是「搜尋」而非「儲存」。

## 相關連結

- [[agent-team-practice-05-dispatch-modes]] —— 本篇開頭延續第五篇的溝通/dispatch 討論，記憶問題是「Agent 完成協調之後接下來要面對的下一層問題」
- [[agent-team-practice-bonus-os-ipc-five-layer-model]] —— 本篇之後，作者請 Em ingest 前同事的 OS IPC 分析文，衍生出番外篇的五層決策模型
- [[agent-team-practice-04-identity-lifecycle-refactor]] —— 系列前作，處理身份層的 JIT loading 瘦身，與本篇「三層各自獨立、需要時才載入/查詢」的原則呼應
