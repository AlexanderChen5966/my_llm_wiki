---
title: "[ Google IO 2026 ] Antigravity CLI 實際使用筆記與 Gemini CLI 搬家指南"
source: "https://medium.com/@simon3458/google-io-2026-antigravity-cli-cb19b42706a6"
author:
  - "[[Simon Liu]]"
published: 2026-05-20
created: 2026-05-20
description: "Google I/O 2026 主舞台把 Google Antigravity 變成「agent-first 開發平台」的主角 — — 同一套被稱為 Antigravity Harness 的 agent runtime，從 Gemini API 的 Managed Agents"
tags:
  - "clippings"
status: processed
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*nrFZskvHtbgiJlqAioh3vg.png)

\[ Google IO 2026 \] Antigravity CLI 實際使用筆記與 Gemini CLI 搬家指南

## 為什麼這件事突然發生：Gemini CLI 被併進 Antigravity

Google I/O 2026 主舞台把 **Google Antigravity** 變成「agent-first 開發平台」的主角 — — 同一套被稱為 **Antigravity Harness** 的 agent runtime，從 Gemini API 的 **Managed Agents** 、AI Studio 的 coding agent，一路接到 Antigravity 2.0 桌面版。

## Get Simon Liu’s stories in your inbox

Join Medium for free to get updates from this writer.

在這個敘事下，原本的 gemini cli 變得有點尷尬：它是另一套用 Node.js 寫、另一條維護線、跟桌面版沒共用 harness 的工具。所以 Google 直接把它砍掉、改在 Antigravity 下用 Go 重寫，命名為 **Antigravity CLI** ，指令叫 `agy` 。隨之而來的就是這次的 **遷移期** ——從 5/19 公告到 6/18 ， **給你整整 30 天搬家** 。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*fWsKmXb1_MWVGivJxSue6w.png)

對日常開發者最直觀的影響有兩件：

- 你 CI 裡的 `gemini "..."` 要換成 `agy "..."` 。
- 你的 `~/.gemini/` 設定（自訂指令、MCP server、hooks、skills）要搬。

好消息是 **這兩件都有官方工具直接搞定** 。下面是我在 macOS 上實際跑一輪的過程。

## 第一步：30 秒裝好 agy

- macOS / Linux 可以透過以下的指令進行安裝：
```c
curl -fsSL https://antigravity.google/cli/install.sh | bash
```
- Windows 用 PowerShell：
```c
irm https://antigravity.google/cli/install.ps1 | iex
```

安裝腳本自己會做完幾件事，我在 macOS 上跑的時候，腳本：偵測平台（ `darwin_arm64` ）、查 latest release（ `1.0.0` ）、下載＋驗 checksum、解壓到 `~/.local/bin/agy` ，然後把 PATH 寫進 `.zshrc / .bashrc / .zprofile / .bash_profile` 。整段過程在終端機看起來像這樣：

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_Xb9ScvwVX3PtVKxA2-qCA.png)

整段 install 約 10 秒。注意最後一行： 「Antigravity CLI installed successfully at ~/.local/bin/agy ✓ Run ‘agy’ to start the CLI」 。

裝完開新 terminal 就能跑 `agy --version` 。如果你的 `$PATH` 沒有 `~/.local/bin` ，腳本會主動寫進去——所以重新開一個視窗就好，不需要手改。

## 第二步：第一次啟動的 3 個問題

第一次跑 `agy` 會連續問你三件事： **同意條款 → 選主題 → 信任資料夾** 。沒有任何隱藏選項，但每個都值得停下來看一下。

## 1\. 選色系 ＋ 順手匯入 Gemini 擴充

第一個是色系選擇 — — 預設選 `light` 。對我這種終端機常開到深夜的人來說當然要拉到 `dark` 。但這頁真正的重點不是顏色，是右下角的 **Migration options** 。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*DeQLby-_SJi_yGQ7uCn-cA.png)

歡迎畫面：左邊選色系、右邊預覽 agent diff、下面那行 「Import extensions from Gemini CLI (1 found: Stitch)」 是這次最關鍵的入口，勾下去，理論上就不用手動再操作一次 agy plugin import gemini 。

## 2\. Terms of Service ＆ Data Use

第二個畫面是條款同意。值得注意的是它 **明確列出 AI coding agent 的已知風險** — — autonomous code execution、data exfiltration、prompt injection、supply chain。預設勾的是「允許 Antigravity 收集互動資料協助改善」， **可以反悔** ，之後在設定裡 opt out 即可。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*g45p3ITn2fH03PwgjbkiIQ.png)

### 3\. 信任這個資料夾

最後一個問題很重要 — — agent 要拿到讀／寫／執行的權限，必須 **逐資料夾授權** 。第一次在專案啟動 `agy` 都會出現這個確認框。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_7V2OzvY7ZVY-yGUTKUMJg.png)

「Antigravity CLI requires permission to read, edit, and execute files here.」 — — 預設選 No，要主動上去選 Yes。建議你只信任你自己的 workspace，不要對家目錄整個說 Yes。

選完信任後，會進到 `agy` 的主畫面。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*g-oejttIXYLgInGpf1ARqw.png)

## 模型

目前提供以下模型，包含最新的 Gemini 3.5 Flash 模型，可以去做使用，並且跟著 Antigravity Quota 去計算，至於 Antigravity Quota 給的不夠這個問題嘛…Google 官方對 GDE 講法，其實這塊一直都很曖昧，就等官方給予正確資訊吧：

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*SGS7XJQEGRtwxh811qsMag.png)

## 第三步：把舊的 Gemini CLI 設定搬過來

> 根據目前資訊，據說 Migration 會有可能出現搬移沒有成功的問題，建議大家可以稍等一下，我猜陸續會將此問題解決掉。

如果你在歡迎畫面忘了勾 *Import extensions from Gemini CLI* ，或你想之後重新跑一次，可以手動執行：

```c
agy plugin import gemini
```

這個指令會去掃 `~/.gemini/` ，把裡面每個擴充拆成五類處理： **skills、agents、commands、mcpServers、hooks** ，再轉成 Antigravity 的結構。實際輸出長這樣：

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*sDDaA83Q9JMRtEZSQrf_4w.png)

agy plugin import gemini 的輸出。Stitch 這個擴充裡： 1 個 command 自動轉成 skill、1 個 mcpServer 直接搬 ，沒有的項目印 skipped (not found) 。匯入後檔案 stage 在 ~/.gemini/antigravity-cli 。

幾個值得注意的轉換規則：

- Gemini CLI 的 **commands → Antigravity skills** 。命名空間不變，但檔案結構（前情提要、指令模板等）會被改寫成 Antigravity skill 的 manifest。
- **mcpServers 一對一搬** ， `command` 、 `args` 、 `env` 都保留——這意味著你接 Notion、GitHub、Linear 的 MCP server 不用重設。
- **skills / agents / hooks 若** `**~/.gemini/**` **沒寫** ，就會印 `skipped (not found)` ——這不是錯誤，是正常告知。
- **檔案會先 stage 在獨立目錄** （範例輸出顯示在 `~/.gemini/antigravity-cli/` ），不會直接動到正式設定，你可以先 review 再 commit。

遷移完，重新跑 `agy` 就會看到 plugin 列表多出剛搬過來的東西。CI 端記得同步把 `gemini …` 換成 `agy …` ，呼叫慣例幾乎一致（單次 prompt、stdin pipe、headless mode 全都支援）。

## Antigravity CLI 跟 Gemini CLI 相比的優勢

如果只是換指令名稱、改用 Go 寫，這次遷移就沒什麼好寫的。主要在以下兩件事上，會比之前更好：

### 1\. 共用 Antigravity 2.0 的 harness

同一個 agent runtime，意味著 **agent 能力一升級，CLI 與桌面同時受惠** — — 例如桌面 2.0 新加的 **Dynamic Subagents** （agent 自己 spawn 子代理平行做事）跟 **Scheduled Tasks** （cron 排程把 agent 變 autopilot），未來會直接落到 CLI 端。Gemini CLI 那條獨立線就沒有這個好處。

### 2\. 非同步、多 agent 編排

過去在 Gemini CLI 跑大型重構或多主題研究，要靠 `tmux` 自己切 session。Antigravity CLI 內建多 agent 排程——你丟一個任務、agent 自己拆成多支在背景跑，主終端不會被鎖死。對「同時改多個 repo」這類大重構是質的差別。

> 把 CLI 跟桌面版接到同一套 harness， **就等於把終端機開發者跟 GUI 開發者放回同一條升級線** 。

## 對照表：Antigravity CLI vs Gemini CLI

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Pv0BUBy5hGRdiUNjs0BD0w.png)

## 總結：不只是換殼，更是接軌 Agent 生態系的必然升級

要在短短 30 天內（至 2026/06/18）強迫個人用戶改變習慣，聽起來或許有點擾民，但從 Google 這次的大動作可以看出他們整合 AI 開發工具的決心。將 Node.js 寫的 Gemini CLI 砍掉，換成 Go 寫的單一 Binary `agy` ，並不是單純的技術重構，而是為了解決「終端機」與「桌面端 GUI」的割裂感。

這次遷移最大的意義在於 **底層 Runtime 的統一** 。當 CLI 也接上了 Antigravity 2.0 的 Harness，終端機開發者就不再是次等公民。未來桌面版擁有的 Dynamic Subagents、非同步多 Agent 編排等強大功能，都能在 CLI 裡無縫享受，真正實現了「Agent-first」的開發體驗。

## I am Simon

大家好，我是 Simon 劉育維，是一位 AI 領域解決方案專家，目前也擔任 Google Cloud AI 領域開發者專家 (GDE)，期待能夠幫助企業導入人工智慧相關技術解決問題。如果這篇文章對您有幫助，請在 Medium 上按一下鼓勵，並追蹤我的個人帳號，這樣您就可以隨時閱讀我所撰寫的文章。歡迎在我的 Linkedin 上留言提供意見，並與我一起討論有關人工智慧的主題，期待能夠對大家有所幫助！

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*oIxjIZ8kiSz623Zj.png)

My Personal Website: https://simonliuyuwei.my.canva.site/link-in-bio