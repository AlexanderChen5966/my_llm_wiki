---
title: "Pi Coding Agent 安裝、配置與使用 Codex 訂閱"
source: "https://vervecode.dev/ai/pi-coding-agent/"
author:
  - "[[Ted Liou]]"
published: 2026-06-14
created: 2026-08-17
description: "Pi Coding Agent 是一個極簡化的終端機工具，以只內建 Agent 必須的基礎工具與高度的自定義特性為名，OpenClaw 也是採用他的核心來驅動 AI。"
tags:
  - "clippings"
status: "processed"
---
摘要

Pi Coding Agent 是一個極簡化的終端機工具，以只內建 Agent 必須的基礎工具與高度的自定義特性為名， [OpenClaw](https://docs.openclaw.ai/agent-runtime-architecture) 也是採用他的核心來驅動 AI。

文章目錄

Pi Coding Agent 與主流的 Claude Code、Codex、OpenCode 不同在於，它只提供 read、edit、write 與 bash 四種 AI 開發的基礎工具，沒有複雜的 [系統提示詞](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/src/core/system-prompt.ts) 、沒有 MCP、沒有子代理。想要用非內建的功能，一律靠安裝插件來實現。

Pi 與 Claude Code、Codex 相比，優點是開發者能完全掌控自己的開發工具，但會失去模型官方支援的系統提示詞、工具與週邊服務。有資料說 [Pi 的 Token 消耗量是 Claude Code 的 30 ~ 35%](https://zhuanlan.zhihu.com/p/2043046237939749518) ，但他是以進行「非開發」的需求下來估算，實際開發時真的能否省到還真不好說，所以一概而論說 Pi 能省 Token 是不精準的，一切仍要視專案的規模、需求複雜度與 Prompt 來決定。

## 安裝與更新

Pi 支援 Windows、macOS、Linux，可使用任意終端機工具來驅動，建議盡可能使用支援 Bash 指令的環境，我自己的經驗是 AI 於 Windows 的 Powershell 中很容易犯錯，效率很低。

Pi 使用 npm 管理，請開啟終端機輸入以下指令來安裝至全域環境：

```bash
npm install -g --ignore-scripts @earendil-works/pi-coding-agent
```

安裝完成後，於專案的資料夾中開啟終端機，輸入 Pi 指令即可啟動 Agent。

```bash
pi
```

如果要 [更新 Pi](https://pi.dev/news/releases) 與擴充模組，使用以下指令：

```bash
# 更新 Pi 本體
pi update

# 更新 Pi 本體與所有擴充模組
pi update --extensions
```

## 安裝擴充模組

想要順利用 Pi 來開發就一定要安裝擴充模組，否則 Agent 會嘗試寫很多程式碼來嘗試實現你的需求，有工具不用反而很燒 Token。以下推薦幾個我於軟體開發時必裝的擴充模組。

### 網路搜尋

必裝，自動找資料，開箱即用，也可以在 `~/.pi/web-search.json` 加入自己的 Exa、Brave 等服務的 API，請參考 [pi-web-access 說明](https://pi.dev/packages/pi-web-access) 。

```bash
pi install npm:pi-web-access
```

### 選項提問

必裝，AI 有時候會反問你問題，此模組可以讓我們用方向鍵選，不然要一直打字真的很麻煩。

```bash
pi install npm:@juicesharp/rpiv-ask-user-question
```

模組生效後，AI 有問題時就能給我們選項：

![Pi 擴充模組顯示方向鍵選項提問選單](https://vervecode.dev/ai/pi-coding-agent/pi-ask-user-question-menu.png)

Pi 擴充模組顯示方向鍵選項提問選單

### 子代理

必裝，開發時通常會用貴的模型做規劃，便宜模型做探索與實作，用派子代理的方式來讓便宜模型來做吃力不討好的事情，可避免污染上下文，也能省錢。

```bash
pi install npm:pi-subagents
```

### MCP

選配，如果你有使用 MCP 的需求，例如： [Unity MCP](https://github.com/CoplayDev/unity-mcp) 、 [Serena MCP](https://github.com/oraios/serena) 等再裝，現在有很多 Skills 與 CLI 可以取代 MCP，所以不是必裝的模組。

```bash
pi install npm:pi-mcp-adapter
```

## 使用 Codex 訂閱

Pi 預設能使用 Claude、Codex、GitHub Copilot 的訂閱，也可用 API Key，可參考 Pi 官方文件的 [模型提供者列表](https://pi.dev/docs/latest/providers) 。

進入 Pi Code 後，執行：

```txt
/login
```

接著選擇 Use a subscription。

![Pi 登入流程選擇 Use a subscription 訂閱登入](https://vervecode.dev/ai/pi-coding-agent/pi-login-authentication-method.png)

Pi 登入流程選擇 Use a subscription 訂閱登入

接著在 provider 選單中選擇 ChatGPT Plus/Pro。

![Pi provider 選單選擇 ChatGPT Plus Pro Codex Subscription](https://vervecode.dev/ai/pi-coding-agent/pi-login-provider-chatgpt-codex.png)

Pi provider 選單選擇 ChatGPT Plus Pro Codex Subscription

再選擇 Browser login，這時視窗會印出一個連結，點擊後開啟網頁登入你已訂閱的 ChatGPT 帳號，連結成功後 Pi 就能使用 GPT 5 模型。

![Pi OpenAI Codex 登入方式選擇 Browser login](https://vervecode.dev/ai/pi-coding-agent/pi-login-browser-method.png)

Pi OpenAI Codex 登入方式選擇 Browser login

## 基本操作

/model 可選擇可用模型，反覆點擊 Ctrl + P 可直接切換模型。

![Pi 模型選單顯示可用的 GPT 模型](https://vervecode.dev/ai/pi-coding-agent/pi-model-selector-gpt.png)

Pi 模型選單顯示可用的 GPT 模型

反覆點擊 Shift + Tab 可調整推理等級。

![Pi Thinking Level 選單選擇 medium 推理等級](https://vervecode.dev/ai/pi-coding-agent/pi-thinking-level-selector.png)

Pi Thinking Level 選單選擇 medium 推理等級

## 總結

現階段 Codex App、Claude Code 仍是目前的主流 AI Agent，OpenCode、Pi 都是小眾的選擇。用了一陣子的 Pi 後，我認為它的優勢和 OpenCode 相似，都能支援多家模型的訂閱與 API，我能於 Pi 中連線公司提供的 [Cursor](https://pi.dev/packages/@nitra/cursor?name=cursor) ，開發時規劃用 GPT-5.5 xhigh，實作時視複雜度派 GPT-5.4 或 Composer 2.5 的子代理，可以分攤近期被嚴重閹割的 Codex 額度。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="786" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-9736607245120454&amp;output=html&amp;h=280&amp;slotname=6549963926&amp;adk=1336004163&amp;adf=3407817974&amp;pi=t.ma~as.6549963926&amp;abgtt=6&amp;w=786&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786956314&amp;rafmt=1&amp;format=786x280&amp;url=https%3A%2F%2Fvervecode.dev%2Fai%2Fpi-coding-agent%2F&amp;fwr=0&amp;fwrattr=true&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786956300306&amp;bpp=1&amp;bdt=612&amp;idt=41&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D0638e6dd82dd1486%3AT%3D1786956300%3ART%3D1786956300%3AS%3DALNI_MZ6GS8CuTuLdcxD6A1WwndtmZAMnw&amp;gpic=UID%3D000014e66e43bdad%3AT%3D1786956300%3ART%3D1786956300%3AS%3DALNI_MbWuIwpKpvC8YHBESQq79Gm8Z0GaA&amp;eo_id_str=ID%3D403c8247cdca7757%3AT%3D1786956300%3ART%3D1786956300%3AS%3DAA-Afjbm264LOn3Wt9VwG5xm7DdV&amp;prev_fmts=0x0%2C300x250&amp;nras=1&amp;correlator=5521604787892&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=424&amp;ady=6737&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=3113&amp;eid=95397678%2C122880126%2C122880370&amp;oid=2&amp;psts=AOrYGskv39TAWo8THGZqWjQUQGHY_G_WZ9w3YkXhske88rNSa2MO0_LlEx-NI_6TOG2WgNPlqEGWh2u-4Cwvkj4yg23hxSteSGiDRddF&amp;pvsid=2041074262586467&amp;tmod=734080960&amp;uas=1&amp;nvt=6&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7CEebr%7C&amp;abl=CS&amp;pfx=0&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=13828" title="Advertisement" aria-label="Advertisement"></iframe>