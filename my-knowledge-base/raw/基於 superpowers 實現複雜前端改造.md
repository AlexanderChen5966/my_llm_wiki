---
title: "基於 superpowers 實現複雜前端改造"
source: "https://codelove.tw/@tony/post/qEkP73"
author:
published:
created: 2026-06-09
description: "基於 superpowers 實現複雜前端改造  ==========================> 我們是[袋鼠雲數棧 UED 團隊](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhtt..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月09日](https://codelove.tw/@tony/post/qEkP73 "2026-06-09 16:30:11")

> 我們是 [袋鼠雲數棧 UED 團隊](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fwww.dtstack.com%2Fued) ，致力於打造優秀的一站式資料中台產品。我們始終保持工匠精神，探索前端道路，為社群積累並傳播經驗價值。 本文作者：切果

1. ## 背景：複雜改造真正難的不是寫程式碼，而是控制跨層一致性

superpower 是開源社群非常知名的 harness 框架，我們在本文透過數棧產品 easyIndex 的複雜需求實作來探究 superpower 的價值。

在交叉表改造裡，業務上的直接訴求其實不複雜：原有「表格」展示更適合看明細，但當使用者想同時按多個維度做橫向與縱向對比時，比如看「某類指標在不同區域、不同時間段上的分布關係」，一般表格的閱讀成本會迅速升高。交叉表的價值，就在於把這種「按列看分組、按欄看展開、在交叉點看結果」的分析方式直接放進查詢結果裡。

真正的難點並不是把一個新元件寫出來，而是讓多個鏈路在同一次改造裡穩定協同：

- 配置層要支援結果視圖切換，以及行維度、列維度的獨立編排
- 參數層要保證組參、歷史回填、重置邏輯在雙模式下都成立
- 渲染層要把交叉表從原有展示方式升級為 Canvas，並接入既有查詢和儀表板鏈路

這類需求的風險，通常不在單點實作，而在跨層契約容易斷裂。 如果只用傳統「問答式 AI Coding」推進，常見結果是某一段程式碼看起來能執行，但改造一旦跨過多個模組，AI 就容易丟失上下文、誤擴散修改範圍，最後把問題從「實作功能」變成「收拾整合階段的連鎖迴歸」。

所以這次回顧的重點，不是交叉表本身做了多少業務能力，而是 `superpowers` 如何把複雜改造組織成一個可控、可驗證、可複用的交付過程。

2. ## 為什麼是 superpowers：從問答式 AI 到工程化協作框架

這次專案裡， `superpowers` 承擔的不是「多寫程式碼」的角色，而是「組織 AI 參與方式」的角色。它的核心價值，是先把協作過程標準化，再讓 AI 進入編碼階段。

相比直接讓 AI 對著需求生成程式碼， `superpowers` 更強調幾個前置動作：

- 先固定目標、範圍、非目標和驗收口徑，避免需求在實作中漂移
- 先收斂倉庫上下文，只暴露高相關模組，避免 AI 無邊界擴散修改
- 先拆成可獨立驗證的階段，再逐段交付，而不是一次性大改
- 每輪失敗都結構化回灌，讓 AI 根據證據修復，而不是反覆猜測

這套方法的意義在於，它把 AI 從「程式碼補全器」提升為「受約束的協作執行者」。 對複雜前端需求來說，真正決定交付效率的，往往不是 AI 能不能寫出局部程式碼，而是它能不能在約束下持續輸出可合併結果。

3. ## superpowers 在這次改造中的三個關鍵動作

### 3.1 任務歸一化：先統一問題定義，再開始實作

交叉表改造如果直接進入編碼，很容易把工作理解成「新增一種展示元件」。但在真正的業務鏈路裡，它實際上是一個跨配置、跨參數、跨渲染的聯動改造。

這次在進入實作前，先透過 `superpowers` 把任務統一成幾個明確結論：

- 目標不是單點替換元件，而是在不破壞原查詢鏈路的前提下支援雙模式運行
- 非目標要提前鎖死，例如不改後端協議、不新增拖曳依賴、不碰無關業務模組
- 驗收口徑不是「能看到交叉表」，而是模式切換、參數回填、渲染接入要形成完整閉環

這一步的直接收益，是把後續實作從「邊寫邊補需求」變成「圍繞固定邊界推進」。 對於 AI 協作來說，這比任何 prompt 技巧都更重要，因為約束清晰之後，輸出才會穩定。

### 3.2 最小上下文投餵：限制 AI 的修改半徑

複雜需求的另一個常見問題，是上下文投餵過多，導致 AI 把不該改的地方一起改掉。 這次沒有把整條業務鏈都攤給 AI，而是只圍繞高相關模組收斂上下文，例如：

- `Filters`
- `crossConditionBox`
- `query/help`
- `crossTable`

這種最小上下文策略，核心不是省 token，而是控制 **修改半徑** 。 當 AI 只圍繞關鍵模組做推理時，它更容易理解當前階段真正的契約，減少跨模組誤改和「順手重構」。

### 3.3 分段交付與失敗回灌：讓修復建立在證據上

這次改造沒有一次性推進到底，而是按三個階段逐步落地：

1. 先鎖定維度語義模型
2. 再打通參數閉環
3. 最後推進 Canvas 渲染接入

每一段都有獨立驗證點，出問題時也不是籠統地說「這裡不對」，而是把錯誤類型、位置、重現路徑和期望差異結構化回灌給 AI。 這樣 AI 的下一輪修復，不再依賴猜測，而是基於明確證據收斂。

這一步直接降低了兩類成本：

- 無效往返：避免同一個問題被反覆用不同方式試錯
- 整合爆炸：避免多個未驗證修改堆到最後一起出問題
4. ## 三個案例：superpowers 如何具體作用到程式碼行為

為了避免方法論變成空泛總結，這裡只保留三個最能說明 `superpowers` 價值的技術證據點。

先補一層最小業務語境：這次交叉表並不是單純把欄位換個擺放方式，而是要支援使用者把分析維度拆成「行維度」和「列維度」，再在交叉點上查看聚合結果、彙總和對比關係。也正因為它天然同時牽涉配置、參數和渲染三層，所以非常適合作為 `superpowers` 方法是否有效的驗證樣本。

### 4.1 語義契約：先鎖定 dimensions + dimType

在維度模型上，這次沒有把行維度和列維度拆成兩套獨立欄位，而是保留統一 `dimensions` ，再透過 `dimType` 區分行列語義：

- 表格模式下繼續使用統一 `dimensions`
- 交叉表模式下透過 `dimType: 'row' | 'column'` 補充語義

這裡體現出的 `superpowers` 價值，不在於它提出了某個欄位名，而在於它推動團隊先把「唯一契約」定下來，再進入實作。 一旦契約明確，模式切換、歷史回填、別名共存這些後續問題都能沿同一條路徑推進，避免出現兩套欄位並行演進、後期再做狀態遷移的額外成本。

### 4.2 參數閉環：先定義 resultDisplayMode 分流規則

真正決定功能是否可用的，不是介面能不能切換，而是參數鏈路是否閉環。 這次在參數層先鎖定一條核心規則：由 `resultDisplayMode` 決定是否進入交叉表分流組參。

在這個前提下：

- `rowDimIds` 從 `dimType === 'row' || !dimType` 的維度裡提取
- `colDimIds` 從 `dimType === 'column'` 的維度裡提取
- 僅在交叉表模式下透出交叉表專屬參數

這背後體現的是一種很典型的 `superpowers` 工作方式： 先確定模式邊界，再讓 AI 生成實作細節。這樣改造過程裡每一段程式碼都在回答同一個問題，即「當前模式該產生什麼參數」，而不是在多個檔案裡各自理解展示語義。

### 4.3 渲染分層：先拆職責邊界，再做 Canvas 化

渲染層同樣不是先寫程式碼，而是先拆邊界。交叉表 Canvas 改造在職責上先分成四類：

- 容器與捲動狀態
- 單元格繪製
- 可視視窗計算
- 渲染契約定義

執行時再分成 `corner` 、 `header` 、 `frozen` 、 `body` 四層畫布。 這樣處理的價值，不只是效能最佳化，更是把後續定位問題的成本降下來。凍結區、捲動區、視窗計算各有獨立職責，出了問題更容易追到具體層，而不是在一個巨大的渲染檔案裡混合排查。

從 `superpowers` 視角看，這一步體現的是「先拆模組責任，再推進實作」的紀律。 AI 很擅長補結構化程式碼，但前提是邊界已經被定義清楚；一旦邊界清晰，AI 在這裡的產出就更容易穩定落到可合併區間。

5. ## 收益：提效不只體現在編碼速度，更體現在返工減少

如果只看 AI 參與了多少程式碼，容易把這次改造理解成「AI 幫忙寫了一部分實作」。 但從實際回顧看， `superpowers` 帶來的收益主要體現在交付過程，而不只是編碼速度。

### 5.1 更值得關注的是流程指標

這類複雜需求裡，更有解釋力的指標不是「AI 寫了多少行」，而是：

- 一次通過率是否提高
- 平均修復輪次是否下降
- 單需求交付週期是否縮短
- 回歸缺陷和返工成本是否下降

這些指標更能直接反映 `superpowers` 的作用，因為它真正優化的是協作過程中的不確定性，而不是單點生成能力。

### 5.2 git-ai 指標記錄

從本次交叉表改造的統計看，AI 參與度本身並不低：

- 統計 commit 數： `4`
- AI 參與 commit 數： `4`
- AI 程式碼占比： `11.3%`
- AI 留存率： `90%`

這些資料說明 AI 輸出並不是實驗性的草稿，而是有相當比例進入了最終結果。 但它們不足以單獨證明「提效」，因為程式碼占比高，不必然代表交付更穩。

真正更有價值的結論是：當 `superpowers` 先完成任務歸一化、上下文收斂、分段驗證和失敗回灌後，AI 輸出更容易進入可合併區間，團隊把更多精力放在架構判斷、邊界處理和聯調收口，而不是反覆修補上下文斷裂帶來的問題。

### 5.3 最終形成的是「AI 提速 + 人工控質」的協作分工

在這次改造裡，AI 更適合承擔：

- 結構化樣板程式碼生成
- 已確定契約下的局部實作補全
- 明確邊界內的重構和拼接

而人工仍然聚焦在：

- 關鍵架構決策
- 模式邊界判斷
- 整合驗證與收口
- 異常路徑和品質把關

`superpowers` 的意義，正是在兩者之間建立明確分工，讓 AI 真的成為提效工具，而不是新的不確定性來源。

6. ## 一套可以遷移到其他複雜需求的協作模板

這次交叉表改造之後，比較值得沉澱的不是某個具體實作細節，而是一套可以複製到其他複雜前端需求裡的協作模板：

1. 先做需求歸一化：明確目標、範圍、非目標、驗收標準
2. 再做上下文收斂：只暴露高相關模組，限制修改半徑
3. 先立關鍵契約：欄位、列舉、模式邊界先統一
4. 再分段實作：每一段都必須可獨立驗證
5. 失敗結構化回灌：基於證據修復，而不是讓 AI 自由猜測

這套模板不只適用於交叉表。 凡是涉及圖表、分析卡、複雜表單、低程式碼配置面板這類跨層聯動需求，本質上都在處理相似問題：需求複雜、模組多、上下文容易斷、整合階段風險高。

當團隊把 `superpowers` 當成工程化協作框架，而不是單純的 AI 功能集合時，收益通常會體現在三個方面：

- 穩定性更高：跨層契約一致性更強
- 返工更少：無效往返和整合爆炸明顯減少
- 可遷移性更強：同一套方法可以複用到下一類複雜改造
7. ## 結論：AI 能提速，但 superpowers 才負責控制複雜度

這次交叉表改造最值得回顧的地方，不是「AI 參與了多少程式碼」，而是複雜需求如何被組織成一條可控的交付鏈路。

如果只有 AI，而沒有明確的任務邊界、上下文策略、階段驗證和失敗回灌，複雜改造依然容易失控。 而 `superpowers` 的價值，正是在這些關鍵環節裡建立秩序，讓 AI 輸出從「可以參考」提升到「可以穩定合併」。

所以更準確的結論應該是：

- AI 負責提速
- `superpowers` 負責控制複雜度
- 兩者結合，才更接近複雜需求下真正可複製的工程化提效

## 最後

歡迎關注【袋鼠雲數棧 UED 團隊】~ 袋鼠雲數棧 UED 團隊持續為廣大開發者分享技術成果，相繼參與開源了歡迎 star

- **[大資料分散式任務排程系統——Taier](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fdtstack.github.io%2FTaier%2F)**
- **[輕量級的 Web IDE UI 框架——Molecule](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fdtstack.github.io%2Fmolecule%2F)**
- **[針對大資料領域的 SQL Parser 專案——dt-sql-parser](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fdtstack.github.io%2Fmonaco-sql-languages%2F)**
- **[袋鼠雲數棧前端團隊程式碼審查工程實踐文件——code-review-practices](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fgithub.com%2FDTStack%2Fcode-review-practices)**
- **[一個速度更快、配置更靈活、使用更簡單的模組打包器——ko](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fgithub.com%2FDTStack%2Fko)**
- **[一個針對 antd 的元件測試工具庫——ant-design-testing](https://link.juejin.cn/?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fgithub.com%2FDTStack%2Fant-design-testing)**

---

原文出處： [https://juejin.cn/post/7647868075887345674](https://juejin.cn/post/7647868075887345674)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qEkP73) [6瀏覽](https://codelove.tw/@tony/post/qEkP73)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qEkP73)

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

340

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780996223&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqEkP73&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780996223841&amp;bpp=2&amp;bdt=95&amp;idt=30&amp;shv=r20260604&amp;mjsv=m202606040101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780973912%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780973912%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780973912%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3202112955230&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95390668%2C95392169%2C95390278&amp;oid=2&amp;pvsid=288856944573060&amp;tmod=1659409420&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=60" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>