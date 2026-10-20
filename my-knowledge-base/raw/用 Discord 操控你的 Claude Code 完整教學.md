---
title: "用 Discord 操控你的 Claude Code 完整教學"
source: "https://blog.ray-realms.com/yong-discord-cao-kong-ni-de-claude-code-wan-zheng-jiao-xue/"
author:
  - "[[Ray 貓]]"
published: 2026-03-20
created: 2026-05-19
description: "想要讓 Claude 直接在 Discord 裡面幫你處理任務、回覆訊息嗎？這篇教學將帶你透過 MCP 伺服器，將 Discord 機器人與 Claude Code 完美結合！設定完成後，當機器人收到訊息時，MCP 伺服器就會自動將訊息轉發給 Claude。更棒的是，Claude 還能直接使用工具來回覆、添加表情符號反應，甚至編輯訊息。讓我們開始吧！準備工作在開始之前，請確保你的電腦已經安裝了 Bun（我們的 MCP 伺服器是跑在 Bun 上的）。如果你還沒安裝，可以直接在終端機輸入以下指令快速安裝：Bashcurl -fsSL https://bun.sh/install | bash記得安裝完成後，可以輸入exec /bin/zsh"
tags:
  - "clippings"
status: processed
---
想要讓 Claude 直接在 Discord 裡面幫你處理任務、回覆訊息嗎？這篇教學將帶你透過 MCP 伺服器，將 Discord 機器人與 Claude Code 完美結合！

設定完成後，當機器人收到訊息時，MCP 伺服器就會自動將訊息轉發給 Claude。更棒的是，Claude 還能直接使用工具來 **回覆** 、 **添加表情符號反應** ，甚至 **編輯訊息** 。

讓我們開始吧！

---

## 準備工作

在開始之前，請確保你的電腦已經安裝了 [**Bun**](https://bun.sh/?ref=blog.ray-realms.com) （我們的 MCP 伺服器是跑在 Bun 上的）。

如果你還沒安裝，可以直接在終端機輸入以下指令快速安裝：

Bash

```
curl -fsSL https://bun.sh/install | bash
```
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.18.18.png)

記得安裝完成後，可以輸入

```
exec /bin/zsh 
bun --help
```

重整一下終端機的設定，確認 Bun 是摟有安裝成功喔

---

## 快速設定步驟

> **💡 溫馨提示** ：以下是針對「單人私訊 (DM)」機器人的預設配對流程。如果你需要群組或多人使用的設定，後續可以進一步參考進階的設定檔。

### 第一步：建立 Discord 應用程式與機器人

1. 前往 [Discord 開發者平台](https://discord.com/developers/applications?ref=blog.ray-realms.com) ，點擊右上角的 **New Application** 並幫它取個響亮的名字。
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.01.24.png)

1. 在左側選單點擊 **Bot** ，幫你的機器人設定一個使用者名稱。
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.02.18.png)

1. **（重要！）** 往下捲動找到 **Privileged Gateway Intents** 區塊，將 **Message Content Intent** 打開。如果沒開這個，你的機器人收到的訊息內容會是一片空白喔！
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.03.10.png)

### 第二步：取得機器人金鑰 (Token)

一樣在 **Bot** 頁面，往上捲動找到 **Token** 區塊，點擊 **Reset Token** 。

👉 **請立刻將這串 Token 複製並保存好** ，因為它只會顯示一次！我們在第五步會用到它。

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.03.50.png)

### 第三步：邀請機器人加入你的伺服器

Discord 有個機制：你必須和機器人在「同一個伺服器」內，才能私訊它。

1. 前往左側的 **OAuth2** → **URL Generator** 。
2. 在 Scopes 勾選 `bot` 。
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.32.02.png)

1. 在下方的 **Bot Permissions** 勾選以下權限：

*(註：如果你只打算用私訊功能，其實不需要任何權限，但現在先開好，以後想在伺服器頻道內使用就不用再跑一趟了！)*

![](https://blog.ray-realms.com/content/images/2026/03/-----------------2-.png)

1. 複製最下方的 **Generated URL** ，貼到瀏覽器打開，然後把機器人加入到你所在的任何一個伺服器中。
![](https://blog.ray-realms.com/content/images/2026/03/---------------.png)

### 第四步：安裝 Claude 外掛

打開終端機，先輸入 `claude` 啟動你的 session，接著執行以下指令安裝外掛：

```
/plugin install discord@claude-plugins-official
/reload-plugins
```
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.08.44.png)

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.10.15.png)

> 如果出了什麼錯誤，可以重開看看，或者輸入 /doctor 讓 Claude 幫你檢查喔

安裝完後，測試一下輸入 `/discord:configure` 看會不會自動補齊，如果沒有反應，請重啟你的 Claude。

### 第五步：將 Token 交給伺服器

把剛剛在第二步保存的 Token 填入：

```
/discord:configure 你的機器人Token...
```
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.41.35.png)

### 第六步：掛載頻道並重新啟動

下一步，先關掉 Claude，我們需要改用以下指令重新啟動：

```
claude --channels plugin:discord@claude-plugins-official
```
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.15.17.png)

通常來說，你的 Discord 機器人就會上線了，如果有問題請用 /doctor 詢問 Claude

### 第七步：配對綁定

現在，打開 Discord **私訊** 你的機器人

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.25.08.png)

隨便說句話。它會回覆你一組「配對碼 (Pairing code)」。

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.25.20.png)

複製，回到你的 Claude 終端機，直接貼上：

```
/discord:access pair <這裏填入你的配對碼>
```
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.25.35.png)

搞定！你傳給機器人的下一條私訊，就會直接傳達給 Claude 助理了。

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.45.13.png)

### 第八步：鎖定權限保護

剛剛的配對模式只是為了抓取你的 Discord ID。為了防止陌生人私訊機器人並拿到配對碼，請記得鎖定權限。

你可以直接叫 Claude 幫你鎖定，或者手動輸入：

```
/discord:access policy allowlist
```
![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-20-at-10.45.54.png)

---

## 🔐 權限與安全設定

- **識別方式** ：系統是透過 Discord 的 **Snowflake ID** （一串數字）來識別使用者的。（在 Discord 開啟開發者模式後，右鍵點擊使用者就能「複製 ID」）。
- **預設政策** ：一開始是 `pairing` （配對模式），設定好後強烈建議切換為 `allowlist` （白名單模式）。
- **伺服器頻道** ：預設是不開啟的，需要針對特定的頻道 ID 選擇性開啟。

---

## 🤖 Claude 在 Discord 裡能做什麼？(內建工具)

當你在等待回覆時，Discord 會自動顯示「機器人正在輸入訊息… (botname is typing…)」，非常逼真！以下是 Claude 可以調用的工具列表：

| **工具名稱** | **功能用途** |
| --- | --- |
| `reply` | **發送訊息** ：可以回覆特定頻道，支援原生串聯回覆 (`reply_to`) 與附加檔案。最多可附 10 個檔案，每個最大 25MB。長訊息會自動分段。 |
| `react` | **表情回應** ：為特定訊息加上 Emoji。支援一般 Unicode Emoji，自訂表情則使用 `<:名稱:ID>` 格式。 |
| `edit_message` | **編輯訊息** ：編輯機器人自己發送過的訊息。非常適合用來顯示「處理中...」到「處理完成」的進度更新。 |
| `fetch_messages` | **讀取歷史** ：抓取頻道的近期訊息（從舊到新），每次最多 100 筆。這是機器人回溯對話上下文的唯一方式。 |
| `download_attachment` | **下載附件** ：從特定訊息下載所有夾帶的檔案到本地資料夾。 |

### 📎 關於檔案與附件處理

請注意， **附件不會被自動下載** ！

當你傳送檔案時，Claude 會先收到檔案的「名稱、類型和大小」等資訊。如果 Claude 判斷需要查看該檔案，它會主動呼叫 `download_attachment` 工具，將檔案下載到你的本機目錄 (`~/.claude/channels/discord/inbox/`) 進行分析。

---

希望這篇教學能幫你順利把 Claude 請進 Discord 裡！