---
title: "herdr 實戰指南：一個終端機管好所有 AI Coding Agent（2026）"
source: "https://www.shareuhack.com/zh-TW/posts/herdr-terminal-agent-multiplexer-guide-2026"
author:
  - "[[Shareuhack Team]]"
published: 2026-07-07
created: 2026-08-14
description: "herdr 是 Rust 開發的 AI-native 終端機多工器，讓開發者同時管理 Claude Code、Codex CLI、Devin 等多個 AI Agent，並透過 agent state sidebar 即時顯示 blocked/working/done 狀態。本指南涵蓋安裝、Claude Code 整合、狀態偵測機制、與 tmux/Zellij/Warp 比較、Socket API 進階用法，以及 AGPL 授權說明，幫助你判斷是否該現在就裝 herdr。"
tags:
  - "clippings"
status: "processed"
---
同時跑 Claude Code 和 Codex CLI，你是怎麼知道哪個在等你 approve？多數開發者的答案是：一個個 tab 切換，看看有沒有提示訊息。這個做法在只有一個 agent 的時候還行，但同時管三個、四個 agent 時，tab 切換的心智負擔會快速累積。

herdr 是 2026 年 6 月底在 GitHub Trending 爆紅（Rust 類目第一，整體日榜第 17）的開源工具，它的核心想法很直接：把所有 AI Agent 的執行狀態攤在同一個終端機視窗裡，讓你一眼看清楚誰在跑、誰在等你、誰跑完了。這篇指南從安裝到進階用法，完整拆解 herdr 的實際能力與限制。

**TL;DR**

- herdr 是 Rust 寫的 terminal agent 多工器，讓你在一個視窗裡同時管理 Claude Code、Codex、Devin 等多個 AI Agent，並即時看到每個 agent 的 blocked/working/done/idle 狀態
- 核心差異：tmux 是通用終端多工，完全不知道 agent 在等你還是在跑；herdr 是 AI agent 時代的終端機，狀態感知是原生功能
- 安裝一行搞定（ `brew install herdr` ），對 tmux 用戶學習曲線低；目前 v0.7.1 pre-1.0，Windows --remote 支援有限

---

## 多個 AI Agent 同時跑，你現在是怎麼管的？

我們實際跑過同時管 3 個 Claude Code worktree 的場景：一個處理 collect、一個跑 synthesize、一個在做 write。表面上看起來很有效率，但現實是每隔幾分鐘就要手動切換三個 terminal tab，確認誰在等 approve、誰卡住了、誰還在跑。

這個 tab 切換的成本比看起來更高。每次切換都需要讀取 terminal 輸出來判斷狀態，然後再切回原本的工作。如果 agent 恰好剛進入等待狀態，你一切過去就要馬上互動；如果還在跑，這次切換就是浪費。更糟的是，如果你關掉筆電或網路斷線，正在跑的 session 可能就消失了。

多 agent 並行開發已經是很多人日常的一部分，但管理工具還停留在 AI agent 出現之前的設計。tmux 解決了 session persistence 和 pane 分割，但它對「Claude Code 目前在等你 approve 還是還在思考」這件事一無所知。

> **herdr 的認知翻轉** ：不是「我要不要從 tmux 換到 herdr？」而是「我需不需要即時知道每個 agent 的執行狀態？」如果答案是需要，tmux 無論多熟都解決不了這個問題。

如果你想先了解多 agent 並行工作流的整體設計， [Claude Code 平行作業完整指南](https://www.shareuhack.com/posts/claude-code-parallel-workflow-guide-2026) 有更完整的 git worktree 隔離策略。

---

## herdr 是什麼：從 tmux 到 AI-native 終端機

herdr（讀音：herd-er，即「牧群管理者」）是由 Rust 寫成的 terminal agent 多工器，2026 年 6 月底在 GitHub 取得 12.2k stars、728 forks（2026-07-06 直接 fetch 確認），HN 首頁登上 front page（item #48714802）。

功能定義上，herdr = PTY multiplexer + agent state awareness + persistent session + Socket API。比較重要的是每一項都有具體意義：

**PTY multiplexer** ：和 tmux 一樣，可以分割 pane、建立 tab、detach/reattach。單一 binary，約 10MB，無外部依賴；GitHub 語言統計顯示 Rust 約占 86.0%（2026-07-20 查核）。

**Agent state awareness** ：這是 tmux 沒有的核心功能。herdr 的 sidebar 即時顯示每個 agent 的狀態：blocked（等待輸入）、working（正在執行）、done（完成）、idle（閒置）。不用切 tab，一眼確認。

**Persistent session** ：背景 server 模式，筆電合蓋或 SSH 斷線後，session 仍然存活，重新連線後可直接 reattach。支援 SSH remote attach，遠端伺服器上的 agent 也能從本機監控。

**Socket API** ：本地 Unix socket 讓 agent 可以主動呼叫 herdr，建立 pane、spawn 子 agent、讀取其他 agent 的 output、subscribe 狀態變化事件。這個功能讓 herdr 從「viewer」升級為「orchestrator」（後面 H2-6 詳述）。

| 功能 | herdr | tmux |
| --- | --- | --- |
| Pane 分割 / Tab 管理 | ✅ | ✅ |
| Session persistence (detach/reattach) | ✅ | ✅ |
| SSH remote reattach | ✅ | ✅ |
| Agent state tracking (blocked/working/done) | ✅ | ❌ |
| Agent Socket API | ✅ | ❌ |
| AI agent 原生設計 | ✅ | ❌ |

一個值得注意的細節：中文社群（moewah blog）對 herdr vs tmux 的定位是「互補而非取代」。這個說法有道理，herdr 本身就包含 tmux 的多工功能，對 tmux 老手而言是升級路徑，對新手來說則可以直接從 herdr 開始。

---

## 五分鐘裝好並跑起第一個 Claude Code session

**Step 1：安裝（三選一）**

Mac 用 Homebrew（最快，官方 formula 已確認）：

```bash
brew install herdr
```

Linux 或通用平台用安裝腳本：

```bash
curl -fsSL https://herdr.dev/install.sh | sh
```

從原始碼建置（需要 Rust 環境）：

```bash
cargo install herdr
```

也支援 mise 和 Nix，詳細說明見 [herdr 官方安裝文件](https://herdr.dev/docs/install/) 。

**Step 2：啟動 herdr**

```bash
herdr
```

進入後你會看到類似 tmux 的介面，但多了右側的 agent 狀態 sidebar。基本按鍵：

- `Ctrl+B` 啟動前置鍵（和 tmux 一樣）
- `Ctrl+B V` 垂直分割 pane
- `Ctrl+B C` 建立新 tab
- `Ctrl+B Q` detach（背景持續執行）

**Step 3：啟用 Claude Code 整合**

在 herdr 環境裡執行：

```bash
herdr integration install claude
```

這個指令安裝 Claude Code 的 hook 腳本，讓 herdr 能接收 session 身份資訊。安裝完成後，在 pane 裡直接啟動 `claude` ，sidebar 就會出現 Claude Code 的狀態顯示。

**Step 4：驗證**

啟動 Claude Code 並讓它開始執行任務。herdr sidebar 會顯示目前狀態，等待你輸入時顯示 blocked，執行中顯示 working。

> **重要** ：在 herdr pane 裡再開 tmux session 會導致 agent 狀態偵測失效。建議直接在 herdr pane 裡執行 agent，不要巢狀 tmux。

---

## Agent 狀態追蹤：它怎麼知道 Claude Code 在等你？

herdr 的狀態偵測有兩套機制，準確度截然不同，選擇哪個 agent 直接影響你拿到的資訊品質。

**機制一：Lifecycle Hooks（精確）**

Agent 主動向 herdr 的本地 socket 報告自己的狀態。需要執行 `herdr integration install <agent>` 安裝 hook 腳本。agent 進入 blocked、working、done 等狀態時，hook 會立即通知 herdr，準確度高。

**機制二：Screen Manifest TOML rules（自動但較不精確）**

herdr 定期讀取 terminal 底部的 buffer 截圖，用 TOML 規則比對 UI pattern 來推斷狀態。不需要任何設定，zero-config，但準確度低於 lifecycle hooks。blocked 偵測故意設計偏嚴格，避免把正在執行的 agent 誤判為 blocked。

**哪些 agent 用哪種機制？**

| 支援層級 | 代表 Agent |
| --- | --- |
| Full lifecycle hooks（精確） | Pi、OMP、Kimi Code CLI、OpenCode、Hermes Agent、Kilo Code CLI |
| Screen manifest only（自動，準確度較低） | Claude Code、GitHub Copilot CLI、Codex、Devin CLI、Cursor Agent CLI、Droid、Grok CLI、Amp |
| 部分/實驗性 | Gemini CLI、Cline |

**Claude Code 目前是 screen manifest only** 。 `herdr integration install claude` 安裝的 hook 只負責回報 session 身份（讓 herdr 知道哪個 pane 是哪個 Claude Code session），但 blocked/working 等狀態仍然靠 screen manifest 視覺解析。

這意味著 Claude Code 的狀態偵測比 Pi 或 OpenCode 準確度低。實際使用下來，idle 和 working 的判斷基本可靠，但 blocked 的判斷有時需要等幾秒才會更新。對日常監控夠用，但如果你需要精確到毫秒級的事件驅動，要有對應的預期。

---

## herdr vs tmux vs Warp vs Zellij，應該用哪個？

在決定工具之前，先問自己一個問題： **「我是否需要即時知道每個 AI agent 的執行狀態？」**

如果答案是「不需要，我只是需要分割視窗和 session 持久化」，tmux 完全夠用，沒有理由換。如果答案是「需要，我同時跑多個 agent 而且需要知道誰在等我」，那 tmux 無論多熟都無法解決這個問題。

| 功能維度 | herdr | tmux | Zellij | Warp |
| --- | --- | --- | --- | --- |
| Terminal native（CLI 環境可用） | ✅ | ✅ | ✅ | ❌（GUI app） |
| Session persistence / Detach-reattach | ✅ | ✅ | ✅（部分） | ✅ |
| SSH remote reattach | ✅ | ✅ | ❌ | ❌ |
| Agent state tracking | ✅ | ❌ | ❌ | ❌ |
| Socket / Agent API | ✅ | ❌ | ❌ | ❌ |
| macOS / Linux 支援 | ✅ | ✅ | ✅ | ✅ |
| Windows 支援 | beta | ✅ | ✅ | ✅ |

選擇建議：

- **只需要 terminal 多工** → tmux 或 Zellij 就夠，不需要換
- **同時跑多個 AI agent 且在意狀態可見性** → herdr
- **不想離開 GUI、prefer macOS native 體驗** → Warp（但沒有 agent state tracking）
- **已有 tmux 習慣** → herdr 和 tmux 語法相近（都是 Ctrl+B prefix），遷移成本低

進一步的 AI agent 框架選型思路，可以參考 [AI Agent 框架怎麼選？LangGraph vs CrewAI vs Google ADK 2026 實戰選型指南](https://www.shareuhack.com/posts/ai-agent-framework-comparison-guide-2026) 。

---

## Socket API：讓 Agent 自己管 Agent

大多數介紹 herdr 的文章聚焦在 state visibility（sidebar 看到 blocked/working），但 Socket API 才是 herdr 最有潛力、也最被低估的能力。

herdr 暴露一個本地 Unix socket（預設路徑 `~/.local/share/herdr/herdr.sock` ），任何能呼叫 socket 的程式都可以透過 JSON-RPC 和 herdr 溝通。支援的操作包括：

- **spawn pane** ：在指定 tab 建立新 pane
- **spawn agent** ：在新 pane 裡啟動特定 agent
- **read pane output** ：讀取任意 pane 的 terminal 輸出
- **subscribe state events** ：訂閱 agent 狀態變化事件（agent A 進入 done 狀態時觸發）

這讓 herdr 從「多視窗監控板」升級為「agent orchestration layer」。

一個具體的應用場景：Claude Code 完成 collect 任務後，透過 hook 呼叫 socket API 通知 herdr 啟動下一個 Claude Code instance 執行 synthesize。整個流程不需要人工切換，herdr 負責串接。

```bash
# 透過 socket API 送出 spawn agent 指令（示意）
echo '{"jsonrpc":"2.0","method":"pane.spawn_agent","params":{"agent":"claude","args":["--task","synthesize"]},"id":1}' \
  | nc -U ~/.local/share/herdr/herdr.sock
```

要說清楚的是：Socket API 需要你有基本的 shell scripting 或程式能力，不是點幾個按鍵就能設定的功能。FossEngineer 的實測有展示具體呼叫方式，但端對端的 orchestration workflow 需要自己組裝。對想把 herdr 變成 pipeline 觸發器的開發者，這是最值得深挖的方向。

---

## 限制與你應該知道的注意事項

herdr v0.7.1 是 pre-1.0 工具，官方文件對限制的描述比較散，以下按影響程度整理：

**1\. Windows：--remote 不支援（影響：使用 Windows 的開發者）**

Windows 版本使用 ConPTY backend，目前缺少 `--remote` 支援，SSH remote attach 功能無法使用。本機基本操作可以試，不建議用於遠端管理或生產環境。

**2\. pre-1.0 protocol version bump（影響：生產環境 CI/CD）**

herdr 升版時如果 protocol version 跟著升，舊的 client 無法直接 attach 新的 server，需要重啟 herdr server 才能繼續。對日常開發影響不大，但 CI/CD 環境如果長時間 detach 後需要 reattach，要有對應的 runbook（例如重啟腳本）。

**3\. Claude Code 是 screen manifest only（影響：仰賴精確 blocked 偵測的用戶）**

如同前面說明，Claude Code 的 blocked 狀態靠視覺解析而非事件回報。偵測故意設計偏嚴格，有時需要多等幾秒才更新。多數場景下夠用，但不適合做時序敏感的自動化觸發。

**4\. tmux inside herdr pane：agent 偵測失效（影響：既有 tmux 工作流的用戶）**

在 herdr pane 內再開 tmux session，herdr 無法 inspect 裡面的內容。Docker/VM 內的 agent 需要額外設定 `HERDR_AGENT=<agent>` 環境變數。

**5\. AGPL-3.0 授權（影響：把 herdr 包進 SaaS 產品的開發者）**

AGPL-3.0 對不同使用場景的影響：

- **個人開發者本機使用** ：無任何限制，完全免費
- **公司內部工具** （員工自己用，不對外提供服務）：無實質限制
- **SaaS 場景** （把修改後的 herdr 作為服務提供給外部用戶）：需公開修改後的原始碼，或洽談商業授權

herdr GitHub repo 同時提供商業授權選項，有商業化需求的場景建議直接聯繫作者確認。

---

## 結論：誰應該現在就裝 herdr？

herdr 在 GitHub 拿到 12.2k stars（2026-07-06）不是意外。它解決的問題很具體：多 agent 並行時的 visibility 混亂，而且安裝門檻低。但不是所有人都需要它。

**立即值得裝**

同時跑 2 個以上 AI coding agent，且需要知道哪個在 blocked 等你輸入的開發者。特別是已有 Claude Code + Codex 並行工作流、或正在建 agent pipeline 的場景，herdr 的 state sidebar 和 Socket API 都有直接價值。

**建議觀望**

Windows 用戶（--remote 缺失）、在生產 CI/CD 環境部署多 agent 且對 pre-1.0 穩定性有疑慮的 DevOps 工程師。建議先在 staging 環境試跑，確認 protocol bump 的 runbook 準備好再評估。

**暫時不需要**

只用單一 AI agent 的開發者，tmux 的 session persistence 已經夠用。或者，對「哪個 agent 在等我」這件事完全不在意、工作流以同步為主的開發者。

---

要不要試試？最快的方式就是直接裝起來感受一下 sidebar 的差異：

```bash
brew install herdr
herdr
```

官方文件在 [herdr.dev/docs](https://herdr.dev/docs/) ，Socket API 的詳細說明在 agents 頁面，如果你打算串接自己的 pipeline，那個頁面是最重要的起點。

## FAQ

herdr 怎麼安裝？

Mac 用 \`brew install herdr\`，Linux 或通用平台用 \`curl -fsSL https://herdr.dev/install.sh | sh\`，也可用 \`cargo install herdr\` 從原始碼建置，或透過 mise 安裝。

herdr 需要付費嗎？

herdr 是 AGPL-3.0 開源授權，個人使用和公司內部工具（不對外提供 herdr 服務）完全免費。只有將修改後的 herdr 作為 SaaS 服務提供給外部用戶時，才需要公開原始碼或洽談商業授權。

herdr 跟 tmux 可以一起用嗎？

可以共存，但要注意：在 herdr pane 內再開 tmux session，herdr 的 agent 狀態偵測會失效。建議直接在 herdr pane 裡執行 agent，讓 herdr 負責多工和狀態追蹤。

herdr 支援 Windows 嗎？

有 Windows beta 版，但 ConPTY backend 目前缺少 \`--remote\` 支援，SSH remote attach 無法使用。個人本機可試，不建議用於生產環境。

herdr 目前穩定嗎？

herdr v0.7.1 對日常開發場景相當穩定，12.2k GitHub stars（2026-07-06 數據）代表大量真實用戶驗證。pre-1.0 的主要限制是 protocol version 升級時舊 client 需重啟 server 才能重新 attach，生產環境 CI/CD 建議準備對應的 runbook。

這篇文章對你有幫助嗎？