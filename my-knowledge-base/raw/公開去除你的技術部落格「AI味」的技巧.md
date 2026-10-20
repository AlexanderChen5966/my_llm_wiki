---
title: "公開去除你的技術部落格「AI味」的技巧"
source: "https://codelove.tw/@tony/post/qP5wRx#google_vignette"
author:
published:
created: 2026-07-14
description: "以下內容直接複製貼上，然後請 Claude Code 或 Codex 幫你加到「使用者範圍的技能」裡就可以。```---name: writing-guidedescription: 消除 AI 味的通用寫作指南。可用於部落格、投影片等各種寫作的文章風格規則user-invoca..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月14日](https://codelove.tw/@tony/post/qP5wRx "2026-07-14 08:45:12")

## 公開去除你的技術部落格「AI味」的技巧

以下內容直接複製貼上，然後請 Claude Code 或 Codex 幫你加到「使用者範圍的技能」裡就可以。

```
---
name: writing-guide
description: 消除 AI 味的通用寫作指南。可用於部落格、投影片等各種寫作的文章風格規則
user-invocable: true
model: opus
---
```

## 通用寫作指南 — 消除 AI 味的文章術

技術部落格、投影片、書籍等，各種寫作都適用的共通文章風格規則。  
與各專案固有規則（ [CLAUDE.md](http://claude.md/) 等）搭配使用。

## 這個技能的使用方式

把它當成寫作時的「品質檢查清單」。

1. **寫作前** ：先把這些規則記在心裡再開始寫
2. **寫作中** ：一旦出現 AI 味很重的表達，立刻改寫
3. **寫作後** ：將完成的文章對照禁止清單做自我檢查

---

## 1\. AI 味很重的表達禁止清單

以下是 AI 生成文章中常見、容易讓讀者感到「這是 AI 寫的」的模式。請有意識地避開。

### 文體上要避免的表達

| 禁止模式 | 為什麼不好 | 替代方式 |
| --- | --- | --- |
| 粗體（ `**...**` ）大量使用 | AI 常會機械式地把它判斷為「重要」而加粗 | 真的需要強調的地方再用。基本上以平實書寫為主 |
| 破折號（—、──、―、 `-` 作為區隔） | AI 特有的分隔表達。 **絕對不要使用** ，包括投影片標題、本文、blockquote 都一樣 | 用「…」、逗號，或拆成句子；採用「項目名：說明」的形式 |
| 標題或正文大量使用冒號 | 「XX：YY」是 AI 很愛用的結構化寫法 | 改用引號、逗號，或乾脆省略副標題，讓結構更簡潔 |
| 整篇都用「です・ます」體 | 全部敬體會顯得冗長又像 AI | 改用斷定句、名詞止句（例如「〜できる」「〜有必要」「〜的 3 個步驟」）。正文也盡量偏常體，只在呼籲時用敬體 |
| 標題、引言、正文結尾全部都用名詞止句 | 名詞止句連發會像公告標語，失去人的語氣 | 混入動詞收尾、真實感受（例如「老實說〜」「其實我也曾〜」）或提問。名詞止句不要過度使用 |
| 驚嘆號濫用 | AI 常想營造親切感而過度加驚嘆號 | 只有真正需要強調時才使用 |
| 過多連接詞 | 「此外」「再加上」「另外」「另一方面」每段都出現會很機械 | 依照文章脈絡自然銜接，刪掉多餘連接詞 |
| 「〜することができます」 | AI 常用的冗長說法 | 直接寫成「〜できます」 |
| 濫用「活用」 | AI 常把「用」換成更抽象的「活用」 | 直接寫「使用」「利用」「用」 |
| 「〜についてご紹介します」 | AI 文章常見的制式開場 | 直接切入主題 |
| 把具體行為硬改成抽象動詞 | 例如把「介紹」寫成「走訪 6 個部署目標」這類不自然表達 | 直接說「今天要介紹這些內容」 |

### 構成上要避免的模式

- **自動加入議程／目錄** ：短文不需要，重點放在內容本身
- **總結段落** ：不要重複讀者已經看得懂的內容
- **平均而全面的列舉** ：AI 容易把所有項目都列出來，應該只挑重要的
- **「以下所示」→ 條列** ：每次都這樣會很制式
- **先下結論再用條列展開** ：這是 AI 文章的典型結構，應依情境調整

---

## 2\. 強化人味的技巧

消除 AI 味的最大關鍵，就是加入「只有這個人寫得出來的元素」。

### 加入個人經驗與見解

- 堅持「實際碰過再寫」的態度。不要只評論，先親手操作後再寫
- 也要老實寫出不順利的地方（失敗經驗最有人味）
- 像「我個人特別在意的是〜」「這個老實說有點微妙〜」這類主觀表達，不必害怕
- 補上背景與故事脈絡（為什麼想寫這個、契機是什麼）
- 不要擅自生成與作者意圖不同的內容。先對齊、來回確認後再寫

### 比喻與白話

- 專業術語第一次出現時，補上中文說明並用括號標示
	- 例：Function Calling（函式呼叫）
- 用「講得很粗略地說」的方式，把嚴格定義講白
- 比喻要用身邊常見、但可以大膽一點的說法
	- 例：把 LLM 比成「哆啦 A 夢」，把容器比成「一次性的虛擬電腦」
- 用「抽象 → 具體 → 比喻」三個層次來解釋，通常最容易懂

### 考慮讀者立場

- 加入安心語句，例如「其他地方維持預設即可」「只要掌握這裡就好」
- 預先處理常見卡點：先說明讀者可能會遇到的問題
- 不要預設讀者已有太多背景知識：一直想著初學者跟不跟得上

---

## 3\. 資訊密度的控制

### 條列規則

- 3～4 項就是上限。再多就拆開，或改在內文說明
- 層級最多 1 層，撐死 2 層。3 層以上的巢狀會讓讀者迷路
- 每項控制在 1～2 句。太長就回到正文裡寫

### 1 個主題 1 個區塊

- 投影片就是「1 張投影片 1 個訊息」
- 部落格就是「1 段 1 個主題」
- 書籍就是「1 節 1 個主題」
- 不要塞太滿，讓理解能一層一層累積上去（逐步建構式）

---

## 4\. 圖表製作規則

- 製作圖並插入正文或投影片時，不要輕易使用 Mermaid 語法
- 對於重視外觀與資訊設計的圖，原則上應製作並插入 SVG 圖檔。版面配置、留白、字級、配色都要依媒體調整
- Mermaid 只限於簡單流程圖或結構確認等情境，也就是當「可編輯性」比「圖的品質」更重要時才使用
- 插入 SVG 後，要確認實際顯示效果，修正文字截斷、重疊、縮小後難以閱讀等問題

---

## 5\. 表記規則

### 程式碼表記

- 行內程式碼（`...`）前後要保留半形空格
- 程式碼區塊一定要標註語言（例如 `python` 、 `sh` ）
- 程式碼要以可直接複製貼上的狀態呈現

### 主詞與動作主體

- 模糊的動作主體要明確寫出
	- NG：「會被設定」→ OK：「使用者設定」「系統自動設定」
- 優先使用主動語態，而不是被動語態

---

原文出處： [https://qiita.com/minorun365/items/699e89544da8b0de300d](https://qiita.com/minorun365/items/699e89544da8b0de300d)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qP5wRx) [2瀏覽](https://codelove.tw/@tony/post/qP5wRx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qP5wRx)

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

📝17 💬3 ❤️5

741

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

170

🥉

[流引色群@jptd847utpp](https://codelove.tw/@sp9863)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783990681&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqP5wRx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783990681256&amp;bpp=2&amp;bdt=109&amp;idt=35&amp;shv=r20260713&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783932846%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783932846%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783932846%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1284768809428&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396252&amp;oid=2&amp;pvsid=4576216282851155&amp;tmod=761720134&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=54" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260713/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>