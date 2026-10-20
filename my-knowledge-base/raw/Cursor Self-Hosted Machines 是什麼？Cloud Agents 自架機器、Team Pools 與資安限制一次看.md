---
title: "Cursor Self-Hosted Machines 是什麼？Cloud Agents 自架機器、Team Pools 與資安限制一次看"
source: "https://www.aiposthub.com/cursor-self-hosted-machines-cloud-agents/"
author:
  - "[[Philo]]"
published: 2026-09-03
created: 2026-09-04
description: "Cursor 推出 Self-Hosted Machines，讓 Cloud Agents 在企業自有機器、GPU、Mac 或 Kubernetes 執行工具。本文解析 Team Pools、自動擴縮、資料邊界與成本。"
tags:
  - "clippings"
status: processed
---
Cursor 在 2026 年 9 月 2 日宣布，Cloud Agents 現在可以在企業管理的基礎設施上執行，還能把多台機器組成會隨需求擴縮的 Team Pools。

這項功能解決的是一個很實際的企業問題：AI Coding Agent 想改程式、跑測試，往往需要碰到內部程式庫、私有服務、特殊建置環境，甚至 GPU 或 Mac，但這些資源不一定適合搬到外部雲端環境。

不過，Cursor Self-Hosted Machines 並不是把整套 AI 搬進公司內網。企業接手的是「工具執行環境」，模型推論、規劃與 Agent loop 仍由 Cursor 在雲端處理。這個差別會直接影響資安評估，也決定它適不適合你的團隊。

本文會說明 Cursor Self-Hosted Machines 的運作方式、Team Pools 如何自動擴縮，以及企業導入前最容易忽略的資料、成本與維運限制。

## Cursor Self-Hosted Machines 是什麼？

Cursor Self-Hosted Machines 是 Cloud Agents 的一種執行方式。Cloud Agent 是能在背景接收開發任務、讀取程式碼、修改檔案、執行指令與測試結果的 AI 程式代理人。

過去使用 Cursor Cloud Agents 時，程式碼與工具主要在 Cursor 管理的隔離式雲端虛擬機器中執行。現在企業可以改用自己的筆電、開發機、雲端 VM、容器、Mac 或 Kubernetes 叢集，讓 Agent 在公司指定的環境工作。

Cursor 將這台執行任務的機器稱為 Worker。管理者在機器上安裝 Cursor CLI，執行 `agent worker start` 後，Worker 會主動建立一條連向 Cursor 雲端的 HTTPS 連線。Cursor 不需要從外部主動連入公司的網路，因此不必為它開放入站連接埠或提供公開 IP。

任務開始後，雙方的分工如下：

| 工作內容 | 執行位置 |
| --- | --- |
| 模型推論、任務規劃、Agent loop | Cursor 雲端 |
| 讀寫程式碼、執行終端指令、跑測試 | 企業管理的 Worker |
| 連接內部服務、套件庫與本機 MCP 工具 | 企業管理的 Worker |
| 顯示任務、管理 Agent、查看成果 | Cursor 桌面版、網頁、手機或整合工具 |

換句話說，Self-Hosted Machines 改變的是「工作在哪一台機器執行」，不是把 Cursor 的大語言模型與控制系統完整私有化部署。

![Cursor Self-Hosted Machines 架構圖，Agent loop 在 Cursor 雲端，工具在企業網路執行](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/cursor-agent-loop.png)

Self-Hosted Machines 只移動工具執行位置；模型推論、規劃與 Agent loop 仍在 Cursor 雲端。 圖片來源： Cursor 官方公告 。

## 為什麼企業需要讓 Cursor Cloud Agents 跑在自己的機器？

一般團隊使用 Cursor 管理的 Cloud Agents，維護成本最低。Cursor 會負責虛擬機器的配置、隔離、啟動、快照與容量管理，團隊只要準備程式庫、依賴套件、Secrets 與網路權限。

Self-Hosted Machines 真正有價值的情境，是現有開發環境很難搬進標準雲端虛擬機器。

### 需要直接連接公司內部服務

有些 Agent 任務不只會讀 GitHub 上的程式碼，還要存取內部 API。API 是讓不同軟體互相交換資料或呼叫功能的介面。除此之外，Agent 也可能需要使用私有套件庫、測試資料庫、CI 系統，或只能從公司網路連線的服務。

如果這些資源不能透過 PrivateLink、Tailscale 或其他私有網路方案安全地提供給 Cursor 管理的環境，讓 Worker 直接執行在內部網路，通常會比重新打通一整套外部存取路徑更實際。

### 需要 GPU、Mac 或特殊作業系統

標準雲端開發環境不一定有團隊要的硬體。例如 AI 團隊可能需要 GPU 驗證模型，iOS 團隊需要真正的 Mac 與 Apple silicon，舊系統則可能依賴特定作業系統、驅動程式或大型本機快取。

Self-Hosted Machines 讓 Agent 使用企業已經配置完成的環境，而不是為了配合 Agent，重新包裝所有建置條件。

![Cursor Cloud 與企業自有伺服器、VM、Kubernetes 及公有雲的部署方式比較](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/cursor-deployment-options.png)

Cloud Agents 可使用 Cursor 管理的機器，也可在企業指定的伺服器、VM、Kubernetes 或公有雲環境執行。 圖片來源： Cursor 官方公告 。

### 政策要求程式碼與工具執行留在企業邊界內

部分公司的書面政策會要求完整程式庫、建置快取、機器憑證與工具執行都留在受控環境。Self-Hosted Machines 可以讓這些資產保留在企業提供的機器上。

但這不代表所有任務資料都不會離開內網。這項限制必須在資安審查時另外處理，後文會詳細說明。

## My Machines 與 Team Pools 有什麼差別？

Cursor 把 Self-Hosted Machines 分成兩種使用方式。My Machines 適合個人或小規模測試，Team Pools 則是給需要共享容量與集中管理的企業團隊。

| 比較項目 | My Machines | Team Pools |
| --- | --- | --- |
| 適合對象 | 個人開發者、小型測試 | 團隊與企業 |
| 常見設備 | 個人筆電、開發機、單台 VM | 多台 VM、容器、GPU 主機、Mac 或 Kubernetes |
| 驗證方式 | 個人登入或個人 API key | Service account API key |
| 資源分配 | 連接特定個人的機器 | 從共享 Worker 池分配可用機器 |
| 擴充方式 | 手動增加機器 | 可搭配 Controller 或 API 自動擴縮 |
| 方案限制 | 依官方帳號與設定為準 | 需要 Cursor Enterprise |

如果只是想確認內部專案能不能正常跑，先用 My Machines 做小型驗證比較快。當團隊開始遇到多人排隊、不同硬體需求、集中權限與自動擴縮問題，再投入 Team Pools，維護成本會更合理。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/09/cursor-self-hosted-x-demo.mp4" width="1370" height="720" controls=""></video>

Cursor 官方示範從執行位置選單切換到自己的 Mac。 影片來源： Cursor 官方 X 貼文 。

## Team Pools 如何讓 Agent 隨需求自動擴縮？

Team Pool 可以理解成一個有名稱的 Worker 等候區。團隊可建立 `gpu` 、 `ios` 或 `staging` 等不同 Pool，讓任務依照程式庫、團隊與硬體需求，送到正確的機器。

每個 Agent 工作階段一次會取得一台專用 Worker。如果 Pool 裡有閒置機器，Worker 就會接下任務。若沒有可用容量，請求會留在佇列中等待新機器加入。

自動擴縮並不是 Cursor 直接進入企業雲端帳號開機。企業需要準備自己的啟動腳本或基礎設施控制流程，再由 Cursor 的 Worker Controller 監看待處理請求，呼叫團隊提供的腳本建立新 Worker。企業也可以直接使用 Cloud Agents API，依照佇列深度與使用率設計自己的擴縮邏輯。

這套架構的價值在於，團隊不用為尖峰需求長期保留所有機器。任務結束後，Worker 可在設定的閒置時間到期後釋放，讓雲端平台回收容量。若工作環境很大，也能搭配快照或休眠，在後續任務到來時恢復原本狀態，減少重新安裝依賴套件的時間。

Cursor 也提供 Kubernetes 的 Helm chart 與 Operator，可管理 WorkerDeployment、暖機容量、滾動更新與 Token 輪替。不過，這已經是正式基礎設施，而不是打開 Cursor 設定就會自動完成的功能。

## Self-Hosted Machines 能做到哪些事？

Agent 在企業 Worker 上不只可以修改程式碼與執行測試，也能使用該環境能連到的內部工具與本機 MCP Server。MCP 是一套讓 AI 連接資料庫、API 與企業工具的標準介面。

如果 Worker 已安裝桌面環境與必要套件，Agent 還能操作瀏覽器。Linux 與 macOS 都支援 Computer Use，可進行點擊、截圖與瀏覽器操作，使用者也能觀看桌面或接手控制。

這讓 Agent 有機會完成更接近真實工程工作的流程，例如：

- 修改內部系統後，在公司測試環境執行整合測試。
- 使用 GPU 跑模型驗證或效能測試。
- 在 Mac 上編譯與測試 iOS、macOS 專案。
- 開啟內部網站，操作瀏覽器確認介面是否正常。
- 在多個程式庫之間修改前端、後端與共用套件。

但能不能穩定完成這些任務，仍取決於企業是否準備好依賴套件、權限、測試資料與可重現的工作流程。換一台更強的機器，不會自動修復混亂的開發環境。

## 最大的誤解：Self-Hosted 不等於資料完全不離開內網

Self-Hosted Machines 這個名稱很容易讓人聯想到完整的 On-Premises 部署，也就是模型、控制系統與資料處理全部留在公司內部。但 Cursor 官方文件清楚說明，Agent loop、推論與規劃仍在 Cursor 雲端。

完整的程式庫、建置快取與機器本機憑證會留在 Worker。不過，為了讓雲端模型決定下一步，Worker 仍會把任務所需內容送給 Cursor，其中可能包含檔案內容、終端輸出、程式差異、截圖、本機 MCP 結果與路由資訊。

Agent 產生的截圖、影片與日誌參照等 Artifacts，也可能上傳到 Cursor 管理的儲存空間，方便在 Pull Request 與管理介面查看。企業可以阻擋特定 Artifact 網域來停用上傳，但這只會關閉成果檔案的顯示，不會讓模型推論改成在內部執行。

Privacy Mode 可套用到 Self-Hosted Machines。依 Cursor 文件，啟用後，Worker 傳出的程式碼不會被 Cursor 或模型供應商拿去訓練。然而，「不用於訓練」與「資料不會被處理或傳出」是兩件不同的事。

因此，資安團隊不能只問 Worker 架在哪裡，還要逐項確認：

- 哪些檔案、指令輸出與 MCP 結果可能送往 Cursor？
- 截圖、錄影與日誌是否可能包含 Token、客戶資料或內部介面？
- 哪些程式庫與資料夾允許 Agent 存取？
- Secrets 是否可能被指令輸出、差異檔或 Artifact 意外帶出？
- 公司政策是否允許推論與任務紀錄由外部服務處理？

對需要「任何程式內容都不能離開內網」的組織來說，Self-Hosted Machines 仍未必符合要求。它解決的是執行位置與資源控制，不是完整的離線 AI 或本地模型部署。

## 導入 Self-Hosted Machines 的成本不只來自 Cursor

Cursor 管理的 Cloud Agents 已包含執行基礎設施。改用 Self-Hosted Machines 後，模型仍按照 Cursor 的方案與用量計價，企業還要另外負擔自己的 VM、容器、GPU、Mac、儲存與網路成本。

如果採用 Team Pools，還會增加幾項容易被低估的工作：

- 建立並更新 Worker 映像檔。
- 管理 Service account API key 與 Secrets。
- 設計網路白名單、權限與程式庫隔離。
- 處理排隊、擴縮、失敗重試與 Worker 健康狀態。
- 保存或清除工作區，避免不同任務之間留下敏感狀態。
- 監控 GPU、Mac 與雲端 VM 的使用率，避免閒置成本失控。

所以，Self-Hosted Machines 並不是 Cursor Cloud Agents 的低成本版本。它比較像用額外的維運成本，換取特殊硬體、內部網路與執行環境的控制權。

## Cursor Self-Hosted Machines 值得導入嗎？

我的判斷是：這是一項對大型工程團隊有實質價值的功能，但多數公司不該只因為看到「Self-Hosted」就直接導入。

如果團隊只是希望 Cloud Agent 能存取私有程式庫與部分內部服務，先評估 Cursor 管理環境搭配網路控管、PrivateLink 或 Tailscale，通常比較快，也不必自己維護 Worker Fleet。Cursor 官方也把管理式 Cloud Agents 列為多數團隊的預設選擇。

只有在下列條件成立時，Self-Hosted Machines 的優勢才會明顯：

- 公司政策明確要求程式庫與工具執行留在企業控制的邊界。
- Agent 必須直接使用外部環境難以取得的內部服務。
- 團隊需要 GPU、Mac、特殊驅動或難以容器化的建置流程。
- Agent 使用量已經大到需要共享 Worker 池、集中路由與自動擴縮。
![Cursor 官方 Self-Hosted Machines 與代管 Cloud Agents 選擇流程圖](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/cursor-self-hosted-decision.png)

官方建議先依企業邊界、內部網路與特殊硬體需求，判斷是否真的需要 Self-Hosted Machines。 圖片來源： Cursor 官方公告 。

反過來說，小型團隊、環境單純的 Web 專案，或沒有專人管理雲端基礎設施的公司，直接使用 Cursor 管理的 Cloud Agents 會更實際。

最可能改變這個判斷的條件，是 Cursor 未來是否提供完整的私有推論或更細緻的資料留存控制。如果模型與 Agent loop 也能留在企業環境，Self-Hosted 的資安意義才會從「自主管理執行機器」進一步變成「完整掌控 AI 工作流程」。

## 企業可以怎麼開始？

最實際的做法不是一開始就建 Kubernetes 叢集，而是挑一個不涉及正式客戶資料的內部專案，用 My Machines 完成小型驗證。

第一階段先確認三件事：Agent 能不能安裝依賴並跑完測試、需要傳出哪些任務內容，以及權限縮到多小仍能完成工作。若結果穩定，再把同一套環境包成可重建的 VM 或容器映像。

第二階段才測 Team Pool。先建立單一用途的 Pool，例如只服務某個測試專案，不要一開始就讓任意程式庫共用同一批 Worker。等到權限、Secrets、清理與監控都能穩定運作，再加入自動擴縮與跨程式庫支援。

這種順序雖然不像一次導入整套平台那麼漂亮，卻比較容易找出真正的阻力。對企業而言，Agent 能不能工作只是第一關。能不能在可控權限、合理成本與可追蹤紀錄下持續工作，才是能否正式採用的關鍵。

## 常見問題

### Cursor Self-Hosted Machines 是把 Cursor 完整部署在公司內部嗎？

不是。檔案修改、指令與測試等工具操作會在企業管理的 Worker 上執行，但模型推論、規劃與 Agent loop 仍在 Cursor 雲端。

### 使用 Self-Hosted Machines 後，程式碼會離開公司網路嗎？

完整程式庫會留在 Worker，但任務需要的檔案內容、終端輸出、差異、截圖與 MCP 結果可能傳給 Cursor 處理。因此不能把它理解成完全離線或零資料外傳的方案。

### Team Pools 可以自動增加與關閉機器嗎？

可以，但企業必須提供自己的啟動腳本或擴縮控制。Cursor 的 Worker Controller 或 Cloud Agents API 會提供請求佇列與 Worker 狀態，真正建立、休眠與關閉機器的流程仍由企業管理。

### Self-Hosted Machines 支援 GPU 與 Mac 嗎？

支援。Worker 可以執行在企業自己的 GPU 主機、Mac、VM、容器或 Kubernetes 環境，前提是該機器能安裝 Cursor CLI、連出 HTTPS，並具備任務需要的工具與權限。

### Team Pools 要使用哪一種 Cursor 方案？

依 2026 年 9 月 3 日的官方文件，Team Pools 需要 Cursor Enterprise，並使用 Service account API key 驗證。Self-Hosted Machines 的硬體與雲端基礎設施費用由企業另外負擔。

## 結語：Cursor 把 Cloud Agent 帶進企業環境，但沒有把雲端移除

Cursor Self-Hosted Machines 最重要的進展，是讓企業不必在「使用 Cloud Agent」與「保留現有開發環境」之間二選一。Agent 現在可以使用公司內部服務、特殊硬體、Mac 與既有建置流程，Team Pools 也讓這套能力能夠從個人機器擴展到共享基礎設施。

但它不是完整私有化的 Cursor。推論與規劃仍在雲端，部分任務內容也會傳回 Cursor。企業若忽略這條界線，很容易高估它對資料隔離的改善。

對多數團隊來說，管理式 Cloud Agents 仍是維護成本最低的起點。對真的受限於內網、硬體或政策的企業，Self-Hosted Machines 才是值得付出額外維運成本的選項。