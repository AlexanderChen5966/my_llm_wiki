---
title: "7 個實用技巧，讓 Claude Code 的 Token 消耗暴降 80%"
source: "https://codelove.tw/@tony/post/xNQwEq"
author:
published:
created: 2026-05-25
description: "7 個實用技巧，讓 Claude Code 的 Token 消耗暴降 80%====================================    > 不要只優化 Prompt，更要優化 Context。Claude Code 很強大，這在前面的實踐文章中我們已經驗證過了，但與此同..."
tags:
  - "clippings"
status: processed
---
<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779686270&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxNQwEq&amp;pra=5&amp;asro=0&amp;itsi=1&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779686270623&amp;bpp=1&amp;bdt=16&amp;idt=32&amp;shv=r20260520&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779686067%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2290225598102&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=4&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31098652%2C95391807&amp;oid=2&amp;pvsid=601412629763332&amp;tmod=1302172221&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=36" title="Advertisement" aria-label="Advertisement"></iframe>

> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月21日](https://codelove.tw/@tony/post/xNQwEq "2026-05-21 19:02:15")

## 7 個實用技巧，讓 Claude Code 的 Token 消耗暴降 80%

## 7 個實用技巧，讓 Claude Code 的 Token 消耗暴降 80%

```
> 不要只優化 Prompt，更要優化 Context。
```

Claude Code 很強大，這在前面的實踐文章中我們已經驗證過了，但與此同時，也有不少朋友說 Token 消耗過多，成本過高。

面對這個問題，很多人第一反應是：是不是 Prompt 寫得太囉嗦了？

其實，很多時候真正「燒 Token」的，並不是輸入的那句話，而是 Claude 背後帶著的 **整段上下文** 。

它們可能包括：

- 之前的聊天記錄
- 已經讀取過的程式碼檔案
- 工具呼叫輸出
- 像 `CLAUDE.md` 這樣的記憶檔案
- 系統或後台注入的額外指令

也就是說， **當 Token 消耗越來越高時，問題往往不是 Prompt 不夠簡潔，而是上下文已經變得臃腫。**

很多所謂的建議，比如「盡量縮短對話」，當然沒錯，但太泛了，真正落地時幫助有限。

真正有效的做法，是搞清楚 Claude Code 的上下文是怎麼構建的、哪些內容會被重複攜帶，以及工作流程裡有哪些「隱性成本」正在不斷累積。

這篇文章我們來講 7 個真正實用的方法，在不犧牲效率的前提下，盡可能降低 Claude Code 的 Token 開銷。

1. ## 根據任務複雜度切換模型

這一點最簡單、但也最容易被忽視： **不是所有任務都值得用最貴的模型。**

如果使用按 API 計費，不同模型的成本大概有幾倍的差距；如果使用訂閱方案，那麼更重的模型也會更快消耗額度。

在執行任務時，可以做一個簡單的分層：

- 日常任務：寫測試、簡單改程式碼、解釋邏輯、常規重構
- 複雜任務：多檔案架構設計、棘手 bug 排查、跨系統分析
- 輕量任務：查找、格式化、重新命名、重複性操作

不同的任務採用不同的模型，像日常任務使用適中的模型即可，複雜任務則切換到能力更強、具備深度分析能力的模型，而且單純的普通操作、機械性任務交給一般的低參數模型即可。

以國外模型為例：

[![國外模型選擇](https://i.imgur.com/F7TdQS9.jpeg)](https://i.imgur.com/F7TdQS9.jpeg)

國內模型參考：

[![國內模型選擇參考](https://i.imgur.com/GJ4pgHB.jpeg)](https://i.imgur.com/GJ4pgHB.jpeg)

另外，不少人忽略了 `/effort` 。

[![effort 命令](https://i.imgur.com/eRZLY6k.jpeg)](https://i.imgur.com/eRZLY6k.jpeg) 對於一些本來就很直接的問題，適當降低 effort level，可以減少模型的「思考預算」，從而直接降低輸出 Token。

[![effort level 設定](https://i.imgur.com/ACbycRX.jpeg)](https://i.imgur.com/ACbycRX.jpeg)

一句話總結： **模型能力要和任務複雜度匹配，不要讓高效能模型去做低價值工作。**

2. ## 把 CLAUDE.md 當「規則索引」，而不是「百科全書」

如果你經常在每次對話裡重複輸入專案約束、開發規範、測試方式，那其實是在持續浪費 Token。這正是 `CLAUDE.md` 的意義所在。

在《 [使用 Claude Code 最需要做的一件事：與 AI 簽訂一份契約（CLAUDE.md）](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FEmTEA0ALoZMJmWLieF4iQw) 》一文中我們也有專門講到。

`CLAUDE.md` 會在 Claude 讀取程式碼之前就被載入，而且會在 **整個會話過程中一直駐留在上下文裡** 。

重點在於：它不是按需載入的，也不會輕易被「擠出去」。

這意味著什麼？如果你的 `CLAUDE.md` 有 5000 Token，那麼 **每一輪對話幾乎都要為這 5000 Token 付費** 。

不管你本次會話只有 2 輪，還是 200 輪，它都會持續產生成本。

### 那麼，適合放進 CLAUDE.md 的內容有哪些？

建議只放那些 **長期穩定、反覆要用到** 的規則，比如：

- 專案如何執行測試
- 使用哪個套件管理器
- 程式碼格式要求
- 關鍵架構約束
- 哪些目錄不要碰
- 團隊通用的開發約定

### 不建議放進去的內容

很多團隊會把下面這些東西也一股腦塞進去：

- 會議紀錄
- 設計演進歷史
- 冗長的實作說明
- 臨時性的任務背景
- 很長的業務文件

這些都不適合。

一個更好的原則是：

**讓** `CLAUDE.md` **像「速查手冊」，而不是「資訊垃圾場」。**

寫得越精煉，長期收益越高。

3. ## 把囉嗦任務交給 Subagent，但別濫用

這是一個非常值得重視的技巧，因為它能從根本上改變上下文膨脹的方式。Claude Code 的 Subagent，本質上是一個 **獨立上下文視窗中的 Claude 實例** 。

當你讓 Subagent 去執行任務時，它產生的很多「過程性雜訊」——比如：

- 檔案檢索
- 大段日誌分析
- 多輪推理過程
- 中間步驟輸出

當使用 Subagent 去執行任務時，可以避免上述雜訊直接污染主會話。

最終回到主執行緒的，通常只是一個總結結果。這對於保持主上下文乾淨非常有幫助。

但這裡也有一個常見誤區：

**Subagent 並不天然更省 Token。**

如果只是處理很小的任務，比如簡單 shell 操作、快速 git 指令，Subagent 往往反而更浪費。

因為它本身也有啟動成本，包括：

- 子代理的初始提示
- 工具定義注入
- 額外的工具呼叫往返
- 獨立上下文建構開銷

所以，正確的使用原則不是： **「所有事情都交給 Subagent。」**

而應該是：

**「只有當它節省下來的主上下文污染，足以覆蓋啟動成本時，再使用它。」**

適合交給 Subagent 的任務，通常有以下特徵：

- 輸出會很長
- 檢索範圍較廣
- 過程資訊多但結果摘要短
- 不需要主執行緒保留完整過程細節
4. ## 明確指定檔案和行號，別讓 Claude 在倉庫裡「自由發揮」

很多 Token 浪費，根本不是因為 Claude 回答太長，而是因為你給它的任務太模糊，導致它要先花很多 Token 去「找問題」。

比如這類說法：

「你幫我看看 auth 相關程式碼哪裡有問題。」

這聽起來很自然，但在 Claude 看來，這基本等於：

- 去 repo 裡搜一圈
- 打開多個相關檔案
- 試圖猜測你真正關心的點
- 還可能走很多冤枉路

如果問題實際上只在 1～2 個檔案裡，這種探索就是純浪費。更好的寫法應該像這樣：

「請對比 `src/auth/session.ts` 第 30～90 行，和 `src/api/login.ts` 第 10～60 行，說明兩者之間的邏輯不一致在哪裡。」

這類表達有幾個好處：

- 直接縮小搜尋範圍
- 減少無意義檔案讀取
- 降低模型重建上下文的成本
- 更容易得到準確結論

### 另一個容易被忽略的技巧：先用 Plan Mode

在執行一些可能成本較高的操作前，可以先切到 **Plan Mode** （ `Shift+Tab` ）。這個模式下，Claude 會先給出一個分步驟計畫，而不會直接修改程式碼。

你可以先審核這個計畫，把明顯沒必要的步驟刪掉，再切回正常模式執行。

為什麼這很重要？

因為實際使用中，最浪費 Token 的環節之一就是： **試錯式執行。**

比如 Claude 先嘗試一種方案，失敗了；再試第二種；又報錯；然後繼續修正……每一次嘗試、每一次報錯、每一次迭代，都是在消耗 Token。

而提前規劃，往往能大幅減少這種無效來回。

5. ## 主動使用 /compact，不要等「上下文快炸了」才想起來

很多人知道 Claude Code 有 `/compact` ，但真正用得好的人並不多。原因通常不在於「會不會用」，而在於 **用得太晚** 。

[![campact 命令](https://i.imgur.com/sMlN6bv.jpeg)](https://i.imgur.com/sMlN6bv.jpeg)

一個典型場景是：

- Claude 已經看過多個檔案
- 跑過若干命令
- 試過幾條錯誤方向
- 上下文裡塞滿了中間過程

這時候其實已經累積了很多「歷史雜訊」。而這些內容，對你接下來的任務未必還有價值。這正是最適合執行 `/compact` 的時機。

### 為什麼要盡早 compact？

因為如果你拖到後面，等到 Claude 開始遺忘前文，出現上下文警告，回答品質變差時，才去壓縮，那麼此時的會話已經很「髒」了。

這時生成的摘要，往往也不夠清晰、不夠高品質。

相反，如果在會話還比較健康的時候就主動 compact：

- 關鍵資訊更容易被保留下來
- 無關細節更容易被清理掉
- 後續每一步都會更輕量

所以， `/compact` 的最佳用法不是「亡羊補牢」，而是「定期保養」。

一個很實用的心法是：

**當關鍵結論已經出來，而中間過程開始變多時，就該考慮 compact 了。**

6. ## 優化之前，先用 /context 找到真正的「耗 Token 元兇」

很多開發者在發現 Token 消耗過快時，第一反應是改 Prompt、縮短提問、減少對話輪次。這些當然可能有幫助，但很多時候你根本沒打到重點。

因為真正昂貴的內容，未必是你當前看到的 Prompt。它可能是：

- 之前讀入的超大檔案
- 工具呼叫留下的大段輸出
- 某個過重的記憶檔案
- 某些整合工具帶來的系統開銷

這時候， `/context` 就非常關鍵。它相當於你的「上下文診斷面板」。

[![context 命令](https://i.imgur.com/prOcMul.jpeg)](https://i.imgur.com/prOcMul.jpeg)

在大改工作流程之前，先看一眼到底是誰在占用上下文，通常會更有價值。

[![Context Usage](https://i.imgur.com/e3xtkqe.jpeg)](https://i.imgur.com/e3xtkqe.jpeg)

很多優化收益最大的場景，並不是你 Prompt 寫得更精煉了，而是你終於發現：

有一個「沉默的大塊頭」，一直在每一輪對話裡默默消耗 Token。

所以，不要盲目優化。正確順序應該是：

1. 先檢查 `/context`
2. 看看哪些內容被重複載入或重複攜帶
3. 找出真正的臃腫來源
4. 再有針對性地刪減

**先診斷，再優化。** 這條原則在 Claude Code 裡非常重要。

7. ## 工具鏈要克制，整合不是越多越好

Claude Code 可以接很多外部工具、資料來源和輔助能力，這一點非常強大。但強大的另一面，是它也更容易把你的上下文結構搞複雜。

當工具接得越來越多時，模型可能需要額外處理：

- 工具定義
- 呼叫協定
- 上下文橋接資訊
- 工具返回結果
- 多工具協同帶來的額外說明

問題在於： **很多任務根本不需要這麼重的配置。**

如果你把所有能接的技能、外掛、輔助器全部掛上去，最後很可能出現一個尷尬局面：

任務很小，但系統開銷很大。

因此，一個更穩妥的策略是：

- 只保留真正高頻、剛需的工具整合
- 只接那些能持續解決重複問題的能力
- 不要因為「可以接」就全部接上

對 Claude Code 來說， **精簡的工具鏈通常比「全家桶式」整合更高效。**

## 小結

真正該優化的，不只是 Prompt，而是 Context Architecture。

如果只用一句話概括這篇文章，那就是：

**降低 Claude Code Token 成本的核心，不是對每條 Prompt 精打細算，而是設計好你的上下文架構。**

真正帶來大收益的，通常不是「把一句話少寫 20 個字」，而是這些更本質的動作：

- 控制自動注入的上下文
- 縮小任務搜尋範圍
- 及時壓縮會話
- 把高雜訊工作隔離出去
- 避免不必要的工具鏈負擔

說到底，Claude Code 的成本問題，本質上是一個 **上下文管理問題** 。很多開發者只盯著 Prompt 在優化，但真正成熟的用法，應該開始轉向另一層思考： **不要只寫 Prompt，要設計 Context。**

以下是 Claude Code 系列其餘文章：

**第 1 篇：** 《 [**國內環境下的 Claude Code 安裝與使用教學**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FRY9_LBVxPqK-1_RBXm2BmA) 》

第 2 篇：《 [**使用 Claude Code 最需要做的一件事：與 AI 簽訂一份契約（CLAUDE.md）**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FEmTEA0ALoZMJmWLieF4iQw) 》

第 3 篇：《 [**Claude Code 實踐：從零開始，一行程式碼不寫生成一個專案**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FygH5j-LX816gOS31RDplBg) 》

第 4 篇：《 [**Claude Code 的 Skills 實踐及利器推薦：工欲善其事，必先利其器**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FBkr-tw68m0y5zKXGAPIkMA) 》

第 5 篇：《 [**6 條 Claude Code 實踐中的經驗與思考**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FuoX8fEIUPX7p5FwWEtU9jA) 》

**第 6 篇：《** [**Claude Code 的一次真實專案實踐**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FJwoFRrHVD0vDL_tfvklOyQ) **》**

**第 7 篇：《** [**Claude Code 在不同開發環節的應用案例分享**](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FAe-1cL308VoDhDkZi9HF_g) **》**

---

原文出處： [https://juejin.cn/post/7640677540941283378](https://juejin.cn/post/7640677540941283378)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xNQwEq) [145瀏覽](https://codelove.tw/@tony/post/xNQwEq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xNQwEq)

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

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝18 💬11 ❤️1

550

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

82

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260520/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>