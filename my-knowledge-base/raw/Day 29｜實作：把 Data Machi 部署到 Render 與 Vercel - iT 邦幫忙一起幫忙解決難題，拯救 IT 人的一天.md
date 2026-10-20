---
title: "Day 29｜實作：把 Data Machi 部署到 Render 與 Vercel - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10408189"
author:
  - "[[iThome]]"
published:
created: 2026-09-11
description: "前面所有實作都還可以在自己的電腦上完成，但如果 Data Machi 要真正讓其他人使用，就必須離開 localhost，把前端與後端部署到可以公開存取的環境。到了這一步，我們處理的已經不只是 AI，..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 30

0

[AI 自動化](https://ithelp.ithome.com.tw/2026ironman/ai-automation)

### Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI系列 第 30 篇

## Day 29｜實作：把 Data Machi 部署到 Render 與 Vercel

- 分享至

前面所有實作都還可以在自己的電腦上完成，但如果 Data Machi 要真正讓其他人使用，就必須離開 `localhost` ，把前端與後端部署到可以公開存取的環境。到了這一步，我們處理的已經不只是 AI，而是一個真正 Web Application 上線時會遇到的問題：程式碼版本、環境變數、Frontend 與 Backend 的連線、CORS、正式環境權限，以及部署之後如何確認每一項能力仍然正常。

這篇會以 **Render 部署 FastAPI Backend、Vercel 部署 React Frontend** 為例，把目前的 Data Machi 從 GitHub 一路部署成可以從瀏覽器使用的產品。真正重要的不是一定要選這兩個平台，而是理解前後端分離部署之後，各層之間是怎麼連起來的。

整體架構可以先理解成：

**Browser → Vercel Frontend → Render Backend → Gemini / Google Sheets / Document RAG / External Tools**

Browser 不直接拿 Gemini API Key，也不直接使用 Google Service Account。前端只呼叫 Backend，再由 Backend 在受信任的環境中使用 Secret 與各種企業 Tool。這也延續 Day 28 的安全原則：真正的 Credential 應該留在 Backend，而不是跟著 React 程式送進使用者瀏覽器。

## 先確認 GitHub 才是正式版本來源

正式部署之前，第一件事情不是打開 Render 或 Vercel，而是先確認 GitHub 上的程式碼就是準備上線的版本。

本機開發很容易出現一種情況：自己的電腦明明已經修改完成，甚至測試成功，但某些檔案還沒有 Commit 或 Push。這時 Render 與 Vercel 從 GitHub 拉下來的仍然是舊版本，最後就會出現「本機明明正常，部署後卻還是舊功能」的問題。

因此要建立一個基本觀念：

**Local 是開發環境，GitHub 才是部署版本的來源。**

如果 Repository 同時包含 Frontend 與 Backend，例如：

```markdown
data-machi/
├── frontend/
├── backend/
└── README.md
```

部署前也應該先確認兩個資料夾各自的 Root Directory、Dependency File 與啟動方式。Backend 可能使用 `requirements.txt` ，Frontend 則可能使用 `package.json` 。如果這一層沒有整理清楚，後面的部署平台就算設定正確，也不知道應該從哪個資料夾開始 Build。

## 第一步：先把 FastAPI Backend 部署到 Render

部署順序可以先從 Backend 開始，因為 Frontend 最後需要知道正式 API 位址。

在 Render 建立 Web Service 後，可以連接 GitHub Repository，指定 Backend 所在的 Root Directory，再設定 Build Command 與 Start Command。假設 FastAPI 放在 `backend` 資料夾，常見設定可能是：

```markdown
Root Directory: backend
Build Command: pip install -r requirements.txt
Start Command: uvicorn main:app --host 0.0.0.0 --port $PORT
```

真正的指令仍然要依自己的專案結構調整。例如 FastAPI `app` 如果不在 `main.py` ，Start Command 就要跟著修改。

這裡最容易忽略的是 `$PORT` 。本機可能習慣使用 `8000` ，但正式部署平台通常會提供自己的 Port，因此不要直接把本機 Port 寫死。Backend 應該讓 Uvicorn 綁定平台提供的環境變數，Render 才能正確把外部流量送進服務。

如果 Repository 已經存在 `render.yaml` ，也不要因為它曾經可以使用就直接假設永遠正確。隨著專案重構，Root Directory、Python Version、Build Command 或啟動檔案都可能已經改變，部署前仍然需要確認設定是否和現在的 Repository 一致。

## 把本機.env 搬到正式 Environment Variables

Backend 能成功 Build，只代表 Python Application 可以啟動；真正要使用 Gemini、Google Sheets 或其他 Tool，還需要正式環境的 Credential 與設定。

本機可能透過 `.env` 保存：

```markdown
GOOGLE_API_KEY=
GEMINI_MODEL=
GEMINI_FAST_MODEL=
GOOGLE_SERVICE_ACCOUNT_JSON=
GOOGLE_SHEET_KEY=
ALLOWED_ORIGINS=
```

正式環境則應該把這些值建立在 Render 的 Environment Variables 或 Secret 管理介面中，而不是把 `.env` 一起 Commit 到 GitHub。

Trello、Confluence、Groq 或其他選配 Tool 也是相同原則：只有正式環境真的需要使用時才增加對應 Credential，而且 Secret 應該只存在需要它的 Backend。

另外要注意，修改 Environment Variable 後，執行中的服務不一定會自動讀到新值。有些平台需要 Redeploy 或 Restart，因此遇到「我明明已經改 Key，為什麼還是使用舊設定」時，也應該確認最新設定是否真的進入目前部署版本。

## Backend 上線後，第一個測試不是問 AI

Render 顯示 Deploy Successful 後，不要立刻丟一個「請分析最近三個月資料並查相關政策與專案」的複雜問題。

第一個測試應該是 **Health Check（健康檢查）** 。

例如 Backend 可以提供簡單 Endpoint：

`GET /health`

正常時只需要回傳類似：

```json
{
  "status": "ok"
}
```

Health Check 的目的不是測 Gemini，也不是測 RAG，而是先回答最基本的問題：

**這個 Backend 現在活著嗎？**

如果 `/health` 都打不開，就沒有必要先檢查 Prompt、Google Sheet Permission 或 RAG Index。

確認 Backend 本身正常後，再逐層測試 Gemini、Google Sheets、PDF RAG，最後才測完整 Coordinator / LangGraph Workflow。這和前面一路使用的除錯原則完全一樣： **一次驗證一層，不要把所有功能一起測，再猜是哪裡壞掉。**

## Cold Start 也會影響使用者體驗

某些較低成本的 Hosting Plan 可能在一段時間沒有流量後縮減或暫停運算資源，下一次 Request 就需要先等待服務重新啟動。這會讓第一次回答明顯比後續 Request 慢。

從 Backend 角度看，服務最後仍然成功；但從使用者角度看，可能只是看到畫面一直 Loading。

這正好連回 Day 27 的 Agent UX：前端應該有明確的 Connecting / Loading State，而不是讓使用者不知道現在是 Agent 正在執行 Workflow，還是 Backend 本身正在啟動。

正式使用時，也需要根據預期使用量、延遲要求與平台方案評估是否可以接受這種行為，而不是只用「部署成功」當成唯一標準。

## 第二步：把 React Frontend 部署到 Vercel

Backend 正常之後，下一步才部署 Frontend。

在 Vercel 建立 Project 後，同樣連接 GitHub Repository，並指定 React Application 所在的 Root Directory，例如 `frontend` 。Vercel 會根據 `package.json` 安裝套件並進行 Build。

Frontend 最重要的一項正式環境設定，是：

**API 到底要打去哪裡？**

本機開發可能使用：

`http://localhost:8000`

正式環境則要改成 Render URL，例如：

```markdown
VITE_API_BASE_URL=https://your-backend.onrender.com
```

因此 Backend URL 不應該散落在 React 各個 Component 裡，更不應該直接 Hard Code。比較好的方式，是讓所有 API Request 都透過同一個 API Client 或 Environment Variable 取得 Base URL。

這樣 Local、Preview 與 Production 才可以在不修改程式碼的情況下使用不同 Backend。

## VITE\_ Environment Variable 並不是 Secret

這裡要再次延續 Day 28 的安全原則。

Vite 會把特定前綴的 Environment Variable 放進 Frontend Build，最後這些值可以在 Browser 裡被讀取。因此像：

`VITE_API_BASE_URL`

沒有問題，因為 Backend URL 本來就是公開資訊。

但下面這些內容不能因為 Vercel 有 Environment Variable 功能，就直接放到 Frontend：

- Gemini API Key
- Google Service Account JSON
- Trello Token
- Confluence Token
- Database Password

Frontend Environment Variable 和 Backend Secret 並不是同一件事情。

判斷方式其實很簡單： **如果使用者打開 DevTools 看到這個值會造成安全問題，那它就不應該存在 Frontend。**

## 第三步：為什麼前後端都正常，瀏覽器還是連不上？

Frontend 和 Backend 都部署成功後，接下來非常常見的問題就是 **CORS（Cross-Origin Resource Sharing，跨來源資源共享）** 。

本機時，Frontend 可能是：

`http://localhost:5173`

Backend 是：

`http://localhost:8000`

正式環境則變成：

`https://your-app.vercel.app`

以及：

`https://your-backend.onrender.com`

對瀏覽器來說，這些都是不同 Origin。即使 Render API 本身完全正常，Browser 仍然會根據 CORS Policy 判斷這個 Frontend 是否有權呼叫 Backend。

因此正式 Backend 應該明確設定允許哪些 Frontend Origin，而不是為了方便直接永久設定：

`*`

允許所有來源雖然可以快速解決開發階段的 CORS Error，但也代表其他網站可能直接從 Browser 呼叫你的 Backend，消耗模型額度與運算資源。如果 Backend 未來還包含其他敏感操作，風險會更高。

比較合理的方式是把允許來源也做成 Environment Variable，例如：

```markdown
ALLOWED_ORIGINS=https://your-app.vercel.app,http://localhost:5173
```

後端再把它解析成允許清單。

## Local、Preview、Production 最好分開理解

部署後很容易發生一種混亂：本機正常、Vercel Production 正常，但 Preview URL 卻壞掉；或者 Preview 正常，正式版本反而出現 CORS Error。

原因通常是不同環境具有不同 URL。

可以先整理成：

| 環境 | Frontend | Backend | 用途 |
| --- | --- | --- | --- |
| Local | `localhost:5173` | `localhost:8000` | 本機開發 |
| Preview | Vercel Preview URL | 測試或正式 Render | PR / Branch 驗收 |
| Production | 正式 Vercel URL | 正式 Render URL | 正式使用 |

Vercel 常會替 Branch 或 Pull Request 建立新的 Preview Domain。如果 Backend CORS 只允許正式 Production Domain，那 Preview 前端自然會被擋住。

這時不要直接為了 Preview 把 CORS 改成 `*` ，而可以考慮建立受控的 Preview 規則、固定測試 Domain，或者使用另一個 Staging Backend。正式產品越往後發展，Local、Staging、Production 的界線就應該越清楚。

## CORS 問題其實可以從「請求走到哪裡」判斷

當 Browser 出現 CORS Error 時，不要看到 `CORS` 就開始亂改設定。可以先判斷 Request 到底走到了哪一步。

| 現象 | 優先檢查 |
| --- | --- |
| Browser 顯示 CORS，Backend 沒看到正式 Request | 檢查 Preflight |
| Render 收到 `OPTIONS` ，但沒有收到 `POST` | Methods / Headers / Origin 設定 |
| Frontend 一直打舊 Render URL | Vercel Env 更新後是否 Redeploy |
| Local 正常、Production 失敗 | Production Origin 是否在 Allowlist |
| Postman 正常、Browser 失敗 | 優先檢查 CORS |

這裡尤其要理解 **Preflight Request** 。

某些跨來源 Request 在真正送出 `POST` 之前，Browser 會先送出一個 `OPTIONS` Request，詢問 Backend 是否允許目前 Origin、HTTP Method 與 Header。如果 Preflight 沒有通過，真正的 `POST` 根本不會被 Browser 送出去。

因此 Render Log 裡如果只看到 `OPTIONS` ，卻永遠沒有下一個 `POST` ，就應該先檢查 CORS Middleware，而不是懷疑 Gemini API 壞掉。

另外，Postman 或類似 API Tool 不受 Browser CORS Policy 限制。因此「Postman 打得到」只能證明 Backend API 本身可能正常，不能證明 Vercel Frontend 一定可以呼叫。

## 正式部署後，跑一輪 Smoke Test

Frontend 和 Backend 都能打開之後，還不能直接宣布上線完成。

下一步應該做 **Smoke Test（基本驗收測試）** ，沿著真正的使用流程走一次，確認本機已經測試過的能力在正式環境仍然正常。

至少可以測：

| 測試項目 | 通過條件 |
| --- | --- |
| Backend Health Check | 正常回應，不是 Timeout / 5xx |
| 一般聊天 | Frontend 可以成功取得 Backend 回覆 |
| PDF RAG | 能找到文件並顯示來源 |
| Google Sheets | 可以讀取測試資料並得到預期結果 |
| 跨來源問題 | Coordinator 能正確使用兩個以上 Tool |
| Credential Error | 顯示可理解錯誤，不讓頁面崩潰 |
| Timeout | 能 Retry、Fallback 或清楚停止 |
| CORS | 正式 Vercel 可以呼叫 Render |
| Redeploy | 必要資料與設定沒有意外消失 |

也可以延續前面幾天的做法，建立固定 Deployment Test Case：

| ID | 測試項目 | 預期結果 | 實際結果 | 通過 |
| --- | --- | --- | --- | --- |
| PROD-01 | Backend Health Check | `status = ok` |  |  |
| PROD-02 | PDF RAG | 回答並顯示來源 |  |  |
| PROD-03 | Google Sheets | 回傳測試預期數值 |  |  |
| PROD-04 | Cross-source Workflow | 正確使用多來源 |  |  |
| PROD-05 | CORS | Production Frontend 正常連線 |  |  |

這份清單真正有價值的地方，是每一次部署都可以重新執行，而不是上線當天人工問兩題覺得沒問題，以後就完全不再測試。

## 不要忘記檢查 Secret 有沒有真的留在 Backend

正式部署完成後，可以再做一次 Day 28 的 Security Check。

首先檢查 GitHub Repository，確定沒有 `.env` 、Credential JSON 或 Token 被 Commit。接著打開 Browser DevTools，確認 Frontend Bundle 與 Network Request 中沒有 Gemini Key、Service Account 或其他 Secret。

還要確認 Backend Error 不會把完整 Credential、Internal Path 或 Stack Trace 直接送給 Frontend。

這些檢查非常值得在正式部署後再做一次，因為有些 Secret Leakage 只有 Build 完成之後才會被發現。例如原本以為某個 Vercel Environment Variable 是「平台幫忙保密」，實際 Build 後才發現它已經被包進 JavaScript。

## 本機正常、正式環境失敗，其實很常見

Deployment 最容易讓人挫折的一件事，就是本機全部正常，正式環境卻突然失敗。

但這不一定代表程式邏輯寫錯。Local 和 Production 之間本來就存在不少差異，例如：

- Environment Variable 沒有同步
- 檔案路徑大小寫不同
- Dependency Version 不一致
- Google Service Account 沒有正式資料權限
- CORS Origin 不一致
- Build Command 不同
- 暫存檔案在 Redeploy 後消失
- Production Domain 沒有加入 Allowlist

所以遇到部署問題時，不要同時修改五個設定。

仍然可以使用這 29 天一路建立的除錯方法： **先確認是哪一層失敗，再看那一層的 Log 與 State。**

例如：

Frontend 打不開，就先看 Vercel Build；Frontend 正常但 API 沒反應，就檢查 Render Health；Backend 正常但 Google Sheets 失敗，就檢查 Credential 與 Sharing；只有 Browser 失敗但 Postman 正常，就優先看 CORS。

問題一層一層縮小，通常比一直修改 Prompt 或重新部署有效。

## Deployment 成功後，真正的產品工作才開始

Render 顯示 Live、Vercel 顯示 Ready，代表 Deployment 完成，但不代表產品已經準備好長期運作。

正式產品至少還需要回答四個問題：

**怎麼測？怎麼看？怎麼存？怎麼安全地更新？**

這四個問題分別對應 Testing、Observability、Persistence 與 CI/CD。

## Testing：不要再只靠人工聊天

前面的 Smoke Test 是最基本的安全網，但 AI Application 還應該逐步加入更固定的 Regression Test。

例如 Day 19 已經建立 Routing Test，可以確認「數字問題」仍然會走 Data Tool、「政策問題」仍然會走 Document Tool；Day 10 的 RAG Test 則可以保留一批已知問題，檢查文件更新或 Embedding 更換後，Retrieval 是否仍然找得到正確來源。

因此未來每次修改 Prompt、Model、Tool Description 或 Workflow，都可以重新執行：

- Smoke Test
- Routing Test
- RAG Retrieval Test
- Schema Test
- Reliability Test

AI 的輸出本來就比傳統程式更具有變動性，所以修改速度越快，Regression Test 反而越重要。

## Observability：不只要知道答案，還要知道答案怎麼來的

使用者通常只需要看到最終答案與必要來源，但維護者需要看到更多。

例如一個 Request 可以記錄：

- 使用哪一版 Workflow
- Router 最後選擇哪些 Tool
- 各 Tool 執行時間
- Data Source
- Retry 次數
- Verification Result
- 使用的 Model
- Token Usage
- 最終 Task Status
- 整體 Latency

這些資料可以讓團隊回答一些非常實際的問題，例如：「最近為什麼回答速度變慢？」「是不是 RAG Search 特別慢？」「哪一個 Tool 的錯誤率最高？」「換模型後 Cost 增加多少？」

不過延續 Day 28 的治理原則，Observability 不代表把完整 Prompt、Secret 與敏感企業資料全部寫進普通 Log。要留下的是能解釋系統行為的 Metadata，而不是另一份完整資料副本。

## Persistence：不要把雲端硬碟當成永久資料庫

另一個正式部署後很常出現的問題，是把 Server Local Disk 當成永久儲存。

例如 RAG 在 Backend 啟動時把向量索引寫進本機資料夾，看起來可以正常使用，但 Render Redeploy 或 Instance Restart 後，這些檔案可能消失。

這不是 LangGraph 或 RAG 的問題，而是 Infrastructure 沒有真正處理 Persistence。

不同資料適合放在不同儲存方式：

| 資料 | 適合的持久化方式 |
| --- | --- |
| Conversation / Task Metadata | PostgreSQL |
| PDF 原始檔 | Object Storage |
| Vector Index | pgvector / Vector Database |
| Workflow Checkpoint | Database / Checkpointer |
| 短期 Cache | Redis |

不是每個 Data Machi Prototype 都需要立刻架設所有基礎設施，但至少要知道哪些資料如果在 Redeploy 後消失會造成問題。

尤其前面 Day 21 的 Memory 與 Day 25 的 Workflow State，一旦真的要支援長期對話與 Pause / Resume，就不能只存在 Python Process Memory 裡。

## CI/CD：讓每次修改至少有一層安全網

現在 GitHub 已經成為正式部署來源，就可以逐步導入 CI/CD。

最基本的流程可能是：

**Push / Pull Request → Code Check → Test → Build → Deploy**

Backend 可以先跑 Python Unit Test，Frontend 可以跑 Build 與 Type Check，再執行幾個基本 API Test。只有通過後才進入 Production Deployment。

這件事對 AI-assisted Development 特別重要。現在使用 AI Coding Tool 可以非常快速修改十幾個檔案，但速度變快不代表錯誤變少。修改得越快，越需要自動測試告訴我們「哪些原本正常的功能被不小心破壞」。

因此 CI/CD 不是大型工程團隊才需要的流程，而是讓快速迭代不至於一直破壞既有產品的一層最低安全網。

## Preview Environment 也需要自己的治理

Vercel Preview 很方便，但到了企業產品，也需要注意它連的是什麼資料。

如果每一個 Pull Request Preview 都直接連 Production Backend，而且使用完整 Production Credential，就可能讓尚未驗證的 Frontend Code 直接存取正式資料。

因此比較成熟的設計，可以逐步把環境拆成：

**Development → Preview / Staging → Production**

Preview 使用測試 Credential、測試資料或受限權限；Production 才使用正式 Secret 與正式 Data Source。

這也延續 Day 28 的 Principle of Least Privilege：不是只有 User Permission 需要限制，不同 Deployment Environment 也應該擁有不同權限。

## 最後重新從瀏覽器走一次完整流程

做到最後，可以暫時忘記 Render、Vercel、LangGraph、FastAPI 這些技術名稱，重新站在使用者角度打開 Data Machi。

從 Browser 送出一個問題，Frontend 把 Request 送到 Render；Backend 讀取自己的 Secret，Coordinator 判斷使用哪些 Tool；Tool 查 Google Sheets 或文件；Verification 檢查資料；最後回答回到 Browser。

如果中間有 Timeout，使用者看到合理 Progress；如果某一來源失敗，可以得到 Partial Success；如果權限不足，畫面告訴他問題在哪；如果 Workflow 完成，也可以看到明確 Completion State。

這時才真正能說：

Data Machi 不再只是本機裡可以 Demo 的 AI Prototype，而開始具備一個可以被其他人實際使用的產品形態。

---

> **今天的重點：**  
> Deployment 不是把程式「丟到雲端」就結束，而是把 GitHub、Backend、Frontend、Environment Variables、CORS、Credential 與正式資料來源真正接成同一套系統。部署後還要透過 Smoke Test 確認本機功能沒有在 Production 失效，並開始思考 Testing、Observability、Persistence 與 CI/CD。真正的里程碑不是 Render 和 Vercel 都顯示部署成功，而是 **另一個人打開瀏覽器，就能可靠地使用 Data Machi 完成一項工作。**

明天是最後一天。我們會回頭整理這 30 天建立的所有能力，進行最後的 **Production Readiness Check、資產交接與維護規劃** ，再把前面五個階段的成果整合成一份完整的 **Enterprise AI Blueprint** 。

我們下集見囉！

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10408186)

[

Day 28｜API 金鑰（API Key）只是開始：企業 AI 的權限、安全與治理該怎麼做

](https://ithelp.ithome.com.tw/articles/10408186)[下一篇](https://ithelp.ithome.com.tw/articles/10409206)

[

Day 30｜從聊天到產品：30 天後，我們到底完成了什麼？

](https://ithelp.ithome.com.tw/articles/10409206)

系列文

[Data Machi 30 天學習系列：從 Chat 到 Product，打造真正能工作的企業 AI](https://ithelp.ithome.com.tw/users/20169646/ironman/9207) 共 31 篇

目錄

1. 27
	[Day 26｜AI 一定會失敗：逾時（Timeout）、重試（Retry）、備援（Fallback）怎麼設計？](https://ithelp.ithome.com.tw/articles/10408173)
2. 28
	[Day 27｜代理（Agent）在工作時，使用者為什麼不能只看到轉圈圈](https://ithelp.ithome.com.tw/articles/10408179)
3. 29
	[Day 28｜API 金鑰（API Key）只是開始：企業 AI 的權限、安全與治理該怎麼做](https://ithelp.ithome.com.tw/articles/10408186)
4. 30
	[Day 29｜實作：把 Data Machi 部署到 Render 與 Vercel](https://ithelp.ithome.com.tw/articles/10408189)
5. 31
	[Day 30｜從聊天到產品：30 天後，我們到底完成了什麼？](https://ithelp.ithome.com.tw/articles/10409206)
[完整目錄](https://ithelp.ithome.com.tw/users/20169646/ironman/9207)

[![.](https://itadstatic.ithome.com.tw/B9/1782203851_6a3a45cbd87c1.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9005)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/a3e9a897-de84-4879-9068-553e4db5f75f_small.jpg)

[Policy priorities and initiatives for securing Industrial Automation and Control Systems (IACS) in the Netherlands](https://itplus.ithome.com.tw/vod-page/161?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/bfb909cb-c713-44ce-bf7b-ef2976001bea_small.jpg)

[大規模網路掃描：以在全球上找出有漏洞的 Data Distribution Service (DDS) 為例](https://itplus.ithome.com.tw/vod-page/339?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/10/5a8abfe5-156d-4f08-a876-62c23d668038_small.png)

[Flowmon - 結合MITRE 與 Netflow 達成CP值最高的資安事件應變手段](https://itplus.ithome.com.tw/vod-page/489?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/1/21/e0bb98b2-5844-42a6-bdb1-313b41f156ab_small.png)

[運動x程式x工程師](https://itplus.ithome.com.tw/vod-page/1063?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

23 分