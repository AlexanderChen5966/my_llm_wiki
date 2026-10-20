---
title: "讓 AI 代理人從一開始就成為戰力的原始碼倉庫設計 — AGENTS.md、目錄結構與驗證策略實作指南"
source: "https://codelove.tw/@tony/post/x8NJKa#google_vignette"
author:
published:
created: 2026-05-13
description: "有沒有過這樣的經驗：坐在你隔壁的同事用 Cursor 或 Copilot 開發速度飛快。明明使用的是相同的工具，但在自己的專案裡卻完全不順，AI 會產生偏離目標的程式碼，每次都要重複相同的說明，漸漸開始覺得「果然 AI 沒什麼用」。…請稍等一下。老實說，我一開始也是這樣。導入 Clau..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [03月22日](https://codelove.tw/@tony/post/x8NJKa "2026-03-22 09:01:26")

## 讓 AI 代理人從一開始就成為戰力的原始碼倉庫設計 — AGENTS.md、目錄結構與驗證策略實作指南

有沒有過這樣的經驗：

坐在你隔壁的同事用 Cursor 或 Copilot 開發速度飛快。明明使用的是相同的工具，但在自己的專案裡卻完全不順，AI 會產生偏離目標的程式碼，每次都要重複相同的說明，漸漸開始覺得「果然 AI 沒什麼用」。

…請稍等一下。

老實說，我一開始也是這樣。導入 Claude Code 或 Cursor，帶著期待開始使用，但也曾覺得「好像沒有想像中好」。直到有一天我意識到：

問題不是 AI 的能力，而是你提供給它的「文脈」品質不同。

更具體地說，整個「程式碼倉庫本身」是否對 AI 友好，會決定 AI 輸出的品質。無論你怎麼優化 prompt，如果程式碼庫的結構對 AI 來說難以理解，根本就有限制。

本文從三大支柱實作說明，如何讓 AI agent「從一開始就成為戰力」：AGENTS.md / [CLAUDE.md](http://claude.md/) 的撰寫方式、目錄結構最佳化、驗證（validation）策略。

文章內會附上可直接使用的範本與檢查清單，請從你有興趣的地方開始試試看。

---

## 1\. AI agent 在「讀取」程式碼庫時，實際看的是什麼

> 本章重點：AI 如何理解程式碼，以及「容易閱讀」的程式庫條件

### 「上下文視窗」（Context window）這個「視野」

AI agent 有一個稱為「上下文視窗」的概念，指的是它一次能看到的範圍。到 2026 年為止，Claude 3.5 可處理約 200K token、GPT‑4o 約 128K token、Gemini 2.0 Pro 最多可達 1,000K token。

拿人來類比就是：

上下文視窗越大 = 桌上可以攤開的文件越多。但即便桌子很大，如果文件亂成一團，也沒辦法完成工作。

對 AI 來說，比起能塞多少資訊進視窗，更重要的是「以什麼結構把資訊給它」。

### AI 真正看的東西

當 AI 程式碼輔助工具（Cursor、Claude Code、GitHub Copilot 等）理解一個程式碼庫時，它主要在看：

1. 專案根目錄的文件 — [README.md](http://readme.md/) 、 [AGENTS.md](http://agents.md/) 、 [CLAUDE.md](http://claude.md/) 、package.json
2. 目錄結構 — 等同於 `tree` 命令的整體概況
3. 型別定義 / 介面 — TypeScript 的 `.d.ts` 、OpenAPI 規格
4. 測試程式碼 — 作為期待行為的「規格書」
5. Linter / Formatter 的設定 — 程式碼風格規則

反過來說，如果這些項目沒有整理好，AI 就像在「黑暗中摸索」。每次被問「這個專案用哪個 framework？」「測試怎麼跑？」時，就是 AI 無法抓住文脈的證據。

### 「易讀」程式庫的 3 個要件

- 自我描述（Self‑descriptive）：透過文件就能理解專案全貌。問題：README 還是初始模板。
- 明確規約（Explicit conventions）：編碼規範有設定檔。問題：慣例全靠口耳相傳。
- 可驗證（Verifiable）：透過測試、linter 自動判斷正確性。問題：沒有測試或測試已壞掉。

---

## 2\. AGENTS.md / CLAUDE.md 完整設計指南

> 本章重點：各檔案的角色、設計原則、可複製的範本

### 這些檔案是做什麼用的

- [AGENTS.md](http://agents.md/) ：針對各類 AI 程式碼 agent 的專案使用說明書。到 2026 年 3 月已有超過數萬個倉庫採用，正逐漸成為事實上的標準。
- [CLAUDE.md](http://claude.md/) ：Anthropic 的 Claude Code 每次會自動讀取的設定檔。放在專案根目錄即可讓 Claude Code 以專案既有規約的狀態開始工作。
- .cursorrules：對 Cursor 的對應設定檔。

建議的使用方式如下：

- [AGENTS.md](http://agents.md/) ：通用，適用 Copilot、Codex 等（建議必備）
- [CLAUDE.md](http://claude.md/) ：給 Claude Code（若使用 Claude Code 必備）
- .cursorrules：給 Cursor（若使用 Cursor 建議有）

實用技巧：若以 [CLAUDE.md](http://claude.md/) 為主管理，可以在 [AGENTS.md](http://agents.md/) 建立符號連結（symlink）讓兩者共用。

範例：

```bash
# 以 CLAUDE.md 為主，AGENTS.md 建符號連結
ln -s CLAUDE.md AGENTS.md
```

### 設計的 5 個原則

撰寫良好 [AGENTS.md](http://agents.md/) / [CLAUDE.md](http://claude.md/) 的五項原則如下：

原則 1：規則要附上「為什麼」

- ❌ 範例：- 使用 App Router
- ✅ 範例：- 使用 App Router（Pages Router 正在逐步淘汰，新檔案請放在 app/ 之下）

如果沒有「為什麼」，AI（以及新成員）在遇到例外情況時容易判斷錯誤。知道理由的人比較能靈活應用規約。

原則 2：明確 NEVER（絕對禁止）區塊  
AI 傾向犯的錯誤要明確列出禁止事項。對 AI 而言，「不能做的事」比「可以做的事」更容易遵守。

```md
## NEVER（絕對不能做）
- 不使用 default export（僅允許 named export）
- 不直接修改 node_modules/ 或 vendor/
- 不把 .env 檔案加入程式碼
- 不刪除既有測試（可修正）
- 不在生產程式碼遺留 console.log
```

原則 3：提供 2–3 個程式碼範例  
文字描述不如實例來得清楚，範例可顯著提升傳達效果。

```typescript
// API Route 範例
// app/api/users/route.ts
import { NextResponse } from 'next/server'
import { z } from 'zod'

const querySchema = z.object({
  page: z.coerce.number().min(1).default(1),
  limit: z.coerce.number().min(1).max(100).default(20),
})

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url)
  const query = querySchema.parse(Object.fromEntries(searchParams))
  // ... 實作
  return NextResponse.json({ data, pagination })
}
```

原則 4：安裝 / 啟動命令用一行寫好  
AI 想要嘗試東西時，一行能啟動的指令會直接提升生產力。需要五個步驟的設定，對 AI（及人）都是障礙。

```bash
# 範例
npm install && npm run dev    # 啟動開發伺服器
npm test                      # 執行所有測試
npm run lint                  # 執行 linter
npm run typecheck             # 型別檢查
```

原則 5：明確劃定權限邊界  
要讓 AI 清楚知道哪些可以直接改、哪些需要先確認、哪些絕對不可以做。

```md
## 權限
### 可以自由修改
- src/ 底下的程式碼
- 新增或修正測試
- 更新文件

### 需先確認
- package.json 的相依套件新增
- 資料庫 schema 變更
- 新增環境變數

### 絕對不可以
- 修改 .env* 檔案
- 變更 CI/CD pipeline 設定
- 直接存取生產資料庫
```

### 可直接複製貼上的範本

以下為可填寫的 [AGENTS.md](http://agents.md/) 範本，請把 {...} 替換成專案相應內容。

```md
# AGENTS.md

## 專案概要
{專案名稱} 是 {一句話描述專案}。
技術棧: {Next.js / TypeScript / MongoDB / etc.}

## 架構
- 使用 {App Router / Pages Router / etc.}
- 狀態管理: {Zustand / Redux / etc.}
- DB: {MongoDB / PostgreSQL / etc.} with {ORM: Prisma / Mongoose / etc.}
- 認證: {NextAuth / Clerk / etc.}

## 目錄結構
```

{tree 輸出或目錄結構概述}

```
## 程式碼規範
- 命名規則: {camelCase / PascalCase 等}
- import 順序規則
- 元件寫法規範

## 設定（Setup）
\`\`\`bash
{安裝指令}
{啟動開發伺服器}
{執行測試}
{執行 linter}
```

## NEVER

- {專案特有禁止事項1}
- {專案特有禁止事項2}
- {專案特有禁止事項3}

## 程式碼範例

{貼上 2–3 個代表性範例}

## 權限

### 可以自由處理

- {範圍}
	### 需先確認
- {範圍}
	### 絕對不可以
- {範圍}

這些地方需要靠你自己思考，不適合完整複製貼上：

- NEVER 區塊：根據 AI 在專案中實際犯過的錯誤逐步補上
- 程式碼範例：用你專案的真實程式碼，不要用通用範例
- 架構：說明為何做出該技術選型

把 [AGENTS.md](http://agents.md/) 當成「會演進的文件」，在與 AI 共事的過程中發現錯誤就加到 NEVER，持續養成。

---

## 3\. 把程式庫結構改成 AI‑Ready 的 5 項變更

> 本章重點：針對既有程式庫，讓它更 AI 友好的 5 個具體改法

### ① 一行指令化（One‑command）

AI agent 最常要執行的是「測試」和「Lint」。能否用一條指令把它們跑完會大幅影響效率。

在 package.json 中加入如下 scripts：

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "test": "vitest run",
    "test:watch": "vitest",
    "lint": "eslint . --ext .ts,.tsx",
    "typecheck": "tsc --noEmit",
    "validate": "npm run typecheck && npm run lint && npm run test"
  }
}
```

重點是 `validate` 指令：型別檢查 → Lint → 測試 一次跑完。AI 做完修改後跑 `npm run validate` ，就能立即知道有沒有破壞東西。

### ② 目錄架構採用 Vertical Slice（功能切片）

分層式架構（Layered Architecture）範例：

```
src/
├── controllers/
│   ├── userController.ts
│   ├── orderController.ts
│   └── productController.ts
├── services/
│   ├── userService.ts
│   ├── orderService.ts
│   └── productService.ts
├── repositories/
│   ├── userRepository.ts
│   ├── orderRepository.ts
│   └── productRepository.ts
└── models/
    ├── user.ts
    ├── order.ts
    └── product.ts
```

Vertical Slice（功能切片）範例：

```
src/
├── features/
│   ├── users/
│   │   ├── api.ts
│   │   ├── service.ts
│   │   ├── repository.ts
│   │   ├── schema.ts
│   │   ├── types.ts
│   │   └── __tests__/
│   │       └── users.test.ts
│   ├── orders/
│   │   ├── api.ts
│   │   ├── service.ts
│   │   └── ...
│   └── products/
│       ├── ...
└── shared/
    ├── db.ts
    ├── auth.ts
    └── errors.ts
```

為什麼推薦 Vertical Slice？因為對 AI 而言，將一個功能相關的檔案集中在同個資料夾，比起在 controllers/、services/、repositories/ 之間橫跨尋找，更容易把需要的資訊完整放進上下文視窗。對人類也是一樣：新成員問「使用者功能在哪裡？」直接說「看 features/users/」就好。

### ③ 豐富型別定義（Type definitions）

對 AI 來說，型別定義是理解專案領域模型（domain model）的捷徑。多寫型別和 JSDoc 註解，會讓 AI 更精準推斷驗證與行為。

範例：

```typescript
// src/features/users/types.ts

/** 使用者建立請求 */
export interface CreateUserInput {
  /** 電子郵件（符合 RFC 5322） */
  email: string
  /** 顯示名稱（2-50 字） */
  displayName: string
  /** 角色（預設: member） */
  role?: 'admin' | 'member' | 'viewer'
}

/** 使用者回傳（不包含密碼雜湊等內部資訊） */
export interface UserResponse {
  id: string
  email: string
  displayName: string
  role: 'admin' | 'member' | 'viewer'
  createdAt: string
  updatedAt: string
}

/** 使用者列表的分頁回傳 */
export interface UsersListResponse {
  users: UserResponse[]
  pagination: {
    page: number
    limit: number
    total: number
    hasNext: boolean
  }
}
```

JSDoc 註解很重要。AI 會讀到像「email 要符合 RFC 5322」「displayName 需 2–50 字」的資訊，能推測該實作什麼驗證邏輯。花時間在型別與註解上，遠比事後修正 AI 生成錯誤來得省時。

### ④ 將錯誤訊息結構化

當 AI 產生的程式碼出錯時，若專案使用結構化錯誤（含錯誤碼、訊息、HTTP status、details），修正效率會大幅提升。

範例：

```typescript
// src/shared/errors.ts

export class AppError extends Error {
  constructor(
    public readonly code: string,
    message: string,
    public readonly statusCode: number = 500,
    public readonly details?: Record<string, unknown>
  ) {
    super(message)
    this.name = 'AppError'
  }
}

// 使用方式
throw new AppError('USER_NOT_FOUND', \`User with id ${userId} not found\`, 404, { userId })

// ❌ 不建議
throw new Error('not found') // 無法知道是哪個資源找不到
```

如果有像 `USER_NOT_FOUND` 這樣的錯誤碼，AI 就能立刻理解「這是使用者不存在的情境」，並提出適當修正。只寫「not found」，上下文就丟失了。

### ⑤ 在每個主要資料夾放 README

大型專案僅靠根目錄的 [AGENTS.md](http://agents.md/) 常不夠。每個主要功能資料夾放一個小型 README，讓 AI 在需要時可以取得關鍵文脈。

範例：src/features/users/ [README.md](http://readme.md/)

```md
# Users Feature

## 概要
負責使用者的 CRUD 與角色管理。

## 相依
- MongoDB（透過 Mongoose）
- NextAuth（認證）

## 測試
\`\`\`bash
npm test -- --filter users
```

## 注意事項

- 郵件唯一性由 DB 保證
- 密碼雜湊使用 bcrypt（rounds=12）
- 使用者刪除採用軟刪除（deletedAt 欄位）

這對 monorepo 或大型專案特別有效。當 AI 要處理 users 功能時，讀這個 README 就能知道「是用軟刪除」「bcrypt rounds=12」等專案細節。

---

## 4\. 驗證策略 — 不讓 AI 的輸出「被放著不管」

> 本章重點：用測試、linter、CI 自動保證 AI 輸出的品質

把 AI 產生的程式碼放著「看起來能跑就算了」，其實風險很高。人寫的程式也要被 code review，AI 的程式也需要自動驗證機制。

### 測試覆蓋率與 AI 生產力的關係

有趣的現象是：專案測試覆蓋率越高，AI 的輸出品質傾向越高。

原因很簡單：AI 會把測試當成「規格書」來讀。測試越完整，AI 就越能清楚知道「這個函式對哪些輸入要回傳什麼輸出」。沒有測試，AI 只能憑猜測實作，容易遺漏重複檢查、驗證等重要行為。

測試範例：

```typescript
// src/features/users/__tests__/users.test.ts
import { describe, it, expect } from 'vitest'
import { createUser } from '../service'

describe('createUser', () => {
  it('能用有效輸入建立使用者', async () => {
    const input = {
      email: 'test@example.com',
      displayName: '測試太郎',
      role: 'member' as const,
    }
    const user = await createUser(input)
    expect(user.email).toBe(input.email)
    expect(user.displayName).toBe(input.displayName)
    expect(user.role).toBe('member')
    expect(user.id).toBeDefined()
  })

  it('重複郵件應回傳錯誤', async () => {
    await createUser({ email: 'dup@example.com', displayName: 'A' })
    await expect(
      createUser({ email: 'dup@example.com', displayName: 'B' })
    ).rejects.toThrow('USER_EMAIL_DUPLICATE')
  })

  it('不合法郵件應回傳驗證錯誤', async () => {
    await expect(
      createUser({ email: 'invalid', displayName: 'C' })
    ).rejects.toThrow('VALIDATION_ERROR')
  })
})
```

有了這些測試，AI 能完整理解 createUser 的行為。沒有測試就有遺漏重複檢查或驗證的風險。

### 使用 pre-commit hook 保證最低品質

至少設定以下項目可以顯著穩定 AI 產出品質：

.husky/pre-commit：

```bash
npm run typecheck && npm run lint
```

並在 package.json 加入 lint-staged：

```json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"]
  }
}
```

這樣能阻擋型別錯誤與違反 linter 規則的程式碼被 commit。AI 生成的程式碼也必須通過這些關卡才能進入倉庫。

### CI/CD pipeline 範例

建立一個 validation 的 GitHub Actions workflow：

```yaml
# .github/workflows/validate.yml
name: Validate
on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '22'
          cache: 'npm'
      - run: npm ci
      - run: npm run typecheck
      - run: npm run lint
      - run: npm run test -- --coverage
      - name: Coverage check
        run: |
          COVERAGE=$(npx vitest run --coverage --reporter=json | jq '.total.lines.pct')
          if (( $(echo "$COVERAGE < 80" | bc -l) )); then
            echo "Coverage $COVERAGE% is below 80% threshold"
            exit 1
          fi
```

重點是設定「覆蓋率閾值」。有這個機制時，AI 新增程式碼就必須同時新增測試，否則 CI 會失敗。不需每次口頭要求 AI 寫測試，機制會強制執行。

---

## 5\. AI 適應度自我檢查清單

請用下列檢查表測試你的程式庫目前有多「AI‑Ready」。

### 文件（5 項）

- [AGENTS.md](http://agents.md/) / [CLAUDE.md](http://claude.md/) 在專案根目錄
- 專案概要、技術棧、架構有記載
- 明確的 NEVER（AI 不可做）區塊
- 一鍵完成的設定步驟（Setup）
- 含 2–3 個程式碼風格範例

### 結構（5 項）

- 以功能單位分資料夾（Vertical Slice）
- 型別檔（如 types.ts）有 JSDoc 註解
- 錯誤為結構化（錯誤碼 + 訊息 + status）
- 主要資料夾有 README
- package.json 的 scripts 有 test / lint / typecheck / validate

### 驗證（5 項）

- 測試覆蓋率達 80% 以上
- pre-commit hook 執行型別檢查與 linter
- CI 自動執行所有測試 + linter + 型別檢查
- 設定覆蓋率閾值
- PR 範本包含審查重點

評分參考：

- 12–15 個：AI‑Ready。AI 可從一開始就成為戰力
- 8–11 個：還不錯。先從加入 [AGENTS.md](http://agents.md/) 與一鍵化 validate 開始會有顯著提升
- 4–7 個：需要較多改善，但從這裡開始做也不算晚
- 0–3 個：不只對 AI，對於新成員接手也會很困難

---

## 總結 — 從寫程式的時代，走向設計文脈的時代

感謝看到這裡。

本文想傳達的核心是：

AI 編程工具之間的性能差距已經越來越小。到 2026 年為止，Cursor、Copilot、Claude Code 等都已經相當強大。決定差異的是你交給工具的「文脈品質」，也就是程式庫設計。

只要多加一個 [AGENTS.md](http://agents.md/) 檔、調整目錄結構、或是補一點測試，都會產生很大的差異。不需要一次全部做完，選一件今天就能做的小事開始即可。

我個人感受到工程師的工作在改變：從「寫程式的人」變成「設計 AI 可讀文脈的人」。這是詮釋會讓職務被取代，還是提升工作品質，端看你的觀點。不過至少對我來說，花時間設計要給 AI 的文脈，是最有創意也最有趣的工作。

今天就做的三件事建議：

1. 為你的專案新增 [AGENTS.md](http://agents.md/)
2. 設定 `npm run validate` ，讓測試、linter、型別檢查一鍵跑完
3. 在 NEVER 區塊寫下一項 AI 曾犯過的錯誤

小步快跑，但能確實改變與 AI 協作的品質。

---

原文出處： [https://qiita.com/akira\_papa\_AI/items/0385b6d1468e6d564b50](https://qiita.com/akira_papa_AI/items/0385b6d1468e6d564b50)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/x8NJKa) [1,051瀏覽](https://codelove.tw/@tony/post/x8NJKa)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/x8NJKa)

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

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝14 💬8

297

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1778640771&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fx8NJKa%23google_vignette&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjEuMCIsImFybSIsIiIsIjE0Ny4wLjc3MjcuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDcuMC43NzI3LjEzOCJdLFsiTm90LkEvQnJhbmQiLCI4LjAuMC4wIl0sWyJDaHJvbWl1bSIsIjE0Ny4wLjc3MjcuMTM4Il1dLDBd&amp;abgtt=6&amp;dt=1778640771311&amp;bpp=2&amp;bdt=310&amp;idt=342&amp;shv=r20260511&amp;mjsv=m202605070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1778123537%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1778123537%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1778123537%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=696033270636&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=7&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1049&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1652&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95386814&amp;oid=2&amp;pvsid=118753809659196&amp;tmod=789095963&amp;uas=0&amp;nvt=3&amp;fsapi=1&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;fc=1920&amp;brdim=1920%2C31%2C1920%2C31%2C1920%2C31%2C1892%2C985%2C1652%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.15&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=353" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260511/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=2" title="Advertisement" aria-label="Advertisement"></iframe>