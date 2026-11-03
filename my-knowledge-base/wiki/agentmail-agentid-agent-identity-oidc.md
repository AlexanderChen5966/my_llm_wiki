---
title: AgentID：AgentMail 讓 AI 代理以獨立 OIDC 身分登入，補上身分層但不決定權限
date: 2026-10-30
tags: [ai/agent]
source: https://www.aiposthub.com/agentid-agentmail-agent-identity-owner-oidc/
type: article
status: processed
asset_value: 24
---

## T（核心論點）

AgentMail 於 2026-10-06 推出 AgentID，讓 AI 代理以**獨立的 OIDC 身分**登入網站，並可連結真人擁有者，補上「身分層」。但它**不決定代理能做什麼**：驗證身分、資料揭露與應用內權限必須分開設計。

🔴 單一來源（aiposthub／Philo）；作者聲明**未實際建帳或整合**，管理情境（蒐集資料代理 vs 客服工單代理）為本文示例，非官方企業實測。

## B（適用場景）
**適用：** 需要讓網站辨識「哪個代理、屬於誰」以做配額、追蹤與單獨停用的產品；已使用 Clerk、Supabase、Auth0、Better Auth、Auth.js 的應用（官方有整合文件）。
**不適用 / 注意：**
- 「支援 OIDC」不等於不用修改；帳號映射與權限規則仍須自行接好
- 驗證身分不等於可讀代理信箱；接受登入也不給應用郵件內容或密碼
- 「一千萬代理能找到你」是目錄觸及主張，非客戶或付費數
- 擁有者 email 不代表每個代理行為皆獲個別同意

## R（操作步驟）
1. 每個 AgentMail 信箱對應一個 AgentID；subject 在信箱生命週期內穩定，**重建同址信箱會換新值**，不可只用 email 當帳號主鍵，應保留提供者 subject
2. 登入證明需驗證簽章、發行者與目標應用
3. 擁有者姓名／email 僅對已註冊客戶端、在相應 scope 且憑證具 App: Share Owner 權限時提供；可用 owner_sub 聚合配額。開放客戶端不能要求擁有者資料
4. 擁有者資料可出現在 ID token 與 userinfo（以當前文件為準，舊介紹文欄位位置不同）；登入套件可能只保留標準欄位，需確認應用工作階段真的存了
5. 有效期：登入交易 5 分鐘、授權碼 60 秒（一次性）、ID／access token 10 分鐘、瀏覽器憑證 30 天、同意紀錄 180 天；**撤銷憑證只阻止新登入，不會終止已建立的 session**
6. 費用：接受登入的應用無每次登入費用，AgentMail 以信箱收費；已列 Firecrawl、Turso、TinyFish、Manufact、Telnyx、Rho

## C（關鍵行動）
- [ ] 整合時以 subject 為主鍵，並制定地址重建的帳號政策
- [ ] 另行設計應用內權限（建立工作區、唯讀、沿用組織配額）
- [ ] 驗收時檢查實際用於配額與帳號管理的資料欄位
- [ ] 設計停用流程時補上既有 session 的終止

## 原始摘要

2026-10-08 發布於 aiposthub，依 AgentMail 官方發布文與技術文件整理，說明代理獨立身分、OIDC 整合、擁有者資料揭露條件與權限邊界。

## 相關連結

- [[agent-tool-authorization-gatekeeper]] — 工具授權閘門；身分層之後的權限控制屬於這一層
- [[agent-permission-and-os-sandbox]] — 代理權限與沙箱
