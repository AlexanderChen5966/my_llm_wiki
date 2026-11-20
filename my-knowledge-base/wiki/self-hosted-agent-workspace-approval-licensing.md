---
title: 自架代理工作區怎麼看：OpenBot 1.0 與 Open WebUI 0.13.1——程式代理接進來之後，工具核准、資料路徑與授權要分層確認
date: 2026-11-19
tags: [ai/agent, ai/workflow, type/comparison]
source: https://www.aiposthub.com/copilotkit-openbot-10-self-hosting-license-approvals/（aiposthub／Philo〈OpenBot 1.0 發布：自架 AI 同事工作區，模型、核准與授權怎麼分？〉，2026-11-16；依 11-16 官網與儲存庫整理，作者未實際執行）
source_2: https://www.aiposthub.com/open-webui-0131-tether-coding-agents-upgrade-license/（aiposthub／Philo〈Open WebUI0.13.1 讓 Claude Code 與 Codex 進模型選單：Tether、工具核准與升級怎麼看？〉，2026-11-09；依 GitHub 發行說明與 Tether LICENSE 整理，未實測）
source_3: https://www.aiposthub.com/openclaw-enterprise-persistent-agent-pilot-guide/（aiposthub〈OpenClaw Enterprise 開放企業代理平臺〉，2026-10-01；補充資料）
source_4: https://www.aiposthub.com/openduet-self-host-dependencies/（aiposthub〈OpenDuet 自架教學：開源助理需要哪些服務，模型費用與資料流向怎麼查〉，2026-11-11；補充資料）
official_refs:
  - https://github.com/open-webui/open-webui/releases/tag/v0.13.1
  - https://github.com/open-webui/tether/blob/main/LICENSE
type: article
status: processed
asset_value: 60
---

兩個同期的自架工作區更新：**OpenBot 1.0**（CopilotKit，2026-11-10 發布）把代理、工具、人工核准與紀錄整合成「AI 同事工作區」；**Open WebUI 0.13.1**（2026-11-08）讓 Open Terminal 把 Claude Code、Codex、Pi 等程式代理列進模型選單，並推出桌面伴隨程式 Tether 把自己的電腦接進來。兩篇皆為 aiposthub 二手整理、作者未實測；版本、限制數字與授權條件是 2026-11 中旬快照。

## T（核心論點）

> **「能自架」只回答了部署位置，沒回答四件事**：代理能碰哪些工具（授權）、這一次行動誰放行（核准）、資料實際送去哪裡（路徑）、每個元件用什麼條款（授權分層）。四件要分開確認，不能從「自己的伺服器」一句話推出來。

三個支撐點：

1. **接上代理≠取得工具能力；技能是指令，不是權限**（OpenBot 官方說法）：寫一份「請用某工具」的技能，不會自動授予工具存取。讀與寫要分開授權
2. **核准要放在執行路徑上，且失敗偏向停止**：OpenBot 拒絕規則優先、政策缺漏或損壞不放行；Open WebUI 的 Auto 工具審查出錯／逾時／內容太多時**回退人工核准**，不把「審查失敗」當「允許」
3. **自架≠資料不外送、開源≠全部免費**：雲端模型、線上工具、甚至 Auto 審查用的評分 API 都是另一條對外路徑；OpenBot 的 MIT 只涵蓋應用模板，Open WebUI 的 Tether 免費只限個人非商用

## B（適用場景）

**適用：**
- 已在自架 Open WebUI，想把對話接到實際專案檔案與程式代理
- 評估要不要讓團隊共用一個代理工作區（多人登入、工具權限、稽核）
- 盤點自架方案的總成本：主程式、記憶／學習元件、桌面伴隨程式、模型費用分開算

**不適用 / 注意：**
- 🔴 **單人預設不能直接公開**：OpenBot 原始碼的 single-user 設定讓能連到部署的人都成為管理者；官方要求給別人用之前先配登入與加密連線
- 🔴 **Open WebUI 0.13.1 含資料庫變更、不支援滾動更新**：升級前備份 DB；多工作程序／多伺服器／負載平衡要**全部實例一起更新**。同日 0.13.2 修 HTTP 下聊天卡載入，但沒改變這個條件；0.13.1 另含安全修正，細節待 CVE 公布
- OpenBot 平台狀態：Mac／Windows 桌面可下載；企業方案、手機、Slack、Teams 為**早期開放**；網頁入口**即將推出**

## R（操作步驟）

### 兩者的分層對照

| 層 | OpenBot 1.0 | Open WebUI 0.13.1 |
|---|---|---|
| 代理怎麼接 | **AG-UI**（代理↔介面互動規則）接既有代理或內建 Bot | Open Terminal 把程式代理列成「模型」，**每個對話只用一個代理**；終端離線時對話回到預設模型 |
| 工具怎麼接 | **MCP**；讀與寫分開授權 | 檔案與工具由**所選終端**提供，不是模型選單決定 |
| 本機怎麼接 | 桌面版；完全本地需 OpenBot＋Intelligence＋相容模型都在本機 | **Tether 主動連出**接本機（不開對外埠），出現在 Your devices；會話預設 90 天未用才到期 |
| 核准 | 工作區閘道：政策→稽核→執行；拒絕優先；人接管登入／2FA 時 Bot 動作被**拒絕**（不排隊） | 完整存取／要求核准／**Auto**（另一模型審查，預設關閉、管理員啟用）；審不出結果→人工核准 |
| 工具迴圈保護 | 排程連續失敗 10 次停用 | 同參數連續失敗 2 次即結束工具迴圈；回覆被 token 上限截斷時，未完成的工具呼叫不執行 |
| 記憶／排程 | 交給 CopilotKit Intelligence；排程以使用者身分執行、最短 15 分鐘、最多 20 項啟用 | 記憶回想、技能列表、知識清單改成使用者訊息後的**隱藏工具結果**（仍送模型），讓開頭穩定利於提示快取（**無官方省費比例**） |
| 授權 | 模板 MIT；Intelligence 另授權（個人本地免費、K8s 評估試用、正式部署需生產授權；本地授權 30 天續期） | 主程式授權重點是保留品牌（30 天內 ≤50 使用者有品牌修改例外，不是只能 50 人用）；**Tether 免費只限個人非商用**，組織正式使用需企業授權 |

### Open WebUI 0.13.1 的讀檔行為變更（升級前要知道）

- 原生工具呼叫時**附檔預載預設關閉**，改由模型用檔案工具讀；模型沒工具能力或沒開 Files 工具，就要另外開預載才看得到附檔
- 舊式工具呼叫、非網頁聊天的 API 請求保留原行為 → 同一個檔案在不同呼叫方式下，進不進模型不一樣
- 擷取文字少於預設 50,000 字元的檔案改用全文；檢索預設 top 3→10、切塊 1000→1024；已自存的設定不被覆蓋，舊知識要**重建索引**才會改切法
- 自訂處理程式可能看到「最後一則訊息變成工具結果」，依賴舊排列的擴充要調整
- 代理對話不支援重新生成、接續生成、編輯舊訊息重送（後續提問仍可）

### 試用驗收法（兩篇共同建議）

1. 第一項工作只讀一份熟悉文件、產出有來源的待辦，由人核對，不給寫入權
2. 加一次需要核准的**低影響**操作，看提案→核准→完成是否一致；代理說「已執行」要到**工具端**核對紀錄
3. 故意做一個應被拒絕的動作，核對畫面與稽核紀錄是否一致
4. 畫出資料路徑圖：經過哪個模型、哪個工具、記憶存哪、Auto 審查送到哪個評分 API
5. 模型費用與 Intelligence／授權費用**分開記帳**；加排程或學習後支出變動才找得到源頭
6. 多人之前：誰能登入、哪個 Bot 有哪些工具、操作以誰的身分執行，三者一起檢查

## C（關鍵行動）

- [ ] 若在公司評估 Open WebUI＋Tether，先確認 Tether 的使用身分是否落在「個人非商用」之外
- [ ] 任何自架工作區開放給第二個人之前，先關掉單人預設、配好登入與 HTTPS
- [ ] 啟用 Auto 類「模型審查工具呼叫」前，確認評分服務的位置與會送出的內容（工具名稱、參數、最近幾則訊息）

## 原始摘要

**OpenBot 1.0**：CopilotKit 的可自架代理工作區，AG-UI 接代理、MCP 接工具，政策／人工核准／稽核放進執行路徑；技能不是權限；記憶與學習由另外授權的 Intelligence 支援；自架仍會把任務資料送給所用的外部模型與線上工具；單人設定不可直接公開給團隊。

**Open WebUI 0.13.1**：Open Terminal 讓 Claude Code、Codex 等程式代理進模型選單，Tether 把本機接進工作區；新增 Auto 工具審查（失敗回退人工）、為提示快取調整訊息排列、附檔預載預設關閉；含資料庫變更不支援滾動更新；Tether 授權與主程式分開。

**合併判斷（2026-11-19）**：[[odysseus-self-hosted-ai-workspace]] 是單一自架工作區的產品介紹，[[agent-tool-authorization-gatekeeper]] 是工具授權的通用框架，兩者都不涵蓋「自架工作區＋程式代理＋核准＋授權分層」這條組合論點，故合併兩篇新建，前兩者只作相關連結。

## 補充資料

**2026-11-19 補充（來源：aiposthub〈OpenClaw Enterprise 開放企業代理平臺：免費使用，為什麼仍要先做內部試點？〉，https://www.aiposthub.com/openclaw-enterprise-persistent-agent-pilot-guide/ ，2026-10-01）**：二手整理官方 X 發布，站方未部署；OCE 官方定位為 1.0 前的開放開發階段、適合內部試點。

- **第三個自架例子，對象換成「持續運作的代理」**：OpenClaw Enterprise（OCE，2026-09-29 宣布）是開源控制平臺，主打自架、多租戶、安全邊界與稽核；官方稱最初起於 OpenAI、捐給 OpenClaw Foundation，與 Red Hat、NVIDIA 協作（參與者名單不能取代自己環境的驗證）
- **控制平臺 ≠ 模型 ≠ 執行環境**：三者可替換，但每換一個元件就可能改變效果與邊界——模型與工具版本要進變更管理；採購時分開問平臺、模型費用、執行環境
- 呼應本條「開源≠全部免費」：平臺免費使用，不代表模型呼叫、紀錄儲存、維護與人工覆核免費
- **任務邊界表**：接到什麼訊號、讀哪些資料、可產生哪些輸出、哪些動作必須人決定；權限**同時涵蓋輸入與輸出**（避免把受限資訊整理到更廣的地方）；第一輪選低影響、可停止的任務（例：只讀已授權的建置紀錄、產待辦摘要，修復由人確認）
- **稽核紀錄要能回答**：誰提出任務、用哪個身分、讀了哪些來源、呼叫哪些工具、改變了什麼；保留完整機器紀錄＋可連回證據的事件摘要
- 試點結果分**完成／未完成／需要覆核**，證據不足時回報不確定，不追求表面完成率；指定維護者（更新、權限申請、定期檢查）

**2026-11-19 補充（來源：aiposthub〈OpenDuet 自架教學：開源助理需要哪些服務，模型費用與資料流向怎麼查〉，https://www.aiposthub.com/openduet-self-host-dependencies/ ，2026-11-11）**：依 11-11 官方 README 與設定文件整理，站方未實測。

- **OpenDuet 已改定位為 OpenBot 的可選介面外觀**：原倉庫留作社群維護的起始範本；要多人登入、個別權限、稽核 → 走 OpenBot。它是 CopilotKit 專案，**不是** Anthropic 的 Claude Duet
- 自架仍需 CopilotKit Intelligence 專案（對話與記憶）、資料庫、存憑證用的加密金鑰——呼應本條「記憶元件另授權、另一條資料路徑」
- **依賴表**（比「支援自架」更接近部署決定）：程式與執行環境（誰更新、備份、排錯）／模型服務（哪家、誰付費）／對話與記憶（存哪、怎麼刪與匯出）／電腦與工具（能開哪些網站、能寫哪些服務）
- 第一個試用任務：無敏感資料、可人工核對（例：整理指定公開頁面的更新、輸出帶來源的草稿），成功條件寫成「讀了哪些頁、找到哪些變動、結果放哪」

## 相關連結

- [[agent-tool-authorization-gatekeeper]] — 二元 allow/deny 是可達性不是授權；本條的 OpenBot 閘道與 Auto 審查是其產品實例
- [[odysseus-self-hosted-ai-workspace]] — 另一個開源自架 AI 工作區（產品介紹）
- [[generative-ui-a2ui-open-intelligent-ui]] — 同為 CopilotKit 產品的 Open Intelligent UI（生成式介面），本篇 OpenBot 是其後續
- [[notion-3-7-agent-skills]] — 對照：Notion 是「Skill 檢視權限＝執行權」，OpenBot 明說「技能是指令不是權限」
- [[agent-permission-and-os-sandbox]] — 代理權限與作業系統沙箱
- [[github-hot-ai-tools-10-selection-2026]] — 另提及 Open WebUI 的品牌授權要求
- [[claude-code-cloud-scheduled-tasks-practice]] — 另一種「讓程式代理不在電腦前也能跑」的做法（雲端 session＋排程）
