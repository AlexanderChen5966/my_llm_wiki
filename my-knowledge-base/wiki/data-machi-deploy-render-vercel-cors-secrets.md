---
title: 把 Agent 產品部署上線：Render/Vercel 分離部署、CORS 除錯表與 Secret 邊界
date: 2026-09-11
tags: [dev/ci-cd, ai/workflow, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10408189（《Data Machi 30 天》Day 29）
type: article
status: processed
asset_value: 60
---

**系列：《Data Machi 30 天學習系列：從 Chat 到 Product》Day 29。**
前面 Day 25–28 的架構設計見 [[langgraph-coordinator-refactor-seven-nodes]]、
[[agent-reliability-timeout-retry-fallback-verification]]、
[[agent-ux-status-transparency-maturity]]、[[enterprise-ai-permission-three-layers-governance]]；
本篇是它們的部署實作篇，下一天（Day 30）系列完結，見
[[enterprise-ai-blueprint-six-stages-eight-questions]]。

## T（核心論點）

> **部署不是把程式「丟到雲端」就結束，而是把 GitHub、Backend、Frontend、Environment Variables、
> CORS、Credential 與正式資料來源真正接成同一套系統。**

信任邊界：`Browser → Vercel Frontend → Render Backend → Gemini / Google Sheets / 外部 Tool`。
Browser 不直接拿 API Key 或 Service Account，前端只呼叫 Backend，真正的 Credential 只留在
受信任的後端環境——這延續 Day 28 的權限治理原則。

## B（適用場景）

**適用：**
- 把已在本機跑通的 FastAPI + React（或類似前後端分離）Agent 產品第一次部署上線
- 遇到「本機正常、正式環境失敗」但抓不到是哪一層的問題
- 需要建立可重複執行的 Deployment Smoke Test，而非上線當天人工問兩題就宣布完成

**不適用 / 注意：**
- 本篇以 Render（Backend）＋ Vercel（Frontend）為例，重點是理解前後端分離部署後「各層怎麼連起來」，
  非兩個平台本身的教學
- ⚠️ Postman 打得通不代表瀏覽器打得通——Postman 不受 CORS 限制，只能證明 Backend API 本身正常

## R（操作步驟）

### 1. 先確認 GitHub 才是正式版本來源

**Local 是開發環境，GitHub 才是部署版本的來源。** 部署前先確認 Frontend/Backend 各自的
Root Directory、Dependency File（`requirements.txt` / `package.json`）與啟動方式是否清楚，
否則平台設定再對也不知道從哪個資料夾開始 Build。

### 2. 先部署 Backend（Render），再部署 Frontend（Vercel）

Backend 設定範例：`Root Directory: backend`／`Build Command: pip install -r requirements.txt`／
`Start Command: uvicorn main:app --host 0.0.0.0 --port $PORT`。
🔴 **`$PORT` 不能寫死**——本機習慣的 8000 在正式平台通常由平台動態指派。

Backend 上線後第一個測試不是問 AI，而是 **Health Check**（`GET /health` 回 `{"status": "ok"}`）——
目的只回答「這個 Backend 現在活著嗎」，`/health` 打不開就沒必要先檢查 Prompt 或 RAG。

### 3. 本機 `.env` 搬到正式 Environment Variables

`.env` 內容不隨程式碼一起 Commit，改建在 Render 的 Environment Variables／Secret 管理介面。
修改 Environment Variable 後執行中的服務不一定自動讀到新值，部分平台需 Redeploy 或 Restart。

### 4. Frontend 的 API Base URL 用環境變數管理，不可 Hardcode

```
VITE_API_BASE_URL=https://your-backend.onrender.com
```

讓 Local、Preview、Production 可在不改程式碼的情況下切換 Backend。

### 5. 🔴 `VITE_` 前綴變數不是 Secret

Vite 會把該前綴的變數打包進 Frontend Build，最終可在瀏覽器被讀取。Gemini API Key、
Service Account JSON、Trello/Confluence Token、資料庫密碼**絕不能**因為平台有「環境變數功能」
就放進 Frontend。判準：**如果使用者打開 DevTools 看到這個值會造成安全問題，它就不該存在 Frontend。**

### 6. CORS 除錯表：先判斷 Request 走到哪一步

| 現象 | 優先檢查 |
| --- | --- |
| Browser 顯示 CORS，Backend 沒看到正式 Request | 檢查 Preflight |
| Render 收到 `OPTIONS`，沒收到 `POST` | Methods/Headers/Origin 設定 |
| Frontend 一直打舊 Render URL | Vercel Env 更新後是否 Redeploy |
| Local 正常、Production 失敗 | Production Origin 是否在 Allowlist |
| Postman 正常、Browser 失敗 | 優先檢查 CORS |

正式 Backend 應明確設定允許的 Frontend Origin（`ALLOWED_ORIGINS=https://your-app.vercel.app,...`），
不要為了方便永久設 `*`——那代表任何網站都能從瀏覽器呼叫你的 Backend、消耗模型額度。

**Preflight Request** 機制：某些跨來源 `POST` 在真正送出前，瀏覽器會先送 `OPTIONS` 詢問 Backend
是否允許目前 Origin/Method/Header；Preflight 沒過，真正的 `POST` 根本不會被送出去。

### 7. Local / Preview / Production 分開治理

Vercel Preview 常有獨立 Domain，若 Backend CORS 只允許 Production Domain，Preview 前端會被擋住；
不要為了 Preview 直接把 CORS 開成 `*`，而應建立受控 Preview 規則或使用另一個 Staging Backend。
更成熟的設計是 Preview 使用測試 Credential/測試資料，不直接連 Production 正式資料
（延續 Day 28 的最小權限原則，環境本身也該分權限）。

### 8. 部署後跑一輪固定 Smoke Test

| ID | 測試項目 | 通過條件 |
| --- | --- | --- |
| PROD-01 | Backend Health Check | `status = ok` |
| PROD-02 | PDF RAG | 回答並顯示來源 |
| PROD-03 | Google Sheets | 回傳測試預期數值 |
| PROD-04 | Cross-source Workflow | 正確使用多來源 |
| PROD-05 | CORS | Production Frontend 正常連線 |

這份清單的價值在於**每次部署都可以重新執行**，而非上線當天人工測過就不再測。

### 9. 部署後再做一次 Secret 檢查

確認 GitHub 沒有 Commit `.env`/Credential JSON/Token；打開 DevTools 確認 Frontend Bundle 與
Network Request 沒有洩漏 Secret；確認 Backend Error 不會把完整 Credential/Internal Path/
Stack Trace 送給前端——有些洩漏只有 Build 完成後才會被發現。

### 10. 除錯原則：先定位是哪一層失敗，再修改那一層

Frontend 打不開先看 Vercel Build；Frontend 正常但 API 沒反應查 Render Health；Backend 正常但
Google Sheets 失敗查 Credential 與 Sharing；只有 Browser 失敗但 Postman 正常優先看 CORS。
**不要同時修改五個設定。**

### 11. 部署成功後才是產品工作的開始：Testing／Observability／Persistence／CI-CD

- **Testing**：Smoke Test 只是安全網，還要逐步加 Routing Test、RAG Retrieval Test、Reliability Test
- **Observability**：記錄 Workflow 版本、Router 選擇的 Tool、各 Tool 執行時間、Retry 次數、
  Token Usage、最終 Task Status 等 **Metadata**，而非完整 Prompt 或 Secret
- **Persistence**：Server Local Disk 不是永久資料庫——RAG 向量索引寫在本機資料夾，Redeploy 後
  會消失；依資料類型分流到 PostgreSQL／Object Storage／pgvector／Checkpointer／Redis
- **CI/CD**：`Push/PR → Code Check → Test → Build → Deploy` 是 AI-assisted Development 特別需要的
  最低安全網——修改速度變快不代表錯誤變少

## C（關鍵行動）

- [ ] 檢查 Backend 是否正確綁定平台提供的 `$PORT`，而非寫死本機的 Port
- [ ] 用 DevTools 檢查 Frontend Bundle／Network Request，確認沒有任何 Secret 隨 `VITE_` 前綴外流
- [ ] 把 CORS Allowlist 從 `*` 改成明確 Domain 清單，Preview 環境另外設計受控規則
- [ ] 建立可重複執行的 Deployment Smoke Test（PROD-01～05），每次部署都重新跑一次
- [ ] 盤點 Persistence：檢查哪些資料還存在 Server Local Disk，Redeploy 後會不會消失

## 原始摘要

Data Machi 系列 Day 29，把 FastAPI Backend 部署到 Render、React Frontend 部署到 Vercel。
涵蓋環境變數搬遷（`.env` → 平台 Environment Variables）、`$PORT` 動態綁定、`VITE_` 前綴變數
與後端 Secret 的邊界、CORS 除錯表與 Preflight Request 機制、Local/Preview/Production 三環境
分開治理，以及部署後的 Smoke Test 清單。強調除錯原則「先定位是哪一層失敗，再修改那一層」，
並延伸出上線後仍需處理的 Testing、Observability、Persistence、CI/CD 四大工作。

## 相關連結

- [[enterprise-ai-permission-three-layers-governance]] — 同系列 Day 28：本篇的 Secret 只留在
  Backend、VITE_ 前綴的判準，是它 Platform/System/User 三層權限與信任邊界的部署面落地
- [[langgraph-coordinator-refactor-seven-nodes]] — 同系列 Day 25
- [[agent-reliability-timeout-retry-fallback-verification]] — 同系列 Day 26：本篇 Observability
  只記 Metadata 不記 Secret 的規範來自這裡
- [[agent-ux-status-transparency-maturity]] — 同系列 Day 27：本篇的 Cold Start／Loading 現象
  正是它主張的「前端要有明確 Connecting State」的實例
- [[enterprise-ai-blueprint-six-stages-eight-questions]] — 同系列 Day 30，**系列完結**：
  本篇是它 Deployment 驗收面向的實作展開
- [[testing-your-ai-workflow-config-ci]] — CI/CD 落地的另一種實作參考，可與本篇第 11 節對照
- [[production-agent-architecture]] — 生產級 Agent 架構的 Guardrails 設計，與本篇部署面互補
