---
title: "一套 Spec-First 的 AI 編程工作流程"
source: "https://codelove.tw/@tony/post/xMVeX3"
author:
published:
created: 2026-06-22
description: "一套 Spec-First 的 AI 編程工作流  ===========================    > 我們是[袋鼠雲數棧 UED 團隊](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月22日](https://codelove.tw/@tony/post/xMVeX3 "2026-06-22 10:07:35")

## 一套 Spec-First 的 AI 編程工作流

```
> 我們是[袋鼠雲數棧 UED 團隊](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fwww.dtstack.com%2Fued)，致力於打造優秀的一站式資料中台產品。我們始終保持工匠精神，探索前端道路，為社群累積並傳播經驗價值。本文作者：霽明
```

## 前言

這半年，伴隨公司內 AI Coding 事項的推進，AI 編程工具（Cursor、Codex、Claude Code 等）在公司內部普及得很快，大家應該都有在使用 AI 輔助編程。我想大家應該有發現一件事：讓 AI「直接寫程式」和讓 AI「先寫方案再寫程式」，得到的結果會不一樣。

這篇文章主要是：我最近使用 AI 輔助編程的工作流是怎樣的。

1. ## 先說個常見的情況

大家應該有遇到過這種情況：接到一個需求，圖省事，直接一句話丟給 AI ——「幫我實作 xxx」，然後它「啪」地給你寫了一大坨程式碼。

乍看之下挺像樣，但細看就會冒出一些問題：它可能順手改了幾個你沒讓它動的檔案、自己腦補了一些你沒交代過的邊界處理，或者明明專案裡已經有現成的工具函式，它非要自己再寫一個。最後你花在「讀它到底寫了什麼、再把多餘的改動挑出去」上的時間，未必比自己寫少多少。

這不是 AI 不行，而是它在 **資訊不全的時候只能靠猜** 。我們沒把「要做什麼、不要做什麼」講清楚，它就只好替我們做主。

2. ## Spec-First 工作流：先約定，再實作

### 2.1 一句話定義

**做需求前，先讓 AI 輸出一份可被人工 review 的實作方案（spec），人工確認後再讓它按方案寫程式。**

注意三個關鍵詞：

- **可被 review** ：方案是中文說明，不是直接的程式碼，其他同學也能看得懂
- **人工確認** ：spec 要通過確認後才能讓 AI 去執行，避免在錯誤前提上累積工作量
- **按方案寫** ：第二輪 prompt 把 spec 餵回去，讓 AI 按既定方案寫，約束自由發揮的空間

### 2.2 五步概覽

```
plain 代碼解讀複製代碼1. 餵上下文：提示詞、產品需求說明、設計稿、AGENTS.md、相關程式碼/文件、...
2. 出方案：讓 AI 輸出 spec
3. Review 改方案：重要的一步，人工主導，必須經人工確認後才可進行下一步
4. 按方案實作：讓 AI 嚴格依據 spec 編碼
5. 驗收 review：AI review + 人工 review
```

### 2.3 適用 / 不適用

## 場景是否適用備註新功能開發✅最佳場景舊模組重構✅spec 階段需要先對齊邊界技術方案調研✅spec 就是調研報告本身 Bug 修復⚠️修復 bug 主要在於問題排查，改動往往是小範圍的，效果有限純 Code Review❌程式碼審查是為了提前發現問題，沒有必要3. 完整案例

### Step 1 · 給 AI 足夠的上下文

AI 出方案之前，先要讓它「看到」足夠的資訊。對於一個常規需求，我們的輸入主要有：

- 使用者提示詞（手動撰寫，需要包含基本的一些描述）
- 需求說明（形式可以為截圖或文字描述，需要注意去識別化）
- figma 設計稿（形式可以是截圖或 MCP 讀取，推薦使用 MCP）
- [AGENTS.md](http://agents.md/) （agent 自動讀取）

### Step 2 · 讓 AI 出方案

上下文準備好之後， **這一輪的目標不是寫程式碼，是寫方案** 。給 AI 的提示詞大致是這樣：

```
plain 代碼解讀複製代碼我要在選單下，新增一個模組：
- 需求原型見上傳的截圖（需要手動上傳）
- figma設計稿如下：
https://www.figma.com/xxx

請根據需求原型、figma設計稿和當前程式碼上下文輸出詳細實作的方案。

注意：
- 需求原型截圖和figma設計稿中的頂部導覽列和側邊選單欄是既有的，不需要實作
- 原型截圖中的紅色文案是業務邏輯相關描述，要重點關注
- 不清楚的地方需要和我進行確認，禁止隨意推測
```

可以指定編程工具當前為 plan 模式（這個 claude、codex、cursor 都支援），輸出實作方案。

AI 輸出方案示例：

[![image (7).png](http://openwrite.cn/uploads/12244/63745/2dc7d106-2cef-4e94-a84b-967385364f71.png)](http://openwrite.cn/uploads/12244/63745/2dc7d106-2cef-4e94-a84b-967385364f71.png)

### Step 3 · 人工 Review 並修正方案

我們 review 方案的時候通常會改這幾類東西：

1. **需求理解偏差** ：AI 複述的需求和我們想的不一樣，及時糾正，spec 階段發現成本最低
2. **越界改動** ：AI 順手要改一些跟當前需求無關的檔案，要砍掉，保持最小範圍
3. **重複造輪子順序錯** ：AI 想自己新寫一個工具函式，但目前專案裡已經有了
4. **假設沒說明** ：AI 默默地假設了一些邊界條件，要讓它顯式寫出來或主動進行確認
5. **遺漏邊緣情況** ：載入狀態、空狀態、權限、異常分支等

Review 完後讓 AI 重新生成 spec，再進行確認， **直到 spec 雙方都認可，才進入 Step 4。**

示例：

[![image (8).png](http://openwrite.cn/uploads/12244/63745/92df6ab7-4c1e-4a0d-b2cd-8feadd4a5ffc.png)](http://openwrite.cn/uploads/12244/63745/92df6ab7-4c1e-4a0d-b2cd-8feadd4a5ffc.png)

### Step 4 · 讓 AI 按方案實作

第二輪的提示詞示例：

```
plain 代碼解讀複製代碼請嚴格按照上面確認的方案實作。
- 不引入方案之外的改動
- 實作完成後，請按方案的「實作步驟」逐條說明你做了什麼
- 如果實作過程中發現方案有問題，先停下來告訴我，不要自行調整
```

或者有的 AI 工具直接提供了 spec 的 build 按鈕，可以不需要額外提示詞。

AI 生成程式碼示例：

\`\`tsx 代碼解讀複製代碼import React, { useCallback, useEffect, useState } from 'react';

import OverviewPanel from './components/overviewPanel';  
import RecordTable from './components/recordTable';  
//...省略

const ApiMonitor: React.FC = () => {  
const \[overviewData, setOverviewData\] = useState\<IApiMonitorOverview>();  
const \[overviewLoading, setOverviewLoading\] = useState(false);

//...省略

return (  
\<div className="api-monitor">  
\<OverviewPanel data={overviewData} loading={overviewLoading} />  
<RecordTable  
data={recordData}  
loading={recordLoading}  
total={total}  
current={listParams.current?? 1}  
pageSize={listParams.size?? DEFAULT\_PAGE\_SIZE}  
sortField={sortField}  
sortOrder={sortOrder}  
searchForm={  
<SearchForm  
callerOptions={callerOptions}  
onSearch={handleSearch}  
onReset={handleReset}  
/>  
}  
onPageChange={handlePageChange}  
onTableChange={handleTableChange}  
/>  
\</div>  
);  
};

export default ApiMonitor;

```
頁面效果：

![image (9).png](http://openwrite.cn/uploads/12244/63745/dac96a6d-da76-4ba8-aaac-e56aaf96be8b.png)

### Step 5 · 對照 Spec 做程式碼 Review

可以讓 AI 根據 spec 先 review 一遍，再人工進行 review，​**強烈建議大家做好本地 AI + 自己人工 review 這一步**​，可以大幅減少 MR review 者的工作量。

對於 MR review，我們主要都是在 gitlab 頁面上人工 review，可以使用 gitlab mcp 取得 MR 資訊，然後讓 AI 給出詳細 review 意見，本地 AI review 和 gitlab 上的 AI review 不太一樣，本地上下文會更全，也可以使用更強的 AI 模型，review 效果一般會比 gitlab 上的好。經過 AI review 後，**仍然需要人工仔細 review ​** 一遍。

補充說明：

- 使用 gitlab mcp 可能會導致上下文比較大，可以使用內部的 gitlab-MR-code-review skill 替換
- 也可以使用 Superpowers 的 code review 功能

5. 讓 Spec-First 跑順的「基礎設施」
-------------------------

工作流要跑順，靠的不是某次 prompt 寫得好，而是底下基礎設施在持續起作用。

### 5.1 AGENTS.md：始終在線的「團隊 spec」

\`AGENTS.md\` 是倉庫根目錄的一份 Markdown 檔案，被 Cursor、Codex 等工具識別後​**每次會話自動載入**​。它解決了一個核心問題：團隊約定不需要每次都靠人工敲進 prompt，AI 預設就遵守。

我們的 AGENTS.md 分四層（按優先級從高到低）：

層級關注點例子**安全邊界**哪些事不能做不讀 \`.env\`、不自動 \`git push\`、不主動 \`pnpm install\`**倉庫邊界**改動作用域monorepo 多產品隔離、跨產品改動須確認**工程約束**技術棧 / 重複造輪子順序React、重複造輪子順序 公共包 → antd → 新增**行為約定**溝通 / 驗證預設中文溝通、交付前必須跑 lint + check-types下面是真實的一段摘錄，感受一下「硬規則」的寫法：
```

markdown 代碼解讀複製代碼### 3.3 安全邊界

- 不自動提交程式碼、不自動推送分支、不改寫 git 歷史；  
	commit / push / reset / rebase / git add. 等寫入類操作  
	僅在使用者明確要求時執行。
- 不讀取.env\*、應用執行期設定以及金鑰/憑證類檔案；  
	不修改 node\_modules/、建置產物、工具快取目錄。
- 不主動執行 curl / wget / ssh / scp 以及帶 deploy /  
	release / publish 關鍵字的命令。
- 非必要不新增依賴、不執行 pnpm install / add / update。
- 不主動啟動 pnpm dev / pnpm start 等長駐服務。
```
### 5.2 MCP：Spec 的輸入端

MCP 讓 AI 能像調工具一樣直接讀外部系統，它們為什麼重要：

以前給 AI 餵上下文，靠的是「人肉中轉」——自己看 Figma、整理成一段文字再貼給它，光這一步就能耗掉小半天。MCP 把這層中轉去掉了，讓 AI 直接拿到你能拿到的東西。

MCP 的本質就是​**讓 AI 拿到你拿到的東西**​。Spec-first 之所以能跑順，很大程度上靠 MCP 把「上下文餵入」這一步的人力成本降了下來。

6. 還沒解決的問題
----------

誠實講，這套工作流還有幾個我們沒解決的事，列出來一起想：

### 6.1 怎麼知道這套真的有效？

目前我們對「AI 幫我們提了多少效率」基本靠感覺。為了讓這件事可量化，我們最近在做一個 \`ai-coding-stats\` 的 skill，基於 git-ai 在 commit 上記錄的中繼資料，能統計出 AI 程式碼有效採納率、AI 程式碼提交留存率、AI 程式碼占比等指標。

個人維度已經能跑，團隊維度（跨倉庫聚合、視覺化看板）還在跟進中。

### 6.2 Spec-Driven Development 的正式形態是怎樣的

我目前的做法本質上是「輕量版 Spec-Driven Development（SDD）」。SDD 作為一個更系統的方法論，把 spec 的形態、版本管理、自動化測試生成都做了更完整的設計。這塊我也還在學，歡迎討論。

### 6.3 跨職位的延伸

這套工作流不只前端能用，但不同職位用起來的角度可能不一樣：

- ​**後端**​：spec 階段是不是天然適合做介面設計？
- ​**測試**​：能不能拿 spec 直接當測試案例的輸入？
- ​**產品**​：spec 是不是可以為 PRD 查缺補漏？需求文件的顆粒度需不需要為「AI 友好」做調整？
- ​**維運**​：基礎設施變更能不能也跑 spec-first？

7. 總結
-----

整篇重點：

1. ​**讓 AI 先寫方案，再讓它按方案寫程式碼**​，**review 環節要重視**
2. **用 AGENTS.md 把團隊規範沉澱成 AI 預設能看到的上下文**
3. **用 MCP 讓 AI 拿到你想拿到的東西**

最後一句話：

AI Coding 不是「AI 幫我們寫程式碼」，而是「我們用一種新的方式表達意圖，由 AI 把意圖翻譯成程式碼」。**意圖表達得越清楚，AI 的產出就越接近你想要的。** Spec-first 就是把意圖表達清晰化的一個具體實踐。

最後
--

歡迎關注【袋鼠雲數棧 UED 團隊】~ 袋鼠雲數棧 UED 團隊持續為廣大開發者分享技術成果，相繼參與開源了歡迎 star

- **[大數據分散式任務排程系統——Taier](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fdtstack.github.io%2FTaier%2F)**
- **[輕量級的 Web IDE UI 框架——Molecule](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fdtstack.github.io%2Fmolecule%2F)**
- **[針對大數據領域的 SQL Parser 專案——dt-sql-parser](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fdtstack.github.io%2Fmonaco-sql-languages%2F)**
- **[袋鼠雲數棧前端團隊程式碼審查工程實踐文件——code-review-practices](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fgithub.com%2FDTStack%2Fcode-review-practices)**
- **[一個速度更快、設定更靈活、使用更簡單的模組打包器——ko](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fgithub.com%2FDTStack%2Fko)**
- **[一個針對 antd 的元件測試工具庫——ant-design-testing](https://link.juejin.cn?target=https%3A%2F%2Flink.zhihu.com%2F%3Ftarget%3Dhttps%253A%2F%2Fgithub.com%2FDTStack%2Fant-design-testing)**

---

原文出處：https://juejin.cn/post/7652581847889330202
```

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xMVeX3) [7瀏覽](https://codelove.tw/@tony/post/xMVeX3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xMVeX3)

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

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝16 💬1 ❤️1

507

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

30

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782108370&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxMVeX3&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782108370584&amp;bpp=1&amp;bdt=164&amp;idt=89&amp;shv=r20260617&amp;mjsv=m202606170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1781666864%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1781666864%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1781666864%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=2509428388188&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099218&amp;oid=2&amp;pvsid=73151860518712&amp;tmod=1256732551&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=112" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260617/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>