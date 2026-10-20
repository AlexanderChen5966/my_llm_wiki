---
title: "因為 GitHub Copilot 的費用調整鬧得沸沸揚揚，所以我整理了最新的 Token 削減手法"
source: "https://codelove.tw/@tony/post/qz8LQ3"
author:
published:
created: 2026-06-08
description: "2026年6月1日起，GitHub Copilot 的計費機制改變了。在 X（Twitter）上，不論日本還是海外，幾乎都是一片負面聲浪。部分使用者回報，Claude Sonnet 的點數消耗量是舊制度的 9 倍，Claude Opus 甚至高達 27 倍；對於平常就有在大量使用 Agent m..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月06日](https://codelove.tw/@tony/post/qz8LQ3 "2026-06-06 08:15:12")

## 因為 GitHub Copilot 的費用調整鬧得沸沸揚揚，所以我整理了最新的 Token 削減手法

2026年6月1日起，GitHub Copilot 的計費機制改變了。

在 X（Twitter）上，不論日本還是海外，幾乎都是一片負面聲浪。部分使用者回報，Claude Sonnet 的點數消耗量是舊制度的 9 倍，Claude Opus 甚至高達 27 倍；對於平常就有在大量使用 Agent mode 的人來說，幾乎等同於大幅漲價。

因應這場騷動，我去研究了「到底要怎麼節省 token」這個問題。這次主要整理的是 X（Twitter）上海外使用者分享的 token 節省資訊。

關於 Netflix 工程師做的 token 壓縮工具「Headroom」，請參考我先前發表的文章： [以前投稿した記事](https://qiita.com/shinkai_/items/61b10d10c63db47a64e7) 。這次則會以其他手法為主來整理。

---

## 3 行總結

- GitHub Copilot 自 6/1 起改為用量計費，重度使用 Agent mode 的人等於實質大漲價
- 目前減少輸出最有效的是 **穴居人（原始人）Prompt** ，減少輸入最有效的是 **Prompt Cache**
- 設計書 → 程式碼適合用快取策略；程式碼 → 設計書則以子代理分離最實際

---

## 首先，改了什麼

舊制度是以「Premium Request」這個單位來管理。每次呼叫 GPT-4o 或 Claude Sonnet 都會消耗一定數量的 Premium Request，超過上限後就會自動切換成較低品質的模型。即使到達上限，也不會完全停機，算是有一道保護機制。

新制度的「GitHub AI Credits」則是依照 token 消耗量來計費。1 AI Credit 大致相當於 **約 $0.01 美元（目安）** ，輸入、輸出、快取 token 都會依照各模型的單價計算。

各方案每月提供的 credit 如下：

方案每月價格基本 credit 彈性配額每月合計 AI creditsCopilot Pro$101,0005001,500Copilot Pro+$393,9003,1007,000Copilot Max$10010,00010,00020,000※ Pro / Pro+ 含彈性配額  
※來源： [GitHub 官方文件 - 個人用量計費](https://docs.github.com/ja/copilot/concepts/billing/usage-based-billing-for-individuals)

也就是說，月費不變，但會給等值的 credit。

### 程式碼補完仍然免費

這點很重要，特別強調一下。 **行內補完與 Next Edit 建議不會消耗 AI Credits。** 所有方案目前 **暫時** 都還是無限使用。現在引發爭議的是聊天、Agent mode 和程式碼審查。

### 為什麼 Agent mode 會突然變得很貴

如果只是透過聊天問一個簡短問題，例如輸入 500 token、輸出 200 token，那消耗的 credit 很少，單次成本也不算高。

但 Agent mode 不一樣。當你說「幫我修這個功能」，代理會去讀多個檔案、呼叫工具、把前一輪脈絡再餵回去，然後反覆實作與驗證。單一請求會在內部拆成數十次模型呼叫，輸入與輸出 token 累積得很快。而且舊制度的 fallback 也被取消了，所以 credits 用完後，功能就會停止（程式碼補完仍可繼續）。

---

## X（Twitter）上討論的 4 種減量手法

### 1\. Caveman Prompt（減少輸出 token）

2026 年 3 月左右，GitHub 上出現了一個 [`JuliusBrussee/caveman`](https://github.com/JuliusBrussee/caveman) 倉庫，幾天內就拿到數千顆星。

概念很簡單， **「讓 LLM 用原始人一樣的方式說話」** 。  
※ Caveman = 原始人

```
# Caveman Mode
- No filler
- No grammar if not needed
- No repetition
- Use keywords, arrows, symbols
- Compress aggressively
- Assume user smart
Output = shortest correct answer possible
```

一般 LLM 會這樣回答：

> 「你可以使用 JOIN。在 SQL 中，當你使用共通鍵將兩個資料表的資料結合時，可以活用 JOIN 子句。」

Caveman Mode 會變成這樣：

> 「JOIN 使う」

意思一樣，但 token 數差很多。

#### 實際上能省多少 %

雖然以「可減少 75%」的宣傳方式廣為流傳，但實測者的數據如下：

檢驗者條件減少率作者方基準測試各類 coding task 最高 75% Kuba GuzikClaude Sonnet/Opus，18 次14～21%Better Stack 自建基準45%（僅輸出）和 Headroom 的情況一樣，「程式碼區塊本身不會被壓縮」這個規格會拉低數字。如果輸出大多是程式碼，效果就會有限。

#### 讓回答變簡潔，準確率反而可能上升

根據 2026 年 3 月的 arXiv 論文（arXiv:2604.00025），在 31 個模型、1,485 題的評估中發現， **限制簡潔度有時反而能提升大型模型的準確率** 。

原因似乎是大型模型在寫太多多餘說明時，會讓自己也混亂，這種現象被稱為 spontaneous scale-dependent verbosity。也就是說，不只省成本，對品質也可能有正面影響。

#### 不建議用在單次查詢

Caveman Prompt 的系統提示詞本體有 **數百 token** 。如果是單次查詢，這個額外成本可能比省下來的部分還多。只有在多輪對話、且 Prompt Cache 能生效時，情況才會改變。

**適合的情境：**

- 重複進行長時間的代理工作階段（可吃到快取）
- 主要是在生成說明、分析、摘要，而不是程式碼
- 代理會反覆問同一類問題的流程

**不適合的情境：**

- 單次提問
- 輸出大多是程式碼區塊時
- 需要很詳細說明的文件生成

---

### 2\. Prompt Caching（減少輸入 token）

GitHub Copilot 新的計費制度中，快取 token 的單價也比輸入 token 便宜。是否有意識地做這種設計，會直接影響同樣工作下的帳單金額。

#### 機制回顧

LLM 每次都從頭讀相同文字非常沒效率。Anthropic 使用 KV（Key-Value）Cache，將已讀過的前綴計算結果儲存在伺服器端。第二次以後可享 **90% 折扣（Anthropic 官方）** 。

在 X（Twitter）上，也有人分享透過 **快取大幅降低成本** 的案例。

#### 靜態內容放上面，動態內容放下面

```
# 讓快取生效的配置方式（順序很重要）

[放在上面：靜態內容]
- 系統提示詞
- 專案規範・限制
- 工具定義

[放在下面：動態內容]
- 對話紀錄
- 工具執行結果
- 最新的使用者訊息
```

Anthropic API 需要指定 cache breakpoint（cache\_control），但在 Copilot 或各種 AI 代理中，內部也使用類似的快取策略，因此一般都建議把靜態資訊放在開頭。

2026 年 4 月 29 日推出的 VSCode 1.118，已經把 GitHub Copilot 的快取相關機制優化了不少。  
如果你有一陣子沒更新 VSCode，光是升級到最新版，可能就能感受到效果。  
[Visual Studio Code 1.118 - 官方發行說明](https://code.visualstudio.com/updates/v1_118)

**在 GitHub Copilot 中，使用者可以有意識地讓快取更容易生效的方法**

- 在.github/ [copilot-instructions.md](http://copilot-instructions.md/) （自訂指示）中寫入靜態的專案規範 → 每個 session 都會有相同文字出現在前面，內部快取更容易生效
- 在 session 中不要切換模型 → 會破壞快取
- 把規則拆到提示詞檔（.[prompt.md](http://prompt.md/) ）中 → 每次重用時更容易吃到快取

##### 有效使用快取時不該做的事

在系統提示詞中塞入時間戳、在 session 中切換模型、改變工具定義的順序。  
這些都會造成快取 miss。

---

### 3\. 模型路由 —— 把 credit 用在刀口上

這是 Copilot 新制度下特別重要的概念。把高性能模型拿來做所有任務，就像搭計程車通勤一樣。

Copilot 可用的主要模型成本感如下（credit 消耗量以模型實際 token 單價為準）  
※ 模型清單會依用戶端或方案不同而異，請以最新的 model picker 為準

模型群成本感適合的工作Gemini Flash・GPT mini 系列・Claude Haiku便宜輕量作業・例行性工作GPT-5 系列・Claude Sonnet 系列・Gemini Pro 系列中等一般開發任務GPT-5.5・Claude Opus 系列昂貴複雜設計・高難度問題解決部分使用者回報，Claude Sonnet 的倍率是舊制度的 9 倍，Opus 則是 27 倍。如果在 Pro（月 1,500 credits）方案上大量使用 Opus 系列，幾天內就會用光。

所以，首先應該把 Auto 模式當作核心。Auto 模式會根據 Copilot 即時追蹤系統健康狀況與可用性，並評估任務複雜度（推理、程式碼生成複雜度、除錯難度、工具編排需求），再自動選擇最佳模型。  
雖然為了讓快取生效，通常不建議在 session 中切換模型，但 Auto 模式的設計本身會考量快取邊界來路由。從這個角度來看，Auto 模式和成本最佳化很相容。

※ Auto 會把 Opus 等高成本模型排除在路由對象之外，所以如果你想用 Opus 等模型，必須手動指定。

---

### 4\. 子代理分離（防止脈絡污染）

這是一種把檔案探索、grep、執行測試等「讀取成本很高的操作」丟到另一個 context window，讓主對話保持輕量的做法。

如果在 Agent mode 中大量讀取檔案，輸入 token 會迅速累積。把調查工作委派給子代理，並且只讓它回傳 **摘要** ，就能將對主脈絡的影響降到最低。

可以這樣指示：

```
請調查 src/auth 底下的驗證流程，
只列出畫序列圖需要的類別與方法，
用條列式回覆即可（不需要程式碼本體）。
```

這樣一來，檔案探索的成本就不會算進主 session 裡。

[![子代理.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1203022%2F1e357919-8f63-4e94-88ea-344e429184de.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a16f6130800bae9ace81cc4cbfaec623)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1203022%2F1e357919-8f63-4e94-88ea-344e429184de.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=a16f6130800bae9ace81cc4cbfaec623)

---

### 手法適用情境速查表

情境主要問題建議手法DB 查詢結果、Log、JSON 太大輸入 token 過多Headroom代理說明太長輸出 token 過多Caveman Prompt同一個系統提示詞反覆送出輸入重複Prompt Caching檔案探索、調查導致脈絡膨脹脈絡污染子代理分離把 Opus 用在單純任務上模型過度使用模型路由也可以搭配使用。把 Caveman Prompt 放進系統提示詞後， **額外的提示詞部分會成為快取對象** ，因此可以在降低額外負擔的同時，得到減少輸出的效果。

---

### 設計書 → 程式碼、程式碼 → 設計書的情況

我前一篇有提到，Headroom 對這兩種情境幫助不大。那麼該用什麼？

#### 設計書 → 程式碼

最有效的是 **Prompt Caching** 。設計書屬於「每一輪都不太會變的靜態文字」，和快取的相容性非常高。把設計摘要或規範寫到 [copilot-instructions.md](http://copilot-instructions.md/) 中後，在每次新增或修改規格時，就不需要每次都用完整成本重新讀取。

#### 程式碼 → 設計書

**子代理分離** 才是最實際的解法。如果把整份程式碼都讀進主脈絡，很快就會爆掉。透過「只回傳需要的類別與方法條列」這種委派任務，就能大幅減少流入主脈絡的資訊量。

另外，不建議在設計書輸出中使用 Caveman Prompt，因為設計書會變成原始人語。

---

## 最後

GitHub Copilot 的價格調整，不只是「價格表改了」而已，也讓人重新思考「要用哪個模型、怎麼用、用在哪裡」。

如果毫無思考地一直用 Agent mode，credits 會一下子燒光。不過，程式碼補完 **目前** 還是無限使用，所以短期內比較現實的做法，是「只要注意聊天和代理就好」。

Copilot 的儀表板現在已經能看到各模型、各功能的消耗量，所以我認為應該先確認自己的消耗模式，再決定要採取哪些對策。有試過的人，也歡迎留言分享心得！

※ 我另外也研究了 Netflix 工程師做的省 token 工具「Headroom」，想節省 token 費的人也可以看下面這篇文章。

---

## 參考連結

- [GitHub Copilot 個人用量計費（官方）](https://docs.github.com/ja/copilot/concepts/billing/usage-based-billing-for-individuals)
- [JuliusBrussee/caveman（GitHub）](https://github.com/JuliusBrussee/caveman)
- [I Benchmarked the Viral "Caveman" Prompt - Kuba Guzik](https://medium.com/@KubaGuzik/i-benchmarked-the-viral-caveman-prompt-to-save-llm-tokens-then-my-6-line-version-beat-it-d8e565f95e15)
- [Caveman: Reducing LLM Output Tokens - Better Stack](https://betterstack.com/community/guides/ai/caveman-llm/)
- [arXiv: Brevity Constraints Reverse Performance Hierarchies in Language Models（2026 年 3 月）](https://arxiv.org/abs/2604.00025)

---

原文出處： [https://qiita.com/shinkai\_/items/626dfa7857f2d554784e](https://qiita.com/shinkai_/items/626dfa7857f2d554784e)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qz8LQ3) [49瀏覽](https://codelove.tw/@tony/post/qz8LQ3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qz8LQ3)

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

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝6 💬2

337

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780888535&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fqz8LQ3&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780888535002&amp;bpp=1&amp;bdt=70&amp;idt=52&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780888487%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780888487%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6907185729555&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668%2C95392915&amp;oid=2&amp;pvsid=4683570438314003&amp;tmod=473573085&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=99" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="970" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>