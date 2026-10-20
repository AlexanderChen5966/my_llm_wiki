---
title: "CLAUDE.md 設計模式集──依專案規模分別使用的 7 種範本"
source: "https://codelove.tw/@tony/post/3KKzQ3"
author:
published:
created: 2026-07-23
description: "[](#%E7%B5%90%E8%AB%96claude-code-%E3%81%AE%E5%87%BA%E5%8A%9B%E5%93%81%E8%B3%AA%E3%82%92%E6%9C%80%E3%82%82%E5%B7%A6%E5%8F%B3%E3%81%99%E3%82%8B%E3%81%A..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月23日](https://codelove.tw/@tony/post/3KKzQ3 "2026-07-23 08:15:10")

## CLAUDE.md 設計模式集──依專案規模分別使用的 7 種範本

## 結論：Claude Code 的輸出品質最左右的，不是 prompt 也不是模型，而是 CLAUDE.md 的寫法。

在導入 Claude Code 的團隊中，若要找出「能產出符合預期的程式碼」的團隊與「每次都需要手動修正」的團隊之間的差異，最後會發現關鍵在於 **[CLAUDE.md](http://claude.md/) 的設計品質** 。

本文將介紹依專案規模區分使用的 **7 種 [CLAUDE.md](http://claude.md/) 範本** 。從個人腳本到大型 monorepo，都能直接複製使用的實戰模式集。

---

## 環境・前提條件

- **Claude Code** CLI 已安裝完成
- Shell 環境預設為 macOS / Linux / WSL
- 文章中的範本以 Markdown 格式撰寫

---

## 1\. CLAUDE.md 是什麼──角色、載入順序、作用範圍的機制

### 角色

[CLAUDE.md](http://claude.md/) 是 Claude Code 在產生與編輯程式碼時， **會自動讀取的專案指示文件** 。它可以集中管理那些不必每次寫進 prompt 的「隱含脈絡」。

主要會定義以下內容：

- 專案的技術棧與架構方針
- 程式碼風格規範與命名規則
- 不可執行的操作（反模式）
- 與測試、建置、部署相關的指令與限制

### 載入順序與作用範圍的階層結構

[CLAUDE.md](http://claude.md/) 可以放在 **3 種作用範圍** 中，且越下層優先度越高、會進行合併。

| 作用範圍 | 路徑範例 | 用途 | 共享範圍 |
| --- | --- | --- | --- |
| 使用者全域 | `~/.claude/CLAUDE.md` | 個人偏好（語言、風格） | 只有自己 |
| 專案根目錄 | `./CLAUDE.md` | 技術棧、規範 | 團隊全體（Git 管理） |
| 子目錄 | `./packages/api/CLAUDE.md` | 套件專屬限制 | 團隊全體（Git 管理） |

**重點：** 子目錄的 [CLAUDE.md](http://claude.md/) ，會在 Claude Code 操作該目錄內的檔案時 **額外** 讀取。最佳做法不是覆蓋根目錄內容，而是作為補充來設計。

---

## 2\. 個人腳本向：極簡型範本

**預期規模：** 1 人 / 檔案數 1～10 / 一次性～小型工具

最大重點是「不要寫太多」。對個人腳本來說，寫一份冗長的 [CLAUDE.md](http://claude.md/) 是過度工程化。

```
# CLAUDE.md

## 專案概要
CLI 工具。從標準輸入讀取 CSV，並將彙整結果以 JSON 輸出。

## 技術棧
- Python 3.12
- 不可使用外部函式庫（僅限標準函式庫）

## 程式碼風格
- 必須使用型別提示
- 函式需撰寫 1 行的 docstring

## 建置・執行
- 執行：\`python main.py < input.csv\`
- 測試：\`python -m pytest tests/\`
```

**範本大小建議：10～20 行**

你真正需要的只有三件事：「在做什麼」「可以使用哪些工具」「要怎麼執行」。

---

## 3\. 中規模 Web 應用向：分層型範本

**預期規模：** 2～5 人 / 檔案數 50～300 / Web 應用程式

透過明確定義分層式架構的邊界，Claude Code 才能正確判斷「哪個邏輯應該放在哪個檔案」。

```
# CLAUDE.md

## 專案概要
內部用庫存管理系統（SaaS）。

## 技術棧
- Backend: TypeScript / NestJS / Prisma / PostgreSQL
- Frontend: TypeScript / Next.js（App Router）/ Tailwind CSS v4
- 測試: Vitest（單元）/ Playwright（E2E）
- CI: GitHub Actions

## 架構方針
### 目錄結構與責任
- \`src/domain/\` … 領域模型・商業規則。禁止相依外部套件。
- \`src/application/\` … 使用案例。只可 import domain。
- \`src/infrastructure/\` … DB・外部 API 連接。Prisma Client 只允許放在這裡。
- \`src/presentation/\` … Controller / DTO / 驗證。

### 相依方向（嚴格遵守）
presentation → application → domain ← infrastructure

### 程式碼風格
- 變數／函式：camelCase / 類別：PascalCase
- 禁止 \`any\`。請使用 \`unknown\` + 型別守衛。
- 錯誤請以 Result 型別（neverthrow）回傳。不要 throw。
- 禁止魔術數字。常數請定義在 \`src/constants/\`。

## 禁止事項
- 從 \`domain/\` 直接 import \`infrastructure/\`
- 將 Prisma 的模型型別洩漏到領域層
- 用 \`console.log\` 除錯（請使用 logger）

## 測試方針
- domain / application 必須有單元測試（目標覆蓋率 80% 以上）
- infrastructure 使用整合測試
- E2E 只針對主要使用流程

## 指令集
- 開發：\`pnpm dev\`
- 測試：\`pnpm test\`
- 單一測試：\`pnpm test -- --run src/path/to/file.test.ts\`
- Lint：\`pnpm lint\`
- Migration：\`pnpm prisma migrate dev\`
```

**範本大小建議：40～80 行**

「相依方向」與「禁止事項」這兩個區塊特別有效。Claude Code 會更高精度地生成遵守這些規則的程式碼。

---

## 4\. 大型團隊開發向：階層式 CLAUDE.md ＋ 子目錄分拆模式

**預期規模：** 5 人以上 / 套件數 3 以上 / monorepo 架構

規模一大，把所有內容寫在單一檔案就會失控。 **根目錄只寫共通方針，各套件再放各自專屬的 [CLAUDE.md](http://claude.md/)** ，這是鐵則。

### 根目錄 CLAUDE.md（僅共通方針）

```
# CLAUDE.md（根目錄）

## 專案概要
EC 平台。monorepo（pnpm workspace）。

## 共通規則
- 必須使用 TypeScript strict 模式
- Commit 訊息：Conventional Commits 格式
- PR 以「一個功能一個 PR」為原則。建議控制在 500 行以下。

## 套件間相依
- shared → 不依賴其他套件
- api、web → 可依賴 shared
- 禁止 api ↔ web 直接相依

## 共通指令
- 全體建置：\`pnpm build\`
- 全體測試：\`pnpm test\`
- 指定套件：\`pnpm --filter @app/api test\`
```

### 子目錄 CLAUDE.md（例：packages/api/）

```
# CLAUDE.md（packages/api）

## 這個套件的責任
REST API 伺服器。提供認證、庫存、訂單領域。

## 額外技術棧
- NestJS v10 / Prisma v6 / PostgreSQL 16

## 這個套件的專屬規則
- 新增 endpoint 時，一定要加上 OpenAPI 裝飾器
- 必須在 \`src/modules/\` 底下，以功能為單位建立模組
- DB migration 一定要用 \`pnpm prisma migrate dev --name <說明>\` 建立

## 測試
- \`pnpm --filter @app/api test\`
```

**範本大小建議：根目錄 20～40 行 + 各套件 15～30 行**

---

## 5\. 用途特化型範本──3 種變化

這是與專案規模無關、但在 **讓 Claude Code 執行特定任務** 時很有效的範本。可追加到根目錄的 [CLAUDE.md](http://claude.md/) ，或作為用途別區塊管理在 `~/.claude/CLAUDE.md` 中。

### 5-1. 程式碼審查特化型

```
## 程式碼審查模式
請以下列觀點進行審查：

### 必須檢查的項目
1. 安全性：SQL injection、XSS、認證繞過的可能性
2. 效能：N+1 查詢、不必要的重繪、記憶體洩漏
3. 錯誤處理：例外被吞掉、對使用者洩漏資訊
4. 測試：邊界值、異常情況的測試案例不足

### 審查輸出格式
- 明確標示嚴重程度（Critical / Warning / Info）
- 顯示對應行號
- 提供包含程式碼的修正建議
```

### 5-2. 測試生成特化型

```
## 測試生成模式
### 方針
- 測試框架：Vitest
- 以 AAA 模式（Arrange / Act / Assert）結構化
- 原則上一個測試案例只寫一個 assertion

### 必要測試案例
- 正常系：代表性輸入下的預期行為
- 邊界值：空陣列、空字串、0、最大值
- 異常系：null/undefined、型別不正確、網路錯誤
- 冪等性：相同輸入應回傳相同結果

### Mock 方針
- 外部 API 呼叫一律 mock
- DB 使用 in-memory SQLite 或 mock repository
- 與時間相關的測試請使用 \`vi.useFakeTimers()\`
```

### 5-3. 重構特化型

```
## 重構模式
### 原則
- 不改變外部可見的行為（輸入／輸出）
- 重構前先確認既有測試都通過
- 一個 commit 只做一項重構（不要混在一起）

### 優先套用的模式
1. 萃取函式（Extract Function）：5 行以上的巢狀結構應拆分
2. 提早回傳（Guard Clause）：減少巢狀
3. 將魔術數字改為常數
4. 嚴格化型別：\`string\` → 聯集型別 / 品牌型別

### 禁止事項
- 把功能新增混進重構
- 對沒有測試的程式碼直接改結構（請先補測試）
- 同時進行效能最佳化與重構
```

---

## 6\. 反模式：CLAUDE.md 過於龐大而產生反效果的案例

[CLAUDE.md](http://claude.md/) 並不是「寫越多越好」。請注意以下反模式。

### ❌ 反模式列表

| 模式 | 症狀 | 對策 |
| --- | --- | --- |
| 全部塞進型 | 在單一檔案中記載所有套件細節，超過 300 行。 | 改為階層式拆分（模式 4） |
| 只追加不整理型 | 矛盾指示共存。「禁止 any」與「型別不用太在意」同時存在。 | 每月盤點，刪除不必要的內容 |
| 程式碼大量內嵌型 | 貼上 50 行以上的範例程式碼。 | 範例保持最少（5 行以內），細節請參考其他文件 |
| 願望清單型 | 像「請寫出可讀性高的程式碼」這類模糊指示的羅列。 | 改寫成具體規則（例如「函式 30 行以內」「巢狀 3 層以內」） |
| 混入機密資訊型 | 在 [CLAUDE.md](http://claude.md/) 中寫入 API Key 或 DB 密碼。 | 只記錄環境變數名稱，並指示參照 `.env` 。 |

### 適切的大小建議

- **個人專案：** 10～20 行
- **中型團隊：** 40～80 行
- **monorepo（根目錄）：** 20～40 行 + 各套件 15～30 行
- **總行數超過 500 行時就是警訊**

---

## 7\. 總結──專案首日用 15 分鐘整理 CLAUDE.md 的檢查清單

只要掌握以下 3 點，Claude Code 的輸出品質就會大幅提升。

1. **選擇符合規模的範本。** 個人腳本用極簡型（10 行）、團隊開發用分層型（40～80 行）、monorepo 則採階層式分拆。最重要的是寫得恰到好處，不多不少。
2. **一定要寫「禁止事項」。** Claude Code 往往比起「應該做什麼」，更忠實遵守「不可以做什麼」。請明確寫出禁止事項。
3. **每月盤點一次。** 反映技術棧變更、規則新增或廢止，避免矛盾與膨脹。 [CLAUDE.md](http://claude.md/) 和程式碼一樣，也需要持續維護。

### 🚀 首日 15 分鐘檢查清單

```
□ 用 1～2 行寫出專案概要
□ 列出技術棧
□ 寫出目錄結構與各目錄的責任
□ 寫出相依方向規則
□ 用 5 項以內寫出程式碼規範
□ 寫出 3 項以上「禁止事項」
□ 寫出建置・測試・執行指令
□ 如果是 monorepo，判斷是否需要階層式拆分
```

---

## 參考連結

- [Claude Code 官方文件 – Memory](https://docs.anthropic.com/en/docs/claude-code/memory) – [CLAUDE.md](http://claude.md/) 的機制與放置方式官方說明
- [Anthropic 官方 – Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview) – Claude Code 的整體概覽
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices) – Anthropic 官方最佳實踐指南

---

原文出處： [https://qiita.com/hikariclaude01/items/e54c70c90c6aa84d0f66](https://qiita.com/hikariclaude01/items/e54c70c90c6aa84d0f66)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3KKzQ3) [9瀏覽](https://codelove.tw/@tony/post/3KKzQ3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3KKzQ3)

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

🔥 [首次亮相 NVIDIA Jetson Orin Nano Super - 最經濟實惠的生成式 AI 超級計算機](https://codelove.tw/@tony/post/qvpzV3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝10 💬2 ❤️5

266

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬6 ❤️3

111

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784776822&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3KKzQ3&amp;pra=5&amp;aiof=10&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTI5IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjEyOSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTI5Il1dLDBd&amp;dt=1784776821959&amp;bpp=15&amp;bdt=103&amp;idt=70&amp;shv=r20260721&amp;mjsv=m202607170101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784686841%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784686841%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784686841%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3442166909577&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396139%2C95396252&amp;oid=2&amp;pvsid=7148456808228268&amp;tmod=529415610&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1533%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=142" title="Advertisement" aria-label="Advertisement"></iframe><iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="320" height="50" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260721/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>