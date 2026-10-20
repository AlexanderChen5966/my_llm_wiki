---
title: "Claude Code Plugins (外掛 / 插件) 功能試用與心得分享"
source: "https://muki.tw/claude-code-plugins/"
author:
  - "[[MUKI space*]]"
published: 2025-10-13
created: 2026-08-14
description: "Claude Code 最新版本 (v2.0.14) 推出了..."
tags:
  - "clippings"
status: "processed"
---
Claude Code Plugins (外掛 / 插件) 功能試用與心得分享.md

Claude Code 最新版本 (v2.0.14) 推出了全新的 Plugins 系統，可以說是今年見過最實用、最能解決痛點的功能之一。

Plugins 可以整合我們寫好的 Slash Commands (就是用 `/` 叫出來的快捷指令)、Sub Agents (子代理)、Hooks (鉤子)、MCP Servers，簡單來說，就是一個超級大禮包，把所有的功能全部整合在一起，可以有效解決以下常見的情境與痛點：

- 團隊成員的開發環境與規範不統一
- 安裝環境複雜
- 新進員工的磨合與理解

現在我們可以將這些流程與用到的工具、指令都做成一個插件，讓其他團隊成員「一鍵安裝」，不到 10 秒鐘就能把整個工作流程複製過去，省了非常多的時間

此外，我們也能把插件 push 到 GitHub 上，當然也能下載全球使用者做好的插件，可想而知又是一個龐大的開源庫惹！此外因為全部都放在 GitHub 託管，所以有詳細的版本管理、敘述和更新機制，也可以在下載前確認對方的 plugins 檔案內容，確保安全與隱私性

## 安裝 Plugins

要使用 Plugins 功能，必須將 claude 升級到最新版，請輸入以下指令進行更新

Shell

```
xxxxxxxxxx
```

```
claude update
```

▼ 我從 `2.0.11` 更新到最新版 `2.0.14`

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-08.58.52.png)

▼ 再來輸入 `/plugin` 指令，可以先安裝 anthropics 官方提供的 claude code plugins

Shell

```
xxxxxxxxxx
```

```
/plugin marketplace add anthropics/claude-code
```

▼ anthropics 提供了五個 plugins，可以在 GitHub 的 [claude-code/.claude-plugin/marketplace.json](https://github.com/anthropics/claude-code/blob/main/.claude-plugin/marketplace.json) 看到詳細的 JSON 內容

marketplace.json

```
xxxxxxxxxx
```

```
{
```

```
"$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
```

```
"name": "claude-code-plugins",
```

```
"version": "1.0.0",
```

```
"description": "Bundled plugins for Claude Code including Agent SDK development tools, PR review toolkit, and commit workflows",
```

```
"owner": {
```

```
"name": "Anthropic",
```

```
"email": "support@anthropic.com"
```

```
},
```

```
"plugins": [
```

```
{
```

```
"name": "agent-sdk-dev",
```

```
"description": "Development kit for working with the Claude Agent SDK",
```

```
"source": "./plugins/agent-sdk-dev",
```

```
"category": "development"
```

```
},
```

```
{
```

```
"name": "pr-review-toolkit",
```

```
"description": "Comprehensive PR review agents specializing in comments, tests, error handling, type design, code quality, and code simplification",
```

```
"version": "1.0.0",
```

```
"author": {
```

```
"name": "Anthropic",
```

```
"email": "support@anthropic.com"
```

```
},
```

```
"source": "./plugins/pr-review-toolkit",
```

```
"category": "productivity"
```

```
},
```

```
{
```

```
"name": "commit-commands",
```

```
"description": "Commands for git commit workflows including commit, push, and PR creation",
```

```
"version": "1.0.0",
```

```
"author": {
```

```
"name": "Anthropic",
```

```
"email": "support@anthropic.com"
```

```
},
```

```
"source": "./plugins/commit-commands",
```

```
"category": "productivity"
```

```
},
```

```
{
```

```
"name": "feature-dev",
```

```
"description": "Comprehensive feature development workflow with specialized agents for codebase exploration, architecture design, and quality review",
```

```
"version": "1.0.0",
```

```
"author": {
```

```
"name": "Siddharth Bidasaria",
```

```
"email": "sbidasaria@anthropic.com"
```

```
},
```

```
"source": "./plugins/feature-dev",
```

```
"category": "development"
```

```
},
```

```
{
```

```
"name": "security-guidance",
```

```
"description": "Security reminder hook that warns about potential security issues when editing files, including command injection, XSS, and unsafe code patterns",
```

```
"version": "1.0.0",
```

```
"author": {
```

```
"name": "David Dworken",
```

```
"email": "dworken@anthropic.com"
```

```
},
```

```
"source": "./plugins/security-guidance",
```

```
"category": "security"
```

```
}
```

```
]
```

```
}
```

五個插件的簡易介紹如下，你可以根據自己的喜好安裝，我自己都有裝，因為看敘述都覺得很棒 (流口水：

- agent-sdk-dev
	- 用於處理 Claude Agent SDK 的開發工具包， **必裝**
- pr-review-toolkit
	- 作為一個全面的 PR 審查 Agent，包含這些功能：評論、測試、錯誤處理、類型設計、程式品質和程式簡化
- commit-commands
	- git 提交工作流程的指令，包括提交、推送和建立 PR
- feature-dev
	- 一個全面的功能開發 Agent，包含程式碼探索、架構設計與程式碼品質的審查等工作流程
- security-guidance
	- 檢查安全性的 Hook，當我們在編輯檔案時，會警告潛在的安全性問題，包括指令注入、跨站腳本 (XSS) 和不安全的程式碼模式

▼ 安裝成功後，可以再次輸入 `/plugins` 指令觀看你安裝的所有外掛，出現以下內容就表示安裝成功囉

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.13.34.png)

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.14.00.png)

AD

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="722" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-6816776861246904&amp;output=html&amp;h=280&amp;slotname=2619535264&amp;adk=536108718&amp;adf=3143620324&amp;pi=t.ma~as.2619535264&amp;abgtt=7&amp;w=722&amp;fwrn=4&amp;fwrnh=0&amp;lmt=1786685203&amp;rafmt=1&amp;armr=3&amp;format=722x280&amp;url=https%3A%2F%2Fmuki.tw%2Fclaude-code-plugins%2F&amp;host=ca-host-pub-2644536267352236&amp;fwr=0&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM3IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzNyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzNyJdXSwwXQ..&amp;dt=1786685192785&amp;bpp=1&amp;bdt=2041&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608100101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Dabed5c4f6c137a51%3AT%3D1786685192%3ART%3D1786685192%3AS%3DALNI_MYfJJNV1SVrdt4l6IPLqwFH3KHkcg&amp;gpic=UID%3D000014df72091a95%3AT%3D1786685192%3ART%3D1786685192%3AS%3DALNI_MYFojP7AHvvhSEstSBqIy9HMPHDYQ&amp;eo_id_str=ID%3D61820914debd3376%3AT%3D1786685192%3ART%3D1786685192%3AS%3DAA-AfjbBDTvN3lpfNfXahxBDXTZo&amp;prev_fmts=0x0%2C220x600%2C1634x908%2C1005x124&amp;nras=3&amp;correlator=2366553127110&amp;frm=20&amp;pv=1&amp;rplot=4&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=322&amp;ady=4798&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=1166&amp;eid=95398730&amp;oid=2&amp;psts=AOrYGskfFTDz48qqAhL_9oSySiYkpxMVHc_58L47xuNYVSvDZgfFYEGvLRtriG4TsW4imKfHP_QBNSb81QgiAAt8QcPWpnkAWSeE36BfIuc5BFw%2CAOrYGsmp1GT0r1NTSL5MO3HAq_bRaziBI39mZGGDYg45zho8gwd92c_pMs58kKaecQOAw_Al2fU37sZnbOW5FGfKf1qX8zQKUc1nAz9N_jx_FvrPbFWSFN9QfOHsXjsDEkDX&amp;pvsid=7572627927085767&amp;tmod=1284125063&amp;uas=3&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=M%7C%7CeEbr%7Cp&amp;abl=XS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;plas=216x714_l%7C241x714_r&amp;bz=1.15&amp;pgls=CAEaBTcuMC4x~CAEQBBoHMS4xODIuMA..&amp;ifi=2&amp;uci=a!2&amp;btvi=2&amp;fsb=1&amp;dtd=11077" title="Advertisement" aria-label="Advertisement"></iframe>

## 使用 agent sdk dev plugins 進行開發

讓我們先看看 `agent-sdk-dev` 的提示詞內容，以確保沒有被寫入什麼奇怪的指令

▼ 從 Claude Code 的 GitHub 文件可以看到 agent-sdk-dev 包含了 2 個代理和 1 個命令文件

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.16.55@2x.png)

agent-sdk-verifier-py 和 agent-sdk-verifier-ts.md 分別表示 python 和 typescript 的 SDK，agent-sdk-dev plugin 在使用前會先詢問使用者偏好的開發語言是 python 還是 typescript？根據使用者的選擇會載入對應的文件

### agent-sdk-verifier-py

使用此代理程式來驗證 Python Agent SDK 應用程式是否正確設定、遵循 SDK 最佳實踐和文件建議，並且已準備好進行部署或測試。此代理程式應在 Python Agent SDK 應用程式建立或修改之後被呼叫

### agent-sdk-verifier-ts

使用此代理來驗證 TypeScript Agent SDK 應用程式是否正確設定、遵循 SDK 最佳實踐和文件建議，並且已準備好進行部署或測試。此代理應在 TypeScript Agent SDK 應用程式建立或修改之後被調用

### new-sdk-app

透過 commands 的 new-sdk-app，幫我們建立設定 Claude Agent SDK 應用程式，在使用中 AI 會問我們多個問題，就是在這一份 markdown 中設定的

▼ 大致確認沒問題後，輸入 `/plugin` 選擇 `/agent-sdk:dev:new-sdk-app` 來建立自己的應用程式

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.22.59.png)

▼ 期間我回應了他幾個問題，可以看到當我直接使用正體中文回應，他後續的回覆幾乎也會使用正體中文，所以不需要在提示詞特別強調要切換語言

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.24.34.png)

▼ 所有問題回答完後，就會開始跑令我很 HIGH 的 Todos (身為一個 J 人，看到 Todos 就開心 )

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.26.51.png)

▼ 讓我們執行檔案，來看他的完成度如何

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.34.32.png)

完成度很不錯，功能完全正常，但是我覺得根本沒有參考 codepen 的設計稿照著做，而且我應該跟他說不要再用紫色 XXD。

▼ 鑑於上個版實在太醜了，所以我請他調整一下版面，這是後續修正的版本，修改後好很多了 XXD

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.42.30.png)

## 如何安裝其他 Market Place 的 Plugin

除了 anthropics 官方提供的五個 Plugins 之外，也可以自由新增或下載其他使用者建立的 Plugins，下載的方式一開始從官方啦取時輸入的指令類似，這邊用 [aitmpl.com](https://www.aitmpl.com/agents) 為例，如果你想要使用他們提供的 plugins，請在 claude code 輸入以下指令：

Shell

```
xxxxxxxxxx
```

```
/plugin marketplace add https://github.com/davila7/claude-code-templates
```

▼ 再執行 `/plugins` 應該就能看到跟我一樣的畫面了，目前用了兩個 Git (marketplace) 的 Plugins

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.29.34.png)

▼ 除了 aitmpl.com 外，我還發現一位帳號為 @jeremylongshore 的網友也有在做 [claude-code-plugins](https://github.com/jeremylongshore/claude-code-plugins) ，不過 stars 數量只有 17 個，想試用的朋友可以自行評估，會推薦是因為他的 plugins 數量好多 XXD

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.49.54.png)

### 使用 Plugins 出現找不到 Sonnet 模型？

▼ 在使用 aitmpl.com 提供的 Plugins 時，我出現了以下錯誤訊息： `API Error: 404 {"type":"error","error":{"type":"not_found_error","message":"model: sonnet"},"request_id":""}`

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.51.01.png)

這是因為在這次更新後，anthropics 官方有調整他們的模型名稱，而 aitmpl.com 的 markdown 文件都有指定模型為 sonnet，所以才會出現錯誤

目前 aitmpl.com 的 issue 顯示他還在調整處理，但如果你想要先使用，可以自己手動更新 markdown 文件，把 model 移除即可。

以我為例，我下載了 aitmpl.com 的 git-workflow plugins，所以我要編輯他的相關檔案：

Shell

```
xxxxxxxxxx
```

```
# 我使用 cursor IDE 開啟這個資料夾，所以才在開頭輸入 cursor 指令，如果你沒有進行設定，可以手動開啟該目錄
```

```
cursor ~/.claude/plugins/marketplaces/claude-code-template
```

找到對應的 plugins 檔案後，移除 `model: sonnet` 這一行，存檔後再重新呼叫 plugins 就能正常使用了

![](https://muki.tw/wordpress/wp-content/uploads/2025/10/CleanShot-2025-10-13-at-11.57.50.png)

## 小結

以上就是 Claude Code 的 Plugins 功能介紹分享。

其實 Plugins 並不是什麼新鮮玩意，他只是再把所有的工具包成一個新的工作流，我們不需要再手動切換指令，就能透過 Plugins 進行整合，非常適合用在團體協作與交接，也適合懶到極致的工程師朋友們 XXD。

muki@journal ~/AI 應用 $ 0