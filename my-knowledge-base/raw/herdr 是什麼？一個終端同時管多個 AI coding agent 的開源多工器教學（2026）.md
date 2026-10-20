---
title: "herdr 是什麼？一個終端同時管多個 AI coding agent 的開源多工器教學（2026）"
source: "https://slash-invest.com/herdr-terminal-ai-agent-multiplexer-2026/"
author:
  - "[[Mr. Slash]]"
published: 2026-07-02
created: 2026-08-14
description: "herdr 是開源終端多工器，讓你在一個終端同時跑多個 AI coding agent（Claude Code、Codex、Cursor、Antigravity 等 13+），狀態一眼看懂、可斷線重連。安裝、操作、AGPL 授權一次看懂。"
tags:
  - "clippings"
status: "processed"
---
*截至 2026 年 7 月。功能與授權以官方 GitHub 為準，本文為中性技術介紹，不構成投資或消費建議。*

當你開始同時用好幾個 AI coding agent——一個 [Claude Code](https://slash-invest.com/claude-code-vs-cursor-vs-codex-2026/) 跑重構、一個 Codex 寫測試、再開個 Cursor Agent 查 bug——很快就會遇到一個蠢問題： **終端機一團亂** 。哪個在跑、哪個卡住、哪個跑完了，全靠你自己記。最近一個約 9,900 星的開源工具 **herdr** 就是來收拾這個亂局的。

它是用 Rust 寫的、約 10MB 的單一執行檔，定位是「AI agent 的終端多工器」。這篇幫你看懂它解決什麼、怎麼用、要不要裝。

## herdr 是什麼？

白話講：它讓你 **在一個終端裡，把多個 AI coding agent 排排站、各自有獨立工作區同時跑** ，而且一眼就看得出每個 agent 現在是什麼狀態。跟那種花俏的 GUI 管理器不同，它就是個輕量執行檔，能在 SSH、任何終端環境裡跑，還能斷線後重連、session 不會掉。

## 核心功能

- **每個 agent 一個「真」終端** ：不是模擬的，完整支援 TUI 畫面渲染。
- **狀態一眼看懂** ： 卡住／ 工作中／ 完成／ 閒置，不用一個個點進去確認。
- **工作區、分頁、分割窗** ：滑鼠原生操作，也能純鍵盤。
- **Session 持久化** ：可 detach／reattach，斷線重連不掉進度。
- **超輕量** ：約 10MB 執行檔，Linux／macOS（Windows beta）。
- **可編程** ：本地 Unix socket API、可寫腳本整合與外掛。

## 支援哪些 agent？

這是它最實用的地方——幾乎你聽過的 coding agent 它都認得：Claude Code、Codex、Cursor Agent、 [Antigravity CLI](https://slash-invest.com/gemini-cli-vs-antigravity-cli-2026/) 、Hermes Agent、GitHub Copilot CLI、Grok CLI、Devin CLI、Kimi Code CLI 等 13+ 種都有官方整合（ `herdr integration install <agent>` ）。偵測靠「行程名比對＋終端輸出啟發式」。

## 怎麼裝、怎麼用

1. **安裝** （擇一）：  
	`curl -fsSL https://herdr.dev/install.sh | sh`  
	或 `brew install herdr`
2. **啟動** ：打 `herdr` ，它會連上（或建立）背景 server 並開一個工作區。
3. **操作** ：在窗格裡啟動你的 agent。滑鼠可拖拉分頁窗格；鍵盤用 `Ctrl+B` 當前綴鍵，接著按動作鍵（例如 `Ctrl+B` 再按 `c` 開新分頁、 `w` 切工作區、 `q` detach）。

用過 tmux 的人會覺得很熟——它基本上就是「為 AI agent 場景重做的 tmux」。

## 要錢嗎？授權要看清楚

個人／開源用途 **免費** 。但授權要注意：它是 **AGPL-3.0 雙授權** ——開源用 AGPL-3.0， **組織／商業使用另有商業授權** 。AGPL 對「把它整合進你對外提供的服務」有較嚴格的開源要求，公司要用前建議先確認授權條款，別踩雷。

## 適合誰？不適合誰？

- **適合** ：同時跑多個 AI agent、終端已經亂到管不動的重度開發者；常在 SSH／遠端機器工作、需要 session 持久化的人。
- **不適合** ：一次只用一個 agent 的人（沒必要）；完全不碰終端機的純新手；商業整合但不想處理 AGPL 授權的團隊（要走商業授權）。

## 常見問題 FAQ

### 它跟 tmux 差在哪？

核心概念像 tmux（多工、持久 session），但 herdr 是專為「管理多個 AI agent」設計的——內建 agent 狀態指示、官方整合十幾種 CLI、有給 agent 用的 orchestration API。tmux 通用但不懂 agent；herdr 專精這個場景。

### Windows 能用嗎？

有 Windows preview（\`powershell\` 安裝腳本），但目前 Linux／macOS 較成熟，Windows 還在 beta。

*免責聲明：本文為 AI 工具的中性資訊整理，數據以官方 GitHub 為準，可能隨版本變動。授權條款（AGPL-3.0／商業授權）請以官方為準、商業使用前自行確認。內容不構成任何投資、消費或技術採用建議。*