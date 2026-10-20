---
title: "我是如何讓 Rust 熱路徑快 27 倍，以及我拒絕合併的 AI 修正"
source: "https://codelove.tw/@tony/post/37ZWgx"
author:
published:
created: 2026-07-20
description: "兩年前，我把 KeyEcho 開源了。那是一個小型桌面應用程式，當你按下按鍵的瞬間，就會播放機械鍵盤的聲音。它拿到了 800 多顆星。之後我在 2024 年 7 月發佈了 v0.0.5，接著就沉寂了。問題從未停止。大家開始詢問音效包、回報平台 bug，並持續使用這個我已經停止維護的工具。這個月，..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月20日](https://codelove.tw/@tony/post/37ZWgx "2026-07-20 12:03:19")

## 我是如何讓 Rust 熱路徑快 27 倍，以及我拒絕合併的 AI 修正

兩年前，我把 KeyEcho 開源了。那是一個小型桌面應用程式，當你按下按鍵的瞬間，就會播放機械鍵盤的聲音。它拿到了 800 多顆星。之後我在 2024 年 7 月發佈了 v0.0.5，接著就沉寂了。

問題從未停止。大家開始詢問音效包、回報平台 bug，並持續使用這個我已經停止維護的工具。這個月，我回來並在一個 PR 中直接推出了 1.0：130 個檔案、增加 11,405 行、刪除 9,292 行。核心是把音訊熱路徑整個重建。快取查找的微基準測試從每次操作 1184.07 ns、每次按鍵複製 66.84 KiB，降到 43.50 ns、且不複製任何取樣位元組。平均切片快了 27 倍，最大切片快了 38 倍。

我是在 AI agent 寫了大量程式碼的情況下完成這件事的。這篇文章會說明這次重構是怎麼運作的、為什麼 Rust 讓它能又快又安全地做到，以及 agent 建議的一個看起來很乾淨、卻會悄悄破壞功能的改動。

## 熱路徑

KeyEcho 對延遲敏感的路徑很短。全域鍵盤 hook 會在每次按鍵事件時觸發。每次按下的第一個 key-down 事件會被送進有界佇列。音訊執行緒會從佇列取出資料，把按鍵映射到所選音效包中的某個切片，然後在本機播放。

因為這條路徑很短，任何留在其中的工作都會在每次按鍵時付出代價：任何解碼、任何配置、任何取樣複製、任何鎖。整個重點就是把這些東西從每次按鍵的路徑中移除。

## v0.0.5 本來就很快

先說清楚這點，v0.0.5 確實不錯：Tauri + Rust、建置小、記憶體用量低；我手寫了各平台原生鍵盤監聽，幾乎沒有使用 unsafe；使用無損 WAV 音訊，沒有解壓步驟；而且已經在 LRU 中快取了解碼後的音訊，所以同一個按鍵按兩次不會解碼兩次。它跑了兩年，也有幾百人使用。這不是慢軟體。

但「已經很快」和「沒有進一步優化空間」是兩回事。即使是快取命中，每次按鍵仍然會複製取樣（在基準測試中是 66.84 KiB），而且會經過一個與音效包切換和音量變更共用的全域 mutex。快取未命中時，還是會當場解碼。1.0 移除的正是這些：那個複製、那把鎖，以及快取未命中這件事本身。

## 重建

這次重建主要靠四個動作。

**在選取音效包時一次性預先解碼。** 音效包是一個資料夾，裡面有 `sound.ogg` 和 `config.json` 。設定檔會把每個按鍵對應到音訊切片： `key -> [start_ms, duration_ms]` 。當你選擇某個音效包時，所有切片都會一次預先解碼。按鍵時不再進行任何解碼。

**去重相同切片。** 很多按鍵會指向同一個切片，所以如果每個按鍵都各自解碼，會重複解同一段音訊很多次。建構器會用 `(start_ms, duration_ms)` 這組值當作 map 的 key，把每個唯一切片只解碼一次，然後共用它：

```rust
// 每個唯一切片只解碼一次；相同切片共用同一個緩衝區。
let mut slice_sources: HashMap<[u64; 2], AudioSource> = HashMap::new();
let mut key_sources: HashMap<Key, AudioSource> = HashMap::new();

for (key, slice) in defines {
    let source = slice_sources
        .entry(slice)
        .or_insert_with(|| {
            let [start_ms, duration_ms] = slice;
            let samples = decode(start_ms, duration_ms); // Vec<f32>，只做一次
            AudioSource::new(Arc::from(samples), channels, sample_rate)
        })
        .clone(); // Arc 複製，不是取樣複製
    key_sources.insert(key, source);
}
```

**零拷貝播放。** 解碼後的取樣會存在 `Arc<[f32]>` 中。查找按鍵時只會複製這個 Arc，也就是引用計數加一，而不是複製取樣。每次按鍵的複製成本變成 0 位元組。

**移除全域 mutex。** 舊路徑用 mutex 保護目前音效與音量。新的播放 handle 把目前音效放在 `ArcSwapOption<KeySound>` 中，把音量放在 `AtomicU32` 中（透過 `f32::to_bits` ）。查找按鍵時是無鎖讀取：

```rust
struct PlaybackSoundpack {
    current_sound: Arc<ArcSwapOption<KeySound>>,
    volume_bits: Arc<AtomicU32>,
}

fn source_for_key(&self, key: Key) -> Option<(AudioSource, f32)> {
    let sound = self.current_sound.load();          // 無鎖
    let source = sound.as_ref()?.key_source(key)?;  // map 查找 + Arc 複製
    let volume = f32::from_bits(self.volume_bits.load(Ordering::Relaxed));
    Some((source, volume))
}
```

預先解碼是把前置工作換成記憶體用量，所以需要兩道保護。鍵盤事件會經過有界佇列，因此短時間大量輸入會形成反壓，而不會讓記憶體無限制成長。另外，一個音效包必須符合 10 MiB 的已解碼取樣預算：在載入前，程式會根據唯一切片的持續時間估算解碼後大小，若超過就拒絕載入。只有在有界的情況下，預先解碼才是安全的。

## 資料

release build 下，查找路徑的微基準測試如下：

| 路徑 | v0.0.5 | v1.0 | 變化 |
| --- | --- | --- | --- |
| 快取查找，平均切片 | 1184.07 ns/op；複製 66.84 KiB | 43.50 ns/op；複製 0 位元組 | 快 27.2 倍 |
| 快取查找，最大切片 | 1638.57 ns/op；複製 98.88 KiB | 43.10 ns/op；複製 0 位元組 | 快 38.0 倍 |
| 按下/放開門檻 | 58.82 ns/tap；2 則訊息 | 54.69 ns/tap；1 則訊息 | 訊息減半 |

這些是查找路徑的微基準測試，不是端到端的喇叭延遲；後者取決於你的作業系統與硬體。方法與記憶體預算寫在 `docs/performance.md` ，基準測試也隨 repo 一起發布。如果你不相信我，可以執行 `pnpm run bench:audio` 。

## 為什麼 Rust 讓這件事能又快又安全

這次 diff 的大部分，我都交給 agent 來做。之所以覺得安全，是因為有編譯器。

借用檢查器與型別系統，是 AI 產生程式碼的第一道審查。大多數錯誤程式碼都過不了 `cargo check` 。把共享音訊緩衝區改成 `Arc<[f32]>` 、並移除 mutex，正是那種一旦出錯就會變成別名問題或 `Send` / `Sync` 錯誤的改動；而 Rust 會在編譯階段就把這些拒絕掉，甚至還沒到人工審查或使用者手上。agent 參與的 diff 越多，這種防線就越有價值。

## agent 實際做了什麼

不是「它寫了整個 app」。真正有用的工作更窄，也老實說更有價值。

- **遷移助理。** Tauri 1 升到 2 會碰到設定、權限、更新器，以及每個 plugin 邊界。能讀完所有遷移文件的夥伴，真的能省下不少時間。
- **熱路徑稽核員。** 它跟我逐行檢視舊的播放路徑，並推動了上面提到的預先解碼、共享緩衝區、無 mutex 重構。
- **待辦清單分流。** 我讓它讀每一個 open issue，並依照哪些屬於 1.0 來排序。其中一個是 10 個月前的需求，來自一位曾表示願意為特定音效付費的人。等到 1.0 後我回覆他，結果變成這個專案的第一位付費客戶。我原本已經不再看待辦清單了，但它其實就是需求。
- **基準測試與文件紀律。** 它讓效能說明保持結論先行、可重現，而且誠實說明這些數字代表什麼、不代表什麼。

我很少下逐步指令。我給的是目標（像是「把按鍵路徑中的樣本複製降到 0」、「把待辦清單按屬於 1.0 的內容排序」），然後在檢查點審核。速度提升本身來自重構：預先解碼、共享緩衝區、移除 mutex、有界佇列。agent 則是讓這整個找出、執行、驗證的流程快到真的能完成。

## 那個看起來很乾淨、但其實不行的改動

這個我希望你學起來。

Linux armv7 的 CI 建置在 QEMU 下失敗了：libgit2 無法抓取 git 依賴。agent 提出的修正很乾淨：把音訊 crate 上的 git pin 拿掉，改用 [crates.io](http://crates.io/) 上的 release。CI 變綠了。

但我拒絕了。那個 pin 的存在有其理由，而且程式碼裡哪裡都沒寫。它把 `cpal` 鎖在 0.18，而這個版本提供預設裝置重新導向（以及 `DeviceChanged` 通知），讓你拔掉耳機或切換到藍牙喇叭時，聲音會跟著切過去。改回 [crates.io](http://crates.io/) 版本會悄悄把 `cpal` 降到 0.17.3，並失去裝置跟隨功能。兩個使用者問題，#41 和 #20，談的就是這個行為。沒有任何測試涵蓋「拔掉裝置後聲音仍能跟著走」，所以如果不是知道那個 pin 為什麼存在，根本不會有人發現這個 regression。

真正的修正是用 `cargo vendor` 把相依套件供應到本機，然後在 QEMU 內離線建置。那個 pin 從頭到尾都沒動。

我現在把兩件事當成規則：

1. 每個 pin、hack、魔術數字旁邊都要寫上「為什麼」，可以寫在註解裡或專案的 AI 規則檔中。你的 agent 不記得當初為什麼把它放在那裡。它是個很會做事的工程師，但對你的專案毫無上下文。
2. 不要接受你沒有要求的相依版本變更，即使 CI 變綠也不行。綠燈只能證明測試通過，不能證明測試涵蓋了你即將失去的功能。

## 我刪掉了什麼

armv7 的建置在模擬器下大約要跑兩個小時，而且看不出有清楚的 32 位元 ARM Linux 使用者。我把它從 1.0 中移除了。

agent 不會有沉沒成本或機會成本的概念。只要你放任它，它會永遠持續最佳化某個建置目標。它可以無限接近「完成」。決定何時停止，這是人的工作。

## 這次工作是怎麼組織的

再補充兩個這次發佈時的小觀察。

正式發佈前，我在 git worktree 裡跑了 AI 審查流程，這樣多個 agent 就能平行掃描程式碼並提出修正，而不會碰到主樹。每個 agent 的輸出都只是提案，必須經過審查後才能合併。探索可以很乾淨地平行化，但整合仍然必須在一個人腦中完成，因為每個 agent 只看得到自己那一小塊。

模型也開始分工了。Claude Fable 給我的感覺像一位會跳脫框架思考的工程師，常常能帶出我忽略的角度。GPT-5.6 是我用過最強的執行者：你只要把已決定好的計畫交給它，回來的程式碼幾乎完美。整體感覺就像在帶一個兩人團隊：一個負責思考，一個負責實作，而我負責決策與簽核。

## 1.0 是什麼

還是免費，還是開源。AGPL-3.0、精簡的原生安裝程式、沒有帳號、沒有分析追蹤。基於 Tauri 2 + SolidJS 重建。Windows 安裝檔有簽章，macOS 安裝檔有公證，減少了未簽章應用程式警告和防毒軟體誤判。Linux 提供 x64 與 ARM64 套件。

你甚至不用下載任何東西就能試：打開 [keyecho.app](https://keyecho.app/) 並開始打字，你就會在瀏覽器裡聽到聲音。基準測試與方法都在 repo 裡。

我每週會寫一篇建置日誌，記錄如何用 AI agents 交付真實的 production system，只講真實資料。歡迎訂閱 [upweb.dev](https://upweb.dev/) 。

---

原文出處： [https://dev.to/zacharylee/how-i-made-a-rust-hot-path-27x-faster-and-the-ai-fix-i-refused-to-merge-3llg](https://dev.to/zacharylee/how-i-made-a-rust-hot-path-27x-faster-and-the-ai-fix-i-refused-to-merge-3llg)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37ZWgx) [3瀏覽](https://codelove.tw/@tony/post/37ZWgx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37ZWgx)

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

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

🔥 [首次亮相 NVIDIA Jetson Orin Nano Super - 最經濟實惠的生成式 AI 超級計算機](https://codelove.tw/@tony/post/qvpzV3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝7 💬2 ❤️4

224

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

109

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784525139&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37ZWgx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI1Il1dLDBd&amp;dt=1784525139962&amp;bpp=1&amp;bdt=125&amp;idt=16&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784525095%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784525095%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784525095%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1973616155946&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=31099679%2C95396104%2C95396567%2C95396252%2C95395499&amp;oid=2&amp;pvsid=8746678794859696&amp;tmod=2019455844&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1533%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=33" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260716/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>