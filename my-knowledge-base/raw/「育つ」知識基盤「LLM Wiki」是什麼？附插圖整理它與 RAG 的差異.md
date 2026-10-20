---
title: "「育つ」知識基盤「LLM Wiki」是什麼？附插圖整理它與 RAG 的差異"
source: "https://codelove.tw/@tony/post/37lGWa"
author:
published:
created: 2026-06-01
description: "![スクリーンショット 2026-05-26 9.56.48.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F435196..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月31日](https://codelove.tw/@tony/post/37lGWa "2026-05-31 08:45:04")

## 「育つ」知識基盤「LLM Wiki」是什麼？附插圖整理它與 RAG 的差異

[![スクリーンショット 2026-05-26 9.56.48.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Ffd0b4738-d0db-4a6d-a550-07948435b79d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d05328ecbb7d619ac5aa0932a795d3d9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Ffd0b4738-d0db-4a6d-a550-07948435b79d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d05328ecbb7d619ac5aa0932a795d3d9)

大約一個月前公開、引發關注的「LLM Wiki」。  
過了一段時間，我重新稍微拆解了一下，來看看它到底是什麼！

## 什麼是 LLM Wiki

[![スクリーンショット 2026-05-26 9.49.59.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Fdaabceeb-d2a3-4c21-81b3-ad6d207ef215.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2a780c84c5b0d9752d917ff5fb08f1ca)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Fdaabceeb-d2a3-4c21-81b3-ad6d207ef215.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2a780c84c5b0d9752d917ff5fb08f1ca)

- 由 Andrej Karpathy（OpenAI 創辦成員之一，Tesla 前 AI 資深總監）以 GitHub Gist 的點子文件形式公開。
	- 原始來源： [karpathy/llm-wiki.md](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- 這是一種讓 LLM 代理人持續建立與維護永久性 Markdown Wiki 的知識管理模式；不是每次都像 RAG 那樣去搜尋知識，而是將知識累積成經過編輯的百科全書。

## 與 RAG 的差異

[![スクリーンショット 2026-05-26 9.50.29.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F103ffde2-e244-4f78-b791-a8f30c3925eb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2cb55d4f861d45cec57c337eab5d4c0d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F103ffde2-e244-4f78-b791-a8f30c3925eb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2cb55d4f861d45cec57c337eab5d4c0d)

作為比較對象，文章提到了「RAG（檢索增強生成）」。  
我從「結構」與「運作」兩個角度來比較看看。

### 結構上的差異

以索引的結構來看：

- RAG ≒ 倉庫
	- 原始資料未經編輯，直接作為 I/O 使用。
		- → 容易造成各文件的結構與粒度差異很大。
- LLM Wiki ≒ 圖書館
	- 參考原始資料後，使用的是經過編輯的內容作為 I/O。
		- → 能抑制各文件的結構與粒度不一致。

LLM Wiki 的索引由 LLM **持續建立並維護** ，因此 **會不斷進化** ，這是它最大的特徵。

### 運作上的差異

如果用軟體工程來類比：

- RAG → 每次都用直譯器執行原始碼
- LLM Wiki → 先編譯一次，然後持續讓那個可執行檔成長

## 架構

[![スクリーンショット 2026-05-26 9.51.14.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Fdbfa424c-b7a4-4b13-a68b-eeed62a4ebc0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8f0e32fa9698b80ef5cdb4125d39faa6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Fdbfa424c-b7a4-4b13-a68b-eeed62a4ebc0.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=8f0e32fa9698b80ef5cdb4125d39faa6)

LLM Wiki 由三個層次構成，且各自「誰寫、誰讀」的分工非常明確。

### 第 1 層：Raw sources

- 概要：
	- 收納文章、論文、圖片等蒐集來的一次資料（原始資料）。
		- 這些資料不會被改動，是 Wiki 的根據。
- 分工：
	- 一次資料的蒐集與放置：人類
		- 一次資料的讀取：LLM

### 第 2 層：The Wiki

- 概要：
	- 由 LLM 產生的 Markdown 檔集合。
		- 包含摘要、實體頁、概念頁、比較表、總覽、整合頁等。
		- 由 LLM 完全負責，建立與更新頁面、維持交叉參照，並保持一致性。
- 分工：
	- Wiki 的管理：LLM
		- Wiki 的參照：人類

### 第 3 層：The Schema

- 概要：
	- 教 LLM Wiki 結構、規範與工作流程（匯入、問答、維護）的文件。
		- 例如： [CLAUDE.md](http://claude.md/) （給 Claude Code 用）、 [AGENTS.md](http://agents.md/) （給 Codex 用）、...
- 分工：
	- 整體管理：人類、LLM

## 作業流程

LLM Wiki 主要透過三種操作來運作。

### ① Ingest

[![スクリーンショット 2026-05-26 11.09.17.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F5fef42ef-8d11-4991-9d08-16b0f1aa00a8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1b49b3ecd65811cb160524fa50a53926)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F5fef42ef-8d11-4991-9d08-16b0f1aa00a8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=1b49b3ecd65811cb160524fa50a53926)

- 每匯入一份新資料，就會影響 Wiki 的 10～15 個頁面。
- LLM 不會只是把它當成搜尋對象，而是會讀取內容、擷取重要資訊，並整合進既有的 Wiki。

### ② Query

[![スクリーンショット 2026-05-26 11.10.15.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F88f5177d-d213-4ec7-a837-336b5448bf06.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3bfee77468ef46de62ec5aa2774a1b77)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F88f5177d-d213-4ec7-a837-336b5448bf06.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=3bfee77468ef46de62ec5aa2774a1b77)

- 關鍵是名為 `index.md` 的檔案（※詳情後述），它是所有 Wiki 頁面的目錄（附有一行摘要、按類別整理）。
- 當提出問題時，LLM 會先讀取 index，找出相關頁面，再深入閱讀並整合答案。
- 這個系統的特色是，不需要 embedding 基礎設施也能運作。具價值的答案會被「歸檔」成新的 Wiki 頁面。

### ③ Lint

[![スクリーンショット 2026-05-26 11.10.25.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F7b2c39fa-a894-4331-a28e-4fff84ecc43e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c7523f573997a44a3868872ba4f72ced)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F7b2c39fa-a894-4331-a28e-4fff84ecc43e.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=c7523f573997a44a3868872ba4f72ced)

- 透過定期健康檢查，修正矛盾與孤立頁面。
- 定期找出矛盾、過時的主張、孤立頁面、缺少的交叉引用，以及需要額外調查的資料缺口等問題。

## 索引與日誌

隨著 Wiki 成長，有兩個特殊檔案能幫助 LLM（以及人類）瀏覽整個 Wiki。

### index.md

[![スクリーンショット 2026-05-26 9.52.57.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F73a75ee6-118f-499b-af64-8a06c6b10f22.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=88c34f64bbf516de00d36d4c2ad13bd7)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F73a75ee6-118f-499b-af64-8a06c6b10f22.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=88c34f64bbf516de00d36d4c2ad13bd7)

- 包含每個頁面的連結、一行摘要，以及（可選）日期、來源數等中繼資料的 Wiki 全頁目錄。
- 依類別（實體、概念、來源、...）整理。
	- 每次匯入（Ingest）時由 LLM 更新。
		- 在問答（Query）時，LLM 會先讀取這個 `index.md` 找出相關頁面，再深入查閱。

### log.md

[![スクリーンショット 2026-05-26 9.53.28.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Fd192fd06-678a-4b8b-996e-0486dc28dace.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=db75f59270b20078ec8342585d3faac3)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2Fd192fd06-678a-4b8b-996e-0486dc28dace.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=db75f59270b20078ec8342585d3faac3)

- 以追加寫入（append-only）方式記錄「什麼時候發生了什麼事」的檔案。
- 匯入（ingest）、問答（query）、lint 等所有事件都會按時間順序留下紀錄。
	- 若讓每筆記錄都以一致的前綴開頭，就能像 `grep "^## \[" log.md | tail -5` 這樣，用簡單的 Unix 工具解析。
		- 前綴範例： `## [2026-04-02] ingest | 文章標題`
		- 這能讓人掌握 Wiki 演進的時間線，也能幫助 LLM 了解「最近做了什麼」。

## 使用的工具

[![スクリーンショット 2026-05-26 9.53.52.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F7b3f1b5a-ed76-4323-a3e4-a85d0a8c0bdd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=36f57b0bc7e5c54a22cbafaaa7e801d4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F4351963%2F7b3f1b5a-ed76-4323-a3e4-a85d0a8c0bdd.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=36f57b0bc7e5c54a22cbafaaa7e801d4)

- Wiki 檢視器
	- Obsidian、VS Code、...
- LLM 代理人
	- Claude Code、Codex、...

## 結論：什麼是 LLM Wiki

**這是一個會越用越聰明、越整理越有價值的「活的知識庫」。**

### RAG 的適用情境

**【觀點】**

| 觀點 | 適合 RAG | 適合 LLM Wiki |
| --- | --- | --- |
| 資料更新頻率 | 高 | 中～低 |
| 資料規模 | 數千～數百萬 | 數百 |
| 知識性質 | 事實查詢 | 概念理解、整合 |
| 想要的答案 | 「原始資料裡寫了什麼」 | 「綜合多份資料後，可以得出什麼結論」 |

**【具體例子】**

| 具體例子 | RAG or LLM Wiki |
| --- | --- |
| 個人學習・研究 | LLM Wiki |
| 企業內部文件搜尋 | RAG |
| FAQ | RAG |

## 正在尋找一起思考新技術社會實作的夥伴

感謝你讀到最後！  
我想我們大概很合拍。

目前我們公司正在招募能一起寫程式、一起抓頭苦思、成功時也能彼此喝采的工程師。  
就算只是「想先聊聊看」也完全沒問題！我們反而非常歡迎。

→ [Sapeet 招募資訊](https://sapeet.notion.site/recruit)

---

原文出處： [https://qiita.com/shinnosuke\_takami/items/86307593829ac5e70852](https://qiita.com/shinnosuke_takami/items/86307593829ac5e70852)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37lGWa) [5瀏覽](https://codelove.tw/@tony/post/37lGWa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37lGWa)

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

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝12 💬4 ❤️1

465

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

87

🥉

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

#4

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780297291&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37lGWa&amp;pra=5&amp;asro=0&amp;itsi=8&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTgxIixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xODEiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE4MSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780297291838&amp;bpp=1&amp;bdt=577&amp;idt=113&amp;shv=r20260527&amp;mjsv=m202605260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780297054%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780297054%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780297054%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3306164503434&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31098652%2C95390667%2C42533294%2C95391807&amp;oid=2&amp;pvsid=5036298640939611&amp;tmod=1412125679&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=136" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260527/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>