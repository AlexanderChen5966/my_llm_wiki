---
title: "Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！"
source: "https://www.techbang.com/posts/127891-old-pc-budget-steam"
author:
  - "[[小治]]"
published: 2026-06-07
created: 2026-08-18
description: "Valve Steam Machine 價格高昂？本文揭示如何利用 Bazzite，將舊電腦改造為低成本的 Steam Machine。透過 Bazzite，輕鬆實現客廳遊戲的理想體驗！"
tags:
  - "clippings"
status: "processed"
---
去年 11 月，Valve 正式公開了全新的 Steam Machine 遊戲機，透過自家 SteamOS 作業系統主打「開機即玩」的客廳遊戲體驗，在 Steam Deck 熱銷、Proton 相容層技術趨於成熟，以及 Linux 遊戲生態大幅進化的基礎之上，Valve 的夢想藍圖，終於具備落地的現實條件。

不過，Valve 也坦言，其定價勢必不會低於玩家自行組裝一台相近規格的電腦。於是問題亦隨之浮現：我們是否有可能在不投入高額預算的情況下，實現 Steam Machine 所描繪的客廳遊戲願景？其實近年來，坊間已出現多套以 SteamOS 為核心精神的開源解決方案，可讓退役的桌機或老筆電重新煥發價值。本篇企劃將聚焦 Bazzite 這套基於 Linux 的作業系統，一步步示範如何用最低成本，把舊電腦改造成一台「窮人版 Steam Machine」。

### 文章目錄

## 在 PC 安裝 SteamOS 的挑戰與限制

SteamOS 本質上是一套基於 Linux 的作業系統，與 Windows 並不相容，為此 Valve 開發出一套相容轉譯工具 Proton，如今已有將近 90% 的 Windows 遊戲已經可以在 Linux 上運行無礙，但問題在於，現階段官方並未針對一般 PC 提供 SteamOS 安裝程式，官方支援仍以自家 Steam Deck 及授權掌機為主，也因此，若想把桌機或筆電變成 Steam Machine，只能仰賴開發者社群提供的資源。

### 追求 SteamOS-like 體驗

目前常見的「SteamOS-like」方案有幾種：如 HoloISO 是直接把 Steam Deck 裡的那套 SteamOS 移植到 PC，但是其維護節奏與版本相依性較敏感，遇到底層元件及驅動更新時，就需要手動排錯，門檻相對較高；開源專案 SteamFork 則是一套類似 SteamOS 的替代系統，只是目前已不再進行更新。這些方案的目的都在努力「還原官方體驗」，但也因此，它們對於硬體的相容性更為挑剔。大致來說 AMD 的處理器及 GPU 相容性最佳，畢竟 AMD 長期擁抱開源路線，且 Steam Deck 本身就是採用 AMD 平台；而 NVIDIA 則多依賴自家封閉的專有驅動，驅動更新節奏與 Linux 生態相容性的磨合成本更高，非常容易出現不穩定或需要手動調整的情況。

![Steam 去年一連發布三款重要遊戲硬體，試圖以 SteamOS 為核心，進一步擴大 Linux 在遊戲市場中的實際影響力。](https://cdn0.techbang.com/system/images/771253/original/1eb63afbb7b545a86ce281836093f10a.jpg?1770634664)

### 最簡單解決方案：Bazzite

基於上述種種問題，在本篇我們選擇 Bazzite 這套以 Fedora Atomic 為基礎打造的 Linux 系統，目標不是要完整複製官方 SteamOS，而是盡量整合出「適合 PC 使用的客廳 Steam 遊戲體驗」。相較其他 SteamOS 系方案，Bazzite 最大的特色在於安裝流程清楚、說明文件完整、更新節奏穩定，官方更提供了明確的硬體相容性說明與常見問題指引，讓使用者不必反覆試錯。在硬體支援上，Bazzite 明確以 AMD 平台為優先，尤其以 AMD GPU 較能獲得穩定的體驗；Intel Arc 有部分限制；NVIDIA 仍在測試中，雖可安裝，但較容易遇到顯示或驅動問題。Bazzite 透過清楚揭露相容性限制的方式，讓使用者更容易評估自己的硬體是否適合，即使是新手玩家也能用舊 PC 低成本打造客廳 Steam 遊戲機。

![Bazzite 是一個建立在 Universal Blue 開源基礎上的專案，由核心團隊負責維護，但也同時開放社群共同參與開發與改進。](https://cdn0.techbang.com/system/images/771254/original/e20958def6d8ac75af9deaacb0e288a9.jpg?1770634665)

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771255/original/4a2b0afa3422f845144aaef691f05b4c.jpg?1770634665)

## 從官網下載 Bazzite 映像檔

如果你曾經自行安裝過 Windows 作業系統，其實可以先放一半的心，因為 Bazzite 的系統安裝流程與 Windows 相當接近。第一步同樣是先下載官方映像檔，以便用來製作開機碟。比較需要留意的地方，在於 Bazzite 的官方下載頁面提供了多種版本選項，不同映像檔分別對應桌機、掌機以及特定 GPU 硬體配置，以本篇在桌上型電腦上安裝為例，就要選擇「Desktop」版本，在下載前，也可先參考官方說明文件或社群討論，了解各版本的特性與更新節奏。

> Bazzite 下載網址： [https://bazzite.gg/](https://bazzite.gg/)

### 步驟 1：進到 Bazzite 官網， 點擊右上角的「Download bazzite」按鈕。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771256/original/af9f8a9b4fd97600b5c80250a8bf9f2f.jpg?1770634777)

### 步驟 2：在左側的下拉式選單選擇要安裝的硬體，這裡選「Desktop」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771257/original/329dfddcc78876f4f023db45a85f1bd9.jpg?1770634777)

### 步驟 3：選擇電腦所使用的 GPU 類型之後，在桌面環境的項目選擇「KDE」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771258/original/e896140ddb89b877b7787a3099429f80.jpg?1770634778)

### 步驟 4：接著選擇「Yes 」確定要使用 Steam Gaming Mode。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771259/original/d7176b05bb7994a245ebc184ec561f56.jpg?1770634778)

### 步驟 5：最後按下第一個「Download bazzite-desk」按鈕下載映像檔。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771260/original/c281ef37ce52d84782d814466a157044.jpg?1770634778)

## 製作 Bazzite 系統安裝隨身碟

針對 Bazzite 的系統安裝碟的製作流程， 也跟 Windows 大同小異，同樣可使用 Rufus、balenaEtcher 等常見的第三方工具來完成。這裡則以後者為例，基本上只需選擇下載好的映像檔、指定欲寫入的 USB 裝置並開始執行，就能自動完成可開機安裝碟的製作。不過仍建議在操作前先備份 USB 內的重要資料，因為寫入過程會清除所有內容。完成後，這支 USB 隨身碟便可作為 Bazzite 的系統安裝媒體，銜接後續的安裝步驟。

> balenaEtcher 下載網址： [https://etcher.balena.io/](https://etcher.balena.io/)

### 步驟 1：進入 balenaEtcher 官網，點擊中央的「Download Etcher」按鈕。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771261/original/ed5027b96a7f9ecbf8076aa12d3cf389.jpg?1770634864)

### 步驟 2：此處以 Windows 版本為例，點擊下載並在電腦上安裝 balenaEtcher 應用程式。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771262/original/36b541cf0cd4f20dc057249d8bf15b48.jpg?1770634865)

### 步驟 3：執行 balenaEtcher，按「從檔案燒錄」並選擇先前下載的 Bazzite 映像檔（ISO 格式）。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771263/original/6c0577a08c422e6e03331fc6e337c7e4.jpg?1770634865)

### 步驟 4：選擇要製作的目標隨身碟（容量至少 10GB）之後，按下「現在燒錄！」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771264/original/745581a448cd0825ba40d7c9a92c06c9.jpg?1770634865)

### 步驟 5：等待燒錄成功，即可將隨身碟移除，插上準備安裝 Bazzite 的電腦。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771265/original/2ca3caeecbfa56df86c932a274569fb5.jpg?1770634866)

## 安裝 Bazzite 作業系統至電腦

在開始安裝 Bazzite 作業系統之前，有幾個關鍵設定重點。Bazzite 的安裝環境需要電腦以 UEFI 模式開機，因此需確認主機板支援 UEFI 模式；同時，為避免 Secure Boot 造成啟動或驗證問題，建議在 BIOS 中先關閉安全開機功能，能讓後續安裝更加順利。另外補充說明，此處流程皆以實機安裝為基準，部分畫面因清晰度與步驟呈現考量，是在虛擬環境下擷取，但實際操作步驟與實體電腦並無任何差異。

### 步驟 1：在安裝前， 先到 BIOS 將開機順序調整為 USB 隨身碟優先， 此例為微星主機板 BIOS。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771266/original/6f663786b68786b145f278b064a2c642.jpg?1770634998)

### 步驟 2：BIOS 的安全開機（Secure Boot）也建議將其關閉。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771267/original/a295b34843b687f4cc073ac685a09936.jpg?1770634999)

### 步驟 3：BIOS 設定儲存後，重開機後進入安裝流程，選擇第一項「Install⋯」按 Enter。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771268/original/f7c90a6f6c0b02c30aa3dded34b6dbef.jpg?1770634999)

### 步驟 4：選擇安裝語言，可支援繁體中文，按「繼續」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771269/original/c41c8aa11076a770bf12d9fc944d7d51.jpg?1770634999)

### 步驟 5：然後點選中央的「安裝目錄位置」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771270/original/ec8defc7826a10694b3912f4a2ed81f2.jpg?1770635000)

### 步驟 6：選擇要安裝系統的磁碟，並勾選「通過刪除或縮小現有分區來釋放空間」後，按「完成」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771271/original/317545d2e85fe0f074a5b8ba7c5aba17.jpg?1770635066)

### 步驟 7：這裡將整個磁碟完全用於安裝 Bazzite，故選擇「全部刪除」後再按「取回空間」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771272/original/70557cd10e87d13ce6a5f9a7363a82fe.jpg?1770635066)

### 步驟 8：回到安裝摘要頁面，接著就可以按右下角的「開始安裝」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771273/original/9a2bf13e21598daef646e3c0ab2964bb.jpg?1770635067)

### 步驟 9：等待一段時間，當顯示安裝已完成，即可按下「重新啟動系統」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771274/original/f24e52c142725119b59b3fad7f0b5a11.jpg?1770635067)

### 步驟 10：重開機若出現 MOK 管理選單，可以直接選擇「Continue Boot」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771275/original/805c4c0c37de206d000bc1cbb5e7f16f.jpg?1770635068)

### 步驟 11：準備進入 Bazzite 系統，如果安裝正確，將會直接開啟 SteamOS 遊戲模式。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771276/original/a604dfc8b4114b159dd755f245a1050d.jpg?1770635068)

## Bazzite 的 SteamOS 模式初始設定

在完成安裝並啟動後，每次開機 Bazzite 都會直接進入 SteamOS 模式的介面，其沿用 Steam Deck 的操作邏輯，因此與一般 PC 上 Steam 內建的 Big Picture 模式仍存在些許差異。在初始設定階段，建議先完成無線手把的配對，以及鍵盤輸入法相關設定，讓後續的遊玩操作更加便利。此外，Bazzite 也保留切換至原生桌面介面的彈，使用者可在該模式下進行上網瀏覽、帳號登入或系統設定等操作，再視需求返回 SteamOS 模式。

### 步驟 1：接續上頁，系統安裝完成後的首次開機，會進入歡迎頁面，可選擇「繁體中文」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771277/original/9d106370b95357097fde7619ae03cefc.jpg?1770635179)

### 步驟 2：選定時區以及設定完無線網路之後，按下「繼續」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771278/original/d25c73385e3febc56393740b867b69b7.jpg?1770635180)

### 步驟 3：接著就來到熟悉的 Steam 登入畫面，可以透過 QR Code 或帳號密碼登入。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771279/original/045763fcdf6da42ada99b92b1fbe1c54.jpg?1770635180)

### 步驟 4：登入後會進行快速導覽，由此可知其是以 Steam Deck 的操作邏輯為基礎。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771280/original/e5e93d6997261779d11431f163f88945.jpg?1770635180)

### 步驟 5：進入 SteamOS 模式的主畫面，先點選左下角的「Steam 選單」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771281/original/db78910a15ec03730c1f2eae051f3b0b.jpg?1770635181)

### 步驟 6：選擇「設定」。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771282/original/765af2c9cee3e37c791e6487e54ea5e6.jpg?1770635257)

### 步驟 7：在「藍牙」設定中配對無線控制器，不論 Xbox 或 PS5 的無線控制器皆能支援。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771283/original/f319519c4463253052f40f335222914a.jpg?1770635257)

### 步驟 8：在鍵盤項目的「使用中的鍵盤」中按下「編輯」，即可加入注音輸入法。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771284/original/810a95b6b3ce86a1ab299446aef28ec0.jpg?1770635258)

### 步驟 9：遊戲表現與電腦相關，本篇硬體採用 AMD GPU，在遊戲執行上沒有什麼問題。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771285/original/68629521f8a30535f874e967cf8172f6.jpg?1770635258)

### 步驟 10：在步驟 6 若選擇「電源」再按下「切換至桌面」，便可切換 Bazzite 的原生桌面。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn0.techbang.com/system/images/771286/original/cbd155249150810ecd3907635e91021f.jpg?1770635258)

### 步驟 11：內建 Firefox 能滿足上網查攻略需求，要返回 SteamOS 則按下桌面的「Return to⋯」捷徑即可。

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn1.techbang.com/system/images/771287/original/65a412d0fb85581e84bb6a101ed6743c.jpg?1770635258)

**本文同步刊載於 PC home 雜誌**

![Steam Machine 太貴買不起？免驚！安裝 Bazzite 系統，改造舊 PC 變身窮人版 Steam 桌機！](https://cdn2.techbang.com/system/images/771288/original/c54efb36021f004f3b855e44da04ea8c.jpg?1770635259)

歡迎加入 [PC home 雜誌粉絲團](https://www.facebook.com/pchomefans) ！

- **延伸閱讀： [Valve 突襲發佈 Steam Machine 家用遊戲主機、VR 頭戴裝置等三大硬體！15 個產品亮點一次掌握](https://www.techbang.com/posts/126519-valve-new-steam-hardware-highlights)**
- **延伸閱讀： [當年Steam Machine 嘗試打入遊戲主機市場卻失敗，現在 SteamOS 的新世代遊戲掌機會重演歷史嗎？](https://www.techbang.com/posts/121713-valves-big-gamble-driving-steamos-popularity-with-battletime-3)**
- **延伸閱讀： [PS4破解後的更多應用：除安裝Linux外，還能變身Steam Machine](https://www.techbang.com/posts/40608-ps4-cracked-you-can-install-linux-but-also-turned-into-a-steam-machine)**