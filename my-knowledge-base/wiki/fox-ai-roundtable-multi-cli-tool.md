---
title: fox-ai-roundtable：一問三答的本機多 CLI 圓桌工具
date: 2026-07-08
tags: [ai/agent, ai/workflow]
source: https://github.com/PeterPanSwift/fox-ai-roundtable
type: reference
status: processed
asset_value: 55
---

fox-ai-roundtable 是零相依的 Node.js 本機網頁工具，把同一個 prompt **同時**送給 Claude Code、Codex（GPT）、Antigravity（Gemini）三個本機 CLI，並排顯示三家回答，全程透過**已登入的訂閱方案**執行、不需任何 API key。

## T（核心論點）

多模型比較的價值在於**用同一個 prompt 換取三種視角**，且完全靠本機 CLI 的既有訂閱額度完成——不必額外開 API 帳單，也不必在多個網頁分頁間切換貼上同一個問題。伺服器只是薄薄一層並行呼叫三支 CLI 的橋接，本身不含任何模型邏輯。

## B（適用場景）

- 想快速比較 Claude / GPT / Gemini 對同一問題的回答差異（模型選型、答案交叉驗證）
- 已有三家 CLI 訂閱、不想額外付 API 費用
- 需要在同一場對話中持續追問並讓三家都記得上下文

**限制**：三個 CLI 都要先安裝並登入；僅支援文字+圖片輸入，不支援檔案上傳/複雜工具呼叫；單機本地執行，非團隊協作工具。

## R（運作方式）

**架構**：零相依 Node HTTP server（`server.js`），收到 prompt 後平行執行三支 CLI：
- `claude -p --output-format json` → 後續用 `--resume <session_id>` 接續對話
- `codex exec --json` → 後續用 `codex exec resume <thread_id>` 接續
- `agy -p`（Antigravity）→ 首輪從 log 取 conversation ID，後續用 `--conversation <id>` 接續

**功能**：三家同時作答並標示各自耗時；支援接續對話（各自記得上下文）；可勾選只問其中幾家；每家可換模型（Claude: Sonnet/Opus/Haiku；Codex: 調 reasoning effort；Antigravity: Gemini 3.5 Flash/3.1 Pro，甚至 Claude 和 GPT-OSS）；支援附加圖片（Codex 走原生 `-i`，Claude/agy 把圖片路徑寫進 prompt，因兩者都是能讀檔的 agent）；Markdown 渲染 + 深色模式；介面中英雙語自動切換。

**安全設計**：伺服器只綁 `127.0.0.1`（區網連不進來）；CLI 在專用空資料夾 `~/.fox-ai-roundtable` 執行，限縮可讀取檔案範圍；prompt 以參數陣列傳給 `spawn`、不經 shell（無指令注入風險）；回答經 DOMPurify 消毒後才渲染。

## C（關鍵行動）

- [ ] 需要交叉驗證答案品質、或不確定哪家模型更適合某類任務時，可用此工具快速比較
- [ ] 三支 CLI 平行呼叫 + resume session 的實作模式，可參考用於自建多 agent 比較工具
- [ ] 安全設計（127.0.0.1 綁定 + spawn 陣列傳參 + 專用沙箱資料夾）是本機工具防注入的簡潔範例
- [ ] 若要加統籌者，務必做成按鈕觸發的選配，不要改成預設行為——原始賣點是「你自己比較三份原始答案」

## 擴充規劃（A/B 已於 2026-07-09 實作；CONTESTED 暫緩）

### 選配統籌者（借 ai-pair 匯總角色，但做成零額外付費）✅ 已實作

現況是三家平權、無匯總，這是刻意的設計（使用者自己比較，AI 不預先篩選）。若要加統籌功能，設計上必須守住這個核心賣點，同時不重蹈「每次提問都多付一次帳單」的成本問題：

- **按鈕觸發，非自動**：使用者主動按「統合」才觸發，不是每次提問自動附加一次額外呼叫
- **複用已在跑的模型，不開第四支**：統籌步驟直接用三家其中一家（例如 Claude）順手處理，不額外呼叫新模型或第四支 CLI
- **只做摘要，不做篩選**：產出限定為「共識點／分歧點」摘要，不代替使用者判斷孰優孰劣
- **獨立視圖**：統籌摘要顯示在與三份原始答案分開的視圖，使用者可隨時切回並排原始畫面

### CONTESTED 仲裁整合（進階選配，依賴統籌者先落地）⏸️ 暫緩——待出現「只看摘要不讀原文」行為再評估

借用 [[ai-pair-mesh-topology-design]] 的 CONTESTED 概念，但反向應用——ai-pair 的 reviewer 有結構化判定（BLOCK/PASS/WARN）才談得上仲裁，roundtable 三家是平權比較，本來沒有可仲裁的結構。要接上這個概念，需先讓統籌者額外判定「三家是否構成實質衝突」（而非只是措辭不同），若是則標記「⚖️ 三家意見分歧，需要你判斷」。**仲裁者永遠是使用者本人**——不像 ai-pair 討論過的「Fable 先給建議裁決」，roundtable 的定位是輕量比較工具，不該為了仲裁再引入一個額外付費模型。

### CLI 擴充選項 ✅ 已實作（Ollama / Copilot / OpenCode）

架構本身已支援擴充：固定介面是「spawn CLI → 解析 JSON output → 記 session/thread id 供 resume」，`agy`（Antigravity）已證明此模式可複製。候選：本機 Ollama CLI（免費、可測本地模型）、未來若有 Grok 本機 CLI。新增 CLI 應維持零相依原則，不引入新的執行環境或 API key 依賴。

> 完整規劃脈絡見 [[multi-model-orchestration-expansion-plan-2026-07-08]]。

## 相關連結

- [[multi-agent-cli-bridge]] — CCB：tmux 版多 Agent CLI 工作區（同類但走角色分工而非同問比較）
- [[dual-tool-strategy]] — 雙工具策略：Gemini CLI 探索 + Claude Code 執行
- [[gemini-cli-subagent]] — Gemini CLI 接入 Claude Code
- [[ai-pair-skill]] — 統籌者與 CONTESTED 仲裁概念的借鑑來源
- [[ai-pair-mesh-topology-design]] — CONTESTED 仲裁機制原始設計
- [[multi-model-orchestration-expansion-plan-2026-07-08]] — 本次擴充規劃的完整脈絡
