---
title: "OpenClaw 跟 Hermes Agent 該安裝哪一個？AI代理入門、直接上手實戰教學"
source: "https://www.blocktempo.com/openclaw-hermes-agent-comparison-installation-three-scenarios-developer-content-onchain/"
author:
  - "[[Mickey帽鼠]]"
published: 2026-04-14
created: 2026-05-18
description: "想嘗試紅到全球的 OpenClaw 跟 Hermes Agent 嗎？這兩套開源 AI Agent 的差異在哪裡？該怎麼安裝？附上實戰教學讓你快速理解、實測。（前情提要：Hermes Agent 快速上手教學》內建自動學習、從 OpenClaw 搬家、Skill+常用指令表）（背景補充：讓你產能百倍的 Hermes Agent 工具，把龍蝦 OpenClaw 打回寒武紀） 兩套都號稱要把「個人 AI Agent」從玩具變生產力工具，一套主打 24 個訊息平台跟 13,000 多個社群 skill，另一套主打讓 agent 越用越懂你。這篇想讓你快速理解這兩套工具的邏輯、怎麼安裝、怎麼"
tags:
  - "clippings"
status: processed
---
想嘗試紅到全球的 OpenClaw 跟 Hermes Agent 嗎？這兩套開源 AI Agent 的差異在哪裡？該怎麼安裝？附上實戰教學讓你快速理解、實測。  
（前情提要： [Hermes Agent 快速上手教學》內建自動學習、從 OpenClaw 搬家、Skill+常用指令表](https://www.blocktempo.com/nous-hermes-agent-self-improving-loop-openclaw-claude-pro-tutorial/) ）  
（背景補充： [讓你產能百倍的 Hermes Agent 工具，把龍蝦 OpenClaw 打回寒武紀](https://www.blocktempo.com/hermes-agent-nous-research-openclaw-migration-self-evolution-tools/) ）

兩套都號稱要把「個人 AI Agent」從玩具變生產力工具，一套主打 24 個訊息平台跟 13,000 多個社群 skill，另一套主打讓 agent 越用越懂你。這篇想讓你快速理解這兩套工具的邏輯、怎麼安裝、怎麼樣才不會踩雷，還有最重要的，你的系統該選哪一套？

這份指南預設你會用 terminal、聽過 pip 跟 git，但沒玩過 agent framework（代理框架）。讀完你會有兩套都能跑起來的本機環境，跟一張清楚的決策樹。

### 本篇大綱

- OpenClaw 是什麼，從 ClawdBot 到 247K stars 的兩個月旅程
- Hermes Agent 是什麼，Nous Research 的「自我學習」野心
- 安裝 OpenClaw（macOS / Windows + 三個踩雷）
- 安裝 Hermes Agent（macOS / Windows + 三個踩雷）
- 五維對比表：哲學、平台、生態、安全、成本
- 三場景實戰：開發輔助、內容產製、鏈上 DeFi
- 怎麼選（三條決策樹收尾）
- 附錄：Docker 一鍵跑、Tools / Skills 推薦各 8 個、官方資源
- 常見問題 10 題

### OpenClaw 是什麼？

OpenClaw （龍蝦）是奧地利開發者 Peter Steinberger 在 2025 年 11 月發了一個叫 ClawdBot 的東西，幾個月後改名 MoltBot（被 Anthropic 商標投訴），三天後再改名 OpenClaw，定下來。到了今年 3 月，這個 repo 在 GitHub 上有 247,000 個 star、47,700 個 fork，兩個月內爆紅。

簡單說龍蝦的技術定位，一個 long-running 的 Node.js 服務，把自己定位成「local-first Gateway（本機優先閘道）」。意思是它跑在你自己的機器上，把 LLM、訊息平台、外部工具全部接到一個單一控制平面。

核心特色我整理出三條：

1. **廣度先於深度** ：支援 24 個以上的訊息平台。WhatsApp、Telegram、Discord、Slack、Signal、iMessage（含 BlueBubbles 跟 legacy 兩種接法）、IRC、Microsoft Teams、Matrix、Feishu、LINE、Mattermost、Nextcloud Talk、Nostr、Synology Chat、Tlon、Twitch、Zalo、WeChat、WebChat 全部都接。  
	我自己經驗是，要把 agent 同時接 Telegram + Slack + LINE，OpenClaw 是現在唯一能一次到位的
2. **LLM 多模型相容** ：Claude 4、GPT-4o、Gemini 2.0、DeepSeek V3 都行，不必鎖定特定模型供應商
3. **SOUL.md 定義人格** ：每個 OpenClaw agent 都有一個 SOUL.md 檔，定義它的個性、價值觀、禁止事項、approval gate（核可閘門）。例如「未經我同意絕對不發 email」「重要動作前先 ping 我」這種規則寫在 SOUL.md 裡，agent 會嚴格執行

社群生態叫 [**ClawHub**](https://clawhub.ai/) ，是官方的 skill registry（技能註冊中心）。 有超過 1 萬個 community skill 可以自由下載，從 Notion 整合、GitHub PR review、Web automation 到鏈上交易都有。

#### 風險必知

OpenClaw 不是沒有危險，今年 3 月在 4 天內爆出 9 個 CVE（漏洞編號），其中一個 CVSS 評分 9.9（接近滿分 10.0，代表「critical」嚴重級別）。

獨立安全研究者掃出 135,000 多個暴露在公網 IP 的 instance，分布 82 個國家。同時 ClawHub 上被驗證為惡意的 skill 有 341 個，另有研究發現 7.1% 的 skill 處理 secret（API key、信用卡資訊等）的取得方式不當。

這些不是要嚇你不裝，而是裝之前要知道你在裝什麼。安全章節我下面會講怎麼降低風險。

### Hermes Agent 是什麼？Nous Research 的「自我學習」工具

Hermes Agent 出身比較硬，Nous Research 是出 Hermes 系列開源 LLM 模型的研究團隊，2026 年 2 月 25 日把 hermes-agent 推到 v0.1.0，最新版本 v2026.4.8，兩個月內從 0 衝到 61,000 多個 GitHub star。

它的定位是「the agent that grows with you」（陪你一起長大的 agent）。聽起來像行銷詞彙，但 Hermes 真的是現在唯一一個原始內建 learning loop（學習迴圈）的開源 agent framework。

learning loop 怎麼運作？每次你給 Hermes 跑一個複雜任務，它做完之後會自己抽出有用的 pattern（模式），寫成一個新的 skill 檔，存到 \`~/.hermes/skills/\`。

下次再遇到類似問題，它會去 search 自己的 skill 庫，找到先前自己寫的解法直接用。換句話說，agent 真的會自己改寫自己的腦袋，越用越懂你的工作方式。

核心特色有：

1. **built-in learning loop** ：上面講的，自寫 skill、自我最佳化
2. **multi-level memory** ：三層記憶系統。session memory（單次對話）、persistent memory（跨 session 的長期記憶）、skill memory（自寫 skill）。全文檢索 + LLM 摘要，可以撈幾個月前的 context 而不會爆 context window
3. **LLM 廣度第一** ：支援 Nous Portal、OpenRouter（一個 endpoint 接 200+ 模型）、Xiaomi MiMo、z.ai/GLM、Kimi/Moonshot、MiniMax、Hugging Face、OpenAI、或你自己跑的 Ollama / vLLM endpoint
4. **MCP 雙向** ：Hermes 既能當 MCP（Model Context Protocol，模型上下文協定）client，去接外部 MCP server；自己也能當 MCP server，讓 Claude Code、Cursor、Codex 等其他 agent 透過它收發 Telegram / Discord 訊息
5. **cron 排程 + multi-instance profile** ：內建排程系統，可以設「每天早上 9 點跑 X」；profile 機制讓你一臺機器跑多個 hermes instance，例如家用一個、工作一個

原生支援的訊息平台比 OpenClaw 少很多：Telegram、Discord、Slack、WhatsApp、Signal、Feishu / Lark、WeCom、CLI

#### 有哪些弱點？

Hermes Agent 三個常被詬病的點：

1. **還很年輕** ：6 個 release 對上 OpenClaw 的 82 個 release，有些人會質疑釋出時間太短，官方沒立場宣稱穩定。reddit 上有人直接寫「Hermes 才出 6 版，別跟我講它比較穩，根本沒夠長時間驗證」
2. **token 成本** ：73% 的 API call 是固定 overhead，tool definitions 吃掉接近一半。透過 CLI 跑一次 6-8k input token，透過 Telegram / Discord gateway 跑會吃 15-20k input token，差 2-3 倍。如果你沒有特別設計省 Token，長期成本差距會很大
3. **Tirith 安全模組嚴格** ：Tirith 是 Hermes 內建的安全層，但它會 hard block（直接擋）\`curl | sh\` 這類一行下載指令碼，沒有 approval prompt（核准提示）給你選，有些人用起來覺得很擋路

### 如何安裝 OpenClaw（macOS + Windows + 三個踩雷點）

兩種 OS 是最多人使用的，先告訴你怎麼安裝

#### macOS 一行裝完

macOS 上最快的方式：

```js
curl -fsSL https://openclaw.ai/install.sh | bash
```

這個指令碼會幫你檢查 Node.js 版本（要 ≥ 22，推薦 24 或 22.14+），缺的話自動裝，然後啟動 onboarding wizard（上手精靈）。Wizard 會問你三件事：

1. 選 LLM provider（Anthropic / OpenAI / Google / DeepSeek …）
2. 要接哪些訊息平台（每個都要授權，可以晚點補）
3. API key 怎麼存（建議走 keychain，不要寫進 SOUL.md）

如果你已經有 Node.js，可以走 npm 路線：

```js
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

\`–install-daemon\` 注意！這是關鍵 flag。它會把 OpenClaw 註冊成 launchd（macOS 的服務管理）或 systemd（Linux 的服務管理）的背景服務，你重開機之後 agent 還會自動跑起來，不用手動 restart。

裝完驗證：

```js
node -v        # 確認 ≥ 22
openclaw -v    # 看 OpenClaw 版本
openclaw status # 看 daemon 狀態
```

#### Windows 安裝的兩種方式

OpenClaw 在 Windows 上有兩條路：

1. **WSL2（強烈建議）** ：開 PowerShell（管理員）跑 `wsl --install` ，重開機進 Ubuntu 之後跑 macOS 那段一行安裝。優點是穩定、相容性最好
2. **原生 Windows** ：官方有 `install.ps1` ，可以直接在 PowerShell 跑。但 Windows 防毒軟體（含 Defender）常會誤判 daemon 行為，把 OpenClaw 的 background service 當成可疑活動擋掉。我自己經驗是 WSL2 比較省事

#### 三個踩雷

我自己撞過這三個，依嚴重度排名是

1. **Node.js 版本不對** ：Node 22 以下會直接噴 `UnsupportedNodeVersionError` 。Node 22.0 到 22.13 之間有 known bug（已知問題），會在啟動時隨機掛掉。最穩的版本是 22.14+ 或 24。\`node -v\` 確認，不對就用 nvm 切換： `nvm install 24 && nvm use 24`
2. **PATH 沒刷新** ：裝完打 `openclaw` 找不到指令，這通常是 PATH 還沒過載。重開 terminal，或 `source ~/.zshrc` （macOS）/ `source ~/.bashrc` （Linux）就好
3. **Daemon 沒起來** ：跑 `openclaw status` 顯示 inactive，要手動 load。macOS： `launchctl load ~/Library/LaunchAgents/ai.openclaw.daemon.plist` ；Linux： `systemctl --user start openclaw`

裝完 SOUL.md 在哪？預設 `~/.openclaw/soul.md` 。你可以直接編輯定義 agent 個性，加 approval gate。我建議第一步就先寫兩條防呆指令：「未經我同意絕對不寄出任何 email」「任何 swap 或鏈上交易超過 100 USDC 先 ping 我」（這後面測試你可能會用到，可以自己修改數字）。

第一個 skill 該裝什麼？

我自己經驗是先裝 Notion skill 跟 Telegram skill 這兩個。Notion 用來存 agent 整理過的筆記，Telegram 用來收 agent 主動回報的訊息。兩個都接好之後，你會看到 agent 就開始做事了，讓你感覺「又裝了一個跑不起來的工具」。通常這個時候會決定你會不會繼續用下去。

我看過太多人裝完 OpenClaw 之後沒接任何 channel，跑兩天就放棄了，很可惜。

### 安裝 Hermes Agent（macOS + Windows + 地雷點）

Hermes 比 OpenClaw 硬一點，我們會常見到手動安裝。

#### macOS 一行（如果你的防火牆讓你過）

```js
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

這個指令碼會幫你裝 Python 3.11、uv（Python 套件管理工具）、Node.js、ripgrep、ffmpeg，clone repo、建 venv、設 LLM provider，全套搞定。我自己經驗是，這個 one-line 偶爾會被 macOS 的 Application Firewall 或某些防毒軟體擋掉，如果你被擋了就改用手動安裝。

#### macOS 手動裝（推薦學習用）

手動裝安會讓你看到 Hermes 的每個依賴怎麼做，理解它在做什麼，是學習用的好路線：

```js
# 第一步：裝 uv（Python 套件管理）
curl -LsSf https://astral.sh/uv/install.sh | sh

# 第二步：clone repo（必須加 --recurse-submodules）
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent

# 第三步：建 Python 3.11 venv
uv venv venv --python 3.11
export VIRTUAL_ENV="$(pwd)/venv"

# 第四步：裝所有 dependencies
uv pip install -e ".[all]"
uv pip install -e "./tinker-atropos"
npm install   # 跑瀏覽器工具跟 WhatsApp 用
```

裝完還沒結束，要設定（這段最容易卡）：

```js
# 建設定資料夾
mkdir -p ~/.hermes/{cron,sessions,logs,memories,skills,pairing,hooks,image_cache,audio_cache,whatsapp/session}

# 複製範例設定
cp cli-config.yaml.example ~/.hermes/config.yaml

# 設 API key
touch ~/.hermes/.env
echo 'OPENROUTER_API_KEY=sk-or-v1-<YOUR_KEY>' >> ~/.hermes/.env

# 把 hermes 指令 link 到 ~/.local/bin（讓它變 global）
mkdir -p ~/.local/bin
ln -sf "$(pwd)/venv/bin/hermes" ~/.local/bin/hermes

# 驗證安裝
hermes doctor
```

`hermes doctor` 是 Hermes 最好用的指令之一。它會掃所有依賴、設定檔、API key 狀態，缺什麼會明確告訴你怎麼補。第一次跑可以順便：

```js
hermes setup    # 全套 wizard
hermes model    # 選 LLM provider
hermes tools    # 選要啟用哪些工具
```

#### Windows 必須 WSL2

🔴 Hermes Agent 原生 Windows **官方明確不支援** 。雖然 repo 裡有 `scripts/install.ps1` （用 `$env:LOCALAPPDATA\hermes` 存設定），但官方 README 明說「請裝 WSL2 跑」。

1. PowerShell 管理員執行 `wsl --install`
2. 重開機進 Ubuntu，跑上面 macOS 手動那段
3. WSL2 裡跑的 Hermes 可以從 Windows 主機透過 `\\wsl$\Ubuntu\home\<user>\.hermes` 路徑存取設定檔

#### 三個踩雷點

1. **Tirith 安全模組會 hard block `curl | sh`** ：如果你裝完想跑某些一行下載指令碼（例如裝其他 tool、抓 model 權重），會被 Tirith 直接擋掉，沒有 approval prompt。解法是手動 wget 下來、檢查內容、再跑
2. **絕對要注意 token 成本很爆** ：透過 Telegram / Discord gateway 跑 Hermes，每次請求都會吃 15-20k input token，比 CLI（6-8k）多 2-3 倍。長期成本差很多，建議重要任務、長對話走 CLI，臨時通知再走 gateway
3. **`uv` 沒進 PATH** ：第一步 `curl -LsSf https://astral.sh/uv/install.sh | sh` 跑完，要 `source ~/.bashrc` 或 `source ~/.zshrc` 才能用 uv 指令。新開的 terminal 自動載，當下那個 session 不會

裝完第一件事我建議先跑一次 `hermes setup` 走完整 wizard，把 LLM provider 設好，跑通 `hermes doctor` 。

然後丟一個簡單測試： `hermes "幫我寫一個 hello world Python 程式"` 。看到 agent 真的回應之後，再進階去裝 MCP server 跟 cron job。

新手最常犯的錯是一裝完就想設 cron 自動排程，結果 agent 沒測過就跑壞了，很挫折。建議先讓 agent 跟你互動個五次，建立信任感，再開始慢慢往自動化堆上去。

### OpenClaw vs. Hermes Agent 對比

OpenClaw 跟 Hermes Agent 最常見的誤解是「兩個都是 AI Agent 框架嘛，差不了多少」。其實差很多，差別不在預設功能，差在設計邏輯。

- **OpenClaw 是「嚴守說明書」的助手** ：你寫 SOUL.md 定義它能做什麼、不能做什麼、什麼情境用什麼工具。它執行得很穩，但每次任務都是新問題，不會累積經驗
- **Hermes Agent 是「自己改寫說明書」的助手** ：每跑完一次任務，它把學到的 pattern 寫成 skill 檔，下次自動 search 自己的 skill 庫，會感覺越用越懂你，但是就是很吃 token 和模型思考時間

這些差別決定了所有其他大部分的差異：

| 維度 | OpenClaw | Hermes Agent |
| --- | --- | --- |
| 設計哲學 | 嚴守說明書，執行為主 | 自寫說明書，學習為主 |
| 訊息平台 | 24+（最廣的選項） | 8（Telegram、Discord、Slack、WhatsApp、Signal、Feishu、WeCom、CLI） |
| Skill 生態 | ClawHub 13,729 個（其中 341 個被驗證為惡意） | awesome-hermes-agent + wondelai/skills（380+），多數要自己寫 |
| 學習能力 | 無，每次任務都當新問題 | built-in learning loop + multi-level memory |
| 安全紀錄 | 9 個 CVE / 4 天內爆出 / 1 個 CVSS 9.9 / 135K 暴露 instance | 0 agent-specific CVE |
| token 成本 | 標準 | CLI 6-8k input / Discord 15-20k input（gateway 模式 2-3 倍） |
| 安裝門檻 | Node.js ≥ 22 | Python 3.11 + uv + Node.js（Windows 必須 WSL2） |
| release 數 | 82 個 | 6 個 |
| 商用授權 | MIT | MIT |

我自己經驗是這張表只能讓你看出「差異點」，要選哪個可以看下去。

### 三種實戰測試：開發輔助、內容產製、鏈上 DeFi

下面我各寫出一種示範，最後給你選哪個的建議。

#### 場景一：開發輔助

OpenClaw 路線：裝 GitHub skill + Web automation skill，把 Slack channel 接進來。SOUL.md 寫一段：

```js
## 規則
- PR 超過 500 行先 ping 我，不要自己 review
- 任何 push 到 main 都要先得到我的明確確認
- code review comment 用繁體中文回
```

agent 收到 PR notification 會自動跑 lint、寫 review comment、push 到對應的 PR thread。

Hermes Agent 路線：把 GitHub MCP server 加進 `~/.hermes/config.yaml` ：

```js
mcp_servers:
  github:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
```

設 cron 每天早上 9 點自動跑 lint + 寫 daily summary：

```js
hermes cron add --name daily-lint --schedule "0 9 * * *" \
  --task "review yesterday's commits, run lint, write summary to ~/notes/daily.md"
```

Hermes 的 learning loop 會記住「這個專案的 lint config、常見錯誤、命名規則」，跑兩週之後自動套用，不需要每次都教它。

我自己經驗是開發場景兩個都能用，但有微妙差異。 **OpenClaw 的 SOUL.md gate 比較好寫「禁止事項」** （例如「絕對不能 push 到 main」）， **Hermes 的 learning loop 比較好做「持續最佳化」** （agent 會學會你的 commit message 格式跟 PR 描述風格）。短期上手 OpenClaw，長期養 agent 我會覺得用 Hermes 更快迭代你要 build 的東西。

#### 場景二：內容產製

OpenClaw 路線：Notion skill + Telegram channel + WordPress skill。流程是這樣：你在 Telegram 丟一個 URL 給 agent，agent 抓內容、整理成草稿、存到 Notion 對應的 database，排版好之後上 WordPress 草稿狀態，回 Telegram 報告 post\_id。Notion skill 是 ClawHub 用量第一名的 skill，整合度很完整。

Hermes Agent 路線：用 cron 每天抓 RSS / Twitter feed，memory 記住寫過的主題避免重複，skill memory 學你的寫作風格（句長、用詞、結構），跨 session 持續最佳化。設定大概長這樣：

```js
hermes cron add --name daily-news --schedule "0 8 * * *" \
  --task "fetch latest from feeds in ~/feeds.yaml, summarize new items, draft 3 article angles"
```

哪個比較合？我自己經驗是：

- **一次性轉文章（A 寫成 B）** → OpenClaw ，它的流程是線性、結果可以預測
- **長期經營主題（同一個專欄寫一年）** → Hermes。learning loop 會學你的風格，或是說你想要的風格

#### 場景三：鏈上 DeFi 自動化

這個場景是兩套差異最小的，因為 CoinFello 在 2026 年 3 月推出的 OpenClaw skill 是 MIT 開源、明確標示「相容 OpenClaw、Claude Code、Hermes Agent」，就是說這個 skill 兩邊都能用。

CoinFello skill 怎麼運作：

- 用 ERC-4337 smart accounts（智慧帳戶標準）+ ERC-7710 delegation（委派標準）+ MetaMask Smart Accounts Kit
- agent 不直接拿你的 private key，而是請求一個「窄定義授權」（例如「最多 swap 100 USDC、只能在 Uniswap V4」）
- 在授權範圍內，agent 可以做：ERC-20 swap、跨鏈 bridge、NFT 互動、staking、lending、portfolio rebalance、multi-step trading strategy

OpenClaw 路線實戰：SOUL.md 加 approval gate「任何 swap 超過 1000 USDC 先問我」，搭 ClawHub 上的鏈上監控 skill（鯨魚錢包追蹤、gas 費異常 alert、Etherscan 查詢）一起堆。

Hermes Agent 路線實戰：CoinFello skill + Hermes learning loop。重複的 pattern（例如「每天檢查 Aave 健康度、低於 1.5 自動還款」）會被 Hermes 學成 skill，越用越自動化。

你可以設一條 cron：

```js
hermes cron add --name aave-watch --schedule "*/30 * * * *" \
  --task "check aave health factor, if below 1.5 repay smallest debt position"
```

我自己經驗是：CoinFello skill 兩邊都能跑，但生態上有差。 **OpenClaw 的 13K skill 生態讓你能堆更多周邊** （Etherscan / Dune Analytics / DeFiLlama 整合 skill 都有）， **Hermes 的 learning loop 讓重複的 DeFi 動作越跑越聰明** （自動發現「禮拜五 gas 費比較便宜，把不急的交易擠到禮拜五」這種 pattern）。

### 最後怎麼選？

這篇花了超過 1 萬字告訴你兩套代理有什麼不一樣，如果你還在考慮，可以用這三個問題幫你做決定。

**如果你最在意「快速跨多個平台部署」** → OpenClaw

- 例：要把 agent 接 Telegram + Discord + Slack + iMessage + LINE，快速一次到位
- 例：團隊用，需要管理多個 channel 跟多人協作
- 警告：自己顧好安全，特別是 skill 來源跟 API key 暴露（要記得 9 個 CVE 的教訓），全部都要轉成暗碼來讀

**如果你最在意「agent 越用越懂你」** → Hermes Agent

- 例：寫程式時讓 agent 記住專案習慣、debug 模式、命名規則
- 例：長期經營某個內容主題，希望 agent 學你的寫作風格
- 警告：燒錢，自己能支出的最大成本要算清楚，Telegram / Discord gateway 跑會比 CLI 貴 2-3 倍

**如果你最在意「鏈上 DeFi 自動化」** → 兩者都行，看你想要什麼

- CoinFello skill 兩邊相容，先選你習慣的 framework 再加 skill
- OpenClaw 周邊 skill 多，可以堆 Etherscan、Dune、DeFiLlama 整合
- Hermes learning loop 會學你的 DeFi pattern，重複動作越跑越聰明，它會發現一些你沒想過的點

兩個給純新手、純懶人的情境也說明一下：

**如果你完全是新手不會 terminal** → 兩個都先別跑

- 兩套都需要 terminal 跟基本 Python / Node 操作
- 等 managed hosting（託管服務）出來再考慮吧，目前兩套都沒有官方 managed 版本

**如果你做企業團隊正式上線** → 評估狀態

- 有預算 + 願意自己強化安全 → OpenClaw + Docker + 找有經驗的資安做 security audit
- 沒預算 + 等穩定 → 觀望到 Q3，等 Hermes 釋出更穩定的版本，因為你要套的是企業正式環境

### 附錄：Docker 一鍵跑、Tools 推薦、官方資源

#### OpenClaw Docker 一鍵跑

Docker 是 OpenClaw 官方推薦的部署路線，原因是「依賴隔離 + 升級乾淨 + 安全邊界」。重要的是跑在 Docker 裡的 OpenClaw 不會直接碰到你 host 的檔案系統，安全多一層隔離。

```js
git clone https://github.com/openclaw/openclaw.git
cd openclaw
cp .env.example .env
# 編輯 .env，填入你的 LLM provider key
docker compose up -d
```

跑起來後 web admin 介面在 ``http://localhost:18789。資料 bind-mount 到 `OPENCLAW_CONFIG_DIR`（容器內 `/home/node/.openclaw`）跟 `OPENCLAW_WORKSPACE_DIR`（容器內 `/home/node/.openclaw/workspace`），container 重建不會掉資料。``

#### Hermes Agent Docker 部署

官方目前主推 native install（透過 install.sh 跟 uv），Docker compose 還沒進 main repo。社群有第三方 image 但不建議生產用。如果你只是想試試 Hermes，先用 native install。

#### OpenClaw 8 個必裝 Skills

從 ClawHub 13K 個 skill 挑 8 個入門必裝。警告：裝任何 skill 之前先驗證來源！

1. **Notion skill** ：檔案知識庫整合，ClawHub 用量第一名
2. **GitHub skill** ：code review、issue 管理、PR notification
3. **Telegram skill** ：訊息中繼，最常用的 channel
4. **Calendar skill** （Google Calendar）：行事曆自動化
5. **Web Automation skill** ：Puppeteer / Playwright 包裝，agent 能跑瀏覽器任務
6. **Soulcraft** ：互動式 SOUL.md 編輯器，幫你定義 agent 人格
7. **CoinFello onchain skill** ：鏈上 DeFi 整合（MIT，相容 OpenClaw + Claude Code + Hermes）
8. **awesome-openclaw-skills** （VoltAgent 維護）：5,400+ 個 skill 篩選清單，找其他 skill 的入口

#### Hermes Agent 8 個必裝 Tools

Hermes 生態還小，但精挑細選的話：

1. **GitHub MCP server** ： `npx @modelcontextprotocol/server-github` ，開發必裝
2. **Hermes 自己當 MCP server** ：讓 Claude Code / Cursor 透過 Hermes 收發 Telegram / Discord 訊息
3. **awesome-hermes-agent** （0xNyk 維護）：Hermes 專屬 curated tool list
4. **wondelai/skills** ：跨平台 skill library（380+ stars），相容 Hermes + 其他 agent
5. **Tirith security module** ：內建安全層，預設啟用
6. **Cron scheduling** ：內建排程系統，不需另外裝
7. **Multi-instance profiles** ：一臺機器跑多個 hermes instance（家用 / 工作分流）
8. **CoinFello onchain skill** ：鏈上 DeFi 整合（同 OpenClaw 那份）

#### 官方資源連結

OpenClaw：

- [官方檔案 docs.openclaw.ai/install](https://docs.openclaw.ai/install)
- [GitHub repo](https://github.com/openclaw/openclaw)
- [awesome-openclaw-agents（162 個 production-ready template）](https://github.com/mergisi/awesome-openclaw-agents)
- [awesome-openclaw-skills（5,400+ skill 篩選清單）](https://github.com/VoltAgent/awesome-openclaw-skills)

Hermes Agent：

- [官方檔案 hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs)
- [GitHub repo](https://github.com/nousresearch/hermes-agent)
- [DeepWiki 解析](https://deepwiki.com/NousResearch/hermes-agent)
- [awesome-hermes-agent（curated tool list）](https://github.com/0xNyk/awesome-hermes-agent)

### 常見問題

**Q1. Hermes Agent 跟 Claude Code 是同類嗎？差在哪？**  
A：不是。Claude Code 是 Anthropic 官方的 coding 環境，鎖在 Claude 模型。

Hermes Agent 是 Nous Research 的開源框架，能換任何 LLM provider。Hermes 還能反過來當 MCP server，讓 Claude Code 透過它收發訊息。某種程度它跟 Claude Code 是「互補關係」而不是「競爭關係」。

**Q2. OpenClaw 9 個 CVE 是怎麼回事，現在裝還安全嗎？**  
A：2026 年 3 月那一輪 9 個 CVE 已經 patch 掉，但 4 天內爆 9 個漏洞代表 code base 還很新。

降低風險的四個動作：1) 裝最新版（≥ Node 22.14）；2) 透過 Docker 跑加一層隔離；3) 任何 community skill 先 vet 來源，優先用 awesome-openclaw-skills 篩過的；4) API key 不要寫進 SOUL.md，走系統 keychain。

**Q3. 我用 Windows，兩個都裝得起來嗎？**  
A：都裝得起來，但都建議走 WSL2。OpenClaw 原生 Windows 支援，但 Defender 跟其他防毒會誤判 daemon 行為；Hermes Agent 原生 Windows 官方明說不支援，必須 WSL2。如果你 Windows 是主力機，先把 WSL2 裝起來，後面這條路會省很多事。

**Q4. 兩套都能換 LLM 模型嗎？要錢嗎？**  
A：兩套都能換。OpenClaw 支援 Claude 4 / GPT-4o / Gemini 2.0 / DeepSeek V3。

Hermes 更廣，OpenRouter 一個 endpoint 就能接 200+ 模型。要不要錢看你選哪個，用 DeepSeek、Kimi、本機 Ollama 很便宜或免費，用 Claude Opus 4.6 一天可能台幣幾百塊錢（10美元左右），要先在 LLM provider 那邊設 budget alert。

**Q5. 一台機器可以同時跑兩套嗎？**  
A：可以。兩套用不同 port、不同 config 目錄（OpenClaw 在 `~/.openclaw` ，Hermes 在 `~/.hermes` ）。

但兩套同時連同一個 Telegram bot 會打架（兩個 process 同時 polling 一個 bot token 會搶訊息），建議分流裝，像是 OpenClaw 接 Telegram、Hermes 接 Discord，或反過來。

**Q6. CoinFello 鏈上 skill 真的能幫我做 swap，不會被駭嗎？**  
A：CoinFello 用 ERC-4337 smart accounts + ERC-7710 delegation，agent 不會碰到你的 private key。你授權「最多 100 USDC、只能在 Uniswap V4」，agent 就只能在那個範圍內操作。

但 agent 本身的 prompt injection（提示詞注入）風險還在，所以授權金額不要設太高，重要動作用 SOUL.md / Hermes config 加 approval gate。第一次跑建議用 testnet 走一次完整流程。

**Q7. ClawHub 13,729 個 skills 是真的還是膨風？**  
A：真的是這個數字，但水分很高。獨立研究發現 7.1% 處理 secret 不當（可能洩露 API key、credit card 等），341 個被驗證為惡意。

實際可用、值得裝的大概幾百個。從 awesome-openclaw-skills（VoltAgent 維護的 5,400 篩選版）開始比較安全，這個 list 有人幫你過濾過。

**Q8. 我想學 AI Agent，從哪個開始？**  
A：看你目的：

- **想快速看到效果** → OpenClaw 一行裝完接 Telegram，10 分鐘就能跑 demo
- **想搞清楚 agent 內部結構** → Hermes 手動裝一遍，逼自己讀 config、看 dependency、理解 venv
- **想當作長期工作流的核心** → 兩個都跑，三個月後留下你比較適應的那個

**Q9. 兩套都是 MIT，可以商用嗎？**  
A：都可以。MIT 是最寬鬆的開源授權之一，商用、修改、再分發都行，只要保留原作者版權宣告。CoinFello skill 也是 MIT。商用前唯一要注意的：第三方 community skill 的授權各自獨立，裝之前確認該 skill 的 LICENSE 檔。

**Q10. Hermes Agent 的 learning loop 會洩漏隱私嗎？**  
A：預設全部 local。skills、memories、session 都存在 `~/.hermes/` 下面。但如果你的 LLM provider 是雲端（OpenAI / OpenRouter / Anthropic），對話內容會送到 provider 那邊，這跟用一般 chat 一樣。要完全 local 就接 Ollama 或本機 vLLM，模型權重跟 inference 全在你的機器上。

你只需要記得一件事，兩套都是工具，工具沒有絕對的好壞。你想要廣度、要快速接平台、要 13K 個現成 skill，選 OpenClaw；你想要 agent 越用越像你、學你的寫作 / 寫程式 / DeFi 習慣，選 Hermes。

區塊鏈的場景兩個都能用 CoinFello。先選一個跑兩週，再決定要不要換。我自己的習慣是，手機跟團隊溝通的 channel 整合走 OpenClaw，本機長期自動化跟學習工作 pattern 走 Hermes。

兩套同時跑各司其職，這比硬選一個更有用。這篇簡單的介紹到這裡，開始嘗試玩代理吧。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png)](https://t.me/blocktemponews/)

##### 📍相關報導📍

**[小龍蝦風光不再，Hermes Agent 殺出重圍，越用越聰明的自主 AI 來了](https://www.blocktempo.com/hermes-agent-opens-source-nous-research-openclaw-github-stars-learning/)**

**[YC 總裁開源生產級 AI 記憶系統「GBrain」，獨創夢迴圈機制讓 OpenClaw 具備第二大腦](https://www.blocktempo.com/yc-president-garry-tan-open-sources-gbrain-ai-agent-memory-system/)**

**[慢霧專欄：資金交給「龍蝦」 AI Agent 真的安全？聯合 Bitget 報告揭露五大風險](https://www.blocktempo.com/ai-agent-web3-security-risks-slowmist-bitget-prompt-injection-skill/)**

**[巴逆逆變工具人！網友開源「冥燈反指追蹤器」，她一停損你 TG 就響](https://www.blocktempo.com/banini-counter-indicator-tracker-claude-playwright-telegram-bot-taiwan-influencer/)**

**[Solana AI 駭客松上線：一覽 12 個 AI Agent 新專案](https://www.blocktempo.com/overview-of-12-new-projects-from-the-solana-ai-hackathon/)**

### 最新文章

---

[![](https://image.blocktempo.com/2026/04/cover_20260414_072953-1140x815.webp "cover_20260414_072953 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://image.blocktempo.com/2026/04/cover_20260414_072953.webp)

中國地產巨頭恆大集團創辦人許家印，於 2026 年 4 月 13～14 日在深圳中院一審開庭，當庭表示認罪悔罪，法院將擇期宣判；8 項重罪中，集資詐騙罪最重可判處無期徒刑乃至死刑。  
（前情提要： [中國銀行前副行長王永利：中國為何堅決叫停穩定幣？](https://www.blocktempo.com/china-ban-stablecoin-digital-yuan-strategy/) ）  
（背景補充： [穩定幣的 2025年底復盤：你在紅樓，我在西遊](https://www.blocktempo.com/stablecoin-2025-china-regulation-blockchain-infrastructure/) ）

兩年半，從 2023 年 9 月許家印被採取強制措施，到 2026 年 4 月 14 日在深圳中級人民法院庭審末尾當庭認罪，這個曾掌控中國最大房企的男人，用「認罪悔罪」為自己的商業帝國畫下句點。

### 許家印面臨 8 項罪名，集資詐騙最重可判死刑

此次一審中，許家印個人被控罪名多達 8 項：非法吸收公眾存款、集資詐騙、違法發放貸款、違法運用資金、欺詐發行證券、違規披露重要訊息、職務侵佔、單位行賄。

恆大集團則另被控欺詐發行證券、違規披露重要資訊、違法發放貸款與單位行賄等罪。

其中量刑最重的是「集資詐騙罪」，依中國《刑法》第192條，情節特別嚴重者可判處無期徒刑乃至死刑，並處沒收財產。「非法吸收公眾存款」最高可判 10 年有期徒刑，「欺詐發行證券」近年修法後最高刑期也升至 15 年。

8 項罪名合計，理論上累計刑期極為沉重。

與中國近年重大金融詐騙案相比，許家印案的規模屬於歷史級別。2020 年被判決的安邦保險原董事長吳小暉，以集資詐騙罪被判 18年、沒收財產約 105 億人民幣；P2P平台e租寶的張敏，判處無期徒刑。

許家印涉及的資金規模遠超上述案例，外界多預測刑期不會低於 20 年，死刑可能性雖存在但主流觀點認為機率較低，畢竟本案的政治定性是「企業違法」而非「顛覆性威脅」。

旁聽席上，部分全國人大代表與政協委員獲邀出席，彰顯此案的高度政治象徵意義。法院表示，宣判日期將另行公告，外界預計不會拖延太久。

### 恆大危機曾引爆加密市場恐慌

對加密貨幣市場的觀察者而言，「恆大」這個名字並不陌生，它曾是 2021 年全球風險資產最大的系統性恐慌來源之一，加密貨幣市場也受其波及。

這場跌勢的底層邏輯在於「中國系統性風險外溢」的預期，市場擔心恆大 2,300 億美元以上的債務若觸發連鎖違約，將拖累中國銀行體系，進而衝擊全球流動性，而加密貨幣作為高風險資產首當其衝。

儘管事後確認實際衝擊相對可控，但這段歷史留下一個教訓：中國大型金融事件，哪怕與區塊鏈毫無直接關聯，仍足以在數位資產市場製造短暫但劇烈的波動。

### 兩年半才開庭：從債務危機到法庭認罪

恆大集團 1996 年由許家印創辦於廣州，高峰期資產規模超過 3,000 億美元，業務橫跨地產、足球俱樂部、電動車與文旅地產。2021 年恆大債務危機全面爆發，逾 300 億美元境外美元債宣告違約，旗下電動車品牌恆馳汽車亦資金鏈斷裂，觸發中國有史以來最大規模的私人企業破產重組程式。

2023 年 9 月 28 日，許家印以「涉嫌違法犯罪」為由被採取強制措施，正式從公眾視野消失。此後將近兩年半，案件在檢察院偵查、移送起訴、法院受理等程式中緩慢推進，直至今年 4 月才啟動一審庭審。

[![加入動區 Telegram 頻道](https://image.blocktempo.com/2022/11/%E5%8B%95%E5%8D%80%E5%AE%98%E7%B6%B2tg-banner-1116.png "動區官網tg banner-1116 | 動區動趨-最具影響力的區塊鏈新聞媒體")](https://t.me/blocktemponews/)

##### 📍相關報導📍

**[週一早盤比特幣大跳水，加密市場因何再陷恐慌？](https://www.blocktempo.com/crypto-market-panic-selloff-china-regulation-december-2025/)**

**[律師觀點：香港《穩定幣條例》洗版，為何中國多地卻發非法集資警示？](https://www.blocktempo.com/hong-kong-stablecoin-ordinance-illegal-fundraising-warnings/)**

**[加密貨幣案件中「虛擬貨幣值多少錢」？司法機關可以訂價嗎？](https://www.blocktempo.com/crypto-price-determination-in-legal-cases/)**

**[爛尾樓太多？中國政府考慮「禁止預售屋」制度：房子蓋好才能賣](https://www.blocktempo.com/china-is-considering-restricting-the-pre-sale-mechanism-for-real-estate/)**

**[中國民生銀行爆降薪50%、8月失業率創新高，習近平力推「過緊日子」政策](https://www.blocktempo.com/chinas-salary-cuts-and-unemployment-rates-have-both-broken-records-salary-will-be-reduced-by-half-after-shared-prosperity/)**