---
title: "把 Claude Code 的輸出縮短 35% 之後，資訊反而更多了"
source: "https://codelove.tw/@tony/post/al5W1q"
author:
published:
created: 2026-08-24
description: "Claude Code 2.1.237（**本日 2026-08-20 發布**）新增了內建的輸出樣式 **Concise**。> Added a built-in \"Concise\" output style: Claude leads with results and skips pream..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月21日](https://codelove.tw/@tony/post/al5W1q "2026-08-21 17:02:01")

## 把 Claude Code 的輸出縮短 35% 之後，資訊反而更多了

Claude Code 2.1.237（ **本日 2026-08-20 發布** ）新增了內建的輸出樣式 **Concise** 。

機器學習與人工智慧

> Added a built-in "Concise" output style: Claude leads with results and skips preamble and narration, while doing the work just as thoroughly.  
> （新增內建的「Concise」輸出樣式：Claude 會直接先給結果，省略前言與敘述，但處理工作一樣細緻）

一聽到「會變短」，我就 **懷疑是不是偷工減料** 。於是我實測了。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539971&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5W1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536324532&amp;bpp=5&amp;bdt=1482&amp;idt=6&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787539965%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905&amp;nras=3&amp;correlator=6442092398885&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=927&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C122880397%2C122880502%2C42533294&amp;oid=2&amp;pvsid=8704050307439279&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

**輸出縮短了 35%，資訊量卻更多了。**

> 驗證環境：Windows 10 / Claude Code 2.1.237 / 模型固定為 Haiku 4.5

---

## 先看結果

我把 3 個相同問題分別用 Default 與 Concise 試了一遍，結果如下。

DefaultConcise差 **平均字數** （不含空白）697 **456\*\*** −35% **\*\*平均輸出 token** 891 **711\*\*** −20% **\*\*3 題全都由 Concise 變短了。** 各題如下。

問題DefaultConcise用 Python 去除列表中的重複864 字 **585 字** 用 git 修正前一個 commit 訊息662 字 **421 字** 調查 npm test 失敗原因的方法566 字 **362 字\*\*** 字數減少 35%，token 只減少 20%。\*\* 關鍵原因就在這裡。

---

## 減少的只有說明文字

**因為程式碼沒有被刪掉。** 直接看實例。問題是「要怎麼修正 git 的前一個 commit 訊息？」。

開發工具

**Default：**

```
要修正前一個 commit 訊息，可以使用 \`git commit --amend\`。

**基本用法：**

\`\`\`bash
git commit --amend
```

執行這個指令後，會打開預設編輯器，讓你編輯前一個 commit 訊息。  
修改訊息並儲存後，commit 就會被更新。（以下略）

```
**Concise：**
```

要修正前一個 commit 訊息，可以使用以下指令：

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539971&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5W1q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536324553&amp;bpp=1&amp;bdt=1503&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787539965%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280&amp;nras=5&amp;correlator=6442092398885&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2571&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C122880397%2C122880502%2C42533294&amp;oid=2&amp;pvsid=8704050307439279&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

```bash
git commit --amend
```

這個指令會打開編輯器，讓你編輯前一個 commit 訊息。

```
**從 2 句變成 1 句。** 指令本身完全沒變。

---短的反而選項更多
---------------------------------------------------------------------------------------------------------------------------------------

**這點很意外。** 看同一個回答的後半段。

列出的指令DefaultConcise\`git commit --amend\`有有\`git commit --amend -m "..."\`有有**\`git commit --amend --no-edit\`****沒有****有****\`git commit --amend --author="..."\`****沒有****有****Concise 反而多列了 2 個。**

可以看出，省下來的文字被拿去補充選項了。changelog 裡的
\`while doing the work just as thoroughly\`（工作一樣做得很周全）在**我手上測的這 3 題裡確實如此。**
\`--force-with-lease\` 的注意事項兩邊也都有。**被刪掉的只有重複說明。**

---設定方法
-----------------------------------------------------------------

**1. 從 \`/config\` 選擇**（推薦）

\`/config\` → **Output style**。選項會儲存在 \`.claude/settings.local.json\`。

**2. 直接寫進設定檔**
```

{  
"outputStyle": "Concise"  
}

```
**3. 單次試用**
```

claude -p "問題" --settings./st-concise.json

機器學習與人工智慧

```
本文的測試就是用這個方法。**可以在不汙染目前設定的情況下做比較。**

---3 個不知道就會卡住的點
------------------------------------------------------------------------------------------------------------

**1. 要到 \`/clear\` 之後才會生效。**

輸出樣式是**系統提示詞的一部分**，Claude Code 只會在工作階段開始時讀一次。

> Output style is part of the system prompt, which Claude Code reads once at session start. Changes take effect after \`/clear\` or a new session.
> （輸出樣式是系統提示詞的一部分，Claude Code 只會在工作階段開始時讀一次。變更會在 \`/clear\` 或新工作階段後生效）

**2. 不會作用在子代理上。**

> Output styles apply to the main conversation only: a subagent runs its own system prompt
> （輸出樣式只適用於主對話；子代理會使用自己的系統提示詞）

例外是 **fork**。因為它會繼承父層的系統提示詞，所以會沿用。

**3. \`/output-style\` 指令已經沒有了。** **在 v2.1.91 已刪除**。請改用 \`/config\`。

---與其他內建樣式的關係
-------------------------------------------------------------------------------------------------------------------------------------------------

目前內建共有 5 種。

樣式輸出量用途Default基準一般情況下使用**Concise（新）****較短**想快速拿到結果時Proactive—想減少確認、直接執行時Explanatory / Learning**較長**想邊學邊寫時官方對 Explanatory 與 Learning 明確寫了 **\`produce longer responses than Default by design\`**
（設計上會產生比 Default 更長的回應）。
**「輸出樣式」不等於「一定要變短」**。只有 Concise 是縮短方向。

---和在 CLAUDE.md 裡寫「簡潔一點」不一樣
--------------------------------------------------------------------------------------------------------------------------------------------------

很多人應該也會在 \`CLAUDE.md\` 裡寫類似內容，但**機制不同。**

放入的位置**輸出樣式****系統提示詞本身**CLAUDE.md系統提示詞**後面**的使用者訊息官方也區分得很清楚：針對專案規範、程式碼庫等指示，請使用 CLAUDE.md。
**「怎麼回答」屬於輸出樣式；「需要知道什麼」屬於 CLAUDE.md。**
如果你在 CLAUDE.md 裡寫了語氣或文風指示，**可以移到輸出樣式。**

也可以自己做。只要把 Markdown 放到 \`~/.claude/output-styles/\`。
但要注意，**如果沒有寫 \`keep-coding-instructions: true\`，預設的程式碼撰寫指示也會一起消失。**

---總結
-----------------------------------

- 在 2.1.237（**2026-08-20**）新增了 **Concise**。同樣 3 題測試下，**字數減少 35%，輸出 token 減少 20%**
- **減少的是說明文字，指令與程式碼沒有減少**
- **反而選項變更多。** git 範例中，\`--no-edit\` 和 \`--author\` 只有 Concise 版本有
- 設定可從 \`/config\` → Output style，或直接寫 \`outputStyle\`。**要到 \`/clear\` 後才會反映**
- **不適用於子代理**（fork 會繼承父層，因此是例外）。Explanatory 與 Learning 則是**設計上更長**
- **如果你把文風寫在 CLAUDE.md 裡，可以移到輸出樣式**

如果你一直覺得「前言太長」，**只要改一個設定就能改善。**

---

**參考**

- [Claude Code changelog](https://code.claude.com/docs/en/changelog) — 2.1.237 項目
- [Output styles — Claude Code Docs](https://code.claude.com/docs/en/output-styles)
- 驗證環境：Windows 10 / Claude Code 2.1.237 / Haiku 4.5

※ 引用內容同時附上原文與日文翻譯。為了閱讀順暢，譯文有做語意優先的調整，精確措辭請以原典為準。
※ 測試使用 \`claude -p --output-format json\`，每種樣式對同一問題各執行 1 次。**以上是 3 題平均**，實際差異會因問題而不同。相關文章
---------------------------------------------

- [Claude Code 設定最容易踩坑的地方整理](https://qiita.com/jqit_suwa/items/d49739b934606d4abdbd) — 以症狀為切入點整理設定相關陷阱
- [即使把 CLAUDE.md 寫很厚也沒用的故事](https://qiita.com/jqit_suwa/items/2dee3e3d53080c3676a0) — 關於 CLAUDE.md 應該寫什麼

---

JQIT 的工程師有 95% 以上都是從零經驗錄用。
如果有興趣，也歡迎到公司官網逛逛。

▶ [公司官網](https://www.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=corporate)

我們也在招募工程師，若有興趣也歡迎看看。

▶ [招募網站](https://recruitment.jqit.co.jp/?utm_source=qiita&utm_medium=article&utm_campaign=jqit_suwa&utm_content=recruit)

---

原文出處：https://qiita.com/jqit_suwa/items/ccd228bb1c33b2a918f5
```

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/al5W1q) [178瀏覽](https://codelove.tw/@tony/post/al5W1q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/al5W1q)

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

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝9 💬4 ❤️1

190

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

6

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="350" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=1414751727&amp;adf=3620188380&amp;pi=t.aa~a.4029027559~rp.3&amp;abgtt=6&amp;w=350&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787539971&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=350x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5W1q&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536324614&amp;bpp=2&amp;bdt=1563&amp;idt=2&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787539965%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280&amp;nras=6&amp;correlator=6442092398885&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=1167&amp;ady=1079&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C122880397%2C122880502%2C42533294&amp;oid=2&amp;pvsid=8704050307439279&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=6&amp;uci=a!6&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="1200" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;adk=3809598800&amp;adf=1056458448&amp;pi=t.aa~a.1416458249~rp.1&amp;abgtt=6&amp;w=1200&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1787540308&amp;rafmt=1&amp;to=qs&amp;pwprc=3933869671&amp;format=1200x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5W1q&amp;fwr=0&amp;pra=3&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=40&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536324623&amp;bpp=1&amp;bdt=1578&amp;idt=1&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787539965%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787539965%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x905%2C952x280%2C952x280%2C952x280%2C350x280%2C1005x271%2C334x250%2C952x280&amp;nras=9&amp;correlator=6442092398885&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=217&amp;ady=7042&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=3515&amp;eid=95397679%2C122880397%2C122880502%2C42533294&amp;oid=2&amp;psts=AOrYGskt55JIdAAr4-BZRsHf-F5qZyG87PDyChYZfd2-qWv1r5LNTbZMgzLihwSqbnvPs6DDbe1QpzOdpu4tEfPQ6XBT4fUhQpQriynxd_Fjn4bO2l-IFtQKGXCKyH6Y_jk5zYQ%2CAOrYGskfdGvDuVt53VTmgC8X2P4RP-GNHV0B8PNW6qW4Ujq9NXWlxA8qmV2pt7YSniTIKqtQtjFqhh_I7Ivy4W6yYohypBzIQCXs9LLkbos8mQBvRfc&amp;pvsid=8704050307439279&amp;tmod=1573633200&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=1920%2C42%2C1920%2C42%2C1920%2C30%2C1874%2C986%2C1634%2C905&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1787536323&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fal5W1q&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTczIixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjE3MyJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjE3MyJdXSwwXQ..&amp;dt=1787536323280&amp;bpp=1&amp;bdt=143&amp;idt=79&amp;shv=r20260820&amp;mjsv=m202608170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1787536255%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1787536255%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6442092398885&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=905&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C122880397%2C122880502%2C42533294&amp;oid=2&amp;pvsid=8704050307439279&amp;tmod=1573633200&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C905&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=125" title="Advertisement" aria-label="Advertisement"></iframe>