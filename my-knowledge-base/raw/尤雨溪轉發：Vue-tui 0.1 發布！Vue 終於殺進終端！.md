---
title: "尤雨溪轉發：Vue-tui 0.1 發布！Vue 終於殺進終端！"
source: "https://codelove.tw/@tony/post/3kVVOx#google_vignette"
author:
published:
created: 2026-06-29
description: "尤雨溪轉發：Vue-tui 0.1 發布！Vue 終於殺進終端！  ===================================    做過終端 CLI、命令列互動工具的前端應該都知道：以前想做好看、可互動的終端介面，基本上只能用 **React Ink**。Vue 開發者一直很..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月29日](https://codelove.tw/@tony/post/3kVVOx "2026-06-29 09:12:05")

## 尤雨溪轉發：Vue-tui 0.1 發布！Vue 終於殺進終端！

## 尤雨溪轉發：Vue-tui 0.1 發布！Vue 終於殺進終端！

```
做過終端 CLI、命令列互動工具的前端應該都知道：以前想做好看、可互動的終端介面，基本上只能用 **React Ink**。
```

Vue 開發者一直很被動：寫整站 Vue，唯獨做終端工具，卻必須切換成 React 語法。

現在 **vue-tui** 來了，Vue 官方原生終端 UI 框架，徹底解決這個痛點。

[![](https://i.imgur.com/31JB6UL.jpeg)](https://i.imgur.com/31JB6UL.jpeg)

## 不用 React，Vue 開發者專屬優勢

!\[圖片\](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns=' [www.w3.org/2000/svg](https://link.juejin.cn/?target=http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg) ' xmlns:xlink=' [www.w3.org/1999/xlink'…](https://link.juejin.cn/?target=http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%27%253E%253Ctitle%253E%253C%2Ftitle%253E%253Cg) stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E) [![](https://i.imgur.com/Yus1JlC.jpeg)](https://i.imgur.com/Yus1JlC.jpeg)

以往開發互動式終端介面，只能依賴 React Ink，Vue 開發者被迫切換技術棧，成本極高。而 vue-tui 完美適配 Vue 技術棧，優勢拉滿：

**1\. 技術棧無縫統一** 原生支援 Vue3、setup 語法、SFC 單檔元件、響應式 API，日常開發的 Vue 語法可直接復用，無需學習 React 相關知識。

**2\. 極致開發體驗** 支援終端 HMR 熱更新，修改程式碼即可即時刷新介面，無需重啟服務，大幅提升 CLI 開發效率。

**3\. 自帶工程化能力** 官方配套測試工具，支援元件級終端測試，不必手動封裝複雜的終端模擬邏輯。

**4\. 貼合 Vue 開發思維** 透過組合式 API 管理終端輸入、焦點、視窗狀態，和網頁開發邏輯一致，沒有學習割裂感。

## 極簡實用程式碼範例

全程原生 Vue 寫法，簡單易上手，涵蓋核心互動、掛載、測試場景。

### 基礎互動計數器（SFC 完整版）

```
xml 代码解读复制代码<script setup lang="ts">import { shallowRef } from "vue";import { Box, Text, useInput } from "@vue-tui/runtime";// 純 Vue 響應式狀態const count = shallowRef(0);// 監聽終端鍵盤輸入useInput((input) => {  if (input === "+") count.value++;  if (input === "-") count.value--;});</script><template>  <Box gap={2}>    <Text>計數：</Text>    <Text bold color="green">{{ count }}</Text>    <Text dimColor>按 +/- 調整數值</Text>  </Box></template>
```

### 專案入口檔案

```
javascript 代码解读复制代码import { createApp } from "@vue-tui/runtime";import App from "./app.vue";// 直接掛載到終端createApp(App).mount();
```

### 元件單元測試範例

自帶測試庫，一鍵模擬鍵盤輸入、驗證終端輸出：

```
python 代码解读复制代码import { test, expect } from"vitest";import { render } from"@vue-tui/testing";import Counter from"./app.vue";test("計數器正常回應按鍵", async () => {const { lastFrame, stdin } = await render(Counter);  expect(lastFrame()).toContain("計數：0");// 模擬按 + 鍵await stdin.write("+");  expect(lastFrame()).toContain("計數：1");});
```

## 終端版面與樣式：極簡 Flex 寫法

基於 Yoga 版面引擎（和 React Ink、React Native 同源），摒棄繁瑣的字元排版，直接使用前端通用的 Flex 版面，零學習成本。

**Box 版面容器** 終端核心容器，支援橫向、直向排列、間距、邊距、對齊、邊框背景，屬性和 CSS Flex 基本一致。

**Text 文字樣式** 支援粗體、變色、灰階、底線、文字截斷，快速實現各類終端文字樣式。

**Spacer 自適應占位** 自動填充剩餘空間，輕鬆實現兩端對齊、平均分配版面效果。

### 簡單版面案例（兩端對齊狀態列）

```
xml 代码解读复制代码<template>  <Box direction="row" justifyContent="space-between" gap={3}>    <Text color="blue">終端工具 v1.0</Text>    <Spacer />    <Text dimColor>執行中</Text>  </Box></template>
```

## 全方位終端互動能力

[![](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/716a2f6cd8a246679161f07dd695ee88~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgRm9yZXZlcjdf:q75.awebp?rk3s=f64ab15b&x-expires=1783042644&x-signature=QH%2BSQ%2FvKQ1l0R8xZRw9wHUQdrSs%3D)](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/716a2f6cd8a246679161f07dd695ee88~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgRm9yZXZlcjdf:q75.awebp?rk3s=f64ab15b&x-expires=1783042644&x-signature=QH%2BSQ%2FvKQ1l0R8xZRw9wHUQdrSs%3D)!\[圖片\](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns=' [www.w3.org/2000/svg](https://link.juejin.cn/?target=http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg) ' xmlns:xlink=' [www.w3.org/1999/xlink'…](https://link.juejin.cn/?target=http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%27%253E%253Ctitle%253E%253C%2Ftitle%253E%253Cg) stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)框架封裝終端底層邏輯，無需手動操作讀寫串流，開箱即用所有互動能力：

- **鍵盤監聽** ：捕捉所有按鍵、方向鍵、修飾鍵，快速做互動指令
- **焦點管理** ：支援 Tab 切換焦點、手動聚焦，適配終端表單
- **貼上事件** ：完整捕捉批量貼上文字，適配 AI 對話、批次輸入場景
- **視窗適配** ：即時監聽終端寬高，自動適配視窗縮放
- **動畫能力** ：內建幀動畫，可開發終端小遊戲、動態進度介面
- **無互動輸出** ：支援字串渲染，適配 CI 日誌、流水線快照

## Vue 開發者要不要入坑？

**結論先行：非常值得。**

只要你需要做 **Node 腳手架、CLI 工具、終端面板、本地 AI 終端用戶端、維運監控介面** ，vue-tui 就是最優解。

**核心入坑理由**

1. 不用切換技術棧，Vue 經驗零浪費
2. 開發體驗輾壓傳統終端開發，熱更新、元件化、可測試
3. Vue 官方維護，生態可靠，核心 API 已經穩定

---

原文出處： [https://juejin.cn/post/7655263874610085929](https://juejin.cn/post/7655263874610085929)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3kVVOx) [6瀏覽](https://codelove.tw/@tony/post/3kVVOx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3kVVOx)

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

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝13 💬2 ❤️1

571

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

71

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782698931&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3kVVOx&amp;pra=5&amp;asro=0&amp;itsi=17&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782698931179&amp;bpp=1&amp;bdt=131&amp;idt=11&amp;shv=r20260625&amp;mjsv=m202606240101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782697603%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782697603%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782697603%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3612191789465&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1605&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099346%2C31099360&amp;oid=2&amp;pvsid=4150226939915269&amp;tmod=636781205&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3kVVOx&amp;fc=1920&amp;brdim=1930%2C30%2C1930%2C30%2C1920%2C30%2C1845%2C985%2C1605%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=16" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260625/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>