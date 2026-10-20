---
title: Hermes Agent：自我成長的開源 AI 代理框架
date: 2026-05-20
tags: [ai/agent, ai/claude]
source: https://github.com/NousResearch/hermes-agent
source_2: https://www.bnext.com.tw/article/90591/hermes-agent-procedural-memory
source_3: https://www.blocktempo.com/nous-hermes-agent-self-improving-loop-openclaw-claude-pro-tutorial/
type: article
status: processed
asset_value: 36
---

Hermes Agent 是 Nous Research 開源的 AI 代理框架（MIT 授權），核心特色是「程序性記憶」學習迴圈：每次完成任務後自動萃取可重用技能、跨 session 記憶用戶偏好。官方 slogan：「The agent that grows with you」。GitHub 42.2k+ stars。

## T（核心論點）

傳統 AI 助理是 stateless，每次對話從零開始。Hermes 內建三層長效記憶：**Episodic Memory（任務紀錄）→ Skill 抽象（可重用工作流）→ Honcho 使用者建模**。同樣的任務跑得越多，技能庫越精準，長期複利效果明顯。

## B（適用場景）

**適用：** 有大量重複性工作流的重度用戶；需要 AI 累積工作習慣的長期使用；跨平台多裝置操作  
**不適用：** 偶爾一次性問答；不想自託管基礎設施；需要生產環境穩定（v0.8.0 仍為早期版本）  
**Windows 注意：** 不支援原生安裝，需透過 WSL2

## R（安裝與快速上手）

### 安裝（一行指令）

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc  # macOS 用 source ~/.zshrc
hermes
```

首次啟動進入設定精靈。安裝後執行 `hermes doctor` 診斷環境。

### 串接 Claude Pro/Max（最常用路徑）

```bash
hermes model
# 選 Anthropic → Claude Code OAuth → 選模型（sonnet-4-6 為平衡選擇）
```
使用 OAuth 可直接吃訂閱額度，不需另開 API Key。

### 從 OpenClaw 搬家

```bash
hermes claw migrate --dry-run   # 先預覽
hermes claw migrate              # 正式執行（設定/skills/history 全搬）
```

### 常用指令速查

| 指令 | 功能 |
|------|------|
| `hermes` | 啟動互動式 CLI |
| `hermes model` | 切換 AI 供應商與模型 |
| `hermes tools` | 啟用／停用工具 |
| `hermes gateway` | 啟動多平台通訊閘道（Telegram/Discord/Slack/WhatsApp/Signal/Home Assistant）|
| `hermes setup` | 完整設定精靈 |
| `hermes update` | 升級至最新版本 |
| `/skills` | 瀏覽已建立的技能 |
| `/new` 或 `/reset` | 對話中開新對話 |
| `Alt+Enter` / `Ctrl+J` | 多行輸入 |
| `Ctrl+B` | 語音錄音（需另裝 voice 套件）|

## C（關鍵行動）

- [ ] 若已有 Claude Pro/Max 訂閱，優先走 OAuth 路徑，省去額外 API 費用
- [ ] 用 `hermes claw migrate --dry-run` 安全預覽 OpenClaw 遷移內容
- [ ] 前兩週不會感覺差別，第一個月後才會體會到學習迴圈效果
- [ ] 搜尋 agentskills.io 可安裝社群共享的現成 Skill，加速初期學習

## 原始摘要

Hermes Agent 最大差異點在學習迴圈（Episodic Memory → 技能抽象 → Honcho 使用者建模），相較 OpenClaw 的 stateless 設計，適合長期、重複性工作流。支援 16+ 模型供應商、8 個通訊平台統一閘道、6 種終端執行後端（含 Serverless）。

## Hermes vs OpenClaw 比較

| 面向 | Hermes Agent | OpenClaw |
|------|-------------|---------|
| 記憶設計 | 三層長效記憶（Episodic/Skill/Honcho）| Stateless（每次從零）|
| 平台支援 | 8 個通訊平台統一閘道 | 主打 CLI + 少數 webhook |
| 執行後端 | 6 種（含 Serverless 冬眠節費）| 預設本地，隔離要自配 Docker |
| 模型支援 | 16+ 供應商 / 200+ 模型 | 主流供應商 |
| 技能生態 | agentskills.io 開放標準 | ClawHub（較成熟但封閉）|
| 社群成熟度 | 早期（v0.8.0）| 更成熟，常見錯誤已有社群驗證 |

## 學習迴圈機制

```
執行任務 → 評估結果（採用=正向訊號）→ 萃取技能（成功模式存 Markdown）
→ 優化技能（新結果比對舊技能自動更新）→ 檢索技能（新任務先查庫再執行）
```

**Honcho 辯證建模**：跨 session 分析對話風格、拒絕過的選項、強調過的偏好，建立動態「agent 理解的你是誰」模型。效果在第一個月後才顯現。

**Episodic Memory 位置**：`~/.hermes/episodic/`  
**Skill 位置**：`~/.hermes/skills/`（遵循 agentskills.io 開放標準，跨框架可用）

## 補充資料：免費模型供應商速查（供應商彈性是 Hermes 的核心賣點）

來源：codelove.tw（原文 dev.to/dalenguyen，2026-07）。Hermes 支援 200+ 模型，`hermes model` 或對話中 `/model [provider:model]` 免改設定檔即可切換供應商，讓「先用免費方案探索、再決定要不要付費」變得可行。

| 供應商 | 免費方案 | 搭配 Hermes 最佳用途 |
|--------|---------|---------------------|
| **Nous Portal** | 註冊時查看 | 與 Hermes/Nous 生態系最緊密整合 |
| **OpenRouter** | 每日 200 次請求，27+ 個 `:free` 模型，每分鐘 20 次 | 模型多樣性、快速切換比較底層模型行為 |
| **NVIDIA NIM** | 註冊贈點數，無期限，約每分鐘 40 次 | 直接託管 `nousresearch/hermes-3-llama-3.1-70b`，想用 NousResearch 原生模型的首選 |
| **Hugging Face Inference Providers** | 每月免費點數，路由 Groq/Together/SambaNova 等 | 利基/較小模型，冷啟動較慢不適合長互動，適合批次任務 |
| **NovitaAI** | 新帳號贈點數 | OpenRouter/NIM 卡速率限制時的備援 |
| **Kimi / Moonshot** | 長上下文模型免費方案 | 需處理大量文件或超長對話歷史時 |

**實務技巧**：Agentic 迴圈中速率限制會疊加——Hermes 的學習迴圈與子代理可能讓單一任務觸發大量模型呼叫，OpenRouter 每分鐘 20 次上限在多步驟任務中很快就會撞到，建議設定備援供應商並用 `hermes model` 即時切換；免費探索階段優先用 OpenRouter（一個金鑰涵蓋 27+ 模型）；設定出錯時先跑 `hermes doctor` 診斷再排錯。

## 相關連結

- [[agent-skills-toolkit]] — agentskills.io 對應的 Skill 設計標準
- [[claude-skills-guide]] — Claude Code Skills 設計哲學（與 Hermes Skill 的比較）
- [[harness-engineering]] — Agent 框架背後的 Harness 概念
- [[agentic-ai-learning-roadmap]] — Agent 框架選擇的學習路徑

## 補充資料：OpenClaw vs Hermes 完整對比

來源：《OpenClaw 跟 Hermes Agent 該安裝哪一個？》（動區動趨，2026-04-14）、《OpenClaw 和 Hermes 到底哪個更適合你？》（鉅亨網，2026-04-26）

### 設計哲學對比（法拉利 vs 本田）

@garrytan 的精準類比：「OpenClaw 像一輛需要自己當機修工的法拉利——時常拋錨但體驗讓你爽翻天；Hermes 是一輛靠譜的本田；Claude / ChatGPT 是坐公共汽車。」

| 維度 | OpenClaw | Hermes Agent |
|------|---------|-------------|
| 設計哲學 | 嚴守說明書，執行為主（SOUL.md 定義人格） | 自寫說明書，學習為主（learning loop） |
| 訊息平台 | 24+（最廣） | 8（Telegram、Discord、Slack、WhatsApp、Signal、Feishu、WeCom、CLI）|
| Skill 生態 | ClawHub 13,000+ skills（341 個驗證為惡意）| awesome-hermes-agent + wondelai/skills（380+）|
| 學習能力 | 無，每次任務都當新問題 | built-in learning loop + multi-level memory |
| 安全紀錄 | 9 個 CVE / 4 天爆出 / 1 個 CVSS 9.9 / 135K 暴露 instance | 0 agent-specific CVE |
| Token 成本 | 標準 | CLI 6-8k input / gateway（Discord/Telegram）15-20k（2-3 倍）|
| 安裝門檻 | Node.js ≥ 22，一行安裝 | Python 3.11 + uv + Node.js，Windows 必須 WSL2 |
| Release 數 | 82 個（成熟）| 6 個（早期，v2026.4.8）|

**Token 成本實測**：有創始人反映同等任務在 OpenClaw 5 天花 130 USD，換 Hermes 只花 10 USD（成本差 13 倍）。差異來自模型選擇與 Hermes 的 token 效率設計。

### OpenClaw 安裝（macOS）

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
# 或
npm install -g openclaw@latest
openclaw onboard --install-daemon  # --install-daemon 讓服務開機自動啟動
```

**必知雷點**：Node.js 需 22.14+ 或 24（22.0-22.13 有 known bug）；SOUL.md 預設在 `~/.openclaw/soul.md`，第一步建議先寫禁止事項（「任何 swap 超過 100 USDC 先 ping 我」）。

### Hermes 手動安裝（macOS，適合學習理解依賴）

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh  # 安裝 uv
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
uv venv venv --python 3.11
uv pip install -e ".[all]"
cp cli-config.yaml.example ~/.hermes/config.yaml
hermes doctor  # 診斷所有依賴與設定
hermes setup && hermes model
```

### 三場景選擇建議

| 場景 | 建議 | 理由 |
|------|------|------|
| 開發輔助 | OpenClaw（短期）+ Hermes（長期）| OpenClaw SOUL.md gate 易寫禁止事項；Hermes 學習 commit 格式、PR 風格 |
| 內容產製 | 一次性轉文章 → OpenClaw；長期經營專欄 → Hermes | Hermes learning loop 學你的寫作風格 |
| 鏈上 DeFi | 兩者均可（CoinFello skill MIT 開源，兩邊相容）| OpenClaw 有更多周邊 skill；Hermes 學你的 DeFi pattern |

### 決策樹

- **最在意多平台快速部署** → OpenClaw
- **最在意 agent 越用越懂你** → Hermes
- **完全新手，不熟 terminal** → 兩者都先別用，等 managed hosting
- **企業生產環境** → OpenClaw + Docker + 資安 audit；或等 Hermes 更穩定版本

同時跑兩套也可行：分流訊息平台（OpenClaw 接 Telegram、Hermes 接 Discord），各司其職。

---

## 補充資料：Hermes Desktop — GUI 版本降低入門門檻

來源：電腦王阿達（2026-05-16）《Hermes Desktop 讓 Windows 直接就能安裝使用 Hermes Agent！》

Hermes Desktop（v0.4.3，MIT 授權）是由開發者 Fathah 打造的 Hermes Agent GUI 前端，將 CLI 的所有能力包裝成圖形化介面，支援 macOS/Windows/Linux（.dmg/.exe/.AppImage/.deb/.rpm）。累積下載超過 5,000 次。

### 核心功能

| 功能 | 說明 |
|------|------|
| 引導式安裝 | 初次啟動自動檢查 Git/uv/Python 3.11+，若未安裝執行官方腳本 |
| 本地或遠端後端 | 本地（127.0.0.1:8642）或連接雲端 Hermes API 伺服器（URL + API Key）|
| 串流對話 UI | SSE 串流，含工具執行進度、Markdown 渲染、Token 用量即時顯示 |
| 22 個斜線指令 | `/new` `/clear` `/fast` `/web` `/image` `/browse` `/code` `/model` `/memory` 等 |
| 14 工具集 | 網頁搜尋、瀏覽器控制、終端機、檔案操作、程式碼執行、視覺分析、語音合成（TTS）等 |
| 16 種訊息平台 | Telegram、Discord、Slack、WhatsApp、Signal、Matrix、Email、SMS、iMessage、釘釘、飛書等 |
| 排程任務（Cron） | 圖形化建立器，支援分鐘/小時/每日/每週/自訂 Cron，15 種交付目標 |
| 記憶系統管理 | 檢視編輯記憶，支援 Honcho、Hindsight、Mem0 等供應商 |
| 多供應商模型切換 | OpenRouter、Anthropic、OpenAI、Gemini、Qwen、Groq、LM Studio、Ollama 等 |
| SOUL.md 編輯器 | 直接編輯當前 profile 的個性設定檔 |

### 適用對象

- **推薦使用**：不熟悉終端機的初學者；需要在 Windows 上使用 Hermes（無需 WSL2）
- **不需要換**：已在 Mac/Linux 上跑 CLI 版本的使用者（Desktop 與機器運作綁定，關機就停）
- **注意**：背景執行的 CMD 視窗不可關閉（是工作流的一部分）；Windows 安裝檔目前缺少程式碼簽章（SmartScreen 會警告）

NVIDIA 已宣布 Hermes Agent 將登陸 RTX AI PC 平台，Hermes Desktop 未來有機會與 RTX 本地 AI 加速深度整合。
