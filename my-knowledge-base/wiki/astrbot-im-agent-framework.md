---
title: AstrBot：把 AI Agent 部署進既有 IM 平台的開源框架
date: 2026-09-08
tags: [ai/agent, ai/mcp, type/concept]
source: https://tools.wingzero.tw/articles/4056
type: reference
status: processed
asset_value: 60
---

開源 Agentic AI 助理與開發框架（GitHub 約 40K stars、五千次以上 commit），把 LLM 與 Agent 部署到 LINE、Telegram、Discord、Slack、企業微信、飛書、釘釘等日常通訊軟體。

## T（核心論點）

> **AI Assistant 的「能力」和「聊天 App」可以分開考慮——Agent 不必住在另一個網站，可以回到使用者本來就每天開著的 IM 平台。**

它不是「收到訊息 → 呼叫 LLM API → 回傳答案」的聊天機器人，而是把 **Messaging Platform / Model Provider / Agent Runner / Tools / Knowledge Base / Plugins / WebUI** 分層。底層負責 Agent、模型、Context、Plugins 與 Knowledge，上層只決定使用者從哪個平台進來：

```
使用者 → LINE / Discord / Telegram / Slack 等 IM
      → AstrBot → Agent / LLM → Tools / MCP / Knowledge Base / Plugins → 回覆
```

🔑 **連 Agent Runner 本身都可以換**（內建 Runner 之外支援 Dify、Coze、阿里雲百煉、DeerFlow）——已在其他 Agent 平台建好 workflow 的團隊，可以只把 AstrBot 當 **Messaging Gateway 與 Runtime Infrastructure**，不必把 Agent Logic 全部搬進來。

## B（適用場景）

**適用：**
- **個人 AI 助理**——把 Agent 放進 Telegram、LINE、Discord 等本來就會開的 App，不必為私人 AI 再建立一套操作習慣
- **社群 Bot**——把 LLM、知識庫與 Tool Use 加進既有 Discord／QQ／Telegram 群組
- **企業內部助手**——文件問答、內部知識搜尋放進企業微信／飛書／釘釘／Slack／Mattermost
- **Agent Developer**——需要一套已處理好 Messaging Adapter、Plugin、WebUI、MCP、Knowledge、Sandbox 與 Model Provider 的基礎設施
- 24/7 常駐：支援 Docker／Docker Compose、Kubernetes、BT Panel、1Panel、**CasaOS（適合 NAS／Home Server／Home Lab）**

**不適用 / 注意：**
- ⚠️ **小型一次性 Bot 未必需要**。自己用 Python 寫 Telegram Bot + OpenAI API 的初始架構簡單得多；AstrBot 省下的是**日後**自建 Plugin System、Dashboard、Knowledge Base、MCP、Model Switching、Sandbox 與多平台 Adapter 的工作。只有預期 Bot 長期運行、持續增加功能，或同一套 Agent 要服務多個平台時，框架價值才會出現
- 🔴 **第三方 Plugin 的安全性官方不保證**。正式環境（尤其會接觸私人訊息、企業文件、API Token 或 Shell）安裝前應檢查來源與程式碼，**不要看到 Plugin Market 就當成已經官方審核**
- 🔴 **Agent Sandbox 不等於可以忽略 Server Security**。只要 Bot 能接收外部訊息、連接 LLM API、使用 MCP 或執行 Tool Call，就仍須處理 **Authentication、Permission、Network Exposure、Secrets 與 Prompt Injection**（見 [[agent-permission-and-os-sandbox]]、[[prompt-injection-vs-jailbreak]]）
- 🔴 **授權是 AGPL-3.0**：若修改 AstrBot 並用修改版提供商業 Network Service，**必須依授權公開相關修改**；專案另有 EULA。不要只看到「Free & Open Source」就忽略授權條件
- WhatsApp 仍標示 Coming Soon；Matrix、Rocket.Chat、VoceChat 屬 Community Integration

## R（能力盤點與部署選項）

**Messaging Platform**：QQ、OneBot v11、Telegram、WeCom／WeCom AI Bot、微信公眾號、飛書、釘釘、Slack、Discord、LINE、Satori、KOOK、Misskey、Mattermost。

**Agent 能力**：內建 Agent Runner、Multimodal、Tool Calling、**MCP**、**Anthropic Skills**、Knowledge Base、Persona Settings、**Auto Context Compression**、Computer Use、**SubAgent Orchestration**、**Proactive Tasks**、Agent Sandbox、Web Search。

幾個值得單獨注意的：

- **MCP** — 不必替每個 Agent 與外部服務重新設計專屬整合。把 MCP 納入正式功能，也反映它的定位已從 Chatbot Framework 往 **Agent Infrastructure** 發展
- **Knowledge Base** — 讓 AI 根據自己的文件 Retrieval 再生成，適合公司內部文件助手、產品 FAQ、客服知識庫
- **Auto Context Compression** — 長對話的 Context Window 與 Token 成本管理
- **Agent Sandbox** — 隔離、安全執行 Code 與 Shell Calls，支援 Session-level Resource Reuse。當 Agent 開始需要執行程式與系統操作時，這是重要的安全邊界
- **Proactive Tasks** — AI 不必等人傳訊息才動作；Reminder、Monitoring、定期資訊整理讓 Bot 變成常駐助理
- **SubAgent Orchestration** — 複雜任務拆給不同角色或子任務，由主要 Agent 協調

**Plugin 生態**：1000+ 可一鍵安裝，WebUI Plugin Market 或 URL／檔案手動安裝，CLI 亦支援 search／install／update／remove／建立 template。🔑 **Plugin Pages** 讓外掛不只增加聊天指令，還能在 Dashboard 加入自己的管理頁面（複雜表單、Runtime Dashboard、Logs、檔案上傳下載、SSE Stream、Charts）——Plugin System 已不只是「輸入 `/weather` 就跑一個 Python function」，而是具有自己 Backend 與 Management UI 的完整功能模組。

**管理**：內建輕量 Web ChatUI（類 OpenWebUI，整合 Sandbox 與 Web Search）＋ Dashboard（Plugins、Logs、視覺化設定、Statistics；首次啟動產生隨機初始密碼，支援 TOTP 兩階段驗證）。🔑 **把大量日常管理移出 `.env` / YAML / CLI**，對家庭、社群與小型團隊部署友善得多。

**模型**：OpenAI、Anthropic、Gemini，文件另提供 Ollama、LM Studio 等 Local LLM 整合（見 [[local-llm-deployment-overview]]）。

**三種部署角色定位**：

```
簡單模式     → AstrBot 本身就是 AI Bot
進階模式     → AstrBot + Built-in Agent + MCP + Plugins
既有 Agent 平台 → Dify / Coze 等負責 Agent，AstrBot 只負責 IM Integration
```

## C（關鍵行動）

- [ ] 評估前先確認：這個 Bot 會長期運行嗎？要服務多個 IM 平台嗎？否則自己寫一支更省
- [ ] 安裝任何第三方 Plugin 前檢查來源與程式碼，特別是會碰到私訊、企業文件、Token、Shell 的環境
- [ ] 若打算商業化，先讀 AGPL-3.0 與 EULA 的公開修改義務
- [ ] 想試最快路徑：`uv` 安裝體驗 → 正式環境改 Docker Compose → NAS／Home Lab 走 CasaOS

## 補充資料

### 與相鄰工具的定位差異

| 對照 | 差異 |
|---|---|
| **Dify** | Dify 強在建立 LLM Application 與 Workflow；AstrBot 是 **Messaging-first**，重點在讓 Agent 真正出現在 IM。**兩者可直接整合，不必然是競爭關係** |
| **Open WebUI 類** | 那類主要是「打開瀏覽器與模型聊天」；AstrBot 雖也有 ChatUI，核心是多平台 Messaging Integration 與 Bot／Agent Runtime |
| **自己寫 Telegram Bot + OpenAI API** | 初始複雜度高很多，換來的是不必日後自建 Plugin System／Dashboard／Knowledge／MCP／Sandbox／多平台 Adapter |

### 從 Chatbot 到 Agentic Assistant 的差別

```
傳統：User → Prompt → LLM → Text
Agent：User → Agent → 搜尋資料 → 呼叫工具 → 執行程式 → 分析結果 → 再呼叫工具 → 回覆
```

MCP、Skills、Tool Use、Computer Use、SubAgent、Sandbox 與 Proactive Tasks 基本上都是在支援後面這種工作模式。

## 相關連結

- [[agent-permission-and-os-sandbox]] — 🔑 **Sandbox 不等於可以忽略 Authentication／Permission／Network Exposure／Secrets／Prompt Injection**，本篇的安全警示與該條目同源
- [[prompt-injection-vs-jailbreak]] — 開放給外部訊息的 Bot 首要風險
- [[local-llm-deployment-overview]] — 模型側的自架部署（Ollama／LM Studio 整合）
- [[ai-chatbot-free-tier-comparison-2026]] — 比較的是聊天服務本身，不是部署框架
- [[a2a-agent-protocol-implementation]] / [[multi-agent-cli-bridge]] — Agent 之間的通訊；本篇的通道是 IM 而非 Agent 間協定
- [[agent-skills-vs-mcp]] — AstrBot 同時支援 MCP 與 Anthropic Skills，兩者的分工判準見該篇
