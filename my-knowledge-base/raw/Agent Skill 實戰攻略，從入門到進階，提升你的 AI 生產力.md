---
title: "Agent Skill 實戰攻略，從入門到進階，提升你的 AI 生產力"
source: "https://blog.ray-realms.com/agent-skill-shi-zhan-gong-lue-cong-ru-men-dao-jin-jie-ti-sheng-ni-de-ai-sheng-chan-li/"
author:
  - "[[Ray 貓]]"
published: 2026-03-06
created: 2026-05-19
description: "如果你對 AI 的認知還停留在「每次開新對話都要重新貼上背景資料跟規則提示詞」，那你正浪費許多自己的時間與生產力。今天，我會用一篇文章快速帶你實戰 Agent Skill，帶你理解如何使用以及如何快速提升自己的生產力。讓我先來舉一個簡單的例子，假設我們想弄一個專門做「會計對帳」的 ChatGPT，就必須把對帳軟體的完整使用說明書、每日流程（例如：今天有什麼帳要對、怎麼查詢來源資料、遇到特例如何處理等）全部寫進提示詞裡。這就像是在交代新進的實習生，給他一份鉅細靡遺的 SOP。在過去，這看起來只是一個很普通的長篇提示詞，我們只是把流程用文字記錄下來。每次需要 AI 做這件事，我們就得把這段落落長的提示詞翻出來，重新複製貼上給它。但 Agent Skill 最大且最核心的改變，在於它可以替這份提示詞加上「說明（Description）」，告訴 Agent 只有在特定情境下，才需要去讀取這個 Skill。這就像是在查閱百科全書，書中雖然有上萬個條目，但你可以先看目錄找到需要的內容，而不用把上萬頁全部翻完才找到目標。Agent"
tags:
  - "clippings"
status: processed
---
如果你對 AI 的認知還停留在「每次開新對話都要重新貼上背景資料跟規則提示詞」，那你正浪費許多自己的時間與生產力。

今天，我會用一篇文章快速帶你實戰 Agent Skill，帶你理解如何使用以及如何快速提升自己的生產力。

讓我先來舉一個簡單的例子，假設我們想弄一個專門做「會計對帳」的 ChatGPT，就必須把對帳軟體的完整使用說明書、每日流程（例如：今天有什麼帳要對、怎麼查詢來源資料、遇到特例如何處理等）全部寫進提示詞裡。這就像是在交代新進的實習生，給他一份鉅細靡遺的 SOP。

在過去，這看起來只是一個很普通的長篇提示詞，我們只是把流程用文字記錄下來。每次需要 AI 做這件事，我們就得把這段落落長的提示詞翻出來，重新複製貼上給它。

但 Agent Skill 最大且最核心的改變，在於它可以替這份提示詞加上「說明（Description）」，告訴 Agent **只有在特定情境下，才需要去讀取這個 Skill** 。

這就像是在查閱百科全書，書中雖然有上萬個條目，但你可以先看目錄找到需要的內容，而不用把上萬頁全部翻完才找到目標。Agent Skill 替所有提示詞都加上了說明目錄。你可以一次準備幾千個 Skill 放在那邊，當任務來臨時，AI 會先看說明去判斷：「在這幾千個技能裡，我現在只需要讀取、載入這一個。」

而且 Agent Skill 還可以自己維護這本「工作指南百科全書」，他可以根據經驗不斷迭代自己，這為我們的工作流帶來了兩個非常重要的顛覆性好處：

### 1\. 徹底的全自動化

以前想讓 AI 執行特定任務，我們必須打開筆記本，手動找到對應的提示詞，再貼進對話框裡，這個「尋找與貼上」的過程非常麻煩且耗費人工。現在，只要把 Agent Skill 丟給 AI，他便會永久記錄起來，當你下達指令，它就會自己去比對目錄、調用對應的 SOP 並開始處理，真正實現了全自動化。你再也不用當一個無情的複製貼上機器。

![](https://blog.ray-realms.com/content/images/size/w2400/2026/03/9d1a9789-2519-4dac-8b15-df47380b4bc0.jpg)

### 2\. 自動維護與自我進化

這是我認為最迷人的一點。目前大部分的 AI 都具備自動創建與自動維護 Skill 的功能。

當你把 SOP 丟給 AI 後，它每次工作都會根據這個 SOP 操作一遍。如果在過程中流程發生了變化，或是遇到了預期外的錯誤，它可以自己去維護並修改這個 Skill 的內容，做到不斷的自我進化與迭代。

以前我們可能要手寫提示詞來告訴 AI 該怎麼做；現在，我們只需要告訴 AI 去探索某個任務，並將探索成功的流程記錄成新的 SOP、封存為一個 Skill。

這是一個自我進化的過程：

- 第一次探索某個新任務，過程可能卡東卡西，而且還需要人類輔助幫忙才能進行，最後花兩個小時才摸索出來，並把這個過程寫成 Skill
- 第二次執行有了上次 Skill 的輔助，就只需要半小時，雖然偶爾還是會遇到不順的地方，但是他可以將過程中遇到的問題自己解決，並記錄下來。
- 第三次甚至只要 10 分鐘，越做越順、越做越好。

這賦予了 AI 一個極度強大的能力，我覺得這就很像是人類與其他生物拉開差距、建立文明的關鍵：\* **\*知識傳承的能力\*\*** 。

你的 AI 助手不再是每次重開機就失憶的金魚，而是能不斷累積專案經驗、跟著你一起進化的超級員工。

![](https://blog.ray-realms.com/content/images/size/w2400/2026/03/26a1b39b-7c33-4b66-8d1a-6d6227474b32.jpg)

## 實戰 Agent Skill

既然 Agent Skill 這麼強大，那我們該去哪裡用？

首先，並不是每一個 AI 工具都能使用 Agent Skill，而在 **這篇教學會主要以針對一般大眾的 Claude 為 *主***

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.19.17@2x.png)

> 如果你自己習慣使用 ChatGPT 的話，ChatGPT 網頁本身不支援 Skill，但你可以使用 ChatGPT 專為 Coding 特化的 Codex 工具來處理

### 第一步，開啟設定

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.19.25@2x.png)

### 第二步，進入 Capabilities

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.21.09@2x.png)

### 第三步，滑到底部，開啟開啟 Code execution and file creation

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.23.04@2x.png)

### 第四步，進到 Skill 頁面

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.25.06@2x.png)

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.20.00@2x.png)

在這裡你可以看到 Claude 提供的數個官方 Skill  
其實你已經可以跟 Claude 說：「幫我創建一個 Skill 專門做什麼任務」，但是這樣還是太慢了，所以我們來分享如何安裝其他人的 Skill

舉個最簡單的例子： **寫小說**

[https://skillsmp.com/skills/danielmiessler-personal-ai-infrastructure-releases-v3-0-claude-skills-writestory-skill-md](https://skillsmp.com/skills/danielmiessler-personal-ai-infrastructure-releases-v3-0-claude-skills-writestory-skill-md?ref=blog.ray-realms.com)  
這是別人所提供的一個技能。我在之後的章節會來介紹這個網站，這個網站上面搜羅了數十萬個非常優秀的技能，可以直接貼給 AI 使用。

這個技能主要是透過它，可以讓你的 AI 以優秀的敘事科學和修辭手法來進行寫作，快速讓 AI 擁有寫好故事的能力。ㄊ

### 第一步，下載整個檔案

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.31.41@2x.png)

### 第二步，新增進 Claude 裏頭

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.32.23@2x.png)

### 第三步，直接把檔案 ZIP 拖進去裡頭

通常來說，這一步就可以完成安裝了！但是這個 Skill 有個問題

Claude 規定技能的名稱不能有大寫，但這個技能的名稱用了大寫，所以報錯了，這邊我們必須修復一下技能

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.37.39@2x.png)

### 第三之一步：修復技能

首先解壓縮

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.39.09@2x.png)

再來，打開 writestory 資料夾，找到 Skill.md

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.39.32@2x.png)

> Skill.md 是一個技能中最重要的檔案，他紀錄了整個技能應該在何時被調用，以及核心的提示詞，還有說明其他檔案是幹嘛的。

這邊我們右鍵該檔案，用文字編輯修改該檔案（如果你有其他文字編輯器可以使用習慣的工具）

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.40.53@2x.png)

把第二行的 name 改成全小寫

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.41.56@2x.png)

`CMD + S` 存檔，然後重新把整個 /writestory 資料夾給壓縮

你就可以把這個改好的檔案重新上傳看看 Claude 了  
這邊我們就成功了，裡頭可以看到他完整的

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.43.21@2x.png)

### 第四步：使用技能

現在你可以直接在 AI 對話中使用技能了

![](https://blog.ray-realms.com/content/images/2026/03/CleanShot-2026-03-06-at-19.47.25@2x.png)

接下來每次遇到需要寫小說的時候，AI 就會自動讀取該技能並使用該技能

