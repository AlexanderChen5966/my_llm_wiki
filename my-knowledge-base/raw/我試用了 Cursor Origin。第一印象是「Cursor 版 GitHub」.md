---
title: "我試用了 Cursor Origin。第一印象是「Cursor 版 GitHub」"
source: "https://codelove.tw/@tony/post/anbkka"
author:
published:
created: 2026-08-20
description: "Cursor Origin 是 Cursor 提供的 Git 託管服務。把儲存庫放上去、進行審查、再合併。它的角色和 GitHub、GitLab 一樣。官方標語是「A git forge for the agentic era」。它不是以人類一次一個 Pull Request 的速度來設計，而是以代..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月19日](https://codelove.tw/@tony/post/anbkka "2026-08-19 17:15:09")

Cursor Origin 是 Cursor 提供的 Git 託管服務。把儲存庫放上去、進行審查、再合併。它的角色和 GitHub、GitLab 一樣。官方標語是「A git forge for the agentic era」。它不是以人類一次一個 Pull Request 的速度來設計，而是以代理程式能在短時間內產生大量分支與差異為前提。

Cursor Ambassador 有稍微提早釋出，所以我立刻試用了。而在我撰寫本文的 2026 年 8 月 17 日， [初期 Beta 已開始依序提供給所有付費方案](https://cursor.com/ja/changelog/origin-code-hosting) 。除了管理員選擇退出的企業組織外，只要是付費方案（Pro / Teams / Enterprise）就已經可以使用。採用舊版隱私模式的團隊，在切換到新的隱私模式之前無法啟用。 [官方文件](https://cursor.com/ja/docs/origin) 也已公開。這是把既有 GitHub 儲存庫搬到 Origin 上後所理解到的事情之上篇。我會在這篇寫 Origin 是什麼、跟 GitHub 哪些相同、哪些不同，並且整理我能從畫面與 CLI 確認的範圍。Pull Request 的內部模型、remote 名稱的陷阱，以及 CI 和 ruleset 的組法，我分到後篇。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787188793&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbkka&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187582602&amp;bpp=7&amp;bdt=986&amp;idt=8&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787188505%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=6327086646475&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1055&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C122880370%2C42533294&amp;oid=2&amp;pvsid=937286878745343&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

後篇在這裡： [https://qiita.com/Kinopee/items/747de22525344a796eed](https://qiita.com/Kinopee/items/747de22525344a796eed)

我試用的時間是 2026 年 8 月 17 日晚上。剛好 GitHub 正在發生障礙。Web 畫面回傳了「We couldn't respond to your request in time.」，我在碰到的第一天，就親身體會到多一個 push 目的地的可貴。這部分我會留到後半再說。

[![Origin 的空儲存庫畫面](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4428217%2F5fb671b4-37cc-492f-8a6b-6306333dd7f4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d49757abaa8ed056b3ecb2dc86d02f64)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4428217%2F5fb671b4-37cc-492f-8a6b-6306333dd7f4.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d49757abaa8ed056b3ecb2dc86d02f64)

## 這是做什麼的地方

很粗略地說，就是用 Cursor 帳號登入的 GitHub。主機是 `origin.cursor.com` 。Git 操作本身還是一般的 Git， `commit` / `branch` / `push` / `pull` / `clone` 都照常可用。

設定只要兩行。不需要 GitHub 的 PAT，也不需要 `gh auth` 。

```
curl -fsSL https://downloads.cursor.com/origin/install.sh | sh
origin auth login
```

`origin auth login` 會進行認證，並替 `origin.cursor.com` 設定 git 的 credential helper。之後的 `git push` / `git pull` 就不需要手動輸入另一組 token。CLI 支援 macOS 與 Linux（包含 WSL）。原生 Windows 目前還不能用（ [CLI 文件](https://cursor.com/ja/docs/origin/cli) ）。

容易混淆的是，CLI 的名字也叫 `origin` 。它和 git 裡慣例使用的 remote 名稱 `origin` 是不同東西，請注意分辨。

儲存庫與 Pull Request 的管理，改用 `origin` 取代 `gh` 。

```
origin repo list                 # 儲存庫列表
origin repo create <name>        # 建立
origin repo clone owner/repo     # clone
origin pr create                 # 建立 PR（change）
origin pr merge --auto           # 條件齊備時自動合併
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787188793&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbkka&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187582620&amp;bpp=1&amp;bdt=1004&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787188505%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=6327086646475&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2886&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C122880370%2C42533294&amp;oid=2&amp;pvsid=937286878745343&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

把既有 GitHub 儲存庫搬上來，也只是新增一個 remote 然後 push 而已。既有的 `origin` （GitHub）不要動，改用別名新增會比較安全。理由和坑洞我會寫在後篇。

另外，正式版也加入了 GitHub 同步。當你匯入 GitHub 儲存庫後，它會作為同步儲存庫即時更新，PR 評論也會雙向同步。不過這種情況下 GitHub 仍然是正本，push 依舊會送往 GitHub。CLI 裡有 `origin repo create-mirrored` 。

```
git remote add cursor https://origin.cursor.com/owner/repo.git
git push cursor main
```

## 跟 GitHub 哪些相同、哪些不同

相同的是 Git 的內容。commit、branch、tag 都會原封不動帶上去。不同的是外圍。

項目GitHubOrigin主機github.comorigin.cursor.com認證GitHub 帳號 / `gh` Cursor 帳號 / `origin` CLI公開儲存庫可以不行（只有團隊內的 Internal / Private）Issues / Actions / Releases有主機端沒有。CI 需透過 Apps 外掛連接Pull Request新建時通常是 open稱為 change，預設是 draft。具有版本（version）

最讓我驚訝的是不能做公開儲存庫。可見範圍只有兩種：團隊全員都能看見的 Internal，以及只有被允許成員可見的 Private，沒有對網際網路公開的選項。現在還不能當作 OSS 的發布場所。

[![Settings 的 General](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4428217%2F2ff2fb78-7713-4abf-9aad-fb47f007ee0f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c8f3114240f285b3cfb05b7dea89b7dc)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4428217%2F2ff2fb78-7713-4abf-9aad-fb47f007ee0f.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c8f3114240f285b3cfb05b7dea89b7dc)

像 Issues 或 Actions 執行紀錄這類「主機端資料」，不會因為 git push 就跟著移過去。`.github/workflows` 的 YAML 仍然會留在儲存庫中，但不代表 Origin 上會執行 GitHub Actions。CI 是透過名為 Apps 的機制，串接外部服務（Buildkite、Depot、Vercel）。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787188904&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbkka&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187582647&amp;bpp=1&amp;bdt=1037&amp;idt=0&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787188505%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x271%2C334x250%2C952x280&amp;nras=9&amp;correlator=6327086646475&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4815&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=1221&amp;eid=95397679%2C95399754%2C122880370%2C42533294&amp;oid=2&amp;psts=AOrYGsmokK1yCOLMoNBR8hOZGPi297Haly3KbUlLpx024qXTVfqBTV__3UnJzscY2jzh23-72jcv3N3710EokMSEqqp_igISoKV8zzzpc3PF6S04zgnDJ3Qkuw9ghNVpx0K6%2CAOrYGslisvBgPnpQayQ6ZCCSQAkP6EkwefBx-b0VLS2coWAZBY-VuLd-rJsO8ZtIkTyDWTfWD_l1WP9d7UN4senooEmmoeRLdwHAB5VUIA3yKz-C4Lo%2CAOrYGslncp3ggT1_sgsCrhgCYZAMisTNgysQx7PZCpckSrj6LnSIifQVqGdeHabC60KASR7aTbAwR6S99PN4SlsciPkA3AGkeaYLIvWqve5_RdwzI3I%2CAOrYGskbQSwKHpMQiKp6DJ8EpM3ux6aHoeg5MRKJNBMpD05Att7CPztZye9i974-88ypqOS2k26xgOwdZ1sUJ73ehiSXOoaBvsjiRKmSkA&amp;pvsid=937286878745343&amp;tmod=500019996&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1957%2C30%2C1957%2C30%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Pull Request 雖然外觀很接近，但內部模型不同。Origin 的單位是稱為 change、帶有版本（version）的差異集。GitHub 的 Pull Request 會隨著 commit 累積而內容持續變動。當代理程式在短時間內反覆修正時，審查者就得追著不斷變動的 diff 走。Origin 會在每次 push 時，自動把當下的差異記錄成一個 version。審查可以固定「看的是哪一版」，而且之後承認的對象不會再變動。新建 change 的預設是 draft，以及可以堆疊小型 change 的 stacked diffs 選項，也都是同樣的設計思路。這是 Origin 最有趣的部分，我會在後篇用圖解來說明。

## 能不能當成 GitHub 當機時的保險

障礙的頻率，不只是感覺而已。那晚的障礙若從 [GitHub Status](https://www.githubstatus.com/) 追蹤，可以看到從 Issues 與 Pull Requests 劣化開始，接著擴散到 Actions、Webhooks、API，甚至 Git 操作。8 月 6 日時，Actions 劣化約 9 小時，尖峰時有 71% 的 workflow 因基礎設施因素失敗。這是連 GitHub 自己都在 [每月可用性報告](https://github.blog/news-insights/company-news/github-availability-report-july-2026/) 裡寫成 unacceptable（無法接受）的規模。 [外部統計](https://blog.incidenthub.cloud/github-reliability-outage-history-2025-2026) 則顯示，最近一年被列為 major 的障礙有 48 件。自 2025 年 12 月以來，發生頻率還提高了。

git 是分散式的，所以本機上的 commit 在障礙期間仍然可以繼續進行。停住的是匯集到主機的部分，也就是共享、審查與 CI。當有兩個 remote 時，可以把其中的共享先導走。即使 GitHub 那邊掛掉了， `git push cursor main` 還是能成功，其他機器或團隊成員也能從 Origin 做 pull。這次我在 GitHub 回報 Pull Requests 劣化時，正好把 `main` push 到 Origin。Origin 端的 push 正常通過了。

要注意的是，正式版的 GitHub 同步（鏡像）不能直接當作避難所。它會在 Origin 端建立一份可讀的副本，但 push 的設計仍然是透過 GitHub，所以 GitHub 掛掉時，push 也會一起停住。若你在障礙時需要一個可 push 的目的地，就要像本文這樣，另外持有一個 Cursor 主機上的儲存庫作為不同 remote。順帶一提， [官方文件](https://cursor.com/ja/docs/origin/mirror-github) 裡有 Detach from GitHub，可以把同步解除，將 Origin 的副本轉成獨立的 Origin 主機儲存庫。作為 GitHub 長期障礙或移轉的最後手段，官方確實有預留「鏡像 → 切離」這條路徑（但我未驗證）。

即使如此，能避難的也只有 repo 層。Issue、Pull Request 討論和 CI 都不會跟著故障轉移。Origin 本身的實際運作紀錄仍然不多，也沒有公開 SLA。以今天來說，能確定的只有：多一個 push 目的地，可以減少共享停擺的時間，而且增加這個備援只要一條指令。

Cursor 表示，Origin 的設計是以代理程式會大量 clone、branch、commit 的負載為前提。能承受那種負載的設計，理應也會和不容易停擺的設計相互重疊。每次 GitHub 出問題就讓工作停擺，根本原因之一也是託管服務過度集中在同一家。對我來說，能多一個選擇這件事本身就很有價值。

## 目前我會怎麼用

我目前仍然把 GitHub 當正本。公開、CI、既有 Pull Request 都還在 GitHub 端，Origin 只是 Cursor 的備用，以及 GitHub 故障夜晚的避難所。要不要把正本移過去，等 Origin 的實際運作紀錄更清楚後再說也不遲。

要把它加上去本身並不難。先知道可以少踩坑的重點有兩個：不能公開，以及 `origin` 這個 remote 名稱會衝突。先理解這兩點之後，後篇我會寫 change 模型和 remote 名稱的陷阱。

後篇在這裡： [https://qiita.com/Kinopee/items/747de22525344a796eed](https://qiita.com/Kinopee/items/747de22525344a796eed)

---

原文出處： [https://qiita.com/Kinopee/items/639d401573c09ab24667](https://qiita.com/Kinopee/items/639d401573c09ab24667)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/anbkka) [14瀏覽](https://codelove.tw/@tony/post/anbkka)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/anbkka)

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

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝12 💬4 ❤️2

297

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

7

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=3620188380&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787188793&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbkka&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187582788&amp;bpp=1&amp;bdt=1171&amp;idt=1&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787188505%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787188505%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=6327086646475&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1167&amp;ady=1079&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C122880370%2C42533294&amp;oid=2&amp;pvsid=937286878745343&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=9&amp;uci=a!9&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787187581&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbkka&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTY5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE2OSJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE2OSJdXSwwXQ..&amp;dt=1787187581813&amp;bpp=1&amp;bdt=135&amp;idt=78&amp;shv=r20260818&amp;mjsv=m202608140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787187568%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787187568%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6327086646475&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95399754%2C122880370%2C42533294&amp;oid=2&amp;pvsid=937286878745343&amp;tmod=500019996&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=137" title="Advertisement" aria-label="Advertisement"></iframe>