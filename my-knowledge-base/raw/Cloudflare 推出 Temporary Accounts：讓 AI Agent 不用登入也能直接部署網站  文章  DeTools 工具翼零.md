---
title: "Cloudflare 推出 Temporary Accounts：讓 AI Agent 不用登入也能直接部署網站 | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3954"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-06-22
created: 2026-06-22
description: "Cloudflare Temporary Accounts 讓 AI Agent 無需 Cloudflare 帳號即可部署 Cloudflare Workers，支援 Claude Code、Codex 與 Cursor 快速上線應用，縮短從開發到部署的流程。"
tags:
  - "clippings"
status: processed
---
近年來 AI Coding 工具發展速度驚人，無論是 Claude Code、OpenAI Codex、Cursor Agent、Windsurf 或 Gemini CLI，都已經能夠自行撰寫程式、修正錯誤，甚至完成整個專案。然而當 AI 寫完程式之後，往往會在最後一步卡關：部署。

原因其實很簡單。

部署到雲端服務通常需要登入帳號、建立 API Token、完成 OAuth 驗證，甚至還要通過多因素驗證（MFA）。這些流程對人類來說相當正常，但對於自動執行的 AI Agent 而言卻是一道難以跨越的障礙。

為了解決這個問題，Cloudflare 最近推出了全新的 Temporary Accounts（臨時帳號）功能，讓 AI Agent 可以在沒有 Cloudflare 帳號的情況下，直接將應用程式部署到 Cloudflare Workers 上。

## AI 可以先部署，再決定要不要註冊

過去如果你要求 Claude Code 幫你製作一個網站，流程大致會是這樣：

![](https://farm66.static.flickr.com/65535/55349270606_05c8e23b90_b.jpg)

```
AI 完成程式
↓
登入 Cloudflare
↓
建立 API Token
↓
授權給 AI
↓
開始部署
```

現在則完全不同。

透過 Temporary Accounts，AI Agent 可以直接執行部署指令：

```
npx wrangler deploy --temporary
```

或是在已安裝 Wrangler 的環境中：

```
wrangler deploy --temporary
```

Cloudflare 會自動完成以下工作：

- 建立一個臨時 Cloudflare 帳號
- 建立 Worker 專案
- 完成部署
- 配發公開網址
- 提供 Claim URL

整個過程不需要登入 Cloudflare，也不需要建立 API Token。

幾秒鐘後，你就能取得一個已經上線的網址。

## 實際部署範例

假設你建立了一個最簡單的 Worker：

```js
export default{ async fetch(request) { returnnewResponse("Hello World!"); }, };
```

接著在專案目錄執行：

```
wrangler deploy --temporary
```

Wrangler 會自動將程式上傳至 Cloudflare。

部署完成後，終端機可能會顯示類似內容：

```
Deployment successful!

URL:
https://hello-world.temp-workers.dev

Claim URL:
https://dash.cloudflare.com/claim/xxxxxxxx
```

此時網站已經可以立即使用。

你可以直接打開網址測試功能，也可以把網址分享給其他人查看。

## AI Agent 的使用方式

這項功能最大的受益者其實是 AI Agent。

例如你對 Claude Code 說：

> 幫我製作一個待辦事項網站並部署到 Cloudflare。

Claude Code 可以：

1. 建立專案
2. 撰寫程式碼
3. 執行測試
4. 使用 Wrangler 部署
5. 回傳網址給使用者

流程變成：

```
Claude Code
↓
建立專案
↓
撰寫程式
↓
wrangler deploy --temporary
↓
取得網址
↓
回傳給使用者
```

整個過程完全不需要手動建立 Cloudflare 帳號。

## 臨時帳號只能存在 60 分鐘

看到這裡，很多人的第一個問題都是：

「所以網站只能存在 60 分鐘嗎？」

答案是：

不是。

真正受到限制的是「未認領的臨時帳號」。

Cloudflare 建立的 Temporary Account 預設只會保留 60 分鐘。

在這段時間內：

- AI 可以持續重新部署
- 可以測試功能
- 可以分享網址
- 可以決定是否保留專案

如果超過 60 分鐘都沒有認領：

- 臨時帳號自動刪除
- Worker 自動移除
- 網址失效
- 所有資源被回收

因此它比較像是一個短期預覽環境，而不是長期主機服務。

## 如何保留專案？

部署完成後，Cloudflare 會提供一個 Claim URL。

只要在 60 分鐘內開啟該網址，就能將臨時帳號轉換為正式帳號。

流程如下：

```
Temporary Account
↓
取得 Claim URL
↓
登入或建立 Cloudflare 帳號
↓
Claim 專案
↓
轉為正式 Worker
```

完成認領後：

- 網站繼續運作
- 網址不變
- Worker 保留
- 可以正常管理與更新

之後就和一般 Cloudflare Workers 專案沒有差別。

## 為什麼這個功能很重要？

Cloudflare 實際上做了一件非常符合 AI 時代的事情。

過去的流程是：

```
建立帳號
↓
設定權限
↓
建立 Token
↓
部署
```

現在則變成：

```
部署
↓
測試
↓
確認有價值
↓
再建立正式帳號
```

Cloudflare 把原本應該發生在部署之前的註冊流程，移到了部署之後。

這意味著未來使用 Claude Code、Codex、Cursor 或其他 AI Agent 時，我們只需要描述需求，AI 就能自行完成撰寫、測試與部署，並直接提供可使用的網址。

對於原型驗證、小工具開發、Landing Page、API Demo 甚至個人 Side Project 而言，這將大幅降低從「想法」到「上線」之間的距離。

而 Temporary Accounts，就是 Cloudflare 為 AI Agent 時代打造的第一個真正無需登入即可部署的雲端平台功能。

[官方文件說明](https://blog.cloudflare.com/temporary-accounts/)