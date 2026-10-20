---
title: "Fable 5 的消耗太大了，所以我試著用 Claude Code 做出一套只用於設計和審查的機制"
source: "https://codelove.tw/@tony/post/x0nYDa#google_vignette"
author:
published:
created: 2026-07-07
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------平常我都沒多想，直接用 Opus 跑 Claude Code，但切換到期間限定可用的 Fable 5 之後，**tok..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月04日](https://codelove.tw/@tony/post/x0nYDa "2026-07-04 08:02:53")

## Fable 5 的消耗太大了，所以我試著用 Claude Code 做出一套只用於設計和審查的機制

## 前言

平常我都沒多想，直接用 Opus 跑 Claude Code，但切換到期間限定可用的 Fable 5 之後， **token 的消耗比想像中還要兇猛** 。

因此，我改成 **只在設計、審查等重要度較高的部分使用 Fable，其餘部分則交給 Opus** 。  
重點是「不要在 skill 裡寫模型名稱」。  
只要這樣做，即使 Fable 的使用期間結束，也能很容易切換，維護性也會提升。

這次就來整理一下這個設定方法。

## 考量方式

這次我打算把 subagent 分成 **judge（審查／驗證）/ builder（實作）/ worker（調查／擷取）三種角色 + 主 session** ，並依照角色分配不同的模型。

| 層級 | 負責模型 | 主要工作 |
| --- | --- | --- |
| 主體 + `judge` | Fable | 設計、規劃、審查、驗證（ **誤判會導致事故的部分** ） |
| `builder` / `worker` | Opus | 實作、調查、全面擷取（ **大量處理是瓶頸的部分** ） |

大部分 token 都會消耗在平行展開的調查類管線與實作上，所以只要把那一側交給 Opus，就能讓 Fable 的消耗集中在「思考、判斷」的部分。

## 設定方法

### 1\. 在.claude/agents/ 定義角色

每個角色只要放一個 md 檔案即可。frontmatter 裡的 `model:` 就是模型設定的來源。

```
<!-- .claude/agents/worker.md -->
---
name: worker
description: 負責調查與全面擷取的執行角色
model: opus
---
忠實遵循收到的指示，並為事實附上根據（file:line）後回傳。
```

`builder.md` （實作角色，model: opus）也用相同格式建立。

這次只有 `judge.md` **沒有寫 `model:`**。  
未指定模型的 agent 會繼承主 session 的模型，因此我讓它能在切換 `/model` 時，judge 也自動跟著變更。

### 2\. skill 端只寫角色名稱

```
並列 dispatch 各觀點（subagent_type: worker）
並列啟動 3 個審查者（subagent_type: judge）
啟動實作 agent（subagent_type: builder）
```

skill 裡完全不需要知道模型名稱。

### 3\. 切換模型

只要 **修改各個 md 的 `model:`** 就能切換模型。  
另外，主體 + judge 也會透過 `/model` 指令切換。

## 優點

- **維護性** ：模型名稱不會散落在 skill 裡，所以不管增加多少條 skill，模型變更都只要 1～3 個地方就能完成
- **通用性** ：因為是「判斷管線與大量處理管線之間拉開一級差距」這種相對規則，所以無論是 fable+opus+sonnet，還是下一代模型+opus+sonnet，都能沿用同樣的設計。

## 總結

這次是為了更有效率地運用 Fable 而做的設定，但我也覺得，依角色來分配模型的機制本身，在未來持續運用 AI 的過程中，先設好絕對不吃虧。

如果你想用 AI 大量進行多工處理，或正在為 Fable 的 token 消耗而煩惱，不妨試試看。

---

## 參考

---

在株式會社 Cynthia，我們會招募沒有實務經驗的工程師，以及學生工程師實習生，並一起工作。  
※ 關於 Cynthia 的工作方式，可以參考這裡：

在 Cynthia，每年大約有 100 位沒有實務經驗的人提出申請並參加技術面試。透過這些經驗，這裡會介紹希望實務未經驗者務必掌握的技術力（語法）。

---

原文出處： [https://qiita.com/fwatanab/items/e2c51c2a783915b275a9](https://qiita.com/fwatanab/items/e2c51c2a783915b275a9)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x0nYDa) [38瀏覽](https://codelove.tw/@tony/post/x0nYDa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x0nYDa)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783386011&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx0nYDa&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783386011667&amp;bpp=1&amp;bdt=68&amp;idt=41&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4243076030595&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395662%2C42533293%2C95390278%2C95393485&amp;oid=2&amp;pvsid=7685327214564039&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=52" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>