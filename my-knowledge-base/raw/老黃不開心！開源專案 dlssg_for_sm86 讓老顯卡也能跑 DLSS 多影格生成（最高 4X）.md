---
title: "老黃不開心！開源專案 dlssg_for_sm86 讓老顯卡也能跑 DLSS 多影格生成（最高 4X）"
source: "https://www.kocpc.com.tw/archives/668354"
author:
  - "[[達小編]]"
published: 2026-09-10
created: 2026-09-11
description: "過去要在舊款顯示卡上實現影格生成，多數做法是把 DLSS 轉譯成 AMD FSR Frame Generation 的替代方案，這種方式雖然能補幀，但畫面品質與相容性往往不如原生 DLSS。dlssg_for_sm86 走的是另一條路：直接把 NVIDIA 官方的 DLSSG 執行階段（Runtime）移植到"
tags:
  - "clippings"
status: processed
---
NVIDIA 把 DLSS 多幀生成（Multi Frame Generation，MFG）技術限定在最新的 GeForce RTX 50 系列顯示卡上，但這道限制如今被民間高手打破。開發者 sdli1995 在 GitHub 上發布了開源專案「dlssg\_for\_sm86」，成功讓採用 Ampere 架構的 GeForce RTX 30 系列顯示卡執行 NVIDIA 原生的 DLSS 多影格生成，最高支援 4X 幀生成，測試中甚至整合了尚未全面下放的 DLSS 5 技術。NVIDIA 官方一直強調 MFG 需要 Blackwell 的第五代 Tensor Core 與硬體 Flip Metering 支援，RTX 40 系列也只能使用新一代的影格生成模型，如今這個專案讓 RTX 30 也能體驗多影格生成，對還在觀望是否升級的玩家來說話題性十足。  

## dlssg\_for\_sm86 打破限制：直接移植官方 Runtime

過去要在舊款顯示卡上實現影格生成，多數做法是把 DLSS 轉譯成 AMD FSR Frame Generation 的替代方案，這種方式雖然能補幀，但畫面品質與相容性往往不如原生 DLSS。dlssg\_for\_sm86 走的是另一條路：直接把 NVIDIA 官方的 DLSSG 執行階段（Runtime）移植到 Ampere 架構，完整保留原廠的神經網路模型與管線流程，讓 RTX 30 顯示卡繞過原生限制。RTX 30 系列至今仍有大量玩家使用，Steam 硬體調查中多款 Ampere 顯示卡長期佔排行榜前列，這項移植對這群使用者有實際意義。

根據「dlssg\_for\_sm86」開發者說明，專案採用內嵌原版 DLSSG 執行階段與配套後端的方式運作，玩家只需把 version.dll（若遇載入衝突可改用 winmm.dll）與設定檔 dlssg\_sm86.ini 放到遊戲實際渲染的執行檔旁就能啟動，過程不需要 Python 或 PowerShell 等外部啟動器。

### 運作原理：Proxy 代理與內嵌執行階段

這個模組的運作方式並非單純偽裝 GPU 型號。它會在遊戲執行檔同層目錄植入 Proxy 代理程式，把遊戲對影格生成的呼叫重新導向到修改後的 DLSSG 310.1 執行階段，並為 RTX 30 提供 SM86 相容後端。Ampere 架構可以使用預先編譯的 SM86 cubin 核心，或交由驅動程式即時編譯的 PTX 程式碼。設定檔中的 MaxGeneratedFrames 可以調整上報的插幀數量，設為 3 代表每個真實幀間隔最多額外生成 3 幀，也就是最高 4X 多影格生成，實際生成數量仍由遊戲決定，這個設定不會強制遊戲開啟 4X，也不會增加遊戲選單的選項。  
![老黃不開心！開源專案 dlssg_for_sm86 讓老顯卡也能跑 DLSS 多影格生成（最高 4X） - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260910001745_0_2c023f.jpg "老黃不開心！開源專案 dlssg_for_sm86 讓老顯卡也能跑 DLSS 多影格生成（最高 4X） - 電腦王阿達")

這項移植之所以可行，與 DLSS 4 的架構調整有關。NVIDIA 從 DLSS 4 開始拿掉對硬體光流加速器的依賴，改用 AI 光流模型，這讓新版 DLSSG 管線移植到 Ampere 架構成為可能。開發者也提供了 PTX 與 Cubin 兩種核心格式的選擇，RTX 3080 Ti 會被自動識別為 SM86，使用預設設定即可正常運作。首次執行時，代理會把配套檔案釋放到 %LOCALAPPDATA% 下的快取目錄，驗證後載入，之後重複使用快取，檔案損壞時會自動復原。設定檔修改後需要完全退出遊戲再重啟才會生效，目前沒有熱重載功能。

### 實測數據

根據作者實測，使用 GeForce RTX 3080 Ti 並手動調整到 2X 或 4X 模式後，遊戲幀率明顯提升。《黑神話：悟空》在關閉影格生成時約 50 FPS，開啟 2X 提升到 80 FPS，4X 模式下達到約 150 FPS。另一組測試使用 RTX 3080 12GB 跑《電馭叛客 2077》的全光線追蹤搭配 DLSS 5，原生約 35 FPS，2X 提升到 60 FPS，4X 達到約 100 FPS。

![](https://www.youtube.com/watch?v=KNVr5pHAygI)

從測試結果來看，2X 模式約提升一倍幀率，4X 模式則把幀率推到原生三倍左右，效果相當顯著。也有 RTX 3080 12GB 的使用者上傳了《電馭叛客 2077》的實機影片，確認 DLSS 5 在 RTX 30 上可以正常運作。除了這兩款遊戲，社群也陸續在更多作品上測試相容性與穩定度。  
![老黃不開心！開源專案 dlssg_for_sm86 讓老顯卡也能跑 DLSS 多影格生成（最高 4X） - 電腦王阿達](https://www.kocpc.com.tw/wp-content/uploads/2026/09/20260910002038_0_8d786f.jpg "老黃不開心！開源專案 dlssg_for_sm86 讓老顯卡也能跑 DLSS 多影格生成（最高 4X） - 電腦王阿達")

close

kocpc dynamic layout demo

00:00

00:00

00:59

 <video controls=""><source src="https://gnetwork.gliastudios.com/gnetwork/www.kocpc.com.tw/www.kocpc.com.tw-1776849532.388493.mp4?verify=1789090940-xrfA%2FYkGlvVIwNQwHZ%2Fc8HlUYaDm%2Bcdnqh5zCD5EYGg%3D"> <source src="https://gnetwork.b-cdn.net/studio_backend/www.kocpc.com.tw/www.kocpc.com.tw-1776849532.388493.mp4?token=TwXkv4TZLxczHV52fka6bPQJhRAwAF--3_205a4Eu4Y&amp;expires=1789177340"></video><video src="https://cdn.onead.com.tw/uploads/material/video/file/36701/c8e94f9f-90e9-4135-99bf-f33642fb1927_1200k.mp4?vid=36701" controls=""></video><iframe src="https://imasdk.googleapis.com/js/core/bridge3.789.0_zh_tw.html#deid=%22%22&amp;eventfe_experiment_ids=%5B%5D&amp;fid=%22goog_27272127%22&amp;genotype_experiment_data=%7B%22experimentStateProto%22%3A%22%5B%5B%5B45713128%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B962325777%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45796605%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B945081832%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45816264%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45761044%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45722344%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45801847%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45784857%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45668885%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685340%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45734716%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45735891%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45663239%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B839547366%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45676441%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45645574%2Cnull%2C%5B%5D%5D%2C%5B45688859%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45685601%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5Bnull%2C45685602%2Cnull%2C%5Bnull%2C100%5D%5D%2C%5B45830425%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45820997%2Cnull%2Cnull%2C%5B%5D%5D%2C%5Bnull%2C45812678%2Cnull%2C%5B%5D%5D%2C%5B45756824%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45794243%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45747172%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B775241416%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107958%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45729602%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B%5D%5D%5D%2C%5B%5B16%2C%5B%5B1%2C%5B%5B31089630%5D%2C%5B31089631%2C%5B%5B45668885%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B83323635%5D%2C%5B83323636%2C%5B%5Bnull%2C45812678%2Cnull%2C%5Bnull%2C100%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B83323870%5D%2C%5B83323871%2C%5B%5B45830425%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1000%2C%5B%5B95332046%5D%5D%5D%2C%5Bnull%2C%5B%5B95332047%5D%5D%5D%2C%5B10%2C%5B%5B95338769%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%2C%5B95338770%2C%5B%5Bnull%2C45645574%2Cnull%2C%5Bnull%2C2%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95351425%5D%2C%5B95351426%2C%5B%5B45676441%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95373378%2C%5B%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95373379%2C%5B%5B45747172%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107959%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B792614055%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B781107957%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95378629%5D%2C%5B95378630%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381582%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753603%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%2C%5B95381583%2C%5B%5B45729602%2Cnull%2Cnull%2C%5B1%5D%5D%2C%5B45753604%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95389581%5D%2C%5B95389582%2C%5B%5B635466687%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95391735%5D%5D%5D%2C%5B50%2C%5B%5B95393784%5D%2C%5B95393785%2C%5B%5B45796605%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95394237%5D%2C%5B95394238%2C%5B%5B45801847%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B50%2C%5B%5B95396036%5D%2C%5B95396037%2C%5B%5B45756824%2Cnull%2Cnull%2C%5B%5D%5D%5D%5D%5D%5D%2C%5Bnull%2C%5B%5B95396834%5D%2C%5B95396835%2C%5B%5B45816264%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%2C%5B10%2C%5B%5B95398837%5D%2C%5B95398838%2C%5B%5Bnull%2C960449109%2Cnull%2C%5Bnull%2C1%5D%5D%5D%5D%5D%5D%2C%5B1%2C%5B%5B95402052%5D%2C%5B95402053%2C%5B%5B957164676%2Cnull%2Cnull%2C%5B1%5D%5D%5D%5D%5D%5D%5D%5D%5D%2Cnull%2Cnull%2C%5Bnull%2C1000%2C1%2C1000%5D%5D%22%7D&amp;imalib_experiments=%5B95322027%2C95331589%2C83323871%2C95332046%2C95389582%2C95393784%2C95396037%5D&amp;is_eap_loader=false&amp;managed_js_experiment_id=0&amp;page_correlator=3063857764991877&amp;pvsid=4547449843321561&amp;top_accessible_page_url=%22https%3A%2F%2Fwww.kocpc.com.tw%2Farchives%2F668354%22" allowfullscreen="" allow="autoplay" title="Advertisement" width="300" height="168.75"></iframe>

pause

volume\_mute

AD

### 與 RTX 50 的差距與注意事項

雖然 RTX 30 執行多影格生成能讓效能大幅提升，但在系統延遲、能源效率與幀平滑度上仍比不上 RTX 50 Blackwell 架構。NVIDIA 官方把 MFG 限定在 RTX 50，理由是 Blackwell 具備第五代 Tensor Core 與硬體 Flip Metering 等特性，這項移植並不能證明 RTX 30 能追上 Blackwell 的延遲與效率表現。

NVIDIA 曾表示硬體 Flip Metering 能讓多影格生成的畫面呈現更穩定，這是 Ampere 缺乏的硬體特性，因此 RTX 30 上的表現與 RTX 50 仍有一段距離。從實機影片觀察，幀率數字雖然上升，但搭配 DLSS 5 時的整體體驗與原生 80 FPS 仍有差距。開發者也承諾會持續針對 RTX 30 的多影格生成支援進行最佳化。另外，這類 mod 屬於非官方改動，在線上遊戲或啟用反作弊保護的遊戲中使用有風險，建議避免。

### 社群移植熱潮的背景

dlssg\_for\_sm86 的出現，延續了社群先前把 DLSS 5 移植到舊款顯示卡的熱潮。早在 8 月底，Mod 作者 Uncle Burrito 就透過 RenoDX 4.5 讓 DLSS 5 在 RTX 40 與 RTX 30 系列上運作，支援多幀生成。如今 dlssg\_for\_sm86 更進一步，直接以官方 DLSSG 執行階段為基礎，把多影格生成帶到 RTX 30。社群對這類移植的討論也持續發酵，不少玩家在 X 上分享測試結果。 NVIDIA 官方尚未對這類移植作出回應，但社群對舊卡支援的呼聲一直存在，這類專案也讓外界重新審視 MFG 的硬體門檻是否真的無法跨越。對還在用 RTX 30 的玩家來說，這類專案也代表手上的顯示卡仍有延續效能的空間。

### dlssg\_for\_sm86 – GitHub

### 結語

對手上還握著 RTX 30 顯示卡的玩家來說，多影格生成不再是 RTX 50 的專利。dlssg\_for\_sm86 以保留原廠模型與管線的方式完成移植，比過去轉譯成 FSR 的方案更接近原生體驗，但能否在日常遊戲中穩定使用，仍要看後續的相容性與效能表現。對開發者而言，這也是一次技術驗證：只要保留原廠模型與管線，硬體架構的限制並非無法跨越，接下來就看 NVIDIA 會不會對舊卡的 MFG 支援鬆口。