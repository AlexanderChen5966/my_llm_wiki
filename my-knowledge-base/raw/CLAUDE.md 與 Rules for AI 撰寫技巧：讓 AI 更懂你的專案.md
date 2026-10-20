---
title: "CLAUDE.md 與 Rules for AI 撰寫技巧：讓 AI 更懂你的專案"
source: "https://israynotarray.com/ai/20260310/3737444063/"
author:
  - "[[Ray]]"
published: 2026-03-10
created: 2026-05-13
description: "要讓 AI 真正融入你的開發流程，關鍵就在於怎麼寫好那份規則文件。這篇以 CLAUDE.md 為主，分享 8 個 Rules for AI 的撰寫技巧，讓 AI 更精準理解你的專案需求。"
tags:
  - "clippings"
status: processed
---
![CLAUDE.md 與 Rules for AI 撰寫技巧](https://images.israynotarray.com/blog/ai/claude-rules-for-ai/Gemini_Generated_Image_knq8csknq8csknq8_1280x698.png)

CLAUDE.md 與 Rules for AI 撰寫技巧

> **Note**  
> CLAUDE.md 是給 AI 開發工具讀的專案規則文件，類似 Cursor 的.cursorrules、Copilot 的 instructions。寫法上要避免抽象指令（「請好好寫」這種），改成可驗證的具體規則（檔案命名、commit 格式、技術選型），AI 才能真正照做。

## 前言

如果你已經在使用 Claude Code、GitHub Copilot 這類 AI 開發工具，那你應該多少有聽過 Rules for AI 這個概念，簡單來講就是一份文件，告訴 AI 在這個專案中應該怎麼做事。

但很多人寫完之後發現 AI 還是不太聽話，要嘛忽略規則，要嘛產出的程式碼品質不如預期，這其實大部分原因出在規則文件本身寫得不夠好。

所以這一篇就來聊一下怎麼寫好這份文件，讓 AI 真正理解你的專案需求吧！這篇會以 Claude Code 的 CLAUDE.md 為主來舉例，但觀念上不管你用哪一款 AI 開發工具都適用。

> **Note**  
> 如果你還不知道什麼是 CLAUDE.md，可以先去看我之前寫的 [想用 Claude Code 開發？這篇帶你從入門到進階](https://israynotarray.com/ai/20260128/374686091/) ，裡面有非常詳細的介紹。

## 各家 AI 工具的規則文件

在開始之前，先來快速了解一下各家 AI 開發工具的規則文件分別叫什麼、放在哪裡，雖然名稱不同，但核心概念其實是一樣的。

| AI 工具 | 規則文件名稱 | 放置位置 |
| --- | --- | --- |
| Claude Code | `CLAUDE.md` | 專案根目錄 |
| Cursor | `.cursor/rules/*.mdc` | `.cursor/rules/` |
| GitHub Copilot | `.github/copilot-instructions.md` | `.github/` |

雖然格式和放置方式不同，但它們的目的都一樣： **告訴 AI 這個專案的規範與偏好** 。

那麼接下來介紹的撰寫技巧，不管你用的是哪一個工具都適用，只是語法上可能會有些微差異。

## 常見的錯誤寫法

在介紹正確的寫法之前，先來看看幾個常見的錯誤寫法，這些寫法會讓 AI 根本搞不清楚你到底要什麼。

### 寫得太模糊

```md
# 專案規範

請寫好的程式碼。
使用最佳實踐。
注意效能。
```

這種寫法等於什麼都沒寫對吧？好的程式碼、最佳實踐、注意效能，這些都太抽象了，AI 根本無法從中得到任何具體的指引。

### 寫得太冗長

```md
# 專案規範

本專案創立於 2023 年，是由我們團隊在經過長時間討論後決定使用 React 框架來開發的，
當時考慮了 Vue.js、Angular、Svelte 等多個選項，最終因為團隊成員大多熟悉 React，
而且 React 生態系比較完整，所以才決定使用 React。後來在 2024 年我們又導入了
TypeScript，因為...（以下省略 500 行歷史故事）
```

AI 不需要知道你的專案歷史，它只需要知道現在該怎麼做就好。過多的背景故事只會浪費 Token 並且稀釋真正重要的資訊。

### 規則互相矛盾

```md
- 所有函式都要寫 JSDoc 註解
- 程式碼應該要能自我解釋，不需要多餘的註解
```

這兩條規則互相矛盾，AI 會不知道該遵守哪一條，最終的結果就是隨機選擇，導致程式碼風格不一致。

## 撰寫技巧一：使用具體且可執行的指令

好的規則應該要夠具體、夠明確，讓 AI 看完之後就知道該怎麼做。

**不好的寫法：**

```md
請使用適當的命名方式。
```

**好的寫法：**

```md
## 命名規範

- 變數與函式：使用 camelCase（例如：\`getUserName\`、\`isLoading\`）
- 元件：使用 PascalCase（例如：\`UserProfile\`、\`LoginForm\`）
- 常數：使用 UPPER_SNAKE_CASE（例如：\`API_BASE_URL\`、\`MAX_RETRY_COUNT\`）
- CSS class：使用 kebab-case（例如：\`user-profile\`、\`login-form\`）
- 布林值變數：使用 \`is\`、\`has\`、\`should\` 前綴（例如：\`isActive\`、\`hasPermission\`）
```

有沒有發現差異？好的寫法不只告訴 AI 要用什麼，還直接給了範例讓它可以參考。

## 撰寫技巧二：善用正面表述與範例

比起告訴 AI 不要做什麼，直接告訴它要做什麼會更有效，因為 AI 在處理否定語句時，有時候反而會忽略掉前面的「不要」。

**不好的寫法：**

```md
- 不要使用 any 類型
- 不要使用 var
- 不要直接在函式內呼叫 API
```

那好的寫法該怎麼寫呢？我們可以透過正確與錯誤的對比方式來讓 AI 更清楚理解你的意圖。

**好的寫法（以 TypeScript 規範為例）：**

所有變數與函式都必須明確標註型別，禁止使用 `any` 。

正確寫法：

```ts
const getUserName = (userId: string): Promise<string> => { ... }
```

錯誤寫法：

```ts
const getUserName = (userId: any): any => { ... }
```

**好的寫法（以 API 呼叫規範為例）：**

API 呼叫統一封裝在 `services/` 資料夾中，不要在業務邏輯中直接使用 `fetch` 或 `axios` 。

正確寫法：

```ts
// services/userService.ts
export const getUser = async (userId: string): Promise<User> => {
  const response = await apiClient.get(\`/users/${userId}\`);
  return response.data;
};

// 業務邏輯中使用
import { getUser } from '@/services/userService';

const user = await getUser('123');
```

錯誤寫法：

```ts
// 直接在業務邏輯中呼叫 API
const response = await fetch('/api/users/123');
const user = await response.json();
```

透過這樣的對比，AI 就能更明確地知道你期望的程式碼風格，而且提供範例程式碼可以讓 AI 產出更符合你風格的程式碼囉～

## 撰寫技巧三：結構化你的規則文件

一份好的規則文件應該要有清楚的結構，讓 AI 可以快速找到相關的規範，我個人建議的結構大概像這樣：

```md
# 專案名稱

簡短的專案描述（1-2 句話）

## 技術棧

- 框架：Vue 3 + TypeScript 5.7
- 建置工具：Vite
- 狀態管理：Pinia
- 樣式：SCSS
- 測試：Vitest
- 套件管理：pnpm

## 專案架構

src/
├── components/    # 共用元件
├── views/         # 頁面元件
├── composables/   # 組合式函式
├── services/      # API 呼叫
├── stores/        # Pinia 狀態管理
├── types/         # TypeScript 型別定義
└── utils/         # 工具函式

## 開發規範

（命名、風格、測試等具體規範）

## 常用指令

- \`pnpm dev\`：啟動開發伺服器
- \`pnpm build\`：建置專案
- \`pnpm test\`：執行測試
- \`pnpm lint\`：執行 Lint 檢查
```

這邊有幾個重點：

1. **技術棧要寫版本號** ：告訴 AI 你用的是哪個版本，避免它用到已經棄用的 API
2. **專案架構要列出來** ：讓 AI 知道檔案該放在哪裡
3. **常用指令要列出來** ：讓 AI 知道該用什麼指令來執行任務

## 撰寫技巧四：分離關注點

當規則越寫越多的時候，不要全部塞在同一份文件裡，拆分成多個文件會比較好管理。我個人會盡可能把單一檔案控制在 100～200 行以內，太長的話 AI 反而容易漏掉重點。

以 Claude Code 為例，有兩種拆分方式：

### 方式一：使用 Markdown 連結引用

Claude Code 的 CLAUDE.md 官方支援 `@path/to/file` 語法來匯入其他檔案，匯入的檔案會在啟動時一起載入到上下文中。但我個人更常使用 `./` 的 Markdown 連結方式，因為這樣在 GitHub 上閱讀時可以直接點擊跳轉到對應的文件，方便團隊成員在不使用 Claude Code 的情況下也能快速查閱相關規範。

```
CLAUDE.md                          # 主文件：專案概述 + 引用子文件
docs/
├── PROJECT-STRUCTURE.md           # 專案架構說明
├── CODE-STYLE.md                  # 程式碼風格規範
├── GIT-COMMIT.md                  # Commit 規範
└── API-GUIDELINES.md              # API 設計規範
```

主文件 `CLAUDE.md` 保持精簡，引用子文件：

```md
# My Project

這是一個使用 Vue 3 + TypeScript 開發的電商平台。

## 重要文件

- [專案架構說明](./docs/PROJECT-STRUCTURE.md)
- [程式碼風格規範](./docs/CODE-STYLE.md)
- [Commit 規範](./docs/GIT-COMMIT.md)
- [API 設計規範](./docs/API-GUIDELINES.md)
```

### 方式二：使用.claude/rules/ 目錄

另一種方式是將規則放在 `.claude/rules/` 目錄底下，每個 `.md` 檔案涵蓋一個主題：

```
.claude/
├── CLAUDE.md
└── rules/
    ├── code-style.md    # 程式碼風格規範
    ├── testing.md       # 測試規範
    └── git-commit.md    # Commit 規範
```

沒有設定 `paths` 的規則檔案會在啟動時全部載入，而有設定 `paths` 的規則檔案只會在 Claude 處理匹配路徑的檔案時才載入（下一個技巧會介紹）。

這兩種方式的共同好處：

1. **節省 Token** ：避免單一檔案過於龐大
2. **方便維護** ：每個文件只負責一個主題，修改時不會影響其他規則

## 撰寫技巧五：針對路徑設定規則

有些規範並不是整個專案都適用，而是只針對特定的檔案或資料夾，這時候就可以用路徑限定的方式來設定。

以 Claude Code 的 `.claude/rules/*.md` 為例，你可以使用 YAML frontmatter 中的 `paths` 欄位來指定適用的檔案路徑：

```md
---
paths:
  - "src/api/**/*.ts"
---
# API 開發規範

- 所有 API 呼叫都必須使用 try-catch 包裹
- 使用統一的錯誤回應格式
- 回傳型別必須明確標註
```

`paths` 支援 glob 模式，常見的寫法有：

| 模式 | 匹配 |
| --- | --- |
| `**/*.ts` | 所有 TypeScript 檔案 |
| `src/**/*` | `src/` 底下的所有檔案 |
| `src/**/*.{ts,vue}` | `src/` 底下的 `.ts` 和 `.vue` 檔案 |
| `tests/**/*.test.ts` | 測試檔案 |

也可以同時指定多個路徑：

```md
---
paths:
  - "src/api/**/*.ts"
  - "src/services/**/*.ts"
---
```

這樣 AI 只會在處理對應路徑的檔案時才載入這些規則，不相關的規則就不會干擾到其他部分的開發，同時也能節省 Token。

## 撰寫技巧六：寫下決策背後的原因

在某些關鍵的規範中，除了告訴 AI 要怎麼做之外，也可以簡短說明為什麼要這樣做，這樣 AI 在遇到類似但沒有明確規定的情境時，也能做出符合你意圖的判斷。

```md
## 狀態管理

使用 Pinia 進行狀態管理，不使用 Vuex。
原因：Pinia 是 Vue 3 官方推薦的狀態管理方案，API 更簡潔，且原生支援 TypeScript。

## 測試策略

以整合測試為主，單元測試為輔。
原因：元件間的互動邏輯複雜，整合測試更能確保功能正確性。每個頁面至少要有一個整合測試。
```

但也不用每一條規則都附上原因啦，只有那些 AI 可能會質疑或做出不同判斷的規則才需要補充，不然文件會變得太冗長。

## 撰寫技巧七：加入常見任務的步驟指引

這個技巧可能比較少人知道，就是你可以在規則文件中直接定義常見任務的 SOP，像是當使用者說要建新頁面時，AI 就該照著這個流程走，不用每次都重新交代一遍。

```md
## 常見任務指引

### 當使用者說「幫我建一個新的頁面」

1. 在 \`src/views/\` 底下建立對應的 \`.vue\` 檔案
2. 在 \`src/router/\` 中加入路由設定
3. 如果有共用邏輯，在 \`src/composables/\` 底下建立對應的 composable
4. 建立對應的測試檔案

### 當使用者說「幫我加一個 API 串接」

1. 在 \`src/services/\` 底下建立或修改對應的 service 檔案
2. 在 \`src/types/\` 底下定義請求與回應的 TypeScript 型別
3. 使用 \`apiClient\` 封裝（不要直接使用 fetch 或 axios）
4. 建立對應的測試檔案
```

這個方法特別適合團隊開發，把常見的開發模式記錄下來，AI 就能跟著走相同的流程，省去每次口頭交代的時間。

## 撰寫技巧八：設定回應語言與風格

最後一個技巧，如果你希望 AI 用特定的語言或風格來回應，記得也要寫在規則文件裡面。

```md
## 回應規範

- 一律使用繁體中文回應
- 使用 Conventional Commits 格式撰寫 commit message（例如：\`feat: 新增登入功能\`）
- commit message 的描述使用繁體中文
- 程式碼中的變數名稱、註解使用英文
```

這看起來很簡單對吧？但如果你不寫的話，AI 很可能會隨機用英文或簡體中文來回應，導致專案語言不統一。

## 一份完整的範例

最後整合上面所有的技巧，底下提供一份完整的 CLAUDE.md 範例給大家參考：

```md
# E-Commerce Platform

使用 Vue 3 + TypeScript 開發的電商平台前端。

## 技術棧

- 框架：Vue 3.5（Composition API + \`<script setup>\`）
- 語言：TypeScript 5.7（strict mode）
- 建置工具：Vite 6
- 路由：Vue Router 4
- 狀態管理：Pinia
- 樣式：SCSS + BEM 命名
- 測試：Vitest + Vue Test Utils
- 套件管理：pnpm
- Node.js 版本：v22

## 重要文件

- [專案架構說明](./docs/PROJECT-STRUCTURE.md)
- [程式碼風格規範](./docs/CODE-STYLE.md)
- [Commit 規範](./docs/GIT-COMMIT.md)
- [API 設計規範](./docs/API-GUIDELINES.md)
- [測試規範](./docs/TESTING.md)

## 常用指令

- \`pnpm dev\`：啟動開發伺服器（port 5173）
- \`pnpm build\`：建置專案
- \`pnpm test\`：執行測試
- \`pnpm test:coverage\`：執行測試並產出覆蓋率報告
- \`pnpm lint\`：執行 ESLint 檢查

## 開發規範

### 命名規範

- 變數與函式：camelCase（\`getUserName\`）
- 元件與型別：PascalCase（\`UserProfile\`、\`UserType\`）
- 常數：UPPER_SNAKE_CASE（\`API_BASE_URL\`）
- 元件檔案名稱：PascalCase（\`UserProfile.vue\`）
- 非元件檔案名稱：camelCase（\`useUser.ts\`、\`userService.ts\`）
- 布林值：\`is\`/\`has\`/\`should\` 前綴（\`isActive\`）

### TypeScript

- 禁止使用 \`any\`，必要時使用 \`unknown\` 搭配型別守衛
- 使用 \`interface\` 定義物件型別，使用 \`type\` 定義聯合型別或工具型別
- 所有匯出的函式都必須標註回傳型別

### Vue 元件開發

- 統一使用 \`<script setup lang="ts">\` 語法
- Props 使用 \`defineProps<T>()\` 搭配 TypeScript 型別定義
- Emit 使用 \`defineEmits<T>()\` 搭配 TypeScript 型別定義
- 共用邏輯抽成 composable（\`composables/useXxx.ts\`）
- 超過 200 行的元件應拆分為子元件

### 錯誤處理

- API 呼叫使用 try-catch 包裹
- 使用統一的 \`AppError\` 類別處理錯誤
- 全域錯誤使用 \`app.config.errorHandler\` 捕捉

## 回應規範

- 一律使用繁體中文回應
- commit message 使用 Conventional Commits 格式，描述使用繁體中文
- 程式碼中的變數名稱與註解使用英文

## 常見任務

### 建立新頁面

1. 在 \`src/views/\` 底下建立對應的 \`.vue\` 檔案
2. 在 \`src/router/\` 中加入路由設定
3. 如果有共用邏輯，在 \`src/composables/\` 建立對應的 composable
4. 建立對應的測試檔案

### 建立新 API 串接

1. 在 \`src/services/\` 底下建立對應的 service 檔案
2. 在 \`src/types/\` 定義請求與回應的 TypeScript 型別
3. 使用 \`apiClient\` 封裝，不要直接使用 fetch 或 axios
4. 建立對應的測試檔案
```

## 維護你的規則文件

寫好規則文件之後，還有一件很重要的事情就是持續維護。

隨著專案的發展，技術棧可能會改變、開發規範可能會調整，如果規則文件沒有跟著更新，AI 就會繼續按照舊的規則來開發，這反而會造成更多問題。

所以這邊建議幾件事：

1. **定期檢視** ：每次大版本更新或重大技術決策改變時，記得更新規則文件
2. **搭配 Code Review** ：如果 AI 產出的程式碼不符合預期，先檢查是不是規則文件沒有寫清楚
3. **團隊協作** ：如果是團隊開發，規則文件應該要被納入版本控制，讓每個人都可以看到並提出修改建議
4. **善用 /insights** ：如果你是使用 Claude Code 的話，可以定期使用 `/insights` 指令來分析你的使用狀況，找出可以優化的地方（詳見 [優化 Claude Code 沒方向？超簡單指令給你方向！](https://israynotarray.com/ai/20260306/1831816539/) ）

## 不推薦使用 /init 來生成

最後補充一點，Claude Code 有一個 `/init` 指令可以自動幫你生成 CLAUDE.md，但我個人其實不太推薦這樣做。

原因很簡單， `/init` 生成出來的內容通常都很制式化，它只是掃描你的專案結構然後產出一份看起來很完整但其實很空泛的文件。裡面的規則大多是通用的描述，不會真正反映你團隊的開發習慣跟偏好，結果就是你還是得花時間大幅修改，甚至整份砍掉重寫。

與其這樣，不如一開始就自己動手寫，從最重要的幾條規則開始，隨著開發過程慢慢補充。這樣寫出來的規則文件才是真正貼合你專案的，AI 也才能真的照著你的意思做事。

## 結語

不管你使用的是哪一款 AI 開發工具，寫好一份規則文件都是讓 AI 融入你開發流程的關鍵。

那麼最後總結一下這篇的重點：

1. **具體且可執行** ：給 AI 明確的規範，不要寫抽象的描述
2. **正面表述 + 範例** ：直接告訴 AI 要做什麼，搭配程式碼範例更有效
3. **結構化** ：用清楚的標題與分類，讓 AI 快速找到相關規範
4. **分離關注點** ：拆分成多個文件，避免單一檔案過於龐大
5. **路徑限定** ：針對特定路徑設定專屬規則
6. **說明原因** ：在關鍵決策上補充為什麼要這樣做
7. **任務指引** ：加入常見任務的步驟說明
8. **語言風格** ：明確設定回應語言與 commit 格式

希望這篇能幫助你寫出更好的規則文件，讓 AI 更懂你的專案哩～

你的支持會直接轉換成更多技術筆記

如果我的筆記讓你少踩一個坑、節省 Debug 的時間，  
也許你可以請我喝杯咖啡，讓我繼續當個不是 Array 的 Ray ☕

 [![buymeacoffee](https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)](https://www.buymeacoffee.com/israynotarray)| [![line](https://israynotarray.com/images/line-images.png) ](https://store.line.me/search/zh-Hant?q=israynotarray)| [![portaly](https://israynotarray.com/images/logo.b91a6dbf.webp)](https://portaly.cc/israynotarray/support)

Terminal

ray — -zsh — 80x24

Welcome to Ray Terminal v1.0.0

Type "help" to see available commands.

\------------------------------------

➜ ~