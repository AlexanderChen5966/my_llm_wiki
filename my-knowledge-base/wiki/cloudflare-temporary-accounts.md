---
title: Cloudflare Temporary Accounts：AI Agent 無需登入即可部署 Workers
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://tools.wingzero.tw/article/sn/3954
type: article
status: processed
asset_value: 60
---

Cloudflare Temporary Accounts 把部署流程顛倒過來：先部署再決定是否建帳號。AI Agent 寫完程式後可直接執行 `wrangler deploy --temporary`，60 秒內取得公開網址，不需要 API Token 或 OAuth。

## T（核心論點）

**傳統流程（AI Agent 卡關在這裡）：**
```
建立帳號 → 設定權限 → 建立 Token → 部署
```

**Temporary Accounts 新流程：**
```
部署 → 測試 → 確認有價值 → 再建立正式帳號
```

核心轉變：把原本發生在部署之前的「驗證流程」移到部署之後。

## B（適用場景）

- Claude Code / Codex / Cursor 等 AI Agent 自動完成「從開發到部署」閉環
- 快速驗證原型：Landing Page、API Demo、小工具、Side Project
- 60 分鐘內給客戶看的臨時預覽環境

## R（完整內容）

### 一行指令部署

```bash
npx wrangler deploy --temporary
```

Cloudflare 自動完成：
1. 建立臨時帳號
2. 建立 Worker 專案
3. 完成部署
4. 配發公開網址（`https://xxx.temp-workers.dev`）
5. 提供 Claim URL

### Claude Code 使用範例

告訴 Claude Code：
> 幫我製作一個待辦事項網站並部署到 Cloudflare。

Claude Code 流程：建立專案 → 撰寫程式碼 → 執行測試 → `wrangler deploy --temporary` → 回傳網址

**整個過程不需要人工介入建立帳號。**

### 60 分鐘限制說明

- 「未認領的臨時帳號」60 分鐘後自動刪除
- 在此期間可以持續重新部署、測試、分享
- 如要保留：在 60 分鐘內打開 Claim URL，登入/建立 Cloudflare 帳號，轉為正式 Worker
- 認領後網址不變、Worker 保留、可正常管理

### 為什麼這對 AI Agent 重要

過去的 OAuth/MFA 流程對「人」來說正常，但對 AI Agent 是障礙。Temporary Accounts 讓 AI Agent 能完成整個開發到部署的閉環，人只需要在最後確認是否保留。

適合場景：原型驗證、小工具開發、Landing Page、API Demo、個人 Side Project。

## C（關鍵行動）

- [ ] 在 Claude Code 工作流中加入 `wrangler deploy --temporary` 作為快速驗證步驟
- [ ] 告知 AI Agent：部署完成後提供 Claim URL，讓人決定是否轉為正式帳號
- [ ] 評估 LOOP.md 或定時排程中加入 Cloudflare 部署步驟

## 相關連結

- [[codex-advanced-tips-11]] — Codex Sites（Business/Enterprise）：自然語言→可部署網站
- [[loop-engineering-implementation-patterns]] — Loop Engineering：AI 完成整個開發閉環
- [[claude-code-advanced-workflow]] — Claude Code 進階技巧（/loop / /schedule）
