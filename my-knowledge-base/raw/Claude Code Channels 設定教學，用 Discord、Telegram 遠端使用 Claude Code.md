---
title: "Claude Code Channels 設定教學，用 Discord、Telegram 遠端使用 Claude Code"
source: "https://www.darrelltw.com/claude-code-channels-discord-telegram/"
author:
  - "[[Darrell]]"
published: 2026-03-20
created: 2026-05-22
description: "Claude Code Channels 讓你從手機 Discord 或 Telegram 傳訊息給正在跑的 Claude Code，不用坐在電腦前也能讓 AI 幫你改 code、跑指令。完整 Discord Bot 和 Telegram Bot 設定教學和幾個場景測試"
tags:
  - "clippings"
status: processed
---
## 不在電腦旁，也想讓 Claude 幫忙做事？

用 Claude Code 一段時間後，蠻常遇到一個情境：  
人在外面，突然想到「啊那個 bug 可以先讓 Claude 去查一下」，但電腦在家裡，只能乾等回去再說

以前只能等回到電腦前才能動手  
現在 Claude Code 推出了 **Channels** 功能，直接用手機的 Discord 或 Telegram 傳訊息給 Claude Code session  
Claude 在電腦上執行完，結果回傳到手機

## 先搞清楚：Channels 跟 Remote Control 不一樣

Claude Code 最近同時推出了 **Remote Control** 和 **Channels** 兩個功能，很容易搞混  
簡單來說：

- **Remote Control** = 遠端桌面，從手機瀏覽器或 Claude App 連回電腦繼續操作
- **Channels** = 訊息收件匣，Discord/Telegram 訊息從外面推進來觸發 Claude

| 比較項目 | Remote Control | Channels |
| --- | --- | --- |
| 本質 | 遠端操作同一個 session | 外部訊息推入 session |
| 介面 | claude.ai/code 網頁、Claude App | Discord Bot、Telegram Bot |
| 遠端核准權限 | ✅ 可以從手機按核准 | ✅ Permission Relay（Allow / Deny 按鈕） |
| 接收外部事件 | ❌ | ✅ |
| 多人使用 | ❌ 綁定你的帳號 | ✅ 可以透過 allowlist 控制 |
| 啟動方式 | claude --remote-control | claude --channels plugin:... |

Channel 本質上是一個 MCP server  
多了 `--channels` 這個啟動參數來啟用

## 前置條件

| 項目 | 說明 |
| --- | --- |
| Claude Code 版本 | v2.1.80 以上（可用 /status 查詢） |
| 登入方式 | 目前必須用 claude.ai 帳號登入（API key 不支援 Channels） |
| Bun runtime | Channel plugin 需要 Bun 來執行 |
| Team/Enterprise | 管理員需先到後台開啟 channelsEnabled 設定 |

### 安裝 Bun

如果還沒裝過 Bun，一行指令搞定：

```bash
curl -fsSL https://bun.sh/install | bash
```

裝完確認一下版本：

```bash
bun --version
```

## Discord Channel 設定

這次要建的是完整的 DiscordBot，步驟會比 Webhook 還要複雜一點！

### 第一步：建立 Discord 應用程式

1. 打開 [Discord Developer Portal](https://discord.com/developers/applications)
2. 點右上角 **新增應用程式(New Application)**
3. 取個名字（例如 `My Claude Bot` ），點 Create
![Discord Developer Portal 建立新 Application](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_developer_portal_new_app.png)

Discord Developer Portal 建立新 Application

### 第二步：設定 Bot 權限和 Message Content Intent

1. 左側選單點 **機器人** （Bot）
2. 往下捲到 **機器人權限** ，勾選這些：
	- 檢視頻道（View Channels）— 在「一般權限」
		- 傳送訊息（Send Messages）— 在「文字權限」
		- 在討論串中傳送訊息（Send Messages in Threads）— 在「文字權限」
		- 讀取訊息歷史記錄（Read Message History）— 在「文字權限」
		- 附加檔案（Attach Files）— 在「文字權限」
		- 新增反應（Add Reactions）— 在「文字權限」
![在機器人頁面勾選需要的權限](https://www.darrelltw.com/claude-code-channels-discord-telegram/bot_permissions.png)

在機器人頁面勾選需要的權限

3. 再往下把 **Message Content Intent** 打開
![開啟 Message Content Intent 才能讓 Bot 讀取訊息內容](https://www.darrelltw.com/claude-code-channels-discord-telegram/bot_message_content_intent.png)

開啟 Message Content Intent 才能讓 Bot 讀取訊息內容

注意事項

Message Content Intent **超級重要** ，很多人會漏掉。不開的話，Bot 收到的訊息內容會是空的

### 第三步：取得 Bot Token

1. 還是在 **機器人** （Bot）頁面，往上捲到 **Token** 區塊
2. 點 **Reset Token** （會要你確認）
3. 複製 Token
![點 Reset Token 取得 Bot Token，只會顯示一次](https://www.darrelltw.com/claude-code-channels-discord-telegram/bot_token_reset.png)

點 Reset Token 取得 Bot Token，只會顯示一次

注意事項

Token 只會顯示一次，沒複製到就要再 Reset 重新產生

### 第四步：邀請 Bot 進你的 Discord 伺服器

Discord 規定你必須跟 Bot 在同一個伺服器，才能私訊它

1. 左側選單點 **安裝** （Installation）
2. 確認「公會安裝」（就是伺服器安裝）有勾選，範圍選 **bot**
3. 這邊下面也需要確認權限，沒有的話再選一次那六個權限
4. 複製「安裝連結」的 URL
5. 在瀏覽器開啟 → 選「 **新增至伺服器** 」→ 選擇你的伺服器 → **授權**
![安裝頁面設定：①勾選公會安裝 ②範圍選 bot ③確認權限 ④複製安裝連結](https://www.darrelltw.com/claude-code-channels-discord-telegram/installation_page.png)

安裝頁面設定：①勾選公會安裝 ②範圍選 bot ③確認權限 ④複製安裝連結

開啟安裝連結後，選「 **新增至伺服器** 」

![選擇新增至伺服器，不是新增至我的應用程式](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_invite_choose.png)

選擇新增至伺服器，不是新增至我的應用程式

選擇你的伺服器，確認權限後按 **授權**

![選擇伺服器並確認 Bot 需要的權限](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_invite_server.png)

選擇伺服器並確認 Bot 需要的權限

![確認六個權限都有勾選，按授權完成](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_invite_permissions.png)

確認六個權限都有勾選，按授權完成

### 第五步：在 Claude Code 安裝 Discord Plugin

回到 Terminal(終端機)，開啟 Claude Code。

```
claude plugin marketplace add claude-plugins-official anthropics/claude-plugins-official
```

加完後安裝 Discord plugin：

```
claude plugin install discord@claude-plugins-official
```

如果你跟我一樣也沒有 `claude-plugins-official`  
顯示錯誤訊息

```
Plugin "fakechat" not found in any marketplace
Plugin "discord" not found in any marketplace
```

那就要先安裝這個 marketplace  
目前會缺少這個 marketplace 的原因未知，可能是我很久以前就開始用

![先加入 marketplace 再安裝 Discord plugin](https://www.darrelltw.com/claude-code-channels-discord-telegram/cc_plugin_install_marketplace.png)

先加入 marketplace 再安裝 Discord plugin

### 第六步：設定 Bot Token

在同一個 Claude Code session 裡，貼上第三步複製的 Token：

```
/discord:configure 你的Token貼在這裡
```

這會把 Token 存到 `~/.claude/channels/discord/.env`

### 第七步：帶 --Channel 重新啟動

先退出 Claude Code（ `/exit` 或 Ctrl+C），然後重新啟動：

```bash
claude --channels plugin:discord@claude-plugins-official
```

`--channels` 是關鍵，沒加的話 Bot 不會連線

![用 --channels flag 啟動 Claude Code 連接 Discord](https://www.darrelltw.com/claude-code-channels-discord-telegram/cc_channels_startup.png)

用 --channels flag 啟動 Claude Code 連接 Discord

### 第八步：配對你的 Discord

1. 去 Discord，在伺服器成員列表找到你的 Bot，右鍵 → **Message**
2. 隨便傳一句話
3. Bot 會回覆一個 **配對碼** （6 個字元）
4. 回到 Claude Code，輸入：

```
/discord:access pair 配對碼貼這裡
```

![Bot 回覆配對碼，複製貼到 Claude Code 完成配對](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_pairing_code.png)

Bot 回覆配對碼，複製貼到 Claude Code 完成配對

### 第九步：鎖定 Allowlist（建議）

配對完成後，建議把 Bot 切成 allowlist 模式  
這樣其他人私訊你的 Bot 不會收到配對碼：

```
/discord:access policy allowlist
```

![設定 allowlist 後只有已配對的帳號能使用 Bot](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_access_allowlist.png)

設定 allowlist 後只有已配對的帳號能使用 Bot

### 設定完成！

![透過 Discord 私訊讓 Claude Code 執行任務並回傳結果](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_actual_usage.png)

透過 Discord 私訊讓 Claude Code 執行任務並回傳結果

## Telegram Channel 設定

Telegram 的流程比 Discord 簡單很多，BotFather 一條龍搞定

### 第一步：建立 Telegram Bot

1. 在 Telegram 開啟 [@BotFather](https://t.me/BotFather)
2. 傳送 `/newbot`
3. BotFather 會問：
	- **Name** ：Bot 的顯示名稱（隨便取，可以有空格）
		- **Username** ：Bot 的帳號，必須以 `bot` 結尾（例如 `my_claude_bot` ）
4. BotFather 會回覆 Token，長得像 `123456789:AAHfiqksKZ8...`
5. 複製整串（包括前面的數字和冒號）
![在 BotFather 輸入 /newbot 開始建立 Bot](https://www.darrelltw.com/claude-code-channels-discord-telegram/telegram_botfather_newbot.png)

在 BotFather 輸入 /newbot 開始建立 Bot

![BotFather 回覆 Token，複製紅框標示的那段](https://www.darrelltw.com/claude-code-channels-discord-telegram/telegram_botfather_token.png)

BotFather 回覆 Token，複製紅框標示的那段

### 第二步：安裝 Plugin 和設定 Token

如果你之前設定 Discord 時已經加過 marketplace 就不用再加，直接安裝：

```
claude plugin install telegram@claude-plugins-official
```

設定 Token：

```
/telegram:configure 你的Token貼在這裡
```

![安裝 Telegram plugin 並設定 Token](https://www.darrelltw.com/claude-code-channels-discord-telegram/cc_telegram_plugin_setup.png)

安裝 Telegram plugin 並設定 Token

### 第三步：帶 Channel Flag 重新啟動

```bash
claude --channels plugin:telegram@claude-plugins-official
```

### 第四步：配對帳號

1. 在 Telegram 找到你剛建的 Bot，傳一句話
2. Bot 回覆配對碼
3. 回到 Claude Code：

```
/telegram:access pair 配對碼
```

4. 鎖定存取：

```
/telegram:access policy allowlist
```

![Telegram Bot 回覆配對碼，複製到 Claude Code 完成配對](https://www.darrelltw.com/claude-code-channels-discord-telegram/telegram_pairing_code.png)

Telegram Bot 回覆配對碼，複製到 Claude Code 完成配對

### 同時用 Discord + Telegram

如果兩個都想用，啟動時用空格分隔就好：

```bash
claude --channels plugin:discord@claude-plugins-official plugin:telegram@claude-plugins-official
```

## Discord vs Telegram，該選哪個？

兩個平台的 plugin 功能差異比想像中大，不只是「你習慣用哪個 App」這麼簡單

### 支援的工具不同

Discord 有 **5 個** MCP tools，Telegram 只有 **3 個** ：

| 工具 | Discord | Telegram | 說明 |
| --- | --- | --- | --- |
| reply | ✅ | ✅ | 回覆訊息 |
| react | ✅ | ✅ | emoji 反應 |
| edit\_message | ✅ | ✅ | 編輯之前發的訊息 |
| fetch\_messages | ✅ | ❌ | 讀取最近 100 筆歷史訊息 |
| download\_attachment | ✅ | ❌ | 下載附件(PDF 等等，圖片兩者都ok) |

`fetch_messages` 目前還測試不出用途在哪，因為兩者都有自己的記憶。  
除非是新的 Session 連線後，他需要往前讀起歷史紀錄來回想。

### 檔案和訊息限制

| 比較項目 | Discord | Telegram |
| --- | --- | --- |
| 單檔大小上限 | 25 MB | 50 MB |
| 每則訊息檔案數 | 最多 10 個 | 無限制 |
| 單則訊息字數 | 2,000 字元 | 4,096 字元 |
| 圖片品質 | 原始品質 | 預設壓縮（要原圖需選「以檔案方式傳送」） |
| Emoji | 所有 Unicode + 自訂 emoji | 只有固定一些 |

### 設定難度

Telegram 設定真的簡單到有點感動，跟 BotFather 聊兩句就拿到 Token  
Discord 就…要進 Developer Portal 建 Application、開 Intent、設權限，步驟蠻多的但照著做不會出錯

### 什麼情境選哪個？

我覺得就看個人習慣哪個聊天平台即可！  
當然也建議可以兩者都試試看，目前看起來 Discord 功能比較完整沒錯  
Telegram 就稍微單調一點(但安裝真的簡單很多)

## 實際使用場景

設定完之後，到底可以怎麼用？以下是自己實測的幾個場景

### 用 Discord 問明天天氣

直接在手機 Discord 傳「明天台北每小時的降雨機率多少」  
Claude 自己去查氣象資料，整理成表格回覆

![透過 Discord 問天氣，Claude 自動查詢並回覆降雨機率表格](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_actual_usage.png)

透過 Discord 問天氣，Claude 自動查詢並回覆降雨機率表格

電腦端的 Claude Code 會顯示完整的執行過程，可以看到它怎麼找資料的

![Claude Code 端顯示完整的查詢和回覆過程](https://www.darrelltw.com/claude-code-channels-discord-telegram/cc_channels_execution.png)

Claude Code 端顯示完整的查詢和回覆過程

### 用 Telegram 幫忙整理信箱

傳一句「今天信箱有什麼促銷信看起來很吸引人的」  
Claude 去讀信箱，整理出重點回覆到 Telegram

![透過 Telegram 請 Claude 整理今天的促銷信](https://www.darrelltw.com/claude-code-channels-discord-telegram/telegram_actual_usage.png)

透過 Telegram 請 Claude 整理今天的促銷信

### 人在外，遠端臨時改簡報

這應該是蠻多人很需要的一個需求  
人在外面通勤時突然有靈感或是需要調整簡報內容  
直接在 Discord 跟 Claude 說「幫我傳 XX 簡報給我」，收到檔案確認內容後  
再說「第七頁不需要 XX，幫我刪掉」，改完直接再傳回最新檔案

如果有串聯 Google 相關服務或是 CLI，還能直接上傳到 Google Drive 或是寄信給對方！

![透過 Discord 遠端請 Claude 修改 PPTX 簡報內容](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_pptx_remote_edit.png)

透過 Discord 遠端請 Claude 修改 PPTX 簡報內容

### 傳 PDF 讓 Claude 分析

在 Discord 直接丟一個 PDF 檔案，問「這 PDF 在說什麼」  
Claude 會下載附件分析內容回覆

![Discord 可以直接傳 PDF 讓 Claude 下載分析](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_pdf_success.png)

Discord 可以直接傳 PDF 讓 Claude 下載分析

注意事項

同樣的操作在 Telegram 會失敗，因為 Telegram plugin 沒有 `download_attachment` 這個 tool。這也是 Discord 和 Telegram 最大的功能差異之一

![Telegram 傳 PDF 時 Claude 無法下載附件](https://www.darrelltw.com/claude-code-channels-discord-telegram/telegram_pdf_fail.png)

Telegram 傳 PDF 時 Claude 無法下載附件

### 測試 Claude 幫訊息按讚

兩邊都支援 `react` tool，可以請 Claude 對訊息加 emoji 反應

![Discord 的 react emoji 效果](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_react_emoji.png)

Discord 的 react emoji 效果

![Telegram 的 react emoji 效果](https://www.darrelltw.com/claude-code-channels-discord-telegram/telegram_react_emoji.png)

Telegram 的 react emoji 效果

## 常見問題

❓ 這功能要付費嗎？

Channels 功能本身不用收費，Pro（$20/月）和 Max 訂閱方案都能用。Discord Bot 和 Telegram Bot 的建立也都免費。Team/Enterprise 方案需要管理員先開啟才能使用

❓ 每次都要打那一長串 --channels 指令嗎？

對，每次啟動都要加 --channels 參數。可以在 shell 設定一個 alias 省事，例如 alias cc-discord='claude --channels plugin:discord@claude-plugins-official'

### Permission Relay

官方 Discord / Telegram Plugin 在 0.0.2 到 0.0.4 版本連續更新了 **Permission Relay** 功能  
現在可以直接在手機上允許 Claude 的授權詢問

當 Claude Code 遇到需要授權的操作時（例如寫入檔案、執行指令），會直接把請求推到 Discord / Telegram  
你在手機上就能看到是什麼操作，點 **Allow** 或 **Deny** 決定要不要允許

以往都是需要回到電腦前才能授權，或是用 `--dangerously-skip-permissions` 開啟危險的跳過授權檢查

![Discord 上收到 Permission Relay 請求，可以直接按 Allow 或 Deny](https://www.darrelltw.com/claude-code-channels-discord-telegram/discord_permission_relay.png)

Discord 上收到 Permission Relay 請求，可以直接按 Allow 或 Deny

這功能讓 Channels 的方便性大幅提升，再也不會因為跳允許導致整個聊天過程停擺

提示

版本迭代過程： **0.0.2** 加入核心機制，用純文字回覆 yes/no + 驗證碼來核准 →  
**0.0.3** 改成 Allow / Deny 按鈕，不用再打字 →  
**0.0.4** 精簡顯示 + See more 展開詳情，避免訊息太長洗版

### 已知限制

目前多個 Bot 之間無法互相溝通。每個 Bot 是獨立的 Claude Code Session。  
Discord plugin 的程式碼裡直接忽略所有 Bot 發的訊息（ `if (msg.author.bot) return` ），  
所以即使把多個 Bot 放在同一個 Discord channel，它們也不能互相討論，只會各自回答你的訊息