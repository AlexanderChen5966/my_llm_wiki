---
title: "[圖文] Steamdeck 安裝 Batocera 模擬器教學（單系統／雙系統通用）"
source: "https://hackmd.io/@xTaiwanPingLord/steamdeck_batocera"
author:
published:
created: 2026-08-18
description: "If you have a microSD already installed in your Steam Deck, eject it and insert the microSD you plan to use with Batocera.On the Steam Deck, enter Desktop Mode.Open the Fire Fox browser from the icon on the taskbar.Visit batocera.org/download and click the download link under the Steam Deck image (i.e. x86_64). The image is around 2.5GB and will take a few minutes to download.Open another tab within the Fire Fox browser and visit www.balena.io/etcher.Click the Download Etcher button.Click the Download link to the right of Etcher for Linux x64 (64-Bit) (AppImage).Once both Etcher and Batocera have downloaded, open the Dolphin File Manager and select the Downloads sub folder.Double-click on the balenaEtcher-1.xx installer. If prompted What do you wish to do with this file? click the Execute button.You may see a dialog that indicates it will start balenaEtcher and If you do not trust this program, click Cancel. Instead, click the > Continue button.The Etcher user interface will now appear. Click the Flash from file button.Select the batocera-x86_64-36-xxx.img.gz image file that we downloaded earlier and click the Open button.Click the Select targe button and click the checkbox to the left of your microSD card. *NOTE: Verify that the card selected matches the size of the microSD card installed in your Steam Deck.Once checked, click the Select 1 button, then click Flash!Depending on the size of your microSD card, you may see a warning stating: You are about to erase an unusually large drive. Are you sure the selected drive is not a storage drive? Verify that you’ve selected the correct microSD card. Then, click the Yes, I’m sure button.If you have previously assigned a super user password, you may be prompted to enter that password. If you have never set a super user password, you can ignore this statement.Batocera will now be written from the image file to the microSD card. This process may take a few minutes, just wait for it to complete.Once complete, you can click the close button on the balenaEtcher window.Shutdown the Steam Deck by clicking the Steam icon in the lower-left of the taskbar, click the Shut Down button and Shut Down again. The Steam Deck will now shut down.Proceed to the next section, Boot Batocera from the microSD card."
tags:
  - "clippings"
status: "processed"
---
## SteamOS 單系統

## 安裝 Batocera 到 SD 卡

1. 進入 SteamOS 的桌面模式。
2. 開啟 Firefox，在 Batocera 下載 Steamdeck 用的系統檔案（約2.6GB，需要一段時間）。
3. 下載 [balenaEtcher](https://github.com/balena-io/etcher/releases/download/v1.18.11/balenaEtcher-1.18.11-x64.AppImage) 。
4. 打開檔案管理器（Dolphin File Manager），進入 `Downloads` 資料夾。
5. 雙擊開啟 `balenaEtcher-1.xx.xx-x64.AppImage` ，如果詢問要做什麼，選擇執行 `Execute` 。
6. 有對話框提示即將開啟 `balenaEtcher` ，選擇繼續 `Continue` 。
7. 點擊 `Flash from file` ，選擇剛才下載的 `batocera-x86_64-36-xxx.img.gz` 映像檔。
8. 點擊 `Select target` 或者中間的 `Change` ，選擇您的 microSD 卡。 **務必確認選擇正確。**
9. 點擊 `Flash` 開始寫入。此動作會清除 microSD 卡中的所有資料。
10. 此時若要求輸入密碼，預設是 `admin` （如果提示錯誤，請看下方的重設密碼教學），輸入完按 `OK` 。
11. 耐心等待他寫入完。

## 進入 Batocera

1. 關閉 Steam Deck。
2. 按住 `音量鍵-` 以及 `開機鍵`
3. 選擇 `EFI SD/MMC Card (或你的SD卡名字)` 。

## 出現密碼錯誤／設定密碼

> [!warning] Warning
> 輸入指令時出現
> 
> - Sorry, try again.
> - sudo: 3 incorrect password attempts.

1. 開機之後，按住 `開機鍵` 點擊 `切換至桌面` 以進入 `桌面模式`
2. 點左下角，System裡面打開 `Konsole`
3. 在 Konsole 輸入 `passwd` 並按下 Enter 執行。
4. 若要求輸入目前的密碼，請重設密碼。若無，此時請輸入想設定的密碼， **輸入時不會顯示字元，需要盲打** ，按下 Enter 後再輸入一次（確認用），再按下 Enter。此後任何帶 `sudo` 的指令都要使用這個密碼，請勿忘記。

> [!success] Success
> ## 重設密碼
> 
> 請先嘗試用「變更帳戶」登出再重新登入。  
> 如果上面的方法失效，請在SteamOS的設定內按「重設此機器」。

## 雙系統（需要 Windows）

## 安裝 Batocera 到 SD 卡

1. 進入 Windows。
2. 開啟 Edge 或其他瀏覽器，在 Batocera 下載 Steamdeck 用的系統檔案（約2.6GB，需要一段時間）。
3. 下載 [balenaEtcher](https://github.com/balena-io/etcher/releases/download/v1.18.11/balenaEtcher-Portable-1.18.11.exe) 並開啟。
4. 點擊 `從檔案燒錄` ，選擇剛才下載的 `batocera-x86_64-36-xxx.img.gz` 映像檔。
5. 點擊 `選擇目標磁碟` 或者中間的 `更改` ，選擇您的 microSD 卡。 **務必確認選擇正確。**
6. 點擊 `現在燒錄！` 開始寫入。此動作會清除 microSD 卡中的所有資料。
7. 耐心等待他寫入完。

## 添加 Batocera 引導（rEFInd）

1. 進入 Windows。
2. 下載並開啟 [Diskgenius](https://download2.eassos.cn/DG5511508_x64.zip)
3. 找到您的開機硬碟，通常是分區最多的那個。
4. 打開 `esp` 分區的 `efi` 的 `refind` 資料夾，選擇 `refind.conf` 並右鍵（長按）選擇 `複製到「桌面」` 。
5. 用記事本打開桌面的 `refind.conf` ，找到最下面 `Batocera` 的部分，刪除後面的 `Disable` ， **完成後如下圖** ，其他地方不用動，儲存並關閉記事本。
6. 將 `refind.conf` 移動回去剛才 `Diskgenius` 開啟的 `\efi\refind` 資料夾，之後選擇取代原本的檔案。
7. 關機，重開機後應該就會出現 Batocera 的啟動項目。

## Batocera 相關教學

## 改成中文

1. 按 `Start/Menu` 鍵。
2. 選擇 `Settings` -> `Language` 。
3. 往下找到 `正體中文`

## 關機

1. 按 `Start/Menu` 鍵。
2. 選擇 `Quit` -> `Shutdown` 。

## 快捷鍵

1. 在遊戲中打開快捷選單 `SELECT` + `A`
2. 變更目前狀態 `SELECT` + `DPAD UP/DOWN`
3. 保存目前狀態 `SELECT` + `X`
4. 載入目前狀態 `SELECT` + `Y`
5. 變更 Shader `SELECT` + `R2` / `L2`
6. 螢幕截圖 `SELECT` + `L1`
7. Insert Arcade Credit – In an Arcade game, press `SELECT` to insert a credit.
8. Start Arcade Game – In an Arcade game, press `START` to start a 1 player game.