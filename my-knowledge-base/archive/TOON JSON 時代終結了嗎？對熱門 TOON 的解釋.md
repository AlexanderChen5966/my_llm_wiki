---
title: "[TOON] JSON 時代終結了嗎？對熱門 TOON 的解釋"
source: "https://codelove.tw/@tony/post/qzDQQ3"
author:
published:
created: 2026-05-25
description: "## 引言你是否曾經對 LLM 的 API 成本感到擔憂？如果傳遞給提示的大量數據，令標記數量激增，那麼成本和響應速度都會受到影響。造成這一問題的原因之一就是 JSON。因為 JSON 是為了讓人類閱讀而設計，所以符號和重複的內容非常多，對 LLM 來說變成了冗餘的格式。因此，「TOON..."
tags:
  - "clippings"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [11月22日](https://codelove.tw/@tony/post/qzDQQ3 "2025-11-22 12:12:35")

## \[TOON\] JSON 時代終結了嗎？對熱門 TOON 的解釋

## 引言

你是否曾經對 LLM 的 API 成本感到擔憂？如果傳遞給提示的大量數據，令標記數量激增，那麼成本和響應速度都會受到影響。

造成這一問題的原因之一就是 JSON。因為 JSON 是為了讓人類閱讀而設計，所以符號和重複的內容非常多，對 LLM 來說變成了冗餘的格式。

因此，「TOON（Token-Oriented Object Notation）」應運而生。這是一種專為 LLM 優化的數據格式，據說與 JSON 相比可以減少 30% 至 60% 的標記數。

本文將具體說明 TOON 的優點以及如何使用，並附上實例進行說明。

## JSON 的問題在哪裡

讓我們看個具體例子。商品數據使用 JSON 描述為：

```
{
  "products": [
    {
      "id": 301,
      "name": "Mouse",
      "price": 29.99
    },
    {
      "id": 302,
      "name": "Keyboard",
      "price": 89.00
    },
    {
      "id": 303,
      "name": "Hub",
      "price": 45.50
    }
  ]
}
```

問題顯而易見。 `"id":`、 `"name":`、 `"price":` 這些鍵名按照商品數量重複出現。

如果有100個商品，僅這些鍵名就會被發送300次。這就是標記的浪費。

## TOON 的寫法

TOON 則消除了這些重複。在最開始只宣告一次字段名，後面只列出值。

同樣的數據用 TOON 表達為：

```
products{id,name,price}:
  301,Mouse,29.99
  302,Keyboard,89.00
  303,Hub,45.50
```

就這麼簡單。

**發生了什麼：**

- 鍵名（ `"id":`、 `"name":`、 `"price":`）只宣告一次
- 刪除了花括號 `{}` 的重複
- 縮減了引號 `"` 的使用
- 縮減了逗號和冒號的數量

結果，使用 **約少了 40% 的標記** 來表達相同的信息。

## 另一個例子：用戶列表

讓我們再看一個更簡單的例子。

**JSON：**

```
{
  "users": [
    {
      "id": 1,
      "name": "Alice",
      "role": "admin"
    },
    {
      "id": 2,
      "name": "Bob",
      "role": "user"
    }
  ]
}
```

**TOON：**

```
users{id,name,role}:
  1,Alice,admin
  2,Bob,user
```

一目了然，TOON 的格式更加簡潔且易讀。

## TOON 的優點

### 1\. 減少標記與成本

LLM 的 API 是根據標記數量進行收費的。TOON 通過刪除 JSON 的冗餘符號，能夠以 30% 至 60% 更少的標記傳送相同的信息。

實際測試案例：

- GitHub 倉庫數據（100條）：JSON 15,145 標記 → TOON 8,745 標記（減少 42%）
- 日分析數據（180天）：JSON 10,977 標記 → TOON 4,507 標記（減少 59%）

使用 GPT-4o 時：

- 輸入：每 100 萬標記 $5
- 輸出：每 100 萬標記 $15

如果每天向 LLM 傳送 100 萬標記的數據，僅僅改用 TOON 就可以每月節省 75 到 150 美元的成本。對於大規模系統，年節省額可達數十萬至數百萬。

### 2\. 響應速度提升與上下文利用

隨著標記數的減少，LLM 的處理時間也會縮短。特別是對於 RAG 或代理等大量使用上下文窗口的應用，響應時間的改善會變得顯著。

更重要的是，可以更有效地利用有限的上下文窗口。假如上下文窗口為 128K 標記，使用 JSON 只能傳遞約 200 條搜尋結果，但使用 TOON 則可以傳遞約 350 條。這樣能夠使用更多的信息，提高 LLM 的回答精度。

### 3\. 推理精度與搜尋精度的提升

通過以更乾淨的形狀向 LLM 提供結構化數據，推理質量隨之上升。JSON 的冗餘符號和重複內容對 LLM 來說有時候會形成「噪音」。TOON 將這些噪音降到最低，明確展示數據的本質結構。這樣一來，LLM 更容易正確理解數據之間的關係。

特別是在 RAG 系統中搜索精度的提升得到了報告。當以 TOON 格式將搜尋結果傳入 LLM 時，準確提取相關信息的概率會增加。表格形式的數據中，字段名與值的關係更加明確，因此 LLM 誤解的可能性也降低。

實際效果：

- RAG 中相關信息提取：與 JSON 相比，有約 15% 精度提升的案例
- 表格數據的聚合與分析：字段名的誤認識大幅減少
- 多數據源的整合：結構的一致性更容易保持

TOON 特別在數據庫查詢結果、RAG 搜尋結果、CSV 等表格數據、日誌數據和時序數據等「同一字段重複」的數據結構中展現了強大的效果。

### 4\. 與 JSON 完全兼容

TOON 一個重要的特徵是，它能與 JSON 完全互轉。TOON 表達的數據都可以退回到 JSON，反之亦然。這意味著無需放棄現有的 JSON 資產，可以根據需求隨意轉換 TOON 和 JSON。

TOON ⇔ JSON 轉換過程中不會有信息的丟失，數值和字符串等類型信息也會得到保持。甚至複雜的嵌套結構也可以進行雙向轉換。由於這種兼容性，API 可以使用 JSON，內部處理則僅使用 TOON。

循序漸進的導入也是它的一大優勢。無需一次性全部切換，而是可以從效果顯著的部分逐步轉向 TOON。

## 圖解理解：數據流

JSON 的轉換步驟繁多，而 TOON 則可以從一開始就有效率地處理。這是個簡單的圖示，但此差異直接影響延遲和成本。

## 實際使用方法

### 從 JSON 轉換為 TOON

TOON 在多種語言中提供官方 SDK：

- TypeScript/JavaScript: `@toon-format/toon`
- Python: `python-toon`
- .NET: `NIZZOLA.TOON.NET`
- Elixir、R、Gleam 等等

基本流程如下：

```javascript
import { stringify, parse } from '@toon-format/toon';

// 準備 JSON 數據
const data = {
  users: [
    { id: 1, name: 'Alice', role: 'admin' },
    { id: 2, name: 'Bob', role: 'user' }
  ]
};

// 轉換為 TOON
const toonString = stringify(data);

// 傳送給 LLM
const prompt = \`請從以下用戶數據中提取管理者:\n${toonString};\`;

// 根據 LLM 回應的 TOON 進行還原
const result = parse(llmResponse);
```

### 導入模式

推薦的導入方法：

1. **僅將 LLM 的輸入輸出改為 TOON**
	- 現有系統保持 JSON
		- 在傳送給 LLM 之前轉換為 TOON
		- 接收自 LLM 的結果後馬上還原為 JSON
		- 因為能夠完全保護數據的完整性，所以對現有系統沒有影響
2. **逐步擴展**
	- 首先從最消耗標記的部分入手
		- 测量效果，然後再擴大範圍
3. **一定要進行 A/B 測試**
	- 比較 JSON 與 TOON 在精度和成本上的差異
		- 在自己的數據上進行實測

## 適用與不適用的情況及注意事項

### TOON 適用的場景

判斷標準很簡單。如果你向 LLM 傳遞大量的表格數據，且 LLM API 的成本超過每月數萬元，則值得一試。當 RAG 系統的搜尋結果有幾十到幾百條數據時，或者當直接分析數據庫的查詢結果時，或在處理日誌和時序數據時，這類「同一字段重複」的結構 TOON 將最大化其效果。

如果你在上下文窗口的限制上遇到困擾，TOON 更是一個出色的選擇。即使是相同的窗口大小，也能容納更多的資訊。

### TOON 不適用的場景及注意事項

另一方面，TOON 不是萬能的。

**1\. 對複雜嵌套結構較弱**

TOON 最適合於表格數據。對於深度嵌套的數據或結構不均勻的數據則不太適用。例如，像公司組織圖那樣部門→團隊→成員的深度嵌套結構，使用 JSON 或 YAML 更為方便。

**2\. LLM 對 TOON 的熟悉度不足**

LLM 訓練時接觸了大量的 JSON，但對 TOON 了解甚少。因此在簡單數據提取任務中 TOON 可能會佔優勢，但是在複雜推理任務中可能 JSON 更精確。獨立基準測試中報告 TOON 在深度嵌套的數據上精度不及 JSON 或 YAML 的結果。

**3\. 尚未標準化**

TOON 是較新的技術，尚未形成行業標準。雖然主要語言的 SDK 都在增長，但與 Protobuf 或 JSON 的成熟度相比，仍不夠強大。

**4\. 如果需要謹慎**

在醫療或金融等高精度優先的領域應該謹慎。如果 LLM API 的使用量較小，或者將其整合既有系統的成本較高，則可能不會得到相應的收益。

### 推薦的推進方式

小規模開始。先在小規模測試中比較 JSON 和 TOON。在標記減少率和精度上進行實測，只有在有效的部分進行 TOON 化。然後逐步擴大範圍。現有的 API 和數據庫保留 JSON，僅將 LLM 接口改為 TOON 的方法最為現實。

## 總結

TOON 不是 JSON 的替代品，而是專為 LLM 優化的數據傳送格式。

TOON 解決的問題是 LLM API 的標記數和成本、上下文窗口的限制、響應時間的改善等實務問題。當面對大量表格數據（例如 RAG 的搜尋結果、數據庫結果、日誌等）時，效果尤為顯著。

不過對於深度嵌套結構、複雜推理任務，以及高精度所需的情境則不適合。LLM 較熟悉 JSON，但對 TOON 的認識仍然有限。

這並不意味著 JSON 的時代已經結束。TOON 作為用於 LLM 接口的優化工具，將與 JSON 共同存在。導入應該從小規模開始，測量效果後再進行擴展。這是降低風險並最大化收益的明智之舉。

---

原文出處： [https://qiita.com/shanks665/items/a5ec31706af9ffffc491](https://qiita.com/shanks665/items/a5ec31706af9ffffc491)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qzDQQ3) [494瀏覽](https://codelove.tw/@tony/post/qzDQQ3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qzDQQ3)

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

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779686231&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqzDQQ3&amp;pra=5&amp;asro=0&amp;itsi=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779686231836&amp;bpp=1&amp;bdt=54&amp;idt=29&amp;shv=r20260520&amp;mjsv=m202605190101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779686067%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779686067%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5628026436018&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95391808&amp;oid=2&amp;pvsid=3450394061712918&amp;tmod=1302172221&amp;uas=1&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqzDQQ3&amp;fc=1920&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=33" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260520/r20190131/zrt_lookup_fy2021.html#RS-0-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>