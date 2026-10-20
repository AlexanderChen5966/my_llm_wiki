---
title: Claude Code / Codex 沙盒機制解析 + h5i 高速沙盒工具
date: 2026-07-09
tags: [ai/claude, dev/architecture, type/reference]
source: https://codelove.tw/@tony/post/3kV7zx（原文：https://qiita.com/Koukyosyumei/items/3bd4696237ea64c38971）
type: article
status: processed
asset_value: 58
---

Claude Code 與 Codex 內建的 OS 層級沙盒運作方式解析，以及更高性能、面向 agent 的沙盒工具 h5i——透過系統呼叫監控 + namespace 隔離出獨立 Git worktree，0.2-3 秒即可進入沙盒 shell（比 Docker 容器輕量），並記錄環境內所有指令與結果作為「可稽核的工作區」。

## T（核心論點）

Agent 能自動完成工作，但也可能誤刪檔案、外洩機密資訊、跑出高耗資源的低效程式——這些風險來自提示注入攻擊、幻覺或其他誤作動。Claude Code/Codex 的沙盒設計專注在「判斷個別指令是否可執行」（OS 層級限制套用在 Bash Tool 上、其他工具靠 permission model 詢問使用者），而 h5i 的差異化定位是「分離整個工作環境本身」——即使 agent 誤作動，也不會對原始環境造成破壞性變更。

## B（適用場景）

需要讓 Claude Code/Codex 執行高風險或不熟悉的 shell 指令；想要比 Docker 容器更輕量的隔離環境（秒級啟動）；需要「可稽核的工作區」記錄 agent 所有操作以便事後驗證；擔心提示注入攻擊導致 agent 誤刪或外洩資料的場景。

## R（重點整理）

**Claude Code 沙盒**：把 OS 層級沙盒功能（Bubblewrap、socat、seccomp）套用到 Bash Tool；Read/Write/WebFetch 等其他工具靠 permission model 適時詢問使用者。設定寫在 `settings.json` 的 `sandbox` block，可控制網路允許網域（`allowedDomains`）、憑證處理方式（`mask` 模式注入到指定 host）；另有 `permissions` block 可對各工具靜態定義 `allow`/`deny` 規則（如允許 `npm run *`、`git commit *`，拒絕 `git push *`）。

**Codex 沙盒**：結合 sandbox policy（決定「允許什麼」）與 approval policy（決定「要向誰詢問」），皆用 OS 層級沙盒執行所有指令。
- Sandbox policy 三模式：`read-only`（可讀不可寫不可連網）／`workspace-write`（可寫當前目錄與 /tmp）／`danger-full-access`（更大範圍存取）
- Approval policy 三選項：`on-request`（預設，模型判斷需要時才詢問）／`never`（一律拒絕需要提權的請求）／`granular`（可對特定請求做更細緻設定）

**h5i 的差異化設計**：`h5i env` 指令建立一個 OS 層級沙盒中隔離的專用 Git worktree，除了系統呼叫層級限制與資料夾存取限制外，還能設定記憶體/程序數量等資源限制、以網域為單位的網路存取控制、憑證跨環境分離。**預設 supervised 模式不用 Docker 這類重型容器**，靠系統呼叫監控+namespace 設定實現輕量沙盒，進入沙盒 shell 僅需約 0.2-3 秒。

**h5i 基本用法**：
```
curl -fsSL https://raw.githubusercontent.com/h5i-dev/h5i/main/install.sh | sh
h5i init
h5i hook setup --write --wrap-bash

h5i env create claude-env --profile agent-claude
h5i env shell claude-env
box$ claude --dangerously-skip-permissions
box$ exit

h5i env diff claude-env      # 檢查環境內的變更
h5i env propose claude-env   # 提出環境內的變更
h5i env apply claude-env     # 將提議的變更套用回原始分支
```

**「可稽核的工作區」**：h5i 標語強調環境內執行過的指令、結果、使用者提供的提示都會完整記錄，事後可驗證，這對追溯 agent 行為（尤其懷疑提示注入時）很有價值。

## C（關鍵行動）

- [ ] Claude Code 使用者優先確認 `settings.json` 的 `sandbox` 與 `permissions` block 已依需求設定網域白名單與危險指令黑名單
- [ ] Codex 使用者依任務風險選對 sandbox policy，預設 `danger-full-access` 應避免用於不信任的任務
- [ ] 需要「秒級啟動+可稽核」的隔離環境時，評估 h5i 取代 Docker，尤其是高頻率的短期沙盒需求
- [ ] 懷疑遭遇提示注入攻擊時，h5i 的完整操作記錄可作為事後稽核依據

## 相關連結

- [[claude-code-security-guidance-plugin]] — 官方安全稽核外掛 security-guidance，與沙盒是互補的兩層防護（一個攔截危險程式碼生成，一個限制執行環境）
- [[production-agent-architecture]] — Guardrails/HITL 一般性工程設計，沙盒是其在執行環境層的具體實作
- [[claude-squad]] — 同樣用 tmux + git worktree 隔離的多 Agent 管理工具，架構思路可對照
