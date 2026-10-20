---
title: "從超過 16 萬顆星的 CLAUDE.md 學習，如何避免 Claude Code 失控的運用技巧"
source: "https://codelove.tw/@tony/post/37lvWa#google_vignette"
author:
published:
created: 2026-06-01
description: "![ChatGPT 圖片 2026 年 5 月 26 日 10_43_20 PM.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月29日](https://codelove.tw/@tony/post/37lvWa "2026-05-29 08:15:10")

## 從超過 16 萬顆星的 CLAUDE.md 學習，如何避免 Claude Code 失控的運用技巧

[![ChatGPT 圖片 2026 年 5 月 26 日 10_43_20 PM.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F82835%2F47ec0b65-06b1-441b-abd3-8067e290559b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d1f3b8a43a7f03c997fb9c90cdf40f9e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F82835%2F47ec0b65-06b1-441b-abd3-8067e290559b.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d1f3b8a43a7f03c997fb9c90cdf40f9e)

使用 Claude Code 時，我最近越來越覺得重要的是 `CLAUDE.md` 的運用。

Claude Code 非常強大，但在既有程式碼庫中長時間運作時，容易出現以下問題。

```
擅自重構
連不相關的檔案也一起修改
產生巨大的 diff
過度抽象化
把模糊的需求自行解讀後實作
```

這與其說是「AI 能力不足」，不如說是因為沒有對 AI Coding Agent 設下明確的行為約束。

因此可以參考的是 GitHub 上超過 16 萬星的 [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) 。

更精確地說，獲得星號的並不是單獨的 `CLAUDE.md` 檔案，而是這個儲存庫。不過，這個儲存庫的核心正是「用來改善 Claude Code 行為的一個單一 `CLAUDE.md` 」。

也就是說，值得注意的地方在於：受到矚目的不是龐大的工具，而是整理 AI Coding Agent 行為原則的文件。

## CLAUDE.md 是什麼

`CLAUDE.md` 是一個會被載入到 Claude Code 中的永久指示檔。

官方文件也將 `CLAUDE.md` 說明為用來提供 Claude 永久性脈絡的指示。

參考： [Claude 如何記住你的專案](https://docs.anthropic.com/ja/docs/claude-code/memory)

簡單來說，它就像是給 Claude Code 用的 README。

如果給人看的 README 是在說明「這個專案怎麼使用」，那麼 `CLAUDE.md` 說明的就是「Claude Code 在這個專案裡應該怎麼行動」。

`CLAUDE.md` 不是強制設定，而是作為脈絡被載入。  
它不是寫了就一定會遵守的魔法檔案。  
也因此，比起冗長而模糊的描述，寫得精簡且具體反而更有效。

## 為什麼 CLAUDE.md 很重要

AI Coding Agent 的問題，早就不是「能不能寫程式碼」。

現在的問題已經進入下一個階段。

```
能不能安全地長時間運作
會不會破壞既有設計
能不能產出容易審查的 diff
會不會做出人類沒有意圖的變更
```

Claude Code 可以讀取檔案、編輯程式碼、執行命令。

這很方便，但反過來說，如果行為約束太弱，它就很容易「自以為是在幫忙」而做出多餘的修改。

尤其在業務程式碼中，與其說是 AI 生成的程式碼本身危險，不如說是它去動到原本不該碰的地方更危險。

## andrej-karpathy-skills 的核心思想

[![ChatGPT 圖片 2026 年 5 月 26 日 10_48_47 PM.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F82835%2F05116eac-04a0-4204-ae8d-6398c73deb11.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=87c6ef0db44bf4d93973c2691599e440)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F82835%2F05116eac-04a0-4204-ae8d-6398c73deb11.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=87c6ef0db44bf4d93973c2691599e440)

[andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) 的 `CLAUDE.md` ，是為了抑制 Claude Code 的典型失敗而設計的行為原則。

這個儲存庫的特色是，針對 Andrej Karpathy 所指出的 LLM 寫程式典型失敗模式——例如擅自加前提、過度複雜化、連不相關的程式碼都去碰——整理成 4 個原則。

核心就是以下四項。

```
Think Before Coding
Simplicity First
Surgical Changes
Goal-Driven Execution
```

各自的意思都很單純。

## Think Before Coding

意思是在實作前先思考。

Claude Code 遇到模糊需求時，常常會自行解讀後直接往下做。

在人類會判斷「這裡應該先確認一下」的場景，AI 卻很容易先加上一個看似合理的前提就開始實作。

因此，在實作前讓它明確說出以下內容很重要。

```
要達成什麼
要修改哪些檔案
要基於什麼前提進行
有哪些模糊不清的地方
```

尤其在既有程式碼中，比起「看起來合理的實作」，「符合既有設計意圖的實作」更重要。

## Simplicity First

意思是以最小必要範圍進行實作。

AI 若放著不管，常常會過度泛化，或是根據未來可能的擴充來做抽象化。

但在業務程式碼中，「未來可能會用到的抽象化」有時反而會變成負債。

例如，明明只是要新增一個簡單的驗證，卻開始建立新的共用類別或設定物件，就是典型案例。

`Simplicity First` 就是用來抑制這種過度設計的規則。

```
不要新增未被要求的功能
不要把只會用一次的東西抽象化
配合既有寫法
即使可以做得很複雜，也要先用最簡單的方式解決
```

只要讓 Claude Code 持續意識到這個原則，diff 的雜訊就會明顯減少。

## Surgical Changes

意思是只修改必要的地方。

我個人認為，這是最重要的一項。

Claude Code 其中一個危險的行為就是「順手修正」。  
明明只要求修 bug，卻可能連周邊命名、註解、格式、舊程式碼都一起改掉。  
表面上看起來很貼心，但從審查者的角度來看，與原始修正無關的差分混在一起，會讓風險所在變得難以辨識。

`Surgical Changes` 會像這樣加上限制。

```
只動與需求直接相關的檔案
不要做不相關的重構
不要擅自更改既有註解或格式
即使發現未使用的程式碼，只要沒被要求也不要刪除
```

如果要在業務中使用 AI Coding Agent，我認為這個原則是必須的。

## Goal-Driven Execution

意思是把工作從「指示」轉換成「可驗證的目標」。

例如，與其只這樣要求，

```
修復這個 bug
```

不如改成這樣會更穩定。

```
新增一個可以重現這個 bug 的測試，並修正到該測試可以通過
```

只要有明確的成功條件，AI 的表現就會強很多。

相反地，如果成功條件很模糊，它就會憑感覺實作。

在交給 Claude Code 時，最好把需求整理成以下形式。

```
修正對象
預期結果
確認方法
可修改範圍
不可修改範圍
```

如此一來，Claude Code 就不只是單純的程式碼生成工具，而是更容易作為一個會執行驗證迴圈的代理程式來使用。

## plugin 和 CLAUDE.md 的差異

這個儲存庫也提供了以 Claude Code plugin 方式導入的方法。

參考： [andrej-karpathy-skills README](https://github.com/multica-ai/andrej-karpathy-skills)

這裡很容易混淆的是 plugin 和 `CLAUDE.md` 的角色。

我會這樣整理：

```
plugin / skills = 能力追加
CLAUDE.md       = 行為控制
```

plugin 或 skills 是用來替 Claude Code 增加特定作業流程或知識。

另一方面， `CLAUDE.md` 是用來控制 Claude Code 應該如何表現。

也就是說，只裝 skills 並不代表 Claude Code 的行為就會變安全。

相反地，能力變多了，如果約束不夠，失控的範圍也會變大。

因此，在實務上以下這種組合最有力：

```
用 CLAUDE.md 固定行為原則
用 skills / plugin 擴充作業能力
```

## 為什麼要放進全域 CLAUDE.md

`CLAUDE.md` 雖然可以放在每個專案底下，但如果是所有專案都共通想套用的行為原則，放在全域會更方便。

在 Claude Code 中，可以把 `CLAUDE.md` 放在下列位置，作為使用者層級的指示。

```
~/.claude/CLAUDE.md
```

官方文件也將使用者指示的範圍說明為 `~/.claude/CLAUDE.md` 。

參考： [Claude 如何記住你的專案](https://docs.anthropic.com/ja/docs/claude-code/memory)

適合放在全域的，是不依賴專案的原則。

例如：

```
不要憑猜測實作
保持最小 diff
不要碰無關檔案
尊重既有設計
不要做不必要的重構
不要自行新增新的依賴
```

以下這些內容就不適合放在全域：

- 特定專案的建置指令
- API 規格
- 目錄結構
- 分支流程
- iOS 或 React Native 等個別規則

這些比較適合寫在專案根目錄下的 `CLAUDE.md` 。  
如果混在全域裡，Claude Code 可能會在別的專案也用同樣的前提行動。

## 建議的結構

我個人覺得，以下這種結構最好用。

```
~/.claude/
├── CLAUDE.md
├── settings.json
├── skills/
└── commands/
```

各專案內則視需要放這樣的檔案。

```
project/
└── CLAUDE.md
```

角色要明確分開。

```
全域 CLAUDE.md
  所有專案共通的行為原則

專案 CLAUDE.md
  該儲存庫特有的規則

skills / plugin
  可重複使用的作業流程與專業知識

commands
  常用的固定任務
```

這樣分離之後，Claude Code 的行為會更穩定。

## 複製到全域的步驟

在 macOS 上，先建立 Claude Code 用的目錄。

```
mkdir -p ~/.claude
```

接著，把 `andrej-karpathy-skills` 的 `CLAUDE.md` 複製到全域。

```
curl -L \
https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/CLAUDE.md \
-o ~/.claude/CLAUDE.md
```

確認內容。

```
cat ~/.claude/CLAUDE.md
```

這樣就會作為 Claude Code 的使用者共通指示被載入。

如果 `~/.claude/CLAUDE.md` 已經存在，覆蓋前務必要先備份。  
全域指示會影響所有專案，萬一消失會相當麻煩。

```
cp ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.bak
```

如果想要和既有內容合併，不要直接覆蓋，先另存新檔再確認。

```
curl -L \
https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/CLAUDE.md \
-o ~/.claude/CLAUDE.karpathy.md
```

之後再和自己的既有規則整合。

## 如果是我會追加的規則

直接使用 Karpathy 風格的 `CLAUDE.md` 就已經很有效了，但如果是在業務程式碼中使用，我覺得也可以再追加以下規則。

```
## Additional Rules

- Prefer minimal diffs.
- Avoid unnecessary refactors.
- Preserve existing architecture unless explicitly requested.
- Do not modify unrelated files.
- Ask before introducing new dependencies.
- Do not change public API behavior unless required by the task.
- Always explain verification steps after implementation.
```

其中 `Do not modify unrelated files` 特別強。

對 AI Coding Agent 來說，不做無關修改比人類想像的還要重要得多。

## 使用時的注意事項

`CLAUDE.md` 很方便，但不是萬能的。

官方文件也說明了，Claude 對 `CLAUDE.md` 的處理方式是把它當作脈絡，而不是強制設定。

也就是說，寫了不代表一定會遵守。

因此，在實務上比較合理的做法是這樣：

```
用 CLAUDE.md 固定基本方針
重要工作時，也在 prompt 端重新指定約束
在大幅修改前，一定先讓它提出計畫
先確認 diff，再繼續下一步
```

尤其是重要修正時，一開始就這樣指示會更穩定。

```
在實作前，請先說明修改目標檔案、實作方針與驗證方法。
不要修改無關檔案。
```

光是這樣，就能大幅抑制 AI 失控。

## 總結

使用 Claude Code 時，重要的不只是「讓 AI 寫程式碼」。

接下來更重要的是，如何安全地運用 AI Coding Agent。

`andrej-karpathy-skills` 之所以能超過 16 萬星，我認為正是因為這個問題意識打中了許多工程師的痛點。

AI 的能力未來還會持續提升。

但能力越強，約束設計也越重要。

如果你真的要認真使用 Claude Code，那麼先整理 `CLAUDE.md` 的價值非常高。

尤其在既有程式碼庫中，光是加入以下四項就很值得。

```
Think Before Coding
Simplicity First
Surgical Changes
Goal-Driven Execution
```

與其說是為了提升 Claude Code 的品質，不如說 `CLAUDE.md` 是安全使用 Claude Code 的基礎。

## 後續文章在這裡

這篇文章整理了可從 `andrej-karpathy-skills` 的 `CLAUDE.md` 學到的基本思想。

在後續文章中，為了讓 Claude Code 在實務上更安全，我將只聚焦介紹 3 個想加入 `CLAUDE.md` 的安全運用規則。

- 不要對沒有讀過的程式碼自行推測
- 如果無法驗證，就提供理由與手動確認步驟
- 禁止擅自進行 Git 操作或破壞性操作

一起閱讀的話，可以更進一步強化 Claude Code 的運用規則，使其更符合實務需求。

▶ 後續文章： [想加進 andrej-karpathy-skills 的 CLAUDE.md 裡，Claude Code 安全運用規則 3 選](https://qiita.com/4q_sano/items/f313eed59628273b8026)

## 參考

[GitHub - multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)

[andrej-karpathy-skills / CLAUDE.md](https://github.com/multica-ai/andrej-karpathy-skills/blob/main/CLAUDE.md)

[Claude 如何記住你的專案](https://docs.anthropic.com/ja/docs/claude-code/memory)

[Claude Code 的設定](https://docs.anthropic.com/ja/docs/claude-code/settings)

---

原文出處： [https://qiita.com/4q\_sano/items/dc26f7468dcd39fbe62f](https://qiita.com/4q_sano/items/dc26f7468dcd39fbe62f)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/37lvWa) [121瀏覽](https://codelove.tw/@tony/post/37lvWa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/37lvWa)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780297752&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F37lvWa&amp;pra=5&amp;asro=0&amp;itsi=16&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTgxIixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xODEiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE4MSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780297752237&amp;bpp=1&amp;bdt=68&amp;idt=37&amp;shv=r20260527&amp;mjsv=m202605260101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780297616%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780297616%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780297616%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2253263903264&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=11&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31098652%2C95390667%2C95391802&amp;oid=2&amp;pvsid=8959276194226888&amp;tmod=1412125679&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=1920%2C95%2C1920%2C95%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=45" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260527/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>