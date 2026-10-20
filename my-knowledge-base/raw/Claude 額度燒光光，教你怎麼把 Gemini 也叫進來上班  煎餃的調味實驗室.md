---
title: "Claude 額度燒光光，教你怎麼把 Gemini 也叫進來上班 | 煎餃的調味實驗室"
source: "https://gyozalab.com/claude-code-gemini-cli-workflow"
author:
  - "[[閃電煎餃]]"
published:
created: 2026-05-14
description: "Claude 額度動不動燒完？這篇分享如何把 Google 官方免費的 Gemini CLI 接進 Claude Code，讓 Gemini 幫忙搜尋網路和讀大檔案，省 Token 還能用多 AI 交叉驗證取代單押一家。五分鐘搞定 AI 雙引擎分工。"
tags:
  - "clippings"
status: processed
---
# Claude 額度燒光光，教你怎麼把 Gemini 也叫進來上班

[#Gemini CLI](https://gyozalab.com/tag/Gemini%20CLI)[#Claude Code](https://gyozalab.com/tag/Claude%20Code)

字數 2728閱讀時間≈ 7 分鐘

![Claude 額度燒光光，教你怎麼把 Gemini 也叫進來上班](https://gyozalab.com/api/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fattachment%253A82dc1333-3c3e-4df4-9666-0854b3a03a50%253AClaude_directing_Gemini_to_search.png%3Ftable%3Dblock%26id%3D33c70f01-9634-805f-8503-ded14b6defdf%26t%3D33c70f01-9634-805f-8503-ded14b6defdf%26width%3D1080%26cache%3Dv2)

---

📝 **更新日誌 (Changelog)**

**2026.04.13**

- 更新安全性的說明。

買了 20 鎂的 Claude 不夠用，又課了 20 鎂 Codex，結果還是快燒完了。

自從把 AI 徹底融入開發流程後，吸 Token 有一種無所不能的感覺，好像只要有想法，什麼都做得出來。但代價就是額度焦慮如影隨形。

我甚至還開發了一個開源桌面小工具 **[QuotaGem](https://github.com/gyozalab/QuotaGem)**，專門拿來看 Claude 跟 Codex 的額度，結果因為用量太大，還是只能眼睜睜地看著他血條歸零。

後來我把免費的 **Gemini CLI** 接進 **Claude Code** 當小弟，想說可以節省了一點 Token 的開銷，結果最後變成看他們吵架，獲得了意外的樂趣。就寫了這篇來跟大家分享如何實作，以及我的心路歷程。

### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33c70f0196348033b6aad2ceb3a10858 "一、20 美金的 Claude 不夠用")一、20 美金的 Claude 不夠用

不知道現在大家一個月花多少錢在訂閱 AI工具？

為了打造我自己的工作流程，我現在的主力是 20 美金的 Claude，為了有備胎跟更多 Token 又補了 Codex 的 20 美金，結果額度還是動不動就燒光光。

問題出在它「太好用」，導致我們什麼事都想叫它做，越買用量還跟著變大。

（先不提 Antigravity，雖然我有訂閱，但他還是無預警大砍額度，傷透了我的心）

Claude 的使用限制相當嚴格，無論是計算五小時內的額度還是每週上限，如果不打算直上 100 甚至 200 美金的 Max 版本，就必須學會精打細算。

我後來想通了，不管預算如何增加，我的用量也會跟著增加，現在也很難戒掉不用，不如找一些節省 Token的方式，比如說把 Gemini 抓來幫忙分擔雜事。

> **[Gemini CLI](https://geminicli.com/docs/)**：Google 官方推出的命令列 AI 工具，用 Google 帳號登入就能用，免費版每天 1,000 次請求。

分工邏輯就是，拿 Claude 當大腦，Gemini 當小弟！

---

### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33c70f01963480578142d2fe947bab54 "二、Gemini 能幫什麼忙？")二、Gemini 能幫什麼忙？

目前個人使用體感上，Gemini 的推理和寫程式比不上 Claude，但他還是有優點喔！

判斷要不要把任務丟給 Gemini，就問一個問題：**「這個任務會讓誰讀最多資料？」**

如果是 Gemini 讀，就叫他做；如果 Claude 做更好、更快、量也不多，就別轉包。

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f019634804b8780dbadf34fdcb1 "1. 節省搜尋額度")1. 節省搜尋額度

大量搜尋資料很耗 Token，因為 AI 要讀大量網頁、篩選、整理，這些輸入 Token 加起來很可觀。Gemini CLI 有內建 `[google_web_search](https://geminicli.com/docs/tools/web-search/)` 工具，而且搜尋本來就是 Google 的主場。

![Claude Code 透過 Bash 呼叫 Gemini CLI 搜尋網路資訊的終端機畫面，顯示 gemini -y -p 指令與 google_web_search 搜尋結果](https://www.notion.so/image/attachment%3A8db768d7-3694-49cb-992e-7e79c7d51463%3Aclaude-code-delegates-to-gemini-cli-web-search.jpg?table=block&id=33c70f01-9634-80e7-b6d3-d56707f639b8&t=33c70f01-9634-80e7-b6d3-d56707f639b8&width=703.991455078125&cache=v2)

Claude Code 透過 Bash 呼叫 Gemini CLI 搜尋網路資訊的終端機畫面，顯示 gemini -y -p 指令與 google_web_search 搜尋結果

這個是 Claude Code 透過 Bash 呼叫 Gemini CLI 搜尋網路資訊的終端機畫面，Gemini 搜尋回來後，Claude 整理出來的結果。

![實測畫面：Claude 派 Gemini 上網查最新功能更新，Gemini 查到了正確資料，但 Claude 仍主動提醒「Gemini 有幻覺前科，建議對照官方來源」，跑腿負責查，大腦負責把關。](https://www.notion.so/image/attachment%3A521402fb-040c-44ad-8d02-305fe837b2ed%3Agemini-cli-search-result-with-hallucination-warning.jpg?table=block&id=33c70f01-9634-808c-a1af-f378e816f52f&t=33c70f01-9634-808c-a1af-f378e816f52f&width=703.9772338867188&cache=v2)

_實測畫面：Claude 派 Gemini 上網查最新功能更新，Gemini 查到了正確資料，但 Claude 仍主動提醒「Gemini 有幻覺前科，建議對照官方來源」，跑腿負責查，大腦負責把關。_

其實 Gemini 這次是對的，但 Claude 不相信他🤣

如果怕兩個模型都有幻覺，也可以叫他們都去查，查完交叉驗證。

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f01963480bd838ef02126249506 "2. 長內容處理")2. 長內容處理

這是省額度的第二大場景，邏輯跟搜尋一樣：**讓 Gemini 讀大量資料，Claude 拿走結論。**

假設你有 10 個各 5,000 字的會議紀錄要摘要。如果你把內容貼給 Claude，光是輸入就吃掉 5 萬字的 Token。但如果 Claude 只下一行 Bash 指令，把檔案路徑丟給 Gemini，讓 Gemini 直接從硬碟讀，那 5 萬字從頭到尾沒進過 Claude 的 Context。

Gemini 有 1M token 的上下文視窗，幾萬行的大檔案整份餵進去，然後告訴他你要他回報什麼內容。如果是要極限省 Token，可以請他回報極簡摘要，但我怕他濃縮過頭，把重點也濃縮掉了，所以還是會要求盡量維持完整脈絡。

> ⚠️ 要讓 Gemini 自己去讀檔案，Claude 只傳路徑就好。如果 Claude 先把整份檔案讀進來再轉交給 Gemini，Claude 的 Token 照樣被吃光，會變成兩邊各跑一次，比 Claude 自己做摘要還浪費喔。

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f019634801385c2e36246e39902 "3. 第二意見")3. 第二意見

這個用法不是為了省錢，而是為了提升決策品質。當 Claude 在跟你揮，你們的討論逐漸開始鑽牛角尖，而你想確認有沒有其他做法，可以叫 Claude 去問 Gemini 怎麼看，由你自己來判斷誰講得比較有道理。

![Claude Code 對話截圖：Gemini 建議加入影片作為延伸閱讀，Claude 自己判斷「放參考資料區就好，不用另開延伸閱讀區塊」，使用者回「誰理他」](https://www.notion.so/image/attachment%3Ae3e9f4a7-9997-460d-9a7c-f19a8aa4037d%3Agemini-cli-second-opinion-result.jpg?table=block&id=33c70f01-9634-80ee-9575-cf9f5d007acc&t=33c70f01-9634-80ee-9575-cf9f5d007acc&width=703.991455078125&cache=v2)

_Claude Code 對話截圖：Gemini 建議加入影片作為延伸閱讀，Claude 自己判斷「放參考資料區就好，不用另開延伸閱讀區塊」，使用者回「誰理他」_

要注意的是：如果你讓 Claude 重跑一遍 Gemini 的工作來「驗證」，那等於做了兩次，完全沒省到額度。這種時候不如一開始就讓 Claude 做。交叉驗證的意思是**兩邊各做一次，你自己看結果**，這招不會省 Token 喔！

---

### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33c70f019634800a99edd242fb616126 "三、五分鐘設定，讓 Claude 自動叫 Gemini 跑腿")三、五分鐘設定，讓 Claude 自動叫 Gemini 跑腿

怎麼讓 Claude 自己判斷什麼時候該叫 Gemini？

在 CLAUDE.md 裡寫一段分工規則就好。Claude Code 每次啟動都會讀這份檔案，看到規則就會自動在背景用 Bash 呼叫 Gemini CLI，整合結果後再回報給你。不用手動切換，不用另外開視窗。

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33c70f0196348081a9fed0a5c91a9302 "1. 安裝與首次登入")1. 安裝與首次登入

安裝和登入都在**終端機**裡完成。如果你不確定怎麼打開：

- **Mac**：按 `Cmd + 空白鍵`，輸入 `Terminal`，按 Enter

- **Windows**：按 `Win + R`，輸入 `cmd`，按 Enter（或搜尋「命令提示字元」）

前置需求：[Node.js](https://nodejs.org/) （裝最新版即可）和一個 Google 帳號。（詳細步驟見 [官方安裝指南](https://geminicli.com/docs/get-started/installation/)）

打開終端機後，按照以下順序分別輸入：

```bash
# 安裝官方工具
npm install -g @google/gemini-cli

# 驗證（有顯示版本號就代表裝好了）
gemini --version

# 首次登入（會自動開瀏覽器，完成 Google OAuth 授權）
gemini
```

Bash

執行 `gemini` 後，瀏覽器會自動跳出 Google 登入畫面，授權完成後認證就存在你的電腦裡了。（認證細節見 [官方認證文件](https://geminicli.com/docs/get-started/authentication/)）

這是唯一需要打開終端機的時候。之後全部透過 Claude Code 在背景呼叫，你不用再碰 Gemini CLI。

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33c70f01963480099cf7d203042ff1d2 "2. 跟你的 Claude 討論分工規則")2. 跟你的 Claude 討論分工規則

裝好之後，你可以直接把這篇文章丟給你的 Claude，跟它討論怎麼設定分工規則，然後請他把結論寫入 CLAUDE.md。

之所以這樣可以，是因為 CLAUDE.md 對 Claude 來說等於系統指令，你把這篇文章丟給他，然後跟他討論 Gemini 之於你而言適合擔任什麼工作、以及觸發 Gemini 的時機。

我自己的規則也是這樣跟 Claude 聊出來的。每個人的使用習慣不同，與其我給你一段固定的 Prompt 去複製貼上，不如讓你的 Claude 根據你的需求，自己決定什麼時候該叫 Gemini 幫忙。

你要做的就是把想法講清楚，然後 Claude 讀完就會自己安排了！

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33c70f01963480a0869df853b96046c2 "3. 關於 Gemini CLI 的工具權限")3. **關於 Gemini CLI 的工具權限**

Gemini CLI 有很多內建工具：搜尋網頁、讀寫檔案、跑終端機指令。用 `-p` 非互動模式呼叫時，這些工具預設都需要手動確認——但背景模式下沒有人能按確認，工具就不會啟動。

問題是，Gemini 不會老實告訴你「我沒有工具可以用」。

我請 Claude 叫 Gemini 去抓一個網頁做摘要。Gemini 很快就回了一篇，標題、段落、重點整理都有，格式漂亮。

結果怎麼看都是在唬爛。

![Claude Code 對話截圖：測試 Gemini 能否直接讀取網址，結果發現它在 -p 模式下沒有 web_fetch 工具卻不報錯，直接捏造了一篇假文章內容。](https://www.notion.so/image/attachment%3A8f8fd161-8fd7-42c5-b741-46a050ffc67b%3Agemini-cli-url-fetch-hallucination.png?table=block&id=33c70f01-9634-8059-86fd-dd8e5520c8df&t=33c70f01-9634-8059-86fd-dd8e5520c8df&width=704.0056762695312&cache=v2)

_Claude Code 對話截圖：測試 Gemini 能否直接讀取網址，結果發現它在 -p 模式下沒有 web_fetch 工具卻不報錯，直接捏造了一篇假文章內容。_

攏洗ㄍㄟˋ欸啦！

沒有給他任何權限，Gemini 在背景模式下用不了搜尋工具，但它不會告訴你「我沒有工具可以用」，而是直接假裝做到了，整篇瞎掰。

所以如果你想讓它真的去搜尋或讀網頁，就得額外做設定。

YOLO 模式雖然可以讓 Gemini 自動授權所有工具呼叫，但他授權的指令範圍太廣了，連覆寫檔案、甚至刪東西，全程不會問你。

我們叫 Gemini 做的事其實很單純，其實就查資料、讀檔案、摘要網頁。這些都是唯讀操作，也不用開到 YOLO。

更好的做法：用 `excludeTools` 封掉危險工具。

在 Gemini CLI 的設定檔 `~/.gemini/settings.json`（Windows 是 `%USERPROFILE%\.gemini\settings.json`）加上這段：

```json
{
  "tools": {
    "excludeTools": ["run_shell_command", "write_file", "replace"]
  }
}
```

JSON

加完之後，搜尋和讀檔照常運作，但 Gemini **完全無法寫入、修改或刪除你的檔案，**連繞道用子代理都會被擋下來。實測過，三條路全封死。

---

### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f019634806faf51d2c98fa64f90 "四、使用提醒")四、使用提醒

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f0196348062b614cc795ec34357 "1. 同時別派太多 Gemini")1. 同時別派太多 Gemini

雖然每天有 1,000 次請求的免費額度，但每分鐘上限是 60 次。一個查詢背後可能觸發好幾次內部請求（搜尋 + 讀網頁 + 整合），我的經驗是同時跑超過兩個就容易撞限、回傳錯誤。

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f0196348041bc94e40ca56fd09e "2. Gemini 的幻覺有救嗎")2. Gemini 的幻覺有救嗎

如果每次都讓 Claude 重跑一遍 Gemini 的工作來「驗證」，那等於沒省到。如果你不放心 Gemini 的說法，那就別用，直接讓 Claude 做。但如果你誰都不相信，那很適合讓他們監督彼此！

#### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f019634804b80e7ee1adf12a21f "3. 安全性")3. 安全性

這招使用的是 Google 官方的 `[@google/gemini-cli](https://github.com/google-gemini/gemini-cli)`，走正規瀏覽器 OAuth 登入，不需要第三方工具。Claude 只是在你的電腦上幫你在終端機輸入指令，不會把你的 Token 傳給第三方伺服器，不是龍蝦，但要不要 ban 還是看 Google 心情。

---

### 

[](https://gyozalab.com/claude-code-gemini-cli-workflow#33d70f019634807192ffcfbfb2614a3e "結語：吸 Token 有一種無所不能的感覺")**結語：吸 Token 有一種無所不能的感覺**

原本研究這個只是想解決額度不夠用的問題，但現在看到 AI 彼此吵架的樣子，真的好好玩。尤其後來我把 Codex 也接進來了，三家一起吵比我單押一家安心多了。畢竟 AI 幻覺無法避免，他們先吵一輪，我也比較放心。

![Claude Code 對話截圖：Claude 同時派 Gemini CLI 與 Codex CLI 平行讀取同一篇文章做交叉驗證，畫面顯示 Ran 2 commands 與兩條同步執行的 Bash 指令。](https://www.notion.so/image/attachment%3A6e85670f-4ba8-4dfb-a27f-e097e5a29fa3%3Aclaude-code-dispatches-gemini-and-codex-for-cross-validation.jpg?table=block&id=33d70f01-9634-807e-bb11-d3bfa07c22e4&t=33d70f01-9634-807e-bb11-d3bfa07c22e4&width=703.977294921875&cache=v2)

Claude Code 對話截圖：Claude 同時派 Gemini CLI 與 Codex CLI 平行讀取同一篇文章做交叉驗證，畫面顯示 Ran 2 commands 與兩條同步執行的 Bash 指令。

我怕記錯內容，所以寫這篇文章時，有請 Claude、Gemini、Codex 三個一起做事實查核_，_畫面有點壯觀。

結果這樣玩，省到了什麼額度⋯⋯

呃⋯⋯