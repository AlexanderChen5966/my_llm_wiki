---
title: "Hermes Agent 快速上手教學》內建自動學習、從 OpenClaw 搬家、Skill+常用指令表"
source: "https://www.blocktempo.com/nous-hermes-agent-self-improving-loop-openclaw-claude-pro-tutorial/"
author:
  - "[[Mickey帽鼠]]"
published: 2026-04-09
created: 2026-05-18
description: "這篇從零帶你跑起 Nous Research 剛出的 Hermes Agent（v0.8.0），用你已經訂閱的 Claude Pro/Max 當後臺，5 分鐘走完安裝到第一次對話。它內建學習迴圈，會自己寫筆記、累積 skills、跨 session 記住你是誰。本篇附上完整指令、WSL2 踩雷提醒、Claude auth 流程、8 題 FAQ。（前情提要：Anthropic 訂閱 Claude Code 封殺龍蝦 OpenClaw！往後第三方工具僅能付費額度）（背景補充：探討 AI 代理應用：垂直型 Agent 能打破賽道疲態的僵局嗎？）。 這篇要講的不是 Claude、不"
tags:
  - "clippings"
status: processed
---
這篇從零帶你跑起 Nous Research 剛出的 Hermes Agent（v0.8.0），用你已經訂閱的 Claude Pro/Max 當後臺，5 分鐘走完安裝到第一次對話。它內建學習迴圈，會自己寫筆記、累積 skills、跨 session 記住你是誰。本篇附上完整指令、WSL2 踩雷提醒、Claude auth 流程、8 題 FAQ。  
（前情提要： [Anthropic 訂閱 Claude Code 封殺龍蝦 OpenClaw！往後第三方工具僅能付費額度](https://www.blocktempo.com/anthropic-bans-openclaw-claude-subscription-oauth-extra-usage-api-key/) ）  
（背景補充： [探討 AI 代理應用：垂直型 Agent 能打破賽道疲態的僵局嗎？](https://www.blocktempo.com/explore-vertical-agents/) ）。

這篇要講的不是 Claude、不是 ChatGPT、也不是你熟悉的 OpenClaw，而是昨天（2026-04-08）才發布 v0.8.0 的 Hermes Agent。

**[《Hermes Agent 的 GitHub 在此點我》](https://github.com/NousResearch/hermes-agent/releases/tag/v2026.4.8)**

Nous Research 是開源 AI 研究團隊，過去以 Hermes 系列 LLM（Hermes 3、Hermes Mixtral）在圈內出名，這次他們交出首個產品級 agent framework，MIT 授權、GitHub 上累積 42.2k stars、5.4k forks、331 位貢獻者。

官方 slogan 只有一句話：「The agent that grows with you」。意思是這個代理會記住你、越用越懂你的工作習慣。

大多數 agent framework（包括早期的 OpenClaw）都是 stateless，每次對話完就忘光，下次從零開始。除非你讓它記在記錄裡。Hermes Agent 則會把「跨 session 記憶 + 自動技能累積 + 使用者建模」三件事全做成預設功能，目前的 agent framework 圈裡只有這一家這樣做。

如果你已經用 OpenClaw 用了一段時間，覺得處理不同事項過後都要重頭解釋脈絡、agent 好像學不會你的工作習慣，這篇值得你花 5 分鐘看完。

**本篇大綱：**

- Hermes Agent vs OpenClaw，學習迴圈就是結構性差異
- 一行指令搞定安裝，Windows 記得先裝 WSL2
- 串 Claude Pro/Max 最短路徑，已經有訂閱就直接吃
- 學習迴圈實戰看點，第一個 skill 怎麼被自動寫出來
- 8 題常見問題 FAQ

### Hermes Agent vs OpenClaw

先講結論：如果你只用 agent 做單次任務（例如「幫我整理這個資料夾」），OpenClaw 跟 Hermes 用起來的體感差不多。但如果你想要 agent 累積工作習慣、記住你的偏好、自己創造可重用的 skill，兩者就不是同一個東西了。

#### 架構層面的學習迴圈差異

OpenClaw 的運作模式是：接到任務、呼叫 LLM、執行工具、回報結果、結束。每次對話都是獨立 session，前一次學到的東西不會帶到下一次。

Hermes Agent 的運作多了 4 個階段：

1. **episodic memory 寫入** ：每次任務結束，agent 會寫一份結構化紀錄，記下用了哪些工具、每一步引數、哪些成功、哪些失敗、花了多久、遇到什麼錯誤
2. **retrieval 檢索** ：下次遇到類似任務，agent 先去過去的紀錄檢索相關經驗，再決定怎麼做
3. **skill 抽象** ：重複三次以上的成功模式會被抽象成 markdown 格式的 skill 檔案，遵循 [agentskills.io](https://agentskills.io/) 開放標準，存在你的 skills 資料夾
4. **Honcho 辯證建模** ：跨 session 建立「你是誰」的使用者模型，會記住你的工作風格、溝通偏好、拒絕過的選項

配上 FTS5 全文搜尋 + LLM 摘要，你可以問 agent「上週我跟你討論過什麼 API 設計決策？」，它真的找得到。

#### 平台、執行後端、模型支援的覆蓋差異

Hermes 的 messaging gateway 支援 8 個平台統一管理： **CLI、Telegram、Discord、Slack、WhatsApp、Signal、Email、Home Assistant。**

一個 agent instance 跑在你的 VPS 上，從任何裝置都能跟它對話。OpenClaw 主打 CLI 加少數 webhook 整合，覆蓋範圍明顯窄一截。

終端機執行後端 Hermes 給了 6 個： **local、Docker、SSH、Daytona、Singularity、Modal。**

後兩個是 serverless 環境，agent 閒置時冬眠、按需喚醒，跑整個月的成本可以壓到接近零。OpenClaw 預設在本地跑，隔離要你自己配 Docker。

模型支援面，Hermes 原生接 16+ provider： **Nous Portal、OpenRouter（200+ 模型）、Anthropic、OpenAI Codex、DeepSeek、Kimi、GLM、Qwen、Hugging Face（20+ 開源模型）、GitHub Copilot、Vercel AI Gateway、自訂 endpoint（VLLM、SGLang、Ollama 都算）** 。

切 provider 不需要改任何程式碼，打入指令 `hermes model` 重選就好。

#### 內建 OpenClaw 遷移工具

Hermes 內建 `hermes claw migrate` 指令，可以把 OpenClaw 的設定、skills、conversation history 一次搬過來。支援三個常用 flag：

- `--dry-run` ：預覽要搬什麼，不實際寫入
- `--preset user-data` ：搬使用者資料但不含機密（API key 類）
- `--overwrite` ：覆蓋現有衝突

這是 Nous Research 對 OpenClaw 使用者的明確示好，等於在說「你來，我幫你搬家」。

#### 誠實列優劣勢

**Hermes 現在還有的限制** ：

- Windows 不支援原生，必須透過 WSL2
- v0.8.0 是早期版本，v0.7.0 才 5 天前發布，bug 回報管道還在成型
- 學習迴圈要時間累積才會顯現價值，第一週可能感覺跟 OpenClaw 差不多
- Nous Portal 訂閱定價不透明，官方檔案只講 OAuth 流程，沒公開費率
- 檔案以英文為主，中文社群剛起步

**OpenClaw 相對優勢** ：

- 社群更大，第三方 skill hub（ClawHub）生態更完整
- 歷經 Anthropic 封殺事件後依然活躍，抗壓測試透過
- 檔案更成熟，常見錯誤的解法已經被社群驗證過千遍

短期內 Hermes 比較適合有實驗精神的重度使用者，OpenClaw 比較適合需要生產環境穩定的團隊。但半年後，如果學習迴圈真的像 Nous 講的那樣越用越聰明，這個判斷可能會反過來。

### 一行指令搞定安裝，Windows 記得先裝 WSL2

Hermes Agent 的安裝是目前 agent framework 裡最短的路徑。

官方 one-liner 會幫你搞定 Python 3.11 venv、Node.js、ripgrep、ffmpeg、repo clone、全域 hermes 指令、LLM provider 設定這一整串，不用你逐項裝。

Linux、macOS、WSL2 都適用，直接跑這行：

```js
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

跑完後重新載入 shell：

```js
source ~/.bashrc  # 或 source ~/.zshrc
```

然後輸入 `hermes` 就可以進入互動模式。

#### Windows 使用者注意

Hermes 目前 **沒有原生 Windows 支援** ，必須先裝 WSL2，在 WSL2 終端機裡跑上面的指令。這是已知限制，官方檔案明確標註。

如果你從來沒裝過 WSL2，流程大致是：系統管理員許可權開 PowerShell，跑 `wsl --install` ，重開機，從 Microsoft Store 裝 Ubuntu，進 Ubuntu 之後再跑 Hermes 的安裝指令碼。整套走完約 15 到 20 分鐘。

#### 開發者手動安裝選讀

如果你想貢獻 PR 或改 Hermes 本體程式碼，官方推薦走手動路徑：

```js
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
curl -LsSf https://astral.sh/uv/install.sh | sh
uv venv venv --python 3.11
source venv/bin/activate
uv pip install -e ".[all,dev]"
python -m pytest tests/ -q
```

這一步最容易卡的是 `--recurse-submodules` 。這個 flag 不能省，Hermes 有依賴 tinker-atropos 子模組（Nous 自己的 RL 訓練環境），漏了這個 flag 後面的 `uv pip install` 會噴一堆 import error，而且報錯訊息不會直接指向子模組問題，容易多繞半小時才找到原因。

#### 安裝後自檢

裝完之後，直接用 `hermes doctor` 做一次環境診斷：

```js
hermes doctor
```

這個指令會檢查 Python 版本、Node.js、依賴套件、LLM provider 設定、檔案許可權。第一次跑看到紅字是正常的（LLM 還沒設定），下一步就是去設定 provider。

### 串 Claude Pro/Max 最短路徑，已經有訂閱就直接吃

開始用之前的重點來了，如果你已經訂閱 Claude Pro 或 Claude Max（動區讀者不少人都是），Hermes 原生支援 Claude Code 的 OAuth 認證流程，你的訂閱額度可以直接餵給 Hermes，不用另外去 console.anthropic.com 開 API key 付兩份錢。

走這個流程：

```js
hermes model
```

互動式選單會依序問你三個問題：

1. **選 provider** ：從清單選「Anthropic」
2. **選認證方式** ：有三個選項，Claude Code OAuth（推薦，吃訂閱額度）、API key（另外付 API 費用）、Claude Pro 的 device code auth
3. **選模型** ： `claude-opus-4-6` （最聰明、最貴）、 `claude-sonnet-4-6` （平衡）、 `claude-haiku-4-5` （快且便宜）

選完 Claude Code OAuth 後會跳出瀏覽器視窗，用你 Claude.ai 的帳號登入一次，Hermes 就拿到 token 了。整段流程 1 分鐘內結束。

#### 第一次對話

跑 `hermes` 進入互動模式，會看到 welcome banner 顯示目前的模型、可用工具、已載入的 skills。打個試試看：

```js
❯ 你能幫我做什麼？
```

或者直接給它一個真正的任務：

```js
❯ 我的硬碟空間還剩多少？有哪些我很久沒用的大型檔案？
```

Hermes 會自動呼叫終端機工具、跑 `du -sh` 類指令、把結果解析成白話說明。

#### 快捷鍵速查

- `Alt+Enter` 或 `Ctrl+J` ：多行輸入
- `Ctrl+C` ：中斷目前執行的任務
- `Ctrl+B` ：開啟語音錄音（需要先 `pip install "hermes-agent[voice]"` ）
- `/help` ：列所有斜槓指令
- `/tools` ：看這個 session 載入哪些工具
- `/model` ：切換模型重跑
- `/save` ：存對話
- `/personality pirate` ：試試海盜人格，純好玩

要恢復上次沒聊完的對話，直接 `hermes --continue` 或簡短形式 `hermes -c` 。

### 學習迴圈實戰，第一個 skill 怎麼被自動寫出來

Hermes 的學習迴圈不是行銷話術，而是檔案裡可以翻到的設計。你第一次交給它一個多步驟任務（例如「整理某個資料夾、抽出所有 PDF 的摘要、存成 JSON」），agent 跑完之後會在 `~/.hermes/episodic/` 寫下一份紀錄，包括用了哪些工具、每一步的引數、哪些成功、哪些失敗。

下次你再給它類似任務，agent 會先去 `~/.hermes/episodic/` 檢索，找出上次成功的工具鏈、直接套用。如果你連續三次都用類似流程，Hermes 會把它抽象成一個 skill，存到 `~/.hermes/skills/` 下，檔案格式是 markdown（遵循 [agentskills.io](https://agentskills.io/) 開放標準）。

之後你只要下一句「整理資料夾出 PDF 摘要」，agent 會自動載入那個 skill，省掉前面的 planning 時間。

#### agentskills.io 開放標準

agentskills.io 是 Nous Research 推動的 skill 格式標準，把 agent 的可重用能力做成 portable 的 markdown 檔。你可以把自己累積的 skills 分享出來，也可以從官方 Skills Hub 裝別人寫好的：

```js
hermes skills search kubernetes
hermes skills install openai/skills/k8s
```

這是 Hermes 跟 OpenClaw 的另一個分歧點。OpenClaw 的 skill 系統是封閉的、綁在 ClawHub 上，你換 agent 框架 skill 就廢了。Hermes 想要靠著開放標準，任何符合 agentskills.io 規範的 skill 都能跨框架用。

#### Honcho 使用者建模

除了工作流 skill，Hermes 還跑一條叫 Honcho 的使用者建模流程，跨 session 分析你的對話風格、拒絕過的選項、強調過的偏好，建立一個「agent 理解的你是誰」模型。

Nous 把這稱為「辯證建模」（dialectic user modeling），不是單向的使用者畫像，而是 agent 根據你的回饋持續修正自己的理解。

這部分的效果需要時間累積，第一週你不會感受到差別，第一個月以後會開始發現 agent 越來越不需要你重複解釋脈絡。想看 Honcho 的原始設計可以翻 Hermes 的 developer-guide 檔案。

### 常見問題 FAQ

**Q1. Windows 可以裝嗎？**  
A：原生不支援，必須先裝 WSL2，在 WSL2 終端機裡跑安裝指令碼。官方檔案明確標註此限制。

**Q2. Claude Pro 訂閱能直接用嗎？**  
A：可以。 `hermes model` 選 Anthropic 後選 Claude Code OAuth，用 Claude.ai 帳號登入一次，Hermes 就能吃你的訂閱額度，不用另開 API key。

**Q3. 跟 OpenClaw 最大差別是什麼？**  
A：Hermes 內建學習迴圈（episodic memory、自動 skill 抽象、Honcho 建模），OpenClaw 是 stateless；另外 Hermes 支援 8 個 messaging 平台統一 gateway，OpenClaw 主打 CLI。

**Q4. 可以從 OpenClaw 搬家嗎？**  
A：可以。 `hermes claw migrate --dry-run` 先預覽要搬什麼，確認沒問題再跑 `hermes claw migrate` 。支援 `--preset user-data` （不含機密）和 `--overwrite` （覆蓋衝突）。

**Q5. 記憶體跟 skills 存在哪？**  
A：episodic memory 存 `~/.hermes/episodic/` ，自動產生的 skills 存 `~/.hermes/skills/` ，設定檔在 `~/.hermes/config.yaml` 。

**Q6. 要花錢嗎？**  
A：Hermes 本身 MIT 開源免費。LLM 呼叫要錢，但如果你走 Claude Pro/Max OAuth 認證，是吃你的訂閱額度不是另外付 API。Nous Portal 是訂閱制，定價要到 [portal.nousresearch.com](https://portal.nousresearch.com/) 查。

**Q7. 語音模式怎麼啟用？**  
A：先裝語音依賴 `pip install "hermes-agent[voice]"` ，進入 hermes 互動模式後輸入 `/voice on` ，或按 `Ctrl+B` 錄音。想要本地 STT 可以再裝 `faster-whisper` 。

**Q8. 安裝跑到一半出現錯誤** **怎麼辦？**  
A： `hermes doctor` 會診斷大部分環境問題（Python 版本、依賴缺失、許可權錯誤）。如果是 tinker-atropos 子模組的 import error，回頭 clone 時記得加 `--recurse-submodules` 。其他問題到 [GitHub Issues](https://github.com/NousResearch/hermes-agent/issues) 或 [Nous Research Discord](https://discord.gg/NousResearch) 發問。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

##### 📍相關報導📍

**[Claude Code 邊做邊學：這免費網站用 11 堂課教你上手，免安裝直接練](https://www.blocktempo.com/learn-claude-code-interactive-browser-terminal-simulator-free-no-install/)**

**[慢霧專欄：資金交給「龍蝦」 AI Agent 真的安全？聯合 Bitget 報告揭露五大風險](https://www.blocktempo.com/ai-agent-web3-security-risks-slowmist-bitget-prompt-injection-skill/)**

**[頂級 AI 模型走向分化：ChatGPT to C，Claude to B](https://www.blocktempo.com/top-ai-models-divergence-gpt-to-c-claude-to-b/)**

### 最新文章

---

[![Tether 發布 AI 開源套件「QVAC SDK」，主打本地運行終結雲端壟斷](https://image.blocktempo.com/2026/04/QVAC-SDK-Blog-Post-1140x815.webp "QVAC-SDK-Blog-Post | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/04/QVAC-SDK-Blog-Post.webp)

據 Tether 官方於 4 月 9 日發布的最新公告，該公司旗下數據與 AI 團隊正式推出了完全開源的跨平台軟體開發套件「QVAC SDK」。Tether 旨在將 QVAC 打造成通用的 AI 基礎構建塊，讓 AI 模型能直接在手機、電腦等本地設備上運行，徹底擺脫對中心化雲端伺服器的依賴，宣告「穩定智慧時代（Stable Intelligence Era）」的到來。  
（前情提要： [Tether 預告正開發去中心化搜尋引擎「Hypersearch」，採 DHT 技術、挑戰 Google 霸權？](https://www.blocktempo.com/tether-ceo-paolo-ardoino-teases-decentralized-search-engine-hypersearch/) ）  
（背景補充： [Tether 暫停200億美元融資，聘請「四大會計事務所」全面審計，USDT 透明度迎終局之戰](https://www.blocktempo.com/tether-signs-big-four-firm-for-first-full-audit-usdt/) ）

穩定幣發行商龍頭 Tether 正加速剝離純粹的金融標籤，全面進軍人工智慧（AI）與去中心化基礎設施領域。根據 2026 年 4 月 9 日的 [官方公告](https://tether.io/news/tether-launches-qvac-sdk-as-the-ai-universal-building-block-that-runs-trains-and-evolves-intelligence-across-any-device-and-platform/) ，Tether 旗下的 QVAC 團隊正式發表了 **QVAC SDK** ，這是一款完全開源、跨平台的軟體開發套件，旨在成為未來 AI 世界的「通用基礎構建塊」。

Tether 對未來的願景極為宏大：在不久的將來，100 億人類將與 100 億台自主機器以及上兆個 AI 代理（Agents）共享這個星球，而 Tether 將這個新紀元命名為「穩定智慧時代（Stable Intelligence Era）」。

### 執行長開砲：中心化伺服器 AI 是「死胡同」

針對目前由微軟、OpenAI、Google 等科技巨頭主導的雲端 API 模式，Tether 執行長 Paolo Ardoino 給出了極度犀利的評價。他直言：

> 「目前將每一個決策都透過中心化伺服器路由的模式，根本無法擴展以應對未來的現實。」

Ardoino 強調，單憑物理定律，中心化 AI 就註定是一條死胡同：光速帶來的延遲、單點故障的脆弱性，以及控制權的高度集中，這些都是為「更小的世界」所設計的系統缺陷。他表示，QVAC 正是為即將到來的世界所打造的基石。

### 100% 本地端運行，斷網也能用

QVAC SDK 的核心理念是「本地優先（Local-first）」。它讓開發者能將大型語言模型（LLM）及其他 AI 應用直接部署在消費者的設備上，無論是智慧型手機、筆記型電腦、工業級伺服器，甚至是一顆智慧燈泡中的微小晶片。

這項技術突破帶來了兩大優勢：

- **極致隱私與韌性：** 用戶的日常 AI 功能（如寫作輔助、語音轉錄、圖像生成、個人財務規劃等）皆在設備本地瞬間完成，敏感數據不需上傳至雲端。即使網路斷線或遠端伺服器當機，AI 依然能正常運作。
- **跨平台一致性：** 開發者只需編寫一次代碼，即可在 iOS、Android、Windows、macOS 和 Linux 上無縫運行，無需針對不同平台進行重寫。

### 底層架構：結合開源引擎與 P2P 網路

在技術底層方面，QVAC SDK 建立在統一的抽象層上。其核心「QVAC Fabric」是知名開源項目 **llama.cpp** 的分叉版本，支援廣泛的文本生成與多模態工作負載；同時也整合了 whisper.cpp、Parakeet（語音轉文字）與 Bergamot（設備端翻譯）等頂尖本地引擎。

更具「加密原生」色彩的是，QVAC 深度整合了 Tether 支持的 P2P 網路技術堆棧 **Holepunch** 。SDK 內建了去中心化模型分發、無中心化基礎設施的委託推理功能，並預告未來將推出用於去中心化訓練與微調的「P2P 節點群（P2P swarms）」，讓本地 AI 應用具備極強的抗審查與韌性。

Tether 承諾，未來數月與數年內將投入大量資源，擴展 QVAC 的開源生態系，甚至將支援機器人技術與腦機介面（Brain-computer interfaces）的專屬工具包，展現了其重塑未來運算架構的強烈野心。

![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")

## 📍相關報導📍

**[Tether 推出全球首個手機端「十億級 AI 訓練框架」，iPhone、Samsung 都能跑 LoRA](https://www.blocktempo.com/tether-qvac-launches-worlds-first-cross-platform-bitnet-lora-framework/)**

**[Tether 2025 年 Q4 報告：USDT 市值近 1900 億，多項指標創下新高](https://www.blocktempo.com/tether-usdt-q4-2025-record-breaking-report/)**

**[Tether發布開源比特幣挖礦系統MiningOS，免費無後門、無第三方依賴](https://www.blocktempo.com/tether-releases-open-source-miningos-for-bitcoin-mining-no-backdoor-no-deps/)**