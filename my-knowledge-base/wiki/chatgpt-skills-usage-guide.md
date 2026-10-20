---
title: ChatGPT Skills 怎麼用？@ 呼叫技能、3 種建立方式、6 組範例
date: 2026-07-16
tags: [ai/agent, ai/prompt-engineering]
source: https://www.bnext.com.tw/article/91495/chatgpt-skills-tutorial
type: article
status: processed
asset_value: 62
---

ChatGPT Skills（Beta）是 OpenAI 版的可重複使用 SOP 機制，以 `SKILL.md` 為入口，與 GPTs（客製化專屬角色）、Projects（脈絡集中管理）功能互補而非互斥。

## T（核心論點）

Skills、自訂 GPTs、Projects 三者定位不同：Skills 是「寫一份 SOP」讓 ChatGPT 依固定步驟/格式執行高重複性任務；GPTs 是「請一位專屬領域專家」的長期角色；Projects 是「開一間戰情室」集中管理對話與檔案脈絡。三者可同時搭配使用。目前 Skills 尚未支援跨產品（ChatGPT/Codex）自動同步，需手動下載後在各產品分別安裝。

> ⚠️ **T 層時效附註（2026-10-08）**：「自訂 GPTs 與 Skills 互補、可同時搭配」（本段與首段）寫於 2026-07-16。依數位時代 2026-10-01 轉述 OpenAI 公告（**原文未附官方連結，待核**）：10/26 起停止建立新的自訂 GPT，既有 GPT 12/11 退役（合格 Enterprise 可申請延到 2027-02-11），建立者可「遷移到外掛程式」，指令轉成 Skill。原句保留作為當時快照。→ [[custom-gpt-gems-to-skills-migration]]

## B（適用場景）

**適用：** 高度重複性的日常工作任務（週報、會議記錄、行銷企劃、月結報告），需要在多次任務間維持穩定輸出格式時。
**不適用 / 注意：** 目前僅 Business/Enterprise/Edu/Teachers/Healthcare 等工作與教育情境方案開放 Beta；免費版與 Go 方案尚未支援。

## R（3 種建立方式）

1. **對話直接下指令**：輸入「幫我建立一個技能…」，ChatGPT 會詢問輸入資料、輸出格式與所需工具，最適合入門
2. **Skills 編輯器**：進入 chatgpt.com/skills，用「+」→「透過編輯器建立」直接編寫 SKILL.md，適合精細調校
3. **上傳現成檔案**：直接上傳現成 SKILL.md（採開放 Agent Skills 標準格式，可跨 Claude/Codex/ChatGPT 通用），系統會做安全掃描（可用/Needs Review/Blocked 三種結果）

**呼叫方式**：手動輸入 `@skill名稱` 明確指定；或讓 ChatGPT 依 Skill 描述自動判斷觸發（描述寫得越具體，命中率越高）。

**官方 6 個推薦範例**：campaign-brief-builder（行銷企劃）、discovery-to-next-steps（業務拜訪摘要）、monthly-close-narrative（財務月結）、ops-playbook-writer（營運 SOP）、weekly-status-multi-format（管理者週報三格式）、exec-decision-brief（高階決策簡報）。

## C（關鍵行動）

- [ ] 先用「對話直接下指令」建立第一個 Skill 熟悉流程，再考慮編輯器精細調校
- [ ] 從外部來源取得的 SKILL.md，上傳後留意安全掃描結果（Needs Review/Blocked 不可輕忽）
- [ ] 企業環境需先向管理員確認 Skills 建立/上傳/分享權限是否已開放

## 補充資料：跨產品匯入與 Skill 搬家（2026-09-29 併入）

來源：數位時代〈換AI不用重教一次！Claude⇄ChatGPT雙向搬家，記憶、對話紀錄、Skills轉移教學一次看〉（2026-09-28，<https://www.bnext.com.tw/article/92350/claude-chatgpt-migration>；參考 Ruben Hassid《How to AI》流程並比對 Anthropic／OpenAI 官方文件）。

> ⚠️ **T 層時效更新（2026-09-29 附註）**：T 層「Skills 尚未支援跨產品（ChatGPT/Codex）自動同步，需手動下載」寫於 2026-07-16，**已部分過時**——依 OpenAI 說明（經媒體整理，截至 2026-09-28），ChatGPT 桌面版已可從其他代理工具匯入 Skills，且部分設定可自動同步；但 Codex → Claude 方向仍需手動處理。T 層原句保留作為當時快照。

- **ChatGPT 桌面版「設定 → 匯入」**（或「一般 → Import other agent setup」）：支援 Claude Code、Claude Cowork、Cursor，可勾選匯入指令、設定、Skills、專案、近期工作；在 ChatGPT 端建立一份設定、**不刪除原工具內容**，部分設定支援自動更新以維持同步
- **Codex CLI `/import`**：可匯入 Claude Code 設定與近期對話，對話最多近 30 天內 50 則；**不含 claude.ai 網頁版聊天**
- **Codex Skills 位置**：個人 `~/.agents/skills`、專案 `.agents/skills`，以 `SKILL.md` 為主要設定檔
- **上傳到 Claude**：Customize → Skills → ＋ → Create skill → Upload a skill；每個 Skill 資料夾個別壓成 ZIP，**ZIP 最外層須為 Skill 資料夾**（不要把主檔直接放在 ZIP 根層）；Claude Skills 開放 Free／Pro／Max／Team／Enterprise，但須先啟用程式碼執行，Enterprise 需管理員在 Organization settings → Skills 開啟
- 主要由指令與參考資料組成的 Skill 通常可沿用；用到 Codex 專屬工具、MCP、`agents/openai.yaml` 或特定執行環境的，搬到 Claude 後需另外調整
- ⚠️ **待核對**：原文主張搬到 Claude 前「把 Codex 的 SKILL.md 改名為 Claude 使用的 skill.md」——**原文如此主張，與 Claude 官方慣例（`SKILL.md`）及本庫實作不符，待核對**，不宜照做

記憶與偏好的搬家方式見 [[agent-memory-lifecycle]]、[[claude-2026-beginner-guide]] 的 2026-09-29 補充段。

## 補充資料：一次呼叫多個 Skill（2026-10-08 併入）

來源：數位時代〈ChatGPT、Claude怎麼一次使用多個Skill？AI工作流協作教學〉（韋惟珊，2026-10-07，<https://www.bnext.com.tw/article/92530/use-multiple-ai-skills-workflows>）。⚠️ 原文以作者自家**付費**工作流示範，全文帶推廣性質，此處不寫範例 Skill 名稱。

- 一句話點名多個 Skill；要指定順序就寫「先用 A，再用 B」（作者建議）
- 驗證有沒有真的呼叫：Claude 展開思考過程看用了哪些 Skill，或從輸出中各 Skill 的特徵產物判斷（作者建議）
- 先用小任務測試，重大決策才讓多個 Skill 接力（作者觀點）
- 安裝路徑：Claude 網頁版「Settings → Skills → Add → Upload skill」（作者實測宣稱）——與上段 2026-09-29「Customize → Skills → ＋ → Create skill → Upload a skill」**選單命名不同，兩說並列、不擇一，以實際介面為準**；ChatGPT 付費版：開新對話選「工作」，上傳 zip 並輸入「幫我安裝到個人雲端技能」（作者操作描述）
- 跨平台佐證：Gemini 官方也支援同一對話組合多個 Skill → [[custom-gpt-gems-to-skills-migration]]；多 Skill 接力的概念與 [[claude-skills-guide]] 的「軟編排型」相近

## 相關連結

- [[claude-skills-beginner-guide]] — Claude Skills 新手入門，可對照 ChatGPT 與 Claude 兩套 Skills 生態的異同
- [[claude-skills-guide]] — Claude Skills 完整指南（能力包型/軟編排型），與本篇 OpenAI 版 Skills 互為跨平台比較
- [[agent-memory-lifecycle]] — 跨平台搬家時記憶檔的整理方式（五章節＋四規則，2026-09-29 新增）
- [[custom-gpt-gems-to-skills-migration]] — 自訂 GPT／Gems 退場的跨廠商遷移對照（2026-10-08 新增）
