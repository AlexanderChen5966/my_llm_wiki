---
title: Claude Cowork「Record a skill」：3 步驟把 SOP 錄成可重用 AI 技能
date: 2026-07-24
tags: [ai/claude, type/how-to]
source: https://www.bnext.com.tw/article/91585/claude-cowork-record-a-skill
type: reference
status: processed
asset_value: 12
---

Anthropic 在 Claude Cowork 推出「Record a skill」：使用者一邊操作電腦、一邊口述示範一次，Claude 就自動整理成結構化工作流程並存成可重複呼叫的 **Skill**。概念類似 OpenAI Codex 的 Record & Replay，用螢幕示範取代冗長文字指令，降低非技術用戶的自動化門檻。

## T（核心論點）

過去教 AI 做重複任務要寫冗長文字指令甚至腳本；「Record a skill」讓使用者「照平常工作方式做一次給 AI 看」，把獨特流程封裝成可重用技能——讓 Claude Cowork 更接近「AI 員工」定位：直接從人類實際操作中學習工作方法。

## B（適用場景）

**適用：** 例行工作（從特定網站下載報表、整理檔案、資料輸入、填報表單）需重複執行；已倚賴 Claude Cowork 管理檔案/瀏覽器/雲端的團隊。
**限制：** 僅 **Pro / Max / Team 付費用戶**（Claude Cowork 本身為付費功能），在桌面版「+」選單可見。

> ⚠️ **2026-09-16 已改版**：Cowork 獨立分頁已併入一般對話 Chat，下方「桌面版『+』選單」入口所在的
> Cowork 對話視窗已不存在，入口位置可能已變，實際操作前需重新確認，詳見 [[claude-cowork-merged-into-chat-2026-09]]。

## R（3 步驟）

1. **啟動錄製**：Claude Cowork 桌面版對話視窗左下角「＋」→ 選「Record a skill」。
2. **錄製操作與講解**：按 Start recording，Claude 錄螢幕畫面、滑鼠點擊、鍵盤輸入，同時支援口述每一步。
3. **產生並儲存**：結束後自動整理成新 Skill 存入技能庫，未來點選即可重跑同樣流程。

背後掛勾整套 Claude Cowork 架構：本機檔案存取、長期記憶、connectors、plugins、Cowork Projects。

## C（關鍵行動）

- [ ] 若有 Pro/Max/Team，挑一個真正重複的例行任務錄成 Skill 試用
- [ ] 對照 Codex Record & Replay 的差異（見相關連結）

## 補充資料：微軟 Skill Recorder——三家都有了（2026-08-05 併入）

來源：kocpc.com.tw 介紹 `github.com/microsoft/skill-recorder`。微軟開源版**與本功能、Codex「錄製 Skill」是同一概念的第三個實作**——這個功能已成三大陣營標配，故併入對照而不另建條目。

### 三家對照

| | **Claude Cowork「Record a skill」**（本條目）| **微軟 Skill Recorder** | **Codex Record & Replay** |
|---|---|---|---|
| 形式 | Cowork 桌面版內建功能 | **獨立 Electron 桌面 App**（開源）| Codex 內建 |
| 取得 | Pro/Max/Team 付費 | **免費開源**（需串 GitHub 帳號）| 依 Codex 方案 |
| 語音說明 | 支援口述 | **本機 Whisper** 模型轉錄 | — |

### Skill Recorder 的獨有細節

**三階段流程**：Record → Analyze → Create（完成後可選擇建立 Skill 或 Automation）

**錄製期間收集**：目前使用的 App、視窗與文件標題、視窗切換、瀏覽器網址、螢幕畫面、剪貼簿複製文字的短預覽。

🔑 **Narrate 功能**：一邊操作一邊用語音說明「**為什麼現在要做這件事**」——幫 AI 理解光看畫面判斷不出的操作意圖。語音**不上雲**，透過本機 Whisper 處理，支援 99 種語言（含中文），首次使用需同意下載約 **252MB** 模型。

**安裝**（無安裝檔，需終端機指令，Release 頁面可查）：
```bash
# macOS
commit="32fd0b57e02c3ea1e016cca0d64e59052e93a9b9"; curl -fsSL "https://raw.githubusercontent.com/microsoft/skill-recorder/$commit/install.sh" | SKILL_RECORDER_COMMIT="$commit" bash
```
Mac 裝完會在 `~/Applications` 建立「Skill Recorder（Source）」；想關終端機後仍執行可加 `SKILL_RECORDER_DETACHED=1`。

### ⚠️ 最重要的限制

> **用這工具產出的 Skill 會依照微軟 Scout 或 Cowork 的能力來生成內容，其他 AI 代理不一定能直接使用，通常還需要修改。**

換句話說「錄一次到處用」目前做不到——**錄製產物綁定生成它的那個平台的能力模型**。這是三家共同的隱性限制，選工具時應以「主力 agent 是哪一家」為準，而非功能表。

## 相關連結

- [[openai-codex-record-replay]] — OpenAI Codex 的示範錄製功能（同概念，可對照）
- [[claude-cowork-setup-guide]] — Claude Cowork 設定指南
- [[sop-to-skill-4-steps]] — 把 SOP 轉成 Skill 的方法論（本功能的自動化版）
- [[claude-skills-beginner-guide]] — Claude Skills 新手入門
