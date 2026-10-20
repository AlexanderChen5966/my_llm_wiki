---
title: "【安裝教學】 SteamOS / Windows 雙系統 + 遊戲共用磁區"
source: "https://hackmd.io/@xTaiwanPingLord/steamos_dualboot"
author:
published:
created: 2026-08-18
description: "SteamDeck"
tags:
  - "clippings"
status: "processed"
---
## 事前準備

## 硬體工具

- SteamDeck
- 能插隨身碟的 Windows 電腦
- 隨身碟（8GB 以上，速度越快越好）
- `USB Type-C 公` 轉 `USB Type-A 母` 或者 擴展塢
- 鍵盤
- 足夠的耐心

## 需要下載的項目

- 系統
- 軟體工具
	- [Windows 必備驅動程式](https://help.steampowered.com/zh-tw/faqs/view/6121-ECCD-D643-BAA8)
		- APU 驅動程式
				- 無線網路驅動程式（僅 Win10 需要）
				- 藍牙驅動程式
				- SD 讀卡槽驅動程式
				- **兩個** 音訊驅動程式
		- [Rufus](https://github.com/pbatard/rufus/releases/download/v4.1/rufus-4.1p.exe)
		- [Ventoy](https://github.com/ventoy/Ventoy/releases/download/v1.0.93/ventoy-1.0.93-windows.zip)
		- [USBOX PE](https://bit.ly/3MTaw8x)
		- [Explorer++](https://github.com/derceg/explorerplusplus/releases/download/version-1.4.0-beta-2/explorerpp_x64.zip)
		- [rEFInd **整個資料夾**](https://drive.google.com/file/d/1v400caKKdHLS8sgr4uFEQLiek1Aje4zg/view?usp=sharing)
- 可選
	- [MAS](https://github.com/massgravel/Microsoft-Activation-Scripts/archive/refs/tags/1.8.zip)

## 其他

### 請務必備份隨身碟內的檔案，安裝過程中會格式化隨身碟

## SteamOS 安裝

1. 電腦打開 `rufus-4.1p.exe` ，選擇隨身碟（紅框）並選擇 `steamdeck-recovery-4.img` （藍框）。其他勿動，按開始，等他跑完。
2. 隨身碟插入SteamDeck，按住 `音量鍵-` 以及 `開機鍵`
3. 選擇 `EFI USB Device (隨身碟名稱)`
4. 開機完成後，按 `Reimage SteamOS` ，會有一個視窗，點確定。跑完會跳出另一個視窗， **勿點，直接從左下角關機，請勿再次開機！**

## Windows 安裝

1. 電腦打開 `Ventoy2Disk.exe` ，選擇隨身碟（紅框），點 Install 安裝。
2. 完成後電腦會出現一個大的磁碟區，在磁碟區內放入：
	- Windows 11 ISO
		- USBOX PE
		- Explorer++
		- refind 資料夾
		- Windows 必備驅動程式
		- MAS\_AIO.cmd(可選)
3. 隨身碟插入 SteamDeck，按住 `音量鍵-` 以及 `開機鍵` 。 如果意外進到 SteamOS，請直接立刻強制關機
4. 選擇 `EFI USB Device (隨身碟名稱)` ， **請勿選擇 SteamOS**
5. 選擇 `USBOX_V7[ISO版].iso` ，選擇 `Boot in normal mode`
6. 選擇第一個選項，並耐心等待PE載入，直到桌面出現 `DiskGenius` 軟體
7. 打開 `DiskGenius` 。看到上面有一條藍色的，點最長的那塊(Home)。選擇上方 `磁碟區` -> `調整磁碟區容量`
8. 調整後容量輸入 `64GB` ，其他勿動，點開始等他跑完
9. 點灰色的那條(可用)，點新增磁區，容量 `192GB` ，磁碟標籤輸入 `Windows` ，點確定
10. 點灰色的那條(可用)，點新增磁區，容量不動(就是最大) ，磁碟標籤輸入 `Game` ，點確定
11. 點左上角 `套用變更` ，等他跑完就可以關閉 `DiskGenius`
12. 點桌面的 `Windows安裝器`
13. 第一個點選擇，找到隨身碟中的 `Win11_22H2_Chinese_Traditional_x64v2.iso`
14. 第三個點小箭頭，選擇 `本機磁碟` 下的 `Windows` ，容量應該會是 `192GB`
15. 版本選專業版，優化調整選擇 `禁用休眠` / `顯示隱藏檔和資料夾` / `顯示已知類型副檔名`
16. 關閉優化調整視窗後，點開始安裝
17. 跑完之後，會有視窗提示重新啟動，請點重啟

## Windows 設定

1. 開始設定Windows（如果不想要登入 Microsoft 帳號，請在連接Wifi前按 `Shit+F10` ，命令提示字元輸入 `oobe\bypassnro` ，點Enter會自動重新啟動，重新啟動後設定過程 **勿連接 Wi-Fi** ，就不用登入帳號了。）
2. 設定完成進到桌面之後，先長按桌面，顯示設定把它改成橫向
3. 長按工作列，把搜尋等等不用的東西關掉，然後讓他靠左
4. 連接 Wi-Fi
5. 如果沒有正版的金鑰，打開 `MAS_AIO.cmd` ，按數字鍵1啟用
6. 安裝驅動程式：
	- INF 類， **在.inf檔案點右鍵，安裝**: `cs35l41.inf` / `NAU88L21.inf` /藍芽驅動的x64資料夾內有 `Rtkfilter.inf` 。安裝完成會有視窗說: 操作順利完成
		- 讀卡機、顯卡驅動: 點 `setup.exe` ，安裝完先不用重新啟動(I will restart the computer later.)
7. 打開設定，Windows Update點暫停更新一周，再點重新開始更新，耐心等他更新完
8. 重新啟動，檢查是否還有更新，或其他要安裝的。
9. 安裝Steam，登入之後，左上角 `Steam` -> `設定` -> `儲存空間` ，把磁碟區 `Game` 加進去。
10. 關機
11. 隨身碟插入 SteamDeck，按住 `音量鍵-` 以及 `開機鍵`
12. 選擇 `EFI USB Device (隨身碟名稱)`
13. 選擇 `USBOX_V7[ISO版].iso` ，選擇 `Boot in normal mode`
14. 選擇第一個選項，並耐心等待PE載入，直到桌面出現 `DiskGenius` 軟體，打開它
15. 點擇左邊的 `esp(0)` ，上方 `磁碟區` -> `指派新的磁碟機代號` ，點確定
16. 打開隨身碟中的 `Explorer++.exe`
17. 打開 `esp/efi` ，應該會看到 `Microsoft` 資料夾，把它重新命名為 `Deck`
18. 把隨身碟中的 `refind` 資料夾複製到 `esp/efi` ，此時 `esp/efi` 中應該會有四個資料夾: `boot` / `Deck` / `refind` / `steamos` 。請檢查 `refind` 資料夾打開有 `5` 個資料夾 `4` 個檔案
19. 長按關機鍵，關機

## SteamOS 設定

1. 開始設定 SteamOS。如果在更新那步驟卡比較久，可以按右下角 `B` 返回上一步，再點一次 Wi-Fi 重新進到更新階段。
2. 更新完成，進到 SteamOS 時，先去設定點檢查更新並讓它更新完。
3. 在 SteamOS 內，按住 `開機鍵` 點擊 `切換至桌面` 以進入 `桌面模式`
4. 點左下角，System 裡面打開 `Konsole` ；打開瀏覽器(FireFox)，第一次打開應該會自動打開應用程式商店(需要跑一段時間)，按右上角 `Install` 安裝
5. 同時按下 `STEAM` + `X` 可開啟虛擬鍵盤，瀏覽器中網址輸入 [reurl.cc/YeEd34](http://reurl.cc/YeEd34) 應該會來到這篇文章 **（網址分大小寫）**
6. 在 Konsole 輸入 `passwd` ，此步驟為設定密碼，建議設定成 `admin` 。請務必記住密碼，輸入過程不會顯示(盲打)，並且需要重複輸入一次。
7. 在 Konsole 貼上以下指令，並按下 Enter 執行（瀏覽器中用觸控板選取起來，L2可以作為右鍵選擇 `copy` 複製，Konsole的右上角點擊 `Paste` 貼上）

```shell
git clone https://github.com/xTaiwanPingLord/SD_fix
```

8. 在 Konsole 貼上以下指令，並按下 Enter 執行  
	**可能會要求輸入密碼: `admin` ，輸入時不會顯示字元，需要盲打**

```shell
sudo bash SD_fix/fix.sh
```

## 設定共用分區

1. 在 Konsole 貼上以下指令並執行  
	**可能會要求輸入密碼: `admin` ，輸入時不會顯示字元，需要盲打**

```shell
sudo steamos-readonly disable
```

2. 在 Konsole 貼上以下指令並執行

```shell
kwrite /usr/share/polkit-1/actions/org.freedesktop.UDisks2.policy
```

3. 修改第 `173` 行

```xml
<allow_active>auth_admin_keep</allow_active>
```

改為

```xml
<allow_active>yes</allow_active>
```

4. ctrl+s 儲存，輸入密碼按確定。
5. 關閉 Konsole 並重新開啟，在 Konsole 貼上以下指令並執行

```shell
kwrite /etc/fstab
```

6. 在最後一行貼上以下文字，ctrl+s 儲存，輸入密碼按確定。

```
LABEL=Game  /run/media/deck/Game  ntfs   defaults,nofail   0 0
```

7. Konsole 貼上以下指令並執行

```shell
sudo steamos-readonly enable
```

8. 打開桌面的模式的 Steam，左上角 `Steam` -> `設定` -> `儲存空間` ，應該會自動讀取到 `STEAMLIBRARY` 資料夾，選擇完之後應該名稱會是 `/run/media/deck/Game`
9. 同一個頁面中有三個點的按鈕，按它，把 SSD 設定成預設下載位置。
10. 重新開機，看看雙系統選單是否出現了，進 SteamOS 下載遊戲，看看是否能選共用磁區。

### 至此，恭喜你已經裝完 SteamOS / Windows 雙系統 + 遊戲共用磁區

## 可選項目

## SteamOS 桌面模式 改成中文

請看（要跑很久很久，請耐心等待）  
[https://forum.gamer.com.tw/C.php?bsn=60599&snA=39235](https://forum.gamer.com.tw/C.php?bsn=60599&snA=39235)