---
title: "Claude Code 的快取，只會套用在使用工具的回合中"
source: "https://codelove.tw/@tony/post/qEWjX3"
author:
published:
created: 2026-09-07
description: "9月2日我寫了「**Claude Fable 5.1 的第 2 回合有時會命中快取、有時不會**」。測了 18 次、13 次沒命中，**原因不明。**隔天 9 月 3 日的 **2.1.260**，Fable 5.1 的快取修正進了 3 個。重新測試後如下：```2.1.260 / Fab..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月05日](https://codelove.tw/@tony/post/qEWjX3 "2026-09-05 08:09:36")

## Claude Code 的快取，只會套用在使用工具的回合中

9月2日我寫了「 **Claude Fable 5.1 的第 2 回合有時會命中快取、有時不會** 」。測了 18 次、13 次沒命中， **原因不明。**

隔天 9 月 3 日的 **2.1.260** ，Fable 5.1 的快取修正進了 3 個。重新測試後如下：

```
2.1.260 / Fable 5.1  第 2 回合命中率

使用工具      99.7  99.7  99.7  99.7  99.7  99.7      6 次全都命中
不使用工具    38.6  38.6  38.6  38.6  38.6  99.7 …    9 次中 8 次沒命中
```

**我先前測的，不在這次修正的範圍內。**

> 驗證環境：Windows 10 / Claude Code 2.1.260 / 測量時間：2026-09-04

---

## 什麼被修正了

這是發行說明的內容：

> Fixed prompt caching on Claude Fable 5.1 **not covering the context attached after tool results**, so it was re-sent as uncached input on every tool-call turn  
> （修正了 Fable 5.1 的提示快取 **沒有涵蓋在工具結果之後附加的 context** ，導致每次工具呼叫回合都會以未快取輸入重新送出）

**條件是「工具結果之後」。**

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788746080&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqEWjX3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746080631&amp;bpp=1&amp;bdt=1340&amp;idt=-M&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C334x250&amp;nras=2&amp;correlator=4812559548000&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1357&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=513&amp;eid=95395999%2C95399629%2C95400775%2C95401044%2C122880828&amp;oid=2&amp;psts=AOrYGsm2iq0TJY3r9dlhuMAxCpe_yH_dhZmLjzE9tIq7wsi_vD_spJMyaEZaX6up35ExtDd9REhNOiAOges7amyY0xpky--WpRJrr3mPULfmVyrnbMI&amp;pvsid=4056802229448142&amp;tmod=1931121027&amp;uas=3&amp;nvt=7&amp;ref=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqEWjX3&amp;fc=1408&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=3&amp;uci=a!3&amp;fsb=1&amp;dtd=27" title="Advertisement" aria-label="Advertisement"></iframe>

9 月 2 日我測的是 `claude -p "hi"` ， **一次都沒有呼叫工具。** 條件不同。

---

## 按照條件重新測試

我用 `--resume` 延續同一個 session，分別測了 **使用工具** 與 **不使用工具** 兩種情況。

**不使用工具** （ `hi` → `hi again` ）

試行readcreate命中率1〜532,047約51,000 **38.6%** 682,69621199.7%7〜932,047約51,000 **38.6%\*\*** 使用工具\*\*（先用 Read 讀取 1 個 `.md` 檔，再繼續）

試行readcreate命中率1〜6約163,000425〜496 **99.7%\*\*** 6 次全部命中。\*\* 沒有波動。

---

## 也就是說，只修好了一半

2.1.258（9/2）2.1.260（9/4） **使用工具的回合** 未測定 **6 次全都 99.7%\*\*** 不使用工具的回合 **18 次中 13 次約 40%** 9 次中 8 次是 38.6% **\*\*不使用工具那一側，還是原樣保留。** 甚至往比較低的方向去了（72% → 89%）。試行次數只有 9 次， **這個差異可能在誤差範圍內。**

---

## 沒命中的回合要花多少錢

把約 50,959 個 token 重新寫入，依照價目表換算。Fable 5.1 的 1 小時快取寫入是 $20/M。

```
沒命中時  50,959 tok × $20/M  =  $1.02
命中時        496 tok × $20/M  =  $0.010
```

**單一回合就差不多 1 美元。** 我是訂閱制，不會被另外收費，但以 API 定價來看就是這樣。

---

## 要怎麼做才會命中

**讓它呼叫工具就會命中** （6 次全都 99.7%）。實際寫程式時通常都會讀檔，所以 **如果是這種用法，影響應該不大** 。這部分是推測，沒有實測。

**我實際測到的是，不使用工具的那一側還存在。**

- `claude -p` 這種 **只丟問題** 的 batch
- 不讓它動檔案、 **只做文字討論**
- **摘要或翻譯** 用途

這類用法會變成， **第 2 回合要重寫 5 萬個 token。**

---

## 前一篇文章的數字不修正

9 月 2 日的文章裡我寫了「18 次中有 13 次沒命中。原因不明」。 **那是 2.1.258 的實測，記錄本身是正確的。**

這次得知的是， **那個現象不是 2.1.260 修正的對象。** 我認為 **沒有先假設「既然出了修正就一定好了」而是重新測量，是對的。**

**「官方發了修正」和「我看到的現象已經修好了」是兩回事。**

---

## 測量時的注意事項

**不要跳過發行說明中的條件。**

這次修正附帶了「 **after tool results** 」這個限制。若漏看了這點，就會變成：

- 明明沒修好，卻寫成「修好了」
- 明明修好了，卻寫成「沒修好」

**我差點做成後者。** 本來可能會因為用不使用工具都測成一樣，然後直接下結論「沒修好」。

**只有在改變條件後重新測試，才算完成驗證。**

---

## 總結

- 9 月 2 日我寫了「Fable 5.1 第 2 回合快取 18 次中有 13 次沒命中， **原因不明** 」
- 隔天 9 月 3 日的 **2.1.260** 進了 3 個 Fable 5.1 快取修正
- **修正的條件是「工具結果之後附加的 context」。** 我當時的測試沒有使用工具
- **把條件對齊後測試，使用工具的回合 6 次全都 99.7% 命中**
- **不使用工具的回合，9 次中 8 次仍是 38.6%。** 沒有修好
- 沒命中的回合大概要重寫 5 萬個 token。換算成定價約 **$1.02**
- **`claude -p` 只丟問題** 的用法最容易受到影響
- **「官方發了修正」和「我看到的現象修好了」是不同的事**

**當發行說明裡寫了條件時，請用那個條件重新測試。** 我差點就停在「沒修好」這個結論。

---

**參考資料**

- [Claude Code v2.1.260 發行說明](https://github.com/anthropics/claude-code/releases/tag/v2.1.260) — Fable 5.1 的 3 項快取修正
- [How Claude Code uses prompt caching](https://code.claude.com/docs/en/prompt-caching) — 快取機制
- [Pricing](https://platform.claude.com/docs/en/about-claude/pricing) — 1 小時寫入 $20/M

※ 引用同時附上原文與日文翻譯。翻譯以易讀性為優先，精確表述請以原典為準。

## 相關文章

- [Claude Fable 5.1，第 2 回合快取 12 次中有 8 次沒命中](https://qiita.com/suwa_nobu/items/8ae6739d0bb525e1b41b) — 這篇的前兩天。原因不明就先發表的那次
- [在 Claude Code 中切換模型時，只有那一回合的重寫量會暴增 287 倍](https://qiita.com/suwa_nobu/items/5fe930eb46d064b3da06) — 用同樣手順測快取的那次

---

JQIT 的工程師中，95% 以上都是從零經驗錄用的。  
歡迎也來公司網站逛逛。

▶ [公司網站](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

我們也在招募工程師，如果有興趣，歡迎看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處： [https://qiita.com/suwa\_nobu/items/3a135f5db9d53257a9e4](https://qiita.com/suwa_nobu/items/3a135f5db9d53257a9e4)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qEWjX3) [24瀏覽](https://codelove.tw/@tony/post/qEWjX3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qEWjX3)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝21

324

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788746079&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqEWjX3&amp;pra=5&amp;aiof=11&amp;asro=0&amp;itsi=42&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788746079384&amp;bpp=1&amp;bdt=93&amp;idt=20&amp;shv=r20260901&amp;mjsv=m202608310101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1788745928%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1788745928%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=4812559548000&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95395999%2C95399629%2C95400775%2C95401044%2C122880828&amp;oid=2&amp;pvsid=4056802229448142&amp;tmod=1931121027&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqEWjX3&amp;fc=1920&amp;brdim=2007%2C70%2C2007%2C70%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=25" title="Advertisement" aria-label="Advertisement"></iframe>