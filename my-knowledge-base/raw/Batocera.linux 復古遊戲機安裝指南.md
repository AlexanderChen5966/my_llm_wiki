---
title: "Batocera.linux 復古遊戲機安裝指南"
source: "https://zh-tw.manuals.plus/batocera/batocera-linux-retro-gaming-machine-manual#install-batocera-from-batocera"
author:
published: 2025-10-13
created: 2026-08-18
description: "探索 Batocera.linux 復古遊戲機的全面使用手冊，其中包含 ETA Prime 提供的詳細規格、安裝步驟、常見問題和影片指南。了解最低儲存要求、支援平台以及故障排除技巧，以獲得流暢的設定體驗。"
tags:
  - "clippings"
status: "processed"
---
![Batocera.linux Retro Gaming Machine Installation Guide](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-FEATURED-IMAGE.png)

Batocera.linux Retro Gaming Machine Installation Guide

#### Batocera.linux復古遊戲機

![Batoceralinux-復古遊戲機-產品](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-PRODUCT-1.png) **規格：**

- 版本：Batocera
- 最低儲存需求：8 GB
- 建議儲存空間：16 GB以上
- 支援的平台：x86\_64、Raspberry Pi 和其他特定設備

**對於那些喜歡影片指南的人來說，這裡有一些由 ETA Prime 製作的優秀影片：**

- 如果將Batocera安裝到x86\_64電腦上，就可以將一台舊的PC變成一台模擬夢想機器！ （18:27的獨立模擬器的控制器配置已過時，Batocera現在為您處理控制器配置。若要自行修改每個模擬器的控件，請查看此頁面）
- 如果將 Batocera 安裝到單板電腦上，請參閱 Raspberry Pi 4 上安裝 Batocera 完整設定指南 - 復古遊戲的美感！

您可以在支援的設備頁面查看 Batocera 支援在哪些設備上運行！

**大部分平台的通用說明如下：**

1. 為您的作業系統下載 Balena Etcher（也可以使用 Raspberry Pi Imager，但本漸進式指南僅介紹 Balena Etcher）。
2. 從 Batocera 的下載部分下載圖像檔案 web 與您想要用於 Batocera 的架構相似的網站（大多數 PC 為 x86\_64，其他裝置通常有自己的特定圖像）。  
	某些瀏覽器（例如 Chrome/Opera/Safari）可能會初步自解壓縮文件，並進行它的重命名。如果檔案超過 3 GB，則表示已解壓縮。如果下載失敗，請使用其他瀏覽器。沒錯，Chrome 很爛，事實是，請使用其他瀏覽器。
3. 安裝並執行 Etcher（或 Raspberry Pi Imager，如果使用的話）。
4. 將所需的 USB 隨身碟、SD 卡或硬盤插入電腦。 v30 及更高版本至少需要 8 GB，建議使用 16 GB 以上記憶體才能達到完整功能（僅使用 8 GB 無法自動下載更新）。
5. 在 Etcher 中，選擇剛下載的鏡像 batocera-(version)-(arch)-(date).img.gz，然後選擇目標磁碟機（如下圖所示），然後點擊「Flash」！你甚至不需要解壓縮文件，Etcher 會幫忙搞定。 ![Batoceralinux-復古遊戲機-（2）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-2.png)
	- 如果快閃記憶體完全失敗，請嘗試關閉任何不需要的軟體，例如防毒程式或虛擬機器。
6. Etcher可能會要求管理員權限（以解壓縮檔案）。點選“是”。完成後，請檢查鏡像是否已燒錄成功並驗證。  
	![Batoceralinux-復古遊戲機-（3）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-3.png) 如果驗證失敗，請嘗試解壓縮檔案並重試。
7. 完成後，重新插入您的USB驅動器/SD卡/硬盤驅動器，並檢查其中是否包含啟動資料夾（檔案因架構而異）。別擔心，它只有幾GB大小，這只是Batocera啟動的分割區！ ![Batoceralinux-復古遊戲機-（4）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-4.png)
8. 關閉電腦放入USB驅動器/SD卡/硬盤機插入您想要使用Batocera啟動的機器並打開它。
9. 確保您的裝置預設啟動至USB隨身碟/SD卡/硬盤。
	1. 如果系統中只插入一個USB隨身碟/SD卡，大多數單板電腦都會自動啟動到對應的USB隨身碟/SD卡。
		2. 如果最近的 PC 使用 x86/x86\_64 版本，則可能需要在 BIOS 設定中失敗安全啟動。
		3. 如果是在 PC 上，您可能需要按 \[F10\]、\[F11\] 或 \[F12\] 進入啟動選擇選單，然後選擇 Batocera 驅動器。如果可用，UEFI 是首選，但不是必要的。 ![Batoceralinux-復古遊戲機-（5）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-5.png)
10. 啟動Batocera硬盤並讓其自動擴展使用者資料分割區（可能需要幾個或數十個，取決於硬盤的大小/速度），完成後將重新啟動。此步驟中請勿關閉設備，否則可能會造成資料損壞！ ![Batoceralinux-復古遊戲機-（6）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-6.png)
11. 享受！ ![Batoceralinux-復古遊戲機-（7）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-7.png) ![Batoceralinux-復古遊戲機-（8）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-8.png)

請務必了解如何增加某些操作所需的遊戲 ROM/BIOS 檔案。 Batocera 的介面由 EmulationStation（有時稱為 ES）的修改版本所驅動。查看其視圖頁面來學習如何導航它！  
如果未偵測到您的 Nvidia GPU，您可能需要手動啟動官方驅動程式以提高其功能，但如果該驅動程式無法用於您的機器，Batocera 可以使用預設的 nouvaeu 驅動程式正常運作。

**刷機失敗！**  
別慌。 Etcher 通常會在錯誤訊息中解釋錯誤的原因，所以一定要完整閱讀。通常的懷疑是驗證失敗，這意味著讀回 Etcher 的資料與鏡像中的資料不相符；是時候硬換碟了。  
如果 Etcher 沒有提示錯誤了，很可能是 Windows 中存在任何一個老 bug，導致無法整理 USB 磁碟機上的多個分割區。這個問題可以輕鬆修復，只需將整個磁碟機重新格式化為 NTFS格式，然後再試一次即可。如果Windows自帶的工具無法執行此操作，可以使用下面的命令提示字元強制布雷磁碟機。沒錯，這聽起來很危險。

#### BIOS 設定（如果 Batocera 無法啟動）

**在進行故障排除之前，請先檢查 Batocera 是否已啟動！若出現以下情況，則 Batocera 已啟動：**

- Batocera 飛濺影像會出現在任何連接的顯示器上。
- SSH伺服器處於活動狀態並且可以登入。

Batocera 應該可以使用 UEFI 正常啟動，如果出現該選項，則應選擇選項該。  
但是，有時 Legacy（有時也稱為 MBR）是最後一個選項，具體依靠主機板。由於主機板製造商之間不一致，因此無法提供逐步說明。您可以參考威斯康辛大學資訊科的知識庫頁面，以了解特定製造商主機板在啟動過程中如何在不同主機上進入 BIOS設定。典型的按鍵包括 xcNOz、xcNNz、xcNMz、xael\] 或 \[End\]。

進入啟動選擇畫面的常用按鍵是 \[F12\]、\[F11\]、\[F10\]、\[F9\] 或 \[Del\]。某些鍵盤可能還需要按住 \[Fn\] 鍵。主機板隨附的手冊可以提供更多線索，或者您可以嘗試自行探索 BIOS 的所有子選項。若要從 Windows 存取 BIOS，請按住 \[Shift\] 鍵並選擇“重新啟動”，然後前往“疑難排解”>“進階選項：UEFI或者，前往“設定”>“變更電腦設定”>“更新與復原”>「排擷取”>“進階啟動：立即重新啟動”，然後在重新變更設定後前往「「「「」」」」>「「管理」」」>「進階啟動」時  
。

- 安全啟動，又稱為安全金鑰匙、清除金鑰匙、安全金鑰匙產生、更新、含 CSM 的 UEFI 等。通常歸入「安全性」類別，但也可能出現在「啟動」或「身份驗證」類別中。您可能需要終止安全啟動/清除金鑰匙、重新產生金鑰匙和/或啟用相容性支援模組 (CSM)，以允許其他作業系統（例如 Batocera）啟動。
- 傳統啟動允許傳統ROM啟動、EFI、MBR、載入傳統選項、顯示不安全目標等。通常位於「啟動設定」類別中，但有時也有單獨的部分。某些解決主機板會強制所有UEFI啟動都使用原始作業系統金鑰匙的安全啟動，從而阻止其他作業系統（如Batocera）的啟動。如果您的主機板這種情況，一個典型的方法是使用/強制使用傳統啟動。如果這兩個選項都無效，請連接主機板廠商尋求支援。
- 可移動媒體啟動，稱為USB啟動。通常位於「裝置」、「儲存」或「啟動」類別。某些主機板可能會主動阻止從可移動媒體（例如USB）啟動。此功能通常出現在辦公室電腦上。
- 啟動順序又稱為啟動優先權、ROM順序、啟動選項順序、UEFI 硬盤 BBS 優先權等。通常位於「啟動」類別中，但有時也包含單獨的部分。某些主機板可能會將 UEFI 啟動選項中的一個項目視為一個項目，因此您需要在 BIOS 設定中特定的「UEFI BBS 優先權」選單，將順序切換為 Batocera 硬盤。在其他情況下，這不是嚴格要求，但可以將 Batocera硬盤設定為插入時的「預設」啟動選項；此時，您每次都手動選擇 Batocera 硬盤進行啟動。

如果你想見見現實生活中的前放大器樂，看看這個外部連結：https: [//www.tenforums.com/tutorials/85279-enable-disable-secure-boot-windows-10-pc.html](https://www.tenforums.com/tutorials/85279-enable-disable-secure-boot-windows-10-pc.html)  
如果嘗試在通常執行 Windows 的電腦上執行 Batocera，你可能還必須廢棄以下設定：

- 快速啟動快速啟動、快速恢復、混合睡眠等。通常屬於自己的“Windows”類別。
- Windows 8/8.1/10/11 相容性，又稱為 CSM（注意，不支援 UEFI）、相容模式、系統支援等。通常歸入其特有的“Windows”類別。對於其他作業系統，應該終止此功能，但關閉此功能表示您可能需要切換 Windows 系統以支援舊系統啟動（應該會自動執行）。
- TPM，又稱Windows 11相容性（沒錯）、TPM安全模組2.0、USB安全性等等。通常屬於“安全”類別。這是一個現代“功能”，可以阻止其他作業系統啟動。所有預先安裝Windows 11的電腦都將預設為啟用此功能。關閉此功能可能會導致Windows 11及更高版本無法啟動，直到重新啟用，因為是接下來的「系統需求」。

與塔式電腦主機板相比，筆記型電腦型通常對載入內容有更多限制。 Google 在這方面是您的好朋友。  
如果其他方法都失敗了，請參閱故障排除頁面上的啟動部分以了解更多嘗試方法和控制台輸出。

**Mac啟動順序選擇**  
若要在Mac上存取啟動選擇畫面，請在啟動時按住\[Option\]鍵（ASCII鍵盤上為\[Alt\]鍵）。在較新的UEFI Mac上，您可能首先需要將啟動設定為“無安全性”，並在恢復模式下啟用“允許從外部媒體啟動”（啟動時看到Apple標誌後立即按住\[CMD\]（ASCII鍵盤上為\[Win\]）+\[R\]鍵即可存取）。  
有些鍵盤可能還會要求您按住\[Fn\] 鍵！

![Batoceralinux-復古遊戲機-（9）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-9.png)

若此時畫面按住\[Ctrl\]鍵，箭頭將變成圓形；

![Batoceralinux-復古遊戲機-（10）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-10.png)

這表示您的電腦將預設為啟動。按住 \[Ctrl\]+\[Return\]（ASCII 鍵盤上按 \[Enter\]）確認。

#### 從 Batocera 安裝 Batocera

也許您想傳播愛意，把 Batocera 從已設定的 U 盤安裝到機器上？或者，也許您只是想將 Batocera 直接安裝到硬碟上。或者，也許您想惡作劇。這就是您要找的。  
當 Batocera 以這種方式安裝到電腦的內建硬碟上時，現有系統資料（包括目前正在運行的作業）都將被覆蓋。與 USB 從啟動相比，您的板主機可能無法從內建硬碟啟動Batocera。請準備一台額外的計算機，以便在此過程失敗時進行恢復。如果您只是想使用大容量硬盤來儲存所有  
ROM/BIOS/保存，指的是使用外部儲存。  
此方法與使用 Etcher 燒寫 Batocera 鏡像相同。如果您擁有所需的專業知識和工具，則應將驅動器直接連接到正在運行的計算機，然後使用 Etcher 燒寫 Batocera，這樣就重新從伺服器下載鏡像。如果無法執行此操作，請參閱以下說明。 Batocera  
不會像其他即時發行版本那樣透過 USB/SD 卡複製系統檔案來安裝自己。它必須從您下載、安裝和安裝整個映像。

1. 確保您已連接到互聯網，並且您的 USB 記憶棒有足夠的免費儲存空間來下載 Batocera 圖片。
2. 啟動Batocera並按\[START\]開啟主選單。 ![Batoceralinux-復古遊戲機-（11）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-11.png)
3. 導覽至系統設定→在新磁盤上安裝BATOCERA。  
	![Batoceralinux-復古遊戲機-（12）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-12.png) ![Batoceralinux-復古遊戲機- 16](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-16.png)
4. 選擇您想要安裝 Batocera 的目標位置。確保它不是您目前啟動的驅動器，因為這會損壞您的 USB 記憶棒！  
	![Batoceralinux-復古遊戲機-（14）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-14.png)
5. 檢查您是否選擇了正確的驅動器。這很容易搞砸，而且它的行為是不可逆轉的。
6. 深呼吸並檢查第三次。這真的很重要。
7. 選擇您的目標架構。它應該與您第一次安裝到 USB 時在下載頁面上選擇的相同（大多數情況下，這將是 X86\_64）。
8. 確保您確定了這一點。您的菜單如下所示： ![Batoceralinux-復古遊戲機-（15）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-15.png)
9. 按安裝。  
	根據您的網路連線情況，下載和安裝可能需要一些時間。在此期間，您將無法使用 Batocera 系統。

如果您需要在內部硬盤上安裝特定版本的 Batocera，並且您無法將磁碟機插入另一台電腦，將所需的版本刷入其中，您可以隨時安裝最新的 Batocera 並手動降級。

**Batocera 無法從內部硬盤啟動！**

**哎呀，你已經被警告過了。你可以嘗試以下方法：**

- 配置主機板的 BIOS 設置，以允許載入 bootx64.efi 檔案。這可能需要進入特定的 EFI 啟動順序子選項。如上所述，每個主機板在這方面都是配置的。
- 手動將 Batocera 安裝到已經存在的可啟動 EFI 分割區上。
- 使用MBR分區表，而不是預設的GPT。視訊教學講解如何設定。

如果仍然遇到問題，請確保也嘗試完成上述故障修復部分或故障修復頁面上的啟動部分。如果仍然失敗，請從 USB 啟動復原。

**Etcher弄壞了我的USB！**  
實際上，Etcher或任何刷寫工具都無法做到這一點。不過，也可能是驅動器本身快要壞了，在嘗試寫入資料時，其中一個文章單元開始出現故障（需要更換）。請參閱Etcher關於此主題的，以了解更多信息，以及如何“拯救”驅動器。這些方法可用於在刷寫Batocera後「使用我的」 USB」。  
請小心選擇正確的驅動器。否則可能會意外指定您的 Windows 磁碟機。  
本質上，在 Windows 上從命令提示字元執行以下（其中 # 是所需磁盤的編號）：

磁盤分割區  
首發磁盤  
選擇磁盤#  
清理的  
建立分割區主  
選擇分割區1  
快速刪除

**來源：**  
[https://wiki.batocera.org/-Batocera.linux-Wiki](https://wiki.batocera.org/-Batocera.linux-Wiki)

**永久連結：**  
[https://wiki.batocera.org/install\_batocera](https://wiki.batocera.org/install_batocera?rev=1666162744)  
?rev=1666162744最後更新：2022/10/19 08:59

![Batoceralinux-復古遊戲機-（1）](https://manuals.plus/wp-content/uploads/2025/10/Batoceralinux-Retro-Gaming-Machine-1.png)

[https://wiki.batocera.org/](https://wiki.batocera.org/)

#### 常見問題解答

Batocera 的最低儲存要求是什麼？

所需的最小儲存空間為 8 GB，但建議使用 16 GB 或更高容量以實現全部功能和更新。

如果刷機過程失敗，我可以排除故障嗎？

嘗試關閉不需要的軟體，如防毒程序，解壓縮文件，如果驗證失敗，請手動執行，並確保啟動磁碟機包含必要的文件。

### 文件/資源

<table><tbody><tr><td><a href="https://zh-tw.manuals.plus/m/77b5c9139dbd9886c3f55b3515d3d78df210644903dc130512d1a058aef30633"><img src="https://manuals.plus/m/77b5c9139dbd9886c3f55b3515d3d78df210644903dc130512d1a058aef30633.png" width="140"></a></td><td><a href="https://manuals.plus/m/77b5c9139dbd9886c3f55b3515d3d78df210644903dc130512d1a058aef30633"><font><font>Batocera.linux復古遊戲機</font></font></a><p><font><font>安裝指南 · Batocera.linux 復古遊戲機、Batocera.linux、復古遊戲機、遊戲機</font></font></p></td></tr><tr><td colspan="2"></td></tr></tbody></table>

### 參考

- [batocera.linux](https://batocera.org/#download) batocera.org
- [batocera.linux](https://batocera.org/download) batocera.org
- [kb.wisc.edu/helpdesk/page.php?id=58779](https://kb.wisc.edu/helpdesk/page.php?id=58779) kb.wisc.edu
- [開始 \[Batocera.linux - Wiki\]](https://wiki.batocera.org/) 維基.batocera.org
- [add\_games\_bios \[Batocera.linux - 維基\]](https://wiki.batocera.org/add_games_bios) 維基.batocera.org
- [選擇一台桌上型電腦 \[Batocera.linux - Wiki\]](https://wiki.batocera.org/choose_a_desktop_computer) 維基.batocera.org
- [選擇單板計算機 \[Batocera.linux - 維基百科\]](https://wiki.batocera.org/choose_a_single_board_computer) 維基.batocera.org
- [設備 \[Batocera.linux - Wiki\]](https://wiki.batocera.org/devices) 維基.batocera.org
- [顯示問題 \[Batocera.linux - Wiki\]](https://wiki.batocera.org/display_issues) 維基.batocera.org
- [emulationstation\_overview \[Batocera.linux - Wiki\]](https://wiki.batocera.org/emulationstation_overview) 維基.batocera.org
- [使用者手冊](https://manual.tools/?p=17937047#MTQ4LjExMy4yMTAuMjUwOzQwLjc3LjE2Ny4xMzIsIDEwOC4xNjIuMjQxLjE0OTsxMDguMTYyLjI0MS4xNDk7Ow==) 手冊.工具