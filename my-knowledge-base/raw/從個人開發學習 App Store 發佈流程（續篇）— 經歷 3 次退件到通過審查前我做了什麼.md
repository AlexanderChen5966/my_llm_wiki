---
title: "從個人開發學習 App Store 發佈流程（續篇）— 經歷 3 次退件到通過審查前我做了什麼"
source: "https://codelove.tw/@tony/post/aGQ2zx"
author:
published:
created: 2026-07-27
description: "[](#%E5%80%8B%E4%BA%BA%E9%96%8B%E7%99%BC%E3%81%A7app-store%E3%83%AA%E3%83%AA%E3%83%BC%E3%82%B9%E3%83%95%E3%83%AD%E3%83%BC%E3%82%92%E5%AD%A6%E3%81%B6%E..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月26日](https://codelove.tw/@tony/post/aGQ2zx "2026-07-26 08:30:09")



## 個人開發中學習 App Store 上架流程（續篇）—— 3 次拒審與通過審查前做了什麼

[上一篇文章](https://qiita.com/jqit-yukiono/items/d33908602fd060f5e1d1) 中，我介紹了用 iOS（UIKit）+ FastAPI + Kubernetes 打造的 Minecraft 伺服器監控應用程式「MineWatch」的整體設計。那篇文章最後我這樣寫：

> 若審查通過，之後也會把審查中被指出的地方，以及實際送審流程的細節整理成另一篇文章。

之後，因為 **Guideline 5.1.2(i)（追蹤）被拒了 2 次、4.1(c)（Copycat）被拒了 1 次** ，總共送審 3 次後才終於通過。這篇文章不是接著介紹應用程式功能，而是記錄 **實際審查中被指正了什麼，以及我是怎麼修正的** 。

## TL;DR

- 原本做了自訂同意畫面，並明確寫著「不會進行追蹤」，但仍然因為 **Guideline 5.1.2(i) 連續 2 次被拒** 。原因是 SDK 端（Firebase Analytics 的依賴）混入了 IDFA 相關程式碼。
- 最後改成直接使用 Apple 官方的 **App Tracking Transparency（ATT）系統對話框** 才解決。雖然實際上沒有進行追蹤，卻要跳出允許對話框，乍看很矛盾，但就審查應對來說這是最快的做法。
- ATT 的實作不能在 `willConnectTo` 呼叫，必須在 `sceneDidBecomeActive` 才會跳出對話框，這是只能在實機上踩到的坑。
- 我在實機測試中發現並修正了把 App Check 的驗證錯誤（401）和追蹤驗證錯誤（403）混在一起的 bug。
- 也發現 Google OAuth 的同意畫面會另外要求「安全的流程」＝ **App Check 的實作與強制啟用** ，所以它和 Apple 的規則並不是完全無關。
- App Store Connect 的 App Privacy 申報（用於追蹤的資料）也因為要和二進位檔實作內容一致，而有一段重新申報的插曲。

## 第一次、第二次拒審：自訂同意畫面與 IDFA 的陷阱

MineWatch 為了讓使用者決定是否送出匿名使用狀況（Firebase Analytics）與當機回報（Firebase Crashlytics），實作了自訂同意畫面。字句上刻意避開了「追蹤」「追蹤行為」「廣告」「個人化」等字眼，並且原本以為已經具體說明了收集哪些資訊、用途是什麼（只用於品質改善）。

即便如此，還是因為 **Guideline 5.1.2(i)** 連續被拒了 2 次。就算改文案也沒用，所以我開始改變方針，懷疑的是實作本身（自訂同意畫面，以及所使用的 SDK 組成）。

原因有兩個：

1. 使用的 `FirebaseAnalytics` 套件，即使實際上完全沒有呼叫 IDFA，也會把 `GoogleAppMeasurementIdentitySupport` （IDFA 相關程式碼）包含到二進位檔裡。Apple 的靜態分析可能會判定成「具有追蹤功能但未實作 ATT」。
2. 從 Apple 的角度來看，自訂同意畫面 **不能代替 ATT** 。

因此我改成使用不含 IDFA 的 `FirebaseAnalyticsCore` product，並透過乾淨重建確認 `GoogleAppMeasurementIdentitySupport.framework` 沒有被包含進去。到這裡是第二次送審。

## 連副標題也被拒了（4.1(c) Copycat）

除了上面那件事之外，副標題中的 `Minecraft伺服器的存活監控` 也因為 **Guideline 4.1(c)** 被拒。理由是「在副標題中使用品牌名（Minecraft）會被判定為 Copycat」。在內文說明中提到「支援 Minecraft 伺服器」是作為相容性說明而被接受的，但 **副標題這種直接影響搜尋與列表顯示的位置使用品牌名是不行的** 。後來改成 `遊戲伺服器的存活監控` 就解決了。

## 第三次送審前的判斷：統一改用 ATT 對話框

經過連續 2 次拒審後，我把自訂同意畫面完全移除，改成只使用 Apple 的 ATT 系統對話框。實作上做了以下 3 件事：

- 刪除 `ConsentViewController` （自訂同意畫面）
- 新增 `TrackingPermission` ，薄薄包裝 `ATTrackingManager` ，並在啟動後立刻呼叫。 **中間不插入其他說明畫面** （避免被視為雙重提示）
- 當機回報是否送出，則保留為與 ATT 無關的設定畫面開關（因為這不屬於 Apple 定義的「追蹤」）

這裡有個實作上值得記錄的矛盾。 **MineWatch 實際上沒有使用 IDFA，也沒有做 Apple 定義的「追蹤」行為。** 跳出 ATT 對話框本身，就形成了「明明不追蹤，卻還要請求允許」這種看起來不自然的狀況；但考量到前面已經連續 2 次被拒，我優先採取 Apple 在回覆中要求的字面修正。這個決定也一路影響到後面 App Privacy 申報必須重新整理。

## 卡關點：在 willConnectTo 裡不會跳出對話框

我把 ATT 的允許對話框寫在 `scene(_:willConnectTo:options:)` 中，先在模擬器測試，表面上看起來沒問題；但 **在實機（iPhone 15 Plus）上測試時，對話框完全沒有出現** 。

原因是， `willConnectTo` 當下 scene 還沒有切換到 `.active` 。 `ATTrackingManager.requestTrackingAuthorization()` 就算在非 active 狀態呼叫，也不會丟出例外，而是悄悄回傳 `.notDetermined` ，所以很容易看不出來。

```
// NG：scene 還沒進入 .active，因此
// 會呼叫對話框但不會顯示
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options: UIScene.ConnectionOptions) {
    Task { await TrackingPermission.requestIfNeeded() }
}

// OK
func sceneDidBecomeActive(_ scene: UIScene) {
    Task { await TrackingPermission.requestIfNeeded() }
}
```

更麻煩的是， **從 CLI 啟動 App（ `xcrun simctl launch` 或 `xcrun devicectl device process launch` ）時，即使把程式改對了也很難重現** 。CLI 啟動不會產生和「點圖示啟動」完全相同的前景切換，所以就算改到 `sceneDidBecomeActive` ，也無法用這種方式驗證。一定要真的在主畫面點 App 圖示啟動，才看得到這個問題，也才能確認修正成功。

## 混淆了 App Check 的 401 與 403 的 bug

在實機驗證 ATT 對話框時，我還發現了另一個 bug。 **App Attest（用來驗證請求是否來自真正的 App）失敗了，但 App 端卻顯示「請重新登入」** 。

後端（FastAPI）當時把 Firebase ID token 驗證失敗（也就是「誰」不明）與 App Check 驗證失敗（也就是「是不是正牌 App」不明）都回傳成 401。iOS 端無法區分這兩種情況，因此對 App Check 造成的失敗也一律提示使用者重新登入，結果使用者就算重新登入也沒用。

後來我把 App Check 失敗的回應獨立成 403。

```
# 401：不知道「誰」是誰（Firebase ID token 驗證失敗）
#     → 重新登入有可能修好
# 403：不知道是不是「正牌 App」
#     → 重新登入也沒用，需要其他說明
```

iOS 端也配合新增了 `RepositoryError` 的 `.appCheckFailed` case，讓它顯示和 401 不同的訊息。把狀態碼的意義切分清楚雖然是很細瑣的工作，但對於避免 client 端出現「錯誤引導」非常有效。

## Google OAuth 的同意畫面也要求「安全的流程」

在處理 ATT 的同時，我也碰到 Google 登入失敗，顯示 `錯誤 400: invalid_request` / `We cannot verify the authenticity of this app` 的情況。起初我以為是 OAuth 同意畫面的品牌驗證沒過（像是應用程式名稱不一致、首頁沒有充分說明用途），所以在網站上新增了「什麼是 MineWatch」區塊，並通過 Search Console 驗證，原以為已經解決。

但即使品牌驗證完成後，同樣的錯誤還是沒有消失。檢查 Google Cloud Console 的「專案診斷」後，發現 **「使用安全的流程」** 這一項有警告。

查閱 Google 官方文件後發現，iOS App 要在這一項被判定為「安全」，條件之一就是 **實作並強制啟用 App Check** 。也就是說，除了 Apple 的 ATT 之外， **App Check 是否正常運作，也會影響 Google 登入能不能成功** 。

在這個期間，由於我在實機上反覆重新安裝測試，App Attest 的驗證嘗試次數已經達到 Apple 的上限，導致 App Check 的 token 一直取得失敗（ `App attestation failed. Too many attempts.`）。我透過讓 Debug 版在實機上也像模擬器一樣使用 App Check Debug Provider 來避開這個問題（Release 版仍然使用真正的 App Attest，所以正式版的安全等級沒有改變）。

原本我一直以為 App Check 只是 Firebase 的功能，但這次才發現，它其實也會被 Google 的外部服務（這裡是 OAuth 同意畫面的審查邏輯）引用，是比想像中更橫向的一套機制。

## App Store Connect 的 App Privacy 申報也出了一點狀況

第三次送審最後卡住的是 App Store Connect 的 App Privacy（營養標示）申報。當我試著在二進位檔中已包含 `NSUserTrackingUsageDescription` 的情況下，申報「不會追蹤」時，ASC 跳出以下訊息阻止我：

> 應用程式包含 NSUserTrackingUsageDescription，這表示它可能會要求使用者允許追蹤。若要提交審查，請更新應用程式的隱私權回答，指出此 App 收集的資料會用於追蹤目的；或更新應用程式二進位檔並上傳新的建置版本。

前面提到的「實際上沒有追蹤，卻又跳出 ATT 對話框」這個矛盾，在這裡真的成了實際阻礙。若撤回 ATT 實作，很可能又回到 5.1.2(i) 的拒審；因此最後我選擇 **把使用狀況資料（Firebase Analytics）重新申報為「用於追蹤你的資料」** 。這個判斷是基於實務經驗：即使 SDK 構成中沒有 IDFA，Google Analytics 系列 SDK 也常常會被 Apple 判定為符合追蹤定義。

另外在申報過程中，我也發現 **當機資料不小心被勾成「用於追蹤目的」** ，所以把它取消了。當機回報和 ATT 的允許狀態無關，是獨立的開關，因此也必須和 Review Notes 裡「不屬於追蹤，因此不在 ATT 範圍內」的說明一致。

## 通過審查，以及我得到的教訓

第三次送審後終於順利通過，MineWatch 也成功上架到 App Store。

回頭看，最大的收穫是 **Apple、Google 與自家後端這三套審查／驗證機制，其實比想像中還緊密地交錯在一起** 。

- ATT 對話框的呼叫位置只差一行，就會變成只能在實機上重現的問題
- App Check 這一套機制，會同時被 Firebase Auth、自家 API、Google OAuth 三個地方參照，只要其中一處壞掉，別的地方就會以不同錯誤形式冒出來
- 「實際行為」與「申報內容」要一致，這件事不只是改程式碼就能結束

比起「做出 App」，「持續讓 App 通過審查」反而更難。上一篇文章最後那句話，在這次被拒了 3 次之後，變得更加切身有感。

---

[![:sparkles:](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png)](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png) 即使沒有經驗也能學會！一起挑戰吧 [![:sparkles:](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png)](https://cdn.qiita.com/emoji/twemoji/unicode/2728.png)

我也有在寫 note ↓

---

原文出處： [https://qiita.com/jqit-yukiono/items/91755838ea8589fde90d](https://qiita.com/jqit-yukiono/items/91755838ea8589fde90d)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aGQ2zx) [43瀏覽](https://codelove.tw/@tony/post/aGQ2zx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aGQ2zx)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 💬1 ❤️6

301

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

113

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1785117649&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaGQ2zx&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTg0IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjE4NCJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTg0Il1dLDBd&amp;dt=1785117649108&amp;bpp=1&amp;bdt=97&amp;idt=23&amp;shv=r20260721&amp;mjsv=m202607210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1785117592%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1785117592%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1785117592%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=7613510905538&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1619&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95395796%2C95396138%2C95396923%2C31099899%2C95396252&amp;oid=2&amp;pvsid=4860964164067306&amp;tmod=975530735&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1619%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=46" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260721/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>