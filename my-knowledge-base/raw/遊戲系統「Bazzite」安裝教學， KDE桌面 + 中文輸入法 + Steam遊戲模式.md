---
title: "遊戲系統「Bazzite」安裝教學， KDE桌面 + 中文輸入法 + Steam遊戲模式"
source: "https://ivonblog.com/posts/install-bazzite/"
author:
  - "[[Ivon Huang]]"
published: 2026-03-01
created: 2026-08-18
description: "將Bazzite安裝在一般的x86電腦（單碟單系統），當作日常上網與玩遊戲的系統。 1. 為什麼要用Bazzite # 最近Bazzite這個Linux發行版蠻紅的，很多人都想在遊戲掌機和Windows電腦安裝，達到類似SteamOS的效果。"
tags:
  - "clippings"
status: "processed"
---
將Bazzite安裝在一般的x86電腦（單碟單系統），當作日常上網與玩遊戲的系統。

## 1\. 為什麼要用Bazzite

最近Bazzite這個Linux發行版蠻紅的，很多人都想在遊戲掌機和Windows電腦安裝，達到類似SteamOS的效果。

說道玩遊戲的系統，就會想到 [SteamOS](https://store.steampowered.com/steamos/) ，這是Valve開發的Linux發行版，基於Arch Linux所開發，專為玩遊戲設計。但是因為各種因素，直到撰文當下 ，Valve官方都沒有提供通用的ISO，只能在Valve官方發售的硬體，如Steam Deck和Steam Machine安裝。

於是就有許多希望仿造SteamOS體驗的Linux發行版出現。Bazzite就是其中一個。

其實Steam客戶端是可以在大部分的Linux系統安裝的，參見 [Linux版Steam安裝教學](https://ivonblog.com/posts/install-steam-on-linux/) ，例如Ubuntu、Fedora、Arch Linux都支援Steam，不過安裝過程需要手動設定Proton，還要裝GPU驅動，對想要直接玩遊戲的用戶來說可能會有點麻煩。

這個時候Bazzite就有其優勢了，它是基於Fedora Atomic開發，專為遊戲設計的Linux發行版。預先幫玩家裝好了你可能會用到的元件。包括GPU驅動、Steam本體、Proton轉譯層、類似SteamOS的全螢幕遊戲模式、Android模擬器Waydroid、滑鼠自訂鍵位程式、手把驅動等等。

Bazzite針對許多品牌的電腦與掌機進行優化，安裝好後就能用，達成開箱即用的體驗。

開機會自動進入Steam的全螢幕模式(Big Picture Mode)，讓你用手把操控。適合掌機與客廳電腦使用。

此外，針對一般桌上型電腦與筆電，Bazzite提供「桌面模式」，允許你安裝一般Fedora上跑的Linux程式，讓你可以將這個系統當成日常主力上網的系統。

---

從技術細節來說，Bazzite是 [Universal Blue](https://universal-blue.org/) 的一部分，繼承 [Fedora Atomic](https://www.fedoraproject.org/atomic-desktops/) 的特色，包括系統根目錄大部分分區是唯讀的，保持系統穩定。並且使用bootc技術管理系統更新，確保系統出問題能回滾。針對開發者，Bazzite也準備了一系列便捷指令稿，一鍵設定好開發環境。若真的需要使用DNF，也能用rpm-ostree安裝rpm套件到系統。

過去，仿造SteamOS的Linux發行版不是沒有，但就是開發危險性太高了。例如 [HoloISO](https://github.com/HoloISO/releases) ，便是逆向工程SteamOS的還原映像檔所製作出來的產物，可是這種開發模式缺乏穩定性，維護也很不容易。

至於Bazzite，則是透過Fedora Atomic的架構，加上一些開發者的巧思，去趨近SteamOS的使用體驗。

在Valve釋出可於任何硬體安裝的SteamOS之前，Bazzite是體驗Linux玩Steam遊戲的一個快捷上手選擇。

但是必須注意： **Bazzite並非Valve公司開發的專案，也不是downstream fork，跟SteamOS沒有太大關係。** Bazzite是由開源社群維護的系統。它的更新模式為半滾動更新模式，每個月都會不定時收到更新。沒有商業公司維護，穩定性無法保證。

## 2\. 測試環境

- 電腦型號：MSI Modern 15 A10
- CPU：Intel i5-10210U
- GPU: Intel UHD 620 (內顯)
- GPU: Nvidia MX350 (獨顯)
- RAM: 16GB
- 磁碟：NVME 500GB

## 3\. 安裝Bazzite到電腦

1. 因為Bazzite基於Fedora Linux開發，應該先確認電腦硬體是否支援Linux系統。在Linux玩遊戲之前，可以到 [ProtonDB](https://www.protondb.com/) 查詢個別遊戲對Linux的支援度。
2. 目前，要達到最好Bazzite使用體驗，建議使用AMD顯卡的電腦，Intel次之。Nvidia驅動問題很多，可能無法正常使用Steam全螢幕遊戲模式。
3. 到 [Bazzite官網](https://bazzite.gg/) 下載ISO。Bazzite提供多種客製化的ISO，依照你的具體情況做選擇。例如，我個人是要將這個系統當作日常使用，遊戲用途次之，故電腦型號選Other Laptop，GPU選Nvidia版本。然後選擇：開機進入桌面模式，不要自動啟動全螢幕遊戲模式。
4. 下載後， [檢驗ISO完整性](https://ivonblog.com/posts/windows-verify-file-checksum/)
5. 準備一支16GB以上容量的隨身碟，使用 [Ventoy](https://ivonblog.com/posts/ventoy-installation/) 製作開機碟
6. 進入電腦UEFI，關閉Secure Boot。雖然Bazzite會自動產生MOK Key，但我懶得開。
7. 使用隨身碟開機（註：目前Ventoy對Bazzite支援度有問題，開機的時候需要選GRUB Mode才能順利進入安裝系統的畫面）
8. 點桌面圖示安裝系統
9. 選取安裝位置，刪除整個硬碟，完成安裝
10. 輸入使用者名稱，只能英文字母加數字
11. 重開機，第一次可能會看到Enroll MOK Key的畫面，若沒有要用Secure Boot，就點選Continue Boot。

## 4\. 啟動Steam

1. Bazzite預先已經安裝好Steam客戶端，第一次啟動要等個幾分鐘初始化才會跳出登入界面。我選的Bazzite開機後會進入KDE Plasma的桌面模式。Steam遊戲模式沒有啟用。不過Steam客戶端會在開機後自動於背景啟動。
2. 點選桌面的Steam圖示玩遊戲。或者手動點Steam Big Picture Mode圖示進入Steam全螢幕模式。
3. Steam遊戲大部分是針對Windows開發的，在Linux需要安裝Proton相容層才能玩。關於如何啟用Proton相容層，參閱 [Linux版Steam客戶端使用方法](https://ivonblog.com/posts/install-steam-on-linux/)
4. 小提示：點選KDE Plasma桌面右下角的電源模式，將效能調到最大，讓Steam遊戲以最佳效能執行。
5. 如果要開機直接進入Steam全螢幕，而非桌面模式的話……得依照 [Bazzite官網](https://docs.bazzite.gg/Installing_and_Managing_Software/Updates_Rollbacks_and_Rebasing/rebase_guide/) 指令進行系統rebase程序。不要去改SDDM設定，因為atomic的設計使得變更會無效。這樣重開機就會變成直接進入Steam。桌面模式變成要在Steam選單選取才能切換過去。
6. 若你的遊戲不是從Steam下載的，可以用Bazzite內建的Lutris來管理散落的exe。參見 [Lutris使用教學](https://ivonblog.com/posts/lutris-game-manager/) 。

## 5\. 啟用Android模擬器

1. Bazzite採用的是 [Waydroid](https://ivonblog.com/posts/ubuntu-waydroid/) 方案。這是基於LXC打造的容器，並非虛擬機。支援滑鼠與觸控模式操作。
2. 第一次開啟，需要填入 `https://ota.waydro.id/system` 和 `https://ota.waydro.id/vendor` ，下載包含GApps的LinneageOS映像檔。
3. 之後再執行這個 [指令](https://ivonblog.com/posts/waydroid-tips/) 完成非官方Google ROM認證，並安裝x86轉譯器。

## 4\. Bazzite安裝軟體的方式

1. 既然Bazzite是基於Fedora開發的系統，那麼可以用 `dnf install` 指令安裝軟體嗎？儘量不要，因為Bazzite系統是atomic的，大部份系統分區是唯讀的。使用者不應該隨便變更。即使有sudo權限，也不能任意安裝軟體到系統。請參考 [Bazzite官方文件](https://docs.bazzite.gg/Installing_and_Managing_Software/software-intro/) 了解偏好的軟體安裝方式。
2. 推薦優先使用 [Flatpak](https://ivonblog.com/posts/linux-flatpak-introduction/) 安裝軟體。你會發現Bazzite系統除了Steam客戶端以外，Bazzite內建的軟體大部分都是用Flatpak安裝的。Bazzite用「Bazzar」取代了KDE Discover作為軟體商店前端，這裡的軟體都來自Flathub，以沙盒執行，確保安全性。
3. 次要的選擇是使用 [Distrobox](https://ivonblog.com/posts/distrobox-usage/) ，在Podman容器裡面跑Fedora，這裡可以安裝傳統rpm格式的套件。Bazzite內建一個DistroShelf圖形前端讓你管理跑各個發行版的容器。
4. 如果真的需要變更系統底層的話，再透過 [rpm-ostree](https://ivonblog.com/posts/rpm-ostree-usage) 指令安裝rpm套件。
5. 至於系統更新方式，使用Bazzite Portal這個程式更新就可以了。
6. Bazzite預設會每週自動更新一次系統。打開KDE Plasma系統設定可以設定自動更新的週期，或者關閉自動更新。

技術背景：Bazzite使用bootc管理系統映像檔更新，再用rpm-ostree管理使用者手動安裝的rpm套件。

## 5\. 開啟中文輸入法

1. Bazzite內建 [Fcitx5輸入法](https://ivonblog.com/posts/ubuntu-fcitx5/) 模組。開啟系統設定 → 鍵盤 → 虛擬鍵盤，選取Fcitx5。
2. 看一下系統右下角，若在安裝的時候選取中文，應該會幫你啟用新酷音注音輸入法。按下Ctrl + 空白鍵就可以切換打字了
3. 不過我發現有些程式不能輸入，可能要打開檔案管理器，手動編輯家目錄的 `~/.profile` 檔案，設定環境變數

```bash
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
```

4. Bazzite的Fcitx5輸入法是透過rpm-ostree安裝的，若有其他需要就用rpm-ostree安裝 [fcitx5的rpm套件](https://packages.fedoraproject.org/search?query=fcitx5) ，不要用Flatpak安裝免得混亂。

## 6\. Bazzite開發用的指令

一般用戶可能不需要使用這裡的功能。

Bazzite有終端機，使用者可以使用Linux的命令行工具。內建一系列 `ujust` 指令，用於快速部署開發環境，例如，啟用Virt Manager虛擬機。

詳情參閱 [Bazzite官方文件](https://docs.bazzite.gg/)

## 7\. 使用心得

第一印象是，內建軟體太多了，跟Fedora比起來，感覺好臃腫。不過真的是很考慮用戶需要什麼，預裝的都是合理的軟體，是遊戲玩家會需要的東西。幾乎不用打複雜的指令安裝軟體。當我試圖執行指令安裝Tailscale的時候，它貼心的提醒我：不用，我們已經內建了！Fcitx5中文輸入法自動啟用。精挑細選的Bazaar軟體商店響應速度比其他桌面的都要快速。內建Android模擬器Waydroid還有Distrobox。種種細節令人驚喜。

不過果然是因為Fedora滾動發行版的優良傳統，系統剛安裝馬上就有bug。KDE 6.5桌面，Steam開起來會無法顯示視窗。有時候要用指令啟動。並且我還遇到關上筆電蓋子，閒置幾分鐘，SFTP就斷掉的bug。Steam全螢幕遊戲會閃爍，色彩跑掉，遊戲lag。新酷音在Flatpak打字的時候會斷掉。以上問題更新到KDE 6.6之後就大致解決了。

唉，Bazzite更新不像SteamOS，沒有商業公司保證穩定度。要承擔的風險很大。