---
title: AI 生成程式碼的 Code Review 防護層：三大死角 + TypeScript/ESLint 安全護欄
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://codelove.tw/@tony/post/3kkzA3
type: article
status: processed
asset_value: 65
---

AI coding 工具速度極快，但 AI 生成的程式碼有固定的幾個死角——格式完美的程式碼比排版混亂的更容易讓人放鬆警覺。用 AI 驗證 AI，先把靜態分析設到最嚴，再專注人工守住三個動態死角。

## T（核心論點）

AI 生成的程式碼兩大風險：
1. **格式欺騙性**：縮排完美、命名規範、有完整註解，讓人看到就放鬆警覺，但邏輯可能有致命缺陷
2. **規模爆炸**：單日可提交數千行，人工 review 被迫疲勞妥協

解法：**前置靜態分析防線，人工只守 AI 最愛挖的三個動態死角**。

## B（適用場景）

- 有開發者使用 Claude Code / Cursor / Codex 等 AI 生成大量 React/TypeScript 程式碼
- 團隊 Code Review 壓力大，reviewer 需要知道哪裡最容易出問題
- 想建立 CI 靜態分析護欄，讓 AI 幻覺在 commit 前就被攔截

## R（完整內容）

### TypeScript 嚴格模式（針對 AI 生成程式碼的配置）

```json
// tsconfig.json
{
  "compilerOptions": {
    "noImplicitAny": true,           // 禁止 AI 隨手 any
    "strictNullChecks": true,        // 強制處理 null/undefined 邊界
    "exactOptionalPropertyTypes": true, // 防止捏造不存在的可選屬性
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}
```

### ESLint 針對 AI 習慣的強力規則

```javascript
// eslint.config.js
module.exports = {
  rules: {
    // AI 最常為了規避警告而亂刪依賴陣列，必須強制報錯
    'react-hooks/exhaustive-deps': 'error',
    // 忘寫 await 導致非同步狀態失控——極常見低級錯誤
    '@typescript-eslint/no-floating-promises': 'error',
    // 攔截隨手硬編碼的魔法數字
    'no-magic-numbers': ['warn', { ignore: [0, 1, -1] }]
  }
};
```

### AI 最愛挖的三個動態死角（靜態分析抓不到）

#### 死角 1：弱網與競態條件

AI 假設網路永遠暢通、介面毫秒返回、使用者操作按部就班。

**AI 典型輸出（有缺陷）：**
```typescript
// 無防抖、無取消機制——搜尋框在弱網競態下顯示舊資料
useEffect(() => {
  fetchSearchResults(keyword).then(res => setResults(res.data));
}, [keyword]);
```

**正確寫法要補：**
- `AbortController` 取消前一個請求
- `setTimeout` 防抖（300ms）
- 元件卸載時 `controller.abort()` + `clearTimeout`

#### 死角 2：只管掛載不管清理的記憶體洩漏

AI 喜歡用 `addEventListener`、WebSocket 長連線，但常忘記在 `useEffect` cleanup 裡清理。使用者連續操作 10-15 分鐘後瀏覽器 heap 被塞滿，App 卡死崩潰。

**審核問題：** 所有的副作用都在元件卸載時徹底清理乾淨了嗎？

#### 死角 3：捏造不存在的套件和方法

AI 為了讓程式碼看起來邏輯自洽，會捏造不存在的 npm 套件或 import 路徑。

典型案例：`import { deepMerge } from 'lodash/deepMerge'`——Lodash 沒有這種路徑匯出方式。
後果不只是 build 失敗，更大的風險是：攻擊者會準備名稱相似的惡意套件等人安裝。

**審核動作：** 所有第三方相依項都要核對官方文件，確認 import 路徑存在。

### 人機分工原則

| 負責方 | 守備範圍 |
|--------|---------|
| 靜態分析（TypeScript/ESLint）| 型別錯誤、any、浮動 Promise、未使用變數 |
| CI/CD | 建構成功、lint 通過、測試通過 |
| **人工 Review** | 競態條件、記憶體洩漏、業務邊界、捏造套件 |

把 AI 當成「不知疲倦、極度高產，但缺乏業務場景和安全意識的應屆生」——你不會閉眼把應屆生的程式碼直接合併到 prod。

## C（關鍵行動）

- [ ] tsconfig.json 加上 `noImplicitAny`、`strictNullChecks`、`exactOptionalPropertyTypes`
- [ ] ESLint 加上 `react-hooks/exhaustive-deps: error` 和 `no-floating-promises: error`
- [ ] Code Review 清單加入三個死角：競態條件、cleanup 完整性、捏造 import
- [ ] 審查 AI 生成程式碼時，格式越完美越要提高警覺

## 相關連結

- [[loop-engineering-6-components]] — Loop Engineering：讓 AI 自動迭代修正的迴圈設計
- [[spec-first-ai-coding-workflow]] — Spec-First：先讓 AI 出方案再實作，減少後期 review 負擔
- [[rust-hot-path-optimization-ai-review]] — 真實案例：拒絕會靜默降版相依套件的 AI 修正，補上「CI 綠燈也抓不到的相依版本死角」
