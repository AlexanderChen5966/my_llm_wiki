---
title: herdr：懂 agent 狀態的終端多工器（不是另一個 tmux）
date: 2026-08-14
tags: [ai/agent, ai/workflow]
source: https://www.shareuhack.com/zh-TW/posts/herdr-terminal-agent（實戰指南，數據查核最完整，本條目以此為準）
source_2: https://slash-invest.com/herdr-terminal-ai-agent-multiplexer（授權與支援清單）
source_3: https://vocus.cc/article/6a7b245ffd89780001ff7131（概念介紹）
type: article
status: processed
asset_value: 62
---

三來源合併。⚠️ **數據有出入已標注**：star 數 slash-invest 記「約 9,900」（截至 2026-07）、shareuhack 記「12.2k stars、728 forks」並註明 2026-07-06 直接 fetch 確認——兩者時間點不同，應為成長而非矛盾，本條目採後者（查核方法明確）。

## T（核心論點）

> **不是「我要不要從 tmux 換到 herdr」，而是「我需不需要即時知道每個 agent 的執行狀態」——如果答案是需要，tmux 無論多熟都解決不了這個問題。**

功能定義：`herdr = PTY multiplexer + agent state awareness + persistent session + Socket API`。前兩項 tmux 有，**後兩項是 tmux 對「Claude Code 現在是在等你 approve 還是還在思考」一無所知的部分**。

## B（適用場景）

**適用：**
- 同時跑多個 agent（例如三個 Claude Code worktree 分別做 collect／synthesize／write），每隔幾分鐘就要手動切 tab 確認誰在等 approve
- 常在 SSH／遠端機器工作、需要 session 持久化
- 想讓 agent 之間互相編排（Socket API）

**不適用 / 注意：**
- 一次只用一個 agent 的人（沒必要）、完全不碰終端機的新手
- 🔴 **授權要看清楚：AGPL-3.0 雙授權**——個人／開源免費，**組織／商業使用另有商業授權**。AGPL 對「整合進你對外提供的服務」有較嚴格的開源要求，公司採用前先確認條款
- ⚠️ **v0.7.1，pre-1.0**；Windows 為 preview／beta，`--remote` 支援有限
- 🔴 **在 herdr pane 裡再開 tmux session 會導致 agent 狀態偵測失效**——不要巢狀

## R（核心能力與安裝）

單一 binary、約 10MB、無外部依賴（Rust 約占 86.0%，2026-07-20 查核）。2026-06 底在 GitHub Trending 爆紅（Rust 類目第一、整體日榜第 17），HN 首頁。

| 功能 | herdr | tmux |
|---|---|---|
| Pane 分割／Tab 管理 | ✅ | ✅ |
| Session persistence（detach/reattach）| ✅ | ✅ |
| SSH remote reattach | ✅ | ✅ |
| **Agent state tracking（blocked/working/done/idle）** | ✅ | ❌ |
| **Agent Socket API** | ✅ | ❌ |
| AI agent 原生設計 | ✅ | ❌ |

安裝三選一：`brew install herdr`／`curl -fsSL https://herdr.dev/install.sh | sh`／`cargo install herdr`（另支援 mise 與 Nix）。

操作沿用 tmux 肌肉記憶：`Ctrl+B` 為前置鍵，`V` 垂直分割、`C` 新 tab、`Q` detach。🔑 **對 tmux 老手是升級路徑，新手可直接從 herdr 開始**（中文社群 moewah 的定位是「互補而非取代」，因為 herdr 本身就含 tmux 的多工功能）。

啟用整合：`herdr integration install <agent>`（官方整合 13+ 種：Claude Code、Codex、Cursor Agent、Antigravity CLI、Hermes Agent、GitHub Copilot CLI、Grok CLI、Devin CLI、Kimi Code CLI 等）。

## C（關鍵行動）

- [ ] 商業／組織使用前先確認 AGPL-3.0 的義務，必要時走商業授權
- [ ] 不要在 herdr pane 內再開 tmux（狀態偵測會失效）
- [ ] 若你主力是 Claude Code，先理解它只有 screen manifest 等級的狀態偵測（見下）

## 補充資料

### 🔴 兩套狀態偵測機制，準確度差很多——而 Claude Code 屬於較差的那組

**機制一：Lifecycle Hooks（精確）** — agent 主動向本地 socket 回報狀態，需 `herdr integration install <agent>`，狀態變化立即通知。

**機制二：Screen Manifest TOML rules（自動但較不精確）** — 定期讀 terminal 底部 buffer 截圖，用 TOML 規則比對 UI pattern 推斷狀態。zero-config 但準確度低。⚠️ **blocked 偵測故意設計偏嚴格**，避免把正在執行的 agent 誤判為 blocked。

| 支援層級 | 代表 Agent |
|---|---|
| Full lifecycle hooks（精確）| Pi、OMP、Kimi Code CLI、OpenCode、Hermes Agent、Kilo Code CLI |
| **Screen manifest only（較不精確）** | **Claude Code**、GitHub Copilot CLI、Codex、Devin CLI、Cursor Agent CLI、Droid、Grok CLI、Amp |
| 部分／實驗性 | Gemini CLI、Cline |

🔑 **Claude Code 目前是 screen manifest only**：`herdr integration install claude` 安裝的 hook **只負責回報 session 身份**（讓 herdr 知道哪個 pane 是哪個 session），blocked/working 仍靠視覺解析。原文的實測評語：**idle 與 working 的判斷基本可靠，但 blocked 有時要等幾秒才更新**——日常監控夠用，但別預期毫秒級事件驅動。

### Socket API：從 viewer 升級為 orchestrator

本地 Unix socket 讓 agent 可以主動呼叫 herdr：建立 pane、**spawn 子 agent**、讀取其他 agent 的 output、subscribe 狀態變化事件。這是它與單純「終端多工器」的分水嶺，也讓它可以被腳本與外掛整合。

### 它真正解決的痛點：切 tab 的隱藏成本

原文對成本的拆解很準：每次切換都要**讀 terminal 輸出來判斷狀態**，再切回原本的工作；如果 agent 剛好剛進入等待狀態，切過去就得馬上互動；如果還在跑，這次切換就是純浪費。加上關筆電或斷線可能讓 session 消失。

🔑 **多 agent 並行已是日常，但管理工具還停留在 AI agent 出現之前的設計。**

（vocus 那篇用「臨時攤位 vs 24 小時營運總部」比喻同一件事，並強調保留的是**真實終端工作階段而非畫面截圖**，重連看到的是運行中的即時畫面與佈局。該篇為概念介紹、無數據，本條目未採其任何數值。）

## 相關連結
- [[warp-agentic-dev-environment]] — 另一條路線：Warp 把 agentic 能力做進終端環境本身，而非在既有終端上多工
- [[loopx-stateful-control-plane]] — ⚠️ **層次不同但常被混談**：herdr 管的是**終端與 session 的可視性**（誰在跑、誰在等），LoopX 管的是**跨執行階段的持久狀態**（Objective／Evidence／Gate）；一個解決「我看不到」，一個解決「它忘了做到哪」
- [[claude-code-cross-session-messaging]] — Claude Code 官方的 session 間通訊
- [[parallel-code-multi-agent-gui]] — 多 agent 平行作業的 GUI 路線
- [[multi-agent-cli-bridge]] — 跨 CLI 的多 agent 橋接
- [[agent-os-workstation-division]] — 工作站層級的 agent 分工
- [[claude-code-session-handoff]] — session 交接的三個共同限制
- [[fox-ai-roundtable-multi-cli-tool]] — 多 CLI 協作工具
