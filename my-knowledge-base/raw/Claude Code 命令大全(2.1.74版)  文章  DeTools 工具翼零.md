---
title: "Claude Code 命令大全(2.1.74版) | 文章 | DeTools 工具翼零"
source: "https://tools.wingzero.tw/article/sn/3824#google_vignette"
author:
  - "[[DeTools 工具翼零]]"
published: 2026-03-17
created: 2026-05-14
description: "這篇文章整理 Claude Code 的完整命令體系，從終端主命令、子命令到會話內 Slash 指令，幫助快速建立清晰的操作地圖。本文重點介紹高頻命令、MCP 與插件管理，以及新手最常混淆的指令概念。"
tags:
  - "clippings"
status: processed
---
本文來自 [X@shachepi 的整理](https://x.com/shachepi/status/2032036375894413458) ，內容基於 **Claude Code 2.1.74** 整理。不同版本、方案、插件、技能與平台，具體顯示可能會有少量差異，但核心原理基本一致。

這篇教學的目標很簡單：

- 用 **最短的路徑** 讓你能立刻上手
- 用 **最清楚的結構** 讓你不再混淆
- 用 **最低的記憶成本** 建立命令地圖

## 一、先把命令體系分成三層

Claude Code 的命令體系只有三層。記住這個結構，就不會混亂。

**第一層：終端主命令**

也就是在 Shell 中直接輸入的 `claude`  
作用是： **開啟程式**

**第二層：終端子命令**

也就是 `claude` 後面接的功能選單（例如 `auth` 、 `mcp` 、 `plugins` 等）  
作用是： **管理功能**

**第三層：Slash 命令**

也就是進入互動介面後輸入的 `/help` 、 `/model` 、 `/clear` 等  
作用是： **控制會話**

一句話總結：

> 主命令負責開啟程式，子命令負責管理功能，Slash 命令負責控制會話。

---

## 二、新手先記住 3 條主命令

這 3 條命令是 **90% 的入口** 。

**claude**

進入互動式 Claude Code 會話。

**claude -p "任務"**

非互動模式。執行完成後直接輸出並退出，適合腳本與批次處理。

**claude -c**

繼續目前目錄最近的一次會話，適合中斷後繼續聊天。

如果只記三個入口，優先記住這三個。

---

## 三、主命令常見參數（最有實際價值的一組）

下面是最常見且最有實際價值的一組全域參數：

\--add-dir

額外開放更多目錄給 Claude Code 存取，適合跨目錄讀寫。

\--agent

指定當前會話使用的 agent。

\--agents

臨時傳入自訂 agents 的 JSON 定義。

\--allowed-tools

只允許指定工具執行。

\--disallowed-tools

明確禁止某些工具執行。

\--tools

自訂本次會話可使用的工具集合。

\--system-prompt

直接覆蓋本次會話的系統提示詞。

\--append-system-prompt

在預設系統提示詞後追加補充說明。

\--model

指定模型。

\--fallback-model

當預設模型擁堵時，為 print 模式指定備用模型。

\--effort

指定思考強度，例如：

- low
- medium
- high
- max

\--permission-mode

指定權限策略，例如：

- default
- auto
- plan
- dontAsk

\--settings

載入額外 settings 檔案或 JSON。

\--setting-sources

限制只從哪些設定層讀取配置。

\--mcp-config

載入 MCP 設定檔或 JSON。

\--strict-mcp-config

只使用命令列指定的 MCP 配置。

\--plugin-dir

臨時載入某些插件目錄。

\--debug

開啟除錯模式。

\--debug-file

把除錯日誌寫入指定路徑。

\--verbose

強制開啟 verbose。

\-p 或 --print

輸出後退出，不進入互動模式。

\--output-format

控制輸出格式，例如：

- text
- json
- stream-json

\--input-format

控制輸入格式。

\--json-schema

要求輸出符合指定 JSON Schema。

\--include-partial-messages

流式輸出時包含中途分片。

\--max-budget-usd

限制本次 print 模式 API 花費上限。

\--no-session-persistence

不保存會話到本地。

\--session-id

手動指定會話 ID。

\--fork-session

恢復舊會話時分叉出一個新會話。

\--from-pr

從關聯 PR 恢復會話。

\--ide

如果本機只有一個可用 IDE，自動連接。

\--chrome / --no-chrome

開啟或關閉 Claude in Chrome 整合。

\-w 或 --worktree

為本次任務建立新的 Git worktree。

\--tmux

搭配 worktree 建立 tmux 會話。

\--file

啟動時下載檔案資源到指定相對路徑。

---

如果從「新手優先級」排序，最值得先記的是：

- `--model`
- `--effort`
- `--permission-mode`
- `--debug`
- `--output-format`

---

## 四、哪些參數需要特別謹慎

以下參數需要特別小心，因為它們會直接影響 Claude Code 的 **行為邊界與安全邊界** ：

\--dangerously-skip-permissions

\--allow-dangerously-skip-permissions

\--tools

\--system-prompt

\--permission-mode

---

## 五、一級子命令總覽

Claude Code 常見的一級子命令：

claude agents

列出或管理 agents

claude auth

管理登入與帳號認證

claude auto-mode

查看 auto mode 分類器設定

claude doctor

檢查安裝與更新器健康狀態

claude install

安裝 Claude Code

claude mcp

管理 MCP 伺服器

claude plugin / plugins

管理插件

claude setup-token

設定長期認證 token

claude update / upgrade

更新版本

---

## 六、auth 認證命令

用於登入與查看認證狀態。

登入 Anthropic 帳號

預填信箱

使用 SSO 登入

claude auth logout

登出

claude auth status

查看認證狀態

claude auth status --json

JSON 格式顯示

claude auth status --text

文字格式顯示

---

## 七、auto-mode 命令

查看自動模式規則。

claude auto-mode config

輸出目前生效配置

claude auto-mode defaults

輸出預設規則

---

## 八、doctor、install、update、setup-token

claude doctor

檢查安裝與更新器健康狀態

claude install stable

安裝穩定版

claude install latest

安裝最新版

claude install 版本號

安裝指定版本

claude install --force

強制重新安裝

### 如果您喜歡我們的網站，並且希望支持我們的工作，您可以考慮捐款。我們接受各種形式的捐款，包括一次性捐款和定期捐款。您的捐款將幫助我們維護和改進網站，並為用戶提供更好的體驗。

[從零到一：利用 NotebookLM 打造企業專屬 AI 知識機器人實戰手冊](https://tools.wingzero.tw/article/sn/3823) [MySQL 遷移到 PostgreSQL：完整注意事項與實務指南](https://tools.wingzero.tw/article/sn/3825)

### 課程推薦

[![OpenClaw 龍蝦 AI 代理人實作課：零基礎打造 AI 辦公代理人](https://live.staticflickr.com/65535/55234945117_358540613c_o.jpg)](https://www.tiandiren.tw/product/c1625)

#### OpenClaw 龍蝦 AI 代理人實作課：零基礎打造 AI 辦公代理人

本課程從安裝部署、身份設定、技能安裝到實戰應用，帶你從零建立一個專屬的 AI 工作助手－OpenClaw 龍蝦，這個 AI 助手可以替你蒐集情報、摘要文件、整理郵件、生成內容、定時提醒，全部自動完成。

輸入折扣碼 **TC1625FR** 還可以額外獲得 NT$500 優惠喔。

[![Google Gemini AI 多媒體生成工具應用實戰：從 Imagen 4、Veo 3、Chirp、Lyria 到 nano-banana](https://live.staticflickr.com/65535/55113426540_68bda7cebd_o.jpg)](https://www.tiandiren.tw/product/c1609)

#### Google Gemini AI 多媒體生成工具應用實戰：從 Imagen 4、Veo 3、Chirp、Lyria 到 nano-banana

如何只用文字說明提示就生成不同的圖像結果？亦或是會議上的語音檔轉文字？這是一堂通盤性、全面性的Google Gemini AI多媒體生成工具應用實戰！

輸入折扣碼 **TC1609EW** 還可以額外獲得 NT$500 優惠喔。

[![AI 數據自動化工具實戰班：Google Sheet x Apps Script x NotebookLM 暨資料處理函數](https://live.staticflickr.com/65535/55113241088_6f77f7458e_o.jpg)](https://www.tiandiren.tw/product/c1615)

#### AI 數據自動化工具實戰班：Google Sheet x Apps Script x NotebookLM 暨資料處理函數

本門課將教你如何善用 AI 功能與工具，進行自動分析數據提供有幫助的建議與洞察，以及使用 AI 驅動的公式自動化計算。

輸入折扣碼 **TC1615VU** 還可以額外獲得 NT$500 優惠喔。

[![3 小時掌握自動化工作新手應用實作 – n8n AI Agent](https://live.staticflickr.com/65535/54757502159_5ab58ed9d1_o.jpg)](https://www.tiandiren.tw/product/c1600)

#### 3 小時掌握自動化工作新手應用實作 – n8n AI Agent

這門課程將帶你循序漸進掌握 n8n 的自動化技巧，從基礎認識與操作入門，到進階節點應用與流程控制，再到 Google 服務的整合實作，最後延伸至部署思維與 OpenAI API 的智慧化串接。

輸入折扣碼 **TC1600UY** 還可以額外獲得 NT$500 優惠喔。

[![用 AI 生成網站？ AI 高效網站設計實戰課：ChatGPT X HTML X SEO](https://i.imgur.com/qswuv2V.jpeg)](https://www.tiandiren.tw/product/c1533)

#### 用 AI 生成網站？ AI 高效網站設計實戰課：ChatGPT X HTML X SEO

利用 AI 提升網站設計效率與 SEO 排名！了解如何透過 ChatGPT 等工具快速建立 HTML 架構，優化關鍵字與用戶體驗，讓網站更具競爭力。

輸入折扣碼 **TC1533SL** 還可以額外獲得 NT$500 優惠喔。

[![AI工作術全面學習實戰營：6 堂精選課程，學會最好用 AI 工具，翻轉你的人生](https://i.imgur.com/zQ7ipsP.jpeg)](https://bit.ly/3UPY7sz)

#### AI工作術全面學習實戰營：6 堂精選課程，學會最好用 AI 工具，翻轉你的人生

《PChome雜誌》攜手 5 位在 AI 領域的專業講師，打造上述 6 堂實用課程，教你學會時下最好用的 AI 工具，導入生成式 AI 來產製工作內容，改造並升級你的工作流程。

輸入折扣碼 **ZERO2024** 還可以額外獲得 NT$400 優惠喔。

[![HTML與SEO實戰應用—並以ChatGPT助力提升網站品質與流量](https://i.imgur.com/PEkvqR7.jpeg)](https://mastertalks.tw/collections/pre-order/products/html-seo-chatgpt?ref=chiang)

#### HTML與SEO實戰應用—並以ChatGPT助力提升網站品質與流量

本課程專為希望深入了解 HTML 並有效結合 SEO 策略的學員設計。我們將重點放在 HTML 的深度學習與應用上，同時穿插介紹如何透過搜索引擎優化提升網站能見度。透過即時互動式的直播教學，加上 ChatGPT 的輔助，您將學習到如何建立一個結構優良、美觀且符合 SEO 標準的網站。這不僅會提升網站的用戶體驗，還會大幅提高網站的搜索引擎排名，進而增加訪客流量和潛在客戶。

[![用AI強化職場競爭力 ChatGPT、Midjourney從入門到精通](https://i.imgur.com/LrH4O42.jpg)](https://etstar.ettoday.net/courses/k4)

#### 用AI強化職場競爭力 ChatGPT、Midjourney從入門到精通

在快速變遷的職場中，提升競爭力成為關鍵。透過引領潮流的AI技術，ChatGPT和Midjourney將助您勇攀高峰。無論您是AI新手還是專家，這個課程將引導您從入門到精通，解密AI的奧秘，並學習如何運用於職場。

[![GitHub Copilot AI 程式碼編輯工具應用實務班](https://i.imgur.com/pAMMt3X.jpg)](https://www.tiandiren.tw/product/c1456)

#### GitHub Copilot AI 程式碼編輯工具應用實務班

讓學員瞭解有效地使用該工具來加速開發流程、提高程式碼品質和生產力。課程重點放在以 JavaScript 程式語言為例，介紹 Copilot 的基本原理、使用方法和最佳實踐。

輸入折扣碼 **TC1456JA** 還可以額外獲得 NT$500 優惠喔。

[![ChatGPT X Clipchamp AI 生成影片、配音與字幕應用實戰班](https://i.imgur.com/MfvhFHJ.jpg)](https://www.tiandiren.tw/product/c1451)

#### ChatGPT X Clipchamp AI 生成影片、配音與字幕應用實戰班

掌握Clipchamp AI的操作技巧，靈活運用Clipchamp AI進行影片編輯和創作，實現創意表達和傳播目的。

輸入折扣碼 **TC1451JAN** 還可以額外獲得 NT$500 優惠喔。

[![如何串接多種數位工具資訊？Looker Studio 資料視覺化實戰班｜GoogleAds x FB廣告 x GA流量數據](https://i.imgur.com/ilkfXdJ.jpg)](https://www.tiandiren.tw/product/c1270)

#### 如何串接多種數位工具資訊？Looker Studio 資料視覺化實戰班｜GoogleAds x FB廣告 x GA流量數據

Looker Studio除了可協助使用者監控網站流量、廣告成效、選擇匯入資源的管道之外，還可以將數據資料多平台整合、數據報表即時更新、數據範本可重複套用的效益，透過自動化系統，將數據全部匯入同一個報表平台，是企業不可或缺的重要工具。

輸入折扣碼 **TC1270JIA** 還可以額外獲得 NT$500 優惠喔。

<iframe frameborder="0" src="https://4fe7695d13932e0529fe6b116cf4c328.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="728" height="90" allow="private-state-token-redemption;attribution-reporting" aria-label="廣告"></iframe>

### 和我們交流

加入我們的社群，裡面會有一些技術的內容、有趣的技術梗，以及職缺的分享，歡迎和我們一起討論。

![](https://i.imgur.com/xKkxsXI.jpg)