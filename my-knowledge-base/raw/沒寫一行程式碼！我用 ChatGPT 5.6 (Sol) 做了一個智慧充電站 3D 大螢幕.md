---
title: "沒寫一行程式碼！我用 ChatGPT 5.6 (Sol) 做了一個智慧充電站 3D 大螢幕"
source: "https://codelove.tw/@tony/post/qZrjAq"
author:
published:
created: 2026-07-15
description: "沒寫一行程式碼！我用 ChatGPT 5.6（Sol）做了一個智慧充電站 3D 大螢幕  ==============================================    先說結論。從設計稿到能在瀏覽器裡打開、點擊充電樁跳出狀態面板的 3D 視覺化大螢幕，我沒有寫任何一行..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月15日](https://codelove.tw/@tony/post/qZrjAq "2026-07-15 08:45:12")

## 沒寫一行程式碼！我用 ChatGPT 5.6 (Sol) 做了一個智慧充電站 3D 大螢幕

## 沒寫一行程式碼！我用 ChatGPT 5.6（Sol）做了一個智慧充電站 3D 大螢幕

```
先說結論。
```

從設計稿到能在瀏覽器裡打開、點擊充電樁跳出狀態面板的 3D 視覺化大螢幕，我沒有寫任何一行程式碼，沒有在 Blender 裡手動拖過一次控制項，也沒有切過一張圖。

整條鏈路交給了 ChatGPT 5.6（Sol）。

但這篇文章不是來吹 AI 有多厲害的。我想說的是：這條路上我踩了哪些坑，以及為什麼同樣的工具，用法不同，結果差了十萬八千里。順便對 [有了大屏設計稿還不夠，我又用Gpt-image-2把裡面的素材全扒了出來](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzU3MDkwMDI0MQ%3D%3D%26mid%3D2247488698%26idx%3D1%26sn%3Dd29885e9a705b1ed2d3c2ff7f4e815ed%26scene%3D21%23wechat_redirect) 這篇文章做一個補充和資源切割提示詞的修復。

這個是我最終實現的網頁效果，我覺得效果還行。

[![截屏2026-07-12 21.08.24.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/48d41afd242042e4b86391d23f59aa65~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=NoLRMUQjtn6pSmHESoMNkzv3bOk%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/48d41afd242042e4b86391d23f59aa65~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=NoLRMUQjtn6pSmHESoMNkzv3bOk%3D)

---

## 四個步驟，每一步的輸入都是上一步的輸出

鏈路本身不複雜：

**設計稿 → 切割透明素材 → Blender 自動建模 → 3D 大螢幕網頁**

工具組合是 Codex（ChatGPT 5.6（Sol））、blender、blender-mcp，還有一個是我之前做的 @lius1314/visual-dashboard-scaffold 的大螢幕腳手架。

聽起來很順。實際上還是碰到很多問題的，那我們按照步驟來看看整體流程遇到的問題和如何解決吧。

---

## 第一步：設計稿

設計稿不是一次成型的，來回定了好幾輪。先是照著一張暗色科技等距沙盤風格的參考圖生成第一版充電站概念圖，看完覺得質感不夠，又重做成最高畫質的 16:9 版本。到這一步設計稿已經能用了，但為了配合後面的切圖流程，我又另外出了一版預留透明背景的稿子——因為素材切割終究是要把圖示、邊框從背景裡挖出來，源頭設計稿如果本身沒有為分離預留乾淨的邊界，後面怎麼切都費勁。

[![image.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4b2f0e36c14a40548a337779a9aee8f5~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=ok0xgwhV3Gbpd1Zswspsbzo3M84%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4b2f0e36c14a40548a337779a9aee8f5~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=ok0xgwhV3Gbpd1Zswspsbzo3M84%3D)

[![設計稿.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0822163a1c694e45b65ba95668836637~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=BCT7XYfI2JHgbBTmSAs4h%2Fqpuig%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0822163a1c694e45b65ba95668836637~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=BCT7XYfI2JHgbBTmSAs4h%2Fqpuig%3D)

上面的設計稿是主體藍的，我覺得做這種太多了，而且它的介面我覺得太大眾了，所以我就重新設計了設計稿，下面的暗黑色的是我覺得還不錯的。

[![image.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/961fe41c11e14ab4b20265a2849bc798~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=2ZK9HTFl3bhmimn8juGbwFNpKms%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/961fe41c11e14ab4b20265a2849bc798~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=2ZK9HTFl3bhmimn8juGbwFNpKms%3D) [![設計稿-v2.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0c7d878c6d474d76b6283a9d837e7ea2~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=BYmssXu4lS8s1BRrqqjr9%2F8f0bY%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0c7d878c6d474d76b6283a9d837e7ea2~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=BYmssXu4lS8s1BRrqqjr9%2F8f0bY%3D)

[![設計稿-v3.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/7a154633c0624b94b12bfd4b3ec0ec0f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=7Pg3zGx3C%2Fs8SNNEO6Rs8wAavZg%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/7a154633c0624b94b12bfd4b3ec0ec0f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=7Pg3zGx3C%2Fs8SNNEO6Rs8wAavZg%3D) 這一步花的時間不算少，但沒出什麼坑，跳過不細說，直接進第二步。

## 第二步：切圖

### 第一版，27 個素材，全部白切

設計稿定稿後，我讓 Codex 按座標把圖示、卡片邊框、分隔線摳出來。提示詞是使用之前這篇文章的提示詞 [有了大屏設計稿還不夠，我又用Gpt-image-2把裡面的素材全扒了出來](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzU3MDkwMDI0MQ%3D%3D%26mid%3D2247488698%26idx%3D1%26sn%3Dd29885e9a705b1ed2d3c2ff7f4e815ed%26scene%3D21%23wechat_redirect) —背景色 [#0B1426](https://link.juejin.cn/?target=) ，摳出圖示和邊框，校驗一下尺寸和 RGB。

跑出來 27 個素材，數字上什麼都對：12 個圖示、7 個卡片外框、6 個裝飾件，尺寸和顏色校驗全部通過。

然後我把它們貼到頁面上。

圖示放大到 200%，全是虛的。原圖裡很多圖示本身只有二三十像素見方，切圖腳本直接插值放大充數，邊緣發糊，內部鏤空的地方糊成一片。更離譜的是背景根本不透明——貼到頁面上，只要頁面背景和 [#0B1426](https://link.juejin.cn/?target=) 差一點，圖示周圍就多出一圈矩形色塊，像貼了一塊膏藥。

27 個素材，等於白切了。

[![f79c3708756117d7b56abb141f0c1206.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4228fb4f2bca43299e13abd34d89f849~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=Nmn9Ci%2FbfFaww3Lw19cCeJkA1kk%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4228fb4f2bca43299e13abd34d89f849~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=Nmn9Ci%2FbfFaww3Lw19cCeJkA1kk%3D)

後來我想明白了。當時的提示詞裡有一句「RGB 無 Alpha、 [#0B1426](https://link.juejin.cn/?target=) 背景色校驗」——這句話自己就打架。既然要求 RGB 無 Alpha 通道，背景只能是實色，根本談不上透明。模型照著這句話做，做出來的東西完全符合要求，但就是沒法用。

這不是模型的問題，是我的問題。提示詞只寫了要做什麼，沒寫怎麼判斷做得夠不夠好。沒有判定標準，模型就會挑最省事的路走——插值放大代替清晰，實色背景代替真透明，給你一個看起來合格的結果。

### 第二版，把「驗收標準」寫進提示詞

我沒有在原來的提示詞上打補丁，直接推倒重寫。核心變化就一個：把「描述任務」改成「描述任務 + 每一步驗收標準」。

透明度按死：圖示、卡片底板、標題背景、分隔裝飾，全部必須輸出 PNG-32 RGBA，空白區域 Alpha 必須為 0。白底、黑底、棋盤格偽透明，全部列為禁止項。

清晰度分路處理：原圖主體大於 64 像素、輪廓還看得清，走精確摳取，保留原始像素；原圖本身就糊，走受約束的高清重建——但只能照著原圖的外輪廓、比例、線寬去理清邊緣，不允許憑空加紋理，不允許加原本沒有的細節；要是連重建的依據都不夠，直接跳過並說明原因，不硬湊一個將就能用的版本。

驗收動作也寫進流程裡：每個素材切完，分別放到純白和純黑背景下看邊緣有沒有殘留描邊，再用透明棋盤格確認 Alpha 通道是不是真的生效。以前這一步靠人工事後抽查，現在模型給結果之前先自查一遍。

另外定了一條硬規矩：一圖一資產，每個元素單獨成檔。之前模型有時候圖省事把圖示拼成一張宮格圖，這版直接禁掉。

用新提示詞跑下來，20 個素材，四角 Alpha 全部為 0，白底黑底驗邊全部通過。

[![e68f2b7d62692b678eca859ec23ba0cd.png](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/e0c0992b573e4192a26359064a8531ec~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=mJPUuk4WquzP1Id3DcG0bAnGz84%3D)](https://p9-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/e0c0992b573e4192a26359064a8531ec~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5p-z5p2J:q75.awebp?rk3s=f64ab15b&x-expires=1784507574&x-signature=mJPUuk4WquzP1Id3DcG0bAnGz84%3D)

[![f360a77d7e33371a956fef1a7603102d.png](https://i.imgur.com/s6RXLdz.jpeg)](https://i.imgur.com/s6RXLdz.jpeg)

[![3b01268c057227e0dab572a9ce066310.png](https://i.imgur.com/UQHG63t.jpeg)](https://i.imgur.com/UQHG63t.jpeg)

這裡就列舉了一些，貌似切割的資源還是不是很超清，但是已經很夠用了。

拉開效果差距的，不是換了更強的模型，是把驗收標準寫進了提示詞。這句話值得記一下。

下面是新的切割素材資源提示詞，需要的可以自取

```
yaml 代碼解讀複製代碼# 高級 UI 資產透明拆解專家 v5.1

> 輸入：一張完整的視覺化大螢幕設計稿（通常為 1920×1080 PNG）
>
> 輸出：按批次拆分、可直接用於前端專案的獨立透明 PNG 素材

你是一名專業的 UI 資產拆解與圖像摳取專家。你的任務是識別設計稿中的可複用視覺元素，將它們逐個分離，輸出輪廓清晰、邊緣乾淨、背景真實透明的生產級素材。

---

## 最高優先級硬約束

以下規則優先於本文其他要求；發生衝突時，以本節為準。

1. **真實透明**：除 BATCH-05 外，全部輸出 \`PNG-32 RGBA\`。元素外區域的像素必須為 \`Alpha=0\`。
2. **禁止偽透明**：嚴禁使用白底、黑底、深色底、純色底或棋盤格模擬透明；棋盤格只能出現在預覽介面，不能被寫入圖片。
3. **一圖一資產**：每個檔案只包含一個獨立元素。禁止輸出宮格、拼圖、聯絡表、帶標籤的預覽板。
4. **清晰優先**：禁止把低解析度裁片僅靠 Lanczos、雙三次插值或銳化放大後冒充高清素材。
5. **忠實原稿**：保持元素的輪廓、比例、顏色、線寬、光效、透視和視覺含義，不得改變設計語言或添加原稿中不存在的細節。
6. **邊緣無污染**：發光、陰影、羽化、抗鋸齒必須正確過渡到透明，不能殘留原設計稿底色形成深色描邊、色邊或矩形光暈。
7. **無法保證則不輸出**：若目前工具不能匯出真實 Alpha PNG，必須停止並明確說明，禁止用帶底色圖片代替。

---

## 任務指令

基於上傳的設計稿，每次只處理一個批次，嚴禁混合輸出：

- 【BATCH-01】通用圖示：功能、狀態、導覽、UI 圖示
- 【BATCH-02】卡片底板：空心邊框、資訊容器
- 【BATCH-03】標題背景：標題底板、標題裝飾條
- 【BATCH-04】分割與裝飾：線條、角標、科技裝飾
- 【BATCH-05】模組背景：大面積分區底板、整體背景
- 【RESCAN】補掃：提取指定批次中遺漏的元素

每個視覺元素只能歸入一個批次，禁止重複提取。

---

## 輸出常量

| 項目 | 要求 |
|---|---|
| BATCH-01～04 背景 | 真透明，空白區域 Alpha=0 |
| BATCH-01～04 格式 | PNG-32 RGBA，sRGB |
| BATCH-05 格式 | PNG-24 RGB 或 PNG-32 RGBA，按原稿保留 |
| 安全邊距 | 5%～10%；最小 8px；發光元素最小 16px |
| 圖示畫布 | 256×256px；主體占畫布 70%～85% |
| 檔案命名 | \`{batch編號}_{序號}_{英文描述}.png\` |

原設計稿底色只用於識別元素和消除邊緣污染，不得寫入 BATCH-01～04 的最終圖片。

---

## 統一拆解流程

每個資產必須依序完成以下步驟：

1. **定位**：從上到下、從左到右掃描，記錄元素在原圖中的位置。
2. **判類**：根據元素的主要功能分配批次，不按顏色或大小隨意歸類。
3. **判定獨立性**：僅提取輪廓完整、未被遮擋、可與周圍內容分離的元素。
4. **選擇處理方式**：
   - 原圖足夠清晰：執行像素級精確摳取。
   - 原圖過小或已有明顯像素化：執行一次「受約束高清重建」。
5. **去除背景**：刪除元素之外的所有像素，使空白區域 Alpha=0。
6. **邊緣淨化**：消除原大屏底色造成的色邊，同時保留合理的半透明抗鋸齒和光效。
7. **規範畫布**：等比縮放、置中放置、保留透明安全邊距，嚴禁拉伸。
8. **雙底檢查**：分別放在純白和純黑背景上檢查邊緣，再用透明棋盤格確認 Alpha 通道。
9. **獨立匯出**：每個元素輸出為一個單獨 PNG 檔案。

---

## 清晰度處理規則

### A. 像素級精確摳取

符合以下條件時使用：原圖中元素主體短邊 ≥64px，且輪廓、細線和內部鏤空清楚可辨。

- 最大限度保留原始像素和原始視覺效果。
- 不做風格化、重繪、過度銳化或對比度增強。
- 如需縮放，僅進行一次等比高品質重採樣。

### B. 受約束高清重建

僅在元素短邊 ＜64px、明顯模糊或放大後無法使用時啟用。

- 原圖是唯一視覺依據，重建的是同一個圖示，不是創作相似圖示。
- 必須鎖定：外輪廓、長寬比例、內部結構、線寬關係、顏色、漸層方向、發光強度和朝向。
- 僅恢復清晰邊緣與連續線條，不得增加紋理、符號、層級或裝飾。
- 簡單線性圖示應具有穩定線寬、連續輪廓和乾淨內孔。
- 禁止用加粗描邊、強銳化、過度發光或提高對比度製造「清晰」假象。

若無法忠實重建，應跳過該元素並說明原因。

---

## 複雜元素歸類規則

1. 傳遞功能或狀態含義 → BATCH-01
2. 用於容納或框住內容 → BATCH-02
3. 專門服務於標題區域 → BATCH-03
4. 用於分隔、角部點綴或科技裝飾 → BATCH-04
5. 構成大面積區域整體底圖 → BATCH-05
6. 與文字、圖表或其他元素嚴重黏連且無法可靠分離 → 跳過

---

## 分批次規則

### 【BATCH-01】通用圖示

屬於：獨立功能圖示、狀態圖示、導覽圖示、UI 操作圖示。

不屬於：圖表資料點、標題裝飾、卡片角標、與卡片不可分離的業務圖形。

要求：

- 每個圖示輸出為獨立的 256×256px 透明 PNG。
- 圖示主體在畫布內視覺置中，占畫布 70%～85%，不得貼邊。
- 保留完整輪廓、內孔、細線、發光和陰影；不得斷線、黏連或堵孔。
- 不得把原圖中包含底色的矩形裁片直接作為輸出。
- 不得包含任何文字、數字、水印、名稱或編號。

逐圖驗收：

- [ ] PNG 為 RGBA，四角像素 Alpha=0
- [ ] 透明區域沒有白底、黑底、深藍底或棋盤格像素
- [ ] 白底和黑底預覽均無矩形底、深色邊、雜色邊或異常光暈
- [ ] 100% 顯示清晰；200% 顯示時輪廓連續、細線無斷裂、內孔未堵塞
- [ ] 圖示形狀、顏色、比例和視覺含義與原稿一致
- [ ] 檔案內只有一個圖示且無文字

### 【BATCH-02】卡片底板

屬於：完整邊框容器、獨立面板外框。

不屬於：標題列、模組整體背景。

要求：

- 只保留外邊框、科技角、描邊、邊緣發光和多層裝飾結構。
- 移除標題、文字、數字、圖示、圖表、資料和內部填充。
- 容器外部與中心區域均須真實透明，形成可直接疊加的空心邊框。
- 輸出寬度 ≥512px，高度等比，透明安全邊距完整。

### 【BATCH-03】標題背景

屬於：標題區域底板、標題裝飾條。

不屬於：完整卡片邊框、獨立角標。

要求：

- 移除所有標題文字，僅保留裝飾底板。
- 保留複雜輪廓、漸層、節奏線條和兩端漸隱。
- 漸隱與發光必須自然過渡到 Alpha=0。
- 輸出寬度 ≥640px、高度 ≥128px。

### 【BATCH-04】分割線與裝飾

屬於：獨立分隔線、科技線條、角標裝飾。

不屬於：卡片邊框的一部分、標題裝飾條。

要求：

- 線條和角標必須分別獨立匯出。
- 保留線條端部羽化，並自然過渡到 Alpha=0。
- 水平線長度 ≥512px；垂直線高度 ≥512px；角標畫布 ≥128×128px。

### 【BATCH-05】模組背景

屬於：大面積分區底板、整體面板背景區域。

要求：

- 保留紋理、網格、漸層和發光輪廓。
- 移除文字、數字、圖示、圖表和業務內容。
- 最小輸出 1920×1080；若原圖更大則保持原始尺寸。
- 本批次本身是背景層，可保留原稿背景，不受透明背景硬約束。

---

## 排除條件

遇到以下情況應跳過並說明，不得強行輸出劣質資產：

- 元素被遮擋、被畫面邊緣截斷或與其他元素無法可靠分離
- 去除文字後無法合理恢復底層結構
- 原始資訊不足，無法忠實重建形狀或內部結構
- 去背景後出現無法消除的嚴重色邊、斷線或透明孔洞
- 輸出仍包含文字、數字、業務資料或其他批次元素

---

## 不合格重做條件

以下任一條件成立，必須重做：

| 檢查項 | 不合格條件 |
|---|---|
| 透明背景 | 四角任一像素 Alpha≠0，或存在純色底/棋盤格底 |
| 邊緣污染 | 白底或黑底預覽出現原背景色描邊、雜色邊、矩形光暈 |
| 清晰度 | 100% 即可見模糊；200% 出現斷線、堵孔、雙重邊緣或嚴重鋸齒 |
| 忠實度 | 輪廓、比例、顏色、線寬或視覺含義偏離原稿 |
| 完整性 | 發光、陰影、羽化或邊角被裁切 |
| 內容純淨 | 存在文字、數字、水印、標籤或其他無關元素 |
| 文件組織 | 多個元素被合併到同一張圖 |

---

## 數量與命名

- 單批次每輪最多輸出 12 個資產。
- 檔案命名格式：\`{batch編號}_{序號}_{英文描述}.png\`。
- 範例：\`b01_03_settings-icon.png\`、\`b02_01_card-frame-large.png\`。
- 達到 12 個時停止並提示：

> 本批次已提取 12 個資產。回覆「繼續」提取剩餘元素，或指定新批次。

---

## RESCAN 補掃模式

觸發示例：

補掃 BATCH-01，左側面板第 3 個圖示未提取

僅提取明確指出的遺漏元素，不重複已完成資產；仍須通過原批次全部透明度、清晰度和忠實度檢查。

---

## 每輪文字報告

報告內容不得寫入圖片。完成一批後，在回覆中逐項列出：

1. 檔案名稱
2. 元素在原圖中的位置
3. 處理方式：\`精確摳取\` 或 \`受約束高清重建\`
4. 輸出尺寸
5. Alpha 檢查結果
6. 白底/黑底邊緣檢查結果
7. 被跳過元素及原因

---

## 啟動方式

請上傳設計稿並指定批次，例如：

開始 BATCH-01

可選告知原大屏底色，它只用於去除邊緣底色污染，不會寫入最終透明資產。
```

## 第三步：建模與烘焙

設計稿切完，四周的圖示、邊框、分隔線交給前端 UI 層；中間那塊 3D 場景插畫不進前端，拿去給 blender-mcp（地址取得 [github.com/ahujasid/bl…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fahujasid%2Fblender-mcp%25EF%25BC%2589%25E5%25BD%2593%25E5%258F%2582%25E7%2585%25A7%25E5%259B%25BE%25EF%25BC%258C%25E8%25AE%25A9) Codex 照著它在 Blender 裡自動建模。

[![image.png](https://i.imgur.com/BEa8tIQ.jpeg)](https://i.imgur.com/BEa8tIQ.jpeg)

先驗證了一下連通性，讓 Codex 在場景裡建一個正方體，確認 MCP 通道能跑通。通了之後，拿設計稿中間那張暗色科技等距沙盤圖重新生成一張更完整的充電站參考圖，然後讓 Codex 照著這張圖逐個模組建出來：場地底座、光伏頂棚、充電樁、車輛、儲能電池艙、變壓配電設備、控制室、發光地面線路、懸浮 UI 面板。整個過程不用人工在 Blender 裡手動操作，Codex 透過 MCP 下發的是可重複執行的建模腳本，哪裡不對改腳本重跑，不用從頭擺一次場景。

### 第一版模型的問題：形狀對了，沒有質感

建出來之後感覺哪裡不對，但說不清楚。盯著看了一會兒才反應過來：材質是平的。金屬沒有高光，地面純色沒有微紋理，玻璃沒有透射，車漆像塑膠——整個場景像是一個精確的玩具，而不是一個真實的場地。

[![image.png](https://i.imgur.com/Mvf6cbx.jpeg)](https://i.imgur.com/Mvf6cbx.jpeg)重做時把 PBR 材質流程補齊：金屬件的金屬度和粗糙度分開調，地坪加微紋理，玻璃做透射，車漆加清漆層，光伏板加反射。建模顆粒度也往上拉，最終建出一千多個物件，四輛車、四套充電樁、雙排儲能櫃、冷卻設備、線槽都單獨建出來，不再用簡化的占位模型糊弄。

[![image.png](https://i.imgur.com/nlRnhBr.jpeg)](https://i.imgur.com/nlRnhBr.jpeg)### 烘焙前先分組，別等前端接入了才想起來

模型精細了，但沒法直接丟給前端即時渲染——這麼多物體的 PBR 材質，幀率根本扛不住。所以要把光照和材質資訊烘焙成貼圖。但烘焙也有講究，不是一把梭全烘成靜態圖就完事。

在烘焙之前，我先按「將來要不要互動」把所有物體分了一次組。會被點擊的——車輛、充電樁、終端機、電池櫃、伺服器、變壓器、控制台，二十多個根節點，各自單獨烘焙貼圖，保留獨立材質通道，前端可以單獨控制它們的高亮和狀態切換。純粹是背景的——地面、建築主體、光伏頂棚、固定附件，按區域合併成幾組圖集一起烘焙，減少渲染開銷。

每組貼圖統一匯出 BaseColor、Normal、ORM、Emission 四張。ORM 這張把 AO、粗糙度、金屬度分別塞進 RGB 三個通道，前端一張圖拿到三種材質資訊。玻璃部件要保留透射效果，沒有參與合併，單獨留成透明子物件。

[![image.png](https://i.imgur.com/uGQiCAt.jpeg)](https://i.imgur.com/uGQiCAt.jpeg)這一步的判斷標準只有一條：凡是互動邏輯會碰到的物體，寧可多花烘焙成本也要保留獨立性；純粹填場景的，能合併就合併。這個分組做在烘焙階段，而不是等到前端接入時再補——正是因為提前想清楚了，後面接腳手架的時候基本沒有返工。

最終實測：源 GLB 約 77.3 MB，Web 優化後壓到 25.32 MB，107 張內嵌 WebP，貼圖最大邊 1024，23 個互動根節點完整保留。

[![image.png](https://i.imgur.com/gJ9YN5h.jpeg)](https://i.imgur.com/gJ9YN5h.jpeg)## 第四步：拼成網頁

前面三步產出的是三堆各自獨立的東西：透明 UI 素材、烘焙好貼圖的 Blender 場景、材質通道檔案。這些東西要變成使用者能打開的網頁，還差一層骨架——頁面佈局、區塊管理、螢幕自適應。這部分接了 `@lius1314/visual-dashboard-scaffold` 這個大螢幕腳手架(可以看這篇文章 [可視化大屏設計器腳手架：從設計到交付的一站式方案，](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzU3MDkwMDI0MQ%3D%3D%26mid%3D2247489407%26idx%3D1%26sn%3D92da83781f1eba5a9e49492b528e2871%26scene%3D21%23wechat_redirect) 如果你需要原始碼的話可以看這篇 [可視化大屏腳手架原始碼：一週下載突破 700，為什麼這麼多人選它？](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzU3MDkwMDI0MQ%3D%3D%26mid%3D2247489490%26idx%3D2%26sn%3Dc60b803e522df41621430c65005f4bc0%26scene%3D21%23wechat_redirect))，一個不綁定特定圖表庫的 React 大螢幕容器，預設 1920×1080，跟前面切圖流程裡素材尺寸的約束剛好對得上。

業務圖表或者 3D 場景不用改腳手架原始碼，透過 `registerBlock` 把元件註冊成一個區塊 ID，腳手架負責區塊的拖曳、縮放、層級和樣式；預覽模式下接 `autofit.js` 做整體等比縮放，邊調邊看，不用來回換解析度去試。

[![image.png](https://i.imgur.com/MiKzQZT.jpeg)](https://i.imgur.com/MiKzQZT.jpeg)

烘焙階段做的那個分組在這裡起作用：中心 3D 場景包一層 Three.js 渲染元件掛到對應區塊位置上，充電樁、儲能櫃這些單獨烘焙過的物體各自是可互動物件；背景和建築那部分只作為一張整體貼圖渲染，不參與互動邏輯，渲染開銷壓得住。

[![image.png](https://i.imgur.com/B36wQNB.jpeg)](https://i.imgur.com/B36wQNB.jpeg)

切圖流程產出的圖示、邊框、分隔線，按腳手架約定的靜態資源目錄放進去就能用。資訊卡片、告警列表、能耗曲線這些常規區塊，是標準 React 元件按區塊 ID 註冊。整套配置最後匯出成 JSON 作為初始配置載入，後面調佈局不用改程式碼，直接在編輯模式裡拖。

[![image.png](https://i.imgur.com/nL1NVw2.jpeg)](https://i.imgur.com/nL1NVw2.jpeg)

## 最後

做完這整條鏈路，我覺得有兩件事值得單獨拎出來說。

第一件事，提示詞裡有沒有驗收標準，決定了你拿到的是素材還是無法使用的東西。沒有可執行的判定標準，模型永遠挑最省事的路。它不是在糊弄你，它只是在照章辦事——你說的章，本來就沒寫清楚。把驗收標準顯式寫進提示詞之後，產出品質的提升比換一個更強的模型明顯得多，這讓我多少有點意外。

第二件事，前期的分層決策，決定了後期的返工量。烘焙階段有沒有提前想清楚哪些物體將來要被點擊，直接決定前端接入時要不要重來一遍。做在正確的時間點，後面每一步都會順；做晚了，代價是把整個場景重新烘焙一遍。

Codex（ChatGPT 5.6 Sol）在切圖、生成參照圖、下發 Blender 建模腳本這幾個環節都能給出結構化、可執行的結果，中間不需要人再補細節。這是整條鏈路能接起來的前提。但前提之外，還是得自己想清楚每一步要的是什麼。

另外發現 gpt5.6 sol 在 3D 建模方面還是不能真正還原設計圖，也沒有做到那種工業級的質感，不過做的模型確實也很不錯了，未來可期。

預覽地址： [liush.top/solar-stora…](https://link.juejin.cn/?target=https%3A%2F%2Fliush.top%2Fsolar-storage-charging-ems)

如果你需要原始碼參考的話，可以來 **柳杉前端** 公众号同名文章自取哈。

---

原文出處： [https://juejin.cn/post/7661160753004904448](https://juejin.cn/post/7661160753004904448)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qZrjAq) [2瀏覽](https://codelove.tw/@tony/post/qZrjAq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qZrjAq)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Anthropic 為何限制中國大陸使用 Claude？](https://codelove.tw/@tony/post/qO2P8a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝12 💬3 ❤️5

485

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

173

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784077210&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqZrjAq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTE1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjExNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTE1Il1dLDBd&amp;dt=1784077209350&amp;bpp=3&amp;bdt=657&amp;idt=1197&amp;shv=r20260714&amp;mjsv=m202607130101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784077207%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784077207%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784077207%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1598345025229&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=31099736%2C95396252&amp;oid=2&amp;pvsid=3677613048038318&amp;tmod=615443353&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=1282" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260714/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>