---
title: "Google Antigravity 全解析：從 IDE 變成 Agent 開發平台（含 CLI 取代 Gemini CLI）"
source: "https://www.darrelltw.com/google-antigravity-ide/"
author:
  - "[[Darrell]]"
published: 2025-11-19
created: 2026-05-22
description: "Google Antigravity 2026/05 重大更新：從單一 IDE 變成 Agent 開發平台。完整解析 Antigravity IDE、Antigravity 2.0 桌面 app、Antigravity CLI（取代 Gemini CLI）三大產品差異，搭配 IDE 完整安裝教學、Agent Manager、Skills 設定。"
tags:
  - "clippings"
status: processed
---
Google Antigravity 自 2025 年 11 月推出時，是一款基於 Visual Studio Code 的 AI Coding IDE，  
主打 “Agentic” 開發體驗，  
用起來很像 Cursor 和 Windsurf。

但 **2026 年 5 月 19 日 Google I/O** 公布 Antigravity 2.0 後，  
Antigravity 不再只是 IDE，而是變成一個完整的 Agent 開發平台。

## Antigravity 2.0 帶來的三個產品

### Antigravity

變成很像 Codex App 的桌面應用程式，使用者只會看到 Prompt 的輸入欄位  
但能同時啟動多個 Session 來工作

![Antigravity 2.0 桌面 app 主介面：左側 sidebar 顯示專案列表與對話歷史，使用者只看到 Prompt 輸入欄位，設計類似 Codex App](https://www.darrelltw.com/google-antigravity-ide/antigravity_2_chat_interface.png)

Antigravity 2.0 桌面 app 主介面：左側 sidebar 顯示專案列表與對話歷史，使用者只看到 Prompt 輸入欄位，設計類似 Codex App

### Antigravity IDE

其實就是我們熟悉的 Antigravity，只是更名為 Antigravity IDE  
不要小看一個改名字，目前社群上大家都偏反感  
因為需要 IDE 的使用者還需要重新下載安裝一次

![Antigravity IDE Workspace 歡迎畫面：更名後使用者需要重新下載安裝，是社群反感的主因](https://www.darrelltw.com/google-antigravity-ide/antigravity_ide_welcome_screen.png)

Antigravity IDE Workspace 歡迎畫面：更名後使用者需要重新下載安裝，是社群反感的主因

[Antigravity IDE 下載網址](https://antigravity.google/download#antigravity-ide)

### Antigravity CLI

取代 Gemini CLI，用 Go 整個重寫，保留部分 Gemini CLI 的核心功能  
安裝完成後，可以用 `agy` 命令來啟動  
驚喜的是能使用 Claude Opus 和 Sonnet 模型

![Antigravity CLI 切換模型選單：意外支援 Claude Opus 和 Claude Sonnet，不只 Gemini 系列](https://www.darrelltw.com/google-antigravity-ide/antigravity_cli_model_switch.png)

Antigravity CLI 切換模型選單：意外支援 Claude Opus 和 Claude Sonnet，不只 Gemini 系列

## 實際開發體驗

我們透過建立一個天氣卡片的方式，來建立一個網頁看看  
在右側放入，請幫我們建立天氣卡片，要包含四種不同的天氣資訊  
並選擇用 planning 先請他規劃怎麼做

### 規劃階段 (Implementation Plan)

Planning 完成時，他會產生一份 Implementation Plan  
裡面會是他目前的規劃內容和方向

你可以先審查他規劃的是否與你相同  
後續他就會根據這個規劃來思考要怎麼執行

![Antigravity Agent 聊天輸入框：使用者輸入「請幫我們建立天氣卡片，要包含四種不同的天氣資訊」，並選擇 Planning 模式與 Gemini 3 Pro (High) 模型](https://www.darrelltw.com/google-antigravity-ide/antigravity-ide-implementation_plan.png)

Antigravity Agent 聊天輸入框：使用者輸入「請幫我們建立天氣卡片，要包含四種不同的天氣資訊」，並選擇 Planning 模式與 Gemini 3 Pro (High) 模型

### 任務清單 Task

Agent 會將 Implementation Plan 拆解成可執行的任務清單 Task List

除了建立檔案外，就會開始實作四種天氣的動畫  
再加入 JavaScript 來控制天氣的切換  
最後會 **自行做測試**

![Google Antigravity 任務清單面板展開畫面，顯示完整的 Weather Cards 任務列表與執行步驟](https://www.darrelltw.com/google-antigravity-ide/feature-task-list-panel.png)

Google Antigravity 任務清單面板展開畫面，顯示完整的 Weather Cards 任務列表與執行步驟

執行的過程中也會回來更新進度，所以你可以掌握目前執行到哪裡

### 執行中的進度顯示

當 Agent 開始執行時，任務清單會即時更新狀態：

![Google Antigravity 任務清單面板，顯示 Weather Cards 專案的任務進度追蹤，部分任務已完成並標記勾選](https://www.darrelltw.com/google-antigravity-ide/feature-task-progress-tracking.png)

Google Antigravity 任務清單面板，顯示 Weather Cards 專案的任務進度追蹤，部分任務已完成並標記勾選

可以看到：

- 打勾： **Create implementation plan** （已完成）
- 圓圈： 多項任務正在執行中
- 框框： 等待執行的任務

## Agent Manager 一次管理多個專案

Agent Manager 也是 Antigravity 的一大特色  
用來同時指揮不同 Agent

![Google Antigravity Agent Manager 介面，展示左側 Workspaces 工作區列表與中間 Inbox 收件匣對話列表](https://www.darrelltw.com/google-antigravity-ide/google-antigravity-agent-manager-inbox.png)

Google Antigravity Agent Manager 介面，展示左側 Workspaces 工作區列表與中間 Inbox 收件匣對話列表

左邊會呈現 workspaces 和 playground 的列表  
每個 workspace 就是你的專案  
playground 是一個比較有趣的設計，它可以讓你快速測試一些想法

### Workspaces

你可以針對同一個專案一次建立不同的 Agents  
例如前端、後端、資料庫等等  
同時讓他們去做不一樣的事情，最後再把這些拚圖合併在一起

而以往就是需要一步一步慢慢來  
先資料庫 → 後端 → 前端，等到串接發現有問題再回頭修改

現在可以一次性的先跑完，省去很多等待的時間  
當然這在其他的工具也都能做到類似的事情  
只是 Agent Manager 在視覺和管理上更為方便

實際運作時，你可以看到多個任務同時處於 **Running…** 狀態：

![Google Antigravity Agent Manager 展示真正的平行處理：AI Models n8n Workflow Outline 和 Debug n8n Mobile Template 兩個任務同時執行中](https://www.darrelltw.com/google-antigravity-ide/agent-manager-parallel-tasks-running.png)

Google Antigravity Agent Manager 展示真正的平行處理：AI Models n8n Workflow Outline 和 Debug n8n Mobile Template 兩個任務同時執行中

截圖中也能看到，目去有兩個 Agents 正在同一個 workspace 執行中

### Playground

Playground 則是一個蠻有趣的設計  
有時候有些想法想要得到快速驗證  
但這個想法又不在某個專案中

這時候可以先到 Playground 試玩看看  
如果覺得這個靈光一現的想法可行  
後續再把它轉為專案也可以！

例如下面這個範例，在 Playground 中建立了一個「自動遊玩的貪食蛇」：

![Google Antigravity Playground 展示 Self-Playing Snake Game 專案，包含完整的 Walkthrough 驗證報告、自動化測試截圖和瀏覽器錄影](https://www.darrelltw.com/google-antigravity-ide/playground-snake-game-walkthrough.png)

Google Antigravity Playground 展示 Self-Playing Snake Game 專案，包含完整的 Walkthrough 驗證報告、自動化測試截圖和瀏覽器錄影

的確很快就做完，並且開啟瀏覽器讓我們確認是否真的會自己動起來  
接著我們來講講自動化測試這塊。

## Antigravity Browser Extension

Antigravity 還提供了 Chrome 瀏覽器擴充功能：  
**Antigravity Browser Extension**

![Google Antigravity 瀏覽器擴充功能頁面，顯示 Antigravity Browser Extension 在 Chrome Web Store 的安裝頁面](https://www.darrelltw.com/google-antigravity-ide/feature-chrome-extension.png)

Google Antigravity 瀏覽器擴充功能頁面，顯示 Antigravity Browser Extension 在 Chrome Web Store 的安裝頁面

安裝好後，後續需要測試時  
Antigravity 會自動開啟瀏覽器，並測試網頁是否有正確運行  
例如天氣動畫是否真的有動畫  
不同天氣的切換按鈕可以做切換

### 實際瀏覽網站的範例

下圖展示 Agent 自動瀏覽網站並擷取資訊的過程：

![Antigravity Agent 自動化瀏覽範例：左側顯示任務清單（開啟網站、尋找文章標題），右側顯示 Agent 執行 agent-browser 指令瀏覽 darrelltw.com 並分析頁面快照的完整過程](https://www.darrelltw.com/google-antigravity-ide/agent-browser-automation-demo.png)

Antigravity Agent 自動化瀏覽範例：左側顯示任務清單（開啟網站、尋找文章標題），右側顯示 Agent 執行 agent-browser 指令瀏覽 darrelltw.com 並分析頁面快照的完整過程

可以看到 Agent 會：

1. 自動開啟目標網站
2. 分析頁面結構（Analyzing page snapshot）
3. 點擊特定元素（agent-browser click）
4. 擷取需要的資訊回報

這讓自動化測試變得更加直覺，不需要自己寫測試腳本。

## Agent Skills：讓 Agent 真正動手做事

Antigravity 在 2026 年 1 月支援了 **Agent Skills**

Skills 你可以想成是一個方法論，又或者是一個 SOP  
別人分享的 Skills 往往就是為了解決某個問題而產生的

所以最重要的是：「你應該是把自己的工作流程，包裝成一個 skills 未來重複利用」  
更棒的是，這份 Skills 可以跟著你一起成長，你新學到的知識或是更有效率的方法  
都可以整併回 Skills 中！

### SKILL.md 格式

每個 Skill 就是一個包含 `SKILL.md` 檔案的資料夾  
格式很簡單，用 YAML frontmatter 定義名稱和描述：

```yaml
---
name: code-review
description: Reviews code changes for bugs, style violations, and security issues.
---

## Instructions
在這裡寫詳細的指令內容...
```

**YAML frontmatter 欄位說明** ：

- `name` （選用）：識別名稱，預設為資料夾名稱
- `description` （ **必要** ）：清楚描述這個 skill 做什麼、何時使用

**完整資料夾結構** ：

```
.agent/skills/my-skill/
├── SKILL.md          # 主要指令（必要）
├── scripts/          # 輔助腳本（選用）
├── examples/         # 參考實作（選用）
└── resources/        # 模板和素材（選用）
```

Agent 在執行 skill 時可以讀取這些檔案，讓你的 skill 更完整。

**存放位置** ：

- 專案級：`.agent/skills/` （只在該專案生效）
- 全域： `~/.gemini/antigravity/global_skills/` （所有專案都能用）

### 直接把 Claude Skills 連結過來

這裡要分享一個超實用的技巧！

Agent Skills 是 **開放標準** （由 [Anthropic 發布](https://agentskills.io/) ）  
所以 **Claude Code** 和 **Antigravity** 都支援同樣的格式

這代表你可以用 `ln -s` （符號連結）讓兩個工具 **共用同一套 Skills**  
原理就像「Windows 的捷徑」：

![UNIX 符號連結與 Windows 捷徑的概念對照圖：左側展示 ln -s 指令如何將 Claude Skills 連結到 Antigravity Skills，右側展示 Windows 捷徑如何指向原始資料夾](https://www.darrelltw.com/google-antigravity-ide/unix-symlink-windows-shortcut.jpg)

UNIX 符號連結與 Windows 捷徑的概念對照圖：左側展示 ln -s 指令如何將 Claude Skills 連結到 Antigravity Skills，右側展示 Windows 捷徑如何指向原始資料夾

```bash
# 把 Claude Code 的 skill 連結到 Antigravity
ln -s ~/.claude/skills/my-skill ~/.gemini/antigravity/global_skills/my-skill
```

**路徑對照** ：

| 工具 | Skills 路徑 |
| --- | --- |
| Claude Code | ~/.claude/skills/ |
| Antigravity | ~/.gemini/antigravity/global\_skills/ |

這樣只需要維護一份 Skills，兩個工具都能使用！  
對於同時使用 Claude Code 和 Antigravity 的開發者來說非常方便

官方文件： [Antigravity Skills Documentation](https://antigravity.google/docs/skills)

### Skills 使用方式

Skills 採用 **Progressive Disclosure** 機制：

1. **Discovery（發現）** ：Agent 看到可用的 skills 列表（名稱和描述）
2. **Activation（啟用）** ：如果判斷相關，Agent 讀取完整的 [SKILL.md](http://skill.md/) 內容
3. **Execution（執行）** ：Agent 依照 skill 的指令執行任務

這個機制可以避免 context saturation，只在需要時才載入相關知識。

所以有點考驗你在 Skills 的 name 跟 description 寫的描述有沒有很清楚  
讓 AI 能知道說  
當遇到 xx 需求時，我需要載入 aaa 這個 Skill 來使用

### 實際使用範例

下圖展示一個 Remotion best practices Skill 的實際運作：

![Antigravity Skills 實際使用範例：Agent 根據 Remotion best practices Skill 分析程式碼結構，提出模組化、字體載入、資料分離、參數化四個優化方向，並交叉驗證 Skill 檔案內容](https://www.darrelltw.com/google-antigravity-ide/antigravity-skills-demo-remotion_best_practices_code_analysis.webp)

Antigravity Skills 實際使用範例：Agent 根據 Remotion best practices Skill 分析程式碼結構，提出模組化、字體載入、資料分離、參數化四個優化方向，並交叉驗證 Skill 檔案內容

Agent 會：

1. 讀取 Skill 中的 best practices 知識
2. 分析你的程式碼，提出具體優化建議
3. 被質疑時，能交叉比對 Skill 檔案（如 [fonts.md](http://fonts.md/) 、 [transitions.md](http://transitions.md/) ）來佐證建議

這就是 Skills 的價值：不只是給 Agent 一段指令，而是讓它有 **可驗證的知識來源** 。

## 模型選項與配額限制

### 支援的 AI 模型

Antigravity 目前支援以下模型：

- **Gemini 3 Pro (High)**
- **Gemini 3 Pro (Low)**
- **Claude Opus 4.5 Thinking**
- **Claude Sonnet 4.5**
- **GPT-OSS**

### 使用配額限制

- Ultra 和 Pro 用戶的 Quota 是 **每 5 小時重置一次**
- Free 用戶的 Quota 是 weekly limit，使用到上限後需要等到下週

官方文件說明： [https://antigravity.google/docs/plans](https://antigravity.google/docs/plans)

### 節省配額的小技巧

目前可以讓 Claude Opus 4.5 Thinking 模型來做思考跟規劃  
做完規劃後再請 Gemini 3 模型來做實作  
或是使用 Claude Sonnet 4.5 也可以  
理論上就能降低 Quota 的消耗

## 結論

Antigravity 經過半年從「單一 IDE」變成「6 產品平台」，  
等於 Google 想跟 Anthropic（Claude Code + Claude Desktop + Agent SDK）和 OpenAI（Codex CLI + ChatGPT Desktop + Agents SDK）正面對打。

### IDE 心得

這次的 Antigravity IDE 搭配 Gemini 3 是一個不錯的體驗  
對於小型專案的建立和開發又更方便了

以往在 Cursor 例如要自動化測試，自己就需要安裝別的套件或是第三方工具  
這次 Google 自己整合好 Chrome 讓整體體驗更順暢

不過 Gemini 3 模型是否能像 Claude Sonnet 4.5 在各種類型的專案都一樣表現不錯  
就需要時間和大家的心得來驗證

目前 Antigravity IDE 還在早期階段，並且是免費讓大家使用  
很常會遇到 error 或是 model overloaded 的情況  
也算是美中不足的地方，畢竟開發時被打斷很容易就索性不用  
不如用付費的 Claude Code 穩穩定定開發

也期待 Antigravity 後續的付費方案是否能跟 Google AI Pro 或 Ultra 整併

### 平台化後的觀察

最關鍵的訊號是 **Gemini CLI 在 6/18 停服** ，等於 Google 自己承認「Gemini CLI 在跟 Claude Code 對打時輸了」，  
所以才砍掉重練一套 Antigravity CLI（改用 Go 重寫、共用 Antigravity 2.0 brain）。

Antigravity 2.0 桌面 app 「沒有編輯器、只有 chat 視窗」的設計也很大膽，  
比較像 Claude Desktop 而不是 Cursor，  
這是賭「未來開發者不再看 code、只看 agent 結果」的方向。

兩個新產品我都還沒實測，後續會分別寫深度文：

- **Antigravity CLI vs Claude Code 比較實測**
- **Antigravity 2.0 桌面 app 體驗報告**

有興趣的朋友可以追蹤一下。

![Google AI 付費方案比較表：免費版、Google AI Pro（每月 NT$660）、Google AI Ultra（前 3 個月每月 NT$4050）三種方案的功能差異](https://www.darrelltw.com/google-antigravity-ide/google_ai_pro_ultra_pricing_plan.png)

Google AI 付費方案比較表：免費版、Google AI Pro（每月 NT$660）、Google AI Ultra（前 3 個月每月 NT$4050）三種方案的功能差異

## 常見問題

❓ Antigravity IDE、Antigravity 2.0、Antigravity CLI 三個怎麼分？

三個是 **同一平台底下的三種使用介面** ，後端 agent harness 共用。  
  
**Antigravity IDE** ：原本的 VS Code fork IDE，2025/11 推出。看程式碼、改程式碼、跑 debugger 都在這。  
  
**Antigravity 2.0** ：2026/05 新推出的桌面 app。 **沒有編輯器** ，只有 chat 視窗，定位是管理多個 agent 的 Agent Workstation。  
  
**Antigravity CLI** ：2026/05 新推出的終端機工具（Go 寫），取代 Gemini CLI。比較對象是 Claude Code、Codex CLI。

❓ Gemini CLI 什麼時候停服？要怎麼遷移？

**2026/06/18** Gemini CLI 與 Gemini Code Assist IDE extensions 停止服務 Google AI Pro、Ultra、免費 Gemini Code Assist 用戶。要繼續用必須遷移到 **Antigravity CLI** 。  
  
例外：用 Gemini Code Assist **Standard / Enterprise 授權** ，或透過 Google Cloud 用 Gemini Code Assist for GitHub 的組織，不受影響。  
  
Antigravity CLI 保留 Gemini CLI 的核心功能：Agent Skills、Hooks、Subagents、Extensions（改名 plugins），遷移成本不算高。

❓ Antigravity 是否完全免費？

目前 Antigravity 處於早期階段，完全免費讓大家使用。不過實測發現經常會遇到 `model overloaded` 錯誤，影響開發體驗。配額每 5 小時重置一次，建議使用 Gemini 3 (Low) 模型來節省配額。未來 Google 可能會推出付費方案，預估個人版約 $10-20 美元/月。

❓ 可以使用 Claude Opus 4.5 嗎？

可以！Antigravity 現已支援 **Claude Opus 4.5 Thinking** ，包含 extended thinking 功能。你可以在 Gemini 3 Pro、Opus 4.5、Sonnet 4.5 之間自由切換，選擇最適合當前任務的模型。

❓ 與 Cursor、Claude Code 有什麼不同？

2026/05 平台化後對應關係更清楚：  
  
**Antigravity IDE vs Cursor**  
兩者使用體驗相似，現在模型支援也趨近一致（都支援 Opus 4.5）。Antigravity IDE 最大亮點是內建 Chrome 自動化測試功能，不需要額外安裝第三方工具。  
  
**Antigravity CLI vs Claude Code**  
都是終端機 agent，差別在後端模型（Gemini 3 vs Claude）、開發語言（Go vs Node.js）、計價方式（Google AI Pro/Ultra vs Anthropic API + 月費）。功能面 Skills、Hooks、Subagents 都有對齊。

❓ Agent Skills 可以跟 Claude Code 共用嗎？

可以！Agent Skills 是由 Anthropic 發布的 **開放標準** ，Claude Code 和 Antigravity 都支援同樣的 SKILL.md 格式。你可以用 `ln -s` 符號連結讓兩個工具共用同一套 Skills，只需維護一份就能兩邊使用。