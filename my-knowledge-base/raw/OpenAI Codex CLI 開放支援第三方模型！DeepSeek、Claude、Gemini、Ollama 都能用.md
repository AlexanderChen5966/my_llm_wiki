---
title: "OpenAI Codex CLI 開放支援第三方模型！DeepSeek、Claude、Gemini、Ollama 都能用"
source: "https://www.koc.com.tw/archives/646263"
author:
  - "[[達小編]]"
published: 2026-06-18
created: 2026-06-23
description: "這項消息由開發者 Tibo 在 X 上分享，他在貼文中寫道：「提醒你，Codex App、CLI 和 SDK 可以使用任何開源模型，不僅限於 OpenAI 模型。」這對習慣被綁定在單一生態系的開發者來說，是一大福音。"
tags:
  - "clippings"
status: processed
---
OpenAI 旗下的 AI 編碼代理工具 Codex CLI 近期開放支援任何第三方模型，不限於 OpenAI 自家模型。開發者只需透過 config.toml 設定檔，即可將 Codex 接入 DeepSeek、Claude、Gemini、Ollama 本地模型等任意相容 OpenAI Responses API 的服務，大幅擴展了開發者的選擇空間。  

這項消息由開發者 Tibo 在 X 上分享，他在貼文中寫道：「提醒你，Codex App、CLI 和 SDK 可以使用任何開源模型，不僅限於 OpenAI 模型。」這對習慣被綁定在單一生態系的開發者來說，是一大福音。

![](https://x.com/i/status/2067181377028538431)

### Codex CLI 是什麼？

Codex CLI 是 OpenAI 推出的輕量級編碼代理工具，在本機終端機中執行，支援獨立使用或整合至 VS Code、Cursor、Windsurf 等 IDE。與 Claude Code 需要 npm 安裝不同，Codex CLI 以單一二進位檔案發布，支援 macOS、Windows、Linux 三大平台，安裝流程更簡潔。目前 GitHub 上的開源專案已累積超過 91,600 顆星、13,500 個 Fork，最新版本為 0.140.0（2026 年 6 月 15 日發布），總計多達 845 個版本釋出，採用 Rust 語言開發（佔 96.2%）。

![OpenAI Codex CLI 開放支援第三方模型！DeepSeek、Claude、Gemini、Ollama 都能用 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/06/20260618021225_0_48e33b.webp "OpenAI Codex CLI 開放支援第三方模型！DeepSeek、Claude、Gemini、Ollama 都能用 - 電腦王阿達")

除了 CLI 版本，OpenAI 也提供 Codex Desktop 桌面應用程式與 Codex Web 雲端版本（chatgpt.com/codex），讓開發者依需求選擇使用方式。

### 第三方模型支援：config.toml 設定

根據 OpenAI 官方 [開發者文件](https://developers.openai.com/codex/config-advanced#oss-mode-local-providers) ，Codex 的模型設定透過 config.toml 檔案管理，支援多層級覆蓋：使用者層級（~/.codex/config.toml）、專案層級（.codex/config.toml）以及命令列參數。開發者可以在設定檔中註冊多個 Provider，並隨時切換。

每個自訂 Provider 需要三個核心欄位：base\_url（API 端點位址）、env\_key（API Key 對應的環境變數名稱）以及 name（顯示名稱）。由於 2026 年 2 月起 Codex 已移除 Chat Completions API 支援，所有第三方模型必須相容 OpenAI Responses API，這一點在設定時需要特別留意。

以下是接入 DeepSeek 的設定範例：

`[model_providers.deepseek]   name = "DeepSeek"   base_url = "https://api.deepseek.com/v1"   env_key = "DEEPSEEK_API_KEY"   wire_api = "responses"`

設定完成後，即可透過命令列指定使用第三方模型： `codex --provider deepseek --model deepseek-coder "寫一個快速排序"` 。開發者也可以在同一個設定檔中註冊多個 Provider，依任務需求動態切換。

如果不會設置的話，其實直接叫 Codex 幫你設置也可以，可以依照這則 Ｘ裡面的流程去做，動動嘴就搞定：

![](https://x.com/i/status/2067443263276003667)

### 支援的第三方模型生態

由於 Codex 採用開放的 Responses API 協定，理論上任何相容該協定的模型服務都能接入，包括：

第三方模型方面，DeepSeek（deepseek-chat / deepseek-coder）、Kimi（Moonshot）、智譜 GLM、Qwen 等均可透過相同設定方式接入。本地模型方面，Ollama 與 LM Studio 可透過 http://localhost:11434/v1 端點接入，讓開發者在離線環境中使用 Codex，無需網路連線即可進行編碼工作。聚合 API 閘道方面，OfoxAI 等服務提供單一 API Key 即可存取 GPT、Claude、Gemini、DeepSeek 等多種模型，簡化金鑰管理流程。企業級方案方面，Azure OpenAI 與 Google Vertex AI（Gemini 2.5 Pro）也都有對應的設定配方。

### OpenAI 的開放策略

Codex 支援第三方模型的做法，與 OpenAI 過去封閉的生態策略形成鮮明對比。這讓 Codex 從「OpenAI 模型的專用客戶端」轉變為「模型中立的編碼代理平台」，開發者可以根據成本、效能或隱私需求自由選擇底層模型，不再受限於單一供應商。

事實上，OpenAI [官方文件](https://developers.openai.com/codex/models) 也明確指出：「你可以將 Codex 指向任何支援 Chat Completions 或 Responses API 的模型和 Provider，以滿足你的特定使用場景。」這項開放策略擴大了 Codex 的應用範圍，也讓它在與 Claude Code、GitHub Copilot 等競品的競爭中取得了獨特的定位優勢。

### Claude Code 的第三方模型支援：社群自力救濟

相較於 OpenAI 大方開放，Anthropic 對 Claude Code 使用第三方模型的態度則保守許多。Claude Code 原生僅支援 Anthropic 自家模型（Opus、Sonnet、Haiku），官方文件並未提供接入 DeepSeek、Gemini 等第三方模型的正式支援。

然而開發者社群並未因此卻步。由於 Claude Code 的 API 端點可透過環境變數 ANTHROPIC\_BASE\_URL 重新指向，社群開發者透過將請求轉發至相容 Anthropic API 格式的第三方服務（如 OpenRouter、DeepSeek 等），仍能繞過限制使用其他模型。近期 Claude Code Desktop 1.6259.1 版本據報導已開始加入模型白名單機制，進一步封鎖第三方模型 ID，顯示 Anthropic 正逐步收緊這條路徑。

### CC Switch：統一管理所有 AI 編碼 CLI 工具

在這種「官方不給、社群自己來」的背景下，一款名為 CC Switch 的開源桌面應用迅速崛起。這款基於 Tauri 2 建構的工具，提供一個統一的圖形化介面來管理 Claude Code、Codex、Gemini CLI、OpenCode、OpenClaw 以及 Hermes Agent 等七款 AI 編碼 CLI 工具的 Provider 設定，支援 macOS、Windows、Linux 三大平台，在 GitHub 上已累積超過 103,000 顆星、900 萬次下載。

<iframe title="Claude Code 安裝手把手教學：從零開始，搭配 CC Switch 輕鬆切換模型 — 電腦王阿達" src="https://www.koc.com.tw/archives/642628/embed#?secret=tpGDH0Ydfj#?secret=kDSpcaFdOB" width="500" height="621" frameborder="0"></iframe>

CC Switch 的核心功能包括：內建 50 組以上的 Provider 預設、一鍵切換 Provider、本地代理（Local Proxy）支援熱切換與自動故障轉移（當主要 Provider 異常時自動切換到備用 Provider）、統一管理 MCP 伺服器與 Skills、用量儀表板即時追蹤 Token 消耗與成本，以及跨裝置雲端同步。開發者無需再手動編輯 JSON、TOML 或.env 檔案，所有操作都在一個桌面應用中完成。CC Switch 採用 Rust 後端與 React 前端架構，所有 API Key 與設定均安全儲存在本地 SQLite 資料庫中。

![Claude Code 安裝手把手教學：從零開始，搭配 CC Switch 輕鬆切換模型 - 電腦王阿達](https://www.koc.com.tw/wp-content/uploads/2026/05/20260531171145_0_547ad4.jpg "OpenAI Codex CLI 開放支援第三方模型！DeepSeek、Claude、Gemini、Ollama 都能用 - 電腦王阿達")

這也形成了一個明顯的品牌格局對比：OpenAI 選擇開放策略，讓 Codex 成為模型中立的編碼代理平台；Anthropic 則傾向封閉生態，將 Claude Code 綁定在自家模型上。而 CC Switch 這類社群工具的出現，恰好填補了 Claude Code 使用者對模型選擇自由的需求缺口。（編按：個人一直覺得 Claude 是好模型，但它的母公司 Anthropic 就不與置評了..）

### 總結

OpenAI Codex CLI 開放第三方模型支援，是 AI 編碼工具市場的一個重要轉折。這意味著開發者不再被綁定在 OpenAI 的生態系中，可以自由選擇 DeepSeek 的低成本方案、Ollama 的本地離線方案，或是 Claude 與 Gemini 等其他前沿模型。對於一個由 OpenAI 開發並開源的工具來說，這種「擁抱開放」的策略贏得了開發者社群的掌聲，也為 AI 編碼代理的未來發展開創了更多可能性。而 CC Switch 這類社群工具的存在，更讓開發者可以在不同編碼代理之間無縫切換，形成一個完整的開放生態系。