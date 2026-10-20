---
title: "讓 MacBook 也能蓋上螢幕不休眠養龍蝦 AI Agent 的四種方法"
source: "https://www.koc.com.tw/archives/642626"
author:
  - "[[達小編]]"
published: 2026-05-15
created: 2026-05-22
description: "Apple 這樣設計有三大考量。首先是電源管理，合蓋後 Mac 會進入待機模式，大幅降低功耗。其次是硬體保護，如果 MacBook 在背包裡還繼續運作，散熱不足可能導致過熱。最後是自動更新機制，macOS 會在睡眠時偶爾喚醒系統安裝更新或同步資料。"
tags:
  - "clippings"
status: processed
---
許多 Mac 用戶都希望把 MacBook 變成桌上型工作站：外接螢幕、鍵盤和滑鼠，然後把筆電蓋上收好。這種用法被稱為「闔蓋模式」（Clamshell Mode），在 macOS 中早有支援。但問題是，如果你的 MacBook 沒有接外接螢幕，或者想在電池模式下合蓋繼續下載檔案、播放音樂，Mac 就會自動進入睡眠，這在以前是很正常的邏輯，但在 AI Agent 時代，這反而成為 MacBook 跑 AI 代理的限制（因為 macOS 的預設設計就是「蓋蓋子＝睡覺」，目的是節省電量和防止過熱）。不過，現在有幾種方法可以繞過這個限制，這邊也為大家簡單分享。  

### 為什麼 MacBook 蓋上蓋子就會睡覺？

Apple 這樣設計有三大考量。首先是電源管理，合蓋後 Mac 會進入待機模式，大幅降低功耗。其次是硬體保護，如果 MacBook 在背包裡還繼續運作，散熱不足可能導致過熱。最後是自動更新機制，macOS 會在睡眠時偶爾喚醒系統安裝更新或同步資料。

因此，如果你希望 Mac 合蓋後不睡覺，需要告訴系統「我有特殊需求」。

### 方法一：使用官方闔蓋模式（Clamshell Mode）

這是 Apple 官方支援的方式，也是最穩定的做法。但需要滿足以下條件：

- **連接外接螢幕** ：Mac 必須偵測到有外部顯示器
- **連接電源轉接器** ：建議插上充電器，雖然部分 Apple Silicon 機型在電池模式下也能運作
- **連接外接鍵盤和滑鼠** ：有線或藍牙皆可

**設定步驟：**

1. 將外接螢幕、鍵盤和滑鼠連接到 MacBook
2. 開啟筆電，等待 macOS 桌面在外接螢幕上出現
3. 合上筆電蓋子
4. 外接螢幕可能會閃一下，然後顯示桌面。如果沒有自動喚醒，按鍵盤或移動滑鼠即可

**特別注意** ：部分 M3 MacBook Air 和 14 吋 M3 MacBook Pro 機型，只有在闔蓋模式下才能同時驅動兩台外接螢幕。這是因為合蓋後內建螢幕關閉，釋放了硬體資源供第二台外接螢幕使用。而 M4 系列則無論開蓋或合蓋都支援雙外接螢幕。

### 方法二：使用 Amphetamine 應用程式

如果你不想接外接螢幕，或者想在電池模式下合蓋繼續運作，Amphetamine 是目前最熱門的解決方案。這款免費 App 可在 Mac App Store 下載。  

**設定步驟：**

1. 從 Mac App Store 下載並開啟 [Amphetamine](https://apps.apple.com/us/app/amphetamine/id937984704?mt=12)
2. 點擊選單列上的藥丸圖示
3. 選擇「快速設定」（Quick Settings）
4. 在「工作階段預設值」區塊中，取消勾選「允許系統在顯示器關閉時進入睡眠」
5. 啟動新的工作階段，可選擇「無限期」、「下載檔案期間」或「特定 App 執行期間」

設定完成後，即使合上蓋子且沒有接外接螢幕，Mac 也不會進入睡眠。

**替代方案** ：Caffeinated 是另一款付費 App（約 NT$120），功能類似，介面更現代化 [\[3\]](https://caffeinated.app/) 。

### 方法三：使用終端機指令

如果你不想安裝任何第三方軟體，可以直接用終端機關閉睡眠功能。

**開啟終端機，輸入以下指令：**

`sudo pmset -a disablesleep 1`

這會完全禁用睡眠模式。要恢復預設設定，請輸入：

`sudo pmset -a disablesleep 0`

**注意事項** ：這個指令會禁用所有情境下的睡眠功能，包括電池模式。長期使用可能導致電池耗盡或過熱，建議在插上電源時使用。

### 方法四：使用虛擬顯示器轉接頭

這是一個物理層級的解決方案。購買 HDMI 或 USB-C 的虛擬顯示器轉接頭（Dummy Plug），插上後 Mac 會以為連接了外接螢幕，自動進入闔蓋模式。  

這種轉接頭在電商平台上售價約 NT$200～400，體積小巧，適合需要頻繁移動的使用者。前 OpenAI 工程師 Will DePue 曾在社群上推薦這個方案，理由是「能物理控制筆電是否開機，很直觀」。

### macOS 系統設定中的睡眠選項

在較新的 macOS 版本中，Apple 提供了部分睡眠設定選項：

1. 打開「系統設定」，點擊「電池」
2. 滾動到最下方，點擊「選項」
3. 開啟「在使用電源轉接器時，避免在顯示器關閉時自動進入睡眠」

此外，在「鎖定螢幕」設定中，可以將「閒置時啟動螢幕保護程式」和「在使用電源轉接器且閒置時關閉顯示器」都設為「永不」。

**注意** ：macOS Monterey 及之前版本，還可以在「能源節約器」中直接勾選「避免電腦在顯示器關閉時自動進入睡眠」，但後續版本移除了這個選項。

### 最近趨勢：AI 代理讓合蓋模式再度熱門

2026 年 5 月，Business Insider 報導了一個有趣現象：許多 AI 開發者出門時把筆電蓋子留一條縫，目的是讓 AI 代理程式繼續在背景執行。這個現象在社群上引發了大量討論，許多人指出其實有更好的解決方案，但部分使用者表示這些技術設定「摩擦成本太高」，留一條縫反而最直觀 [\[4\]](https://www.businessinsider.com/how-to-keep-laptop-ai-agent-running-while-lid-closed-2026-5) 。

OpenAI 也在 TikTok 上回應了這個趨勢，暗示可能正在開發讓代理程式在雲端執行的方案，減少對本機筆電的依賴。

### 安全提醒

無論使用哪種方法，請注意以下事項：

- **不要將合蓋運作的 Mac 放入封閉空間** ：筆電在合蓋狀態下散熱能力下降，放入背包或抽屜可能導致過熱
- **建議插上電源使用** ：雖然 Apple Silicon 機型支援電池模式下的闔蓋運作，但插上電源更穩定
- **定期檢查溫度** ：長時間合蓋運作時，注意筆電是否過熱

### 總結

MacBook 的闔蓋模式原本是為了讓筆電當桌上型電腦使用而設計，但隨著 AI 代理、背景下載、音樂播放等需求增加，越來越多人希望在不接外接螢幕的情況下也能合蓋運作。Amphetamine 是目前最成熟的解決方案，終端機指令適合進階使用者，而虛擬顯示器轉接頭則提供了物理層級的簡潔方案。選擇哪種方法，取決於你的使用情境和技術舒適度。

### 資料來源

[\[1\] How to connect multiple monitors to MacBooks – Macworld](https://www.macworld.com/article/675869/how-to-connect-two-or-more-external-displays-to-apple-silicon-m1-macs.html) – https://www.macworld.com/article/675869/how-to-connect-two-or-more-external-displays-to-apple-silicon-m1-macs.html

[\[2\] How to stop your MacBook sleeping when the lid is closed – Macworld](https://www.macworld.com/article/673295/how-to-use-macbook-with-lid-closed-stop-closed-mac-sleeping.html) – https://www.macworld.com/article/673295/how-to-use-macbook-with-lid-closed-stop-closed-mac-sleeping.html

[\[3\] Caffeinated – Anti-Sleep App](https://caffeinated.app/) – https://caffeinated.app

[\[4\] Good news, AI coders: You can keep your laptop running while closed – Business Insider](https://www.businessinsider.com/how-to-keep-laptop-ai-agent-running-while-lid-closed-2026-5) – https://www.businessinsider.com/how-to-keep-laptop-ai-agent-running-while-lid-closed-2026-5

[\[5\] 筆電闔蓋模式不進入睡眠設定指南 – Gechic](https://www.gechic.com/tw/use-external-monitor-with-closed-laptop-windows-macbook/) – https://www.gechic.com/tw/use-external-monitor-with-closed-laptop-windows-macbook/

[\[6\] 設定 Mac 的睡眠和喚醒設定 – Apple 支援](https://support.apple.com/zh-tw/guide/mac-help/mchle41a6ccd/mac) – https://support.apple.com/zh-tw/guide/mac-help/mchle41a6ccd/mac