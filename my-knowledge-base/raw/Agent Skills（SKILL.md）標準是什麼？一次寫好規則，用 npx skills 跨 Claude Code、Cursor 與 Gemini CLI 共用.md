---
title: "Agent Skills（SKILL.md）標準是什麼？一次寫好規則，用 npx skills 跨 Claude Code、Cursor 與 Gemini CLI 共用"
source: "https://www.aiposthub.com/agent-skills-standard-npx-skills/"
author:
  - "[[Davin]]"
published: 2026-11-19
created: 2026-11-20
description: "Agent Skills（SKILL.md）標準是什麼？從 Anthropic 開源破 19.7 萬星的規格出發，拆解如何用 npx skills 跨 Claude Code、Cursor 與 Gemini CLI 一鍵安裝與共用開發技能。"
tags:
  - "clippings"
status: "inbox"
---
以前工程師換一套 AI 寫程式工具，最頭痛的就是規則檔全部要重寫一遍：在 Cursor 要維護 `.cursorrules`，換到 Claude Code 要改寫 `CLAUDE.md`，試用 Gemini CLI 或 Codex 又得重新複製貼上一堆提示詞。

現在這個混亂局面正在被 **Agent Skills** 標準統一。由 Anthropic 率先開源的官方技能庫 `anthropics/skills` 目前在 GitHub 上已經突破 197,525 顆星標，連同社群通用的 `SKILL.md` 格式與 `npx skills` 安裝指令，正快速成為跨工具的共同語言。

**Agent Skills** 的概念其實就像替 AI 代理準備的「隨插即用操作手冊」。它把特定任務所需的專業知識、工作流程、腳本工具與安全限制打包成一個標準資料夾，裡面放一份名為 `SKILL.md` 的核心說明文件，讓支援該標準的代理只在需要時才動態載入。

對台灣開發者與技術團隊來說，這代表「寫一次規則，所有代理通用」終於成真，而且不用再把所有規範硬塞進系統提示詞裡浪費 Token。接下來，我們拆解 Agent Skills 的檔案結構、`npx skills` 的實務用法，以及安裝第三方技能前必做的安全檢查。

延伸閱讀： [不只 Vibe Coding：mattpocock skills 完整教學](https://www.aiposthub.com/mattpocock-skills-guide/)

![Anthropic 開源 Agent Skills（SKILL.md）標準官方專案封面圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/11/agent-skills-standard-npx-skills-official.png)

Anthropic 開源的 Agent Skills 標準讓開發規範可跨多個寫程式代理共用。 圖片來源： 官方來源 。

## Agent Skills（SKILL.md）如何解決規則重複與 Token 浪費？

在沒有技能標準之前，團隊習慣把所有測試規範、部署指令與程式碼風格通通寫進單一全域規則檔裡。問題是，當代理只是要幫你改一行前端文字顏色時，它還是得被迫先讀完幾千字的後端資料庫遷移規範，白白燒掉大量輸入 Token。

Agent Skills 透過「按需載入（Lazy Loading）」解決了這個問題。每個技能資料夾的開頭只有幾行輕量的 YAML 描述，告訴代理「這個技能是做什麼的、什麼情況下該打開它」。平時代理只會記住這份目錄清單，直到你真的下達相關任務，它才會去讀取完整的 `SKILL.md` 步驟與附屬腳本。

這種設計同時讓個人工程師與團隊主管受益：專案根目錄不再被十幾個不同工具的設定檔塞滿，新進工程師只要執行一次套件安裝，就能讓自己習慣的終端機或編輯器立刻學會公司的開發規範。

| 規則管理方式 | 跨工具相容性 | 上下文 Token 消耗 | 團隊分享與更新難度 |
| --- | --- | --- | --- |
| 各家專屬規則檔 | 需各自維護不同格式 | 每次對話全量載入 | 需手動複製貼上同步 |
| Agent Skills（SKILL.md） | 跨 Claude Code、Cursor、Gemini CLI 通用 | 平時僅載入摘要，觸發才讀全文 | 可透過 Git 與 `npx skills` 一鍵同步 |

## 怎麼用 npx skills 安裝與建立自己的第一個技能？

根據 [Anthropic Agent Skills 官方專案](https://github.com/anthropics/skills?ref=aiposthub.com) 與跨平台規範文件，一個標準的技能資料夾結構非常簡單，核心只需要一個 `SKILL.md` 檔案，裡面包含名稱、觸發條件描述與具體執行步驟；若有需要，也能在同一資料夾內附上 `scripts/` 輔助腳本或 `references/` 參考文件。

在日常安裝上，社群已經普遍採用 `npx skills add <owner>/<repo>` 這類指令工具，直接把經過驗證的開源技能拉進本地的 `.agents/skills/` 或各家代理指定的設定目錄中。整個過程完全免費開源，不需要特殊硬體或額外帳號，只要電腦有 Node.js 就能操作。

如果你正準備替團隊建立第一份共用技能，建議從「最常重複糾正 AI 的那件事」下手，例如「我們公司提交 PR 前的測試與 Commit 格式規範」，寫成一份不到一百行的 `SKILL.md` 放在團隊倉庫裡，所有人立刻就能同步受惠。想深入了解這套開放標準的跨平台生態與規格細節，也可查閱 [Agent Skills 開放標準網站](https://agentskills.io/?ref=aiposthub.com) 的完整文件。

延伸閱讀： [NVIDIA SkillEvaluator 是什麼？開源多層沙箱評測框架，幫 AI 代理技能把關資安與真實成效](https://www.aiposthub.com/nvidia-skillevaluator-agent-skill-testing/)

## 安裝社群技能包前的資安警訊：千萬別盲目一鍵執行

隨著 GitHub 上出現成千上萬個開源技能包，資安風險也跟著浮現。別忘了，技能資料夾裡除了純文字說明，常常還附帶可直接執行的 Python 或 Shell 腳本；一旦你盲目安裝了來路不明的技能，惡意提示詞就可能誘導你的 AI 代理把本機 `.env` 金鑰傳送到外部伺服器。

因此，透過 `npx skills` 安裝任何第三方技能前，務必養成三個習慣。首先，優先選擇官方團隊或高星標可信維護者的儲存庫；第二，安裝後先自己打開 `SKILL.md` 與 `scripts/` 資料夾掃一眼，確認沒有可疑的網路外連指令；第三，搭配像 SkillEvaluator 這類的沙箱評測工具先跑過驗證，再放進正式開發環境。

## 常見問題（FAQ）

### Q1：Agent Skills 只有 Claude Code 能用嗎？

不是。目前這套開放標準已經獲得 Claude Code、Cursor、OpenAI Codex、Gemini CLI 與 Antigravity 等主流開發工具與代理框架廣泛支援。

### Q2：寫一份 SKILL.md 需要會寫程式嗎？

不需要。最基礎的技能只需要用自然的條列式文字寫清楚「什麼時候用」以及「執行步驟與限制」，只有需要確定性計算或自動化動作時才需要附上腳本。

### Q3：一個專案可以同時裝很多個技能嗎？

可以。因為代理平時只會讀取每個技能的簡短摘要，所以即使裝了二三十個技能，也不會像傳統長提示詞那樣把上下文塞爆；但仍建議避免裝入功能高度重疊的技能，以免觸發錯亂。

### Q4：公司內部的私有規範可以做成 Agent Skills 嗎？

非常適合。你可以把技能資料夾放在公司私有的 Git 倉庫裡，不對外公開，僅供內部工程師拉取共用。

## 結語：把開發經驗打包成可移植的團隊資產

從單純寫提示詞進化到建立標準化的 **Agent Skills** ，最大的價值在於「經驗不會隨著工具汰換而歸零」。今天不管團隊主力用哪一款模型或編輯器，只要把規範沉澱成標準化的 `SKILL.md`，就能立刻跨平台帶走。

建議今天先挑出你最常對 AI 重複下達的兩三個指令，花十五分鐘寫成一份標準技能檔，下次打開不同的寫程式代理時，你會發現它們瞬間就懂你的規矩了。

歡迎免費訂閱 AI 郵報，每週追蹤更多實測工具教學與半導體產業動態。