---
title: "社群瘋傳的自訂Apple Pay卡面怎麼做？使用前要注意什麼？AirCard 6步驟教學一次看"
source: "https://www.bnext.com.tw/article/92514/aircard-apple-pay-card-art-tutorial"
author:
  - "[[黃若彤]]"
published: 2026-10-08
created: 2026-10-08
description: "社群上瘋傳的自訂Apple Pay卡面，可以靠開源工具AirCard就能完成，本文一次整理6步驟教學以及注意事項。"
tags:
  - "clippings"
status: "inbox"
---
最近Threads、Instagram上掀起一股Apple Pay卡面改造風潮。有人把信用卡換成自家寵物的插畫，有人選擇粉嫩色系的卡通圖案，還有人把錢包裡的信用卡全部換成統一風格。這些照片也讓不少網友好奇：Apple Pay的卡面到底怎麼改？

一般情況下，Apple Pay的信用卡卡面由發卡銀行提供，使用者無法自行更換。過去若想修改卡面，通常得透過越獄等較複雜的方式。近期受到關注的開源工具AirCard，則讓使用者不必越獄，只要透過電腦連接iPhone，就能將信用卡卡面換成自己喜歡的圖片。

### 文章目錄

## AirCard是什麼？免費開源的Apple Pay卡面更換工具

AirCard由開發者Mak5er在GitHub上公開，目前已累積超過6,000顆星，是近期受到關注的iPhone個人化工具。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

AirCard原始版本支援Mac，包括Apple Silicon與Intel晶片機型。

AirCard的運作方式是替換錢包App用來顯示卡片的圖片，原有的支付功能原則上仍可正常使用，但由於採用非官方修改方式，仍可能出現卡面顯示異常、系統更新後無法繼續使用等問題。

## AirCard使用前必看：3個風險要注意

AirCard並非蘋果官方提供的功能，使用前有幾項風險需要留意。

**它是利用系統漏洞運作的工具。** 根據開發者在GitHub上的說明，AirCard的核心技術來自一個名為airlift的iOS同步機制漏洞，可支援iOS 18.0至27.0.1，以及iOS 27.2 beta 1、beta 2。Apple已在iOS 27.2 beta 3修補這個漏洞，開發者也提醒想繼續使用的人不要升級，預料iOS 27.2正式版推出後也將無法使用。

➜ [行銷科技之父Scott Brinker：『AI代理分三種！第三種決定品牌的生死』➢11.30高峰會解密](https://edm.bnext.com.tw/martech-trends/?utm_source=web_bn&utm_medium=middle_text&utm_campaign=2026_martech&utm_content=175373&utm_term=channel_5 "行銷科技之父Scott Brinker：『AI代理分三種！第三種決定品牌的生死』➢11.30高峰會解密")

不過，為了保留自訂卡面而刻意不更新iOS，也意味著手機可能無法取得後續的安全性修補。考量到iPhone通常綁定信用卡、銀行帳戶等敏感資訊，不建議為了更換卡面而長期停留在舊版系統。

**軟體未經Apple簽署。** 首次開啟AirCard時，macOS可能會跳出「無法確認開發者」的警示，使用者須手動允許程式執行。由於需要略過部分系統安全檢查，下載前應確認軟體來源，避免安裝來路不明的版本。

**出問題未必能獲得官方協助。** AirCard屬於非官方修改工具，若錢包App出現顯示異常等問題，Apple與發卡銀行未必能協助處理，後續系統更新也不保證相容。

## Apple Pay卡面更換前的準備：工具與圖檔規格

開始前請備妥以下項目：

- **電腦** ：Mac（Apple Silicon或Intel晶片）。
- **iPhone** ：系統版本為iOS 18.0至27.0.1，錢包中至少已加入一張信用卡或簽帳金融卡。
- **傳輸線** ：須具備資料傳輸功能。
- **AirCard** ：至GitHub專案的Releases頁面下載 `AirCard.dmg` 。
- **卡面圖片** ：建議尺寸為1536 × 969像素的PNG，與實體信用卡比例相同。

## 6步驟更換Apple Pay卡面教學

### 步驟1：下載並安裝AirCard

至AirCard的GitHub專案Releases頁面，下載最新版本的 `AirCard.dmg` ，開啟後將 `AirCard.app` 拖曳至「應用程式」資料夾。

由於AirCard非蘋果官方的軟體，首次開啟時macOS會阻擋執行。此時請開啟「系統設定」，進入「隱私權與安全性」，在「安全性」區塊找到AirCard已被阻擋的提示，點選「仍要打開」並輸入電腦密碼確認。

> [下載傳送門](https://github.com/Mak5er/AirCard)

![AirCard更換Apple Pay卡面教學 Step1：下載安裝並於系統設定放行](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-c8d5b9ff717cd39e.png?w=2000&output=webp)

前往GitHub下載最新版本的AirCard，首次開啟若遭macOS阻擋，可至「系統設定」→「隱私權與安全性」，找到AirCard的提示並點選「仍要打開」。

圖／ 數位時代

### 步驟2：連接iPhone

用傳輸線將iPhone接上電腦並解鎖螢幕。iPhone跳出「要信任這部電腦嗎？」時，點選「信任」並輸入手機密碼。連線成功後，AirCard右上角會顯示裝置名稱，左下角則出現「Connected to」的連線提示。

![AirCard更換Apple Pay卡面教學 Step2：連接iPhone並信任電腦](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-e932a2a7577d48ee.png?w=2000&output=webp)

用傳輸線連接iPhone與電腦並解鎖手機，連線成功後，AirCard右上角會顯示裝置名稱，左下角出現「Connected to」提示。

圖／ 數位時代

### 步驟3：掃描錢包中的卡片

確認AirCard上方停在「Apple Wallet」分頁，點擊「Scan Cards」。實測時按下後即自動偵測到錢包中的卡片，並顯示卡片名稱與綠色的「Matched to this iPhone in current scan」標示。

![AirCard更換Apple Pay卡面教學 Step3：掃描錢包中的卡片](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-d3b1560bdcc708e2.png?w=2000&output=webp)

在AirCard的「Apple Wallet」分頁點擊「Scan Cards」，系統會掃描卡片，掃描成功後自動顯示卡片名稱及綠色「Matched to this iPhone in current scan」標示。

圖／ 數位時代

### 步驟4：指定卡面圖片

點擊想更換的卡片，選擇準備好的圖片，或直接將圖片拖曳到卡片預覽區。

### 步驟5：寫入卡面

確認要更換的卡片已勾選後，點擊右下角的「Flash Skins」，AirCard會開始將圖片寫入iPhone，下方進度條跑完後會跳出「Success!」提示視窗。

這一步是實際修改手機內容的動作，按下前請再次確認自己已了解前述風險。

點擊想更換的卡片，選擇準備好的圖片，或直接將圖片拖曳到卡片預覽區，確認後點擊右下角的「Flash Skins」寫入卡面。

圖／ 數位時代

### 步驟6：重新開啟錢包查看新卡面

從iPhone的多工畫面將錢包App完全關閉，再重新開啟，即可看到新卡面。

![AirCard更換Apple Pay卡面教學 Step6：重新開啟錢包查看新卡面](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-6956a68ad6901790.png?w=2000&output=webp)

從多工畫面將錢包App完全關閉後重新開啟，即可看到更換後的Apple Pay卡面。

圖／ 數位時代

## 用AI設計Apple Pay卡面：尺寸規格與提示詞範例

想把Apple Pay信用卡換成自家寵物、喜歡的插畫，或是一整套統一風格的卡面，可以透過ChatGPT、Gemini等支援圖片生成的AI工具製作。只要上傳照片、描述想要的風格，就能讓AI協助去背、更換背景與重新設計構圖。

### 上傳照片，讓AI設計專屬卡面

以ChatGPT為例，本次實測上傳了一張橘貓照片，請AI保留貓咪的毛色、花紋與坐姿，去除原本的街道背景，再轉換成粉嫩色系的插畫卡面。

讀者也可以參考以下提示詞，替換成自己的寵物照片：

> 請將我上傳的寵物照片設計成Apple Pay信用卡卡面，保留寵物原本的毛色、花紋與姿勢，去除原始背景，並轉換成柔和的半寫實插畫風格。
> 
> 背景以奶油白、蜜桃粉、淺杏色為主，搭配簡約的弧形色塊、星星與肉球圖案，整體可愛但不過度花俏。
> 
> 圖片請採用橫式構圖，目標尺寸為 **1536×969像素，長寬比約1.585：1** 。將寵物放在中央偏上位置，四周適度留白，不要加入任何文字、數字或Logo。

如果想替多張信用卡設計統一風格，也可以要求AI依照不同用途調整配色，例如日常消費使用綠色、海外旅遊使用藍色，方便辨識。

### 卡面尺寸與構圖注意事項

AirCard建議使用 **1536×969像素的PNG圖片** 。雖然可以指定AI生成信用卡比例的圖片，但輸出尺寸不一定完全符合規格。本次實測生成的圖片為1579×996像素，仍需要進一步調整。

可以使用AirCard提供的離線卡面編輯器裁切圖片，並匯出符合規格的PNG檔案：

1. 下載編輯器：進入AirCard的GitHub專案頁面，依序點選「tools/card-artwork」資料夾與「index.html」，再點選右上角的「Download raw file」下載。
2. 開啟編輯器：以瀏覽器開啟下載好的index.html，不需安裝或連網，圖片僅在本機處理。
3. 匯入圖片：點選「選擇圖片」，或直接將圖片拖曳至預覽區，支援PNG、JPEG與WebP格式。
4. 調整構圖：透過縮放與水平、垂直取景滑桿調整畫面位置；若圖片四角有白邊，稍微放大即可裁除。
5. 匯出卡面：點選「下載卡面PNG」，即可取得1536 × 969像素的卡面檔案。

> [編輯器路徑](https://github.com/Mak5er/AirCard/tree/main/tools/card-artwork)

完成後，即可將圖片匯入AirCard，依照前述步驟替換卡面。

![AirCard更換Apple Pay卡面教學 小撇步：用離線卡面編輯器裁切圖片](https://image-cdn.learnin.tw/bnextmedia/image/album/2026-10/img-mcp-e721a179baa7cbc6.png?w=2000&output=webp)

可以使用AirCard提供的離線卡面編輯器裁切圖片，並匯出符合規格的1536×969像素PNG檔案。

圖／ 數位時代

## AirCard常見問題

### Q1：Apple Pay卡面要如何還原？

AirCard目前沒有一鍵還原功能。最直接的方式是在錢包中移除該卡片後重新加入，系統會重新下載銀行提供的原始卡面。

### Q2：iOS更新後，自訂卡面會消失嗎？

系統更新可能清除錢包的圖片快取，卡面會恢復為原始樣式。由於漏洞已在新版iOS中修補，更新後也未必能再次寫入，這點需要事先考量。

### Q3：換新手機後，自訂卡面會跟著轉移嗎？

AirCard修改的是單一iPhone本機的圖片快取，新手機重新加入卡片時，會下載銀行提供的原始卡面，需要再用AirCard寫入一次。

> 延伸閱讀： [iPhone舊機回收｜BUNNIT自助機暴紅，整體回饋多4千元？操作流程、限制一次看](https://www.bnext.com.tw/article/92336/bunnit-iphone-recycling-kiosk-guide)  
> [2026行動支付怎麼選？LINE Pay、街口、Apple Pay差在哪？TWQR是什麼？一張表看懂](https://www.bnext.com.tw/article/92274/mobile-payment-taiwan-how-to-choose)

資料來源： [AirCard GitHub專案](https://github.com/Mak5er/AirCard)

本文初稿為AI編撰，整理．編輯/黃若彤