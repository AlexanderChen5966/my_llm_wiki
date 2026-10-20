---
title: "重新實作了 40 篇關於 multi-agent 的論文後，我學到的事"
source: "https://codelove.tw/@tony/post/3bp8Nq"
author:
published:
created: 2026-07-20
description: "コロンビア大学の博士課程で AI・セキュリティの研究をしている Koukyosyumei です。最近暇なので、マルチエージェント LLM に関する 40 本の論文で提案されているワークフローを再実装してみました。論文で報告されているベンチマーク結果を再現するというよりは、それぞれの手法が、どのよ..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月19日](https://codelove.tw/@tony/post/3bp8Nq "2026-07-19 08:30:10")

## 重新實作了 40 篇關於 multi-agent 的論文後，我學到的事

コロンビア大学の博士課程で AI・セキュリティの研究をしている Koukyosyumei です。

最近暇なので、マルチエージェント LLM に関する 40 本の論文で提案されているワークフローを再実装してみました。論文で報告されているベンチマーク結果を再現するというよりは、それぞれの手法が、どのようなループを持ち、どのような役割分担を行い、どのようなルールで複数の出力を統合するのかを理解することを目的として再現実装を行い、いろいろな学びがあったので、この記事で共有したいと思います。

## 結局全部、「ループ構造・プロンプト設計・集約ルール」で説明可能

多くのマルチエージェント手法は、突き詰めると次の 3 つの組み合わせです。

- どのような形で処理を繰り返すか
- 各エージェントにどのような指示を与えるか
- 複数の出力をどのように統合するか

h5i-python という、マルチエージェントのワークフローを Python で定義・実行できるフレームワークを使った実装では、ほぼすべての論文のコアアルゴリズムを 100 行前後で実装できてしまいました。

```
# コード例

from h5i.orchestra import Conductor

async def main(task):
    async with Conductor(repo=".", run="demo-task", launcher="resident") as c:
        claude = await c.hire("claude-agent", runtime="claude")
        codex  = await c.hire("codex-agent",  runtime="codex")

        # Have both agents implement the task independently and in parallel
        claude_work, codex_work = await asyncio.gather(claude.work(task), codex.work(task))

        await c.freeze() # Seal the round, ensuring that neither agent influenced the other beforehand

        # Have each agent review the other's work
        await asyncio.gather(codex.review(claude_work), claude.review(codex_work))

        # Verify each submission in a fresh, neutral sandbox
        await c.verify(claude_work, ["pytest", "--quiet"])
        await c.verify(codex_work, ["pytest", "--quiet"])

        verdict = await c.judge() # Select the smallest diff among the submissions that pass all tests
        print("winner:", verdict.selected_submission)

asyncio.run(main("implement quicksort in python with unit test"))
```

## 獨立性是容易被忽略但很重要的要求

Self-Consistency、CodeT、CoVe 之類的方法，隱含前提是多個樣本彼此看不到對方的輸出。也就是說，每個推理或回答都必須獨立生成。

但是，在所有代理都共享同一個對話上下文的框架中，這種獨立性可能會不小心被破壞。當某個樣本的輸出可以被其他樣本參照時，那就不再是獨立產生想法的多代理系統了。h5i-python 讓每個代理在隔離的沙盒中工作，並且只在被允許時才進行通訊，因此較容易實作並確保這種獨立性。

## 改善迴圈，大致上都一樣

Self-Refine、Reflexion、CRITIC、Self-Debug、Constitutional AI 等等，近年有許多論文提出會自我改善輸出的手法。不過，基本結構幾乎都相同。

1. 先生成初始回答或成果物
2. 取得某種回饋
3. 用回饋改善成果物
4. 視需要重複

不同方法的主要差異，在於回饋從哪裡取得：

- 另一個批評代理
- 自我反思
- 外部工具的執行結果
- 自己重新讀取程式碼後的結果
- 明文化的原則或憲法

如果把回饋當作獨立的一級物件來處理，這些方法幾乎都可以實作成同一個函式，只是把「回饋的產生方式」當成參數傳入。

## 集約規則大約只有 10 行，但本質就在這裡

將多個候選答案或代理的判斷整合起來的規則，多半只要 10 行左右就能實作，但我覺得這一部分常常正是論文新意的核心。

- 多數決
- 依信心度加權投票
- 多個評分者的平均分數
- 計算同意票
- 一對一比較的勝場數

## 辯論方法的本質是「讓誰看見什麼」

讓多個代理進行辯論的方法也很受歡迎，但歸根究柢，關鍵全在於「誰能看到誰的訊息」這條規則。

- 每個人都能看到所有發言的匯流排型
- 經由中央代理傳遞的星型
- 只有相鄰代理能通訊的環型
- 以階層方式傳遞資訊的樹型

再加上停止條件與投票規則，就能表達辯論方法設計空間的大部分。

## 結論：40 本論文大致可分為 8 種系統

實作完 40 本之後，我覺得這些方法大致可以分成以下 8 種類型。

1. 改善迴圈
2. 多樣本生成與投票
3. 辯論
4. 多評審判定
5. 多輸出融合
6. 先探索再確定最終方案的方法
7. 軟體開發與長文生成中的分階段管線
8. 動態團隊編成與管理

如果把這 8 類實作出來，並將角色、通訊結構、回饋來源、投票方法等參數化，就能涵蓋相當大範圍的多代理 LLM 研究。實作這 40 本論文最大的收穫，是我發現個別方法並不是完全不同的演算法，而是少數基本模式用不同方式組合而成。

實作內容已公開在這裡。

---

原文出處： [https://qiita.com/Koukyosyumei/items/e9ee8e26cfdc40a8c2f9](https://qiita.com/Koukyosyumei/items/e9ee8e26cfdc40a8c2f9)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3bp8Nq) [13瀏覽](https://codelove.tw/@tony/post/3bp8Nq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3bp8Nq)

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

223

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

109

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784516569&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3bp8Nq&amp;pra=5&amp;asro=0&amp;itsi=0&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI1Il1dLDBd&amp;dt=1784516569277&amp;bpp=1&amp;bdt=86&amp;idt=50&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784516516%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784516516%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784516516%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8442490125126&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=31099678%2C95396101%2C95396139%2C95396567%2C95396667%2C95396252&amp;oid=2&amp;pvsid=5587531447405210&amp;tmod=426775654&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1533%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=102" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260716/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=2&amp;uci=a!2&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>