---
title: "一份 AGENTS.md，讓 AI 程式碼規範率從 60% 飆升到 95%"
source: "https://codelove.tw/@tony/post/aYQmXa"
author:
published:
created: 2026-07-17
description: "一份 AGENTS.md，讓 AI 程式碼規範率從 60% 飆升到 95%  =======================================    一份 AGENTS.md，讓 AI 程式碼規範率從 60% 飆升到 95%-----------------------..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月17日](https://codelove.tw/@tony/post/aYQmXa "2026-05-17 18:02:41")



```
一份 AGENTS.md，讓 AI 程式碼規範率從 60% 飆升到 95%
```

---

> 前端 AI Skill 體系 · 實戰篇
> 
> 5 分鐘寫一份 [AGENTS.md](http://agents.md/) ，讓 AI 從此按你的規矩寫程式碼。

### 目錄

### 從留言區最多的一個問題說起

上一篇文章 [《基於四層 Skill 體系的前端團隊 AI 提效 50%》](https://juejin.cn/post/7632121609760243748) 發出後，留言區問得最多的問題是：

> " [AGENTS.md](http://agents.md/) 到底怎麼寫？能不能給個範本？"

今天安排。

但在給範本之前，我想先回答一個更根本的問題——

**為什麼一個 Markdown 檔案，就能讓 AI 寫出的程式碼品質發生質變？**

---

### AGENTS.md 到底是什麼

一句話定義：

> **[AGENTS.md](http://agents.md/) 是寫給 AI 程式設計助手看的專案規範檔。**

你可以把它類比成：

檔案給誰看管什麼`.eslintrc` 給 ESLint 看程式碼風格`.prettierrc` 給 Prettier 看格式化規則 `tsconfig.json` 給 TypeScript 看型別檢查 **`AGENTS.md` \*\*** 給 AI 看 **\*\*專案規範、目錄結構、編碼約定、禁止事項**`.eslintrc` 讓 ESLint 知道「用單引號還是雙引號」。

`AGENTS.md` 讓 AI 知道「元件放哪個目錄、樣式用什麼方案、哪些檔案絕對不能動」。

**沒有它，AI 就是一個能力很強、但不知道團隊規矩的新人。**

有了它，AI 知道你的規矩，按你的規矩來。

---

### 一份真實的 AGENTS.md 長什麼樣

以下是我從實際專案中提取的 [AGENTS.md](http://agents.md/) （已脫敏），你可以直接感受它的結構：

```
markdown 體驗AI程式碼助手 代碼解讀複製程式碼# AGENTS.md

- 本專案為 React 19 + TypeScript 前端應用
- 套件管理器：yarn，Node >= 22.12.0
- 樣式方案：Less + CSS Modules
- UI 元件庫：antd v6

## Project Structure

src/
├── components/     # 通用元件（PascalCase）
├── pages/          # 頁面元件
├── hooks/          # 自訂 Hooks
├── services/       # API 請求封裝
├── models/         # 全域狀態
├── utils/          # 工具函式
├── typings/        # 型別定義
└── constants/      # 常數

- Never edit src/.umi/ — 框架自動生成
- Never edit dist/ — 建置產物

## Coding Style

- 使用 interface 定義 Props
- 元件命名：PascalCase，檔名 index.tsx
- Never 使用 any 型別
- Never 使用內聯樣式

## Build Commands

yarn serve    # 開發伺服器
yarn build    # 生產建置
yarn lint     # 格式化
```

看起來很簡單對吧？就是一個 Markdown 檔案。

**但它的效果是驚人的。**

---

### 沒有 AGENTS.md vs 有 AGENTS.md

我用三個真實場景給你看差距。

#### 場景 1：寫一個複製按鈕元件

**❌ 沒有 [AGENTS.md](http://agents.md/) 時，AI 寫的程式碼：**

```
tsx 体验AI代码助手 代码解读复制代码export default function CopyBtn({ text }: any) {
  return (
    <button 
      style={{ color: 'blue', padding: '4px 8px' }}
      onClick={() => navigator.clipboard.writeText(text)}
    >
      Copy
    </button>
  )
}
```

問題清單：

- ❌ `any` 型別——完全沒有型別定義
- ❌ 內聯樣式——團隊用的是 CSS Modules
- ❌ `CopyBtn` 命名——團隊規範是完整的 `CopyButton`
- ❌ 沒有用 antd 的 `message` 元件做成功提示
- ❌ 檔案沒有放在 `src/components/CopyButton/` 目錄下

**✅ 有 [AGENTS.md](http://agents.md/) 後，AI 寫的程式碼：**

```
tsx 体验AI代码助手 代码解读复制代码// src/components/CopyButton/index.tsx
import React, { useCallback } from 'react'
import { message } from 'antd'
import styles from './index.less'

interface CopyButtonProps {
  text: string
  onSuccess?: () => void
}

const CopyButton: React.FC<CopyButtonProps> = ({ text, onSuccess }) => {
  const handleCopy = useCallback(async () => {
    await navigator.clipboard.writeText(text)
    message.success('已複製')
    onSuccess?.()
  }, [text, onSuccess])

  return (
    <button className={styles['copy-button']} onClick={handleCopy}>
      複製
    </button>
  )
}

export default CopyButton
```

差距一目了然：

- ✅ 明確的 `interface` 定義 Props
- ✅ CSS Modules 匯入樣式
- ✅ 用了 antd 的 `message` 元件
- ✅ `CopyButton` 完整命名
- ✅ 放在正確的目錄 `src/components/CopyButton/`

> **後者像團隊裡有 3 年經驗的同事寫的程式碼，前者像實習生第一天寫的。**

#### 場景 2：呼叫一個 API 介面

**❌ 沒有 [AGENTS.md](http://agents.md/) 時：**

```
tsx 体验AI代码助手 代码解读复制代码// 直接在元件裡裸寫 fetch
const [data, setData] = useState(null)

useEffect(() => {
  fetch('/api/customers')
    .then(res => res.json())
    .then(data => setData(data))
}, [])
```

**✅ 有 [AGENTS.md](http://agents.md/) 後：**

```
tsx 体验AI代码助手 代码解读复制代码// src/services/customer.ts
import { request } from 'umi'

export async function fetchCustomerList(
  params: API.CustomerListParams
): Promise<API.Response<API.Customer[]>> {
  return request('/api/customers', { params })
}

// 元件中使用
import { fetchCustomerList } from '@/services/customer'

const { data, loading } = useRequest(fetchCustomerList)
```

區別：

- ✅ API 呼叫封裝在 `src/services/` 下
- ✅ 使用框架提供的 `request` 而非裸 `fetch`
- ✅ 有完整的 TypeScript 型別定義
- ✅ 使用 `useRequest` Hook 管理請求狀態

#### 場景 3：提交程式碼

**❌ 沒有 [AGENTS.md](http://agents.md/) 時：**

```
bash 体验AI代码助手 代码解读复制代码git commit -m "fix button"
```

**✅ 有 [AGENTS.md](http://agents.md/) 後：**

```
bash 体验AI代码助手 代码解读复制代码git commit -m "fix(components): 修復 CopyButton 在 Safari 下複製失敗的問題"
```

一個隨意的描述 vs 一個符合 Conventional Commits 規範的 commit message。

---

### 手把手教你寫一份 AGENTS.md

看完效果，現在教你怎麼寫。

一份合格的 [AGENTS.md](http://agents.md/) 包含 **6 個核心章節** 。逐個教你寫，每個章節都有範本。

#### 第一章：技術棧聲明（必填）

這是 [AGENTS.md](http://agents.md/) 的「開場白」，告訴 AI 你的專案用了什麼。

```
markdown 体验AI代码助手 代码解读复制代码# AGENTS.md

- 本專案為 [React/Vue/Next.js] + TypeScript 前端應用
- 套件管理器：[yarn/pnpm/npm]
- 樣式方案：[Less/Sass/Tailwind CSS] + CSS Modules
- UI 元件庫：[antd/Arco Design/Element Plus] v[版本號]
```

**寫法要點** ：

- 用 **列表** 而非段落——AI 解析列表的準確率更高
- 寫明 **版本號** ——React 18 和 React 19 的寫法不同
- 寫明 **套件管理器** ——否則 AI 可能給你 `npm install` 而不是 `yarn add`

**適配不同技術棧** ：

```
markdown 体验AI代码助手 代码解读复制代码# Vue 3 專案
- 本專案為 Vue 3 + TypeScript 前端應用
- 建置工具：Vite 5
- 狀態管理：Pinia
- UI 元件庫：Ant Design Vue 4

# Next.js 專案
- 本專案為 Next.js 14 + TypeScript 全端應用
- 路由方案：App Router
- 樣式方案：Tailwind CSS v3
```

#### 第二章：目錄結構（必填）

AI 最需要的資訊之一——知道檔案放哪裡。

```
markdown 体验AI代码助手 代码解读复制代码## Project Structure

src/
├── components/     # 通用元件（PascalCase 命名）
├── pages/          # 頁面元件
├── hooks/          # 自訂 Hooks
├── services/       # API 請求封裝
├── models/         # 狀態管理
├── utils/          # 工具函式
├── typings/        # 型別定義
└── constants/      # 常數
```

**寫法要點** ：

- 每個目錄後面 **加註解** 說明用途
- 只列到 **一級目錄** 就夠了——太深了 AI 反而會困惑
- 寫清楚 **命名規範** ——比如「PascalCase」

#### 第三章：編碼規範（必填）

告訴 AI「在這個專案裡，程式碼該怎麼寫」。

```
markdown 体验AI代码助手 代码解读复制代码## Coding Style

**TypeScript/React**
- 使用 interface 定義 Props 型別
- 元件使用 React.FC<Props>
- 元件命名：PascalCase，檔名 index.tsx

**樣式（Less + CSS Modules）**
- 正確用法：import styles from './index.less'
- 使用方式：className={styles['my-class']}

**路徑別名**
- @/* → src/*
```

**寫法要點** ：

- **越具體越好** ——不要寫「遵循最佳實踐」，要寫「使用 interface 不用 type」
- **給出正確用法範例** ——AI 讀範例比讀規則更準確

#### 第四章：建置命令（建議寫）

```
markdown 体验AI代码助手 代码解读复制代码## Build Commands

yarn serve    # 啟動開發伺服器
yarn build    # 生產建置
yarn lint     # 程式碼格式化
```

AI 幫你除錯時可能需要執行命令。如果它不知道你用 `yarn serve` 而不是 `npm start` ，就會給你錯誤的指引。

#### 第五章：Never 規則（最重要！）

這是 [AGENTS.md](http://agents.md/) 中 **價值最高的章節** 。

`Never` 規則告訴 AI「絕對不能做什麼」——這比告訴它「應該怎麼做」更重要。

```
markdown 体验AI代码助手 代码解读复制代码## Never 規則

- Never 修改 src/.umi/ 或 src/.umi-production/ 目錄
- Never 修改 dist/ 目錄
- Never 在元件內使用 any 型別
- Never 使用內聯樣式（style={{ }}），除非需要動態計算
- Never 在 Less 中使用硬編碼顏色值——使用主題變數
- Never 在渲染路徑中執行耗時操作
- Never 在列表渲染中省略 key 屬性
- Never 在 node_modules/ 中安裝依賴
- Never 修改 lock 檔
- Never 使用 git stash
- Never 切換分支
```

**為什麼 Never 規則如此重要？**

因為 AI 犯錯的成本遠高於人類。

人類改錯了 `src/.umi/` ，會很快意識到「哦，這個目錄不應該改」。但 AI 不會——它會認認真真地把自動生成的程式碼改了，然後你下次建置就炸了。

> **一條 Never 規則，能避免一次災難性的誤操作。**

我的建議是：

> **每次 AI 犯了一個你不希望它犯的錯，就往 Never 規則裡加一條。**

這是一個 **持續演進** 的過程。我現在的 Never 規則有 11 條，都是從真實踩坑中累積出來的。

#### 第六章：約定與協作（團隊專案建議寫）

```
markdown 体验AI代码助手 代码解读复制代码## Commit 規範

- 格式：type(scope): description
- 類型：feat / fix / docs / style / refactor / test / chore

## 多 Agent 並發

- 禁止 git stash
- 禁止切換分支
- 只 commit 自己修改的檔案
```

---

### 通用模板：5 分鐘開箱即用

如果你不想從零寫，這裡有一份通用範本，涵蓋 React + TypeScript 專案最常見的場景。

**直接複製到你的專案根目錄，改掉 `[方括號]` 裡的內容就行** ：

```
markdown 体验AI代码助手 代码解读复制代码# AGENTS.md

- 本專案為 [React/Vue/Next.js] + TypeScript 前端應用
- 套件管理器：[yarn/pnpm/npm]
- 樣式方案：[Less/Sass/Tailwind CSS] + CSS Modules
- UI 元件庫：[antd/Arco Design/Element Plus] v[版本號]

## Project Structure

src/
├── components/     # 通用元件（PascalCase 命名）
├── pages/          # 頁面元件
├── hooks/          # 自訂 Hooks
├── services/       # API 請求封裝
├── models/         # 狀態管理
├── utils/          # 工具函式
├── typings/        # 型別定義
└── constants/      # 常數

## Coding Style

**TypeScript**
- 使用 interface 定義 Props
- 元件使用 React.FC<Props> 或函式宣告
- Never 使用 any 型別

**樣式**
- import styles from './index.less'
- Never 使用內聯樣式
- Never 硬編碼顏色值

**路徑別名**
- @/* → src/*

## Build Commands

[你的開發命令]    # 啟動開發伺服器
[你的建置命令]    # 生產建置
[你的格式化命令]  # 程式碼格式化

## Never 規則

- Never 修改框架自動生成的目錄
- Never 修改 dist/ 建置產物
- Never 在元件內使用 any 型別
- Never 使用內聯樣式
- Never 在渲染路徑中執行耗時操作
- Never 在列表渲染中省略 key
- Never 硬編碼敏感資訊
- Never 修改 lock 檔

## Commit 規範

- 格式：type(scope): description
- 類型：feat / fix / docs / style / refactor / test / chore
```

---

### 進階技巧

#### 技巧 1：DESIGN.md 聯動視覺規範

`AGENTS.md` 管的是「程式碼怎麼寫」， `DESIGN.md` 管的是「UI 長什麼樣」：

```
markdown 体验AI代码助手 代码解读复制代码# DESIGN.md

## 品牌色
- 主色：#1677FF
- 主色懸浮：#4096FF
- 主色背景：rgba(22, 119, 255, 0.1)

## 文字色
- 主文字：rgba(0, 0, 0, 0.88)
- 次要文字：rgba(0, 0, 0, 0.65)

## 間距系統
- 基礎單位：8px
- 元件間距：16px / 24px
```

有了 [DESIGN.md](http://design.md/) ，AI 在寫樣式時就不會用 `color: blue` 這種硬編碼顏色值。

#### 技巧 2：AGENTS.local.md 個人偏好

建立一個 `AGENTS.local.md` （加入 `.gitignore` ），寫你的個人偏好：

```
markdown 体验AI代码助手 代码解读复制代码# AGENTS.local.md（不入倉庫）

- 回覆語言：中文
- 程式碼註解語言：中文
- 元件寫法偏好：函式宣告
- 型別定義偏好：interface
```

AI 會同時讀取專案規範和你的個人偏好，輸出更符合你習慣的程式碼。

#### 技巧 3：Never 規則的持續演進

我的建議是維護一個 **Never 規則演進日誌** ：

```
yaml 体验AI代码助手 代码解读复制代码2026-04-15  新增：Never 修改 src/.umi/ 目錄
            原因：AI 把自動生成的路由配置改了，建置報錯

2026-04-18  新增：Never 在 Less 中硬編碼顏色值
            原因：AI 用了 #333 而不是主題變數，切主題時全崩了

2026-04-22  新增：Never 使用 git stash
            原因：多 Agent 並發時，一個 Agent stash 了另一個的改動
```

> **每一條 Never 規則背後，都是一個真實的踩坑故事。**

---

### 效果數據

指標 沒有 [AGENTS.md](http://agents.md/) 有 [AGENTS.md](http://agents.md/) 變化 AI 程式碼規範遵循率 ~60% **95%+** ↑ 35% AI 程式碼一次通過率 ~40% **80%+** ↑ 40% 每次手動修正時間 ~8 分鐘 **~1 分鐘** ↓ 87.5% 新人上手時間 3-5 天 **1 天** ↓ 70%+ > 以上數據基於實際專案 3 週使用期間，對 50+ 次 AI 程式碼生成的人工抽檢統計。不同專案和技術棧可能有差異。

最後一個數據可能出乎意料—— **[AGENTS.md](http://agents.md/) 對新人也有用** 。

因為它不僅是給 AI 看的，也是一份精簡的專案開發規範。新人讀 [AGENTS.md](http://agents.md/) ，5 分鐘就知道專案的技術棧、目錄結構、編碼規範和禁止事項。

> **它是專案文件的一個超級濃縮版。**

---

### 常見問題

#### Q1：AGENTS.md 放在哪裡？

**專案根目錄。** 和 `package.json` 、 `tsconfig.json` 同級。AI 程式設計助手會自動掃描專案根目錄讀取。

#### Q2：需要提交到 Git 嗎？

**需要。** 團隊共享的規範檔。但 `AGENTS.local.md` （個人偏好）應該加入 `.gitignore` 。

#### Q3：寫多長合適？

- **最小可用版本** ：30 行（技術棧 + 目錄 + 3 條 Never 規則）
- **推薦長度** ：60-100 行
- **不建議超過** ：200 行

#### Q4：多個 AI 工具都能識別嗎？

已驗證：Cursor ✅ | Claude ✅ | GitHub Copilot ✅ | Windsurf ✅

#### Q5：已有專案怎麼加？

在專案根目錄建立 `AGENTS.md` ，複製本文範本，5 分鐘填好，提交。 **不需要改任何程式碼，不需要安裝任何工具。**

---

### 一個彩蛋

如果你覺得手動寫還是太麻煩——下篇教學我會分享如何 **用一行命令自動生成 [AGENTS.md](http://agents.md/) + [DESIGN.md](http://design.md/)** ，腳本會讀取 `package.json` 自動識別技術棧、UI 庫、套件管理器，5 秒搞定。

但即使不用任何工具， **手寫的 [AGENTS.md](http://agents.md/) 也能達到 80% 的效果。**

工具只是讓最後的 20% 更自動化。核心永遠是那個 Markdown 檔案本身。

---

### 總結

你可能在想我的回答「一個 Markdown 檔案真能有這麼大效果？」是的，AI 缺的不是能力，是上下文「寫起來會不會很複雜？」6 個章節，30-100 行，5 分鐘「團隊不願意用怎麼辦？」提交到 Git 就行，團隊無需額外操作「要花多少時間維護？」初次 5 分鐘，之後每踩一個坑加一條 Never 規則> **與其每次手動修正 AI 寫的程式碼，不如花 5 分鐘告訴它規矩。**

---

如果你按照本文的方法建立了 [AGENTS.md](http://agents.md/) ，歡迎在留言區分享你的效果數據——特別是「規範遵循率」和「一次通過率」的變化。

也歡迎分享你踩過的坑和總結出的 Never 規則，我會整理一份 **社群版 Never 規則大全** 👇

覺得有用？點個讚 👍 收藏一下，後面我會繼續分享 [DESIGN.md](http://design.md/) 的深度用法——如何讓 AI 寫出的 UI 不再「一眼 AI 味」。

---

原文出處： [https://juejin.cn/post/7639561816260886571](https://juejin.cn/post/7639561816260886571)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/aYQmXa) [290瀏覽](https://codelove.tw/@tony/post/aYQmXa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/aYQmXa)

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

🔥 [【存檔版】Google Antigravity 完全指南 | 掌握下一代「基於代理」IDE 的速查表](https://codelove.tw/@tony/post/qJXye3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝8 💬3 ❤️4

288

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

182

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1784277631&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FaYQmXa&amp;pra=5&amp;asro=0&amp;itsi=47&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MC4wLjc4NzEuMTE1IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdDtBPUJyYW5kIiwiOC4wLjAuMCJdLFsiQ2hyb21pdW0iLCIxNTAuMC43ODcxLjExNSJdLFsiR29vZ2xlIENocm9tZSIsIjE1MC4wLjc4NzEuMTE1Il1dLDBd&amp;dt=1784277630942&amp;bpp=278&amp;bdt=171&amp;idt=278&amp;shv=r20260716&amp;mjsv=m202607140101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1784275192%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1784275192%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1784275192%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=3625750780101&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1533&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=95396567%2C95390278%2C95396252%2C31061690&amp;oid=2&amp;pvsid=6760596126079952&amp;tmod=536656302&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1533%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=343" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="320" height="50" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260716/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>