---
title: 【現在才來問也不晚的 Codex】2026 年 7 月更新版：ChatGPT 桌面版首先該看的功能與使用順序
date: 2026-07-16
tags: [ai/agent]
source: https://codelove.tw/@tony/post/apo5B3
type: article
status: processed
asset_value: 60
---

2026-07-09 更新後，ChatGPT 桌面版整合一般聊天、Work、Codex 三種模式。本篇整理 Codex 桌面版該從哪裡開始上手的實務優先順序，並點出與 Claude Code 容易混淆之處。

## T（核心論點）

技能/外掛程式長期來看非常重要，但不該是第一天就設定的功能——建議的接觸順序是「專案/工作階段 → 資料夾/瀏覽器 → 小型請求 → 差異/稽核 → 側邊面板 → 技能/外掛程式 → 模型/推論調整 → 終端機/排程」。真正讓 Codex 桌面版比 CLI 更好用的關鍵，是能像 IDE 一樣邊看檔案邊下指令，尤其瀏覽器的「註解」功能可直接指定修改位置，不必再用文字描述「右上角那個按鈕」。

## B（適用場景）

**適用：** 已在用 Cursor 或 Claude Code、評估是否/如何導入 Codex 桌面版；需要快速掌握新版 ChatGPT 桌面 App 上手順序。
**不適用 / 注意：** ChatGPT 桌面版沒有 `/clear`（那是 CLI 才有），而是用 `/compact`、`/fork` 與建立新任務來區分工作階段，這是與 Claude Code 最容易混淆之處。

## R（關鍵操作與位置）

**模型對照**：Sol（深度思考主力，複雜實作/調查/設計/最終稽核）、Terra（日常工作平衡型）、Luna（快速便宜，擷取/分類/轉換/大量制式處理）。不確定時從 Sol 開始，日常用 Terra，大量明確處理用 Luna。

> ⚠️ 世代註記（2026-09-29 附註）：上列 Sol／Terra／Luna 為 **GPT-5.6** 分級（Sol 為旗艦）；GPT-6 為 Astra／Sol／Luna、Sol 由旗艦降一格、公告無 Terra，見 [[gpt-6-astra-prompting-guide]]「GPT-6 家族三層定位與價格」段。

**任務管理**：任務依成果單位切分（不是因為對話變長就切新任務）；長任務在節點用 `/compact` 只留重要前提；想花數小時推進同一目標用 `/goal` 明確標示完成條件；想試新方案又保留原脈絡用 `/fork`。

**檔案位置**：`.agents/skills/` 放技能（個人用 `~/.agents/skills/`，team 共用 `<repo>/.agents/skills/`）；`~/.codex/` 放 Codex 本身設定/歷史/工作階段/驗證資訊（含 `sessions`/`archived_sessions`/`history.jsonl`/`memories`），這類檔案可能含機密資訊，不要放進 Git 或截圖分享。

**技能停用而非刪除**：在 `~/.codex/config.toml` 設定 `[[skills.config]] path=... enabled=false` 可保留技能但排除出初始候選清單（技能量大時 Codex 會依上下文量縮短提供的候選清單）。

**Claude Code 技能遷移**：官方支援 `Settings > Import` 直接匯入 instructions/設定/技能/外掛/MCP/hooks/子代理，優於手動整包搬移 `.claude/skills`（因路徑與權限設定可能殘留 Claude Code 特定前提，建議逐一放進 `.agents/skills` 測試）。

**進階功能**：Goal mode（帶完成條件推進長任務）、Record & Replay（把畫面操作轉成可重複使用技能，2026-06 macOS 功能）、worktree（Git 獨立工作區支援平行作業）、Computer Use（僅在真的需要操作 GUI 時使用，優先用瀏覽器或 API/MCP）。

## C（關鍵行動）

- [ ] 第一天只做一件事：建立專案、選資料夾、請它做一個小修改、用差異/瀏覽器確認，不要急著設定所有功能
- [ ] 技能與外掛程式數量增加後，定期用 `enabled=false` 停用不常用項目而非放著不管
- [ ] 從 Claude Code 遷移技能優先用官方 Import 功能，而非整包手動搬移 `.claude/skills`

## 相關連結

- [[chatgpt-chat-work-codex-comparison]] — Chat/Work/Codex 三模式定位比較，可作為選擇進入 Codex 桌面版前的判斷依據
- [[codex-plugins-12-guide]] — Codex 必裝 12 個外掛完整清單，補充本篇「技能/外掛程式」章節的具體選項
- [[codex-onboarding-seven-layer-config]] — Codex 七層設定地圖心智模型（哪個設定該寫在對話/AGENTS.md/config.toml/Skills/Plugins/MCP/Memory 哪一層），與本篇桌面 UI 操作動線互補
