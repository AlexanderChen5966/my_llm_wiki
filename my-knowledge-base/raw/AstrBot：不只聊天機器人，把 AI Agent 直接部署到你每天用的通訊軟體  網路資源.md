---
title: "AstrBot：不只聊天機器人，把 AI Agent 直接部署到你每天用的通訊軟體 | 網路資源"
source: "https://tools.wingzero.tw/articles/4056"
author:
  - "[[赤紅翼零]]"
published: 2026-09-08
created: 2026-09-08
description: "AstrBot 是一套開源 AI Agent 助理與開發框架，可串接 LINE、Telegram、Discord、Slack 等聊天平台，支援多種 LLM、MCP、Skills、知識庫、Agent Sandbox 與 1000+ Plugins，適合打造私人 AI 助理、企業知識庫與自動化聊天機器人。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/AstrBotDevs/AstrBot](https://github.com/AstrBotDevs/AstrBot)

AstrBot 是一套開源的 Agentic AI 助理與開發框架，核心用途是讓開發者把大型語言模型與 AI Agent 部署到日常使用的即時通訊軟體中。它可以串接 QQ、企業微信、飛書、釘釘、Telegram、Discord、LINE、Slack、Mattermost 等平台，再搭配 OpenAI、Anthropic、Gemini 等模型，以及 MCP、Skills、知識庫、工具呼叫、Agent Sandbox 和外掛系統，建立個人 AI 助理、群組機器人、客服、企業知識庫或自動化 Agent。

這也是 AstrBot 和一般 ChatGPT Bot 專案最明顯的差異。它並不是單純做一個「收到訊息 → 呼叫 LLM API → 回傳答案」的聊天機器人，而是把 Messaging Platform、Model Provider、Agent Runner、Tools、Knowledge Base、Plugins 與 WebUI 分成不同層級。開發者可以更換模型、增加聊天平台、安裝外掛，甚至接入其他 Agent 平台，而不需要每次都從頭建立一套 Bot。

從實際使用情境來看，AstrBot 最容易理解的方式，就是把 AI Agent 放進原本已經存在的聊天工具。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="837" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-7782711747648373&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=4270348415&amp;adf=199514577&amp;abgtt=6&amp;w=837&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788829645&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=9252643522&amp;ad_type=text_image&amp;format=837x280&amp;url=https%3A%2F%2Ftools.wingzero.tw%2Farticles%2F4056&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=836&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829618288&amp;bpp=4&amp;bdt=1272&amp;idt=4&amp;shv=r20260902&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dde79b9afe787630e%3AT%3D1765424491%3ART%3D1788829582%3AS%3DALNI_MaZo7WVfzoXyy5lof_hIuSGpaF40A&amp;gpic=UID%3D000011c79b636c65%3AT%3D1765424491%3ART%3D1788829582%3AS%3DALNI_MYUundZNpk3HKTsSU6La3dkCle5uQ&amp;eo_id_str=ID%3Da28a003da0258f0f%3AT%3D1780982398%3ART%3D1788829582%3AS%3DAA-AfjYise-jOnVVlb85XD1yH2KP&amp;prev_fmts=0x0%2C1522x969%2C789x280&amp;nras=3&amp;correlator=5671670132681&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=169&amp;ady=2715&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400775%2C95400618%2C122880333&amp;oid=2&amp;pvsid=4904258241525131&amp;tmod=1169943733&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Ftools.wingzero.tw%2Farticles&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;plas=157x763_l%7C157x763_r&amp;bz=0&amp;ifi=5&amp;uci=a!5&amp;btvi=2&amp;fsb=1&amp;dtd=26973" title="Advertisement" aria-label="Advertisement"></iframe>

例如團隊平常都使用 Slack，就不一定需要要求所有成員另外開啟一套 AI 網站；可以讓 AstrBot 連接 Slack，成員直接在 Channel 或對話中呼叫 AI。社群如果主要使用 Discord，也可以讓同一套 Agent 能力出現在 Discord。企業環境則可以依原本使用的企業微信、飛書或釘釘進行整合。

官方目前列出的 Messaging Platform 涵蓋 QQ、OneBot v11、Telegram、WeCom／WeCom AI Bot、微信公眾號、飛書、釘釘、Slack、Discord、LINE、Satori、KOOK、Misskey 與 Mattermost；Matrix、Rocket.Chat、VoceChat 等則有 Community Integration，WhatsApp 目前仍標示為 Coming Soon。

這種 Multi-platform 架構帶來一個很實際的優點：AI Assistant 的能力和「聊天 App」可以分開考慮。

底層 AstrBot 負責 Agent、模型、Context、Plugins 與 Knowledge；上層只是決定使用者從 Telegram、Discord、Slack 還是其他平台和它互動。

整體架構可以簡化理解成：

`使用者 → LINE / Discord / Telegram / Slack 等 IM → AstrBot → Agent / LLM → Tools / MCP / Knowledge Base / Plugins → 回覆`

目前 AstrBot 比較重要的能力可以整理為：

- 支援多種主流即時通訊平台
- 支援 OpenAI、Anthropic、Gemini 等不同大型語言模型
- 內建 Agent Runner
- 可串接 Dify、Coze、阿里雲百煉、DeerFlow 等 Agent Runner
- 支援 Multimodal 對話
- 支援 Tool Calling
- 支援 MCP
- 支援 Anthropic Skills
- 支援 Knowledge Base
- 支援 Persona Settings
- 支援 Auto Context Compression
- 支援 Computer Use
- 支援 SubAgent Orchestration
- 支援 Proactive Tasks
- 提供 Agent Sandbox
- 提供 Web Search
- 內建 Web ChatUI
- 提供完整 WebUI 管理介面
- 擁有 1000+ 可安裝 Plugins
- 支援自訂 Plugin Development。

其中 Plugin Ecosystem 是 AstrBot 很重要的一部分。

官方 README 目前標示已有 1000+ Plugins 可以一鍵安裝。使用者可以直接從 WebUI 的 Plugin Market 尋找外掛，也能透過 URL 或檔案手動安裝。CLI 同樣提供 Plugin Search、Install、Update、Remove 與建立新 Plugin Template 等操作。

因此，AstrBot 不要求所有功能都進入核心程式。

如果有人需要天氣、遊戲、特定 API、群組管理或其他特殊能力，可以利用 Plugin 擴充。開發者也能自行建立 Plugin，監聽 Message Event、傳送訊息、建立設定介面、使用 AI、保存資料，甚至替 Plugin 建立自己的 WebUI Page。

Plugin Pages 是近期架構中很值得注意的一點。外掛不只能增加聊天指令，還能在 AstrBot Dashboard 裡加入自己的管理頁面。官方文件列出的適用情境包含複雜表單、Runtime Dashboard、Logs、檔案上傳下載、SSE Stream 與 Charts。

這代表 AstrBot 的 Plugin System 已經不只是傳統 Bot 常見的「輸入 /weather 就執行一個 Python Function」，而是開始允許開發者建立具有自己 Backend 與 Management UI 的完整功能模組。

MCP 則讓 AstrBot 可以進一步使用外部工具。

Model Context Protocol 的價值在於，不必替每一個 Agent 與外部服務重新設計專屬整合方式。只要服務提供相容 MCP 的能力，就能讓 Agent 取得額外工具或資料來源。AstrBot 把 MCP 納入正式 Usage 功能，也反映它現在的定位已經從 Chatbot Framework 往 Agent Infrastructure 發展。

Knowledge Base 則適合企業或個人私有資料。

一般 LLM 只能根據模型原本的知識與目前 Conversation 回答問題；建立 Knowledge Base 後，可以讓 AI 根據自己的文件或資料進行 Retrieval，再生成回答。因此 AstrBot 不只能當聊天陪伴機器人，也能建立公司內部文件助手、產品 FAQ、客服知識庫等應用。

官方同時提供 Auto Context Compression。長時間聊天時，Conversation History 很容易持續增加，最後超出模型 Context Window，或造成 Token 成本快速上升。Context Compression 的方向就是在長對話與 Agent Workflow 中管理這類問題。

更值得注意的是 Agent Sandbox。

AstrBot 官方把 Sandbox 定位成隔離、安全執行 Code、Shell Calls，並支援 Session-level Resource Reuse 的環境。當 Agent 不只是「回答問題」，而開始需要執行程式與系統操作時，Sandbox 就成為很重要的安全邊界。

這也是 Agentic Assistant 與普通 Chatbot 的關鍵差異之一。

傳統聊天流程可能只是：

`User → Prompt → LLM → Text`

Agent Workflow 則可能變成：

`User → Agent → 搜尋資料 → 呼叫工具 → 執行程式 → 分析結果 → 再呼叫工具 → 回覆`

AstrBot 提供的 MCP、Skills、Tool Use、Computer Use、SubAgent、Sandbox 與 Proactive Tasks，基本上都是在支援後面這種工作模式。

SubAgent Orchestration 則讓工作可以進一步拆分。複雜任務不一定全部交給單一 Agent 處理，可以依任務需求建立不同角色或子任務，再由主要 Agent 協調。

Proactive Tasks 也值得注意，因為它代表 AI 不一定只能「有人傳訊息才動作」。對 Reminder、Monitoring、定期資訊整理或其他 Automation Scenario 而言，主動式 Task 能讓聊天機器人逐漸變成真正的常駐助理。

AstrBot 也沒有把使用者限制在第三方聊天平台。

系統內建輕量 Web ChatUI，官方將其描述為類似 OpenWebUI 的聊天介面，而且整合 Agent Sandbox 與 Web Search。因此即使不串 Telegram 或 Discord，也能直接從瀏覽器使用自己的 AI Assistant。

管理方面則提供另一套 WebUI。

AstrBot Dashboard 可以管理 Plugins、查看 Logs、修改視覺化設定與查看 Statistics。預設啟動後可以從瀏覽器進入管理介面，首次啟動會產生隨機初始密碼；WebUI 目前也支援基於 TOTP 的 Two-factor Authentication。

這點對 Self-hosted Agent 很重要。

不少開源 Bot Framework 仍高度依賴 `.env` 、YAML 與 Command Line，對非開發者的管理成本比較高。AstrBot 雖然仍保留 CLI 與 Configuration File，但把大量日常管理工作移到 Dashboard，對家庭、社群與小型團隊部署會比較友善。

模型方面，AstrBot 並沒有把自己綁死在單一 Provider。

官方首頁明確列出 OpenAI、Anthropic、Gemini 等大型模型，同時文件也提供 Ollama、LM Studio 等整合方式。這代表使用者可以選擇雲端 API，也能依硬體與需求研究 Local LLM。

更特別的是 Agent Runner 本身也可以更換。

除了 AstrBot Built-in Agent Runner，官方文件目前提供 Dify、Coze、Alibaba Bailian 與 DeerFlow 的整合。也就是說，如果團隊已經在其他 Agent Platform 上建立 Workflow，AstrBot 可以更偏向 Messaging Gateway 與 Runtime Infrastructure，而不是要求所有 Agent Logic 都重新搬進 AstrBot。

這使 AstrBot 的角色可以依部署方式產生變化：

`簡單模式 → AstrBot 本身就是 AI Bot`

`進階模式 → AstrBot + Built-in Agent + MCP + Plugins`

`既有 Agent 平台 → Dify / Coze 等負責 Agent，AstrBot 負責 IM Integration`

對不同成熟度的團隊來說，這種彈性比固定只能使用單一 Agent Runtime 更實際。

部署方式也相當多元。

官方目前推薦熟悉 Command Line、想快速體驗的使用者透過 `uv` 安裝；較重視正式環境穩定性的部署則推薦 Docker／Docker Compose。桌面使用者可以選 AstrBot App；需要多 Instance 隔離則有 AstrBot Launcher。

另外還有 Kubernetes、BT Panel、1Panel、CasaOS 與 Manual Deployment 等方式。CasaOS 的支援尤其適合 NAS、Home Server 與 Home Lab 使用者，因為這類環境很常被拿來長時間運行私人 Bot。

因此 AstrBot 並不是只能架在開發者筆電上的 Python Script，而是明確朝 24/7 Running Service 的方向設計。

CLI 目前也相當完整。除了 `astrbot init` 與 `astrbot run` ，還能直接從 CLI 修改 Timezone、Log Level、Dashboard Port、Username、Password 等設定；Plugin 也可以搜尋、安裝、更新與移除。

從使用族群來看，AstrBot 特別適合幾種需求。

第一種是個人 AI Assistant。使用者可以把 Agent 放進 Telegram、LINE、Discord 等每天本來就會開啟的 App，不需要為私人 AI 再建立另一套操作習慣。

第二種是社群 Bot。Discord、QQ、Telegram 等群組本來就大量使用 Bot，AstrBot 可以進一步把 LLM、Knowledge Base 與 Agent Tool Use 加入既有社群。

第三種是企業內部助手。企業微信、飛書、釘釘、Slack、Mattermost 等整合，使公司可以把文件問答、內部知識搜尋與 Automation 放進原本的工作通訊環境。

第四種則是 Agent Developer。這群使用者可能不在意「聊天陪伴」，而是需要一套已經處理 Messaging Adapter、Plugin、WebUI、MCP、Knowledge、Sandbox 與 Model Provider 的基礎設施，再把自己的 Agent Logic 建立在上面。

如果和 Dify 比較，AstrBot 的重點更偏向 Messaging-first。Dify 強項是建立 LLM Application 與 Workflow；AstrBot 則非常重視讓 Agent 真正出現在 QQ、Telegram、Discord、Slack 等聊天平台。兩者甚至可以直接整合，因此不一定是競爭關係。

如果和 Open WebUI 類工具比較，差異同樣明顯。WebUI 型工具主要讓使用者打開瀏覽器與模型聊天；AstrBot 雖然也提供 ChatUI，但它更核心的能力是多平台 Messaging Integration 與 Bot／Agent Runtime。

如果和自己用 Python 寫 Telegram Bot + OpenAI API 比較，AstrBot 的初始架構會複雜很多，但也省去了日後自己建立 Plugin System、Dashboard、Knowledge Base、MCP、Model Switching、Sandbox 與多聊天平台 Adapter 的工作。

因此，小型一次性 Bot 未必需要 AstrBot；但如果預期 Bot 會長期運行、持續增加功能，或者同一套 Agent 要服務多個 Messaging Platform，Framework 的價值就會開始出現。

安全性方面也需要注意。AstrBot 能安裝第三方 Plugin，而官方文件明確提醒，團隊無法完全保證 Plugin Market 中每一個 Plugin 的安全性。因此正式環境安裝外掛前，尤其會接觸私人訊息、企業文件、API Token 或 Shell 的環境，仍應檢查來源與程式碼，而不是看到 Plugin Market 就直接視為官方安全審核。

同樣地，Agent Sandbox 提供隔離能力，不代表部署者可以忽略 Server Security。只要 Bot 能接收外部訊息、連接 LLM API、使用 MCP 或執行 Tool Call，就需要考慮 Authentication、Permission、Network Exposure、Secrets 與 Prompt Injection 等風險。

授權也是部署前值得注意的一點。

AstrBot 本身採 AGPL-3.0 License。官方說明指出，如果修改 AstrBot 並利用修改版本提供商業 Network Service，必須依授權要求公開相關修改；專案另外提供 EULA，因此正式採用前也應一併閱讀，而不是只看到「Free & Open Source」就忽略授權條件。

截至目前，AstrBot GitHub 已約有 40K Stars、數千個 Fork，Repository 累積超過五千次 Commit，並提供繁體中文、簡體中文、英文、日文、法文、西班牙文與俄文 README。從文件涵蓋 Deployment、Messaging Platform、AI Integration、Usage 與 Plugin Development 的完整程度來看，它已經明顯超過早期「LLM 聊天機器人範例專案」的規模。

整體而言，AstrBot 最值得注意的地方，是把「AI Agent」從獨立聊天網站帶回使用者原本就在使用的 IM 平台。模型可以換、Agent Runner 可以換、工具可以透過 MCP 增加、能力可以透過 1000+ Plugins 擴充，而 Telegram、Discord、LINE、Slack、企業微信等聊天平台則成為 Agent 與人互動的入口。

如果只是需要一個最簡單的 API Chatbot，AstrBot 可能顯得功能過多；但如果目標是架設一個能長期運作、跨平台、可以使用工具與知識庫，而且未來還會持續增加能力的私人或團隊 AI Agent，AstrBot 提供的是一套相當完整的開源基礎架構。